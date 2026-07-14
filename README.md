# GSL Mart Performance Analysis: Diagnosing the Gross-to-Net Margin Meltdown

## 1. Project Background & Overview

**GSLmart India** is a fast-growing, multi-category D2C e-commerce storefront operating on a high-velocity Shopify Plus architecture. To scale domestic operations and optimize its retail supply chain across diverse geographic territories, the company integrates its core front-end transactional data with a robust backend Logistics Aggregator API middleware. This unified data ecosystem tracks everything from storefront user sessions, product category collections, and customer profiles to localized shipping carrier performance, delivery distance metrics, and environmental transit variables.

### 🚨 The Business Problem: The Gross-to-Net Meltdown
Despite exceptional top-line performance generating an impressive **₹10.12M in Gross Revenue**, GSLmart India faces a critical profitability crisis, retaining a meager **₹887.32K in Net Profit**. A staggering **91.2% of revenue leaks** out of the business before hitting the bottom line, driven primarily by heavy customer acquisition costs, a crippling **₹4.37M** spent on defensive discounting, and **₹5.0M** devoured by logistics and fulfillment expenses. 

This margin erosion is severely compounded by a high **21.81% average delivery delay rate**, causing high-intent users to abandon checkouts, forcing first-time buyers into "Lost" or "Hibernating" lifecycle segments, and trapping the brand on an expensive marketing treadmill.

### 🎯 Project Objective
This data analytics project conducts a rigorous, end-to-end diagnosis of GSLmart India’s unified sales and logistics operations to isolate exact points of revenue leakage and customer churn. By transforming raw transactional records into a highly interactive, executive-ready dashboard, this study provides leadership with a concrete, data-backed strategic roadmap to eliminate checkout friction, optimize the regional shipping carrier hierarchy, execute tiered discounting, and maximize net profit margins.

