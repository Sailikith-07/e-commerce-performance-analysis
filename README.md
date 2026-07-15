# GSL Mart Performance Analysis: Diagnosing the Gross-to-Net Margin Meltdown

## 1. Project Background & Overview

**GSLmart India** is a fast-growing, multi-category D2C e-commerce storefront operating on a high-velocity Shopify Plus architecture. To scale domestic operations and optimize its retail supply chain across diverse geographic territories, the company integrates its core front-end transactional data with a robust backend Logistics Aggregator API middleware. This unified data ecosystem tracks everything from storefront user sessions, product category collections, and customer profiles to localized shipping carrier performance, delivery distance metrics, and environmental transit variables.

### 🚨 The Business Problem: The Gross-to-Net Meltdown
Despite exceptional top-line performance generating an impressive **₹10.12M in Gross Revenue**, GSLmart India faces a critical profitability crisis, retaining a meager **₹887.32K in Net Profit**. A staggering **91.2% of revenue leaks** out of the business before hitting the bottom line, driven primarily by heavy customer acquisition costs, a crippling **₹4.37M** spent on defensive discounting, and **₹5.0M** devoured by logistics and fulfillment expenses. 

This margin erosion is severely compounded by a high **21.81% average delivery delay rate**, causing high-intent users to abandon checkouts, forcing first-time buyers into "Lost" or "Hibernating" lifecycle segments, and trapping the brand on an expensive marketing treadmill.

### 🎯 Project Objective
This data analytics project conducts a rigorous, end-to-end diagnosis of GSLmart India’s unified sales and logistics operations to isolate exact points of revenue leakage and customer churn. By transforming raw transactional records into a highly interactive, executive-ready dashboard, this study provides leadership with a concrete, data-backed strategic roadmap to eliminate checkout friction, optimize the regional shipping carrier hierarchy, execute tiered discounting, and maximize net profit margins.

*📂 **Looking for the technical implementation?** You can view the raw [SQL CTE and Query Optimization scripts here](./queries.sql) and the [Python Data Cleaning and ETL scripts here](./etl_pipeline.py).*

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

---

## 3. Executive Summary (CEO Dashboard)

An analysis of GSLmart’s performance metrics indicates that **logistics costs, cart abandonment, and customer churn are the primary drivers of margin erosion**, rather than a lack of market demand. 

*   **Logistics is the Primary Profit Leak:** At **₹5.0M**, logistics costs devour nearly half of gross revenue, outstripping the heavy **₹4.37M** spent on discounting. 
*   **Geographic Concentration Risk:** Customer activity is heavily concentrated, with the top three states (**Maharashtra, Madhya Pradesh, and Chhattisgarh**) driving **~₹3.1M** of total revenue.

```
+------------------+------------------+------------------+
|  Gross Revenue   |  Logistics Spend  |    Net Profit    |
|     ₹10.12M      |      ₹5.00M      |     ₹887.32K     |
+------------------+------------------+------------------+
|   Total Orders   |  Avg Delay Rate  |   Total Items    |
|      6,000       |      21.81%      |       62K        |
+------------------+------------------+------------------+
```

*(Note: Insert a clean, minimalist screenshot of your main Power BI or Tableau dashboard here)*  
`![GSL Mart Executive Performance Dashboard](path_to_your_dashboard_image.png)`

---

## 4. Deep-Dive Insights

### 📣 Marketing & Checkout Performance
*   **Cart Abandonment Leak:** A critical ~65% cart abandonment rate across all acquisition channels causes heavy funnel leakage (25K sessions → 16K carts → 6K orders), indicating that checkout friction or hidden pricing is wasting acquisition spend.
*   **Ineffective Discounting:** Slashing prices shows zero statistical impact on customer acquisition, with conversion rates stalling flat at 22-24% regardless of discount size, turning promotions into a pure margin leak.
*   **Channel Misallocation:** High-intent Organic Search vastly outperforms paid channels (generating ₹1.82M at 23.6% conversion), proving that paid ad spend is currently being funneled into lower-converting streams.

