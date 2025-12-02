# Gemini Prompts for Building Business Finance Planner

Use these prompts with Gemini to help build your UK financial year business planner in Google Sheets.

## Prompt 1: Create the Basic Structure

```
I need to create a business finance planner in Google Sheets for UK financial years (April 1 - March 31). 

Please help me set up the following sheets with the specified column headers:

1. **Dashboard FY2025** - Will show actuals and 5-year projections
   - This will be created later with summary data

2. **Daily Rate Configuration**
   - Column A: Setting Name
   - Column B: Value
   - Column C: Notes
   - Rows: Daily Rate, Contract Start Date, Rate Review Date, Currency

3. **Days Off Calendar**
   - Column A: Date
   - Column B: Year
   - Column C: Month
   - Column D: Day Type (Weekend/Holiday/Vacation/Sick/Other)
   - Column E: Reason/Notes

4. **Monthly Projections** (120 months starting April 2024)
   - Column A: Month/Year (date format)
   - Column B: Working Days in Month (calculated)
   - Column C: Daily Rate
   - Column D: Projected Revenue (calculated)
   - Column E: Actual Working Days (manual entry)
   - Column F: Actual Revenue (manual entry)
   - Column G: Variance (calculated)
   - Column H: Status (Projected/Actual/In Progress)

5. **Expenses Master**
   - Column A: Month/Year (date format)
   - Column B: Category
   - Column C: Budgeted Amount
   - Column D: Actual Amount
   - Column E: Variance
   - Column F: Notes

6. **Loans & Debt**
   - Loan Summary Section (rows 1-5):
     - Loan Name, Principal, Annual Interest Rate, Term (months), Start Date
   - Amortization Table (starting row 7):
     - Column A: Payment Number
     - Column B: Payment Date
     - Column C: Beginning Balance
     - Column D: Monthly Payment
     - Column E: Interest Paid
     - Column F: Principal Paid
     - Column G: Ending Balance

7. **Balance Tracker** (120 months starting April 2024)
   - Column A: Month/Year (date format)
   - Column B: Working Days
   - Column C: Starting Balance
   - Column D: Revenue
   - Column E: Operating Expenses
   - Column F: Loan Payments
   - Column G: Other Income/Expenses
   - Column H: Net Cash Flow
   - Column I: Ending Balance
   - Column J: Data Type (Actual/Projected)

8. **Annual Summary** (by financial year)
   - Column A: Financial Year (e.g., FY2025)
   - Column B: Start Date
   - Column C: End Date
   - Column D: Total Days
   - Column E: Non-working Days
   - Column F: Billable Working Days
   - Column G: Daily Rate
   - Column H: Projected Annual Revenue
   - Column I: Actual Annual Revenue
   - Column J: Variance
   - Column K: Notes

Please create these sheets with the headers in row 1, and format the header row with bold text and a light gray background.
```

## Prompt 2: Set Up Days Off Calendar

```
In the "Days Off Calendar" sheet, I need to track all my non-working days across multiple years.

Please create a structure with these columns:
- Date
- Year
- Month  
- Day Type (Weekend/Holiday/Vacation/Sick)
- Reason/Notes

Also provide a formula to automatically identify weekends, and show me how to calculate total working days in a month by subtracting days off from total days.

My business operates Monday-Friday.
```

## Prompt 3: Build Monthly Projections with Daily Rate

```
In the "Monthly Projections" sheet, I need to calculate revenue based on:
- Working days per month (weekdays minus days off from the Days Off Calendar)
- My daily rate (stored in Daily Rate Configuration sheet, cell B2)

Create a structure with these columns for 120 months starting April 2024:
- Month/Year
- Working Days in Month (calculated)
- Daily Rate
- Projected Revenue (Working Days × Daily Rate)
- Actual Working Days (manual entry)
- Actual Revenue (manual entry)
- Variance
- Status (Projected/Actual)

Provide formulas to:
1. Calculate working days using NETWORKDAYS minus days from Days Off Calendar
2. Calculate projected revenue
3. Calculate variance
```

## Prompt 4: Create Loan Amortization Table

```
In the "Loans & Debt" sheet, I need to create an amortization table for a business loan.

Loan details:
- Principal: [YOUR AMOUNT]
- Annual Interest Rate: [YOUR RATE]%
- Term: [YOUR TERM] months
- Start Date: [YOUR DATE]

Create columns:
- Payment Number
- Payment Date
- Beginning Balance
- Monthly Payment
- Interest Paid
- Principal Paid
- Ending Balance

Provide formulas for calculating:
1. Monthly payment using PMT function
2. Interest portion
3. Principal portion
4. Remaining balance
```

## Prompt 5: Build Balance Tracker

```
In the "Balance Tracker" sheet, create a continuous monthly ledger for 120 months starting April 2024.

Columns needed:
- Month/Year
- Working Days
- Starting Balance
- Revenue (pull from Monthly Projections)
- Operating Expenses (pull from Expenses Master)
- Loan Payments (pull from Loans & Debt)
- Other Income/Expenses
- Net Cash Flow
- Ending Balance
- Data Type (Actual/Projected)

Formula logic:
- Ending Balance = Starting Balance + Revenue - Expenses - Loan Payments
- Next month's Starting Balance = Previous month's Ending Balance

Provide formulas using SUMIFS and VLOOKUP to pull data from other sheets.
```

## Prompt 6: Create FY Dashboard with Filtering