*📂 **Looking for the technical implementation?** You can view the data cleaning and transformation steps [here]([./etl_pipeline.py](https://github.com/Sailikith-07/e-commerce-performance-analysis/blob/main/notebooks/Data%20Cleaning%20and%20Transforming.ipynb)).*

---

## 2. Unified Data Structure & Relationships
To understand GSLmart's multi-layered operation, the analytical model unites front-end transactional tables with backend logistics records. 

The entity relationship structure establishes a robust framework mapping storefront sessions, product category collections, customer segmentation metrics, and shipping aggregator logs:

```
                  [Customers] (ID, Location, Segment)
                       │
                       └───► [Orders] (ID, Revenue, Discount%, Date) ◄─── [Products] (SKU, Category)
                                │
                                └───► [Logistics_Logs] (Carrier, Distance, Delivery_Status, Delay_Time)
```

*(Note: If you have generated a visual ERD diagram, insert your image link below)* `![Database ERD Schema](path_to_your_erd_image.png)`

---

## 3. Executive Summary (CEO Dashboard)

An analysis of GSLmart’s performance metrics indicates that **logistics costs, cart abandonment, and customer churn are the primary drivers of margin erosion**, rather than a lack of market demand. 

* **Logistics is the Primary Profit Leak:** At **₹5.0M**, logistics costs devour nearly half of gross revenue, outstripping the heavy **₹4.37M** spent on discounting. 
* **A Low Average Order Value (AOV) Limits Margin Potential:** Despite generating **6,000 orders**, GSLmart's AOV sits at a low **₹1.80K**, meaning the brand pays high per-package shipping costs without the benefit of high basket values.
* **Geographic Concentration Risk:** Customer activity is heavily concentrated, with the top three states (**Maharashtra, Madhya Pradesh, and Chhattisgarh**) driving **~₹3.1M** of total revenue.

```
+------------------+------------------+------------------+
|  Gross Revenue   |  Logistics Spend  |    Net Profit    |
|     ₹10.12M      |      ₹5.00M      |     ₹887.32K     |
+------------------+------------------+------------------+
|   Average AOV    |   Total Orders   |  Avg Delay Rate  |
|      ₹1.80K      |      6,000       |      21.81%      |
+------------------+------------------+------------------+
```

*(Note: Insert a clean, minimalist screenshot of your main Power BI or Tableau dashboard here)* `![GSL Mart Executive Performance Dashboard](path_to_your_dashboard_image.png)`

---

## 4. Deep-Dive Insights

### 📣 Marketing & Checkout Performance
* **Significant Funnel Leaks:** Of the **25K initial sessions**, **16K users** add items to their cart, yet only **6K** complete their purchase. This reveals a **~65% cart abandonment rate** that remains consistent (63%–66%) across all acquisition channels. The drop-off points to friction in the checkout flow, hidden shipping costs, or localized payment processing failures, rather than poor traffic quality.
* **Ineffective Discounting Strategies:** A correlation analysis between promotional discounts and conversion rates reveals no statistical relationship. Conversion rates remain steady in the **22%–24%** range regardless of whether discounts are set at 8.8% or 9.1%. Defensive discounting represents a major source of margin erosion without driving incremental sales volume.
* **High-Performing Organic Traffic:** Organic Search represents GSLmart's most efficient channel, generating **₹1.82M in revenue** with the highest conversion rate (**23.6%**) and the lowest cart abandonment (**63.32%**). Conversely, Paid Search underperforms, delivering lower conversion rates (**21.66%**) and a lower revenue yield of **₹1.6M** despite higher promotional support.

```
       [ 25,000 Traffic Sessions ] 
                 │
                 ▼  (36% Churn)
       [ 16,000 Add-to-Carts ]
                 │
                 ▼  (65% Abandonment Leak) ◄── Critical Friction Point
       [ 6,000 Completed Orders ]
```

### 🚛 Logistics & Fulfillment Optimization
* **Distance-Driven Delivery Delays:** Delivery delay rates rise sharply with transit distance. Short-haul shipments maintain a modest **~10% delay rate**, whereas long-haul shipments (250–300 km) experience delay rates exceeding **30%**. Because GSLmart's average shipping distance is **150 km**, a substantial volume of orders sit directly at the inflection point where transit delays begin to compound.
* **Broken Last-Mile Pricing Models:** Short-haul deliveries cost significantly more on a per-kilometer basis. Transit costs sit at **₹16/km** for near-zero distances, dropping to **₹5/km** at 50 km, and flatlining thereafter. Fixed handling and last-mile processing fees disproportionately erode margins on local, short-haul deliveries, indicating that the express delivery tier is underpriced relative to actual fulfillment costs.
* **Shipping Carrier Performance Variance:** Delivery partners exhibit highly uneven performance despite similar average costs:
    * **Delhivery (Top Performer):** Achieves the lowest delay rate (**19.31%**) and lowest failure rate (**4.21%**) at a competitive average cost of **₹831.28** per shipment.
    * **Blue Dart (Underperformer):** Exhibits the highest delay rate (**22.64%**) and highest failure rate (**5.95%**) while commanding the highest average cost of **₹889.40** per shipment.
* **Weather Impact Patterns:** Weather-related delays are highly concentrated, with stormy (**34%**) and rainy (**32%**) conditions accounting for **66% of all weather-driven logistics bottlenecks**.

```
| Carrier Name | Avg Cost per Shipment | Delay Rate (%) | Delivery Failure Rate (%) |
| :--- | :---: | :---: | :---: |
| **Delhivery** | ₹831.28 | **19.31%** (Lowest) | **4.21%** (Lowest) |
| **Blue Dart** | ₹889.40 | **22.64%** (Highest) | **5.95%** (Highest) |
```

### 👥 Customer Lifecycle & Retention
* **Untapped Revenue in Inactive Segments:** A majority of historical revenue is tied up in disengaged cohorts. The **"Hibernating"** segment represents **₹4.7M in revenue**, while the **"Lost"** segment represents **₹1.4M**. Combined, these inactive groups account for over **60% of GSLmart's lifetime revenue (~₹6.1M)**, highlighting a significant opportunity for win-back campaigns.
* **Service Delivery Issues as a Churn Driver:** Churn is closely linked to poor delivery experiences. The segments experiencing the highest delivery delays are also those with the lowest engagement: **Hibernating (22.3% delay rate)**, **Lost (21.7%)**, and **At-Risk (19.5%)**. Conversely, active **Champions** maintain a low delay profile. Delivery delays act as a direct driver of customer churn.

---

## 5. Strategic Recommendations

Based on these findings, GSLmart India can improve net profit margins by focusing on the following areas:

### 1. Optimize Logistics Economics & Carrier Allocation
* **Reallocate Volume:** Immediately shift logistics volume away from Blue Dart and toward Delhivery, particularly for high-volume routes in Maharashtra and Madhya Pradesh. This shift is projected to reduce delivery failure rates and lower fulfillment costs by **~₹58 per shipment**.
* **Restructure Short-Haul Pricing:** Revise the pricing structure for short-haul/local express deliveries to include a flat-rate handling surcharge, protecting margins from high per-kilometer costs below 50 km.
* **Predictive Rerouting:** Build automatic carrier routing adjustments during stormy or heavy monsoon periods, shifting shipments on high-risk regional routes to partners with stronger local networks.

### 2. Fix Checkout Friction & High Cart Abandonment
* **Optimize the Checkout Flow:** Investigate checkout page load speeds, verify mobile payment integrations, and display estimated delivery dates upfront to address the **65% cart abandonment rate**.
* **Implement Exit-Intent Offers:** Introduce exit-intent overlays offering non-discount incentives (such as "Free Priority Shipping" on basket sizes above ₹2,000) rather than flat price discounts.

### 3. Move from Flat Discounting to Basket Expansion
* **Implement Tiered Discounting:** Eliminate flat, site-wide discounts. Transition to a tiered framework (e.g., *“Save ₹200 on orders over ₹2,500”*) to drive average order values past the current **₹1.80K** average, helping cover fixed fulfillment costs.
* **Bundle Low-Velocity Stock:** Identify low-performing categories and bundle them as cross-sell options on the cart page to increase overall order values.

### 4. Deploy Lifecycle Win-Back Campaigns
* **Target the "Hibernating" Segment:** Launch a targeted email and SMS win-back sequence aimed at the **₹4.7M Hibernating customer pool**.
* **Apology-Led Reactivation:** Frame win-back offers to **At-Risk** and **Lost** cohorts around an apology for past delivery delays, pairing the outreach with a guaranteed, priority-shipped delivery offer.

---

## 6. Caveats & Assumptions

* **Fixed Logistics Pricing Structure:** This analysis assumes standard transactional carrier pricing from the API logs. It does not account for volume-based enterprise discounts or fuel surcharge adjustments that may occur at quarterly boundaries.
* **Manual Customer Segmentation:** Customer lifecycle categories (e.g., "Hibernating", "At-Risk", "Champions") are mapped using standard Recency, Frequency, and Monetary (RFM) thresholds, which may require adjustment to align with changing seasonal purchasing cycles.
* **Missing Weather Telemetry:** Weather delay markers represent regional forecasts at the destination hub and do not capture localized, mid-transit micro-climatic events.
