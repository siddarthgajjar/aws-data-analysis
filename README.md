# Data Analytics Projects

## Project 1: AWS Data Analytics for Vancouver Bikeways

### Project Overview
This project demonstrates the design and implementation of an end-to-end data analytics platform leveraging AWS services to analyze bikeway infrastructure growth in Vancouver. Using open data from the City of Vancouver, we explored trends in bike route development to assess the city's commitment to sustainable transportation and support informed urban planning decisions.

---

### Table of Contents
- [Project Objective](#project-objective)
- [Dataset](#dataset)
- [Tools and Technologies](#tools-and-technologies)
- [Methodology](#methodology)
  - [Data Ingestion](#data-ingestion)
  - [Data Profiling](#data-profiling)
  - [Data Cleaning](#data-cleaning)
  - [ETL Pipeline Design](#etl-pipeline-design)
  - [Descriptive Analysis](#descriptive-analysis)
  - [Exploratory Analysis](#exploratory-analysis)
  - [Data Enrichment](#data-enrichment)
  - [Data Governance](#data-governance)
  - [Data Observability](#data-observability)
- [Results](#results)
- [Future Improvements](#future-improvements)
- [How to Replicate](#how-to-replicate)
- [License](#license)

---

### Project Objective
The primary goal of this project is to analyze the growth patterns of Vancouver's bikeway network over time, providing insights into the city’s efforts toward sustainable transportation. The analysis identifies:
- The number of bike routes constructed annually.
- Trends in bikeway segment lengths.
- Key periods of growth and stagnation.

---

### Dataset
- **Source**: [City of Vancouver Open Data Portal](https://opendata.vancouver.ca/explore/dataset/bikeways/information/)
- **Description**: Bikeway data, including attributes such as bike route names, segment lengths, and years of construction.
- **Size**: 194 rows x 24 columns (after cleaning).
- **Data Quality**:
  - 88% valid cells.
  - No duplicate rows.
  - 12% missing data, handled during cleaning.

Dataset: The Vancouver Bikeways dataset contains information about the bikeway segments in the city, including attributes such as:

- **Object ID**: Unique identifier for each bikeway segment
- **Bike Route Name**: Name of the bike route as it appears on wayfinding signs (typically the name of the street)
- **Street Name**: Name of the street where the bikeway segment is located
- **Bikeway Type**: Type of bikeway facility (e.g., Protected Bike Lanes, Local Street, Painted Lanes, Shared Lanes)
- **Subtype**: Subtype of bikeway type (e.g., Off Street Bike Path, Raised On Street, Street Level On Street)
- **Status**: Status of the bikeway segment (Active or Legacy)
- **Street Segment Type**: Type of street segment (e.g., Arterial, Collector, Lane, Residential)
- **Overall Direction**: The overall direction of the bikeway (e.g., EW = East-West, NS = North-South)
- **Bikeway Direction**: Direction of bike travel on the route (e.g., Two Way, One Way, Bidirectional)
- **Vehicle Direction**: Direction of vehicle travel on the route (e.g., Two Way, One Way, Off-street)
- **Speed Limit**: Speed limit for vehicles in kilometers per hour (Zero or null for off-street routes)
- **Surface Type**: Whether the surface is paved or unpaved
- **AAA Network**: Whether the segment is part of the All Ages and Abilities (AAA) network
- **AAA Segment**: Whether the segment meets the criteria for All Ages and Abilities cycling
- **W/N Bound Type**: Type of bikeway facility in the West or Northbound direction
- **E/S Bound Type**: Type of bikeway facility in the East or Southbound direction
- **Snow Removal**: Whether the bikeway is on a snow removal route
- **Segment Length**: Approximate length of the centerline of the segment in meters
- **Year of Construction**: Year the segment became an active bikeway (may be zero or blank if not available)
- **Construction Note**: Notes on construction and upgrades
- **Upgrade Year**: Year when the segment was last upgraded, if applicable
- **Notes**: General notes not included in other attributes
- **Geom**: Spatial representation of the bikeway segment
- **Geo Point 2D**: Geographic point of the bikeway segment (latitude and longitude)

![Dataset Overview](/images/Dataset.png))

---

### Tools and Technologies
This project utilized the following AWS services:
- **Amazon S3**: Storage for raw and processed data.
- **AWS Glue**: ETL processes and data preparation.
- **AWS Glue DataBrew**: Visual data cleaning and profiling.
- **Amazon Athena**: Querying enriched datasets.
- **Amazon CloudWatch**: Monitoring and observability.
- **Amazon KMS**: Data encryption and security.


![Tools Overview](/images/city_of_vancouver.png)

---

### Methodology

#### Data Ingestion
- Raw data ingested into Amazon S3 with a structured folder format for organization.
- Data currency: Weekly updates.

![Data Ingestion Process](/images/DataIngestion.png)

#### Data Profiling
- Performed using AWS Glue DataBrew.
- Insights include correlation matrices and column statistics.
- Missing values identified and addressed.

![Data Profiling Process](/images/DataAnalysis.png)

#### Data Cleaning
- Unnecessary columns removed (e.g., Subtype, Vehicle Direction).
- Missing values filled using median imputation.
- Standardized column names for consistency.
- Outputs stored in S3 in CSV and Parquet formats.

![Data Cleaning Process](/images/DataBrewJob.png)

#### ETL Pipeline Design
- Created a visual ETL pipeline using AWS Glue.
- Transformed data to count annual bike route constructions and aggregated metrics.
- Outputs stored in user-accessible and system-accessible S3 buckets.

![ETL Pipeline Design](/images/DescriptiveETL.png)

#### Descriptive Analysis
- Explored annual bikeway construction patterns:
  - Significant growth observed in 2008 (67 new routes).
  - Slower development in recent years (e.g., 1 new route in 2017).

#### Exploratory Analysis
- Analyzed segment lengths over time:
  - Example: 1996 had 9 segments totaling 756.24 meters, while 2008 had 67 segments spanning 6233.54 meters.
- Highlighted key growth periods and stagnation points.

![Exploratory Analysis](/images/ExpAnalysis.png)

![Exploratory Analysis](/images/EXPetl.png)

#### Data Enrichment
- Cataloged datasets using AWS Glue Crawlers.
- Created metadata for querying enriched data via Amazon Athena.

![Data Enrichment Process](/images/DataEnriching.png)
![Data Enrichment Process](/images/DataEnchCraw.png)
![Data Enrichment Process](/images/TablesFromCrawler.png)
![Data Enrichment Process](/images/AggAnalysis.png)

#### Data Protection
Data protection has been a critical part of ensuring the security and integrity of sensitive information in this project. The following steps have been implemented:

- **Customer-Managed Key in AWS KMS**: A customer-managed key (`bikeways-key-sqa`) was created in AWS Key Management Service (KMS) to encrypt sensitive data. The key was configured for both encryption and decryption, ensuring that the data remains secure and protected from unauthorized access.

- **Encryption of S3 Buckets**: All S3 buckets, including those for raw and processed data, were configured to use the `bikeways-key-sqa` KMS key for encryption. This ensures that all data stored within these buckets is encrypted at rest.

- **Data Replication for Redundancy**: To safeguard against data loss, two S3 buckets (`bikeways-raw-bkp-sga` and `bikeways-trf-bkp-sga`) were created for data replication. Replication rules were set up to ensure that data from source buckets (`bikeways-trf-sga` and `bikeways-raw-sga`) is continuously copied to backup buckets across different AWS regions, providing a robust disaster recovery solution.

These measures ensure that the data used in the analysis is protected against unauthorized access, loss, and corruption, ensuring compliance with best practices for data security.

![Data Protection Process](/images/DataProtectionKMS.png)
![Data Protection Process](/images/KMSS3RawBuck.png)
![Data Protection Process](/images/KMSS3TRFBUCK.png)

#### Data Governance
- Detected and masked sensitive data (e.g., driving license patterns).
- Applied data quality rules to ensure completeness and uniqueness.

![Data Governance Process](/images/QCP.png)

#### Data Observability
- Built a CloudWatch dashboard to monitor:
  - S3 bucket usage.
  - ETL job logs.
  - Cost metrics.
- Enhanced security using AWS CloudTrail for activity logging.

![Data Observability Process](/images/DODashboard.png)

---

#### Results
- **Key Findings**:
  - Major bikeway growth in 2008 and 1998.
  - Shift from expansion to maintenance in recent years.
- **Visualizations**:
  - Trends in annual bike route constructions.
  - Segment lengths by year.

---

#### Future Improvements
- Implement IAM policies for finer access control.
- Enable auto-scaling for increased reliability.
- Optimize costs using Reserved Instances.
- Incorporate real-time data analysis using AWS Lambda.
- Enhance sustainability by using low-carbon AWS regions.

---

#### How to Replicate
1. **Setup AWS Services**:
   - Create S3 buckets for raw and transformed data.
   - Configure AWS Glue jobs for ETL processes.
2. **Download Dataset**:
   - Access the dataset from the [City of Vancouver Open Data Portal](https://opendata.vancouver.ca/explore/dataset/bikeways/information/).
3. **Run Data Profiling and Cleaning**:
   - Use AWS Glue DataBrew for profiling and cleaning.
4. **Query Data**:
   - Use Amazon Athena to run queries on enriched datasets.
---
