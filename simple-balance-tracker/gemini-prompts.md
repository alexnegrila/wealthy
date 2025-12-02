# Simple 5-Year Balance Tracker - Gemini Prompts

Quick and simple prompts to create a basic cash balance tracker in Google Sheets.

## Prompt 1: Create New Spreadsheet

```
Create a new Google Spreadsheet called "5-Year Balance Tracker".
```

## Prompt 2: Create Setup Sheet

```
In this spreadsheet, rename "Sheet1" to "Setup".
```

## Prompt 3: Add Setup Sheet Headers

```
In the "Setup" sheet, add these headers:

Row 1: Bold with light gray background
- A1: "Setting"
- B1: "Value"  
- C1: "Notes"

Format column B as currency (£).
```

## Prompt 4: Add Income and Starting Balance

```
In the "Setup" sheet, add:

Row 2:
- A2: "Monthly Income"
- B2: 3000
- C2: "Your regular monthly income"

Row 3:
- A3: "Starting Balance"
- B3: 10000
- C3: "Your current cash balance"
```

## Prompt 5: Add Expense Headers

```
In the "Setup" sheet, add expense section headers:

Row 5: Bold with light gray background
- A5: "Expense Category"
- B5: "Monthly Amount"
- C5: "Notes"
```

## Prompt 6: Add Expense Categories

```
In the "Setup" sheet, add these expense rows:

- A6: "Rent" | B6: 1200
- A7: "Utilities" | B7: 150
- A8: "Food" | B8: 400
- A9: "Transport" | B9: 200
- A10: "Insurance" | B10: 100
- A11: "Entertainment" | B11: 150
- A12: "Subscriptions" | B12: 50
- A13: "Healthcare" | B13: 100
- A14: "Savings Goal" | B14: 200
- A15: "Other" | B15: 100
```

## Prompt 7: Create Loans Sheet

```
Create a new sheet in this spreadsheet called "Loans".
```

## Prompt 8: Add Loans Sheet Headers

```
In the "Loans" sheet, add headers in row 1 (bold with light gray background):

- A1: "Loan Name"
- B1: "Initial Amount"
- C1: "Monthly Payment"
- D1: "Remaining Balance"
- E1: "Interest Rate"
- F1: "Start Date"
- G1: "Term (Months)"
- H1: "End Date"
- I1: "Total Interest"
- J1: "Notes"

Format columns B, C, D, and I as currency (£).
Format column E as percentage.
Format columns F and H as date (MMM YYYY format).
```

## Prompt 9: Add Loan Templates

```
In the "Loans" sheet, add these loan templates:

Row 2 - Template: Calculate Payment from Amount
- A2: Template 1: Calculate Payment
- B2: 10000
- C2: =IF(OR(B2="",E2="",G2=""),"",PMT(E2/12,G2,-B2))
- D2: =IF(OR(B2="",C2="",F2=""),"",MAX(0,B2-C2*DATEDIF(F2,TODAY(),"M")))
- E2: 5%
- F2: Jan 2025
- G2: 36
- H2: =IF(OR(F2="",G2=""),"",EDATE(F2,G2))
- I2: =IF(OR(C2="",G2="",B2=""),"",MAX(0,C2*G2-B2))
- J2: Enter: Amount, Rate, Term → Calculates: Payment

Row 3 - Template: Calculate Amount from Payment
- A3: Template 2: Calculate Amount
- B3: =IF(OR(C3="",E3="",G3=""),"",PV(E3/12,G3,-C3))
- C3: 300
- D3: =IF(OR(B3="",C3="",F3=""),"",MAX(0,B3-C3*DATEDIF(F3,TODAY(),"M")))
- E3: 5%
- F3: Jan 2025
- G3: 36
- H3: =IF(OR(F3="",G3=""),"",EDATE(F3,G3))
- I3: =IF(OR(C3="",G3="",B3=""),"",MAX(0,C3*G3-B3))
- J3: Enter: Payment, Rate, Term → Calculates: Amount

Row 4 - Template: Calculate Term from Amount and Payment
- A4: Template 3: Calculate Term
- B4: 10000
- C4: 300
- D4: =IF(OR(B4="",C4="",F4=""),"",MAX(0,B4-C4*DATEDIF(F4,TODAY(),"M")))
- E4: 5%
- F4: Jan 2025
- G4: =IF(OR(B4="",C4="",E4=""),"",NPER(E4/12,-C4,B4))
- H4: =IF(OR(F4="",G4=""),"",EDATE(F4,G4))
- I4: =IF(OR(C4="",G4="",B4=""),"",MAX(0,C4*G4-B4))
- J4: Enter: Amount, Payment, Rate → Calculates: Term

Format columns F and H as "MMM YYYY".

Note: These templates use PMT, PV, and NPER functions to avoid circular dependencies.
- Template 1: PMT calculates payment from loan amount, rate, and term
- Template 2: PV calculates loan amount from payment, rate, and term
- Template 3: NPER calculates term from loan amount, payment, and rate
- Remaining Balance (D): Calculates current balance based on payments made since start date
- End Date (H): Calculates from Start Date + Term
- Total Interest (I): Calculates total interest over loan life
```

