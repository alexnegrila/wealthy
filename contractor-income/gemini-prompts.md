# Gemini Prompts for Contractor Income Tracker

Use these prompts with Gemini to build your UK contractor income tracker in Google Sheets. This system calculates monthly income based on day rate, working days, bank holidays, and leave.

## Prompt 1: Create Basic Spreadsheet Structure

```
I need to create a contractor income tracker in Google Sheets for UK working days.

Please help me set up the following sheets with the specified columns:

1. **Dashboard** - Summary view (we'll build this later)

2. **Day Rate Config**
   - Column A: Setting Name (Day Rate, Currency, Contract Start, Standard Working Days, Typical Annual Leave, Rate Review Date, Future Day Rate)
   - Column B: Value
   - Column C: Notes

3. **UK Bank Holidays**
   - Column A: Date (DD/MM/YYYY format)
   - Column B: Holiday Name
   - Column C: Region (e.g., England & Wales, Scotland, Northern Ireland, All UK)
   - Column D: Year (calculated from date)

4. **Leave Days**
   - Column A: Leave Start Date
   - Column B: Number of Days
   - Column C: Leave Type (Annual Leave, Sick Leave, Other)
   - Column D: End Date (calculated)
   - Column E: Affected Months (calculated)

5. **Monthly Income** (60 rows for 5 years, starting April 2024)
   - Column A: Month (MMM)
   - Column B: Year (YYYY)
   - Column C: Total Days
   - Column D: Weekdays (Mon-Fri)
   - Column E: Bank Holidays (from UK Bank Holidays sheet)
   - Column F: Leave Days (from Leave Days sheet)
   - Column G: Working Days (calculated: Weekdays - Bank Holidays - Leave)
   - Column H: Day Rate (from Day Rate Config)
   - Column I: Monthly Income (Working Days × Day Rate)
   - Column J: Actual Days (manual entry)
   - Column K: Actual Income (calculated from actual days)
   - Column L: Status (Projected/In Progress/Actual)

6. **Balance Sheet** (60 rows for 5 years)
   - Column A: Month
   - Column B: Income (from Monthly Income)
   - Column C: Expenses (manual entry)
   - Column D: Net Cash Flow (Income - Expenses)
   - Column E: Ending Balance (running total)
   - Column F: Notes

Please create these sheets with headers in row 1, formatted with bold text and light gray background.
```

## Prompt 2: Configure Day Rate Settings

```
In the "Day Rate Config" sheet, help me set up these settings:

Row 1: Day Rate = £500
Row 2: Currency = GBP
Row 3: Contract Start = 01/04/2024
Row 4: Standard Working Days = Monday to Friday
Row 5: Typical Annual Leave = 25 days
Row 6: Rate Review Date = 01/04/2025
Row 7: Future Day Rate = £550 (optional - for planned increases)

Please format:
- Column B, row 1 as currency (£)
- Column B, row 3 and row 6 as dates (DD/MM/YYYY)
- Column A in bold

Also add a notes section explaining:
- Day rate is the amount charged per working day
- Standard working days exclude weekends
- Annual leave is your planned time off per year
```

## Prompt 3: Add UK Bank Holidays

```
In the "UK Bank Holidays" sheet, please add UK bank holidays for England & Wales for 2024-2029.

For 2024:
- 01/01/2024 - New Year's Day
- 29/03/2024 - Good Friday
- 01/04/2024 - Easter Monday
- 06/05/2024 - Early May Bank Holiday
- 27/05/2024 - Spring Bank Holiday
- 26/08/2024 - Summer Bank Holiday
- 25/12/2024 - Christmas Day
- 26/12/2024 - Boxing Day

Please continue with bank holidays for 2025-2029. Use these dates:

For 2025:
- New Year's Day: 01/01/2025
- Good Friday: 18/04/2025
- Easter Monday: 21/04/2025
- Early May Bank Holiday: 05/05/2025
- Spring Bank Holiday: 26/05/2025
- Summer Bank Holiday: 25/08/2025
- Christmas Day: 25/12/2025
- Boxing Day: 26/12/2025

(Continue pattern for 2026-2029)

In Column D, add a formula to extract the year from Column A:
=YEAR(A2)

Sort the entire sheet by date (oldest first).
```

