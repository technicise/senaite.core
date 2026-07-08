# Invoicing, Pricelists & Lab Products

SENAITE can keep track of what the analyses on a sample cost, show a
per-sample invoice (with an optional PDF), publish pricelists of your
analysis services and laboratory products, and maintain a small catalogue of
lab products (consumables) with their own prices and VAT.

This guide explains the feature from an end-user perspective: how prices are
configured on services and profiles, how discounts and VAT are applied, how
the invoice on a sample works, and how to create and print pricelists and
lab products.

---

## Table of Contents

1. [Overview](#overview)
2. [Key Concepts](#key-concepts)
3. [Pricing & Accounting Setup](#pricing--accounting-setup)
4. [Prices on Analysis Services](#prices-on-analysis-services)
5. [Profile Pricing](#profile-pricing)
6. [Client Discounts](#client-discounts)
7. [How a Sample's Price is Calculated](#how-a-samples-price-is-calculated)
8. [The Invoice on a Sample](#the-invoice-on-a-sample)
9. [Excluding a Sample from Invoicing](#excluding-a-sample-from-invoicing)
10. [Lab Products](#lab-products)
11. [Pricelists](#pricelists)
12. [Hiding Prices Completely](#hiding-prices-completely)
13. [Frequently Asked Questions](#frequently-asked-questions)
14. [Related Guides](#related-guides)

---

## Overview

Pricing in SENAITE starts at the **Analysis Service**: every service carries
a price (excluding VAT), an optional bulk price, and a VAT percentage.
**Analysis Profiles** can override this with a single fixed profile price.
On top of that, per-client discount flags and a global member discount
percentage adjust what a given client is charged.

Every sample has an **Invoice** tab that presents these amounts as a
proforma invoice; once the sample is verified or published, a real invoice
record with an attached PDF can be created and printed. Independently of
samples, **Pricelists** let you publish a printable price catalogue of your
analysis services or lab products.

Note that SENAITE is a LIMS, not an accounting package: the invoice
functionality is intentionally simple (one invoice per sample, no payment
tracking). Many laboratories use it as a billing reference and do the actual
accounting in an external system.

## Key Concepts

| Term | Meaning |
|------|---------|
| **Price (excluding VAT)** | The net price of a service, profile or lab product. All prices in SENAITE are entered excluding VAT. |
| **VAT %** | The tax percentage added on top of the net price. A system-wide default is set in Setup and can be overridden per service, profile or lab product. |
| **Bulk price** | An alternative, usually lower, price per analysis for clients flagged with *Bulk discount applies*. |
| **Member discount** | A percentage discount (defined once in Setup) applied to the whole sample subtotal for clients flagged with *Member discount applies*. |
| **Profile price** | A fixed price for an entire Analysis Profile, used instead of the sum of its single analyses when *Use analysis profile price* is enabled. |
| **Proforma** | The Invoice tab of a sample before an invoice record has been created. It shows the same amounts, labelled *Proforma (Not yet invoiced)*. |
| **Invoice** | A record created from a sample, stored under the client, with its own ID, date and attached PDF. |
| **Pricelist** | A printable catalogue of all active analysis services or lab products with their prices, VAT and totals. |
| **Lab Product** | An item in the laboratory's consumables catalogue (e.g. sample containers, kits) with volume, unit, price and VAT. |

## Pricing & Accounting Setup

Go to *Setup* and open the **Accounting** section. It contains the global
pricing options:

| Setting | Default | Description |
|---------|---------|-------------|
| **Include and display pricing information** | on | Master switch for pricing. When disabled, prices and the invoice disappear from the user interface (see [Hiding Prices Completely](#hiding-prices-completely)). |
| **Currency** | EUR | The currency the site uses to display prices. You can pick any standard (ISO) currency; its symbol is shown next to amounts on invoices and in listings. |
| **Country** | — | The country the site shows by default (used e.g. as default for addresses). |
| **Member discount %** | 0.0 | "The discount percentage entered here, is applied to the prices for clients flagged as 'members', normally co-operative members or associates deserving of this discount." |
| **VAT %** | 0.0 | "Enter percentage value eg. 14.0. This percentage is applied system wide but can be overwritten on individual items." |

The **VAT %** entered here is the default that new analysis services and lab
products pick up; each of them can still define its own VAT percentage.

## Prices on Analysis Services

Each Analysis Service (*Setup → Analysis Services*) carries three pricing
fields on its edit form:

- **Price (excluding VAT)** — the standard net price charged per analysis.
- **Bulk price (excluding VAT)** — "The price charged per analysis for
  clients who qualify for bulk discounts". This price is used instead of the
  standard price for clients that have *Bulk discount applies* enabled (see
  [Client Discounts](#client-discounts)).
- **VAT %** — the tax percentage for this service. Pre-filled with the
  system-wide VAT from Setup, but can be changed per service.

The Analysis Services listing in Setup shows a **Price** column so you can
review prices at a glance (the column is hidden when pricing is disabled
globally).

See [Analysis Services, Profiles & Sample Templates](AnalysesSetup.md) for
everything else about configuring services.

## Profile Pricing

Analysis Profiles (*Setup → Analysis Profiles*) can be billed in two ways:

- **By default**, a profile is just a shortcut for selecting analyses: each
  contained analysis is billed individually at its service price.
- With a **fixed profile price**, the profile itself becomes the billable
  item and its analyses are not charged individually.

To give a profile a fixed price, edit the profile and fill in:

1. **Use analysis profile price** — "Use profile price instead of single
   analyses prices". Tick this to activate profile-based billing.
2. **Price (excluding VAT)** — "Please provide the price excluding VAT".
3. **VAT %** — "Please provide the VAT in percent that is added to the
   profile price".
4. Optionally a **Commercial ID** — "Commercial ID used for accounting",
   useful to match the profile against an article number in your external
   accounting system.

On a sample that uses such a profile, the invoice shows one line for the
profile (with its price and VAT) instead of one line per contained analysis.
Analyses added to the sample that are *not* part of the priced profile are
still billed individually.

If *Use analysis profile price* is left unticked, the profile price and VAT
fields have no effect on billing.

## Client Discounts

Two checkboxes on the client's edit form control discounts (see
[Clients & Contacts](ClientsAndContacts.md)):

| Client setting | Effect |
|----------------|--------|
| **Bulk discount applies** | Analyses for this client are charged at each service's **Bulk price (excluding VAT)** instead of the standard price. |
| **Member discount applies** | The **Member discount %** from *Setup → Accounting* is subtracted from the sample's subtotal, and the invoice shows a *Discount* line. |

Both discounts can apply at the same time: the bulk price replaces the unit
price per analysis, and the member discount is then applied to the resulting
subtotal.

## How a Sample's Price is Calculated

The billable items of a sample are:

- each **profile** with *Use analysis profile price* enabled, as one item at
  its profile price, and
- each **analysis** that is not part of such a priced profile, at its
  service price (or bulk price for bulk-discount clients).

Analyses that have been **retracted** or **rejected** are excluded from
billing, as are inactive (cancelled) ones.

From these items the sample computes:

| Amount | How it is calculated |
|--------|----------------------|
| **Subtotal** | Sum of all billable item prices, excluding VAT and before any member discount. |
| **Discount** | Subtotal × Member discount % — only shown/applied if the client has *Member discount applies*. |
| **Taxes** | Sum of each billable item's VAT amount (each item uses its own VAT %). If a member discount applies, the VAT is reduced by the same percentage. |
| **Total** | Subtotal − Discount + Taxes. |

The same figures appear live in the **Add Samples** form while you select
analyses and profiles: a pricing block per sample column shows *Discount*,
*Subtotal*, *VAT* and *Total*, recalculated as you tick services (see
[Registering Samples](SampleRegistration.md)).

## The Invoice on a Sample

### Viewing the invoice

1. Open a sample and click the **Invoice** tab. The tab is only shown when
   *Include and display pricing information* is enabled in Setup, and only
   to users with the `senaite.core: Manage Invoices` permission (by default
   Lab Managers and Lab Clerks — client contacts do not see it).
2. Until an invoice record has been created, the page is headed
   **Proforma (Not yet invoiced)**.

The invoice page shows:

- a header with **Invoice To** (the sample's contact), **Invoice ID**,
  **Invoice Date**, **Client Reference**, **Sample Type**, **Sample ID**,
  **Analysis Profiles**, **Date Received**, **Client Order Number**,
  **Sample Point**, **Verified By** and **Date Published**;
- an items table with **Position**, **VAT** (%) and **Price** per billable
  item (priced profiles appear as single positions);
- the totals: **Subtotal**, **Discount** (only for member-discount clients,
  with the percentage shown), **Taxes** and **Total**, formatted with the
  currency symbol and decimal mark configured in Setup.

### Creating and printing the invoice

Two document actions (icons in the sample toolbar) become available on the
Invoice tab **once the sample is verified or published**:

- **Create Invoice PDF** — creates the invoice record and generates its PDF:
  - an **Invoice** object is stored inside the client, with its own ID
    (generated by the ID server, type *Invoice* — see
    [ID Server](IDServer.md)), the current date as invoice date, and the
    rendered PDF attached;
  - the invoice is linked to the sample, and the Invoice tab header changes
    from *Proforma* to the invoice ID;
  - a confirmation message *"Invoice … created"* is shown.
  - Running the action again on the same sample **updates the existing
    invoice** (new date and PDF) rather than creating a second one — a
    sample has at most one invoice.
- **Print Invoice** — renders the invoice as a PDF and opens/downloads it
  directly, without creating or changing any invoice record. You can use
  this at any time after verification, also for proforma printing before
  *Create Invoice PDF* has been used.

## Excluding a Sample from Invoicing

Every sample has an **Invoice Exclude** checkbox — "Should the analyses be
excluded from the invoice?". You can set it:

- per sample column in the **Add Samples** form, or
- later in the sample's header fields (editable by users with the
  `senaite.core: Field: Edit Invoice Exclude` permission).

When enabled, an **"Exclude from invoice"** icon is displayed next to the
sample title and in the samples listings, so lab and billing staff can see
at a glance that the sample must not be charged (e.g. re-tests, internal QC
samples, goodwill work).

Note that within SENAITE core the flag is **informational**: it marks the
sample, but it does not remove the Invoice tab or zero the calculated
amounts. It is up to your billing procedure (or an external accounting
integration) to honour the flag.

## Lab Products

Lab Products are the laboratory's catalogue of sellable consumables —
sampling kits, containers, transport boxes, etc. They are not linked to
samples or analyses; their purpose is to carry prices so they can be
published on a [Pricelist](#pricelists).

To manage them:

1. Go to *Setup → Lab Products*.
2. Click **Add** (requires the `senaite.core: Add LabProduct` permission)
   and fill in:

   | Field | Notes |
   |-------|-------|
   | **Name** | The product name (required). |
   | **Description** | Optional; can be included on pricelists. |
   | **Volume** | Free text, e.g. `100`. |
   | **Unit** | Free text, e.g. `mL`. Volume and unit are shown in parentheses after the name on pricelists. |
   | **Price (excluding VAT)** | "Please provide the price excluding VAT" (required). |
   | **VAT %** | "Please provide the VAT in percent that is added to the labproduct price". Pre-filled with the system-wide VAT from Setup. |

3. Save. The view then also shows the computed **VAT** amount and the
   **Total Price** (price + VAT).

The Lab Products listing shows **Product**, **Volume**, **Unit**, **Price**,
**VAT Amount** and **Total Price** columns, with the usual *Active* /
*Inactive* / *All* filters. Products you no longer sell can be deactivated;
inactive products are left out of newly saved pricelists.

## Pricelists

A Pricelist is a snapshot catalogue of **all active** analysis services *or*
lab products, with net price, VAT amount and price including VAT per item.

### Creating a pricelist

1. Browse to the **Pricelists** folder of your site (at
   `<your site URL>/pricelists`). Adding pricelists requires the
   `senaite.core: Add Pricelist` permission (by default Lab Manager and
   Lab Clerk).
2. Click **Add** and fill in the form:

   | Field | Notes |
   |-------|-------|
   | **Title** | Name of the pricelist (required). |
   | **Pricelist for** | Choose **Analysis Services** or **Lab Products**. |
   | **Bulk discount applies** | Services only: list each service's **Bulk price** instead of its standard price. |
   | **Discount %** | "Enter discount percentage value". Services only: applies this percentage discount to every service's standard price (ignored when *Bulk discount applies* is ticked). |
   | **Include descriptions** | "Select if the descriptions should be included" — adds each item's description to the list. |
   | **Remarks** | Free text printed at the bottom of the pricelist. |
   | **Start Date** | Effective date. If left empty, the pricelist is effective immediately. |
   | **End Date** | Expiration date. If left empty, it never expires. |

3. Save. The line items are generated automatically from the current
   catalogue:

   - **Analysis Services**: one line per active service, titled with the
     service name (and its unit in parentheses), grouped by analysis
     category and marked when accredited. The price is the bulk price, the
     discounted price or the standard price, depending on the settings
     above; the VAT amount uses each service's own **VAT %**.
   - **Lab Products**: one line per active product, titled with the product
     name plus volume and unit in parentheses; VAT comes from each
     product's **VAT %**.

The pricelist page shows the start/end dates and a table with **Title**,
**Amount** (excluding VAT), **VAT** and **Amount incl. VAT** — so both
VAT-exclusive and VAT-inclusive prices are always visible, each prefixed
with the site currency.

The Pricelists folder lists your pricelists with their **Start Date** and
**End Date**, filtered into *Active* (currently valid), *Inactive* (expired,
not yet effective, or deactivated) and *All*.

### Keeping pricelists up to date

The line items are (re)generated **every time the pricelist is saved** —
they are a snapshot of the prices at that moment. If you change service or
product prices afterwards, existing pricelists keep the old figures until
you edit and re-save them. This also means a saved pricelist documents the
prices that were valid during its start/end period.

### Printing and emailing

- Use the **Print pricelist** tab on a pricelist to get a print-friendly
  page of the same content, and print it (or save it as PDF) with your
  browser's print function.
- **Emailing pricelists is not available** in current SENAITE: there is no
  email action on pricelists (this existed in older Bika LIMS versions). To
  send a pricelist to clients, print/save it and attach it to a regular
  email.

## Hiding Prices Completely

Some laboratories do not want any pricing in the LIMS. Disable
**Include and display pricing information** in *Setup → Accounting*
(registry setting `show_prices`) and SENAITE hides:

- the **Invoice** tab on samples (including the create/print actions),
- the pricing block (*Discount*, *Subtotal*, *VAT*, *Total*) in the **Add
  Samples** form,
- the **Price** column in the Analysis Services setup listing,
- the price columns in the analysis selection widgets of profiles, sample
  templates and the *Manage Analyses* form of a sample,
- the prices in the analysis service information popup and in the
  accredited-services overview.

The setting only affects the user interface: prices stored on services,
profiles and products are kept, and everything reappears when you switch the
option back on. The Pricelists folder itself remains reachable regardless of
this setting.

## Frequently Asked Questions

**Why don't I see the Invoice tab on a sample?**
Either *Include and display pricing information* is disabled in *Setup →
Accounting*, or your user lacks the `senaite.core: Manage Invoices`
permission. By default only laboratory roles (Lab Manager, Lab Clerk) can
see invoices — client contacts cannot.

**Why are the Create Invoice PDF / Print Invoice buttons missing?**
They only appear once the sample has been **verified** or **published**.
Before that, the Invoice tab shows the proforma amounts only.

**Can a sample have more than one invoice?**
No. *Create Invoice PDF* creates one invoice record per sample; running it
again refreshes the same record with a new date and PDF.

**A profile has a price, but the invoice still lists every analysis. Why?**
The profile's **Use analysis profile price** checkbox is not ticked. The
profile price and VAT are only used when that option is enabled.

**How do I charge a client lower "bulk" rates?**
Enter a **Bulk price (excluding VAT)** on each service and tick **Bulk
discount applies** on the client. All analyses for that client are then
priced at the bulk rate automatically.

**Where does the discount percentage on the invoice come from?**
From **Member discount %** in *Setup → Accounting*. It is applied only for
clients that have **Member discount applies** ticked.

**Do rejected or retracted analyses get billed?**
No. Retracted, rejected and cancelled analyses are excluded from the
billable items, so they never appear on the invoice.

**My pricelist shows outdated prices. What happened?**
Pricelist line items are a snapshot taken when the pricelist is saved. Edit
and re-save the pricelist to regenerate the lines from the current service
or product prices.

**Can I email a pricelist to my clients from SENAITE?**
Not directly — there is no email action for pricelists in current SENAITE.
Use *Print pricelist*, save it as PDF via your browser, and send it
manually.

**Does "Invoice Exclude" remove the invoice?**
No. It flags the sample with an *Exclude from invoice* icon so billing staff
know not to charge it, but the Invoice tab and the calculated amounts remain
visible.

## Related Guides

- [Analysis Services, Profiles & Sample Templates](AnalysesSetup.md) — where
  service prices, bulk prices, VAT and profiles are configured.
- [Clients & Contacts](ClientsAndContacts.md) — the client flags *Bulk
  discount applies* and *Member discount applies*.
- [Registering Samples](SampleRegistration.md) — the Add Samples form with
  its live price calculation and the *Invoice Exclude* checkbox.
- [Results Publication](ResultsPublication.md) — publishing results, after
  which invoices are typically created.
- [User Roles & Permissions](UserRolesAndPermissions.md) — who may manage
  invoices, pricelists and lab products.
- [ID Server](IDServer.md) — how invoice and pricelist IDs are generated.
- Back to the [User Guide index](UserGuide.md).
