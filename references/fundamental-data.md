# P123 Fundamental Data Reference

All fundamental items follow the same pattern:

```
FunctionName(offset, periodType [, NAHandling])
```
- **offset**: 0=most recent, 1=one period back, ... (0-20 for QTR, 0-10 for ANN)
- **periodType**: `QTR` (quarterly), `ANN` (annual), `TTM` (trailing 12 months)
- **NAHandling**: `FALLBACK` (default), `KEEPNA`, `ZERONA`

Each item also generates pre-built factors (no parentheses): see Pattern Guide at bottom.

---

## Balance Sheet — Current Assets

| Function | Description |
|----------|-------------|
| `Cash(offset, type)` | Cash only |
| `CashEquiv(offset, type)` | Cash and equivalents (cash + short-term investments) |
| `InvstST(offset, type)` | Short-term investments (not included in CashEquiv on some companies) |
| `Recvbl(offset, type)` | Accounts receivable (net of allowances) |
| `Inventory(offset, type)` | Inventories |
| `AstCurOther(offset, type)` | Other current assets |
| `AstCur(offset, type)` | **Total current assets** |
| `WorkCap(offset, type)` | Working capital (AstCur − LiabCur) |

## Balance Sheet — Non-Current Assets

| Function | Description |
|----------|-------------|
| `GrossPlant(offset, type)` | Gross property, plant & equipment |
| `AccumDep(offset, type)` | Accumulated depreciation |
| `NetPlant(offset, type)` | Net PP&E (GrossPlant − AccumDep) |
| `Goodwill(offset, type)` | Goodwill |
| `AstIntan(offset, type)` | Intangible assets |
| `IntanOther(offset, type)` | Other intangibles |
| `InvstEq(offset, type)` | Equity investments |
| `InvstAdvOther(offset, type)` | Investments and advances, other |
| `AstNonCurOther(offset, type)` | Other non-current assets |
| `AstTot(offset, type)` | **Total assets** |

## Balance Sheet — Current Liabilities

| Function | Description |
|----------|-------------|
| `Payables(offset, type)` | Accounts payable |
| `DbtST(offset, type)` | Short-term debt (due within 12 months) |
| `TxPayable(offset, type)` | Taxes payable |
| `LiabCurOther(offset, type)` | Other current liabilities |
| `LiabCur(offset, type)` | **Total current liabilities** |

## Balance Sheet — Non-Current Liabilities

| Function | Description |
|----------|-------------|
| `DbtLT(offset, type)` | Long-term debt |
| `DbtTot(offset, type)` | Total debt (short-term + long-term) |
| `CapLease(offset, type)` | Capital lease obligations |
| `TxDfdIC(offset, type)` | Deferred taxes and investment credits |
| `LiabNonCurOther(offset, type)` | Other non-current liabilities |
| `LiabTot(offset, type)` | **Total liabilities** |

## Balance Sheet — Shareholders' Equity

| Function | Description |
|----------|-------------|
| `ComEq(offset, type)` | Common equity |
| `BookVal(offset, type)` | Book value |
| `RetainedEarn(offset, type)` | Retained earnings |
| `PfdEquity(offset, type)` | Preferred equity |
| `CapSurplus(offset, type)` | Capital surplus (additional paid-in capital) |
| `NonControlInt(offset, type)` | Non-controlling interest (total) |
| `NonControlIntNonRed(offset, type)` | Non-controlling interest, non-redeemable |
| `NonControlIntRed(offset, type)` | Non-controlling interest, redeemable |
| `EqTot(offset, type)` | **Shareholder's equity total** |
| `TanBV(offset, type)` | Tangible book value (net tangible assets) |

## Balance Sheet — Shares

| Function | Description |
|----------|-------------|
| `Shares(offset, type)` | Common shares outstanding |
| `SharesFD(offset, type)` | Common shares fully diluted |
| `SharesCur(barsAgo)` | Common shares most recent (takes bars, not offset/type) |

---

## Income Statement