## Prompt 4: Set Up Leave Days Sheet with Formulas

```
In the "Leave Days" sheet, I need formulas to calculate end dates and affected months.

Column D (End Date) formula:
Calculate the end date based on leave start date (Column A) and number of working days (Column B).
Use WORKDAY function to skip weekends.

Column E (Affected Months) formula:
Show which months are affected by this leave period.
If leave spans multiple months, show both months.
Format as "MMM YYYY" (e.g., "Aug 2024, Sep 2024").

Please provide the exact formulas for both columns.

Also add data validation for Column C (Leave Type):
- Annual Leave
- Sick Leave  
- Unpaid Leave
- Other

Add some example data:
Row 2: Start 05/08/2024, 5 days, Annual Leave
Row 3: Start 23/12/2024, 7 days, Annual Leave
Row 4: Start 14/04/2025, 3 days, Annual Leave
```

## Prompt 5: Build Monthly Income Projections with Formulas

```
In the "Monthly Income" sheet, create 60 rows covering April 2024 to March 2029.

I need formulas for these columns:

**Column A (Month):** Generate month names (Apr, May, Jun, etc.) starting from April 2024 for 60 rows

**Column B (Year):** Extract year corresponding to each month

**Column C (Total Days):** Calculate total days in each month (28-31)

**Column D (Weekdays):** Count weekdays (Monday-Friday) in each month using NETWORKDAYS.INTL

**Column E (Bank Holidays):** Count bank holidays from "UK Bank Holidays" sheet that fall in this month using COUNTIFS

**Column F (Leave Days):** Count leave days from "Leave Days" sheet that fall in this month. This needs to handle:
- Leave that starts and ends in the same month
- Leave that spans across multiple months
Use SUMPRODUCT to count days

**Column G (Working Days):** Calculate: Weekdays - Bank Holidays - Leave Days
Ensure result is never negative (use MAX function)

**Column H (Day Rate):** Reference the day rate from "Day Rate Config" sheet, cell B2

**Column I (Monthly Income):** Calculate: Working Days × Day Rate

**Column J (Actual Days):** Leave empty for manual entry

**Column K (Actual Income):** If actual days entered (Column J), calculate: Actual Days × Day Rate
Otherwise leave blank

**Column L (Status):** 
- If Column K has value: "Actual"
- If month is past but no actual: "In Progress"
- If month is future: "Projected"

Please provide all formulas, making sure they:
1. Auto-fill down correctly for 60 rows
2. Use absolute references where needed ($ signs)
3. Handle edge cases (leap years, leave spanning months)
4. Reference the correct sheets

Format Columns H, I, K as currency (£).
```

## Prompt 6: Create Balance Sheet with Running Total

```
In the "Balance Sheet" sheet, create a structure that:

1. Has a starting balance row (Row 1):
   - Column A: "Starting Balance"
   - Column E: £10,000 (or my actual starting balance)

2. From Row 2 onwards, for 60 months:
   - Column A: Pull month from "Monthly Income" sheet (format: "MMM YYYY")
   - Column B: Pull monthly income from "Monthly Income" sheet, Column I
   - Column C: Expenses (manual entry - leave blank for now)
   - Column D: Calculate Net Cash Flow = Income - Expenses
   - Column E: Calculate Ending Balance = Previous Ending Balance + Net Cash Flow

The key is that ending balance is cumulative:
- Row 2 ending balance = Starting balance + Row 2 net cash flow
- Row 3 ending balance = Row 2 ending balance + Row 3 net cash flow
- And so on...

Please provide formulas for each column, ensuring:
- The running balance calculation is correct
- Formulas reference the right sheets
- Currency columns are formatted as £

Format:
- Columns B, C, D, E as currency (£)
- Ending Balance column with conditional formatting:
  - Red if negative
  - Orange if < £10,000
  - Green if > £100,000
```

## Prompt 7: Create Dashboard with Summary Metrics

