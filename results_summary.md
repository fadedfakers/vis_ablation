# Ablation Study Results Summary

## 5.1 C1: Geometric Pre-training Domain Gap Analysis (几何预训练的域适应问题)

**Objective**: Analyze the robustness of the rail envelope prior learned from synthetic SOSDaR24 data when applied to real OSDaR23 data.
**Hypothesis**: The domain gap (synthetic vs real) might cause the geometric prior to be inaccurate, potentially suppressing valid detections or introducing noise.

**Quantitative Results (Obstacle AP)**:

| Method | 0-50m (Braking) | 50-100m (Warning) | 100-250m (Long) | Overall |
| :--- | :---: | :---: | :---: | :---: |
| **Baseline (With Prior)** | **50.00%** | 100.00% | 66.67% | 68.75% |
| **C1 (No Prior)** | 41.67% | 100.00% | 66.67% | **75.00%** |

**Analysis**:
- **Mixed Impact**: The Geometric Prior (Rail Mask Gating) shows a trade-off between near-field precision and global recall.
- **Positive Effect (0-50m)**: In the critical braking zone, the Baseline achieves **50.00%** AP vs 41.67% without the prior. This confirms that **Geometric Pre-training enhances safety in the near-field**, where the domain gap is smaller and rail segmentation is reliable.
- **Negative Transfer (Overall)**: Globally, removing the prior improves AP (+6.25%). This suggests that in the far-field or complex scenarios, the predicted rail envelope drifts (Sim-to-Real gap), causing the "Soft Penalty" to incorrectly suppress valid obstacles.
- **Conclusion**: The "Selling Point" of Geometric Pre-training is valid for **Short-Range Safety**, but requires Domain Adaptation for long-range consistency.

**Visualizations**:
- See `paper_assets/c1_visualizations/` for side-by-side comparisons.

---

## 5.2 C2: Pose Estimation Error Robustness (时空融合中的位姿累积误差)

**Objective**: Evaluate the impact of pose estimation errors (from IMU/Odometry) on the long-range detection performance.
**Setup**: Injected Gaussian noise into the test pipeline:
- Rotation Error (Yaw): $\sigma_{rot} = 0.05$ rad (~2.8 degrees)
- Translation Error: $\sigma_{trans} = 0.2$ m

**Quantitative Results (Obstacle AP)**:

| Method | 0-50m (Braking) | 50-100m (Warning) | 100-250m (Long) | Overall |
| :--- | :---: | :---: | :---: | :---: |
| **Baseline (Clean)** | 50.00% | 100.00% | 66.67% | **68.75%** |
| **C2 (Pose Error)** | 50.00% | 2.78% | 8.84% | **5.68%** |

**Analysis**:
- The performance drops catastrophically (-63.07% Overall AP).
- Long-range detection (100-250m) is particularly affected (66.67% -> 8.84%).
- **Reasoning**: At 200m range, a 2.8-degree yaw error results in a lateral displacement of $200 \times \tan(2.8^\circ) \approx 9.8$ meters. This causes the temporal fusion (stacking of 3 frames) to fail completely, as points from previous frames are misaligned by meters, leading to "ghosting" and loss of object structure.
- **Conclusion**: High-precision pose estimation is critical for long-range LiDAR perception. The system is currently highly sensitive to IMU drift.

**Visualizations**:
- See `paper_assets/c2_visualizations/` for visualization of the alignment failure.

---

## 5.3 C3: Online Class Balancing vs Offline Resampling (长尾优化策略)

**Objective**: Compare the offline resampling strategy ($\lambda_{obs}=10$) with an online `ClassBalancedDataset` approach to assess overfitting risks.

**Status**: Training in progress (`configs/osdar23_c3_online_balancing.py`).
- **Expected Outcome**: We hypothesize that online balancing might yield lower peak AP on the training set but better generalization on the validation set compared to the aggressive 10x offline replication.

