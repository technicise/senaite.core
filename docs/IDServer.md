# IDs & Numbering (the ID Server)

Every object that matters in the laboratory — samples, partitions, retests,
secondary samples, worksheets, batches, reference samples, duplicates,
invoices — receives a unique, human-readable ID the moment it is created.
These IDs are produced by the **ID Server**, which builds each ID from a
configurable format string and a set of persistent counters.

This guide explains the feature from an end-user perspective: how IDs are
composed, which variables are available per content type, how the sequence
counters work behind the scenes, how to change formats safely, and how to
seed or reset the numbering.

---

## Table of Contents

1. [What is the ID Server?](#what-is-the-id-server)
2. [Key Concepts](#key-concepts)
3. [The Configuration Screen](#the-configuration-screen)
4. [Format Strings and Variables](#format-strings-and-variables)
5. [Default Formats per Content Type](#default-formats-per-content-type)
6. [How Counters Work](#how-counters-work)
7. [Alphanumeric Sequences](#alphanumeric-sequences)
8. [IDs for Partitions, Retests and Secondary Samples](#ids-for-partitions-retests-and-secondary-samples)
9. [Changing an ID Format](#changing-an-id-format)
10. [Viewing, Seeding and Resetting Counters](#viewing-seeding-and-resetting-counters)
11. [Practical Examples](#practical-examples)
12. [Frequently Asked Questions](#frequently-asked-questions)
13. [Related Guides](#related-guides)

---

## What is the ID Server?

When a sample is registered, a worksheet opened or a batch created, SENAITE
does not use random identifiers. Instead it consults the ID Server
configuration, looks up the entry for that content type, fills a **format
string** with values from the object (client, sample type, date, …), asks a
**number generator** for the next sequence number, and renames the object to
the resulting ID.

The result is IDs like:

```
WATER-0001            ← sample of sample type "WATER"
WATER-0001-P01        ← first partition of that sample
WATER-0001-R01        ← first retest of that sample
WATER-0001-S01        ← first secondary sample derived from it
WS-001                ← worksheet
B-001                 ← batch
QC-001                ← reference (QC) sample
```

IDs are generated **once**, at creation time, and never change afterwards —
they appear on labels, reports and invoices, so they are permanent by design.

## Key Concepts

| Term | Meaning |
|------|---------|
| **Format** (form) | A template for the ID, written with placeholders in curly braces, e.g. `{sampleType}-{seq:04d}`. Each placeholder is replaced with a real value when the ID is generated. |
| **Variable** | A placeholder available inside the format, e.g. `{seq}`, `{year}`, `{clientId}`. Which variables exist depends on the content type. |
| **Sequence number** (`{seq}`) | The incrementing part of the ID. It should always be the *last* element of the format. |
| **Number generator** | The internal storage that remembers the last number issued for each counter key, so numbering survives restarts and never walks backwards. |
| **Sequence Type** | How the number is obtained: *Generated* (from the number generator), *Counter* (by counting existing related objects), or empty (no sequence at all — the ID is derived purely from variables). |
| **Prefix** | An internal, lowercase identifier for the entry (e.g. `worksheet`). It does not appear in the IDs themselves. |
| **Split Length** | How many dash-separated segments of the format (from the left) form the *counter key*. This decides whether you get one global counter or separate counters per sample type, client, year, etc. |

## The Configuration Screen

The configuration lives in *Setup → ID Server* and consists of two parts:

1. **Formatting Configuration** — a table with one row per content type.
   Each row has these columns:

   | Column | Purpose |
   |--------|---------|
   | **Portal Type** | The content type this row applies to, e.g. `AnalysisRequest` (samples), `Worksheet`, `Batch`. |
   | **Format** | The ID template, e.g. `WS-{seq:03d}`. |
   | **Seq Type** | *Generated*, *Counter*, or empty (see above). Almost all rows use *Generated*. |
   | **Context** | Only for *Counter*: the object whose related items are counted (e.g. `parent`). |
   | **Counter Type** | Only for *Counter*: `contained` counts objects contained in the context. (`backreference` is obsolete.) |
   | **Counter Ref** | Only for *Counter*: which kind of contained objects to count. |
   | **Prefix** | Internal identifier of the entry; keep it a single lowercase word. |
   | **Split Length** | Number of format segments included in the counter key (default `1`). |

   Rows can be added, edited, reordered and deleted. If a content type has
   **no row at all**, it falls back to a generic format: the type name in
   lowercase followed by `-` and the sequence number (e.g.
   `samplematrix-1`).

2. **ID Server Values** — a read-only box listing all current counter keys
   and the last number issued for each one, e.g.
   `analysisrequest-WATER: 42`. This is the live state of the number
   generator (see [How Counters Work](#how-counters-work)).

Only users with site-management privileges (typically Lab Managers /
administrators) can edit the setup.

## Format Strings and Variables

Formats use the same syntax as Python's string formatting:

- Anything outside curly braces is literal text: `WS-` in `WS-{seq:03d}`.
- `{seq}` inserts the sequence number; `{seq:03d}` pads it to three digits
  (`001`, `002`, … `999`). Use `04d` for four digits, and so on.
- `{alpha:2a3d}` inserts an *alphanumeric* sequence instead — see
  [Alphanumeric Sequences](#alphanumeric-sequences).
- Date variables accept date formatting, e.g. `{dateSampled:%Y%m%d}`
  becomes `20260131`.
- The sequence variable (`{seq}` or `{alpha}`) must be the **last** element
  of the format.

### Variables available to all content types

| Variable | Value | Example |
|----------|-------|---------|
| `{seq}` | Next number in the sequence | `7`, or `007` with `{seq:03d}` |
| `{alpha}` | Next number as an alphanumeric code (requires a format, e.g. `{alpha:2a3d}`) | `AA001` |
| `{year}` | Current year, **two digits** | `26` |
| `{yymmdd}` | Current date | `260706` |
| `{portal_type}` | The content type name | `Worksheet` |

### Additional variables for samples (`AnalysisRequest` and its sub-types)

| Variable | Value | Example |
|----------|-------|---------|
| `{clientId}` | The Client ID of the sample's client | `HH` |
| `{sampleType}` | The **Prefix** of the sample's sample type (not its title) | `WATER` |
| `{dateSampled}` | Date the sample was taken (falls back to now) | `{dateSampled:%Y%m%d}` → `20260706` |
| `{samplingDate}` | Expected sampling date (falls back to now) | `{samplingDate:%y%m}` → `2607` |
| `{test_count}` | 1 for the original sample, incremented on each retest (for suffix-style IDs) | `1` |

### Additional variables per sample sub-type

| Sub-type | Variables | Meaning |
|----------|-----------|---------|
| **Partition** | `{parent_ar_id}` | Full ID of the primary sample (`WATER-0001`) |
| | `{parent_base_id}` | Primary ID with any `-R01`/`-P01`-style suffix stripped |
| | `{partition_count}` | Ordinal number of the new partition within its primary (1, 2, 3, …) |
| **Retest** | `{parent_ar_id}` | Full ID of the invalidated sample |
| | `{parent_base_id}` | Invalidated ID without suffix (the full ID is kept if the retested sample is a partition) |
| | `{retest_count}` | How many times the sample has been retested |
| | `{test_count}` | `{retest_count}` + 1 — handy for old-style `-R01`, `-R02` suffixes |
| **Secondary** | `{parent_ar_id}` | Full ID of the primary sample |
| | `{parent_base_id}` | Primary ID without suffix |
| | `{secondary_count}` | Ordinal number of the secondary sample |

Results reports also expose `{clientId}`, so a row for the report type can
produce client-scoped report IDs.

**Important:** using a variable that does not exist for that content type
makes ID generation — and therefore object creation — fail with an error.
Stick to the variables listed for the type you are configuring.

## Default Formats per Content Type

A fresh SENAITE installation ships with these entries:

| Portal Type | Default Format | Seq Type | Split Length | Example IDs |
|-------------|----------------|----------|--------------|-------------|
| `AnalysisRequest` (samples) | `{sampleType}-{seq:04d}` | Generated | 1 | `WATER-0001`, `WATER-0002` |
| `AnalysisRequestPartition` | `{parent_ar_id}-P{partition_count:02d}` | *(empty)* | 1 | `WATER-0001-P01` |
| `AnalysisRequestRetest` | `{parent_base_id}-R{retest_count:02d}` | *(empty)* | 1 | `WATER-0001-R01` |
| `AnalysisRequestSecondary` | `{parent_ar_id}-S{secondary_count:02d}` | *(empty)* | 1 | `WATER-0001-S01` |
| `Worksheet` | `WS-{seq:03d}` | Generated | 1 | `WS-001` |
| `Batch` | `B-{seq:03d}` | Generated | 1 | `B-001` |
| `ReferenceSample` (QC samples) | `QC-{seq:03d}` | Generated | 1 | `QC-001` |
| `ReferenceAnalysis` (blank/control analyses) | `SA-{seq:03d}` | Generated | 1 | `SA-001` |
| `DuplicateAnalysis` | `D-{seq:03d}` | Generated | 1 | `D-001` |
| `Invoice` | `I-{seq:03d}` | Generated | 1 | `I-001` |

Note that the three sample sub-types (partition, retest, secondary) have an
**empty** sequence type: their numbering does not come from a counter but
from the `{partition_count}` / `{retest_count}` / `{secondary_count}`
variables, which are computed by looking at the parent sample.

## How Counters Work

For entries with sequence type *Generated*, the number generator keeps one
persistent counter per **counter key**. The key is built from two parts:

```
<portal type in lowercase>-<resolved prefix of the format>
```

The "resolved prefix" is the format cut after **Split Length**
dash-separated segments, with any variables in that part replaced by their
actual values. This is what makes counters global or scoped:

| Format | Split Length | Counter key(s) | Effect |
|--------|--------------|----------------|--------|
| `WS-{seq:03d}` | 1 | `worksheet-WS` | One global worksheet counter. |
| `{sampleType}-{seq:04d}` | 1 | `analysisrequest-WATER`, `analysisrequest-SOIL`, … | **A separate counter per sample type** — this is why `WATER-0003` and `SOIL-0003` can coexist. |
| `{sampleType}-{year}-{seq:04d}` | 2 | `analysisrequest-WATER-26`, `analysisrequest-WATER-27`, … | A counter per sample type **and** year — numbering restarts at `0001` automatically every January 1st. |
| `{clientId}-{sampleType}-{seq:03d}` | 2 | `analysisrequest-HH-WATER`, … | A counter per client and sample type. |

Further points worth knowing:

- Sequences start at **1**: the first ID generated for a new key is
  `...-0001` (or `...-001`, depending on the padding).
- The counter value stored is the **last number issued**; the next ID uses
  that value plus one.
- Counters only ever move forward. Deleting or cancelling objects does
  **not** free their numbers for reuse.
- When the sequence exceeds its padding, it simply grows longer: after
  `WS-999` (with `{seq:03d}`) comes `WS-1000` — nothing breaks.
- Generated IDs are normalised for safe use in URLs and barcodes: accented
  or special characters coming from variables are converted to plain ASCII.

The alternative sequence type *Counter* does not use the number generator at
all: it derives the number from how many related objects already exist (for
example, how many items are contained in a given parent). It is rarely
needed nowadays — the computed variables of partitions, retests and
secondaries cover the common cases.

## Alphanumeric Sequences

If plain numbers grow too long, the `{alpha}` variable produces compact
letter+digit sequences. The format `{alpha:2a3d}` means *2 alphabetic
characters followed by 3 digits*:

```
AA001, AA002, … AA999, AB001, AB002, … AZ999, BA001, …
```

- The digit block runs from `001` to `999` (it is never all zeros), then
  the letter block advances.
- Behind the scenes an alphanumeric sequence is still a single number in
  the number generator, so seeding and continuity work exactly the same.
- Choose the size generously: once all letter combinations are used up
  (e.g. after `ZZ999` for `2a3d`), no further IDs can be generated for that
  key and creation fails. `3a3d` gives you over 17 million IDs.

Example: setting the worksheet format to `WS-{alpha:2a3d}` produces
`WS-AA001`, `WS-AA002`, and so on.

## IDs for Partitions, Retests and Secondary Samples

The three sample sub-types are recognised automatically — you do not choose
the entry, the system does, based on how the sample came into existence:

- A sample created via *Create Partitions* uses the
  `AnalysisRequestPartition` entry.
- A sample created by invalidating another one uses the
  `AnalysisRequestRetest` entry.
- A sample registered against an existing sample (same client sample ID)
  uses the `AnalysisRequestSecondary` entry.

With the default formats, one primary sample and its derivatives look like:

```
WATER-0001            ← primary
WATER-0001-P01        ← partition 1
WATER-0001-P02        ← partition 2
WATER-0001-R01        ← retest (after invalidation)
WATER-0001-S01        ← secondary sample
```

### Old-style suffix IDs

Laboratories coming from very old versions, where every sample ID ended in
`-R01` and a retest replaced the suffix with `-R02`, can reproduce that
scheme:

| Portal Type | Format | Split Length |
|-------------|--------|--------------|
| `AnalysisRequest` | `{sampleType}-{year}-{seq:04d}-R01` | 2 |
| `AnalysisRequestRetest` | `{parent_base_id}-R{test_count:02d}` | 1 |

This yields `WATER-26-0001-R01` for the original sample, `WATER-26-0001-R02`
for its first retest and `WATER-26-0001-R03` for a retest of the retest —
the base part stays constant while only the suffix increments.

## Changing an ID Format

You can edit the formats at any time, but keep these rules in mind:

1. **Existing IDs never change.** A new format only affects objects created
   after the change.
2. **Counters are keyed by the format's prefix part.** If your change alters
   the static part of the format (e.g. `WS-` becomes `W-`), a *new* counter
   key is created and numbering starts again at 1 — which is fine, because
   the IDs themselves are different and cannot collide.
3. **Counters are remembered.** If you change a format and later change it
   back, the old counter key is still in storage and numbering continues
   where it left off — even if you only removed a separator (e.g.
   `{sampleType}-{alpha:3a1d}` and `{sampleType}{alpha:3a1d}` share the same
   counter).
4. **Avoid re-creating an old scheme with fresh counters.** If a format
   change could produce IDs that already exist (for example after manually
   resetting a counter), creation of the conflicting object fails with an
   error — SENAITE never silently overwrites an ID. Check the *ID Server
   Values* box before and after a change.
5. Only use variables that exist for the content type of the row, and keep
   `{seq}` / `{alpha}` at the end of the format.

## Viewing, Seeding and Resetting Counters

Two tools are available:

- **Setup → ID Server → ID Server Values** shows every counter key and its
  current value, read-only.
- The **Manage Numbergenerator** page — reachable by appending `/@@ng` to
  the site address (administrator permission required) — lists every
  counter with an editable number field and the ID template it belongs to.

To seed a counter on the Manage Numbergenerator page:

1. Locate the key you want to change (e.g. `batch-B`).
2. Enter the new value. The value is the *last used* number, so seeding
   `batch-B` to `100` makes the next batch `B-101`.
3. Press **Seed**.
4. Entering `0` removes the key from storage entirely; it will be recreated
   from 1 the next time an ID is needed for it.

Seeding is typically used when migrating from another system ("continue
from 4500") or to align numbering after a data import. **Never seed a
counter backwards** below the highest number already in use for that key:
the next creation attempt would try to reuse an existing ID and fail.

## Practical Examples

**Per-sample-type numbering (default):**
`{sampleType}-{seq:04d}`, Split Length 1 → `WATER-0001`, `SOIL-0001`,
`WATER-0002`, …

**Yearly restart:**
`{sampleType}-{year}-{seq:04d}`, Split Length 2 → `WATER-26-0001` …
`WATER-26-2384`, and on the first sample of the next year: `WATER-27-0001`.

**Client- and date-coded sample IDs:**
`{clientId}-{dateSampled:%Y%m%d}-{sampleType}-{seq:04d}`, Split Length 1 →
`HH-20260706-WATER-0001`. (With Split Length 1 the counter is shared per
client; raise the split length to scope it further.)

**Compact alphanumeric sample IDs:**
`{sampleType}-{alpha:3a1d}`, Split Length 1 → `WATER-AAA1` … `WATER-AAA9`,
`WATER-AAB1`, …

**Worksheets per year:**
`WS{year}-{seq:03d}`, Split Length 1 → `WS26-001`; the counter key includes
the year (`worksheet-WS26`), so worksheet numbering restarts each year.

**Partitions numbered without leading zeros:**
`{parent_ar_id}-P{partition_count:01d}` → `WATER-0001-P1`, `WATER-0001-P2`.

## Frequently Asked Questions

**Can I change the ID of an existing sample or worksheet?**
No. IDs are assigned once at creation and are permanent. Format changes
only apply to objects created afterwards.

**Why do my sample IDs start with WATER, SOIL, etc.?**
The default sample format uses `{sampleType}`, which inserts the **Prefix**
defined on each Sample Type (*Setup → Sample Types*), not the sample type's
title.

**Why did numbering restart at 0001 after I edited the format?**
Counters are stored per prefix. Changing the static part of the format (or
the value of a variable inside the prefix part, such as `{year}` at the
turn of the year) creates a new counter key, which starts at 1. The old
counter is kept and will resume if you revert the format.

**Are numbers of deleted or cancelled objects reused?**
No. Counters only move forward, so gaps in the numbering are normal and, in
regulated environments, desirable.

**Sample creation suddenly fails with an error after I edited the ID
formats. Why?**
Most likely the format references a variable that does not exist for that
content type (a typo, or e.g. `{partition_count}` on the plain sample
entry), or a re-seeded counter is producing an ID that already exists. Fix
the format or seed the counter above the highest existing number.

**Do I need a `{seq}` in the partition/retest/secondary formats?**
No. Those entries usually have an empty sequence type and use the computed
variables `{partition_count}`, `{retest_count}` and `{secondary_count}`
instead, which count within the parent sample.

**What happens when the sequence outgrows its padding, e.g. after
`WS-999`?**
Nothing breaks — the ID simply becomes one digit longer (`WS-1000`).
Alphanumeric sequences, however, are finite: size them generously.

**Who can change the ID Server configuration or seed counters?**
Only users with site administration privileges. The Manage Numbergenerator
page (`/@@ng`) additionally requires the portal-management permission.

## Related Guides

- [Registering Samples](SampleRegistration.md) — where sample IDs are first
  assigned, including secondary sample registration.
- [Sample Partitions](SamplePartitions.md) — partition IDs (`-P01`) and the
  `AnalysisRequestPartition` entry.
- [Secondary Samples](SecondarySamples.md) — secondary sample IDs (`-S01`).
- [Results Publication](ResultsPublication.md) — sample invalidation and
  retest IDs (`-R01`).
- [Worksheets](Worksheets.md) — worksheet IDs and the `Worksheet` entry.
- [Batches](Batches.md) — batch IDs and the `Batch` entry.
- [Quality Control](QualityControl.md) — reference samples (`QC-…`),
  reference analyses (`SA-…`) and duplicates (`D-…`).
- Back to the [User Guide index](UserGuide.md).
