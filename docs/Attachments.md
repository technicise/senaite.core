# Attachments

Attachments let you store files alongside your laboratory records: photos
taken at sampling, scanned chain-of-custody forms, raw data files from
instruments, chromatograms, spectra — any file that documents a sample or
supports an analysis result. Attachments can be linked to a **sample** as a
whole or to an **individual analysis**, can be classified with configurable
**attachment types**, and can be carried through to the final results report
and the publication email.

This guide explains the feature from an end-user perspective: how to upload,
edit, reorder and delete attachments, how to make attachments mandatory for
certain analyses, and how attachments reach the published report.

---

## Table of Contents

1. [What is an Attachment?](#what-is-an-attachment)
2. [Key Concepts](#key-concepts)
3. [Prerequisites and Permissions](#prerequisites-and-permissions)
4. [The Attachments Panel on a Sample](#the-attachments-panel-on-a-sample)
5. [Adding an Attachment](#adding-an-attachment)
6. [Editing, Reordering and Deleting Attachments](#editing-reordering-and-deleting-attachments)
7. [Attachments in Results Entry](#attachments-in-results-entry)
8. [Attachments on Worksheets](#attachments-on-worksheets)
9. [Attachments at Sample Registration](#attachments-at-sample-registration)
10. [Making an Attachment Mandatory for an Analysis](#making-an-attachment-mandatory-for-an-analysis)
11. [Attachment Types Setup](#attachment-types-setup)
12. [Attachments in Reports and Publication Emails](#attachments-in-reports-and-publication-emails)
13. [Frequently Asked Questions](#frequently-asked-questions)
14. [Related Guides](#related-guides)

---

## What is an Attachment?

An attachment is a file of any format uploaded into SENAITE and linked to a
record. There are two levels of linkage:

- **Sample attachments** document the sample as a whole — e.g. a photo of
  the sampling point, the signed request form, a delivery note.
- **Analysis attachments** belong to one specific analysis of the sample —
  e.g. the raw instrument output, a chromatogram or a calculation sheet that
  supports that particular result.

Both kinds are managed together in the sample's **Attachments** panel, and
both can be offered to the client: either embedded/linked in the results
report or as additional files in the publication email.

## Key Concepts

| Term | Meaning |
|------|---------|
| **Attachment** | A file linked to a sample or to one of its analyses. Each attachment gets its own auto-generated ID and records the date it was loaded. |
| **Attachment Type** | A configurable label (e.g. *Photo*, *Chain of Custody*, *Raw Data*) used to classify attachments. Managed under *Setup → Attachment Types*. |
| **Render in Report** | A per-attachment flag. When ticked, the attachment is made available to the results report; only **images** can be displayed in the report directly. |
| **Attachment Keys** | Free-text keywords stored with the attachment, useful for searching and for extra context in listings and the email form. |
| **Attachment required for verification** | A setting on an analysis service that makes an attachment **mandatory** before the result can be submitted. |

A few structural rules:

- An attachment is linked either to the sample or to exactly one analysis —
  the sample's Attachments panel shows both, with the **Analysis** column
  telling them apart.
- When a published sample is invalidated and a retest copy is created, the
  copy shares the original's attachments. A shared attachment is only
  physically removed once no sample or analysis references it any more.
- Attachments of **cancelled**, **retracted** or **rejected** analyses are
  hidden from the sample's Attachments panel (they remain downloadable from
  the analyses listing) and are automatically excluded from reports and
  publication emails.

## Prerequisites and Permissions

Three dedicated permissions govern the Attachments panel of a sample:

- `senaite.core: Sample: Add Attachment` — shows the *Add new Attachment*
  form.
- `senaite.core: Sample: Edit Attachment` — allows changing type, keywords,
  *Render in Report* and the order.
- `senaite.core: Sample: Delete Attachment` — shows the *Remove* column.

By default these are granted to **Lab Manager**, **Lab Clerk** and
**Analyst** (plus site managers). What is possible also depends on the
**workflow state** of the sample:

| Sample state | Add | Edit | Delete |
|--------------|-----|------|--------|
| Registered, Due, Received, To be Verified, To be Preserved | Lab roles | Lab roles | Lab roles |
| To be Sampled, Scheduled Sampling | Lab roles + Sampler, Sampling Coordinator | same | same |
| Verified | Lab Manager, Publisher | Lab Manager, Publisher | nobody |
| Published | Lab Manager | Lab Manager | nobody |
| Rejected, Invalid, Cancelled, Dispatched | nobody | nobody | nobody |

In addition, attachments that belong to an **analysis** can only be edited
or removed by users who may still edit that analysis's result. Once an
analysis has been submitted or verified, its attachments become read-only
for users without result-edit privileges on it.

On worksheets, the attachments panel is protected by the permission
`senaite.core: Worksheet: Add Attachment`, granted by default to
**Analyst** and **Lab Manager**.

## The Attachments Panel on a Sample

Open a sample and press the **Attachments** button (paperclip icon) above
the analyses — the panel expands and shows a table of all attachments of the
sample and of its (valid) analyses:

| Column | Content |
|--------|---------|
| **Name** | The filename; click it to download the file. |
| **Type** | The attachment type (dropdown if you may edit). |
| **Size** | The file size. |
| **Analysis** | The analysis the attachment belongs to — empty for sample-level attachments. |
| **Keywords** | Free-text keywords (editable). |
| **Render in Report** | Checkbox controlling whether the attachment is passed to the results report. |
| **Remove** | Checkbox to delete the attachment (only shown with delete privileges). |

Below the table, the *Add new Attachment* form lets you upload further
files (see next section).

## Adding an Attachment

1. Open the sample and expand the **Attachments** panel.
2. In the **Add new Attachment** row, use the **Browse** button to select a
   file — the *Add Attachment* button becomes active once a file is chosen.
3. Choose the **Type** (see [Attachment Types Setup](#attachment-types-setup)).
4. In the **Analysis** dropdown, either keep **Attach to Sample** to link
   the file to the sample itself, or pick one of the sample's analyses to
   link it to that analysis. Only analyses whose result you are still
   allowed to edit are offered; blanks, controls and duplicates are labelled
   accordingly.
5. Optionally enter **Keywords**.
6. Tick **Render in Report** if the file (an image) should be displayed in
   the results report.
7. Press **Add Attachment**.

A confirmation message tells you whether the file was attached to the
sample or to the selected analysis, and the new row appears at the end of
the attachments table.

## Editing, Reordering and Deleting Attachments

All maintenance happens in the same table of the Attachments panel:

1. **Edit** — change the *Type*, *Keywords* or *Render in Report* value of
   any row you are allowed to edit.
2. **Reorder** — drag and drop the rows: the row order is remembered and is
   the order in which the attachments appear in the results report.
3. **Delete** — tick the **Remove** checkbox of the rows to delete.
4. Press **Update Attachments** to save all changes at once.

Notes on deletion:

- The file is only physically removed if no other sample or analysis still
  references it. If, for example, the attachment is shared with the retest
  copy of an invalidated sample, the link to the current record is removed
  but the file itself is retained.
- Deletion is not possible any more once the sample is *Verified* or
  *Published* (see the permission table above).

## Attachments in Results Entry

The analyses listings — on the sample view and on worksheets — include an
**Attachments** column:

- For each analysis, its attachments are shown as **download links**
  (paperclip), one per file.
- If the analysis service is flagged **Attachment required for
  verification** and no file is attached yet, a **warning icon** with the
  hint *"Attachment required"* is displayed instead — and the result cannot
  be submitted until a file is attached (see
  [Making an Attachment Mandatory](#making-an-attachment-mandatory-for-an-analysis)).

Uploading is not done in the listing row itself, but through the
**Attachments panel**: on the sample view as described above, or on the
worksheet's results view (next section).

## Attachments on Worksheets

The worksheet results-entry view has its own **Attachments** panel, geared
towards attaching instrument output while entering results:

1. Open the worksheet's results view and expand the **Attachments** panel.
2. Select the file, the **Type** and optional **Keywords**.
3. Target the attachment with one of two dropdowns:
   - **Analysis** — a single analysis of the worksheet, listed with its
     slot position and sample ID (e.g. `3: WATER-0012 - pH`);
   - **All Analyses of Service** — attaches a copy of the file to *every*
     analysis of the selected service present on the worksheet (handy for a
     single instrument run covering many samples).
4. Press **Add Attachment**.

Only analyses whose results you may still edit are offered. Attachments
added from the worksheet form are flagged to render in the report; you can
untick that later from the sample's Attachments panel if needed.

**Automatic attachments from instrument imports:** when results are
imported from an instrument file, the raw file itself is attached to the
affected analyses automatically. The system creates (or reuses) an
attachment type named after the file format, stores the keywords
*"Results, Automatic import"*, and leaves *Render in Report* unticked. See
[Instruments](Instruments.md).

## Attachments at Sample Registration

The Add Samples form includes an **Attachment** field that accepts one or
more files per sample — *"Add one or more attachments to describe the
sample, or to specify your request"*. Files uploaded here:

- are linked to the sample once it is created,
- have *Render in Report* **unticked** by default (you can enable it later
  from the Attachments panel),
- are **not** carried over when the sample row is copied to register
  several similar samples.

Analyses can also acquire attachments at registration through **analysis
conditions** of type *file* (configured on the analysis service): the
uploaded file is attached directly to the corresponding analysis. See
[Registering Samples](SampleRegistration.md) and
[Analysis Services, Profiles & Sample Templates](AnalysesSetup.md).

## Making an Attachment Mandatory for an Analysis

Some tests should never be reported without their supporting raw data. To
enforce this:

1. Go to *Setup → Analysis Services* and open the service.
2. On the **Analysis** tab, tick **Attachment required for verification**
   (*"Make attachments mandatory for verification"*).
3. Save.

The effect on every analysis of that service:

- In results entry, the **Attachments** column shows a **warning icon**
  (*"Attachment required"*) as long as no file is attached.
- The result **cannot be submitted**: the submit action is blocked for that
  analysis — exactly like a missing result — until an attachment is linked
  to the analysis (an attachment on the sample alone is not enough).
- Consequently the analysis can never reach verification without its
  attachment.

In older versions this setting was a three-way *Attachment Option*
(*required / permitted / not permitted*); it has since been consolidated
into this single checkbox — attachments are always permitted, and the
checkbox controls whether one is required.

## Attachment Types Setup

Attachment types are simple labels used to classify attachments:

1. Go to *Setup → Attachment Types*.
2. Press **Add** and give the type a **Name** (e.g. *Photo*, *Chain of
   Custody*, *Raw Data*, *Certificate*) and an optional **Description**.
3. Save.

Management notes:

- Types can be **deactivated** rather than deleted; the listing offers
  *Active*, *Inactive* and *All* filters. Only active types are offered in
  the attachment forms.
- Adding types requires the permission
  `senaite.core: Add AttachmentType` (Lab Manager and Lab Clerk by
  default).
- Instrument result imports create attachment types automatically, named
  after the imported file's format, if a matching one does not exist yet.

## Attachments in Reports and Publication Emails

Attachments can reach the client through two channels:

**1. Inside the results report (COA).** Attachments whose **Render in
Report** flag is ticked are made available to the report template, in the
order of the attachments table (drag and drop to change it). Whether and
where they appear depends on the report template used by your publisher
add-on; as the flag's help text notes, *only images can be rendered in the
report directly* — other file formats should be sent as email attachments
instead. Two automatic safeguards apply:

- Retracting or rejecting an analysis automatically **unticks** *Render in
  Report* on its attachments, so superseded raw data never leaks into the
  final report.
- Inline images pasted into a results interpretation are converted into
  attachments that are excluded from the attachments rendering (they appear
  within the interpretation text itself, see
  [Interpretation Templates](InterpretationTemplates.md)).

**2. As files attached to the publication email.** The *Send Analysis
Reports via Email* form lists, next to the report PDFs, **all attachments**
of the involved samples and their analyses (except those of cancelled,
retracted or rejected analyses). Tick the ones to include; the form tracks
the total email size against the configured limit (15 MB by default). See
[Attachments and the Email Size Limit](ResultsPublication.md#attachments-and-the-email-size-limit)
in the publication guide for the full walkthrough.

## Frequently Asked Questions

**Why can't I submit a result even though it is filled in?**
Most likely the analysis service is flagged **Attachment required for
verification** and no file is attached to the analysis yet — the warning
icon in the *Attachments* column confirms it. Attach a file to that
analysis (not just to the sample) and submit again.

**What file formats can I attach?**
Any format. However, only **images** can be displayed inside the results
report; other formats (PDF, spreadsheets, raw data files) are better sent
as additional attachments of the publication email.

**My attachment does not show up in the published report. Why?**
Check that (a) its **Render in Report** checkbox is ticked, (b) it is an
image, (c) its analysis has not been retracted or rejected (this unticks
the flag automatically), and (d) the report template in use actually
renders attachments.

**Where did the "Attachment Option" (required/permitted/not permitted) go?**
It was consolidated into the single **Attachment required for
verification** checkbox on the analysis service. Attachments are now always
permitted; the checkbox only controls whether one is mandatory.

**Can I delete an attachment from a verified or published sample?**
No. Deletion is disabled in those states; in *Verified* and *Published*
only managers (and publishers, while verified) can still add or edit
attachments, e.g. to fix an attachment type before publication.

**I removed an attachment but the file still exists elsewhere.**
Attachments can be shared between records — typically between an
invalidated sample and its retest copy. The file is only physically deleted
once no sample or analysis references it any more.

**Why don't I see attachments of a retracted analysis in the panel?**
Attachments of cancelled, retracted and rejected analyses are hidden from
the sample's Attachments panel to keep it focused on valid work. You can
still download them from the analyses listing.

**Can client contacts upload attachments?**
Not from the sample's Attachments panel — by default the add/edit/delete
permissions are granted to laboratory roles only. Clients can, however,
download the attachments of the samples they may view.

**Does the order of the rows matter?**
Yes. The row order of the attachments table — adjustable by drag and drop —
is the order in which the attachments are handed to the results report.

## Related Guides

- [Publishing Results (COA Reports)](ResultsPublication.md) — the email
  form, additional attachments and the email size limit.
- [Results Entry & Verification](ResultsEntryAndVerification.md) — entering
  and submitting results, where required attachments block submission.
- [Analysis Services, Profiles & Sample Templates](AnalysesSetup.md) —
  configuring analysis services, including the *Attachment required for
  verification* setting and file-type analysis conditions.
- [Registering Samples](SampleRegistration.md) — uploading attachments in
  the Add Samples form.
- [Worksheets](Worksheets.md) — the results-entry environment with its own
  attachments panel.
- [Instruments](Instruments.md) — automatic attachment of imported
  instrument result files.
- [User Roles & Permissions](UserRolesAndPermissions.md) — the roles
  referenced in the permission table above.
- Back to the [User Guide index](UserGuide.md).
