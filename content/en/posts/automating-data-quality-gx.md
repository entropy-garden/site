---
title: "Automating Data Quality Validation with Great Expectations"
date: 2026-07-30
draft: false
tags:
  - Data Quality
  - Data Governance
  - Data Stewardship
  - Great Expectations
  - Python
  - Data Validation
categories:
  - Projects
description: "Automating business rule validation for an aviation dataset using Great Expectations."
hiddenInHomeList: true
---

# Automating Data Quality Validation with Great Expectations

In my previous article, I presented my first project, in which I assessed the **data quality** of an Argentine **aviation dataset**. While it is necessary to perform an initial, thorough analysis when working with a new **data source** that we consider critical, the reality is that, in day-to-day operations, we need different solutions that can be integrated into a **data pipeline**.

This is because we should never assume that a dataset is of good quality. And this is not only because the **data source** may have introduced errors or because it may be more or less reliable. Nor is it necessarily because the file may have been unexpectedly modified during the **ingestion process**. We must never forget that **data quality** depends on the context, and what we consider to meet the expectations for a data source should be verified continuously. This is where tools such as **Great Expectations** become especially useful.

In a future article, we will see how to integrate this tool into a complete **data pipeline**. For now, I will focus on introducing its core features and on demonstrating how to automate the validation of **business rules**, including their corresponding **compliance thresholds**.

So why not use **Great Expectations** for the initial assessment? Because the tool is designed to be integrated into a **production pipeline** and does not provide the flexibility required for a truly exhaustive **exploratory analysis**. In the case of the aviation dataset, one example was identifying aerodromes located over bodies of water, a validation that required **geospatial analysis** and **external reference data**.

At the same time, **Great Expectations** is particularly useful for evaluating the **reasonableness** dimension of **data quality**, since it makes it easy to detect significant changes between consecutive **data deliveries**, such as unexpected variations in the number of records, columns, or missing values compared with previous **batches**.

For this project, however, we will leave this dimension aside, since we do not (yet) have previous data to compare against. Once we integrate **Great Expectations** into the **data pipeline**, we will incorporate the corresponding **business rules** and the necessary code to perform these validations.

---

## Project Overview

### Dataset

- **OurAirports – Argentina**

### Technologies

- Python
- Jupyter Notebook
- Great Expectations
- Pandas

### Data Quality Dimensions

- Completeness
- Uniqueness
- Consistency
- Validity

### Highlights

- Automated business rule validation
- Reusable Expectation Suite
- Validation Definitions
- Data Quality Scorecard
- Great Expectations Data Docs
- Compliance thresholds
- Foundation for future pipeline integration

---

## Technical Report

The complete notebook is available through the link below. It includes the implementation of the **Great Expectations Context**, **Expectation Suite**, **business rules**, **Validation Definition**, validation workflow, **Data Docs** generation, and the final **Data Quality Scorecard**.

📄 **[Read the complete notebook](/reports/great-expectations-airports/)**