# Simple 5-Year Balance Tracker

A straightforward cash balance projection tool built in Google Sheets.

## What It Does

Track your cash balance over 5 years with:
- Fixed monthly income
- Regular monthly expenses
- Loan payments
- Visual chart showing balance over time

## Features

- **Simple setup** - Just enter your income and expenses once
- **Automatic calculations** - Balance updates automatically
- **5-year projection** - See 60 months ahead
- **Loan tracking** - Add multiple loans with monthly payments
- **Visual chart** - Line chart showing balance trend
- **Color coding** - Red/orange/yellow/green based on balance levels
- **Easy updates** - Change any value and see instant impact

## Structure

### Setup Sheet
- Monthly income (single value)
- Starting balance
- List of monthly expenses by category
- Automatically sums all expenses

### Loans Sheet
- Loan name
- Monthly payment amount
- Remaining balance
- Interest rate and end date
- Automatically adds all loan payments

### Balance Tracker Sheet
- 60-month projection (current month + 5 years)
- Columns: Month | Starting Balance | Income | Expenses | Loan Payments | Ending Balance
- Formula: Ending Balance = Starting + Income - Expenses - Loans
- Next month's starting = Previous month's ending

## Use Cases

- Personal finance planning
- Business cash flow (fixed monthly revenue)
- Savings goals tracking
- Debt payoff planning
- Emergency fund building
- Financial milestone planning

## Limitations

This simplified version:
- Uses fixed monthly income (no variation)
- Uses fixed monthly expenses (no variation)
- No daily rate calculations
- No actual vs. projected tracking
- No financial year boundaries
- No working days calculations

For more complex needs with variable income, see `../business-finance/`

## Getting Started

Use the prompts in `gemini-prompts.md` to build this tracker in under 10 minutes.
