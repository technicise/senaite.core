# Clients & Contacts

Clients are the organisations that submit samples to your laboratory, and
Contacts are the people at those organisations who register samples, receive
notifications and read the results reports. SENAITE keeps each client's data
in its own folder, so samples, batches, reports and client-specific setup
never leak from one client to another.

This guide explains the feature from an end-user perspective: how to create
clients and contacts, how to give contacts access to the client portal, how
CC recipients work at publication time, what client contacts can see and do,
and which settings control all of this.

---

## Table of Contents

1. [What is a Client?](#what-is-a-client)
2. [Key Concepts](#key-concepts)
3. [Creating a Client](#creating-a-client)
4. [Client Contacts](#client-contacts)
5. [Granting Portal Access to a Contact](#granting-portal-access-to-a-contact)
6. [CC Contacts and CC Emails](#cc-contacts-and-cc-emails)
7. [Client-Specific Setup](#client-specific-setup)
8. [What Client Contacts See and Can Do](#what-client-contacts-see-and-can-do)
9. [Client Settings](#client-settings)
10. [Sharing Content with Other Clients](#sharing-content-with-other-clients)
11. [Deactivating Clients and Contacts](#deactivating-clients-and-contacts)
12. [Frequently Asked Questions](#frequently-asked-questions)

---

## What is a Client?

A **Client** represents an organisation the laboratory works for. Every
sample belongs to exactly one client, and the client record acts as a
container for everything related to that organisation:

- the samples registered for the client (see
  [Registering Samples](SampleRegistration.md)),
- the client's **contacts** (people),
- client-specific **batches** (see [Batches](Batches.md)),
- client-specific setup items: sample points, analysis profiles, sample
  templates and analysis specifications,
- file **attachments** and the published **analysis reports**.

Because access rights are granted per client folder, a client contact who
logs into SENAITE only ever sees the content of their own client — never
the samples or reports of other clients.

## Key Concepts

| Term | Meaning |
|------|---------|
| **Client** | An organisation that submits samples to the laboratory. Each client has its own folder with samples, contacts, batches, reports and client-specific setup. |
| **Client ID** | A short, unique identifier for the client. Useful for fast searches and can be included in generated sample IDs. |
| **Contact** | A person belonging to a client. Can be selected as the (primary) contact or as CC contact of a sample, and can optionally be linked to a user account for portal access. |
| **Lab Contact** | A member of the laboratory staff (configured under the lab setup, not inside a client). Not covered in this guide. |
| **Global Contact** | A contact created under *Setup → Contacts* instead of inside a client. Global contacts can be selected on samples of **any** client — handy for e.g. regulators or consultants who work across clients. |
| **CC Contact / CC Emails** | Additional recipients that receive the results report email when a sample is published (see [Publishing Results](ResultsPublication.md)). |
| **Client group** | A user group created per client. Users linked to that client's contacts become members and thereby get access to the client's folder. |
| **`Owner` role** | The local role granted to the client group on its own client folder. Everything a client contact may do inside their client is governed by this role. |
| **`Client` role** | The global role given to members of client groups. It marks the user as a "client user" throughout the system. |
| **`ClientGuest` role** | A local role granted automatically on content that has been *shared* with a client (see [Sharing Content with Other Clients](#sharing-content-with-other-clients)). |

A few structural rules:

- A contact always belongs to exactly one client (or is a global contact).
- A user account can be linked to at most one contact.
- Users with laboratory roles cannot be linked to client contacts — client
  portal accounts are always "plain" users.

## Creating a Client

Clients can be added by users with the `senaite.core: Add Client`
permission (by default Lab Managers and Lab Clerks).

1. Go to the **Clients** listing in the main navigation.
2. Click **Add**.
3. Fill in the client details. **Name** and **Client ID** are required, and
   the Client ID must be unique.
4. Press **Save**.

The client edit form is organised in these tabs:

| Tab | Fields |
|-----|--------|
| **Default** | Name, Client ID, VAT number, Phone, Fax, Email Address, Bulk discount applies, Member discount applies, Attachments (file uploads stored in the client folder) |
| **Address** | Physical address, Postal address, Billing address |
| **Bank details** | Account Type, Account Name, Account Number, Bank name, Bank branch |
| **Preferences** | Primary Contact, CC Contacts, CC Emails, Restrict categories, Default decimal mark, Custom decimal mark |

Notes on the **Preferences** tab:

- **Primary Contact** — the default contact for new samples. If set, it is
  pre-selected automatically in the sample add form.
- **CC Contacts** / **CC Emails** — default CC recipients for new samples
  (see [CC Contacts and CC Emails](#cc-contacts-and-cc-emails)).
- **Restrict categories** — when one or more analysis categories are
  selected, only those categories (and their services) are offered when
  registering samples for this client.
- **Default decimal mark** / **Custom decimal mark** — by default the
  decimal mark configured in the lab setup is used on reports; untick
  *Default decimal mark* to use a client-specific one.

Once saved, the client view shows a set of tabs: **Contacts**, **Samples**,
**Batches**, **Sample Points**, **Analysis Profiles**, **Sample Templates**,
**Analysis Specifications**, **Attachments** and **Analysis Reports**. Lab
managers additionally see **Edit** and **Manage Access**.

## Client Contacts

Contacts are the people of a client. At least one contact is needed before
samples can be registered for the client, because every sample requires a
**Contact** (see [Registering Samples](SampleRegistration.md)).

To create a contact:

1. Open the client and go to the **Contacts** tab.
2. Click **Add**.
3. Fill in the person's details — at minimum the name; add an **email
   address** if the contact should receive notifications and reports.
4. Press **Save**.

The contact form contains:

- personal details: salutation, first name, middle initial/name, surname,
  job title, department;
- contact information: email address, business phone, business fax, home
  phone, mobile phone;
- addresses: physical and postal address;
- **Publication preference → Contacts to CC**: other contacts of the same
  client that should automatically be put in CC when this contact is
  selected on a new sample.

The contacts listing shows Full Name, User Name (filled once a user account
is linked), Email Address and phone numbers, with *Active*, *Inactive* and
*All* filters.

### Global contacts

Contacts can also be created under *Setup → Contacts*. These **global
contacts** do not belong to any client and can be selected as contact or CC
contact on samples of every client. In contact selection fields a small
icon indicates the scope of each entry: client contact, lab contact or
global contact.

## Granting Portal Access to a Contact

A contact only gains access to SENAITE (the "client portal") once it is
linked to a user account. This is done from the contact's **Login details**
tab, which is protected by the permission
`senaite.core: Manage Login Details` (Lab Managers only, by default).

1. Open the contact and go to the **Login details** tab.
2. Either **create a new user** — enter a username, email address and a
   password (minimum 5 characters) — or **search and link an existing
   user** from the list.
3. Press the corresponding button. The contact and the user are now linked
   and the contact's *User Name* is shown in the contacts listing.

When a user is linked to a client contact, the system automatically:

- adds the user to the **client group** of that client (the group is named
  after the Client ID). The group carries the global `Client` role and has
  the `Owner` role on the client folder — this is what grants access;
- copies the user's email address to the contact;
- grants the user the `Owner` role on the contact itself, so the person can
  see and maintain their own contact record.

Restrictions when linking existing users:

- Users that already have laboratory roles (Analyst, Lab Clerk, etc.)
  are not offered — only plain users can become client contacts.
- A user already linked to another contact cannot be linked again.

Use the **Unlink** button on the same tab to revoke access: the user is
removed from the client group and no longer sees the client's data.

## CC Contacts and CC Emails

When results are published, the report email is sent **To** the sample's
contact and **CC** to the sample's CC contacts and CC emails (see
[Publishing Results](ResultsPublication.md)). These recipients can be
prepared at three levels:

| Level | Field | Effect |
|-------|-------|--------|
| **Client** (Preferences tab) | CC Contacts | Default CC contacts, pre-selected for every new sample of this client. |
| **Client** (Preferences tab) | CC Emails | Default plain email addresses to CC on all published samples of this client. |
| **Contact** (Publication preference) | Contacts to CC | Contacts put in CC automatically whenever this contact is chosen as the sample's contact. |
| **Sample** | CC Contact, CC Emails | The actual recipients of this particular sample — editable per sample. |

In the sample add form the cascade works like this:

- Selecting the **Client** pre-fills the *Contact* (the client's Primary
  Contact, or the only contact if there is just one), the *CC Contact*
  (client CC Contacts) and *CC Emails* fields.
- Selecting a **Contact** adds that contact's *Contacts to CC* to the
  *CC Contact* field. Client and contact CC lists are merged without
  duplicates.
- Everything can still be adjusted manually, per sample, before or after
  registration.

CC recipients are used for other client notifications as well:

- **Sample rejection** — if *Email notification on Sample rejection* is
  enabled in the lab setup, the client contact is notified when a sample is
  rejected (see [Rejecting Samples](SampleRejection.md)).
- **Sample invalidation** — the invalidation notice goes to the lab
  managers, the sample contact and the CC contacts (see
  [Sample Lifecycle & Workflow States](SampleWorkflow.md)).
- At publication, the responsible persons of the involved lab departments
  can be added automatically with the setup option **Always send
  publication email to responsibles**.

## Client-Specific Setup

Besides the lab-wide setup (see
[Analysis Services, Profiles & Sample Templates](AnalysesSetup.md)), several
setup items can be created *inside* a client. They then apply to that client
only:

| Client tab | Item | Behaviour |
|------------|------|-----------|
| **Sample Points** | Client-specific sample points | Offered in the add form for this client's samples, in addition to the lab-wide sample points. |
| **Analysis Profiles** | Client-specific profiles | Offered for this client only, in addition to lab-wide profiles. |
| **Sample Templates** | Client-specific templates | Offered for this client only, in addition to lab-wide templates. |
| **Analysis Specifications** | Client-specific result ranges | Offered for this client only, in addition to lab-wide specifications for the same sample type. |
| **Batches** | Client batches | Batches created inside the client (or assigned to it) are listed in the client's **Batches** tab; a *Client Batch ID* can be recorded (see [Batches](Batches.md)). |
| **Attachments** | Files and images | Stored in the client folder for later download. |

In the sample add form, selection fields are filtered accordingly: for a
given client you are offered the lab-wide items **plus** the items of that
client — never the items belonging to other clients. The same client-based
filtering applies when selecting a primary sample for a
[secondary sample](SecondarySamples.md).

## What Client Contacts See and Can Do

Once logged in, client contacts operate inside their own client folder,
with rights defined by the `Owner` role. By default they **can**:

- view the client record and edit its details,
- view, add and edit the client's contacts (but not the *Login details*),
- **register samples** for their client (see
  [Registering Samples](SampleRegistration.md)) — note that samples created
  by client contacts are never received automatically, even if the lab has
  *Auto-receive samples* enabled,
- **cancel** and **reinstate** their own samples, while the sample state
  still permits it,
- create, **close** and **reopen** client batches,
- add client-specific sample points, profiles, templates and
  specifications,
- upload attachments,
- follow the progress of their samples and view/download the published
  **analysis reports** from the *Analysis Reports* tab or the sample view.

They **cannot**:

- see other clients or anything inside them,
- receive, reject, retract, verify or publish samples — these are
  laboratory-side actions (see
  [Sample Lifecycle & Workflow States](SampleWorkflow.md)),
- enter or edit results (see
  [Results Entry & Verification](ResultsEntryAndVerification.md)),
- access [Worksheets](Worksheets.md), [Quality Control](QualityControl.md)
  samples or [Instruments](Instruments.md),
- see samples or partitions flagged for **Internal Use** — flagging a
  sample for internal use revokes client access to it (see
  [Sample Partitions](SamplePartitions.md)),
- see sample partitions at all, unless the lab has enabled **Display sample
  partitions to clients** in the setup.

Laboratory users, in contrast, see all clients in the **Clients** listing —
each row linking straight to the client's landing page — and can work in
any client folder according to their lab role.

## Client Settings

Global behaviour is configured in *Setup → Registry*, fieldset **Client
Settings**:

| Setting | Default | Effect |
|---------|---------|--------|
| `auto_create_client_group` (*Automatically Create Client Group*) | on | Creates the client's user group as soon as the client is created. If disabled, the group is still created the first time a contact is linked to a user account. Disable it only if you never link client contacts to users and want to keep the group list clean. |
| `client_landing_page` (*Client landing page*) | Samples | The page shown when a client user logs in, and the page a client name links to in the Clients listing. |

The landing page can be any of the client tabs: **Contacts**, **Samples**,
**Batches**, **Sample Points**, **Analysis Profiles**, **Sample Templates**,
**Analysis Specifications**, **Attachments** or **Analysis Reports**.

Related options elsewhere in the setup:

- **Display sample partitions to clients** (*Setup*, registry setting
  `show_partitions`) — see
  [Partition Visibility for Clients](SamplePartitions.md#partition-visibility-for-clients).
- **Email notification on Sample rejection** and the rejection email body —
  see [Rejecting Samples](SampleRejection.md).
- **Email body for Sample publication notifications** and **Always send
  publication email to responsibles** — see
  [Publishing Results](ResultsPublication.md).

Client users also get a **My Organization** entry in their user menu, which
takes them directly to their client's landing page.

## Sharing Content with Other Clients

Normally, content inside a client is strictly private to that client. Two
mechanisms allow controlled exceptions:

- **Manage Access** — lab managers can open the *Manage Access* tab of a
  client to grant individual users or groups additional roles on that
  client folder (the standard sharing page). Use this sparingly; the normal
  way to grant access is linking contacts to users.
- **Client share** — some content types (mainly provided by add-ons) offer
  a *Client share* section with a **Clients** field. Users belonging to any
  of the selected clients are granted the `ClientGuest` role on that
  content, so it appears in their searches and listings in read-only
  fashion, even though it lives outside (or inside another) client.

## Deactivating Clients and Contacts

Clients and contacts are never deleted through the UI; they are
**deactivated** instead, so historical samples and reports stay intact.

- **Deactivate a client**: select it in the Clients listing and use the
  **Deactivate** button (or the action in the client view). An inactive
  client is hidden from its client contacts entirely — their users may
  still be able to log in, but they will not see any data until the client
  is activated again with **Activate** (from the *Inactive* filter of the
  Clients listing).
- **Deactivate a contact**: from the client's Contacts listing (lab
  personnel only). An inactive contact can no longer be selected on new
  samples; existing samples keep their reference. If the contact had a
  linked user, unlink it from the *Login details* tab to revoke the login
  as well.

## Frequently Asked Questions

**Why can't my client contact see anything after logging in?**
Check three things: the user is actually **linked** to a contact
(*Login details* tab), the **client is active**, and the **contact is
active**. A user account alone, without a linked contact, has no access to
any client data.

**Can a contact belong to more than one client?**
No. A contact lives inside exactly one client, and a user account can be
linked to only one contact. If one person needs to receive reports for
several clients, add their address to the **CC Emails** of each client, or
create a **global contact** (*Setup → Contacts*) and select it as CC
contact on the relevant samples.

**Who receives the results report when a sample is published?**
The email goes to the sample's **Contact**, with the sample's **CC
Contacts** and **CC Emails** in copy. Department responsibles are added if
*Always send publication email to responsibles* is enabled. See
[Publishing Results](ResultsPublication.md).

**Can client contacts register their own samples?**
Yes — that is one of the main purposes of the client portal. Samples
created by client contacts always arrive in the lab as due samples: they
are excluded from the *Auto-receive samples* option, so the lab always
performs the *Receive* step itself.

**Can a client contact cancel a sample?**
Yes. Client contacts may cancel (and reinstate) samples of their own
client, as long as the sample's workflow state still allows cancellation.

**Why doesn't my client see one of their samples?**
The sample is probably flagged for **Internal Use**, which removes client
access, or it is a **partition** and *Display sample partitions to clients*
is disabled. See [Sample Partitions](SamplePartitions.md).

**Why can't I link a lab technician's user account to a client contact?**
By design. Only users without laboratory roles can be linked to client
contacts; otherwise lab-wide privileges would leak into the client portal.
Laboratory staff are managed as Lab Contacts instead.

**How do I limit which analyses a client can request?**
Set **Restrict categories** in the client's *Preferences*: only the
selected analysis categories are offered when registering samples for that
client. Client-specific profiles and templates (see
[Analysis Services, Profiles & Sample Templates](AnalysesSetup.md)) help
standardise what is requested.

**What is the client group for?**
Each client has a user group (named after its Client ID). Members of the
group get the `Client` role plus the `Owner` role on the client folder,
which is what grants them access. Users are added to the group
automatically when linked to a contact — you normally never manage these
groups by hand.
