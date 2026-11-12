# Mapping-the-International-Patient-Pathway-An-Analytical-Study-on-Conversion-Drivers
Operational &amp; Metrics Analysis (Action Learning Project): Designed and deployed interactive Power BI, Excel, and HTML dashboards for tracking IPD footfalls, query TAT, and enquiry-to-billed-revenue drop-offs to optimize service margins and support executive decision-making, KPI analysis and HTML dashboard designing.

An interactive analytics dashboard and supporting data registers that trace the **international patient journey** at a super-speciality hospital in Delhi: from the first enquiry, through clinical engagement and IPD conversion, to admission and realised revenue (**Jan 2025 – Jun 2026**).

The project sits at the intersection of **clinical healthcare and healthcare data analytics / administration**. It turns five operational registers kept by an International Patient Services (IPS) desk into a single, filterable view of where leads come from, where they drop off, how fast the team responds, and what the converted patients are worth.

---

## Headline results

| Metric | Value |
|---|---|
| Total enquiries logged | **2,453** |
| Engaged leads (IPD/OPD log) | **865** (35.3% of enquiries) |
| IPD leads converted | **400** (16.3% query → IPD conversion) |
| Query-sourced admissions | **383** of 2,445 total admissions |
| Revenue realised | **$4.02M** across 409 billed IPD episodes |
| Average ticket size | **$9,821** per converted IPD patient |
| Average length of stay | **5.5 days** |
| Queries beyond the 24-hour TAT | **3.9%** (93 of 2,397) |

> Figures are taken from the dashboard's default view (all months, no filters).

---

## What's in the dashboard

`International_Patient_Funnel_Dashboard.html` is a single, self-contained page with 16 sections:

1. **Programme scorecard**: headline volume, conversion and revenue KPIs
2. **Monthly trend**: enquiries, conversion and revenue by month
3. **Where leads come from**: region, country, channel and partner performance
4. **Conversion & revenue**: lead quality, speciality mix, treatment revenue
5. **Patient mix & operations**: nationality, ward preference, response time, ALOS
6. **Team performance**: revenue by coordinator and consulting doctor
7. **Patient journey**: stage-by-stage view with benchmarks and risks at each gate
8. **What-if revenue simulator**: sliders to model conversion and ticket-size scenarios
9. **SOP timeline & TAT benchmarks**: six operational checkpoints with measured turnaround
10. **Deep segmentation**: sortable, searchable performance tables
11. **Patient records explorer**: row-level view of all 409 billed IPD patients
12. **Doctor directory**: 35 consulting specialists with patient and revenue counts
13. **KPI formula reference**: how every metric is defined
14. **SWOT & strategic recommendations**
15. **Loopholes & operational bottlenecks**: 10 documented process-audit findings, cross-checked against the data
16. **Source data registers**: browse and export the underlying registers as CSV

Global filters (month range, region, partner, channel, speciality and more) apply across the page, with a one-click reset.

### Funnel stages

```
Queries received (2,453)
      ↓  35.3%
Engaged: IPD/OPD log (865)
      ↓
Converted IPD leads (400)  →  Query-sourced admissions (383)  →  Billed episodes & revenue ($4.02M)
```

### Key KPI definitions

| Metric | Formula |
|---|---|
| Engagement rate | Engaged leads ÷ Total enquiries × 100 |
| Query → IPD conversion | Converted IPD leads ÷ Total enquiries × 100 |
| OPD → IPD conversion | Converted leads ÷ Tracked OPD + IPD leads × 100 |
| Admission realisation | Query-sourced admissions ÷ Converted IPD leads × 100 |
| Avg. ticket size (ARPP) | Total realised revenue ÷ Billed IPD episodes |
| First-response TAT | Mean(Closing date-time − Receiving date-time), in hours |
| ALOS | Mean inpatient length of stay, in days |
| Funnel "% carried" | Stage N ÷ Stage N−1 × 100 |
| Funnel "% of total" | Stage N ÷ Stage 1 × 100 |

The full table is in Section 13 of the dashboard.

---

## Repository contents

```
.
├── README.md
├── dashboard/
│   └── International_Patient_Funnel_Dashboard.html
└── data/
    ├── International_Patients_Query_Track_2025_2026.xlsx
    ├── Monthly_Admissions_Log_2025_2026.xlsx
    └── Monthly_International_Queries_with_TAT_v2.xlsx
```

### Data registers

