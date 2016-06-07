<properties
	pageTitle="Einschränkungen für Stretch-Datenbank | Microsoft Azure"
	description="Erfahren Sie mehr über die Einschränkungen für Stretch-Datenbank."
	services="sql-server-stretch-database"
	documentationCenter=""
	authors="douglaslMS"
	manager=""
	editor=""/>

<tags
	ms.service="sql-server-stretch-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/17/2016"
	ms.author="douglasl"/>

# Einschränkungen für Stretch-Datenbank

Erfahren Sie mehr zu Einschränkungen für Tabellen, für die Stretch aktiviert ist und zu Einschränkungen, die Sie derzeit daran hindern, Stretch für eine Tabelle zu aktivieren.

##  <a name="Caveats"></a>Einschränkungen für Tabellen, für die Stretch aktiviert ist

Tabellen, für die Stretch aktiviert ist, weisen die folgenden Einschränkungen auf.

### Einschränkungen

-   Eindeutigkeit wird nicht für UNIQUE-Einschränkungen und PRIMARY KEY-Einschränkungen in der Azure-Tabelle erzwungen, die die migrierten Daten enthält.

### DML-Vorgänge

-   Die Befehle UPDATE oder DELETE können nicht für migrierte Zeilen oder für zur Migration berechtigte Zeilen in einer Tabelle, für die Stretch aktiviert ist, oder in einer Ansicht mit Tabellen ausgeführt werden, für die Stretch aktiviert ist.

-   Sie können in einem verknüpften Server keine Zeilen mit dem Befehl INSERT in eine Tabelle einfügen, für die Stretch aktiviert ist.

### Indexe

-   Sie können keinen Index für eine Sicht erstellen, die Stretch-fähige Tabellen enthält.

-   Filter für SQL Server-Indizes werden nicht an die Remotetabelle weitergegeben.

##  <a name="Limitations"></a> Einschränkungen, die Sie derzeit vom Aktivieren von Stretch für eine Tabelle abhalten

Die folgenden Elemente verhindern derzeit das Aktivieren von Stretch für eine Tabelle.

### Tabelleneigenschaften

-   Tabellen, die mehr als 1.023 Spalten oder mehr als 998 Indizes enthalten

-   FileTables oder Tabellen, die FILESTREAM-Daten enthalten

-   Tabellen, die repliziert werden oder aktiv Change Tracking oder Change Data Capture verwenden

-   Speicheroptimierte Tabellen

### Datentypen

-   text, ntext und image

-   timestamp

-   sql\_variant

-   XML

-   CLR-Datentypen, einschließlich geometry, geography, hierarchyid und benutzerdefinierte CLR-Typen

### Spaltentypen

-   COLUMN\_SET

-   Berechnete Spalten

### Einschränkungen

-   Standardeinschränkungen und CHECK-Einschränkungen

-   Fremdschlüsseleinschränkungen, die auf die Tabelle verweisen In einer unter- und übergeordneten Beziehung (z.B. Bestellung und Bestellung\_Detail) können Sie Stretch für die untergeordnete Tabelle (Order\_Detail), aber nicht für die übergeordnete Tabelle (Order) aktivieren.

### Indexe

-   Volltextindizes

-   XML-Indizes

-   Räumliche Indizes

-   Indizierte Sichten, die auf die Tabelle verweisen

## Weitere Informationen

[Identifizieren von Datenbanken und Tabellen für Stretch-Datenbank durch Ausführen von Stretch Database Advisor](sql-server-stretch-database-identify-databases.md)

[Aktivieren von Stretch-Datenbank für eine Datenbank](sql-server-stretch-database-enable-database.md)

[Aktivieren von Stretch-Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md)

<!---HONumber=AcomDC_0525_2016-->