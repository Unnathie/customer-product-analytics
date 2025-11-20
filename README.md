## 📊 Project Description

This repository contains **SQL reporting views** built on the **Gold Layer** of a data warehouse[https://github.com/Unnathie/data-warehouse-project-sql/tree/main/scripts/gold], designed to generate high-quality analytical datasets for business intelligence, performance tracking, and customer insights.
It includes two major reporting views:

---

## **1. Product Performance Report (`report_prod`)**

Creates an end-to-end analytical layer for product-level performance.

### **Key Features**

* Merges product dimension data with sales fact data
* Computes **total sales, order count, quantity sold, unique customers**, and **product lifespan**
* Calculates **average selling price** using sales-to-quantity ratios
* Assigns **product performance categories**:

  * High-Performer
  * Mid-Performer
  * Low-Performer
* Derives **since last order**, **average revenue per order**, and **monthly revenue contribution**
* Helpful for: merchandising analytics, pricing decisions, inventory planning, product lifecycle studies

---

## **2. Customer Insights Report (`customer_report`)**

Generates a comprehensive customer-level dataset for behaviour analysis and segmentation.

### **Key Features**

* Combines customer dimension data with sales transactions
* Calculates **order count, total revenue, quantity purchased**, and **unique products bought**
* Measures **customer lifespan** and **last order activity**
* Adds **age segmentation**: Under 20, 20–29, 30–39, 40–49, 50+
* Assigns customer categories:

  * VIP
  * Regular
  * New
* Computes **CVO (Customer Value per Order)** and **average monthly spend**
* Useful for: churn analysis, CRM targeting, loyalty program insights, cohort reporting

---

## 🎯 Purpose

These views transform raw warehouse tables into **analysis-ready datasets** that support:

* BI dashboards
* Product and customer performance monitoring
* Marketing & CRM insights
* Revenue analytics
* Data-driven decision making
