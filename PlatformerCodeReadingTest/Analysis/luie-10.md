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