```
In the "Dashboard" sheet, create a summary view with these sections:

**Section 1: Key Metrics** (Top of page)
Create a 2-column table:

| Metric | Value (with formula) |
|--------|---------------------|
| Current Day Rate | Pull from Day Rate Config |
| Average Working Days/Month | Average of Working Days from Monthly Income |
| Average Monthly Income | Average of Monthly Income column |
| Projected Annual Income (Current Year) | Sum of income for current year only |
| Total Leave Days (Current Year) | Sum of leave days for current year |
| Current Balance | Latest ending balance from Balance Sheet |
| Projected Balance (End of Year) | Balance at end of current year |

Please provide formulas for the "Value" column for each metric.

**Section 2: 5-Year Income Summary**
Create a table summarizing each year:

| Year | Total Working Days | Projected Income | Projected Ending Balance |
|------|-------------------|------------------|-------------------------|
| 2024 | | | |
| 2025 | | | |
| 2026 | | | |
| 2027 | | | |
| 2028 | | | |

Use SUMIFS to sum data from Monthly Income sheet by year.

**Section 3: Current Year Monthly Breakdown**
Filter and display only current year months from Monthly Income sheet.
Show columns: Month, Working Days, Income, Status

Use FILTER or QUERY function to show only rows where year = current year.

Please provide all formulas needed for these sections.
```

## Prompt 8: Add Charts and Visualizations

```
Help me create these charts in the "Dashboard" sheet:

**Chart 1: Monthly Income (Next 12 Months)**
- Chart type: Column chart
- X-axis: Month name
- Y-axis: Monthly Income (£)
- Data range: Next 12 months from Monthly Income sheet
- Title: "Projected Monthly Income (Next 12 Months)"
- Show data labels

**Chart 2: 5-Year Balance Projection**
- Chart type: Line chart
- X-axis: Month/Year
- Y-axis: Ending Balance (£)
- Data range: All 60 months from Balance Sheet
- Title: "5-Year Balance Projection"
- Smooth line, with markers

**Chart 3: Working Days by Month (Current Year)**
- Chart type: Bar chart
- X-axis: Working Days
- Y-axis: Month
- Data range: Current year from Monthly Income
- Title: "Working Days Distribution (Current Year)"
- Color-code: < 18 days (red), 18-20 days (yellow), > 20 days (green)

**Chart 4: Annual Income Comparison**
- Chart type: Column chart
- X-axis: Year (2024-2028)
- Y-axis: Total Income (£)
- Data range: 5-Year Income Summary table
- Title: "Projected Annual Income (5 Years)"

Please provide step-by-step instructions to create each chart, including:
- Where to click in Google Sheets
- How to select data ranges
- Chart customization settings
```

## Prompt 9: Add Conditional Formatting

```
Help me add conditional formatting to highlight important information:

**In "Monthly Income" sheet:**

1. Column G (Working Days):
   - Red background if < 15 days
   - Yellow background if 15-17 days
   - Green background if > 20 days

2. Entire row:
   - Light blue background if this is the current month
   - Gray out if month is more than 3 months in the past and Status = "Actual"

3. Column J (Actual Days):
   - Orange border if column is empty and month is in the past

4. Column L (Status):
   - Green text for "Actual"
   - Orange text for "In Progress"
   - Gray text for "Projected"

**In "Balance Sheet":**

1. Column E (Ending Balance):
   - Red background and bold if negative
   - Orange background if < £10,000
   - Green background if > £100,000

2. Column D (Net Cash Flow):
   - Red text if negative
   - Green text if positive

**In "Leave Days" sheet:**

1. Entire row:
   - Yellow background if leave is in current month
   - Light gray if leave is in the past

Please provide the conditional formatting rules with exact conditions for each.
```

## Prompt 10: Handle Day Rate Changes Over Time

