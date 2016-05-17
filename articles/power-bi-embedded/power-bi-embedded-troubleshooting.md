<properties
   pageTitle="Problembehandlung bei der Vorschau von Microsoft Power BI Embedded"
   description="Problembehandlung bei der Vorschau von Microsoft Power BI Embedded"
   services="power-bi-embedded"
   documentationCenter=""
   authors="dvana"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="04/25/2016"
   ms.author="derrickv"/>

# Problembehandlung bei der Vorschau von Microsoft Power BI Embedded
Dieser Artikel enthält Möglichkeiten zur Problembehandlung bei **Power BI Embedded**.

<a name="connection-string"/>
## Festlegen von SQL Server-Verbindungszeichenfolgen
Beim Festlegen einer SQL Server-Verbindungszeichenfolge müssen Sie ein bestimmtes Format befolgen. Unten sehen Sie ein Beispiel für eine Verbindungszeichenfolge für SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

Weitere Informationen zu SQL Server-Verbindungszeichenfolgen finden Sie in den folgenden Artikeln:

-	[SQL Server-Verbindungszeichenfolgen](https://msdn.microsoft.com/library/jj653752.aspx)
-	[SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>
## Festlegen von Anmeldeinformationen
Falls Sie Anmeldeinformationen für eine Entwicklungs- oder eine Stagingumgebung besitzen, d.h. einen Benutzernamen und ein Kennwort, müssen Sie möglicherweise Anmeldeinformationen aktualisieren, die einer Produktionslösung entsprechen.

## Weitere Informationen
- [Erste Schritte mit dem Beispiel](power-bi-embedded-get-started-sample.md)
- [Was ist Power BI Embedded?](power-bi-embedded-what-is-power-bi-embedded.md)

<!---HONumber=AcomDC_0504_2016-->