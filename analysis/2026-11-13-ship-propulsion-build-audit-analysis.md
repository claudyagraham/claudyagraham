# Stage 3 — FX Hedging Model Build and Audit Analysis

**Course:** FIN 321 — International Finance and Securities  
**Scenario:** 18 — Ship Propulsion, EUR Receivable  
**Analyst:** Claudya Graham  
**Audit date:** November 13, 2026  
**Workbook:** `capabilities/fx-hedging/model.xlsx`  
**Specification:** `capabilities/fx-hedging/spec.md`

## 1. Build overview

An AI-assisted Excel workbook was constructed from my revised Stage 2 specification to compare no hedge, forward, money-market, and EUR put hedging strategies for a EUR 5,600,000 receivable due in 365 days. The workbook contains separate sheets for inputs, calculations, sensitivity analysis, a legend, and supporting notes. I opened the workbook in Microsoft Excel for Mac and reviewed its displayed results, calculation checks, and sensitivity behavior.

## 2. Audit findings

### Finding 1 — Currency presentation error in the money-market hedge

**What I checked:** The three explicit money-market steps on the `Calc_MoneyMarket` sheet, including the currency units shown for the initial borrowing.

**What I found:** The EUR borrowing result was initially displayed with a dollar sign, even though the calculation represented euros. The calculation itself produced 5,419,071.50 units of EUR. The subsequent USD conversion and invested settlement proceeds were $6,004,873.13 and $6,287,977.88, respectively.

**What I did:** I changed the number format of the borrowing result to euros. The corrected cell displayed **€5,419,071.50**. The numerical value and subsequent formulas were unchanged.

### Finding 2 — Forward/money-market parity threshold failed

**What I checked:** The money-market hedge result against the forward hedge and the visible 0.01% parity tolerance check.

**What I found:** Money-market proceeds were **$6,287,977.88**, versus forward proceeds of **$6,611,920.00**. The calculated absolute parity gap was **4.8994%**, and Excel correctly displayed **FAIL**. The implied forward rate was approximately **1.122853 USD/EUR**, compared with the model's scenario-indicative forward quote of **1.1807 USD/EUR**.

**What I did:** I retained the failed validation status rather than changing input values to force a pass. The specification uses market-rate proxies that are not matched to the scenario-indicative forward quotation; the mismatch is documented as a model limitation and remains visible in the workbook. The reported rates and source dates have not been independently verified against the original publications.

### Finding 3 — Sensitivity table dynamically recalculated

**What I checked:** Whether the 11-row sensitivity table responded to a temporary change in the named spot input `S0_in`.

**What I found:** Changing `S0_in` from **1.1081** to **1.1200** moved the zero-change settlement spot rate from **1.108100** to **1.120000**. At that row, unhedged proceeds changed from **$6,205,360.00** to **$6,272,000.00**, and money-market proceeds changed from **$6,287,977.88** to **$6,355,505.12**. Forward proceeds remained **$6,611,920.00**, as expected for a fixed forward quote. The seven visible sensitivity checks continued to report **PASS**.

**What I did:** I restored `S0_in` to **1.1081** and saved the workbook. A subsequent screenshot showed the original sensitivity amounts again.

### Finding 4 — Sensitivity chart visual inspection

**What I checked:** Whether the sensitivity chart visibly contained the required strategy comparisons.

**What I found:** The chart displayed four series—Unhedged, Forward, Money Market, and Put Net. The forward and money-market lines were horizontal, while the unhedged line increased with settlement spot and the put net line had a floor followed by increasing proceeds. The initial chart screenshot did not show a chart title or descriptive axis titles.

**What I did:** I identified the missing chart labels for correction in Excel. **Before submission, confirm that the saved workbook has the chart title “FX Hedging Strategy Comparison,” horizontal axis title “Settlement Spot Rate (USD/EUR),” and vertical axis title “USD Proceeds.”** This item is not yet confirmed complete by the screenshots reviewed.

## 3. Validation summary and remaining checks

- The visible money-market borrowing, conversion, and investment-step checks reported **PASS**.
- The visible sensitivity checks for 11 scenarios, minimum and maximum grid, base spot, flat forward and money-market outcomes, and put base-case reconciliation reported **PASS**.
- The covered-interest-parity threshold reported **FAIL**, with its 4.8994% gap retained and explained above.
- The market source figures listed for November 12, 2026 are still marked **not verified** in the workbook, so they should not be presented as independently confirmed quotes.
- **Before submission:** confirm the ten named input ranges in Excel's Defined Names/Name Manager, confirm the chart titles, and re-save the final workbook. The screenshots reviewed do not establish completion of the name-manager inspection or the chart-title correction.

## 4. Conclusion

The tested calculations and sensitivity results behaved consistently with the supplied model assumptions, and the Excel workbook exposed rather than concealed its parity failure. The audit identified and corrected a EUR currency-formatting issue and independently tested sensitivity recalculation. Remaining checks and data-source limitations are stated explicitly so that the workbook is not represented as more thoroughly verified than it is.
