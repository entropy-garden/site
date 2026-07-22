---
title: "Data Quality Assessment of an Argentine Airport Dataset"
date: 2026-07-22T15:00:00+03:00
draft: false

description: "A comprehensive data quality assessment of the OurAirports dataset for Argentina using the DAMA-DMBOK framework. The project evaluates completeness, uniqueness, consistency, validity, and accuracy through business rules, spatial analysis, and external reference datasets."

summary: "A practical application of Data Stewardship principles, including business rule validation, identity resolution, spatial matching, and external accuracy assessment."

tags:
  - Data Quality
  - Data Governance
  - Data Stewardship
  - DAMA-DMBOK
  - Python
  - Pandas
  - GeoPandas
  - Aviation
  - Identity Resolution
  - Master Data Management

keywords:
  - data quality
  - data governance
  - data stewardship
  - DAMA
  - DMBOK
  - python
  - airport database
  - aviation
  - identity resolution
  - spatial matching
  - master data management

author: "Paula Petrenko"

showToc: true
TocOpen: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowCodeCopyButtons: true
ShowWordCount: true
comments: false
---


# Data Quality Assessment of an Airport Database for Argentina

The assessment follows the data quality dimensions defined in the DAMA-DMBOK framework and evaluates the dataset through a set of business rules supported by external reference sources where applicable.

## 1. Introduction

In the aviation industry, data quality plays a crucial role, as accuracy and reliability directly impact safety, operational efficiency, and strategic planning. This report assesses the data quality of an airport database for Argentina, focusing on key dimensions such as completeness, uniqueness, validity, consistency, and accuracy.

The dataset used in this analysis was obtained exclusively from:

https://ourairports.com/world.html

All airport types, including closed airports, were retained throughout the data quality assessment. Although these facilities are no longer classified as operational, they remain valid aeronautical entities and may still be relevant for historical records, specialized aviation activities, or emergency use. Excluding them before the assessment could artificially improve several quality metrics and reduce the ability to identify genuine differences between reference sources. For this reason, all airport types were included in the primary analysis, while a supplementary analysis excluding closed airports was performed only to provide additional context for the remaining unmatched records

## 2. Data Dictionary

The dataset schema is defined as follows::

| Column Name | Description                                                   | Data Type | Key Type |
| ----------- | ------------------------------------------------------------- | --------- | -------- |
| ID          | Unique internal identifier of the airport record              | integer   | Primary Key |
| Type        | Classification of the airport (e.g., small_airport, heliport) | string    | No          |
| Name        | Official name of the airport or landing facility              | string    | No          |
| ISO Country | ISO 3166-1 alpha-2 country code (Argentina = AR)              | string    | No          |
| Latitude    | Geographic latitude coordinate in decimal degrees             | float     | No       |
| Longitude   | Geographic longitude coordinate in decimal degrees            | float     | No       |
| Elevation   | Elevation of the airport above sea level (feet)               | float     | No      |
| Ident       | Local airport identifier (primary business key)               | string    | No       |
| GPS Code    | External GPS/ICAO-related code (if available)                 | string    | Alternate Key      |


## 3. Methodology

The data quality assessment follows a structured methodology consisting of four main phases.

### 3.1 Data Loading and Exploration

The dataset is loaded into the analytical environment and an initial exploration is performed to understand its structure, attributes, and overall characteristics. This step provides the necessary context for defining the subsequent quality assessment.

### 3.2 Business Rule Definition

Business rules are defined to represent the expected characteristics of the dataset. These rules establish the criteria against which the data quality dimensions are evaluated throughout the assessment.

### 3.3 Data Quality Assessment

The dataset is evaluated against the defined business rules using the following data quality dimensions:

- **Completeness:** Identification of missing or null values.
- **Uniqueness:** Detection of duplicate records and spatial duplicates.
- **Consistency:** Verification of logical consistency between related attributes.
- **Validity:** Verification that values conform to predefined domains, formats, and acceptable ranges.
- **Accuracy:** Assessment of data correctness through comparison with independent reference sources.

### 3.4 Results Presentation

