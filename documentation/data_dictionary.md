# Data Dictionary

## Purpose
This data dictionary provides detailed descriptions of the fields used in the Power BI project. It serves as a reference for understanding the structure, calculations, and relationships of the data model.

## Scope
The data dictionary includes details for the `Purchase List` and `Vendor Table` tables, including raw data columns and calculated fields.

## Table Relationships
- `Purchase List` is linked to `Vendor List` by the `VendorID` column (one-to-many relationship).
- Each vendor in the `Vendor Table` can have multiple purchase orders in the `Purchase List`.
![image](https://github.com/user-attachments/assets/9e14cd7f-5f22-4f2a-bafe-97eb80f9006b)


## Business Rules
- A vendor's rank is calculated based on the total quantity ordered, average receiving days, and the number of purchase orders.
- Vendors with lower total ranks are considered better performers.
- Weightings for rankings are as follows: Quantity (20%), Receiving Days (50%), and Number of POs (30%).

## Data Sources
- `Purchase List`: Simulated data from Kaggle datasets, representing purchase orders and related details.
  - https://www.kaggle.com/code/abdulmelikhmeda/inventory-purchase-sales-analysis-and-optimization/input?select=PurchasesFINAL12312016.csv
- `Vendor Table`: Derived from the `Purchase List` data by aggregating vendor-specific metrics.

## Assumptions
- All dates are formatted in `YYYY-MM-DD`.
- Each `PO Number` is unique to a single purchase order.
- Vendors are assumed to have valid `VendorID` entries in both `Purchase List` and `Vendor Table`.

## Limitations
- The dataset does not account for partial deliveries or backorders.
- Simulated data may not fully represent real-world scenarios.
- Rankings may not reflect business-critical factors outside of the provided data.

## Glossary
- **PO**: Purchase Order.
- **Qty**: Quantity.
- **Rec Days**: Receiving Days; The number of days between the purchase order date and receiving date.
- **Rank**: A numerical value representing the relative performance of vendors.

## Purchase List Data

| Column Name      | Description                                        | Data Type   | Example Value      | Notes                                      |
|------------------|----------------------------------------------------|-------------|--------------------|--------------------------------------------|
| Store ID         | Unique identifier for each store                   | Integer     | 56                 | Represents a specific location             |
| Inventory ID     | Unique identifier for each item                    | Integer     | 2478               | Represents a specific inventory item       |
| Vendor ID        | Unique identifier for each vendor                  | Integer     | 4425               | Represents a specific vendor               |
| PO Number        | Identifier for each Purchase Order                 | Integer     | 10384              | Represents a specific purchase order       |
| PODate           | Date when the purchase order was made              | Date        | 2023-03-15         | The date should be formatted as YYYY-MM-DD. |
| ReceivingDate    | Date when the goods were received                  | Date        | 2023-03-20         | The date should be formatted as YYYY-MM-DD. |
| InvoiceDate      | Date when the goods were invoiced                  | Date        | 2023-03-20         | The date should be formatted as YYYY-MM-DD. |
| PayDate          | Date when the goods were paid                      | Date        | 2023-03-20         | The date should be formatted as YYYY-MM-DD. |
| PurchasePrice    | Price per unit of the item                         | Decimal     | 5.75               | The cost per unit of the item.             |
| Quantity         | Quantity of items ordered                          | Integer     | 150                | Number of items in the order.              |
| TotalValue       | Total cost of the order (Qty * UnitPrice)          | Decimal     | 862.50             | Calculated value.                          |
| Days to Invoice  | # of Days from Order to Invoice (DATEDIFF([PODate],[InvoiceDate],DAY))                   | Integer     | 10                 | Calculated value.                          |
| Days to Pay      | # of Days from Order to Payment (DATEDIFF([PODate],[PayDate],DAY))                   | Integer     | 10                 | Calculated value.                          |
| Days to Receive  | # of Days from Order to Receiving (DATEDIFF([PODate],[ReceivingDate],DAY))                 | Integer     | 10                 | Calculated value.                          |

## Vendor List Data

| Column Name         | Description                                             | Data Type   | Calculation/Formula                  | Notes                                      |
|---------------------|---------------------------------------------------------|-------------|-------------------------------------|--------------------------------------------|
| Vendor_ID           | Unique identifier for each vendor                       | Integer     |                                     | Represents a specific vendor               |
| VendorName          | Name of the vendor                                      | String      |                                     | Full name of the vendor                    |
| TotalQty            | Total quantity of items ordered for each vendor         | Integer     | SUM('Purchase List'[Qty])           | Aggregated from the `Qty` column in the `Purchase List` table. |
| QtyRank             | Rank of vendors based on total quantity ordered (descending) | Integer  | RANKX(ALL('Vendor List'), 'Vendor List'[TotalQtyOrdered], , DESC, DENSE) | Uses the `RANKX` function to assign a rank to each vendor. |
| Avg Rec Days        | Average number of days from order to receipt for each vendor         | Integer     | AVERAGE('Purchase List'[Days to Receive])           | Aggregated from the `Days to Receive` column in the `Purchase List` table. |
| Rec Days Rank       | Rank of vendors based on average receiving days (ascending) | Integer  | RANKX(ALL('Vendor List'), 'Vendor List'[AvgRecDays], , ASC, DENSE) | Uses the `RANKX` function to assign a rank to each vendor. |
| TotalPOs            | Number of unique purchase orders for each vendor        | Integer     | CALCULATE(DISTINCTCOUNT('Purchase List'[PONumber]), FILTER('Purchase List', 'Purchase List'[Vendor ID] = 'Vendor Table'[Vendor_ID])) | Counts the distinct number of purchase orders linked to each vendor. |
| POs Rank            | Rank of vendors based on total quantity of orders (descending) | Integer  | RANKX(ALL('Vendor List'), 'Vendor List'[Total POs], , DESC, DENSE) | Uses the `RANKX` function to assign a rank to each vendor. |
| Qty Wt Rank         | Weighted rank based on custom weights for quantity      | Decimal | ('Vendor List'[Qty Rank] * LOOKUPVALUE('RankingWeights'[Weight], 'RankingWeights'[Measure],"Total Qty") | A weighted score using specified weights for quantity. |
| Days Wt Rank        | Weighted rank based on custom weights for Days to Receive      | Decimal | ('Vendor List'[POs Rank] * LOOKUPVALUE('RankingWeights'[Weight], 'RankingWeights'[Measure],"# of PO's") | A weighted score using specified weights for # of days to receive. |
| PO Wt Rank          | Weighted rank based on custom weights for orders        | Decimal | ('Vendor List'[Rec Days Rank] * LOOKUPVALUE('RankingWeights'[Weight], 'RankingWeights'[Measure],"Rec Qty") | A weighted score using specified weights for days to receive. |
| Total Rank          | Sum of all weighted ranks for each vendor               | Decimal | ('Vendor List'[Qty Wt Rank]+'Vendor List'[Days Wt Rank]+'Vendor List'[PO Wt Rank]) | Aggregates the weighted ranks to get the final score for each vendor.  |

### Ranking Weights Data

| Column Name      | Description                                        | Data Type   | Example Value      | Notes                                      |
|------------------|----------------------------------------------------|-------------|--------------------|--------------------------------------------|
| Measure          | List of the criteria for weighting each vendor     | String      | Total Qty          | Represents a specific criteria for weighting    |
| Weight           | Input percentage criteria should be weighted (as a decimal)   | Decimal     | 0.2     | Represents the specific weighting (entire column must total 100%)  
