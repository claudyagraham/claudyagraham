<div style="border-top: 6px solid #024731; border-bottom: 1px solid #B2B2B2; padding: 12px 0; margin-bottom: 24px; font-family: 'Open Sans', Helvetica, Arial, sans-serif;">
  <div style="color: #024731; font-weight: 700; letter-spacing: 0.06em; text-transform: uppercase; font-size: 0.85rem;">University of Hawaiʻi at Mānoa · Shidler College of Business</div>
  <div style="color: #000000; font-weight: 700; font-size: 1.25rem; margin-top: 4px;">FIN 321: International Finance and Securities</div>
  <div style="color: #525252; font-weight: 400; font-size: 0.95rem;">FX Transaction Hedging Project — Technical Specification</div>
</div>

<!--
BRAND FORMATTING — applied per docs/_branding/design.json (v1.0.0)
  ┌─ Colors ─────────────────────────────────────────────────────────────┐
  │ Primary green ........... #024731  RGB 2,71,49    Pantone 3435 C     │
  │ Primary black ........... #000000  RGB 0,0,0      Process Black      │
  │ Silver (secondary) ...... #B2B2B2  RGB 178,178,178 Cool Gray 5 C     │
  │ White (secondary) ....... #FFFFFF                                    │
  │ Neutral-600 (muted) ..... #525252  (secondary text, captions)        │
  │ UH-green 700 (hover) .... #013D26  (link hover, pressed state)       │
  │ UH-green 50 (tint) ...... #E6F2EF  (callout backgrounds)             │
  │ Yellow (Excel only) ..... #FFFF00  (input highlight — not brand)     │
  ├─ Typography ─────────────────────────────────────────────────────────┤
  │ Headings (web)  .......... Open Sans Bold (H1/H2) / Semibold (H3/H4) │
  │ Headings (print)  ........ Avenir Bold                               │
  │ Body (web)  .............. Open Sans Regular                         │
  │ Body (print)  ............ Avenir Book                               │
  │ Fallback stack  .......... Helvetica, Arial, sans-serif              │
  │ Monospace  ............... Consolas / ui-monospace                   │
  │ Body minimum size  ....... 10 pt (11–12 pt preferred for print)      │
  │ Leading  ................. 3–5 pt greater than type size (print)     │
  │ Alignment  ............... Flush left, ragged right                  │
  ├─ Accessibility ──────────────────────────────────────────────────────┤
  │ • ADA-compliant contrast ratios for ALL text and UI elements         │
  │ • No red body type                                                   │
  │ • No layouts that are too dark for readability                       │
  │ • No custom palettes or gradients outside the official brand         │
  └──────────────────────────────────────────────────────────────────────┘
  Full brand standard: docs/_branding/design.json · Source: https://manoa.hawaii.edu/brand/
-->

# [ship-propulsion] — FX Transaction Hedge Model · Technical Specification

> <span style="color:#024731; font-weight:700;">Technical specification</span> for the FX transaction hedge model — the named-range contract, calculation flow, and validation checks, precise enough that an AI or a colleague could build (or rebuild) the workbook from this document alone. This spec is the input the AI-assisted build works from.

| Field | Value |
|------|------|

| **Created by** | Claudya Graham |
| **Updated by** | Claudya Graham |
| **Date Created** | 2026-11-06 |
| **Date Updated** | 2026-11-13 |
| **Version** | 0.2 |
| **LLM Used** | Claude (Anthropic) for initial proofreading; ChatGPT (OpenAI) for revisions following instructor feedback. |

| **Role** | Treasury Analyst / FP&A Analyst |
| **Audience** | CFO / Director of Treasury |
| **Companion Workbook** | capabilities/fx-hedging/model.xlsx |

---

## 1. Problem Statement


A U.S. ship-propulsion engineering firm has invoiced a Greek shipyard EUR 5,600,000, payable 365 days after inception. Because the receivable is denominated in euros, the USD value of the cash received depends on the EUR/USD spot exchange rate at settlement.

