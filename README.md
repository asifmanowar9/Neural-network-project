# DDoS Detection — Neural Network Project

This repository demonstrates end-to-end DDoS detection using deep learning (RNN / LSTM / GRU / CNN / MLP) on the CIC-IDS style DDoS dataset. The primary analysis and experiments are implemented in `DDoS.ipynb`.

**Key results and artifacts**
- Trained models: `ddos_detection_model_rnn.keras`, `ddos_detection_lstm_model.keras`
- Scalers: `scaler.pkl`, `scaler_cleaned.pkl`
- Portable test data: `test_samples.csv`
- Main notebook: `DDoS.ipynb`

## Table of Contents
- Overview
- Dataset
- Notebook workflow
- Models implemented
- Reproduce locally
- Run in Google Colab
- Outputs and files
- Notes, limitations and next steps
- License & Contact

## Overview

This project explores feature selection, preprocessing, and multiple neural network architectures for detecting Distributed Denial of Service (DDoS) network traffic. It shows model training, evaluation (confusion matrices, ROC-AUC, F1, MCC), sensitivity analyses (gradients, partial-dependence), SHAP explanations, cross-validation, and comparative benchmarking of inference latency and computational cost.

The notebook focuses on preparing top features using a Random Forest importance ranking, balancing the dataset with SMOTE, scaling, and training sequence models (SimpleRNN, LSTM, GRU) as well as MLP and 1D-CNN baselines.

## Dataset

- Source used in the notebook: `Friday-WorkingHours-Afternoon-DDos.pcap_ISCX.csv` (originally mounted from Google Drive in the notebook). The notebook expects the CSV formatted dataset with a `Label` column containing class names (e.g., `BENIGN`, `DDoS`).
- The repository contains a `Dataset/` folder with ancillary files; if you wish to run the notebook locally, copy the dataset CSV into a convenient path and update the notebook path accordingly.

## Notebook workflow (high-level)

1. Load dataset and basic inspection.
2. Data cleaning: strip column whitespace, replace +/-inf with NaN, drop missing rows.
3. Encode target labels with `LabelEncoder`.
4. Feature selection: drop constant columns, use RandomForest feature importances to pick top-20 features.
5. Scale data with `StandardScaler` and prepare RNN-shaped inputs (samples, time_steps=1, features).
6. Train and evaluate a SimpleRNN model; compute confusion matrix and classification report.
7. Visualize training history (accuracy/loss) and save trained RNN and scaler.
8. Create prediction helpers, a small portable `test_samples.csv`, and an interactive CLI-style prediction helper.
9. Apply SMOTE to balance classes, retrain, and re-evaluate.
10. Perform correlation checks, reduce features, and retrain on the cleaned feature set.
11. Train LSTM, GRU, CNN-1D, and MLP baselines; compare with metrics, inference latency, and resource usage.
12. Explain models with gradients and SHAP, and perform cross-validation.

## Models implemented

- SimpleRNN (stacked RNN) — baseline recurrent model.
- LSTM — gated recurrent architecture.
- GRU — efficient recurrent unit.
- CNN-1D — convolutional feature extractor for tabular sequences.
- MLP — dense network baseline for tabular data.

All models use binary cross-entropy with sigmoid output for a two-class DDoS vs BENIGN task. Early stopping and validation splits are used during later experiments.

## Reproduce locally

Prerequisites
- Python 3.8 or newer
- Recommended virtual environment (venv, conda)

Install required packages (example):

```bash
python -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install pandas numpy scikit-learn seaborn matplotlib tensorflow imbalanced-learn shap jupyterlab
```

Notes:
- `tensorflow` installation may vary by platform; you may prefer `tensorflow-cpu` or a GPU-enabled build.
- `shap` can be resource heavy; use a smaller subset when computing explanations.

Running locally
1. Place the dataset CSV in an accessible path and update the file path in the first notebook cell (the notebook mounts Google Drive by default). Alternatively, set `df_dataset = pd.read_csv("./path/to/Friday-WorkingHours-Afternoon-DDos.pcap_ISCX.csv")`.
2. Start Jupyter and open the notebook:

```bash
jupyter lab  # or jupyter notebook
```

3. Run cells sequentially. The notebook is organized into clear numbered steps.

## Run in Google Colab

The notebook includes a Colab badge and mounting code. To run in Colab:

1. Click the Colab badge at the top of `DDoS.ipynb` or upload the notebook to Colab.
2. When prompted, mount your Google Drive or upload the dataset to the Colab environment.
3. Execute cells in order; Colab provides a convenient GPU/TPU runtime if you enable it (Runtime -> Change runtime type).

## Important files produced by the notebook

- `ddos_detection_model_rnn.keras` — trained SimpleRNN model (native Keras format).
- `ddos_detection_lstm_model.keras` — trained LSTM model.
- `scaler.pkl` — scaler used with the original top-20 features.
- `scaler_cleaned.pkl` — scaler used with the reduced feature set.
- `test_samples.csv` — 10 synthetic samples to quickly test the prediction pipeline.
- `model_architecture.png` — architecture visualization (if plotting utilities run).

## Usage examples

From within the notebook, the helper `predict_traffic()` accepts a DataFrame (with the chosen feature ordering) and returns `(labels, confidences)`.

Example (from the notebook):

```python
# Load test samples
import pandas as pd
samples = pd.read_csv('test_samples.csv')
labels, confidences = predict_traffic(samples)
print(labels, confidences)
```

Interactive prediction
- The notebook defines `interactive_prediction()` which accepts 20 comma-separated feature values (or prompts for manual entry) and prints the predicted class and confidence.

## Results & Evaluation (summary)

- The notebook produces per-model metrics (Accuracy, AUC-ROC, F1-score, MCC, Balanced Accuracy) and confusion matrices.
- It also compares inference latency and parameter counts to help choose an architecture for deployment.
- SHAP and gradient sensitivity analyses are included to interpret feature importance across models.

For exact numeric results, open and run the notebook cells — metrics are computed and displayed as pandas DataFrames and plots throughout.

## Notes, limitations & next steps

- Dataset dependence: the model is trained on a specific DDoS CSV (CIC-style). Performance on other datasets will vary — always validate on representative traffic.
- Feature drift and concept drift are common in network data; consider periodic retraining and online monitoring.
- For production deployment, export models in a serving-friendly format (TensorFlow SavedModel or TensorFlow Lite) and measure end-to-end latency including feature extraction.
- Consider adding packet-level feature extraction code, feature stores, and streaming inference for real-time protection.

## License

This repository is provided for research and educational purposes. No license file is included by default — add a LICENSE if you intend to open-source or distribute the work.

## Contact

If you have questions or want help extending this work, open an issue or contact the author through the project's GitHub repository.
