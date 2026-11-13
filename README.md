# xgoduck-sim2real (weights)

ONNX policies for the Luwu / XGO-Duck robot duck (Arduino UNO Q + 15 Feetech HLS1910 servos, ~0.8 kg), trained in
simulation (mjlab + rsl_rl PPO) and tested on the real robot: walk, get-up and 39 skills (dances, peck, bow, sit/stand,
jumps, ...).

This release contains the **model weights only**. Training code, runtime changes and tools may follow later.

## Contents
| folder | content |
|---|---|
| `models/walk/` | walk v19, v24, v25, **v27** (current default) |
| `models/getup/` | getup_real, **getup_v7** (9/9 real get-ups, 1.4-4.6 s) |
| `models/skills/` | 39 phase-driven skills + `skills.json` (period, cycles, risk, Chinese label) |
| `models/MODEL_CARD.md` | inputs/outputs, action scales, notes on real-robot behaviour and limitations |

## Interface (all policies)
MLP 512-256-128 (ELU), input `obs` [1, 61] (gyro, gravity, joint pos/vel, last action, command / phase, head
command), output `actions` [1, 14] (5 left leg, 4 head, 5 right leg), 50 Hz. Observation and joint conventions follow
[LuwuDynamics/xgoduck_rl](https://github.com/LuwuDynamics/xgoduck_rl).

## Key results (real robot)
- Walk v25/v27 with a runtime heading hold: stable forward / backward / turning and push recovery on hard floors.
- Get-up v7: reliable after the sim was fixed to match the real robot (the board on the back raises the trunk
  2.5-3.6 cm, the beak wedges between the legs).
- Known gaps: on thin grippy carpet the walk and several skills can tip backward.

## License
Apache-2.0 (see `LICENSE`, `NOTICE`).