A weaker euro reduces USD proceeds. At the model's current spot assumption of 1.1081 USD/EUR, a 1% depreciation of the euro reduces the unhedged receivable's USD value by approximately $62,053.60.

This specification defines an Excel workbook that compares four strategies: no hedge, forward hedge, money-market hedge, and EUR put option hedge.

The objective is to help the CFO evaluate USD proceeds, hedging costs, downside protection, and upside potential across a +/-5% range of settlement exchange rates.

---

## 2. Inputs (Known Variables)

All values below are indicative, replaced with live market data at Stage 4. Input cells appear only on the Inputs tab.


| Name | Placeholder / Value | Unit | Stage 4 Data Source |
|---|---|---|---|
| `FC_AMT` | 5,600,000 | EUR | Scenario 18 (client invoice) |
| `S0_in` | 1.1081 | USD per EUR | ECB reference rate, 2026-11-12 |
| `F0_in` | 1.1807 | USD per EUR | Scenario-indicative forward quote; replace with live 12-month quote at Stage 4 |
| `R_USD` | 4.65% | Annual % | 1-year U.S. Treasury yield, 2026-11-12 |
| `R_FC` | 3.293% | Annual % | 12-month EURIBOR, 2026-11-12 |
| `K_PUT` | = `S0_in` | USD per EUR | Strike at spot, per scenario |
| `K_CALL` | = `S0_in` | USD per EUR | Strike at spot, per scenario |
| `PREM_PUT` | 0.0245 | USD per EUR | Scenario-indicative option premium; replace at Stage 4 |
| `PREM_CALL` | 0.0448 | USD per EUR | Scenario-indicative option premium; replace at Stage 4 |
| `T_DAYS` | 365 | Days | Contract settlement date minus inception date |




### 2.1 Core Inputs

The model uses the ten standardized inputs listed in Section 2. All inputs are maintained on the Excel workbook's Inputs worksheet, with one authoritative value for each variable.

Scenario 18 involves a EUR 5,600,000 receivable due in 365 days. The functional currency is USD, and all exchange rates are quoted in USD per EUR.

The spot exchange rate and annual interest-rate proxies are dated November 12, 2026. The forward rate and option premiums remain scenario-indicative assumptions until Stage 4.

All calculated outputs must reference standardized named ranges rather than hard-coded values.

### 2.2 Derived / Intermediate Values

The workbook calculates the following intermediate values using Excel formulas:

| Name | Description | Calculation |
|---|---|---|
| `S_T` | Settlement spot rate | Sensitivity scenario rate |
| `USD_NO_HEDGE` | Unhedged USD proceeds | `S_T * FC_AMT` |
| `EUR_BORROW` | EUR amount borrowed today | `FC_AMT / (1 + R_FC * T_DAYS / 360)` |
| `USD_INVEST` | USD proceeds invested today | `EUR_BORROW * S0_in` |
| `MM_PROCEEDS` | Money-market USD proceeds at settlement | `USD_INVEST * (1 + R_USD * T_DAYS / 360)` |
| `FWD_PROCEEDS` | Forward hedge USD proceeds | `FC_AMT * F0_in` |
| `PUT_NET` | Net put hedge USD proceeds | `FC_AMT * MAX(S_T, K_PUT) - FC_AMT * PREM_PUT` 
| `CALL_NET` | Net payoff from a long EUR call, reference only | `FC_AMT * MAX(S_T - K_CALL, 0) - FC_AMT * PREM_CALL` |

The put and call premium calculations use the simplified nominal-premium convention specified in Section 3. Premiums are not future-valued. All intermediate values must be calculated dynamically using formulas.


---


## 3. Assumptions & Constraints