The assessment results are consolidated into a **Data Quality Scorecard (DQ Scorecard)** to provide a structured summary of the business rule evaluation.

Unlike the previous dimensions, **Accuracy** cannot be evaluated solely through deterministic business rules because correctness must be be verified against independent reference sources. Consequently, the accuracy assessment is presented separately and relies on external validation techniques rather than predefined business rules.

The consolidated results are then interpreted to identify confirmed data quality issues, potential discrepancies, and records requiring manual review.

### 3.5 Conclusion

Finally, the assessment concludes with a brief summary of the overall data quality findings, highlighting the principal strengths of the dataset, the most relevant issues identified, and the main areas requiring further investigation.

## 4. Data Loading and Exploration

This section presents a basic overview of the dataset, including its structure and content. It covers column names, data types, and basic summary statistics to provide an initial understanding of the data.

To gain a clearer perspective, the first five records of the dataset are displayed. This allows for a quick inspection of how the data is structured and the type of information contained in each field.

| id | type | name | iso_country | latitude_deg | longitude_deg | elevation_ft | ident | gps_code |
|---:|:----------------|:------------------------------|:-----------:|-------------:|--------------:|-------------:|:---------|:---------|
| 35333 | small_airport | Cullen Airport | AR | -52.885740 | -68.414956 | 132 | AR-0001 | NaN |
| 35334 | small_airport | Estancia Los Cerros Airport | AR | -54.343000 | -67.837532 | 1914 | AR-0002 | NaN |
| 35335 | small_airport | Rio Bellavista Airport | AR | -53.982700 | -68.523598 | 201 | AR-0003 | NaN |
| 35398 | small_airport | Merlo Airport | AR | -32.358200 | -65.017403 | 796 | AR-0004 | NaN |
| 35399 | small_airport | Bragado Airport | AR | -35.145811 | -60.480294 | 196 | AR-0005 | SA2X |


In the following output, the basic structure of the DataFrame is presented. The dataset consists of 942 entries and 9 columns.

A summary of data completeness and structure is provided below:

* **id**: No missing values detected.  
* **type**: No missing values detected.  
* **name**: No missing values detected.  
* **iso_country**: No missing values detected.  
* **latitude_deg**: No missing values detected.  
* **longitude_deg**: No missing values detected.  
* **elevation_ft**: Contains 34 missing values.
* **ident**: No missing values detected.  
* **gps_code**: Contains a high number of missing values (717 null entries).  

The dataset includes integer, float, and object data types. Missing values are primarily concentrated in `elevation_ft` and `gps_code`, which will be further analyzed in the data quality assessment phase.

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 942 entries, 0 to 941
    Data columns (total 9 columns):
     #   Column         Non-Null Count  Dtype  
    ---  ------         --------------  -----  
     0   id             942 non-null    int64  
     1   type           942 non-null    object 
     2   name           942 non-null    object 
     3   iso_country    942 non-null    object 
     4   latitude_deg   942 non-null    float64
     5   longitude_deg  942 non-null    float64
     6   elevation_ft   908 non-null    float64
     7   ident          942 non-null    object 
     8   gps_code       225 non-null    object 
    dtypes: float64(3), int64(1), object(5)
    memory usage: 66.4+ KB


The following summary statistics provide an initial overview of the numerical attributes `latitude_deg`, `longitude_deg`, and `elevation_ft`.

- **Latitude and Longitude:** The latitude values range from approximately -54.84 to -22.12, while the longitude values range from -72.89 to -53.67. These ranges appear to be consistent with the geographic extent of Argentina. A formal validation against the country's boundaries will be performed during the data quality assessment phase.

- **Elevation:** Elevation values range from 6 to 13,000 feet. Although the minimum value appears reasonable, the maximum value is unusually high for an airport in Argentina and will be further evaluated as part of the validity assessment.

Overall, these descriptive statistics provide an initial understanding of the numerical data and help identify potential anomalies that will be investigated through the business rules defined for this assessment.