```
I want to model a day rate increase from £500 to £550 on 01/04/2025.

In the "Day Rate Config" sheet, I have:
- Current Day Rate (B2): £500
- Rate Review Date (B7): 01/04/2025
- Future Day Rate (B8): £550

In the "Monthly Income" sheet, Column H (Day Rate), I need a formula that:
- Uses current rate (£500) for months before 01/04/2025
- Uses future rate (£550) for months on or after 01/04/2025

Please provide a formula using IF statement that:
1. Compares the month date (Columns A and B) to the rate review date
2. Returns the appropriate rate
3. Can be copied down for all 60 rows

Also show me how to model multiple rate changes (e.g., another increase in 2026).
Should I create a rate schedule table?
```

## Prompt 11: Add Data Validation

```
Help me set up data validation for user input fields:

1. **Monthly Income, Column J (Actual Days):**
   - Dropdown or number validation
   - Range: 0 to 23 (maximum working days per month)
   - Show validation error if outside range
   - Custom error message: "Working days must be between 0 and 23"

2. **Leave Days, Column C (Leave Type):**
   - Dropdown with options:
     * Annual Leave
     * Sick Leave
     * Unpaid Leave
     * Other
   - Reject invalid input
   - Show dropdown arrow

3. **Leave Days, Column B (Number of Days):**
   - Number validation
   - Range: 1 to 20 (max leave duration)
   - Warning message: "Leave periods longer than 20 days should be split"

4. **Balance Sheet, Column C (Expenses):**
   - Number validation
   - Must be positive (≥ 0)
   - Custom error: "Expenses must be a positive number"

5. **Day Rate Config, Currency (B3):**
   - Dropdown: GBP, EUR, USD
   - Default: GBP

Please provide step-by-step instructions for setting up each validation rule.
```

## Prompt 12: Create Leave Planning Helper

```
I want to add a leave planning section to the "Dashboard" sheet.

Create a table showing:

**Annual Leave Tracker (Current Year)**

| Leave Type | Days Used | Days Planned (Future) | Days Remaining | Total Allowance |
|------------|-----------|----------------------|----------------|----------------|
| Annual Leave | [formula] | [formula] | [formula] | 25 |
| Sick Days (Estimate) | [formula] | [formula] | [formula] | 5 |
| **Total** | [formula] | [formula] | [formula] | 30 |

Formulas needed:
- **Days Used:** Count leave days from "Leave Days" sheet where leave date < today and leave type matches
- **Days Planned:** Count leave days where leave date >= today and leave type matches
- **Days Remaining:** Total Allowance - Days Used - Days Planned
- **Total Allowance:** Pull from Day Rate Config sheet (row 5)

Also add:
- Conditional formatting: Highlight "Days Remaining" in red if negative (over-allocated)
- Progress bar showing leave used as percentage of allowance

Below the table, add:
**Next Leave Period:** Show the next upcoming leave from Leave Days sheet (date and duration)

Please provide all formulas.
```

## Prompt 13: Add Working Days Analysis

```
In the "Dashboard" sheet, create a "Working Days Analysis" section showing:

**Working Days Statistics (Current Year)**

| Statistic | Value |
|-----------|-------|
| Total Available Working Days | [formula: count weekdays in year] |
| Bank Holidays | [formula: count from UK Bank Holidays] |
| Planned Leave Days | [formula: count from Leave Days] |
| **Billable Working Days** | [formula: Total - Bank Holidays - Leave] |
| Days Worked So Far (Actual) | [formula: sum actual days from Monthly Income] |
| Days Remaining This Year | [formula: Billable - Days Worked] |
| Average Working Days/Month (Actual) | [formula: average of completed months] |

**Monthly Working Days Range:**
- Minimum: [formula: MIN of working days column]
- Maximum: [formula: MAX of working days column]
- Average: [formula: AVERAGE of working days column]

Also create a visualization:
**Working Days Heatmap** - Show each month colored by number of working days:
- Red: < 16 days
- Yellow: 16-19 days
- Green: 20+ days

Please provide formulas and instructions for the heatmap.
```

## Prompt 14: Add Scenario Planning

