<properties 
	pageTitle="Data Factory - .NET API Change Log | Microsoft Azure" 
	description="Describes breaking changes, feature additions, bug fixes etc... in a specific version of .NET API for the Azure Data Factory." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="10/08/2015" 
	ms.author="spelluru"/>

# Azure Data Factory - .NET SDK Change Log 
This article provides information about changes to Azure Data Factory SDK in a specific version. You can find the latest Nuget package for Azure Data Factory [here](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) 

## Version 4.0.0
_Release date: 2015.10.02_

### Breaking Changes



- The Following classes/interfaces have been renamed.

| Old name | New name |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Table | [Dataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    
- The **API version** for this release is: **2015-10-01**.

- The **List** methods return paged results now. If the response contains a non-empty **NextLink** property, the client application needs to continue fetching the next page until all pages are returned.  Here is an example: 

		PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
	    var pipelines = new List<Pipeline>(response.Pipelines);
	
	    string nextLink = response.NextLink;
	    while (string.IsNullOrEmpty(response.NextLink))
	    {
	        PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
	        pipelines.AddRange(nextResponse.Pipelines);
	
	        nextLink = nextResponse.NextLink;
	    }
	
- **List** pipeline API returns only the summary of a pipeline instead of full details. For instance, activities in a pipeline summary only contain name and type.

### Feature Additions
- The [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) class supports two new properties, **SliceIdentifierColumnName** and **SqlWriterCleanupScript**, to support idempotent copy to Azure SQL Data Warehouse. See the [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) article, specifically, the [Mechanism 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) and [Mechanism 2](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) sections, for details about these properties.

- We now support running stored procedure against Azure SQL Database and Azure SQL Data Warehouse sources as part of the Copy Activity. The [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) and [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) classes have the following properties to support this: **SqlReaderStoredProcedureName** and **StoredProcedureParameters**. See the [Azure SQL Database](data-factory-azure-sql-connector.md#sqlsource) and [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) articles on Azure.com for details about these properties.  