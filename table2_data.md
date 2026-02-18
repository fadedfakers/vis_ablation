# Table 2: Geometry Prior Ablation Analysis

| Configuration | Voxel Size | Rail BEV mIoU | Precision | Recall | False Positives (Pixels) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Phase 1 (Low Res)** | 0.4m | 50.55% | 66.10% | 68.23% | 10,524,293 |
| **Phase 1 (High Res)** | 0.16m | **66.64%** | **69.68%** | **93.85%** | 38,106,218 |

**Analysis:**
- High Resolution (0.16m) significantly improves mIoU (+16.1%) and Recall (+25.6%) compared to Low Resolution (0.4m).
- The higher recall indicates the model captures more of the rail geometry, which is crucial for the "Rail Geometry Prior" strategy.
- False Positives are higher in High Res due to the finer grid (more pixels total), but Precision is also slightly higher, indicating the detections are reliable.
