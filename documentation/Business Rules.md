# **Business Rules**

## **1. Overview**
This document outlines the business rules applied in the Vendor Ranking and Analysis project. 
These rules define the logic, assumptions, and processes used to rank vendors based on performance metrics derived from the `Purchase List` and `Vendor List`. 
The rankings aim to help stakeholders identify high-performing vendors based on key criteria.

## **2. Ranking Logic**
Vendors are ranked based on three performance metrics, with weights applied to reflect their relative importance:

### **Metrics**:
1. **Total Quantity Ordered (20%)**: Sum of all units ordered from each vendor.
2. **Average Receiving Days (50%)**: Average number of days between placing an order (`PODate`) and receiving the goods (`ReceivingDate`).
3. **Number of Purchase Orders (30%)**: Count of distinct purchase orders associated with each vendor.

### **Ranking Formula**:
Each metric is ranked independently:
- Lower values for **Average Receiving Days** and **Ranked Metrics** are considered better.
- A final weighted score is calculated for each vendor using the formula:
  \[
  \text{Weighted Score} = (\text{Qty Rank} \times 0.2) + (\text{Rec Days Rank} \times 0.5) + (\text{POs Rank} \times 0.3)
  \]
Vendors with the **lowest weighted score** are ranked highest.

## **3. Data Processing Rules**
- **Date Handling**:
  - All `PODate` and `ReceivingDate` values are validated to ensure they are complete and correctly formatted.
  - Orders with missing or null date values are excluded.
- **Aggregations**:
  - Total quantities, distinct purchase orders, and average receiving days are calculated at the vendor level.
- **Relationships**:
  - The `Purchase List` table is linked to the `Vendor List` table through the `VendorID` field.

## **4. Assumptions**
- Vendors fulfill purchase orders without partial quantities.
- All data is accurate and complete as provided in the dataset.
- Weights assigned to the ranking metrics are determined by business stakeholders and reflect their priorities.

## **5. Constraints**
- The analysis does not consider costs or price discounts in vendor rankings.
- The dataset may not include all external factors influencing vendor performance, such as quality or geographic proximity.
- Simulated data has been used, and rankings may not perfectly reflect real-world scenarios.

## **6. Exclusions**
The following records are excluded from the analysis:
- Purchase orders with missing or null `PODate` or `ReceivingDate`.
- Vendors with no associated purchase orders.

## **7. Reporting Guidelines**
- **Dashboard Title**: "Vendor Performance Analysis"
- **Clarification**: A note on the dashboard specifies, "Lower scores represent better vendor performance."
- **Dynamic Filters**: Interactibility (clicking) allow users to filter by:
  - Vendor

## **8. Future Enhancements**
To further improve the analysis, consider:
1. Adding cost and quality metrics to the ranking criteria.
2. Including a geographic dimension to account for delivery locations.
3. Incorporating seasonal trends in order volume and receiving times.
