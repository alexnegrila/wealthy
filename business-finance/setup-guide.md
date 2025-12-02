# Business Finance Planner Setup Guide

Step-by-step instructions for creating a comprehensive business finance planner in Google Spreadsheets with rolling 5-year projections.

## Architecture Overview

**Single Spreadsheet Approach:** Keep all data in one spreadsheet with year-specific dashboard views. This ensures:
- Single source of truth for all calculations
- Continuous balance and loan tracking across years
- Easy formula maintenance and updates
- Year-by-year views with actuals vs. projections
- Rolling 5-year forward projections from any year

**Financial Year:** UK financial year ending March 31st (April 1 - March 31)

## Step 1: Create the Spreadsheet Structure

Create a new Google Spreadsheet with the following sheets:

**Year-Specific Dashboards (Views):**
- Dashboard FY2025 (Apr 2024-Mar 2025 actuals + FY2025-FY2029 projections)
- Dashboard FY2026 (Apr 2025-Mar 2026 actuals + FY2026-FY2030 projections)
- Dashboard FY2027 (Apr 2026-Mar 2027 actuals + FY2027-FY2031 projections)
- Dashboard FY2028 (Apr 2027-Mar 2028 actuals + FY2028-FY2032 projections)
- Dashboard FY2029 (Apr 2028-Mar 2029 actuals + FY2029-FY2033 projections)

**Master Data Sheets (Shared by all dashboards):**
- Daily Rate Configuration
- Days Off Calendar (all years)
- Monthly Projections (rolling 60+ months)
- Expenses Master
- Loans & Debt (full amortization)
- Balance Tracker (continuous from start)
- Cash Flow Master
- Annual Summary (by financial year)

## Step 2: Configure Daily Rate

In the **Daily Rate Configuration** sheet:
- Set current daily rate
- Add field for rate change date (if applicable)
- Calculate potential rate increases over 5 years
- Document contract terms

## Step 3: Set Up Days Off Calendar

In the **Days Off Calendar** sheet:
- List all days off for the current year (actual)
- Estimate days off for next 5 years by category:
  - Weekends (automatically calculated)
  - Public holidays
  - Vacation days
  - Sick days (estimate)
  - Other time off

Create columns:
- Date
- Year
- Month
- Day Type (Weekend/Holiday/Vacation/Sick)
- Reason/Notes

Calculate working days:
- Total days in year: 365 (or 366 for leap years)
- Minus weekends: ~104 days
- Minus public holidays: varies by country (~10-15)
- Minus planned vacation: your estimate
- Minus estimated sick days: ~5-10
- = Billable working days per year

## Step 4: Set Up Annual Revenue Plan

In the **Annual Summary** sheet:
- Create rows for at least 10 financial years (to support rolling 5-year views)
- For each financial year (April 1 - March 31):
  - Financial Year (e.g., FY2025 = Apr 2024 to Mar 2025)
  - Start Date (April 1)
  - End Date (March 31)
  - Total days in financial year (365 or 366)
  - Non-working days (from Days Off Calendar)
  - Billable working days
  - Daily rate
  - **Formula: Annual Revenue = Billable Working Days × Daily Rate**
  - Actual annual revenue (filled in as year progresses)
  - Variance
- Add buffer percentage for rate changes
- Include notes on assumptions

This sheet feeds all year-specific dashboards with their 5-year projections.

**Note:** Ensure all calculations respect the April-March financial year boundary.

## Step 5: Create Monthly Revenue Projections

In the **Monthly Projections** sheet:
- Create rows for at least 120 months (10 years) to support rolling views
- Add columns:
  - Month/Year
  - Working Days in Month (from Days Off Calendar)
  - Daily Rate
  - Projected Revenue (Working Days × Daily Rate)
  - Actual Working Days (manually entered or imported)
  - Actual Revenue (manually entered or imported)
  - Variance (Actual - Projected)
  - Status (Projected/Actual/In Progress)

Calculation approach:
- Count total days per month
- Subtract weekends in that month
- Subtract holidays/vacation days from Days Off Calendar
- Multiply remaining working days by daily rate
- **Formula: Monthly Revenue = (Days in Month - Days Off) × Daily Rate**

**Key:** Each year's dashboard will use `FILTER()` or `QUERY()` to show only its relevant 60-month window.

## Step 6: Set Up Loan Management

In the **Loans & Debt** sheet:
- List all business loans with: Principal, Interest Rate, Term, Start Date, Monthly Payment
- Create amortization tables for each loan
- Calculate monthly interest and principal portions
- Track remaining balance for each payment

Create columns:
- Payment Number
- Payment Date
- Beginning Balance
- Monthly Payment
- Interest Paid
- Principal Paid
- Ending Balance