| Statistic | latitude_deg | longitude_deg | elevation_ft |
|:---------|-------------:|--------------:|-------------:|
| Count | 942.000000 | 942.000000 | 908.000000 |
| Mean | -35.390905 | -62.908536 | 801.696035 |
| Standard Deviation | 6.671947 | 4.095665 | 1296.457558 |
| Minimum | -54.843300 | -72.885820 | 6.000000 |
| 25th Percentile | -38.005901 | -65.493250 | 131.750000 |
| Median (50%) | -34.444700 | -62.180665 | 316.000000 |
| 75th Percentile | -31.508499 | -59.450899 | 903.000000 |
| Maximum | -22.123510 | -53.673332 | 13000.000000 |


## 5. Business Rules Definition

The business rules governing the dataset are formally defined as a set of data quality constraints. These rules translate business requirements into measurable validation criteria and serve as the reference framework for the subsequent data quality assessment.

The business rules used in this assessment are summarized below:

| Rule ID | Business Rule                                                     | Dimension    | Severity |
|---------|-------------------------------------------------------------------|--------------|----------|
| DQ-001  | `ident` must not be null                                          | Completeness | High     |
| DQ-002  | Coordinates must not be null (`latitude`, `longitude`)           | Completeness | High     |
| DQ-003  | Elevation must not be null                                        | Completeness | Medium   |
| DQ-004  | Every airport must have a unique identifier                       | Uniqueness   | High     |
| DQ-005  | Coordinates must fall within Argentina boundaries                 | Validity     | High     |
| DQ-006  | Elevation must fall within the valid elevation range for Argentina.                     | Validity     | Medium   |
| DQ-007  | If a 'gps_code' is available, its value must match 'ident'                    | Consistency  | Medium     |
| DQ-008  | Coordinates must be unique within a 3 km spatial radius         | Uniqueness   | High     |    |

These rules are implemented as deterministic validation checks and constitute the baseline for assessing compliance across the selected data quality dimensions.

## 6. Data Quality Assessment

### 6.1 Data Completeness

The first dimension evaluated is **completeness**, which measures the extent to which all required data is present in the dataset.

As observed during the initial data exploration, missing values were identified in the `elevation_ft` and `gps_code` columns.

| Field | Null Values | Completeness (%) |
|:---------------|------------:|-----------------:|
| `gps_code` | 717 | 23.89 |
| `elevation_ft` | 34 | 96.39 |
| `id` | 0 | 100.00 |
| `type` | 0 | 100.00 |
| `name` | 0 | 100.00 |
| `iso_country` | 0 | 100.00 |
| `latitude_deg` | 0 | 100.00 |
| `longitude_deg` | 0 | 100.00 |
| `ident` | 0 | 100.00 |


### 6.2 Data Uniqueness

The uniqueness assessment focuses on the attributes most likely to uniquely identify an airport or heliport.

The following fields are included in the analysis:

- `gps_code`
- `ident`
- `latitude_deg`
- `longitude_deg`

These attributes are evaluated to identify potential duplicate records that may represent the same airport or heliport. In the case of latitude and longitude, uniqueness is assessed as an approximation based on spatial proximity rather than exact equality.

**Ident**: No duplicate values were detected in the `ident` field. This indicates full compliance with the uniqueness constraint defined for the primary business identifier.

**GPS Code**: No duplicate values were detected in the `gps_code` field among non-null records.


| Field | Duplicate Count | Total Records | Uniqueness (%) |
|:-----------|----------------:|--------------:|---------------:|
| `ident` | 0 | 942 | 100.00 |
| `gps_code` | 0 | 225 | 100.00 |


**Spatial Uniqueness**: To identify potential spatial duplicates, a proximity-based clustering analysis was performed using a 2 km radius. Heliports, balloonports, and closed airports were excluded from this assessment. Heliports are commonly located in close proximity within urban areas, while balloonports and closed airports are not relevant to the objective of identifying potential duplicates among active airports. Including these facility types would introduce expected proximity clusters and increase the likelihood of false positives. Consequently, this analysis focuses on active airport records, where close geographic proximity is more likely to indicate duplicate or inconsistent location data rather than distinct facilities.

