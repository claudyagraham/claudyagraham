# Prompt log


## 2026-11-13: Stage 2 FX Hedging Model Specification

**What I asked:** I used Claude to help review and proofread my Stage 2 FX hedging specification for Scenario 18, involving a EUR 5,600,000 receivable due in 365 days. After receiving instructor feedback, I used ChatGPT to help identify and correct issues in the specification before beginning the Stage 3 Excel workbook.

**What went wrong:** My original specification contained duplicate calculation instructions, inconsistent treatment of option premiums, outdated example inputs, and incomplete validation requirements. Some calculations were based on template values rather than my specific scenario. These inconsistencies could have caused the AI to generate an incorrect workbook.

**How I caught it:** I reviewed my professor's feedback and compared it with the specification saved in GitHub. I identified conflicting calculation sections, outdated GBP examples, placeholder inputs, and missing workbook requirements.

**What I changed:** I revised the specification to use one consistent calculation flow, clarified the nominal-premium convention for options, added worksheet architecture, updated the input table, and strengthened the validation and audit requirements. I also revised the sensitivity plan and base-case calculations to better support the Stage 3 workbook.

**What I would do differently:** I would organize the specification around one set of standardized variables and calculations from the beginning. I would also verify market inputs and financial formulas before using AI to generate a model, rather than relying solely on AI-generated explanations.

**AI Use:** Claude (Anthropic) assisted with the initial specification review. ChatGPT (OpenAI) helped interpret instructor feedback, identify inconsistencies, and draft revisions. I reviewed and edited the specification before committing the changes to GitHub.

AI use: Claude (Anthropic) did the arithmetic and drafted the brief; I reviewed and edited it.

----


## 2026-11-13: Stage 3 FX Hedging AI-Assisted Build and Audit

**What I asked:** I used ChatGPT to generate a working Excel workbook from my revised Stage 2 FX hedging specification. I requested ten named inputs, forward and money-market hedge calculations, option payoffs, a dynamic sensitivity table and chart, and visible validation checks.

**What went wrong:** During my review, I discovered that the EUR borrowing amount was incorrectly formatted with a dollar symbol. The covered-interest-parity check also reported a 4.8994% difference between forward and money-market proceeds, exceeding the specified 0.01% threshold.

**How I caught it:** I opened the workbook in Microsoft Excel, inspected the money-market calculations and validation results, and tested the model by changing the initial spot rate from 1.1081 to 1.1200. The sensitivity calculations updated correctly.

**What I changed:** I corrected the EUR borrowing currency formatting, retained and documented the failed parity validation, and restored the original spot rate after testing. I also reviewed the sensitivity chart and its four hedge strategies.

**What I would do differently:** I would verify original market-data sources earlier, check currency formatting immediately, and test model recalculation before relying on any AI-generated results.

**AI Use:** ChatGPT (OpenAI) generated the initial workbook and assisted with interpreting the results and preparing the audit documentation. I independently reviewed the workbook in Excel and performed the recorded checks.
