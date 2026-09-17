# Task 1 — Business Scenario and Project Objectives

## 1. Project Title

**Telecommunication Customer Revenue, Service Usage and Churn Analytics Data Warehouse and Business Intelligence Solution**

## 2. Business Scenario

A fictional telecommunications company operating in California provides phone, internet, and related digital services to a large customer base.

The company maintains customer information across separate operational data sources. These sources contain demographic details, geographical information, subscribed services, usage measures, contracts, payment methods, charges, revenue, customer satisfaction, Customer Lifetime Value (CLTV), and churn outcomes.

Although this information is available, it is distributed across multiple source files. This makes it difficult for managers and analysts to obtain a consistent view of customer behaviour, revenue performance, service adoption, and customer churn.

The organisation requires an integrated Data Warehousing and Business Intelligence solution that consolidates these operational sources, improves data quality, supports multidimensional analysis, and presents actionable information through interactive Power BI dashboards.

## 3. Business Problem

Customer churn directly affects recurring revenue, customer acquisition costs, and long-term profitability. The company needs to understand which customers are leaving, why they are leaving, which services and contract conditions are associated with churn, and which high-value customers require retention attention.

The current source-based reporting approach presents several challenges:

- Customer information is distributed across multiple operational sources.
- Managers do not have a consolidated customer-level analytical view.
- Churn, revenue, service, location, and demographic information cannot be analysed efficiently together.
- Missing values require consistent business interpretation.
- Operational source structures are not optimised for analytical queries.
- Decision-makers lack interactive dashboards for monitoring important performance indicators.
- High-value customers with a strong likelihood of churn are difficult to identify.
- Churn causes cannot be compared easily across customer segments, services, contracts, and locations.

## 4. Proposed Solution

The proposed solution will implement an end-to-end Data Warehousing and Business Intelligence pipeline.

The solution will:

1. Extract data from five IBM Telco Customer Churn Excel sources.
2. Preserve the original source files in an immutable raw-data layer.
3. Profile and validate source data quality.
4. Clean, standardise, and integrate customer-related records.
5. Load cleaned data into a staging area.
6. Transform the integrated data into a dimensional data warehouse.
7. Create a Customer Retention and Revenue Data Mart.
8. Connect Power BI to the analytical layer.
9. Develop interactive dashboards and business measures.
10. generate evidence-based findings and recommendations for decision-makers.

## 5. Primary Project Objective

The primary objective is to develop a reliable and reproducible DWBI solution that integrates customer, location, service, billing, revenue, satisfaction, and churn data to support customer-retention and revenue-management decisions.

## 6. Specific Objectives

The project aims to:

- integrate five related operational data sources into a consistent analytical model;
- verify key completeness, uniqueness, and referential integrity;
- identify and treat missing values according to documented business rules;
- standardise data types, categories, column names, and derived attributes;
- design a dimensional model suitable for customer, revenue, usage, and churn analysis;
- preserve historical loading capability through warehouse keys and snapshot-oriented design;
- create a focused Customer Retention and Revenue Data Mart;
- calculate relevant business KPIs using SQL and DAX;
- develop interactive Power BI report pages for different analytical perspectives;
- identify the principal characteristics and reasons associated with churn;
- identify high-value customers who may require retention intervention;
- compare customer value, service adoption, and churn across meaningful segments;
- provide actionable recommendations based on validated analytical results;
- maintain complete technical documentation, data lineage, and reproducibility evidence.

## 7. Stakeholders

| Stakeholder | Information requirement |
|---|---|
| Executive Management | Overall customer, revenue, churn, and CLTV performance |
| Customer Retention Team | High-risk and high-value customers requiring retention action |
| Marketing Team | Customer segments, offers, contracts, and service-adoption patterns |
| Finance Team | Monthly charges, total revenue, refunds, and customer value |
| Service Management Team | Service subscriptions, internet types, usage, and support-related patterns |
| Regional Management | Customer, revenue, and churn performance by city and ZIP code |
| Data and BI Team | Reliable integrated data, quality controls, warehouse structures, and reusable measures |

## 8. Key Business Questions

The solution will address the following questions:

1. How many customers are active, retained, joined, or churned?
2. What is the overall customer churn rate?
3. What revenue is represented by the current customer base?
4. Which customer segments have the highest churn rate?
5. Which contract types and payment methods are associated with higher churn?
6. Which internet and telecommunications services are associated with customer retention or churn?
7. What are the most common churn categories and churn reasons?
8. Which cities and ZIP-code areas show comparatively high churn?
9. How do tenure, satisfaction, monthly charges, and service usage relate to churn?
10. Which customers have high CLTV but also show churn risk?
11. How does service adoption vary across customer groups?
12. Which customer segments should be prioritised for retention campaigns?
13. How do revenue and churn patterns vary by demographic and geographical characteristics?
14. What practical actions could improve customer retention and protect revenue?

## 9. Proposed Key Performance Indicators

