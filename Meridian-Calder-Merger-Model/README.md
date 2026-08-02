# Meridian / Calder — M&A Merger Model

A full merger model and board memorandum for a proposed all-stock acquisition in the North
American industrial distribution sector.

> **All companies, financial statements and market data in this model are fictional.** Meridian
> Industrial Supply and Calder Fluid & Flow Systems do not exist, and neither do the comparable
> companies, precedent transactions or peer betas used in the valuation. Every figure was invented
> for this exercise. The model was built from scratch to practise the mechanics of a sell-side M&A
> case: three-statement forecasting, purchase accounting, accretion/dilution, synergy analysis and
> transaction valuation.

---

## The transaction

Meridian Industrial Supply (NYSE: MIDS), a $6.2bn distributor of MRO consumables, fasteners,
power transmission and fluid-handling products, proposes to acquire Calder Fluid & Flow Systems
(NASDAQ: CFFS), a $2.6bn flow-control specialist, in an all-stock deal.

| | |
|:---|---:|
| Exchange ratio | 1.4000x |
| Implied offer price | $48.30 per share |
| Premium to undisturbed price | 27.8% |
| Purchase equity value | $2,697.8m |
| Purchase enterprise value | $2,984.3m |
| Purchase TEV / FY25E EBITDA | 9.4x |
| Pro-forma ownership (MIDS / CFFS) | 60.3% / 39.7% |
| Run-rate cost synergies by Year 3 | $95.4m |
| Assumed close | 31 December 2025 |

## What the analysis concludes

- **EPS is dilutive until Year 3** on a fully loaded basis: (10.8%) in FY26E, (2.6%) in FY27E,
  turning positive at +3.7% in FY28E. Management's expectation of first-year accretion only holds
  once integration costs and purchase accounting are excluded, and even then it is roughly
  break-even.
- **The price is defensible.** A DCF at a 10.57% WACC gives $47.66 per share against the $48.30
  offered, and the deal prices below both the trading comparables (10.9x) and the precedent
  transactions (10.6x).
- **The financing structure is the problem.** Meridian trades at 8.3x EBITDA against a 10.9x peer
  median, so paying in stock means using an undervalued currency. Switching to a cash and debt
  mix takes Year 1 from (10.8%) to roughly break-even and Year 3 to +25%, at the cost of opening
  leverage of 3.3x.
- **The deal is synergy-dependent.** It returns 16.2% against a 10.4% cost of capital, but at the
  proposed exchange ratio it is dilutive even in Year 3 if less than ~62% of the planned savings
  are delivered.

## What is in the workbook

Twelve tabs, ~2,900 live formulas:

| Tab | Contents |
|:---|:---|
| `Cover` | Transaction snapshot, colour key, integrity checks, tab index |
| `Merger_Model` | Deal assumptions and switches, financing capacity, sources & uses, purchase price allocation, combined income statement and cash flow, EPS accretion, two sensitivity grids |
| `Acquirer` / `Target` | Full three-statement operating models, FY24A–FY30E, on a shared row structure |
| `Synergies` | Headcount-driven cost synergies, cross-sell revenue synergies, costs to achieve, PV of synergies vs premium paid |
| `Combined_BS` | Purchase-accounting balance sheet combination at close |
| `DCF_IRR` | Ten-year unlevered DCF of the target plus the acquirer's IRR on the transaction, with sensitivities |
| `WACC` | Peer beta unlevering and re-levering; discount rates for both companies |
| `Pub_Comps` / `MA_Comps` | Trading comparables and precedent transactions |
| `Contrib` | Relative contribution analysis and the exchange ratio each metric implies |
| `Value_Creation` | Combined entity valued against larger-cap platform multiples |

## Modelling conventions

A few choices worth flagging, since they are what make the model work rather than just look
finished:

- **SG&A is modelled as headcount × cost per employee.** This is what lets the synergy schedule
  translate a named reduction in force directly into dollars, rather than assuming a percentage
  of the cost base.
- **No circular references.** Interest expense runs on average debt balances and interest income
  on opening cash, so the workbook needs no iterative calculation.
- **Operating leases** are held equal on both sides of the balance sheet and excluded from the
  enterprise value bridge, since EBITDA is presented post-ASC 842.
- **Buybacks are switched off** from FY25E so the pro-forma share count moves only with deal
  consideration, and the standalone forecasts assume no further bolt-on M&A so that segment
  growth is not double-counted.
- **Deal switches** on `Merger_Model` let you flip between 100% stock and a cash/debt/stock mix,
  turn revenue and expense synergies on or off, and flex the synergy realisation factor.

**Colour convention:** blue = hardcoded input, black = formula on that tab, green = link to
another tab, grey italics = commentary explaining the assumption.

## Checks performed

- Both standalone balance sheets and the pro-forma opening balance sheet balance to zero in every
  period
- Balance sheet cash ties to closing cash on the cash flow statement in every period
- Sources equal uses; total consideration reconciles to the purchase equity value
- The workbook recalculates without errors under both financing structures
- No external links, no circular references, no `#REF!` or `#DIV/0!`

## Files

| File | |
|:---|:---|
| `Meridian-Calder-Merger-Model.xlsx` | The model |
| `Project-Calder-Board-Memo.pdf` | Six-page board memorandum answering the case questions |

## Disclaimer

Built for educational and portfolio purposes. The companies and data are fictional and the
analysis should not be relied upon for any investment, financing or commercial decision.

