# GUGUTA's Maternal Emergency Data Drill Dashboard
**Multi-Level Interactive Public Health Surveillance Dashboard — Google Looker Studio**

> A fully interactive, multi-page public health analytics dashboard built in Google Looker Studio, using a synthetic dataset generated in Python and refined in Google Sheets to mirror real-world maternal and neonatal health trends in sub-Saharan Africa. The dashboard enables evidence-based decision-making across all administrative levels — from national leadership to facility-level clinical teams — by providing drill-down visibility into maternal mortality, case fatality rates, complication profiles, and delivery outcomes across a fictitious country called **Guguta**, comprising **9 geopolitical zones**, **45 states**, and **560 health facilities**.

🔗 **[View Live Dashboard](https://lookerstudio.google.com/s/sH1vy4j381I)**

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [The Problem This Solves](#2-the-problem-this-solves)
3. [Dataset — Design, Generation & Refinement](#3-dataset--design-generation--refinement)
4. [Dashboard Architecture & Design Principles](#4-dashboard-architecture--design-principles)
5. [Dashboard Pages — Detailed Walkthrough](#5-dashboard-pages--detailed-walkthrough)
   - [Pages 1–2: National Summary (Leadership View)](#pages-12--national-summary-leadership-view)
   - [Pages 3–10: State-Level Deep Dive (Zone E, State 09)](#pages-310--state-level-deep-dive-zone-e-state-09)
6. [Key Insights & Recommendations](#6-key-insights--recommendations)
7. [Tools & Technologies Used](#7-tools--technologies-used)
8. [Conclusion & Next Steps](#8-conclusion--next-steps)

---

## 1. Project Overview

This project showcases an end-to-end data analytics workflow applied to a complex public health domain: **Comprehensive Emergency Obstetric and Newborn Care (CEmONC)**. 

To bypass data privacy restrictions while demonstrating advanced analytics, I engineered a highly realistic, synthetic dataset representing 36 months (2015–2017) of reporting data from 560 hospitals in the fictitious nation of Guguta. The resulting 8-page dashboard is designed with a dual-audience architecture: it provides macro-level executive summaries for national/zonal leadership, and micro-level diagnostic views for technical Monitoring & Evaluation (M&E) teams to troubleshoot failing facilities.

---

## 2. The Problem This Solves

**The Challenge:** Public health ministries often rely on static, highly aggregated quarterly reports. When maternal mortality rates spike, leadership can see the national trend, but they cannot easily see *where* it is happening (which specific hospital) or *why* (was it Eclampsia? Was there a stockout of emergency drugs?).

**The Solution:** This dashboard transitions CEmONC data from a static reporting format into a dynamic, automated surveillance system. 
* **For Leadership:** Instant visibility into YoY trends, national Case Fatality Rates (CFR), and zonal performance heatmaps.
* **For Technical Teams:** The ability to instantly filter down to a specific state, isolate statistical outliers (facilities with high deaths but low caseloads), and correlate complication spikes with supply chain stockouts.

---

## 3. Dataset — Design, Generation & Refinement

Because standard random data generators cannot produce the nuanced clinical and temporal relationships required for a healthcare dashboard, I built the dataset from scratch.

### Step 1: Algorithmic Data Generation (Python)
I wrote a custom Python script utilizing `pandas` and `numpy` to generate nearly 20,000 reporting records. 
* **Clinical Integrity:** Enforced strict mathematical rules (e.g., Total Deliveries perfectly equals the sum of SVD, Assisted, and C-Sections; maternal deaths never exceed maternal complication cases).
* **Epidemiological Weighting:** Applied realistic probability distributions (Poisson, Log-Normal) so that complications like Postpartum Hemorrhage and Eclampsia correctly appeared as the leading causes of mortality.
* **Time-Series Continuity (The 95% Rule):** Programmed exactly 95% of the 560 facilities to report flawlessly for 36 consecutive months, simulating realistic non-compliance in the remaining 5%. This ensured smooth, unbroken line charts for trend analysis.

### Step 2: Upstream Data Transformation (Google Sheets)
To prepare the data for visualization and complex cross-filtering, I performed advanced upstream transformation in Google Sheets:
* **Array Formulas & Restructuring:** I created 4 supplementary sheets using combination of `ARRAYFORMULA`, `SPLIT`, and `FLATTEN` functions to reshape the maternal and neonatal cases and mortality data. This transformed the wide-format survey data into a strict tabular format optimized for BI ingestion.
* **Dynamic Geography Extraction:** Used logical text extraction to pull Zone and State attributes directly from unique Facility ID strings (e.g., mapping `GGTA-F-13-0352` automatically to Zone F, State 13).

### Step 3: BI Data Blending & Validation (Looker Studio)
Within Looker Studio, I engineered 3 distinct **Blended Datasets** applying appropriate Outer and Inner Joins to merge the flattened case/mortality tables with the master facility dictionary and the core delivery metrics.
* **Data Science Validation:** To prevent "Cartesian Join" (data multiplication) errors—a common pitfall in BI blending—I rigorously cross-checked the outputs. I validated the blended dataset metrics (specifically the Case Fatality Rates and Total Cases) against the raw tabular data to ensure 100% mathematical accuracy before finalizing the visual layers.

---

## 4. 🏗️ Dashboard Architecture & Design Principles

**1. Dual-Audience Architecture**
* **Executive Layer (Pages 1-2):** Macro-level KPIs, geographic heatmaps, and aggregate Case Fatality Rates (CFR) designed for national leadership.
* **Diagnostic Layer (Pages 3-10):** Micro-level analytical tools tailored for M&E teams to troubleshoot specific zones, states, and facilities.

**2. UI/UX Principles**
* **Contextual Baselining:** Scorecards feature 3-year YoY sparklines to show metric velocity, while bar charts use hardcoded target lines to instantly flag unacceptable mortality rates.
* **Hierarchical Drill-Down:** Cascading filters allow seamless navigation from Zone ➔ State ➔ LGA ➔ Facility.

**3. Standout Visualizations**
* **Outlier Scatter Plots:** *Cases vs. Deaths* charts instantly isolate failing hospitals with disproportionately high mortalities.
* **Dual-Axis Charts:** Prove systemic clinical links (e.g., Obstructed Labour vs. Neonatal Asphyxia).
* **Drop-off Funnels:** Highlight systemic retention failures (e.g., FP counseling vs. commodities received).

**4. Technical Implementation**
* **Upstream Flattening:** Merged the 560-facility dictionary with 20K raw records using Google Sheets (`ARRAYFORMULA`, `FLATTEN`) to prevent Looker Studio Cartesian Join errors.
* **Optimized Blending:** Deployed strict joins to accurately calculate 80+ metrics without data duplication.

---

## 5. Dashboard Pages — Detailed Walkthrough

### 📍 Pages 1–2: National Summary (Leadership View)
*Target Audience: Ministry Executives, National Program Directors*

* **Page-1:** Guguta's Maternal Summary Page
![Summary Page Maternal](images/National_page_1.jpeg)

* **Page-2:** Guguta's Neonatal Summary Page
![Summary Page Maternal](images/National_page_2.jpeg)

### 📊 Dashboard Highlights
* **Massive Scale:** The dashboard aggregates data from **560 health facilities** across 9 Zones and 45 States, capturing **1.84 million deliveries** between 2015 and 2017.
* **Maternal Burden:** Over the 3-year period, facilities managed **614,998 maternal complications**, which tragically resulted in **21,717 maternal deaths**.
* **Neonatal Burden:** The data recorded **126,244 neonatal complications** (Birth Asphyxia and Sepsis), leading to **41,313 neonatal deaths**.
* **The Leading Complication:** **Obstructed Labour** is the most prevalent maternal complication nationally, accounting for **25%** of all cases.
* **The Leading Killer (Maternal):** **Eclampsia** is responsible for the largest share of maternal mortalities (**32.8%**), despite being only the third most common complication (15.5% of cases).
* **The Leading Killer (Neonatal):** **Severe Birth Asphyxia** accounts for a massive **58.3%** of all neonatal deaths.
* **Case Fatality Rates (CFR):** The national CFR for Eclampsia is alarmingly high at **15.1%**, dwarfing the mortality rates of other conditions like Post-Abortal complications (8.04%) and Obstetric Hemorrhage (8.03%).

---

### 💡 Key Insights
* **The Eclampsia Discrepancy:** There is a severe national failure in managing hypertensive disorders in pregnancy. While Obstructed Labour and Obstetric Hemorrhage occur much more frequently, Eclampsia kills at nearly double the rate (15.1% CFR). This points to a systemic inability to halt disease progression once convulsions begin.
* **The Obstructed Labour -> Asphyxia Pipeline:** Obstructed Labour makes up 25% of maternal complications. Unsurprisingly, Severe Birth Asphyxia drives 58.3% of neonatal deaths. Prolonged, unmanaged labor is directly causing fetal distress, resulting in a dual crisis where both mother and baby are severely compromised during delivery.
* **Consistent Reporting, Persistent Death:** The facility data submission chart shows near-perfect compliance (steady at ~560 facilities/month). However, the "Trend of MMR" and neonatal death charts show that mortalities remain stubbornly high and flat across the 36 months. The data is being collected reliably, but it is not driving systemic clinical improvement.

---

### 🛠️ Actionable Recommendations
* **Comprehensive Preeclampsia/Eclampsia Management:** With Eclampsia driving 32.8% of deaths, the Ministry must mandate rigorous clinical training on hypertensive disorders. This must start at the antenatal level with aggressive surveillance and risk-stratification for high-risk women. This training must be coupled with an immediate national audit of the Magnesium Sulfate (MgSO4) supply chain to ensure life-saving anticonvulsants are available the moment they are needed.
* **Community Awareness & Prompt Referrals:** Launch targeted public health campaigns educating communities, women, and traditional birth attendants on identifying early danger signs in pregnancy. This must be paired with strengthened emergency referral protocols to ensure women reach CEmONC facilities *before* complications become fatal.
* **Enforce Intrapartum Monitoring (The Partograph):** To break the fatal link between Obstructed Labour and Severe Birth Asphyxia, clinical directors must strictly mandate and audit the use of the WHO Partograph during active labor. Early detection of abnormal labor progress will prompt timely C-sections.
* **Invest in Neonatal Resuscitation:** Since Severe Birth Asphyxia causes over 58% of neonatal deaths, delivery rooms must be equipped with ambu bags and penguin suckers, backed by mandatory "Helping Babies Breathe" (HBB) resuscitation training for all birth attendants

---

### 📍 Pages 1–8: State-Level Deep Dive (Zone E, State 09)
*Target Audience: State Health Commissioners, M&E Officers, Quality Improvement Teams*

*(The following analysis and pages of the dashboard is filtered for State 09 of Zone E)*

* **Page-1:** State 09's Maternal Summary Page
![Summary Page Maternal](images/state_page_1.jpeg)

* **Page-2:** State 09's Neonatal Summary Page
![Summary Page Maternal](images/state_page_2.jpeg)

* **Page-3:** State 09's Deliveries Cases and Mortalities
![Summary Page Maternal](images/state_page_3.jpeg)

* **Page-4:** State 09's Trends of Deliveries Cases, Mortalities and MMR by Months
![Summary Page Maternal](images/state_page_4.jpeg)

* **Page-5:** State 09's Trends of MOrtalities per Facility by Months
![Summary Page Maternal](images/state_page_5.jpeg)

* **Page-6:** State 09's Trends of Case Specific Mortalities (CFR) per Facility by Months
![Summary Page Maternal](images/state_page_6.jpeg)

* **Page-7:** State 09's Trends of Case Specific Mortalities (CFR) by Month
![Summary Page Maternal](images/state_page_7.jpeg)

* **Page-8:** State 09's Trends of Case vs Mortalities by Month
![Summary Page Maternal](images/state_page_8.jpeg)


**📊 Key Highlights:**
* **Scale of Operations:**  The data captures maternal health outcomes across 15 health facilities reporting over a 36-month period (Jan 2015 – Dec 2017), covering a total of 55,873 deliveries.
* **Maternal Morbidity & Mortality:**  During this period, there were 7,433 maternal complication cases, resulting in 1,038 maternal deaths.
* **Leading Complication (Volume):**  Obstructed Labour is the most frequent complication by a significant margin, accounting for over a quarter (25.9%) of all recorded maternal cases. It is followed by Eclampsia (18.7%) and Preeclampsia (17.4%).
* **Leading Cause of Death:**  Eclampsia is the deadliest complication in the state, driving almost a quarter (23.9%) of all maternal mortalities.
* **Case Fatality Rates (CFR):**  The bar charts indicate that Eclampsia has the highest Case Fatality Rate (peaking near 20%), drastically outpacing the CFR of conditions like Preeclampsia (~8%) and Obstetric Hemorrhage (~9%).
* **Reporting Compliance:**  Facility data submission remained relatively strong, hovering around the maximum of 15 facilities per month, with only a few minor drop-offs across the 3-year timeline.


---

## 6. 💡 Key Insights & Recommendations

Based on the simulated data parameters, the dashboard highlights several critical intervention points:

* **The Eclampsia Crisis:** Despite being the second most common complication, Eclampsia is the leading killer. A critical ~20% CFR indicates facilities struggle to save mothers once convulsions begin.
* **Systemic Labor Delays:** Obstructed Labour accounts for 25.9% of cases, signaling failed early detection, poor intrapartum monitoring, or delayed referrals to these CEmONC facilities.
* **Zero Surge Capacity:** Mortality lines perfectly mirror caseload spikes (e.g., mid-2017). The system lacks the capacity to handle sudden emergency influxes, resulting in proportionate deaths rather than successful interventions.

**🛠️ Recommendations:**
* **Urgent MgSO4 Audit:** Immediately audit Magnesium Sulfate and antihypertensive supply chains across all 15 facilities. Stockouts or delayed administration are the likely culprits for the high Eclampsia CFR.
* **Enforce Partograph Use:** Mandate and audit WHO Partograph utilization for active labor. Early "alert line" detection will prompt timely C-sections and drastically reduce Obstructed Labour cases.
* **Investigate Seasonal Spikes:** Conduct root-cause analyses on specific spike months (e.g., Q3 2016) to identify correlations with rainy season transport delays, staff strikes, or drug stockouts.
* **Address Data Drop-offs:** Identify facilities responsible for minor reporting dips. 100% compliance is critical; missing data during peak crisis months could mask even higher mortality realities.

---

## 7. Tools & Technologies Used

| Discipline | Tools & Techniques | Purpose |
| :--- | :--- | :--- |
| **Data Engineering** | Python (`pandas`, `numpy`, `datetime`) | Generation of 20,000+ synthetic records with strict epidemiological weighting, clinical logic balancing, and time-series continuity. |
| **Data Transformation** | Google Sheets (`ARRAYFORMULA`, `SPLIT`, `FLATTEN`) | Upstream normalization and reshaping of wide-format survey data into optimized tabular schemas. |
| **Data Modeling** | Looker Studio (Data Blending, Joins) | Engineered complex blended datasets merging flattened tables with facility dictionaries; rigorously validated outputs against raw data to prevent Cartesian errors. |
| **Business Intelligence** | Google Looker Studio | Architecture of an 8-page, dual-audience interactive dashboard featuring dynamic geo-mapping, scatter plots, funnels, and YoY sparklines. |

---

## 8. Conclusion & Next Steps

This project demonstrates the immense value of transitioning public health data from static spreadsheets into dynamic, interactive surveillance systems. By combining rigorous data engineering with thoughtful BI design, health ministries can move from reacting to historical data to proactively deploying resources to the exact facilities and supply chains that need them most. 

*Dashboard designed and developed as a portfolio project demonstrating applied data analytics, data engineering, and multi-level data visualization competencies. All data is synthetic and does not represent any real country, facility, or patient.*