```
I want to add scenario planning to model different situations.

In a new sheet called "Scenarios", create comparison columns:

| Month | Base Case (£500/day) | Optimistic (£550/day) | Conservative (£475/day) | Part-Time (3 days/week) |
|-------|---------------------|-----------------------|------------------------|------------------------|

For each scenario:
- **Base Case:** Current projections from Monthly Income
- **Optimistic:** Same working days but day rate = £550
- **Conservative:** Same working days but day rate = £475
- **Part-Time:** Reduced working days (60% of normal) at £500/day

Calculate for each scenario:
- Monthly income
- Annual income
- 5-year total income
- Ending balance after 5 years

Add a comparison section:
| Scenario | 5-Year Total | Ending Balance | vs. Base Case |
|----------|--------------|----------------|---------------|

Show difference from base case as percentage and absolute value.

Please provide formulas for each scenario, referencing the Monthly Income sheet and applying different assumptions.
```

## Prompt 15: Create Tax Estimation

```
Add a tax estimation feature to help plan for UK taxes.

In the "Balance Sheet" sheet, add these columns after Expenses:

- Column D: Gross Income (from Monthly Income)
- Column E: Estimated Tax (calculated)
- Column F: Estimated NI (National Insurance - calculated)
- Column G: Net Income (Gross - Tax - NI)
- Column H: Expenses
- Column I: Net Cash Flow (Net Income - Expenses)
- Column J: Ending Balance

UK Tax Calculation (2024/25):
- Personal Allowance: £12,570 (no tax)
- Basic Rate (20%): £12,571 to £50,270
- Higher Rate (40%): £50,271 to £125,140
- Additional Rate (45%): Over £125,140

Assume contractor operates through limited company taking salary + dividends:
- Salary: £12,570/year (below personal allowance)
- Dividends: Remainder of income
- Dividend tax: 8.75% (basic), 33.75% (higher), 39.35% (additional)
- Dividend allowance: £500 tax-free

NI Calculation (Rough estimate):
- 13.8% employer NI on salary above threshold
- No NI on dividends

Create a formula to estimate monthly tax based on projected annual income.

Also add:
**Annual Tax Summary** showing:
- Estimated annual income
- Tax band
- Estimated total tax
- Effective tax rate

Please provide formulas for tax estimation.
```

## Prompt 16: Add Alerts and Notifications

```
I want to add a "Alerts & Notifications" section to the Dashboard.

Create a table that automatically shows warnings:

| Alert Type | Status | Message | Action Required |
|------------|--------|---------|----------------|
| Low Working Days | [formula] | "August has only 14 working days" | Review leave plan |
| Overallocated Leave | [formula] | "You have planned 27 days leave (2 days over)" | Reduce leave |
| Balance Warning | [formula] | "Balance will drop below £10,000 in July" | Plan expenses |
| Rate Review Due | [formula] | "Day rate review due in 3 months" | Prepare negotiation |
| Missing Actuals | [formula] | "3 past months have no actual data" | Enter actual days |

Formulas needed for Status column:
- "✓" (checkmark) if OK
- "⚠" (warning) if issue detected
- Leave blank if not applicable

Logic for each alert:
1. **Low Working Days:** Any month with < 16 working days
2. **Overallocated Leave:** Total leave days > allowance
3. **Balance Warning:** Any future month where ending balance < £10,000
4. **Rate Review Due:** Rate review date within 3 months
5. **Missing Actuals:** Past months (< today) with empty Actual Days column

Also add conditional formatting:
- Highlight row in yellow if Status = "⚠"
- Bold the Message column

Please provide formulas for detecting each condition.
```

## Prompt 17: Create Named Ranges

```
Help me create named ranges to make formulas easier to read and maintain.

Set up these named ranges:

**From "Day Rate Config" sheet:**
- DayRate → B2 (current day rate)
- Currency → B3
- ContractStart → B4
- AnnualLeaveAllowance → B5
- RateReviewDate → B7
- FutureDayRate → B8

**From "Monthly Income" sheet:**
- MonthlyIncomeData → A2:L61 (all 60 months)
- WorkingDaysColumn → G2:G61
- MonthlyIncomeColumn → I2:I61
- ActualDaysColumn → J2:J61
- StatusColumn → L2:L61

**From "Leave Days" sheet:**
- LeaveDates → A2:A100
- LeaveDaysCount → B2:B100
- LeaveTypes → C2:C100

**From "UK Bank Holidays" sheet:**
- BankHolidayDates → A2:A500

**From "Balance Sheet" sheet:**
- EndingBalances → E2:E61
- NetCashFlow → D2:D61

Show me:
1. How to create each named range in Google Sheets
2. Example formulas using named ranges instead of cell references
3. Benefits of using named ranges

For example, change this:
`='Day Rate Config'!B2`

To this:
`=DayRate`
```