Records belonging to clusters with geo_cluster_size > 1 are considered potential cases requiring further review. The first five records are displayed below for inspection purposes.


| id | type | name | iso_country | latitude_deg | longitude_deg | elevation_ft | ident | gps_code | geo_cluster_size |
|---:|:----------------|:----------------------------------|:-----------:|-------------:|--------------:|-------------:|:---------|:---------|-----------------:|
| 35329 | small_airport | Lago Fagnano North Airport | AR | -54.499699 | -67.173103 | 600 | SA15 | SA15 | 2 |
| 41560 | small_airport | Tolhuin Lago Fagnano Airport | AR | -54.499596 | -67.172084 | 90 | SAWL | SAWL | 2 |
| 348446 | small_airport | Campamento Darwin Airport | AR | -47.896640 | -66.460930 | 384 | AR-0681 | NaN | 2 |
| 348448 | small_airport | Campamento Darwin West Airport | AR | -47.880520 | -66.471050 | 325 | AR-0683 | NaN | 2 |
| 348749 | small_airport | Stroeder Southeast Airport | AR | -40.209680 | -62.604850 | 92 | AR-0725 | NaN | 2 |

>The spatial uniqueness assessment identified 31 airport records belonging to geographic clusters within a 2 km radius. These records represent 4.10% of the evaluated airports and require further review to determine whether they correspond to duplicate records or distinct facilities located in close proximity. The remaining 95.90% satisfy the spatial uniqueness criterion.


### 6.3 Consistency

The third dimension evaluated is **consistency**, which measures whether related data values are logically coherent across the dataset.

For this analysis, the relationship between the `ident` and `gps_code` fields is evaluated. Records where both values are present but do not match are identified as potential consistency issues.

The first five records with identified inconsistencies are displayed below for inspection purposes.


| id | type | name | iso_country | latitude_deg | longitude_deg | elevation_ft | ident | gps_code | equal |
|---:|:----------------|:----------------------------------|:-----------:|-------------:|--------------:|-------------:|:---------|:---------|:-----:|
| 35399 | small_airport | Bragado Airport | AR | -35.145811 | -60.480294 | 196 | AR-0005 | SA2X | False |
| 38698 | small_airport | Bahia Blanca Aeroclub Airport | AR | -38.672059 | -62.352846 | 49 | AR-0023 | SA92 | False |
| 38699 | small_airport | Balcarce Aeroclub Airport | AR | -37.915600 | -58.344200 | 393 | AR-0024 | SA20 | False |
| 38700 | small_airport | Cipoletti Aeroclub Airport | AR | -38.893101 | -67.988602 | 885 | AR-0025 | SAHI | False |
| 38703 | small_airport | Tandil Aeroclub Airport | AR | -37.265598 | -59.093300 | 583 | AR-0028 | SA1B | False |



>The consistency assessment identified 46 inconsistent records. These records represent 4.88% of the dataset and require further review. The remaining 95.12% satisfy the consistency criterion.


### 6.4 Validity

Validity assesses whether attribute values are plausible and consistent with real-world conditions. This section focuses on evaluating the geographic coordinates and elevation values to identify records that may contain invalid or unrealistic information.

The exploratory analysis presented in **Section 4.1** showed that airport elevations range from **6 ft** to **13,000 ft**. To assess the plausibility of these values, they are compared against the geographical elevation limits of Argentina. According to the National Geographic Institute (IGN), the country's highest point is **Cerro Aconcagua** (6,961 m), while its lowest point is the **Gran Bajo de San Julián** (-105 m).¹ Since all recorded airport elevations fall within these geographical limits, no implausible elevation values were identified.

This validation confirms that the recorded elevation values are **plausible**, rather than **correct**. Their correctness is evaluated separately in the **Accuracy** section through comparison with independent reference sources.

Geographic validity is then assessed by visualizing the location of airports and heliports on a map of Argentina. This preliminary inspection provides an intuitive overview of the spatial distribution of the dataset and helps identify records that appear to fall outside the country's boundaries or in other implausible geographic locations. The visual assessment is subsequently complemented by spatial validation against Argentina's official boundaries.


    
![Distribution of airports and heliports in Argentina](/images/argentina.png)
    