## Prompt 9a: Create Loan Schedule Sheet

```
Create a new sheet in this spreadsheet called "Loan Schedule".
```

## Prompt 9a1: Add Loan Schedule Headers

```
In the "Loan Schedule" sheet, add headers in row 1 (bold with light gray background):

- A1: "Month/Year"
- B1: "Loan Name"
- C1: "Payment"
- D1: "Interest Charge"
- E1: "Total Payment"

Format column A as date (MMM YYYY format).
Format columns C, D, and E as currency (£).
```

## Prompt 9a2: Generate Loan Schedule Months

```
In the "Loan Schedule" sheet, starting in row 2:

Create one row for each month from the earliest loan start to the latest end date across all loans.

For the example loans (Jan 2025 onwards for 36-month terms), create rows from Jan 2025 onwards:
- Row 2: A2: =DATE(2025,1,1) | B2: =TEXTJOIN(", ",TRUE,FILTER(Loans!A:A,(Loans!F:F<=A2)*(Loans!H:H>=A2)*(ROW(Loans!A:A)>1))) | C2: =SUMPRODUCT((FILTER(Loans!F:F,ROW(Loans!F:F)>1)<=A2)*(FILTER(Loans!H:H,ROW(Loans!H:H)>1)>=A2),FILTER(Loans!C:C,ROW(Loans!C:C)>1)) | D2: =SUMPRODUCT((FILTER(Loans!F:F,ROW(Loans!F:F)>1)<=A2)*(FILTER(Loans!H:H,ROW(Loans!H:H)>1)>=A2),FILTER(Loans!D:D,ROW(Loans!D:D)>1)*FILTER(Loans!E:E,ROW(Loans!E:E)>1)/12) | E2: =C2+D2
- Row 3: A3: =DATE(2025,2,1) | Copy formulas from row 2
- Row 4: A4: =DATE(2025,3,1) | Copy formulas from row 2
- Continue for each month...

Format column A as "MMM YYYY".

Note: TEXTJOIN concatenates loan names that are active in that month (where Start Date <= month AND End Date >= month). Column C sums base monthly payments for active loans. Column D calculates interest charges (remaining balance × annual rate ÷ 12) for active loans. Column E is total payment.
```

## Prompt 9b: Create Credit Cards Sheet

```
Create a new sheet in this spreadsheet called "Credit Cards".
```

## Prompt 9b1: Add Credit Cards Sheet Headers

```
In the "Credit Cards" sheet, add headers in row 1 (bold with light gray background):

- A1: "Card Name"
- B1: "Principal Amount"
- C1: "Setup Fee %"
- D1: "Monthly Payment"
- E1: "Start Date"
- F1: "End Date"
- G1: "Notes"

Format column B and D as currency (£).
Format column C as percentage.
Format columns E and F as date (MMM YYYY format).
```

## Prompt 9b2: Add Example Credit Card

```
In the "Credit Cards" sheet, add an example:

Row 2:
- A2: Credit Card 0%
- B2: 10000
- C2: 5%
- D2: 25
- E2: =DATE(2025,11,1)
- F2: =DATE(2027,11,1)
- G2: 0% for 24 months

Format E2 and F2 to show as "MMM YYYY".
```

## Prompt 9b3: Create Credit Card Schedule Sheet

```
Create a new sheet in this spreadsheet called "Credit Card Schedule".
```

## Prompt 9b4: Add Credit Card Schedule Headers

```
In the "Credit Card Schedule" sheet, add headers in row 1 (bold with light gray background):

- A1: "Month/Year"
- B1: "Card Name"
- C1: "Payment Type"
- D1: "Fee"
- E1: "Minimum Payment"
- F1: "Principal"
- G1: "Total Payment"

Format column A as date (MMM YYYY format).
Format columns D, E, F, and G as currency (£).
```

