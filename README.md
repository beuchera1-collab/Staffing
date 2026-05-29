[README.md](https://github.com/user-attachments/files/28379933/README.md)
# Predict Optimal Nurse Staffing Levels for Emergency Departments

Final project for the [Building AI course](https://buildingai.elementsofai.com)

## Summary

This solution aims to predict the right number of nursing staff needed each shift in hospital emergency departments — ensuring patient safety while reducing burnout from chronic understaffing and wasted cost from overstaffing. The model uses historical admission records, time-of-day patterns, seasonal illness trends, and local event data to output a recommended headcount and confidence interval for each upcoming 8-hour shift.

## Background

Emergency departments face an unpredictable demand problem. A flu outbreak can triple patient volume overnight. A sunny holiday weekend may be deceptively quiet, then suddenly overwhelmed by accidents. Staffing decisions are traditionally made days in advance based on rough seasonal intuition — a method that is both imprecise and inflexible.

The consequences are severe in both directions:

* Understaffing leads to dangerously long wait times, missed critical cases, and nurse burnout — a leading cause of healthcare worker attrition.
* Overstaffing wastes scarce hospital resources — a single 12-hour RN shift can cost $500–$900 with agency rates, and hospitals may overspend millions per year unnecessarily.

This problem is compounded by the fact that each hospital is unique — a rural ED behaves very differently from an urban trauma center. A data-driven approach tailored to each facility's patterns could dramatically improve outcomes.

The two core problems this project addresses:

* problem 1: predicting the minimum safe nurse-to-patient ratio for each upcoming shift.
* problem 2: identifying early signals (search trends, weather, local events) that precede volume surges.

## How Is It Used?

The tool takes the form of a web dashboard used daily by ED charge nurses and operations managers. Each morning, the manager opens the dashboard to see a 72-hour staffing forecast — a timeline of recommended headcount per shift, color-coded by confidence level.

The manager can override any recommendation and flag unusual local events (concerts, sporting events, school outbreaks) that the model may not yet know about. These overrides feed back into the model as labeled training examples, improving it over time.

People affected by the system include:

* **Nursing staff** — whose schedules become more predictable and less chaotic.
* **Patients** — who receive more timely care and shorter wait times.
* **Hospital administrators** — who gain a tool for budget justification and accreditation reporting.

## Data Sources and AI Methods

The model depends on two categories of data:

**Internal hospital data:** Historical ED admission records (date, time, acuity level, diagnosis category), shift-level nurse counts, and patient-to-nurse ratios going back at least 3 years. Most hospitals already store this in their EMR system (Epic, Cerner, etc.).

**External signals:** Local weather forecasts, regional flu surveillance data (CDC FluView), local event calendars, and optionally Google Trends data for symptoms like "chest pain" or "fever" in the local area.

The AI approach combines a gradient-boosted tree model (XGBoost) for structured tabular features with a lightweight LSTM layer to capture multi-week seasonal trends. The output is a point estimate of patient volume per shift plus a prediction interval. Staffing levels are then derived using historical nurse-to-patient ratio targets as a policy layer on top of the volume forecast.

## Challenges

This project will never be able to reliably predict staffing needs during mass casualty events, severe weather disasters, or sudden disease outbreaks — these fall outside any historical distribution and emergency override protocols must always take precedence.

Patient data is highly sensitive under HIPAA. Even aggregate admission records require careful data governance, de-identification, and institutional review before they can be used for model training. Smaller community hospitals may lack the IT infrastructure to extract this data cleanly.

Nurse managers may distrust algorithmic recommendations, especially if the model has been wrong in memorable past cases. Building trust requires a transparent UI that explains *why* a given staffing level was suggested — not just *what* the number is.

Union agreements and nursing contracts constrain how quickly staffing levels can actually be adjusted — the model must output recommendations far enough in advance to be actionable within scheduling rules.

## What Next?

The most immediate next step is piloting the system at a single mid-sized hospital ED to validate forecast accuracy and measure impact on staffing cost and patient wait times.

Longer-term expansions include multi-department support (ICU, surgical floors), integration with nurse scheduling software like Kronos or NurseGrid, and an alert system that pages on-call staff automatically when a volume surge is predicted.

The model could also be extended to incorporate real-time ambulance dispatch feeds, giving a 30–90 minute early warning window before high-acuity patients arrive.

## Acknowledgments

* [elementsofai.com](https://www.elementsofai.com) — Building AI course framework and project structure
* [CDC FluView](https://www.cdc.gov/flu/weekly/fluviewinteractive.htm) — Regional flu surveillance data
* [XGBoost](https://xgboost.readthedocs.io) — Gradient boosting library
* [HIPAA Safe Harbor guidelines](https://www.hhs.gov/hipaa/for-professionals/privacy/special-topics/de-identification/index.html) — De-identification standards
