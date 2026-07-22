---
title: "Data Quality Assessment of an Argentine Airport Dataset"
date: 2026-07-22T15:00:00+03:00
draft: false
tags:
  - Data Quality
  - Data Governance
  - DAMA
  - Python
  - Data Stewardship
categories:
  - Projects
description: "A data quality assessment of the OurAirports dataset following DAMA-DMBOK principles."
---

# Data Quality Assessment of an Argentine Airport Dataset

As the **DAMA-DMBOK** correctly states, the primary objective of **Data Management** is to enable organizations to derive value from their data. Among its Knowledge Areas, **Data Quality** plays a fundamental role and has one of the greatest impacts on the value of this digital asset.

Just as high-quality data increases business value and creates new opportunities, poor-quality data generates significant costs. According to the *MIT Sloan Management Review*, these costs account for between **15% and 20% of a company's revenue**. In an increasingly data-driven world, achieving higher standards of data quality is becoming more important than ever.

But what exactly is **high-quality data**? According to the DAMA-DMBOK, it is data that *"meets the expectations and needs of data consumers."* In other words, whether a dataset is considered to be of good or poor quality depends largely on its intended use.

The project presented here not only proposes a methodology for assessing the quality of an aviation dataset, but also explores the inherent complexity of the process. Rather than focusing solely on missing values or duplicate records, the assessment incorporates business rules, external reference sources, identity resolution, and domain knowledge to provide a more comprehensive evaluation.

The analysis was developed in a **Jupyter Notebook** using **Python**, with the objective of producing a reproducible and transparent data quality assessment aligned with the principles described in the DAMA-DMBOK.

---

## Project Overview

**Dataset**

- OurAirports – Argentina

**Technologies**

- Python
- Jupyter Notebook
- Pandas
- GeoPandas
- BallTree

**Data Quality Dimensions**

- Completeness
- Uniqueness
- Consistency
- Validity
- Accuracy

**Highlights**

- Business-rule driven assessment
- Data Quality Scorecard
- Identity Resolution
- Spatial record matching
- External validation using independent reference datasets

---

## Technical Report

The complete technical report is available through the link below. It includes the full assessment methodology, business rules, implementation details, results, and conclusions.

**📄 [Read the complete report](static/reports/airport-data-quality.html)**
