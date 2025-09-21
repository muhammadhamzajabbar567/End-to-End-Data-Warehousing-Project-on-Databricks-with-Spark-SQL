# 🚀 End-to-End Data Warehousing Project (Databricks Spark SQL)

<div align="center">

**🔥 DATABRICKS** • **⚡ APACHE SPARK** • **🔍 SPARK SQL** • **📊 SQL** • **🔄 ETL** • **📈 ANALYTICS**
</div>

## 🛠️ **Technology Stack**

<div align="center">

| 🔥 **Databricks** | ⚡ **Apache Spark** | 🔍 **Spark SQL** | 📊 **SQL** |
|:-----------------:|:------------------:|:----------------:|:-----------:|
| Cloud Analytics Platform | Distributed Computing | Query Processing | Data Language |

| 🔄 **ETL Pipeline** | 📈 **Dimensional Modeling** | 🗃️ **Data Warehousing** | 📉 **Business Intelligence** |
|:------------------:|:---------------------------:|:-----------------------:|:---------------------------:|
| Data Processing | Star Schema Design | Storage Architecture | Analytics & Reporting |

</div>

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Data Model](#data-model)
- [Implementation Details](#implementation-details)
- [Key Features](#key-features)
- [Getting Started](#getting-started)
- [Future Enhancements](#future-enhancements)
- [Contributing](#contributing)

## 🎯 Overview

This project demonstrates a complete end-to-end Data Warehousing solution built on **Databricks** using **Spark SQL**. The implementation showcases modern data engineering practices including dimensional modeling, ETL processes, and Slowly Changing Dimensions (SCD) management.

### 🎪 Project Highlights

- **Complete Pipeline**: Raw data ingestion to analytical insights
- **Layered Architecture**: Staging → Transformation → Core layers
- **Star Schema Design**: Optimized for analytical reporting
- **SCD Implementation**: Types 1 & 2 for historical data management
- **Surrogate Key Management**: Efficient relationship handling

## 🏗️ Architecture

**📊 Data Flow Architecture:**

```
🗂️  SalesDB (Source) 
    ⬇️
🔄 Staging Layer (Raw Data Ingestion)
    ⬇️  
🔧 Transformation Layer (Cleansing & Rules)
    ⬇️
🌟 Core Layer (Star Schema)
    ⬇️
📈 Analytics & Reporting

📋 SCD Database ➡️ Transformation Layer
```

### 🔄 Data Flow

1. **Ingestion**: Raw data from SalesDB operational database
2. **Staging**: Initial data loading and basic validation
3. **Transformation**: Data cleansing, standardization, and business rule application
4. **Core**: Dimensional model implementation with star schema
5. **Analytics**: Business intelligence and reporting layer

## 💻 Technologies Used

<div align="center">

| 🛠️ **Technology** | 🎯 **Purpose** | 📋 **Description** |
|-------------------|---------------|-------------------|
| **🔥 Databricks** | Data Engineering Platform | Cloud-based analytics platform for big data processing |
| **⚡ Apache Spark** | Distributed Computing | Unified analytics engine for large-scale data processing |
| **🔍 Spark SQL** | Query Engine | Distributed SQL processing with Spark optimization |
| **📊 SQL** | Query Language | Data manipulation, analysis, and reporting |
| **🔄 ETL Pipeline** | Data Processing | Extract, Transform, Load processes automation |
| **📈 Dimensional Modeling** | Data Architecture | Star schema design for analytical workloads |
| **🗃️ Data Warehousing** | Storage Architecture | Centralized repository for analytical data |
| **📉 Business Intelligence** | Analytics & Reporting | Data-driven decision making tools |

</div>

## 📊 Data Model

### 🔹 Dimension Tables

| Table | Purpose | SCD Type |
|-------|---------|----------|
| `DimCustomers` | Customer master data | Type 2 (Historical) |
| `DimProducts` | Product catalog information | Type 2 (Historical) |
| `DimRegions` | Geographic reference data | Type 1 (Overwrite) |
| `DimDate` | Date dimension (Role-playing) | Static |

### 🔸 Fact Tables

| Table | Type | Measures |
|-------|------|----------|
| `FactSales` | Transaction Fact | Quantity, UnitPrice, TotalAmount |
| `FactMonthlySales` | Periodic Snapshot | *Future Implementation* |
| `FactOrderLifecycle` | Accumulating Snapshot | *Future Implementation* |

### 🎯 Star Schema Design

```
           📅 DimDate
               |
🙋 DimCustomers ➡️ 💰 FactSales ⬅️ 🛍️ DimProducts
               |         |
           🌍 DimRegions  📊 [Measures]
                         • Quantity
                         • UnitPrice  
                         • TotalAmount
```

## 🔧 Implementation Details

### 📂 Project Structure

```
├── notebooks/
│   ├── 01_source_database_setup.sql
│   ├── 02_staging_layer.sql
│   ├── 03_dimension_tables.sql
│   ├── 04_fact_tables.sql
│   └── 05_scd_implementation.sql
├── scripts/
│   ├── create_databases.sql
│   ├── etl_pipeline.py
│   └── data_validation.sql
├── docs/
│   ├── architecture_diagram.png
│   ├── data_model_erd.png
│   └── scd_process_flow.md
└── README.md
```

### 🔄 Slowly Changing Dimensions (SCD)

#### SCD Type 1 - Overwrite Strategy
```sql
-- Example: Update customer phone number
UPDATE DimCustomers 
SET PhoneNumber = 'new_phone_number'
WHERE CustomerKey = 123;
```

#### SCD Type 2 - Historical Preservation
```sql
-- Example: Track customer address changes
INSERT INTO DimCustomers (
    CustomerKey, CustomerID, CustomerName, 
    Address, EffectiveDate, ExpirationDate, IsCurrent
) VALUES (
    NEXT_SURROGATE_KEY, 'CUST001', 'John Doe',
    'New Address', CURRENT_DATE, '9999-12-31', 'Y'
);

-- Expire previous record
UPDATE DimCustomers 
SET ExpirationDate = CURRENT_DATE - 1, IsCurrent = 'N'
WHERE CustomerID = 'CUST001' AND IsCurrent = 'Y';
```

### 🔑 Surrogate Key Management

- **Auto-generated** surrogate keys for all dimension tables
- **Business keys** preserved for data lineage
- **Efficient joins** between fact and dimension tables
- **Historical tracking** enabled through key relationships

## ✨ Key Features

### 🎯 Data Quality & Governance
- **Data Validation**: Comprehensive checks at each layer
- **Business Rules**: Automated rule application during transformation
- **Audit Trail**: Complete data lineage tracking
- **Error Handling**: Robust exception management

### 📈 Performance Optimization
- **Partitioning**: Strategic partitioning for large fact tables
- **Indexing**: Optimized access patterns
- **Caching**: Frequently accessed dimension tables
- **Parallel Processing**: Leverage Spark's distributed computing

### 🔒 Scalability & Maintenance
- **Modular Design**: Separated concerns across layers
- **Parameterized Queries**: Flexible and maintainable code
- **Version Control**: All code tracked in Git
- **Documentation**: Comprehensive technical documentation

## 🚀 Getting Started

### Prerequisites
- Databricks workspace access
- Spark SQL knowledge
- Understanding of dimensional modeling concepts

### Setup Instructions

1. **Clone the repository**
   ```bash
   git clone https://github.com/muhammadhamzajabbar567/End-to-End-Data-Warehousing-Project-on-Databricks-with-Spark-SQL.git
   cd End-to-End-Data-Warehousing-Project-on-Databricks-with-Spark-SQL
   ```

2. **Set up Databricks environment**
   - Import notebooks to your Databricks workspace
   - Configure cluster with appropriate Spark version

3. **Create source database**
   ```sql
   %sql
   RUN notebooks/01_source_database_setup.sql
   ```

4. **Execute ETL pipeline**
   ```sql
   %sql
   -- Run in sequence
   RUN notebooks/02_staging_layer.sql
   RUN notebooks/03_dimension_tables.sql  
   RUN notebooks/04_fact_tables.sql
   RUN notebooks/05_scd_implementation.sql
   ```

### 📊 Sample Queries

```sql
-- Monthly sales by region
SELECT 
    dr.RegionName,
    dd.Year,
    dd.Month,
    SUM(fs.TotalAmount) as MonthlySales
FROM FactSales fs
JOIN DimRegions dr ON fs.RegionKey = dr.RegionKey
JOIN DimDate dd ON fs.OrderDateKey = dd.DateKey
GROUP BY dr.RegionName, dd.Year, dd.Month
ORDER BY dd.Year, dd.Month, MonthlySales DESC;
```

## 🔮 Future Enhancements

- [ ] **Real-time Streaming**: Implement Delta Live Tables for streaming data
- [ ] **Machine Learning**: Integrate MLflow for predictive analytics
- [ ] **Advanced Analytics**: Implement OLAP cubes and MDX queries
- [ ] **Data Governance**: Add Unity Catalog for metadata management
- [ ] **Visualization**: Connect to Power BI/Tableau for reporting
- [ ] **Automation**: Implement Databricks Workflows for scheduling

## 📈 Business Value

- **Improved Decision Making**: Fast access to analytical insights
- **Historical Analysis**: Complete audit trail of data changes
- **Scalable Architecture**: Handles growing data volumes efficiently  
- **Cost Optimization**: Leverages cloud-native technologies
- **Data Governance**: Ensures data quality and compliance

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Contact

**Muhammad Hamza Jabbar** - hamzamughal8096@gmail.com

Project Link: [https://github.com/muhammadhamzajabbar567/End-to-End-Data-Warehousing-Project-on-Databricks-with-Spark-SQL](https://github.com/muhammadhamzajabbar567/End-to-End-Data-Warehousing-Project-on-Databricks-with-Spark-SQL)

---

⭐ **If this project helped you, please give it a star!** ⭐

#DataWarehousing #DataEngineering #ETL #DimensionalModeling #FactTables #DimensionTables #SCD #SparkSQL #Databricks #BigData #DataAnalytics #BusinessIntelligence #SurrogateKeys #StarSchema #CloudComputing #DataModeling