| Function | Description |
|----------|-------------|
| `Sales(offset, type)` | **Revenue / Net sales** |
| `SalesIntl(offset, type)` | International sales |
| `CostG(offset, type)` | Cost of goods sold |
| `CostG_GAAP(offset, type)` | Cost of goods sold (GAAP) |
| `GrossProfit(offset, type)` | Gross profit |
| `GrossProfit_GAAP(offset, type)` | Gross profit (GAAP) |
| `SGandA(offset, type)` | Selling, general & administrative expense |
| `SGandA_GAAP(offset, type)` | SG&A (GAAP) |
| `RandD(offset, type)` | Research & development expense |
| `OpInc(offset, type)` | **Operating income** |
| `OpIncBDepr(offset, type)` | Operating income before depreciation |
| `OpIncAftDepr(offset, type)` | Operating income after depreciation |
| `EBIT(offset, type)` | Earnings before interest and taxes |
| `EBITDA(offset, type)` | EBITDA |
| `IntExp(offset, type)` | Interest expense |
| `IntInc(offset, type)` | Interest income |
| `ExpNonOp(offset, type)` | Non-operating expenses |
| `IncBTax(offset, type)` | **Income before taxes (pre-tax income)** |
| `IncTaxExp(offset, type)` | Income tax expense |
| `IncAftTax(offset, type)` | **Income after taxes** |
| `IAC(offset, type)` | Income available to common |
| `NetIncBXor(offset, type)` | Net income before extraordinary items |
| `NetIncBXorNonC(offset, type)` | Net income before xor and non-controlling interest |
| `IncBXorForCom(offset, type)` | Income before xor items available for common |
| `IncBXorAdjCSE(offset, type)` | Income before xor items adjusted for common stock equivalents |
| `EPSExclXor(offset, type)` | **EPS excluding extraordinary items** |
| `EPSInclXor(offset, type)` | EPS including extraordinary items |
| `DivPS(offset, type)` | Dividends per share |
| `DepAmort(offset, type)` | Depreciation & amortization |
| `Amort(offset, type)` | Amortization of intangibles |
| `FundsFromOp(offset, type)` | Funds from operations (REITs) |
| `StkOptExp(offset, type)` | Stock option compensation expense |
| `PfdDiv(offset, type)` | Preferred dividends paid |
| `SpcItems(offset, type)` | Special items |
| `Impair(offset, type)` | Impairment charges |

---

## Cash Flow Statement

### Operating
| Function | Description |
|----------|-------------|
| `OperCashFl(offset, type)` | **Operating cash flow** |
| `NetIncCFStmt(offset, type)` | Net income (cash flow statement) |
| `DepAmortCF(offset, type)` | Depreciation & amortization (cash flow line) |
| `TxDfd(offset, type)` | Deferred taxes |
| `StkOptCF(offset, type)` | Stock option compensation (cash flow line) |
| `RecvblChg(offset, type)` | Change in accounts receivables |
| `InvtyChg(offset, type)` | Change in inventories |
| `PayablesChg(offset, type)` | Change in accounts payable |
| `AccruedExp(offset, type)` | Change in accrued liabilities (accrued expenses) |
| `TxAcrudChg(offset, type)` | Change in accrued income taxes |
| `OtherWCChg(offset, type)` | Change in other working capital lines |

### Investing
| Function | Description |
|----------|-------------|
| `CapEx(offset, type)` | **Capital expenditures** |
| `CashFrInvest(offset, type)` | Cash from investing |
| `Acquis(offset, type)` | Acquisitions |
| `Divest(offset, type)` | Divestitures |
| `InvstOther(offset, type)` | Other investing cash flow |

### Financing
| Function | Description |
|----------|-------------|
| `CashFrFin(offset, type)` | Cash from financing |
| `ChangeDebt(offset, type)` | Change in debt (net) |
| `ChangeEq(offset, type)` | Change in equity (net) |
| `DbtLTIssued(offset, type)` | Long-term debt issued |
| `DbtLTReduced(offset, type)` | Long-term debt reduced |
| `EqIssued(offset, type)` | Equity issued |
| `EqPurch(offset, type)` | Equity purchased (buybacks) |
| `DivPaid(offset, type)` | Dividends paid (total) |

### Summary
| Function | Description |
|----------|-------------|
| `CashFl(offset, type)` | Total cash flow |
| `FCF(offset, type)` | **Free cash flow** (OperCashFl − CapEx) |
| `NetFCF(offset, type)` | Net free cash flow (OperCashFl − CapEx − DivPaid) |
| `NetChgCash(offset, type)` | Net change in cash position |

---