## Step 7: Build Monthly Balance Tracker

In the **Balance Tracker** sheet:
- Create rows for at least 120 months (10 years) for continuous tracking
- Add columns:
  - Month/Year
  - Working Days
  - Starting Balance
  - Revenue (from Monthly Projections: Working Days × Daily Rate)
  - Operating Expenses
  - Loan Payments (total from Loans sheet)
  - Other Income/Expenses
  - Net Cash Flow
  - Ending Balance
  - Data Type (Actual/Projected)
- Formula: `Ending Balance = Starting Balance + Revenue - Expenses - Loan Payments`
- Next month's Starting Balance = Previous month's Ending Balance

**Critical:** This is a continuous ledger. Each year's dashboard filters this master tracker.

## Step 8: Set Up Expense Tracking

In the **Expenses Master** sheet:
- Create monthly expense records for the full period (120+ months)
- Categories: Payroll, Rent, Utilities, Marketing, Insurance, Taxes, Supplies, etc.
- Columns: Month/Year, Category, Budgeted Amount, Actual Amount, Variance
- Each year's dashboard filters and summarizes this data

In the **Expenses** sheet:
- Categories: Payroll, Rent, Utilities, Marketing, Insurance, Taxes, Supplies, etc.
- Monthly budgeted amounts
- Monthly actual amounts
- Year-to-date totals

## Step 9: Create Cash Flow Projection

In the **Cash Flow Master** sheet:
- Operating Activities: Daily rate revenue and expenses
- Financing Activities: Loan proceeds and payments
- Investing Activities: Capital expenditures
- Calculate net cash flow for each month
- Create rows for the full period (120+ months)
- Each year's dashboard filters relevant periods

## Step 10: Build Year-Specific Dashboards

For **each Dashboard sheet (FY2025, FY2026, FY2027, etc.)**:

### Current Financial Year Overview Section:
- Financial year header (e.g., "FY2025: April 2024 - March 2025")
- Current date and months remaining in FY
- YTD actual revenue vs. projected
- YTD actual expenses vs. budget
- Current cash balance
- Working days completed vs. planned
- Key metrics: Average daily rate, days worked, days remaining in FY

### Actuals Entry Section:
- Monthly table for the current financial year (12 rows: Apr-Mar) with:
  - Month (Apr, May, Jun... Mar)
  - Projected working days
  - **Actual working days (editable)**
  - Projected revenue
  - **Actual revenue (editable)**
  - Variance
- Use data validation for easy entry
- Updates flow to Master sheets

### 5-Year Forward Projection Section:
- Starting from current financial year, show next 5 financial years
- Annual summary table with:
  - Financial Year (FY2025, FY2026, etc.)
  - Date Range (Apr YYYY - Mar YYYY)
  - Projected working days
  - Projected revenue
  - Projected expenses
  - Projected loan payments
  - Projected ending balance
- Use `FILTER()` or `QUERY()` to pull from master sheets

### Charts and Visualizations:
- Current FY: Monthly actuals vs. projections (bar chart, Apr-Mar)
- 5-year balance projection (line chart)
- 5-year loan payoff timeline (area chart)
- Working days trend by financial year (line chart)
- Revenue breakdown by source if applicable

### Formula Examples:
```
// Filter Monthly Projections for FY2025 (Apr 2024 - Mar 2025)
=FILTER('Monthly Projections'!A:H, 
  'Monthly Projections'!A:A >= DATE(2024,4,1), 
  'Monthly Projections'!A:A <= DATE(2025,3,31))

// Get FY2025 5-year window (FY2025-FY2029)
=QUERY('Monthly Projections'!A:H, 
  "SELECT * WHERE A >= date '2024-04-01' AND A < date '2029-04-01'")

// Sum projected balance for FY2026 (Apr 2025 - Mar 2026)
=SUMIFS('Balance Tracker'!I:I, 
  'Balance Tracker'!A:A, ">=2025-04-01", 
  'Balance Tracker'!A:A, "<=2026-03-31")

// Calculate which FY a date belongs to
=IF(MONTH(A2)>=4, YEAR(A2), YEAR(A2)-1)
```

In the **Dashboard** sheet:
- Current month balance
- 12-month balance trend (chart)
- 5-year balance projection (chart)
- Key metrics: Runway, burn rate, total debt
- Revenue vs. expenses comparison
- Loan payoff timeline visualization

## Step 11: Add Formulas and Automation

