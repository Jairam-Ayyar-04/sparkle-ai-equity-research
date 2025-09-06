# AI-Powered Equity Research Report Generator

This project provides a Jupyter Notebook (`.ipynb`) that generates a comprehensive, multi-section equity research report for stocks listed on the National Stock Exchange (NSE) of India.

It leverages the Google Gemini model to analyze both a company's official annual report and live market data, producing a detailed report that serves as a powerful first draft for investors and analysts.

## Note
Please note that this project is a **Minimum Viable Product (MVP)** designed to demonstrate the core workflow. As an early version, it requires the user to manually find and upload the company's annual report PDF. The current model excels at synthesizing and analyzing the provided text but does not yet perform its own mathematical calculations for metrics like Discounted Cash Flow (DCF). Future versions will aim to automate document retrieval, integrate direct financial metric calculations, and generate significantly more comprehensive and quantitative reports.
---

## How It Works

The process is designed to synthesize detailed financial documents with real-time data to create an insightful report:

1.  **User Input:** The user defines a target stock ticker (e.g., "RELIANCE") and is prompted to upload the company's latest annual report in PDF format.
2.  **Data Extraction:** The script extracts text from the first 50 pages of the uploaded PDF, capturing key information like the management's discussion, business overviews, and financial summaries.
3.  **Live Data Fetching:** It then uses the `yfinance` library to pull the latest available market data, including a business summary and key stock price metrics (52-week high/low, last close, 1-year return).
4.  **Prompt Engineering:** The extracted annual report text and the live market data are combined into a single, comprehensive prompt. This prompt instructs the Gemini model to act as a senior analyst and structure its output into specific, professional sections.
5.  **AI Generation:** The request is sent to the Gemini API, which generates the final, detailed equity research report.

---

## Setup and Usage

The notebook is largely self-explanatory, but the key setup step is configuring your API key.

### **1. Get a Google AI API Key**

First, you need to generate a free API key from Google AI Studio:
- Go to [**Google AI Studio**](https://aistudio.google.com/app/apikey).
- Click **"Create API key"** and copy the generated key.

### **2. Set Up the API Key in Google Colab**

For security, the notebook is designed to read your API key from Colab's built-in Secrets manager.

- Open the notebook in Google Colab.
- Click the **key icon** in the left sidebar to open the "Secrets" tab.
- Click **"Add a new secret"**.
- In the **Name** field, enter `GOOGLE_API_KEY`.
- In the **Value** field, paste the API key you copied.

### **3. Run the Notebook**

Run the cells in the notebook from top to bottom. You will be prompted to upload the annual report PDF when you reach that step.

---

## Model Selection: Gemini Flash vs. Pro

This project uses the `gemini-1.5-flash-latest` model. While the `pro` model is also highly capable, the `flash` model was chosen for a key reason: **rate limits**.

The free tier of the Gemini API has limits on not just how many requests you can make per minute (RPM), but also on how many total tokens (words) you can process per minute (TPM). Because this tool sends a large amount of text from the annual report, a single request can easily exceed the free TPM limit of the `pro` model. The `flash` model is optimized for speed and often has more generous free-tier limits, making it more reliable for this high-volume token task.

---

## Limitations

While this tool produces a high-quality and detailed report, it is an MVP with key limitations to be aware of:

-   **Valuation is a Synthesis, Not a Calculation:** The AI does **not** perform a mathematical DCF or any other valuation. When it provides a target price, it is generating text that *mimics* the output of a valuation based on patterns from countless reports it was trained on. The numbers are plausible but not the result of a verifiable calculation.
-   **Dependent on Source Documents:** The report's quality is entirely dependent on the quality and content of the uploaded annual report. It does not have access to quarterly reports, conference call transcripts, or other sources.
-   **Data Source Reliability:** The `yfinance` library depends on Yahoo Finance, which can occasionally have temporary data availability issues for certain tickers.