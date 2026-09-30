# P123 Technical Functions Reference

All technical functions operate on price/volume bar data (trading days).

Common optional parameters:
- **offset**: bars ago to start calculation (default 0 = from latest)
- **series**: price series to use (default = stock's own prices)
  - `#Close`, `#Open`, `#High`, `#Low` — stock prices
  - `#Bench` — benchmark
  - `$SPY`, `$QQQ`, etc. — specific ticker
  - `#Industry`, `#Sector` — index prices
  - `#Equity` — portfolio value (strategies only)
  - Numeric ID — specific P123 stock ID

Many functions also have `_D` (calendar days) and `_W` (weeks) variants.

---

## Price Functions (OHLCV)

### Bar-based (trading days)

| Function | Description |
|----------|-------------|
| `Close(barsAgo [, series])` | Closing price N bars ago |
| `Open(barsAgo [, series])` | Opening price |
| `Hi(barsAgo [, series])` | **High price** |
| `Low(barsAgo [, series])` | Low price |
| `Vol(barsAgo)` | **Volume** N bars ago |
| `CloseAdj(barsAgo)` | Split-adjusted close |
| `CloseExDiv(barsAgo)` | Close unadjusted by dividends |

### Calendar-day variants (`_D`)

| Function | Description |
|----------|-------------|
| `Close_D(days [, series])` | Close price N calendar days ago |
| `Open_D(days [, series])` | Open price N calendar days ago |
| `Hi_D(days [, series])` | High price N calendar days ago |
| `Low_D(days [, series])` | Low price N calendar days ago |
| `Vol_D(days [, series])` | Volume N calendar days ago |

### Weekly variants (`_W`)

| Function | Description |
|----------|-------------|
| `Close_W(weeks [, series])` | Weekly close price |
| `Open_W(weeks [, series])` | Weekly open price |
| `Hi_W(weeks [, series])` | Weekly high price |
| `Low_W(weeks [, series])` | Weekly low price |
| `Vol_W(weeks [, series])` | Weekly volume |

### Benchmark Prices

| Function | Description |
|----------|-------------|
| `BenchClose(barsAgo)` | Benchmark close price |
| `BenchHi(barsAgo)` | Benchmark high price |
| `BenchLow(barsAgo)` | Benchmark low price |
| `BenchOpen(barsAgo)` | Benchmark open price |

### Bid-Ask

| Function | Description |
|----------|-------------|
| `Spread(barsAgo)` | Closing spread (ask − bid) for the bar |
| `Spread_D(daysAgo)` | Closing spread N calendar days ago |

---

## Volume & Liquidity Functions

| Function | Description |
|----------|-------------|
| `AvgVol(bars [, offset, series])` | Average volume over N bars |
| `MedianVol(bars [, offset, series])` | Median volume over N bars |
| `AvgDailyTot(bars [, offset])` | **Avg daily dollar volume** (price × volume) |
| `MedianDailyTot(bars [, offset])` | Median daily dollar volume |
| `MinLiquidity(bars [, offset])` | Lowest daily dollar volume in period |
| `PctAvgDailyTot(bars [, offset])` | % of liquidity traded |
| `UpDownRatio(bars, offset)` | Up/down volume ratio |

**Unit:** `AvgDailyTot` returns **plain dollars**, not thousands (verified 2026-09-29: IBM `AvgDailyTot(20)` ≈
918,509,430 = `Close(0)` × `AvgVol(20)`). Write $100K a day as `AvgDailyTot(20) > 100000`, not `> 100`.
`MedianDailyTot` and `MinLiquidity` are dollar-volume functions too; assume the same scale, but check with a
`data()` call if it matters.

**Pre-built volume/price factors:**

| Factor | Description |
|--------|-------------|
| `AvgVol5` / `AvgVol10` / `AvgVol1M` / `AvgVol3M` / `AvgVol6M` | Average volume (5/10 day, 1/3/6 month) |
| `Vol10DAvg` / `Vol3MAvg` | Volume vs 10-day/3-month average |
| `VolD%ShsOut` / `VolM%ShsOut` | Volume as % of shares outstanding (daily/monthly) |
| `Pct52WH` / `Pct52WL` | Price as % of 52-week high/low |
| `Pct3MH` / `Pct3ML` / `Pct4WH` / `Pct4WL` | Price as % of 3-month/4-week high/low |
| `PriceH` / `PriceL` | Period high/low price |

---

## Return Functions

### Price returns (include dividends by default)

| Function | Description |
|----------|-------------|
| `Ret%Chg(bars [, offset, series])` | **Total return** (incl dividends) over N bars |
| `Ret%Chg_D(days [, offset, series])` | Total return in calendar days |
| `Rel%Chg(bars [, offset, series])` | Return **relative to benchmark** |
| `Rel%Chg_D(days [, offset, series])` | Relative return in calendar days |
| `FXPerf(bars [, offset])` | Currency adjustment ratio for the period |

### Pre-built return factors

| Factor | Description |
|--------|-------------|
| `Ret1W%Chg` | Total return 1 week |
| `Ret4W%Chg` | Total return 4 weeks |
| `Ret3M%Chg` | Total return 13 weeks (~3 months) |
| `Ret6M%Chg` | Total return 6 months |
| `Ret1Y%Chg` | Total return 1 year |
| `Ret2Y%Chg` | Total return 2 years |

### Pre-built price change factors (excl dividends)

| Factor | Description |
|--------|-------------|
| `Pr4W%Chg` | Price change 4 weeks (excl div) |
| `Pr13W%Chg` | Price change 13 weeks |
| `Pr26W%Chg` | Price change 26 weeks |
| `Pr52W%Chg` | Price change 52 weeks |
| `Pr4WRel%Chg` | Price change 4 weeks, relative to benchmark |
| `Pr13WRel%Chg` | Price change 13 weeks, relative |
| `Pr26WRel%Chg` | Price change 26 weeks, relative |
| `Pr52WRel%Chg` | Price change 52 weeks, relative |

### Future returns (for backtesting/labels)

| Function | Description |
|----------|-------------|
| `Future%Chg(bars [, series])` | Future total return |
| `Future%Chg_D(days [, series])` | Future total return in calendar days |
| `FutureRel%Chg(bars [, series])` | Future return relative to benchmark |
| `FutureRel%Chg_D(days [, series])` | Future relative return in calendar days |

```
// 12-1 month momentum (skip most recent month)
Ret%Chg(252, 21)

// Relative strength vs benchmark
Rel%Chg(252) > 0
```

---

## Moving Averages

| Function | Description |
|----------|-------------|
| `SMA(bars [, offset, series])` | Simple moving average |
| `EMA(bars [, offset, series])` | Exponential moving average |
| `WMA(bars [, offset, series])` | Weighted moving average |
| `VMA(bars [, offset])` | Volume-weighted moving average |
| `SMA_D(days [, offset, series])` | SMA in calendar days |
| `SMA_W(weeks [, offset, series])` | SMA in weeks |
| `EMA_D(days [, offset, series])` | EMA in calendar days |
| `EMA_W(weeks [, offset, series])` | EMA in weeks |
| `WMA_D(days [, offset, series])` | WMA in calendar days |

### Percent from moving average

| Function | Description |
|----------|-------------|
| `SMAPct(bars [, series])` | % distance from SMA (incl div) |
| `SMAPct_W(weeks [, series])` | % distance from SMA (weekly series) |

### Crossover functions

| Function | Description |
|----------|-------------|
| `CrossOver(type, bars, period1, period2)` | MA crossover signal |
| `CrossUnder(type, bars, period1, period2)` | MA crossunder signal |

```
// Golden cross
SMA(50, 0) > SMA(200, 0)

// Price above 200-day moving average
Close(0) > SMA(200, 0)
```

---

## MACD

| Function | Description |
|----------|-------------|
| `MACD(offset [, series])` | MACD line (26-bar EMA minus 12-bar EMA) |
| `MACDD(short, long [, period, offset, series])` | **Customizable MACD histogram** (MACD minus signal line); when `period > 1` returns the histogram, when `period = 1` returns MACD |

**Note**: P123 does NOT have separate `MACDSig()` or `MACDHist()` functions. Use `MACDD()` instead:
- `MACDD(12, 26, 9)` → MACD histogram (signal period = 9)
- `MACDD(12, 26, 1)` → MACD line itself

```
// MACD histogram bullish
MACDD(12, 26, 9) > 0

// MACD histogram turning positive
MACDD(12, 26, 9) > 0 AND MACDD(12, 26, 9, 1) <= 0
```

---

## Oscillators

| Function | Description |
|----------|-------------|
| `RSI(period [, offset, series])` | **Relative Strength Index** (Wilder's, period in bars) |
| `RSI_D(days [, offset, series])` | RSI with period in calendar days |
| `RSI_W(weeks [, offset, series])` | RSI with period in weeks |
| `StochK(period [, smoothK, offset, series])` | Stochastic %K |
| `StochD(period, smoothK, smoothD [, offset, series])` | Stochastic %D |
| `CCI(bars [, offset, series])` | Commodity Channel Index |
| `ROC(bars [, offset, series])` | Rate of Change |
| `Momentum(bars [, offset, absolute])` | **Momentum** (amount price changed); `absolute` parameter controls output |
| `ULTOSC(offset)` | Ultimate Oscillator (Williams) |
| `FlipFlop(series, on_value, off_value [, initial_state])` | Flip-flop oscillator (custom on/off trigger) |

**Note**: P123 does NOT have `MOM()` or `WilliamsR()`. Use `Momentum()` and `ULTOSC()` respectively.

```
// RSI oversold
RSI(14) < 30

// Slow stochastic
StochK(14, 3) > StochD(14, 3, 3)
```

---

## Volatility & Risk

| Function | Description |
|----------|-------------|
| `BBUpper(period [, deviations, offset, series])` | Bollinger Band upper (default 2 std dev) |
| `BBLower(period [, deviations, offset, series])` | Bollinger Band lower |
| `ATR(bars [, offset, series])` | Average True Range |
| `ATRN(bars [, offset, series])` | Normalized ATR (as % of price) |
| `StdDev(bars [, offset, series])` | Standard deviation of returns |
| `PctDev(samples, bars [, offset, min_samples, annualize])` | Standard deviation of % moves |
| `PctAvg(samples, bars [, offset, min_samples, annualize])` | Average of % moves |
| `RelStdDev(...)` | Relative standard deviation |
| `BetaFunc(period, samples [, min_samples, offset, series])` | Beta calculation |
| `Correl(period, samples, series [, series2])` | Correlation coefficient |
| `Sharpe(range [, bars, offset])` | Sharpe-like ratio |
| `Sortino(range [, bars, offset])` | Sortino-like ratio |

**Pre-built volatility/risk factors:**

| Factor | Description |
|--------|-------------|
| `Beta1Y` / `Beta3Y` / `Beta5Y` | Pre-computed beta (1/3/5 year) |
| `Sharpe1Y` / `Sharpe2Y` | Pre-computed Sharpe ratio |
| `Sortino1Y` / `Sortino2Y` | Pre-computed Sortino ratio |
| `TRSD30D` / `TRSD60D` / `TRSD90D` | Total return std dev (30/60/90 day) |
| `TRSD1YD` / `TRSD3YD` / `TRSD5YD` | Total return std dev (1/3/5 year, daily) |
| `TRSD3YM` / `TRSD5YM` | Total return std dev (3/5 year, monthly) |

```
// Price near lower Bollinger Band
Close(0) < BBLower(20, 2)

// Custom beta (52-bar return periods, 104 samples)
// For weekly returns use period=5: BetaFunc(5, 52) = 1Y weekly beta
BetaFunc(52, 104)
```

---

## Trend & Directional Movement

| Function | Description |
|----------|-------------|
| `ADX(period, offset [, series])` | Average Directional Index |
| `DMIPlus(period, offset [, series])` | **DMI+ component** (directional indicator) |
| `DMIMinus(period, offset [, series])` | **DMI− component** |
| `DMICrossOver(period, bars [, series])` | TRUE if DMI+ crossed above DMI− |
| `DMICrossUnder(period, bars [, series])` | TRUE if DMI+ crossed below DMI− |
| `PrcRegEst(bars [, series])` | Price regression end value (incl div) |
| `PrcRegEst_W(bars [, series])` | Price regression end value (weekly) |
| `PrcRegEst10` / `PrcRegEst20` / `PrcRegEst50` | Pre-built regression estimates (10/20/50 day) |
| `PrcRegEst10W` / `PrcRegEst20W` / `PrcRegEst50W` | Weekly regression estimates |
| `SAR` | **Parabolic SAR** value (acceleration factor 0.02, max 0.20) |

**Note**: P123 uses `DMIPlus` / `DMIMinus`, NOT `PlusDI` / `MinusDI`.

```
// Strong uptrend
ADX(14, 0) > 25 AND DMIPlus(14, 0) > DMIMinus(14, 0)

// Price above Parabolic SAR
Close(0) > SAR
```

---

## Accumulation & Volume Indicators

| Function | Description |
|----------|-------------|
| `ChaikinAD(period [, offset])` | Chaikin Accumulation/Distribution |
| `ChaikinMFP(period, lookback [, offset])` | Chaikin Money Flow Persistence |
| `ChaikinTrend(bars [, offset, increment, series])` | Chaikin Trend |
| `OBV(offset)` | On-Balance Volume |
| `OBVSlopeN(offset, bars)` | Normalized rate of change of OBV |
| `UpDownRatio(bars, offset)` | Up/Down volume ratio |

---

## Price Extremes & Percent from Hi/Lo

| Function | Description |
|----------|-------------|
| `HighVal(period [, offset, series])` | **Highest value** in period |
| `LowVal(period [, offset, series])` | **Lowest value** in period |
| `HighValBar(period [, offset, series])` | Bars since highest value |
| `LowValBar(period [, offset, series])` | Bars since lowest value |
| `HighPct(bars [, series])` | **% from high** in period (incl div) |
| `LowPct(bars [, series])` | **% from low** in period (incl div) |
| `HighPct_W(weeks [, series])` | % from high (weekly series) |
| `LowPct_W(weeks [, series])` | % from low (weekly series) |

**Note**: P123 does NOT have `HiValue()`, `LoValue()`, `HiBar()`, `LoBar()`. Use `HighVal()`, `LowVal()`, `HighValBar()`, `LowValBar()`.

```
// Drawing down from 1-year peak
HighPct(252) < -20

// Close near 1-year high
Close(0) / HighVal(252) > 0.95
```

---

## Gap Patterns

| Function | Description |
|----------|-------------|
| `GapUp(GapPct, VolPct, bars, offset)` | TRUE if gap up occurred (with volume filter) |
| `GapDown(GapPct, VolPct, bars, offset)` | TRUE if gap down occurred |

---

## Streak Functions

**⚠️ Use `LoopStreak`, not `Streak`.** The correct function name is `LoopStreak` (see advanced-functions.md for full signature).

Streak modes: `#Positive`, `#NotPositive`, `#Increasing`, `#NotIncreasing`

```
// Price rising for 5+ consecutive days
LoopStreak("Close(0) - Close(CTR)", 20, 1, 1, #Positive) >= 5
```

---

## Strategy-Only Functions

These work only inside P123 strategy buy/sell rules.

### General (portfolio-level)

| Factor/Function | Description |
|-----------------|-------------|
| `PortBars` | Bars since strategy inception |
| `SimWeeks` | Weeks since strategy inception |
| `PortCash` | Cash amount in portfolio |
| `CashPct` | Cash as % of total market value |
| `TotMktVal` | Total portfolio value (incl cash) |
| `PosCnt` | Number of positions in portfolio |

### Position-level

| Factor/Function | Description |
|-----------------|-------------|
| `NoBars` | Trading days since position opened |
| `NoDays` | Calendar days since position opened |
| `GainPct` | **Return % of position** (excl dividends) |
| `Gain` | Dollar return of position |
| `PctFromHi` | **% decline from position's highest close** |
| `Weight` | Position weight as % of total market value |
| `BuyAmount` | Amount used to buy stock (before commissions/slippage) |
| `BenchPct` | Benchmark return since position opened |
| `BenchPctFromPosHi` | Benchmark % from position's high (for relative stops) |
| `LastSellPrice` | Price stock was last sold at, or NA |
| `LastSellDaysLT(days)` | TRUE if stock was sold within last N days |

### Ranking

| Factor/Function | Description |
|-----------------|-------------|
| `RankPrev(weeksAgo)` | Historical weekly rank |
| `RankPosPrev(weeksAgo)` | Historical weekly rank position |
| `Rating("RankName")` | Rank from a named ranking system |
| `RatingPos("RankName")` | Rank position from a named ranking system |
| `GetRank("ticker")` | Rank of a specific ticker |
| `GetRankPos("ticker")` | Rank position of a specific ticker |

### Diversification

| Factor/Function | Description |
|-----------------|-------------|
| `CountryCount` | Number of positions in same country |
| `IndCount` | Running count of stocks in same industry |
| `SubIndCount` | Running count in same sub-industry |
| `MaxCorrel(period_bars, pct_bars)` | Maximum correlation with other holdings |

```
// Trailing stop: sell if dropped 20% from peak
PctFromHi <= -20

// Time-based exit: sell after ~6 months
NoBars > 125

// Only enter stop-loss if market isn't also dropping
PctFromHi <= -20 AND BenchPctFromPosHi > -20
```

---

## Utility & Date Functions

| Function | Description |
|----------|-------------|
| `BarsSince(date)` | Bars since YYYYMMDD date |
| `DaysSince(date)` | Calendar days since date |
| `DaysDiff(from, to)` | Calendar days between two dates |
| `MonthBars(bars [, "cid"])` | TRUE if as-of date is within N bars of month-end |
| `Holiday(weekday)` | TRUE if weekday is a holiday |
| `GetSeries("name")` | Get a handle to a custom imported data series (use inside `Close()`, `SMA()`, etc. — see api-reference.md § Data Series) |
