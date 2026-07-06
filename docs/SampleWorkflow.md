# Sample Lifecycle & Workflow States

Every sample in SENAITE moves through a well-defined lifecycle: it is
registered, optionally collected and preserved, received by the laboratory,
analysed, verified and finally published to the client. Alongside this main
path, samples can be cancelled, rejected, invalidated, dispatched or split
into partitions.

This guide explains the lifecycle from an end-user perspective: what each
state means, which actions are available in each state, who may perform them,
and how the optional sampling, scheduling, preservation and auto-receive
settings change the flow.

---

## Table of Contents

1. [The Lifecycle at a Glance](#the-lifecycle-at-a-glance)
2. [Workflow States](#workflow-states)
3. [The Standard Path: Registered to Published](#the-standard-path-registered-to-published)
4. [The Sampling Workflow](#the-sampling-workflow)
5. [Scheduled Sampling](#scheduled-sampling)
6. [Sample Preservation](#sample-preservation)
7. [Receiving Samples and Auto-Receive](#receiving-samples-and-auto-receive)
8. [Cancelling and Reinstating a Sample](#cancelling-and-reinstating-a-sample)
9. [Invalidating a Sample and Retests](#invalidating-a-sample-and-retests)
10. [Dispatching and Restoring a Sample](#dispatching-and-restoring-a-sample)
11. [Rejecting a Sample](#rejecting-a-sample)
12. [Detaching a Partition](#detaching-a-partition)
13. [Who Can Transition a Sample](#who-can-transition-a-sample)
14. [Partitions, Secondary Samples and Retests](#partitions-secondary-samples-and-retests)
15. [Frequently Asked Questions](#frequently-asked-questions)

---

## The Lifecycle at a Glance

With the default configuration (sampling workflow disabled), a sample
follows this path:

```
Registered → Sample due → Received → To be verified → Verified → Published
```

With the **sampling workflow** enabled, two collection steps are inserted at
the beginning:

```
Registered → To be sampled [→ Scheduled sampling] → Sample due → Received → …
```

The *Registered* state is transient: as soon as a sample is created (see
[Sample Registration](SampleRegistration.md)), the system automatically moves
it either to *To be sampled* (sampling workflow enabled) or to *Sample due*
(sampling workflow disabled).

From most states the sample can also branch off to *Cancelled*, *Rejected*,
*Invalid* or *Dispatched* — these side paths are covered in their own
sections below.

## Workflow States

| State | Meaning | Typical next step |
|-------|---------|-------------------|
| **Registered** | The sample has just been created in the system. Transient — it immediately becomes *To be sampled* or *Sample due*. | Automatic |
| **To be sampled** | The physical sample still needs to be collected by laboratory personnel. Only shown when the sampling workflow is enabled. | *Sample*, *Schedule sampling* |
| **Scheduled sampling** | A sampling round has been scheduled: a sampler and an expected sampling date are assigned. Only used when sampling scheduling is enabled. | *Sample* |
| **Sample due** | The sample has been collected (or collection was not required) and the laboratory is waiting for it to arrive. | *Receive* |
| **To be preserved** | The sample was identified as requiring preservation. Kept for compatibility — see [Sample Preservation](#sample-preservation). | *Preserve* |
| **Received** | The sample is physically in the laboratory. Results can now be entered on its analyses. | *Submit* (promoted from analyses) |
| **To be verified** | All results have been submitted and await review. | *Verify*, *Retract* |
| **Verified** | All results have been reviewed and approved. The sample becomes mostly read-only. | *Publish*, *Invalidate* |
| **Published** | The results report has been generated and delivered to the client. | *Republish*, *Invalidate* |
| **Cancelled** | The request was withdrawn before any results were captured. Can be undone. | *Reinstate* |
| **Rejected** | The sample was refused by the laboratory (e.g. arrived in poor condition). Final. See [Sample Rejection](SampleRejection.md). | *Dispatch* only |
| **Invalid** | Previously verified/published results were invalidated. A retest sample is created automatically. Final. | *Dispatch* only |
| **Dispatched** | The sample was sent out of the laboratory (e.g. to a reference lab or back to the client). | *Restore* |

Notes on how the sample moves forward:

- The *Submit* and *Verify* steps of the **sample** are not triggered
  directly by the user. They are **promoted automatically** when all the
  analyses contained in the sample have been submitted or verified,
  respectively (see
  [Results Entry and Verification](ResultsEntryAndVerification.md)).
- Analyses in *cancelled*, *rejected* or *retracted* states are ignored when
  the system decides whether the sample can move on, so a single discarded
  analysis never blocks the sample.
- If an analysis of a sample in *To be verified* is retracted, the sample
  automatically **rolls back** to *Received* so results can be worked on
  again. This rollback also removes a *Verified* flag if one had been set.

## The Standard Path: Registered to Published

1. **Register** the sample from the Add Samples form (see
   [Sample Registration](SampleRegistration.md)). With the default setup the
   sample lands in **Sample due**.
2. **Receive** the sample when it physically arrives: select it in the
   samples listing (or open it) and use the **Receive** action. The reception
   date is recorded and the analyses become ready for result entry. A sample
   can only be received when its *Date Sampled* is set.
3. **Enter and submit results** for the analyses, either directly on the
   sample's *Analyses* tab, through [Worksheets](Worksheets.md), or imported
   from [Instruments](Instruments.md). When all analyses are submitted, the
   sample moves to **To be verified** automatically.
4. **Verify** the results. When all analyses are verified, the sample becomes
   **Verified**. From this point on, most sample fields are read-only.
5. **Publish** the results report to the client contact. The sample reaches
   **Published**. A **Republish** action remains available to generate and
   send the report again (see
   [Results Publication](ResultsPublication.md)).

Two publication-related shortcuts exist earlier in the flow:

- **Prepublish** — available while the sample is *Received* or *To be
  verified*, as long as at least one analysis has been submitted. It
  produces a **provisional report** without changing the sample state.
- **Rollback** — an automatic, system-triggered transition that returns a
  *To be verified* or *Verified* sample to *Received* when one of its
  analyses is retracted, keeping sample and analyses consistent.

Analyses can be added to a sample while it is due or received (and by lab
managers even later); see [Analyses Setup](AnalysesSetup.md) for how
services, profiles and templates are configured, and
[Quality Control](QualityControl.md) for QC samples, which follow their own
workflow on worksheets. Samples grouped in [Batches](Batches.md) each follow
this lifecycle individually.

## The Sampling Workflow

By default, SENAITE assumes the sample is collected **before** it is
registered. If your laboratory also carries out the collection (field work),
enable the sampling workflow:

1. Go to *Setup*, fieldset **Sampling**.
2. Tick **Enable Sampling** (setting `sampling_workflow_enabled`).

From then on, newly registered samples start in **To be sampled** instead of
*Sample due*. Sample Templates can override this per template, so individual
sample types can require (or skip) the collection step regardless of the
global setting.

To record the collection:

1. Select the sample(s) in the listing, or open the sample.
2. Use the **Sample** action.
3. The transition requires a **Date Sampled** and a **Sampler** to be set —
   users with the Sampler role can trigger it directly, and the current date
   and their own user are filled in automatically when empty.
4. The sample moves to **Sample due**, waiting for reception.

While the sample is *To be sampled* or *Scheduled sampling*, samplers can
also enter results for **field analyses** (analyses performed at the point
of collection). A sample that only contains field analyses can even jump
straight to *To be verified* once all of them are submitted.

## Scheduled Sampling

When collections are planned in rounds, an extra scheduling step can be
inserted between registration and collection:

1. Go to *Setup*, fieldset **Sampling**.
2. Tick **Enable Sampling Scheduling** (setting
   `schedule_sampling_enabled`). This only takes effect when the sampling
   workflow itself is active.

With scheduling enabled, samples in *To be sampled* offer a **Schedule
sampling** action:

1. Set the **Expected Sampling Date** and the **Scheduled Sampler** on the
   sample.
2. Trigger **Schedule sampling** — the sample moves to **Scheduled
   sampling**.
3. When the collection actually happens, use the **Sample** action as usual
   to move it to *Sample due*.

Scheduling is primarily meant for the **Sampling Coordinator** role, which
can schedule, collect and receive samples but is not involved in results
management.

## Sample Preservation

Preservation support is controlled by **Enable Sample Preservation**
(setting `sample_preservation_enabled`) in *Setup*, fieldset **Sampling**.
When enabled:

- The **Preservation**, **Preserver** and **Date Preserved** fields become
  visible on samples, so the preservation applied to each sample (or
  [partition](SamplePartitions.md)) can be recorded.
- The samples listing shows a **To be preserved** filter.

About the *To be preserved* state itself: the workflow defines it — together
with a **Preserve** action (guarded by the
`senaite.core: Transition: Preserve Sample` permission, granted to
Preservers and Lab Managers) that returns the sample to *Sample due* — but
in current versions of SENAITE samples are **not routed through this state
automatically**; it is kept for compatibility with older versions and
add-ons. In practice, preservation is handled as data on the sample and,
most naturally, per partition: each partition can carry its own container
and preservation, as defined manually or through sample templates (see
[Sample Partitions](SamplePartitions.md)).

## Receiving Samples and Auto-Receive

Reception is the moment the laboratory takes responsibility for the physical
sample:

- Use the **Receive** action from the samples listing (several samples at
  once) or from the sample view.
- The **Date Received** is set to the current date and time, and results can
  be entered from then on.
- The action requires the `senaite.core: Transition: Receive Sample`
  permission — by default Lab Clerks, Lab Managers and Sampling
  Coordinators. It is only possible when the sample has a *Date Sampled*.

### Auto-receive

If your laboratory registers samples that are already on the bench, the
reception click can be skipped:

1. Go to *Setup*, fieldset **Sampling**.
2. Tick **Auto-receive samples** (setting `autoreceive_samples`).

When enabled, samples are received **automatically upon registration**,
provided that:

- the sampling workflow is **disabled** for the sample (auto-receive never
  skips the collection steps), and
- the sample is created by **laboratory personnel**. Samples registered by
  client contacts are *not* auto-received — the lab still confirms their
  arrival manually (see [Clients and Contacts](ClientsAndContacts.md) for
  what client users can do).

Note that [partitions](SamplePartitions.md) and
[secondary samples](SecondarySamples.md) are always received automatically,
regardless of this setting, because their physical sample is already in the
laboratory.

## Cancelling and Reinstating a Sample

Cancellation is meant for requests that should never have been submitted, or
were submitted wrongly. No notification is sent and the sample is simply
taken out of circulation.

- **When:** available while the sample is *To be sampled*, *Scheduled
  sampling*, *Sample due*, *To be preserved* or *Received* — but only as
  long as **no analysis has been assigned to a worksheet or submitted**.
  Once results exist, use *Retract*, *Reject* or *Invalidate* instead.
- **Who:** users with `senaite.core: Transition: Cancel Analysis Request` —
  by default Lab Clerks, Lab Managers, Managers and the sample **Owner**
  (i.e. the client contact who created it).
- **How:** select the sample(s) and use the **Cancel** action. The
  cancellation cascades to the sample's analyses and to its
  [partitions](SamplePartitions.md); a primary sample can only be cancelled
  if all of its partitions can be cancelled too.

A cancelled sample can be brought back with the **Reinstate** action
(permission `senaite.core: Transition: Reinstate Analysis Request`, same
default roles). The sample returns to the exact state it was in before
cancellation, and the reinstatement cascades to its analyses and partitions.
A cancelled partition can only be reinstated if its primary sample is not
itself cancelled (or is reinstated as well).

## Invalidating a Sample and Retests

Invalidation is the controlled way to flag results as wrong **after** they
have been verified or even published:

- **When:** available in *Verified* and *Published* states only.
- **Who:** users with `senaite.core: Transition: Invalidate` — by default
  Lab Managers and Managers only.
- **How:**
  1. Select the sample(s) and choose **Invalidate**.
  2. Enter the **reason** for the invalidation. Whether the reason is
     mandatory is controlled by the *Setup* option requiring an invalidation
     reason.
  3. Optionally tick the **notify** checkbox to send an email to the client
     contact informing them that the results are no longer valid.

What happens next:

- The sample moves to the final **Invalid** state and becomes read-only.
- A **retest sample** is created automatically: a full copy of the
  invalidated sample with the same analyses, ready to be analysed again. By
  default its ID is derived from the original with an `-R` suffix (e.g.
  `WATER-0001-R01`), as configured in the ID Server for the retest type.
- Both samples display an informative banner: the invalid sample links to
  its retest (with date, user and reason of the invalidation), and the
  retest links back to the sample it supersedes.

Do not confuse invalidation with **Retract**: retracting happens *before*
verification is complete (sample in *To be verified*), returns the sample to
*Received*, and creates retest **analyses** inside the same sample rather
than a whole new sample (see
[Results Entry and Verification](ResultsEntryAndVerification.md)).

## Dispatching and Restoring a Sample

Dispatch marks a sample as having physically **left the laboratory** — for
example, forwarded to a reference laboratory or returned to the client —
while keeping its record and full history in the system.

- **When:** available for received samples in most states (*Received*, *To
  be verified*, *Verified*, *Published*, and even *Rejected* or *Invalid*).
  A sample **cannot be dispatched while any of its analyses is assigned to a
  worksheet** — unassign or submit them first (see
  [Worksheets](Worksheets.md)).
- **Who:** users with `senaite.core: Transition: Dispatch Sample` — by
  default Lab Managers and Managers only.
- **How:** select the sample(s) and choose **Dispatch**. A form asks for a
  mandatory **reason/comment**, which is stored in the sample's audit trail.

While dispatched, the sample is read-only: no results can be entered and no
fields edited. Use **Restore** (permission
`senaite.core: Transition: Restore Sample`, same default roles) to bring the
sample back — it returns to the state it was in before being dispatched, and
work can continue normally.

Dispatch and partitions: dispatching a primary sample dispatches its
partitions as well; when all partitions of a primary have been dispatched
individually, the primary follows automatically. Restoring works the same
way in reverse (see [Sample Partitions](SamplePartitions.md)).

## Rejecting a Sample

Rejection is used when the laboratory refuses to process a sample — for
instance because it arrived broken, insufficient or too late. It differs
from cancellation in that a **rejection reason is recorded** and the client
contact can be **notified automatically** with a rejection report.

- **When:** available from *To be sampled*, *Scheduled sampling*, *Sample
  due*, *To be preserved*, *Received* and *To be verified* — i.e. any time
  before the results are verified.
- **Who:** users with `senaite.core: Transition: Reject Sample` — by default
  Lab Clerks, Lab Managers and Managers.
- **Requires:** the rejection workflow must be enabled in *Setup* (with
  predefined rejection reasons); otherwise the action is not offered.

The *Rejected* state is final (apart from *Dispatch*). Rejecting a primary
sample also rejects its partitions. The full feature — reasons, client
notifications and the rejection report — is covered in
[Sample Rejection](SampleRejection.md).

## Detaching a Partition

The **Detach** action applies to [sample partitions](SamplePartitions.md)
only. It unlinks a partition from its primary sample so it continues life as
an independent sample:

- Available while the partition is *Sample due*, *Received*, *To be
  verified*, *To be preserved* or *Verified*.
- Protected by `senaite.core: Transition: Detach Sample Partition` — by
  default Lab Clerks, Lab Managers and Managers.
- The partition's own workflow state does **not** change; it simply stops
  being followed by its primary, and keeps a permanent "Detached from"
  reference for traceability.

See [Detaching a Partition](SamplePartitions.md#detaching-a-partition) for
details.

## Who Can Transition a Sample

Each transition is guarded by a dedicated permission. The table lists the
**default** role assignments — your administrator may have adjusted them.

| Action | Permission | Default roles |
|--------|------------|---------------|
| Schedule sampling | `senaite.core: Transition: Schedule Sampling` | Lab Manager, Manager, Sampling Coordinator |
| Sample | `senaite.core: Transition: Sample Sample` | Lab Manager, Manager, Sampler, Sampling Coordinator |
| Preserve | `senaite.core: Transition: Preserve Sample` | Lab Manager, Manager, Preserver |
| Receive | `senaite.core: Transition: Receive Sample` | Lab Clerk, Lab Manager, Manager, Sampling Coordinator |
| Cancel | `senaite.core: Transition: Cancel Analysis Request` | Lab Clerk, Lab Manager, Manager, Owner (client contact) |
| Reinstate | `senaite.core: Transition: Reinstate Analysis Request` | Lab Clerk, Lab Manager, Manager, Owner (client contact) |
| Reject | `senaite.core: Transition: Reject Sample` | Lab Clerk, Lab Manager, Manager |
| Retract | `senaite.core: Transition: Retract` | Lab Manager, Manager |
| Prepublish / Publish / Republish | `senaite.core: Transition: Publish Results` | Lab Manager, Manager, Publisher |
| Invalidate | `senaite.core: Transition: Invalidate` | Lab Manager, Manager |
| Dispatch | `senaite.core: Transition: Dispatch Sample` | Lab Manager, Manager |
| Restore | `senaite.core: Transition: Restore Sample` | Lab Manager, Manager |
| Create partitions | `senaite.core: Transition: Create Partitions` | Lab Clerk, Lab Manager, Manager |
| Detach partition | `senaite.core: Transition: Detach Sample Partition` | Lab Clerk, Lab Manager, Manager |

The sample-level *Submit* and *Verify* steps have no sample permission of
their own: they are promoted automatically from the analyses, so the
permissions that matter are the analysis-level submit/verify permissions
described in
[Results Entry and Verification](ResultsEntryAndVerification.md).

## Partitions, Secondary Samples and Retests

Several kinds of "derived" samples participate in the lifecycle. Each is a
full sample with its own state, but coordinated with its source:

- **Partitions** — aliquots of a received sample. Created while the sample
  is *Received*, *To be verified*, *Verified* or *Published*; received
  automatically; their submit/verify/publish/dispatch progress **rolls up**
  to the primary, and cancel/reject/reinstate **cascade down** from it. See
  [Sample Partitions](SamplePartitions.md).
- **Secondary samples** — additional requests registered against an existing
  physical sample. They skip the collection and reception steps when their
  primary has already been received. See
  [Secondary Samples](SecondarySamples.md).
- **Retests** — samples created automatically when a sample is
  [invalidated](#invalidating-a-sample-and-retests); they restart the
  results workflow for the same analyses.

## Frequently Asked Questions

**Why do my samples skip the "To be sampled" step?**
The sampling workflow is disabled in *Setup* (or in the sample template
used). Samples then go straight from *Registered* to *Sample due*.

**Why is the Receive action not offered for a sample?**
Either the sample has no *Date Sampled* set (required for reception), you
lack the `senaite.core: Transition: Receive Sample` permission, or the
sample is not in the *Sample due* state.

**Why don't I see the Schedule sampling action?**
*Enable Sampling Scheduling* must be ticked in *Setup* **and** the sampling
workflow must be active. The action is also limited to Sampling
Coordinators and Lab Managers by default.

**Can I cancel a sample once results have been entered?**
No. Cancel is only possible while all analyses are still unassigned. Once
work has started, retract the affected analyses, reject the sample, or —
after verification — invalidate it.

**What is the difference between Cancel, Reject, Retract and Invalidate?**
*Cancel* withdraws a request before work starts (no notification, can be
reinstated). *Reject* refuses the sample with recorded reasons and an
optional client notification. *Retract* sends submitted results back for
re-testing within the same sample. *Invalidate* voids verified/published
results and creates a whole new retest sample, optionally notifying the
client.

**Why can't I dispatch a sample?**
At least one of its analyses is assigned to a worksheet, or you lack the
`senaite.core: Transition: Dispatch Sample` permission (Lab Managers only by
default). Unassign the analyses from the worksheet and try again.

**Does a client contact see all these states?**
Client contacts see the states of their own samples in the client area, but
they can only *act* on a few of them — typically registering samples and
cancelling/reinstating samples that have not been received yet. See
[Clients and Contacts](ClientsAndContacts.md).

**A sample went back from "To be verified" to "Received" on its own. Why?**
One of its analyses was retracted or reassigned, so the system rolled the
sample back automatically to keep sample and analyses consistent. Once all
results are submitted again, it will return to *To be verified*.

**Why does the samples listing not show a "To be preserved" filter?**
The filter (like the *To be sampled*, *Scheduled sampling* and *Rejected*
ones) only appears when the corresponding feature is enabled in *Setup* —
in this case *Enable Sample Preservation*.
