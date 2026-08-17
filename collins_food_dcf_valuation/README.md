# Collins Foods (ASX: CKF) — DCF Valuation

Working notes on `CKF_DCF_Model.xlsx`. I've recorded the reasoning behind every non-obvious decision, including the ones I got wrong first time.

**Base case: $13.85 per share against a market price of $8.24.** Twelve tabs, ~1,500 live formulas, seventeen integrity checks, all passing.

I don't present that premium as a conviction call. The unlevered beta is still a placeholder, and it moves the answer more than anything else in the model:

| Unlevered beta | WACC | Value | Premium |
|---|---|---|---|
| 0.65 (placeholder) | 8.56% | $13.85 | +68% |
| 0.80 | 9.44% | $11.75 | +43% |
| 0.95 | 10.31% | $10.14 | +23% |

Until I populate the comparables table with real betas, the honest reading is a range of roughly $10–14, not a point estimate. Broker consensus at the time of build was $10.27.

One observation I do find useful: **my downside case lands at $8.24, which is exactly where the stock trades.** The market is pricing something close to my downside — Germany stalling, Australian margin eroding, Netherlands flat.

---

## 1. Build order

I built it in this sequence, and I'd do it the same way again. Tab order is arranged for a reader; build order is different.

| # | Step | Reason |
|---|---|---|
| 1 | Historicals, with a source note on every figure | Can't forecast what I haven't reconciled |
| 2 | AASB 16 bridge | Determines the whole architecture |
| 3 | **FY26A column of the Model** | The wiring test — see section 2 |
| 4 | Assumptions as three scenario blocks | Retrofitting scenarios means rewiring everything |
| 5 | Forecast columns | Trivial once FY26A ties — copy sideways, swap inputs |
| 6 | Capex waterfall, then debt schedule | |
| 7 | WACC, DCF, sensitivities, checks | |

## 2. The FY26A column

Column C of the Model rebuilds reported FY26 using the same formulas as the forecast columns, fed by reported inputs. Both integrity checks read zero difference.

This is the part of the build I'd defend hardest. It's easy to produce a forecast that looks plausible and is silently wrong — a mis-anchored average, a margin on the wrong revenue line, a segment counted twice. Rebuilding the actual year is the only cheap proof the arithmetic chain holds before any judgement gets layered on top.

## 3. Revenue drivers

Australian revenue is ~78% of the group and is built as:

```
average restaurants trading  ×  average unit volume
```

I use the average of opening and closing store count, not closing. A restaurant opened in March trades for two months, not twelve — closing counts overstate revenue in every rollout year, and CKF is in a rollout.

AUV grows at same store sales **plus a separately-modelled Kwench uplift**, so the initiative can be isolated and switched off.

Germany and the Netherlands are modelled separately because their drivers are unrelated — Germany is a rollout from 17 restaurants toward a targeted 71 by FY30, the Netherlands is a mature 63-restaurant network with flat comps.

A note on where the value actually sits: the entire strategic narrative is Germany, but Germany is under 5% of revenue. A 50bp change in Australian same store sales moves the valuation more than doubling the German network does.

## 4. Solving the Germany / Netherlands split

CKF reports Europe as one segment, so the country revenue split isn't disclosed. It's solvable rather than something to estimate around, because the Directors' Report gives each country's growth rate — Netherlands +11.6%, Germany +15.8%:

```
NL₂₅ + DE₂₅ = 312.3                    (reported Europe FY25)
NL₂₅ × 1.116 + DE₂₅ × 1.158 = 351.3    (reported Europe FY26)
```

Netherlands $246.3m → $274.8m, Germany $66.0m → $76.5m. Implied AUV of $4.40m and $4.63m.

Two reasons this was worth doing. Because both countries are now derived independently, their sum reconciling to reported Europe revenue is a genuine cross-check rather than an identity — it's an integrity check on the Control sheet. And it converted the model's largest judgement into a sourced figure.

