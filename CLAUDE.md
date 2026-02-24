# CLAUDE.md

## Repository Overview

This repository hosts open-sourced ONNX neural network policy files for humanoid robots. The policies are trained reinforcement learning controllers exported in ONNX format for deployment.

## Repository Structure

```
onnx_policy/
├── unitree/
│   └── g1/
│       ├── unitree_locomotion/   # Official Unitree G1 locomotion policy + metadata
│       ├── whole_body_tracking/  # Whole-body motion tracking policies (e.g., dancing, idle)
│       ├── boxing/               # Boxing/martial arts motion policies
│       ├── legged_lab/           # AMP-based locomotion policy
│       └── holosoma_locomotion/  # Holosoma locomotion policies (fastsac variants)
└── agibot/
    └── x2/
        ├── whole_body_tracking/  # Whole-body motion tracking policies
        ├── boxing/               # Boxing/martial arts motion policies
        └── holosoma_locomotion/  # Holosoma locomotion policy
```

## File Formats

- **`.onnx`** — Neural network policy files stored via Git LFS. These are the deployable controllers.
- **`*_metadata.json`** — Machine-readable policy metadata (observation/action dimensions, joint names, PD gains, scaling factors, RNN config).
- **`*_metadata.txt`** — Human-readable version of the same metadata.

## Policy Metadata Fields

| Field | Description |
|---|---|
| `num_observations` | Observation vector dimension fed to the policy |
| `num_actions` | Action vector dimension (number of controlled joints) |
| `action_scale` | Scalar multiplier applied to policy output |
| `joint_names` | Ordered list of joint names matching action indices |
| `default_joint_pos` | Default/nominal joint positions (radians) |
| `kp` / `kd` | PD controller stiffness and damping gains per joint |
| `observation_names` | Ordered list of observation components |
| `*_scale` | Scaling factors for each observation component |
| `is_recurrent` | Whether the policy uses a recurrent network (LSTM) |
| `rnn_type` / `rnn_hidden_size` / `rnn_num_layers` | RNN architecture parameters |

## Git LFS

All `.onnx` files are tracked by Git LFS (see [.gitattributes](.gitattributes)). Ensure `git-lfs` is installed before cloning:

```bash
git lfs install
git clone <repo-url>
```