## Key Pre-Built Ratios & Statistics

### Valuation

| Factor | Function | Description |
|--------|----------|-------------|
| `PEExclXorTTM` | `PEExclXor(offset, type)` | **P/E excluding extraordinary items** |
| `PEInclXorTTM` | `PEInclXor(offset, type)` | P/E including extraordinary items |
| `PEInclRDTTM` | `PEInclRD(offset, type)` | Innovation P/E (incl R&D capitalized) |
| `Pr2BookQ` | `Pr2Book(offset, type)` | **Price to book** |
| `Pr2SalesTTM` | `Pr2Sales(offset, type)` | **Price to sales** |
| `Pr2CashFlTTM` | `Pr2CashFl(offset, type)` | Price to cash flow |
| `Pr2FrCashFlTTM` | `Pr2FrCashFl(offset, type)` | **Price to free cash flow** |
| `Pr2NetFrCashFlTTM` | `Pr2NetFrCashFl(offset, type)` | Price to net free cash flow |
| `Pr2TanBkQ` | `Pr2TanBk(offset, type)` | Price to tangible book value |
| `EV2EBITDATTM` | `EV2EBITDA(offset, type)` | **EV/EBITDA** |
| `EV2SalesTTM` | `EV2Sales(offset, type)` | EV/Sales |

### Yield

| Factor | Description |
|--------|-------------|
| `Yield` | **Dividend yield** |
| `EarnYield` | Earnings yield (inverse of PE) |
| `ShareholderYield` | Shareholder yield (dividends + buybacks) |
| `EBITDAYield` | EBITDA yield |
| `FCFYield` | Free cash flow yield |
| `OCFYield` | Operating cash flow yield |
| `OpIncYield` | Operating income yield |

### Dividends

| Factor / Function | Description |
|--------|-------------|
| `DivPSTTM` / `DivPS52W` | Dividends per share (TTM / 52-week) |
| `DivPSNextQ` / `DivPSNextQCnt` | Next quarter dividend estimate / count |
| `IAD` / `IAD13W` / `IAD26W` / `IAD52W` | Indicated annual dividend (current / 13/26/52 week) |
| `Div%ChgA` / `Div%ChgTTM` / `Div%ChgPYQ` | Dividend growth (annual / TTM / YoY quarter) |
| `DaysFromDivPay` / `DaysToDivEx` / `DaysToDivPay` | Dividend calendar |
| `DaysFromMergerAnn` | Days since merger announcement |

### Actuals (Analyst-sourced, from press releases/brokers)

| Function | Description |
|--------|-------------|
| `Actual(item, offset, type)` | Generic actual function |
| `EPSActual(offset, type)` / `EPSActualTTM` | EPS actuals |
| `SalesActual(offset, type)` / `SalesActualTTM` | Sales actuals |
| `EBITDAActual(offset, type)` / `EBITDAActualTTM` | EBITDA actuals |
| `ActualQ(item)` / `ActualPQ(item)` / `ActualTTM(item)` / `ActualA(item)` | Period shortcuts |
| `ActualGr%TTM(item)` / `ActualGr%PYQ(item)` | Growth rates |
| `LatestActualDays` / `LatestActualPeriodDate` | Timing |

Items: `#EPS`, `#EPS_GAAP`, `#SALES`, `#EBITDA`, `#EBIT`, `#NET`, `#PTI`, `#FCF`, `#FFO`, `#CAPX`, `#SHS_REPURCH`, `#SOE`

### Margins

| Factor | Function | Description |
|--------|----------|-------------|
| `GMgn%TTM` | `GMgn%(offset, type)` | **Gross margin** |
| `GMgn%_GAAPTTM` | `GMgn%_GAAP(offset, type)` | Gross margin (GAAP) |
| `OpMgn%TTM` | `OpMgn%(offset, type)` | **Operating margin** |
| `NPMgn%TTM` | `NPMgn%(offset, type)` | **Net profit margin** |
| `PTMgn%TTM` | `PTMgn%(offset, type)` | Pre-tax margin |
| `EBITDAMgn%TTM` | `EBITDAMgn%(offset, type)` | EBITDA margin |
| `FCFLMgn%TTM` | `FCFLMgn%(offset, type)` | Free cash flow margin |
| `NetFCFLMgn%TTM` | `NetFCFLMgn%(offset, type)` | Net free cash flow margin |
| `SGA2Sales%TTM` | `SGA2Sales%(offset, type)` | SG&A as % of sales |

