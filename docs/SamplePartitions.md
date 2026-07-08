# Sample Partitions

Sample Partitions let you split a received sample into aliquots (sub-samples)
so that different portions of the same physical sample can be analysed
separately — for example in different departments, with different containers
or preservations, or with different sets of analyses.

This guide explains the feature from an end-user perspective: what partitions
are, how to create them, how they behave through the laboratory workflow, and
how to configure the system around them.

---

## Table of Contents

1. [What is a Partition?](#what-is-a-partition)
2. [Key Concepts](#key-concepts)
3. [Prerequisites and Permissions](#prerequisites-and-permissions)
4. [Creating Partitions Manually](#creating-partitions-manually)
5. [Creating Partitions from a Sample Template](#creating-partitions-from-a-sample-template)
6. [Partition IDs](#partition-ids)
7. [How Partitions Behave in the Workflow](#how-partitions-behave-in-the-workflow)
8. [Internal Use Partitions](#internal-use-partitions)
9. [Partition Visibility for Clients](#partition-visibility-for-clients)
10. [Detaching a Partition](#detaching-a-partition)
11. [Copying Samples that have Partitions](#copying-samples-that-have-partitions)
12. [Frequently Asked Questions](#frequently-asked-questions)
13. [Related Guides](#related-guides)

---

## What is a Partition?

When a laboratory receives a physical sample, it often needs to divide it
into several portions (aliquots). Each portion may:

- go to a different laboratory section or instrument,
- be stored in a different container,
- require a different preservation,
- carry a different subset of the requested analyses,
- or even be registered with a different sample type.

In SENAITE, each of these portions is a **Partition**: a full sample record
of its own, linked to the original sample (the **Primary Sample**). A
partition has its own ID, its own analyses, its own results and its own
workflow state — but the system keeps primary and partitions coordinated, so
the client still sees one coherent sample.

## Key Concepts

| Term | Meaning |
|------|---------|
| **Primary Sample** | The sample as it was registered and received. Once partitioned, it acts as the "parent" of its partitions. |
| **Partition** | A sub-sample created from a received primary sample. Behaves like a regular sample but stays linked to its primary. |
| **Detached Partition** | A partition that has been unlinked from its primary and now lives as an independent sample. A reference to the sample it was detached from is kept. |
| **Internal Use** | A flag on a sample or partition that hides it (and its analyses) from client contacts. |

A few structural rules:

- A partition always belongs to exactly one primary sample.
- **Partitions of partitions are not allowed.** The *Create Partitions*
  action is not available on a sample that is itself a partition.
- Both the primary sample and each partition show an informative banner at
  the top of their view: the primary lists links to all its partitions, and
  each partition links back to its primary.

## Prerequisites and Permissions

- Partitions can only be created from samples that have been **received**.
  The *Create Partitions* action is available while the sample is in the
  *Received*, *To be Verified*, *Verified* or *Published* state. Samples
  that are still due, or that are cancelled, rejected, invalid or
  dispatched, cannot be partitioned.
- The action is protected by the permission
  `senaite.core: Transition: Create Partitions`. By default this is granted
  to laboratory roles (e.g. Lab Manager, Lab Clerk, Analyst) according to
  your role/permission setup.
- Detaching a partition is protected by the permission
  `senaite.core: Transition: Detach Sample Partition`.

## Creating Partitions Manually

1. Go to the **Samples** listing (or a client's samples listing) and select
   one or more **received** samples using the checkboxes.
2. Click the **Create Partitions** button that appears at the bottom of the
   listing.
3. You are taken to the **Manage Partitions** form. For each selected
   sample the form shows a table where you define the partitions:

   - **Number of partitions** — add or remove partition rows as needed. If
     the sample was created from a template that defines partitions, the
     rows are pre-populated from the template (see next section).
   - **Sample Type** — optionally give the partition a different sample
     type. Defaults to the primary sample's type.
   - **Container** — the container the aliquot is stored in.
   - **Preservation** — the preservation applied to the aliquot.
   - **Internal Use** — tick to hide this partition from client contacts
     (see [Internal Use Partitions](#internal-use-partitions)).
   - **Analyses** — select which of the primary sample's analyses each
     partition should carry. Click on an analysis row to toggle its
     selection.

4. Press **Create Partitions**.

The system then:

- creates one new sample per partition row, copying the relevant details
  (client, contact, date sampled, specifications, etc.) from the primary
  sample;
- **automatically receives** each partition, using the primary sample's
  reception date — no separate *Receive* step is needed;
- **moves** the selected analyses from the primary sample into the
  partitions, provided they have not been assigned or submitted yet. The
  analyses keep the result ranges (specifications) of the primary sample;
- flags the analyses that remain on the primary sample but were moved to
  partitions for removal from the primary.

Notes:

- The same analysis/service can be assigned to more than one partition if
  needed.
- Partitions may also be created **without any analyses** — for example,
  aliquots stored for later or sent elsewhere. Analyses can be added to them
  afterwards like on any other sample.
- After creation you are returned to the samples listing with a
  confirmation message, and the partitions appear as regular samples with
  their own IDs.

## Creating Partitions from a Sample Template

Sample Templates (configured under *Setup*) can predefine a partition
scheme:

- Each template can define a list of partitions (`P1`, `P2`, …), each with
  its own **container** and **preservation**, and each analysis in the
  template can be assigned to one of these partitions.
- When you open the *Manage Partitions* form for a sample created from such
  a template, the partition rows and the analysis assignment are
  **pre-populated** from the template — you normally just review and press
  *Create Partitions*.

### Automatic partitioning on reception

Templates also have an **Auto-partition on receive** option. When enabled:

- As soon as a sample created from that template is **received**, the lab
  user is automatically redirected to the *Manage Partitions* form with the
  template's partition scheme pre-loaded.

This ensures samples that always require splitting (e.g. water samples with
microbiology and chemistry portions) are partitioned consistently right at
reception.

## Partition IDs

Partitions get their own IDs, generated from a dedicated entry in the ID
Server configuration (*Setup → ID Server*, type **AnalysisRequestPartition**).

The ID format has access to these variables, among others:

- `parent_ar_id` — the full ID of the primary sample (e.g. `WATER-0001`),
- `parent_base_id` — the primary ID without any suffix,
- `partition_count` — the sequential number of the partition within its
  primary.

A typical format is `{parent_ar_id}-P{partition_count:01d}`, which produces
IDs like:

```
WATER-0001        ← primary sample
WATER-0001-P1     ← first partition
WATER-0001-P2     ← second partition
```

This makes it easy to recognise partitions and trace them back to their
primary sample at a glance.

## How Partitions Behave in the Workflow

Partitions are full samples, so results are entered, submitted and verified
on each partition just like on any other sample. However, primary and
partitions are kept **in sync** automatically:

**From partition to primary (roll-up):**

- When all partitions of a primary have their results **submitted**, the
  primary sample moves to *To be Verified* as well.
- When all partitions are **verified**, the primary becomes *Verified*.
- When all partitions are **published**, the primary is *Published*.
- When all (relevant) partitions are **dispatched**, the primary is
  dispatched too; restoring a partition restores the primary accordingly.

**From primary to partitions (cascade):**

- **Cancelling** a primary sample cancels its partitions (and a primary can
  only be cancelled if all its partitions can be cancelled too).
- **Rejecting** or **invalidating/retracting** propagates to the partitions
  as appropriate.
- **Reinstating** a cancelled partition is only possible if its primary
  sample is not cancelled (or is reinstated as well).

Analyses in *cancelled*, *rejected* or *retracted* states are ignored when
the system evaluates whether a sample or partition can progress, so a single
discarded analysis never blocks the rest of the work.

## Internal Use Partitions

When creating a partition you can mark it for **Internal Use**. Internal-use
partitions (and their analyses):

- are visible to laboratory personnel as normal,
- are **hidden from client contacts** — they do not appear in the client's
  sample listings, nor in the partitions banner of the primary sample, and
  their results are not included in client-facing reports.

This is useful for QC aliquots, retained/archived portions, or any portion
of the sample the client should not see. In the *Manage Partitions* form
the checkbox comes pre-ticked when the primary sample is itself flagged for
internal use, or when the sample template marks that partition as internal
use; otherwise it is unticked by default.

## Partition Visibility for Clients

Laboratory users always see partitions. Whether **client contacts** see
partitions is controlled globally:

- Go to *Setup* and enable/disable **Show Partitions** (registry setting
  `show_partitions`).
- When disabled, clients only see the primary samples; results from
  partitions are still consolidated at the primary level, so from the
  client's perspective nothing is missing.
- When enabled, clients also see the partition samples themselves (except
  those flagged for internal use).

## Detaching a Partition

Sometimes a partition needs to continue its life independently of the
original sample — for instance if it is re-routed as a stand-alone job.

- Open the partition and use the **Detach** action (available only on
  partitions, and only to users with the
  `senaite.core: Transition: Detach Sample Partition` permission).
- The partition is unlinked from its primary and from then on behaves like a
  regular, independent sample. Its workflow state does not change when it
  is detached.
- The sample keeps a permanent **"Detached from"** reference, shown in a
  banner at the top of the sample view, so traceability to the original
  primary sample is preserved.
- The primary sample no longer follows the detached sample's transitions
  (its submit/verify/publish roll-up ignores it).

Detaching cannot be undone from the user interface — the detached sample
does not become a partition again.

## Copying Samples that have Partitions

Two registry settings (*Setup → Registry*, fieldset *Samples*) control what
happens when a sample with partitions is used as the source of a **Copy**
in the Add Samples form:

| Setting | Default | Effect |
|---------|---------|--------|
| `sample_add_form_copy_partitions` | off | When enabled, copying a sample also recreates its partition structure (same sample types, containers, preservations, internal-use flags and analysis assignment). |
| `sample_add_form_skip_partition_analyses` | off | When enabled, analyses that live in partitions are excluded when copying a sample. |

## Frequently Asked Questions

**Can I create a partition of a partition?**
No. Partitions can only be created from primary samples. Detach the
partition first if you truly need to sub-divide it — once detached it acts
as a primary sample.

**Do I need to receive partitions?**
No. Partitions are automatically set to *Received*, inheriting the
reception date of their primary sample.

**Can a partition have analyses that the primary never had?**
Yes. A partition is a full sample — you can add analyses to it at any time,
just like on any other sample.

**What happens to an analysis when it is moved into a partition?**
If the analysis on the primary is still unassigned (no worksheet, no
result), it is physically moved into the partition, keeping its
specifications. Analyses that were already assigned or submitted stay where
they are.

**Why can't my client see the partitions?**
Either the global **Show Partitions** setting is disabled, or the partition
is flagged for **Internal Use**. Both are intentional visibility controls.

**Can I cancel a primary without cancelling its partitions?**
No. Cancelling a primary sample requires that all of its partitions can be
cancelled, and the cancellation cascades to them.

## Related Guides

- [Secondary Samples](SecondarySamples.md) — a different way to derive a
  sample from an existing one: a new sample sharing the primary's physical
  material, rather than a split of it.
- [Sample Lifecycle & Workflow States](SampleWorkflow.md) — the full sample
  workflow that primaries and partitions move through.
- [Registering Samples](SampleRegistration.md) — the Add Samples form,
  including copying samples that have partitions.
- [Analysis Services, Profiles & Sample Templates](AnalysesSetup.md) —
  configuring the sample templates that drive automatic partitioning.
- [Sample Rejection](SampleRejection.md) — how rejection cascades between
  primaries and partitions.
- Back to the [User Guide index](UserGuide.md).
