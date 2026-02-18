## 5.2 C2: Pose Estimation Error Robustness (时空融合中的位姿累积误差)

**Objective**: Evaluate the impact of pose estimation errors (from IMU/Odometry) on the long-range detection performance.

**Quantitative Results (Obstacle AP)**:

| Method | 0-50m (Braking) | 50-100m (Warning) | 100-250m (Long) | Overall |
| :--- | :---: | :---: | :---: | :---: |
| **Baseline (Clean)** | **50.00%** | **100.00%** | **66.67%** | **68.75%** |
| **C2 (Pose Error)** | 50.00% | 2.78% | 8.84% | 5.68% |

**Analysis**:
- The performance drops catastrophically (-63.07% Overall AP).
- Long-range detection (100-250m) is particularly affected (66.67% -> 8.84%).
- **Reasoning**: At 200m range, a 2.8-degree yaw error results in a lateral displacement of $200 \times \tan(2.8^\circ) \approx 9.8$ meters. This causes the temporal fusion (stacking of 3 frames) to fail completely.