### Profitability

| Factor | Function | Description |
|--------|----------|-------------|
| `ROE%TTM` | `ROE%(offset, type)` | **Return on equity** |
| `ROA%TTM` | `ROA%(offset, type)` | **Return on assets** |
| `ROI%TTM` | `ROI%(offset, type)` | Return on investment |

### Per Share Ratios

| Factor | Function | Description |
|--------|----------|-------------|
| `BVPSTTM` | `BVPS(offset, type)` | Book value per share |
| `CashPSTTM` | `CashPS(offset, type)` | Cash per share |
| `SalesPSTTM` | `SalesPS(offset, type)` | Sales per share |
| `CashFlPSTTM` | `CashFlPS(offset, type)` | Cash flow per share |
| `FCFPSTTM` | `FCFPS(offset, type)` | Free cash flow per share |
| `NetFCFPSTTM` | `NetFCFPS(offset, type)` | Net free cash flow per share |
| `OCFPSTTM` | `OCFPS(offset, type)` | Operating cash flow per share |
| `OpIncPSTTM` | `OpIncPS(offset, type)` | Operating income per share |
| `CapExPSTTM` | `CapExPS(offset, type)` | Capital expenditures per share |
| `EBITDAPSTTM` | `EBITDAPS(offset, type)` | EBITDA per share |

### Efficiency

| Factor | Function | Description |
|--------|----------|-------------|
| `AstTurnTTM` | `AstTurn(offset, type)` | Asset turnover |
| `InvTurnTTM` | `InvTurn(offset, type)` | Inventory turnover |
| `RecTurnTTM` | `RecTurn(offset, type)` | Receivables turnover |
| `IncPerEmpTTM` | `IncPerEmp(offset, type)` | Income per employee |
| `SalesPerEmpTTM` | `SalesPerEmp(offset, type)` | Sales per employee |
| `SGA2GPTTM` | `SGA2GP(offset, type)` | SG&A to gross profit |

### Financial Strength

| Factor | Function | Description |
|--------|----------|-------------|
| `CurRatioQ` | `CurRatio(offset, type)` | Current ratio |
| `QuickRatioQ` | `QuickRatio(offset, type)` | Quick ratio |
| `DbtTot2EqQ` | `DbtTot2Eq(offset, type)` | **Total debt to equity** |
| `DbtTot2AstQ` | `DbtTot2Ast(offset, type)` | Total debt to assets |
| `DbtTot2CapQ` | `DbtTot2Cap(offset, type)` | Total debt to capital |
| `DbtLT2EqQ` | `DbtLT2Eq(offset, type)` | Long-term debt to equity |
| `DbtLT2AstQ` | `DbtLT2Ast(offset, type)` | Long-term debt to assets |
| `DbtLT2CapQ` | `DbtLT2Cap(offset, type)` | Long-term debt to capital |
| `IntCovTTM` | `IntCov(offset, type)` | Interest coverage |
| `DbtS2NITTM` | `DbtS2NI(offset, type)` | Debt service to net income |
| `NI2CapExTTM` | `NI2CapEx(offset, type)` | Net income to capital expenditures |
| `PayRatioTTM` | `PayRatio(offset, type)` | Payout ratio |
| `Retn%TTM` | `Retn%(offset, type)` | Retention rate |
| `DepAmort2GPTTM` | `DepAmort2GP(offset, type)` | Depreciation & amort to gross profit |

### Advanced Scores