## Prompt 9b5: Generate Credit Card Schedule Months

```
In the "Credit Card Schedule" sheet, starting in row 2:

Create one row for each month from the earliest start date to the latest end date across all credit cards.

For the example card (Nov 2025 to Nov 2027), create 25 rows:
- Row 2: A2: =DATE(2025,11,1) | B2: =TEXTJOIN(", ",TRUE,IF((FILTER('Credit Cards'!F:F,ROW('Credit Cards'!F:F)>1)=A2),FILTER('Credit Cards'!A:A,ROW('Credit Cards'!A:A)>1),"")) | C2: Mixed | D2: =SUMPRODUCT((FILTER('Credit Cards'!E:E,ROW('Credit Cards'!E:E)>1)=A2)*(FILTER('Credit Cards'!B:B,ROW('Credit Cards'!B:B)>1)*FILTER('Credit Cards'!C:C,ROW('Credit Cards'!C:C)>1))) | E2: =SUMPRODUCT((FILTER('Credit Cards'!E:E,ROW('Credit Cards'!E:E)>1)<=A2)*(FILTER('Credit Cards'!F:F,ROW('Credit Cards'!F:F)>1)>=A2)*(FILTER('Credit Cards'!D:D,ROW('Credit Cards'!D:D)>1))) | F2: =SUMPRODUCT((FILTER('Credit Cards'!F:F,ROW('Credit Cards'!F:F)>1)=A2)*(FILTER('Credit Cards'!B:B,ROW('Credit Cards'!B:B)>1))) | G2: =D2+E2+F2
- Row 3: A3: =DATE(2025,12,1) | Copy formulas from row 2
- Row 4: A4: =DATE(2026,1,1) | Copy formulas from row 2
- Continue for each month...
- Row 26: A26: =DATE(2027,11,1) | Copy formulas from row 2

Format column A as "MMM YYYY".

Note: TEXTJOIN concatenates card names that have their final payment (end date) in that month. SUMPRODUCT with FILTER(ROW()>1) formulas sum all applicable fees, payments, and principal across all cards while skipping the header row.
```

## Prompt 10: Create Balance Tracker Sheet

```
Create a new sheet in this spreadsheet called "Balance Tracker".
```

## Prompt 11: Add Balance Tracker Headers

```
In the "Balance Tracker" sheet, add headers in row 10 (bold with light gray background):

- A10: "Month"
- B10: "Starting Balance"
- C10: "Income"
- D10: "Total Expenses"
- E10: "Loan Payments"
- F10: "Credit Card Payments"
- G10: "One-Off Expenses"
- H10: "Ending Balance"

Format columns B through H as currency (£).
Add borders around the header row.
```

## Prompt 12: Add Month Dates

```
In the "Balance Tracker" sheet:

In cell A11, enter: =DATE(2025,1,1)

Format cell A11 to show date as "MMM YYYY" (e.g., "Jan 2025").
```

## Prompt 13: Fill Down Month Dates

```
In the "Balance Tracker" sheet:

In cell A12, enter: =EDATE(A11,1)

Select cell A12 and copy the formula down to A70 (to create 60 months total).
```

## Prompt 14: Add Starting Balance Formula

```
In the "Balance Tracker" sheet:

In cell B11, enter: =Setup!B3
```

## Prompt 15: Add Continuing Balance Formula

```
In the "Balance Tracker" sheet:

In cell B12, enter: =H11

Select cell B12 and copy the formula down to B70.
```

## Prompt 16: Add Income Formula

```
In the "Balance Tracker" sheet:

In cell C11, enter: =Setup!B2

Select cell C11 and copy the formula down to C70.
```

## Prompt 17: Add Total Expenses Formula

```
In the "Balance Tracker" sheet:

In cell D11, enter: =SUM(Setup!B6:B15)

Select cell D11 and copy the formula down to D70.
```

## Prompt 18: Add Loan Payments Formula

```
In the "Balance Tracker" sheet:

In cell E11, enter: =SUMPRODUCT((FILTER('Loan Schedule'!A:A,ROW('Loan Schedule'!A:A)>1)>=DATE(YEAR(A11),MONTH(A11),1))*(FILTER('Loan Schedule'!A:A,ROW('Loan Schedule'!A:A)>1)<DATE(YEAR(A11),MONTH(A11)+1,1))*(FILTER('Loan Schedule'!E:E,ROW('Loan Schedule'!E:E)>1)))

Select cell E11 and copy the formula down to E70.

Note: This formula sums all Total Payment values (base payment + interest) from the Loan Schedule that match the current month.
```

