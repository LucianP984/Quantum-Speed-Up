📊 AAPL Tick Data Analysis in Google Colab

This project loads and processes Apple Inc. (AAPL) high-frequency tick data using Google Colab. The primary dataset file is AAPL_Tick.parquet, stored on your Google Drive.
📁 Project Structure

.
├── AAPL_Tick.parquet        # Main dataset (must be uploaded to your Google Drive)
├── your_notebook.ipynb      # Google Colab notebook
└── README.md                # This file

✅ Requirements

    A Google account with access to Google Drive

    Python packages:

        pandas

        pyarrow (for reading parquet files, usually pre-installed in Colab)

🚀 Getting Started

Follow these steps to run the notebook in Google Colab:

    Upload AAPL_Tick.parquet to your Google Drive under:

    /MyDrive/Bachelor Thesis/

    Open the notebook in Google Colab

    Ensure the following code cell is included and executed early in the notebook:

# Mount Google Drive
from google.colab import drive
drive.mount('/content/drive')

    Load the dataset with this code snippet:

import pandas as pd

# Load the data
try:
    data = pd.read_parquet("/content/drive/MyDrive/Bachelor Thesis/AAPL_Tick.parquet")
    print("Data loaded from Google Drive.")
except FileNotFoundError:
    try:
        data = pd.read_parquet("AAPL_Tick.parquet")
        print("Data loaded from local file.")
    except FileNotFoundError:
        print("Error: AAPL_Tick.parquet not found in Google Drive or locally.")
        data = None

📌 Notes

    Adjust the path if your dataset is stored in a different folder on Drive.

    If you're working locally (outside Colab), make sure the file is in your working directory.
