# Superstore-Performance-Analytics-Dashboard-Tableau-Data-Visualization

## README.md: US Superstore Executive Analytics Dashboard

### **1. Dataset Description: `sample_-_superstore.xls`**
The dataset provides a comprehensive record of retail transactions for a multi-national superstore. It consists of two primary tables linked by `Order ID`: **Orders** and **Returns**.

*   **Row Count:** 9,994 transaction records.
*   **Unique Orders:** 5,009 distinct order IDs.
*   **Column Structure:**
    *   **Categorical/Dimensions:** Includes `Category`, `Sub-Category`, `Region`, `State`, and `Product Name`.
    *   **Temporal:** `Order Date` and `Ship Date` (covering 2014–2017).
    *   **Numerical/Measures:** `Sales`, `Quantity`, `Discount`, and `Profit`.
*   **Data Quality Considerations:**
    *   **No Missing Values:** The core transaction columns (Sales, Profit, etc.) are 100% complete.
    *   **Granularity:** Data is at the line-item level; multiple rows may exist for a single `Order ID`.
    *   **Relational Integrity:** Accurate Return Rate analysis requires a **Left Join** between the Orders and Returns sheets to avoid dropping non-returned orders.

---

### **2. Problem Statement & Solution**
**The Problem:**
Despite a high total sales volume of **$2.30M**, the superstore faced inconsistent profitability across different US regions. Management needed to identify "Profit Leaks" and understand if high discounting was eroding margins.

**The Solution:**
I developed a dynamic business intelligence dashboard to isolate the root causes of loss. By implementing interactive filtering and parameter-driven metrics, I identified that the **Texas** and **Illinois** regions were significant loss centers. The analysis revealed a direct correlation between average discounts exceeding **30%** and negative profit margins in sub-categories like **Binders** and **Tables**.

---

### **3. Dashboard Visualizations & Logic**

| Graph Type | Purpose | Underlying Logic |
| :--- | :--- | :--- |
| **Big Angry Numbers (BANs)** | Provides immediate high-level health checks for the CEO. | Uses aggregate calculations for **Total Sales**, **Profit Margin (12.47%)**, and **Return Rate (8.06%)**. |
| **Geospatial Map** | Visualizes regional performance and identifies "Red Zones". | Maps `State` to a red-green diverging palette based on the `SUM(Profit)`. |
| **Area Chart (Monthly Trends)** | Tracks seasonality and long-term business growth. | Uses a continuous date axis (Month/Year) to show sales volume by category over time. |
| **Scatter Plot (Profit vs. Discount)** | Investigates the impact of pricing strategies on the bottom line. | Compares `AVG(Discount)` against `SUM(Profit)` to highlight how high discounts lead to losses. |
| **Donut Chart (Segment Analysis)** | Displays the contribution of each customer segment to total metrics. | Slices the `Selected Metric` (Sales/Profit/Orders) by the `Segment` dimension. |
| **Viz-in-Tooltip (Bar Charts)** | Provides deep-dive details without cluttering the main view. | Embeds sub-category profit bars that filter automatically when hovering over any category or date point. |

---

### **4. Interactive Features**
*   **Metric Parameter:** A dynamic "switch" that allows the entire dashboard to toggle between **Sales**, **Profit**, and **Order Count** views.
*   **Global Filters:** Users can drill down by **Year** or **Region** to see specific local trends.
