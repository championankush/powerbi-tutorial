# Financial Analytics Dashboard 💰

## Project Overview

This comprehensive financial analytics dashboard demonstrates advanced PowerBI techniques for financial reporting, including P&L analysis, cash flow tracking, budget variance, and financial KPIs. The dashboard provides insights for CFOs, financial analysts, and business stakeholders.

## Dashboard Features

### 📊 Key Financial Metrics
- **Revenue Growth**: Month-over-month and year-over-year analysis
- **Profit Margins**: Gross, operating, and net profit margins
- **Cash Flow**: Operating, investing, and financing cash flows
- **Budget Variance**: Actual vs budget performance
- **Financial Ratios**: Liquidity, profitability, and efficiency ratios

### 🎯 Interactive Elements
- **Time Period Slicer**: Dynamic financial period selection
- **Department Filter**: Department-specific financial analysis
- **Account Category Filter**: Revenue, expense, and balance sheet analysis
- **Variance Threshold**: Customizable variance analysis

### 📈 Advanced Visualizations
- **P&L Waterfall Chart**: Revenue to net income breakdown
- **Cash Flow Statement**: Operating, investing, financing flows
- **Budget vs Actual**: Variance analysis with trend lines
- **Financial Ratios Dashboard**: Key performance indicators
- **Department Performance**: Cross-departmental analysis

## Data Model

### Tables Structure

#### Financial Transactions Table
```sql
FinancialTransactions (
    TransactionID (Primary Key),
    Date,
    AccountID (Foreign Key),
    DepartmentID (Foreign Key),
    TransactionType,
    Amount,
    Description,
    Category,
    BudgetAmount,
    FiscalYear,
    FiscalPeriod
)
```

#### Chart of Accounts Table
```sql
ChartOfAccounts (
    AccountID (Primary Key),
    AccountName,
    AccountType,
    AccountCategory,
    ParentAccountID,
    IsActive,
    Description
)
```

#### Departments Table
```sql
Departments (
    DepartmentID (Primary Key),
    DepartmentName,
    DepartmentCode,
    Manager,
    Budget,
    CostCenter
)
```

#### Budget Table
```sql
Budgets (
    BudgetID (Primary Key),
    AccountID (Foreign Key),
    DepartmentID (Foreign Key),
    FiscalYear,
    FiscalPeriod,
    BudgetAmount,
    BudgetType
)
```

## DAX Measures

### Core Financial Measures

#### Revenue Metrics
```dax
// Total Revenue
Total Revenue = CALCULATE(
    SUM(FinancialTransactions[Amount]),
    ChartOfAccounts[AccountType] = "Revenue"
)

// Revenue Growth YoY
Revenue Growth YoY = 
VAR CurrentRevenue = CALCULATE(
    [Total Revenue],
    YEAR(FinancialTransactions[Date]) = YEAR(TODAY())
)
VAR PreviousRevenue = CALCULATE(
    [Total Revenue],
    YEAR(FinancialTransactions[Date]) = YEAR(TODAY()) - 1
)
VAR Growth = CurrentRevenue - PreviousRevenue
VAR GrowthPercent = DIVIDE(Growth, PreviousRevenue, 0)
RETURN
    GrowthPercent
```

#### Expense Metrics
```dax
// Total Expenses
Total Expenses = CALCULATE(
    SUM(FinancialTransactions[Amount]),
    ChartOfAccounts[AccountType] = "Expense"
)

// Operating Expenses
Operating Expenses = CALCULATE(
    SUM(FinancialTransactions[Amount]),
    ChartOfAccounts[AccountType] = "Expense",
    ChartOfAccounts[AccountCategory] IN {"Operating", "Administrative", "Sales"}
)

// Cost of Goods Sold
COGS = CALCULATE(
    SUM(FinancialTransactions[Amount]),
    ChartOfAccounts[AccountType] = "Expense",
    ChartOfAccounts[AccountCategory] = "Cost of Goods Sold"
)
```

#### Profitability Metrics
```dax
// Gross Profit
Gross Profit = [Total Revenue] - [COGS]

// Gross Profit Margin
Gross Profit Margin % = DIVIDE([Gross Profit], [Total Revenue], 0)

// Operating Income
Operating Income = [Gross Profit] - [Operating Expenses]

// Operating Margin
Operating Margin % = DIVIDE([Operating Income], [Total Revenue], 0)

// Net Income
Net Income = [Operating Income] - [Interest Expense] - [Tax Expense]

// Net Profit Margin
Net Profit Margin % = DIVIDE([Net Income], [Total Revenue], 0)
```

### Budget Analysis