Key formulas to implement:
- `FILTER()` to show year-specific views from master data
- `QUERY()` for complex filtering and aggregation across years
- `NETWORKDAYS` or `COUNTIF` to calculate working days minus days off
- `SUMIFS()` for aggregating expenses by category and date range
- `VLOOKUP()` or `INDEX/MATCH` to pull loan payments and daily rates
- `IF()` statements for conditional calculations
- Array formulas for multi-year projections
- `EOMONTH()` for date calculations
- Charts that auto-update based on filtered data

Example working days formula:
```
=NETWORKDAYS(start_date, end_date) - COUNTIF(DaysOffCalendar!A:A, ">="&start_date, DaysOffCalendar!A:A, "<="&end_date)
```

Example to pull 5-year balance projection for FY2025 dashboard:
```
=QUERY('Balance Tracker'!A:J, 
  "SELECT A, I WHERE A >= date '2024-04-01' AND A < date '2029-04-01' ORDER BY A")
```

Helper formula to determine financial year from any date:
```
// Returns FY year (e.g., 2025 for dates Apr 2024 - Mar 2025)
=IF(MONTH(date_cell)>=4, YEAR(date_cell)+1, YEAR(date_cell))
```

## Step 12: Validate and Test

- Verify all loan calculations match amortization schedules
- Ensure balance tracker flows correctly month-to-month across financial years
- Check that totals sum correctly in all master sheets
- Test that each FY dashboard shows correct 5-year window (April-March boundaries)
- Verify actuals entered in one dashboard update master sheets
- Validate formulas across all 120+ months
- Test financial year transitions: Verify March→April calculations are correct
- Test rolling forward: As you move to FY2026, confirm FY2026 dashboard shows FY2026-FY2030
- Ensure charts auto-update when switching between FY dashboards
- Verify annual summaries correctly aggregate April-March periods

## Workflow: Using the Planner

### At the Start of Each Financial Year (April):
1. Create a new dashboard sheet for the financial year (e.g., Dashboard FY2026)
2. Set up formulas to filter 5-year window starting from April of that FY
3. Extend master data sheets if needed (add more future months)
4. Update days off calendar with new estimates for the new FY
5. Review and adjust daily rate if changed

### Monthly:
1. Open current financial year's dashboard
2. Enter actual working days for completed month
3. Enter actual revenue and expenses
4. Review variance between actual and projected
5. Check updated ending balance and 5-year projection
6. Adjust future projections if needed based on trends

### End of Financial Year (March):
1. Finalize all actuals for the completed FY
2. Compare annual actual vs. projected for the full FY
3. Review financial year performance
4. Use insights to adjust projections for future FYs
5. Prepare for new FY starting April 1

### Benefits of This Structure:
- **Single source of truth:** All calculations reference same master data
- **Historical accuracy:** Past financial years retain their actual data
- **Rolling projections:** Always see 5 financial years forward from any FY
- **Easy updates:** Change daily rate or add loan in one place, all dashboards update
- **Comparative analysis:** Easily compare projected vs. actual across financial years
- **Continuous tracking:** Balance and loans flow seamlessly across FY boundaries
- **UK compliance:** Aligned with UK financial year (April-March)

## Example Monthly Balance Row

| Month | Working Days | Daily Rate | Revenue | Expenses | Loan Payment | Ending Balance |
|-------|--------------|------------|---------|----------|--------------|----------------|
| Apr 2024 | 20 | £500 | £10,000 | £5,000 | £2,500 | £52,500 |
| May 2024 | 18 | £500 | £9,000 | £5,000 | £2,500 | £54,000 |
| ... | ... | ... | ... | ... | ... | ... |
| Mar 2025 | 21 | £500 | £10,500 | £5,000 | £2,500 | £67,000 |

**Note:** This represents one complete financial year (FY2025)

## Tips for Gemini Assistance

When working with Gemini to build this:
1. Ask for `FILTER()` and `QUERY()` formulas to create year-specific views from master data
2. Request `NETWORKDAYS` formulas to calculate working days automatically
3. Get help creating a days-off tracker that integrates with monthly projections
4. Ask for specific formulas for loan amortization calculations
5. Request help creating conditional formatting to highlight:
   - Low balances or negative cash flow
   - Months with many days off
   - Variance thresholds (actual vs. projected)
   - Current month in dashboards
6. Get assistance with chart creation for visualizations:
   - Working days trend across years
   - Revenue projection with actual overlay
   - Rolling 5-year balance forecast
   - Loan payoff timelines
7. Ask for array formulas to project 120+ months at once
8. Request validation formulas to catch errors (negative balances, formula breaks)
9. Get help with scenario planning:
   - Rate increase scenarios
   - Additional time off impact
   - Loan early payoff scenarios
   - Best/worst case revenue projections
10. Ask for a formula to highlight months where you're working below average days
11. Request help setting up data validation for actuals entry sections
12. Get assistance creating named ranges for cleaner formulas
