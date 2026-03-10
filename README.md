# 🔍 Process Mining Analysis — Road Traffic Fines Management

## Project Overview
This project performs a **data-driven process mining analysis** of the real-world
**Road Traffic Fines Management Process** using **Disco**. The analysis covers
process discovery, conformance checking, and process enhancement through
time, resource, and data perspectives.

---

## 📂 Dataset

| Detail | Info |
|---|---|
| **Dataset** | Road Traffic Fines Management Process |
| **Type** | Real-world event log (.xes format) |
| **Source** | [4TU Research Data](https://data.4tu.nl/articles/dataset/Road_Traffic_Fine_Management_Process/12683249) |
| **Total Events** | 561,470 |
| **Total Cases** | 150,370 |
| **Activities** | 11 |
| **Time Period** | 01/01/2000 — 06/18/2013 |

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| **Disco** | Main process mining tool — discovery, conformance & analysis |
| **Signavio** | BPMN process modeling |

---

## 📋 Tasks

### Task 1: Process Discovery
- Loaded the dataset into Disco and observed the initial process map
- Adjusted the **Activities slider to 70%** and **Paths slider to 50%** (Phase 1)
- Further reduced to **Activities 30%** and **Paths 10%** with a **frequency filter of top 10% variants** (Phase 2)
- Created a **BPMN model** for more control over the process content

**Key Activities included in BPMN Model:**
- Create Fine → Send Fine → Insert Fine Notification
- Insert/Send Appeal to Prefecture → Receive Result → Notify Offender
- Add Penalty → Payment → Send for Credit Collection

**Details Omitted:**
- Adding penalty directly after payment (out of place)
- Adding penalty after inserting notification
- Appeal to Judge (rare exception — only 555 cases, 0.1%)

---

### Task 2: Process Conformance
Three conformance questions investigated using Disco filters:

**Q1 — Does it ever happen that a fine is paid before it is sent out?**
- Applied follower filter: "Create Fine" directly followed by "Payment"
- Result: **46,880 such cases found** — indicating a significant process issue

**Q2 — Are there cases where a payment reminder is sent after the fine has already been paid?**
- Applied follower filter: "Payment" directly followed by "Insert Fine Notification"
- Result: **74 such cases found** — unnecessary reminders being sent after payment

**Q3 — What is the typical outcome for cases that involve an appeal to the Prefecture?**
- Applied filter: "Receive Result Appeal from Prefecture" → "Notify Result Appeal to Offender" → "Payment"
- Result: Out of **532 cases**, 314 had fines re-assessed (paid), and 218 had fines cancelled but were still routed to credit collection

---

### Task 3: Other Perspectives

**Time Perspective — How long do cases take and where are the bottlenecks?**
- Median case duration: **28.3 weeks**
- Mean case duration: **48.8 weeks**
- Mean much higher than median → presence of outliers with very long durations
- Main bottleneck: **"Send for Credit Collection"** with very high frequency (59,013 cases) and long duration

**Resource Perspective — Is there specific resource behavior affecting execution?**
- Dataset has **149 resources**
- Resource **538** is the most overutilized: handles **64,097 events (29.81%)** of all events
- "Insert Date Appeal to Prefecture" takes **22.6 months** for 39% of cases
- "Appeal to Judge" takes **30.6 months** for only 0.1% of cases

**Data Perspective — What is the impact of data quality on process efficiency?**
- No missing or invalid data found in the log
- Cases with short durations (e.g., 1 day) showed efficient and complete data logging
- Cases with long durations (e.g., 2 years, 241 days) showed delays likely due to resource or system issues, not data quality

---

### Task 4: Further Reflection

**Missing Data that would help:**
- Detailed resource performance data (speed and effectiveness per employee/system)
- Exception handling descriptions
- Customer feedback and satisfaction scores
- Event-level comments and reasons for actions taken
- Payment method details (online, in-person, bank transfer)

**Clear Improvement Points:**
- Address delays between "Add Penalty" and "Send for Credit Collection" through process optimization
- Balance workload among resources — Resource 538 is significantly overutilized
- Implement automated data validation checks at the point of data entry

---

## 📁 Project Structure

```
process-mining-analysis/
├── data/               # Event log file (.xes)
├── images/             # Process maps and screenshots from Disco
├── models/             # BPMN model (Signavio)
└── report/             # Full project report (PDF)
```

---

## 📊 Key Findings

- **46,880 cases** show payment occurring before the fine is even sent — a major conformance issue
- The **"Send for Credit Collection"** step is the biggest bottleneck in the process
- **Resource 538** handles nearly 30% of all events — significant workload imbalance
- **41% of appeals** to the Prefecture result in fines being cancelled, suggesting issues with initial fine accuracy

---

## 📚 References

- Dataset: https://data.4tu.nl/articles/dataset/Road_Traffic_Fine_Management_Process/12683249
- Process Mining Resources: https://www.tf-pm.org/resources/logs