| KPI | Definition |
|---|---|
| Total Customers | Distinct count of customers in the analytical dataset |
| Churned Customers | Number of customers whose churn value is equal to 1 |
| Churn Rate | Churned customers divided by total customers |
| Retained Customers | Number of customers who remained with the company |
| New Customers | Number of customers classified as joined |
| Total Revenue | Sum of customer total revenue |
| Average Monthly Charge | Average current monthly charge per customer |
| Average Customer Tenure | Average tenure in months |
| Average CLTV | Average predicted Customer Lifetime Value |
| High-Value Customers | Customers classified within the selected high-CLTV threshold |
| High-Value Customers at Risk | High-CLTV customers with elevated churn risk or churn status |
| Average Satisfaction Score | Average customer satisfaction score |
| Average Monthly Data Usage | Average monthly data download for eligible customers |
| Service Adoption Rate | Customers subscribed to a service divided by eligible customers |
| Revenue at Risk | Revenue associated with churned or high-risk customer segments |

Final KPI calculations and thresholds will be documented in the data dictionary and Power BI measure catalogue.

## 10. Project Scope

### Included

- IBM Telco Customer Churn five-source dataset
- source-data inventory and provenance documentation;
- source profiling and data-quality assessment;
- extraction from Microsoft Excel workbooks;
- customer and ZIP-code source integration;
- staging-layer implementation;
- dimensional warehouse design;
- Customer Retention and Revenue Data Mart;
- SQL-based validation and analytical queries;
- Python-assisted ETL and quality checks;
- Power BI data modelling and DAX measures;
- interactive dashboards;
- business findings and recommendations;
- technical documentation, report, presentation, and repository evidence.

### Excluded

- real-time streaming ingestion;
- deployment to a live telecommunications production environment;
- integration with a real customer relationship management platform;
- automated customer communication or campaign execution;
- live billing or payment processing;
- collection of real personally identifiable customer information;
- artificial generation of monthly or yearly historical records;
- production-grade churn machine-learning model deployment.

Predictive fields already provided by the dataset, such as churn score and CLTV, may be analysed. However, developing a new production machine-learning model is outside the main scope of this DWBI assignment.

## 11. Assumptions

The project is based on the following assumptions:

- `Customer ID` consistently identifies a customer across the customer-level source files.
- `Zip Code` connects customer locations with population information.
- The supplied source records represent a consistent Q3 reporting snapshot.
- Churn labels, churn values, and financial measures follow the definitions provided by the data source.
- Missing churn categories and reasons for non-churned customers mean that the fields are not applicable.
- Missing internet types represent customers without an internet service.
- Missing offer values represent customers without a named offer.
- The fictional dataset is sufficiently representative for demonstrating a realistic DWBI implementation.
- The source workbooks will remain unchanged after ingestion.

## 12. Constraints and Limitations

The project has the following limitations:

- The dataset represents a fictional telecommunications company.
- The available data represents a single Q3 customer snapshot.
- Genuine month-over-month and year-over-year analysis is not possible.
- `Tenure in Months` represents customer lifecycle duration and not calendar time.
- Churn score and CLTV were calculated externally, and their original predictive formulas are not supplied.
- The dataset does not contain detailed individual transactions, invoices, support tickets, or campaign-response events.
- Analytical relationships can identify associations but do not independently establish causation.
- Dashboard conclusions are limited to the supplied population and source definitions.

These limitations will be stated clearly in the final report and presentation.

## 13. Expected Deliverables

The completed solution is expected to produce:

- verified raw data sources and a source manifest;
- dataset provenance and business-scenario documentation;
- source profiling and data-quality reports;
- documented transformation and validation rules;
- a staging database;
- a dimensional data warehouse;
- a Customer Retention and Revenue Data Mart;
- SQL scripts for creation, loading, and validation;
- an automated or repeatable ETL process;
- a Power BI semantic model;
- an interactive multi-page Power BI dashboard;
- a KPI and DAX measure catalogue;
- architecture, ETL, and dimensional-model diagrams;
- documented business findings and recommendations;
- a final academic report;
- a presentation and demonstration evidence;
- an orderly GitHub repository with meaningful branches, commits, and pull requests.

## 14. Expected Business Value

The proposed DWBI solution will provide a consolidated and trustworthy view of telecom customer performance.

It will enable the fictional organisation to:

- detect customer segments with elevated churn;
- protect revenue associated with valuable customers;
- understand the main reported reasons for customer departure;
- compare service and contract performance;
- identify opportunities for targeted retention strategies;
- improve access to consistent management information;
- reduce manual source reconciliation;
- support faster and more evidence-based decision-making.

## 15. Success Criteria

The project will be considered successful when:

- all five selected sources are extracted and integrated without unintended customer loss or duplication;
- documented data-quality rules are applied consistently;
- fact and dimension tables follow a clearly defined grain;
- warehouse and data-mart relationships pass referential-integrity validation;
- SQL totals reconcile with transformed-source totals;
- Power BI measures reconcile with validated SQL results;
- dashboards answer the defined business questions;
- reported findings are supported by reproducible calculations;
- project limitations and assumptions are communicated transparently;
- another project member can reproduce the main pipeline using repository instructions.
