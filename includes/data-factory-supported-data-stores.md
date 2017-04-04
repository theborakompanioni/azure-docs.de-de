Die Kopieraktivität in Data Factory kopiert die Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher. Data Factory unterstützt die folgenden Datenspeicher. Daten aus beliebigen Quellen können in beliebige Senken geschrieben werden. Klicken Sie auf einen Datenspeicher, um zu erfahren, wie Daten in diesen/aus diesem Speicher kopiert werden.

| Kategorie | Datenspeicher | Als Quelle unterstützt | Als Senke unterstützt |
|:--- |:--- |:--- |:--- |
| **Azure** |[Azure Blob Storage](../articles/data-factory/data-factory-azure-blob-connector.md) |✓  |✓  |
| &nbsp; |[Azure Data Lake-Speicher](../articles/data-factory/data-factory-azure-datalake-connector.md) |✓  |✓  |
| &nbsp; |[Azure DocumentDB](../articles/data-factory/data-factory-azure-documentdb-connector.md) |✓ |✓  |
| &nbsp; |[Azure SQL-Datenbank](../articles/data-factory/data-factory-azure-sql-connector.md) |✓  |✓  |
| &nbsp; |[Azure SQL Data Warehouse](../articles/data-factory/data-factory-azure-sql-data-warehouse-connector.md) |✓  |✓  |
| &nbsp; |[Azure Search-Index](../articles/data-factory/data-factory-azure-search-connector.md) | |✓  |
| &nbsp; |[Azure Table Storage](../articles/data-factory/data-factory-azure-table-connector.md) |✓  |✓  |
| **Datenbanken** |[Amazon Redshift](../articles/data-factory/data-factory-amazon-redshift-connector.md) |✓  | |
| &nbsp; |[DB2](../articles/data-factory/data-factory-onprem-db2-connector.md)* |✓  | |
| &nbsp; |[MySQL](../articles/data-factory/data-factory-onprem-mysql-connector.md)* |✓  | |
| &nbsp; |[Oracle](../articles/data-factory/data-factory-onprem-oracle-connector.md)* |✓  |✓  |
| &nbsp; |[PostgreSQL](../articles/data-factory/data-factory-onprem-postgresql-connector.md)* |✓  | |
| &nbsp; |[SAP Business Warehouse](../articles/data-factory/data-factory-sap-business-warehouse-connector.md)* |✓  | |
| &nbsp; |[SAP HANA](../articles/data-factory/data-factory-sap-hana-connector.md)* |✓  | |
| &nbsp; |[SQL Server](../articles/data-factory/data-factory-sqlserver-connector.md)* |✓  |✓  |
| &nbsp; |[Sybase](../articles/data-factory/data-factory-onprem-sybase-connector.md)* |✓  | |
| &nbsp; |[Teradata](../articles/data-factory/data-factory-onprem-teradata-connector.md)* |✓  | |
| **NoSQL** |[Cassandra](../articles/data-factory/data-factory-onprem-cassandra-connector.md)* |✓  | |
| &nbsp; |[MongoDB](../articles/data-factory/data-factory-on-premises-mongodb-connector.md)* |✓  | |
| **Datei** |[Amazon S3](../articles/data-factory/data-factory-amazon-simple-storage-service-connector.md) |✓  | |
| &nbsp; |[Dateisystem](../articles/data-factory/data-factory-onprem-file-system-connector.md)* |✓  |✓  |
| &nbsp; |[FTP](../articles/data-factory/data-factory-ftp-connector.md) |✓  | |
| &nbsp; |[HDFS](../articles/data-factory/data-factory-hdfs-connector.md)* |✓  | |
| &nbsp; |[SFTP](../articles/data-factory/data-factory-sftp-connector.md) |✓  | |
| **Andere** |[Generisches HTTP](../articles/data-factory/data-factory-http-connector.md) |✓  | |
| &nbsp; |[Generisches OData](../articles/data-factory/data-factory-odata-connector.md) |✓  | |
| &nbsp; |[Generisches ODBC](../articles/data-factory/data-factory-odbc-connector.md)* |✓  | |
| &nbsp; |[Salesforce](../articles/data-factory/data-factory-salesforce-connector.md) |✓  | |
| &nbsp; |[Webtabelle (HTML-Tabelle)](../articles/data-factory/data-factory-web-table-connector.md) |✓  | |
| &nbsp; |[GE Historian](../articles/data-factory/data-factory-odbc-connector.md#ge-historian-store)* |✓  | | |

> [!NOTE]
> Datenspeicher mit * können lokal oder in Azure IaaS verfügbar sein. Für ihre Verwendung müssen Sie das [Datenverwaltungsgateway](../articles/data-factory/data-factory-data-management-gateway.md) auf einem lokalen oder einem Azure IaaS-Computer installieren.
>
>