- **Rate basis:** Simple interest, ACT/360, with a 365-day settlement period.
- **Transaction costs:** No bid-ask spreads, brokerage fees, or commissions are modeled. The forward rate is treated as an indicative executable quote for modeling purposes.
- **Borrowing/investing:** The firm borrows EUR at `R_FC` and invests USD at `R_USD`. These rates are market proxies rather than actual corporate financing quotes.
- **Parity expectation:** Covered interest parity is evaluated by comparing the forward hedge with the money-market hedge. The model calculates and reports the percentage difference. Because the forward quote and interest-rate inputs are not matched market instruments, a difference is possible and must not be artificially forced to zero.
- **Option premiums:** Premiums are quoted in USD per EUR. Total premium equals `FC_AMT` multiplied by the relevant option premium. Premiums are deducted at their nominal USD amounts and are not future-valued under this simplified model.
- **The call:** A long EUR call is included as a reference payoff only and is not considered a hedge for the EUR receivable.
- **Credit and counterparty risk:** Ignored; the shipyard pays the full receivable at settlement.
- **Taxes and hedge accounting:** Excluded.
- **Build constraints:** All ten input variables must have Excel named ranges. Calculated cells must use formulas rather than pasted results.

### 3.1 Workbook Tab Architecture

| Worksheet | Purpose |
|---|---|
| Cover | Identifies Scenario 18, author, date, receivable amount, and financial objective. |
| Inputs | Contains the ten standardized named inputs, units, source dates, and assumptions. |
| Calc_Forward | Calculates USD proceeds under the forward hedge. |
| Calc_MoneyMarket | Calculates the three money-market hedge steps and covered-interest-parity check. |
| Calc_Options | Calculates the put and call payoffs and USD premium costs. |
| Sensitivity | Contains the formula-driven -5% to +5% settlement exchange-rate scenarios and comparison chart. |
| Legend | Explains worksheet color conventions: yellow inputs, blue assumptions, green formulas, and gray outputs. |
| Notes | Documents market sources, model conventions, limitations, and validation results. |

All worksheets must use consistent variable names and formula-driven calculations. The Inputs worksheet serves as the single source of truth for the ten named inputs.



## 4. Calculation Flow

All calculations must use Excel formulas referencing the standardized named inputs. Settlement spot rate is represented by `S_T`.

### 4.1 Forward Hedge

The forward hedge locks in the USD value of the EUR receivable.

- `FWD_PROCEEDS = FC_AMT * F0_in`

### 4.2 Money-Market Hedge

The money-market hedge must show three separate calculation steps:

1. Borrow the present value of the EUR receivable:
   `MM_BORROW_FC = FC_AMT / (1 + R_FC * T_DAYS / 360)`

2. Convert the borrowed EUR to USD at the spot rate:
   `MM_USD_NOW = MM_BORROW_FC * S0_in`

3. Invest the USD proceeds until settlement:
   `MM_PROCEEDS = MM_USD_NOW * (1 + R_USD * T_DAYS / 360)`

### 4.3 Covered Interest Parity Check

Calculate the theoretical forward rate and the percentage difference between the money-market and forward hedge proceeds.

- `F_IMPLIED = S0_in * (1 + R_USD * T_DAYS / 360) / (1 + R_FC * T_DAYS / 360)`
- `PARITY_GAP_PCT = ABS(MM_PROCEEDS - FWD_PROCEEDS) / ABS(FWD_PROCEEDS)`

The calculated difference must be displayed in the workbook. Because market-rate proxies and a scenario-indicative forward quote are used, the parity check may fail the specified tolerance. A failed check must be documented rather than forced to pass.

### 4.4 Put Option Hedge

The firm purchases a EUR put option to protect against depreciation of the euro.

- `PUT_PREM_TOTAL = FC_AMT * PREM_PUT`
- `PUT_FLOOR = FC_AMT * (K_PUT - PREM_PUT)`
- `PUT_NET(S_T) = FC_AMT * MAX(S_T, K_PUT) - PUT_PREM_TOTAL`

