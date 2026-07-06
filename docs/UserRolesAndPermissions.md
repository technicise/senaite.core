# Users, Roles & Permissions

Everything a person can see and do in SENAITE is governed by **roles**:
named bundles of permissions such as *LabManager*, *Analyst* or *Verifier*.
Users are placed into **groups** (Analysts, LabClerks, Verifiers, …), each
group grants one role, and the workflows then decide — per sample state —
which roles may perform which transition.

This guide explains the feature from an end-user perspective: the standard
SENAITE roles and what each one can do, the groups that map to them, how to
create laboratory users, how Lab Contacts (laboratory personnel) are
registered and linked to user accounts, and how departments and the
laboratory information record fit in.

---

## Table of Contents

1. [How Access Control Works](#how-access-control-works)
2. [The Standard SENAITE Roles](#the-standard-senaite-roles)
3. [Role–Capability Matrix](#rolecapability-matrix)
4. [Groups and How They Map to Roles](#groups-and-how-they-map-to-roles)
5. [Creating Laboratory Users](#creating-laboratory-users)
6. [Lab Contacts — Laboratory Personnel](#lab-contacts--laboratory-personnel)
7. [Linking a Lab Contact to a User](#linking-a-lab-contact-to-a-user)
8. [Departments](#departments)
9. [Laboratory Information](#laboratory-information)
10. [How Client Contacts Get Access](#how-client-contacts-get-access)
11. [Security Settings that Fine-Tune Roles](#security-settings-that-fine-tune-roles)
12. [Frequently Asked Questions](#frequently-asked-questions)
13. [Related Guides](#related-guides)

---

## How Access Control Works

Three layers work together:

| Layer | What it does |
|-------|--------------|
| **User** | The account a person logs in with (username and password). A user by itself can do almost nothing. |
| **Group → Role** | Membership in a group such as *Analysts* grants the corresponding role (*Analyst*). The role carries a fixed set of permissions, e.g. `senaite.core: Edit Results`. |
| **Workflow state** | Each sample, analysis and worksheet has a workflow state, and every state defines which permissions are active. This is why an Analyst can enter results on a *Received* sample but not on a *Verified* one — the permission is simply switched off in that state. |

Every action button you see (or don't see) in SENAITE is the visible result
of this combination: *does my role hold the permission, and is that
permission active in the object's current state?*

Two special "local" roles are granted automatically on specific objects
rather than through groups:

- **Owner** — granted to a client contact's user inside its own client
  folder (and to the creator of an object). This is what lets client
  contacts work with their own samples only.
- **ClientGuest** — granted on individual objects that have been explicitly
  shared with users of another client. Never assign it as a global role.

## The Standard SENAITE Roles

| Role | Typical holder | In a nutshell |
|------|----------------|---------------|
| **LabManager** | Laboratory manager | Full control of the laboratory: every sample transition, worksheets, results, verification, publication, all setup editing and user management. The everyday "superuser" of the LIMS. |
| **LabClerk** | Administrative staff | Sample administration: registers, receives, rejects and cancels samples, creates and detaches partitions, adds clients and batches, and edits most setup items. Does not enter, verify or publish results. |
| **Analyst** | Bench scientist | Does the lab work: works on worksheets, assigns analyses, enters and submits results, imports instrument results, adds attachments. |
| **Verifier** | Senior reviewer | Verifies submitted results (`senaite.core: Transition: Verify`), can also assign/unassign analyses to worksheets and edit the results interpretation. |
| **Publisher** | Reporting staff | Publishes verified samples: generates and emails analysis reports, edits the publication specification and can hide analyses from reports. |
| **Sampler** | Field staff | Collects samples when the sampling workflow is enabled (`senaite.core: Transition: Sample Sample`) and enters *field* results. |
| **SamplingCoordinator** | Sampling planner | Schedules sampling (`senaite.core: Transition: Schedule Sampling`), can also collect samples and receive them at the lab. |
| **Preserver** | Sample preparation staff | Preserves samples (`senaite.core: Transition: Preserve Sample`). |
| **RegulatoryInspector** | Auditor / inspector | Read-only access across the whole laboratory: sees all samples, worksheets and results, but performs no transitions. |
| **Client** | Client contact | Portal access for customers: sees and manages only the samples of their own client organisation. Granted automatically when a client contact is linked to a user — see [Clients & Contacts](ClientsAndContacts.md). |
| **Owner / ClientGuest** | (automatic) | Local roles granted on specific objects, as described above — not assigned via groups. |
| **Member** | Any logged-in user | Plain authenticated user with no LIMS capabilities. A user that belongs to no SENAITE group can log in but sees essentially nothing. |
| **Manager / Site Administrator** | System administrator | Technical administration of the whole site, above LabManager. Reserved for IT staff. |

## Role–Capability Matrix

The table below summarises the defaults shipped with SENAITE (what each
role is granted portal-wide; the workflow additionally switches
capabilities off in states where they make no sense — e.g. nobody can enter
results on a *Verified* sample).

| Capability | Lab Manager | Lab Clerk | Analyst | Verifier | Publisher | Sampler | Sampling Coordinator | Preserver | Regulatory Inspector | Client |
|---|---|---|---|---|---|---|---|---|---|---|
| Register samples | ✓ | ✓ | — | — | — | — | — | — | — | ✓ ¹ |
| Receive samples | ✓ | ✓ | — | — | — | — | ✓ | — | — | — |
| Collect samples (sampling workflow) | ✓ | — | — | — | — | ✓ | ✓ | — | — | — |
| Schedule sampling | ✓ | — | — | — | — | — | ✓ | — | — | — |
| Preserve samples | ✓ | — | — | — | — | — | — | ✓ | — | — |
| Reject samples | ✓ | ✓ | — | — | — | — | — | — | — | — |
| Cancel / reinstate samples | ✓ | ✓ | — | — | — | — | — | — | — | ✓ ¹ |
| Create / detach partitions | ✓ | ✓ | — | — | — | — | — | — | — | — |
| Dispatch / restore samples | ✓ | — | — | — | — | — | — | — | — | — |
| Create and remove worksheets | ✓ ² | — | — | — | — | — | — | — | — | — |
| Work on worksheets (assign analyses) | ✓ | — | ✓ | ✓ ³ | — | — | — | — | — | — |
| Enter and submit lab results | ✓ | — | ✓ | — | — | — | — | — | — | — |
| Enter field results | ✓ | — | — | — | — | ✓ | — | — | — | — |
| Import instrument results | ✓ | — | ✓ | — | — | — | — | — | — | — |
| Verify results | ✓ | — | — | ✓ | — | — | — | — | — | — |
| Retract / retest / invalidate | ✓ | — | — | — | — | — | — | — | — | — |
| Publish results | ✓ | — | — | — | ✓ | — | — | — | — | — |
| Add attachments to samples | ✓ | ✓ | ✓ | — | — | — | — | — | — | — |
| Edit laboratory setup | ✓ | ✓ | — | — | — | — | — | — | — | — |
| Add clients | ✓ | ✓ | — | — | — | — | — | — | — | — |
| Manage users and login details | ✓ | — | — | — | — | — | — | — | — | — |
| View all samples and results | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ ¹ |

¹ Client contacts only within their **own** client organisation, and only
  when client permissions allow it — see
  [Clients & Contacts](ClientsAndContacts.md).

² When *Restrict worksheet management to lab managers* is disabled in
  *Setup → Security*, Analysts and Lab Clerks can also create and manage
  worksheets — see
  [Security Settings that Fine-Tune Roles](#security-settings-that-fine-tune-roles).

³ Verifiers can assign/unassign analyses (`senaite.core: Transition: Assign
  Analysis`), which is required for batch verification, but cannot enter
  results.

## Groups and How They Map to Roles

On installation SENAITE creates one group per laboratory role. **Always
grant access by adding users to these groups** rather than assigning roles
directly — it keeps user administration transparent and reversible.

| Group ID | Group title | Role granted |
|----------|-------------|--------------|
| `Analysts` | Analysts | Analyst |
| `Clients` | Clients | Client |
| `LabClerks` | Lab Clerks | LabClerk |
| `LabManagers` | Lab Managers | LabManager |
| `Preservers` | Preservers | Preserver |
| `Publishers` | Publishers | Publisher |
| `Verifiers` | Verifiers | Verifier |
| `Samplers` | Samplers | Sampler |
| `RegulatoryInspectors` | Regulatory Inspectors | RegulatoryInspector |
| `SamplingCoordinators` | Sampling Coordinator | SamplingCoordinator |

Notes:

- A user may belong to **several groups at once**. A senior analyst who
  also verifies colleagues' results would typically be in both `Analysts`
  and `Verifiers`.
- Do **not** add laboratory personnel to the `Clients` group manually. It
  exists for client contacts and is managed automatically: when a client
  contact is linked to a user, that user is placed in a group specific to
  its client organisation (see
  [How Client Contacts Get Access](#how-client-contacts-get-access)).
- In addition to these, each Client object maintains its own hidden group
  for its contacts, and the standard system groups (`Administrators`,
  `Site Administrators`, …) exist for technical administration.

## Creating Laboratory Users

Creating users requires the `Manage users` permission — by default Lab
Managers and system administrators.

The recommended way is to start from the **Lab Contact** and create the
user from there, so the person's account and their personnel record are
linked from the outset (see
[Linking a Lab Contact to a User](#linking-a-lab-contact-to-a-user)).

To create a user directly instead:

1. Open the user administration page: *Site Setup → Users and Groups* (the
   gear icon in the toolbar).
2. Click **Add New User** and fill in the full name, user name, email
   address and password.
3. Open the **Groups** tab (or edit the new user's group memberships) and
   add the user to the SENAITE group(s) matching their duties — e.g.
   `Analysts` for a bench scientist.
4. If the person is laboratory personnel, also create a **Lab Contact**
   for them and link it to the new user, so their name and signature can
   appear on worksheets and reports.

A user who is in no SENAITE group can log in but will not be able to work
with samples — group membership is what activates the role.

## Lab Contacts — Laboratory Personnel

A **Lab Contact** is the personnel record of a laboratory employee. It is
what SENAITE shows on worksheets, analysis results reports and setup
records — the user account alone is not enough.

Lab Contacts are used as:

- the **analyst** and **verifier** identities printed on reports,
- **department managers** (each department requires one),
- the laboratory **supervisor** in the
  [Laboratory Information](#laboratory-information) record,
- selectable **samplers** and other personnel throughout the system.

To create a Lab Contact:

1. Go to *Setup → Lab Contacts* and press **Add**.
2. Fill in the personal details: name, job title, phone and email address.
3. Optionally upload a **Signature** image — it is printed on analysis
   results reports (ideal size around 250 × 150 pixels).
4. Select the **Departments** the person works in, and one of them as the
   **Default Department** (the default can only be chosen among the
   selected departments).
5. Save. Lab Contacts that leave the laboratory can be **deactivated**
   later; they are never deleted, so historical records stay intact.

## Linking a Lab Contact to a User

The **Login Details** tab of a Lab Contact connects the personnel record to
an actual user account. Access to this tab is protected by the permission
`senaite.core: Manage Login Details` (Lab Managers and administrators).

1. Open the Lab Contact and select the **Login Details** tab.
2. Either **link an existing user**: search for the user name and press
   the link button. Or **create a new user** directly from the form: enter
   a user name, email address and password (minimum 5 characters).
3. When creating the user you can also select the laboratory **groups**
   the new account should join (Analysts, Verifiers, …) — system groups
   and client-specific groups are not offered here.
4. Optionally tick the option to **send a registration email** to the new
   user.

What linking does:

- the contact's email address is synchronised with the user's,
- the user's full name is taken from the contact,
- the user appears with the contact's name on worksheets and reports.

A user can be linked to **one contact only**; the form warns you if the
selected user is already taken. The **Unlink** button reverses the
connection at any time (optionally deleting the user account as well).

## Departments

Departments model the sections of your laboratory (e.g. *Microbiology*,
*Chemistry*) and are managed under *Setup → Lab Departments*.

Each department has:

| Field | Purpose |
|-------|---------|
| **Name / Description** | The department as shown throughout the system. |
| **Department ID** | A short unique identifier. |
| **Manager** (required) | A Lab Contact. Departmental managers are referenced on analysis results reports that contain analyses of their department. |

Department assignment happens in two places:

- **Analysis Services** are assigned to a department (see
  [Analysis Services, Profiles & Sample Templates](AnalysesSetup.md)), so
  every analysis — and hence every sample — is traceable to the
  department(s) doing the work.
- **Lab Contacts** list the departments they belong to, with one default
  department.

Like most setup items, departments can be **deactivated** when no longer
used, but not deleted.

## Laboratory Information

*Setup → Laboratory Information* holds the identity of your laboratory,
used prominently on printed and emailed analysis reports:

- laboratory **name**, VAT/tax number, phone and email,
- **physical, postal and billing addresses**,
- the **Lab URL** (web address),
- the **Supervisor** — a Lab Contact,
- the **accreditation** details: accreditation body, reference, logo and
  confidence level, which drive the accreditation statements shown on
  reports for accredited analyses.

Keeping this record complete is mostly a one-off task during initial
configuration, but remember to update it if the laboratory moves or its
accreditation changes.

## How Client Contacts Get Access

Client contacts are **not** laboratory users and are managed differently:
each client organisation has its own folder with its own contacts, and
linking a contact to a user automatically grants the `Owner` local role in
that client and membership in the client's own group (which carries the
global **Client** role). The result: the contact sees and manages only the
samples of their own organisation, and never internal-use samples or
partitions.

The full story — registering clients, adding contacts, granting portal
access, CC contacts and per-client permissions — is covered in
[Clients & Contacts](ClientsAndContacts.md). Partition visibility for
clients is covered in [Sample Partitions](SamplePartitions.md).

## Security Settings that Fine-Tune Roles

A few settings in *Setup* adjust how strictly roles are enforced:

| Setting (Setup → Security) | Default | Effect |
|----------------------------|---------|--------|
| **Restrict worksheet access to assigned analysts** | on | Analysts only see the worksheets assigned to them; disable to let analysts access all worksheets. |
| **Restrict worksheet management to lab managers** | on | Only lab managers create and manage worksheets; disable to let analysts and lab clerks do so too. Automatically locked on while the previous setting is enabled. |
| **Allow submission of results for unassigned analyses** | on | When disabled, users may only submit results for analyses assigned to themselves (lab managers are exempt). |
| **Automatic log-off** | 0 (off) | Minutes of inactivity before a user is logged out. |

And in *Setup → Analyses*:

| Setting | Default | Effect |
|---------|---------|--------|
| **Allow self-verification of results** | off | When enabled, a user who submitted a result may also verify it (only meaningful for users whose role can verify). Can be overridden per analysis service. |
| **Number of required verifications** | 1 | How many different verifiers must verify a result before it becomes *Verified*. |

Finally, note that the **Sampler**, **SamplingCoordinator** and
**Preserver** roles only come into play when the sampling workflow is
active (*Setup → Sampling → Enable Sampling*, plus *Enable Sampling
Scheduling* for the coordinator) — see
[Sample Lifecycle & Workflow States](SampleWorkflow.md).

## Frequently Asked Questions

**What is the difference between a user, a Lab Contact and a client
contact?**
The *user* is the login account. A *Lab Contact* is the personnel record of
a laboratory employee (signature, departments, appearance on reports). A
*client contact* is a person at a customer organisation. Both kinds of
contact can be linked to a user — the location of the contact decides
whether the person acts as lab staff or as a client.

**Can one person hold several roles?**
Yes. Add the user to several groups — e.g. `Analysts` plus `Verifiers` for
a senior analyst who also verifies. Keep in mind that, unless
self-verification is enabled, they still cannot verify results they
submitted themselves.

**Why can't my analyst see a worksheet a colleague created?**
The setting *Restrict worksheet access to assigned analysts* is enabled (it
is by default). Either assign the analyst to the worksheet or disable the
restriction in *Setup → Security*.

**Why doesn't the Verify button appear for my Verifier?**
Most commonly the result was submitted by the same user and
self-verification is disabled, or the sample is not yet in *To be Verified*
state. See
[Results Entry & Verification](ResultsEntryAndVerification.md).

**Why does the Sampler role seem to have no effect?**
The sampling workflow is disabled. Enable *Setup → Sampling → Enable
Sampling*; only then do the *To be Sampled* state and the sample collection
transition exist.

**Should I assign roles directly to a user instead of using groups?**
No. Direct role assignments work but are hard to audit. Group membership is
the supported and transparent mechanism.

**How do I remove access for someone who left the laboratory?**
Remove the user from all groups (or delete/disable the account) in *Site
Setup → Users and Groups*, and **deactivate** their Lab Contact. Do not try
to delete the Lab Contact — historical samples and reports still reference
it.

**What is the "Owner" role I sometimes see mentioned?**
A local role granted automatically on specific objects — most importantly
to client contacts inside their own client folder, and to creators of
objects. You never assign it manually.

**Who can create other users?**
Users holding the `Manage users` permission: Lab Managers and system
administrators.

## Related Guides

- [Clients & Contacts](ClientsAndContacts.md) — client organisations,
  client contacts and how they get portal access.
- [Sample Lifecycle & Workflow States](SampleWorkflow.md) — the transitions
  the roles in this guide act upon.
- [Registering Samples](SampleRegistration.md) — who can register samples
  and how.
- [Sample Rejection](SampleRejection.md) — the rejection workflow performed
  by lab clerks and managers.
- [Worksheets](Worksheets.md) — worksheet creation, analyst assignment and
  the worksheet restrictions.
- [Results Entry & Verification](ResultsEntryAndVerification.md) —
  submitting and verifying results, multi- and self-verification.
- [Publishing Results](ResultsPublication.md) — what the Publisher role
  does.
- [Analysis Services, Profiles & Sample Templates](AnalysesSetup.md) —
  assigning analysis services to departments.
- [Quality Control](QualityControl.md) and [Instruments &
  Calibration](Instruments.md) — day-to-day work of analysts.
- [Batches](Batches.md), [Sample Partitions](SamplePartitions.md),
  [Secondary Samples](SecondarySamples.md) — other areas governed by the
  same roles.
- [Dashboard & Reports](DashboardAndReports.md) and [Storage
  Locations](StorageLocations.md) — companion guides.
- Back to the [User Guide index](UserGuide.md).
