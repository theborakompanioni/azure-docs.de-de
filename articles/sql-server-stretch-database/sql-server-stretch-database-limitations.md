---
title: "Einschränkungen für Stretch-Datenbank | Microsoft Docs"
description: "Erfahren Sie mehr über die Einschränkungen für Stretch-Datenbank."
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 0a865872-7d42-4873-99b9-cbae45691e54
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: anvang
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 3c1599dfc6aee8adf7ec8714c392a7d8d4427d78


---
# <a name="limitations-for-stretch-database"></a>Einschränkungen für Stretch-Datenbank
Erfahren Sie mehr zu Einschränkungen für Tabellen, für die Stretch aktiviert ist und zu Einschränkungen, die Sie derzeit daran hindern, Stretch für eine Tabelle zu aktivieren.

## <a name="a-namecaveatsa-limitations-for-stretch-enabled-tables"></a><a name="Caveats"></a> Einschränkungen für Tabellen, für die Stretch aktiviert ist
Tabellen, für die Stretch aktiviert ist, weisen die folgenden Einschränkungen auf.

### <a name="constraints"></a>Einschränkungen
* Eindeutigkeit wird nicht für UNIQUE-Einschränkungen und PRIMARY KEY-Einschränkungen in der Azure-Tabelle erzwungen, die die migrierten Daten enthält.

### <a name="dml-operations"></a>DML-Vorgänge
* Die Befehle UPDATE oder DELETE können nicht für migrierte Zeilen oder für zur Migration berechtigte Zeilen in einer Tabelle, für die Stretch aktiviert ist, oder in einer Ansicht mit Tabellen ausgeführt werden, für die Stretch aktiviert ist.
* Sie können in einem verknüpften Server keine Zeilen mit dem Befehl INSERT in eine Tabelle einfügen, für die Stretch aktiviert ist.

### <a name="indexes"></a>Indexe
* Sie können keinen Index für eine Sicht erstellen, die Stretch\-fähige Tabellen enthält.
* Filter für SQL Server-Indizes werden nicht an die Remotetabelle weitergegeben.

## <a name="a-namelimitationsa-limitations-that-currently-prevent-you-from-enabling-stretch-for-a-table"></a><a name="Limitations"></a> Einschränkungen, die Sie derzeit vom Aktivieren von Stretch für eine Tabelle abhalten
Die folgenden Elemente verhindern derzeit das Aktivieren von Stretch für eine Tabelle.

### <a name="table-properties"></a>Tabelleneigenschaften
* Tabellen, die mehr als 1.023 Spalten oder mehr als 998 Indizes enthalten
* FileTables oder Tabellen, die FILESTREAM-Daten enthalten
* Tabellen, die repliziert werden oder aktiv Change Tracking oder Change Data Capture verwenden
* Speicheroptimierte Tabellen

### <a name="data-types"></a>Datentypen
* text, ntext und image
* timestamp
* SQL\_Varianten
* XML
* CLR-Datentypen, einschließlich geometry, geography, hierarchyid und benutzerdefinierte CLR-Typen

### <a name="column-types"></a>Spaltentypen
* COLUMN\_SET
* Berechnete Spalten

### <a name="constraints"></a>Einschränkungen
* Standardeinschränkungen und CHECK-Einschränkungen
* Fremdschlüsseleinschränkungen, die auf die Tabelle verweisen In einer unter- und übergeordneten Beziehung (z.B. In einer unter- und übergeordneten Beziehung \(z.B. Order und Order\_Detail\) können Sie Stretch für die untergeordnete Tabelle \(Order\_Detail\), aber nicht für die übergeordnete Tabelle \(Order\) aktivieren.

### <a name="indexes"></a>Indexe
* Volltextindizes
* XML-Indizes
* Räumliche Indizes
* Indizierte Sichten, die auf die Tabelle verweisen

## <a name="see-also"></a>Weitere Informationen
[Identifizieren von Datenbanken und Tabellen für Stretch-Datenbank durch Ausführen von Stretch Database Advisor](sql-server-stretch-database-identify-databases.md)

[Aktivieren von Stretch-Datenbank für eine Datenbank](sql-server-stretch-database-enable-database.md)

[Aktivieren von Stretch-Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md)




<!--HONumber=Nov16_HO3-->


