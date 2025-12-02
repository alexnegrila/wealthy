# Contractor Income Tracker

A Google Sheets-based income tracking system for UK contractors working on day rates, with automatic calculation of monthly income based on working days, bank holidays, and personal leave.

## Overview

This system is designed for contractors who:
- Work on a fixed day rate
- Need to track UK bank holidays
- Want to plan personal leave days
- Need monthly income projections for the next 5 years
- Want to see income on a balance sheet

## Key Features

### Day Rate Calculation
- Single day rate configuration
- Automatic working day calculation per month
- Excludes weekends, UK bank holidays, and leave days
- Monthly income = Working Days × Day Rate

### UK Working Days
- Automatically excludes weekends (Saturday/Sunday)
- Integrates UK bank holidays from separate sheet
- Accounts for personal leave days
- Calculates actual billable days per month

### Leave Management
- Track leave periods with start date and duration
- Automatically deducts from working days
- Annual leave planning and visualization
- Leave balance tracking

### 5-Year Monthly Projections
- Monthly income breakdown for 60 months
- Automatic working day calculation for each month
- Assumes standard leave pattern (adjustable)
- Displays on balance sheet format

## Components

### Daily Rate Configuration
- Current day rate
- Rate change schedule (optional)
- Contract terms and notes

### UK Bank Holidays Sheet
- Comprehensive list of UK bank holidays
- Covers England & Wales, Scotland, Northern Ireland
- Multi-year coverage (5+ years)
- Automatically excluded from working days

### Leave Days Sheet
- Leave start date
- Number of leave days
- Leave type (Annual, Sick, Other)
- Automatic calculation of affected months

### Monthly Income Projections
- 60-month rolling projection
- Columns: Month, Working Days, Day Rate, Income
- Actual vs. Projected tracking
- Integration with balance sheet

### Balance Sheet
- Monthly income entries for 5 years
- Starting balance
- Income additions
- Expense deductions (optional)
- Running balance calculation

## Calculation Logic

```
Working Days per Month = 
  Total Weekdays in Month
  - Bank Holidays in Month
  - Leave Days in Month

Monthly Income = Working Days × Day Rate
```

## Getting Started

1. Create a new Google Spreadsheet
2. Follow the [Setup Guide](setup-guide.md)
3. Use the [Gemini Prompts](gemini-prompts.md) to build the system
4. Configure your day rate
5. Add UK bank holidays (or use provided list)
6. Plan your leave days
7. Review your 5-year income projection

## Typical UK Working Days

Approximate working days per year for UK contractors:

- Total days: 365
- Weekends (52 weeks): ~104 days
- UK Bank Holidays: 8 days (England & Wales)
- Annual Leave (typical): 25 days
- **Estimated Billable Days: ~228 days/year**

Monthly average: ~19 working days (varies by month and leave)

## Use Cases

- **Income Planning**: Project income for next 5 years
- **Leave Planning**: See financial impact of leave days
- **Rate Negotiation**: Model income at different day rates
- **Tax Planning**: Estimate annual income for tax provisions
- **Financial Goals**: Plan savings and investments based on projected income
- **Contract Comparison**: Compare different day rates and contracts

## Integration with Balance Sheet

The monthly income projections feed directly into your balance sheet, allowing you to:
- Track cumulative wealth over 5 years
- Plan for large expenses
- Model different savings rates
- Visualize financial trajectory

## Notes

- This system focuses on income only (expenses tracked separately)
- Assumes steady day rate (but can model rate changes)
- UK-specific (England & Wales bank holidays by default)
- Best for contractors on full-time equivalent contracts
- Can be adapted for part-time or variable schedules