#### Budget Variance
```dax
// Budget Variance
Budget Variance = 
VAR ActualAmount = SUM(FinancialTransactions[Amount])
VAR BudgetAmount = SUM(Budgets[BudgetAmount])
VAR Variance = ActualAmount - BudgetAmount
RETURN
    Variance

// Budget Variance %
Budget Variance % = 
VAR ActualAmount = SUM(FinancialTransactions[Amount])
VAR BudgetAmount = SUM(Budgets[BudgetAmount])
VAR VariancePercent = DIVIDE(
    ActualAmount - BudgetAmount,
    BudgetAmount,
    0
)
RETURN
    VariancePercent

// Budget Performance Status
Budget Performance Status = 
VAR VariancePercent = [Budget Variance %]
VAR Status = SWITCH(
    TRUE(),
    VariancePercent <= -0.1, "🔴 Over Budget",
    VariancePercent <= -0.05, "🟡 Near Budget",
    VariancePercent <= 0.05, "🟢 On Budget",
    VariancePercent <= 0.1, "🟡 Under Budget",
    "🔴 Significantly Under Budget"
)
RETURN
    Status
```

### Financial Ratios

#### Liquidity Ratios
```dax
// Current Ratio
Current Ratio = 
VAR CurrentAssets = CALCULATE(
    SUM(FinancialTransactions[Amount]),
    ChartOfAccounts[AccountType] = "Asset",
    ChartOfAccounts[AccountCategory] = "Current Assets"
)
VAR CurrentLiabilities = CALCULATE(
    SUM(FinancialTransactions[Amount]),
    ChartOfAccounts[AccountType] = "Liability",
    ChartOfAccounts[AccountCategory] = "Current Liabilities"
)
VAR Ratio = DIVIDE(CurrentAssets, CurrentLiabilities, 0)
RETURN
    Ratio

// Quick Ratio
Quick Ratio = 
VAR QuickAssets = CALCULATE(
    SUM(FinancialTransactions[Amount]),
    ChartOfAccounts[AccountType] = "Asset",
    ChartOfAccounts[AccountCategory] IN {"Cash", "Accounts Receivable", "Marketable Securities"}
)
VAR CurrentLiabilities = CALCULATE(
    SUM(FinancialTransactions[Amount]),
    ChartOfAccounts[AccountType] = "Liability",
    ChartOfAccounts[AccountCategory] = "Current Liabilities"
)
VAR Ratio = DIVIDE(QuickAssets, CurrentLiabilities, 0)
RETURN
    Ratio
```

#### Efficiency Ratios
```dax
// Asset Turnover Ratio
Asset Turnover Ratio = 
VAR TotalAssets = CALCULATE(
    SUM(FinancialTransactions[Amount]),
    ChartOfAccounts[AccountType] = "Asset"
)
VAR Revenue = [Total Revenue]
VAR Ratio = DIVIDE(Revenue, TotalAssets, 0)
RETURN
    Ratio

// Inventory Turnover
Inventory Turnover = 
VAR COGS = [COGS]
VAR AverageInventory = CALCULATE(
    AVERAGE(FinancialTransactions[Amount]),
    ChartOfAccounts[AccountCategory] = "Inventory"
)
VAR Ratio = DIVIDE(COGS, AverageInventory, 0)
RETURN
    Ratio
```

### Cash Flow Analysis

#### Cash Flow Components
```dax
// Operating Cash Flow
Operating Cash Flow = 
VAR OperatingIncome = [Operating Income]
VAR Depreciation = CALCULATE(
    SUM(FinancialTransactions[Amount]),
    ChartOfAccounts[AccountCategory] = "Depreciation"
)
VAR WorkingCapitalChange = 
    CALCULATE(
        SUM(FinancialTransactions[Amount]),
        ChartOfAccounts[AccountCategory] IN {"Accounts Receivable", "Inventory", "Accounts Payable"}
    )
VAR OperatingCF = OperatingIncome + Depreciation - WorkingCapitalChange
RETURN
    OperatingCF

// Investing Cash Flow
Investing Cash Flow = CALCULATE(
    SUM(FinancialTransactions[Amount]),
    ChartOfAccounts[AccountCategory] IN {"Capital Expenditure", "Investments", "Asset Sales"}
)

// Financing Cash Flow
Financing Cash Flow = CALCULATE(
    SUM(FinancialTransactions[Amount]),
    ChartOfAccounts[AccountCategory] IN {"Debt Issuance", "Debt Repayment", "Dividends", "Equity Issuance"}
)

// Net Cash Flow
Net Cash Flow = [Operating Cash Flow] + [Investing Cash Flow] + [Financing Cash Flow]
```

### Department Performance

