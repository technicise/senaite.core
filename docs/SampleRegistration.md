# Registering Samples

The **Add Samples** form is where new samples enter SENAITE. In a single
screen you describe who the sample belongs to, when and how it was taken,
what kind of sample it is, and which analyses the laboratory should perform
on it — for one sample or for many samples at once.

This guide explains the feature from an end-user perspective: how to open
and fill in the form, what each field means, how to register many samples
efficiently (columns, copy, paste, templates, profiles), what happens when
you press *Save*, and how to configure the system around it.

---

## Table of Contents

1. [The Add Samples Form at a Glance](#the-add-samples-form-at-a-glance)
2. [Key Concepts](#key-concepts)
3. [Prerequisites and Permissions](#prerequisites-and-permissions)
4. [Opening the Form](#opening-the-form)
5. [Registering a Single Sample](#registering-a-single-sample)
6. [Field Reference](#field-reference)
7. [Selecting Analyses](#selecting-analyses)
8. [Working with Sample Templates](#working-with-sample-templates)
9. [Working with Analysis Profiles](#working-with-analysis-profiles)
10. [Registering Multiple Samples at Once](#registering-multiple-samples-at-once)
11. [Copying Columns from Existing Samples](#copying-columns-from-existing-samples)
12. [Saving the Form](#saving-the-form)
13. [After Registration](#after-registration)
14. [Customising the Form](#customising-the-form)
15. [Configuration Reference](#configuration-reference)
16. [Frequently Asked Questions](#frequently-asked-questions)

---

## The Add Samples Form at a Glance

The form (titled *Request new analyses*) is laid out as a table:

- **Rows** are the sample fields (Client, Contact, Date Sampled, Sample
  Type, …) followed by the available analyses, grouped into *Field
  Analyses* and *Lab Analyses* and their categories.
- **Columns** are the samples being registered. Each column is one sample
  record; tabs at the top (*Sample 1*, *Sample 2*, …, *Show all*) let you
  switch between columns or view them side by side.
- Between the labels and the columns sits a small **copy column** with two
  helper buttons per field: one to copy the value of the first sample to
  all other columns, and one to paste a list of individual values, one per
  column (see [Registering Multiple Samples at Once](#registering-multiple-samples-at-once)).
- At the bottom you find the **Save**, **Save and Copy** and **Cancel**
  buttons, and — if pricing is enabled in setup — a running *Discount /
  Subtotal / VAT / Total* price calculation per column.

## Key Concepts

| Term | Meaning |
|------|---------|
| **Sample** | The central record of the laboratory: who it belongs to, what was sampled, and the analyses requested on it. Historically also called *Analysis Request (AR)*. |
| **Column** | One sample in the Add Samples form. A form can hold many columns, each producing (at least) one sample on save. |
| **Sample Template** | A predefined recipe (sample type, sample point, analyses, partition scheme, composite flag) that fills in a whole column at once. |
| **Analysis Profile** | A named group of analysis services that is selected as one unit. Several profiles can be combined on one sample. |
| **Analysis Specification** | A set of result ranges (min/max/warnings) applied to the sample's analyses, used for out-of-range highlighting. |
| **Secondary Sample** | A new sample created against an existing (primary) sample by filling in the *Primary Sample* field. See [Secondary Samples](SecondarySamples.md). |

## Prerequisites and Permissions

- Registering samples requires the permission
  `senaite.core: Add AnalysisRequest`. By default laboratory roles (Lab
  Manager, Lab Clerk, Sampler) and **client contacts** hold it, so clients
  can register their own samples from within their client area — see
  [Clients and Contacts](ClientsAndContacts.md).
- The catalogue of selectable **analysis services, profiles, templates and
  specifications** must be set up beforehand — see
  [Analyses Setup](AnalysesSetup.md).
- Some fields only appear when the related functionality is enabled in
  *Setup* (e.g. the sampling workflow fields, preservation fields or the
  rejection field — see [Configuration Reference](#configuration-reference)).

## Opening the Form

The Add Samples form can be reached from several places, and the starting
point pre-fills the form:

1. **Samples listing** — press the **Add** button. A number selector next
   to it lets you choose how many columns (1–99) the form should open with.
   The proposed count comes from the setup option *Default count of Sample
   to add*.
2. **Inside a Client** — same button; the **Client** field is filled in
   automatically and hidden, and client-specific contacts, sample points,
   profiles, templates and specifications are offered first.
3. **Inside a Batch** — the **Batch** field is pre-filled and hidden, and
   the batch's client (if any) is applied. See [Batches](Batches.md).
4. **From existing samples** — select samples in a listing and use **Copy
   to new** (see [Copying Columns from Existing Samples](#copying-columns-from-existing-samples)).

## Registering a Single Sample

1. Open the Add Samples form with one column.
2. Select the **Client**. The **Contact** is auto-completed when the client
   has a primary contact or only one contact; CC contacts and CC e-mail
   addresses stored on the client are proposed automatically.
3. Enter the **Date Sampled** — the date and time the sample was taken. By
   default this field is required (setup option *Date sampled required*)
   unless the sampling workflow is active, in which case you enter an
   *Expected Sampling Date* instead and the lab collects the sample later.
4. Select the **Sample Type** (required). The choice narrows down the
   sample points, profiles, templates and specifications offered in the
   other fields to those compatible with the selected type.
5. Optionally apply a **Sample Template** and/or one or more **Analysis
   Profiles**, or tick the required analyses by hand in the analyses
   section below the fields.
6. Fill in any other fields you need — batch, priority, client reference,
   environmental conditions, remarks, attachments, etc. (see
   [Field Reference](#field-reference)).
7. Press **Save**. The sample is created, gets its ID, and you are
   returned to the listing with a confirmation message.

## Field Reference

Which fields are visible, and in which order, is configurable (see
[Customising the Form](#customising-the-form)); add-ons may contribute
extra fields. The standard fields are:

| Field | Meaning |
|-------|---------|
| **Client** | The client the sample belongs to. Required; hidden and pre-filled when registering from within a client. |
| **Contact** | The client contact who requests the analyses and receives the reports. Must belong to the selected client (or be a lab-wide contact). |
| **CC Contact** / **CC Emails** | Additional contacts / e-mail addresses to be notified. Pre-filled from the client's and the contact's CC settings. |
| **Client Order Number** / **Client Reference** / **Client Sample ID** | The client's own order number, reference and sample identifier, printed on reports for traceability. |
| **Primary Sample** | Select an existing, received sample here to register a **secondary sample** of it. Dates are inherited from the primary and the secondary is received automatically. See [Secondary Samples](SecondarySamples.md). |
| **Batch** / **Batch Sub-group** | Assign the sample to a [Batch](Batches.md) (and optionally one of its sub-groups). Hidden and pre-filled when registering from within a batch. |
| **Sample Template** | Applies a whole registration recipe at once — see [Working with Sample Templates](#working-with-sample-templates). |
| **Analysis Profiles** | Adds predefined groups of analyses — see [Working with Analysis Profiles](#working-with-analysis-profiles). |
| **Date Sampled** | The date (and time) the sample was taken. Also the anchor for analysis holding-time checks. |
| **Expected Sampling Date** / **Sampler** / **Sampler for scheduled sampling** | Sampling workflow fields: when the sample will be taken and by whom. Only relevant when the sampling workflow (and sampling scheduling) is enabled in setup. |
| **Sample Type** | The kind of sample (water, soil, blood, …). Required. Filters the sample points, profiles, templates and specifications offered. |
| **Sample Point** | Where the sample was taken. Filtered by client and sample type. |
| **Container** / **Preservation** / **Date Preserved** / **Preserver** | The container the sample arrived in and its preservation details (relevant when sample preservation is enabled in setup). |
| **Sample Condition** | The condition the sample is in (e.g. chilled, damaged). |
| **Sampling Deviation** | A predefined deviation from the standard sampling procedure. |
| **Analysis Specification** | The set of result ranges to evaluate the results against. Only specifications matching the selected sample type are offered. The ranges are stored with the sample at registration, so later edits of the specification do not silently change them. |
| **Storage Location** | Where the sample is stored in the laboratory. |
| **Priority** | Urgency of the sample: *Highest*, *High*, *Medium* (default), *Low* or *Lowest*. Shown as a flag in listings and usable for sorting work. |
| **Environmental conditions** | Free text describing the environmental conditions during sampling. Cleared automatically when you change the *Primary Sample* selection. |
| **Composite** | Tick when the sample is a composite (a blend of several increments) rather than a grab sample. Can be preset by the sample template. |
| **Invoice Exclude** | Excludes the analyses of this sample from invoicing. |
| **Internal use** | Marks the sample for internal use only: it is visible to lab personnel but hidden from client contacts, like internal-use [partitions](SamplePartitions.md). |
| **Attachment** | One or more files describing the sample; attached to the created sample. |
| **Remarks** | Free-text remarks stored on the sample. |
| **Sample Rejection** | Only shown when the rejection workflow is enabled: tick it and select reasons to register the sample as **rejected** right away (e.g. arrived broken). See [Sample Rejection](SampleRejection.md). |
| **Number of samples** | How many identical samples to create from this column — see [Registering Multiple Samples at Once](#registering-multiple-samples-at-once). |

## Selecting Analyses

Below the fields, the form lists every active analysis service, grouped by
point of capture (**Field Analyses** / **Lab Analyses**) and category:

- Click a category row to expand it, and tick the checkbox of each analysis
  you want, per column. A filter box (*Filter analyses by name…*) helps to
  find services quickly.
- The **info button** (ⓘ) next to a selected analysis shows its details:
  unit, price, methods, calculation dependencies, and the profiles or
  templates it belongs to.
- Selecting a service that depends on other services (through its
  calculation) selects the dependencies automatically.
- A **lock icon** means the analysis cannot be deselected individually,
  because it is part of a selected profile or template. Remove the profile
  or template first (a dialog asks whether its services should be removed
  too).
- A **prohibition icon** appears on analyses that would be conducted beyond
  their **maximum holding time**, counted from the *Date Sampled*. Such
  analyses cannot be selected for that sample.
- Services can define **conditions** — extra questions (text, numbers,
  choices, even file uploads) that appear under the analysis when it is
  selected. Required conditions must be answered before saving; uploaded
  files are stored as attachments.

Whether at least one analysis is mandatory is controlled by the setup
option *Require sample analyses* (enabled by default). When disabled,
samples can be registered without analyses and services can be added later.

## Working with Sample Templates

A **Sample Template** (configured under *Setup*) bundles a complete
registration recipe. Selecting one in the *Sample Template* field:

- fills in the **Sample Type** and **Sample Point** defined by the
  template,
- sets the **Composite** flag as defined,
- selects all the template's **analyses** (locked against individual
  deselection while the template is applied),
- applies the template's **hidden analyses** settings (analyses performed
  but not shown to clients),
- remembers the template's **partition scheme**, which pre-populates the
  partitioning form when the sample is later split — see
  [Sample Partitions](SamplePartitions.md).

Only templates compatible with the selected client and sample type are
offered. Removing the template unlocks its analyses and asks whether they
should be deselected as well.

## Working with Analysis Profiles

**Analysis Profiles** are named groups of services (e.g. "Metals panel").
In the *Analysis Profiles* field you can select **several profiles** for
the same sample:

- All services of a selected profile are ticked automatically and locked;
  removing the profile asks whether to deselect its services too.
- Profiles may define their own **price** (used instead of the sum of the
  individual service prices), which is reflected in the price footer.
- Profiles may define **custom units** and **hidden analyses** for their
  services; both are applied to the created sample automatically.
- Only profiles compatible with the selected client and sample type are
  offered.

## Registering Multiple Samples at Once

There are three complementary ways to register in bulk:

**1. Multiple columns.** Open the form with several columns (the *Add*
button count) — each column is an independent sample. To avoid re-typing:

- Use the **copy button** (») of a field row to copy the value of the
  first column to all other columns.
- Use the **paste button** of a field row to open a small editor where you
  paste one value per line — line 1 goes to Sample 1, line 2 to Sample 2,
  and so on. The paste button only shows for the fields listed in the
  registry setting `sample_add_form_allow_multi_paste` (or, if that list is
  empty, for common text/reference/checkbox fields; date fields are
  excluded).
- Columns left completely empty are simply skipped on save.

**2. The *Number of samples* field.** Set it to *n* and the column is
registered *n* times: *n* identical samples, each with its own ID. The
maximum allowed value is configurable in setup (*Maximum value for 'Number
of samples' field on registration*, default 10).

**3. Save and Copy.** The **Save and Copy** button saves the current form
and immediately reopens it, pre-filled with a copy of the samples just
created — convenient for registering series in rounds.

## Copying Columns from Existing Samples

To register new samples that look like existing ones:

1. Go to the **Samples** listing (or a batch's sample listing) and select
   the source samples with the checkboxes.
2. Press **Copy to new**. The Add Samples form opens with one column per
   selected sample, pre-filled with the source values.
3. Review, adjust and **Save** — new, independent samples are created.

Notes on what gets copied:

- Remarks, attachments and the *Number of samples* value are **not**
  copied, and the new samples get no link to the source.
- If the source sample is a [partition](SamplePartitions.md), the values
  are taken from its **primary** sample.
- Analyses in the workflow states listed in the registry setting
  `sample_add_form_skip_analyses_in_states` (default: *rejected*) are not
  copied.
- Two more registry settings control how samples with partitions are
  copied: `sample_add_form_copy_partitions` (recreate the partition
  structure on the new sample) and
  `sample_add_form_skip_partition_analyses` (ignore analyses living in
  partitions). See [Sample Partitions](SamplePartitions.md) for details.

## Saving the Form

- **Save** validates all columns first. Missing required fields, an
  invalid contact, a too-high *Number of samples* or unanswered required
  service conditions are flagged per column, and nothing is created until
  the errors are fixed. Completely empty columns are ignored.
- Some installations show a **confirmation dialog** ("Do you want to
  continue?") before creation — this is provided by add-ons that hook into
  the form to double-check the entered records. Plain SENAITE saves
  without confirmation.
- **Save and Copy** saves and reopens the form pre-filled with copies of
  the just-created samples.
- **Cancel** abandons the form ("Sample creation cancelled") and returns
  to the previous listing.

After a successful save, a message confirms the created sample IDs, e.g.
*"Sample WATER-0001 was successfully created."*

## After Registration

**Initial workflow state.** The created sample starts in one of these
states (see [Sample Workflow](SampleWorkflow.md)):

- **To be sampled** — when the sampling workflow applies and the sample
  has not been collected yet.
- **Received** — when the setup option **Auto-receive samples** is enabled
  and the registering user has reception privileges. Samples registered by
  client contacts are *not* auto-received. Secondary samples and
  partitions are always received automatically, inheriting the reception
  date of their primary/parent.
- **Sample due** — in all other cases; a lab user receives the sample
  manually when it physically arrives.

If rejection reasons were set in the form, the sample is created and
immediately **rejected** — see [Sample Rejection](SampleRejection.md).

**Sticker printing.** Sticker behaviour is configured in *Setup*, fieldset
*Sticker*:

| Setting | Effect |
|---------|--------|
| **Automatic Sticker Printing** | *Register*: after saving, you are redirected to the sticker view and the barcode labels print automatically. *Receive*: stickers print automatically upon sample reception instead (when *Auto-receive samples* is active, this fires right after registration). *None*: no automatic printing. |
| **Default Sticker Template** | The sticker layout used for automatic printing. |
| **Small / Large Sticker Template** | Default layouts for manually printed small/large stickers; a sample type can override them. |

Stickers can always be printed manually later: select samples in a listing
and use **Print stickers**.

**Immediate results entry.** When the setup option *Immediate results
entry* is enabled and you have the required privileges, saving takes you
straight to a results-entry view for the new samples — useful for field
results or for labs that auto-receive. See
[Results Entry and Verification](ResultsEntryAndVerification.md).

From here the samples follow the normal path: reception, optional
[partitioning](SamplePartitions.md), assignment to
[Worksheets](Worksheets.md) and [Instruments](Instruments.md), results
entry, [verification](ResultsEntryAndVerification.md) and
[publication](ResultsPublication.md).

## Customising the Form

Lab Managers see an extra toolbar icon on the form that opens **Manage
Sample Form Fields**:

- **Reorder** fields by dragging them in the list.
- **Show or hide** fields with the checkboxes. Required fields cannot be
  hidden.
- The configuration applies to **all** Add Samples forms of the instance.
  *Reset* restores the defaults.

Additionally, the number of columns proposed by the *Add* button and the
visibility of whole features (prices, sampling fields, preservation
fields, rejection) follow the setup options listed below.

## Configuration Reference

Setup options (*Setup*) affecting registration:

| Option | Default | Effect |
|--------|---------|--------|
| Default count of Sample to add | 4 | Number of columns proposed when pressing *Add* in a samples listing. |
| Maximum value for 'Number of samples' field on registration | 10 | Upper limit for the *Number of samples* field. |
| Require sample analyses | on | Whether at least one analysis must be selected per sample. |
| Date sampled required | on | Makes *Date Sampled* mandatory (only when the sampling workflow is not active). |
| Enable Sampling / Enable Sampling Scheduling | off | Activates the sample collection workflow and its form fields. |
| Auto-receive samples | off | Receive samples automatically at registration when created by lab personnel (sampling workflow disabled; not applied to client contacts). |
| Immediate results entry | off | Jump straight to results entry after registration. |
| Enable the rejection workflow | off | Shows the *Sample Rejection* field on the form. |
| Automatic Sticker Printing / sticker templates | None | Automatic label printing on registration or reception, and the layouts used. |

Registry settings (*Setup → Registry*, fieldset *Samples*):

| Setting | Default | Effect |
|---------|---------|--------|
| `sample_add_form_copy_partitions` | off | Copying a sample also recreates its partition structure (sample types, containers, preservations, analyses). |
| `sample_add_form_skip_partition_analyses` | off | Analyses that live in partitions are excluded when copying a sample. |
| `sample_add_form_skip_analyses_in_states` | rejected | Analyses in these workflow states are not copied to the new sample. |
| `sample_add_form_allow_multi_paste` | empty | Field names for which the multi-value paste button is shown. When empty, a built-in list of field types applies. |
| `trigger_events_on_sample_creation` | off | Triggers "before" and "after" workflow transition events when samples are created. Slower, but some add-ons need it to operate correctly. |

## Frequently Asked Questions

**Why don't I see the Client field?**
You are registering from within a client (or a batch that belongs to one),
so the client is set automatically and the field is hidden.

**Why can't I deselect an analysis?**
It is locked because it belongs to a selected profile or template, or it is
a calculation dependency of another selected analysis. Remove the profile
or template (or the dependent analysis) first.

**Why is an analysis blocked with a prohibition sign?**
Given the *Date Sampled* you entered, the analysis would be performed
beyond its configured maximum holding time, so the system prevents its
selection.

**Can I register a sample without analyses?**
Yes, if the setup option *Require sample analyses* is disabled. Analyses
can be added to the sample at any time afterwards.

**Why was my sample received automatically?**
Either *Auto-receive samples* is enabled and you are lab personnel, or the
sample is a secondary sample or a partition — those always inherit the
reception of their primary sample.

**Why did the stickers not print after saving?**
*Automatic Sticker Printing* is probably set to *None*, or set to *Receive*
while your samples are still due. You can always print manually via
*Print stickers* from the samples listing.

**Why were some analyses missing when I copied a sample?**
Analyses in skipped workflow states (by default *rejected*) are excluded,
and if `sample_add_form_skip_partition_analyses` is enabled, analyses that
live in partitions are excluded too.

**Can several people be notified of the results?**
Yes — add extra recipients in *CC Contact* and *CC Emails*. Defaults come
from the client's and the contact's own CC settings; see
[Clients and Contacts](ClientsAndContacts.md).

**The form asks me to confirm before saving — why?**
An add-on installed on your instance performs an extra plausibility check
on the entered records. Confirm to proceed or go back and adjust the data.

**Is this the right form for QC samples?**
No. Blanks, controls and reference samples are managed through the quality
control machinery — see [Quality Control](QualityControl.md).
