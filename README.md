# Excel Agent \- "Financial Model from Structured Inputs", MVP Specification

**Wu Kun ([wukun2005@gmail.com](mailto:wukun2005@gmail.com)), Jan. 2026**

## Demo: [DemoVideo.mp4](https://1drv.ms/v/c/203d7a34c28a5187/IQBp1-UH8gl9SqjJkJ520d9JAad6qSS1ImpMMigcVgaSxGA?e=LoYmlk)

## 0\. Motivation

This finance model scenario is a simplified but representative abstraction. It reflects a common pattern in Excel finance workflows where users start from structured assumptions and iteratively build and validate models. The MVP focuses on validating that pattern before scaling to more complex real-world cases.

### 0.1 Which real Excel finance workflows does this scenario represent?

This scenario represents a **very common Excel moment**: a finance user opens a spreadsheet, pastes or types a small set of assumptions, and then spends the next 10–20 minutes **creating sheets, wiring formulas, copying across scenarios, and checking for errors**.

In today’s Excel, **the user already has the inputs in cells** — the pain starts when they need to turn those cells into a consistent, multi-scenario model.

In real Excel usage, this typically appears in workflows like:

* scenario-based forecasting (base / best / worst case),

* budget planning for new initiatives,

* or quick sensitivity analysis before a more detailed model is built.

The MVP intentionally focuses on the **first 10–20 minutes of a finance workflow**, where users are repeatedly doing manual setups: defining assumptions, wiring formulas, and validating correctness.

This is not meant to replace complex, long-lived finance models, but to accelerate the **model creation and iteration phase**, which is both frequent and error-prone in day-to-day Excel usage.

### 0.2 What evidence do we have that this scenario reflects day-to-day user behavior?

There are three layers of evidence behind this scenario.

**First**, from existing Excel usage patterns and telemetry, finance users heavily rely on Excel for ad-hoc modeling and scenario analysis, not just for final, polished financial statements.

**Second**, industry research from firms like Gartner and McKinsey consistently shows that a significant source of productivity loss in finance work comes from manual model setup, formula wiring, and validation—not from the final analysis itself.

**Third**, while this MVP is simplified, it intentionally captures behaviors that are very common in day-to-day work: users repeatedly adjusting assumptions, sanity-checking outputs, and asking “what if” questions.

The goal of the MVP is not to claim this exact model exists in isolation, but to validate whether Excel Agent can reliably support this **class of behavior** at scale.

### 0.3 Why This Scenario First?

**Value**: Finance model is high value for organization decision-making within decision-making window. 

**Frequency**: Financial modeling is one of the most frequent professional use cases in Excel.

**Severity**: Modeling work for human effort is high cost of errors; high cost of rework.

**Adoption Wedge**: Users are already in Excel. There is no need to learn new tools or paradigms.

**Feasibility**: Inputs are structured (cells). Outputs are native Excel objects (sheets / formulas).

👉 This is a "low psychological barrier" scenario for safely introducing an Agent into Excel.

## 1\. Problem Statement & User Context

### 1.1 Target Users

This scenario primarily targets **Finance / BizOps users** in medium to large organizations, such as Finance Managers, Business Operations Analysts and/or Strategy & Planning teams

These users typically work inside Excel for hours every day, often starting from **messy but structured tables**, and iteratively building models across multiple sheets. They frequently need to:

* Build or adjust financial models for planning and decision-making.  
* Explore multiple business scenarios under uncertainty.  
* Communicate assumptions and outcomes clearly to stakeholders.

### 1.2 Core User Pain Points

Top pain points are surfacing industry reports. For examples: 

*McKinsey – FP\&A Digital Transformation* consistently emphasizes

* Financial modeling remains heavily dependent on Excel.  
  * The costs of model building and scenario iteration are high.  
    * Executives care most about speed \+ trust.

*BCG / Bain – AI in Finance* highlights*:*

* The biggest barrier to automation is not model capability, but rather auditability & explainability.

*Gartner – Augmented Analytics* explicitly states:

* "*Lack of transparency*" is the top blocker to AI adoption in Finance.

Per these reports, users face several recurring challenges:

* **High manual effort** to build initial financial models from scratch.  
* **Low iteration speed** when adjusting assumptions (e.g., growth rate, cost structure).  
* **Trust concerns** with automated or AI-generated financial outputs.  
* **Poor auditability**, where it is hard to trace how a number is derived.

## 2\. Product Goal & Value Proposition

### 2.1 Product Goal

Enable users to **quickly generate an auditable and adjustable financial model** from structured business assumptions, while maintaining full transparency and user control.

### 2.2 Value Proposition

The Agent experience should:

* Reduce time to first usable financial model.  
* Make assumptions explicit and easy to modify.  
* Support scenario-based thinking (Bear / Base / Bull).  
* Preserve trust through transparency and explainability.

The Agent does **not** replace financial judgment; it accelerates model creation and iteration. As a result, transformative value is as natural as that finance users can now go from business assumptions to a fully auditable, scenario-based financial model in minutes instead of days—without sacrificing transparency, control, or trust.

### 2.3 Structured Inputs

Users provide a set of structured assumptions, for example:

* Time horizon (e.g., 3 years).  
* Revenue assumptions: Initial revenue, Growth rate.  
* Cost assumptions: Gross margin, Operating expenses.  
* Tax rate.  
* Scenario parameters: Bear / Base / Bull variations (e.g., different growth rates).

These inputs are captured via structured tables in Excel, forms, or clearly labeled input ranges.

### 2.4 Scenario Outputs

For each scenario (Bear / Base / Bull), the model produces Revenue forecast, Cost breakdown and Profit / cash flow summary.

All outputs remain **formula-driven**, not static numbers.

## 3\. MVP

This MVP intentionally limits free-form interaction and instead focuses on **high-signal, low-friction interactions** embedded into the workflow. Excel users are not looking for speed alone. They want **control, transparency, and confidence**. 

This MVP is designed around those expectations. This is not an automation-first MVP, it’s a trust-first MVP.

### 3.1 Scope

The MVP intentionally limits scope to ensure reliability:

* Single product or business line.  
* Simplified financial structure (Revenue, Cost, Profit).  
* 3-year projection.  
* Three predefined scenarios (Bear / Base / Bull).

### 3.2 Agent Responsibilities

Agent is responsible for:

* Translating structured inputs into a standard financial model template.  
* Generating formulas (not hard-coded values).  
* Clearly labeling all assumptions.  
* Highlighting scenario differences.

Agent explicitly does **not**:

* Override user assumptions.  
* Introduce hidden heuristics.  
* Produce unverifiable outputs.

### 3.3 Out of Scope

* For better auditability and explainability, complicated finance models like DCF, IRR, Deprecation, etc. are NOT supported.  
  * For better human-trust earnings, focus on Excel formula generation, NOT hard-coded value.   
  * For simplifying purposes, the agent is NOT a general-purpose finance agent. 

### 3.4 Workflow & Interaction

Agent helps finance users quickly go from structured assumptions to a transparent, auditable financial model through guided interaction and iterative refinement.

🔴 **Before Agent (Current Workflow)**

1. User manually creates:

* Revenue assumptions

* Cost assumptions

2. Designs formulas across sheets

3. Builds scenarios (Base / Bull / Bear) one by one

4. Debug errors

5. Adjust assumptions → repeat

| Pain Point | Time Lost | Risk Level |
| ----- | ----- | ----- |
| Model structure setup | 45-60 min | Low |
| Formula writing & linking | 2-3 hours | **High** (errors propagate) |
| Scenario logic (Bear/Base/Bull) | 30-45 min | **Medium** |
| Error checking & auditing | 30-60 min | **High** (missed errors \= bad decisions) |
| Formatting & polish | 20-30 min | Low |

The opportunities to improve are obvious as following:

❌ Time-consuming  
❌ High cognitive load  
❌ Error-prone

