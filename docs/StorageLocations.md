# Storage Locations

Storage Locations let you record **where a physical sample is kept** in the
laboratory — a fridge, a cupboard, a store room, an off-site archive. They
are simple, descriptive setup records: you define them once under *Setup*,
and then pick one on each sample, either when the sample is registered or at
any point later while the sample is still being worked on.

This guide explains the feature from an end-user perspective: how to
configure storage locations, how to assign them to samples, where they are
displayed, and what SENAITE core does — and deliberately does not — provide
in terms of sample storage.

---

## Table of Contents

1. [What is a Storage Location?](#what-is-a-storage-location)
2. [Key Concepts](#key-concepts)
3. [Prerequisites and Permissions](#prerequisites-and-permissions)
4. [Configuring Storage Locations in Setup](#configuring-storage-locations-in-setup)
5. [The Storage Locations Listing](#the-storage-locations-listing)
6. [Activating and Deactivating Storage Locations](#activating-and-deactivating-storage-locations)
7. [Assigning a Storage Location at Registration](#assigning-a-storage-location-at-registration)
8. [Changing the Storage Location of an Existing Sample](#changing-the-storage-location-of-an-existing-sample)
9. [Where the Storage Location is Displayed](#where-the-storage-location-is-displayed)
10. [Searching and Filtering by Storage Location](#searching-and-filtering-by-storage-location)
11. [Advanced Storage: the senaite.storage Add-on](#advanced-storage-the-senaitestorage-add-on)
12. [Frequently Asked Questions](#frequently-asked-questions)
13. [Related Guides](#related-guides)

---

## What is a Storage Location?

A Storage Location is a record in the laboratory setup that describes a
place where samples are physically kept — for example *Fridge 2, Cold Room,
Building A* or *Archive Shelf 3, Store Room, Main Site*.

Each sample carries a single **Storage Location** field ("Location where the
sample is kept"). The field is a reference to one of the storage locations
defined in setup, so all samples share a controlled, consistent vocabulary
of places rather than free text.

It is important to understand the scope of the feature in SENAITE core:

- A storage location is **one flat record**. Its *Site*, *Location* and
  *Shelf* fields are descriptive text fields on that single record — they do
  **not** create a browsable hierarchy of sites containing locations
  containing shelves.
- Assigning a location to a sample is **informative only**. It does not
  change the sample's workflow state, does not track capacity or occupied
  positions, and there is no "store" / "recover" transition in core.
- If you need real storage management — freezers, shelves, numbered
  positions, a *stored* sample state — that is provided by the separate
  **senaite.storage** add-on, not by core (see
  [Advanced Storage](#advanced-storage-the-senaitestorage-add-on)).

## Key Concepts

| Term | Meaning |
|------|---------|
| **Storage Location** | A setup record describing a place where samples are kept. Selected on samples via the *Storage Location* field. |
| **Address** | The name (title) of the storage location record — the text shown wherever the location is displayed. It is the only required field. |
| **Site / Location / Shelf fields** | Optional descriptive text fields (title, code, description) that let you note the site, the location within the site, and the shelf. Purely informative. |
| **Active / Inactive** | Only *active* storage locations can be selected on samples. Deactivated ones are kept for traceability but disappear from the selection list. |

## Prerequisites and Permissions

- Creating storage locations is protected by the permission
  `senaite.core: Add StorageLocation`. By default it is granted to lab
  roles such as Lab Manager and Lab Clerk.
- Setting the field on a sample **after** registration is protected by the
  permission `senaite.core: Field: Edit Storage Location`, granted by
  default to Lab Clerk, Lab Manager and Manager.
- The field itself is readable by anyone who can view the sample, so client
  contacts see the assigned storage location too.
- The field stops being editable once the sample reaches the *Verified*
  state (see
  [Changing the Storage Location](#changing-the-storage-location-of-an-existing-sample)).

See [User Roles & Permissions](UserRolesAndPermissions.md) for how roles and
permissions work in general.

## Configuring Storage Locations in Setup

1. Go to **Setup → Storage Locations**.
2. Click the **Add** button.
3. Fill in the form. Only the first field is mandatory:

   | Field | Required | Purpose |
   |-------|----------|---------|
   | **Address** | yes | The name of the storage location. This is the text displayed on samples, in listings and in the selection dropdown, so make it self-explanatory (e.g. *Fridge 2 — Cold Room*). |
   | **Description** | no | Free-text description of the storage location. |
   | **Site Title** | no | Title of the site (e.g. the building or facility). |
   | **Site Code** | no | Code for the site. |
   | **Site Description** | no | Description of the site. |
   | **Location Title** | no | Title of the location within the site (e.g. a room). |
   | **Location Code** | no | Code for the location. |
   | **Location Description** | no | Description of the location. |
   | **Location Type** | no | Free-text type of location (e.g. *fridge*, *cabinet*). |
   | **Shelf Title** | no | Title of the shelf. |
   | **Shelf Code** | no | Code for the shelf. |
   | **Shelf Description** | no | Description of the shelf. |

4. Press **Save**.

Notes:

- The site / location / shelf fields are plain text. They are shown in the
  storage locations listing (see next section) but are not searchable from
  samples and do not link records together. If you want a recognisable
  structure, encode it in the **Address**, e.g.
  `Main Lab / Cold Room / Fridge 2 / Shelf B`.
- Storage location records get an automatically generated internal ID; you
  do not need to configure anything for that.

## The Storage Locations Listing

*Setup → Storage Locations* shows all defined locations with the following
columns (some are toggleable via the listing's column selector):

- **Storage Location** (the Address / name, linked to the record),
- **Description**,
- **Site Title**, **Site Code**,
- **Location Title**, **Location Code**,
- **Shelf Title**, **Shelf Code**.

The listing offers three filters: **Active** (default), **Inactive** and
**All**, and each record can be opened and edited at any time via its
**Edit** action.

## Activating and Deactivating Storage Locations

Storage locations follow the standard activate/deactivate lifecycle used by
most setup records:

1. In *Setup → Storage Locations*, select one or more locations with the
   checkboxes.
2. Click **Deactivate** (or, in the *Inactive* filter, **Activate**).

Effects of deactivating:

- The location no longer appears in the *Storage Location* selection field
  on samples — only **active** locations are offered.
- Samples that already reference the location **keep** it; nothing changes
  on existing records, so traceability is preserved.

Deactivate rather than delete when a fridge is decommissioned or a store
room is retired.

## Assigning a Storage Location at Registration

The *Storage Location* field is part of the **Add Samples** form (see
[Registering Samples](SampleRegistration.md)):

1. Open the Add Samples form (e.g. **Add** from the samples listing or from
   within a client).
2. Find the **Storage Location** row ("Location where the sample is kept").
3. Start typing and pick a location from the dropdown. Only **active**
   locations are offered, sorted alphabetically.
4. Complete the rest of the form and save.

Behaviour worth knowing:

- The field is optional — samples can be registered without a storage
  location and given one later.
- When you **copy** a sample in the Add form, the storage location is
  copied across like the other sample fields.
- When registering a **secondary sample** (see
  [Secondary Samples](SecondarySamples.md)), the field is filled
  automatically from the primary sample and shown **disabled** — a
  secondary sample shares the primary's physical material, so it inherits
  its storage location.

## Changing the Storage Location of an Existing Sample

Samples move around: from the reception bench to a fridge, from the fridge
to an archive. To update the field on a sample that already exists:

1. Open the sample.
2. In the sample header (the field area at the top of the sample view),
   locate **Storage Location**. If it is not shown, it may be in the
   collapsed part of the header — use the toggle to show all fields — or it
   may have been hidden by a lab manager (see next section).
3. Pick the new location and press **Save**.

Whether the field is editable depends on two things:

- **Your permission** — `senaite.core: Field: Edit Storage Location`
  (Lab Clerk, Lab Manager, Manager by default). Users without it see the
  field read-only.
- **The sample's workflow state** — the field is editable while the sample
  is in *Registered*, *Scheduled sampling*, *To be sampled*,
  *To be preserved*, *Sample due*, *Received* or *To be verified*. From
  **Verified** onwards (and in *Published*, *Rejected*, *Invalid*,
  *Cancelled* and *Dispatched*) the field is locked for everyone, like the
  other sample fields.

See [Sample Lifecycle & Workflow States](SampleWorkflow.md) for the full
state model. Changes to the field are recorded in the sample's audit log
like any other sample modification.

## Where the Storage Location is Displayed

- **Sample view** — the storage location appears as a field in the sample
  header, showing the location's name as a link to the setup record. Lab
  managers can rearrange the header via its *manage* (gear) action: each
  field, including *Storage Location*, can be made prominent, standard or
  hidden, and the field order can be changed.
- **Samples listings** — the samples listing has a **Storage Location**
  column. It is **hidden by default**: open the listing's column selector
  (gear icon above the table) and tick *Storage Location* to show it. The
  column displays the location's name and is sortable.
- **Published reports** — the storage location is part of the sample's
  data and is available to report templates, but whether it is printed
  depends on the report template your instance uses. See
  [Publishing Results](ResultsPublication.md).

## Searching and Filtering by Storage Location

Core keeps this deliberately simple:

- The **search box** above the samples listing does *not* match storage
  locations — it searches sample IDs, client data, sample types, sample
  points and batches.
- To review samples by location, enable the **Storage Location column** in
  the samples listing and **sort** on it; all samples of the same location
  group together.
- There is no dedicated storage-location filter in the samples listing in
  core, and no listing on the storage location record itself showing the
  samples kept there.

If you routinely need "what is in fridge 2?" queries, that is a sign you
have outgrown the core field — see the next section.

## Advanced Storage: the senaite.storage Add-on

SENAITE core intentionally provides only the descriptive field documented
here. Full sample-storage management is the job of the separate
**senaite.storage** add-on, which must be installed in addition to core.
That add-on models storage facilities with containers and numbered
positions and adds proper store/recover handling for samples.

Keep the two apart when reading documentation or community answers:

| | Core *Storage Location* field | *senaite.storage* add-on |
|---|---|---|
| Nature | Descriptive label on the sample | Managed storage structure |
| Positions / capacity | No | Yes |
| Effect on sample workflow | None | Samples are stored and recovered |

If the add-on is not installed on your instance, only the behaviour
described in this guide is available. The add-on itself is outside the
scope of this guide — refer to its own documentation.

## Frequently Asked Questions

**Why is the name field of a storage location labelled "Address"?**
That is simply the label of the record's title field on the add/edit form.
Whatever you type there is the name shown on samples, in listings and in
the selection dropdown.

**Can I build a hierarchy of sites, locations and shelves?**
No. Each storage location is a single flat record; the site, location and
shelf fields are descriptive text on that record. Use a naming convention
in the *Address* field to make the structure visible, or use the
*senaite.storage* add-on for a real hierarchy.

**Who can set the storage location?**
Anyone who can register samples can fill the field in the Add Samples form.
Changing it on an existing sample additionally requires the
`senaite.core: Field: Edit Storage Location` permission, held by lab roles
(Lab Clerk, Lab Manager, Manager) by default.

**Why can't I edit the field on my sample anymore?**
Either you lack the field edit permission, or the sample has reached the
*Verified* state (or later, e.g. *Published*), where the field is locked
for everyone.

**Can clients see the storage location?**
Yes — client contacts who can view the sample also see its storage
location. Only editing is restricted to lab roles.

**Do partitions inherit the primary sample's storage location?**
Partitions are created as full samples copying details from the primary,
but each partition has its own *Storage Location* field that you can set
independently — useful, since aliquots often end up in different places.
See [Sample Partitions](SamplePartitions.md).

**What happens to samples when I deactivate their storage location?**
Nothing. The samples keep their reference and continue to display the
location; it just cannot be assigned to new samples anymore.

**Does assigning a storage location change the sample's state?**
No. It is purely informative metadata. Store/recover workflow transitions
exist only in the *senaite.storage* add-on.

## Related Guides

- [Registering Samples](SampleRegistration.md) — the Add Samples form where
  the storage location is first assigned.
- [Sample Lifecycle & Workflow States](SampleWorkflow.md) — the states that
  determine when the field is still editable.
- [Secondary Samples](SecondarySamples.md) — why secondary samples inherit
  the primary's storage location.
- [Sample Partitions](SamplePartitions.md) — aliquots that may each get
  their own storage location.
- [Publishing Results](ResultsPublication.md) — report templates and what
  sample data they print.
- [User Roles & Permissions](UserRolesAndPermissions.md) — how the
  permissions mentioned in this guide are assigned.
- Back to the [User Guide index](UserGuide.md).
