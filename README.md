# Auditing Emails – Streamlit App (Enhanced)

A Python + Streamlit project for **auditing and cleaning email lists** stored in CSV files.

This is an upgraded version of the CLI project, with a web UI that you can deploy and share with clients for deliverability checks.

## Features

- Upload a CSV with contacts (name, email, phone, etc.).
- Auto-detect the email column, or let the user choose it.
- Email hygiene logic:
  - Syntax validation using `email-validator`
  - Domain + registered domain extraction using `tldextract`
  - Optional MX lookup using `dnspython`
  - Duplicate removal (case-insensitive)
  - Filters:
    - Disposable / temporary providers
    - Freemail providers (optional)
    - Role accounts like `info@`, `support@`, etc. (optional)
    - Custom blocklist domains
    - Custom suppression (emails/domains) uploaded as CSV or TXT
- Download:
  - Cleaned email CSV
  - Full audit report CSV with reasons for each decision.

## UI Enhancements

- Live **0–100% progress bar** while processing.
- **Estimated time remaining** and **emails/second throughput** during the run.
- Summary metrics after completion:
  - total rows
  - kept rows
  - dropped rows
  - deliverability keep-rate (%)
- A small success animation (`st.balloons()`) when the audit completes.

## Project structure

```text
auditing_emails_streamlit/
├── app.py
├── README.md
├── requirements.txt
├── auditing_emails/
│   ├── __init__.py
│   ├── rules.py
│   └── email_auditor.py
├── scripts/
│   └── audit_emails.py
├── data/
│   ├── freemail_domains.txt
│   ├── disposable_domains.txt
│   └── blocklist_domains.txt
└── tests/
    └── (placeholder)
```

## Setup

```bash
python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

## Run the Streamlit app

```bash
streamlit run app.py
```

Then open the link shown in the terminal (usually http://localhost:8501).

## Use as CLI (optional)

You can still use the CLI script like this:

```bash
python -m scripts.audit_emails \
  --input contacts.csv \
  --output cleaned_emails.csv \
  --report audit_report.csv \
  --dedupe \
  --require-mx \
  --drop-disposable \
  --drop-freemail \
  --drop-role \
  --suppression suppression.csv
```

This uses the same core `EmailAuditor` as the Streamlit app.