FX sense-check: the presentation quotes constant-currency growth of 6.1% and 10.1% against reported 11.6% and 15.8%. Applying that ~5.6pp translation benefit to the solved bases implies about $17m of FX tailwind against the $16.0m disclosed. The split holds.

## 5. AASB 16 — why I value pre-lease-capitalisation

CKF leases essentially all 375 restaurants. Under AASB 16 rent leaves operating costs and reappears as right-of-use depreciation plus lease interest. FY26 EBITDA is **$160.8m pre-AASB 16 and $244.5m post** — identical business, $83.7m apart.

The rule: the capital structure in the WACC must match the cash flow definition. Rent still an operating cost → leases are not debt. Rent added back → leases are debt and must come out in the equity bridge.

**I value entirely pre-AASB 16**, which matches CKF's own syndicated facility (net leverage 0.77x on that basis).

I built it both ways first, and the post-AASB 16 version produced a nonsense answer. The arithmetic is on the DCF sheet:

| | A$m |
|---|---|
| FY27E rent added back | 89.1 |
| After tax | 62.4 |
| Capitalised in perpetuity at (WACC − g) | **1,029** |
| Reported lease liability available to offset it | 630.5 |
| **Unfunded gap** | **398** |

Adding rent back creates value in perpetuity, but the balance sheet liability covers only committed lease terms — roughly a decade, not forever. Deducting the book number offsets a fraction of what was added back, so a naive post-AASB 16 DCF overvalues the equity by the difference. Fixing it properly means capitalising the full perpetual lease obligation rather than the accounting figure, at which point I'd be estimating the number the standard exists to avoid estimating.

I still need post-AASB 16 EBITDA for one thing: comparing multiples, since most screens now quote EV/EBITDA on that basis. Both measures are on the Model sheet and the Comps tab shows CKF both ways.

## 6. Depreciation — the vintage waterfall

My first version grew D&A at 60% of revenue growth. That was a placeholder, and Note G5 let me replace it.

Disclosed lives: leasehold improvements 20 years or lease term, plant and equipment 8 years, motor vehicles 4 years. I split capex **55/45** between leasehold improvements and plant and equipment — a KFC build is weighted toward fitout, drive-thru works and shopfront over kitchen equipment. Not disclosed, so it's flagged as an assumption.

I use 15 years as the effective leasehold improvement life rather than the 20-year cap, because most QSR primary lease terms run 10–15 years and Note G5 caps at the shorter of the two. Blended rate 9.29%, implying a **10.8-year life** on new capex.

Structure:

| Layer | Depreciated over |
|---|---|
| FY26 legacy NBV of $238.8m | declining balance at the FY26-implied rate |
| Each year's capex, split 55/45 | 15 years and 8 years respectively |

The legacy pool runs off on a declining balance rather than straight-line to zero. A pool of mixed vintages decays smoothly; straight-lining it to exhaustion creates an artificial cliff, which would have landed in the terminal year.

**A mistake I made and corrected.** My original 21.3% rate was struck on net book value but applied as though it were a gross-cost rate. Those have different denominators — depreciation is charged on gross cost, and NBV is well below that. It's why I originally read a 4.7-year "life" against Note G5's 8 years. Both figures are right but measure different things: 4.7 years is the remaining book life of the existing balance, 8 years is the useful life of a new asset. Correcting it reduced FY27 depreciation from $64.6m to $57.4m.

## 7. Debt, covenants and capacity

The Debt & Leases tab runs a financing waterfall: FCFF, less interest, plus the tax shield on interest, less dividends, then a cash sweep repaying debt with anything above a minimum operating balance.

**None of it touches the DCF.** FCFF is measured before financing. This answers a different question — can the balance sheet carry the plan, and how much more could it carry.

Interest is charged on **opening** balances deliberately. Using average balances makes interest depend on the repayment, which depends on cash flow, which depends on interest — circular. Banks resolve that with iterative calculation; a file that gets emailed around shouldn't depend on a setting the recipient may not have on.

