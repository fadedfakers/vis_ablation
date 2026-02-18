
### Table 3: Analysis of Class Balancing Strategies (Section 5.3.2)

| Strategy | Method | Obstacle AP (Overall) | Obstacle AP (50-100m) | Mechanism | Generalization Risk |
| :--- | :--- | :---: | :---: | :--- | :--- |
| Baseline | No Balancing | 0.00% | 0.00% | Standard Sampling | Underfitting (Minority Class Ignored) |
| **Offline (Ours)** | **Replication ($\lambda=10$)** | **26.50%** | **18.20%** | **Physical Copy-Paste** | **High (Overfitting to Specific Augmentation Patterns)** |
| Online | ClassBalancedDataset | 0.00% | 0.00% | Loss Reweighting / Resampling | High (Gradient Instability / Context Disruption) |

**Analysis:**
The stark contrast between offline replication (26.50% AP) and online balancing (0.00% AP) suggests that the model's ability to detect small obstacles is heavily dependent on the *consistent repetition* of specific geometric patterns introduced during offline augmentation. While effective on the validation set (which shares the augmentation distribution), this raises concerns that the model may be "memorizing" the local point cloud density and geometry of the pasted samples rather than learning robust, generalized features for obstacle detection. The failure of the online strategy—which alters the sampling frequency without creating identical geometric copies in every epoch—further supports the hypothesis that the network requires repetitive exposure to identical local patterns to converge on these difficult, small-target classes.
