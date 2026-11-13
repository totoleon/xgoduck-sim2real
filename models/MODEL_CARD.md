# Models

All policies: MLP (512-256-128, ELU) exported to ONNX, input `obs` [1, 61] (gyro, gravity, joint pos/vel, last
action, command / phase, head command), output `actions` [1, 14] (5 left leg, 4 head, 5 right leg, policy order),
50 Hz. Joint/obs conventions are the upstream XgoDuck ones (see https://github.com/LuwuDynamics/xgoduck_rl).

## Walk
| file | action scale | notes (real robot) |
|---|---|---|
| xgoduck_walk_v19.onnx | 1.0 (run at 0.7) | first real-aligned walk; falls walking backward |
| xgoduck_walk_v24.onnx | 0.7 | best sim numbers but walks in circles on the real duck (weak yaw tracking) |
| xgoduck_walk_v25.onnx | 0.7 | tight yaw tracking; with runtime heading hold: stable fwd/back/turn/push |
| xgoduck_walk_v27.onnx | 0.7 | v25 + head safety (@63750). Current default; no head whip backward |

## Get-up
| file | notes |
|---|---|
| xgoduck_getup_real.onnx | first real-aligned get-up (self-collision, battery) |
| xgoduck_getup_v7.onnx | back-height DR 0.4-3.6 cm, beak, over-current cut-out model, real-pose resets, stall penalty, head safety. Real: 9/9, 1.4-4.6 s |

## Skills (`skills/`)
Phase-driven (command = [cos 2πφ, sin 2πφ, 0]) unless `cmd` says otherwise; `skills.json` gives file, period,
cycles, risk and Chinese label. Highlights: dance steps (`dance_waddle`, `dance_sway`, `dance_greet`), peck,
quack_nod, sleepy2, curious, proud, bow (bow_safe), sit/stand, laydown (crouch rest), jumps/hops (medium/high
risk), kicks/one-foot (high risk). Names were checked against what the motion looks like (owner + Gemini blind
naming) and some were renamed accordingly.

Limitations: trained on hard flat floors (friction <= 1.5); on thin grippy carpet several skills tip backward.
