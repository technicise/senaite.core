# Rejecting Samples

Sometimes a sample cannot be processed: it arrives broken, insufficient,
badly preserved, past its holding time, or the paperwork does not match.
SENAITE's **rejection workflow** lets the laboratory formally reject such
samples (or single analyses), record the reasons why, produce a signed
rejection report, and — optionally — notify the client by email
automatically.

This guide explains the feature from an end-user perspective: how to enable
and configure it, how to reject samples at registration or later, how to
reject individual analyses, what the client receives, and how rejection
interacts with partitions and the rest of the workflow.

---

## Table of Contents

1. [What is Sample Rejection?](#what-is-sample-rejection)
2. [Reject, Cancel, Retract or Invalidate?](#reject-cancel-retract-or-invalidate)
3. [Enabling the Rejection Workflow](#enabling-the-rejection-workflow)
4. [Predefined Rejection Reasons](#predefined-rejection-reasons)
5. [Email Notification Settings](#email-notification-settings)
6. [Rejecting Samples at Registration](#rejecting-samples-at-registration)
7. [Rejecting Samples after Registration or Reception](#rejecting-samples-after-registration-or-reception)
8. [Rejecting Individual Analyses](#rejecting-individual-analyses)
9. [The Rejection Report and Client Notification](#the-rejection-report-and-client-notification)
10. [What Happens to Partitions](#what-happens-to-partitions)
11. [Can a Rejection be Undone?](#can-a-rejection-be-undone)
12. [Frequently Asked Questions](#frequently-asked-questions)

---

## What is Sample Rejection?

Rejection is a formal, documented refusal to process a sample. When a sample
is rejected:

- it moves to the **Rejected** state and no further laboratory work is done
  on it — its analyses are rejected along with it, and any of them that were
  placed on [worksheets](Worksheets.md) are removed from their slots;
- the **rejection reasons** (predefined ones and/or free text) are stored
  permanently with the sample and shown in a warning banner at the top of
  the sample view ("*This Sample has been rejected due to the following
  reasons…*");
- a **rejection report** (PDF) is generated and attached to the sample;
- optionally, an **email notification** with the report attached is sent to
  the client contacts.

Rejected samples remain in the system for traceability. The samples listing
gains a **Rejected** filter tab so they can be reviewed at any time, and a
rejected sample can still be **dispatched** (e.g. physically returned to the
client — see the dispatch action in the [Sample Workflow](SampleWorkflow.md)
guide).

Besides whole samples, **individual analyses** can also be rejected, so a
single failed test does not force the rejection of the entire sample (see
[Rejecting Individual Analyses](#rejecting-individual-analyses)).

## Reject, Cancel, Retract or Invalidate?

SENAITE provides several ways to discard work, each meant for a different
situation:

| Action | Typical moment | Reasons required | Client email | Retest created | Reversible |
|--------|----------------|------------------|--------------|----------------|------------|
| **Reject** | Any time before the sample is verified | Yes | Optional (see [notification settings](#email-notification-settings)) | No | No |
| **Cancel** | Before the sample is received, or before results are captured | No | No | No | Yes — *Reinstate* |
| **Retract** (analysis) | After a result was submitted but turns out to be wrong | No | No | Yes — a retest of the analysis | — |
| **Invalidate** | After the sample has been verified or published | Depends on the *Invalidation reason required* setting | Yes | Yes — a whole retest sample | — |

Rules of thumb:

- The **sample itself** is the problem (bad container, not enough volume,
  broken seal…) → **Reject**.
- The sample was registered **by mistake** and never worked on → **Cancel**.
- A **result** is wrong and must be redone → **Retract** the analysis (see
  [Results Entry and Verification](ResultsEntryAndVerification.md)).
- Results were already **verified/published** and must be withdrawn →
  **Invalidate** (see [Results Publication](ResultsPublication.md)).

## Enabling the Rejection Workflow

The rejection workflow is **disabled by default**. To enable it:

1. Go to *Setup* and open the **Analyses** tab.
2. Tick **Enable the rejection workflow** ("*Select this to activate the
   rejection workflow for Samples. A 'Reject' option will be displayed in
   the actions menu.*").
3. Save.

Once enabled:

- A **Reject** button/option appears in the samples listings and in the
  actions menu of individual samples, for the users and states where the
  transition is allowed.
- The **Sample Rejection** field becomes visible in the Add Samples form
  (see [Rejecting Samples at Registration](#rejecting-samples-at-registration)).

Permissions:

- Rejecting a **sample** is protected by the permission
  `senaite.core: Transition: Reject Sample`. By default it is granted to
  Lab Managers, Lab Clerks and site managers.
- Rejecting an **analysis** is protected by the permission
  `senaite.core: Transition: Reject Analysis`. By default it is granted to
  Lab Managers and site managers only.
- Editing the rejection reasons of a sample is protected by the permission
  `senaite.core: Field: Edit Rejection Reasons`.

A sample can be rejected while it is in one of these states: *Scheduled
sampling*, *To be sampled*, *To be preserved*, *Sample due*, *Received* or
*To be verified*. Verified or published samples can no longer be rejected —
use *Invalidate* instead.

## Predefined Rejection Reasons

To speed up the rejection form and keep reasons consistent, define a list of
common reasons up front:

1. Go to *Setup* and open the **Analyses** tab.
2. In **Rejection reasons** ("*Enter the predefined rejection reasons that
   users can select when rejecting a sample*"), type one reason per line,
   for example:

   ```
   Sample container broken
   Insufficient sample volume
   Holding time exceeded
   Improper preservation / temperature
   Sample documentation incomplete
   ```

3. Save.

These reasons then appear as checkboxes in the rejection form and as a
selection list in the Add Samples form. Users can always add free-text
"other reasons" as well, so the predefined list does not have to cover every
case. The predefined list is also printed as a checklist on the
[rejection report](#the-rejection-report-and-client-notification), with the
applicable reasons ticked.

Note: the **Rejected** filter tab of the samples listing is shown when
predefined rejection reasons are configured.

## Email Notification Settings

Two settings under *Setup → Notifications* control the client notification:

| Setting | Purpose |
|---------|---------|
| **Email notification on Sample rejection** | "*Select this to activate automatic notifications via email to the Client when a Sample is rejected.*" Also determines whether the per-sample *Email notification* checkbox in the rejection form comes pre-ticked. |
| **Email body for Sample Rejection notifications** | Template for the body of the notification email. Supports the reserved keywords `$sample_id`, `$sample_link`, `$reasons` and `$lab_address`. |

The default email body reads: "*The sample $sample_link has been rejected
because of the following reasons: … $reasons … For further information,
please contact us under the following address. $lab_address*".

Who receives the email is determined by the sample's **contact**, its **CC
contacts** and any additional **CC email addresses** entered on the sample —
see [Clients and Contacts](ClientsAndContacts.md). Only valid email
addresses are used; if the sample has none, no email can be sent.

## Rejecting Samples at Registration

A sample can be marked as rejected at the very moment it is registered — for
example when the courier delivers a visibly damaged container and the lab
still wants the delivery on record.

1. Open the Add Samples form as usual (see
   [Sample Registration](SampleRegistration.md)).
2. Fill in the sample details and analyses.
3. In the **Sample Rejection** field, tick the **Reject** checkbox. A panel
   unfolds where you can:
   - **Select reasons** — pick one or more predefined rejection reasons,
   - tick **Other reasons** and type a free-text reason.
4. Save the form.

The sample is created normally (so client, analyses and dates are all on
record) and then **immediately rejected**: it lands directly in the
*Rejected* state, the rejection report PDF is generated and attached, and
the client notification email is sent if **Email notification on Sample
rejection** is enabled in Setup.

Notes:

- The **Sample Rejection** field is only visible in the form while the
  rejection workflow is enabled in Setup.
- The field is not available when registering a
  [secondary sample](SecondarySamples.md).

## Rejecting Samples after Registration or Reception

Any sample that has not yet been verified can be rejected from the samples
listings or from the sample view itself:

1. Go to the **Samples** listing — or a client's or a
   [batch's](Batches.md) samples listing — and select one or more samples
   with the checkboxes. Alternatively, open a single sample and choose
   **Reject** from its actions menu.
2. Click the **Reject** button. You are taken to the **Reject samples**
   form, which shows one card per selected sample with its client, sample
   ID and sample type. For each sample:

   - **Rejection reasons** — tick the applicable predefined reasons. If no
     predefined reasons are configured, the form tells you "*There are no
     pre-defined conditions set*" and you use the text box instead.
   - **Other reasons** — free-text box for reasons not covered by the
     predefined list.
   - **Email notification** — tick to send the rejection email to the
     recipients shown ("*Send an email notification to …*"). The checkbox
     comes pre-ticked when the global notification setting is enabled. If
     the sample has no valid email recipients, the checkbox is disabled and
     the form shows "*No email recipients available for this sample*".

3. Press **Reject** to confirm (or **Cancel** to abort).

For every processed sample the system:

- stores the reasons and moves the sample to the **Rejected** state,
- rejects all its analyses (and removes them from worksheets),
- cascades the rejection to its partitions (see
  [What Happens to Partitions](#what-happens-to-partitions)),
- generates and attaches the rejection report PDF,
- sends the notification email when requested.

You are returned to the listing with a confirmation message ("*Rejected N
samples: …*").

**Important:** a sample cannot be rejected without at least one reason —
samples left without any ticked or typed reason are simply skipped, and if
none of the selected samples has a reason the form reports "*No samples were
rejected*".

## Rejecting Individual Analyses

When only part of the work is compromised — one test failed for QC reasons,
one aliquot was spoiled — you can reject single **analyses** instead of the
whole sample:

1. Open the sample (or the [worksheet](Worksheets.md)) and select the
   analyses to discard using the checkboxes.
2. Click the **Reject** button.

Analyses can be rejected while they are *unassigned*, *assigned* to a
worksheet, or awaiting verification (*to be verified*). Once an analysis has
been **verified** it can no longer be rejected — retract or invalidate
instead.

Effects of rejecting an analysis:

- The analysis moves to the **Rejected** state; no result can be captured
  for it anymore, neither manually nor through
  [instrument imports](Instruments.md). Unlike *retract*, **no retest copy
  is created**.
- If it was assigned to a worksheet, it is removed from its slot, and any
  [duplicate analyses](QualityControl.md) created from it on the worksheet
  are rejected/removed as well.
- Analyses whose [calculations](AnalysesSetup.md) depend on the rejected
  analysis are rejected automatically too (and an analysis cannot be
  rejected if a dependent analysis can no longer be rejected).
- Attachments of the rejected analysis are excluded from the results
  report.
- The sample's own state is re-evaluated: rejected analyses are ignored, so
  if all *remaining* analyses are already submitted or verified the sample
  moves forward accordingly — a single rejected analysis never blocks the
  sample. Conversely, if the rejected analysis was the only one submitted,
  the sample rolls back to *Received*.
- Rejected analyses stay visible under the **Invalid** filter of the
  sample's analyses listing.

Note that rejecting an analysis does **not** generate a rejection report or
a client email — those belong to whole-sample rejection only.

## The Rejection Report and Client Notification

Whenever a **sample** is rejected, SENAITE generates a PDF titled **Samples
rejection reporting form** and attaches it to the sample (file name
`<SampleID>-rejected.pdf`, available among the sample's attachments). The
report contains:

- the laboratory name,
- the client's name, client ID and address,
- the sample ID (and client sample ID, if any) and sample type,
- the collection, reception and rejection dates,
- the list of analyses that had been requested,
- the full checklist of predefined rejection reasons, with the applicable
  ones ticked, plus any free-text "other" reasons,
- a **Reviewed by** block with the name, job title, contact details and
  signature image of the user who performed the rejection, and
  **Authorized by** blocks for the managers of the lab departments involved
  in the sample.

If the email notification is sent (globally enabled, or ticked per sample in
the rejection form), the client contact, CC contacts and CC email addresses
receive a message with:

- subject: "*<Sample ID> has been rejected*",
- body: the configurable template with the sample link, the list of reasons
  and the laboratory address,
- the rejection report PDF attached.

## What Happens to Partitions

Rejection **cascades from a primary sample to its partitions**: when you
reject a primary sample, all its [partitions](SamplePartitions.md) are
rejected along with it, and the analyses they contain are rejected too. You
do not (and cannot) keep a partition alive while its primary is rejected.

Related behaviour worth knowing:

- A **partition can be rejected on its own** without affecting its primary
  sample or its sibling partitions.
- Analyses in *rejected* state are ignored when the system evaluates
  whether a sample or partition can progress, so rejected partitions or
  analyses never block the submission, verification or publication of the
  remaining work.
- A partition that must survive independently of a problematic primary can
  be **detached** first (see
  [Detaching a Partition](SamplePartitions.md#detaching-a-partition)).

## Can a Rejection be Undone?

**No.** Rejection is designed to be final:

- A rejected **sample** offers no transition back to an active state. The
  only action still available is **Dispatch** (e.g. to record that the
  rejected material was sent back to the client).
- A rejected **analysis** cannot be reinstated either.

The **Reinstate** action you may know from the [Sample
Workflow](SampleWorkflow.md) applies to **cancelled** samples only: it
returns a cancelled sample (and its analyses and partitions) to the state it
was in before cancellation. This is precisely one of the differences between
*Cancel* and *Reject* — cancellation is reversible and needs no reasons,
rejection is permanent, documented and communicated to the client.

If a sample was rejected by mistake, register it again (the *Copy to new*
action on the rejected sample is a quick way to recreate it) and proceed
with the new sample.

## Frequently Asked Questions

**Why don't I see the Reject option at all?**
The rejection workflow is probably disabled. Enable it in *Setup →
Analyses → Enable the rejection workflow*. Also check that you have the
`senaite.core: Transition: Reject Sample` permission and that the sample is
in a state that still allows rejection (not verified, published, cancelled
or invalid).

**Do I have to pick a predefined reason?**
No. You can type any free text under *Other reasons*. But you must provide
at least one reason of either kind — rejection without reasons is not
possible.

**Is the client always notified?**
No. The email is sent only when *Email notification on Sample rejection* is
enabled in Setup, or when you tick the *Email notification* checkbox for the
sample in the rejection form. And in any case the sample needs at least one
valid recipient email address.

**Who receives the rejection email?**
The sample's contact, its CC contacts and any CC email addresses entered on
the sample — see [Clients and Contacts](ClientsAndContacts.md).

**Can I reject a sample whose results are already verified or published?**
No. Rejection is only possible before verification. For verified or
published samples use **Invalidate** (see
[Results Publication](ResultsPublication.md)), which notifies the client and
creates a retest sample.

**What happens to analyses that were on a worksheet?**
They are rejected and removed from their worksheet slots, together with any
duplicates created from them. The worksheet itself is not otherwise
affected.

**Does rejecting an analysis create a retest?**
No. That is the key difference with **Retract**: retracting creates a fresh
copy of the analysis to be measured again; rejecting simply discards the
analysis for good.

**Where can I find the rejection report later?**
It is stored as an attachment of the rejected sample, named after the sample
ID (e.g. `WATER-0001-rejected.pdf`). The rejection reasons themselves are
also always visible in the banner at the top of the sample view.

**Can I reject a primary sample but keep one of its partitions?**
Not directly — rejection cascades to all partitions. Detach the partition
first if it must live on as an independent sample (see
[Sample Partitions](SamplePartitions.md)).

**Can a rejected sample be deleted?**
Rejected samples are kept for traceability and appear under the *Rejected*
filter of the samples listing. They can still be dispatched, but they are
not meant to be removed.