The put protects against EUR depreciation while preserving upside if the EUR appreciates. The premium is deducted at nominal USD value, following the simplified model convention.

### 4.5 Call Option Reference Payoff

A long EUR call is included for reference only and is not a hedge for the EUR receivable.

- `CALL_PREM_TOTAL = FC_AMT * PREM_CALL`
- `CALL_PAYOFF(S_T) = FC_AMT * MAX(S_T - K_CALL, 0)`
- `CALL_NET(S_T) = CALL_PAYOFF(S_T) - CALL_PREM_TOTAL`

### 4.6 Unhedged Baseline

The unhedged position converts the full EUR receivable at the settlement spot rate.

- `UNHEDGED(S_T) = FC_AMT * S_T`

### 4.7 Calculation Requirements

- All exchange rates are quoted as USD per EUR.
- The settlement horizon is `T_DAYS = 365`.
- Simple interest uses the ACT/360 convention.
- All calculated values must use Excel formulas.
- All ten standardized input named ranges must exist.
- Option premiums must be denominated in USD.
- The workbook must not contain hard-coded calculated outputs.

## 5. Sensitivity Analysis Requirements

The workbook must include a formula-driven sensitivity analysis showing how the settlement exchange rate affects the USD proceeds of each hedging strategy.

### 5.1 Settlement Exchange Rate Scenarios

- The settlement exchange rate, `S_T`, must range from 95% to 105% of `S0_in`.
- The analysis must use 1% increments, producing 11 scenarios.
- All exchange rates must be expressed in USD per EUR.
- Every sensitivity row must recalculate automatically when the relevant model inputs change.

### 5.2 Required Sensitivity Table

The sensitivity table must include:

1. Settlement exchange rate (`S_T`)
2. Percentage change from the initial spot rate
3. Unhedged USD proceeds
4. Forward hedge USD proceeds
5. Money-market hedge USD proceeds
6. Put option net USD proceeds
7. Hedge profit relative to the unhedged position
8. Best-performing strategy for each exchange-rate scenario

All calculated values must reference the formulas and named inputs defined in Section 5.

### 5.3 Required Sensitivity Chart

Create one line chart with:

- X-axis: settlement exchange rate (`S_T`)
- Y-axis: USD proceeds
- Series: Unhedged, Forward, Money Market, and Put Option
- Clear title, axis labels, and legend

The chart must update automatically when the underlying model inputs change.

### 5.4 Financial Interpretation
The chart should demonstrate the financial trade-offs between:

- Forward hedge: fixed USD proceeds and protection against EUR depreciation.
- Money-market hedge: fixed USD proceeds based on borrowing, spot conversion, and investment.
- Put option hedge: downside protection with upside participation, reduced by the option premium.
- Unhedged position: full exposure to exchange-rate movements.

The forward and money-market results may differ because the model uses market-rate proxies and a scenario-indicative forward quote.

The sensitivity analysis must support the financial recommendations developed in Stage 4.
---

## 6. Outputs

The Excel workbook must display the results of each hedging strategy for Scenario 18, including USD proceeds, hedge profit relative to the unhedged position, and sensitivity to exchange-rate movements.

Required outputs include:

- A summary comparing all hedging strategies.
- USD proceeds under the unhedged position.
- Locked-in USD proceeds under the forward hedge.
- USD proceeds under the money-market hedge.
- Net USD proceeds under the put option hedge.
- Hedge profit or loss relative to no hedge.
- A formula-driven sensitivity table and chart.
- Validation results and documented modeling limitations.

### 6.1 Computed Base-Case Values

The base case assumes the settlement spot exchange rate equals the initial spot rate, `S_T = S0_in`.

The following calculations use the current model inputs:

