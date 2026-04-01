# SaaS Renewal Automation using Power Automate & Excel

## 📌 Overview

This project automates the SaaS/software renewal approval process using Microsoft Power Automate and Excel (stored in SharePoint/OneDrive).

The flow automatically:

* Detects upcoming renewals (within 5 days)
* Sends approval requests to managers
* Updates renewal status based on approval/rejection
* Maintains approval history
* Captures manager comments
* Automatically extends renewal dates (adding +30days)

---

## 🧩 Technologies Used

* Microsoft Power Automate
* Excel (OneDrive / SharePoint)
* Outlook (Email Notifications)
* Teams (Notifications)

---

## 📊 Excel Structure

The Excel file must be formatted as a table (crtl+t) :

## 📊 Sample Data File

 Click here to view Excel file:
[Download Excel File](SaaSTracker.xlsx)

---

## ⚙️ Flow Design

### 1. Trigger

* **Recurrence (Scheduled Flow)**
* Runs daily

---

### 2. Read Excel Data

* Action: **List rows present in a table**

---

### 3. Loop Through Records

* Action: **Apply to each**

---

### 4. Condition (Renewal Check)

Check if:

* Renewal Date ≤ Today + 5 days
* Status = Pending

---

### 5. If YES (TRUE Branch)

#### Step 1: Update Status

* Set Status = `In Approval`

#### Step 2: Send Approval Request

* Action: **Start and wait for an approval**
* Type: Approve/Reject
* Assigned to: Manager Email
* Comments: Enabled

---

### 6. Approval Decision Condition

Check:

* Outcome = Approve

---

## ✅ If Approved

* Status → `Approved`
* Manager Comments → Captured from approval
* Renewal Date → +30 days

Expression:

```
addDays(items('Apply_to_each')?['Renewal Date'], 30)
```

* Approval History Update:

```
concat(
items('Apply_to_each')?['Approval History'],
' | ',
formatDateTime(utcNow(),'MMMM yyyy'),
' Approved'
)
```

---

## ❌ If Rejected

* Status → `Rejected`
* Manager Comments → Captured
* Renewal Date → +30 days

Expression:

```
addDays(items('Apply_to_each')?['Renewal Date'], 30)
```

* Approval History Update:

```
concat(
items('Apply_to_each')?['Approval History'],
' | ',
formatDateTime(utcNow(),'MMMM yyyy'),
' Rejected'
)
```

---

## 🧠 Key Logic

* Flow runs daily and checks upcoming renewals
* Approval request is sent only once (Status prevents duplicates)
* Both approval and rejection update renewal cycle
* History is appended (not overwritten)

---

## 🚨 Important Notes

* Excel file must be stored in OneDrive or SharePoint
* Table format is mandatory
* Status must start as `Pending`
* Avoid duplicate approval emails by updating status to `In Approval`

---

## 📈 Example Output

| Software | Renewal Date | Status   | Approval History    |
| -------- | ------------ | -------- | ------------------- |
| Zoom     | 20 Apr 2026  | Approved | March 2026 Approved |
| Slack    | 25 Apr 2026  | Rejected | March 2026 Rejected |

---

## 🔥 Future Enhancements

* Reminder if approval is pending
* Multiple approvers
* Dashboard reporting
* Separate audit log sheet

---

## 👨‍💻 Author

Sudheer
ICT Team – Fortunapix

---
