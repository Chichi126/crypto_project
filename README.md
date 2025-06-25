
# **Azure Data Engineering Project - Real-Time Cryptocurrency Data Pipeline**

## **Overview**

This project demonstrates an **end-to-end real-time cryptocurrency data pipeline** leveraging **Azure Data Factory (ADF)** for orchestration, **Azure Databricks** for transformation, and **Delta Lake** for storing processed data. The pipeline extracts cryptocurrency data from the **CoinGecko API**, processes it following the **Medallion Architecture** (Silver and Gold layers), and loads the results into an **Azure SQL Database**. **Azure Key Vault** is used to store **Databricks tokens** and **SQL database connection strings** securely.

---

## **Table of Contents**

1. [Prerequisites](#prerequisites)
2. [Creating Resources in Azure](#creating-resources-in-azure)
3. [Setting Up CoinGecko API Integration](#setting-up-the-coingecko-api-integration)
4. [Setting Up Delta Lake and Autoloader](#setting-up-delta-lake-and-autoloader)
5. [Medallion Architecture Implementation](#medallion-architecture-implementation)
6. [Unity Catalog Setup](#unity-catalog-setup)
7. [SQL Database Setup](#sql-database-setup)
8. [Orchestrating with Azure Data Factory (ADF)](#orchestrating-with-azure-data-factory-adf)
9. [Using Dynamic Parameters in ADF](#using-dynamic-parameters-in-adf)
10. [Running the Pipeline](#running-the-pipeline)
11. [Conclusion](#conclusion)

---

## **1. Prerequisites**

Ensure you have the following before starting:

* **Azure Subscription**: A valid Azure subscription.
* **Azure Databricks Workspace**: Set up in your Azure subscription.
* **Azure Data Factory (ADF)**: Set up for orchestrating the workflow.
* **Azure Key Vault**: Set up for secure storage of sensitive credentials.
* **Azure SQL Database**: For storing processed data.
* **Python 3.7+**: For interacting with Databricks and other Azure services.
* **Databricks CLI**: For managing Databricks resources.

---

## **2. Creating Resources in Azure**

### **2.1 Create Azure Databricks Workspace**

1. Go to the **Azure Portal** and search for **Databricks**.
   
2. Create a new **Databricks Workspace**, providing details like **Subscription**, **Resource Group**, and **Workspace Name**.
   
3. Once created, note the **Workspace URL** for accessing Databricks.

### **2.2 Create Azure Data Factory (ADF)**

1. In the **Azure Portal**, search for **Data Factory** and create a new instance to manage orchestration.

2. Once created, open **ADF** and note the **ADF name** and **resource group**.

### **2.3 Create Azure Key Vault**

1. Go to **Key Vault** in the Azure Portal and create a new **Key Vault**.

2. Add the **Databricks token** and **SQL database connection string** as **secrets** in the Key Vault to securely store sensitive information.

---

## **3. Setting Up the CoinGecko API Integration**

### **3.1 Get CoinGecko API Key**

1. Sign up for an API key at **CoinGecko** to access cryptocurrency data.

2. **No need to store the CoinGecko API key** in **Azure Key Vault** as it can be directly accessed within **ADF** using the **Web Activity**.

---

## **4. Setting Up Delta Lake and Autoloader**

### **4.1 Delta Lake Setup in Databricks**

1. In **Databricks**, create a **cluster** to run the data transformation tasks (Standard or Jobs Cluster).

2. Set up **Delta Lake tables** for the **Silver** and **Gold layers** (not the **Bronze** layer, which stores raw data as JSON). This allows for **data versioning** and

**schema evolution**.


---

## **5. Medallion Architecture Implementation**

The **Medallion Architecture** follows a multi-layer approach where data progresses through the **Bronze**, **Silver**, and **Gold** layers. In this project:

* The **Bronze Layer** stores **raw data** in **JSON format**.
* The **Silver Layer** begins using **Delta tables** to store **cleaned and transformed data**.
* The **Gold Layer** holds the **aggregated, ready-to-use data** for reporting and analysis.

### **5.1 Bronze Layer (Raw Data - JSON)**

1. The **Bronze Layer** stores **raw data** fetched from the **CoinGecko API**. This data is saved in **JSON format** in **Azure Blob Storage**.
2. Data in this layer is untransformed, providing an **immutable record** of the raw API responses for historical reference.

### **5.2 Silver Layer (Cleaned and Transformed Data)**

1. The **Silver Layer** is where data is **cleaned** and **transformed**. This can include:

   * Autoloader will be configured to automatically readStream the **raw cryptocurrency data** (in **JSON format**) in the raw **Azure Blob Storage**.

   * Removing invalid entries.

   * Aggregating data.

   * Enriching data from additional sources (e.g., weather data).
     
 **Databricks notebooks** are used to process and store the cleaned data into **Delta Lake** as **Silver tables**.

### **5.3 Gold Layer (Aggregated Data for Reporting)**

1. The **Gold Layer** contains **aggregated and optimized data** for reporting and analysis.

2. Data in the **Gold Layer** is ready for downstream tools such as **Power BI** or **Databricks SQL** for business insights.

3. **Databricks notebooks** are used for aggregation tasks like calculating **average prices**, **market trends**, and more.

---

## **6. Unity Catalog Setup**

### **6.1 Set Up Unity Catalog**

1. **Unity Catalog** is set up in **Databricks** to manage **data governance**, ensuring secure access to data and providing clear data lineage.

2. Create a **catalog** in **Unity Catalog** and organize your data into different **schemas** such as:


   * **raw\_data** (for **Bronze Layer** data).

   * **cleaned\_data** (for **Silver Layer** data).

    * **aggregated\_data** (for **Gold Layer** data).

### **6.2 Implementing Data Governance**

1. Use **Unity Catalog** to manage **access controls** for different teams or individuals working with the data in various stages (Bronze, Silver, and Gold).

2. Set up **data access permissions** to ensure sensitive data is only accessible by authorized users.

---

## **7. SQL Database Setup**

### **7.1 Create Azure SQL Database**

1. In **Azure**, create an **SQL Database** to store **aggregated data** from the **Gold Layer**.

2. Set up the necessary **tables** in the **SQL Database** to hold information such as:

   * ** Fact and Dimension tables 

### **7.2 Data Loading from Delta Lake to SQL**

1. Use **Azure Data Factory** to **move** the transformed and aggregated data from **Delta Lake** (Silver and Gold layers) to **Azure SQL Database** for reporting.

---

## **8. Orchestrating with Azure Data Factory (ADF)**

### **8.1 Create a Pipeline in ADF**

1. In **Azure Data Factory**, create a **new pipeline** to automate the entire process:

   * **Move Data From Databricks to Blob Storage**.

   * **Trigger Databricks**: Use ADF to trigger **Databricks notebooks** that will clean, transform, and aggregate the data.

### **8.3 Trigger Databricks from ADF**

1. After the data is stored in **Azure Blob Storage**, trigger **Databricks notebooks** that:

   * Process the raw data (from **Bronze Layer**).

   * Clean and transform it (to the **Silver Layer**).

    * Aggregate the data for reporting (to the **Gold Layer**).

---

## **9. Using Dynamic Parameters in ADF**

### **9.1 Create Dynamic Parameters**

1. Define dynamic parameters in ADF for:

   Source Dynamics

   Sink Dynamic Parameters

  Pass Parameters to Activities**




---

## **10. Running the Pipeline**

### **10.1 Trigger the Pipeline**

1. Once everything is set up, manually trigger the pipeline or schedule it to run at specified intervals (e.g., hourly, daily).

2. Monitor the progress of each activity to ensure the pipeline runs smoothly.

### **10.2 Monitor the Pipeline**

1. Use **Azure Data Factory's monitoring features** to track the pipeline's progress, log details, and troubleshoot any errors.

---

## **11. Conclusion**

This project demonstrates how to integrate **real-time cryptocurrency data** from **CoinGecko** into a data pipeline built using **Azure Data Factory**, **Databricks**, and **Delta Lake**. The pipeline follows the **Medallion Architecture** to clean, transform, and aggregate data into **Silver** and **Gold layers**, and stores the results in **Azure SQL Database** for further analysis. **Unity Catalog** ensures proper data governance and access control, while **Azure Key Vault** securely manages credentials.

### **Next Steps**:

* **Extend the pipeline** to include more data sources or additional transformations.
* **Visualize** the data using **Power BI** or **Databricks SQL**.
* Set up **monitoring** and **alerts** in **Azure Data Factory** for better observability.

---
