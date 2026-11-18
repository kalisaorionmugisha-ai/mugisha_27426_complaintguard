# ComplaintGuard: Consumer Complaint Resolution Tracking and Regulatory Escalation System

**Student:** Mugisha Kalisa Orion
**Student ID:** 27426
**Course:** INSY 8311 – Database Development with PL/SQL
**Instructor:** Eric Maniraguha

---

## Problem Statement
Financial institutions receive thousands of consumer complaints monthly, but tracking them in spreadsheets or email inboxes causes regulatory response deadlines to be missed and systemic company failures to go unnoticed. The CFPB received over 2.7 million credit reporting complaints in 2024 alone, representing an 85% share of all complaints and a 106% increase from the previous year. Manual tracking cannot scale to enforce deadlines or detect patterns. ComplaintGuard solves this by using PL/SQL to automatically calculate deadlines, enforce resolution logging, and flag non-compliant companies.

## Objectives
**Main objective:** Build an Oracle PL/SQL database that enforces complaint response deadlines, validates resolution actions, and detects companies with unresolved complaint patterns.

**Specific objectives:**
1. Store company, product, issue, complaint, and resolution data in 3NF tables.
2. Calculate regulatory response deadlines automatically with a PL/SQL function.
3. Block closing a complaint without a recorded resolution using triggers.
4. Flag complaints within 3 days of breaching their deadline and escalate them via a stored procedure.
5. Record every data change in an audit log.
6. Show deadline alerts, overdue complaints, and company-level unresolved patterns on a live dashboard.

## Data Source
**Dataset:** Complaint Tracking and Analytics Dashboard Dataset
**Link:** https://www.kaggle.com/datasets/chinmayprakash12/complaint-tracking-and-analytics-dashboard-dataset
**Size:** 100,000 complaint records (34.06 MB CSV)
**Language:** English
**Privacy:** Anonymized records; no personally identifying consumer information.

## Innovation
**Option:** User Interface + Smart Automation.

**Tools:** Oracle APEX (or Streamlit with python-oracledb) for the dashboard; DBMS_SCHEDULER for nightly automation.

**How it works:** Intake officers log complaints through the APEX app, which calls `sp_log_complaint`; a trigger calculates the deadline. Investigators record resolutions; `sp_close_complaint` succeeds only if a resolution row exists. The nightly DBMS_SCHEDULER job runs `sp_flag_approaching_deadlines` and `sp_company_pattern_report`, refreshing the alert queue and company pattern table. The dashboard displays deadline urgency, overdue complaints, and company-level unresolved patterns.

## Planned Tables

| Table | Purpose |
|---|---|
| **COMPANIES** | Company identifiers and names |
| **PRODUCT_CATEGORIES** | Lookup: Mortgage, Debt collection, Credit reporting, Checking account, etc. |
| **ISSUE_TYPES** | Lookup: Incorrect information, Improper use of report, etc. |
| **COMPLAINTS** | Core complaint record: ID, date received, company, product, issue, response deadline, status |
| **RESOLUTIONS** | Resolution action recorded for each complaint (who, what, when) |
| **DEADLINE_ALERTS** | Alerts for complaints approaching or past deadline |
| **COMPANY_PATTERNS** | Precomputed company-level unresolved complaint counts by category |
| **AUDIT_LOG** | All INSERT/UPDATE/DELETE on COMPLAINTS and RESOLUTIONS |

## Planned PL/SQL Features

| Feature | Where It Is Used |
|---|---|
| **Function** `fn_calculate_deadline` | Returns the regulatory deadline date based on product category and receipt date |
| **Procedure** `sp_log_complaint` | Validates and inserts a complaint; calls `fn_calculate_deadline` |
| **Procedure** `sp_close_complaint` | Attempts to close a complaint; raises exception if no resolution recorded |
| **Procedure** `sp_flag_approaching_deadlines` | Scans open complaints and inserts DEADLINE_ALERTS rows for those within 3 days of breach |
| **Procedure** `sp_company_pattern_report` | Counts unresolved complaints per company per category; writes to COMPANY_PATTERNS |
| **Cursor** overdue complaint report | Loops through open complaints past deadline to build the escalation list |
| **Trigger on COMPLAINTS** (BEFORE INSERT) | Calculates and sets the response deadline automatically |
| **Trigger on COMPLAINTS** (BEFORE UPDATE) | Blocks changing status to 'CLOSED' if no RESOLUTION row exists |
| **Trigger on COMPLAINTS** | Blocks setting a resolution date earlier than the complaint date |
| **Audit triggers** | Log every INSERT/UPDATE/DELETE on COMPLAINTS and RESOLUTIONS |
| **Package** `pkg_complaintguard` | Groups deadline, resolution, and pattern functions |
| **Custom exceptions** | `e_no_resolution_recorded`, `e_invalid_resolution_date` |
| **Scheduler job** `JOB_NIGHTLY_DEADLINE_CHECK` | Runs `sp_flag_approaching_deadlines` and `sp_company_pattern_report` every night |

**References**
Akingbade, L. O. (2025). Effective utilization and operation of an online complaint management system for customer satisfaction and low-price supermarkets in Lagos, Nigeria. GVU Journal of Science, Health and Technology, 10(1), 176–180. https://zenodo.org/records/16886481

ChinmayPrakash. (2025). Complaint Tracking and Analytics Dashboard Dataset [Data set]. Kaggle. https://www.kaggle.com/datasets/chinmayprakash12/complaint-tracking-and-analytics-dashboard-dataset

Consumer Financial Protection Bureau. (2025). 2024 Consumer Response Annual Report. https://www.consumerfinance.gov/data-research/research-reports/2024-consumer-response-annual-report/

Itai, M., Ufual, D., & Okorie, U. (2024). Complaint handling strategies and customer advocacy in the Nigerian banking sector. In Customer Advocacy and Complaint Handling in Emerging Markets. Taylor & Francis.

Oracle. (2025). Oracle Database PL/SQL Packages and Types Reference, 19c. https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/

Oracle. (2025). CREATE TRIGGER. In Oracle Database SQL Language Reference, 19c. https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/CREATE-TRIGGER.html
