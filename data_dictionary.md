# Airline Loyalty Program — Data Dictionary

This document provides a comprehensive data dictionary for the raw dataset used in the **Airline Customer Churn and Promotion Analysis** project. The dataset consists of two primary relational tables tracking member flight activity and demographic/loyalty history.

---

## Relational Overview & Primary Keys

* **Primary Entity:** Customer / Loyalty Member
* **Primary Key:** `Loyalty Number` (present in both tables, enabling 1-to-Many join from `Customer Loyalty History` to `Customer Flight Activity`).

```
+--------------------------+          1 : N          +--------------------------+
| Customer Loyalty History | ----------------------- | Customer Flight Activity |
| (Loyalty Number - PK)    |                         | (Loyalty Number - FK)    |
+--------------------------+                         +--------------------------+
```

---

## Table 1: `Customer Flight Activity`

Contains granular monthly activity records for loyalty program members, tracking flights booked, distance flown, and points accumulated vs. redeemed.

| Field Name | Expected Data Type | Key Type | Description & Business Logic |
| :--- | :---: | :---: | :--- |
| **Loyalty Number** | String / Integer | **FK** | Customer's unique loyalty program identifier (Foreign Key linking to Customer Loyalty History). |
| **Year** | Integer | Metric | Year of the observation period (e.g., `2017`, `2018`). |
| **Month** | Integer | Metric | Month of the observation period (`1` through `12`). |
| **Total Flights** | Integer | Metric | Sum of flights booked and flown by the customer within the period. |
| **Distance** | Integer / Float | Metric | Total flight distance traveled in the period (measured in kilometers). |
| **Points Accumulated** | Integer / Float | Metric | Loyalty points earned/accumulated by the customer during the period. |
| **Points Redeemed** | Integer / Float | Metric | Loyalty points redeemed for rewards/flights during the period. |
| **Dollar Cost Points Redeemed** | Float / Currency | Metric | Direct monetary cost equivalent incurred by the airline for redeemed points (in **CDN / C$**). |

---

## Table 2: `Customer Loyalty History`

Contains customer demographic profiles, location details, education/salary tiers, loyalty tier status, lifetime customer value (CLV), and enrollment/cancellation history.

| Field Name | Expected Data Type | Key Type | Description & Value Domain |
| :--- | :---: | :---: | :--- |
| **Loyalty Number** | String / Integer | **PK** | Customer's unique loyalty identifier (Primary Key). |
| **Country** | String | Attribute | Country of residence (e.g., `Canada`). |
| **Province** | String | Attribute | Province of residence (e.g., `Ontario`, `British Columbia`, `Quebec`). |
| **City** | String | Attribute | City of residence. |
| **Postal Code** | String | Attribute | Postal code of primary residence. |
| **Gender** | String | Attribute | Customer gender (`Female`, `Male`). |
| **Education** | String | Categorical | Highest completed education level: `High School or Below` > `College` > `Bachelor` > `Master` > `Doctor`. |
| **Salary** | Float / Currency | Attribute | Estimated annual income of the customer (in **C$**). |
| **Marital Status** | String | Categorical | Marital status (`Single`, `Married`, `Divorced`). |
| **Loyalty Card** | String | Categorical | Loyalty card status tier: `Star` > `Nova` > `Aurora`. |
| **CLV** | Float / Currency | Metric | **Customer Lifetime Value**: Total invoice revenue for all flights ever booked by the member (in **C$**). |
| **Enrollment Type** | String | Categorical | Channel/type of membership enrollment (`Standard` vs. `2018 Promotion`). |
| **Enrollment Year** | Integer | Date Attribute | Year the customer enrolled in the loyalty program. |
| **Enrollment Month** | Integer | Date Attribute | Month the customer enrolled in the loyalty program (`1`–`12`). |
| **Cancellation Year** | Integer / Null | Date Attribute | Year the customer explicitly cancelled membership (`Null` if active or silent churn). |
| **Cancellation Month** | Integer / Null | Date Attribute | Month the customer explicitly cancelled membership (`Null` if active or silent churn). |

---

## Key Derived Metrics & Analytical Concepts

In analytical workflows, these raw attributes are combined to calculate critical business metrics:

1. **Explicit Churn:** Members with a non-null `Cancellation Year` / `Cancellation Month` record.
2. **Silent Churn:** Active members (`Cancellation Year` is null) with zero flights (`Total Flights == 0`) across the observation year (e.g., 2018).
3. **Real Churn:** Combined Total Churn = `Explicit Churn` + `Silent Churn`.
4. **Net Value (C$):** Customer Lifetime Value minus subsidy redemption cost:
   $$\text{Net Value} = \text{CLV} - \text{Dollar Cost Points Redeemed}$$
5. **Monetary Points Leakage:** Defined as instances where $\text{Net Value} < 0$ (low CLV or early churners redeeming high point costs).

---
