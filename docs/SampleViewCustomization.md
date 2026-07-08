# Customizing the Sample View

The Sample View is the central working screen for a single sample: a header
with the sample's details at the top, followed by sections for analyses,
remarks and results interpretation. SENAITE lets laboratory managers decide
which sample fields appear in that header, how they are arranged, and how the
sections below behave — without any programming.

This guide explains the feature from an end-user perspective: how the sample
header is structured, how to rearrange and hide fields with the *Manage
Sample Fields* form, how inline editing works, which registry settings change
the sample view, and what users can customize in the samples listing.

---

## Table of Contents

1. [What Can Be Customized?](#what-can-be-customized)
2. [Key Concepts](#key-concepts)
3. [Prerequisites and Permissions](#prerequisites-and-permissions)
4. [The Sample Header at a Glance](#the-sample-header-at-a-glance)
5. [Managing Sample Header Fields](#managing-sample-header-fields)
6. [Column Layout and the Standard Fields Toggle](#column-layout-and-the-standard-fields-toggle)
7. [Resetting to Defaults](#resetting-to-defaults)
8. [Scope: Global, Not Per-User](#scope-global-not-per-user)
9. [Inline Editing in the Sample Header](#inline-editing-in-the-sample-header)
10. [Sample View Sections](#sample-view-sections)
11. [Choosing Analysis Columns in the Sample View](#choosing-analysis-columns-in-the-sample-view)
12. [Customizing the Samples Listing](#customizing-the-samples-listing)
13. [Frequently Asked Questions](#frequently-asked-questions)
14. [Related Guides](#related-guides)

---

## What Can Be Customized?

Three different layers of the sample-related screens can be tailored:

- **The sample header** — the table of sample fields (client, contact, sample
  type, dates, etc.) shown at the top of every sample. Via the *Manage Sample
  Fields* form you decide which fields are *prominent*, which are *standard*,
  which are hidden, their order, and how many columns are used.
- **The sample view sections** — the analyses tables and other sections below
  the header. Registry settings control whether these tables start collapsed
  or expanded, and a Setup option controls which analysis columns are shown.
- **The samples listing** — the folder listing of all samples, with its
  filter tabs and its own set of toggleable columns.

## Key Concepts

| Term | Meaning |
|------|---------|
| **Sample Header** | The field table at the top of the sample view. It doubles as an inline edit form for users with sufficient permissions. |
| **Prominent Fields** | Fields pinned at the very top of the header. They are always visible and cannot be faded out. |
| **Standard Fields** | Fields listed below the prominent ones. They can be collapsed/expanded with a chevron toggle. |
| **Hidden Fields** | Fields whose visibility checkbox is unticked in *Manage Sample Fields*. They are not rendered in the header at all. |
| **Sample Sections** | The blocks below the header: *Remarks*, *Field Analyses*, *Analyses*, *QC Analyses*, *Results Interpretation*, attachments, etc. |
| **SENAITE Registry** | A configuration screen (*Site Setup → SENAITE Registry*) where the underlying display settings are stored and can also be edited directly. |

## Prerequisites and Permissions

- The *Manage Sample Fields* form and the *SENAITE Registry* control panel
  are protected by the site management permission (`senaite.core: Manage
  Bika`). In a default installation this means **Lab Managers and site
  administrators** can change these settings.
- The small slider icon that links to *Manage Sample Fields* from the sample
  header is shown only to users with the **Manager** or **LabManager** role.
- All display settings described here are **global** — they change the sample
  view for every user of the site (see
  [Scope: Global, Not Per-User](#scope-global-not-per-user)).
- Whether an individual user can *edit* values in the header is a separate
  matter of field permissions and workflow state — see
  [Inline Editing in the Sample Header](#inline-editing-in-the-sample-header).

## The Sample Header at a Glance

The header renders the sample's fields in two blocks:

- **Prominent fields** appear first, by default in a single column
  (one field per row). Use this block for the handful of fields your lab
  always wants to see — e.g. client, contact, sample type.
- **Standard fields** appear below, by default in three columns. This block
  can be collapsed and expanded with the **chevron toggle** (up/down arrow) at
  the top right of the header ("Show/Hide additional fields"). Whether it
  starts expanded is controlled by the **Always show standard fields**
  setting.

Each field is rendered with its label (hover it for the field's description
as a tooltip) and a red marker when the field is required. On a primary
sample that has partitions, a small tree icon next to an editable field warns
that *"Changes will be propagated to partitions"*.

Which fields are offered at all is determined automatically from the sample's
schema and your lab's setup. For example, sampler and sampling-date fields
only appear when the **Sampling Workflow** is enabled in the setup. When new
fields become available they are automatically appended to the standard
fields as visible; fields that disappear from the schema are dropped from the
configuration.

## Managing Sample Header Fields

1. Open any sample and click the **slider icon** (⚙-style icon titled
   *Manage sample fields*) at the top right of the sample header — or browse
   directly to the `manage-sample-fields` page of a sample. The **Manage
   Sample Form Fields** page opens, described as *"Manage the order and
   visibility of the sample fields"*.
2. The page shows two sortable lists:

   - **Prominent fields** — *"The fields are always listed on top and can
     not be faded out."*
   - **Standard fields** — *"The fields are listed below the prominent
     fields and can be faded out."*

3. **Drag and drop** fields:

   - within a list, to change their display **order**;
   - between the two lists, to promote a field to prominent or demote it to
     standard.

4. **Tick or untick the checkbox** next to a field to control its
   **visibility**. Unticked fields are hidden from the sample header
   entirely (they remain in the list here so you can bring them back later).
5. Adjust the layout options at the top of the form if needed (see next
   section).
6. Press **Save**. A *"Changes saved."* message confirms the update, and all
   sample views now use the new arrangement.

Required fields are marked in the lists just like in the header, so you can
see at a glance which fields are mandatory during registration. Hiding a
field here only affects the *display* in the sample header — it does not
remove the field from the sample or from the Add Samples form.

## Column Layout and the Standard Fields Toggle

The top of the *Manage Sample Fields* form offers three layout settings.
They correspond to registry records that can also be edited under *Site
Setup → SENAITE Registry* (fieldset *Sample Header*):

| Form option | Registry setting | Default | Effect |
|-------------|------------------|---------|--------|
| **Number of prominent columns** | `sampleheader_prominent_columns` | 1 | How many prominent fields are placed side by side per row (0–10). Setting **0** hides the prominent block completely. |
| **Number of standard columns** | `sampleheader_standard_columns` | 3 | How many standard fields are placed side by side per row (0–10). Setting **0** hides the standard block completely. |
| **Always show standard fields** | `sampleheader_show_standard_fields` | on | Whether the standard fields block starts **expanded**. When off, the block starts collapsed and users expand it on demand with the chevron toggle. |

The remaining registry records of the same fieldset store what you arrange
with drag & drop and the checkboxes: `sampleheader_prominent_fields`
(ordered list of prominent fields), `sampleheader_standard_fields` (ordered
list of standard fields) and `sampleheader_field_visibility` (which fields
are shown or hidden). Editing them through the *Manage Sample Fields* form
is strongly recommended over editing the registry directly.

## Resetting to Defaults

Press the **Reset** button on the *Manage Sample Fields* form to flush all
sample header settings. A *"Configuration restored to default values."*
message confirms it, and the header goes back to the out-of-the-box
behaviour: prominent/standard assignment and order as defined by each
field's own default, one prominent column, three standard columns, all
fields visible, standard fields expanded.

## Scope: Global, Not Per-User

The *Manage Sample Fields* form states it explicitly: *"Note: The settings
are global and apply to all sample views."*

- There is **no per-user sample header layout** — the configuration is
  stored once, site-wide, and every user sees the same field arrangement.
- What still differs per user is *content and editability*: fields a user is
  not allowed to view are omitted for that user, and fields render read-only
  when the user lacks edit permission (see next section). Client contacts
  therefore typically see a leaner, read-only header even though the layout
  configuration is the same.
- The only per-user element is transient: any user can collapse/expand the
  standard fields block with the chevron while looking at a sample, but this
  is not remembered.

## Inline Editing in the Sample Header

The sample header is not just a display — it is also an inline edit form:

- Each field renders in **edit mode** when the current user has that field's
  edit permission on the sample in its **current workflow state**; otherwise
  it renders read-only. As soon as at least one field is editable, a
  **Save** button appears below the header.
- Field-by-field edit rights are granted and revoked by the sample workflow:
  early states (e.g. *Registered*, *Due*, *Received*) keep most fields
  editable for lab roles, while later states (*To be Verified*, *Verified*,
  *Published*) progressively lock fields down. See
  [Sample Lifecycle & Workflow States](SampleWorkflow.md) for the states and
  [User Roles & Permissions](UserRolesAndPermissions.md) for who holds which
  permissions.
- After saving, values are validated (invalid entries are flagged on the
  corresponding field), the sample is updated and re-indexed, and a
  *"Changes saved."* message is shown.
- On a **primary sample with partitions**, fields marked with the tree icon
  propagate their new value to the partitions when saved — see
  [Sample Partitions](SamplePartitions.md).

## Sample View Sections

Below the header, the sample view is composed of sections. Analyses are
grouped by their *point of capture* and each group only appears when the
sample actually contains such analyses:

- **Field Analyses** — analyses captured in the field. Only shown if the
  sample has at least one field analysis.
- **Analyses** — the regular laboratory analyses.
- **QC Analyses** — quality control analyses assigned via worksheets. Only
  shown if the sample has QC analyses.
- **Remarks** — free-text remarks on the sample.
- **Results Interpretation** — rich-text interpretation of the results,
  optionally per department.
- **Attachments** — files attached to the sample and its analyses.

Whether the three analyses tables start **collapsed or expanded** is
controlled by registry settings under *Site Setup → SENAITE Registry*,
fieldset *Sample View* ("Sample view configuration"):

| Setting | Label | Default |
|---------|-------|---------|
| `sampleview_collapse_field_analysis_table` | Collapse field analysis table | off (expanded) |
| `sampleview_collapse_lab_analysis_table` | Collapse lab analysis table | off (expanded) |
| `sampleview_collapse_qc_analysis_table` | Collapse qc analysis table | **on (collapsed)** |

Collapsed sections can always be expanded manually by clicking the section
title — the settings only choose the initial state, so labs can e.g. keep
the rarely-consulted QC table out of the way by default.

## Choosing Analysis Columns in the Sample View

The columns of the analyses tables inside the sample view can be trimmed and
reordered globally:

1. Go to **Setup** and open the **Analyses** tab of the setup form.
2. Locate **Sample view analysis columns**: *"Select which columns to
   display in sample analysis listings. The order of selection determines
   the display order. Unselected columns are hidden. Leave empty to show all
   columns in default order."*
3. Select the columns you want, in the order you want, and save.

A sibling option, **Worksheet view analysis columns**, does the same for
analysis listings on worksheets.

## Customizing the Samples Listing

The **Samples** folder listing offers customization to every user, directly
in the listing interface:

- **Filter tabs** along the top narrow the listing by workflow state:
  *Active* (the default), *To Be Sampled*, *To Be Preserved*, *Scheduled
  sampling*, *Due*, *Received*, *To be verified*, *Verified*, *Published*,
  *Dispatched*, *Cancelled*, *Invalid*, *All* and *Rejected*, plus icon tabs
  for *Assigned*, *Unassigned* and *Late* samples. Sampling-related tabs are
  only meaningful when the Sampling Workflow is enabled.
- **Toggleable columns**: the listing defines many more columns than are
  shown by default. Always/initially visible are e.g. *Priority*,
  *Progress*, *Sample ID*, *Creator*, *Date Sampled*, *Client*, *Client ID*,
  *Sample Type* and *State* (plus *Expected Sampling Date* and *Sampler*
  when the sampling workflow is on). Hidden by default but available are
  e.g. *Client Order*, *Date Registered*, *Date Received*, *Due Date*,
  *Date Verified*, *Date Published*, *Batch ID*, *Client Ref*, *Client SID*,
  *Contact*, *Sample Point*, *Storage Location*, *Sampling Deviation*,
  *Preserver*, *Profile*, *Number of Analyses*, *Template* and *Printed*.
- Use the **column toggle menu** of the listing table (the control at the
  top right of the table) to show or hide any of these columns, and use the
  column headers to sort. The interactive listing table itself — including
  the column toggle menu, searching, and any user-saved listing
  configurations — is provided by SENAITE's listing component, a companion
  add-on to the core; its exact controls may therefore vary slightly with
  the version installed.
- Rows are searchable via the listing's search box and can be selected with
  checkboxes to perform bulk actions (receive, print stickers, etc.).

The same listing mechanics (tabs, toggleable columns, search) apply to the
per-client samples listings and to most other listings in SENAITE.

## Frequently Asked Questions

**Can each user save their own sample header arrangement?**
No. The *Manage Sample Fields* configuration is global and applies to all
sample views and all users. Only the temporary collapse/expand of the
standard fields block is under each user's control while viewing a sample.

**I hid a field in Manage Sample Fields — why does it still show in the Add
Samples form?**
The form only controls the **sample header** of existing samples. The Add
Samples form has its own field configuration — see
[Registering Samples](SampleRegistration.md).

**Why did a new field suddenly appear in my header?**
When new fields become available — for example after enabling the Sampling
Workflow in the setup — they are added automatically to the **standard
fields** as visible, so nothing gets silently lost. You can then reposition
or hide them.

**Why can't I edit a certain field in the header?**
Either your role lacks the edit permission for that field, or the sample has
progressed to a workflow state where the field is locked. See
[Sample Lifecycle & Workflow States](SampleWorkflow.md) and
[User Roles & Permissions](UserRolesAndPermissions.md).

**I don't see the slider icon to manage the fields.**
The icon is only shown to users with the Manager or Lab Manager role, and
the page itself requires the site management permission.

**Can I hide the prominent fields block entirely?**
Yes — set **Number of prominent columns** to 0 (the block is also omitted
when no fields are assigned to it). The same works for the standard block
via **Number of standard columns**.

**Why is the QC Analyses table collapsed by default?**
That is the default value of `sampleview_collapse_qc_analysis_table`. Change
it in *Site Setup → SENAITE Registry*, fieldset *Sample View*, if your lab
prefers it expanded.

**Something went wrong — how do I start over?**
Press **Reset** on the *Manage Sample Fields* form to restore all sample
header settings to their defaults.

## Related Guides

- [Sample Lifecycle & Workflow States](SampleWorkflow.md) — which workflow
  states lock which header fields for inline editing.
- [User Roles & Permissions](UserRolesAndPermissions.md) — who may manage
  display settings and who may edit sample fields.
- [Registering Samples](SampleRegistration.md) — the Add Samples form, whose
  fields are configured independently of the sample header.
- [Sample Partitions](SamplePartitions.md) — why some header fields
  propagate their changes to partitions.
- [Storage Locations](StorageLocations.md) — the storage location shown as a
  header field and as an optional listing column.
- [Interpretation Templates](InterpretationTemplates.md) — the Results
  Interpretation section of the sample view.
- [Attachments](Attachments.md) — the attachments section of the sample
  view.
- [Labels](Labels.md) — labelling samples, available from the sample view.
- Back to the [User Guide index](UserGuide.md).
