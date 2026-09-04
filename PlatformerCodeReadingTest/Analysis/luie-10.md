using SchoolEscape.Core;
using SchoolEscape.Player;
using UnityEngine;

namespace SchoolEscape.World
{
    public sealed class StrongBrick : Brick
    {
        [SerializeField]
        private SpriteRenderer _brickRenderer;
        [SerializeField]
        private Color _usedColor = new Color(0.86f, 0.47f, 0.20f);
        [SerializeField, Min(0f)]
        private float _destroyDelay = 0.08f;

        private bool _isUsed;
        private int BrickHP = 1;

        protected override void OnHitFromBelow(PlayerMotor playerMotor)
        {
            switch (BrickHP)
            {
                
                case 1:
                    _isUsed = false;
                    BrickHP--;
                    _brickRenderer.color = _usedColor;
                    break;
                case 0:
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
