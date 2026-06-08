# Loan-Approval-Automation
An automated loan approval workflow built using n8n, Google Sheets, AI-based loan approvals, Email Alerts, and Power BI dashboards to approve loan in real time.
# 🚀 Bank Loan Approval Automation System using n8n

![n8n](https://img.shields.io/badge/n8n-Automation-orange)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-Database-green)
![Gmail](https://img.shields.io/badge/Gmail-Notifications-red)
![JavaScript](https://img.shields.io/badge/JavaScript-Logic-yellow)

## 📌 Overview

This project automates the loan approval process using n8n. The workflow reads loan applications from Google Sheets, calculates the applicant's EMI Burden Ratio, evaluates creditworthiness, classifies risk levels, updates records automatically, and sends personalized emails through Gmail.

### Key Benefits

- Fully Automated Decision Making
- Zero Manual Processing
- Real-Time Loan Evaluation
- Automated Email Notifications
- Scalable Banking Workflow
- Audit Trail Through Google Sheets

---

# 🏦 Business Problem

Traditional loan approval systems involve manual verification and assessment, resulting in:

- Delays in approvals
- Human errors
- Inconsistent decisions
- High operational costs

This workflow automates the entire decision-making process based on predefined banking rules.

---

# 🛠 Technology Stack

| Tool | Purpose |
|--------|---------|
| n8n | Workflow Automation |
| Google Sheets | Data Storage |
| Gmail | Email Notifications |
| JavaScript | Loan Logic |
| OAuth2 | Authentication |

---

# 📊 Input Dataset

Google Sheet contains:

| Column |
|----------|
| Loan_ID |
| Applicant_Name |
| Email |
| Monthly_Salary |
| Credit_Score |
| Existing_EMI |
| Loan_Amount |

### Auto Generated Columns

| Column |
|----------|
| EMI_Burden_Ratio |
| Risk_Level |
| Loan_Status |

---

# 📐 EMI Burden Ratio Formula

EMI Burden Ratio measures how much of a person's salary is already committed to debt repayment.

Formula:

EMI_Burden_Ratio = (Existing_EMI / Monthly_Salary) × 100

Example:

Salary = ₹80,000

Existing EMI = ₹15,000

EMI Burden Ratio = 18.75%

---

# ⚙️ Loan Decision Rules

## ❌ Rejection Criteria

Reject if:

Credit Score < 650

OR

EMI Burden Ratio > 50%

Result:

- Risk Level = High
- Loan Status = Rejected

---

## ✅ Approval Criteria

Approve if:

Credit Score ≥ 750

AND

EMI Burden Ratio < 30%

Result:

- Risk Level = Low
- Loan Status = Approved

---

## ⏳ Pending Review

Everything else goes for manual review.

Result:

- Risk Level = Medium
- Loan Status = Pending Review

---

# 🔄 Complete Workflow

```text
Manual Trigger
      │
      ▼
Read Data From Google Sheets
      │
      ▼
Calculate EMI Burden Ratio
      │
      ▼
Check Rejection Conditions
      │
 ┌────┴─────┐
 │          │
TRUE      FALSE
 │          │
 ▼          ▼
Rejected   Approval Check
 Email          │
 Update         │
                │
          ┌─────┴─────┐
          │           │
        TRUE       FALSE
          │           │
          ▼           ▼
      Approved     Pending
        Email       Email
      Update       Update
```

# 💻 Complete JavaScript Code

```javascript
const creditScore = Number($json.Credit_Score);
const salary = Number($json.Monthly_Salary);
const existingEMI = Number($json.Existing_EMI);

const emiBurdenRatio = (existingEMI / salary) * 100;

let riskLevel = "";
let loanStatus = "";
let emailSubject = "";
let emailBody = "";

if (creditScore < 650 || emiBurdenRatio > 50) {

    riskLevel = "High";
    loanStatus = "Rejected";

    emailSubject = "Loan Application Status";

    emailBody = `
Dear ${$json.Applicant_Name},

We regret to inform you that your loan application could not be approved.

Reason:
- Low Credit Score or
- High EMI Burden Ratio

Regards,
Blue Wave Bank
`;

}
else if (creditScore >= 750 && emiBurdenRatio < 30) {

    riskLevel = "Low";
    loanStatus = "Approved";

    emailSubject = "Congratulations! Your Loan is Approved";

    emailBody = `
Dear ${$json.Applicant_Name},

Congratulations!

Your loan application for ₹${$json.Loan_Amount} has been approved.

Risk Level: Low

Regards,
Blue Wave Bank
`;

}
else {

    riskLevel = "Medium";
    loanStatus = "Pending Review";

    emailSubject = "Loan Application Under Review";

    emailBody = `
Dear ${$json.Applicant_Name},

Your loan application requires manual review.

Our team will contact you shortly.

Regards,
Blue Wave Bank
`;

}

return {
json: {
...$json,
EMI_Burden_Ratio: emiBurdenRatio.toFixed(2),
Risk_Level: riskLevel,
Loan_Status: loanStatus,
Email_Subject: emailSubject,
Email_Body: emailBody
}
};
```

# 📧 Gmail Configuration

## To

```text
{{$json.Email}}
```

## Subject

```text
{{$json.Email_Subject}}
```

## Message

```text
{{$json.Email_Body}}
```

# 📊 Google Sheets Update

After processing each application, n8n updates:

| Field | Value |
|---------|---------|
| EMI_Burden_Ratio | Calculated Value |
| Risk_Level | Low / Medium / High |
| Loan_Status | Approved / Pending Review / Rejected |

---

# 📈 Sample Results

| Category | Count |
|-----------|--------|
| Approved | 15 |
| Pending Review | 25 |
| Rejected | 20 |

Total Applications Processed: 60

---

# 🎯 Key Learnings

- Workflow Automation using n8n
- Financial Risk Assessment
- Credit Score Evaluation
- Google Sheets Integration
- Gmail Automation
- Conditional Logic (AND / OR)
- Banking Underwriting Concepts
- FOIR / EMI Burden Analysis

---

# 🔮 Future Enhancements

- Machine Learning Credit Scoring
- CIBIL API Integration
- Fraud Detection
- WhatsApp Notifications
- SMS Alerts
- Power BI Dashboard
- PostgreSQL/MySQL Database
- Web Application Frontend

---

# 👨‍🎓 Academic Project

MBA Finance Project

Chitkara University

## Author

Aniket Bansal

---

⭐ If you like this project, give it a star on GitHub.