#### Department Metrics
```dax
// Department Revenue
Department Revenue = CALCULATE(
    [Total Revenue],
    USERELATIONSHIP(FinancialTransactions[DepartmentID], Departments[DepartmentID])
)

// Department Expenses
Department Expenses = CALCULATE(
    [Total Expenses],
    USERELATIONSHIP(FinancialTransactions[DepartmentID], Departments[DepartmentID])
)

// Department Profit
Department Profit = [Department Revenue] - [Department Expenses]

// Department Profit Margin
Department Profit Margin % = DIVIDE([Department Profit], [Department Revenue], 0)

// Department Budget Variance
Department Budget Variance = 
VAR ActualExpenses = [Department Expenses]
VAR BudgetAmount = SUM(Budgets[BudgetAmount])
VAR Variance = ActualExpenses - BudgetAmount
RETURN
    Variance
```

### Time Intelligence

#### Rolling Averages
```dax
// 3-Month Rolling Revenue
3-Month Rolling Revenue = AVERAGEX(
    DATESINPERIOD(
        FinancialTransactions[Date],
        LASTDATE(FinancialTransactions[Date]),
        -3,
        MONTH
    ),
    [Total Revenue]
)

// 12-Month Rolling Revenue
12-Month Rolling Revenue = AVERAGEX(
    DATESINPERIOD(
        FinancialTransactions[Date],
        LASTDATE(FinancialTransactions[Date]),
        -12,
        MONTH
    ),
    [Total Revenue]
)
```

#### Year-to-Date Analysis
```dax
// Revenue YTD
Revenue YTD = CALCULATE(
    [Total Revenue],
    DATESYTD(FinancialTransactions[Date])
)

// Expenses YTD
Expenses YTD = CALCULATE(
    [Total Expenses],
    DATESYTD(FinancialTransactions[Date])
)

// Profit YTD
Profit YTD = [Revenue YTD] - [Expenses YTD]
```

## Dashboard Setup Instructions

### Step 1: Data Import
1. **Import financial data** from your accounting system
2. **Set up Chart of Accounts** with proper categorization
3. **Import budget data** for variance analysis
4. **Create department hierarchy** for organizational analysis

### Step 2: Data Model Setup
1. **Create relationships** between tables
2. **Set up date table** for time intelligence
3. **Configure account hierarchies** for drill-down analysis
4. **Set up department relationships** for organizational reporting

### Step 3: Create Measures
1. **Build core financial measures** (revenue, expenses, profit)
2. **Create budget variance measures** for analysis
3. **Develop financial ratios** for performance monitoring
4. **Set up cash flow measures** for liquidity analysis

### Step 4: Build Visualizations

#### Page 1: Executive Summary
1. **KPI Cards**: Revenue, Profit, Cash Flow, Budget Variance
2. **P&L Waterfall**: Revenue to net income breakdown
3. **Trend Charts**: Revenue and profit trends
4. **Budget vs Actual**: Variance analysis

#### Page 2: Financial Ratios
1. **Ratio Dashboard**: Key financial ratios
2. **Ratio Trends**: Historical ratio analysis
3. **Benchmark Comparison**: Industry standards
4. **Ratio Alerts**: Threshold monitoring

#### Page 3: Department Analysis
1. **Department Performance**: Revenue and profit by department
2. **Budget Variance**: Department budget analysis
3. **Efficiency Metrics**: Department-specific ratios
4. **Cross-Department Comparison**: Performance ranking

#### Page 4: Cash Flow Analysis
1. **Cash Flow Statement**: Operating, investing, financing
2. **Cash Flow Trends**: Historical cash flow analysis
3. **Working Capital**: Current assets and liabilities
4. **Cash Flow Forecast**: Projected cash flows

### Step 5: Add Interactivity
1. **Time Period Slicers**: Fiscal year and period selection
2. **Department Filters**: Department-specific views
3. **Account Category Filters**: Revenue, expense, asset analysis
4. **Variance Threshold Controls**: Customizable variance analysis

## Sample Data

### Financial Transactions Sample
```csv
TransactionID,Date,AccountID,DepartmentID,TransactionType,Amount,Description,Category,FiscalYear,FiscalPeriod
1,2024-01-15,1001,1,Revenue,50000.00,Sales Revenue,Revenue,2024,1
2,2024-01-16,2001,2,Expense,-15000.00,Office Supplies,Operating,2024,1
3,2024-01-17,2002,1,Expense,-25000.00,Cost of Goods Sold,COGS,2024,1
4,2024-01-18,1002,3,Revenue,30000.00,Service Revenue,Revenue,2024,1
5,2024-01-19,2003,2,Expense,-8000.00,Marketing Expenses,Sales,2024,1
```