```
In the "Dashboard FY2025" sheet (covering April 2024 - March 2025), create three sections:

1. Current FY Overview showing:
   - YTD actual revenue vs projected
   - YTD expenses
   - Current cash balance
   - Working days completed vs planned
   - Days remaining in FY

2. Monthly Actuals vs Projections Table:
   - Use FILTER or QUERY to show only Apr 2024 - Mar 2025 from Monthly Projections
   - Show: Month, Projected Days, Actual Days, Projected Revenue, Actual Revenue, Variance

3. 5-Year Forward Projection:
   - Annual summary for FY2025-FY2029
   - Show: Financial Year, Date Range, Working Days, Revenue, Expenses, Loan Payments, Ending Balance
   - Use FILTER to pull only relevant periods from master sheets

Provide the FILTER and QUERY formulas for UK financial year boundaries (April-March).
```

## Prompt 7: Add Charts and Visualizations

```
Help me create the following charts for the Dashboard FY2025 sheet:

1. Bar chart: Monthly Actuals vs Projections for current FY (Apr-Mar)
   - X-axis: Months (Apr-Mar)
   - Y-axis: Revenue
   - Two series: Projected and Actual

2. Line chart: 5-Year Balance Projection
   - X-axis: Months over 5 years
   - Y-axis: Ending Balance
   - Pull data from Balance Tracker filtered for FY2025-FY2029

3. Area chart: Loan Payoff Timeline
   - X-axis: Payment dates
   - Y-axis: Remaining balance
   - Pull from Loans & Debt sheet

Provide step-by-step instructions for creating each chart in Google Sheets.
```

## Prompt 8: Create Expense Tracking

```
In the "Expenses Master" sheet, create a structure to track monthly expenses across 120 months.

Categories needed:
- Payroll
- Rent
- Utilities
- Marketing
- Insurance
- Taxes
- Supplies
- Professional Services
- Other

Structure with columns:
- Month/Year
- Category
- Budgeted Amount
- Actual Amount
- Variance
- Notes

Provide formulas to:
1. Sum expenses by category for a given month
2. Calculate YTD totals by category
3. Create a summary that can be filtered by financial year
```

## Prompt 9: Add Conditional Formatting

```
Help me add conditional formatting to highlight important information:

1. In Balance Tracker:
   - Red background if Ending Balance < £10,000
   - Yellow if between £10,000-£25,000
   - Green if > £25,000

2. In Monthly Projections:
   - Highlight variance > 10% in orange
   - Highlight months with < 15 working days in yellow

3. In Dashboard FY2025:
   - Highlight current month in light blue
   - Highlight incomplete actuals (empty cells) in light yellow

Provide the conditional formatting rules for each.
```

## Prompt 10: Create Helper Formulas

```
I need helper formulas for working with UK financial years (April-March):

1. Formula to calculate which FY a date belongs to (e.g., 15-May-2024 should return "FY2025")

2. Formula to get the start date of a financial year (e.g., input "FY2025" returns "01-Apr-2024")

3. Formula to get the end date of a financial year (e.g., input "FY2025" returns "31-Mar-2025")

4. Formula to count working days in a specific financial year, excluding weekends and days from Days Off Calendar

5. Formula to check if a date falls within a specific financial year

Please provide these as custom formulas I can use throughout the spreadsheet.
```

## Prompt 11: Create Data Validation

```
Help me set up data validation for user input cells:

1. In Monthly Projections "Actual Working Days" column:
   - Dropdown with values 0-23 (max working days per month)

2. In Monthly Projections "Status" column:
   - Dropdown: Projected, Actual, In Progress

3. In Days Off Calendar "Day Type" column:
   - Dropdown: Weekend, Holiday, Vacation, Sick, Other

4. In Expenses Master "Category" column:
   - Dropdown with all expense categories

Provide step-by-step instructions for setting up each validation rule.
```

## Prompt 12: Create Named Ranges

```
Help me set up named ranges to make formulas easier to read:

- DailyRate → Daily Rate Configuration!B2
- CurrentFY → A cell showing "FY2025"
- MonthlyProjectionsData → Monthly Projections!A2:H121
- BalanceTrackerData → Balance Tracker!A2:J121
- DaysOffList → Days Off Calendar!A2:E1000
- LoanPayment → Loans & Debt![Monthly Payment Cell]

Show me how to create these named ranges and provide example formulas using them instead of cell references.
```

## Prompt 13: Add Validation and Error Checking

```
Help me add validation formulas to catch errors:

1. Check if Balance Tracker has any negative balances that shouldn't occur
2. Verify that each month's starting balance equals previous month's ending balance
3. Alert if actual revenue variance exceeds 20% of projected
4. Check if sum of monthly revenues equals annual summary
5. Verify loan amortization final balance reaches zero
6. Alert if days off exceed reasonable limits (e.g., > 200 days per year)

Provide formulas using conditional formatting or separate validation columns.
```

## Tips for Using These Prompts

1. **Use them sequentially** - Start with Prompt 1 and work through in order
2. **Customize the placeholders** - Replace [YOUR AMOUNT], [YOUR RATE], etc. with your actual values
3. **Ask follow-up questions** - If Gemini's response isn't clear, ask for clarification or examples
4. **Test incrementally** - Build one sheet at a time and test formulas before moving on
5. **Share your sheet** - You can share a view-only link with Gemini to get specific help
6. **Iterate** - Ask Gemini to modify formulas based on your specific needs

## Example Follow-up Questions

- "Can you show me how to make this formula work with array formulas?"
- "How do I copy this formula down 120 rows automatically?"
- "What's the best way to handle leap years in the working days calculation?"
- "Can you help me add a scenario where the daily rate changes mid-year?"
- "How do I create a dropdown that filters based on another cell's value?"
