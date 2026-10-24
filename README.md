# 📊 UK E-Commerce Sales & Customer Behavior Analytics

An end-to-end Power BI analytics solution engineered to transform raw, transactional e-commerce data into actionable executive insights. This project models over 500,000 transactions from a UK-based online retailer, establishing a robust normalized data architecture, business-critical DAX measures, and a modern two-page interactive report designed following SaaS UI/UX design standards.

---

## 📌 Executive Summary

* **Dataset:** UCI / Kaggle UK Online Retail Transaction Dataset (~500k records, Dec 2010 – Dec 2011).
* **Core Business Problem:** The executive team lacked visibility into net revenue performance after accounting for returns/cancellations, customer concentration risk, and product volume elasticity across international markets.
* **Key Achievements:**
  * Cleaned and transformed raw transactional log files into a star-schema analytical model using Power Query ETL.
  * Formulated time-intelligence and customer profiling DAX metrics to track Month-over-Month (MoM) momentum and Average Order Value (AOV).
  * Designed an executive overview and a granular product/customer deep-dive interface with dynamic cross-filtering and modern visual container cards.

---

## 🏗 Data Architecture & Data Model

To ensure query performance, scalable filtering, and accurate time intelligence, the flat transaction file was normalized into a **Star Schema** with single-direction 1-to-Many relationships.