| File | Sheets | Rows | Description |
|---|---|---|---|
| `International_Patients_Query_Track_2025_2026.xlsx` | `Query Tracking`, `IPD & OPD Log` | 2,453 enquiries; 865 engaged leads (415 IPD, 450 OPD) | Every enquiry with channel, partner, country/region, doctor, speciality, receive/close dates, remarks and plan number; the second sheet holds enquiries that progressed to IPD/OPD |
| `Monthly_Admissions_Log_2025_2026.xlsx` | 18 monthly sheets (`Admissions_Jan_2025` … `Admissions_Jun_2026`) | 2,445 admissions | Admission ID, date of admission, age, gender, doctor, speciality, treatment, ALOS, ward type, nationality and conversion source (Walk-in 1,249 · Client Referral 813 · Query Lead 383) |
| `Monthly_International_Queries_with_TAT_v2.xlsx` | 18 monthly sheets (`Jan_2025` … `Jun_2026`) | 2,397 queries | Query-level receive/close timestamps with computed **TAT (hours)** and an in/out-of-TAT flag (2,304 inside TAT · 93 exceeded) |

The dashboard also embeds the **Lead Conversion** register (2,399 rows, with HOT/WARM/COLD status, coordinator and revenue) and the **Converted IPD Master** (409 billed episodes). These are included inside the HTML file and can be browsed and exported from Section 16.

### Data dictionary (core fields)

| Field | Meaning |
|---|---|
| `Plan No.` | Treatment-plan reference (`PLN-YYYYMM-NNNN`) |
| `Subj. Line` | Enquiry channel: Website, Email, WhatsApp, Call or the internal desk |
| `Partners` | Referring facilitator / partner agency (9 partners) |
| `Region` | Middle East, East Africa, South Asia, Central Asia, West Africa |
| `Remarks` | Pipeline status, e.g. Query Processed, Under Evaluation, Medical Opinion Shared, Visa Assistance Provided, Patient Traveling |
| `Conversion Source` | How the admission originated: Walk-in, Client Referral or Query Lead |
| `TAT (Hours)` | Closing minus receiving date-time; the SOP target is 24 hours |

---

## How to use

**View the dashboard**
- Download `International_Patient_Funnel_Dashboard.html` and open it in any modern browser. No build step, server or installation is needed.
- Or host it with GitHub Pages: *Settings → Pages → Deploy from branch*, then open `/dashboard/International_Patient_Funnel_Dashboard.html`.
- The only external resource is Google Fonts (Fraunces, Inter, IBM Plex Mono). The page still works offline with fallback fonts.

**Explore the data**
- Open the `.xlsx` files in Excel or Google Sheets, or load them in Python:

```python
import pandas as pd

queries = pd.read_excel("data/International_Patients_Query_Track_2025_2026.xlsx",
                        sheet_name="Query Tracking")

# Admissions: one sheet per month
admissions = pd.concat(
    pd.read_excel("data/Monthly_Admissions_Log_2025_2026.xlsx", sheet_name=None).values(),
    ignore_index=True,
)

# TAT log: one sheet per month
tat = pd.concat(
    pd.read_excel("data/Monthly_International_Queries_with_TAT_v2.xlsx", sheet_name=None).values(),
    ignore_index=True,
)
print(tat["TAT (Hours)"].describe())
```

---

## Notable findings from the process audit

The dashboard's bottleneck section documents 10 findings, grouped as:

- **Documentation & compliance**: critical errors in regulatory documentation
- **Billing & estimates**: untagged medical estimates; unclear in-house vs. outside cost communication
- **Clinical coordination**: prolonged admissions and delayed diagnostics; excessive TAT on international queries (3.9% exceed 24 hours, and eight consultants average 12.4–12.8 hrs against an 11.7 hr fleet average)
- **Infrastructure & governance**: one shared front desk handling ~136 admissions a month; dual IPS office confusion
- **Regulatory / visa**: FRRO visa-extension delays caused by Medical Superintendent sign-off
- **Patient support**: interpreter unavailability and unresponsive communication lines
- **Data governance**: no shared patient ID linking Admissions to billed revenue, so Query-Lead admissions cannot be tied directly to revenue

---

## Tech stack

- **Dashboard:** HTML, CSS and vanilla JavaScript in a single file; data embedded as JSON; no frameworks or build tooling
- **Data:** Microsoft Excel (`.xlsx`) registers
- **Analysis:** KPI logic implemented in the dashboard; Python/pandas snippet provided for further exploration

---

## Data & privacy note

This repository contains patient-level records. All patient names and identifiers in the data are **anonymised / synthetic placeholders and do not correspond to real individuals**. Do not add real patient-identifiable information to this repository.

---

## Possible extensions

- Add a shared patient ID so admissions can be linked to billed revenue end to end
- Cohort and survival analysis of enquiry-to-admission lag
- Predictive lead scoring using channel, partner, region and speciality
- Automated SLA alerts when a query approaches the 24-hour TAT
- Port the dashboard to Power BI or Tableau for production reporting

---

## Author

**Shubhi**: BDS, MBA (Healthcare Administration), IIHMR University.
Portfolio project bridging clinical healthcare and healthcare data analytics.

---

## Disclaimer

This is an academic / portfolio project. It is not affiliated with, endorsed by, or an official publication of any hospital or organisation. Findings reflect the supplied dataset only.
