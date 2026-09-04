using SchoolEscape.Core;
using SchoolEscape.Player;
using UnityEngine;

namespace SchoolEscape.World
{
    public sealed class StrongBrick : Brick
    {
        private enum BrickState
        {
            Normal, // 기본
            Damaged // 데미지 받았을 때
        }

        [SerializeField]
        private SpriteRenderer _brickRenderer;
        [SerializeField]
        private Color _usedColor = new Color(0.86f, 0.47f, 0.20f);
        [SerializeField, Min(0f)]
        private float _destroyDelay = 0.08f;

        private bool _isUsed;
        private BrickState _currentState = BrickState.Normal;

        protected override void OnHitFromBelow(PlayerMotor playerMotor)
        {
            switch (_currentState)
            {
                case BrickState.Normal:
                    _isUsed = false;
                    _currentState = BrickState.Damaged;
                    _brickRenderer.color = _usedColor;
                    break;
                case BrickState.Damaged:
                    _isUsed = true;
                    DisableCollision();
                    _brickRenderer.enabled = false;
                    Destroy(gameObject, _destroyDelay);
                    break;
            }

            if (_isUsed)
            {
                return;
            }
        }
    }
}
1. 처리 순서도 (Flowchart)Plaintext[밑에서 충돌 발생 (OnHitFromBelow)]
           │
           ▼
    [벽돌 상태 체크]
           │
 ┌─────────┴─────────┐
 │                   │
[Normal 상태]       [Damaged 상태]
 │                   │
 ├─ 색상 변경          ├─ 사용 상태 설정 (_isUsed = true)
 │ (_usedColor)      ├─ 충돌 판정 제거 (DisableCollision)
 ├─ 상태 변경          ├─ 이미지 비활성화 (_brickRenderer.enabled = false)
 │ (-> Damaged)      └─ 게임 오브젝트 삭제 요청 (Destroy)
 └─ 종료

_brickRendererSpriteRenderer
벽돌의 색상 및 표시 상태 조절

_usedColorColor
1차 타격 시 변경될 색상

_destroyDelayfloat
파괴 시 오브젝트 삭제 지연 시간내부 상태 변수 

 _currentStateBrickState (enum)
 현재 벽돌 내구도 상태
 
 (Normal / Damaged)_isUsedbool
 벽돌 상호작용 종료 여부