### Chart of Accounts Sample
```csv
AccountID,AccountName,AccountType,AccountCategory,ParentAccountID,IsActive
1001,Sales Revenue,Revenue,Revenue,,TRUE
1002,Service Revenue,Revenue,Revenue,,TRUE
2001,Office Supplies,Expense,Operating,,TRUE
2002,Cost of Goods Sold,Expense,COGS,,TRUE
2003,Marketing Expenses,Expense,Sales,,TRUE
3001,Cash,Asset,Current Assets,,TRUE
3002,Accounts Receivable,Asset,Current Assets,,TRUE
4001,Accounts Payable,Liability,Current Liabilities,,TRUE
```

### Budget Sample
```csv
BudgetID,AccountID,DepartmentID,FiscalYear,FiscalPeriod,BudgetAmount,BudgetType
1,2001,2,2024,1,12000.00,Monthly
2,2002,1,2024,1,20000.00,Monthly
3,2003,2,2024,1,10000.00,Monthly
4,1001,1,2024,1,45000.00,Monthly
5,1002,3,2024,1,25000.00,Monthly
```

## Advanced Features

### 1. Financial Forecasting
```dax
// Simple Revenue Forecast
Revenue Forecast = 
VAR HistoricalRevenue = [12-Month Rolling Revenue]
VAR GrowthRate = 0.05 // 5% growth assumption
VAR ForecastedRevenue = HistoricalRevenue * (1 + GrowthRate)
RETURN
    ForecastedRevenue
```

### 2. Variance Analysis
```dax
// Variance Analysis with Thresholds
Variance Analysis = 
VAR VariancePercent = [Budget Variance %]
VAR Analysis = SWITCH(
    TRUE(),
    VariancePercent <= -0.15, "Critical Over Budget",
    VariancePercent <= -0.10, "Significant Over Budget",
    VariancePercent <= -0.05, "Slightly Over Budget",
    VariancePercent <= 0.05, "On Budget",
    VariancePercent <= 0.10, "Under Budget",
    "Significantly Under Budget"
)
RETURN
    Analysis
```

### 3. Financial Health Score
```dax
// Financial Health Score
Financial Health Score = 
VAR ProfitabilityScore = IF([Net Profit Margin %] > 0.15, 25, 
    IF([Net Profit Margin %] > 0.10, 20,
    IF([Net Profit Margin %] > 0.05, 15, 10)))
VAR LiquidityScore = IF([Current Ratio] > 2, 25,
    IF([Current Ratio] > 1.5, 20,
    IF([Current Ratio] > 1, 15, 10)))
VAR EfficiencyScore = IF([Asset Turnover Ratio] > 1, 25,
    IF([Asset Turnover Ratio] > 0.5, 20,
    IF([Asset Turnover Ratio] > 0.25, 15, 10)))
VAR GrowthScore = IF([Revenue Growth YoY] > 0.10, 25,
    IF([Revenue Growth YoY] > 0.05, 20,
    IF([Revenue Growth YoY] > 0, 15, 10)))
VAR TotalScore = ProfitabilityScore + LiquidityScore + EfficiencyScore + GrowthScore
RETURN
    TotalScore
```

## Best Practices

### 1. Data Accuracy
- **Reconcile with accounting system** regularly
- **Validate data integrity** through checksums
- **Document data sources** and refresh schedules
- **Implement data quality checks**

### 2. Performance Optimization
- **Use appropriate data types** for financial data
- **Optimize DAX measures** for large datasets
- **Implement incremental refresh** for transaction data
- **Monitor query performance** regularly

### 3. Security and Compliance
- **Implement row-level security** for department access
- **Audit data access** and changes
- **Comply with financial reporting standards**
- **Maintain data confidentiality**

## Troubleshooting

### Common Issues
1. **Data Reconciliation**: Ensure PowerBI matches accounting system
2. **Performance Issues**: Optimize large transaction datasets
3. **Security Access**: Configure proper row-level security
4. **Budget Variance**: Verify budget data accuracy

### Solutions
1. **Data Validation**: Implement automated reconciliation checks
2. **Performance Tuning**: Use aggregations and efficient DAX
3. **Security Setup**: Configure department-based access controls
4. **Data Quality**: Establish data governance processes

## Next Steps

After completing this dashboard:
1. **Customize**: Adapt to your specific financial structure
2. **Extend**: Add more advanced financial analytics
3. **Integrate**: Connect with real-time data sources
4. **Deploy**: Share with stakeholders and management

## Resources

- [PowerBI Financial Reporting](https://docs.microsoft.com/en-us/power-bi/guidance/power-bi-optimization)
- [Financial Analytics Best Practices](https://www.microsoft.com/en-us/microsoft-365/blog/2016/01/19/data-visualization-best-practices/)
- [DAX Financial Functions](https://docs.microsoft.com/en-us/dax/)

---

**Build this financial dashboard to provide comprehensive financial insights and drive business decisions!** 💰📊✨ 