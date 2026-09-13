---
title: "Building a Sales ETL Pipeline with Databricks, PySpark, and Delta Lake"
description: "A walkthrough of an end-to-end ETL pipeline built on Databricks using Medallion Architecture, PySpark, and Delta Lake, with automated data quality checks and a star schema model."
pubDate: 2026-09-13
heroImage: "/post_img.webp"
tags: ["databricks", "pyspark", "data-engineering", "etl", "delta-lake"]
---

Building a reliable **ETL pipeline** is one of the most common problems a data engineer solves day to day — and one of the best ways to learn it deeply is to build one end to end, outside of work, where you control every decision. That's what this post is about: a **Sales ETL pipeline** I built on **Databricks** using **PySpark**, **Delta Lake**, and the **Medallion Architecture** pattern, on a retail sales dataset.

## The Goal

The aim was to take raw, messy retail sales data and turn it into something trustworthy and query-ready — while building in the same practices used in production data platforms: layered architecture, data quality gating, and dimensional modeling. If you want to see the project itself, it's listed on my [Projects page](/projects).

## Why Medallion Architecture

The **Medallion Architecture** pattern organizes data into three layers:

### Bronze — Raw ingestion
Data lands here exactly as received, with minimal transformation. This preserves a full audit trail and lets you reprocess from scratch if downstream logic changes.

### Silver — Cleaned and conformed
This is where PySpark transformations and window functions clean, deduplicate, and standardize the data — fixing types, handling nulls, and applying business rules.

### Gold — Business-ready
The final layer holds aggregated, modeled data ready for reporting and analytics — in this case, a **star schema** with fact and dimension tables.

Using **Delta Lake** tables at every layer gave the pipeline ACID transactions and schema evolution — meaning failed writes don't corrupt data, and the schema can evolve safely as new fields show up in source data.

## Data Quality Isn't Optional

A pipeline that runs successfully but silently produces bad data is worse than one that fails loudly. So this project included a **53-check automated data quality validation framework**, with severity-based gating — meaning a critical failure stops the pipeline before bad data reaches the Gold layer, while lower-severity warnings get logged but don't block the run.

## Modeling for Analytics

Once cleaned, the data was modeled into a **star schema** — a central fact table (sales transactions) surrounded by dimension tables (product, customer, date, store). This structure is what makes the data genuinely useful for BI tools and analysts, rather than just "cleaned."

## Orchestration

The whole pipeline runs on a schedule via **Databricks Jobs**, with failure alerting so issues get caught immediately rather than discovered days later in a stale dashboard.

## What's Next

I'm currently extending this project with **AWS S3** integration for external storage, **Docker** for environment consistency, **CI/CD** for automated testing and deployment, and **Airflow** for more advanced orchestration beyond what Databricks Jobs alone provides.

If you're working on something similar or have questions about any part of this pipeline, feel free to [reach out](mailto:kathirvelrajmohan9498@gmail.com) — always happy to talk data engineering.