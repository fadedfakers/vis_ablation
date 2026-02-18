| Method | 0-50m (Braking) | 50-100m (Warning) | 100-250m (Long) | Overall |
| :--- | :---: | :---: | :---: | :---: |
| **Baseline (With Prior)** | 2.63% | 33.33% | 6.46% | 1.80% |
| **C1 (No Prior)** | **33.33%** | **100.00%** | **22.34%** | **2.46%** |

*Analysis: Removing the geometric prior leads to a massive improvement in AP (e.g., 0-50m improves from 2.63% to 33.33%). This confirms that the geometric prior learned from synthetic data (SOSDaR24) suffers from severe Negative Transfer when applied to the real-world OSDaR23 dataset, causing the "Soft Penalty" mechanism to incorrectly suppress valid obstacles.*
