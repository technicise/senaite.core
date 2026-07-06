# Labels

Labels are reusable tags that can be attached to samples — and, if enabled,
to almost any other kind of object in SENAITE — to categorise, flag and find
them: "Urgent", "Re-test", "Audit 2026", "VIP client", and so on. Labelled
objects show their tags as small badges, and a dedicated *Labeled Objects*
listing lets you find everything that carries a given label.

This guide explains the feature from an end-user perspective: what labels
are (and what they are *not*), how to create them in setup, how to attach
and remove them, where they are displayed, and how to enable them for more
content types.

---

## Table of Contents

1. [What is a Label?](#what-is-a-label)
2. [Labels vs. Batch Labels vs. Stickers](#labels-vs-batch-labels-vs-stickers)
3. [Key Concepts](#key-concepts)
4. [Prerequisites and Permissions](#prerequisites-and-permissions)
5. [Creating Labels in Setup](#creating-labels-in-setup)
6. [Enabling Labels for Samples and Other Types](#enabling-labels-for-samples-and-other-types)
7. [Attaching Labels to a Sample](#attaching-labels-to-a-sample)
8. [Removing Labels](#removing-labels)
9. [Where Labels are Displayed](#where-labels-are-displayed)
10. [Finding Everything with a Given Label](#finding-everything-with-a-given-label)
11. [Deactivating Labels](#deactivating-labels)
12. [Frequently Asked Questions](#frequently-asked-questions)
13. [Related Guides](#related-guides)

---

## What is a Label?

A **Label** is a lightweight, reusable tag defined once in setup and then
attached to any number of objects. A label has only:

- a **Name** (must be unique among all labels, active or inactive), and
- an optional **Description** (shown as a hint when users pick labels).

Labels carry no workflow logic of their own — they do not change what
happens to a sample. They are an organisational tool: a way to mark objects
so that people can spot them at a glance and find them again later.

Typical uses:

- flagging priority work ("Urgent", "Rush"),
- marking samples that belong to a project or campaign ("Survey Q3"),
- tagging setup items for housekeeping ("Review", "Deprecated"),
- grouping objects of different kinds under one theme — the same label can
  sit on a sample, a client and a worksheet at the same time.

## Labels vs. Batch Labels vs. Stickers

SENAITE has three features with similar names. They are independent of each
other:

| Feature | What it is | Where it is configured |
|---------|-----------|------------------------|
| **Labels** (this guide) | General-purpose tags that can be attached to samples and other enabled object types. Displayed as badges on screen. | *Setup → Labels* |
| **Batch Labels** | A separate, older set of tags that apply **only to batches**, offered as checkboxes on the batch add/edit form. See the [Batches guide](Batches.md#batch-labels). | *Setup → Batch Labels* |
| **Stickers** | **Printed** barcode/QR labels for physical sample containers — paper, not tags in the database. See [Sticker Printing](ResultsPublication.md#sticker-printing). | *Setup → Sticker* settings and sample type options |

If you want to tag a batch with the labels described in this guide (instead
of Batch Labels), that is also possible — batches are just one more object
type that can be enabled for labels (see
[Enabling Labels for Samples and Other Types](#enabling-labels-for-samples-and-other-types)).

## Key Concepts

| Term | Meaning |
|------|---------|
| **Label** | A named tag defined in *Setup → Labels*, offered as a suggestion wherever labels can be attached. |
| **Labels field** | The field on an object (e.g. in the sample header or on an *Labels* edit tab) where its labels are attached and removed. |
| **Labeled object** | Any object that currently carries at least one label. All labeled objects appear in the *Labeled Objects* listing. |
| **Types with labels** | The registry setting (`label_enabled_portal_types`) that decides which object types show the Labels field on **all** their objects. |
| **Ad-hoc label** | A label typed directly into the Labels field of an object without creating it in setup first. It is stored on that object only. |

A few structural rules:

- Label names are **unique** — the system refuses a second label with the
  same name.
- An object can carry **any number of labels**, and the same label can be
  attached to any number of objects.
- Labels attached to an object are stored **by name** on that object. They
  are not hard-linked to the setup entry: renaming or removing a label in
  setup does not change the tags already sitting on objects.
- Labels are always displayed in **alphabetical order**.

## Prerequisites and Permissions

- Managing the global label list (*Setup → Labels*) and viewing the
  *Labeled Objects* listing require the general setup-management permission
  (`senaite.core: Manage Bika`). By default this is granted to laboratory
  management roles (e.g. Lab Manager) and site administrators — regular
  analysts and clients do not see these setup screens.
- **Attaching or removing labels on an object** requires permission to
  modify that object (*Modify portal content*). In practice: whoever may
  edit a sample may also label it.
- **Seeing** the labels of an object only requires permission to view the
  object itself.
- The Labels field only appears on objects whose type has been enabled for
  labels (see the next sections) — out of the box **no type is enabled**,
  so the first configuration step is always to switch labels on for the
  types you want.

## Creating Labels in Setup

1. Go to **Setup → Labels**.
2. Click **Add**.
3. Fill in:

   - **Name** — the tag text, e.g. `Urgent`. Must be unique.
   - **Description** — optional; shown alongside the name in the label
     picker, so a short hint about when to use the label is helpful.

4. Press **Save**.

The listing shows all labels with **Active**, **Inactive** and **All**
filters. Active labels are the ones offered as suggestions when users
attach labels to objects.

> **Tip:** Users with edit rights can also invent new label texts on the
> fly while tagging an object (see below). Creating the important ones in
> setup first keeps the vocabulary consistent, because only setup labels
> are suggested to everybody.

## Enabling Labels for Samples and Other Types

Which kinds of objects offer the Labels field is controlled by a registry
setting:

1. Go to *Setup → Registry* and open the fieldset **Labels**.
2. In **Types with labels** (registry setting
   `label_enabled_portal_types`), tick every content type that should
   support labels — for example *AnalysisRequest* (samples), *Batch*,
   *Client*, *Worksheet* or any setup type.
3. Save.

Notes:

- The list offers essentially **all content types** of the system, so
  labels are not limited to samples.
- The setting takes effect immediately on the instance you changed it on.
  **In cluster setups** (several SENAITE instances sharing one database)
  the other instances must be **restarted manually** for the change to take
  effect there — the setting screen reminds you of this.
- Independently of this setting, an individual object that already carries
  labels always shows its Labels field, so its tags remain visible and
  editable even if its type is later removed from the list.

## Attaching Labels to a Sample

Once samples are enabled for labels:

1. Open the sample. The **Labels** field appears in the sample header
   (among the standard header fields; like the other header fields it can
   be repositioned or hidden via the header's *Manage Fields*
   configuration).
2. Click into the Labels field and start typing.

   - Matching **active labels** from setup are suggested as you type,
     with their name and description.
   - Pick one or more suggestions — each selected label is shown as a
     small badge in the field.
   - You may also type a text that matches no existing label and add it as
     an **ad-hoc label**. It is stored on this object only and does *not*
     create a new entry in *Setup → Labels*.

3. Save the header (or the edit form).

The sample now carries the labels, appears in the *Labeled Objects*
listing, and can be found by searching for the label name there.

For other enabled types (clients, worksheets, setup items, …) the procedure
is the same, except that the Labels field lives on a dedicated **Labels**
tab/fieldset of the object's edit form rather than in a sample header.

## Removing Labels

1. Open the object and go to its Labels field (sample header or *Labels*
   edit tab).
2. Remove the unwanted badge(s) from the field.
3. Save.

When the last label is removed, the object disappears from the *Labeled
Objects* listing again.

## Where Labels are Displayed

- **Sample view** — the Labels field in the sample header shows the
  attached labels as badges and allows inline editing (for users with edit
  rights).
- **Edit forms** — on other enabled types, the *Labels* tab of the edit
  form shows and manages the attached labels.
- **Labeled Objects listing** — the central overview of everything that
  carries labels, with the labels rendered as badges next to each object
  (see next section).

Labels are an **internal, on-screen** tool: they are not printed on
stickers and are not included in analysis reports by default.

## Finding Everything with a Given Label

1. Go to **Setup → Labels**.
2. Open the **Labeled Objects** tab.

The listing shows **every object in the system that carries at least one
label**, whatever its type, with the columns:

| Column | Content |
|--------|---------|
| **Title** | The object's title/ID, linked — click to jump straight to the object. |
| **Type** | The kind of object (Sample, Client, Worksheet, …). |
| **Labels** | All labels attached to the object, as badges. |

To find everything with a *specific* label, type the label name into the
listing's **search box** — the search covers the objects' IDs, titles and
label names, so searching for `Urgent` returns exactly the objects tagged
`Urgent` (plus any object whose ID or title happens to contain that word).

## Deactivating Labels

Labels follow the usual activate/deactivate lifecycle of setup items:

1. Go to *Setup → Labels*, select the label(s) and press **Deactivate**.
   (Use the *Inactive* filter and **Activate** to bring one back.)

Effects of deactivating a label:

- It is **no longer suggested** when users attach labels to objects.
- Objects that already carry it **keep it** — deactivation does not strip
  the tag from anything, and the objects remain findable through the
  *Labeled Objects* listing.
- Its name stays reserved: a new label with the same name cannot be
  created while the inactive one exists.

## Frequently Asked Questions

**Why don't I see a Labels field on my samples?**
No content type supports labels out of the box. A manager must first add
the sample type (*AnalysisRequest*) — or any other type — to **Types with
labels** in *Setup → Registry*, fieldset *Labels*.

**Are these the same as Batch Labels?**
No. Batch Labels are a separate feature that applies only to batches and is
configured under *Setup → Batch Labels* (see the
[Batches guide](Batches.md#batch-labels)). The labels described here are
general-purpose and work on any enabled type.

**Do labels get printed on the sample stickers or reports?**
No. Labels are on-screen tags only. Printed barcode labels for sample
containers are **stickers** — see
[Sticker Printing](ResultsPublication.md#sticker-printing).

**Who can see the labels on a sample?**
Anyone who can view the sample can see its labels. Only users who can edit
the object can attach or remove labels.

**I typed a new label on a sample — why isn't it offered on other samples?**
Typing a free text into the Labels field creates an *ad-hoc* label stored
on that object only. To make a label available as a suggestion everywhere,
create it in *Setup → Labels*.

**What happens to objects if I rename a label in setup?**
Nothing — labels are stored on objects by their text. Objects tagged before
the rename keep the old text; only newly attached labels use the new name.
If you need a consistent rename, re-tag the affected objects (find them via
the *Labeled Objects* listing first).

**Does deactivating a label remove it from my samples?**
No. Deactivating only removes the label from the suggestions offered when
tagging. Existing tags stay in place until someone removes them from the
objects.

**I enabled a new type for labels but a colleague's screen doesn't show the
field.**
If your installation runs several SENAITE instances against one database (a
cluster), the instances that did not process the settings change must be
restarted before they pick it up.

## Related Guides

- [Batches](Batches.md) — including **Batch Labels**, the separate tagging
  feature for batches only.
- [Publishing Results](ResultsPublication.md) — including **sticker
  printing** (printed barcode labels for physical containers).
- [Registering Samples](SampleRegistration.md) — how samples are created
  before you tag them.
- [Sample Lifecycle & Workflow States](SampleWorkflow.md) — labels never
  change a sample's workflow; this guide covers what does.
- [Sample View Customization](SampleViewCustomization.md) — arranging the
  sample header fields, including where the Labels field appears.
- Back to the [User Guide index](UserGuide.md).
