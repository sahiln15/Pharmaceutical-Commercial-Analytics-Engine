# DAX Measure Reference

All measures live in a dedicated **Measures Table** (not attached to any physical fact/dimension table), organized into display folders inside the Power BI model.

## Folder: Core KPIs
- Total Units Sold
- Total Transactions
- Average Units Sold
- Average Daily Sales
- Average Units Per Day

## Folder: Time Intelligence
- Previous Month Units
- Previous Year Units
- Running Total
- Month-over-Month Growth %
- Year-over-Year Growth %
- Latest Month MoM Growth %
- Peak Volume Month

## Folder: Business Intelligence / Strategy
- Highest Selling Drug
- Fastest Growing Drug
- Top 1 Growth Product
- Top 1 Growth Value
- Bottom 1 Decline Product
- Bottom 1 Decline Value

## Folder: Dynamic Dashboard Support
- Drug Classes (distinct count / list)
- Last Refresh
- Maximum Units Sold
- Minimum Units Sold
- Sales Trend
- Dynamic Top Product
- Dynamic Peak Hour
- Dynamic Peak Weekday
- Tooltip MoM Growth %

## Confirmed Formulas

```dax
Total Units Sold = SUM(FactSales[Units Sold])

Total Transactions = COUNTROWS(FactSales)

MoM Growth % =
VAR CurrentMonthSales = [Total Units Sold]
VAR PreviousMonthSales =
    CALCULATE([Total Units Sold], DATEADD(DimCalendar[Date], -1, MONTH))
RETURN
    DIVIDE(CurrentMonthSales - PreviousMonthSales, PreviousMonthSales, 0)

Dynamic Top Product =
SELECTCOLUMNS(
    TOPN(1, ALL(FactSales[Drug Class]), CALCULATE(SUM(FactSales[Units Sold]))),
    "Name", FactSales[Drug Class]
)
```

> The remaining measure formulas (YoY growth, running totals, top/bottom N by growth, peak hour/weekday) follow the same pattern — `CALCULATE` + time-intelligence functions (`DATEADD`/`PREVIOUSMONTH`/`PREVIOUSYEAR`) wrapped in `DIVIDE(...,...,0)` for safe division, and `TOPN`/`RANKX` for leader/laggard detection. Open the `.pbix` in Power BI Desktop and check the Measures Table for exact syntax if you want to quote a specific one in a write-up.
