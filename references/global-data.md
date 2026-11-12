# Global Data (Europe, APAC & Emerging) - Portfolio123 Reference

<!-- name-whitelist: PRIMARYGLOBAL PRIMARYAPEM ALLFUNDEUR ALLFUNDOLD ALLSTOCKSOLD TradesIn NewOct2026
FXRate IsUnlisted IsBDC JPY AUD HKD KRW INR CNY TWD SGD NZD BRL ILS MXN DKK IDR VND MYR THB PHP ZAR -->

Portfolio123 went from US/Canada/Europe coverage to global coverage in autumn 2026: a new security
master in October, then APAC & Emerging Markets data in production from 31 October to 2 November 2026.
The 2026-06-09 Factor Reference extraction behind the other files predates all of this, and so does
P123's own `Universe()` page, which still lists only the North American and European universes. This
file records what changed and what was **verified on 2026-11-12**: on the live API (mostly calls
that end in an error, so they cost nothing, plus a few member counts) and in the P123 web app, whose
86 P123 Universes and their rules were read one by one. Where a statement comes from a P123 forum
announcement instead, it says so.

Read this file for any non-US work: universes outside the US, country filters, currencies, ticker
formats, and the data gaps that make some factors NA outside North America.

## Contents

- [Timeline](#timeline)
- [Regions and subscriptions](#regions-and-subscriptions)
- [Universes](#universes)
- [Probing a universe ID for free](#probing-a-universe-id-for-free)
- [Primary, foreign primary and tickers](#primary-foreign-primary-and-tickers)
- [Country and listing functions](#country-and-listing-functions)
- [Currencies, regions and FX in the API](#currencies-regions-and-fx-in-the-api)
- [Data gaps and caveats](#data-gaps-and-caveats)
- [Common Mistakes](#common-mistakes)
- [Sources](#sources)

---

## Timeline

| Date | Event (source: P123 forum announcements unless marked) |
|---|---|
| 2026-08-24 | Global beta opens on a separate site and database; no API on the beta. |
| 2026-10-10/11 | **Security master release in production** (North America and Europe only). The primary issue of each company is now the vendor's (FactSet's) choice. About 2% of stocks moved to another primary universe (example: `SHOP:USA` replaced by `SHOP:CAN`); Canada (Primary) grew from about 1,650 to 2,350 stocks and United States (Primary) from about 6,200 to 6,650. `ALLFUND` and `ALLSTOCKS` were redefined (13,184 and 14,420 stocks on 2026-11-07); the old definitions stay available as `ALLFUNDOLD` (9,296) and `ALLSTOCKSOLD` (10,220), and P123's US P123 Universes (Prussell, cap tiers, exchanges) still use them (measured, not announced). Europe's foreign-primary universe was retired. Dividend functions with `#AnnDate` now fall back to `#ExDate`. Identical backtests return slightly different numbers from this date on; P123 says small differences will keep appearing. |
| 2026-10-16 | `TradesIn()` made point-in-time. |
| 2026-10-31 to 11-02 | **Global Phase 1 in production**: APAC & Emerging added. The Global universe holds over 43,000 current and over 159,000 historical stocks. API and AI Factor work for all regions. New benchmarks for APAC & EM and Global. |
| to 2026-11-30 | All three regions are free for every paying membership (other plan limits, such as years of history and API credits, still apply). |
| Dec 2026 (Phase 2, planned) | Fix the missing cash-flow data (see [Data gaps](#data-gaps-and-caveats)); recover inactive stocks lost because they changed primary issue or moved region; three data updates a day instead of two. |
| Q1 2027 (Phase 3, planned) | Ownership data for Europe and APAC & EM; map non-primary issues held in strategies to the primary issue in rebalances. |

## Regions and subscriptions

Data is sold by region: **North America** (US and Canada), **Europe**, and **APAC & Emerging**.
A universe outside your regions fails with HTTP 400 `Universe <X> is not in your subscribed
regions`. Until 2026-11-30 every paying account has all three regions; after that date expect
this error for regions the subscription does not include. Linked broker trading (IBKR) for APAC/EM
was not planned as of 2026-10-28.

## Universes

P123 has two kinds of universe. **Preset** universes are the base sets (a region, a country, all
listings). **P123 Universes** are rules on top of a preset, listed in the web app under
Universes → P123 Universes (86 of them on 2026-11-12, all read for this file). Both are addressed
the same way in the API.

**How to address a universe in the API (verified 2026-11-12):**

- A **short ID** (`PRIMARYGLOBAL`, `ALLFUND`, `Prussell3000`) where one exists.
- Otherwise the **exact display name** as the app shows it (`'Japan (Primary)'`). Matching ignores
  case (`'China A-Shares'` finds `China A-shares`) but not spelling: P123 spells
  `'Malasya (Primary)'`, and `'Malaysia (Primary)'` is not found.
- **Not** the numeric ID from the app's URL (`/app/universe/summary/345796`): `'345796'` is not found.
- A short ID and its display name return identical members (checked for `ALLFUND`, `ALLFUNDOLD`,
  `ALLSTOCKS`, `ALLSTOCKSOLD`, `Prussell3000`, `MicroCap`).
- 15 of the 86 P123 Universes do **not** resolve by name: the 13 `Sector ...` universes,
  `P123 Invest` and `S&P 500 Sector ETFs`. Rebuild them from their rules (below) on the base universe.
- ETF universes need `'type': 'ETF'` in the request.

### Preset universes

| Short ID | Display name | Notes (members on 2026-11-07 where counted) |
|---|---|---|
| `PRIMARYGLOBAL` | Global (Primary) | All regions, primary issues |
| `PRIMARYAPEM` | APAC & Emerging (Primary) | Base of the APAC/EM P123 Universes |
| `PRIMARYNOAT` | North Atlantic (Primary) | North America + Europe |
| `PRIMARYNOAM` | North America (Primary) | US + Canada |
| `PRIMARYEUR` | Europe (Primary) | Base of the European P123 Universes |
| `PRIMARYUSA` | United States (Primary) | 6,646 stocks. |
| `ALLFUND` | United States (Incl. Foreign Primary) | 13,184 stocks. **New definition since October 2026.** US listings including foreign primaries and ADRs |
| `ALLFUNDOLD` | United States-OLD (Incl. Foreign Primary) | 9,296 stocks. The pre-October definition, kept. **Base of P123's US P123 Universes** (Prussell, cap tiers, exchanges, Easy to Trade US) |
| `ALLSTOCKS` | United States (All Listed Stocks) | 14,420 stocks. New definition |
| `ALLSTOCKSOLD` | United States-OLD (All Listed Stocks) | 10,220 stocks. Pre-October definition; base of ADR, MLP and OTC universes |
| `PRIMARYCAN` | Canada (Primary) |  |
| `ALLFUNDCAN` | Canada (Incl. Foreign Primary) | Base of TSX, TSXV, CanadaTrust |
| `ALLSTOCKSCAN` | Canada (All Listed Stocks) |  |
| `ALLFUNDCDRCAD` | Canada (Incl. Foreign Primary and CDRs) |  |
| - | ETFs (United States) | ETF base; use `'type': 'ETF'` |

The ETF-based and index universes of the US remain: `SP500`, `SP400`, `SP600`, `SP1500`, `DJIA`,
`NASDAQ100` (see [misc.md](misc.md#universe-ids)).

**ALLFUND changed under you.** Since the October security master, `ALLFUND` holds about 3,900 more
stocks than `ALLFUNDOLD`, which keeps the old definition (13,184 against 9,296), and P123's own US
P123 Universes still sit on `ALLFUNDOLD`. A system on `ALLFUND` built before October is now testing a different universe; use
`ALLFUNDOLD` to reproduce old work, and `PRIMARYUSA` for new research.

### P123 Universes: countries

Unless stated, each is `ExchCountry("<code>")` plus `MedianDailyTot(20) > 5000` (at least $5,000
median daily dollar volume, in the system currency) on the base shown.

| Display name | Rule country | Base |
|---|---|---|
| Austria (Primary), Belgium (Primary), Denmark (Primary), Finland (Primary), France (Primary), Germany (Primary), Ireland (Primary), Italy (Primary), Netherlands (Primary), Norway (Primary), Portugal (Primary), Spain (Primary), Sweden (Primary), Switzerland (Primary), United Kingdom (Primary) | AUT, BEL, DNK, FIN, FRA, DEU, IRL, ITA, NLD, NOR, PRT, ESP, SWE, CHE, GBR | `PRIMARYEUR` |
| Australia (Primary), Brazil (Primary), Hong Kong (Primary), Israel (Primary), Japan (Primary), New Zealand (Primary), Singapore (Primary), South Korea (Primary), Taiwan (Primary), Vietnam (Primary) | AUS, BRA, HKG, ISR, JPN, NZL, SGP, KOR, TWN, VNM | `PRIMARYAPEM` |
| **Malasya (Primary)** (P123's spelling) | MYS | `PRIMARYAPEM` |
| **Indonesia (Primary)** | **IND - that is India** | `PRIMARYAPEM` |
| China A-shares | `TradesIn("CHN")` (any issue trading in mainland China) | `PRIMARYAPEM` |

**P123 bug (2026-11-12): "Indonesia (Primary)" holds Indian stocks.** Its rule is
`ExchCountry("IND")`, India's code; Indonesia is `IDN`. A screen on it returned 1,770 stocks, all
with the `:IND` suffix. There is no Indonesia universe and no India universe under its own name: for
India use `'Indonesia (Primary)'` knowingly or `PRIMARYAPEM` with `ExchCountry("IND")`; for Indonesia,
`PRIMARYAPEM` with `ExchCountry("IDN")`. Re-check before relying on it, P123 may fix the rule.

No country universe exists for China (other than China A-shares), India, Indonesia, Thailand,
Philippines, Mexico, Chile, South Africa, Turkey, Saudi Arabia, Poland or Greece: use the regional
universes below or a preset with `ExchCountry()`.

### P123 Universes: regions and "Trades In"

Same pattern: `ExchCountry(...)` or `TradesIn(...)` plus `MedianDailyTot(20) > 5000`.

| Display name | Countries | Base |
|---|---|---|
| Developed APAC | AUS HKG JPN NZL SGP | `PRIMARYAPEM` |
| Emerging APAC | CHN IDN IND KOR MYS PHL THA TWN | `PRIMARYAPEM` |
| Emerging LATAM | BRA CHL COL MEX PER | `PRIMARYAPEM` |
| World Frontier | ARG BGD BGR BHR BWA CIV EST GHA HRV ISL JAM JOR KAZ KEN LKA LTU MAR MLT MUS MWI NAM NGA OMN PAK PAN ROU SRB SVN TZA UGA VNM | `PRIMARYAPEM` |
| Developed Europe + Israel | AUT BEL CHE DEU DNK ESP FIN FRA GBR IRL ISR ITA NLD NOR PRT SWE | `PRIMARYGLOBAL` |
| Emerging EMEA | ARE CZE EGY GRC HUN KWT POL QAT SAU TUR ZAF | `PRIMARYGLOBAL` |
| Trades In Canada / Germany / Norway / United States | `TradesIn("CAN")` / `("DEU")` / `("NOR")` / `("USA")`: companies with any issue trading there, wherever their primary listing is | `PRIMARYGLOBAL` |

Note the overlaps: Korea and Taiwan count as Emerging APAC, Israel sits in both Israel (Primary) and
Developed Europe + Israel, and Vietnam is in both Vietnam (Primary) and World Frontier.

### P123 Universes: Europe and North America

| Display name | Short ID | Rules | Base |
|---|---|---|---|
| Europe 600 | `EU600` | `Country(...)` with 17 European codes (domicile, not exchange); `FOrder("MktCap", #Previous) <= 600` | `PRIMARYEUR` |
| Europe 200 Large / Mid / Small | `EU200L` / `EU200M` / `EU200S` | Same countries; market-cap order 1-200 / 201-400 / 401-600 | `PRIMARYEUR` |
| Easy to Trade Europe | `TRADEEUR` | `Price > 0.5`; `MedianDailyTot(126) > 10000` | `PRIMARYEUR` |
| Easy to Trade North America | `TRADENOAM` | `Price > 3`; `!IsMLP & !IsOTC & !IsUnlisted`; `MedianDailyTot(126) > 50000` | `PRIMARYNOAM` |
| Easy to Trade North Atlantic | `TRADENOAT` | As North America for US/Canada listings, `Price > 0.5` and `MedianDailyTot(126) > 10000` elsewhere | `PRIMARYNOAT` |
| Easy to Trade US | `TRADEUSA` | As North America | `ALLFUNDOLD` |
| Prussell 1000 / 2000 / 3000 | `Prussell1000` / `2000` / `3000` | `!IsMLP`, `!IsADR`, no OTC (`!InSet(ExchangeCode, 13, 19)`), BDCs excluded from 2014-06-28, `Close(0) >= 1`, `Country("USA,CYM,BMU,BHS")`, `FOrder("MktCap", #Previous)` 1-1000 / 1001-3000 / 1-3000 | `ALLFUNDOLD` |
| Prussell 1000 / 2000 / 3000 (OLD) | - | Same without the BDC rule (an older P123 definition, unrelated to the October change); 2,942 against 2,938 members for the 3000 | `ALLFUNDOLD` |
| Large / Mid / Small / Micro / Nano Cap | `LargeCap` ... `NanoCap` | `!IsMLP`, `!IsADR`, `FRank("MktCap", #Previous)` above 91 / 77-91 / 53-77 / 29-53 / up to 29; reconstituted quarterly | `ALLFUNDOLD` |
| NASDAQ Exchange / NY Stock Exchange / NYSE MKT (AMEX) / No OTC Exchange | `NASD` / `NYSE` / `NYSEMKT` / `NOOTC` | `InSet(ExchangeCode, 14, 15)` / `ExchangeCode = 11` / `= 12` / `!InSet(ExchangeCode, 13, 19)` | `ALLFUNDOLD` |
| Over The Counter / American Depositary Receipt / Master Limited Partnership | `OTC` / `$ADR` / `MasterLP` | `InSet(ExchangeCode, 13, 19)` / `IsADR` / `IsMLP` | `ALLSTOCKSOLD` |
| Toronto Stock Exchange / TSX Venture Exchange / CanadaTrust | `TSX` / `TSXV` / `CanadaTrust` | `ExchangeCode = 7` / `= 9` / `Ticker("*.UN* *.UT*")` | `ALLFUNDCAN` |
| P123 Invest (not reachable by name) | - | `FRank("MktCap") > 80`, no OTC/ADR/MLP, `SalesGr%TTM` not NA, `AvgDailyTot(20)` and `AvgDailyTot(20, 20)` above $10M, $50M below a $5 price | `ALLFUNDOLD` |
| 13 sector universes (not reachable by name) | - | `Sector = MATERIALS`, `CYCLICALS`, `ENERGY`, `FINANCIAL`, `HEALTHCARE`, `TECH`, `UTIL`, `INDUSTRIAL`, `BIZSVCE`, `CONSUMERSVCE`; `GICS(30)` (Consumer Non-Cyclical), `GICS(2030)` (Transportation), `GICS(50)` (Telecommunications) | `ALLFUNDOLD` |

The short IDs in the second column were verified separately (2026-09-17 and 2026-11-12); for
`Prussell3000` and `MicroCap` the member lists equal the named universe.

### P123 Universes: ETFs (`'type': 'ETF'`)

Conservative, Defensive Equity ETFs, Hard Assets and Special Models resolve by name; they filter
`ETFs (United States)` with the ETF taxonomy sets (`ETFAssetClassSet`, `ETFMethodSet`,
`ETFSectorSet`, ... see [taxonomy.md](taxonomy.md)). `S&P 500 Sector ETFs`
(`Ticker("XLC,XLF,XLE,XLY,XLP,XLV,XLI,XLRE,XLK,XLU,XLB")`) does not resolve by name.

### Removed or never valid (verified 2026-11-12)

| ID | Status | Use instead |
|---|---|---|
| `ALLFUNDEUR` / `Europe (Incl. Foreign Primary)` | **Removed** with the October security master. P123 re-pointed saved systems to Europe (Primary). The `Universe()` page still lists it. | `PRIMARYEUR`; for global companies that also trade in Europe, `PRIMARYGLOBAL` with `TradesIn(...)` |
| `CDR` | Not a universe ID (the `Universe()` page lists it). | `ALLFUNDCDRCAD` |
| `AMEX` | Not a universe ID (the `Universe()` page lists it). | `NYSEMKT` |
| `'Malaysia (Primary)'` | Not found: P123 spells it `Malasya`. | `'Malasya (Primary)'` |

For a universe not listed here (a user's own, a group's, or one P123 adds later), copy its name
exactly from the app and probe it as below.

## Probing a universe ID for free

Send `data_universe` a formula that cannot parse. The universe is checked first, so the error
message tells you whether the ID exists, and a failed call is not charged:

| Message | Meaning |
|---|---|
| `Error near 'ZZQNotAFactor9': Invalid command ...` | The universe exists and you can use it |
| `Universe <X> not found` | No such ID or name |
| `Universe <X> is not in your subscribed regions` | It exists, but your plan does not cover its region |

```python
def universe_exists(client, name):
    try:
        client.data_universe({'universe': name, 'asOfDts': ['2026-11-07'],
                              'formulas': ['ZZQNotAFactor9']})
    except p123api.ClientException as e:
        msg = str(e)
        if 'ZZQNotAFactor9' in msg:
            return True
        if 'not found' in msg:
            return False
        return msg          # e.g. not in your subscribed regions
```

The same trick checks that a function parses: put it first in one formula, `TradesIn("JPN") +
ZZQNotAFactor9`. If the error names `ZZQNotAFactor9`, everything before it parsed.

## Primary, foreign primary and tickers

- **Primary issue.** Each company has one primary issue, chosen by FactSet since October 2026. The
  new universes (Global, APAC & Emerging, Developed APAC, the country universes) hold primary issues
  only, which is what research should use.
- **Foreign primary** is a P123 construct: a company's primary issue copied into another
  country's universe (for example `NVDA:EUR`). It survives only in the US universes (`ALLFUND` =
  "United States (incl. Foreign Primary)") and Canada (`ALLFUNDCAN`, `ALLFUNDCDRCAD`). P123
  discourages foreign-primary universes for research.
- **`ALLFUND` is US-listed, not global.** It holds US listings, ADRs included. For a global universe
  use `PRIMARYGLOBAL`.
- **Tickers carry a country suffix**: `RY:CAN`, `2782:JPN`, `500209:IND` (Indian stocks appear under
  their local listing, not their US ADR). A ticker without a suffix is the US listing. On
  `POST /data` with `region` `Europe` or `APAC & Emerging`, a bare ticker that has listings in that
  region fails with `Ticker resolves to multiple issues: IBM`; add the suffix or pass `p123Uids`.
- **One company, several `p123Uid`s.** Each listing has its own UID (Royal Bank of Canada: `7652`
  for `RY`, `47778` for `RY:CAN`). Joining on `p123Uid` across regions silently misses those pairs.
- The October change also swapped some issues inside a company (for example a preferred line
  replacing the common line, or a B share replacing an A share). P123 calls this technically
  correct vendor data. Compare saved results by `p123Uid`, and expect a few tickers to change.

## Country and listing functions

All of these parsed on the production API on 2026-11-12.

| Function | What it tests |
|---|---|
| `Country("JPN,AUS")` | Country of **domicile** of the company |
| `ExchCountry("JPN")` | Country of the exchange where **this listing** trades |
| `TradesIn("USA CAN")` | The **company** has any issue trading in one of the countries (up to 20 codes, commas or spaces). Point-in-time since 2026-10-16; P123's page says it works for active and inactive companies. |
| `IsPrimary` | 1 if this listing is the company's primary issue |
| `ExchangeCode` | Numeric point-in-time exchange code of the listing (table below) |
| `FXRate` | Exchange rate that converts the stock's currency to the currency of the screen, ranking system or simulation |
| `Universe(PRIMARYAPEM)` | 1 if the stock is in that universe |
| `NewOct2026` | Per P123's forum (2026-09-28): marks stocks that entered with the October security master; `NewOct2026 = FALSE` rebuilds the old membership. It parses on production; its values were not checked. |

Country codes are ISO-3 (`JPN`, `AUS`, `HKG`, `CHN`, `IND`, `KOR`, `TWN`, `SGP`, `NZL` ...); the
full list is in [misc.md](misc.md#constants).

**Exchange codes changed between the beta and production** (Tokyo was 1497 on the beta and is 1496
in production), so take codes from this table or from P123's `ExchangeCode` page, never from
beta-era screens. APAC and Emerging codes (from P123's `ExchangeCode` page, 2026-11-12):

| Country | Exchange | Code |
|---|---|---|
| Japan | Tokyo Stock Exchange | 1496 |
| Japan | Osaka Exchange / Nagoya / Sapporo / Fukuoka / JASDAQ | 1451 / 1439 / 1465 / 1349 / 1380 |
| Australia | ASX / Cboe Australia / National Stock Exchange of Australia | 1256 / 1309 / 1435 |
| Hong Kong | Hong Kong Stock Exchange | 1363 |
| China | Shanghai / Shenzhen | 1476 / 1475 |
| India | BSE / National Stock Exchange of India | 1586 / 1444 |
| South Korea | KRX / KONEX | 1590 / 1589 |
| Taiwan | Taiwan Stock Exchange | 1491 |
| Singapore | Singapore Exchange | 1469 |
| New Zealand | New Zealand Stock Exchange | 1448 |
| Indonesia / Malaysia / Thailand / Philippines | national exchange | 1381 / 1393 / 1274 / 1456 |
| Viet Nam | Ho Chi Minh / Hanoi | 1485 / 1368 |
| Israel | Tel Aviv Stock Exchange | 1489 |
| Brazil / Mexico / Chile | B3 / Mexican SE / Santiago | 1587 / 1414 / 1474 |
| South Africa / Saudi Arabia | JSE / Saudi Exchange | 1385 / 1466 |

North American codes are small numbers (NYSE 11, NASDAQ 14, Toronto 7, TSX Venture 9) and European
ones start at 1001 (London 1003, XETRA 1021, Euronext Paris 1043).

## Currencies, regions and FX in the API

- **`currency` accepts 21 codes** (verified 2026-11-12): the nine of the 2026-08-28 spec (`USD`,
  `CAD`, `EUR`, `GBP`, `CHF`, `NOK`, `PLN`, `SEK`, `TRY`) plus `JPY`, `AUD`, `HKD`, `KRW`, `INR`,
  `CNY`, `TWD`, `SGD`, `NZD`, `BRL`, `ILS` and `MXN`. Rejected with `Invalid currency <X>`:
  `DKK`, `IDR`, `VND`, `MYR`, `THB`, `PHP`, `ZAR` and most other emerging-market codes; their stocks
  are converted to the currency you choose.
- **`region` on `POST /data`** accepts `United States`, `Canada`, `North America`, `Europe`,
  `Global` and `APAC & Emerging` (verified 2026-11-12). `North Atlantic` is rejected
  (`Invalid region <North Atlantic>`) although the spec lists it; `Asia Pacific` and `APAC` are
  rejected too.
- **Money thresholds are in the system's currency, not the stock's.** `MktCap`, `AvgDailyTot` and
  prices are converted with `FXRate`; `rank_perf` applies `minPrice` and `minLiquidity` in the ranking
  system's currency (USD by default; verified 2026-10-07). A `Close(0) > 3` rule on a USD system
  means three US dollars, which can exclude many Japanese and Hong Kong stocks that trade below
  that in USD terms.
- Simulation benchmarks are converted to the simulation's currency.
- **Slippage:** P123 announced on 2026-09-16 (for the beta) a variable slippage based only on USD
  daily liquidity, from 4% below $40,000 a day down to 0.08% above $7.5M, because the old
  "base slippage" gave absurd values in Asia (6.2% for one Hong Kong stock). Check the simulation
  settings before relying on it in production.

## Data gaps and caveats

As of 2026-11-12, per P123's release notes and forum:

- **Missing cash-flow data in Japan, China, Australia, Hong Kong and India** for many stocks. These
  companies file semi-annual cash-flow statements; combined with quarterly income statements and
  balance sheets they produce statements P123 cannot use yet. Every cash-flow based factor is then
  NA: `OperCashFlTTM`, `FCF`, `FCFYield`, `OCFYield`, accrual measures such as `MScoreTATA`, and
  composites that use them (`PiotFScore`). Fix planned in Phase 2 (Dec 2026). Until then, check NA
  counts per country (for example with `rank_ranks` and `includeNaCnt`) before trusting a global
  ranking, and decide how the ranking handles NAs.
- **Short interest is USA only.**
- **Insider and institutional ownership** is missing for Europe and APAC & EM until Phase 3.
- **Analyst estimates** are thin for some countries.
- **Survivorship:** inactive stocks that changed primary issue or moved to another region are
  missing until Phase 2, and pre-2010 coverage outside North America is thinner. Treat long
  APAC/EM backtests with care.
- A Japanese split-and-dividend-on-the-same-day bug that inflated backtests was fixed in
  August/September 2026.
- Some depositary receipts are classified as ETFs and have no fundamentals.
- P123 rebuilt its data on 10-11 October 2026: backtests run before that date do not reproduce
  exactly, also for US-only systems.

## Common Mistakes

| Wrong | Correct | Note |
|---|---|---|
| `'universe': 'ALLFUND'` for a global screen | `'universe': 'PRIMARYGLOBAL'` | `ALLFUND` is US-listed securities only |
| `'universe': 'ALLFUNDEUR'` | `'universe': 'PRIMARYEUR'` | Removed in October 2026: `Universe <ALLFUNDEUR> not found` |
| `'universe': 'PRIMARYJPN'`, `'JAPAN'` | `'universe': 'Japan (Primary)'` | APAC country universes have no short ID; use the exact display name |
| `'universe': 'Indonesia (Primary)'` for Indonesian stocks | `PRIMARYAPEM` with `ExchCountry("IDN")` | P123 bug: that universe's rule is `ExchCountry("IND")`, so it holds Indian stocks |
| `'universe': 'India (Primary)'` | `PRIMARYAPEM` with `ExchCountry("IND")` (or `'Indonesia (Primary)'`, knowingly) | No India universe under its own name |
| `'universe': 'Malaysia (Primary)'` | `'universe': 'Malasya (Primary)'` | P123's spelling; the correct spelling is not found |
| `'universe': '345796'` (the number in the app URL) | `'universe': 'Japan (Primary)'` | Numeric universe IDs are not found |
| Reproducing a pre-October `ALLFUND` system on `ALLFUND` | `ALLFUNDOLD` (or `ALLSTOCKSOLD`) | `ALLFUND` was redefined and grew from 9,296 to 13,184 stocks |
| Assuming `Prussell3000`, `MicroCap`, `TRADEUSA` follow the new US definition | They are built on `ALLFUNDOLD` | P123's own US P123 Universes kept the old base |
| `Country("JPN")` to find stocks that trade in Tokyo | `ExchCountry("JPN")` (this listing) or `TradesIn("JPN")` (any listing of the company) | `Country` is domicile |
| `ExchangeCode = 1497` for Tokyo | `ExchangeCode = 1496` | 1497 was the beta code |
| `'currency': 'DKK'` | a supported currency, e.g. `EUR` | 21 codes are accepted, DKK is not |
| `'region': 'North Atlantic'` on `POST /data` | `Global`, or `North America` / `Europe` | Rejected live although the spec lists it |
| Joining two regions on `p123Uid` | Join on a company-level key, or map listings first | Each listing has its own UID |
| Reading NA cash-flow factors in Japan as "no cash flow" | Data gap until Phase 2 | Exclude or neutralize those nodes for APAC |

## Sources

- Live API checks, 2026-11-12, `p123api` 3.1.0: universe IDs and names, currencies, `region` values
  and function parsing through calls that ended in an error (no credits); member counts through 15
  `screen_run` calls (`maxNumHoldings: 0`, 2 credits each).
- P123 web app, 2026-11-12: Universes → P123 Universes (86 entries) with each universe's base and
  rules, and the currency list of the universe editor (the same 21 codes the API accepts).
- P123 Factor Reference pages `Universe`, `ExchangeCode` and `TradesIn`
  (portfolio123.com/doc), read 2026-11-12.
- P123 community announcements: "Global data has been released!" (2026-11-02), "Global data
  launching this week in production free for 30 days" (to 2026-11-04), "IMPORTANT: Pre-global
  release next weekend" (2026-09-28 to 10-16), "Global data including Asia and Emerging now
  available in beta!", "Asia data update" (2026-09-15/16).

## See Also

- [api.md](api.md) - endpoints, the earlier regional universe check (2026-09-17) and Known Pitfalls.
- [misc.md](misc.md) - country IDs and the US universe IDs.
- [fundamentals.md](fundamentals.md) - `Country`, `ExchCountry`, `IsPrimary`, `ExchangeCode`.
- [universe-filters.md](universe-filters.md) - `UnivExclude`, `UnivSubset`, `UnivRBICS`.
