# Instruments & Calibration

Instruments represent the physical analytical devices of your laboratory.
SENAITE keeps a complete quality record for each instrument — calibration
certificates, calibration and validation downtimes, maintenance tasks and QC
test results — and uses that record to decide whether the instrument may be
used to capture analysis results. Instruments can also import result files
automatically, so analysts don't have to type results by hand.

This guide explains the feature from an end-user perspective: how to register
instruments, how calibration certificates and validity work, how out-of-date
instruments are flagged and kept away from results entry, how instruments
relate to methods and analysis services, and how the results import framework
works.

---

## Table of Contents

1. [What is an Instrument?](#what-is-an-instrument)
2. [Key Concepts](#key-concepts)
3. [Prerequisites and Permissions](#prerequisites-and-permissions)
4. [Registering an Instrument](#registering-an-instrument)
5. [Calibration Certificates](#calibration-certificates)
6. [Calibrations](#calibrations)
7. [Validations](#validations)
8. [Maintenance and Scheduled Tasks](#maintenance-and-scheduled-tasks)
9. [When is an Instrument Valid?](#when-is-an-instrument-valid)
10. [Out-of-Date Warnings and How Results are Blocked](#out-of-date-warnings-and-how-results-are-blocked)
11. [Linking Instruments to Methods and Analysis Services](#linking-instruments-to-methods-and-analysis-services)
12. [Instrument QC — Internal Calibration Tests](#instrument-qc--internal-calibration-tests)
13. [Importing Results from Instruments](#importing-results-from-instruments)
14. [Automatic Results Import](#automatic-results-import)
15. [Frequently Asked Questions](#frequently-asked-questions)

---

## What is an Instrument?

An instrument in SENAITE is more than a name in a list. Each instrument is a
small dossier that groups, in tabs on the instrument's own page:

- its identification (type, manufacturer, supplier, model, serial number,
  asset number, location, photo, installation details),
- its **Calibration Certificates** and their validity dates,
- its **Calibrations** and **Validations** (periods during which the
  instrument is down and unavailable),
- its **QC Results** (control and blank measurements performed on the
  instrument),
- its **Documents** (manuals, SOPs and other files),
- its **Auto-Import Logs** (history of automatic result imports).

The system continuously evaluates this dossier: only *valid* instruments are
offered to analysts when capturing results (see
[When is an Instrument Valid?](#when-is-an-instrument-valid)).

## Key Concepts

| Term | Meaning |
|------|---------|
| **Instrument Type** | A category of instruments (e.g. balance, spectrometer), maintained under *Setup → Instrument Types*. Every instrument must have one. |
| **Manufacturer** | The company that built the instrument (shown as *Brand* in listings), maintained under *Setup → Manufacturers*. |
| **Supplier** | The vendor the instrument was purchased from, maintained under *Setup → Suppliers*. Suppliers also hold supplier contacts and reference samples (see [Quality Control](QualityControl.md)). |
| **Calibration Certificate** | The proof that the instrument was calibrated, with a *Valid from* / *Valid to* date range. An instrument with no currently valid certificate is **out of date**. |
| **Calibration** | A period during which the instrument is away being calibrated (down from / down to). While in progress, the instrument is unavailable. |
| **Validation** | A period during which the instrument is being validated by lab personnel. While in progress, the instrument is unavailable. |
| **Maintenance Task** | A record of preventive maintenance, repair or enhancement work, with cost and status. Informational — it does not make the instrument invalid. |
| **Internal Calibration Test / QC Results** | Control and blank analyses measured on the instrument. If the latest batch is out of range, the instrument fails QC. |
| **Import Interface** | A driver that understands a specific instrument's results file format, used to import results manually or automatically. |

## Prerequisites and Permissions

- Instruments live under *Setup → Instruments*. Adding one is protected by
  the permission `senaite.core: Add Instrument`, granted by default to Lab
  Clerk, Lab Manager and Manager.
- **Instrument Type**, **Manufacturer** and **Supplier** are mandatory when
  registering an instrument — create them first under *Setup → Instrument
  Types*, *Setup → Manufacturers* and *Setup → Suppliers*. An optional
  **Location** can be prepared under *Setup → Instrument Locations*.
- Importing instrument results requires the permission
  `senaite.core: Import Instrument Results`, granted by default to Analyst,
  Lab Manager and Manager. The **Import** entry only appears in the top-right
  user menu for users holding this permission.
- Like most setup items, instruments can be **deactivated** instead of
  deleted; inactive instruments no longer take part in results capture or
  auto-import.

## Registering an Instrument

1. Go to *Setup → Instruments* and press **Add**.
2. Fill in the identification fields:

   | Field | Notes |
   |-------|-------|
   | **Name** / Description | The instrument's display name. |
   | **Asset Number** | The instrument's ID in the lab's asset register. |
   | **Instrument type** | Required. Select from the registered instrument types. |
   | **Manufacturer** | Required. Shown as *Brand* in the instruments listing. |
   | **Supplier** | Required. The vendor of the instrument. |
   | **Model** | The instrument's model number. |
   | **Serial No** | The serial number that uniquely identifies the instrument. |
   | **Supported methods** | The analytical methods this instrument can perform (see [Linking Instruments to Methods and Analysis Services](#linking-instruments-to-methods-and-analysis-services)). |
   | **De-activate until next calibration test** | If ticked, the instrument is unavailable until a valid calibration test (QC result) is recorded. The box unticks itself automatically. |
   | **Data Interface** | Export interface for the instrument (worksheet export). |
   | **Import Data Interface** | One or more import interfaces used to read the instrument's results files. |
   | **Result files folders** | For each import interface, a folder where the system looks for results files during [automatic import](#automatic-results-import). |

3. On the *Procedures* tab you can record the **In-lab calibration
   procedure** and the **Preventive maintenance procedure** — free-text
   instructions intended for analysts.
4. On the *Additional info* tab you can set the **Location**, upload a
   **Photo** and an **Installation Certificate**, and record the
   **Installation Date**.
5. Save. The instrument now appears in the instruments listing with columns
   *Instrument*, *Type*, *Brand*, *Model*, *Expiry Date*, *Weeks To Expire*
   and *Methods*.

Note that a freshly registered instrument has **no calibration certificate
yet and is therefore considered out of date** — add a certificate right away
if the instrument should be usable.

## Calibration Certificates

Open an instrument and go to its **Calibration Certificates** tab to manage
certificates. Each certificate records:

| Field | Notes |
|-------|-------|
| **Certificate Code** | The certificate's identifying code (its title). |
| **Internal Certificate** | Tick for an in-house calibration certificate; the *Agency* field is then not applicable. |
| **Agency** | Organization responsible of granting the calibration certificate (external certificates). |
| **Date granted** | When the certificate was issued. |
| **Interval** | Optional expiration interval in days (with shortcuts for daily, weekly, monthly, quarterly, biannually, yearly). When set, the *Valid to* date is recalculated from *Valid from* on every save. |
| **Valid from** / **Valid to** | Required. The date range in which the certificate is valid. |
| **Prepared by** / **Approved by** | The persons (lab or supplier contacts) who prepared and approved the certificate. |
| **Report upload** | The certificate document (downloadable from the listing). |
| **Remarks** | Free text. |

How validity is evaluated:

- A certificate is **valid** when today falls between its *Valid from* and
  *Valid to* dates.
- An instrument may hold several certificates; the one with the **most
  remaining days** counts. Its *Valid to* date is shown as the instrument's
  *Expiry Date*, and the time left as *Weeks To Expire*.
- If **no certificate is currently valid**, the instrument is **out of
  date**. Expired certificates are marked with an *Out of date* warning icon
  in the certificates listing, and the instruments listing shows *Out of
  date* in the *Weeks To Expire* column.

## Calibrations

The **Calibrations** tab records periods during which the instrument is away
for calibration:

- **Task ID** (title), **Report Date** and **Report ID** identify the
  calibration job and its report.
- **From** / **To** define the downtime: from the date the instrument goes
  down until the date it becomes available again (shown as *Down from* /
  *Down to* in the listing).
- **Calibrator** and **Performed by** record who did the work;
  **Considerations**, **Work Performed** and **Remarks** document it.

While today is inside the *From*/*To* range, the instrument is **under
calibration** and is not offered for results capture. Calibrations can be
scheduled in advance — a calibration dated in the future has no effect until
its start date arrives. If several calibrations overlap, the instrument stays
down until the latest end date.

## Validations

The **Validations** tab works exactly like calibrations, but for validation
work performed by the lab (fields: *Task ID*, *Report Date*, *From*, *To*,
*Validator*, *Considerations*, *Work Performed*, *Performed by*, *Report ID*,
*Remarks*). While a validation is in progress, the instrument is **under
validation** and unavailable for results capture.

## Maintenance and Scheduled Tasks

Instruments can also carry **maintenance tasks**: records of *Preventive*,
*Repair* or *Enhancement* work with a downtime range (*Down from* / *Down
to*), the responsible *Maintainer*, descriptions of the work and its
*Price*. A task moves through the informal states *In queue* (before its
start date), *Pending* (started), *Overdue* (past its end date without being
closed) and *Closed* (ticked as done); tasks can also be cancelled.

A companion **Schedule** listing allows recurring task definitions
(calibration, validation, preventive, repair, enhancement) with repetition
criteria.

Two things to keep in mind:

- Maintenance tasks are **informational**: unlike calibrations and
  validations, they do *not* make the instrument invalid for results entry.
- In current SENAITE versions the *Maintenance* and *Schedule* tabs are
  hidden from the instrument page by default; the calibration, certification
  and validation tabs are the primary quality-control tools.

## When is an Instrument Valid?

An instrument is considered **valid** — and therefore selectable when
capturing results — only when *all* of the following hold:

1. It is **not out of date**: it has a calibration certificate that is valid
   today.
2. Its **latest QC test batch passed**: the most recent group of control or
   blank analyses measured on the instrument was within the specification
   range (an instrument with no QC analyses at all passes this check).
3. It is **not flagged** *De-activate until next calibration test*.
4. **No validation is in progress.**
5. **No calibration is in progress.**

## Out-of-Date Warnings and How Results are Blocked

**Dashboard warnings.** Lab Managers see dismissible warning banners on the
dashboard and front page whenever instruments are in trouble, grouped by
cause:

- *Instruments' calibration certificates expired* — links to the
  certificates tab,
- *Instruments disabled until successful calibration* — the last QC test
  failed,
- *Instruments disposed until new calibration tests being done* — flagged
  *De-activate until next calibration test*,
- *Instruments in validation progress*,
- *Instruments in calibration progress*.

**Results entry.** When entering results on a sample or a
[worksheet](Worksheets.md), the *Instrument* selector of each analysis only
offers **valid** instruments:

- Instruments that are **out of date** still appear, but greyed out and
  suffixed with **"(Out of date)"** — they cannot be selected.
- Instruments under calibration or validation, that failed QC, or that are
  flagged for de-activation do not appear at all.
- Exception: for **QC analyses** (controls and blanks) all instruments remain
  selectable, including invalid ones — otherwise it would be impossible to
  record the very calibration tests that bring an instrument back into
  service.

**Worksheet warning.** If a worksheet contains analyses whose assigned
instrument has become invalid, the results view displays the warning *"Some
analyses use out-of-date or uncalibrated instruments. Results edition not
allowed"*, listing the affected analyses and instruments.

**Automatic retraction on QC failure.** If a QC analysis performed with an
instrument is submitted with an **out-of-range** result, the system reacts
immediately:

- all routine and duplicate analyses measured on that same instrument that
  are awaiting verification are **automatically retracted** (see
  [Results Entry and Verification](ResultsEntryAndVerification.md)),
- each retracted analysis gets the remark *"Instrument failed reference
  test"* with a timestamp,
- a PDF report listing the retracted analyses is generated, attached to the
  failed QC analysis (visible in the instrument's QC Results listing under
  *Retractions*), and sent out by email,
- the instrument itself fails the QC validity check and disappears from the
  instrument selectors until a passing QC batch is recorded.

Conversely, submitting an **in-range** QC result for the instrument clears
the *De-activate until next calibration test* flag automatically.

## Linking Instruments to Methods and Analysis Services

Instruments, methods and analysis services form a chain (see
[Analyses Setup](AnalysesSetup.md)):

- On the **instrument**, the *Supported methods* field lists the analytical
  methods the instrument can perform.
- On the **method**, the *Instruments* picklist shows the same relationship
  from the other side (*"Instruments supporting this method"*).
- On the **analysis service** (*Method* fieldset), you select the *Methods*
  of the service, the *Instruments* available for it (*"Available
  instruments based on the selected methods"*) and optionally a **Default
  Instrument** used to pre-fill results entry.

During results capture, the instruments offered for an analysis are those
selected on its service, restricted to the ones that support the analysis'
current method — and, as described above, filtered by validity.

## Instrument QC — Internal Calibration Tests

The **QC Results** tab of an instrument (titled *Internal Calibration
Tests*) lists every control and blank analysis measured on the instrument,
together with a trend chart of the results over time. Each row shows the QC
Sample ID, the reference sample it came from, the result and, when the
instrument failed, a link to the *Retractions* report.

QC analyses reach this listing in two ways:

1. **Through worksheets** — blank and control reference analyses added to a
   [worksheet](Worksheets.md) and measured with this instrument. This is the
   regular QC route; see the [Quality Control](QualityControl.md) guide for
   reference samples and definitions.
2. **Directly, without a worksheet** — when a results file imported for the
   instrument references a **Reference Sample ID** instead of a regular
   sample ID, the system automatically creates an *Internal Calibration
   Test* (a reference analysis) linked to the instrument and stores the
   imported result there. Recording such tests also clears the *De-activate
   until next calibration test* flag.

The instrument passes QC while the **latest batch** of its QC analyses (the
analyses captured together for the same reference sample) is fully within
range. One single out-of-range result in that batch makes the instrument
fail QC and triggers the retraction mechanism described above.

## Importing Results from Instruments

Results files produced by instruments can be imported manually:

1. Open **Import** from the top-right user menu (requires the
   `senaite.core: Import Instrument Results` permission) and stay on the
   **Instrument Import** tab.
2. Choose the **instrument**. Only instruments that have an *Import Data
   Interface* configured are listed.
3. Choose the **import interface**. SENAITE ships interfaces for many
   manufacturers (Abaxis, Abbott, Beckman Coulter, FOSS, Horiba, Roche
   Cobas, Shimadzu, Sysmex, Thermo Scientific and more) plus generic
   formats such as a two-dimensional CSV.
4. Select the results **file** and, under *Advanced options*:

   | Option | Choices |
   |--------|---------|
   | **Samples state** | *Received* (default) or *Received and to be verified* — which samples the importer may write into. |
   | **Results override** | *Don't override results* (default), *Override non-empty results*, or *Override non-empty results (also with empty)*. |

5. Press **Submit**. The import report lists infos, warnings and errors —
   e.g. records skipped because no matching sample or analysis was found.

What the importer does with each record:

- It matches the record identifier against **Sample IDs** and **Client
  Sample IDs** (see [Sample Registration](SampleRegistration.md)), and also
  against QC analyses and their group IDs; if nothing matches, it tries
  **Reference Sample IDs** and creates internal calibration tests as
  described above.
- Only analyses in the states *unassigned*, *assigned* or *to be verified*
  are updated, and only within samples in the allowed states.
- Results, interim (intermediate) values, the capture date and other
  supported fields are written; analyses whose result comes from a
  **calculation** are not overwritten — they are recalculated once their
  dependencies have been imported.
- By default each updated analysis is **submitted** automatically, moving it
  along the [sample workflow](SampleWorkflow.md). This is controlled by the
  registry setting `import_analysis_submit`; disable it if analysts should
  review and submit imported results manually.
- If the registry setting `import_analysis_attach_importfile` is enabled,
  the results file is attached to the worksheet and to each
  worksheet-assigned analysis it updated.

Note that the importer does not re-check the instrument's calibration state:
keep an eye on the dashboard warnings so that files from an out-of-date
instrument are not imported unnoticed.

## Automatic Results Import

Imports can also run unattended:

1. On the instrument, select one or more **Import Data Interfaces** and, in
   **Result files folders**, assign each interface a folder on the server
   where the instrument (or a transfer job) drops its results files. Using
   one folder per instrument and interface keeps things tidy.
2. Arrange for the `auto_import_results` address of your SENAITE site to be
   called periodically — typically via a cron job, as shown on the **Auto
   Import** tab of the Import page. The same tab offers a **Manually
   trigger auto import** button for on-demand runs.
3. On each run, the system scans every active instrument's folders, imports
   any **new** files (already-processed files are remembered in an index
   file inside the folder and skipped), and by default does not override
   existing results.

Every automatic run is recorded:

- The **Auto-Import Logs** tab of each instrument keeps a per-instrument
  history with *Time*, *Interface*, *Imported File* and the full import
  *Results* messages.
- The **Show last auto import logs** button on the Import page shows the
  same history across all instruments.
- A plain-text log file is also written into each import folder.

## Frequently Asked Questions

**I registered a new instrument but analysts cannot select it. Why?**
Most likely it has no valid calibration certificate yet — instruments
without one are treated as out of date. Also check that the instrument is
selected on the analysis service (and supports the analysis' method), and
that no calibration or validation is currently in progress.

**Can an instrument have several calibration certificates?**
Yes. The certificate with the most remaining days determines the
instrument's expiry date. The instrument only becomes out of date when none
of its certificates is valid.

**Do maintenance tasks block the instrument?**
No. Maintenance tasks are documentation only. To make an instrument
unavailable, record a calibration or validation covering the downtime, or
tick *De-activate until next calibration test*.

**What is the difference between a calibration and a calibration
certificate?**
A *calibration* records the downtime while the work is done; during that
period the instrument is unavailable. A *calibration certificate* is the
outcome: the proof of calibration with its validity period. Without a valid
certificate the instrument is out of date even when no calibration is
running.

**What happens when an instrument fails a QC test?**
All analyses measured on that instrument that are awaiting verification are
retracted automatically, a remark and a PDF report are attached, an email is
sent, and the instrument disappears from the instrument selectors until a
passing QC batch is recorded.

**Why can I still pick an invalid instrument on a control or blank?**
By design. QC analyses accept invalid instruments so that the calibration
tests needed to re-qualify the instrument can be recorded; out-of-date
instruments are labelled *"(Out of date)"* in the selector.

**Are imported results verified automatically?**
No. Imported results are (by default) *submitted*, which moves the analyses
to *To be Verified* — verification and [publication](ResultsPublication.md)
still follow the normal workflow.

**Why was a file in my auto-import folder not imported?**
Files are only processed once: the folder's index file remembers imported
filenames. Check the instrument's *Auto-Import Logs* tab for the reason —
typical causes are an unparsable file, no matching sample in an allowed
state, or a missing/incorrect folder path for the interface.
