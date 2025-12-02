# Contractor Income Tracker Setup Guide

Step-by-step instructions for creating a contractor income tracker in Google Sheets with 5-year monthly projections based on UK working days.

## Architecture Overview

**Simple Structure:** Calculate monthly income from day rate and working days, accounting for weekends, UK bank holidays, and personal leave.

**Time Horizon:** 5-year monthly projection (60 months) with option to track actuals

**Key Formula:** `Monthly Income = (Weekdays - Bank Holidays - Leave Days) × Day Rate`

## Step 1: Create the Spreadsheet Structure

Create a new Google Spreadsheet with the following sheets:

### Core Sheets:
1. **Dashboard** - Summary view and key metrics
2. **Day Rate Config** - Your day rate and contract details
3. **UK Bank Holidays** - List of UK bank holidays (5+ years)
4. **Leave Days** - Your planned leave periods
5. **Monthly Income** - 60-month income projection
6. **Balance Sheet** - Running balance with income

## Step 2: Set Up Day Rate Configuration

Create the **Day Rate Config** sheet:

| Setting | Value | Notes |
|---------|-------|-------|
| Day Rate | £500 | Your current day rate |
| Currency | GBP | £ (British Pound) |
| Contract Start | 01/04/2024 | When you started |
| Standard Working Days | Mon-Fri | Weekdays only |
| Typical Annual Leave | 25 days | Your leave allowance |
| Rate Review Date | 01/04/2025 | Next rate review |
| Future Day Rate | £550 | Rate after review (optional) |

**Key Cell:** `B2` contains your current day rate - this will be referenced throughout

## Step 3: Create UK Bank Holidays Sheet

Create the **UK Bank Holidays** sheet with columns:
- **Column A:** Date (DD/MM/YYYY format)
- **Column B:** Holiday Name
- **Column C:** Region (England & Wales, Scotland, Northern Ireland, or All UK)

### Common UK Bank Holidays (England & Wales)

**2024:**
- 01/01/2024 - New Year's Day
- 29/03/2024 - Good Friday
- 01/04/2024 - Easter Monday
- 06/05/2024 - Early May Bank Holiday
- 27/05/2024 - Spring Bank Holiday
- 26/08/2024 - Summer Bank Holiday
- 25/12/2024 - Christmas Day
- 26/12/2024 - Boxing Day

**2025:**
- 01/01/2025 - New Year's Day
- 18/04/2025 - Good Friday
- 21/04/2025 - Easter Monday
- 05/05/2025 - Early May Bank Holiday
- 26/05/2025 - Spring Bank Holiday
- 25/08/2025 - Summer Bank Holiday
- 25/12/2025 - Christmas Day
- 26/12/2025 - Boxing Day