**Where Time/Risk Concentrates:** Formula construction and scenario linking—this is where **70%** of time goes and where errors cause the most downstream harm.

🟢 **With Agent （Proposed Workflow）**

Workflow is divided into 5 steps, each of which has interaction between Agent and Users. 

**Step 1: Structured Input Confirmation （Interaction \#1）**

**UI**

* Excel shows a **predefined “Assumptions” section**

* Agent highlights detected inputs:

  * Revenue drivers

  * Cost drivers

  * Growth assumptions

**Agent Prompt example:**

“I detected revenue growth, COGS, and operating expense assumptions. Do you want to build a financial model based on these inputs?”

👉 Users explicit confirm or edits the assumptions. ✅ This is the 1st interaction. 

***Step 2: Model Intent Clarification (Interaction \#2)***

Agent asks **one constrained question** (not open-ended): “What type of model do you want to build?”

* A: 3-statement projection

* B: P\&L only

* C: Scenario comparison (Base / Bull / Bear)

👉 Users select one of options, instead free input.  ✅ Dis-ambiguity purpose interaction

**Step 3: Model Generation with Transparency**

Agent generates:

* Structured formulas

  * Clearly labeled sheets

* Each section includes:

  * Formula explanation (inline comment)

  * Source assumptions reference

**Example**

\=Revenue\_Assumptions\!B2 \* (1 \+ Growth\_Rate)

With tooltip: “Revenue calculated based on Year 1 revenue and growth rate assumption.”

👉 No black-box generation

***Step 4: Iterative Refinement (Interaction \#3)***

User changes an assumption (e.g. growth rate), 

Agent proactively responds “Revenue, Profit, and cash flow are impacted. Do you want to update all scenarios?”, 

User input **Yes / Review first.** ✅ Interaction \+ Trust building 

**Step 5: Verification & Confidence Signals**

Agent shows:

* Formula consistency check ✅

* Scenario completeness check ✅

* Out-of-range warnings ⚠️

**Agent Prompt example**:

“Bull case margin exceeds the historical range. Would you like to review?”

👉 Auditability, Verifiability first is always priority for Trust

**Workflow & Interaction Design Summary:**

| Workflow & Interaction \# | Purpose |
| ----- | ----- |
| Input confirmation, \#**1** | Reduce hallucination |
| Model intent selection, \#**2** | Reduce ambiguity |
| Inline explanations | Build trust |
| Iteration prompts, \#**3** | Support real workflow |
| Warnings & checks | Prioritize accuracy |

The alternative flow sample can be found in [Appendix 2: Alternative Happy Path Flow](#2.-alternative-happy-path-flow). 

## 4\. Guardrails & Trust Design

### 4.1 Transparency Guardrails

* All AI-generated cells are tagged or commented as “AI-generated”.  
* Each output can be traced back to a specific assumption.  
* No silent inference to missing financial assumptions.

### 4.2 Control Guardrails

* Users can manually edit any assumption.  
* Model recalculates deterministically after edits.  
* Users can disable Agent suggestions at any time.

### 4.3 Risk Mitigation

* Clear disclaimer: “This model is for planning support, not financial advice”.  
* Encourage user validation before external use.

## 5\. Demo: [DemoVideo.mp4](https://1drv.ms/v/c/203d7a34c28a5187/IQBp1-UH8gl9SqjJkJ520d9JAad6qSS1ImpMMigcVgaSxGA?e=LoYmlk)

### ![][image1]5.1 Demo Flow

1. Users provide structured assumptions.  
2. Per user's explicit request query, Agent is invoked to:  
   1. Generate model structure.  
   2. Create formulas for referencing input cells.  
3. User switches between Bear / Base / Bull assumptions.  
4. Output updates automatically.  
5. User inspects formulas to validate logic.

### 5.2 Key Demo Message

Agent accelerates model creation while keeping humans fully in control.

## 6\. Evaluation

### 6.1 Core Metrics

1. **Correctness (Correct Rate)**  
   * Are generated numbers mathematically correct?  
   * Are formulas strictly derived from provided inputs?  
   * No hallucinated assumptions  
2. **Relevance**  
   * Are outputs strictly related to financial modeling?  
   * No irrelevant narrative or unrelated calculations  
3. **Completeness (Recall):** Does the output cover all expected components?  
   * Revenue  
   * Cost  
   * Profit  
   * Cash Flow  
   * Scenario differentiation

### 6.2 Evaluation Set

We maintain a **small but high-quality golden evaluation set** to validate Agent outputs before and after each iteration.

🟢 **Good Case (Expected to Pass)**

* **Structured Inputs**

  * Initial Revenue: 10M  
  * Growth Rate (Base): 12%  
  * Gross Margin: 65%  
  * Tax Rate: 20%  
  * Time Horizon: 3 years  
* **Query:** “Generate a 3-year financial model with revenue, cost, profit, and cash flow based on the provided assumptions.”  
* **Expected Output**

* Correct year-over-year revenue growth.  
* Cost is derived from gross margins.  
* Profit \= Revenue – Cost  
* Cash flow correctly applies tax.  
* All outputs generated as formulas referencing inputs.

🔴 **Bad Case (Expected to Fail)**

* **Structured Inputs**

  * Missing tax rate;  
  * Conflicting growth assumptions;  
  * Incomplete scenario table;  
* **Query:** “Generate a complete financial model.”

* **Expected Behavior: Agent shall** 

  * Flag missing or conflicting assumptions  
  * Ask for clarification  
  * **Not generate fabricated numbers**

👉 **Failing Safely is success here.**

🌕 More Samples: 

**Tier 1: Must Pass (20 queries) \- Golden Cases**

*1\. "Create a 12-month revenue forecast starting at $100K with 10% monthly growth"*  
*2\. "Build Bear/Base/Bull scenarios for headcount: 50/75/100 employees"*  
*3\. "Show monthly burn rate if revenue is X and costs are Y"*

**Tier 2: Should Pass (20 queries) \- Edge Cases**

*1\. "Model our runway with current burn and two funding scenarios"*  
*2\. "Create a unit economics table with CAC, LTV, and payback period"*  
*3\. "Build a cohort retention model from these percentages: \[list\]"*

**Tier 3: Graceful Degradation (10 queries) \- Edge / Adversarial case**

*1\. "Make a financial model" (too vague → should ask clarifying questions)*  
*2\. "Build a DCF model with WACC" (too complex for MVP → should explain limitation)*  
*3\. "Model the economy" (out of scope → should redirect)*

### 6.3 Offline \+ Online Evaluation

Offline: 

* Run Agent against golden evaluation sets.  
* Track regression on: Correctness, Completeness, Relevance and Failure Safety

| Eval Set | Size | Pass Criteria |
| ----- | ----- | ----- |
| **Golden Queries** | 50 canonical financial model requests | 100% produce valid formulas |
| **Edge Cases** | 30 ambiguous/complex requests | 80% either succeed or gracefully ask for clarification |
| **Adversarial** | 20 requests designed to cause errors | 0% produce incorrect calculations silently |

Online

* A/B test with internal finance users.  
* Monitor:  
  * **Product Metrics**

| Metric | Definition | Target | Measurement |
| ----- | ----- | ----- | ----- |
| **Task Completion Rate** | % of "build model" intents that result in working model | ≥70% | Intent detected → formulas inserted → no *immediate (?)* undo |
| **Time to First Value** | Time from first message to working formulas in sheet | \<5 min | Timestamp tracking |
| **Formula Accuracy** | % of generated formulas that calculate correctly | ≥95% | Automated validation against expected outputs |
| **User Edit Rate** | % of generated formulas user modifies | \<30% | Track cell edits within 10 min of generation |
| **Return Usage** | Users who build 2+ models within 14 days | ≥40% | Cohort tracking |

  * **Trust & Quality Metrics**  
    * User-reported confidence score  
      * Audit-ability satisfaction  
        * Adoption rate among repeat users  
  * **Business Impact Metrics** 

| Metric | Hypothesis | Leading Indicator |
| ----- | ----- | ----- |
| **Activation** | 40% of trial users who attempt financial model task complete it successfully | First model completion within 7 days |
| **Retention** | Users who build 2+ models in month 1 retain at 3x rate | Weekly active model-building sessions |
| **Expansion** | Finance user → team adoption; 1 seat → 5 seats within 90 days | Shared workbook collaboration |
| **Time-to-Value** | First "wow" moment in \<10 minutes (seeing formulas auto-generated) | Time to first successful formula insertion |

### 6.4 Guardrails & Thresholds

* **MUST NEVER happen \- P0 blockers**

| Guardrail | Example | Detection | Response |
| ----- | ----- | ----- | ----- |
| **Silent calculation error** | Formula returns wrong number without warning | Automated validation against test cases | Block release; require fix |
| **Formula injection** | User input becomes executable code | Input sanitization; formula allowlist | Reject input; log incident |
| **Data destruction** | Agent overwrites user data without confirmation | Preview-before-apply; undo stack | Always preview; 1-click undo |
| **Circular reference** | Generated formulas create infinite loop | Pre-insertion validation | Warn user; suggest fix |
| **Hardcoded values** | Agent puts numbers where formulas should go | Formula pattern detection | Flag for review; suggest formula |

* **Correctness threshold**:   
  * ≥ 95% on golden sets;  
  * Zero silent errors on adversarial set.  
  * Undo rate ≤ 15% (users accept output)  
* **Completeness threshold**: All required sections present  
* **Safety guardrail**: Missing inputs must block generation, not hallucinate  
* **“Bad Outcome”** Definition: Even if user does not notice immediately,

| Bad Outcome | Why It's Bad | How We Detect |
| ----- | ----- | ----- |
| **Off-by-one errors in date ranges** | Month 12 shows Month 11 data; compounds in downstream analysis | Automated boundary testing |
| **Scenario logic inverted** | "Bear" shows Bull numbers; user makes wrong decision | Named scenario validation |
| **Reference drift** | Copy-paste breaks relative refs; errors appear in later columns | Full-model recalculation check |
| **Assumption disconnection** | Changing input doesn't update outputs; model looks dynamic but isn't | Dependency graph validation |
| **Formatting masks errors** | *\#REF\!* error hidden by conditional formatting | Pre-format error scan |

**Detection Strategy:**

1. **Shadow calculation:** Agent recalculates expected values independently  
2. **Mutation testing:** Change inputs, verify outputs change correctly  
3. **User feedback loop:** "Did this model work for you?" prompt after 24 hours

## 7\. Summary

This is a simplified case for illustrative purposes, but it is not an arbitrary one.

I intentionally chose a finance model with a **clear structure, explicit assumptions, and strong auditability,** because the goal of this MVP is NOT to prove domain complexity (as mentioned in section “3.3 Out of Scope”), but to validate the **reliability of Excel Agent across the chain from structured inputs → formulas → explainability**.

If the agent cannot perform reliably in a controlled, well-defined scenario, introducing more complex real-world cases would only amplify the risk.

This scenario demonstrates how AI Copilot can deliver high business value in a traditionally high-risk domain by:

* Starting from structured inputs

* Making all assumptions explicit

* Supporting scenario-based decision-making

* Maintaining transparency, control, and trust

The success of this product depends less on model sophistication, and more on **responsible AI design and user empowerment.**

This MVP does not introduce a new way to work. It **compresses an existing Excel workflow** that users already perform daily — without asking them to trust a black box. **MVP Success \= User says:** *"I described my model in plain English, watched formulas appear, changed the dropdown to Bull case, and everything updated correctly. I spent my time reviewing the logic instead of writing it."*

## Appendix

### 1\. Technical Architecture

#### *1.1 LLM Output Format*

**User Input:**

"*Build a SaaS revenue model. Starting MRR $50K, growth rates: Base 12%, Bear 5%, Bull 20%. Show 6 months with scenario toggle.*"

**Ideal LLM Output (structured for Excel execution):**

`{`  
  `"model_type": "revenue_projection",`  
  `"structure": {`  
    `"scenario_cell": "B1",`  
    `"scenario_options": ["Bear", "Base", "Bull"],`  
    `"default_scenario": "Base",`  
    `"headers_row": 2,`  
    `"data_start_row": 3`  
  `},`  
  `"assumptions": {`  
    `"starting_mrr": 50000,`  
    `"growth_rates": {`  
      `"Bear": 0.05,`  
      `"Base": 0.12,`  
      `"Bull": 0.20`  
    `},`  
    `"periods": 6`  
  `},`  
  `"rows": [`  
    `{`  
      `"label": "Monthly Revenue",`  
      `"row": 3,`  
      `"formulas": {`  
        `"B3": "=50000",`  
        `"C3": "=B3*(1+IF($B$1=\"Bear\",0.05,IF($B$1=\"Bull\",0.20,0.12)))",`  
        `"D3:G3": "=COPY(C3, adjust_references)"`  
      `}`  
    `},`  
    `{`  
      `"label": "Growth Rate",`  
      `"row": 4,`  
      `"formulas": {`  
        `"B4": "=IF($B$1=\"Bear\",0.05,IF($B$1=\"Bull\",0.20,0.12))",`  
        `"C4:G4": "=COPY(B4)"`  
      `},`  
      `"format": "percentage"`  
    `},`  
    `{`  
      `"label": "Cumulative Revenue",`  
      `"row": 5,`  
      `"formulas": {`  
        `"B5": "=B3",`  
        `"C5": "=B5+C3",`  
        `"D5:G5": "=COPY(C5, adjust_references)"`  
      `}`  
    `}`  
  `],`  
  `"validation": {`  
    `"expected_month6_base": 98706,`  
    `"formula_check": "all_cells_use_relative_refs_correctly"`  
  `}`  
`}`  
**What This Propose:**

1. **Structured extraction:** LLM parses natural language into typed assumptions  
2. **Formula generation:** Creates Excel-native formulas with proper reference locking ($B$1)  
3. **Scenario logic:** Uses nested IF for multi-scenario support  
4. **Self-validation:** Includes expected outputs for verification  
5. **Extensibility:** Structure allows easy addition of rows/columns

#### *1.2 High-level design*

┌─────────────┐     ┌──────────────────┐     ┌─────────────────┐  
│  `Chat UI`    │────▶│  `Intent Parser`   │────▶│  `Model Builder`  │  
│  `(React)`    │     │  `(LLM + Schema)`  │     │  `(Excel API)`    │  
└─────────────┘     └──────────────────┘     └─────────────────┘  
                            │                        │  
                            ▼                        ▼  
                    ┌──────────────────┐     ┌─────────────────┐  
                    │ `Assumption Store` │     │ `Formula Engine`  │  
                    │ `(validated JSON)` │     │ `(generation +`   │  
                    └──────────────────┘     │  `validation)`    │  
                                             └─────────────────┘  
**Key Engineering Decisions:**

1. **Formula templates:** Pre-built templates for common patterns (growth, cumulative, conditional) reduce LLM hallucination risk  
2. **Two-pass generation:** LLM generates structure → deterministic code generates exact formulas  
3. **Preview before apply:** Show user what will change before modifying workbook  
4. **Atomic operations:** Each model-building step is undoable

### *2\. Alternative Happy Path Flow* {#2.-alternative-happy-path-flow}

┌─────────────────────────────────────────────────────────────────────┐  
│                         `USER JOURNEY`                                 │  
└─────────────────────────────────────────────────────────────────────┘

`STEP 1: User Intent`  
┌──────────────────────────────────────────────────────────────────────┐  
│ `[CHAT PANEL]`                                                         │  
│                                                                      │  
│ `User: "I need to build a 12-month revenue model with 3 scenarios.`   │  
│        `Base case: 15% MoM growth, starting ARR $500K.`               │  
│        `Bear: 8% growth. Bull: 25% growth."`                          │  
└──────────────────────────────────────────────────────────────────────┘  
                                    │  
                                    ▼  
`STEP 2: Agent Confirms & Clarifies`  
┌──────────────────────────────────────────────────────────────────────┐  
│ `[CHAT PANEL]`                                                         │  
│                                                                      │  
│ `Agent: "I'll create a 12-month revenue projection with:`             │  
│         `• Row 1: Headers (Month 1-12)`                               │  
│         `• Row 2: Scenario selector (Bear/Base/Bull dropdown)`        │  
│         `• Row 3: Monthly Revenue with growth formulas`               │  
│         `• Row 4: Cumulative Revenue`                                 │  
│                                                                      │  
│         `Should I also include:`                                       │  
│         □ `Monthly growth rate row (for transparency)`                │  
│         □ `YoY comparison row`                                        │  
│                                                                      │  
│         `[Build Model] [Adjust Structure]"`                           │  
└──────────────────────────────────────────────────────────────────────┘  
                                    │  
                                    ▼  
`STEP 3: Agent Executes in Excel`  
┌──────────────────────────────────────────────────────────────────────┐  
│ `[EXCEL WORKBOOK]`                                                     │  
│                                                                      │  
│    │    `A`        │    `B`      │    `C`      │    `D`      │ `...`          │  
│ ───┼─────────────┼───────────┼───────────┼───────────┼─────          │  
│  `1` │ `Scenario:`   │ `[Base` ▼`]`  │           │           │               │  
│  `2` │             │  `Month 1`  │  `Month 2`  │  `Month 3`  │ `...`          │  
│  `3` │ `Revenue`     │ `$500,000`  │ `$575,000`  │ `$661,250`  │ `...`          │  
│  `4` │ `Growth Rate` │   `15%`     │   `15%`     │   `15%`     │ `...`          │  
│  `5` │ `Cumulative`  │ `$500,000`  │`$1,075,000` │`$1,736,250` │ `...`          │  
│                                                                      │  
│ `[Cells highlighted as Agent builds]`                                  │  
└──────────────────────────────────────────────────────────────────────┘  
                                    │  
                                    ▼  
`STEP 4: Agent Explains What It Built`  
┌──────────────────────────────────────────────────────────────────────┐  
│ `[CHAT PANEL]`                                                         │  
│                                                                      │  
│ `Agent: "`✓ `Model created in cells A1:M5`                              │  
│                                                                      │  
│         `Formula logic:`                                               │  
│         `• B3: =500000 (starting ARR)`                                │  
│         `• C3: =B3*(1+IF($B$1="Bear",0.08,IF($B$1="Bull",0.25,0.15)))`│  
│         `• Growth formulas reference the scenario dropdown in B1`     │  
│                                                                      │  
│         `Try changing the dropdown to 'Bear' to see projections`      │  
│         `update automatically.`                                        │  
│                                                                      │  
│         `[Explain C3 Formula] [Add Cost Section] [Export Summary]"`   │  
└──────────────────────────────────────────────────────────────────────┘  
                                    │  
                                    ▼  
`STEP 5: User Verifies & Iterates`  
┌──────────────────────────────────────────────────────────────────────┐  
│ `[CHAT PANEL]`                                                         │  
│                                                                      │  
│ `User: "Actually, make Bear case 10% not 8%, and add a row for`       │  
│        `monthly expenses at 60% of revenue"`                          │  
│                                                                      │  
│ `Agent: "Updated:`                                                     │  
│         `• Bear case growth: 8%` → `10%`                                │  
│         `• Added Row 6: Expenses = Revenue × 0.6`                     │  
│         `• Added Row 7: Net Margin = Revenue - Expenses`              │  
│                                                                      │  
│         `[Show Changes] [Undo Last Change]"`                          │  
└──────────────────────────────────────────────────────────────────────┘  


[image1]: <data:image/png;base64,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>