- `FC_AMT = 5,600,000 EUR`
- `S0_in = 1.1081 USD/EUR`
- `F0_in = 1.1807 USD/EUR`
- `R_USD = 4.65%`
- `R_FC = 3.293%`
- `T_DAYS = 365`
- `K_PUT = S0_in`
- `PREM_PUT = 0.0245 USD/EUR`

| Strategy | USD Proceeds (Receivable) | USD Outlay (Payable) | Hedge Profit vs. No Hedge |
|---|---:|---|---:|
| No hedge | $6,205,360.00 | N/A | $0.00 |
| Forward | $6,611,920.00 | N/A | +$406,560.00 |
| Money market | $6,287,977.88 | N/A | +$82,617.88 |
| Put option | $6,068,160.00 | N/A | -$137,200.00 |

### 6.2 Base-Case Interpretation

At the base-case settlement exchange rate of 1.1081 USD/EUR, the forward hedge produces the highest USD proceeds of $6,611,920.00.

The money-market hedge produces $6,287,977.88, exceeding the unhedged position by $82,617.88.

The put option provides downside protection while preserving potential upside from EUR appreciation. However, at the base-case exchange rate, the premium reduces net proceeds to $6,068,160.00.

The difference between the forward and money-market hedge results reflects the use of a scenario-indicative forward quote and unmatched market interest-rate proxies. This difference must be documented and evaluated through the parity validation check.

These figures are illustrative checkpoints for Stage 3. The Excel workbook must calculate them dynamically using formulas rather than manually entered values.


---


## 7. Model Review - What Worked & What to Improve

### 7.1 What Worked

The Stage 2 specification establishes a structured financial model for Scenario 18, involving a EUR 5,600,000 receivable due in 365 days.

The model compares four strategies: remaining unhedged, entering a forward contract, establishing a money-market hedge, and purchasing a EUR put option.

The calculation flow uses standardized variables and explicitly identifies the three money-market hedge steps: borrowing EUR, converting EUR to USD at the spot exchange rate, and investing USD until settlement.

The model also includes a sensitivity analysis that evaluates how exchange-rate fluctuations affect USD proceeds. These features provide a foundation for the Excel workbook developed in Stage 3.

### 7.2 What to Improve

Reviewing the original specification identified several areas requiring improvement before the AI-assisted workbook build.

First, the original document contained overlapping calculation instructions that could produce inconsistent results. These were consolidated into one calculation flow.

Second, the option premium treatment was inconsistent. The revised specification uses a simplified nominal-premium convention and clearly states that premiums are not future-valued.

Third, the specification required clearer worksheet definitions, consistent variable naming, and explicit validation rules.

Finally, the model uses market-rate proxies and a scenario-indicative forward rate. The resulting covered-interest-parity difference must be calculated and reported rather than assumed to equal zero.

### 7.3 Validation and Auditability Requirements

The Stage 3 workbook must include the following calculated validation checks.

1. **Named-range validation:** All ten standardized inputs must have named ranges referencing the correct cells on the Inputs worksheet.

2. **Formula integrity:** Every calculated output must use an Excel formula. Hard-coded numerical results are not acceptable in calculated cells.

3. **Forward hedge verification:** Forward USD proceeds must equal `FC_AMT * F0_in`.

4. **Money-market hedge verification:** The workbook must calculate EUR borrowing, USD conversion, and USD investment in three separate steps using the specified ACT/360 convention.

5. **Covered-interest-parity check:** Calculate `PARITY_GAP_PCT = ABS(MM_PROCEEDS - FWD_PROCEEDS) / ABS(FWD_PROCEEDS)`. Report PASS if the difference is within 0.01% and FAIL otherwise. A failure must be documented rather than artificially corrected.

6. **Put option verification:** Confirm that the put payoff equals `FC_AMT * MAX(S_T, K_PUT) - FC_AMT * PREM_PUT`, following the nominal-premium convention.

7. **Sensitivity analysis:** Confirm that all eleven scenarios from -5% to +5% recalculate automatically when the relevant named inputs change.

