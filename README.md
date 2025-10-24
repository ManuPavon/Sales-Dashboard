# Sales Dashboard - Part 1

A comprehensive sales analytics dashboard built in Power BI featuring interactive visualizations for tracking sales performance, profitability, and trends across multiple dimensions.

## 📊 Dashboard Preview

<img width="1270" height="803" alt="imagen" src="https://github.com/user-attachments/assets/6e6d5214-7e3a-485e-9ba5-6954962ea1f8" />


## ✨ Key Features

- **KPI Cards**: Quick view of Sales, Profit, Orders, and Year-over-Year comparison
- **Monthly Trends**: Comparative analysis of current vs. last year sales with rounded column charts
- **Geographic Analysis**: Sales distribution by country (USA 98.34%, Canada 1.66%) with interactive donut chart
- **Profit vs Sales Correlation**: Dual-line chart showing relationship between revenue and profitability over time
- **Category Performance**: Sales breakdown across product categories (Technology, Office Supplies, Furniture)
- **Scatter Plot Analysis**: Profit and Sales relationship visualization for deeper insights

## 🛠️ Technical Stack

- **Power BI Desktop**
- **Custom Deneb visuals** with Vega-Lite for rounded column charts
- **DAX measures** for calculations
- **Interactive filters** for Year, Region, Category, and additional dimensions

## 📁 Data Model

- Orders table with Sales, Profit, Category, Country, and Date dimensions
- createOrReplace
    table Metrics
        lineageTag: 27684c67-8ede-4040-a341-315082043178

        /// Measure: Below Target Sales
        /// Returns total sales only when below last year's target
        measure #Below_Target_Target =
            IF (
                [#Total_Sales] < [#LY_Sales],
                [#Total_Sales],
                BLANK()
            )
            lineageTag: c0d11428-3f15-4320-9261-0854a49ea63b

        /// Measure: Total Sales
        /// Sum of all sales from Orders table
        measure #Total_Sales = 
            SUM ( Orders[Sales] )
            formatString: \$#,0;(\$#,0);\$#,0
            lineageTag: 23ab68cc-5f85-435c-a8f3-ff129945c9e4

        /// Measure: Last Year Sales
        /// Calculates total sales from the previous year
        measure #LY_Sales =
            CALCULATE (
                SUM ( Orders[Sales] ),
                DATEADD ( 'Calendar'[Date], -1, YEAR )
            )
            /* Alternative approach:
            CALCULATE (
                [#Total_Sales],
                CALCULATETABLE (
                    DATEADD ( 'Calendar'[Date], -1, YEAR )
                )
            )
            */
            lineageTag: e0e72da1-ff3e-4b61-ba6a-b78988e9108d
            annotation PBI_FormatHint = {"isGeneralNumber":true}

        /// Measure: Data Label
        /// Returns the maximum value between current and last year sales
        measure #Data_label = 
            MAX ( [#Total_Sales], [#LY_Sales] )
            lineageTag: 628f3ad2-da32-449c-aed5-4638a82fdbe8
            annotation PBI_FormatHint = {"isGeneralNumber":true}

        /// Measure: Lower Bar
        /// Baseline value for bar charts
        measure #Lower_Bar = 0
            formatString: 0
            lineageTag: c1fcd124-399c-4260-8918-bf95b6f0a46f

        /// Measure: Total Profit
        /// Sum of all profit from Orders table
        measure #Profit = 
            SUM ( Orders[Profit] )
            formatString: \$#,0;(\$#,0);\$#,0
            lineageTag: 41b22d9f-8ce5-48ec-bc5b-dbec05624407

        /// Measure: Above Target Sales
        /// Returns total sales only when meeting or exceeding last year's target
        measure #Above_Target_Target =
            IF (
                [#Total_Sales] >= [#LY_Sales],
                [#Total_Sales],
                BLANK()
            )
            lineageTag: a294a45e-4eb4-4ed9-9e23-fc332ff49e0d
            annotation PBI_FormatHint = {"isGeneralNumber":true}

        /// Partition: Metrics
        /// Empty table structure for measure storage
        partition Metrics = m
            mode: import
            source =
                let
                    Source = Table.FromRows(
                        Json.Document(
                            Binary.Decompress(

## 🚀 Getting Started

1. Clone this repository
2. Open the `.pbix` file in Power BI Desktop
3. Refresh the data source connections
4. Explore the interactive dashboard

## 📝 Requirements

- Power BI Desktop (latest version recommended)
- Deneb custom visual (available from AppSource)

## 👤 Author

Manu P.
