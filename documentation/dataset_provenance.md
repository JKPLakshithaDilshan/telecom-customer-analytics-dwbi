# Dataset Provenance

## Dataset Selection

This project uses the enhanced five-source version of the IBM Telco Customer Churn sample dataset.

The dataset represents 7,043 customers of a fictional telecommunications company operating in California, United States, during the third quarter. It contains customer demographics, geographical information, subscribed services, usage measures, billing information, revenue measures, satisfaction indicators, Customer Lifetime Value (CLTV), and churn information.

The five-source version was selected instead of the combined workbook because it supports realistic multi-source extraction, data integration, data-quality validation, dimensional modelling, ETL implementation, and business intelligence analysis.

## Original Source

**Dataset:** IBM Telco Customer Churn Sample
**Provider:** IBM Cognos Analytics
**Business domain:** Telecommunications
**Geographical coverage:** California, United States
**Reporting period:** Q3 customer snapshot
**Access date:** 16 September 2026

Official dataset description:

https://community.ibm.com/community/user/businessanalytics/blogs/steven-macko/2019/07/11/telco-customer-churn-1113

The dataset describes a fictional telecommunications company. Therefore, it is used as a realistic educational sample rather than as evidence about an actual organisation or its customers.

## Selected Source Files

| Source file | Purpose | Sheet | Records | Attributes | Operational key |
|---|---|---|---:|---:|---|
| `Telco_customer_churn_demographics.xlsx` | Customer demographic information | `Telco_Churn` | 7,043 | 9 | `Customer ID` |
| `Telco_customer_churn_location.xlsx` | Customer geographical information | `Telco_Churn` | 7,043 | 9 | `Customer ID` |
| `Telco_customer_churn_population.xlsx` | ZIP-code population information | `Population` | 1,671 | 3 | `Zip Code` |
| `Telco_customer_churn_services.xlsx` | Services, usage, billing, and revenue information | `Telco_Churn` | 7,043 | 30 | `Customer ID` |
| `Telco_customer_churn_status.xlsx` | Satisfaction, status, CLTV, and churn information | `Telco_Churn` | 7,043 | 11 | `Customer ID` |

A machine-readable inventory of the selected files is maintained in `data/raw/source_manifest.csv`. The manifest records each file name, business role, worksheet name, row count, column count, operational key, and SHA-256 checksum.

## Source Relationships

The operational sources can be integrated through the following relationships:

```text
Demographics.Customer ID → Location.Customer ID
Demographics.Customer ID → Services.Customer ID
Demographics.Customer ID → Status.Customer ID
Location.Zip Code → Population.Zip Code
````

`Customer ID` provides the customer-level integration key across the demographics, location, services, and status sources. `Zip Code` connects customer locations to the population source.

The four customer-level files have the same 7,043 unique customer identifiers. Every ZIP code used in the location source has a corresponding record in the population source.

## Source Grain

The grain of each selected source is defined as follows:

| Source       | Grain                                    |
| ------------ | ---------------------------------------- |
| Demographics | One row per customer                     |
| Location     | One row per customer                     |
| Population   | One row per ZIP code                     |
| Services     | One row per customer for the Q3 snapshot |
| Status       | One row per customer for the Q3 snapshot |

Defining the source grain prevents accidental duplication during joins and supports the design of fact and dimension tables in the data warehouse.

## Raw Data Integrity Verification

The raw files were inspected before ETL development. The initial verification produced the following results:

* All five files are valid Microsoft Excel workbooks.
* The demographics, location, services, and status sources each contain 7,043 customer records.
* The population source contains 1,671 ZIP-code records.
* No fully duplicated rows were identified.
* `Customer ID` is complete and unique in all four customer-level sources.
* `Zip Code` is complete and unique in the population source.
* Customer identifier coverage matches across demographics, location, services, and status.
* Every customer ZIP code in the location source matches a ZIP code in the population source.
* The services and status sources contain the same reporting-quarter value, `Q3`.
* No inconsistencies were identified between `Churn Label` and `Churn Value`.
* The supplied total revenue values are consistent with their contributing revenue fields.
* SHA-256 checksums were recorded to support raw-file integrity and reproducibility.

These checks confirm that the five selected workbooks can be integrated without losing customer records or creating unintended many-to-many relationships.

## Missing-Value Interpretation

The initial profiling identified missing values in the following fields:

| Field            | Missing records | Interpretation                                                    | Planned treatment                                      |
| ---------------- | --------------: | ----------------------------------------------------------------- | ------------------------------------------------------ |
| `Offer`          |           3,877 | The customer did not accept or receive a named offer              | Replace with `No Offer` in the transformed layer       |
| `Internet Type`  |           1,526 | The customer does not have an internet service                    | Replace with `No Internet` in the transformed layer    |
| `Churn Category` |           5,174 | The customer did not churn, so a churn category is not applicable | Replace with `Not Applicable` in the transformed layer |
| `Churn Reason`   |           5,174 | The customer did not churn, so a churn reason is not applicable   | Replace with `Not Applicable` in the transformed layer |

These values are structurally missing because the attributes do not apply to particular customers. They are not treated as random data-quality errors.

The original values will remain unchanged in `data/raw`. Missing-value treatments will be applied only during transformation and documented in the ETL rules.

## Raw Data Preservation

Files stored in `data/raw` are treated as immutable source records.

The following controls apply:

* Raw workbook contents will not be edited manually.
* Column names and source values will not be changed in the raw layer.
* Cleansing and standardisation will occur in the processed or staging layers.
* File checksums will be used to detect unintended source-file changes.
* Generated datasets and analytical outputs will not overwrite the original files.
* Any replacement source file must be recorded in the source manifest and decision log.

This approach preserves data lineage from the source workbooks to the final data warehouse and Power BI dashboard.

## Excluded Combined Workbook

The download also contained the following combined workbook:

```text
Telco_customer_churn.xlsx
```

This workbook is excluded from the primary ETL pipeline because it represents a flattened and narrower version of the same business data. Loading it together with the five selected source files could duplicate customer information and create conflicting definitions.

The five-source version is preferred because it:

* provides clearer operational source boundaries;
* supports genuine multi-source extraction and integration;
* contains richer service, usage, revenue, status, and location attributes;
* enables explicit relationship and referential-integrity validation;
* provides stronger evidence for ETL and data-warehouse design;
* better satisfies the requirements of the DWBI assignment.

The combined workbook may be retained outside the repository for reference, but it will not be used as a production ETL input.

## Dataset Suitability for the DWBI Solution

The selected dataset supports the main components of the proposed solution:

| DWBI component        | Dataset support                                                                                   |
| --------------------- | ------------------------------------------------------------------------------------------------- |
| Business scenario     | Telecom customer retention, service usage, and revenue management                                 |
| Multiple data sources | Five related Excel workbooks                                                                      |
| Data integration      | Customer and ZIP-code relationships                                                               |
| Data quality          | Missing-value treatment, key validation, type conversion, and business-rule validation            |
| Dimensional modelling | Customer, location, service, contract, payment, status, and snapshot dimensions                   |
| Fact modelling        | Customer performance, service usage, revenue, and churn measures                                  |
| Data mart             | Customer Retention and Revenue Data Mart                                                          |
| Business intelligence | Revenue, churn, service adoption, CLTV, satisfaction, and geographical analysis                   |
| Dashboard development | Executive overview, churn analysis, revenue analysis, service analysis, and customer segmentation |

## Temporal Limitation

The dataset is a Q3 customer snapshot rather than a multi-period transaction history. Although the source contains a `Quarter` attribute, it does not provide observations across several quarters or years.

Therefore:

* the project will not generate artificial historical dates;
* the dashboard will not claim to show genuine monthly or yearly churn trends;
* `Tenure in Months` may be used for customer lifecycle analysis, but it will not be presented as calendar time;
* analysis may compare tenure groups, contract types, services, locations, customer segments, and churn outcomes;
* the limitation will be disclosed in the final report and presentation.

The warehouse design may include a snapshot date or date dimension to support future periodic data loads. However, the current analytical results will remain limited to the supplied Q3 snapshot.

## Reproducibility and Lineage

The project maintains data lineage through the following artefacts:

* `data/raw/source_manifest.csv` records the identity and checksum of every selected source file.
* Data-quality scripts and results will be stored under `data/quality`.
* Extraction, transformation, and loading logic will be maintained under `src`.
* Database creation and validation scripts will be maintained under `sql`.
* Important project decisions will be recorded in `decision_log.csv`.
* Process diagrams and supporting evidence will be stored under `documentation`.
* Generated analytical datasets will be stored separately from immutable raw data.

These controls allow another project member to trace a dashboard measure back through the data mart, data warehouse, transformation logic, and original source file.

## Usage and Redistribution

The source is used for educational and portfolio development purposes. The original IBM source must be acknowledged in the report, presentation, repository documentation, and dashboard documentation where appropriate.

Before changing the GitHub repository from private to public, the dataset's applicable source terms and redistribution permissions must be reviewed. If redistribution of the original Excel workbooks is not clearly permitted, the public portfolio version should exclude the raw workbooks and provide source attribution and download instructions instead.

No real customer identities or confidential organisational records are included because the dataset represents a fictional telecommunications company.

## Selection Decision

The IBM five-source Telco Customer Churn dataset is approved as the primary data source for this project.

It provides sufficient data volume, business relevance, analytical depth, multiple-source integration, data-quality opportunities, measurable facts, descriptive dimensions, and dashboard potential for an end-to-end Data Warehousing and Business Intelligence solution.