### 🚛 Logistics & Fulfillment Optimization
*   **Broken Last-Mile Pricing:** Short-haul delivery costs are twice as high per kilometer (₹16/km under 50km) due to heavy fixed processing fees, meaning local express fulfillment is underpriced and running at a loss.
*   **Carrier Selection Variance:** Delhivery operates with peak efficiency (19.31% delay, 4.21% failure), while Blue Dart causes severe service issues (22.64% delay, 5.95% failure) despite charging a higher premium per package.
*   **Weather Disruptions:** Stormy (34%) and rainy (32%) conditions drive 66% of controllable transit delays, revealing a clear need for weather-integrated predictive delivery buffers.

### 📦 Product Performance
*   **High-Discount Revenue Leaks:** Products with an aggressive Revenue-to-Discount (R to D) ratio of ≤ 2.0 caused a severe ₹1.65M revenue loss because deep price cuts failed to scale order quantities past 75 units.
*   **Category Margin Traps:** Books are the least profitable category with a meager ₹2K net profit due to high discount rates and high delivery costs, while Footwear (₹198K) and Home & Kitchen (₹179K) emerge as top profit drivers.
*   **Regional Stream Dominance:** The West Region stands as the clear geographic winner, contributing a dominant 26% of GSLmart's total revenue streams across India.

### 👥 Customer Lifecycle & Retention
*   **Stagnant Revenue Capital:** Over 60% of brand revenue (~₹6.1M) is locked away in inactive customer groups (Hibernating at ₹4.7M and Lost at ₹1.4M), highlighting an urgent need for targeted win-back campaigns over new acquisition spend.
*   **Logistics-Driven Churn:** Courier service failures directly trigger customer disengagement, as the highest delivery delays are heavily concentrated among churned cohorts (Hibernating at 22.3% and Lost at 21.7% delay rates).

---

## 5. Strategic Recommendations

### 1. Optimize Logistics Economics & Carrier Allocation
*   **Shift Volume:** Instantly move shipping volume away from Blue Dart and give it to Delhivery. This cuts failure rates and lowers shipping costs by **~₹58 per delivery**.
*   **Adjust Local Pricing:** Add a flat handling surcharge for short-distance express deliveries under 50 km to cover fixed carrier costs.

### 2. Fix Checkout Friction & High Cart Abandonment
*   **Smooth out the Funnel:** Improve checkout loading speeds and simplify payment steps to reduce the **65% cart abandonment rate**.
*   **Smart Promos:** Stop using flat, site-wide discounts. Switch to tiered promotions (e.g., *“Save ₹200 on orders over ₹2,500”*) to raise order values and offset shipping fees.

### 3. Reform Product Pricing & Inventory Placement
*   **Enforce Discount Limits:** Stop running promotions on products where the R to D ratio falls below 2.0 unless they clear a high-volume target of 75+ units.
*   **Drop or Bundle Books:** Restructure the Books category by turning them into multi-item bundles to cover shipping fees, while putting more ad spend into Footwear and Home & Kitchen.
*   **West Hub Inventory:** Store top-selling items directly in regional fulfillment centers out West to lower shipping distances and cut delivery delays.

### 4. Deploy Lifecycle Win-Back Campaigns
*   **Target Inactive Buyers:** Launch re-engagement email campaigns specifically built for the **₹4.7M Hibernating customer pool**.
*   **Apology Perks:** Send out win-back offers to At-Risk and Lost customers that include a small discount combined with guaranteed, fast-priority shipping.

---

## 6. Caveats & Assumptions

*   **Fixed Logistics Pricing Structure:** Assumes standard transactional carrier pricing without volume-based enterprise discounts.
*   **Manual Customer Segmentation:** Lifecycle categories are mapped using standard Recency, Frequency, and Monetary (RFM) limits.
*   **Missing Weather Telemetry:** Weather delay markers represent regional forecasts at the hub rather than precise mid-transit conditions
