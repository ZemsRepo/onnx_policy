# ONNX Policy Zoo

Open-sourced ONNX neural network policy files for humanoid robots

## Overview

This repository provides pre-trained reinforcement learning controllers exported in ONNX format for direct deployment on humanoid robots. Policies cover locomotion, whole-body motion tracking, and combat/sports motions.

## Supported Robots

| Robot | Manufacturer | DOF |
|---|---|---|
| G1 | Unitree Robotics | 29 |
| X2 | Agibot | 29 |

## Repository Structure

```
onnx_policy/
├── unitree/
│   └── g1/
│       ├── unitree_locomotion/   # Locomotion policy with metadata
│       ├── whole_body_tracking/  # Motion tracking (dancing, idle, kicks, ...)
│       ├── boxing/               # Boxing and MMA strike motions
│       ├── legged_lab/           # AMP-based locomotion
│       └── holosoma_locomotion/  # Holosoma fastsac locomotion variants
└── agibot/
    └── x2/
        ├── whole_body_tracking/  # Motion tracking (dancing, idle, look around, ...)
        ├── boxing/               # Boxing and MMA strike motions
        └── holosoma_locomotion/  # Holosoma locomotion
```

## Getting Started

### Prerequisites

- [Git LFS](https://git-lfs.com/) — all `.onnx` files are stored via Git LFS.
- [ONNX Runtime](https://onnxruntime.ai/) — to run inference with the policies.

### Clone

```bash
git lfs install
git clone https://github.com/IO-AI-TECH/onnx_policy.git
```

### Running Inference

Each policy is a standard ONNX model. Load it with ONNX Runtime:

```python
import onnxruntime as ort
import numpy as np

session = ort.InferenceSession("unitree/g1/unitree_locomotion/g1_policy.onnx")
input_name = session.get_inputs()[0].name

obs = np.zeros((1, 47), dtype=np.float32)  # num_observations = 47
action = session.run(None, {input_name: obs})[0]
```

Refer to the accompanying `*_metadata.json` for the correct observation dimension, joint order, PD gains, and scaling factors for each policy.

## Policy Metadata

For policies that include a metadata file, the fields are:

| Field | Description |
|---|---|
| `num_observations` | Observation vector dimension |
| `num_actions` | Number of controlled joints |
| `action_scale` | Scalar multiplier applied to policy output |
| `joint_names` | Ordered joint names matching action indices |
| `default_joint_pos` | Nominal joint positions (radians) |
| `kp` / `kd` | PD stiffness and damping gains per joint |
| `observation_names` | Ordered observation components |
| `*_scale` | Scaling factors for each observation component |
| `is_recurrent` | Whether the policy uses an LSTM |
| `rnn_type` / `rnn_hidden_size` / `rnn_num_layers` | RNN architecture (if recurrent) |

## License

This project is licensed under the [MIT License](LICENSE).