## Prompt 18a: Add Credit Card Payments Formula

```
In the "Balance Tracker" sheet:

In cell F11, enter: =SUMPRODUCT((FILTER('Credit Card Schedule'!A:A,ROW('Credit Card Schedule'!A:A)>1)>=DATE(YEAR(A11),MONTH(A11),1))*(FILTER('Credit Card Schedule'!A:A,ROW('Credit Card Schedule'!A:A)>1)<DATE(YEAR(A11),MONTH(A11)+1,1))*(FILTER('Credit Card Schedule'!G:G,ROW('Credit Card Schedule'!G:G)>1)))

Select cell F11 and copy the formula down to F70.

Note: This formula sums all Total Payment values from the Credit Card Schedule that match the current month.
```

## Prompt 19: Add Ending Balance Formula

```
In the "Balance Tracker" sheet:

In cell H11, enter: =B11+C11-D11-E11-F11-G11

Select cell H11 and copy the formula down to H70.
```

## Prompt 20: Add Current Month Balance

```
In the "Balance Tracker" sheet:

- A2: "Current Month Balance" (bold)
- B2: =H11

Format B2 as currency (£) with font size 14.
```

## Prompt 21: Add Lowest Balance Metric

```
In the "Balance Tracker" sheet:

- A3: "Lowest Balance (5 years)"
- B3: =MIN(H11:H70)

Format B3 as currency (£).
```

## Prompt 22: Add Highest Balance Metric

```
In the "Balance Tracker" sheet:

- A4: "Highest Balance (5 years)"
- B4: =MAX(H11:H70)

Format B4 as currency (£).
```

## Prompt 23: Add Average Balance Metric

```
In the "Balance Tracker" sheet:

- A5: "Average Balance"
- B5: =AVERAGE(H11:H70)

Format B5 as currency (£).
```

## Prompt 24: Add Total Income Metric

```
In the "Balance Tracker" sheet:

- A6: "Total Income (5 years)"
- B6: =SUM(C11:C70)

Format B6 as currency (£).
```

## Prompt 25: Add Total Expenses Metric

```
In the "Balance Tracker" sheet:

- A7: "Total Expenses (5 years)"
- B7: =SUM(D11:D70)

Format B7 as currency (£).
```

## Prompt 26: Add Total Loan Payments Metric

```
In the "Balance Tracker" sheet:

- A8: "Total Loan Payments (5 years)"
- B8: =SUM(E11:E70)

Format B8 as currency (£).
```

## Prompt 27: Add Borders to Summary

```
In the "Balance Tracker" sheet, add borders around cells A2:B8.
```

## Prompt 28: Create Balance Chart

```
In the "Balance Tracker" sheet, create a line chart:

1. Select range A10:A70
2. Hold Ctrl (or Cmd on Mac) and also select H10:H70
3. Insert a line chart
4. Set chart title to "5-Year Balance Projection"
5. Set horizontal axis title to "Month"
6. Set vertical axis title to "Balance (£)"
```

## Prompt 29: Position Chart

```
Move the chart in "Balance Tracker" to position it in the area spanning cells D1:J9 (above the summary metrics and data table).
```

## Prompt 30: Add Red Conditional Formatting

```
In the "Balance Tracker" sheet, add conditional formatting to H11:H70:

If cell value is less than 0:
- Background: Red
- Text color: White
```

## Prompt 31: Add Orange Conditional Formatting

```
In the "Balance Tracker" sheet, add conditional formatting to H11:H70:

If cell value is between 0 and 5000:
- Background: Orange
```

## Prompt 32: Add Yellow Conditional Formatting

```
In the "Balance Tracker" sheet, add conditional formatting to H11:H70:

If cell value is between 5000 and 15000:
- Background: Yellow
```

## Prompt 33: Add Green Conditional Formatting

```
In the "Balance Tracker" sheet, add conditional formatting to H11:H70:

If cell value is greater than 15000:
- Background: Light green
```

## Prompt 34: Highlight Current Month

```
In the "Balance Tracker" sheet, add conditional formatting to A11:H70:

Custom formula: =AND(MONTH(A11)=MONTH(TODAY()),YEAR(A11)=YEAR(TODAY()))
Format: Light blue background

Note: This checks both the month and year match the current date, so only the current month's row is highlighted.
```

