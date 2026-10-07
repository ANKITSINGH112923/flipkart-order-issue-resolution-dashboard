# Flipkart Order Issue Resolution Dashboard

A Python/Pandas internal-tool prototype that unifies order delivery status, refund status, customer complaints, and support tickets into a single order-level dashboard — with transparent, multi-signal priority scoring and business-specific recommended actions.

> **Note:** All data used in this project is synthetic and was generated solely for an educational exercise. It does not represent real Flipkart customers, sellers, or transactions, and no real company data is used at any point.

---

## Problem Statement

E-commerce support and operations teams typically have to check four separate systems — order/logistics, refunds, complaints, and support tickets — to understand the full picture of a single problem order. This project builds a single Colab notebook that:

- Loads and validates data from all four systems (CSV, Excel, JSON, TXT)
- Detects delivery delays, refund risk, complaint sentiment, and ticket escalation independently
- Combines those signals into one order-level feature table **without accidental row duplication**
- Produces a transparent priority score (Critical / High / Medium / Low) with a documented, explainable reasoning trail
- Generates a specific, signal-driven recommended action per order
- Provides a simple search/filter GUI for triage
- Exports a final issue-resolution report as CSV

## Repository Structure

```
.
├── notebooks/
│   └── Flipkart_Order_Issue_Resolution_Dashboard_Completed.ipynb   # main deliverable — run this
├── data/
│   └── flipkart_order_issue_resolution_dataset.zip                 # synthetic input dataset
├── requirements.txt
├── .gitignore
└── README.md
```

## How to Run

### Option 1 — Google Colab (recommended)
1. Open [colab.research.google.com](https://colab.research.google.com) → **File → Open notebook → GitHub** and paste this repo's URL, or upload `notebooks/Flipkart_Order_Issue_Resolution_Dashboard_Completed.ipynb` directly.
2. Run all cells (**Runtime → Run all**).
3. When prompted, upload `data/flipkart_order_issue_resolution_dataset.zip` from this repo.

### Option 2 — Local Jupyter
```bash
git clone https://github.com/ANKITSINGH112923/flipkart-order-issue-resolution-dashboard.git
cd flipkart-order-issue-resolution-dashboard
pip install -r requirements.txt
jupyter notebook notebooks/Flipkart_Order_Issue_Resolution_Dashboard_Completed.ipynb
```
The notebook auto-detects it isn't running in Colab and skips the upload prompt as long as the ZIP is already present in the working directory (copy it from `data/` into the same folder as the notebook, or update `DATASET_ZIP` at the top of the notebook to point at `../data/flipkart_order_issue_resolution_dataset.zip`).

## Key Design Decisions

- **Aggregate before merge:** refunds, complaints, and support tickets are each aggregated to one row per `order_id` *before* being merged into the master orders table, which avoids the row-multiplication bug that raw many-to-one merges cause when an order has more than one refund/complaint/ticket.
- **Nothing is silently deleted:** data-quality issues (duplicate IDs, orphan foreign keys, invalid dates, refund-amount anomalies) are detected, counted, and reported — never dropped without a trace.
- **Transparent scoring:** priority is a documented, additive point system across delay/refund/complaint/ticket/segment signals, with a plain-language `priority_reasons` explanation attached to every order — not a black-box score.
- **No external AI calls:** the AI-ready prompt feature only formats structured text from data already in the final table; no API is called anywhere in the notebook.

## Results Snapshot

| Metric | Value |
|---|---|
| Orders processed | 5,000 |
| Severe delivery delays | 1,590 (32%) |
| Refund-priority candidates | 354 |
| Critical-priority orders | 149 |
| High-priority orders | 903 |
| Final validation checks passed | 12 / 12 |

## Tech Stack

Python · Pandas · NumPy · ipywidgets · Jupyter/Google Colab

## License

This project is for educational purposes. See [LICENSE](LICENSE) for terms.