**Note:** Add holidays for 2026-2029 as well. Bank holidays dates can be found at [gov.uk](https://www.gov.uk/bank-holidays).

### Formula Tips:
- Use data validation to ensure date format
- Sort by date (oldest to newest)
- Color-code by region if needed
- Add a helper column for year: `=YEAR(A2)`

## Step 4: Create Leave Days Sheet

Create the **Leave Days** sheet with columns:

| Leave Start Date | Number of Days | Leave Type | End Date (Calculated) | Affected Months |
|-----------------|----------------|------------|----------------------|-----------------|
| 05/08/2024 | 5 | Annual Leave | 09/08/2024 | Aug 2024 |
| 23/12/2024 | 7 | Annual Leave | 02/01/2025 | Dec 2024, Jan 2025 |
| 14/04/2025 | 3 | Annual Leave | 16/04/2025 | Apr 2025 |

### Column Formulas:

**Column D (End Date):**
```
=WORKDAY(A2, B2-1)
```
This calculates the end date based on start date and number of working days.

**Column E (Affected Months):**
```
=TEXT(A2,"MMM YYYY") & IF(MONTH(A2)<>MONTH(D2), ", " & TEXT(D2,"MMM YYYY"), "")
```
Shows which months are affected by this leave period.

### Leave Planning Tips:
- Plan annual leave in advance for accurate projections
- Include public holidays that fall adjacent to leave
- Consider adding buffer/sick days (e.g., 5 days/year estimate)
- Update actuals as the year progresses

## Step 5: Create Monthly Income Projections

Create the **Monthly Income** sheet with 60 rows (5 years) and these columns:

| A: Month | B: Year | C: Total Days | D: Weekdays | E: Bank Holidays | F: Leave Days | G: Working Days | H: Day Rate | I: Monthly Income | J: Actual Days | K: Actual Income | L: Status |
|----------|---------|---------------|-------------|------------------|---------------|-----------------|-------------|-------------------|----------------|------------------|-----------|
| Apr | 2024 | 30 | 22 | 1 | 0 | 21 | £500 | £10,500 | | | Projected |
| May | 2024 | 31 | 23 | 1 | 0 | 22 | £500 | £11,000 | | | Projected |

### Column Formulas:

**Column A (Month):**
```
=TEXT(DATE(2024,4,1) + (ROW()-2)*30, "MMM")
```
Generates month names starting from April 2024.

**Column B (Year):**
```
=YEAR(DATE(2024,4,1) + (ROW()-2)*30)
```
Generates corresponding year.

**Column C (Total Days):**
```
=DAY(EOMONTH(DATE(B2,MONTH(A2&" 1"),1),0))
```
Calculates days in the month.

**Column D (Weekdays):**
```
=NETWORKDAYS.INTL(DATE(B2,MONTH(A2&" 1"),1), EOMONTH(DATE(B2,MONTH(A2&" 1"),1),0), 1)
```
Counts weekdays (Mon-Fri) in the month.

**Column E (Bank Holidays):**
```
=COUNTIFS('UK Bank Holidays'!A:A, ">="&DATE(B2,MONTH(A2&" 1"),1), 'UK Bank Holidays'!A:A, "<="&EOMONTH(DATE(B2,MONTH(A2&" 1"),1),0), 'UK Bank Holidays'!A:A, "<>")
```
Counts bank holidays in this month from the UK Bank Holidays sheet.

**Column F (Leave Days):**
```
=SUMPRODUCT(
  ('Leave Days'!$A$2:$A$100>=DATE(B2,MONTH(A2&" 1"),1))*
  ('Leave Days'!$A$2:$A$100<=EOMONTH(DATE(B2,MONTH(A2&" 1"),1),0))*
  'Leave Days'!$B$2:$B$100
) + SUMPRODUCT(
  ('Leave Days'!$D$2:$D$100>=DATE(B2,MONTH(A2&" 1"),1))*
  ('Leave Days'!$D$2:$D$100<=EOMONTH(DATE(B2,MONTH(A2&" 1"),1),0))*
  'Leave Days'!$B$2:$B$100
)
```
Counts leave days that fall in this month (handles leave periods spanning months).

**Column G (Working Days):**
```
=MAX(0, D2 - E2 - F2)
```
Calculates actual billable working days (ensures no negative values).

**Column H (Day Rate):**
```
='Day Rate Config'!B2
```
References your day rate. Can add IF statement for future rate changes.

**Column I (Monthly Income):**
```
=G2 * H2
```
Calculates income: Working Days × Day Rate.

**Column J (Actual Days):**
Leave blank for manual entry of actual days worked.

**Column K (Actual Income):**
```
=IF(J2<>"", J2*H2, "")
```
Calculates actual income if actual days are entered.

**Column L (Status):**
```
=IF(K2<>"", "Actual", IF(DATE(B2,MONTH(A2&" 1"),1)<TODAY(), "In Progress", "Projected"))
```
Shows whether month is actual, in progress, or projected.

### Formatting:
- Format columns H, I, K as currency (£)
- Format column A as custom "MMM"
- Bold the header row
- Freeze the header row (View > Freeze > 1 row)
- Apply alternating row colors for readability

## Step 6: Create Balance Sheet

Create the **Balance Sheet** sheet with columns:

| Month | Income | Expenses | Net Cash Flow | Ending Balance | Notes |
|-------|--------|----------|---------------|----------------|-------|
| Start | - | - | - | £10,000 | Starting balance |
| Apr 2024 | £10,500 | £2,000 | £8,500 | £18,500 | |
| May 2024 | £11,000 | £2,000 | £9,000 | £27,500 | |

### Column Formulas:

**Month (Column A):**
```
='Monthly Income'!A2 & " " & 'Monthly Income'!B2
```
Links to the month from Monthly Income sheet.

**Income (Column B):**
```
='Monthly Income'!I2
```
Pulls projected income (or use column K for actuals).

**Expenses (Column C):**
Manual entry or link to expense tracker.

**Net Cash Flow (Column D):**
```
=B2 - C2
```
Income minus expenses.

**Ending Balance (Column E):**
```
=E1 + D2
```
Previous balance plus net cash flow (running total).

### Set Starting Balance:
In cell E1, enter your current bank/business account balance.

## Step 7: Create Dashboard

Create the **Dashboard** sheet with these sections:

### Summary Metrics (Top of dashboard)

| Metric | Value | Formula |
|--------|-------|---------|
| Current Day Rate | £500 | `='Day Rate Config'!B2` |
| Average Working Days/Month | 19.2 | `=AVERAGE('Monthly Income'!G:G)` |
| Average Monthly Income | £9,600 | `=AVERAGE('Monthly Income'!I:I)` |
| Projected Annual Income | £115,200 | `=SUMIFS('Monthly Income'!I:I, 'Monthly Income'!B:B, YEAR(TODAY()))` |
| Total Leave Days This Year | 25 | `=SUMIFS('Leave Days'!B:B, 'Leave Days'!A:A, ">="&DATE(YEAR(TODAY()),1,1))` |
| Current Balance | £27,500 | `=INDEX('Balance Sheet'!E:E, COUNTA('Balance Sheet'!E:E))` |

### 5-Year Projection Summary

Create a summary table:

| Year | Working Days | Projected Income | Ending Balance |
|------|--------------|------------------|----------------|
| 2024 | 228 | £114,000 | £135,000 |
| 2025 | 228 | £114,000 | £249,000 |
| 2026 | 229 | £114,500 | £363,500 |
| 2027 | 228 | £114,000 | £477,500 |
| 2028 | 229 | £114,500 | £592,000 |

Use `SUMIFS` to aggregate from Monthly Income by year.

### Charts

**Chart 1: Monthly Income (Next 12 Months)**
- Type: Column chart
- X-axis: Month
- Y-axis: Income
- Data: Next 12 rows from Monthly Income sheet

**Chart 2: 5-Year Balance Projection**
- Type: Line chart
- X-axis: Month/Year
- Y-axis: Ending Balance
- Data: Balance Sheet ending balance column

**Chart 3: Working Days by Month**
- Type: Bar chart
- X-axis: Month
- Y-axis: Working Days
- Data: Working Days column from Monthly Income
- Shows seasonality and leave impact

## Step 8: Add Conditional Formatting

### Monthly Income Sheet:
1. **Low working days** (Column G < 15): Yellow background
2. **High working days** (Column G > 21): Green background
3. **Current month**: Light blue background (entire row)
4. **Past months with no actuals** (Column J empty, date < today): Orange background

### Balance Sheet:
1. **Negative balance**: Red background and bold
2. **Balance < £10,000**: Orange background (warning threshold)
3. **Balance > £100,000**: Green background

### Leave Days:
1. **Leave in current month**: Highlight in yellow
2. **Past leave**: Gray out

## Step 9: Add Data Validation

### Monthly Income - Actual Days (Column J):
- List of values: 0 to 23 (dropdown)
- Or custom range: `>=0` and `<=23`

### Leave Days - Leave Type (Column C):
- Dropdown: Annual Leave, Sick Leave, Unpaid Leave, Other

### Day Rate Config:
- Currency dropdown: GBP, EUR, USD (if relevant)

## Step 10: Handle Rate Changes

If your day rate changes during the 5-year period:

**Option 1: Simple IF Statement**
Update Column H formula in Monthly Income:

```
=IF(DATE(B2,MONTH(A2&" 1"),1) >= 'Day Rate Config'!B7, 'Day Rate Config'!B8, 'Day Rate Config'!B2)
```
Where B7 is rate change date and B8 is new rate.

**Option 2: Rate Schedule Table**
Create a rate history table in Day Rate Config:

| Effective From | Day Rate |
|----------------|----------|
| 01/04/2024 | £500 |
| 01/04/2025 | £550 |
| 01/04/2026 | £600 |

Then use VLOOKUP or IFS in Monthly Income Column H.

## Step 11: Annual Leave Planning

In the Dashboard, add a leave summary:

### Current Year Leave Tracker

| Category | Days Used | Days Planned | Days Remaining | Total Allowance |
|----------|-----------|--------------|----------------|-----------------|
| Annual Leave | 10 | 15 | 0 | 25 |
| Sick Days | 2 | 0 | 3 | 5 (estimate) |

**Formula for Days Used:**
```
=SUMIFS('Leave Days'!B:B, 'Leave Days'!C:C, "Annual Leave", 'Leave Days'!A:A, "<"&TODAY())
```

**Formula for Days Planned:**
```
=SUMIFS('Leave Days'!B:B, 'Leave Days'!C:C, "Annual Leave", 'Leave Days'!A:A, ">="&TODAY())
```

**Calendar View:** Create a simple calendar showing leave days highlighted for easy visualization.

## Step 12: Validation & Testing

**Verify calculations:**
1. Check that weekdays calculation is correct (should be ~20-23 per month)
2. Verify bank holidays are being deducted correctly
3. Test leave days calculation with various scenarios:
   - Leave within one month
   - Leave spanning two months
   - Multiple leave periods in same month
4. Ensure balance sheet flows correctly month-to-month
5. Check that formulas copy down all 60 rows correctly
6. Validate that Current Day Rate changes propagate through all calculations

**Test scenarios:**
- Add a week of leave in August - does income drop correctly?
- Change your day rate - does future income update?
- Add a new bank holiday - is it excluded from working days?

## Step 13: Monthly Workflow

### At the start of each month:
1. Review projected working days for current month
2. Check upcoming leave and bank holidays
3. Note any rate changes

### At the end of each month:
1. Enter actual working days in Monthly Income sheet (Column J)
2. Verify actual income matches (or note variance)
3. Update Balance Sheet with actual expenses
4. Review ending balance
5. Update Status column to "Actual"

### Quarterly:
1. Review year-to-date income vs. projection
2. Adjust future leave plans if needed
3. Update future projections based on trends
4. Review rate increase opportunities

## Tips for Accuracy

1. **UK Bank Holidays:** Verify dates annually from [gov.uk/bank-holidays](https://www.gov.uk/bank-holidays)

2. **Leave Planning:** Be realistic with leave estimates. Most contractors take:
   - Annual leave: 20-28 days
   - Sick days: 3-7 days (estimate)
   - Other time off: 2-5 days

3. **Working Days Range:** Typical monthly working days range:
   - Minimum: 15-17 days (December, August with leave)
   - Maximum: 22-23 days (full months, no leave)
   - Average: 19-20 days

4. **Rate Reviews:** Most contractors review rates annually. Plan increases conservatively.

5. **Buffer:** Consider adding a "buffer" factor of 95% to projected income to account for:
   - Unexpected sick days
   - Bank holidays not initially accounted for
   - Contract gaps
   - Administrative days

## Advanced Features

### Scenario Planning
Create additional columns in Monthly Income:
- Best Case (higher rate or more days)
- Worst Case (lower rate or fewer days)
- Conservative Case (current rate with buffer)

### Tax Planning
Add a Tax Calculation column:
- Estimated income tax
- National Insurance contributions
- Net income after tax

### Contract Comparison
Duplicate the sheet structure to compare:
- Current contract vs. new opportunity
- Different day rates
- Different leave allowances
- Impact of going permanent

## Integration with Balance Tracker

To integrate with the simple balance tracker (`/wealthy/simple-balance-tracker/`):

1. **Export monthly income** from this sheet (Column I)
2. **Import as income entries** in the balance tracker
3. **Frequency:** Set to monthly
4. **Amount:** Use projected or actual income
5. **Auto-update:** Use `IMPORTRANGE` if in separate sheets

This gives you a complete financial picture: income projections + balance tracking + expense management.

## Troubleshooting

**Issue:** Negative working days
- Check that bank holidays sheet has correct date format
- Verify leave days aren't counted twice
- Ensure formulas use MAX(0, ...) to prevent negatives

**Issue:** Income not calculating
- Verify Day Rate Config cell reference is correct
- Check that Working Days column has values
- Ensure currency formatting isn't breaking formula

**Issue:** Leave days not deducting
- Confirm Leave Days sheet has dates in Column A
- Check that date formats match (DD/MM/YYYY or MM/DD/YYYY)
- Verify SUMPRODUCT formula includes correct range

**Issue:** Balance sheet not flowing
- Check that first ending balance formula references starting balance
- Verify each row's starting balance equals previous ending balance
- Ensure no circular references

## Next Steps

Once your contractor income tracker is set up:

1. **Integrate with expense tracking** (link to `/wealthy/business-finance/` if needed)
2. **Connect to balance sheet** for full financial picture
3. **Set up alerts** (conditional formatting) for low balance or unusual patterns
4. **Create reports** for tax purposes or contract reviews
5. **Plan financial goals** based on 5-year projections

## Example Use Cases

**Rate Negotiation:** 
"If I increase my day rate from £500 to £550, my annual income goes from £114,000 to £125,400 - a £11,400 increase."

**Leave Planning:** 
"Taking 2 weeks off in August will reduce my August income from £11,000 to £6,000. I need to ensure my balance can cover this."

**Contract Comparison:** 
"Contract A offers £500/day with 25 leave days. Contract B offers £480/day with 30 leave days. Which is better financially?"

**Savings Goals:** 
"Over 5 years, I project £570,000 in income. After expenses, I should be able to save £300,000 towards a house deposit."