The tax shield gets handed back in the waterfall because FCFF was struck after tax on the full EBIT. Same principle as keeping the shield in the WACC rather than in FCFF.

**A discrepancy worth knowing.** My FY26 net leverage reads 0.74x against CKF's reported 0.77x. Solving backwards, the covenant EBITDA is about $155m — continuing-operations EBITDA of $160.8m *less* the $5.6m pre-AASB 16 Taco Bell loss. The covenant is struck on total group earnings including discontinued operations. Worth checking which EBITDA any covenant actually bites on.

**The lease roll-forward, and a construction error I fixed.** I first forecast lease additions directly — growing repayments with revenue, then setting additions to repayments grown again. Compounding growth twice was arbitrary, and it produced a liability that rose only 8% while the estate grew by a third. It also left the model holding two contradictory lease numbers, since a separate memo line scaled with revenue and reached $1,158m against the roll-forward's $683m.

The right way round is to drive the balance and solve the additions:

```
closing lease liability  =  opening × (1 + revenue growth)
new and renewed leases   =  closing − opening + principal repayments
```

The obligation scales with the restaurant estate, so the *balance* is the thing you can forecast and additions are the plug. Both tabs now read from one schedule: $630.5m growing to $1,158m, in line with revenue. Lease-adjusted leverage runs 3.1x down to 2.2x rather than falling implausibly to 1.1x.

The valuation didn't move a cent, which is the point — the DCF is pre-AASB 16, so the lease schedule is genuinely a memo. That it *didn't* move is a useful confirmation nothing was leaking into the cash flows.

## 8. Forecast horizon and terminal value

I originally ran FY27–FY31. That was too short, and the model told me so.

With depreciation built properly, FY31 had capex at 1.7x depreciation and revenue still growing 8.1% — while I was applying a 2.5% perpetuity to it. Those can't all be true. I added a terminal normalisation (capex reset to depreciation × (1+g), working capital rescaled to terminal growth), and it moved the base case from $9.50 to $12.65. **A 33% swing from one modelling choice.**

That swing was the diagnostic. So I extended the forecast to **FY35**, nine years, far enough for the rollout to complete and growth to fade inside the explicit period:

| | FY27E | FY31E | FY35E |
|---|---|---|---|
| Revenue growth | 6.5% | 8.1% | 4.1% |
| Capex / depreciation | 1.72x | 1.75x | **0.97x** |

Capex and depreciation converge, which is what a steady state looks like. The results:

| | 5 years (FY31) | 9 years (FY35) |
|---|---|---|
| Terminal value as % of EV | 85% | **69%** |
| Swing from terminal treatment | 33% | **6%** |

The terminal year is now doing a defensible share of the work rather than carrying the valuation.

**The cost of doing this**, which I'd raise before anyone else does: CKF has a five-year strategic plan, so FY32–FY35 are entirely my extrapolation. A longer horizon reduces dependence on terminal value but increases the amount of pure guesswork inside the explicit forecast. I've traded one weakness for a smaller one, not eliminated it.

## 9. Details worth flagging

**The 53rd week.** FY26 ran 28 April 2025 to 3 May 2026 — 53 weeks against FY25's 52. That's ~1.9% more trading days, inflating revenue and profit with no underlying improvement. FY27 reverts to 52, so one week of Australian trading ($23.4m) is stripped out. If I'd grown FY26 forward without adjusting, the error would have compounded into every forecast year and the terminal value. Same store sales are unaffected — companies report those on comparable weeks. It's headline revenue growth of +7.6% that carries the extra week.

*Known gap: I applied this to Australia only. The 53-week period is the group's financial year, so Europe should carry roughly $6.6m too.*

**Acquired versus built restaurants.** The 8 Munich restaurants completed 1 June 2026 — after the 3 May year end, so they fall in FY27. They're excluded from new-restaurant capex and charged as acquisition consideration instead. Charging them as builds pushed FY27 capex to $117m against $80–100m guidance; correcting it landed at $99m.

