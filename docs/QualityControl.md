# Quality Control

Quality Control (QC) in SENAITE lets the laboratory verify that its
analytical results can be trusted. Blanks and controls made from certified
reference material, together with duplicates of routine analyses, are placed
on worksheets next to the real samples. Their results are automatically
checked against expected values, and deviations are flagged immediately.

This guide explains the feature from an end-user perspective: how to set up
suppliers, reference definitions and reference samples, how to add blank,
control and duplicate analyses to worksheets, how QC results are evaluated,
and where to find QC trends and charts.

---

## Table of Contents

1. [What is Quality Control in SENAITE?](#what-is-quality-control-in-senaite)
2. [Key Concepts](#key-concepts)
3. [Prerequisites and Permissions](#prerequisites-and-permissions)
4. [Suppliers](#suppliers)
5. [Reference Definitions](#reference-definitions)
6. [Reference Samples](#reference-samples)
7. [Reference Sample Lifecycle: Expiry and Disposal](#reference-sample-lifecycle-expiry-and-disposal)
8. [Adding Blanks and Controls to a Worksheet](#adding-blanks-and-controls-to-a-worksheet)
9. [Adding Duplicates to a Worksheet](#adding-duplicates-to-a-worksheet)
10. [Predefining QC in Worksheet Templates](#predefining-qc-in-worksheet-templates)
11. [How QC Results are Evaluated](#how-qc-results-are-evaluated)
12. [QC Analysis Workflow and IDs](#qc-analysis-workflow-and-ids)
13. [QC Charts and Trends](#qc-charts-and-trends)
14. [QC Analyses on the Sample View](#qc-analyses-on-the-sample-view)
15. [Frequently Asked Questions](#frequently-asked-questions)

---

## What is Quality Control in SENAITE?

SENAITE supports three types of QC analyses, all of them performed on
[worksheets](Worksheets.md):

- **Blanks** — analyses of reference material whose expected values are
  zero (or "blank"). They detect contamination and background signal.
- **Controls** — analyses of reference material with known, certified
  values. They verify accuracy against the expected result.
- **Duplicates** — repeat analyses of a routine sample already present on
  the worksheet. They verify precision (repeatability).

Blanks and controls are created from **reference samples**: physical
standards purchased from **suppliers** and registered in the system with
their expected values and expiry date. Duplicates need no reference
material — they are copies of routine analyses.

When results are entered, each QC analysis is compared against its valid
range and an alert icon is shown next to any result that falls outside it —
the same mechanism used for routine results (see
[Results Entry and Verification](ResultsEntryAndVerification.md)).

## Key Concepts

| Term | Meaning |
|------|---------|
| **Supplier** | An external organisation the laboratory purchases reference material (and instruments) from. Reference samples are always registered *inside* a supplier. |
| **Reference Definition** | A reusable "recipe" of expected values per analysis service, configured once in Setup. Marking it as *Blank* means the expected values are zero. |
| **Reference Sample** | A physical batch/lot of reference material from a supplier, with lot number, expiry date and its own expected values (usually inherited from a reference definition). |
| **Blank** | A reference sample (or QC analysis made from it) whose values are zero or "blank". |
| **Control** | A reference sample (or QC analysis made from it) with non-zero certified values. |
| **Reference Analysis** | A blank or control analysis placed on a worksheet, created from a reference sample. |
| **Duplicate Analysis** | A copy of a routine analysis on the same worksheet, used to check repeatability. |
| **Duplicate Variation %** | Per analysis service, the maximum percentage a duplicate result may differ from the original result before an alert is raised. |
| **QC Analysis ID** | Identifier grouping the QC analyses added together in one slot, e.g. `QC-BLANK-001-002` for references or `WATER-0001-D` for duplicates. |

## Prerequisites and Permissions

- Reference definitions are managed in *Setup → Reference Definitions*.
  Adding them is protected by the permission
  `senaite.core: Add ReferenceDefinition`.
- Suppliers are managed in *Setup → Suppliers*. Adding them is protected by
  `senaite.core: Add Supplier`.
- Managing reference samples (including the *Expire* and *Dispose*
  actions) is protected by `senaite.core: Manage Reference`. By default
  Lab Managers, Lab Clerks and Analysts can create and edit reference
  samples.
- Adding blanks, controls and duplicates to a worksheet requires the
  permission `senaite.core: Manage Worksheets` and is only possible while
  the worksheet is **open** (not yet verified or published).
- QC analyses cannot be created for analyses whose calculation depends on
  other analyses (dependent services).

## Suppliers

Suppliers represent the vendors of your reference material and instruments.

1. Go to *Setup → Suppliers* and press **Add**.
2. Fill in the organisation details: name, tax number, phone, fax, email,
   physical/postal/billing addresses, **Lab Account Number**, **Website**,
   bank details (**NIB**, **IBN**, **SWIFT code**) and **Remarks**.
3. Save. The supplier appears in the *Active* listing.

Each supplier has three tabs:

- **Reference Samples** — the reference samples purchased from this
  supplier (see [Reference Samples](#reference-samples)).
- **Contacts** — contact persons at the supplier, with their own address
  and contact details. These are separate from client contacts (see
  [Clients and Contacts](ClientsAndContacts.md)).
- **Instruments** — instruments purchased from this supplier (see
  [Instruments](Instruments.md); an instrument's *Procurement* tab lets
  you select the supplier).

Suppliers can be deactivated when no longer used; inactive suppliers stay
in the system for traceability.

## Reference Definitions

A reference definition describes *what* a certain kind of reference
material contains, independently of any particular lot or batch. You define
it once and reuse it for every reference sample of that kind, and in
[worksheet templates](#predefining-qc-in-worksheet-templates).

1. Go to *Setup → Reference Definitions* and press **Add**.
2. On the **Description** tab, enter a title and description and set the
   flags:
   - **Blank** — tick if the "Reference sample values are zero or 'blank'".
     This is what makes the definition (and the samples created from it)
     behave as a blank instead of a control.
   - **Hazardous** — tick if "Samples of this type should be treated as
     hazardous".
3. On the **Reference Values** tab, select the analysis services this
   material certifies and enter, per service:

   | Field | Meaning |
   |-------|---------|
   | **Expected Result** | The certified value (required). |
   | **Permitted Error %** | Optional percentage of tolerance around the expected result. When set, **Min** and **Max** are recalculated as *expected ± error %*. |
   | **Min** / **Max** | The valid results range. If you leave both empty (and no error %), only the exact expected result is accepted. |

4. Save.

Reference definitions can be deactivated when obsolete. Only active
definitions can be selected on reference samples and worksheet templates.

## Reference Samples

A reference sample is a concrete lot of reference material sitting on a
shelf in your lab. Reference samples are created **inside a supplier**:

1. Go to *Setup → Suppliers*, open the supplier, and open its
   **Reference Samples** tab.
2. Press **Add**.
3. On the **Description** tab, fill in:
   - **Title** and description.
   - **Reference ID** — an optional manual identifier. If given, it is
     used as the sample's ID instead of the automatically generated one
     (ID formats are configured in *Setup → ID Server*). It can no longer
     be changed once QC analyses have been performed on the sample.
   - **Reference Definition** — selecting one automatically copies its
     **Blank** and **Hazardous** flags and pre-fills the expected values.
   - **Blank** / **Hazardous** — as on the definition; you can override
     them here.
   - **Manufacturer** — the manufacturer of the material (managed in
     *Setup → Manufacturers*).
   - **Catalogue Number**, **Lot Number** and **Remarks**.
4. On the **Dates** tab, fill in:
   - **Date Sampled**, **Date Received** (defaults to today) and
     **Date Opened**.
   - **Expiry Date** — required; the sample cannot be used after this
     date (see next section).
5. On the **Reference Values** tab, review the **Expected Values** table
   (Analysis Service, Expected Result, Permitted Error %, Min, Max). The
   values inherited from the reference definition can be adjusted for this
   specific lot.
6. Save.

Each reference sample offers:

- an **Analyses** tab listing every QC analysis ever performed on it,
  together with a trend chart (see [QC Charts and Trends](#qc-charts-and-trends));
- a **Reference Values** tab showing the expected values;
- **Sticker** actions to print barcode labels for the physical container.

Besides the per-supplier listing, all reference samples of the laboratory
are listed together under **Reference Samples** (with *Current*, *Expired*,
*Disposed* and *All* filters), including columns for Supplier,
Manufacturer, Reference Definition, dates and Expiry Date. Blank and
hazardous samples are marked with icons.

## Reference Sample Lifecycle: Expiry and Disposal

Reference samples follow a simple workflow:

| State | Meaning |
|-------|---------|
| **Current** | The sample is valid and can be used on worksheets. |
| **Expired** | The expiry date has passed (or the sample was expired manually). It can no longer be added to worksheets. |
| **Disposed** | The physical material has been discarded. Final state. |

- When the **Expiry Date** passes, the system automatically transitions
  the sample to *Expired* (the transition is triggered when the reference
  samples listing is rendered) and records the expiration date.
- An expired sample can be **Disposed** with the *Dispose* action, which
  records the disposal date.
- Only **current**, active reference samples are offered when adding
  blanks or controls to a worksheet.

## Adding Blanks and Controls to a Worksheet

Blanks and controls are added from an open [worksheet](Worksheets.md) that
already contains routine analyses:

1. Open the worksheet and choose **Add Blank Reference** or
   **Add Control Reference** from the worksheet's actions.
2. The listing shows the reference samples that:
   - are *Current* (not expired or disposed) and active,
   - are blanks (for *Add Blank Reference*) or controls (for *Add Control
     Reference*),
   - support at least one analysis service present on the worksheet
     (i.e. have an expected value defined for it).
3. For each reference sample row you can set:
   - **Supported Services** — which of the sample's certified services to
     create QC analyses for. Services already on the worksheet come
     pre-selected.
   - **Position** — an empty slot number, or *new* to append a new slot at
     the end of the worksheet.
4. Select the reference sample(s) with the checkbox and press **Add**.

One reference analysis per selected service is created in the chosen slot
and you are returned to the *Manage Results* screen. All the reference
analyses added together in a slot share the same **QC Analysis ID**
(reference sample ID plus a sequential suffix, e.g. `QC-BLANK-001-002`).

## Adding Duplicates to a Worksheet

1. Open the worksheet and choose **Add Duplicate** from its actions.
2. The listing shows the samples currently on the worksheet, with their
   **Position**, **Request ID**, **Client** and **Date Requested**.
3. Select the sample(s) whose analyses you want to duplicate and press
   **Add**.

The system copies all the analyses of the selected slot into a new slot
(retracted analyses and analyses with dependent calculations are skipped).
The duplicates are identified by the source sample ID with a `-D` suffix
(e.g. `WATER-0001-D`) and results are entered on them like on any other
analysis.

## Predefining QC in Worksheet Templates

Instead of adding QC manually every time, [worksheet
templates](Worksheets.md#setting-up-worksheet-templates) can reserve fixed
positions for QC:

- Each position in the template layout has an **Analysis Type**:
  *Analysis* (routine), *Blank*, *Control* or *Duplicate*. The *Blank* and
  *Control* options are only offered once at least one blank/control
  reference definition exists.
- For a *Blank* or *Control* position, select the **Blank Reference** /
  **Control Reference** — a reference definition.
- For a *Duplicate* position, select **Duplicate Of** — the position of
  the routine sample to duplicate.

When the template is applied to a worksheet, the system fills the QC
positions automatically: for each blank/control position it searches the
current, active reference samples created from the selected reference
definition and picks the one that supports the most services of the
template; duplicates are created from the sample at the indicated position.
Slots already in use are never overwritten.

## How QC Results are Evaluated

Results for QC analyses are captured on the worksheet's *Manage Results*
screen like any other result (see
[Results Entry and Verification](ResultsEntryAndVerification.md)). The
valid range, however, is determined differently per QC type:

| QC type | Valid range |
|---------|-------------|
| **Blank** | The **Min**–**Max** range defined in the reference sample's expected values (typically around zero). |
| **Control** | The **Min**–**Max** range defined in the reference sample's expected values (expected result ± permitted error %). |
| **Duplicate** | The result of the original analysis ± the **Duplicate Variation %** configured on the analysis service. If the variation is 0 (or the result is non-numeric or uses result options), the duplicate must match the original result exactly. |

- A result outside its valid range is flagged with a red alert icon
  ("Result out of range") next to the result field, both on the worksheet
  and in every listing where the analysis appears.
- If the original analysis of a duplicate has no result yet, the duplicate
  cannot be evaluated and no alert is shown.
- The **Duplicate Variation %** is set per analysis service on its
  *Analysis* tab (see [Analyses Setup](AnalysesSetup.md)): "When the
  results of duplicate analyses on worksheets, carried out on the same
  sample, differ with more than this percentage, an alert is raised".

An out-of-range QC result does **not** block submission or verification by
itself — it is up to the lab to decide whether to retract and repeat the
affected analyses. QC analyses do, however, count towards the worksheet's
progress: they must be submitted (and verified) like the routine analyses
for the worksheet to advance (see
[Sample Workflow](SampleWorkflow.md) for the overall lifecycle).

## QC Analysis Workflow and IDs

QC analyses follow the same basic workflow as routine analyses:

- **Assigned** — the QC analysis sits on the worksheet awaiting a result.
- **Submit** — once a result is entered, it moves to *To be verified*.
- **Verify** — an authorised user verifies the result (with multi-level
  verification if enabled), moving it to *Verified*.
- **Retract** — a submitted QC result can be retracted. The analysis moves
  to *Retracted* and the system automatically creates a retest copy in the
  same slot with the same QC Analysis ID, rolling the worksheet back to
  *open* so the test can be repeated.
- **Remove** — an unneeded QC analysis can be removed from the worksheet
  while it has not been submitted.

Identifiers:

- **Blanks/controls** are grouped under a QC Analysis ID formed by the
  reference sample ID plus a 3-digit sequence (e.g. `QC-BLANK-001-002` for
  the second blank round created from `QC-BLANK-001`).
- **Duplicates** are grouped under the source sample ID plus a `-D` suffix.

These IDs appear in the **QC Analysis ID** / **QC Sample ID** columns of
the QC listings.

## QC Charts and Trends

SENAITE core includes simple control charts based on the QC history:

- **Reference sample trend chart** — open a reference sample and go to its
  **Analyses** tab. Besides the table of all reference analyses performed
  on the sample (with links to their worksheets), a chart plots each
  analysis service's results over time against the expected value and the
  upper/lower limits, so drift and outliers are easy to spot. Only numeric
  results with a defined expected value and range are plotted.
- **Instrument QC** — instruments have an **Internal Calibration Tests**
  tab with the same kind of table and chart for the reference analyses
  measured on that instrument, useful to monitor instrument performance
  between calibrations (see [Instruments](Instruments.md)).

More elaborate QC reporting is provided by add-ons; the charts above are
what is available in core.

## QC Analyses on the Sample View

Every sample shows a collapsible **QC Analyses** section (below its
regular analyses) listing the QC analyses that back its results: the
blanks, controls and duplicates performed on worksheets that contain at
least one of the sample's analyses, restricted to matching services. The
listing shows the QC type (blank/control/duplicate icon), the
**Worksheet**, the **QC Sample ID** and the **Source** (the reference
sample or the duplicated sample).

This gives reviewers direct evidence of the QC context when verifying a
sample. QC analyses are internal to the laboratory: they are not part of
the sample's own analyses and are not included in the results reports sent
to clients (see [Results Publication](ResultsPublication.md)). If you need
a client-invisible aliquot of a real sample for internal QC purposes, use
an internal-use [partition](SamplePartitions.md) instead.

## Frequently Asked Questions

**What is the difference between a blank and a control?**
Both are created from reference samples; the only difference is the
**Blank** flag. A blank's expected values are zero ("Reference sample
values are zero or 'blank'"), while a control has non-zero certified
values. The flag also determines whether the sample is offered under *Add
Blank Reference* or *Add Control Reference* on worksheets.

**Why doesn't my reference sample show up when adding a blank/control?**
Check that: it is in the *Current* state (not expired or disposed) and
active; its *Blank* flag matches the action you chose; and it has expected
values defined for at least one analysis service that is present on the
worksheet.

**Can I add a blank or control for an analysis with a calculation?**
Only if the calculation does not depend on other analyses. QC analyses
cannot be created for services with dependent calculations; the same
applies to duplicates.

**What happens when a reference sample expires?**
The system flips it to *Expired* automatically once the expiry date has
passed, and it can no longer be added to worksheets. QC analyses already
on worksheets are not affected. Once discarded physically, use *Dispose*
to record the disposal date.

**A QC result is out of range — is anything blocked?**
No. The alert icon warns the analyst and verifiers, but SENAITE does not
block submission or verification. Retract the affected analyses if the run
must be repeated; retracting a QC analysis automatically creates a retest
in the same slot and reopens the worksheet.

**How strict is the duplicate check?**
The duplicate must fall within the original result ± the service's
**Duplicate Variation %**. With a variation of 0, or when the analysis
uses result options or text results, the duplicate must match the original
exactly.

**Can I duplicate a duplicate, or duplicate a blank?**
No. Duplicates can only be created from routine analyses of samples placed
on the worksheet.

**Do clients see QC analyses?**
No. QC analyses live on worksheets and reference samples, which are
laboratory-internal. Client contacts see neither the QC listings nor QC
results in published reports.

**Where do I get a control chart?**
Open the reference sample and go to its **Analyses** tab, or open an
instrument and go to **Internal Calibration Tests**. Both show the QC
results plotted over time against the expected value and limits.
