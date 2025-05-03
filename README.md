# Membership Inference Attack Implementation

## Overview

This repository contains an implementation of the membership inference attack described in "Membership Inference Attacks Against Machine Learning Models". The attack aims to determine whether a specific data record was used to train a machine learning model, posing significant privacy implications.

## Project Description

### 1. Introduction

This implementation focuses on the basic version of the membership inference attack, where the adversary has access to data from the same distribution as the target model's training dataset. We evaluate the attack effectiveness on three popular datasets:
- MNIST (handwritten digits)
- CIFAR-10 (10 object classes)
- CIFAR-100 (100 object classes)

The implementation uses PyTorch for the target and shadow models, and LightGBM for the attack model.

### 2. Methodology

#### 2.1 Architecture

**Target and Shadow Models:**
- Standard convolutional neural network (CNN) architecture
- Two convolution layers with max pooling
- Fully connected layer of size 128
- SoftMax output layer
- Tanh activation function (as in the original paper)

**Attack Model:**
- LightGBM classifier
- 10000 estimators
- Regularization lambda: 10

#### 2.2 Training Parameters

- Learning rate: 0.001
- Learning rate decay: 1e-07 per epoch
- Training epochs: 100
- Target train dataset size: 2500
- Test dataset size: 1000
- Number of shadow models: 25
- Batch size: 64
- Momentum: 0.5

### 3. Results and Interpretation

| Dataset   | Accuracy | Precision | Recall  |
|-----------|----------|-----------|---------|
| CIFAR-10  | 76.23%   | 71.78%    | 64.13%  |
| MNIST     | 94.40%   | 93.69%    | 92.45%  |
| CIFAR-100 | 67.97%   | 50.25%    | 50.65%  |

#### Analysis:

- **CIFAR-10**: Successfully reproduced the results from the original paper (median precision: 0.72 in paper vs 0.7178 in our implementation)
- **MNIST**: Very high accuracy and precision, likely indicating strong overfitting in the target model, making it easier for the attack to succeed
- **CIFAR-100**: Precision equivalent to random guessing (around 50%), which aligns with our observations that the target network doesn't learn the dataset effectively (see training curves)

## Project Structure

```
├── data/                 # Dataset storage and handling
│   ├── mnist/
│   ├── cifar10/
│   └── cifar100/
├── models/               # Model definitions
│   ├── target_model.py   # Target model architecture
│   ├── shadow_model.py   # Shadow model architecture
│   └── attack_model.py   # Attack model (LightGBM)
├── training/             # Training scripts
│   ├── train_target.py
│   ├── train_shadows.py
│   └── train_attack.py
├── evaluation/           # Evaluation scripts
│   ├── evaluate_target.py
│   └── evaluate_attack.py
├── results/              # Results and visualizations
│   ├── figures/
│   └── metrics/
├── utils/                # Utility functions
├── configs/              # Configuration files
├── requirements.txt      # Project dependencies
└── README.md             # This file
```

## Installation and Usage

### Prerequisites
- Python 3.8+
- PyTorch 1.9+
- LightGBM
- CUDA (optional, for GPU acceleration)

### Setup

```bash
# Clone the repository
git clone https://github.com/Ishmeet13/Membership_Inference_Attack.git
cd Membership_Inference_Attack

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Running the Experiments

1. **Train target models:**
```bash
python -m training.train_target --dataset cifar10
```

2. **Train shadow models:**
```bash
python -m training.train_shadows --dataset cifar10 --num-shadows 25
```

3. **Train attack model:**
```bash
python -m training.train_attack --dataset cifar10
```

4. **Evaluate attack:**
```bash
python -m evaluation.evaluate_attack --dataset cifar10
```

5. **Generate visualizations:**
```bash
python -m evaluation.generate_figures --dataset cifar10
```

## Extending the Project

You can extend this project by:
1. Implementing more sophisticated attack methods
2. Testing different model architectures
3. Applying defense mechanisms against membership inference
4. Testing on additional datasets

## References

[1] Ke, G., Meng, Q., Finley, T., Wang, T., Chen, W., Ma, W., Ye, Q., & Liu, T. Y. (2017). LightGBM: A highly efficient gradient boosting decision tree. Advances in Neural Information Processing Systems, 30.

[2] Shokri, R., Stronati, M., Song, C., & Shmatikov, V. (2017). Membership inference attacks against machine learning models. 2017 IEEE Symposium on Security and Privacy (SP).

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Citation

If you use this code in your research, please cite:

```
@misc{ishmeet2025membershipattack,
  author = {Ishmeet},
  title = {Membership Inference Attack Implementation},
  year = {2025},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/Ishmeet13/Membership_Inference_Attack}}
}
```
