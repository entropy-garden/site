---
title: "Building a Data Pipeline with dbt and DuckDB"
date: 2026-09-03
draft: false
tags:
  - Data Quality
  - Data Governance
  - Data Stewardship
  - dbt
  - DuckDB
  - Great Expectations
  - Data Pipeline
categories:
  - Projects
description: "Building a simple data pipeline with dbt and DuckDB and integrating Great Expectations for data quality validation."
hiddenInHomeList: true
---

Hello!

I’m back after a month away.

During this time, I have not only been training to continue publishing **quality articles** on my blog, but I have also been dealing with some **work-related matters**. I’ll share more details about that later — exciting things are coming.

But let’s get back to the topic at hand.

In my first article, I presented an example of **data quality assessment using an aerodrome dataset**. In the second one, I showed how to **automate that assessment using Great Expectations**.

In this third article, as a natural evolution of the previous two, I decided to take it one step further and build a small **data pipeline** using **dbt** and **DuckDB**.

But why build a pipeline?

The answer is simple: in a **real-world environment**, data does not arrive just once for a manual analysis. It arrives repeatedly, its structure may change, its volume may grow, new sources may be incorporated, and it may need to be processed by different people.

Therefore, while an initial manual assessment should be carried out to determine the **quality of the source**, integrating a series of **data quality tests** into a pipeline makes the process **reproducible, scalable, and traceable**.

So why did I choose **dbt** and **DuckDB**?

First of all, this decision is partly driven by a general principle of this blog: prioritizing **Open Source solutions** and only choosing **proprietary alternatives** when they provide a justified advantage. One of the main reasons for favoring open source tools is their **accessibility and transparency**. The idea is that anyone should be able to clone or reproduce my projects.

In the case of **DuckDB**, I particularly value its **simplicity**. Since it runs locally, it allows you to work with data without having to deploy or manage a separate database infrastructure. This makes it a very convenient alternative for **small projects and prototypes**.

As for **dbt**, *it is an open source **command-line tool** that helps analysts and engineers transform data more efficiently*. In an **ETL process**, it would correspond to the T in Transform. It supports **version control** (dbt natively supports Git) and is also simple and intuitive. Finally, the tool itself places an emphasis on **data quality** by integrating standardized tests.. In an **ETL process**, it would correspond to the T in Transform. It supports **version control** (dbt natively supports Git) and is also simple and intuitive. Finally, the tool itself places an emphasis on **data quality** by integrating standardized tests.

In the project uploaded to Git, we will only include a few of these tests, since, following the **DRY principle**, I preferred not to recreate all the tests and instead integrate **Great Expectations** directly. In this particular case, adding GX to the pipeline was remarkably straightforward.

Great Expectations allows you to create **persistent projects** in which expectations, configurations, and validation definitions are stored and can be reused later. In this project, I chose to use this approach because the intention is not to limit ourselves to the current validations, but to continue expanding the project in the future.

For example, so far we have focused mainly on dimensions such as **completeness, uniqueness, and validity**. One of the next improvements will be to incorporate additional **quality dimensions**, such as **reasonableness**, to assess whether values, in addition to meeting certain technical conditions, are actually consistent within the context of the data.

Finally, for this particular project, I decided simply to upload the **project to GitHub**, unlike the previous ones, where I uploaded the code to the blog.

I hope you enjoy it and learn from it as much as I did!


---

## Project Overview

### Dataset

- **OurAirports – Argentina**

### Technologies

- Python
- DuckDB
- dbt
- Great Expectations

### Data Quality Dimensions

- Completeness
- Uniqueness
- Validity
- Reasonableness *(future improvement)*

### Highlights

- Simple data pipeline
- Data transformation with dbt
- Local analytical database with DuckDB
- Automated business rule validation
- Reusable Great Expectations project
- Persistent Expectation Suite
- Validation Definitions
- Foundation for future pipeline and metadata integration

---

## Technical Report

The complete project is available through the link below. It includes the **dbt transformations**, **DuckDB database**, **Great Expectations configuration**, **Expectation Suite**, **Validation Definition**, and the code required to execute the data quality validation.

📄 **[View the complete project on GitHub](https://github.com/entropy-garden/entropy-garden/tree/main/end-to-end-data-quality)**