# AI-Powered Equity Research Report Generator

It uses the Google Gemini model to examine both a company's official annual report and live market data. This generates a thorough report that acts as a first draft for analysts.

## Note

This project is a **Minimum Viable Product (MVP)** aimed at showing the main workflow. As an early version, it requires the user to manually find and upload the company's annual report PDF. The current model is good at interpreting the provided text but doesn't yet do its own calculations for metrics like Discounted Cash Flow (DCF). Future updates will try to focus on automating document retrieval, integrating direct financial calculations, and producing much more detailed reports.


## How It Works

1.  **User Input:** The user selects a stock ticker (e.g., "RELIANCE") and uploads the company's latest annual report in PDF format.
2.  **Data Extraction:** The script extracts text from the first 50 pages of the uploaded PDF
3.  **Live Data Fetching:** It then uses the `yfinance` library to obtain a business summary and key stock price metrics (52-week high/low, last close, 1-year return).
4.  **Prompt Engineering:** The extracted annual report text and live market data are combined into one cohesive prompt, guiding the Gemini model to behave like a senior analyst and organize the output into specific professional sections.
5.  **AI Generation:** The request is sent to the Gemini API, which produces the final, equity research report.

## Setup and Usage

The main setup step involves configuring your API key.

### **1. Get a Google AI API Key**

To start, generate a free API key from Google AI Studio:
- Go to [**Google AI Studio**](https://aistudio.google.com/app/apikey).
- Click **"Create API key"** and copy the generated key.

### **2. Set Up the API Key in Google Colab**

For security, the notebook reads your API key from Colab's built-in Secrets manager.
- Open the notebook in Google Colab.
- Click the **key icon** in the left sidebar to open the "Secrets" tab.
- Click **"Add a new secret"**.
- In the **Name** field, enter `GOOGLE_API_KEY`.
- In the **Value** field, paste the API key you copied.

### **3. Run the Notebook**

Execute the cells in the notebook from top to bottom. You will be asked to upload the annual report PDF when you reach that section.

## Model Selection: Gemini Flash vs. Pro

This project uses the `gemini-1.5-flash-latest` model. While the `pro` model is also very capable, the `flash` model was chosen due to **rate limits**.

The free tier of the Gemini API places limits on both the number of requests you can make per minute (RPM) and the total tokens (words) you can process each minute (TPM). Since this tool sends a lot of text from the annual report, a single request can quickly exceed the free TPM limit of the `pro` model. The `flash` model is designed for speed and usually has more generous free tier limits, making it more suitable for this high-volume task.

## Limitations

While this tool produces a report, it is an MVP with limitations to consider:

-   **Valuation is a not a Calculation:** The AI does **not** perform a mathematical DCF or any other valuation. When it gives a target price, it generates text that *mimics* valuation outputs based on patterns from many reports it has seen.
-   **Dependent on Source Documents:** The report's quality entirely relies on the quality and content of the uploaded annual report. It does not access quarterly reports, conference call transcripts, or other sources.