The map provides a preliminary overview of the spatial distribution of airports and heliports across Argentina. Some records appear to be located over bodies of water and are examined further to assess the validity of their geographic coordinates.

A spatial validation is then performed using a geographic database of ocean water bodies. Records whose coordinates intersect these polygons are extracted into a separate dataset for individual review. The first five records of this subset are displayed below for inspection purposes.t these polygons are extracted into a separate dataset for individual review.

| id | type | name | iso_country | latitude_deg | longitude_deg | elevation_ft | ident |
|---:|:----------|:------------------------------------------|:-----------:|-------------:|--------------:|-------------:|:---------|
| 38721 | heliport | Aries Heliport | AR | -52.6831 | -68.0419 | NaN | AR-0046 |
| 38781 | heliport | Club Nautico San Isidro Heliport | AR | -34.4614 | -58.5003 | 6 | AR-0106 |
| 38861 | heliport | Heliplataforma Am I Heliport | AR | -52.5192 | -68.3858 | 98 | AR-0186 |
| 38862 | heliport | Heliplataforma Carina/Total Fina ELF | AR | -52.7572 | -67.2194 | 30 | AR-0187 |
| 38863 | heliport | Heliplataforma/Am-2 Heliport | AR | -52.5489 | -68.3125 | 134 | AR-0188 |

>The validity assessment identified 21 records whose coordinates intersect ocean water bodies. These records represent 2.23% of the dataset and require further review. The remaining 97.77% satisfy the validity criterion.


### 6.5 Accuracy

Accuracy assesses the correctness of the data by comparing it with independent reference sources. Airport elevation values are first validated using the Open-Elevation API. The MADHEL airport database is then used to verify airport records and identifiers, providing an additional assessment of data accuracy.

**Elevation Validation**: An elevation difference of up to 100 feet was considered acceptable, as minor discrepancies between independent data sources are expected due to differences in measurement methodologies and data resolution. Values exceeding this threshold were flagged as potential accuracy issues.

The first five records with elevation differences greater than 100 feet are displayed below for inspection purposes.

| id | type | name | iso_country | latitude_deg | longitude_deg | elevation_ft | ident | gps_code | elevation_api | elevation_api_ft |
|---:|:----------------|:--------------------------------|:-----------:|-------------:|--------------:|-------------:|:---------|:---------|--------------:|-----------------:|
| 35334 | small_airport | Estancia Los Cerros Airport | AR | -54.343000 | -67.837532 | 1914 | AR-0002 | NaN | 82.0 | 269.03 |
| 35335 | small_airport | Rio Bellavista Airport | AR | -53.982700 | -68.523598 | 201 | AR-0003 | NaN | 95.0 | 311.68 |
| 35398 | small_airport | Merlo Airport | AR | -32.358200 | -65.017403 | 796 | AR-0004 | NaN | 843.0 | 2765.75 |
| 42800 | small_airport | Agro Servicio Yoris Airport | AR | -26.583055 | -64.529724 | 1043 | AR-0013 | NaN | 355.0 | 1164.70 |
| 35409 | closed | Saenz Peña Airport | AR | -26.815800 | -60.448299 | 20 | AR-0015 | NaN | 91.0 | 298.56 |


>The accuracy assessment was performed on 906 records with available elevation values, excluding 36 records with missing data. The assessment identified 68 records with elevation differences exceeding the 100-foot tolerance. These records represent 7.51% of the assessed records and require further review. The remaining 92.49% satisfied the accuracy criterion.


**Airport Record Matching** The MADHEL airport database is used as an independent reference source for record matching. The first five records are displayed below to provide an overview of its structure and key attributes.


