# Quantum Speedup for High-Frequency Trading

**BSc (Hons) Computer Science thesis — University of Derby, 2025**
*A Multi-Dimensional Analysis Using Taylor Expansion and Optimization*

Can quantum optimization give high-frequency trading an edge? This thesis benchmarks **hybrid quantum–classical optimization** (Qiskit) against classical optimizers (BFGS, gradient descent) for fitting multi-dimensional Taylor expansions to real AAPL tick data — evaluated on prediction error (MSE), runtime, and trading profitability (P&L, Sharpe ratio).

## Repository contents

```
├── *_Bachelor_Thesis.pdf   # Full 59-page dissertation
├── Thesis_Code.ipynb       # All experiments (Google Colab notebook)
├── AAPL_Tick.parquet       # AAPL high-frequency tick dataset (~21 MB)
└── README.md
```

## The idea

1. **Model:** approximate the AAPL mid-price `(ask + bid) / 2` locally with a quadratic Taylor expansion — in 1D (time) and in multi-dimensional variants (2D/3D/4D) using engineered features such as volume and momentum, with a diagonal quadratic form to keep the parameter count linear in dimensions.
2. **Optimization problem:** find the Taylor coefficients minimizing MSE against the observed tick data.
3. **Quantum approach:** a refined hybrid scheme — a classical random-search initialization, then iterative refinement where quantum circuits (simulated on Qiskit `AerSimulator`; variational/VQE-style techniques) propose coefficient perturbations. Qubits are allocated per coefficient (1D: 6 qubits across a0/a1/a2; multi-D: a configurable qubit budget per feature).
4. **Classical baselines:** the same objective optimized with `scipy.optimize` (BFGS, gradient descent).
5. **Evaluation:** MSE of the fit, wall-clock runtime, and a trading simulation — thresholded signals from each model's predictions, backtested for returns and Sharpe ratio, with comparative visualizations (matplotlib/Plotly).

## Key finding

From the thesis abstract: hybrid quantum optimization (VQE-style) shows substantial potential for finding good Taylor coefficients with minimal computational effort — most notably in the **multi-dimensional settings where classical methods struggle** — while acknowledging the limits of current (simulated/NISQ-era) quantum hardware. The work contributes a concrete performance benchmark for quantum finance.

## Pipeline details

- **Data:** tick-level AAPL quotes (bid/ask price and volume, ms timestamps), loaded from Parquet, sorted, mid-price computed.
- **Features:** min–max normalized time index, price, combined bid+ask volume, and n-tick momentum (`pct_change`).
- **Reproducibility:** fixed random seed; MSE helpers shared between quantum and classical paths so both optimize the identical objective.
- **Stack:** Python, Qiskit (`qiskit-aer`, `qiskit-algorithms`, `qiskit-optimization`), NumPy, pandas, SciPy, matplotlib, Plotly. The setup cell handles both current and legacy Qiskit APIs.

## Running it

Designed for **Google Colab**:

1. Open `Thesis_Code.ipynb` in Colab.
2. Either upload `AAPL_Tick.parquet` to Google Drive under `/MyDrive/Bachelor Thesis/` or place it in the working directory (the loader tries Drive first, then local).
3. Run cells top to bottom — the first cells install the required Qiskit packages.

Note: quantum circuits run on a **simulator** (`AerSimulator`), so "quantum runtime" measures the hybrid algorithm's behavior, not real quantum hardware latency — this caveat is discussed in the thesis.

## Read the thesis

The full dissertation (motivation, methodology, results tables, limitations, and future work) is included as a PDF in this repo.