## Prompt 18: Create Validation Summary

```
Create a "Validation" sheet that automatically checks for errors and inconsistencies.

Add these validation checks:

| Check | Status | Details | Sheet | Issue |
|-------|--------|---------|-------|-------|
| Day rate configured | [✓/✗] | | Day Rate Config | |
| Bank holidays added | [✓/✗] | "Found 40 holidays" | UK Bank Holidays | |
| Future months valid | [✓/✗] | | Monthly Income | |
| Balance calculation | [✓/✗] | | Balance Sheet | |
| Working days range | [✓/✗] | "All months 15-23 days" | Monthly Income | |
| Leave within allowance | [✓/✗] | | Leave Days | |
| No negative balances | [✓/✗] | | Balance Sheet | |
| Formulas not broken | [✓/✗] | | All Sheets | |

Each validation should:
1. Check a specific condition
2. Show ✓ if passed, ✗ if failed
3. Provide details/count
4. Identify which sheet has the issue
5. Describe the issue if failed

Formulas needed:
1. **Day rate configured:** Check if DayRate > 0
2. **Bank holidays added:** Count rows in UK Bank Holidays sheet
3. **Future months valid:** Check all 60 months are populated
4. **Balance calculation:** Verify each ending balance = previous ending + cash flow
5. **Working days range:** Check all working days between 0-23
6. **Leave within allowance:** Check total leave ≤ allowance
7. **No negative balances:** Check no balance < 0
8. **Formulas not broken:** Check for #REF!, #VALUE!, #ERROR!

Add conditional formatting: Highlight failed checks (✗) in red.

Please provide formulas for each validation check.
```

## Tips for Using These Prompts

1. **Use sequentially:** Start with Prompt 1 and work through in order for best results.

2. **Customize values:** Replace example values (£500 day rate, dates, etc.) with your actual information.

3. **Test as you go:** After each prompt, verify the results before moving to the next.

4. **Ask follow-ups:** If a formula doesn't work, share the error message and ask for corrections.

5. **Share your sheet:** You can share a view-only link with Gemini for specific troubleshooting.

6. **Combine prompts:** You can combine related prompts (e.g., Prompts 2 and 3) in one request.

7. **Save formulas:** Keep a note of complex formulas for future reference.

## Example Follow-Up Questions

- "The SUMPRODUCT formula for leave days is returning an error. Can you help debug it?"
- "How do I extend this to 120 months (10 years) instead of 60?"
- "Can you modify the tax calculation for Scotland's different tax bands?"
- "How do I add quarterly tax payment reminders?"
- "Can you show me how to add a chart comparing projected vs actual income?"
- "How do I handle unpaid leave or partial days?"
- "Can you add a formula to exclude bank holidays that fall on weekends?"

## Advanced Prompts

### Prompt A: Multi-Currency Support
```
I work with multiple currencies. Help me add currency conversion to track income in GBP, EUR, and USD. Add exchange rate table and convert all income to base currency (GBP).
```

### Prompt B: Weekly Reporting
```
Create a weekly income report that shows expected income for next 4 weeks, accounting for working days, holidays, and planned leave.
```

### Prompt C: Contract Gap Planning
```
I may have gaps between contracts. Help me model contract end dates, notice periods, and gap periods between contracts, showing impact on income.
```

### Prompt D: Expense Integration
```
Integrate common contractor expenses (accountant fees, insurance, software subscriptions, travel) into the balance sheet with monthly/annual tracking.
```

### Prompt E: Quarterly Tax Payments
```
Add quarterly tax payment reminders and calculations based on estimated annual income, with payment due dates and amounts.
```

