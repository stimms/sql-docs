---
title: "Oracle to SQL Server: Migration guide"
description: 'This guide teaches you to migrate your Oracle schemas to Microsoft SQL Server by using SQL Server Migration for Oracle (SSMA for Oracle). '
ms.prod: sql
ms.reviewer: ""
ms.technology: migration-guide
ms.custom:
ms.devlang:
ms.topic: how-to
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
---

# Migration guide: Oracle to SQL Server
[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

This guide teaches you to migrate your Oracle databases to SQL Server by using SQL Server Migration Assistant for Oracle. 

For other migration guides, see [Database Migration](https://docs.microsoft.com/data-migration). 

## Prerequisites 

To migrate your Oracle database to SQL Server, you need:

- To verify your source environment is supported.
- To install [SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2019?filetype=EXE).
- To download [SQL Server Migration Assistant (SSMA) for Oracle](https://www.microsoft.com/download/details.aspx?id=54258).
- The [necessary permissions for SSMA for Oracle](/sql/ssma/oracle/connecting-to-oracle-database-oracletosql) and [provider](/sql/ssma/oracle/connect-to-oracle-oracletosql).
- Connectivity and sufficient permissions to access both source and target. 


## Pre-migration

As you prepare for migrating to the cloud, verify that your source environment is supported and that you have addressed any prerequisites. This will help to ensure an efficient and successful migration.

This part of the process involves conducting an inventory of the databases that you need to migrate, assessing those databases for potential migration issues or blockers, and then resolving any items you might have uncovered. 


### Discover

Use the [MAP Toolkit](https://go.microsoft.com/fwlink/?LinkID=316883) to identify existing data sources and details about the features that are being used by your business to get a better understanding of and plan for the migration. This process involves scanning the network to identify all your organization's Oracle instances together with the version and features in use.

To use the MAP Toolkit to perform an inventory scan, follow these steps: 

1. Open the [MAP Toolkit](https://go.microsoft.com/fwlink/?LinkID=316883).
1. Select **Create/Select database**: 

   ![Select database](./media/oracle-to-sql-server/select-database.png)

1. Select **Create an inventory database**, enter a name for the new inventory database you're creating, provide a brief description, and then select **OK**:

   :::image type="content" source="media/oracle-to-sql-server/create-inventory-database.png" alt-text="Create an inventory database":::

1. Select **Collect inventory data** to open the **Inventory and Assessment Wizard**: 

   :::image type="content" source="media/oracle-to-sql-server/collect-inventory-data.png" alt-text="Collect inventory data":::

1. In the **Inventory and Assessment Wizard**, choose **Oracle** and then select **Next**: 

   ![Choose oracle](./media/oracle-to-sql-server/choose-oracle.png)

1. Choose the computer search option that best suits your business needs and environment, and then select **Next**: 

   ![Choose the computer search option that best suits your business needs](./media/oracle-to-sql-server/choose-search-option.png)

1. Either enter credentials or create new credentials for the systems that you want to explore, and then select **Next**:

    ![Enter credentials](./media/oracle-to-sql-server/choose-credentials.png)

1. Set the order of the credentials, and then select **Next**:

   ![Set credential order](./media/oracle-to-sql-server/set-credential-order.png)  

1. Specify the credentials for each computer you want to discover. You can use unique credentials for every computer/machine, or you can choose to use the **All Computer Credentials** list:

   ![Specify the credentials for each computer you want to discover](./media/oracle-to-sql-server/specify-credentials-for-each-computer.png)

1. Verify your selection summary, and then select **Finish**:

   ![Review summary](./media/oracle-to-sql-server/review-summary.png)

1. After the scan completes, view the **Data Collection** summary report. The scan take a few minutes, and depends on the number of databases. Select **Close** when finished:

   ![Collection summary report](./media/oracle-to-sql-server/collection-summary-report.png)

1. Select **Options** to generate a report about the Oracle Assessment and database details. Select both options (one by one) to generate the report.




### Assess

After identifying the data sources, use the [SQL Server Migration Assistant (SSMA) for Oracle](https://www.microsoft.com/download/details.aspx?id=54258) to assess the Oracle instance(s) migrating to the SQL Server VM so that you understand the gaps between the two. Using the migration assistant, you can review database objects and data, assess databases for migration, migrate database objects to SQL Server, and then migrate data to SQL Server.

To create an assessment, follow these steps: 

1. Open the  [SQL Server Migration Assistant (SSMA) for Oracle](https://www.microsoft.com/download/details.aspx?id=54258). 
1. Select **File** and then choose **New Project**. 
1. Provide a project name, a location to save your project, and then select a SQL Server migration target from the drop-down. Select **OK**: 

   ![New project](./media/oracle-to-sql-server/new-project.png)

1. Select **Connect to Oracle**. Enter in values for Oracle connection details on the **Connect to Oracle** dialog box:

   ![Connect to Oracle](./media/oracle-to-sql-server/connect-to-oracle.png)

   Select the Oracle schema(s) you want to migrate:

   ![Select schema to load](./media/oracle-to-sql-server/select-schema.png)

1. In **Oracle Metadata Explorer**, select the Oracle schema, and then select **Create Report** to generate an HTML report with conversion statistics and error/warnings, if any. Alternatively, you can choose **Create report** from the navigation bar after selecting the schema:

   ![Create report](./media/oracle-to-sql-server/create-report.png)

1. Review the HTML report to understand conversion statistics and any errors or warnings. You can also open the report in Excel to get an inventory of Oracle objects and the effort required to perform schema conversions. The default location for the report is in the report folder within SSMAProjects  

   `drive:\<username>\Documents\SSMAProjects\MyOracleMigration\report\report_2016_11_12T02_47_55\`

   ![Conversion Report](./media/oracle-to-sql-server/conversion-report.png)


### Validate data types

Validate the default data type mappings and change them based on requirements if necessary. To do so, follow these steps: 

1. Select **Tools** from the menu. 
1. Select **Project Settings**. 
1. Select the **Type mappings** tab:

   ![Type Mappings](./media/oracle-to-sql-server/type-mappings.png)

1. You can change the type mapping for each table by selecting the table in the **Oracle Metadata explorer**. 



### Convert schema

To convert the schema, follow these steps: 

1. (Optional) To convert dynamic or ad-hoc queries, right-click the node and choose **Add statement**.
1. Select **Connect to SQL Server** from the top-line navigation bar. 
     1. Enter connection details for your SQL Server instance. 
     1. Choose your target database from the drop-down, or provide a new name, in which case a database will be created on the target server. 
     1. Provide authentication details. 
     1. Select **Connect**:

   ![Connect to SQL](./media/oracle-to-sql-server/connect-to-sql.png)

1. Right-click the schema and choose **Convert Schema**. Alternatively, you can choose **Convert schema** from the top line navigation bar after choosing your database:

   ![Convert Schema](./media/oracle-to-sql-server/convert-schema.png)

1. After the conversion completes, compare and review the converted objects to the original objects to identify potential problems and address them based on the recommendations:

   ![Convert Schema Compare And Review object code](./media/oracle-to-sql-server/table-mapping.png)

   Compare the converted Transact-SQL text to the original code and review the recommendations:

   ![Review converted procedures](./media/oracle-to-sql-server/procedure-comparison.png)

1. Select **Review results** in the Output pane, and review errors in the **Error list** pane. 
1. Save the project locally for an offline schema remediation exercise. Select **Save Project** from the **File** menu. This gives you an opportunity to evaluate the source and target schemas offline and perform remediation before you can publish the schema to SQL Server.


## Migrate

After you have the necessary prerequisites in place and have completed the tasks associated with the **Pre-migration** stage, you are ready to perform the schema and data migration. Migration involves two steps – publishing the schema and migrating the data. 


To publish your schema and migrate the data, follow these steps: 

1. Publish the schema: Right-click the database from the **SQL Server Metadata Explorer**  and choose **Synchronize with Database**. This action publishes the Oracle schema to SQL Server:

   ![Synchronize with Database](./media/oracle-to-sql-server/synchronize-database.png)

   Review the mapping between your source project and your target:

   ![Synchronize with Database - Review mapping](./media/oracle-to-sql-server/synchronize-database-review.png)

1. Migrate the data: Right-click the schema or object you want to migrate in **Oracle Metadata Explorer**, and choose **Migrate data**. Alternatively, you can select **Migrate Data** from the top-line navigation bar. To migrate data for an entire database, select the check box next to the database name. To migrate data from individual tables, expand the database, expand Tables, and then select the check box next to the table. To omit data from individual tables, clear the check box:

   ![Migrate Data](./media/oracle-to-sql-server/migrate-data.png)

1. Provide connection details for Oracle and SQL Server at the dialog box.
1. After migration completes, view the **Data Migration Report**:

    ![Data Migration Report](./media/oracle-to-sql-server/data-migration-report.png)

1. Connect to your SQL Server using [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) to review data and schema on your SQL Server instance:

   ![Validate in SSMA](./media/oracle-to-sql-server/validate-in-ssms.png)


In addition to using SSMA, you can also use SQL Server Integration Services (SSIS) to migrate the data. To learn more, see: 
- The article [Getting Started with SQL Server Integration Services](https://docs.microsoft.com//sql/integration-services/sql-server-integration-services).
- The white paper [SQL Server Integration Services: SSIS for Azure and Hybrid Data Movement](https://download.microsoft.com/download/D/2/0/D20E1C5F-72EA-4505-9F26-FEF9550EFD44/SSIS%20Hybrid%20and%20Azure.docx).



## Post-migration 

After you have successfully completed the **Migration** stage, you need to go through a series of post-migration tasks to ensure that everything is functioning as smoothly and efficiently as possible.

### Remediate applications

After the data is migrated to the target environment, all the applications that formerly consumed the source need to start consuming the target. Accomplishing this will in some cases require changes to the applications.

The [Data Access Migration Toolkit](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) is an extension for Visual Studio Code that allows you to analyze your Java source code and detect data access API calls and queries, providing you with a single-pane view of what needs to be addressed to support the new database back end. To learn more, see the [Migrate our Java application from Oracle](https://techcommunity.microsoft.com/t5/microsoft-data-migration/migrate-your-java-applications-from-oracle-to-sql-server-with/ba-p/368727) blog. 

### Perform tests

The test approach for database migration consists of performing the following activities:

1. **Develop validation tests**. To test database migration, you need to use SQL queries. You must create the validation queries to run against both the source and the target databases. Your validation queries should cover the scope you have defined.

2. **Set up test environment**. The test environment should contain a copy of the source database and the target database. Be sure to isolate the test environment.

3. **Run validation tests**. Run the validation tests against the source and the target, and then analyze the results.

4. **Run performance tests**. Run performance test against the source and the target, and then analyze and compare the results.


### Optimize

The post-migration phase is crucial for reconciling any data accuracy issues and verifying completeness, as well as addressing performance issues with the workload.

> [!Note]
> For additional detail about these issues and specific steps to mitigate them, see the [Post-migration Validation and Optimization Guide](../../../relational-databases/post-migration-validation-and-optimization-guide.md).


## Migration assets 

For additional assistance with completing this migration scenario, please see the following resources, which were developed in support of a real-world migration project engagement.


| **Title/link**                                                                                                                                          | **Description**                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Data Workload Assessment Model and Tool](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | This tool provides suggested “best fit” target platforms, cloud readiness, and application/database remediation level for a given workload. It offers simple, one-click calculation and report generation that greatly helps to accelerate large estate assessments by providing and automated and uniform target platform decision process.                                                          |
| [Oracle Inventory Script Artifacts](https://github.com/Microsoft/DataMigrationTeam/tree/master/Oracle%20Inventory%20Script%20Artifacts)                 | This asset includes a PL/SQL query that hits Oracle system tables and provides a count of objects by schema type, object type, and status. It also provides a rough estimate of ‘Raw Data’ in each schema and the sizing of tables in each schema, with results stored in a CSV format.                                                                                                               |
| [Automate SSMA Oracle Assessment Collection & Consolidation](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Automate%20SSMA%20Oracle%20Assessment%20Collection%20%26%20Consolidation)                                             | This set of resource uses a .csv file as entry (sources.csv in the project folders) to produce the xml files that are needed to run SSMA assessment in console mode. The source.csv is provided by the customer based on an inventory of existing Oracle instances. The output files are AssessmentReportGeneration_source_1.xml, ServersConnectionFile.xml, and VariableValueFile.xml.|
| [SSMA for Oracle Common Errors and how to fix them](https://aka.ms/dmj-wp-ssma-oracle-errors)                                                           | With Oracle, you can assign a non-scalar condition in the WHERE clause. However, SQL Server doesn’t support this type of condition. As a result, SQL Server Migration Assistant (SSMA) for Oracle doesn’t convert queries with a non-scalar condition in the WHERE clause, instead generating an error O2SS0001. This white paper provides more details on the issue and ways to resolve it.          |
| [Oracle to SQL Server Migration Handbook](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20SQL%20Server%20Migration%20Handbook.pdf)                | This document focuses on the tasks associated with migrating an Oracle schema to the latest version of SQL Server base. If the migration requires changes to features/functionality, then the possible impact of each change on the applications that use the database must be considered carefully.                                                     |

These resources were developed as part of the Data SQL Ninja Program, which is sponsored by the Azure Data Group engineering team. The core charter of the Data SQL Ninja program is to unblock and accelerate complex modernization and compete data platform migration opportunities to Microsoft's Azure Data platform. If you think your organization would be interested in participating in the Data SQL Ninja program, please contact your account team and ask them to submit a nomination.

## Next steps

After migration, review the [Post-migration validation and optimization guide](../../../relational-databases/post-migration-validation-and-optimization-guide.md). 

For a matrix of the Microsoft and third-party services and tools that are available to assist you with various database and data migration scenarios, as well as specialty tasks, see [Data migration services and tools](/azure/dms/dms-tools-matrix).

For other migration guides, see [Database Migration](https://datamigration.microsoft.com/). 

For video content, see:
- [Overview of the migration journey](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)