**Acquisitions in FCFF.** The standard formula has no acquisitions line, and a purely organic DCF would exclude them. But the Munich restaurants are in the store count and generate revenue in every forecast year, so the $50.6m has to come out. Include the cost only if the benefit is included. The debt drawdown that funded it correctly sits below the FCFF line.

**Tax on EBIT, not profit before tax.** FCFF is pre-financing, so the interest tax shield belongs in the WACC. Deducting it twice is the most common error in a DCF.

**Mid-year discounting matters less than expected.** It lifts explicit-period cash flows ~4%, but terminal value is discounted at the full final period either way. Total value moves under 1.5%.

## 10. Cross-checks

Seventeen integrity checks on the Control sheet. The ones that matter most:

- **FY26A revenue and EBITDA rebuild to zero difference** — the wiring test
- **Europe split sums to reported revenue** — real cross-check, since both countries are derived independently
- **FY26A D&A per the waterfall vs reported** — the roll-forward reproduces reported depreciation and amortisation
- **Terminal capex / depreciation between 0.95x and 1.15x** — catches capitalising a growth-phase year
- **Implied Gordon exit multiple** — if the perpetuity implies a multiple far from where peers trade, an input is wrong
- **Covenant never breached, cash never below minimum** — the plan is fundable

## 11. What this model doesn't do

- **No linked three-statement build.** There's a fixed asset roll-forward, a debt schedule and a lease roll-forward, but no balance sheet that balances — so no balance-sheet integrity check. Biggest remaining structural gap.
- **No stub period.** Valued at the 3 May 2026 balance date, not the 8 August valuation date.
- **No FX layer.** European earnings are modelled in Australian dollars, despite FY26 carrying a $16.0m favourable translation benefit.
- **Franchise agreement risk isn't modelled.** The Netherlands Corporate Franchise Agreement runs to 31 December 2029, inside the forecast. Renewal is assumed.
- **No credit for future M&A.** Management flags bolt-on ambitions, but I've only modelled Munich — so no capital deducted for future deals either. Consistent, but it values CKF as a static estate rather than a roll-up platform.

## 12. Scenario outputs

| Scenario | Value per share | vs market ($8.24) |
|---|---|---|
| Downside | $8.24 | 0% |
| Base | $13.85 | +68% |
| Upside | $19.82 | +141% |

Switching Kwench off costs $0.42 per share, about 3% of equity value, against $45m of committed capex. Note the switch isolates the revenue uplift only — the project capex stays in either way, so that figure is not the project NPV.

## 13. Still to replace

| Input | Current | Source |
|---|---|---|
| Unlevered beta | 0.65 placeholder | Comparables table on the WACC tab — **the highest-impact gap** |
| Cost of corporate debt | 5.6% | Note B2 Borrowings |
| Covenant limit | 3.0x assumed | Note B2 — drives all headroom and capacity output |
| Working capital | −6.0% of revenue | Notes G4 and G9 |
| Leasehold improvement life | 15 years assumed | Note G8 weighted average lease term |
| Capex split 55/45 | assumed | Not disclosed — Note G5 gives lives, not mix |
| Munich PP&E allocation | 40% of $50.6m | Note A2 — CKF's own allocation is provisional until HY27 |
| Peer multiples | blank | Data provider — a fabricated multiple is worse than a blank one |

---

## Sources

- Collins Foods FY26 Results Presentation, 30 June 2026 — every historical figure traces to a page reference in column H of the Historicals tab
- Collins Foods FY26 Annual Report, 30 June 2026 — Directors' Report for the country growth rates, Note G5 for asset lives, Subsequent Events for the Munich consideration (€31.1m / $50.6m, completed 2 June 2026, funded from existing facilities)
- Australian 10-year Commonwealth Government bond yield, ~4.9%, early August 2026
- CKF share price ~$8.24; reported broker consensus target ~$10.27

*Educational and illustrative only. Not investment advice and not a professional valuation. Forecasts are my own and are not company guidance.*
