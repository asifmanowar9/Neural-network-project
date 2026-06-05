# DDoSGuard-NN: Neural Defense Against DDoS

> A deep learning framework for real-time DDoS attack detection using the CICIDS 2017 dataset.

---

## 📌 Overview

**DDoSGuard-NN** is a research project that designs, implements, and evaluates five distinct deep learning architectures for binary classification of network traffic as either **Benign** or **DDoS**. The framework applies a rigorous feature engineering pipeline and compares models across accuracy, latency, and throughput to identify the most deployable architecture for real-time intrusion detection.

This work was submitted as a thesis project for the degree of **Master of Science in Computer Science and Engineering** at **R. P. Shaha University**, Narayanganj, Bangladesh.

---

## 🧠 Models Evaluated

| Model | Parameters | Test Accuracy | F1-Score | Inference Latency |
|-------|-----------|--------------|----------|-------------------|
| **GRU** ✅ | 17,281 | **99.9247%** | **0.999336** | 0.0604 ms |
| CNN-1D | 20,865 | 99.9158% | 0.999258 | 0.0498 ms |
| MLP | 10,113 | 99.8782% | 0.998926 | 0.0445 ms |
| SimpleRNN | 9,185 | 99.8759% | 0.998906 | — |
| LSTM | 33,473 | 99.8671% | 0.998828 | 0.0809 ms |

> ✅ **GRU** was selected as the optimal architecture based on the best overall balance of accuracy, efficiency, and real-time deployment viability.

---

## 📂 Project Structure

```
├── chapters/
│   ├── introduction.tex
│   ├── literature_review.tex
│   ├── methodology.tex
│   ├── result_discussion.tex
│   └── conclusion.tex
├── figures/
│   ├── ddos_topology.png
│   ├── workflow_diagram.png
│   ├── class_distribution.png
│   ├── metrics_comparison_bar.png
│   ├── confusion_matrices.png
│   ├── inference_latency.png
│   └── inference_throughput.png
├── pages/
│   ├── abstract.tex
│   ├── acknowledgement.tex
│   ├── dedication.tex
│   └── abbr.tex
├── parameters/
│   ├── thesistitle.txt
│   ├── author.txt
│   ├── StudentID.txt
│   ├── degree.txt
│   ├── session.txt
│   ├── thesisdate.txt
│   └── supervisor.txt
├── references.bib
├── ju_cse_msc_thesis.sty
└── main.tex
```

---

## 🔬 Methodology

### Dataset
- **CICIDS 2017** — Canadian Institute for Cybersecurity
- Contains realistic benign and DDoS traffic (LOIC, HOIC, HTTP Flood, TCP SYN Flood)
- 79 raw flow features extracted via CICFlowMeter

### Preprocessing Pipeline
1. **Data Sanitization** — NaN removal and infinite value capping
2. **Dataset Split** — 70% Train / 15% Validation / 15% Test
3. **Random Forest Feature Importance** — Top 20 features retained
4. **StandardScaler Normalization** — Zero mean, unit variance
5. **Pearson Correlation Pruning** — Reduced to 14 final features (threshold > 0.5)
6. **SMOTE** — Applied to training set only to address class imbalance

### Training Setup
- Optimizer: Adam
- Loss: Binary Cross-Entropy
- Activation: ReLU (hidden layers), Sigmoid (output)
- Early Stopping: patience = 10 epochs
- Evaluation: k-fold Cross-Validation

---

## 📊 Key Results

- All five models exceeded **99.86% test accuracy**
- GRU achieved **99.9247% accuracy**, **0.999986 AUC**, and **99.90% live-stream accuracy**
- Cross-validation spread across all models was only **0.03 percentage points**, confirming statistical robustness
- GRU inference latency of **0.0604 ms/sample** confirms real-time deployment viability

---

## 🚀 Future Work

- **Multi-class attack classification** using CIC-DDoS2019 dataset
- **Hybrid CNN–GRU with Multi-Head Attention** for improved temporal sensitivity
- **Real-time SDN integration** via Ryu controller over Mininet topology
- **Adversarial robustness** testing against evasion attacks
- **Federated learning** for privacy-preserving distributed training
- **Edge and IoT deployment** via model quantization and knowledge distillation

---

## 🛠️ Requirements

To compile the LaTeX thesis:

```bash
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

Packages required (included via `ju_cse_msc_thesis.sty`):
- `graphicx`, `amsmath`, `amssymb`, `mathtools`
- `hyperref`, `cleveref`, `float`, `placeins`
- `datatool`, `glossaries`, `setspace`, `geometry`
- `caption`, `multirow`, `listings`, `algorithm`

---

## 📖 Citation

If you use or reference this work, please cite:

```
@thesis{ddosguard2026,
  author  = {[Your Name]},
  title   = {DDoSGuard-NN: Neural Defense Against DDoS},
  school  = {R. P. Shaha University},
  year    = {2026},
  type    = {MSc Thesis},
  address = {Narayanganj, Bangladesh}
}
```

---

## 📜 License

This project is submitted for academic purposes. All rights reserved © 2026.

---

## 🙏 Acknowledgements

Special thanks to the Canadian Institute for Cybersecurity for providing the CICIDS 2017 dataset, and to the supervisor and course teacher for their guidance throughout this research.
