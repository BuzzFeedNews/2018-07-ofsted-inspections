# Analysis of Ofsted children's homes inspections, 2017-18 inspection year

This repository contains data, Python code, and findings relevant to BuzzFeed News's analysis of children's home inspection data from the UK's [Office for Standards in Education, Children's Services and Skills](https://www.gov.uk/government/organisations/ofsted) ("Ofsted").

For important context, please see these two related BuzzFeed News articles:

- https://www.buzzfeed.com/richholmes/care-price (26 July 2018)
- https://www.buzzfeed.com/richholmes/keys-group-aaron-leafe (2 August 2018)

## Table Of Contents

- [Input data](#input-data)
- [Analysis](#analysis)
- [Reproducibility](#reproducibility)
- [Licensing](#licensing)
- [Feedback / Questions?](#feedback--questions)

# Input Data

## Inspection results

The main inputs two spreadsheets in Ofsted's ["Children's social care data in England 2018" release](https://www.gov.uk/government/statistics/childrens-social-care-data-in-england-2018), published 19 July 2018. Specifically, these two files:

- "Children's social care data in England 2017 to 2018: provider level data as at 31 March 2018," which contains the most recent evaluations for all active children's social care providers (including but not limited to children's homes). In this repository, the data has been saved as [`inputs/providers-as-at-2018-03-31.csv`](inputs/providers-as-at-2018-03-31.csv).

- "Children's social care data in England 2017 to 2018: provider level data", which contains *all* inspections of children's social care providers conducted during Ofsted's 2017-18 "inspection year", which began on 1 April 2017 and ended on 31 March 2018 — regardless of whether the provider's registration is still active. In this repository, the data has been saved as [`inputs/inspections-in-year-2017-18.csv`](inputs/inspections-in-year-2017-18.csv).

### Notes

- In the data, "URN" stands for "Unique Reference Number" and is "a unique number used by Ofsted to distinguish between each individual home".

- The data above includes only inspections conducted by 31 March 2018 and published by 30 April 2018.  ([Ofsted's guidelines](https://www.gov.uk/guidance/social-care-common-inspection-framework-sccif-children-s-homes-including-secure-children-s-homes/11-timeframe) aim for inspection reports to be published a "maximum of 28 working days of the end of the inspection", but there are sometimes longer delays.)

- Additional details and definitions can be found in the "Cover", "Notes", and "Glossary" tabs of the "Children’s social care data in England 2017 to 2018: charts, tables and underlying data" spreadsheet [here](https://www.gov.uk/government/statistics/childrens-social-care-data-in-england-2018).

## Provider ownership

The files above do not contain information about private-sector providers' ownership. However, other Ofsted datasets — such as those published [here](https://www.gov.uk/government/publications/local-authority-and-childrens-homes-in-england-inspections-and-outcomes/local-authority-and-childrens-homes-in-england-inspections-and-outcomes-autumn-2017-main-findings) and [here](https://www.gov.uk/government/statistical-data-sets/quarterly-management-information-ofsteds-childrens-homes-inspection-outcomes), do contain such information. The ownerships listed in those publications accounted for most of the private-sector children's homes. BuzzFeed News collected the ownership information for the remaining homes — generally those opened recently and those no longer active — from individual [inspection PDFs on Ofsted's website](https://reports.beta.ofsted.gov.uk/). 

BuzzFeed News combined those two sources of ownership information into a single spreadsheet — [`inputs/private-sector-ownerships.csv`](inputs/private-sector-ownerships.csv) — that links private-sector children's homes to ownership information, and indicates whether the ownership information was derived from Ofsted's own spreadsheets or BuzzFeed News research.

# Analysis

The [`00-analyze-ofsted-data`](notebooks/00-analyze-ofsted-data.ipynb) notebook contains the following analyses (and the Python code underlying them):

## Main analysis

The main analysis begins with the "as at 31 March 2018" data described above, and then takes the following steps:

- Filters the raw Ofsted data to __include only__:
    - Providers of the following two types: "Children's home" and "Residential special school registered as a children's home". (This excludes homes that Ofsted has categorized as "Secure children's home".)
    - Providers whose most recent listed inspection occurred on or after 1 April 2016.
    - Providers in the following sectors: "Private", "Voluntary", and "Local Authority". (This excludes the relatively few homes listed in the "Health Authority" sector.)

- Merges this data with the private-sector ownership information described above.

- Identifies homes owned by Cambian PLC and by Keys Group.

- Tallies the number of homes by "overall effectiveness", which Ofsted rates as either "Inadequate", "Requires improvement [to be good]", "Good", or "Outstanding".

- Calculates the proportion of homes, by provider sector and ownership, that were given subpar effectiveness ratings — either "Inadequate" or "Requires improvement".

### Results

- Cambian homes: 39 homes "Inadequate" or "Requires improvement" out of 159 inspected (25% subpar)
- Non-Cambian private sector: 259 of 1,370 (19% subpar)
- Keys homes: 15 of 69 (22% subpar)
- Non-Keys private sector: 283 of 1,460 (19% subpar)
- All private sector: 298 of 1,529 (19% subpar)
- Voluntary sector: 24 of 153 (16% subpar)
- Local authority sector: 61 of 401 (15% subpar)


## Additional analyses

The [`00-analyze-ofsted-data`](notebooks/00-analyze-ofsted-data.ipynb) also includes two additional analyses, which are very similar to the analysis above, but take slightly different approaches.

The first of the alternative analyses uses the most recent full inspection conducted during the 2017-18 inspection year (1 April 2017 - 31 March 2018), for each provider. This approach, unlike the “as at” analysis, includes homes that were inspected in the 2017-18 inspection year but that are no longer registered. Conversely, it excludes homes that either did not have an inspection in the 2017-18 inspection year, or for which the inspection was not published until after 30 April 2018. Results:

- Cambian homes: 40 homes "Inadequate" or "Requires improvement" out of 159 inspected (25% subpar)
- Non-Cambian private sector: 280 of 1,380 (20% subpar)
- Keys homes: 19 of 74 (26% subpar)
- Non-Keys private sector: 301 of 1,465 (21% subpar)
- All private sector: 320 of 1,539 (21% subpar)
- Voluntary sector: 26 of 158 (16% subpar)
- Local authority sector: 66 of 392 (17% subpar)


The second alternative analysis calculates the proportion of homes that received *at least one subpar rating* among its full inspections in inspection year 2017-18. Results:

- Cambian homes: 41 homes "Inadequate" or "Requires improvement" in at least one inspection, out of 159 inspected (26% subpar)
- Non-Cambian private sector: 307 of 1,380 (22% subpar)
- Keys homes: 20 of 74 (27% subpar)
- Non-Keys private sector: 328 of 1,465 (22% subpar)
- All private sector: 348 of 1,539 (23% subpar)
- Voluntary sector: 27 of 158 (17% subpar)
- Local authority sector: 72 of 392 (18% subpar)

# Reproducibility

To reproduce the findings, you'll need to do the following:

- Ensure that you have installed [Jupyter](http://jupyter.org/), [Python](https://www.python.org/), and the Python libraries listed in [`requirements.txt`](requirements.txt)
- Use Jupyter to run the notebook in the [`notebooks/`](notebooks/) directory. (Shortcut: run `make reproduce`; requires Python 3.)

# Licensing

All code in this repository is available under the [MIT License](https://opensource.org/licenses/MIT). The `inputs/private-sector-ownerships.csv` file is available under the [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0) license. All other data files come directly from Ofsted and are subject to Ofsted's terms. 

# Feedback / Questions?

Contact Jeremy Singer-Vine at [jeremy.singer-vine@buzzfeed.com](jeremy.singer-vine@buzzfeed.com).

Looking for more from BuzzFeed News? [Click here for a list of our open-sourced projects, data, and code](https://github.com/BuzzFeedNews/everything).