| name | idtext | n_id | iata | lat | lon | traffic_usage | traffic_type | waypoint_type |
|:------------------------------------|:------:|:----:|:----:|---------:|----------:|:-------------|-------------:|:-------------|
| CORONEL BOGADO / AGROSERVICIOS | NaN | ACB | NaN | -33.272260 | -60.570660 | PRIVADO | 2 | AD |
| GENERAL ACHA | NaN | ACH | NaN | -37.401640 | -64.613510 | PUBLICO | 2 | AD |
| ARRECIFES / LA CURA MALAL | NaN | ACM | NaN | -34.075740 | -60.141700 | PRIVADO | 2 | AD |
| LA BANDA / ALAS DEL NORTE | NaN | ADN | NaN | -27.689278 | -64.167861 | NaN | 2 | AD |
| PUERTO DESEADO | SAWD | ADO | PUD | -47.735110 | -65.904100 | PUBLICO | 2 | AD |


Since each data source uses its own internal identifiers, records cannot be matched solely by their primary keys. Therefore, the record matching process is performed in two stages. First, a full outer join is carried out using the airport business identifier to identify records present in both datasets, as well as records unique to each source. Subsequently, records without an identifier match are evaluated using geographic proximity to determine whether they represent the same real-world airport despite using different identifiers.

>The OurAirports dataset contains 942 airport records, while the MADHEL reference dataset contains 723 records. An initial record matching based on airport identifiers identified 119 matching records across both datasets. In addition, 823 records were found only in the OurAirports dataset, while 604 records were unique to the MADHEL dataset. 


Geographic proximity is used as a secondary matching criterion for records that could not be linked through airport identifiers. A maximum distance of 500 meters was selected to account for minor positional differences between independent data providers. Such differences may arise because each dataset can represent a different reference point within the airport (e.g., the Airport Reference Point, runway threshold, or geometric center), as well as from variations in coordinate precision and data collection methodologies. Airports located within 500 meters of each other are therefore considered potential matches representing the same real-world entity.

>0 OurAirports records and 2 MADHEL records were excluded from the spatial matching due to missing coordinates.
The spatial matching process identified 499 additional record pairs within a 500-meter threshold.
Following both the identifier-based and geographic matching stages, 324 records from the OurAirports dataset and 105 records from the MADHEL dataset remained unmatched.


The table below presents the first five records that remained unmatched after completing both matching stages. Although these airports could not be reconciled with the MADHEL reference dataset, the nearest airport candidate is included together with the calculated geographic distance. This information facilitates the manual review of unmatched records and helps determine whether they correspond to missing reference data, closed airports, or airports represented with substantially different locations.


| id | type | name_ourairports | name | ident | idtext | spatial_match | distance_m |
|---:|:----------------|:------------------------------|:-------------------------------------------|:---------|:------:|:-------------:|-----------:|
| 35333 | small_airport | Cullen Airport | RÍO CULLEN / TOTAL AUSTRAL | AR-0001 | NaN | False | 4092.09 |
| 35334 | small_airport | Estancia Los Cerros Airport | RÍO GRANDE / HELIPUERTO EA. RÍO EWAN | AR-0002 | NaN | False | 42307.93 |
| 35335 | small_airport | Rio Bellavista Airport | BAHÍA SAN SEBASTIÁN / PLANTA CRUZ DEL SUR | AR-0003 | NaN | False | 75293.45 |
| 35398 | small_airport | Merlo Airport | YACANTO DE CALAMUCHITA / ROCA VAL | AR-0004 | NaN | False | 39981.13 |
| 333805 | small_airport | Los Rulos Airport | BANDERA / AGROSERVICIO D.ª TERESA | AR-0006 | NaN | False | 52107.22 |



> Following the exclusion of closed airports, 67.48% of the active OurAirports dataset was successfully matched against the MADHEL reference dataset. The remaining 293 records (32.52%) could not be matched and require further review. A total of 31 closed airports were excluded from this analysis.


## 7. Results Presentation

### 7.1 Business Rule Scorecard

The results obtained throughout the assessment are consolidated into two complementary summaries.

The **Business Rule Scorecard** provides a structured overview of the quality rules evaluated across the dimensions of Completeness, Uniqueness, Validity, and Consistency. Each rule is assessed individually to identify specific quality issues and to facilitate comparison across dimensions.

