# Batches

Batches let you group related samples into one logical unit — for example all
samples of a client order, a sampling round, a project or a production lot.
A batch has its own identifier, an optional client, free-text metadata and
labels, and its samples can be worked on together: listed, filtered,
progressed through the workflow, and even resulted side by side in a
spreadsheet-like *Batch Book*.

This guide explains the feature from an end-user perspective: what batches
are, how to create client and lab batches, how batch IDs and labels work, how
samples get assigned to a batch, the batch workflow, the batch book view and
its sub-groups, and the printing options available.

---

## Table of Contents

1. [What is a Batch?](#what-is-a-batch)
2. [Key Concepts](#key-concepts)
3. [Prerequisites and Permissions](#prerequisites-and-permissions)
4. [Creating a Batch](#creating-a-batch)
5. [Client Batches vs Lab Batches](#client-batches-vs-lab-batches)
6. [Batch IDs, Titles and the Client Batch ID](#batch-ids-titles-and-the-client-batch-id)
7. [Batch Labels](#batch-labels)
8. [Assigning Samples to a Batch](#assigning-samples-to-a-batch)
9. [The Batches Listing](#the-batches-listing)
10. [Batch Workflow States](#batch-workflow-states)
11. [The Batch Book](#the-batch-book)
12. [Sub-groups](#sub-groups)
13. [Printing and Stickers](#printing-and-stickers)
14. [Frequently Asked Questions](#frequently-asked-questions)

---

## What is a Batch?

A **Batch** combines multiple samples into a logical unit. Unlike a
[worksheet](Worksheets.md), which organises *analyses* for the bench, a batch
organises *samples* from an administrative point of view: it answers the
question "which samples belong together?" rather than "who tests what, and
on which instrument?".

Typical uses:

- all samples received under one client order or delivery,
- a sampling campaign or sampling round,
- a study, project or production lot that spans many samples.

Each sample can belong to **at most one batch**. The batch view lists all its
samples, shows the overall **progress** (percentage of analyses completed
across all samples in the batch), and offers a **Batch Book** for quick,
side-by-side results entry.

## Key Concepts

| Term | Meaning |
|------|---------|
| **Batch** | A container that groups samples into a logical unit. Samples keep their own IDs, analyses and workflow — the batch only groups them. |
| **Batch ID** | The system-generated identifier of the batch (e.g. `B-001`), created by the ID Server. |
| **Title** | An optional human-friendly name for the batch. If left empty, the Batch ID is used as the title. |
| **Client Batch ID** | A free-text reference for the batch on the client's side (e.g. the client's own order or lot number). |
| **Batch Label** | A reusable tag (configured in Setup) that can be ticked on a batch to categorise it. Shown as badges in the batches listing. |
| **Batch Book** | A grid view of a batch with one row per sample and one column per analysis, allowing direct results entry. |
| **Batch Sub-group** | An optional tag on each sample (field *Batch Sub-group*) used by the Batch Book to group the rows. Sub-groups are configured in Setup. |

## Prerequisites and Permissions

- Creating batches requires the permission `senaite.core: Add Batch`. By
  default it is granted to Lab Manager, Lab Clerk and Manager — and to the
  Owner role, which means **client contacts can create batches inside their
  own client** (see [Client Batches vs Lab Batches](#client-batches-vs-lab-batches)).
- Assigning a sample to a batch (the *Batch* field on the sample) is
  protected by `senaite.core: Field: Edit Batch`, granted by default to Lab
  Manager, Lab Clerk, Manager and Client.
- The batch workflow transitions are protected by their own permissions:
  `senaite.core: Transition: Close`, `senaite.core: Transition: Reopen`,
  `senaite.core: Transition: Cancel` and
  `senaite.core: Transition: Reinstate`.
- Batch Labels are managed in Setup and require
  `senaite.core: Add BatchLabel` to be added.

## Creating a Batch

1. Open **Batches** from the sidebar navigation (or the **Batches** tab of a
   client — see next section).
2. Click the **Add** button at the top of the listing.
3. Fill in the batch form:

   - **Title** — optional. *"If no Title value is entered, the Batch ID will
     be used."*
   - **Client** — optional. *"Select the client of this batch."* Assigning a
     client turns this into a client batch (see next section).
   - **Client Batch ID** — optional free text; the client's own reference
     for this batch.
   - **Description** — optional free text.
   - **Date** — the batch date; pre-filled with the current date and time.
   - **Batch Labels** — tick any of the labels configured in Setup (see
     [Batch Labels](#batch-labels)).
   - **Remarks** — a running log of remarks, as on samples.

4. Save. The batch receives an auto-generated **Batch ID** and starts in the
   **Open** state.

## Client Batches vs Lab Batches

Batches may or may not be assigned to a [client](ClientsAndContacts.md):

**Lab batches (no client):**

- Live in the central *Batches* folder.
- Are visible to laboratory personnel only — client contacts do not see
  them, even if some of their samples are assigned to them. On such samples,
  client contacts do not even get the *Batch* field offered for edit.
- Useful for internal groupings such as instrument runs, sampling rounds or
  projects spanning several clients.

**Client batches (client assigned):**

- As soon as a client is selected in the batch's *Client* field (at creation
  or later via *Edit*), the batch is automatically **moved into that
  client's folder** and appears in the client's **Batches** tab as well as
  in the central *Batches* listing.
- They are visible to that client's contacts, who can also **create batches
  themselves** from their client's *Batches* tab.
- When registering samples from inside a client batch, the *Client* field of
  the Add form is pre-set to the batch's client.

The central *Batches* listing shows a **Client** and **Client ID** column, so
lab personnel see lab and client batches side by side.

## Batch IDs, Titles and the Client Batch ID

Batch IDs are generated by the ID Server (*Setup → ID Server*, type
**Batch**). The default format is `B-{seq:03d}`, producing:

```
B-001
B-002
B-003
```

The format is fully configurable, like the ID formats of samples and
[partitions](SamplePartitions.md#partition-ids).

Three identifiers appear around a batch, and it helps to keep them apart:

| Identifier | Set by | Purpose |
|------------|--------|---------|
| **Batch ID** | System (ID Server) | Unique, permanent identifier of the batch. |
| **Title** | User (optional) | Friendly display name. Falls back to the Batch ID when empty. |
| **Client Batch ID** | User (optional) | The client's own reference (order number, lot code, …). Shown in the batches listing and in the batch lookup when assigning samples. |

## Batch Labels

Batch Labels are reusable tags for categorising batches:

1. Go to **Setup → Batch Labels**.
2. Click **Add** and give the label a **Name** (e.g. "Urgent",
   "Round Robin", "Stability Study").
3. Labels can be deactivated later; inactive labels no longer appear on the
   batch form.

On the batch add/edit form, all active labels are offered as **checkboxes**
in the *Batch Labels* field — tick as many as apply. In the batches listing
the assigned labels are displayed as badges in the *Batch Labels* column.

## Assigning Samples to a Batch

### At registration

The [Add Samples form](SampleRegistration.md) includes a **Batch** field
(*"Assign sample to a batch"*). The lookup searches existing batches and
shows their **Batch ID**, **Title**, **CBID** (Client Batch ID) and
**Client**, so you can find a batch by any of these. Only active
(non-cancelled) batches are offered.

### Registering samples from inside a batch

The most convenient way to fill a batch:

1. Open the batch and go to its **Samples** tab.
2. Use the **Add** button to open the Add Samples form.
3. The *Batch* field is hidden and set automatically to the current batch.
   If the batch has a client, the *Client* field is pre-set as well.

All samples registered this way land directly in the batch.

### After registration

An existing sample can be assigned to a batch (or moved to another batch, or
removed from its batch) at any time before it is verified:

1. Open the sample and expand its header fields (*Edit*).
2. Set or change the **Batch** field and save.

The *Batch* field stays editable while the sample is in the *Registered*,
*To be sampled*, *Scheduled sampling*, *To be preserved*, *Sample due*,
*Received* or *To be verified* state. Once a sample is **verified,
published, rejected, invalid, cancelled or dispatched**, its batch
assignment can no longer be changed (see
[Sample Workflow](SampleWorkflow.md)).

### Partitions

The batch *Samples* tab lists **root samples only**. If a batched sample has
been split, its [partitions](SamplePartitions.md) are reached through the
primary sample rather than listed separately in the batch.

## The Batches Listing

The central **Batches** folder (and the *Batches* tab of each client) lists
batches with the columns Title, **Progress**, Batch ID, Batch Labels,
Description, Date, Client, Client ID, Client Batch ID, State and Created.

- The **Progress** column shows a progress bar with the average completion
  of all samples in the batch — handy for spotting batches that are ready to
  be closed.
- Filter tabs at the top switch between **Open** (default), **Closed**,
  **Cancelled** and **All**.
- Clicking the batch title or Batch ID opens the batch's *Samples* tab.

## Batch Workflow States

Batches have a simple workflow of their own, independent of the workflow of
the samples they contain:

| State | Meaning |
|-------|---------|
| **Open** | The working state. The batch can be edited, and this is where new batches start. |
| **Closed** | The batch is finished. It becomes read-only: it can no longer be edited or deleted, but remains fully visible. |
| **Cancelled** | The batch was created in error or is no longer wanted. It is read-only and disappears from the active listings (find it under the *Cancelled* filter). |

Available transitions:

- **Close** — from *Open* to *Closed*, permission
  `senaite.core: Transition: Close`.
- **Open** — reopens a *Closed* batch, permission
  `senaite.core: Transition: Reopen`.
- **Cancel** — from *Open* to *Cancelled*, permission
  `senaite.core: Transition: Cancel`.
- **Reinstate** — returns a *Cancelled* batch to *Open*, permission
  `senaite.core: Transition: Reinstate`.

Notes:

- Closing or cancelling a batch does **not** change the workflow state of
  the samples inside it — samples keep progressing through their own
  [workflow](SampleWorkflow.md) (and can still be
  [rejected](SampleRejection.md), verified or published individually).
- Closing is a manual, administrative action; the system does not close
  batches automatically when all samples are published.

## The Batch Book

The **Batch Book** tab of a batch shows all its active samples in a single
grid, designed for rapid results entry across the whole batch:

- One **row per sample**, with columns for Sample, Sample Type, Sample
  Point, Client Order Number, Date Registered and State.
- One additional **column per analysis** performed in the batch (using the
  analysis' short title where available, with the unit displayed next to the
  result).
- Rows are grouped by the samples' **Batch Sub-group**; samples without one
  fall under *"No Subgroup"* (see [Sub-groups](#sub-groups)).

Results can be typed **directly into the grid** for any analysis you are
allowed to edit, except analyses whose result comes from a calculation. As
the view itself explains:

> "The batch book allows to introduce analysis results for all samples in
> this batch. Please note that submitting the results for verification is
> only possible within samples or worksheets, because additional information
> like e.g. the instrument or the method need to be set additionally."

So the typical flow is: capture raw results quickly in the batch book, then
open the individual samples or the [worksheet](Worksheets.md) to complete
instrument/method details and **submit** for
[verification](ResultsEntryAndVerification.md).

The batch book also offers a **Copy to new** button: select samples and copy
them into the Add Samples form as new samples with the same setup — useful
for recurring batches (see also [Sample Registration](SampleRegistration.md)).

## Sub-groups

Sub-groups provide an optional **second grouping level below the batch**:
each sample can carry a *Batch Sub-group* tag, and the
[Batch Book](#the-batch-book) groups its rows by it. Typical examples: a
stability-study batch with sub-groups per time point (*Week 1*, *Week 4*,
…), or a sampling round with a sub-group per location.

### Creating sub-groups

1. Go to **Setup → SubGroups** and click **Add**.
2. Enter a **Name** (e.g. "Week 1") and an optional **Description**.
3. Optionally set the **Sort Key** — *"Float value from 0.0 - 1000.0
   indicating the sort order. Duplicate values are ordered
   alphabetically."* The sort key determines the order in which the groups
   appear in the batch book.
4. Save. Like other setup items, sub-groups can later be **deactivated**;
   inactive sub-groups are no longer offered for selection.

### Assigning a sub-group to a sample

Sub-groups are assigned per **sample**, not per batch:

- On the [Add Samples form](SampleRegistration.md), pick one in the
  **Batch Sub-group** field (*"The assigned batch sub group of this
  request"*).
- On an existing sample, expand the header fields (*Edit*), set the
  **Batch Sub-group** field and save.

A sample holds at most one sub-group. The field is guarded by the same
permission as the *Batch* field and stays editable in the same workflow
states (see [Assigning Samples to a Batch](#assigning-samples-to-a-batch)).

### Effect in the Batch Book

The batch book renders one group of rows per sub-group used in the batch,
ordered by the sub-groups' **Sort Key**, each with the sub-group's name as
group header. Samples without a sub-group are collected under
*"No Subgroup"*. Outside the batch book, sub-groups have no effect — the
samples listing of the batch shows all samples together.

## Printing and Stickers

- Each batch has **Sticker** and **Stickers preview** actions (the sticker
  icons in the batch view). These print a **barcode sticker for the batch
  itself** — the default batch template is a Code 39 barcode sticker of
  40x20 mm carrying the Batch ID — so physical containers, trays or crates
  can be labelled with a scannable batch barcode. *Stickers preview* lets
  you check (and change) the template before printing; the plain *Sticker*
  action opens the print dialog directly.
- From the batch's **Samples** tab you have the same bulk actions as in any
  samples listing: print **sample stickers**, print sample sheets (when the
  printing workflow is enabled), create worksheets for the selected samples,
  and [publish results reports](ResultsPublication.md) for samples that are
  ready.

## Frequently Asked Questions

**Can a sample belong to more than one batch?**
No. The *Batch* field of a sample holds a single batch. If you need multiple
groupings, consider using the batch for the main grouping and *Batch
Sub-groups* or sample metadata for the second level.

**Can client contacts create and see batches?**
Yes — for their own client. A client contact can create batches from the
client's *Batches* tab and sees all batches assigned to that client.
Lab batches without a client are never visible to client contacts.

**Can I still add samples to a closed batch?**
A closed batch is read-only, so the recommended way is to **reopen** it
first. Note that only *cancelled* batches are excluded from the batch lookup
on the sample form, so avoid picking closed batches there by mistake.

**Does closing a batch verify or publish its samples?**
No. The batch state is purely administrative. Samples are submitted,
verified and published individually or via worksheets, exactly as without a
batch.

**Why can't I change the Batch field on a sample?**
Either the sample has progressed too far (the field is locked from the
*Verified* state onwards), or you lack the
`senaite.core: Field: Edit Batch` permission, or you are a client contact
and the sample is assigned to a lab batch that does not belong to your
client.

**How do I get rid of a batch created by mistake?**
**Cancel** it. The batch disappears from the active listings but is kept for
traceability, and can be **reinstated** later if needed. The samples inside
it are not affected — remember to re-assign or cancel them separately.

**Can I enter results for calculated analyses in the Batch Book?**
No. Analyses whose result is computed by a calculation are shown read-only
in the batch book; their results are derived from the entered values of
other analyses (see [Analyses Setup](AnalysesSetup.md)).

**Where do I define Batch Sub-groups?**
Sub-groups are setup items (Setup → SubGroups). Assign one to a sample via
its *Batch Sub-group* field; the batch book uses them to group its rows.
See [Sub-groups](#sub-groups).
