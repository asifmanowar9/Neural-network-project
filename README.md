# DDoSGuard-NN: Neural Defense Against DDoS

> A deep learning framework for real-time DDoS attack detection and classification.

***

## 📌 Overview

**DDoSGuard-NN** is a lab course project that designs, implements, and evaluates five distinct deep learning architectures for binary classification of network traffic as either **Benign** or **DDoS**. The framework applies a rigorous multi-stage feature engineering pipeline and benchmarks each model across classification accuracy, statistical stability, and real-time inference efficiency to identify the most suitable architecture for production network intrusion detection.

This project was developed as part of the **Computer Science and Engineering** programme at **R. P. Shaha University**, Narayanganj, Bangladesh.

***

## 📂 Project Structure

```
NEURAL-NETWORK-PROJECT/
├── Dataset/
│   └── link.txt        # Download link for the CICIDS 2017 dataset
├── DDoS.ipynb          # Main notebook: preprocessing, training & evaluation
└── README.md
```

***

## 🧠 Models Evaluated

| Model | Parameters | Test Accuracy | F1-Score | Inference Latency |
|-------|-----------|--------------|----------|-------------------|
| **GRU** ✅ | 17,281 | **99.9247%** | **0.999336** | 0.0604 ms |
| CNN-1D | 20,865 | 99.9158% | 0.999258 | 0.0498 ms |
| MLP | 10,113 | 99.8782% | 0.998926 | 0.0445 ms |
| SimpleRNN | 9,185 | 99.8759% | 0.998906 | 0.0736 |
| LSTM | 33,473 | 99.8671% | 0.998828 | 0.0809 ms |

> ✅ **GRU** was selected as the optimal architecture based on the best overall balance of classification performance, parameter efficiency, and real-time inference viability.

***

## 🔬 Methodology

### Dataset
- **CICIDS 2017** — Canadian Institute for Cybersecurity
- Contains realistic benign and DDoS traffic: LOIC, HOIC, HTTP Flood, TCP SYN Flood
- 79 raw flow features extracted via CICFlowMeter
- Download link provided in `Dataset/link.txt`

### Preprocessing Pipeline
1. **Data Sanitization** — NaN row removal and infinite value capping
2. **Dataset Split** — 70% Train / 15% Validation / 15% Test
3. **Random Forest Feature Importance** — Top 20 features selected
4. **StandardScaler Normalization** — Zero mean, unit variance
5. **Pearson Correlation Pruning** — Final 14 features retained (threshold > 0.5)
6. **SMOTE** — Applied to training set only to address class imbalance

### Training Configuration
- **Optimizer:** Adam
- **Loss Function:** Binary Cross-Entropy
- **Hidden Activation:** ReLU
- **Output Activation:** Sigmoid
- **Early Stopping:** patience = 10 epochs
- **Validation:** k-fold Cross-Validation

***

## ⚙️ Model Architectures

```python
# MLP
Dense(64, relu) → Dropout(0.2) → Dense(32, relu) → Dense(1, sigmoid)

# CNN-1D
Conv1D(64, kernel_size=1, relu) → GlobalMaxPooling1D() → Dense(32, relu) → Dense(1, sigmoid)

# GRU
GRU(64) → Dense(32, relu) → Dense(1, sigmoid)

# SimpleRNN
SimpleRNN(64, relu) → Dense(32, relu) → Dense(1, sigmoid)

# LSTM
LSTM(64) → Dense(32, relu) → Dense(1, sigmoid)
```

All models compiled with: `optimizer='adam'`, `loss='binary_crossentropy'`

***

## 📊 Key Results

- All five models exceeded **99.86% test accuracy**
- **GRU** achieved the best overall results: **99.9247% accuracy**, **AUC 0.999986**, **99.90% live-stream accuracy**
- Cross-validation spread across all models was only **0.03 percentage points**, confirming statistical robustness
- GRU inference latency of **0.0604 ms/sample** confirms compatibility with real-time network monitoring

***

## 🛠️ Installation & Usage

### 1. Clone the repository
```bash
git clone https://github.com/asifmanowar9/NEURAL-NETWORK-PROJECT.git
cd NEURAL-NETWORK-PROJECT
```

### 2. Install dependencies
```bash
pip install tensorflow scikit-learn imbalanced-learn pandas numpy matplotlib seaborn jupyter
```

### 3. Download the dataset
Open `Dataset/link.txt` and download the CICIDS 2017 dataset, then place the CSV files inside the `Dataset/` folder.

### 4. Run the notebook
```bash
jupyter notebook DDoS.ipynb
```

***

## 🔮 Future Work

- Multi-class attack classification using CIC-DDoS2019 dataset
- Hybrid CNN–GRU with Multi-Head Attention for improved temporal sensitivity
- Real-time SDN integration via Ryu controller over Mininet topology
- Adversarial robustness testing against traffic evasion attacks
- Federated learning for privacy-preserving distributed training
- Edge and IoT deployment via model quantization and knowledge distillation

***

## 📜 License

This project was developed for academic and educational purposes. All rights reserved © 2026.

***

## 🙏 Acknowledgements

Special thanks to the Canadian Institute for Cybersecurity for providing the publicly available CICIDS 2017 dataset used in this project.