| Factor | Description |
|--------|-------------|
| `AltmanZOrig` | Original Altman Z-score (cutoff ≥1.81) |
| `AltmanZPriv` | Private-firm Z-score (cutoff ≥1.23) |
| `AltmanZNonManu` | Non-manufacturing Z-score (cutoff ≥1.10) |
| `AltmanX1`–`AltmanX5` | Z-score components (WorkCap/Assets, RetEarn/Assets, EBIT/Assets, MktCap/Liab, Sales/Assets) |
| `AltmanX4Rev` | Revised X4 (book equity / liabilities, for private firms) |
| `PiotFScore` | Piotroski F-score (financial strength, 0-9) |
| `BeneishMScore` | Beneish M-score (earnings manipulation detector) |
| `MScoreDSRI` / `MScoreGMI` / `MScoreAQI` / `MScoreSGI` / `MScoreDEPI` | M-score components |
| `MScoreSGAI` / `MScoreLVGI` / `MScoreTATA` / `MScoreDEPAMI` | M-score components (continued) |
| `ValROETTM` | Valuation-adjusted ROE |
| `EPS#Positive` / `EPSStableQ` | Earnings quality |
| `NoPosEPS5Q` / `NoPosEBITDA5Y` / `NoIncP4YN2Y` | Positive period counts |
| `SusGr%` | Sustainable growth rate |
| `EVPS` | Enterprise value per share |
| `PEHigh` / `PELow` / `PERelative` | PE range and relative PE |
| `PayRatio5Y` | 5-year payout ratio |
| `WCapPS2PrA` / `WCapPS2PrQ` | Working capital per share to price |

### Other

| Factor | Description |
|--------|-------------|
| `MktCap` | **Market capitalization** |
| `EV` | Enterprise value |
| `Float` | Shares float |
| `Beta` | Beta (from `BetaFunc()`) |

### Market / Price

| Factor | Description |
|--------|-------------|
| `AvgDailyTot(N)` | Avg daily dollar volume over N bars, in **plain dollars** ($100K = `100000`, not thousands) |

### Estimates

| Factor | Description |
|--------|-------------|
| `EstEPSCQ` | EPS estimate current quarter |
| `EstEPSNQ` | EPS estimate next quarter |
| `EstEPSCY` | EPS estimate current year |
| `EstEPSNY` | EPS estimate next year |
| `EstEPSGr%NQ` | Estimated EPS growth next quarter |
| `EstEPSGr%NY` | Estimated EPS growth next year |
| `NumEstEPSCQ` | Number of analyst estimates CQ |
| `Surprise%Q1` | EPS surprise % current quarter |
| `SumRevisions` | Sum of EPS revisions |
| `EstSalesCY` | Sales estimate current year |
| `EstSalesNY` | Sales estimate next year |
| `AvgRec` | Average analyst recommendation (1=Strong Buy to 5=Strong Sell) |
| `LTGrRt` | Long-term EPS growth rate estimate |
| `FY2EPSMean` / `FY3EPSMean` | EPS estimate FY+2 / FY+3 |
| `EstSalesCY` / `EstSalesNY` | Sales estimate current/next year |
| `NTMSalesMean` / `FY2SalesMean` / `FY3SalesMean` | Sales estimates NTM / FY+2 / FY+3 |
| `CapExEstCY` / `CapExEstNY` | CapEx estimate current/next year |
| `EBITDAEstCY` / `EBITDAEstNY` | EBITDA estimate current/next year |
| `FCFEstCY` / `FCFEstNY` | FCF estimate current/next year |
| `#AnalystsCurQ` / `#AnalystsCurFY` / `#AnalystsNextQ` / `#AnalystsNextFY` | Analyst count |
| `#AnalystsCurFYSales` / `#AnalystsNextFYSales` | Sales analyst count |
| `#AnalystsLTGrthRt` / `#AnalystsPriceTarget` | LT growth / price target analyst count |
| `PriceTargetMean` / `PriceTargetHi` / `PriceTargetLo` / `PriceTargetStdDev` | Price targets |
| `ProjPECurFY` / `ProjPENextFY` / `ProjPENTM` | Forward PE ratios |
| `Pr2SalesNTM` | Price to NTM sales |
| `PEGLT` / `PEGST` / `PEGLTY` / `PEGSTY` | PEG ratios (LT/ST, with/without yield) |
| `AvgRec` / `AvgRec1WkAgo` / `AvgRec4WkAgo` / `AvgRec8WkAgo` / `AvgRec13WkAgo` | Recommendation history |
| `CurQUpRevLastWk` / `CurQDnRevLastWk` / `CurFYUpRevLastWk` / `CurFYDnRevLastWk` | Revision counts (last week) |
| `CurQUpRev4WkAgo` / `CurQDnRev4WkAgo` / `CurFYUpRev4WkAgo` / `CurFYDnRev4WkAgo` | Revision counts (4 weeks ago) |
| `NextQUpRevLastWk` / `NextQDnRevLastWk` / `NextFYUpRevLastWk` / `NextFYDnRevLastWk` | Next period revisions |
| `TotRevisions4W` / `TotRevisionsLastW` | Total revision counts |
| `Surprise%Q1`–`Surprise%Q5` / `Surprise%Y1`–`Surprise%Y4` | EPS surprise history |
| `SalesSurp%Q1`–`SalesSurp%Q5` / `SalesSurp%Y1`–`SalesSurp%Y4` | Sales surprise history |
| `SUEQ1`–`SUEQ4` / `SUEY1`–`SUEY4` | Standardized unexpected earnings |
| `SUSQ1`–`SUSQ4` / `SUSY1`–`SUSY4` | Standardized unexpected sales |

