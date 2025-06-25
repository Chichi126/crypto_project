# Azure Data Engineering Project - Real-Time Cryptocurrency Data Pipeline

This project creates a real-time cryptocurrency data pipeline using Azure Data Factory (ADF) to orchestrate data ingestion, transformation, and storage in Delta Lake. The pipeline extracts data from the CoinGecko API, uses Autoloader for efficient data ingestion, leverages Unity Catalog for data governance, and integrates Azure Key Vault for secure management of secrets.

Got it! Here's a **step-by-step walkthrough** of setting up and executing your **Azure Data Engineering Project** without going into the specifics of the code.

---

## **Azure Data Engineering Project Overview:**

The goal of this project is to **build a data pipeline** that integrates real-time cryptocurrency data from the **CoinGecko API** and processes it using **Azure Databricks** with **Delta Lake**, **Autoloader**, **Unity Catalog**, and **Azure Key Vault** for secure management of credentials. The orchestration of this process is handled by **Azure Data Factory (ADF)**, which will manage the flow of data and trigger activities at the right times.

---

### **1. Creating Resources in Azure**

Before building the pipeline, you'll need to create a few core resources in **Azure**.

#### 1.1 **Create Azure Databricks Workspace**

* In the **Azure portal**, create a **Databricks workspace**. This workspace will be where the transformation of data occurs, including the creation of Delta Lake tables and the use of **Autoloader** for file ingestion.

#### 1.2 **Create Azure Data Factory (ADF)**

* Set up **Azure Data Factory** (ADF) to orchestrate and automate the workflow. ADF will handle tasks like data extraction, triggering **Lambda** or **Web Activities**, and loading data into Databricks for processing.

#### 1.3 **Create Azure Key Vault**

* Create an **Azure Key Vault** to securely store secrets, such as the **CoinGecko API key**. This will ensure that your credentials are kept safe and are easily accessible by ADF and Databricks.

---

### **2. Setting Up the CoinGecko API Integration**

To get cryptocurrency data, you'll use the **CoinGecko API**.

#### 2.1 **Get API Key from CoinGecko**

* Visit **CoinGecko** and get an API key to access the cryptocurrency data.

#### 2.2 **Store API Key in Azure Key Vault**

* Store the **CoinGecko API key** in **Azure Key Vault**. This will allow you to **securely reference** the key from **Azure Data Factory (ADF)** using its built-in Key Vault integration.

---

### **3. Setting Up Delta Lake and Autoloader**

**Databricks** will be the engine for data transformation. Specifically, **Delta Lake** and **Autoloader** will be used to handle the storage and processing of data.

#### 3.1 **Create Delta Lake Table in Databricks**

* In **Databricks**, set up a **Delta Lake** table to store the **cryptocurrency data**. Delta Lake will provide version control and ACID transactions for your data.

#### 3.2 **Configure Autoloader for Efficient Data Ingestion**

* **Autoloader** in Databricks will be set up to automatically ingest data from **Azure Blob Storage** into Delta Lake. Autoloader will allow for **efficient and scalable** processing of data as it arrives.

---

### **4. Setting Up Unity Catalog**

The **Unity Catalog** in Databricks will be used to manage **data governance**, ensuring that all data is accessible, well-organized, and securely managed.

#### 4.1 **Create a Catalog in Unity Catalog**

* Set up a **catalog** in Unity Catalog to store the **Delta tables** and other datasets.

#### 4.2 **Define Access Policies and Permissions**

* You can define **access control** policies to ensure that the right teams or users have access to the data, improving security and compliance.

---

### **5. Azure Key Vault Setup**

To manage secrets like API keys, connection strings, and other sensitive data, you’ll use **Azure Key Vault**.

#### 5.1 **Store Secrets in Key Vault**

* Store sensitive data (like the **CoinGecko API key** and **Databricks credentials**) in **Azure Key Vault**.

#### 5.2 **Use Key Vault in ADF**

* In **ADF**, configure a **linked service** to **Azure Key Vault** to securely fetch the API key and other sensitive credentials during pipeline execution.

---

### **6. Orchestrating with Azure Data Factory (ADF)**

**Azure Data Factory** will orchestrate the entire process, from data extraction to transformation and loading.

#### 6.1 **Create a Pipeline in ADF**

* Set up an **ADF pipeline** that will coordinate the extraction of data from the **CoinGecko API**, store it in **Blob Storage**, and then trigger Databricks to process the data.

#### 6.2 **Use Web Activity to Call the CoinGecko API**

* Use a **Web Activity** in ADF to call the **CoinGecko API**. This will fetch real-time data about cryptocurrencies. You’ll pass the necessary parameters (such as the **crypto symbol** or **date range**) dynamically from ADF.

#### 6.3 **Trigger Databricks from ADF**

* After fetching the data, use **ADF** to trigger a **Databricks notebook** that will process the cryptocurrency data, transform it, and store it in **Delta Lake**.

---

### **7. Using Dynamic Parameters in ADF**

To make your pipeline more flexible and reusable, you’ll use **dynamic parameters** in ADF.

#### 7.1 **Create Dynamic Parameters in ADF**

* Define **dynamic parameters** in your pipeline. For example:

#### 7.2 **Pass Parameters to Activities**

* You’ll use **dynamic expressions** to pass these parameters to the **Web Activity** (CoinGecko API call) and Databricks (data processing tasks).

---

### **8. Running the Pipeline**

After configuring all the components, you can run the pipeline.

#### 8.1 **Trigger the Pipeline**

* You can manually trigger the pipeline or schedule it to run at specific intervals (e.g., daily or hourly).

#### 8.2 **Monitor the Pipeline**

* Use the **monitoring features** in ADF to track the progress of the pipeline and troubleshoot any issues.

---

### **9. Conclusion**

This project demonstrates how to create a **real-time cryptocurrency data pipeline** in **Azure**. By integrating **CoinGecko API**, **Delta Lake**, **Autoloader**, **Unity Catalog**, **Azure Key Vault**, and **Azure Data Factory**, you can efficiently manage and process large-scale cryptocurrency data.

With **dynamic parameters** in ADF, you can easily scale and customize the pipeline to handle different cryptocurrencies, data ranges, and transformation requirements.

---

### **Next Steps**:

* Extend the pipeline to handle additional data sources or more complex transformations.
* Implement additional **data validation** and **error handling** mechanisms in ADF.
* Use the data processed in **Delta Lake** to **train machine learning models** for **predictive analytics** (e.g., predicting future cryptocurrency prices).

---

Let me know if you need any further clarifications or if you'd like to dive deeper into any of the steps!






