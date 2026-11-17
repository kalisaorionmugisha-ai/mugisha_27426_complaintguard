# mugisha_27426_complaintguard
# ComplaintGuard
### Consumer Complaint Resolution Tracking and Regulatory Escalation System (Oracle PL/SQL)

**Student:** Mugisha Kalisa Orion | **ID:** 27426
**Course:** INSY 8311, Database Development with PL/SQL
**Instructor:** Eric Maniraguha

---

### Problem Statement
Consumer complaints about financial products—incorrect credit reporting, debt collection harassment, and loan servicing errors—are often tracked in spreadsheets or email inboxes, so complaints breach regulatory response deadlines unnoticed and companies with repeated unresolved complaints go undetected. This affects consumers who suffer financial harm when complaints are ignored, and regulators who cannot see patterns of non-compliance until fines are issued. As financial services expand and complaint volumes grow, the lack of a deadline-enforcing, pattern-detecting system becomes a growing compliance risk.

### Objectives
**Main objective:** Build an Oracle PL/SQL database that enforces complaint response deadlines, validates resolution actions, and detects companies with unresolved complaint patterns.

**Specific objectives:**
1. Store company, product, issue, complaint, and resolution data in 3NF tables.
2. Calculate regulatory response deadlines automatically with a PL/SQL function and block closing a complaint without a recorded resolution using triggers.
3. Flag complaints within 3 days of breaching their deadline and escalate them to a supervisor queue via a stored procedure.
4. Record every data change in an audit log.
5. Show deadline alerts, overdue complaints, and company-level unresolved patterns on a live dashboard.

### Data Source
Complaint Tracking and Analytics Dashboard Dataset, Kaggle: https://www.kaggle.com/datasets/chinmayprakash12/complaint-tracking-and-analytics-dashboard-dataset (100,000 complaint records, 34 MB CSV, English, anonymized, educational licence)

### Innovation
**User interface + Smart automation:** Oracle APEX/Streamlit dashboard connected live to the Oracle database, showing complaints by deadline urgency, overdue complaint queues, and company-level unresolved complaint patterns. A DBMS_SCHEDULER nightly job runs `sp_flag_approaching_deadlines` and `sp_company_pattern_report`, escalating at-risk complaints and refreshing the pattern table automatically.

### Planned Tables
COMPANIES, PRODUCT_CATEGORIES, ISSUE_TYPES, COMPLAINTS, RESOLUTIONS, DEADLINE_ALERTS, COMPANY_PATTERNS, AUDIT_LOG

### Planned PL/SQL Features
- **Procedures:** `sp_log_complaint`, `sp_close_complaint`, `sp_flag_approaching_deadlines`, `sp_company_pattern_report`
- **Function:** `fn_calculate_deadline` — returns regulatory deadline based on product category and receipt date
- **Cursor:** overdue complaint report — loops through open complaints past deadline to build escalation list
- **Package:** `pkg_complaintguard` — groups deadline, resolution, and pattern logic
- **Triggers:** a BEFORE INSERT trigger calculates and sets the response deadline automatically; a BEFORE UPDATE trigger blocks changing status to 'CLOSED' if no RESOLUTION row exists; an audit trigger logs every change to COMPLAINTS and RESOLUTIONS
- **Exceptions:** `e_no_resolution_recorded`, `e_invalid_resolution_date`

### References
See `docs/research/research_log.md` (APA references to be added).
