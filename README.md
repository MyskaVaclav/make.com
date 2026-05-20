# BESS Make.com Scenarios

This repository contains exported Make.com scenario JSON files for implementing automated BESS data acquisition and evaluation.

The scenarios read operational data from the MARFY system, store the collected values in Google Sheets, evaluate predefined operating limits, generate a daily PDF report, upload the report to Google Drive, and send it by e-mail.

## Repository Contents

- `Proces vyčítání dat.json` – scenario for BESS data acquisition
- `Proces vyhodnocení dat.json` – scenario for daily data evaluation and report generation

## Data Acquisition Scenario

The `Proces vyčítání dat` scenario periodically reads BESS operational data from the MARFY system.

The scenario performs authentication against the MARFY web interface, extracts the required antiforgery token and session cookies, computes a stable 30-minute acquisition window aligned to `:00` and `:30`, and requests historical measurement data through an HTTP request on the MARFY SYSTEM.

The returned data are transformed into a structured format using JavaScript modules and stored in Google Sheets.

The stored values include:

- minimum and maximum cell voltage
- minimum and maximum module voltage
- minimum and maximum cell temperature
- minimum and maximum module temperature
- state of charge (SOC)
- DC voltage
- identifiers of cells and modules with extreme values

The scenario also includes retry logic to improve robustness during temporary communication failures.

## Data Evaluation Scenario

The `Proces vyhodnocení dat` scenario runs once every 24 hours.

The scenario loads stored operational data from Google Sheets, filters the previous 24-hour evaluation window according to the `Europe/Prague` time zone, evaluates operating limits, detects violation intervals, and generates a detailed HTML report.

The evaluation includes:

- cell and module temperature limits
- SOC limits
- safe cell and module voltage range
- voltage differences between cells and modules
- temperature differences between cells and modules
- duration of limit violations
- occurrence frequency of cells and modules at extreme values

The generated HTML report is converted into a PDF document using the CustomJS HTML-to-PDF service.

The resulting PDF report is:

- sent by e-mail using Gmail
- uploaded to Google Drive
- archived for later analysis

## Technologies Used

- Make.com
- JavaScript
- Google Sheets
- Google Drive
- Gmail
- HTML/CSS
- PDF

## Requirements

Before running the scenarios, configure the required Make.com connections:

- Google Sheets connection
- Google Drive connection
- Gmail connection
- CustomJS connection for HTML-to-PDF conversion

The following values must also be configured in the imported scenarios:

- MARFY login credentials
- Google Sheets document IDs
- Google Drive folder IDs
- target e-mail address

## Import into Make.com

1. Open Make.com.
2. Import the scenario JSON files.
3. Configure all required connections.
4. Check the MARFY authentication settings.
5. Check the Google Sheets and Google Drive IDs.
6. Activate the scenarios.