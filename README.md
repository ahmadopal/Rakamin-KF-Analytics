# 📊 Kimia Farma Big Data Analytics - Project-Based Internship Program

## 📌 About the Program
The Project-Based Internship Program, a collaboration between **Rakamin Academy** and **Kimia Farma Big Data Analytics**, is a self-development and career acceleration program intended for those of you who are interested in exploring the Big Data Analytics position at the Kimia Farma company. This program provides access to basic learning in the form of Article Review (reading material) and Company Coaching Video (video learning) to introduce you to the competencies and skills that Big Data Analytics must have in the company. In addition to the material, there will be testing of your learning outcomes in the form of Task questions every week and ends with the creation of a final project that will become your portfolio in this program.

## 🏢 About Kimia Farma

**Kimia Farma** is the first pharmaceutical industry company in Indonesia, established by the Dutch East Indies Government in 1817. The name of the company was originally **NV Chemicalien Handle Rathkamp & Co**. Based on the nationalization policy of former Dutch companies in the early days of independence, in 1958, the Government of the Republic of Indonesia consolidated a number of pharmaceutical companies into PNF (State Pharmaceutical Company) **Bhinneka Kimia Farma**. Then on August 16, 1971, the legal form of PNF was changed to a Limited Liability Company, so the company name changed to **PT Kimia Farma (Persero)**.

## 🎯 Project Objectives

The project aims to:
- Create an analysis table based on Transactions, Products, Branches, and Inventory data
- Create a dashboard analyzing **Kimia Farma's** performance 2020-2023 in **Google Looker Studio**
- Evaluate **Kimia Farma's** business performance from 2020 to 2023

## 🧰 Tools required

- **Google BigQuery** – for data warehousing and querying
- **SQL** – for data processing and analysis
- **Google Looker Studio** – for dashboard creation and data visualization

## 📂 Datasets

The following datasets were used:
1. `kf_final_transaction` – Final transaction data
2. `kf_inventory` – Inventory data
3. `kf_kantor_cabang` – Branch office information
4. `kf_product` – Product details
5. 'kimia_farma_analysis' - Merged dataset
   
🔗 [Dataset Here](https://drive.google.com/drive/folders/1MhoRZymclQnXH36bqXZR5UvDGlAImKsD?usp=sharing)

---

## 🛠️ Process & Challenges

### ✅ Challenge 1: Import Datasets to BigQuery

Imported four datasets into Google BigQuery for further processing and analysis.

---

### ✅ Challenge 2: Create Analysis Table

A combined table called `kf_analisa_penjualan` was created by joining transaction, branch, and product data, along with calculated fields such as nett sales and nett profit.

### BigQuery Syntax

```sql
SELECT
  ft.transaction_id,
  ft.date,
  ft.branch_id,
  kc.branch_name,
  kc.kota,
  kc.provinsi,
  kc.rating AS rating_cabang,
  ft.customer_name,
  ft.product_id,
  p.product_name,
  ft.price AS actual_price,
  ft.discount_percentage,

  -- Persentase gross laba berdasarkan kategori harga
  CASE 
    WHEN ft.price <= 50000 THEN 0.10
    WHEN ft.price > 50000 AND ft.price <= 100000 THEN 0.15
    WHEN ft.price > 100000 AND ft.price <= 300000 THEN 0.20
    WHEN ft.price > 300000 AND ft.price <= 500000 THEN 0.25
    ELSE 0.30
  END AS persentase_gross_laba,

  -- Nett Sales = price setelah dikurangi diskon
  ROUND(ft.price * (1 - ft.discount_percentage / 100), 2) AS nett_sales,

  -- Nett Profit = nett_sales * persentase_gross_laba
  ROUND(ft.price * (1 - ft.discount_percentage / 100) *
    CASE 
      WHEN ft.price <= 50000 THEN 0.10
      WHEN ft.price > 50000 AND ft.price <= 100000 THEN 0.15
      WHEN ft.price > 100000 AND ft.price <= 300000 THEN 0.20
      WHEN ft.price > 300000 AND ft.price <= 500000 THEN 0.25
      ELSE 0.30
    END, 2) AS nett_profit,

  ft.rating AS rating_transaksi

FROM
  `kimia_farma.kf_final_transaction` ft
JOIN
  `kimia_farma.kf_kantor_cabang` kc
ON
  ft.branch_id = kc.branch_id
JOIN
  `kimia_farma.kf_product` p
ON
  ft.product_id = p.product_id

---

### ✅ Challenge 3: Build Dashboard in Google Looker Studio

An interactive dashboard was built with the following insights:
-  Total Transaction, Customer, and Revenue Trends
-  Total Annual Sales
-  Top 10 Total Branch Transactions
-  Top 10 Nett Sales by Branch/Province
-  High-rated Branches with Low Transactions
-  Profit Distribution across Regions