## Prompt 35: Create One-Off Expenses Sheet

```
Create a new sheet in this spreadsheet called "One-Off Expenses".
```

## Prompt 36: Add One-Off Expenses Headers

```
In the "One-Off Expenses" sheet, add headers in row 1 (bold with light gray background):

- A1: "Month/Year"
- B1: "Description"
- C1: "Amount"

Format column A as date (MMM YYYY format).
Format column C as currency (£).
```

## Prompt 37: Add Example One-Off Expense

```
In the "One-Off Expenses" sheet, add an example:

Row 2:
- A2: =DATE(2025,10,1)
- B2: Car Repair
- C2: 500

Format A2 to show as "MMM YYYY" (e.g., "Oct 2025").

Note: Enter amounts as numbers, not as text.
```

## Prompt 37a: Link Credit Card Schedule to One-Off Expenses

```
Note: With the separate Fee, Minimum Payment, and Principal columns in Credit Card Schedule, all credit card payments are now automatically included in the Balance Tracker column F.

The One-Off Expenses sheet is now only for truly one-off expenses like car repairs, holidays, etc.

Remove the formulas from row 3 if they were added, and keep only manual entries like the example in row 2.
```

## Prompt 38: Update Balance Tracker to Include One-Off Expenses

```
In the "Balance Tracker" sheet, add this formula to cell G11:

=SUMPRODUCT((FILTER('One-Off Expenses'!A:A,ROW('One-Off Expenses'!A:A)>1)>=DATE(YEAR(A11),MONTH(A11),1))*(FILTER('One-Off Expenses'!A:A,ROW('One-Off Expenses'!A:A)>1)<DATE(YEAR(A11),MONTH(A11)+1,1))*(FILTER('One-Off Expenses'!C:C,ROW('One-Off Expenses'!C:C)>1)))

This will sum all one-off expenses (including credit card final payments) that fall within the same month and year.
```

## Prompt 39: Copy One-Off Expenses Formula Down

```
In the "Balance Tracker" sheet:

Select cell G11 and copy the formula down to G70.
```

## Prompt 40: Update Ending Balance Formula

```
This step is already complete - the ending balance formula in column H already includes one-off expenses (column G).
```

## Prompt 41: Verify All Formulas

```
In the "Balance Tracker" sheet, verify that:

- Column B (Starting Balance) references H11 in row 12 onwards
- Column H (Ending Balance) uses formula: =B11+C11-D11-E11-F11-G11

All formulas should be copied down to row 70.
```

## Prompt 42: Verify Balance Tracker Layout

```
In the "Balance Tracker" sheet, verify the final layout has these columns:

A: Month
B: Starting Balance
C: Income
D: Total Expenses
E: Loan Payments
F: Credit Card Payments
G: One-Off Expenses
H: Ending Balance

All formulas should be in place and working correctly.
```

## Quick Start Instructions

1. Create a new Google Sheet in your browser
2. Copy and paste each prompt (1-42) into Gemini one at a time
3. Wait for Gemini to complete each action before moving to the next prompt
4. After all prompts are complete, update the values in Setup and Loans sheets with your actual data
5. Done! Your balance tracker is ready to use

**Note:** Each prompt is a single action. This ensures Gemini can execute each step successfully.

## How to Use Your Tracker

**To change income/expenses:**
- Go to Setup sheet
- Update the amounts
- Balance Tracker automatically updates

**To add a specific expense:**
- Go to Setup sheet
- Add new row with: Category Name | Amount | Notes
- It automatically includes in calculations

**To add/remove a loan:**
- Go to Loans sheet
- Add/remove rows with loan details
- Balance Tracker automatically updates

**To add a one-off expense:**
- Go to One-Off Expenses sheet
- Add a new row: Date (MMM YYYY) | Description | Amount
- The expense will automatically appear in the Balance Tracker for that month
- You can add multiple one-off expenses for different months

**To extend beyond 5 years:**
- Copy the formulas in the last row
- Paste to add more months

## Example Values to Test With

**Setup Sheet:**
- Monthly Income: £3,000
- Starting Balance: £10,000
- Rent: £1,200
- Utilities: £150
- Food: £400
- Transport: £200
- Insurance: £100
- Other: £300

**Loans Sheet:**
- Car Loan | £250 | £5,000 | 5% | Dec 2026
- Personal Loan | £180 | £3,000 | 8% | Jun 2027

This should show your balance growing from £10,000 to around £41,000 over 5 years.
