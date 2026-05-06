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

## The Visualization Process & Technical Logic
**Step 1: Data Architecture & Connection**
**Action:** Connected the Orders and Returns sheets.

**Logic:** Applied a Left Join on Order ID. This ensures that all orders remain in the dataset as the denominator for the Return Rate, while only matching return records are brought in.

**Step 2: Creating Parameters (User Input)**
**Purpose:** To allow a single chart to switch between different metrics (Sales, Profit, # Orders).

**Implementation:** Created a String Parameter titled Metric Selector with a list containing "Sales", "Profit", and "Order".

**Step 3:** Building Calculated Fields (The Logic Layer)
To make the dashboard dynamic and accurate, the following calculated fields were engineered:
**1] Metric Choice (Switch Logic):**
``` 
CASE [Metric Selector]
  WHEN "Sales" THEN SUM([Sales])
  WHEN "Profit" THEN SUM([Profit])
  WHEN "Order" THEN COUNTD([Order ID])
END
```

* **Condition:** Used COUNTD (Count Distinct) for orders to ensure that an order with multiple line items is only counted once.

* **Condition:** Wrapped Sales and Profit in SUM to avoid "mixing aggregate and non-aggregate" errors.

**2]Return Rate:**
```SUM(IF [Returned] = "Yes" THEN [Quantity] ELSE 0 END) / SUM([Quantity])```

* **Logic:** Created a row-level IF statement to isolate returned quantities before aggregating them against total quantities.

**Step 4: Interactive Dashboarding**
* **Actions:** Set the State Map as a global filter.

**Logic:** When a user clicks a "Red State" (e.g., Texas), Tableau passes that state's value as a filter to the Scatter Plot and Trend Chart, allowing for instantaneous root-cause analysis.

## Conclusion
This project demonstrates the transition from raw data to strategic insights. By utilizing CASE statements, Parameters, and Relational Joins, the dashboard provides a high-level executive summary while maintaining the "drill-down" capability required for deep-dive data investigation.