### Insider & Institutional

Names verified against official P123 syntax reference (2026-03-21).

**Insider (pre-built factors):**

| Factor | Description |
|--------|-------------|
| `Insider#Own` | Number of shares owned by insiders |
| `Insider%Own` | Insider ownership as % |
| `InsiderBuySh1M(offset)` – `InsiderBuySh12M(offset)` | Shares bought (1M/3M/6M/12M windows) |
| `InsiderSellSh1M(offset)` – `InsiderSellSh12M(offset)` | Shares sold |
| `InsiderBuyTran1M(offset)` – `InsiderBuyTran12M(offset)` | Buy transactions |
| `InsiderSellTran1M(offset)` – `InsiderSellTran12M(offset)` | Sell transactions |
| `InsiderUniqBuy1M(offset)` / `InsiderUniqBuy3M(offset)` | Unique insiders buying |
| `InsiderUniqSell1M(offset)` / `InsiderUniqSell3M(offset)` | Unique insiders selling |

**Institutional:**

| Factor / Function | Description |
|--------|-------------|
| `Inst%Own` / `Inst%OwnPQ` | Institutional ownership % (current / prev quarter) |
| `Inst#ShsOwn` / `Inst#ShsOwnPQ` | Shares owned |
| `Inst#ShsPurch` / `Inst#ShsSold` | Shares purchased / sold |
| `InstNetPurch` / `InstNetPurchPQ` | Net purchases |
| `InstitutionalPctOwn(offset)` | % of shares owned (function form) |
| `InstitutionalPctChg(offset)` | Net shares changed % |
| `InstitutionalHolders(offset)` | Total institutional holders |
| `InstitutionalBuyers(offset)` / `InstitutionalSellers(offset)` | Buyers / sellers count |
| `InstitutionalShsHeld(offset)` / `InstitutionalShsBought(offset)` / `InstitutionalShsSold(offset)` / `InstitutionalShsNet(offset)` | Share counts |

**Short Interest:**

| Factor | Description |
|--------|-------------|
| `SI%Float` / `SI%FloatPM` / `SI%FloatPM2` / `SI%FloatPM3` | Short interest as % of float (current + history) |
| `SI%ShsOut` / `SI%ShsOutPM` / `SI%ShsOutPM2` / `SI%ShsOutPM3` | Short interest as % of shares outstanding |
| `SIRatio` / `SIRatioPM` / `SIRatioPM2` / `SIRatioPM3` | Days to cover |
| `SIPM` / `SIPM2` / `SIPM3` / `SICM` | Short interest volume (prev months / current) |
| `SI1Mo%Chg` | Short interest 1-month % change |

**⚠️ Common hallucinations (NOT valid P123 names):** `InsiderBuySell%3Mo`, `InsiderBuySell%6Mo`, `InstOwn%`, `InstOwnChg%QoQ`, `ShortInt%Float`, `ShortInt%ShOut`, `ShortIntRatio`, `SI%ShOut` (correct: `SI%ShsOut`), `SI%Chg` (correct: `SI1Mo%Chg`).

For detailed usage examples, see `advanced-functions.md`.

---

## Pre-Built Factor Naming Pattern

For any base function `Item`, P123 auto-generates factors:

