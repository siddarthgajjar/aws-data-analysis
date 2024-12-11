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



## Project 2: Recruitment and Selection Analytics

### Project Overview
This project focuses on analyzing recruitment and selection data to improve hiring efficiency and identify key trends. Leveraging AWS services, the project aims to uncover insights such as the application-to-hire ratio across departments and the relationship between website visits and application acceptance. The analysis enables better resource allocation and targeted recruitment strategies.

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
The primary goal of this project is to enhance recruitment strategies by analyzing:
- Application-to-hire ratio by department.
- Candidate-to-hire ratio.
- Average employee age by department.
- Correlations between website visits and application acceptance.

---

### Dataset
- **Source**: Internal HR records.
- **Description**: Recruitment-related datasets including employee lists, candidate lists, and applications.
- **Partitions**:
  - Employees List (structured, owner: HR)
    - **EmployeeID**: Unique identifier for each employee.
    - **Name**: Full name of the employee.
    - **Position**: Job title or role of the employee.
    - **Department**: Department where the employee works.
    - **Salary**: Annual salary of the employee.
    - **HireDate**: Date when the employee was hired.
    - **EmploymentType**: Type of employment (e.g., Full-time, Part-time).
    - **Gender**: Gender of the employee.
    - **Age**: Age of the employee.
    - **City**: City where the employee is based.

  - Candidates List (structured, owner: HR)
    - **CandidateID**: Unique identifier for each candidate.
    - **Name**: Full name of the candidate.
    - **Email**: Email address of the candidate.
    - **Phone**: Contact phone number of the candidate.
    - **PositionApplied**: Position the candidate applied for.
    - **Department**: Department associated with the applied position.
    - **Experience (Years)**: Years of relevant work experience.
    - **Education**: Educational qualification of the candidate.
    - **ApplicationDate**: Date of application submission.
    - **Status**: Current status of the application (e.g., Under Review, Interview Scheduled, Rejected).
  - Applications List (structured, owner: HR)
    - **ApplicationID**: Unique identifier for each application.
    - **CandidateName**: Full name of the candidate associated with the application.
    - **Email**: Email address of the candidate.
    - **Phone**: Contact phone number of the candidate.
    - **PositionApplied**: Position the candidate applied for.
    - **Department**: Department associated with the applied position.
    - **Experience (Years)**: Years of relevant work experience.
    - **Education**: Educational qualification of the candidate.
    - **ApplicationDate**: Date of application submission.
    - **Status**: Current status of the application (e.g., Under Review, Interview Scheduled, Rejected).

- **Key Attributes**:
  - Employee and candidate details.
  - Application statuses and demographics.
  - Partitioned by ingestion year and month for efficiency.

---

### Tools and Technologies
This project leveraged the following AWS services:
- **Amazon S3**: Centralized storage for raw and processed datasets.
- **AWS Glue**: ETL processes for dataset preparation.
- **Amazon Athena**: Querying datasets and calculating metrics.
- **Amazon CloudWatch**: Monitoring and observability for processes.
- **AWS CloudTrail**: Activity logging for enhanced security.


![Tools Overview](/images/dap_design_project2.png)

---

### Methodology

#### Data Ingestion
- Structured datasets uploaded to Amazon S3.
- Organized using folder partitioning by ingestion date and scope.

![Data Ingestion Process](/images/Dataingestion2.png)

#### Data Profiling
- Used AWS Glue DataBrew to:
  - Examine dataset quality and consistency.
  - Identify missing or erroneous values.

![Data Profiling Process](/images/DataProfileP2.png)
![Data Profiling Process](/images/DataProfilingP2.jpg)
![Data Profiling Process](/images/DataProfile1P2.png.png)
![Data Profiling Process](/images/DataProfile2P2.png.png)


#### Data Cleaning
- Removed unnecessary fields.
- Handled missing values using imputation techniques.
- Standardized field formats for uniformity.

![Data Cleaning Process](/images/DataCleaningJobP2.png)
![Data Cleaning Process](/images/DataCleaningTRFP2.png)



#### ETL Pipeline Design
- Visual pipelines created using AWS Glue to automate:
  - Aggregation of metrics like application-to-hire and candidate-to-hire ratios.
  - Segmentation of data for department-wise analysis.

![Data ETL Pipeline Process](/images/etl_p2_1.png)
![Data ETL Pipeline Process](/images/etl_p2_2.png)
![Data ETL Pipeline Process](/images/etl_p2_3.png)


#### Descriptive Analysis
- Calculated:
  - Application-to-Hire Ratio: `(Total Hires / Total Applications) * 100`
  - Candidate-to-Hire Ratio: `(Total Hires / Total Candidates) * 100`
  - Average Employee Age by Department.

#### Exploratory Analysis
- Investigated the relationship between webpage visits and application acceptance rates.
- Derived correlations for each application ID.

![Data Exploratory Analysis Process](/images/exp_click_stream.png)

![Data Exploratory Analysis Process](/images/etl_click_stream_p2.png)

![Data Exploratory Analysis Process](/images/etl_click_stream_result.png)



#### Data Enrichment
- Cataloged datasets with metadata using AWS Glue Crawlers.
- Generated enriched datasets for querying in Amazon Athena.

![Data Enrichment Process](/images/data_enc_p2_1.png)
![Data Enrichment Process](/images/data_enc_p2_2.png)
![Data Enrichment Process](/images/data_enc_p2_3.png)
![Data Enrichment Process](/images/data_enc_p2_4.png)
![Data Enrichment Process](/images/data_enc_p2_5.png)


#### Data Governance
- Monitored sensitive data and ensured compliance with internal policies.
- Applied quality control rules to validate data integrity.

![Data Governance Process](/images/data_gov_p2_1.png)

#### Data Observability
- Deployed a CloudWatch dashboard to track:
  - Data pipeline execution metrics.
  - S3 bucket utilization.
  - Recruitment cost analysis.
- Utilized CloudTrail for auditing changes and security.

![Data Observability Process](/images/cloud_trail_p2.png)
![Data Observability Process](/images/dashboard_p2.png)

---

### Results
- **Key Findings**:
  - Identified departments with the highest and lowest application-to-hire ratios.
  - Established meaningful relationships between website visits and application success.
  - Highlighted demographic trends, such as age distribution by department.
- **Visualizations**:
  - Department-wise application metrics.
  - Correlation heatmaps for exploratory insights.

---

### Future Improvements
- Automate updates to the dashboard with real-time data using AWS Lambda.
- Implement role-based access control with IAM policies.
- Optimize costs by transitioning to serverless architectures.
- Introduce predictive analytics for hiring trends using Amazon SageMaker.

---

### How to Replicate
1. **Setup AWS Services**:
   - Create S3 buckets for each dataset type.
   - Configure AWS Glue for ETL processing.
2. **Upload Datasets**:
   - Partition datasets by ingestion year and month.
3. **Run ETL Processes**:
   - Use Glue jobs to clean, transform, and analyze data.
4. **Query Results**:
   - Execute queries in Amazon Athena for derived metrics.