8. **Calculation error detection:** Verify that calculated cells contain no Excel errors, including `#REF!`, `#DIV/0!`, `#VALUE!`, or `#NAME?`.

9. **Base-case verification:** Compare the workbook results at `S_T = S0_in` against the base-case calculations recorded in Section 6.

10. **Color conventions:** Yellow identifies inputs, blue identifies assumptions, green identifies formulas, and gray identifies outputs. The Legend worksheet must explain these conventions.

### 7.4 Stage 3 Audit Expectations

The AI-generated workbook is considered a draft until it has been independently reviewed.

The audit report must document at least three substantive findings. Each finding must identify:

- What was checked.
- What the reviewer found.
- What was corrected or independently confirmed.

Any failed validation checks must remain visible in the workbook and be discussed in the audit report.

All identified issues must be corrected through changes to the specification or workbook, as appropriate, and retested before final submission.

---


## 8. Limitations & Next Steps

### 8.1 Model Limitations

The FX hedging model evaluates a EUR 5,600,000 receivable due in 365 days for Scenario 18. Although the model compares several hedging strategies, it has the following limitations:

- **Market-data limitations:** The EUR/USD spot exchange rate and interest rates are based on dated market observations or proxies. The forward rate and option premiums are scenario-indicative assumptions rather than executable dealer quotes.
- **Interest-rate assumptions:** The money-market hedge assumes the firm can borrow EUR and invest USD at the specified market proxy rates. Actual corporate borrowing and deposit rates may differ.
- **Simplified option premiums:** The model deducts option premiums at nominal USD value without future-valuing the initial cash outflow to settlement.
- **Transaction costs:** Bid-ask spreads, brokerage fees, commissions, and additional financing costs are excluded.
- **Credit and counterparty risk:** The model assumes the shipyard pays the full receivable at settlement and that hedge counterparties fulfill their obligations.
- **Hedge accounting and taxes:** The model excludes tax consequences and hedge accounting requirements.
- **Scope:** The model evaluates one currency receivable with one settlement date. It does not consider partial, layered, or dynamic hedging strategies.

These limitations must be considered when interpreting model results or making financial recommendations.

### 8.2 Next Steps

Stage 3 will use this specification to generate a functioning Excel workbook containing all ten named inputs, the forward, money-market, and option hedge calculations, a sensitivity table and chart, and visible validation checks.

The workbook will be independently audited, with at least three substantive findings documented in a separate audit report.

Stage 4 will update the model using the required market observations and examine changes in hedge performance.

Stage 5 will use the model's results to prepare an executive decision memo comparing hedging strategies and recommending an appropriate approach to managing the EUR receivable.

---

## 9. Stage 3-5 Implementation Handoff

This specification serves as the technical blueprint for developing and validating the FX hedging workbook.

### Stage 3 - AI-Assisted Build and Audit

The workbook must implement the standardized inputs, calculation formulas, worksheet architecture, sensitivity analysis, and validation requirements in this specification.

All calculated outputs must contain formulas referencing the required named inputs. The model must be audited before submission, and at least three substantive findings must be documented.

### Stage 4 - Market Data and Population

The workbook will be updated with the required market data and source documentation. The analysis will evaluate the effect of updated exchange rates, interest rates, and option quotes on the relative performance of hedging strategies.

### Stage 5 - Executive Decision Memo

The validated workbook and sensitivity results will support an executive recommendation. The memo will consider cash-flow certainty, downside protection, upside participation, hedging costs, and the model's limitations.


---


## Appendix A - Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 0.1 | 2026-11-09 | Claudya Graham | Initial Stage 2 FX hedging model specification for Scenario 18. |
| 0.2 | 2026-11-13 | Claudya Graham | Revised specification following instructor feedback. Updated market inputs, standardized workbook architecture, removed duplicate calculation instructions, clarified option premium treatment, added base-case calculations, and strengthened validation and audit requirements. |

