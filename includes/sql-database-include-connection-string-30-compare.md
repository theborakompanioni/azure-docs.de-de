
<!--
includes/sql-database-include-connection-string-30-compare.md

Latest Freshness check:  2015-09-03 , GeneMi.

## Connection string
-->


### <a name="compare-the-connection-string"></a>Vergleichen der Verbindungszeichenfolge
In der folgenden Tabelle werden die Verbindungszeichenfolgen verglichen, die das C#-Programm zum Herstellen einer Verbindung zum lokalen SQL Server im Vergleich zu Azure SQL-Datenbank in der Cloud benötigt. Die Unterschiede werden in Fettschrift angezeigt.

| Verbindungszeichenfolge für die <br/>Azure SQL-Datenbank | Verbindungszeichenfolge für die <br/>Microsoft SQL Server |
|:--- |:--- |
| Server=**tcp:**{Ihr_Servername}**.database.windows.net,1433**;<br/>User ID={Ihr_Anmeldename}**@{your_serverName_here}**;<br/>Kennwort={Ihr_Kennwort};<br/>**Database={Name_Ihrer-Datenbank};**<br/>**Connection Timeout=30**;<br/>**Encrypt=True**;<br/>**TrustServerCertificate=False**; |Server={Ihr_Servername};<br/>User ID={Ihr_Anmeldename};<br/>Kennwort={Ihr_Kennwort}; |

**Datenbank =** ist für SQL Server optional, aber für SQL-Datenbank erforderlich.

[SqlConnectionStringBuilder-Eigenschaften](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder_properties.aspx) – erläutert alle Parameter ausführlich.

<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->


<!--HONumber=Jan17_HO3-->


