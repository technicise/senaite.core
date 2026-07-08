# Secondary Samples

Secondary Samples let you register a new sample against a sample that already
exists in the laboratory — for example when a client requests additional
testing on material that was already delivered and received. The new
(secondary) sample shares the physical sample of the original (primary)
sample, so nothing new arrives at the lab: the secondary is received
automatically and inherits the key details of its primary.

This guide explains the feature from an end-user perspective: what secondary
samples are, how to create them, which fields they inherit, how their IDs are
built, and how they differ from [Sample Partitions](SamplePartitions.md).

---

## Table of Contents

1. [What is a Secondary Sample?](#what-is-a-secondary-sample)
2. [Key Concepts](#key-concepts)
3. [Secondary Samples vs Partitions](#secondary-samples-vs-partitions)
4. [Prerequisites and Permissions](#prerequisites-and-permissions)
5. [Creating a Secondary Sample](#creating-a-secondary-sample)
6. [Fields Inherited from the Primary Sample](#fields-inherited-from-the-primary-sample)
7. [Automatic Reception](#automatic-reception)
8. [Secondary Sample IDs](#secondary-sample-ids)
9. [Dates Kept in Sync with the Primary](#dates-kept-in-sync-with-the-primary)
10. [Banners Linking Primary and Secondary Samples](#banners-linking-primary-and-secondary-samples)
11. [Secondary Samples and Partitions Combined](#secondary-samples-and-partitions-combined)
12. [Frequently Asked Questions](#frequently-asked-questions)

---

## What is a Secondary Sample?

Sometimes work has to be registered against sample material the laboratory
already holds:

- the client orders **additional analyses** on a sample after it was
  registered and received,
- a **new, separate job** (with its own report and invoice) must be run on
  the same physical material,
- retained/stored material is re-used for **follow-up testing**.

In SENAITE, each of these new registrations is a **Secondary Sample**: a full
sample record of its own, created against an existing sample (its **Primary
Sample**). Because the secondary shares the physical sample of its primary —
no new material is delivered to the lab — it is **received automatically**
upon creation, with the same reception date as its primary, and it inherits
the primary's sampling details.

Apart from that, a secondary sample behaves like any regular sample: it has
its own ID, its own analyses, its own [workflow](SampleWorkflow.md), its own
[results entry and verification](ResultsEntryAndVerification.md) and its own
[results report](ResultsPublication.md).

## Key Concepts

| Term | Meaning |
|------|---------|
| **Primary Sample** | An existing, received sample that is used as the source of a secondary sample. Any active, received sample can act as a primary. |
| **Secondary Sample** | A new sample registered against a primary sample. It shares the primary's physical sample material and inherits its sampling and reception dates. |
| **Partition** | A sub-sample (aliquot) split off a received sample. A different concept — see [Secondary Samples vs Partitions](#secondary-samples-vs-partitions). |

A few structural rules:

- A secondary sample always refers to exactly one primary sample. The
  reference is permanent and shown in the sample's header and banner.
- A primary sample can have **any number** of secondary samples.
- Secondary samples of secondary samples **are allowed** — the new secondary
  simply refers to the previous one as its primary.
- The workflows of primary and secondary samples are **independent**: only
  the sampling and reception dates are kept in sync (see
  [Dates Kept in Sync with the Primary](#dates-kept-in-sync-with-the-primary)).

## Secondary Samples vs Partitions

Secondary samples and [partitions](SamplePartitions.md) both link a new
sample record to an existing one, but they serve different purposes:

| Aspect | Partition | Secondary Sample |
|--------|-----------|------------------|
| Typical use | Split one received sample into aliquots so its analyses can be distributed (departments, containers, preservations) | Register a new, additional request against sample material the lab already holds |
| Created from | Samples listing → **Create Partitions** action | **Add Samples** form → **Primary Sample** field |
| Analyses | Selected analyses are **moved out** of the primary into the partitions | Analyses are **chosen fresh** for the secondary; the primary's analyses are not touched |
| Workflow | Coordinated: partitions roll up to the primary (submit/verify/publish) and primary transitions cascade down (cancel/reject) | Independent: primary and secondary progress, get verified and are published separately |
| Dates | Reception date inherited on creation | Sampling and reception dates inherited **and kept in sync** with the primary |
| Default ID suffix | `-P01`, `-P02`, … | `-S01`, `-S02`, … |
| Nesting | Partitions of partitions are **not** allowed | Secondaries of secondaries **are** allowed |
| Client visibility | Controlled by the **Show Partitions** setting and the **Internal Use** flag | Visible like any regular sample |

Rule of thumb: use **partitions** to organise the work of *one* request
across several aliquots; use a **secondary sample** when a *new* request
must be fulfilled with sample material that is already in the lab.

## Prerequisites and Permissions

- Secondary samples are created through the regular **Add Samples** form, so
  you need the permission to register samples
  (`senaite.core: Add AnalysisRequest`). See
  [Sample Registration](SampleRegistration.md) for the form in general.
- The **Primary Sample** field is protected by the permission
  `senaite.core: Field: Edit Client`. By default, both laboratory personnel
  and client contacts who may register samples can use it.
- Only samples that are **active** and already **received** can be selected
  as the primary. Samples that are still due, or that are cancelled or
  otherwise inactive, are not offered in the search.
- When a **client** is already selected in the form (or you register from
  within a client), the Primary Sample search only returns samples of that
  client — see [Clients and Contacts](ClientsAndContacts.md).

## Creating a Secondary Sample

1. Open the **Add Samples** form — from the **Samples** listing, from a
   client, or from a [batch](Batches.md) — as described in
   [Sample Registration](SampleRegistration.md).
2. At the top of the sample column, use the **Primary Sample** field
   ("Select a sample to create a secondary Sample"). Search by sample ID or
   any other indexed text; the search dialog shows the **Sample ID**,
   **Client SID**, **Sample Type**, **Name** and **Client ID** columns to
   help you pick the right sample.
3. As soon as a primary sample is selected, the form **auto-fills** the
   fields listed in the
   [next section](#fields-inherited-from-the-primary-sample) with the
   primary's values, and locks those that must stay identical to the
   primary.
4. Select the **analyses** (services and/or profiles) for the secondary
   sample, exactly as for a regular sample — see
   [Analyses Setup](AnalysesSetup.md). The analyses of the primary are *not*
   copied over; you choose freely what the new request should include.
5. Press **Save**.

The system then creates the secondary sample and — provided the primary has
been received — sets it directly to **Received** with the primary's
reception date, with its analyses ready to be assigned to
[worksheets](Worksheets.md).

Notes:

- Each column of the Add Samples form has its own Primary Sample field, so
  you can register secondaries for **different primaries in one go**.
- A primary sample can be the source of **many** secondary samples — simply
  repeat the procedure whenever a new request comes in.

## Fields Inherited from the Primary Sample

When a primary sample is selected in the Add Samples form, the following
fields are **auto-filled** from the primary:

- **Client**, **Contact**, **CC Contacts** and **CC Emails**
- **Batch**
- **Date Sampled** and **Expected Sampling Date**
- **Sample Type**
- **Sample Point**, **Sample Condition**, **Storage Location**,
  **Container** and **Sampling Deviation**
- **Client Sample ID**, **Client Reference** and **Client Order Number**
- **Environmental Conditions**
- **Composite**

Most of these are **locked** (shown greyed-out) once the primary is
selected, because they describe the shared physical sample and must match
the primary: Date Sampled, Expected Sampling Date, Sample Type, Sample
Point, Sample Condition, Storage Location, Sampling Deviation, Client Sample
ID, Client Reference, Client Order Number and Composite. The **Sample
Template**, **Publication Specification** and **Sample Rejection** fields
are disabled as well, and the *Number of samples* option is hidden — one
secondary is created per column.

After creation, the **Date Sampled**, **Expected Sampling Date** and **Date
Received** fields remain read-only on the secondary sample: they are managed
by the primary (see
[Dates Kept in Sync with the Primary](#dates-kept-in-sync-with-the-primary)).

## Automatic Reception

Because the physical material is already in the laboratory, a secondary
sample skips the reception step of the regular
[sample workflow](SampleWorkflow.md):

- On creation, the secondary sample is transitioned **directly to
  *Received***, provided its primary sample has been received (which is
  always the case when the primary was picked in the Add Samples form, since
  only received samples are offered there).
- The secondary's **Date Received** is set to the **primary's reception
  date** — not to the moment the secondary was registered.
- The secondary's analyses are **initialized immediately**, so they show up
  as unassigned and can be placed on [worksheets](Worksheets.md) right away,
  with [instruments](Instruments.md) and
  [QC controls](QualityControl.md) used as for any other analysis.

No manual *Receive* action is needed for secondary samples. This mirrors the
behaviour of [partitions](SamplePartitions.md#how-partitions-behave-in-the-workflow),
which are also received automatically.

## Secondary Sample IDs

Secondary samples get their own IDs, generated from a dedicated entry in the
ID Server configuration (*Setup → ID Server*, type
**AnalysisRequestSecondary**).

The ID format has access to these variables, among others:

- `parent_ar_id` — the full ID of the primary sample (e.g. `W-0001`),
- `parent_base_id` — the primary ID without any suffix,
- `secondary_count` — the sequential number of the secondary within its
  primary.

The default format is `{parent_ar_id}-S{secondary_count:02d}`, which
produces IDs like:

```
W-0001            ← primary sample
W-0001-S01        ← first secondary sample
W-0001-S02        ← second secondary sample
W-0001-S02-S01    ← secondary created from W-0001-S02
```

Because the full primary ID is used as the base, a secondary of a secondary
simply extends the chain, so the whole ancestry stays readable at a glance —
compare with [partition IDs](SamplePartitions.md#partition-ids), which use a
`-P` suffix in the same fashion.

## Dates Kept in Sync with the Primary

Since primary and secondary samples share the same physical material, the
dates that describe that material are managed centrally on the **primary**
sample and propagated to all its secondaries (and to their secondaries, all
the way down the chain):

| Date | Behaviour |
|------|-----------|
| **Date Sampled** | Inherited on creation; whenever it changes on the primary, all secondaries are updated. Not editable on the secondary. |
| **Expected Sampling Date** | Inherited on creation; changes on the primary propagate to all secondaries. Not editable on the secondary. |
| **Date Received** | Set to the primary's reception date on creation; changes on the primary propagate to all secondaries. Not editable on the secondary. |

Everything else — analyses, results, remarks, attachments, workflow state —
is fully independent between primary and secondary samples.

## Banners Linking Primary and Secondary Samples

Every secondary sample displays a dismissible informative banner at the top
of its view:

> **Info** — This is a Secondary Sample of *W-0001*

The sample ID in the banner is a link that takes you straight to the primary
sample. In addition, the **Primary Sample** field is shown prominently
(read-only) in the secondary sample's header information.

Note that, unlike with [partitions](SamplePartitions.md) (where the primary
shows a banner listing all its partitions), the primary sample does **not**
display a banner enumerating its secondaries — use the samples listing or
the search to find the secondaries of a given sample (their IDs start with
the primary's ID by default).

## Secondary Samples and Partitions Combined

Secondary samples and [partitions](SamplePartitions.md) can be freely
combined, and the system keeps the two concepts apart:

- **Partitions of a secondary sample** are regular partitions — they belong
  to the secondary (their "primary" in partition terms), not to the original
  primary sample, and they carry no secondary reference of their own. With
  the default formats their IDs read e.g. `W-0001-S03-P01`.
- **A secondary sample can be created from a partition** by selecting the
  partition as the Primary Sample in the Add Samples form. The result is a
  secondary sample (not a partition), with the partition as its primary,
  e.g. `W-0001-S03-P02-S01`.
- Creating a secondary from a partition is also the practical answer to the
  "partition of a partition" limitation: partitions cannot be partitioned,
  but new work can always be registered against them as a secondary.

## Frequently Asked Questions

**Can I create a secondary sample of a secondary sample?**
Yes. The new sample refers to the previous secondary as its primary, and its
ID extends the chain (e.g. `W-0001-S02-S01`).

**Why can't I find my sample in the Primary Sample search?**
Only samples that are active and already **received** can act as primaries.
Also, when a client is selected in the form, only that client's samples are
offered.

**Do I need to receive a secondary sample?**
No. A secondary sample is automatically set to *Received*, inheriting the
reception date of its primary sample.

**Do results roll up from the secondary to the primary (or vice versa)?**
No. Unlike [partitions](SamplePartitions.md), primary and secondary samples
progress through the [workflow](SampleWorkflow.md) independently: each is
submitted, verified and [published](ResultsPublication.md) on its own.

**Can the secondary sample have different analyses than the primary?**
Yes. You select the analyses for the secondary freely when registering it;
nothing is copied from — or removed from — the primary.

**Can I change the Date Sampled or Date Received of a secondary sample?**
No. These dates are managed on the primary sample; changing them there
updates all its secondaries automatically.

**Can a secondary sample be rejected or cancelled on its own?**
Yes. Its workflow is independent of the primary's, so it can be
[rejected](SampleRejection.md), cancelled or invalidated without affecting
the primary. Note, however, that the rejection option is disabled in the Add
Samples form while a primary sample is selected.

**I copied a secondary sample in the Add Samples form — why is the new one
not a secondary?**
The Primary Sample reference is deliberately not carried over when copying a
sample in the Add form. Select the primary sample again in the new column if
the copy should be a secondary too.
