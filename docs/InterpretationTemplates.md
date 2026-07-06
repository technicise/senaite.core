# Results Interpretation & Interpretation Templates

Beyond raw analysis results, laboratories often need to add a written
conclusion to a sample — a professional judgement of what the results mean,
statements of compliance, or standard remarks required by accreditation. In
SENAITE this text lives in the **Results Interpretation** area of the sample,
and reusable boilerplate for it can be managed as **Interpretation Templates**
in setup.

This guide explains the feature from an end-user perspective: where the
Results Interpretation area is, who can edit it and in which sample states,
how per-department interpretations work, how to set up Interpretation
Templates, and how the text ends up on the published report.

---

## Table of Contents

1. [What is the Results Interpretation?](#what-is-the-results-interpretation)
2. [Key Concepts](#key-concepts)
3. [Where to Find it on the Sample](#where-to-find-it-on-the-sample)
4. [Who Can Edit, and in Which States](#who-can-edit-and-in-which-states)
5. [Per-Department Interpretations](#per-department-interpretations)
6. [Writing an Interpretation](#writing-an-interpretation)
7. [Interpretation Templates in Setup](#interpretation-templates-in-setup)
8. [Scoping Templates to Sample Types and Sample Templates](#scoping-templates-to-sample-types-and-sample-templates)
9. [Applying a Template to a Sample](#applying-a-template-to-a-sample)
10. [Interpretations on the Published Report](#interpretations-on-the-published-report)
11. [Frequently Asked Questions](#frequently-asked-questions)
12. [Related Guides](#related-guides)

---

## What is the Results Interpretation?

The Results Interpretation is a rich-text (formatted) free-text area attached
to every sample. Typical uses include:

- an expert conclusion drawn from the measured results,
- compliance statements ("meets the requirements of …"),
- standard disclaimers or method remarks,
- department-specific commentary (e.g. a microbiology conclusion separate
  from the chemistry one).

The text is written with a WYSIWYG editor, so it can contain headings, bold
text, lists, tables and even images. It is stored with the sample and is
available to the report templates when the sample is published, so the
interpretation normally appears on the analysis report the client receives.

## Key Concepts

| Term | Meaning |
|------|---------|
| **Results Interpretation** | A rich-text area on the sample where laboratory staff write conclusions and commentary about the results. |
| **General interpretation** | The default interpretation text of a sample, not tied to any department. Every sample has one. |
| **Department interpretation** | An additional interpretation text for one specific laboratory department. One tab appears per department involved in the sample's analyses. |
| **Interpretation Template** | A reusable, predefined rich-text snippet managed in setup. It can be inserted into a sample's interpretation instead of typing the text from scratch. |
| **Template scope** | Optional restrictions on an Interpretation Template — by sample type and/or by sample template — that control on which samples it is offered. |

## Where to Find it on the Sample

Open any sample and scroll below the analyses listings. A section titled
**Results interpretation** is shown at the bottom of the sample view:

- Users **with edit rights** (see next section) see a form with:
  - a **Template** selector with a **Use Template** button (only useful if
    Interpretation Templates have been set up),
  - a **General** tab plus one tab per laboratory department involved in the
    sample (see [Per-Department Interpretations](#per-department-interpretations)),
  - a rich-text editor in each tab, and
  - a **Save** button.
- Users **without edit rights** see the same content read-only: the
  **General** text first, followed by the text of each department.

Saving shows a *Changes Saved* confirmation and returns to the sample view.

## Who Can Edit, and in Which States

Editing the Results Interpretation is controlled by the dedicated field
permission `senaite.core: Field: Edit Results Interpretation`. By default it
is granted to the **Lab Manager**, **(Site) Manager** and **Verifier** roles
only — Analysts, Lab Clerks and Clients can read the interpretation but not
change it. See [User Roles & Permissions](UserRolesAndPermissions.md) for how
to adjust this.

Whether the field is editable also depends on the **sample's workflow
state**. Broadly: the interpretation can be written while the sample is being
worked on — up to and including *Verified* — and becomes read-only once the
sample is published or leaves the normal workflow:

| Sample state | Interpretation editable? |
|--------------|--------------------------|
| Registered, Sample due | No |
| To be sampled, Scheduled sampling, To be preserved | Yes |
| Received | Yes |
| To be verified | Yes |
| Verified | Yes |
| Published | No |
| Rejected / Invalid / Cancelled / Dispatched | No |

In practice the most common moment to write the interpretation is between
results verification and publication — which is also why the Verifier role
holds the permission by default.

## Per-Department Interpretations

Each analysis service belongs to a laboratory department (assigned in
*Setup*, see [Analyses Setup](AnalysesSetup.md)). The Results Interpretation
section builds one tab per department that appears among the sample's
analyses, in addition to the always-present **General** tab:

- A drinking-water sample with microbiology and chemistry analyses shows
  three tabs: **General**, **Microbiology** and **Chemistry**.
- Each tab holds its own independent rich text; all of them are stored with
  the sample and all are saved together with the single **Save** button.
- If none of the sample's analyses has a department, only the **General**
  tab is shown.

This lets each section of the laboratory contribute its own conclusion while
the General text carries the overall statement.

## Writing an Interpretation

1. Open the sample and scroll to the **Results interpretation** section.
2. Pick the tab you want to write in (**General** or a department).
3. Type and format the text in the rich-text editor. You can optionally
   insert a predefined template first — see
   [Applying a Template to a Sample](#applying-a-template-to-a-sample).
4. Repeat for other tabs if needed.
5. Press **Save**.

Notes:

- **Images**: pictures pasted or embedded into the editor are not kept
  inline. On save, the system automatically converts each embedded image
  into an **attachment** of the sample and replaces it with a link, keeping
  the sample lightweight. These auto-created attachments are flagged *not*
  to render in the report by default.
- The text can be edited and re-saved as often as needed while the sample is
  in an editable state.

## Interpretation Templates in Setup

If your laboratory issues similar conclusions again and again, define them
once as **Interpretation Templates**:

1. Go to **Setup → Interpretation Templates**.
2. Press **Add**.
3. Fill in the form:

   | Field | Purpose |
   |-------|---------|
   | **Name** | The title shown in the template selector on samples. Required. |
   | **Description** | Optional internal note about when to use the template. |
   | **Analysis templates** | Optional scope: sample templates this interpretation template is intended for (see next section). |
   | **Sample types** | Optional scope: sample types this interpretation template is intended for (see next section). |
   | **Text** | The rich-text boilerplate itself, written with the same WYSIWYG editor used on samples. |

4. Save.

The listing at *Setup → Interpretation Templates* shows the **Title**,
**Description**, linked **Sample Types** and a plain-text preview of the
**Text** of each template, with **Active**, **Inactive** and **All** filters.
Templates you no longer want offered can be **deactivated** from the listing
instead of deleted — inactive templates are never offered on samples, but
past interpretations that were created from them are unaffected (the text
was copied into the sample, not linked).

Writing tip: because the template text is *inserted* into the sample's
interpretation (not merged field-by-field), write it as ready-to-publish
prose with placeholders the user fills in manually where sample-specific
values are needed.

## Scoping Templates to Sample Types and Sample Templates

By default (both scope fields left empty) an Interpretation Template is
offered on **every** sample. The two reference fields narrow this down:

- **Sample types** — the template is offered on samples of any of the
  selected sample types.
- **Analysis templates** — the template is offered on samples that were
  created from any of the selected sample templates.

The two criteria work as alternatives: a template is offered when the sample
matches its sample types *or* its sample template matches one of the listed
analysis templates. This keeps the **Template** dropdown on each sample
short and relevant — a water-microbiology conclusion is never offered on a
food-chemistry sample.

## Applying a Template to a Sample

1. Open the sample and scroll to the **Results interpretation** section
   (you need edit rights — see
   [Who Can Edit, and in Which States](#who-can-edit-and-in-which-states)).
2. Select the tab (**General** or a department) where the text should go.
3. In the **Template** dropdown above the tabs, choose one of the offered
   templates (the dropdown initially reads *Select template* and lists only
   the active templates whose scope matches this sample).
4. Press **Use Template**.

The template's text is **inserted at the cursor position** of the editor in
the currently active tab, with its formatting preserved. It does not replace
what is already there, so you can:

- insert several templates one after another,
- combine boilerplate with free text,
- edit the inserted text freely afterwards — it is a plain copy, no link to
  the template remains.

5. Adjust the text as needed and press **Save**.

## Interpretations on the Published Report

The Results Interpretation is part of the data handed to the report
templates when a sample is published (see
[Results Publication](ResultsPublication.md)). With the default report
templates:

- the **General** interpretation and each **department** interpretation are
  printed on the analysis report, formatting included;
- because the sample becomes read-only on publication, the interpretation
  shown on the report is frozen with it — to change it, the sample must be
  invalidated and a retest published, as with any other reported data.

Custom report templates decide for themselves whether and where to render
the interpretation, so if your laboratory uses a customised layout, check
with whoever maintains your report templates.

## Frequently Asked Questions

**I don't see the Template dropdown on the sample. Why?**
Either you lack the `senaite.core: Field: Edit Results Interpretation`
permission (the section then renders read-only), the sample is in a state
where the field is not editable (e.g. *Published*), or no active
Interpretation Template matches the sample — with no templates to offer the
selector is of no use.

**Why can't the Analyst write the interpretation?**
By default the permission is limited to Lab Manager, Manager and Verifier,
on the assumption that the interpretation is a senior sign-off written
around verification time. Your administrator can grant the field permission
to other roles.

**Can I have different interpretations for different departments?**
Yes. One tab is shown per department involved in the sample's analyses, plus
the always-present **General** tab. Each tab stores its own text.

**Does applying a template overwrite my existing text?**
No. *Use Template* inserts the template's text at the cursor position in the
active tab. Existing text stays; delete it manually if you want a clean
replacement.

**If I edit an Interpretation Template later, do past samples change?**
No. The template text is copied into the sample at the moment you press
*Use Template*. Later changes to the template only affect future uses.

**Can clients edit the Results Interpretation?**
No. Client contacts can read it (and see it on the report) but the edit
permission is not granted to the Client role by default.

**What happens to images I paste into the interpretation?**
On save they are converted into attachments of the sample and replaced by
links in the text. The auto-created attachments are excluded from report
rendering by default; see [Attachments](Attachments.md).

**Do partitions inherit the primary sample's interpretation?**
No. The Results Interpretation is deliberately not copied when partitions
are created — each partition (like each sample) carries its own.

## Related Guides

- [Results Entry & Verification](ResultsEntryAndVerification.md) — the
  workflow steps during which the interpretation is typically written.
- [Results Publication](ResultsPublication.md) — generating and delivering
  the report on which the interpretation appears.
- [Analysis Services, Profiles & Sample Templates](AnalysesSetup.md) —
  departments on analysis services (they drive the interpretation tabs) and
  the sample templates used for template scoping.
- [User Roles & Permissions](UserRolesAndPermissions.md) — granting or
  restricting the field permission that controls who may edit
  interpretations.
- [Attachments](Attachments.md) — how images embedded in the interpretation
  are stored as sample attachments.
- Back to the [User Guide index](UserGuide.md).
