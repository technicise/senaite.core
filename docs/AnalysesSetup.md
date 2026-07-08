# Analysis Services, Profiles and Sample Templates

Before the laboratory can register its first sample, SENAITE needs to know
what the lab actually tests: which analyses it offers, how results are
captured and calculated, what the valid result ranges are, and which sample
types, containers and preservations are in use. All of this is configured
once, under *Setup*, and then reused every day at sample registration.

This guide explains that laboratory setup from an end-user perspective:
analysis categories and services, calculations and methods, analysis
profiles, sample templates, analysis specifications, sample types, sample
matrices, sample points, containers and preservations.

---

## Table of Contents

1. [Overview: The Building Blocks](#overview-the-building-blocks)
2. [Analysis Categories](#analysis-categories)
3. [Analysis Services](#analysis-services)
4. [Calculations and Interim Fields](#calculations-and-interim-fields)
5. [Methods](#methods)
6. [Analysis Profiles](#analysis-profiles)
7. [Sample Templates](#sample-templates)
8. [Analysis Specifications](#analysis-specifications)
9. [Sample Types](#sample-types)
10. [Sample Matrices](#sample-matrices)
11. [Sample Points](#sample-points)
12. [Containers and Preservations](#containers-and-preservations)
13. [Frequently Asked Questions](#frequently-asked-questions)

---

## Overview: The Building Blocks

| Item | What it is | Where to configure |
|------|------------|--------------------|
| **Analysis Category** | A grouping of analysis services (e.g. *Metals*, *Microbiology*), used to organise listings and reports. | *Setup → Analysis Categories* |
| **Analysis Service** | A single test the lab offers (e.g. *Calcium*), with its unit, precision, price, method, limits, etc. | *Setup → Analysis Services* |
| **Calculation** | A formula that derives a result from other analyses and/or interim (intermediate) values. | *Setup → Calculations* |
| **Method** | A description of *how* a test is performed, with instructions, documents, supported instruments and calculations. | *Setup → Methods* |
| **Analysis Profile** | A named set of services that is frequently requested together, optionally with its own price. | *Setup → Analysis Profiles*, or inside a client |
| **Sample Template** | A blueprint for a whole sample: sample type, sample point, services and a partition scheme. | *Setup → Sample Templates*, or inside a client |
| **Analysis Specification** | Valid result ranges (min/max, warning shoulders) per service for a given sample type. | *Setup → Analysis Specifications*, or inside a client |
| **Sample Type** | The kind of material sampled (e.g. *Water*, *Soil*), with ID prefix, retention period, etc. | *Setup → Sample Types* |
| **Sample Matrix** | An optional classification of sample types by the medium sampled (e.g. *Liquid*, *Solid*). | *Setup → Sample Matrices* |
| **Sample Point** | The physical location where samples are collected. | *Setup → Sample Points*, or inside a client |
| **Container / Container Type** | The vessels samples and partitions are stored in. | *Setup → Sample Containers*, *Setup → Container Types* |
| **Preservation** | The preservation method applied to a sample or partition. | *Setup → Sample Preservations* |

At registration time these pieces come together: the user picks a sample
type and sample point, selects services directly or via profiles and
templates, and the system applies the matching specification, containers and
partition scheme. See [Sample Registration](SampleRegistration.md) for the
registration side of the story, and
[Clients and Contacts](ClientsAndContacts.md) for the client-specific
variants of profiles, templates, specifications and sample points. The same
setup applies when samples are registered as part of a batch (see
[Batches](Batches.md)) or created as
[Secondary Samples](SecondarySamples.md) from an existing sample.

Nearly all setup items can be **deactivated** rather than deleted: a
deactivated item is kept for traceability of existing samples but no longer
offered for selection.

## Analysis Categories

Analysis categories group services for listings and reports. Every analysis
service must belong to exactly one category.

To create one, go to *Setup → Analysis Categories*, press *Add* and fill in:

| Field | Meaning |
|-------|---------|
| **Name** | The category title, e.g. *Anions*. |
| **Description** | Free text, shown in listings. |
| **Comments** | Text displayed below each analysis category section on results reports. |
| **Department** | The responsible lab department (required). |
| **Sort Key** | A number from 0.0 to 1000.0 that defines the ordering of categories; duplicates are ordered alphabetically. |

Whether analyses are visually grouped by category in the LIMS tables is
controlled by the setup option **Categorise analysis services**
(*Setup → Analyses* tab, setting `CategoriseAnalysisServices`) — helpful
when the list of services is long.

## Analysis Services

An analysis service describes one test of the laboratory's catalogue. Go to
*Setup → Analysis Services* and press *Add*. The edit form is organised in
tabs; the most relevant fields are described below.

### Description tab

| Field | Meaning |
|-------|---------|
| **Title** | The name of the analysis, e.g. *Calcium*. |
| **Short title** | Used instead of the title in column headings where space is limited. |
| **Sort Key** | Number from 0.0 – 1000.0 defining the sort order within listings. |
| **Commercial ID / Protocol ID** | Free identifiers for accounting and for the analytical protocol. |
| **Scientific name** | If enabled, the name is rendered in italics. |
| **Analysis Keyword** | A **unique** keyword identifying the service. It is used in instrument results imports, in bulk sample import files, and as the variable name in calculation formulas. Choose it carefully — see [Calculations](#calculations-and-interim-fields). |
| **Default Unit** | The measurement unit for results, e.g. *mg/l*, *ppm*. |
| **Units for Selection** | An optional list of alternative units the analyst may choose from at results entry; include the default unit in the list. |
| **Point of Capture** | *Lab* or *Field*. Field analyses are captured during sampling at the sample point (e.g. water temperature in the river); lab analyses are done in the laboratory. |
| **Analysis Category** | The [category](#analysis-categories) the service belongs to (required). |
| **Price (excluding VAT)** | The base price per analysis. |
| **Bulk price (excluding VAT)** | The price charged to clients who qualify for bulk discounts. |
| **VAT %** | Tax percentage; defaults to the system-wide VAT set in *Setup*. |
| **Department** | The responsible lab department. |
| **Accredited** | Tick if the service is included in the laboratory's schedule of accredited analyses; accredited analyses are marked on reports. |

A **Member discount %** can additionally be defined in *Setup → Accounting*;
it is applied to prices for clients flagged as members (see
[Clients and Contacts](ClientsAndContacts.md)).

### Analysis tab

| Field | Meaning |
|-------|---------|
| **Precision as number of decimals** | Number of decimals used when displaying and reporting the result. |
| **Exponential format precision** | If the result precision exceeds this value (default 7), results are shown in scientific notation. |
| **Attachment required for verification** | Makes an attachment mandatory before results can be verified. |
| **Maximum turn-around time** | Maximum time allowed for completing the analysis (counted from sample reception). A *late analysis* alert is raised when it elapses. If empty, the *Default turnaround time for analyses* from *Setup* applies. See [Sample Workflow](SampleWorkflow.md). |
| **Maximum holding time** | If the time elapsed since sample collection exceeds this limit, the service is no longer offered on the sample registration form (it remains available in the *Manage Analyses* view of a sample). |
| **Duplicate Variation %** | Allowed difference between an analysis and its duplicates on worksheets; larger deviations raise an alert. See [Quality Control](QualityControl.md). |
| **Hidden** | If enabled, the analysis and its results are not displayed by default in reports. This can be overridden per profile, per template and per sample. See [Results Publication](ResultsPublication.md). |
| **Self-verification of results** | Whether the user who submitted a result may also verify it: *System default*, *Yes* or *No*. Overrides the global setting. |
| **Number of required verifications** | How many different qualified users (1–4) must verify a result before it becomes *verified*; overrides the system default. See [Results Entry and Verification](ResultsEntryAndVerification.md). |

### Method tab

| Field | Meaning |
|-------|---------|
| **Methods** | The [methods](#methods) available to perform the test. |
| **Default Method** | The method preselected for new analyses of this service. |
| **Instruments** | The instruments that may be used. When methods are selected above, the list only offers instruments that support those methods. See [Instruments](Instruments.md). |
| **Default Instrument** | The instrument preselected at results entry; must be one of the selected instruments. |
| **Calculation** | The [calculation](#calculations-and-interim-fields) used to compute the result. When methods are selected, the choice is limited to the calculations supported by those methods. |

### Uncertainties tab

| Field | Meaning |
|-------|---------|
| **Uncertainty** | A table of result ranges (*Range min* / *Range max*) with an *Uncertainty value* each. A result of 6.67 with uncertainty 0.5 is reported as 6.67 ± 0.5. Append `%` to the value to express the uncertainty as a percentage of the result. Set the value to 0 (or below) for a range where no uncertainty should be displayed, and make sure successive ranges are continuous (0.00–10.00, 10.01–20.00, …). |
| **Calculate Precision from Uncertainties** | Derives the number of significant digits from the uncertainty instead of the fixed precision, e.g. result 5.243 with uncertainty 0.22 is displayed as 5.2 ± 0.2. When no uncertainty range matches, the fixed precision is used. |
| **Allow manual uncertainty value input** | Lets the analyst replace the default uncertainty value at results entry. |

### Result Options tab

| Field | Meaning |
|-------|---------|
| **Result type** | The control rendered at results entry: *Numeric*, *String*, *Text*, *Selection list*, *Multiple selection*, *Multiple selection (with duplicates)*, *Multiple choices*, *Date* or *Datetime*. |
| **Predefined results** | A list of allowed final results, each with a *Result Value* (a number, used internally and in calculations) and a *Display Value* (the text shown). When set, the analyst must choose from these values and cannot enter free results. |
| **Sorting criteria** | How the predefined options are ordered in the selection list. |
| **Default result** | The result preselected at results entry. |
| **Result variables** | Additional interim result values captured next to the final result (same format as calculation interim fields — see below). |

### Limits tab

| Field | Meaning |
|-------|---------|
| **Lower Limit of Detection (LLOD)** | Lowest concentration the methodology can reliably detect. Results below it are typically reported as *< LLOD*. |
| **Lower Limit of Quantification (LLOQ)** | Lowest concentration that can be reliably *quantified*; results below are typically reported as *< LOQ*. |
| **Upper Limit of Quantification (ULOQ)** | Highest concentration that can be reliably quantified; results above are typically reported as *> ULOQ*. |
| **Upper Limit of Detection (ULOD)** | Highest concentration the methodology can reliably measure; results above are typically reported as *> ULOD*. |
| **Display a Detection Limit selector** | Shows a selector next to the result field at results entry, so the analyst can record the value as a detection limit (`<` or `>`) instead of a regular result. |
| **Allow Manual Detection Limit input** | Only relevant when the selector is enabled: lets the analyst replace the default detection limit values manually; otherwise the configured limits are used read-only. |

### Advanced tab — Analysis conditions

**Analysis conditions** are questions asked at sample registration when this
service is selected — for example the temperature, ramp and flow for a
thermogravimetric analysis. Each condition has a *Title*, *Description*, a
*Control type* (*Text*, *Number*, *Checkbox*, *Select* or *File upload*),
optional *Choices* (for selects, in the format
`key1:value1|key2:value2|…`), a *Default value*, and *Required* and
*Report* flags. The captured values are shown to lab personnel when
performing the test.

## Calculations and Interim Fields

Calculations compute an analysis result from other analyses and/or
intermediate values. Typical examples: *Total Hardness* from Calcium and
Magnesium, or a titration result from volume and factor.

To create one, go to *Setup → Calculations* and press *Add*:

1. Enter a **Name** and optional **Description**.
2. Define **Calculation Interim Fields** — intermediate values the analyst
   enters at results entry (e.g. vessel mass, dilution factor). Each row
   has:

   | Column | Meaning |
   |--------|---------|
   | **Keyword** | The variable name used in the formula. |
   | **Field title** | The label shown as column header at results entry. |
   | **Default value** | Value pre-filled at results entry. |
   | **Choices** | Optional list of selectable options. |
   | **Result type** | Numeric, string, etc. |
   | **Allow empty** | Whether the field may be left blank. |
   | **Unit** | Displayed next to the field. |
   | **Report** | Include the interim value on results reports. |
   | **Hidden** | Hide the field from results entry views. |
   | **Apply wide** | Shows the field in a selection box at the top of the worksheet, so one value can be applied to all corresponding fields on the sheet at once (see [Worksheets](Worksheets.md)). |

3. Optionally add **Additional Python Libraries** — pairs of *Module* and
   *Function* (e.g. module `math`, function `floor`) that make extra
   mathematical functions available to the formula.
4. Enter the **Calculation Formula** using standard maths operators
   (`+ - * / ( )`) and keywords in square brackets. Keywords may be interim
   field keywords or the *Analysis Keyword* of other services, e.g.
   `[Ca] + [Mg]` for Total Hardness.
5. Test the calculation: the **Test Parameters** table lists all keywords
   the formula uses; enter sample values and save — the **Test Result**
   field shows the outcome.

Services whose keywords appear in the formula automatically become
**dependencies**: when the calculated service is added to a sample, its
dependencies are added too, and the result is computed as soon as the
dependency results are captured. Assign the calculation to a service on the
service's *Method* tab.

## Methods

A method describes how an analysis is performed and links services to
instruments and calculations. Go to *Setup → Methods* and press *Add*:

| Field | Meaning |
|-------|---------|
| **Method ID** | A unique identifier code for the method. |
| **Description** | Short method description. |
| **Accredited** | Tick if the method has been accredited. |
| **Instructions** | Technical description and instructions intended for analysts (rich text). |
| **Method Document** | Upload a document describing the method. |
| **Instruments** | The instruments supporting this method. Selecting instruments here also registers the method on those instruments (see [Instruments](Instruments.md)). |
| **Calculations** | The calculations supported by this method. |

On the analysis service, the selected methods drive which instruments and
calculations are offered, and the analyst can switch between the service's
available methods at results entry.

## Analysis Profiles

An analysis profile is a named set of services that clients frequently
order together, e.g. an *Irrigation water screen*. At registration the user
picks the profile and all its analyses are added in one go (see
[Sample Registration](SampleRegistration.md)).

Profiles live in *Setup → Analysis Profiles* for lab-wide profiles, or
inside a **client** for profiles only that client can use.

1. Go to *Setup → Analysis Profiles* (or the *Profiles* tab of a client)
   and press *Add*.
2. Enter a **Name**, optional **Description** and a **Profile Keyword** —
   a unique keyword that can be used to reference the profile, e.g. in
   import files.
3. On the **Analyses** tab, tick the services that belong to the profile.
   For each selected service you can override its **Hidden** flag, so an
   analysis that is normally visible is hidden on reports when ordered via
   this profile (or vice versa).
4. Optionally restrict the profile to certain **Sample types**: the profile
   is then only offered at sample creation/edit when one of those sample
   types is selected. If no sample type is set, the profile is always
   available.

### Profile price and discounts

The **Accounting** tab controls how samples with this profile are priced:

| Field | Meaning |
|-------|---------|
| **Commercial ID** | Identifier used for accounting. |
| **Use analysis profile price** | If enabled, the profile price is charged *instead of* the sum of the individual analysis prices. |
| **Price (excluding VAT)** | The profile price. |
| **VAT %** | The VAT percentage added to the profile price. |

When *Use analysis profile price* is disabled, each analysis is invoiced at
its own service price (with the client's member/bulk discounts where
applicable).

## Sample Templates

A sample template is a blueprint for a complete sample: it can preselect
the sample type, sample point, the analyses to order and even how the
sample will be split into partitions. Choosing a template in the Add
Samples form fills all of this in automatically (see
[Sample Registration](SampleRegistration.md)).

Templates live in *Setup → Sample Templates* for lab-wide use, or inside a
**client** for client-specific templates.

1. Go to *Setup → Sample Templates* (or the *Templates* tab of a client)
   and press *Add*.
2. On the **Default** tab, enter a **Name** and optional **Description**,
   and select:

   | Field | Meaning |
   |-------|---------|
   | **Sample Point** | The [sample point](#sample-points) preselected for samples created from this template. |
   | **Sample Type** | The [sample type](#sample-types) preselected for the sample. |
   | **Composite** | Tick if the sample is a mix of sub-samples. |
   | **Sample collected by the laboratory** | Enables the sampling workflow for samples created from this template, so a sampler and sampling date are recorded before reception (see [Sample Workflow](SampleWorkflow.md)). |

3. On the **Partitions** tab, define the partition scheme. Each row defines
   one partition:

   | Column | Meaning |
   |--------|---------|
   | **Partition ID** | An identifier within the template, e.g. `part-1`. |
   | **Container** | The [container](#containers-and-preservations) for the aliquot. |
   | **Preservation** | The preservation applied to the aliquot. |
   | **Sample Type** | Optional different sample type for the partition. |

   The **Auto-partition on sample reception** option redirects the lab user
   to the partitions view as soon as a sample created from this template is
   received, with this scheme pre-loaded. How partitions are created and
   behave is explained in [Sample Partitions](SamplePartitions.md).

4. On the **Analyses** tab, tick the services included in the template. For
   each service you can set the **Partition** column (which partition of
   the scheme the analysis is assigned to) and override the **Hidden** flag
   for reports.

Unlike profiles, templates do not carry a price of their own — the ordered
analyses (or a profile) determine the price.

## Analysis Specifications

Analysis specifications define the valid results ranges per service for a
given sample type. When a specification is applied to a sample, results
outside the ranges are flagged at results entry and on reports.

Specifications live in *Setup → Analysis Specifications* for lab-wide
("Lab") specifications, or inside a **client** for client-specific ones.

1. Go to *Setup → Analysis Specifications* and press *Add*.
2. Select the **Sample Type** the specification applies to (required), and
   give it a **Title** and optional **Description**.
3. In the **Specifications** table, enter values for each relevant service:

   | Column | Meaning |
   |--------|---------|
   | **Min warn / Max warn** | The *shoulder* range. A result outside the valid range but within the shoulders raises a less severe (warning) alert. |
   | **Min / Max** | The valid results range. Any result outside it raises an out-of-range alert. |
   | **Min operator** | `>=` or `>` — whether the minimum itself is still valid. |
   | **Max operator** | `<=` or `<` — whether the maximum itself is still valid. |
   | **< Min / > Max** | If the result falls below *< Min* (or above *> Max*), that literal value is displayed in listings and results reports **instead of** the real result. |
   | **Out of range comment** | Text shown on the results report when the result is out of range. |

At registration, the **Analysis Specification** field of the sample offers
the specifications matching the selected sample type. The chosen
specification acts as a *template*: its ranges are copied onto the sample
and its analyses, so later changes to the setup specification do not affect
already created samples. With the setup option **Enable Sample
Specifications** (`EnableARSpecs`), the ranges can additionally be edited
directly on the sample. Out-of-range and shoulder alerts appear at results
entry (see [Results Entry and Verification](ResultsEntryAndVerification.md))
and on the published report (see
[Results Publication](ResultsPublication.md)).

### Dynamic Analysis Specifications

For matrices where ranges depend on more than the sample type (e.g. per
method, per sample point), use *Setup → Dynamic Analysis Specifications*:

1. Prepare an Excel file whose first sheet has at least the columns
   `Keyword`, `min` and `max`. Any additional column (e.g. `Method`) is
   matched against the corresponding value of the analysis or sample, so
   different rows can apply under different conditions.
2. Create a Dynamic Analysis Specification and upload the file.
3. Link it to a regular specification via its **Dynamic Analysis
   Specification** field. Whenever that specification is used, ranges from
   the spreadsheet take precedence for matching rows.

## Sample Types

Sample types describe the material being sampled and are mandatory for
every sample. Go to *Setup → Sample Types* and press *Add*:

| Field | Meaning |
|-------|---------|
| **Name** | E.g. *Water*, *Soil*. |
| **Sample Type Prefix** | Short ASCII code without whitespaces, typically used to build sample IDs (e.g. prefix `WATER` → `WATER-0001`). |
| **Retention Period** | How long unpreserved samples of this type can be kept before they expire and cannot be analysed any further. Defaults to the setup's default sample lifetime. |
| **Hazardous** | Samples of this type should be treated as hazardous. |
| **Sample Matrix** | Optional classification of the matrix (see [Sample Matrices](#sample-matrices)). |
| **Minimum Volume** | The minimum sample volume required for analysis, e.g. *10 ml* or *1 kg*. Samples arriving with insufficient volume are typical candidates for rejection at reception (see [Sample Rejection](SampleRejection.md)). |
| **Default Container Type** | New sample partitions are automatically assigned a container of this type, unless specified in more detail elsewhere. |
| **Admitted sticker templates** | Which barcode/label stickers may be used for this sample type, and the default small and large sticker. |

Sample types are referenced almost everywhere: by templates, profiles,
specifications, sample points and at registration.

## Sample Matrices

A sample matrix classifies sample types by the medium that is sampled —
for example *Liquid*, *Solid* and *Gas*, or finer distinctions such as
*Drinking Water* and *Waste Water*. Matrices are purely descriptive: they
add no behaviour of their own, but give related sample types a common
label, so the lab's catalogue of sample types stays organised as it grows.

To create one, go to *Setup → Sample Matrices* and press *Add*:

| Field | Meaning |
|-------|---------|
| **Name** | The matrix title, e.g. *Liquid*. |
| **Description** | Free text, shown in listings. |

To assign a matrix, edit a [sample type](#sample-types) and select it in
the **Sample Matrix** field (*"Select the sample matrix for this sample
type"*). Each sample type can carry at most one matrix, and the field is
optional — sample types without a matrix are perfectly valid. The assigned
matrix is displayed as a column in the *Sample Types* listing, with a link
to the matrix itself.

Like most setup items, sample matrices can be **deactivated** instead of
deleted: an inactive matrix is kept for existing sample types but is no
longer offered for selection.

## Sample Points

Sample points record where samples are collected — a borehole, a tap, a
production line. Go to *Setup → Sample Points* (or the *Sample Points* tab
of a client for client-specific locations) and press *Add*:

| Field | Meaning |
|-------|---------|
| **Name / Description** | Identify the location. |
| **Location** | GPS coordinates (latitude/longitude) of the point or area. |
| **Elevation** | The height or depth at which the sample has to be taken. |
| **Sampling Frequency** | If samples are taken periodically here, the frequency, e.g. weekly. |
| **Sample Types** | The sample types that can be collected at this point. The sample point selection at registration is filtered accordingly; sample points without sample types assigned are always available. |
| **Composite** | Tick if samples taken here are composites, mixed from several sub-samples; unticked means *grab* samples. |
| **Attachment** | An optional file, e.g. a map or photo of the location. |

## Containers and Preservations

These items describe the physical handling of samples and are mainly used
by the partition scheme of [sample templates](#sample-templates) and by the
*Manage Partitions* form (see [Sample Partitions](SamplePartitions.md)).

### Container Types

Simple classifications like *Glass bottle* or *Plastic vial*
(*Setup → Container Types*, just name and description). They are used by
the sample type's **Default Container Type** field.

### Sample Containers

Concrete containers (*Setup → Sample Containers*):

| Field | Meaning |
|-------|---------|
| **Name / Description** | E.g. *500 ml amber glass bottle*. |
| **Container Type** | The [container type](#container-types) this container belongs to. |
| **Capacity** | Maximum size or volume of samples, e.g. *500 ml*. |
| **Pre-preserved** | Tick if the container already contains the preservative. A pre-preserved container must have a **Preservation** selected; partitions stored in it skip the separate preservation step of the workflow. |
| **Preservation** | The preservation method of this container (required when pre-preserved). |
| **Security seal intact** | Whether the container's security seal is intact. |

### Sample Preservations

Preservation methods (*Setup → Sample Preservations*) have a name, a
description and a **Category** — *Lab Preservation* (default) or *Field
Preservation*, depending on whether the preservation is applied in the
laboratory or at the sampling site.

## Frequently Asked Questions

**What is the difference between an analysis profile and a sample
template?**
A profile is just a bundle of analyses (optionally with its own price). A
template goes further: it can preselect the sample type and sample point,
enable the sampling workflow, assign analyses to partitions and trigger
auto-partitioning on reception.

**Why is a service, profile or template not offered at registration?**
Check that it is active, that any sample type restriction matches the
selected sample type, and — for services — that the *Maximum holding time*
has not elapsed since the sample was collected.

**Why can't I pick an instrument for my service?**
When methods are selected on the service's *Method* tab, only instruments
supporting those methods are offered. Either assign the instrument to one
of the methods, or clear the methods selection.

**Can I rename an Analysis Keyword?**
The keyword must stay unique and is validated against existing calculations
and analyses. Avoid changing keywords that are referenced in calculation
formulas or used by instrument import interfaces — they identify the
service there.

**I changed a specification in Setup — why do existing samples still use
the old ranges?**
The specification is copied onto the sample when it is assigned. Existing
samples keep their ranges unless you re-assign the specification (or edit
the ranges on the sample itself when *Enable Sample Specifications* is
active).

**What happens to profiles and templates when I deactivate a service?**
The service is removed from the profiles and templates that contain it, so
they remain usable without it.

**How do I keep an analysis off the client report?**
Enable the **Hidden** flag on the service, or override it per profile, per
template or per sample. Hidden analyses are still processed normally by the
lab — they are simply not displayed by default on reports (see
[Results Publication](ResultsPublication.md)).

**What is the difference between *Result Value* and *Display Value* in
predefined results?**
The *Display Value* is what users see and select; the *Result Value* must
be a number and is what the system stores and uses in calculations.

**Where do the `< LLOD` / `> ULOD` style results come from?**
From the detection and quantification limits on the service's *Limits* tab.
If the *Detection Limit selector* is enabled, the analyst can explicitly
record a result as `<` or `>` a detection limit at results entry.

**Do partitions inherit containers and preservations automatically?**
When a sample is created from a template with a partition scheme, the
*Manage Partitions* form is pre-populated with the containers and
preservations of the scheme. See
[Sample Partitions](SamplePartitions.md) for details.
