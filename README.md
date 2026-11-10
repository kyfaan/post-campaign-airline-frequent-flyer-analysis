# Airline Frequent Flyer Analysis (Churn & Loyalty Promotion)

From my experience working with SEO, increasing traffic is only half the battle. The most intriguing part is how those users convert as paying customers by bridging their search intent with business revenue. One of the ways to bridge is by collaborating with business stakeholders.

I once worked on an initiative with marketing and business stakeholders. The idea was to provide offers for users who come from SEO. In order to initiate and communicate the initiative, I could not vaguely say ‘this project will increase our GMV’. I need to communicate through a metric that can tick all teams’ goals. 

Recently I explored a campaign analysis project from a fictitious airline that just held a promotion. From this project, I am able to comprehend what success is behind marketing initiatives, just like the promotion that I did with my stakeholders. 

In the fictitious project I explored how to measure a promotion’s success through metrics like Customer Lifetime Value (CLV) and dollar cost point redemption (how many dollars spent by the airline for every point redemption). When travelers redeem more points than their CLV, it means the campaign failed to attract high-value travelers hence a financial leakage happened. 

What I can learn from the project is: When I have to collaborate in the future, I won't bring a vague goal, but am able to bring a sharp, chunked-goal ‘what does success look like from this initiative or what does failure look like?’. Instead of a vague ‘Increase GMV’ I would break it down into ‘how do we increase GMV? →  ‘through promotion’ → ‘what makes a promotion fail? → ‘if the analysis revealed that the campaign attracts promo-hunters’ → ‘how do we mitigate this?’ → ‘The solution is to work backwards from this problem to improve performance’


---

## Business Context

* **Situation:** The airline held a promotional campaign in 2018 to attract more travelers. The data set of Flyer loyalty program tracks over 16,000 members, capturing flight activity, Customer Lifetime Value (CLV), and total travelers point redemption in CAD
* **Complication:** Standard management reports track **Explicit Churn** (official account cancellations) but often miss **Silent Churn** (inactive travelers who stop flying without cancelling their frequent flyer). Furthermore, promotional campaigns (such as the 2018 Loyalty Promotion) risk attracting "low-budger travelers"—low-value travelers who redeem heavy subsidies and churn quickly, causing monetary points leakage.
* **Resolution:** A three-phase statistical and cohort analysis framework was developed to detect total real churn, test promotional cohort sustainability, and evaluate monetary net leakage.

---

## Framework & Methodology

### Phase 1: Real Churn Identification
* **Explicit Churn:** Customers with documented Frequent Flyer cancellation dates (`Cancellation Year` is not null).
* **Silent Churn:** Customers with no cancellation record (`Cancellation Year` is null) but zero flight activity throughout 2018 (`Total Flights == 0`).
* **Real Churn:**
  $$\text{Real Churn} = \text{Explicit Churn} + \text{Silent Churn}$$

### Phase 2: Promotional Cohort Quality Evaluation
* Compares members enrolled under the **2018 Promotion** against those under **Standard Enrollment** in the 2018 cohort.
* Evaluates real churn rates and flight frequency to assess whether promotional members behave as high-value frequent flyers or short-term discount hunters.

### Phase 3: Points Monetary Leakage & Financial Value
* **Dollar Cost Points Redeemed:** Direct monetary cost (in C$) incurred by the airline when points are redeemed.
* **Net Value Formula:**
  $$\text{Net Value} = \text{CLV} - \text{Dollar Cost Points Redeemed}$$
* **Points Leakage Definition:** Defined as negative Net Value ($\text{Net Value} < 0$), where low-CLV or early-churning members exhaust high redemption subsidies without generating offsetting customer value.
* **Non-Parametric Hypothesis Testing:** Mann-Whitney U test applied due to skewed CLV and points redemption distributions.

---

## Key Findings

