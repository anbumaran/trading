# Mark Minervini Stock Screening Conditions

> Source note: These conditions come from Minervini's published method (*Trade Like a Stock Market Wizard*: Trend Template + VCP), not a transcript of the video. The video page could not be fetched. Verify against the video and adjust if needed.

## 1. Trend Template (Stage 2 Uptrend)

1. Price is above both the 150-day and 200-day moving averages.
2. The 150-day MA is above the 200-day MA.
3. The 200-day MA has been trending up for at least 1 month (ideally 4-5 months).
4. The 50-day MA is above both the 150-day and 200-day MAs.
5. Price is above the 50-day MA.
6. Price is at least 30% above its 52-week low (many traders prefer 40-100%+).
7. Price is within 25% of its 52-week high (closer is better).
8. Relative Strength rating is 70 or higher (ideally 80-90+).

## 2. Fundamentals (SEPA)

- Current quarterly EPS growth of at least 25%, ideally accelerating quarter over quarter.
- Sales growth of at least 20-25%.
- Expanding profit margins.
- Leadership in a strong industry group; a new product or catalyst is a bonus.

## 3. VCP (Volatility Contraction Pattern) Base

- Prior uptrend, then a base of at least 3 weeks (typically 3-65 weeks).
- 2-6 successive pullbacks (contractions), each shallower than the last, e.g. 25% -> 12% -> 6% -> 3%.
- Volume dries up as the base tightens, with the quietest days at the right edge.
- Final contraction is tight (roughly under 10%), with price closing near the highs of the range.

## 4. Entry Trigger

- Buy on a breakout above the pivot point (high of the final contraction).
- Breakout-day volume is at least 40-50% above the 50-day average volume.
- Avoid stocks extended more than 5% above the pivot.

## 5. Risk Rules

- Initial stop 7-8% below entry (tighter if the pattern is tight).
- Risk-to-reward ratio of at least 1:3.
- Trade only liquid stocks (enough average volume and a minimum price).

## Screenability

| Condition group | Fully screenable? |
|---|---|
| Trend Template | Yes |
| Fundamentals | Yes |
| Volume and 52-week high/low distance | Yes |
| VCP shape | Partly (approximate with shrinking range and volume; confirm on chart) |

# Chartink Minervini-Style Scan (Trend Template + Fundamentals)

> Syntax is written from memory. Chartink's fundamental clauses are best added via the **filter icon** in the scan builder so field names are exact. If a line errors, fix the highlighted part.

## Step 1: Technical clause (paste into the scan clause box)

```
( {cash} (
  latest close > latest sma( close, 50 ) and
  latest sma( close, 50 ) > latest sma( close, 150 ) and
  latest sma( close, 150 ) > latest sma( close, 200 ) and
  latest sma( close, 200 ) > 21 days ago sma( close, 200 ) and
  latest close >= 1.3 * min( 252, latest low ) and
  latest close >= 0.75 * max( 252, latest high ) and
  latest close / 126 days ago close >= 1.2 and
  latest close >= 10 and
  latest sma( volume, 50 ) * latest close >= 10000000
) )
```

```
( {cash} (
  latest close > latest sma( close, 50 ) and
  latest sma( close, 50 ) > latest sma( close, 150 ) and
  latest sma( close, 150 ) > latest sma( close, 200 ) and
  latest sma( close, 200 ) > 21 days ago sma( close, 200 ) and
  latest close >= 1.3 * min( 252, latest low ) and
  latest close >= 0.75 * max( 252, latest high ) and
  latest close / 126 days ago close >= 1.2 and
  latest close >= 10 and
  latest sma( volume, 50 ) * latest close >= 10000000 and

  latest "Net Sales[quarter]" >= 1.2 * 4 quarters ago "Net Sales[quarter]" and
  latest "Net Profit[quarter]" >= 1.25 * 4 quarters ago "Net Profit[quarter]" and
  4 quarters ago "Net Profit[quarter]" > 0 and
  latest "Operating profit margin[qr]" > 4 quarters ago "Operating profit margin[qr]"
) )
```

| Rule | Covered by |
|---|---|
| Price > 50 > 150 > 200 DMA | Lines 1-3 |
| 200 DMA rising (1 month) | Line 4 |
| 30%+ above 52w low | Line 5 |
| Within 25% of 52w high | Line 6 |
| RS proxy (6m return >= 20%) | Line 7 |
| Liquidity (price >= 10, turnover >= 1 cr) | Lines 8-9 |

For a 4-5 month rising 200 DMA, add: `and latest sma( close, 200 ) > 105 days ago sma( close, 200 )`

## Step 2: Fundamental clauses (add with the filter icon)

Add these as "AND" conditions:

1. Latest `Net Sales[quarter]` >= 1.2 x `Net Sales[quarter]` 4 quarters ago  (sales growth >= 20% YoY)
2. Latest `Net Profit[quarter]` >= 1.25 x `Net Profit[quarter]` 4 quarters ago  (profit growth >= 25% YoY)
3. Latest `Operating profit margin[qr]` > `Operating profit margin[qr]` 4 quarters ago  (margin expansion)
4. Optional: `Market Cap` above your minimum size

Notes:
- Net profit growth can mislead when the base quarter is tiny or negative.
- Validate a few results against Chartink's fundamental page or Screener.in.

## Step 3: Sort and shortlist

- Sort results by 6-month return (relative strength proxy).
- Review the top 20-30 names.

## Step 4: Take to TradingView

1. Export or copy the symbols.
2. Format as `NSE:SYMBOL,NSE:SYMBOL,...` and save as .txt.
3. Import into a TradingView watchlist.
4. Add `minervini_scan.pine` to the chart and review VCP and breakout setups by eye.

## Step 5: Manual checks on finalists

- EPS growth accelerating quarter over quarter
- Strong industry group and any catalyst
- Clean base with tightening price and drying volume

## Tuning

| Goal | Change |
|---|---|
| Stricter | 1.3 -> 1.5, 0.75 -> 0.85, 1.2 -> 1.4 (return) |
| Looser | 0.75 -> 0.70, 1.2 -> 1.1 (return) |