| Suffix | Meaning | Example |
|--------|---------|---------|
| `Q` | Most recent quarter | `SalesQ` |
| `A` | Most recent annual | `SalesA` |
| `TTM` | Trailing 12 months | `SalesTTM` |
| `PQ` | Previous quarter | `SalesPQ` |
| `PY` | Previous year | `SalesPY` |
| `PYQ` | Previous year same quarter | `SalesPYQ` |
| `PTM` | Previous trailing 12 months | `SalesPTM` |
| `Gr%TTM` | Growth TTM vs prior TTM | `SalesGr%TTM` |
| `Gr%A` | Growth annual vs prior annual | `SalesGr%A` |
| `Gr%PQ` | Growth QoQ | `SalesGr%PQ` |
| `Gr%PYQ` | Growth Q vs prior year Q (YoY) | `SalesGr%PYQ` |
| `Gr%PQTTM` | Growth in TTM vs prior Q TTM | `SalesGr%PQTTM` |
| `Gr%3Y` | 3-year CAGR | `SalesGr%3Y` |
| `Gr%5Y` | 5-year CAGR | `SalesGr%5Y` |
| `Gr%10Y` | 10-year CAGR | `SalesGr%10Y` |
| `PSA` | Per share (annual) | `SalesPSA` |
| `PSQ` | Per share (quarterly) | `SalesPSQ` |
| `%AssetsA` | As % of assets (annual) | `Sales%AssetsA` |
| `%AssetsQ` | As % of assets (quarterly) | `Sales%AssetsQ` |
| `%SalesA` | As % of sales (annual) | `CapEx%SalesA` |
| `%SalesQ` | As % of sales (quarterly) | `CapEx%SalesQ` |
| `3YAvg` | 3-year average | `Sales3YAvg` |
| `5YAvg` | 5-year average | `Sales5YAvg` |
| `RegGr%ANN` | Regression growth (annual) | `SalesRegGr%ANN` |
| `RegGr%TTM` | Regression growth (TTM) | `SalesRegGr%TTM` |
| `RSD%ANN` | Regression residual std dev (annual) | `SalesRSD%ANN` |
| `RSD%TTM` | Regression residual std dev (TTM) | `SalesRSD%TTM` |
| `RegEstANN` | Regression estimate (annual) | `SalesRegEstANN` |
| `RegEstTTM` | Regression estimate (TTM) | `SalesRegEstTTM` |

---

## Company / Classification

| Factor | Description |
|--------|-------------|
| `Sector` | GICS sector code (10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60) |
| `Industry` | GICS industry code (8-digit) |
| `SubIndustry` | GICS sub-industry code |
| `SectorCode` / `SectorDescr` | GICS sector (code / text name) |
| `SubSector` / `SubSectorCode` / `SubSectorDescr` | GICS sub-sector |
| `IndCode` / `IndDescr` | GICS industry (code / text) |
| `SubIndCode` / `SubIndDescr` | GICS sub-industry (code / text) |
| `CountryCode` | ISO country code |
| `ExchCountry("cid")` | Exchange country filter (function) |
| `Country("cid")` | Country of domicile filter (function) |
| `ExchangeCode` | Exchange code |
| `SecurityType` | Security type |
| `IsADR` / `IsMLP` / `IsOTC` / `IsPrimary` | Boolean flags |
| `StockID` / `EvenID` / `ccFIGI` / `scFIGI` | Identifiers |
| `FYEndMonth` | Fiscal year end month (1-12) |
| `NoEmp` | Number of employees |
| `CoName("name")` | Match company name (returns TRUE/FALSE) |
| `Ticker("AAPL")` | Match ticker |
| `FIGI("figi")` | Match FIGI identifier |
| `#APeriods` / `#QPeriods` | Number of historical periods available |
| `FloatPct` | Float as % of shares outstanding |
| `DaysLate` | Days since filing was expected |
| `WeeksIntoQ` / `WeeksToQ` / `WeeksToY` | Calendar position |
| `LatestFilingDate` / `LatestPeriodDate` / `LatestNewsDate` | Key dates |
| `PeriodDateA` / `PeriodDateQ` | Period end dates (YYYYMMDD) |
| `CompleteStmt` / `QtrComplete` / `StaleStmt` | Filing completeness flags |
| `AsOfDate` / `MonthDay` / `WeekDay` / `PrevBarDaysAgo` | Date utilities |
| `FXRate` | FX rate to USD |

**GICS Sectors**: 10=Energy, 15=Materials, 20=Industrials, 25=Consumer Disc, 30=Consumer Staples, 35=Health Care, 40=Financials, 45=Info Tech, 50=Communication, 55=Utilities, 60=Real Estate
