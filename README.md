# Vendor Ranking and Analysis with Power BI
![image](https://github.com/user-attachments/assets/abd9cb56-b75c-4c7b-8a0c-802da1465407)

## 📄 **Overview**
This project demonstrates the use of Power BI to analyze vendor performance based on purchase data. The analysis incorporates measures such as total quantities ordered, average receiving times, and the number of purchase orders to rank vendors effectively. The project is designed to showcase advanced data modeling, calculated fields, and interactive visualizations.

## 📊 **Key Features**
- **Interactive Dashboards**: Visualize vendor performance and rankings with dynamic charts and tables.
- **Custom Weighting**: Apply flexible weightings to ranking criteria for tailored vendor evaluations.
- **Calculated Fields**: Include advanced DAX calculations such as total quantities, distinct PO counts, and weighted ranks.
- **Data Modeling**: Demonstrates relationships between `Vendor List` and `Purchase List`.

## 🗂️ **Project Structure**
```
Vendor-Ranking-Analysis/
│
├── data/
│   ├── Purchase List 1.xlsx  # Simulated purchase data (file was split due to github restrictions)
│   ├── Purchase List 2.xlsx  # Simulated purchase data
│   ├── Vendor List.xlsx    # Vendor details
│
├── PowerBI/
│   └── Vendor Ranking Analysis.pbix # Power BI project file
│
├── visuals/
│   ├── Vendor Ranking Summary.png  # Overview of vendors (currently ranked bad to good)
│   ├── Vendor Ranking Relationships.png   # Table and their relationships
│   ├── One Vendor (Red).png   # Summary view with one vendor chosen
│   ├── One Vendor (Green).png   # Summary view with one vendor chosen
│
├── documentation/
│   ├── data_dictionary.md  # Detailed explanation of fields
│   ├── Business Rules.md   # Rules and assumptions used
│
├── README.md               # Project overview and instructions
```

## 🧑‍💻 **Getting Started**
### Prerequisites
- [Microsoft Power BI](https://powerbi.microsoft.com/)
- A basic understanding of DAX and data visualization concepts

### Setup Instructions
1. Clone the repository:
   ```bash
   git clone https://github.com/cometkaren/Vendor-Ranking-Analysis.git
   ```
2. Open the `Vendor Ranking.pbix` file in Power BI Desktop.
3. Combine `Purchase List 1.xlsx` and `Purchase List 2.xlsx` into a single file (`Purchase List.xlsx`)
4. Review the data sources (`Purchase List.xlsx` and `Vendor List.xlsx`) in the **Data** folder.

## 📁 **Data**
- **Purchase List**: Contains details of purchase orders, including quantities, dates, and vendor IDs.
- **Vendor List**: Contains vendor-specific details such as names and IDs.
- **Weights**: Specifies the weightings for ranking criteria (e.g., quantity, receiving days, number of POs). Adjust for custom rankings.

For a detailed explanation, refer to the [Data Dictionary](./documentation/data_dictionary.md).

## 🖼️ **Visualizations**
### Example Visuals:
- **Bar Chart**: Compares final rankings across vendors.
- **Cards**: Quickly view the indvidual rankings; colour coded to show good (green), neutral (yellow), bad (red)
- **Filterable**: click on any bar or vendor name within the chart to filter all visuals

## ⚙️ **Features Demonstrated**
- **Data Modeling**: Linking tables with one-to-many relationships.
- **DAX Measures**:
  - `Total Qty Ordered`
  - `Distinct PO Count`
  - `Weighted Rank`
- **Custom Columns**:
  - Rank calculations with flexible weights.
- **Visualizations**:
  - Tables, bar charts, and filtering to explore the data interactively.

## 📌 **Notes**
- **Ranking Logic**: Lower scores indicate better vendor performance.
- **Assumptions**: Data is simulated for demonstration purposes and may not reflect real-world complexities.

## ✨ **Future Improvements**
- Automate data refresh from external sources.
- Include additional KPIs, such as cost-effectiveness, defect rates, rebates or payment terms.
- Extend analysis to include geographic or seasonal trends.