The acceptable compliance threshold depends on the intended use of the dataset and the criticality of each business rule. In operational aviation systems, for example, some rules may require 100% compliance, while other datasets may tolerate a lower level of conformity depending on their purpose.

Since no specific business context has been defined for this assessment, a **98% compliance threshold** is adopted as a general benchmark to simplify the evaluation. Business rules meeting or exceeding this threshold are considered satisfactory, while rules below the threshold are flagged for further review. This threshold is intended solely for the purposes of this assessment and should not be interpreted as a universal data quality standard.

| Rule ID | Business Rule | Dimension | Severity | Compliance |
|:--------|:--------------|:----------|:--------:|-----------:|
| DQ-001 | `ident` must not be null | Completeness | High | 🟢 100% |
| DQ-002 | Coordinates must not be null (latitude, longitude) | Completeness | High | 🟢 100%|
| DQ-003 | Elevation must not be null | Completeness | Medium | 🟡 96.39% |
| DQ-004 | Every airport must have a unique identifier | Uniqueness | High | 🟢 100% |
| DQ-005 | Coordinates must fall within Argentina boundaries | Validity | High |🟡 97.77%|
| DQ-006 | Elevation must fall within the valid elevation range for Argentina. | Validity | Medium |🟢 100% |
| DQ-007 | If a 'gps_code' is available, its value must match 'ident' | Consistency | Medium|🟡  95.12%|
| DQ-008 | Coordinates must be unique within a 2 km spatial radius | Uniqueness | High |🟡  95.90%|

**Compliance Status**

- 🟢 Meets the 98% compliance threshold.
- 🟡 Below the compliance threshold and requires review.

### 7.2 Accuracy Assessment

Unlike the previous data quality dimensions, **Accuracy** cannot be evaluated exclusively through predefined business rules because correctness must be assessed against independent reference sources rather than internal validation rules.

For this reason, the accuracy assessment is presented separately. The evaluation includes two complementary analyses:

- **Elevation Accuracy**, assessed by comparing airport elevations against an independent elevation reference.
- **Overall Matching Rate**, assessed by comparing airport records against the MADHEL reference dataset using both identifier-based and geographic matching.

| Accuracy Indicator | Assessment | Result |
|:-------------------|:-----------|-------:|
| Elevation Accuracy | Agreement with external elevation reference | 92.49% |
| Overall Matching Rate | Records successfully matched with the MADHEL reference dataset | 67.48%  |

The indicators presented above should not be interpreted as pass/fail criteria. Instead, they measure the level of agreement between the evaluated dataset and independent reference sources. Differences may reflect discrepancies between sources, differences in dataset scope, or records requiring manual review rather than confirmed data quality issues.

## 8. Conclusion

This assessment evaluated the quality of the OurAirports dataset for Argentina across the dimensions of Completeness, Uniqueness, Validity, Consistency, and Accuracy using a combination of business rules and external reference sources.

The business rule assessment indicates that the dataset demonstrates a generally high level of quality. Most rules achieved full compliance, while the remaining findings were limited to relatively small proportions of the dataset and primarily represent records requiring further review rather than confirmed quality defects.

It is important to recognize that data quality cannot be described as inherently "good" or "bad" without considering its intended use. Data quality is fundamentally determined by **fitness for purpose**. For applications requiring an open and comprehensive aviation reference dataset, the results suggest that the dataset provides an adequate level of quality. However, for operational or safety-critical aviation systems, more authoritative or domain-specific reference datasets may be preferable, depending on the business requirements.

The accuracy assessment also illustrates an important aspect of Master Data Management: **identity resolution**. In this study, record linkage was performed using deterministic matching techniques, combining exact identifier matching with geographic proximity rules. This approach provides high-confidence matches but does not resolve all legitimate correspondences between independent datasets.

Consequently, records that could not be matched should not be interpreted as inaccurate. They may instead reflect differences in identifiers, dataset scope, maintenance practices, or the inclusion of facilities such as private airstrips or landing areas. In practice, unresolved records represent candidates for further investigation by a Data Steward, who may apply additional business knowledge or more sophisticated matching techniques, such as probabilistic or machine learning-based entity resolution, where appropriate.