### 1. Overall Population Churn Breakdown (13,886 Members)
| Customer Status | Member Count | Percentage (%) |
| :--- | :---: | :---: |
| **Active Members** | 11,545 | 83.14% |
| **Explicit Churn** | 2,067 | 14.89% |
| **Silent Churn** | 274 | 1.97% |
| **Total Population** | **13,886** | **100.00%** |

> **Key Insight:** Standard tracking captured 14.89% churn, but real total churn reached **16.86%** when incorporating undetected Silent Churn.

---

### 2. 2018 Promotional vs. Standard Cohort Performance
| Metric | 2018 Promotion Group | Standard Group | Insight / Difference |
| :--- | :---: | :---: | :--- |
| **Total Members** | 971 | 2,039 | Cohort split (32.3% Promo vs 67.7% Standard) |
| **Explicit Churn Count** | 115 | 44 | Official cancellations |
| **Silent Churn Count** | 46 | 341 | Inactive without cancellation |
| **Total Real Churn** | 161 | 385 | Combined churned members |
| **Real Churn Rate (%)** | **16.58%** | **18.88%** | **Promo churn rate is ~2.30% lower** |
| **Mean Flights** | **46.22** | **8.42** | Promo members fly **5.5x more frequently** |
| **Median Flights** | **47.00** | **7.00** | Strong sustained flight activity |

> **Key Insight:** The 2018 Promo cohort demonstrated **higher retention** (lower real churn) and dramatically higher flight engagement than standard enrollees.

---

### 3. Financial Metrics & Statistical Hypothesis Testing

#### Financial Metrics Summary
| Metric | 2018 Promotion | Standard Group | Statistical Significance |
| :--- | :---: | :---: | :--- |
| **Mean CLV (C$)** | C$ 8,046.51 | C$ 8,007.18 | $p = 0.584$ (Not Significant) |
| **Median CLV (C$)** | C$ 5,926.39 | C$ 5,757.44 | Equivalent baseline customer value |
| **Mean Points Cost (C$)** | C$ 69.62 | C$ 39.91 | $p = 1.76 \times 10^{-28}$ (Statistically Significant) |
| **Median Points Cost (C$)** | C$ 63.00 | C$ 0.00 | Promo members actively utilize rewards |
| **Median Net Value (C$)** | **C$ 5,860.17** | **C$ 5,708.61** | **Positive net yield for airline** |

#### Hypothesis Testing Summary (Mann-Whitney U Test)
1. **Customer Lifetime Value (CLV):**
   * $H_0$: No difference in CLV between 2018 Promo and Standard cohorts.
   * Result: $p = 0.584 \ge 0.05 \implies$ Fail to reject $H_0$. Both cohorts deliver comparable baseline CLV.
2. **Dollar Cost Points Redeemed:**
   * $H_0$: No difference in points redemption monetary costs.
   * Result: $p = 1.756 \times 10^{-28} < 0.05 \implies$ Reject $H_0$. 2018 Promo members redeem significantly higher point values.

---

### 4. Monetary Points Leakage Audit
| Enrollment Type | Total Members | Leakage Count ($\text{Net Value} < 0$) | Leakage Percentage (%) |
| :--- | :---: | :---: | :---: |
| **2018 Promotion** | 971 | 0 | **0.00%** |
| **Standard Enrollment** | 2,039 | 0 | **0.00%** |

> **Final Financial Verdict:** Zero points monetary leakage was detected across all 2018 travelers. Higher point redemptions among promotional travelers were fully backed by high flight frequency and customer lifetime value.

## Key Takeaways:

1. **Implement Early Warning Inactivity Tracking:** Formalize Silent Churn detection ($\ge 12$ months no flight) into core CRM dashboards to trigger re-engagement campaigns before accounts are officially cancelled.
2. **Scale High-Engagement Promotional Structures:** The 2018 promotional design successfully acquired active, high-volume flyers without eroding CLV or inducing point subsidy leakage.
3. **Optimise Reward Tier Incentives:** Maintain point redemption accessibility for active promotional cohorts, as their high flight frequency (mean 46.2 flights/yr) far outweighs the monetary redemption costs.

---
