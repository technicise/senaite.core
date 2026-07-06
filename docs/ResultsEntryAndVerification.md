# Results Entry & Verification

Results Entry & Verification covers the core of the laboratory's daily work
in SENAITE: capturing analysis results — whether typed manually, picked from
predefined options, computed by calculations or imported from instruments —
and pushing them through the review chain until they are verified and ready
for publication.

This guide explains the feature from an end-user perspective: where and how
results are entered, how calculations, uncertainties and detection limits
behave, how results are submitted and verified (including multi-verification
and self-verification), how analyses are retracted or retested, and what the
out-of-specification icons mean.

---

## Table of Contents

1. [The Analysis Life Cycle](#the-analysis-life-cycle)
2. [Key Concepts](#key-concepts)
3. [Where Results are Entered](#where-results-are-entered)
4. [Entering Results](#entering-results)
5. [Calculations and Interim Fields](#calculations-and-interim-fields)
6. [Result Types and Predefined Results](#result-types-and-predefined-results)
7. [Units](#units)
8. [Uncertainties](#uncertainties)
9. [Detection and Quantification Limits](#detection-and-quantification-limits)
10. [Decimal Mark, Precision and Scientific Notation](#decimal-mark-precision-and-scientific-notation)
11. [Results Out of Specification](#results-out-of-specification)
12. [Remarks on Analyses](#remarks-on-analyses)
13. [Submitting Results](#submitting-results)
14. [Verifying Results](#verifying-results)
15. [Multi-Verification](#multi-verification)
16. [Self-Verification](#self-verification)
17. [Retracting, Retesting and Rejecting Analyses](#retracting-retesting-and-rejecting-analyses)
18. [Frequently Asked Questions](#frequently-asked-questions)

---

## The Analysis Life Cycle

Each analysis on a sample follows its own workflow, coordinated with the
workflow of the sample it belongs to (see
[Sample Workflow](SampleWorkflow.md)):

| State | Meaning |
|-------|---------|
| **Registered** | The sample has not been received yet. Lab results cannot be entered; field results can (see below). |
| **Unassigned** | The sample is received and the analysis is ready for testing, but not placed on a worksheet. Results can be entered directly on the sample. |
| **Assigned** | The analysis has been placed on a [Worksheet](Worksheets.md). |
| **To be verified** | A result has been submitted and awaits verification. The result is no longer editable. |
| **Verified** | The result has been reviewed and approved. It can now be [published](ResultsPublication.md). |
| **Published** | The result has been included in a published results report. Final state. |
| **Retracted** | The submitted result was withdrawn; a retest copy was created automatically. |
| **Rejected** | The analysis was discarded without a replacement. |
| **Cancelled** | The whole sample was cancelled; its analyses are cancelled with it. |

The default role assignments are:

- Entering and submitting results requires the permission
  `senaite.core: Edit Results` (granted to Analyst, Lab Manager and Manager)
  or, for field analyses, `senaite.core: Edit Field Results` (Sampler, Lab
  Manager, Manager).
- Verifying requires `senaite.core: Transition: Verify` (Lab Manager,
  Manager, Verifier).
- Retracting requires `senaite.core: Transition: Retract` (in the
  *To be verified* state: Analyst, Lab Manager, Manager, Sampler).
- Retesting requires `senaite.core: Transition: Retest` and rejecting a
  single analysis requires `senaite.core: Transition: Reject Analysis`
  (both Lab Manager and Manager only, by default).

## Key Concepts

| Term | Meaning |
|------|---------|
| **Result** | The value captured for an analysis: a number, a text, a date, or one or more predefined options. |
| **Interim Field** | An intermediate value (e.g. vessel mass, dilution factor) entered alongside the result, usually feeding a calculation. |
| **Calculation** | A formula, configured in *Setup*, that computes the final result from interim fields and/or the results of other analyses. |
| **Uncertainty** | The ± value reported with a numeric result, taken from per-range settings or entered manually. |
| **Detection Limits (LLOD / ULOD)** | The lowest / highest concentration the method can reliably *detect*. Results outside are reported as `<` or `>` values. |
| **Quantification Limits (LLOQ / ULOQ)** | The lowest / highest concentration the method can reliably *quantify*. |
| **Submit** | The transition that finalises a captured result and sends the analysis to *To be verified*. |
| **Verify** | The review step (one or several, see multi-verification) that approves a submitted result. |
| **Retract** | Withdraw a submitted result. A fresh copy of the analysis (a *retest*) is created automatically. |
| **Retest** | Verify the current result *and* create a new copy for re-measurement. |

## Where Results are Entered

Results can be captured in several places — the same results table (and the
same rules) apply everywhere:

- **On the sample:** open a sample and use the analyses listing at the bottom
  of the sample view.
- **On a worksheet:** organise analyses from several samples into a
  [Worksheet](Worksheets.md) and enter results there — including QC analyses
  such as blanks, controls and duplicates (see
  [Quality Control](QualityControl.md)). The worksheet results table can be
  displayed in *Classic* layout (samples in rows) or *Transposed* layout
  (samples in columns); the default is set in *Setup* with **Default layout
  in worksheet view**.
- **From an instrument:** results can be imported from instrument result
  files instead of being typed (see [Instruments](Instruments.md)). Imported
  interim values and results land in the same fields described in this guide.

Timing rules:

- **Lab analyses** can only receive results once the sample has been
  **received**.
- **Field analyses** (services with *Point of Capture: Field*) can receive
  results as soon as the sample is *sampled*, even before reception —
  typically entered by the Sampler.
- The *Setup → Analyses* option **Immediate results entry** additionally
  allows results to be entered right after
  [sample registration](SampleRegistration.md), which is handy together with
  automatic sample reception.

Note that when a sample has been split, results are entered on each
[partition](SamplePartitions.md) individually, and samples registered as
[secondary samples](SecondarySamples.md) carry their own analyses like any
other sample.

## Entering Results

1. Open the sample (or worksheet) and locate the analysis row.
2. Type the value in the **Result** column — or pick it from the dropdown /
   checkboxes if the service defines predefined results. If the service has
   a **Default result** configured, the field comes pre-filled.
3. Fill in any **interim fields** shown as extra columns (see next section).
4. If applicable, adjust:
   - **Method** and **Instrument** — dropdowns listing the methods and
     instruments allowed for the service,
   - **Unit** — when the service defines several selectable units,
   - **± (Uncertainty)** — when manual uncertainty input is allowed,
   - **DL** — the detection limit selector (`<` / `>`), when enabled,
   - **Captured** — the result capture date. It is recorded automatically
     when the result changes, but can be set manually (never in the future)
     if the *Setup → Analyses* option **Allow to set the result capture
     date** is enabled (default).
5. Attach files with the attachment control if needed. Services flagged with
   **Attachment required for verification** display a warning icon in the
   *Attachments* column until a file is attached, and cannot be submitted
   without one.
6. Press **Save** to store the values without changing the workflow state —
   you can come back and edit them later.
7. When the results are final, select the analyses and press **Submit** (see
   [Submitting Results](#submitting-results)).

The **Analyst** column records who is responsible for the analysis; the
**Submitter** column records who actually submitted the result.

## Calculations and Interim Fields

Calculations are configured under *Setup → Calculations* and assigned to
analysis services (see [Analyses Setup](AnalysesSetup.md)). A calculation
consists of:

- **Calculation Interim Fields** — the intermediate values the formula
  needs. Each interim defines a *Keyword*, *Field title*, *Default value*,
  optional *Choices* (a predefined list of values for the interim), *Result
  type*, *Unit*, and the flags *Allow empty*, *Report*, *Hidden* and *Apply
  wide*.
- **Calculation Formula** — standard maths operators plus keywords in square
  brackets, e.g. `[Ca] + [Mg]`. Keywords can refer to the calculation's own
  interim fields or to the keywords of other analysis services, whose
  results then become *dependencies* of this analysis.
- **Additional Python Libraries** — optional imports (e.g. `floor` from
  `math`) that can be used in the formula.
- **Test Parameters** and **Test Result** — enter sample values for every
  parameter and the formula is evaluated on save, so you can check the
  calculation before using it in production.

How calculations behave at results entry:

- Interim fields appear as extra editable columns next to the result. As
  soon as all required values are available, the result is calculated
  automatically and shown in the (read-only) result field.
- An analysis **cannot be submitted** while a required interim field is
  empty — tick *Allow empty* on the interim if a blank value is acceptable.
- Interims flagged **Apply wide** offer an input at the top of the
  worksheet, letting you apply one value (e.g. an ambient temperature) to
  all matching fields on the sheet at once.
- When the formula depends on other analyses, those dependencies must have
  results before the dependent result can be calculated and submitted.
  Submitting or verifying a dependent analysis automatically carries its
  dependencies along.
- The formula and its imports are **frozen into each analysis when the
  analysis is created**. Editing a calculation in *Setup* later does not
  change analyses that already exist — only new ones.
- A division by zero displays `0/0` as the result; a formula that cannot be
  evaluated displays `NA`.

## Result Types and Predefined Results

Each analysis service has a **Result type** (*Result Options* tab of the
service, see [Analyses Setup](AnalysesSetup.md)) that controls the input
rendered at results entry:

| Result type | Input at results entry |
|-------------|------------------------|
| **Numeric** | A numeric field (the default). Supports uncertainties, detection limits, precision, etc. |
| **String** | A free-text single-line field. The result is reported exactly as typed. |
| **Text** | A free-text multi-line field. |
| **Selection list** | A dropdown with the predefined results. |
| **Multiple selection** | A selection allowing several predefined results at once. |
| **Multiple selection (with duplicates)** | As above, but the same option can be picked more than once. |
| **Multiple choices** | The predefined results displayed as checkboxes. |
| **Date** | A date picker. |
| **Datetime** | A date and time picker. |

For the selection-based types you must define **Predefined results**: a list
of options, each with a **Result Value** (a number, stored internally and
used by calculations) and a **Display Value** (the text shown to users and
printed on reports). When predefined results are set, no custom result can
be typed — the user has to choose from the list. The **Sorting criteria**
option controls the order of the options in the selection list (keep the
order defined, or sort by result/display value ascending or descending).

String, text, date and selection results are never interpreted numerically:
no uncertainty, detection limits or decimal formatting apply to them. String
results can still be used in calculation formulas of dependent analyses.

## Units

- The **Default Unit** of the service (e.g. `mg/l`, `ppm`) is displayed
  after the result and the uncertainty at results entry, and on reports.
- If the service defines **Units for Selection** (a list that should include
  the default unit), a **Unit** dropdown appears in the results table and
  the analyst can pick the unit that applies.

Note that selecting a different unit only changes the unit *recorded and
displayed* with the result — SENAITE does not convert the numeric value
between units.

## Uncertainties

Uncertainties are configured per service on the *Uncertainties* tab:

- **Uncertainty** ranges: rows of *Range min*, *Range max* and *Uncertainty
  value*. When a numeric result falls within a range, the corresponding
  uncertainty applies and the result is reported as `value ± uncertainty`
  (e.g. `6.67 ± 0.5`).
  - The uncertainty value may be given as a **percentage** of the result by
    appending `%` (e.g. `2%`).
  - Set the value to `0` (or below) to *not* display an uncertainty for that
    range.
  - Successive ranges should be continuous, e.g. `0.00–10.00`,
    `10.01–20.00`, …
- **Allow manual uncertainty value input**: when enabled, the **±** column
  becomes editable at results entry and the value typed by the analyst
  overrides the range-based default.
- **Calculate Precision from Uncertainties**: when enabled, the number of
  decimals of the reported result is derived from the (rounded) uncertainty
  instead of the fixed precision — e.g. a result of `5.243` with an
  uncertainty of `0.22` is reported as `5.2 ± 0.2`. If no uncertainty range
  matches, the fixed precision is used.

No uncertainty is stored or displayed when the result is a detection limit
or falls outside the quantifiable range (below LLOQ or above ULOQ), since
the measurement is not reliable enough to quantify.

## Detection and Quantification Limits

Four limits can be set per service on the *Limits* tab:

| Limit | Label in the service | Effect on reported results |
|-------|----------------------|----------------------------|
| LLOD | **Lower Limit of Detection (LLOD)** | Numeric results below it are reported as `< LLOD`, or as **Not detected** when an LLOQ different from the LLOD is defined. |
| LLOQ | **Lower Limit Of Quantification (LLOQ)** | Results between LLOD and LLOQ are reported as **Detected but < LLOQ**. |
| ULOQ | **Upper Limit Of Quantification (ULOQ)** | Results above it are reported as `> ULOQ`. |
| ULOD | **Upper Limit of Detection (ULOD)** | Caps the ULOQ; results cannot be quantified beyond it. |

Two further options control how analysts *enter* detection limits:

- **Display a Detection Limit selector**: adds a **DL** column with a small
  dropdown (`<` / `>`) next to the result field. Choosing `<` or `>` marks
  the result as a Lower/Upper Detection Limit; the default LLOD/ULOD value
  is filled into the result field.
- **Allow Manual Detection Limit input**: additionally lets the analyst
  replace the default limit with their own value. It is also possible to
  simply type `<5` or `>1000` directly into the result field — the operand
  is recognised and stripped, and the analysis is flagged as a detection
  limit result. If manual input is *not* allowed, the system keeps the
  operand but forces the default LLOD/ULOD value.

Detection-limit results are displayed and reported as `< value` /
`> value`, without uncertainty. Calculations of dependent analyses can
still access the flags and limit values of their dependencies.

## Decimal Mark, Precision and Scientific Notation

How numeric results are rounded and rendered is controlled at three levels:

**Per analysis service** (*Analysis* tab):

- **Precision as number of decimals** — fixed number of decimals for the
  result (unless *Calculate Precision from Uncertainties* is enabled, see
  above).
- **Exponential format precision** — the precision used when the value is
  converted to exponent (scientific) notation. Default 7.

**Global, for results entry and listings** (*Setup*, *Analyses* tab):

- **Default decimal mark** — dot (`.`) or comma (`,`) used when displaying
  results.
- **Default scientific notation format for results** — one of `aE+b`,
  `ax10^b`, `ax10^b` (with superscript exponent), `a·10^b` or `a·10^b`
  (with superscript exponent).
- **Exponential format threshold** — result values with at least this
  number of significant digits are displayed in scientific notation.

**Global, for reports** (*Setup*, *Results Reports* tab):

- **Default decimal mark** and **Default scientific notation format for
  reports** — the equivalents used when [publishing](ResultsPublication.md).
  Each client can override the report decimal mark with its own **Custom
  decimal mark** preference (see
  [Clients and Contacts](ClientsAndContacts.md)).

## Results Out of Specification

When an analysis has a result range (specification) assigned — from an
Analysis Specification, a dynamic specification or ranges set directly on
the sample — the range appears in the **Specification** column and the
result is evaluated against it as soon as it is captured:

- A range defines a *min* and *max*, and optionally *warn min* / *warn max*
  values that create **shoulder** zones adjacent to the valid range.
- A result outside min–max **and** outside the shoulders shows a red
  **exclamation icon** with the tooltip *"Result out of range"*.
- A result outside min–max but still within a shoulder zone shows a yellow
  **warning icon** with the tooltip *"Result in shoulder range"*.
- If the specification defines a **range comment**, an extra comment icon
  with that text appears next to the specification of out-of-range results.
- If the range applied to an individual analysis differs from the sample's
  specification, a warning icon *"Result range is different from
  Specification"* is displayed next to it.

These indicators follow the analysis through submission, verification and
into the listings, so reviewers can spot out-of-specification results at a
glance. The same icons are used for worksheet QC analyses, where the ranges
come from reference definitions and duplicate variations (see
[Quality Control](QualityControl.md)).

## Remarks on Analyses

- Enable the *Setup → Analyses* option **Add a remarks field to all
  analyses**. A free-text **Remarks** field is then displayed close to each
  analysis in results entry views (as an expandable row under the analysis).
- Remarks can be entered and edited by the same users who may edit the
  result, while the analysis is still in an editable state (before
  submission).
- Analysis remarks are kept with the analysis and can be included in results
  reports (see [Results Publication](ResultsPublication.md)).

Remarks on the *sample* level (the sample's own Remarks tab) are independent
of per-analysis remarks.

## Submitting Results

1. Enter and **Save** the results as described above.
2. Select the analyses (or the whole sample/worksheet) with the checkboxes.
3. Press **Submit**.

Each submitted analysis moves to **To be verified** and its result becomes
read-only. An analysis can only be submitted when:

- it has a **result** (or its calculation could compute one),
- all its **interim fields** have values (except those flagged *Allow
  empty*),
- an **attachment** is present, if the service requires one for
  verification,
- the sample is **received** (lab analyses) or **sampled** (field
  analyses),
- all analyses it **depends on** (via calculations) are submitted or
  submittable — dependencies are submitted automatically together with the
  dependent analysis.

By default any user with result-entry rights may submit any analysis. If
the *Setup* option **Allow submission of results for unassigned analyses**
is disabled, users can only submit analyses assigned to themselves (Lab
Managers are exempt).

When all analyses of a sample are submitted, the sample itself moves to *To
be verified*; the same roll-up applies to worksheets (see
[Sample Workflow](SampleWorkflow.md) and [Worksheets](Worksheets.md)).

## Verifying Results

Verification is the review step performed by a second pair of eyes —
typically a Lab Manager or a user with the *Verifier* role:

1. Open the sample or worksheet and switch to the **To be verified** filter.
2. Review results, uncertainties, out-of-range icons, attachments and
   remarks.
3. Select the analyses and press **Verify**.

Verified analyses move to the **Verified** state and can no longer be
changed — only [published](ResultsPublication.md), or invalidated at sample
level. When all analyses of a sample are verified, the sample is verified
automatically, provided the *Setup* option **Automatic verification of
samples** is enabled (default); otherwise a user with sufficient privileges
must verify the sample manually.

The status column displays informative icons during verification:

- *"Submitted and verified by the same user"* — warning shown when the
  submitter also verified the result.
- *"Cannot verify, submitted by current user"* — you submitted this result
  and self-verification is disabled.
- *"Can verify, but submitted by current user"* — self-verification is
  enabled, but flagged so you are aware.
- A **n/m badge** showing verifications done vs. required, when
  multi-verification applies (tooltip *"Multi-verification required"* with
  the number of pending verifications).

## Multi-Verification

A result can be required to pass **more than one** verification before it
is considered verified:

- *Setup → Analyses*: **Number of required verifications** (1–4, default 1)
  sets the system-wide default.
- Each analysis service can override it with its own **Number of required
  verifications** (options: *System default*, 1, 2, 3, 4).
- While verifications are pending, each **Verify** action is counted but the
  analysis stays in *To be verified*, with the progress badge updated
  (e.g. `1/3`). The final verification moves it to *Verified*.

Whether the *same user* may provide several of those verifications is
controlled by the *Setup* option **Multi Verification type**:

| Option | Behaviour |
|--------|-----------|
| **Allow same user to verify multiple times** | The same user may perform consecutive verifications (default). |
| **Allow same user to verify multiple times, but not consecutively** | The same user may verify again, but someone else must verify in between. |
| **Disable multi-verification for the same user** | Every verification must come from a different user. |

## Self-Verification

By default, the user who **submitted** a result is not allowed to verify
it, even if they hold verification rights:

- *Setup → Analyses*: **Allow self-verification of results** (default
  disabled) enables it system-wide.
- Each analysis service can override this with **Self-verification of
  results** (options: *System default*, *No*, *Yes*).

Self-verification only takes effect for users whose role already allows
verification (by default managers, lab managers and verifiers). Keep in
mind that permitting self-verification may conflict with accreditation
requirements — verified-by-submitter results are always flagged with a
warning icon.

## Retracting, Retesting and Rejecting Analyses

Three different actions are available for analyses in *To be verified*:

**Retract** — "this result is wrong, measure again":

1. Select the analyses and press **Retract**.
2. Each analysis moves to the **Retracted** state and is kept for
   traceability, but its result is disregarded and its attachments are
   excluded from reports.
3. A fresh copy of the analysis (a **retest**) is created automatically,
   without a result. If the original was assigned to a worksheet, the
   retest is placed on the same worksheet.
4. The sample rolls back to the *Received* state so the retest can be
   processed, and the retraction cascades to analyses that depend on the
   retracted one (and to its dependencies), so a whole calculation chain is
   remeasured consistently.

**Retest** — "this result is good, but measure again anyway":

- Available to Lab Managers/Managers. The current analysis is **verified**
  in the same action and a retest copy is created for re-measurement. Use
  this for confirmatory re-runs where the original result remains valid.

**Reject** — "discard this analysis entirely":

- The analysis moves to **Rejected**; no copy is created, its attachments
  are excluded from reports, and dependent analyses are rejected with it.
  Only Lab Managers/Managers can reject analyses by default. Rejecting a
  whole *sample* is a separate feature — see
  [Sample Rejection](SampleRejection.md).

Once an analysis is **verified**, it can no longer be retracted
individually — instead, the whole sample can be **invalidated**, which
creates a retest sample (see [Sample Workflow](SampleWorkflow.md)).

## Frequently Asked Questions

**Why can't I enter a result for an analysis?**
The sample has probably not been received yet (lab analyses become editable
only after reception, unless *Immediate results entry* is enabled), or the
analysis is already submitted, or you lack result-entry rights. Field
analyses only require the sample to be sampled.

**Why is the Submit button not doing anything for my analysis?**
Check that a result is present, all interim fields are filled (unless
flagged *Allow empty*), and a file is attached if the service requires an
attachment for verification. If *Allow submission of results for unassigned
analyses* is disabled, you can only submit analyses assigned to you.

**Why can't I verify a result I submitted?**
Self-verification is disabled (the default). Either another user with
verification rights must verify it, or self-verification must be enabled in
*Setup* or on the analysis service.

**The result field is read-only and shows a calculated value. How do I
change it?**
The analysis uses a calculation: edit the interim fields (and/or the
results of the analyses it depends on) and the result is recalculated
automatically.

**How do I report a result as "less than" a value?**
Enable *Display a Detection Limit selector* on the service and choose `<`
or `>` in the **DL** column — or simply type `<value` / `>value` into the
result field. Whether your own limit value is accepted depends on *Allow
Manual Detection Limit input*.

**Why is no uncertainty shown for my result?**
Either no uncertainty range matches the result, the range's uncertainty
value is 0, the result is a detection limit, or it falls outside the
quantifiable range (below LLOQ / above ULOQ).

**What is the difference between the red and the yellow icon next to a
result?**
The red exclamation icon means the result is out of the specification
range. The yellow warning icon means it is outside the valid range but
still within the shoulder (warning) zone defined by the specification.

**What is the difference between Retract and Retest?**
Both create a new copy of the analysis for re-measurement. *Retract* marks
the original result as withdrawn (not usable), while *Retest* verifies the
original result and keeps it valid.

**Do samples get verified automatically?**
Yes, when all their analyses are verified — provided *Automatic
verification of samples* is enabled in *Setup* (the default). Otherwise the
sample must be verified manually.

**Can I change a result after it was submitted?**
No. Retract the analysis (before verification) and enter the new result on
the automatically created retest, or invalidate the sample if it was
already verified or published.
