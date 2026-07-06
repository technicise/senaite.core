# Worksheets

Worksheets are the laboratory's bench sheets: they group analyses from
different samples into a single work unit, assigned to an analyst and,
optionally, to an instrument. Analyses are arranged in numbered slot
positions — mirroring, for example, an instrument tray — together with the
quality control analyses (blanks, controls and duplicates) that accompany a
run.

This guide explains the feature from an end-user perspective: how to create
worksheets manually or from Worksheet Templates, how to assign and unassign
analyses, how to add QC analyses, how worksheets move through the workflow,
and how to print and export them.

---

## Table of Contents

1. [What is a Worksheet?](#what-is-a-worksheet)
2. [Key Concepts](#key-concepts)
3. [Prerequisites and Permissions](#prerequisites-and-permissions)
4. [The Worksheets Listing](#the-worksheets-listing)
5. [Creating a Worksheet Manually](#creating-a-worksheet-manually)
6. [Creating a Worksheet from a Worksheet Template](#creating-a-worksheet-from-a-worksheet-template)
7. [Setting up Worksheet Templates](#setting-up-worksheet-templates)
8. [The Add Analyses Screen and its Filters](#the-add-analyses-screen-and-its-filters)
9. [Worksheet Layout and Positions](#worksheet-layout-and-positions)
10. [Adding QC Analyses (Blanks, Controls, Duplicates)](#adding-qc-analyses-blanks-controls-duplicates)
11. [Assigning Analyst and Instrument](#assigning-analyst-and-instrument)
12. [Unassigning Analyses](#unassigning-analyses)
13. [Worksheet States and Workflow](#worksheet-states-and-workflow)
14. [Removing a Worksheet](#removing-a-worksheet)
15. [Printing Worksheets](#printing-worksheets)
16. [Exporting to Instruments](#exporting-to-instruments)
17. [Setup Options affecting Worksheets](#setup-options-affecting-worksheets)
18. [Frequently Asked Questions](#frequently-asked-questions)

---

## What is a Worksheet?

While samples (see [Sample Registration](SampleRegistration.md)) group
analyses by *client request*, a worksheet groups analyses by *how the work
is done in the lab*: one analyst, one bench or instrument run, one sheet.

A worksheet:

- contains **routine analyses** pulled from received samples, plus optional
  **QC analyses** (blanks, controls and duplicates),
- arranges them in numbered **slot positions**, one slot per sample (or per
  QC sample/duplicate),
- is assigned to an **analyst**, and optionally to an **instrument** and a
  **method**,
- serves as the main **results entry** screen for bench work (see
  [Results Entry and Verification](ResultsEntryAndVerification.md)),
- follows its own workflow (*Open* → *To be verified* → *Verified*) that is
  kept in sync with the analyses it contains.

Analyses keep belonging to their samples — a worksheet only *references*
them — so results entered on a worksheet appear on the samples immediately.

## Key Concepts

| Term | Meaning |
|------|---------|
| **Worksheet** | A logical group of analyses across samples, assigned to an analyst. Identified by its own ID (default format `WS-001`, `WS-002`, …). |
| **Worksheet Template** | A reusable definition (under *Setup*) of a worksheet's size, slot layout, QC scheme and analysis services. |
| **Slot / Position** | A numbered row of the worksheet. All analyses of the same sample share one slot; each blank, control or duplicate group occupies its own slot. |
| **Routine analysis** | A regular analysis that belongs to a sample. |
| **Blank / Control** | QC analyses created from a **Reference Sample** (see [Quality Control](QualityControl.md)). A blank is a reference sample with expected results of zero; a control has certified expected values. |
| **Duplicate** | A QC copy of the routine analyses in a given slot, used to check repeatability. |
| **Assign / Unassign** | The transitions that place an analysis on a worksheet or remove it again. |

## Prerequisites and Permissions

- Only analyses of **received** samples can be placed on a worksheet, and
  only while they are still unassigned (an analysis can be on one worksheet
  at a time). See [Sample Workflow](SampleWorkflow.md).
- Creating worksheets requires the permission
  `senaite.core: Add Worksheet`; managing them (adding/removing analyses,
  applying templates) requires `senaite.core: Manage Worksheets`, and
  editing (results entry, analyst/instrument changes) requires
  `senaite.core: Edit Worksheet`.
- By default these permissions are granted to **Lab Managers** only,
  because the setup option *Restrict worksheet management to lab managers*
  is enabled out of the box. When that option is disabled, **Analysts** and
  **Lab Clerks** can create and manage worksheets too (see
  [Setup Options affecting Worksheets](#setup-options-affecting-worksheets)).
- Assigning and unassigning analyses is additionally protected by
  `senaite.core: Transition: Assign Analysis` and
  `senaite.core: Transition: Unassign Analysis`; removing empty worksheets
  by `senaite.core: Transition: Remove Worksheet`.

## The Worksheets Listing

The **Worksheets** view (available from the navigation) lists all
worksheets with the following columns: a **Progress** bar, the worksheet
ID, **Analyst**, **Template**, number of **Samples**, **QC Analyses** and
**Routine Analyses**, the creation date and the **State**.

Filter buttons at the top of the list:

| Filter | Shows |
|--------|-------|
| **Active** | Worksheets in *Open* or *To be verified* state (default). |
| **Open** | Only open worksheets. |
| **To be verified** | Worksheets whose results have all been submitted. |
| **Verified** | Fully verified worksheets. |
| **All** | Everything, including historical rejected worksheets. |
| **Mine** | Worksheets assigned to the current user. |

Notes:

- The progress bar reflects how far the analyses of the worksheet have
  advanced (submitted counts half, verified counts full).
- Clicking a worksheet takes you to its **Manage Results** view — or
  directly to the **Add Analyses** screen if it is still empty.
- On open worksheets, privileged users can change the **Analyst** directly
  in the listing: select one or more worksheets, pick a new analyst in the
  Analyst column and press the **Reassign** button.
- If the setup option *Restrict worksheet access to assigned analysts* is
  enabled, analysts only see their own worksheets here; the *Mine* filter
  is hidden (everything is "mine") and the Analyst column is hidden too.
  Lab Managers and Regulatory Inspectors always see all worksheets.

## Creating a Worksheet Manually

1. Go to the **Worksheets** listing and click the **Add** button.
2. In the creation bar, select:
   - **Analyst** — mandatory. The list offers users with the Analyst, Lab
     Manager or Manager role. If no analyst is selected you get the warning
     *"Analyst must be specified."*
   - **Template** — optional (see next section).
   - **Instrument** — optional. If a template with a preferred instrument
     is selected, the instrument is preselected accordingly.
3. Confirm. A new worksheet is created with an auto-generated ID and the
   default results layout from *Setup* (Classic or Transposed).
4. If **no template** was selected you land on the empty worksheet's
   **Add Analyses** screen, where you pick the analyses to work on (see
   [The Add Analyses Screen](#the-add-analyses-screen-and-its-filters)).

## Creating a Worksheet from a Worksheet Template

If you select a **Template** in the *Add* bar, the system applies the
template right away:

1. **Routine analyses**: the system searches for *unassigned* analyses of
   the services defined in the template, sorted by priority, and fills the
   template's routine slots with them — one slot per sample. If the
   template has a preferred **instrument** or a **method** restriction,
   only analyses compatible with that instrument/method are taken.
2. **Duplicates**: for each duplicate slot in the template layout, the
   routine analyses of the referenced source slot are duplicated into it.
3. **Blanks and controls**: for each blank/control slot, the system looks
   for an active, non-expired **Reference Sample** that matches the slot's
   Reference Definition and covers as many of the template's services as
   possible, and creates the reference analyses in the slot.
4. The template's preferred **instrument** and **method** are assigned to
   the worksheet and to all compatible analyses.

If at least one slot could be populated, you are taken directly to
**Manage Results**. If nothing could be added (e.g. no unassigned analyses
matched), the message *"No analyses were added"* appears and you land on
the *Add Analyses* screen instead.

Slots that the template defines but that could not be filled remain
visible in the worksheet as empty, reassignable slots, and are used
automatically when matching analyses or QC samples are added later.

A template can also be applied to an **existing** worksheet from the *Add
Analyses* screen (template selector at the top). Already occupied slots
are never overwritten.

## Setting up Worksheet Templates

Worksheet Templates are managed under *Setup → Worksheet Templates*. Each
template defines:

| Field | Purpose |
|-------|---------|
| **Name** / **Description** | Identify the template. |
| **Restrict to Method** | Only analyses (and instruments) supporting the selected method are used when the template is applied, and the *Add Analyses* screen gains a matching filter. |
| **Preferred instrument** | The instrument assigned to the worksheet and its compatible analyses when the template is applied. |
| **Number of Positions** (*Layout* tab) | The size of the worksheet, e.g. an instrument's tray size. Changing the number adds or removes layout rows. |
| **Worksheet Layout** (*Layout* tab) | One row per position. For each position select the **Analysis Type**: routine analysis, **Blank**, **Control** or **Duplicate**. For blanks/controls, also select the **Reference** (a Reference Definition, see [Quality Control](QualityControl.md)); for duplicates, select **Duplicate Of** — the position of the routine slot to duplicate. |
| **Analysis Services** (*Analyses* tab) | Which analyses should be included on the worksheet (see [Analyses Setup](AnalysesSetup.md)). |

## The Add Analyses Screen and its Filters

The **Add Analyses** screen of a worksheet lists all analyses in the
system that are ready to be assigned: analyses in **unassigned** state,
i.e. belonging to received samples and not yet placed on any worksheet.

Columns shown: a **priority** indicator, **Client**, client **Order**
number, **Request ID** (the sample), **Category**, **Analysis**, **Date
Received** and **Due Date** (late analyses are flagged with a "late"
icon). The list is sorted by priority by default and offers the usual
listing search box and column sorting.

Filter buttons:

- **All** — every unassigned analysis (default for worksheets without a
  template).
- **Filter by template services** — only analyses of the services defined
  in the worksheet's template. Shown (and preselected) when the worksheet
  has a template assigned.
- **Filter by template method** — only analyses whose service supports the
  template's method restriction. Shown when the template restricts to a
  method.

To assign analyses:

1. Tick the checkboxes of the analyses you want to work on. You can use
   the select-all checkbox in the header.
2. Press **Assign**.

The analyses are added to the worksheet, grouped by sample: samples are
placed in ascending sample-ID order and all analyses of one sample land in
the same slot. Each assigned analysis switches to the *assigned* state and
receives the worksheet's analyst (and instrument/method, if compatible).
When done, you are redirected to *Manage Results*.

The screen also contains a **Worksheet Template** selector: choosing a
template here applies it to the current worksheet (services, QC slots,
instrument), which is handy to add the QC scheme after picking analyses
manually.

The *Add Analyses* action is available while the worksheet is not yet
verified, and only to users who can manage worksheets.

## Worksheet Layout and Positions

Inside a worksheet, analyses are displayed grouped in numbered **slots**:

- The first cell of each slot shows the "parent" of its analyses: for
  routine slots the **sample** (with links to the sample and client, the
  sample type and sample point); for QC slots the **reference sample** and
  its supplier, or the duplicated sample. Icons distinguish routine
  samples, blanks, controls and duplicates. A remarks icon appears when
  the sample carries remarks.
- Analyses added for a sample that already has a slot always join that
  slot; new samples get the next free position.
- Slots defined by a template but still unoccupied appear as
  **Reassignable Slot** rows. They are consumed automatically when
  matching analyses, duplicates or reference analyses are added, and can
  be chosen explicitly as the **Position** when adding blanks or controls.

Two display layouts are available for the results table:

| Layout | Display |
|--------|---------|
| **Classic** | Samples in rows, analyses in columns (one row per analysis, grouped by slot). |
| **Transposed** | Samples in columns, analyses in rows — convenient for wide runs of the same analysis panel. |

Switch the layout with the **Layout** selector in the worksheet header and
press **Apply**; the choice is stored per worksheet. The default for new
worksheets comes from *Setup → Appearance → Default layout in worksheet
view*. The columns shown for each analysis (result, uncertainty, method,
instrument, etc.) follow the worksheet-view column configuration in
*Setup*.

## Adding QC Analyses (Blanks, Controls, Duplicates)

QC analyses can only be added while the worksheet is **open**. The three
actions are available as tabs/actions on the worksheet: **Add Blank
Reference**, **Add Control Reference** and **Add Duplicate**. For
background on reference definitions, reference samples and QC result
evaluation, see [Quality Control](QualityControl.md).

### Adding a blank or a control

1. Open the worksheet and choose **Add Blank Reference** (or **Add Control
   Reference**).
2. The screen lists the active, non-expired **Reference Samples** that
   support at least one of the services currently assigned to the
   worksheet — blanks on the blank screen, controls on the control screen.
3. For the reference sample you want, review the **Supported Services**
   selection: services already present on the worksheet come preselected;
   adjust as needed.
4. Choose the **Position**: `new` appends a new slot at the end of the
   worksheet, or pick one of the empty (reassignable) slot numbers.
   If the worksheet has a template, leaving the automatic choice will use
   the template's matching blank/control slot when one is still free.
5. Tick the reference sample's checkbox and press **Add**.

One reference analysis is created per selected service, all sharing a
group ID derived from the reference sample. Services whose calculation
depends on other services cannot be used for reference analyses.

### Adding duplicates

1. Open the worksheet and choose **Add Duplicate**.
2. The screen lists the routine slots of the worksheet (position, sample
   ID, client, date requested).
3. Select the slot(s) to duplicate and press **Add**.

All analyses of the source slot are copied as duplicate analyses into a
new slot (or into the template's duplicate slot for that source position,
if free). Retracted analyses and analyses with dependent calculations are
skipped. Duplicates only work for slots that contain routine sample
analyses — you cannot duplicate a blank or control slot.

## Assigning Analyst and Instrument

The header of the *Manage Results* view shows the current **Analyst** and
**Instrument** of the worksheet:

- **Analyst** — pick a user from the dropdown (users with the Analyst,
  Lab Manager or Manager role). The change is saved immediately and is
  propagated to **all analyses** of the worksheet. Every analysis added
  later also inherits the worksheet's analyst.
- **Instrument** — pick an instrument from the dropdown. The instrument
  is assigned to every analysis that supports it (incompatible analyses
  are left unchanged), and a supported method is selected for each of
  those analyses when needed. See [Instruments](Instruments.md).

Both dropdowns are only editable while the worksheet is *Open* or
*To be verified* and the user has management privileges; otherwise the
current values are displayed read-only.

If any analysis uses an instrument that is out of calibration or has
failing calibration tests, a warning is displayed — *"Some analyses use
out-of-date or uncalibrated instruments. Results edition not allowed"* —
and results cannot be entered for them (see
[Instruments](Instruments.md)).

## Unassigning Analyses

To remove analyses from a worksheet without losing them:

1. Open the worksheet's *Manage Results* view.
2. Select the analyses to remove using their checkboxes.
3. Press **Unassign**.

The analyses return to the *unassigned* state, leave the worksheet layout
and become available again on any worksheet's *Add Analyses* screen. Their
results, if any had been captured but not submitted, remain on the
analysis. Slots that become empty are purged from the layout.

Rejecting an analysis (see [Sample Rejection](SampleRejection.md) for the
sample-level counterpart) also removes it from its worksheet
automatically, as does cancelling the sample it belongs to.

## Worksheet States and Workflow

A worksheet's state is driven by the analyses it contains — you normally
never transition a worksheet by hand:

| State | Meaning |
|-------|---------|
| **Open** | The worksheet is being worked on: analyses can be added, removed and results captured. Initial state. |
| **To be verified** | Every active analysis on the worksheet has been submitted. Awaiting verification. |
| **Verified** | Every active analysis has been verified. The worksheet is read-only. |
| **Rejected** | Legacy state kept for old installations where whole worksheets could be rejected and replaced by a copy; such worksheets show a banner linking to their replacement. |

How the transitions happen:

- **Submit** — when the results of the *last* pending analysis are
  submitted, the worksheet automatically moves to *To be verified*.
  Analyses that are cancelled, rejected or retracted are ignored in this
  evaluation, but at least one active, submitted analysis is required.
  See [Results Entry and Verification](ResultsEntryAndVerification.md).
- **Verify** — likewise, the worksheet becomes *Verified* automatically
  when its last active analysis is verified. Verified results then follow
  the publication flow on their samples (see
  [Results Publication](ResultsPublication.md)).
- **Retract** — available on worksheets in *To be verified* or *Verified*
  state (permission `senaite.core: Transition: Retract`). It retracts
  **all** analyses of the worksheet at once; a retest copy of each is
  created and assigned to the same worksheet, and the worksheet returns to
  *Open*.
- **Rollback** — if a single analysis of a *To be verified* worksheet is
  retracted, the worksheet automatically rolls back to *Open* so the
  retest can be processed; the retest is placed on the same worksheet.

While the worksheet is *Open*, Analysts, Lab Clerks and Lab Managers can
edit it. In *To be verified*, editing is limited to Lab Managers (e.g. for
verification); attachments can no longer be added. In *Verified*, the
worksheet is read-only for everyone.

## Removing a Worksheet

Worksheets that turn out to be unnecessary can be removed — but only while
they are **empty**:

1. Unassign all analyses from the worksheet (QC analyses are deleted when
   removed, routine analyses return to the unassigned pool).
2. Use the **Remove** action of the worksheet.

The action is protected by `senaite.core: Transition: Remove Worksheet`
and is refused while any analysis is still assigned. Removal deletes the
worksheet permanently — it does not appear in any listing afterwards.

## Printing Worksheets

Use the **Print** button in the worksheet header to open the print
preview:

- **Available templates** — choose the print layout. Two templates are
  bundled: *Sample By Row* (each sample on one row, analyses as columns)
  and *Sample By Column* (samples as columns). Add-ons can register
  additional templates, and the template order can be configured via the
  registry record `worksheet_print_templates_order`.
- **Num columns** — for the by-column template, how many samples are
  printed side by side per table (default 3).
- Press **Print** to send the rendered sheet to the printer, or **Cancel**
  to go back. A PDF version can also be generated from the same view.

The printout includes the worksheet metadata (ID, analyst, template,
instrument, print date), one section per slot with sample/QC information,
the analyses with result fields, and is typically used as a bench sheet
for manual result capture.

Worksheet **stickers** (barcode labels) are also available from the
document actions (sticker icon) in the worksheet view.

## Exporting to Instruments

If the worksheet has an **instrument** assigned and that instrument has a
data **import/export interface** configured, the **Export** action
generates an instrument input file (e.g. a sample list for the
autosampler) from the worksheet's analyses. See
[Instruments](Instruments.md) for configuring instrument interfaces and
for importing the results back.

## Setup Options affecting Worksheets

All options live in *Setup*, most of them under the *Security* and
*Appearance* fieldsets:

| Setting | Default | Effect |
|---------|---------|--------|
| **Restrict worksheet access to assigned analysts** (`restrict_worksheet_users_access`) | on | Analysts only see and access worksheets to which they are assigned. Lab Managers, Managers and Regulatory Inspectors always see all worksheets. |
| **Allow submission of results for unassigned analyses** (`allow_to_submit_not_assigned`) | on | When disabled, users can only submit results for analyses assigned to themselves (via a worksheet). Does not apply to Lab Managers. See [Results Entry and Verification](ResultsEntryAndVerification.md). |
| **Restrict worksheet management to lab managers** (`restrict_worksheet_management`) | on | Only Lab Managers can create and manage worksheets. When disabled, Analysts and Lab Clerks can too. This setting is automatically enabled and locked while worksheet access is restricted to assigned analysts. |
| **Default layout in worksheet view** (`worksheet_layout`) | Classic | Whether new worksheets use the Classic or Transposed results table. |
| **Worksheet view columns order** | all | Which analysis columns are shown in worksheet views and in which order. |
| **ID Server format** for type *Worksheet* | `WS-{seq:03d}` | The format of worksheet IDs (`WS-001`, `WS-002`, …). |

## Frequently Asked Questions

**Why can't I see the Add button in the Worksheets listing?**
You lack worksheet management privileges. By default only Lab Managers can
create worksheets; disable *Restrict worksheet management to lab managers*
in *Setup* to allow Analysts and Lab Clerks as well.

**Why does an analysis not appear on the Add Analyses screen?**
Only analyses in *unassigned* state are listed: the sample must have been
received, and the analysis must not sit on another worksheet already. If
the worksheet has a template, also check the *Filter by template services*
/ *All* filter buttons — the template filter is preselected.

**Can an analysis be on two worksheets at the same time?**
No. An analysis can only be assigned to one worksheet. Unassign it first
if it must move to a different worksheet.

**Why is the worksheet still Open although I submitted results?**
The worksheet only moves to *To be verified* when **all** of its active
analyses (including blanks, controls and duplicates) have been submitted.
Cancelled, rejected and retracted analyses are ignored.

**Why can't I add a blank/control?**
Blanks and controls can only be added while the worksheet is *Open*, and
only reference samples that are active, not expired and that support at
least one service of the worksheet are offered. Check your reference
sample definitions under [Quality Control](QualityControl.md). Also note
that services with dependent calculations cannot be used for reference
analyses or duplicates.

**How do I delete a worksheet?**
Unassign all its analyses, then use the **Remove** action. Non-empty
worksheets cannot be removed.

**What happens to the worksheet when a result is retracted?**
The retracted analysis stays on the worksheet (marked retracted), a retest
copy is created in the same slot, and the worksheet rolls back to *Open*
if it had already reached *To be verified*. The worksheet-level *Retract*
action does the same for all analyses at once.

**Does changing the worksheet's instrument overwrite the instrument of my
analyses?**
Yes — selecting an instrument in the worksheet header assigns it to every
analysis that supports it, along with a compatible method. Analyses that
do not support the instrument are left untouched. Individual analyses can
still be changed afterwards in their Method/Instrument columns.

**Who may verify a worksheet?**
Verification follows the same rules as for analyses and samples —
including the self-verification restrictions — described in
[Results Entry and Verification](ResultsEntryAndVerification.md). Once the
last analysis is verified, the worksheet becomes *Verified* automatically.
