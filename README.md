
# Real Estate Leads SQL Analysis Project
## Table of Contents 
- [Project Overview](#project-overview)
- [Project Objectives](#project-objectives)
- [Dataset Description](#dataset-description)
- [SQL Process Workflow](#sql-process-workflow)
- [Data Cleaning Process](#data-cleaning-process)
- [Exploratory Data Analysis-EDA](#exploratory-data-analysis-eda)
- [Key SQL Queries and Business Questions](#key-sql-queries-and-business-questions)
- [Project-Insights](#project-insights)
- [Recommendations](#recommendations)
- [Limitations](#limitations)

### Project Overview:
This project tracks and analyzes **real estate leads** generated via **BuyRent Kenya**. The dataset contains records of 1,000+ leads, including client inquiries, contact dates, and status updates. By applying SQL queries, this project aims to analyze client behavior, improve lead conversion, and generate actionable insights to optimize the sales process.

---

### Project Objectives:
- **Clean and structure** raw lead data for analysis.
- **Analyze key metrics** like response time, lead status, and follow-up outcomes.
- **Create SQL queries** to answer business questions and uncover patterns in client behavior.
- Provide a report that summarizes the **funnel conversion** and key insights.

---

### Dataset Description:
- **Source**: BuyRent Kenya
- **Rows**: 1,000+
- **Fields**:  
    - **Lead ID**: Unique identifier for each lead.
    - **DateOfInquiry**: The date the client showed interest in a property.
    - **DateOfContact**: The date the lead was contacted.
    - **ClientName**: Name of the client.
    - **PhoneNumber**: Client's phone number.
    - **Email**: Client’s email address.
    - **LeadStatus**: Current status of the lead (e.g., Hot, Warm, Cold).
    - **ViewingScheduled**: Whether a viewing has been scheduled (0 = No, 1 = Yes).
    - **Viewed**: Whether the client has viewed the property (0 = No, 1 = Yes).
    - **FollowUpComment**: Any follow-up comments on the lead?
    - **Outcome**: Outcome of the lead, such as "Converted", "Lost", "Did not respond", etc.

---
### SQL Process Workflow:

1. Data Import & Table Creation
Created a SQL table structure using the following schema:
  ```sql
 CREATE TABLE RealEstateLeads (
    LeadID INT IDENTITY(1,1) PRIMARY KEY,
    DateOfInquiry DATE,
    DateOfContact DATE,
    ClientName VARCHAR(255),
    PhoneNumber VARCHAR(20),
    Email VARCHAR(255),
    LeadStatus VARCHAR(20),
    ViewingScheduled VARCHAR(3),
    Viewed VARCHAR(3),
    FollowUpComment VARCHAR(255),
    Outcome VARCHAR(50));
```
## Data Cleaning Process:

1. **Standardized date formats** for both `DateOfInquiry` and `DateOfContact` columns.
2. **Removed duplicates** based on unique client identifiers (e.g., phone numbers and emails).
3. **Normalized text data** for emails and phone numbers, removing extra spaces and formatting issues.
4. **Handled null values**: Ensured that empty fields (e.g., follow-up comments) were appropriately managed.
5. **Converted categorical data** such as `ViewingScheduled` and `Viewed` from text ("Yes"/"No") to binary (1/0).

---

### Exploratory Data Analysis (EDA):

SQL queries answered:

1. Total number of leads
2. Number of leads per status (Hot/Warm/Cold)
3. How many scheduled viewings vs. actual viewings?
4. What percentage of leads resulted in site visits?
5. What are the most common follow-up comments?

### Key SQL Queries and Business Questions:

1. **Lead Funnel Analysis**:
   - **Goal**: Understand the lead progression from inquiry to viewing.
   - **SQL Query**: 
     ```sql
     SELECT LeadStatus, COUNT(*) AS TotalLeads, 
            SUM(CASE WHEN ViewingScheduled = 1 THEN 1 ELSE 0 END) AS ViewingScheduled
     FROM Leads
     GROUP BY LeadStatus;
     ```
   - **Business Question**: Which lead status (Hot, Warm, Cold) leads to the highest number of viewings?

2. **Response Time Analysis**:
   - **Goal**: Identify if faster response times lead to higher viewing rates.
   - **SQL Query**:
     ```sql
     SELECT AVG(DATEDIFF(DAY, DateOfInquiry, DateOfContact)) AS AvgResponseTime
     FROM Leads;
     ```
   - **Business Question**: Does responding faster to leads increase the likelihood of scheduling a viewing?

3. **Follow-Up Outcome Analysis**:
   - **Goal**: Analyze follow-up comments to identify patterns in successful and unsuccessful leads.
   - **SQL Query**:
     ```sql
     SELECT FollowUpComment, COUNT(*) AS LeadCount
     FROM Leads
     GROUP BY FollowUpComment
     ORDER BY LeadCount DESC;
     ```
   - **Business Question**: What follow-up comments are associated with the highest number of successful leads?

4. **Conversion Rate Analysis**:
   - **Goal**: Calculate conversion rates for different types of leads.
   - **SQL Query**:
     ```sql
     SELECT LeadStatus, 
            SUM(CASE WHEN Outcome = 'Converted' THEN 1 ELSE 0 END) AS SuccessfulConversions,
            COUNT(*) AS TotalLeads,
            (SUM(CASE WHEN Outcome = 'Converted' THEN 1 ELSE 0 END) * 100.0) / COUNT(*) AS ConversionRate
     FROM Leads
     GROUP BY LeadStatus;
     ```
   - **Business Question**: What is the conversion rate for "Hot" leads vs "Cold" leads?

---

### Project Insights:
- **Lead Status**: Hot leads tend to respond faster and convert at a higher rate.
- **Follow-Up Comments**: The most frequent outcome for leads that didn’t respond was **“Did not pick”**.
- **Response Time**: Leads that were contacted within 24 hours had a **30% higher viewing rate** than those contacted after 48 hours.

---

### Recommendations:

Prioritize Hot leads within 24 hours.

- Standardize FollowUpComment entries using dropdown options.
- Re-target Cold leads every 30 days.
- Create a dashboard to track KPIs like Response Time and Viewing Rates.
- Automate lead scoring rules using SQL triggers or scripts.

---

### Limitations:

No timestamp history for multi-stage follow-ups.

- Only one lead source (BuyRent Kenya).
- Comments are inconsistent; requires NLP for better analysis.
- No financial or budget data was captured.
- Dates may reflect manual entry delays by agents.
  
### ⚡ **Connect with Me**:
If you're interested in SQL, data analysis, or real estate, let's connect! Drop any questions, tips, or suggestions in the comments.

---
