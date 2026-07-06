# Dashboard & Reports

SENAITE gives laboratory staff several ways to keep an eye on the work and
the history of the system: the **System Dashboard** with live panels and
evolution charts for samples, analyses and worksheets; a built-in **Reports**
section with productivity and administrative reports (available in older
versions, see the version note below); the **Audit Log**, which records
every change made to the objects in the system; and a **Global Search** for
finding any record from anywhere.

This guide explains the feature from an end-user perspective: what each
dashboard panel and report shows, how to filter and export them, and how to
configure who sees what.

---

## Table of Contents

1. [The System Dashboard](#the-system-dashboard)
2. [Key Concepts](#key-concepts)
3. [Dashboard Panels](#dashboard-panels)
4. [Time Filters and Evolution Charts](#time-filters-and-evolution-charts)
5. [Dashboard Visibility and Personal Filters](#dashboard-visibility-and-personal-filters)
6. [The Reports Section](#the-reports-section)
7. [Running and Exporting a Report](#running-and-exporting-a-report)
8. [Productivity Reports](#productivity-reports)
9. [Administrative Reports](#administrative-reports)
10. [Report History](#report-history)
11. [The Audit Log](#the-audit-log)
12. [Global Search](#global-search)
13. [Frequently Asked Questions](#frequently-asked-questions)
14. [Related Guides](#related-guides)

---

## The System Dashboard

The **System Dashboard** is the laboratory's home screen. It summarises, at
a glance, how much work is in each stage of the pipeline — samples waiting
for reception, analyses waiting for results, worksheets waiting for
verification — and how the workload has evolved over time.

Where you see it:

- When **Use Dashboard as default front page** is enabled (*Setup →
  Appearance*, enabled by default), the dashboard is what laboratory users
  see right after logging in.
- A **Switch to frontpage** link at the top right takes you to the classic
  front page instead; from the front page you can navigate back to the
  dashboard at any time.
- **Client contacts never see the laboratory dashboard** — when a client
  user logs in, they are redirected to their own client area (see
  [Clients & Contacts](ClientsAndContacts.md)). Anonymous visitors are sent
  to the public front page (or to the configured **Landing Page**).

## Key Concepts

| Term | Meaning |
|------|---------|
| **Section** | A block of the dashboard dedicated to one kind of object: *Samples*, *Analyses* or *Worksheets*. |
| **Panel** | A single counter inside a section, e.g. *"Samples to be verified"*. Shows the number of items in that state, and what percentage of the total they represent. |
| **Evolution chart** | A stacked bar chart per section showing how many items were created per period, coloured by their current workflow state. |
| **Periodicity** | The time grouping of the evolution charts: Daily, Weekly, Monthly, Quarterly, Biannual or Yearly. |
| **Panels visibility** | A per-role setting that controls which user roles can see each dashboard section. Stored in the registry record `senaite.core.dashboard_panels_visibility`. |
| **Dashboard filter cookie** | A browser cookie (`dashboard_filter_cookie`) that remembers, per section, whether the counters consider *all* items or only *yours* (items you created). |

## Dashboard Panels

Each section shows one panel per workflow stage. Every panel displays:

- the **number** of items currently in that state,
- a legend *"of N (x%)"* — the total of active items and the percentage the
  panel represents,
- a small vertical progress bar visualising that percentage.

Clicking a panel in the *Samples* section takes you to the samples listing
pre-filtered by the corresponding status.

**Samples section:**

| Panel | Shows |
|-------|-------|
| Samples to be sampled | Samples awaiting the sampling step. Only shown when the **Sampling Workflow** is enabled in Setup. |
| Samples to be preserved | Samples awaiting preservation. Only with the sampling workflow enabled. |
| Samples scheduled for sampling | Samples with a scheduled sampling date. Only with the sampling workflow enabled. |
| Samples to be received | Registered samples whose physical reception is pending. |
| Samples with results pending | Received samples still waiting for results. |
| Samples to be verified | Samples with all results submitted, awaiting verification. |
| Samples verified | Verified samples, ready to be published. |
| Samples published | Samples whose results reports have been published. |
| Samples to be printed | Published samples whose report has not been printed yet. Only shown when the **Printing Workflow** is enabled in Setup. |

**Analyses section:**

| Panel | Shows |
|-------|-------|
| Assignment pending | Analyses not yet assigned to any worksheet. |
| Results pending | Analyses (assigned or not) still waiting for a result. |
| To be verified | Analyses with submitted results awaiting verification. |
| Verified | Verified analyses. |

**Worksheets section:**

| Panel | Shows |
|-------|-------|
| Results pending | Open worksheets. |
| To be verified | Worksheets with all results submitted. |
| Verified | Verified worksheets. |

See [Sample Lifecycle & Workflow States](SampleWorkflow.md),
[Worksheets](Worksheets.md) and
[Results Entry & Verification](ResultsEntryAndVerification.md) for what each
of these states means and how items move between them.

## Time Filters and Evolution Charts

Each section also carries an **evolution chart** (*Evolution of Samples*,
*Evolution of Analyses*, *Evolution of Worksheets*):

1. Click **Show/hide timeline summary** at the top of a section. The
   counters are replaced by a stacked bar chart (click again to switch
   back — the choice is remembered per browser).
2. Each bar represents one period (day, week, month, …) based on the
   **creation date** of the items; the coloured segments show the current
   workflow state of the items created in that period (Reception pending,
   Results pending, To be verified, Verified, Published, Rejected,
   Invalid, …), with a colour legend on the right.
3. Hovering over a segment shows the exact count.

Above the chart, a **time selector** changes the periodicity. Each option
also determines how far back the chart looks:

| Option | Bars grouped by | Date range loaded |
|--------|-----------------|-------------------|
| Daily | day | last 30 days |
| Weekly *(default)* | week | last six months |
| Monthly | month | last 2 years |
| Quarterly | quarter | last 4–5 years |
| Biannual | half-year | last 10–11 years |
| Yearly | year | last 15–16 years |

The date range in effect is displayed under the chart title. Because
computing the charts is expensive, the underlying data is cached and
**updated every 2 hours** (also indicated next to the date range) — recent
changes may take up to two hours to show in the charts, while the panel
counters are always current.

## Dashboard Visibility and Personal Filters

**Who sees which sections** is controlled per user role:

- The visibility matrix is stored in the registry record
  `senaite.core.dashboard_panels_visibility`, with one entry per section
  (`analyses`, `analysisrequests`, `worksheets`) listing every role and a
  yes/no flag.
- By default, only users with the **LabManager** or **Manager** role see
  the dashboard sections; those two roles can never be switched off.
- Older versions exposed a *Visibility* drop-down on each section header
  where a lab manager could tick/untick roles directly on the dashboard;
  in current versions these on-page controls are hidden, and the registry
  record is the single place where the matrix lives (a site administrator
  can edit it through the registry). See
  [User Roles & Permissions](UserRolesAndPermissions.md) for the role
  system in general.

**"All" vs "Mine":** each section supports a personal filter that restricts
the counters and charts to the items *you* created, instead of all items in
the laboratory. The selection is stored per section in the
`dashboard_filter_cookie` browser cookie and defaults to *All*. As with the
visibility controls, the on-page selector is hidden in current versions, so
the dashboard effectively shows *All* items.

## The Reports Section

> **Version note:** the built-in *Reports* section described in the
> following sections is part of SENAITE up to and including version
> **2.4.x**. It was **removed from the core product in version 2.5.0**.
> On newer installations, equivalent (and richer) reporting is typically
> provided by add-ons, or the data can be exported directly from the
> listings. The Quality Control reports of very old versions were removed
> even earlier and never returned to core; QC evaluation is done through
> the QC charts instead (see [Quality Control](QualityControl.md)).

In versions that include it, the **Reports** entry in the main navigation
opens a section with three tabs:

| Tab | Contents | Who sees it |
|-----|----------|-------------|
| **Productivity** | Sample, analyses and other productivity reports. | All lab users; client contacts see a reduced set restricted to their own data. |
| **Administration** | Administrative/traceability reports. | Users allowed to modify content (lab managers). |
| **History** | Archive of every PDF report generated so far. | All lab users; client contacts only see their own reports. |

## Running and Exporting a Report

All reports follow the same pattern:

1. Open **Reports** and pick the *Productivity* or *Administration* tab.
2. Reports are listed by group, each with a one-line description. Click a
   report's name to unfold its **filter form**.
3. Fill in the filters. Almost every report offers:
   - one or more **date ranges** (*From* / *to* date pickers) on the date
     relevant to the report (received, requested, published, created, …).
     You can fill only *From* (everything after), only *to* (everything
     before), or both;
   - an **Output format** selector: **PDF** or **CSV**;
   - report-specific selectors (Client, Analysis state, Analyst,
     Instrument, Group by period, …) described per report below. Leaving a
     selector blank means "all".
4. Press **Generate report**.

What happens then depends on the output format:

- **PDF** — the report is rendered with the laboratory letterhead (lab
  name, address, logo), the name and signature of the user who generated
  it, and the filter criteria used. The PDF is downloaded **and** stored
  in the [Report History](#report-history) for later retrieval.
- **CSV** — a plain data file is downloaded immediately. CSV outputs are
  *not* kept in the history.

## Productivity Reports

### Sample related reports

| Report | What it shows | Filters |
|--------|---------------|---------|
| **Daily samples received** | Lists all samples received in a date range. | Date Received; Output format |
| **Samples received vs. samples reported** | For each period, the number of samples received and the number of samples reported (published), with the difference between the two. | Date Received; Output format |

### Analyses related reports

| Report | What it shows | Filters |
|--------|---------------|---------|
| **Analyses per service** | The number of analyses requested per analysis service. | Client; Date Requested; Date Published; Analysis state; Output format |
| **Analyses per sample type** | The number of analyses requested per sample type. | Client; Date Requested; Analysis state; Output format |
| **Samples and analyses per client** | The number of samples and analyses per client. (For client contacts the report is titled *Samples and analyses* and is limited to their own data.) | Client; Date Requested; Analysis state; Output format |
| **Analysis turnaround time** | Turnaround statistics for **published** analyses, grouped by analysis category and service: total count, how many finished late/early/on time, and the average lateness/earliness (in days/hours/minutes), with category and grand totals. | Client; Date Received; Output format |
| **Analysis turnaround time over time** | The turnaround time of selected analyses plotted over time, so trends become visible. | Analysis service(s) (multi-select); Analyst; Instrument; Period (Day / Week / Month); Date Received; Output format |
| **Analyses summary per department** | The number of analyses requested and published per lab department, expressed as a percentage of all analyses performed. | Date Requested; Analysis state; Group by (Day / Week / Month / Year); Output format |
| **Analyses performed and published as % of total** | Per period, the number of analyses published expressed as a percentage of all analyses performed. | Date Requested; Group by (Day / Week / Month / Year); Output format |

Turnaround times are only meaningful if your analysis services define a
**maximum turnaround time** — see
[Analysis Services, Profiles & Sample Templates](AnalysesSetup.md).

### Other productivity reports

| Report | What it shows | Filters |
|--------|---------------|---------|
| **Attachments** | The attachments linked to samples and analyses in the period. | Client; Date Loaded; Output format |
| **Data entry day book** | One line per sample created in the period: dates created / received / published, batch, sample type, number of analyses, client, creator and remarks — plus totals with received/created, published/created and published/received ratios. | Date Created; Output format |

## Administrative Reports

| Report | What it shows | Filters |
|--------|---------------|---------|
| **Samples not invoiced** | Published samples that have not been invoiced yet. | Date Published; Analysis state; Output format |
| **User history** | The actions performed by all users — or one specific user — in a period of time. Useful for traceability audits. | Modification date; User; Output format |

For deeper, field-level traceability, prefer the
[Audit Log](#the-audit-log), which records every single change.

## Report History

The **History** tab of the Reports section keeps every PDF report ever
generated:

- Columns: **Title**, **Size**, **Created**, **By** (creator) and — for lab
  users — **Client**.
- Click a report's title to download the stored PDF again.
- Lab users see all reports; a client contact only sees reports that belong
  to their client.

## The Audit Log

SENAITE keeps a complete change history — a **snapshot** per modification —
for samples, analyses, worksheets, batches, clients, setup items and most
other objects. There are two ways to consult it.

### The Audit Log of a single object

1. Open the object (a sample, a worksheet, a client, an instrument, …).
2. Open the **Audit Log** tab/action of that object.

You get a listing with one row per version (5 per page, newest first):

| Column | Content |
|--------|---------|
| Version | Sequential version number (0 = creation). |
| Date Modified | When the change happened. |
| Actor | The user account that made the change. |
| Fullname | The full name of that user. |
| Roles | The roles the actor had at that moment (hidden by default, enable via the column toggle). |
| Remote IP | The network address the change came from. |
| Action | What was done: Create, Edit, or the workflow transition (Receive, Submit, Verify, Publish, …). |
| Workflow State | The state after the change; state changes are displayed as *old → new*. |
| Changes | A field-by-field diff of exactly which values changed, from what to what. |
| Snapshot | The complete raw snapshot data (hidden by default). |

Viewing audit logs requires the permission
`senaite.core: View Log Tab`, granted by default to lab managers only (see
[User Roles & Permissions](UserRolesAndPermissions.md)).

### The global Audit Log

The global Audit Log gathers the latest change of **every** object in the
system in one searchable place:

1. Open the **user menu** (your name, top right) and choose **Audit Log**.
2. A listing appears (25 rows per page) with the same columns as above plus
   a **Title** column; the title links to the full per-object audit log.
   The listing is sortable and searchable — e.g. type a user name or a
   sample ID to find all recent changes related to it.

The global log depends on a dedicated index, which is **disabled by
default** because it adds overhead every time an object is created or
modified. To enable it:

1. Go to *Setup* and open the **Security** settings.
2. Tick **Enable global Audit Log** and save.

Notes:

- While the global option is disabled, per-object snapshots are still
  recorded, so the **Audit Log tab of each object keeps working** and no
  history is lost — only the global, cross-object listing stays empty (a
  warning banner *"Global Audit Log is disabled"* with a link to the Setup
  is shown on it).
- Changes made while the global option was off are indexed again after it
  is re-enabled (a reindex of existing objects may be needed for the
  global listing to be complete).

## Global Search

Besides the dashboards and logs, SENAITE offers a **global search** that is
available from every page — handy whenever you know *what* you are looking
for (a sample ID, a client name, a batch, …) but not *where* it lives:

- Click the **magnifier icon** at the top right of the toolbar, or
- press **Ctrl+Space** anywhere in the application (*"Press Ctrl+Space to
  trigger the Spotlight search"*, as the icon's tooltip says).

A search box opens in which you simply start typing. Matching records from
across the whole system — samples, clients, batches, worksheets, contacts,
setup items and so on, limited to what your role is allowed to see — appear
as you type, and clicking a result takes you straight to it. The search is
only offered to logged-in users.

The search interface itself is provided by the **senaite.app.spotlight**
add-on. It is a required part of the product and installed automatically
with every standard SENAITE installation, so no extra setup is needed; the
exact behaviour and result presentation may evolve with that add-on
independently of the core version.

**Global search vs. listing filters:** the search field at the top of a
listing (samples, batches, worksheets, …) only *filters the rows of that
listing*, within the currently selected state tab. The global search spans
all object types at once and works from any page. Use the global search to
**find and open** a specific record; use the listing filters to **narrow
down** a working list you then act on (select, transition, print, …).

## Frequently Asked Questions

**Why don't I see the dashboard after logging in?**
Either **Use Dashboard as default front page** is disabled in *Setup →
Appearance*, or your user role is not enabled in the dashboard panels
visibility registry record (by default only lab managers see the panels),
or you are logged in as a client contact — client users are always taken
to their client area instead.

**The evolution chart doesn't show the sample I just registered.**
Chart data is cached and refreshed every 2 hours. The numeric panels,
however, are always up to date.

**Do the dashboard counters include cancelled or rejected samples?**
No. The panels count active items only. Rejected and invalid items do
appear in the evolution charts, though, as their own coloured segments.

**Where did the Reports section go after upgrading?**
The legacy Productivity/Administration reports were removed from the core
product in version 2.5.0. Previously generated PDFs are ordinary content
and remain accessible; for new reporting needs use an add-on or export
listings to CSV/XLSX.

**Can a client contact run reports?**
In versions with the Reports section, yes — client contacts get a reduced
productivity report list, automatically limited to their own samples and
analyses, and a history restricted to their own reports.

**Is the audit trail lost when the global Audit Log is disabled?**
No. Snapshots are always taken per object; disabling the global option
only stops the central indexing that powers the cross-object listing.

**Can I export the audit log?**
There is no built-in export for the audit log listing. For a printable
summary of user actions over a period, versions up to 2.4.x offer the
*User history* administrative report.

**Who can see the Audit Log?**
Users with the `senaite.core: View Log Tab` permission — lab managers by
default. Client contacts cannot see audit logs.

## Related Guides

- [Sample Lifecycle & Workflow States](SampleWorkflow.md) — the states
  behind every dashboard panel and chart segment.
- [Worksheets](Worksheets.md) — the worksheet states summarised on the
  dashboard.
- [Results Entry & Verification](ResultsEntryAndVerification.md) — how
  analyses progress from *Results pending* to *Verified*.
- [Publishing Results](ResultsPublication.md) — the per-sample analysis
  reports sent to clients (not to be confused with the lab-wide reports
  covered here).
- [Quality Control](QualityControl.md) — QC samples and control charts.
- [Sample Rejection](SampleRejection.md) — rejection workflow and the
  per-sample rejection notifications.
- [Clients & Contacts](ClientsAndContacts.md) — what client users see
  instead of the dashboard.
- [User Roles & Permissions](UserRolesAndPermissions.md) — roles referenced
  by the visibility and audit log permissions.
- Back to the [User Guide index](UserGuide.md).
