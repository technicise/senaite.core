# Publishing Results (COA Reports)

Publishing is the final step of the sample workflow: once results have been
verified, the laboratory generates a results report (Certificate of Analysis)
as a PDF and delivers it to the client — typically by email to the sample's
contact and CC contacts.

This guide explains the feature from an end-user perspective: how to publish
verified samples, how the publish preview and report templates work, how to
email reports (including attachments and size limits), how to prepublish
preliminary reports, how to re-publish, and what happens when a published
sample has to be invalidated. Report printing and sticker printing are also
covered.

---

## Table of Contents

1. [What is Publishing?](#what-is-publishing)
2. [Key Concepts](#key-concepts)
3. [Prerequisites and Permissions](#prerequisites-and-permissions)
4. [Publishing a Verified Sample](#publishing-a-verified-sample)
5. [The Publish Preview and Report Templates](#the-publish-preview-and-report-templates)
6. [Emailing Reports to Contacts](#emailing-reports-to-contacts)
7. [Attachments and the Email Size Limit](#attachments-and-the-email-size-limit)
8. [Publishing without Sending an Email](#publishing-without-sending-an-email)
9. [Prepublishing Preliminary Reports](#prepublishing-preliminary-reports)
10. [Re-publishing](#re-publishing)
11. [Where to Find Generated Reports](#where-to-find-generated-reports)
12. [Invalidating a Published Sample and Automatic Retests](#invalidating-a-published-sample-and-automatic-retests)
13. [Report Printing and the Printed Flag](#report-printing-and-the-printed-flag)
14. [Sticker Printing](#sticker-printing)
15. [Configuration Checklist](#configuration-checklist)
16. [Frequently Asked Questions](#frequently-asked-questions)

---

## What is Publishing?

A sample travels through the laboratory workflow — received, results entered,
submitted and verified (see [Sample Workflow](SampleWorkflow.md) and
[Results Entry and Verification](ResultsEntryAndVerification.md)). Once a
sample is **Verified**, its results are approved and ready to be released to
the client.

Publishing does three things:

1. It generates an **Analysis Report**: a PDF rendering of the sample's
   results, stored permanently inside the client record, together with
   metadata about how and to whom it was delivered.
2. It (optionally) **emails** the PDF to the sample's contact, CC contacts
   and CC emails, and to the responsible persons of the involved lab
   departments.
3. It moves the sample to the **Published** state, which marks the results
   as officially released.

A sample can be published as many times as needed — each publication creates
a new report — and, if something turns out to be wrong after publication, the
sample can be **invalidated**, which automatically creates a **retest**
sample so the work can be repeated with full traceability.

## Key Concepts

| Term | Meaning |
|------|---------|
| **Analysis Report** | The generated results report (COA). A stored object containing the PDF, the primary sample, any other contained samples, the recipients and a complete email send log. |
| **Publish Preview** | The page where you preview the report, choose the report template, paper format and orientation before generating it. Provided by the SENAITE IMPRESS add-on, which ships with standard SENAITE installations. |
| **Prepublish** | Publication of a *provisional* report for a sample that is not yet fully verified — at least one of its analyses must have a submitted result. The sample keeps its current state. |
| **Republish** | Publishing an already published sample again, e.g. after adding late analyses or to re-send the report. |
| **Invalidate** | Declaring the published results erroneous. The sample moves to the *Invalid* state and a **retest** sample is created automatically. |
| **Retest** | The new sample automatically created when a sample is invalidated. It carries copies of the original analyses so results can be produced again. |
| **Send Log** | The per-report history of emails sent: date, sender, recipients, responsibles, subject, body and attachments. |

## Prerequisites and Permissions

- Regular publication requires the sample to be in the **Verified** state.
  Prepublication is possible earlier — while the sample is *Received* or
  *To be Verified* — provided at least one analysis has a submitted result.
- Publishing (including prepublish and republish) is protected by the
  permission `senaite.core: Transition: Publish Results`. By default it is
  granted to *Lab Manager*, *Manager* and the dedicated *Publisher* role.
- Invalidating is protected by `senaite.core: Transition: Invalidate` and by
  default is granted to *Lab Manager* and *Manager* only.
- Samples flagged for **internal use** cannot be published or prepublished —
  they are laboratory-internal by definition (see
  [Sample Partitions](SamplePartitions.md)).
- Email delivery requires a working outgoing mail server and a valid
  **Publication 'From' address** (see
  [Configuration Checklist](#configuration-checklist)).
- The report recipients are the sample's **Contact**, **CC Contacts** and
  **CC Emails**, defined when the sample is registered (see
  [Sample Registration](SampleRegistration.md)). Client contacts and their
  publication preferences — including the *Contacts to CC* that are added
  automatically to new samples — are managed as described in
  [Clients and Contacts](ClientsAndContacts.md).

## Publishing a Verified Sample

1. Go to the **Samples** listing (or a client's samples listing) and filter
   by **Verified**, or open a verified sample directly.
2. Select the sample(s) with the checkboxes and press the **Publish**
   button. On a single sample view the same button is available in the
   workflow actions.
3. You are taken to the **publish preview**, where all selected samples are
   rendered as they will appear in the final PDF (see next section). Review
   the reports, pick the template and layout options.
4. From the preview you can typically either:
   - **generate and email** the report — you are taken to the
     [email form](#emailing-reports-to-contacts), and the samples are
     transitioned to *Published* once the email is sent; or
   - **generate the report without emailing** — the report is stored and
     you can publish the samples manually afterwards from the reports
     listing (see
     [Publishing without Sending an Email](#publishing-without-sending-an-email)).

Notes:

- Several samples can be selected and published in one go. Depending on the
  options chosen in the preview, samples can be rendered as one report per
  sample or combined into a single multi-sample PDF (the report then has a
  *primary sample* plus *contained samples*).
- When a **primary sample with partitions** is published, its partitions are
  published automatically as well. Conversely, results of partitions are
  consolidated at the primary level, so the client receives one coherent
  report (see [Sample Partitions](SamplePartitions.md)).
- Publishing also sets the *Date Published* on the sample and moves its
  analyses to their published state.
- Analyses flagged as **Hidden** are excluded from the client-facing report
  by default (see [Analyses Setup](AnalysesSetup.md)). Internal-use
  partitions and their analyses are never included in client reports.

## The Publish Preview and Report Templates

The publish preview is provided by **SENAITE IMPRESS**, the reporting engine
that is installed together with SENAITE LIMS. From the preview page you can:

- see a faithful **preview** of the report exactly as it will be rendered to
  PDF;
- choose the **report template** — SENAITE ships with default single-sample
  and multi-sample templates, and laboratories commonly add their own
  branded templates through add-ons;
- choose the **paper format** (e.g. A4, Letter) and **orientation**
  (portrait/landscape);
- control whether multiple selected samples are merged into a **single
  report** or rendered as **separate reports**.

The template, paper format and orientation used are stored with each
generated report: click the **info icon** next to a report in any reports
listing to open the *Analysis Report* popup, which shows **Template**,
**Paperformat**, **Orientation**, the **Primary Sample**, any **Contained
Samples**, and the complete **Email Log**.

Consult the SENAITE IMPRESS documentation for details on designing custom
report templates.

## Emailing Reports to Contacts

After the reports have been generated, the **Send Analysis Reports via
Email** form lets you deliver them. You reach this form:

- directly from the publish preview when choosing to email, or
- later, from any reports listing: select the report(s) and press
  **Email**. The button hint reads: *"Open email form to send the selected
  reports to the recipients. This will also publish the contained samples of
  the reports after the email was successfully sent."*

The form shows:

| Field | Content |
|-------|---------|
| **From** | The sender, taken from the **Publication 'From' address** configured in *Setup → Notifications* (falls back to the site's mail settings). |
| **Recipients** | The sample's **Contact**, **CC Contacts** and **CC Emails**, each with a checkbox. Contacts without an email address are flagged with a warning icon and cannot be selected. |
| **Responsibles** | The responsible persons of the lab departments involved in the sample's analyses. If *Always send publication email to responsibles* is enabled in *Setup → Notifications*, they are included automatically; otherwise you can tick them individually. |
| **Subject** | Defaults to *"Analysis Results for {client name}"* — editable. |
| **Text** | The email body, prefilled from the **Publication Email Text** template in *Setup → Notifications*. The variable `$recipients` is replaced automatically with the names and emails of the finally selected recipients; the template also supports `$client_name`, `$lab_name` and `$lab_address`. |
| **Attachments** | The report PDF(s) — always attached — plus optional additional attachments (see next section). |

To send:

1. Review/select the recipients and responsibles.
2. Adjust subject and body text if needed.
3. Optionally add additional attachments with the **+** button.
4. Press **Send** (or **Cancel** to abort without publishing).

When you press **Send**, the system first **publishes the samples** of the
selected reports (verified samples are *published*, already published ones
are *republished*, not-yet-verified ones are *prepublished*), then sends
**one email per recipient** and records everything in the report's **send
log**. A confirmation message lists the addresses the message was sent to.

Notes:

- If you select several reports at once and their samples do not share the
  same contacts, a warning appears: *"Not all contacts are equal for the
  selected Reports. Please manually select recipients for this email."* —
  pre-selected checkboxes then only cover the recipients common to all
  reports.
- The email cannot be sent without at least one recipient, a subject, a body
  text and a valid *From* address.

## Attachments and the Email Size Limit

Besides the report PDFs, the email form offers all **attachments** of the
involved samples and their analyses — for example photos uploaded at
registration or raw data files captured during results entry, including
files imported from instruments (see [Instruments](Instruments.md)).

- Attachments belonging to cancelled, rejected or retracted analyses are
  never offered.
- Use the checkbox on each file, or **Select all**, to include them.
- The form permanently displays the number of files and the **total size**
  of the email, and recalculates it as you toggle attachments.

The total size is checked against a maximum:

- The limit is defined by the registry setting `senaite.core.max_email_size`
  and defaults to **15 MB**.
- If the total exceeds the limit, an error is shown — *"Total size of email
  exceeded … MB"* — and the **Send** button is disabled until you deselect
  enough attachments.

## Publishing without Sending an Email

Not every laboratory delivers reports by email. To publish samples without
notifying anyone:

1. Open the client's **Analysis Reports** listing (or the sample's
   **Published results** tab).
2. Select the generated report(s).
3. Press **Publish** — hint: *"Manually publish all contained samples of the
   selected reports."*

All samples contained in the selected reports are transitioned (publish,
republish or prepublish, depending on their current state) without any email
being sent. You can also use **Download** in the same listing to save the
PDFs and deliver them by any other channel.

## Prepublishing Preliminary Reports

Sometimes a client needs preliminary results before the whole sample is
verified — for instance when some analyses take much longer than others.

- The **Prepublish** action is available while the sample is in the
  *Received* or *To be Verified* state, as soon as **at least one analysis
  has a submitted result** (retracted and rejected analyses do not count).
- Prepublish behaves like publish: it opens the publish preview, generates a
  report and optionally emails it. Preliminary results are marked as such on
  the default report templates.
- The key difference: the sample **keeps its current workflow state** — it
  is not moved to *Published*. Work continues normally, and the sample is
  published again (now definitively) once everything is verified.
- Prepublishing a primary sample publishes those of its partitions that are
  already in a publishable state.
- Prepublish requires the same permission as publish
  (`senaite.core: Transition: Publish Results`) and is not available for
  internal-use samples.

## Re-publishing

A sample in the *Published* state offers the **Republish** action. Use it
to:

- re-send a report to the contacts (e.g. it never arrived),
- issue a corrected layout or an updated template,
- publish again after complementary information was added.

Republishing runs through the same preview and email form and creates a
**new** Analysis Report — previous reports are never overwritten, so the
full publication history remains available. The sample stays in the
*Published* state.

If the *results themselves* are wrong, do **not** republish — invalidate the
sample instead (see next section).

## Where to Find Generated Reports

**On the sample — the *Published results* tab:**

Every sample has a **Published results** tab showing all Analysis Reports
that include this sample (as primary or contained sample). For each report
you see:

| Column | Content |
|--------|---------|
| *(info icon)* | Opens the report popup: template, paper format, orientation, contained samples and the full email log. |
| **Primary Sample** | Link to the report's primary sample. |
| **Batch** | The batch of the primary sample, if any (see [Batches](Batches.md)). |
| **Review State** | Current workflow state of the primary sample. |
| **Download PDF** | Direct PDF download link. |
| **Filesize** | Size of the stored PDF. |
| **Published Date** / **Published By** | When and by whom the report was created. |
| **Email sent** / **Sent to** | Whether the report was emailed, and to which addresses. |

**On the client — the *Analysis Reports* tab:**

Each client has an **Analysis Reports** tab listing all reports generated
for that client, with the same columns and the **Download**, **Email** and
**Publish** buttons. This is the central place to re-send or batch-download
COAs for a client.

## Invalidating a Published Sample and Automatic Retests

If published (or verified) results turn out to be erroneous, the sample must
be **invalidated** rather than deleted, so the laboratory keeps full
traceability of what was released to the client.

1. In a samples listing, select the verified/published sample(s) and press
   **Invalidate** (requires the `senaite.core: Transition: Invalidate`
   permission), or use the action on the sample view.
2. The **Sample Invalidation** form opens. For each sample:
   - enter the **Invalidation reason**. Whether the reason is mandatory is
     controlled by the *Invalidation reason required* setting in *Setup*
     (enabled by default);
   - decide whether to send an **Email notification**. The checkbox is
     ticked by default and the form shows the recipients: the laboratory
     managers plus the sample's contact, CC contacts and CC emails.
3. Press **Invalidate** (or **Cancel** to abort).

What happens then:

- The sample moves to the **Invalid** state. Its data and reports remain
  accessible, but it can no longer progress.
- A **retest sample is created automatically**. It is a copy of the
  invalidated sample carrying fresh copies of all its analyses (retracted
  analyses are excluded), ready for [results entry and
  verification](ResultsEntryAndVerification.md) — e.g. via a new
  [worksheet](Worksheets.md).
- The retest gets its own ID from the ID Server entry
  **AnalysisRequestRetest**; the default format `{parent_base_id}-R{retest_count:02d}`
  produces IDs like `WATER-0001-R01`.
- The invalidated sample shows a **warning banner**: who invalidated it,
  when, the reason given, and a link to the retest. The retest links back to
  the invalidated sample, and the samples listing marks invalidated samples
  with a *"Results have been withdrawn"* icon.
- If the email notification was selected, the recipients receive a message
  with the subject *"Erroneous result publication: {sample ID}"*. The body
  is taken from **Email body for Sample Invalidation notifications** in
  *Setup → Notifications* and supports the placeholders `$sample_id`,
  `$sample_link`, `$retest_id`, `$retest_link`, `$reason` and
  `$lab_address`.

Notes:

- A sample can only be invalidated **once** — the retest takes over from
  there. If the retest's results are also wrong, invalidate the retest.
- Invalidation is for results already released as erroneous. For samples
  that should never enter processing at all (broken container, wrong
  paperwork, …), use rejection instead — see
  [Sample Rejection](SampleRejection.md).

## Report Printing and the Printed Flag

Laboratories that deliver paper COAs can track which published samples have
had their report printed:

- Enable **Enable the Results Report Printing workflow** in *Setup*
  (fieldset *Sampling*; disabled by default).
- The samples listing then shows a **Printed** column with three states:
  *Not printed*, *Printed*, and *Republished after last print* (a warning
  that the paper copy on file is outdated).
- Select published samples and press the **Print** button to mark them as
  printed — this stamps the *Date Printed* on the sample's most recent
  report. The date is also visible on the report object itself.

The actual printing is done from the PDF (use **Download PDF** in any
reports listing, then print from your PDF viewer).

## Sticker Printing

Stickers (barcode/QR labels for physical containers) are related to samples
rather than to reports, but they are printed from the same listings:

1. Select one or more samples in a samples listing and press
   **Print stickers**.
2. A preview page opens where you choose the sticker **Template** and the
   **Number of copies**, then print (a PDF is generated for the printer).

Configuration lives in *Setup*, fieldset *Sticker*:

| Setting | Purpose |
|---------|---------|
| **Automatic Sticker Printing** | Print stickers automatically when samples are *registered*, when they are *received*, or never (*None*, the default). When set to *Receive*, receiving samples takes you straight to the sticker printing page. |
| **Default Sticker Template** | Template used for automatic printing. |
| **Small Sticker Template** / **Large Sticker Template** | Defaults for the small/large sticker actions; sample-type-specific small/large stickers can be configured on each sample type. |
| **Default Number of Copies** | How many copies of each sticker are printed by default. |

SENAITE ships with several barcode sticker templates (Code 128, Code 39,
QR, address labels); add-ons can register additional ones.

## Configuration Checklist

Everything publication-related is configured in *Setup*, mainly under the
**Notifications** fieldset:

| Setting | Default | Effect |
|---------|---------|--------|
| **Publication 'From' address** | site email address | Sender address for report emails. Overrides the portal mail settings. |
| **Publication Email Text** | built-in template | Default body for report emails. Supports `$client_name`, `$recipients`, `$lab_name`, `$lab_address`. |
| **Always send publication email to responsibles** | on | Department responsibles automatically receive every publication email. |
| **Invalidation reason required** | on | Whether a reason must be entered when invalidating a sample. |
| **Email body for Sample Invalidation notifications** | built-in template | Body of the invalidation email. Supports `$sample_id`, `$sample_link`, `$retest_id`, `$retest_link`, `$reason`, `$lab_address`. |
| `senaite.core.max_email_size` (registry) | 15 MB | Maximum total size of a publication email including all attachments. |
| **Enable the Results Report Printing workflow** | off | Adds the *Printed* tracking and the *Print* action for published samples. |

Also relevant: the ID Server format for **AnalysisRequestRetest** (retest
IDs), the *Sticker* fieldset (see above), and your outgoing mail server in
the site's mail settings.

## Frequently Asked Questions

**Why is there no Publish button on my sample?**
The sample must be in the *Verified* state and you need the
`senaite.core: Transition: Publish Results` permission (Lab Manager,
Manager or Publisher by default). Samples flagged for internal use cannot be
published at all.

**Does publishing always send an email?**
No. The email step is optional: you can generate reports and publish the
samples manually from the reports listing, or just download the PDFs. The
samples are only auto-published through the email form after the message was
actually sent.

**Who receives the report email?**
The sample's contact, its CC contacts and CC emails — plus the responsible
persons of the involved departments if *Always send publication email to
responsibles* is enabled. Each recipient receives an individual email. See
[Clients and Contacts](ClientsAndContacts.md) for managing contacts.

**The Send button is greyed out — why?**
The total email size (reports + attachments) exceeds the configured limit
(`senaite.core.max_email_size`, 15 MB by default). Deselect attachments
until the size indicator drops below the limit.

**Can I publish results before all analyses are verified?**
Yes — use **Prepublish**. It requires at least one submitted analysis and
produces a provisional report without changing the sample's state. Publish
normally once the sample is verified.

**How do I correct a report that has already been sent?**
If only the presentation is wrong (template, typo in a remark, late
addition), **Republish** — a new report is generated and can be re-sent. If
the *results* are wrong, **Invalidate** the sample: a retest sample is
created automatically and the contacts can be notified that the previous
publication was erroneous.

**Where can I see whether and when a report was emailed?**
In the **Published results** tab of the sample or the client's **Analysis
Reports** listing: the *Email sent* and *Sent to* columns, and the info
popup's *Email Log* with the full history (sender, recipients, subject,
text, attachments, date).

**Is the retest a copy of the original sample?**
It is a new sample copying the registration details and analyses of the
invalidated one (results are not copied). It is not the same as registering
a secondary sample from the same physical material — see
[Secondary Samples](SecondarySamples.md) for that.

**Are partitions published separately?**
Normally not — publishing the primary sample publishes its partitions and
the report consolidates all results at the primary level. See
[Sample Partitions](SamplePartitions.md).

**Are QC analyses included in the report?**
Quality control analyses (blanks, controls, duplicates) live on worksheets,
not on the client's sample, so they are not part of the COA. Hidden analyses
of the sample are excluded from the report by default as well. See
[Quality Control](QualityControl.md) and [Analyses Setup](AnalysesSetup.md).
