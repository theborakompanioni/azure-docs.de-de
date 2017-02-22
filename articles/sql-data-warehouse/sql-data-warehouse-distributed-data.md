---
title: Funktionsweise von verteilten Daten in Azure SQL Data Warehouse | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Daten für MPP-Vorgänge (Massively Parallel Processing) verteilt werden, und Sie erhalten Informationen zu den Optionen zum Verteilen von Tabellen in Azure SQL Data Warehouse und Parallel Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: bae494a6-7ac5-4c38-8ca3-ab2696c63a9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2548f779767635865daf790d301d86feff573a29
ms.openlocfilehash: 195b78a7f634d01f228c90efb34763e4175708ac


---
# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Verteilte Daten und verteilte Tabellen für MPP (Massively Parallel Processing)
Es wird beschrieben, wie Benutzerdaten in Azure SQL Data Warehouse und Parallel Data Warehouse – den MPP-Systemen (Massively Parallel Processing) von Microsoft – verteilt werden. Wenn Sie Ihr Data Warehouse so entwerfen, dass verteilte Daten effektiv verwendet werden, können Sie Vorteile der MPP-Architektur in Bezug auf die Abfrageverarbeitung nutzen. Einige Entscheidungen beim Datenbankentwurf können erhebliche Auswirkungen auf die Verbesserung der Abfrageleistung haben.  

> [!NOTE]
> Für Azure SQL Data Warehouse und Parallel Data Warehouse wird der gleiche MPP-Entwurf (Massively Parallel Processing) verwendet, aber aufgrund der zugrunde liegenden Plattform gibt es einige Unterschiede. SQL Data Warehouse ist ein Dienst vom Typ „Platform as a Service (PaaS)“, der in Azure ausgeführt wird. Parallel Data Warehouse wird unter Analytics Platform System (APS) ausgeführt. Hierbei handelt es sich um eine lokale Appliance, die unter Windows Server ausgeführt wird.
> 
> 

## <a name="what-is-distributed-data"></a>Was sind verteilte Daten?
In SQL Data Warehouse und Parallel Data Warehouse sind verteilte Daten Benutzerdaten, die an mehreren Speicherorten im MPP-System gespeichert werden. Jeder dieser Speicherorte dient als unabhängige Speicher- und Verarbeitungseinheit, die Abfragen für den jeweiligen Teil der Daten ausführt. Verteilte Daten sind für die parallele Ausführung von Abfragen wichtig, um eine hohe Abfrageleistung zu erzielen.

Zum Verteilen von Daten weist das Data Warehouse jede Zeile einer Benutzertabelle einem verteilten Speicherort zu.  Sie können Tabellen per Hashverteilung oder mit einer Roundrobin-Methode verteilen. Die Verteilungsmethode ist in der CREATE TABLE-Anweisung angegeben. 

## <a name="hash-distributed-tables"></a>Tabellen mit Hashverteilung
Im folgenden Diagramm ist dargestellt, wie eine vollständige (nicht verteilte) Tabelle als Tabelle mit Hashverteilung gespeichert wird. Mit einer deterministischen Funktion wird jede Zeile einer Verteilung zugewiesen. In der Tabellendefinition wird eine der Spalten als Verteilungsspalte festgelegt. Die Hashfunktion verwendet den Wert in der Verteilungsspalte, um jede Zeile einer Verteilung zuzuweisen.

Bei der Auswahl einer Verteilungsspalte sollten einige Aspekte in Bezug auf die Leistung beachtet werden, z.B. Eindeutigkeit, eventuelle Datenschiefe und die Arten von Abfragen, die im System ausgeführt werden.

![Verteilte Tabelle](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "Verteilte Tabelle")  

* Jede Zeile gehört zu einer Verteilung.  
* Mit einem deterministischen Hashalgorithmus wird jede Zeile einer Verteilung zugewiesen.  
* Die Anzahl von Tabellenzeilen pro Verteilung variiert, wie an den unterschiedlichen Größen von Tabellen zu sehen ist.

## <a name="round-robin-distributed-tables"></a>Tabellen mit Roundrobin-Verteilung
Bei einer Tabelle mit Roundrobin-Verteilung werden die Zeilen verteilt, indem jede Zeile nacheinander einer Verteilung zugewiesen wird. Es geht schnell, Daten in eine Roundrobin-Tabelle zu laden, aber unter Umständen leidet die Abfrageleistung.  Für das Verknüpfen einer Roundrobin-Tabelle ist normalerweise eine erneute Anordnung der Zeilen erforderlich, damit die Abfrage ein genaues Ergebnis produzieren kann. Dieser Vorgang dauert einige Zeit.

## <a name="distributed-storage-locations-are-called-distributions"></a>Verteilte Speicherorte werden als „Verteilungen“ bezeichnet.
Jeder verteilte Speicherort ist eine Verteilung. Wenn eine Abfrage parallel ausgeführt wird, führt jede Verteilung eine SQL-Abfrage für den jeweiligen Teil der Daten durch. Für SQL Data Warehouse wird SQL-Datenbank zum Ausführen der Abfrage verwendet. Parallel Data Warehouse nutzt SQL Server, um die Abfrage auszuführen. Dieser Entwurf mit einer Shared-Nothing-Architektur ist wichtig, um paralleles Computing mit horizontaler Hochskalierung zu erreichen.

### <a name="can-i-view-the-distributions"></a>Kann ich die Verteilungen anzeigen?
Jede Verteilung verfügt über eine Verteilungs-ID und ist in den Systemsichten von SQL Data Warehouse und Parallel Data Warehouse sichtbar. Sie können die Verteilungs-ID verwenden, um die Problembehandlung für die Abfrageleistung durchzuführen und andere Probleme zu lösen. Eine Liste mit den Systemsichten finden Sie in der [Übersicht über die MPP-Systemsichten](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Unterschied zwischen einer Verteilung und einem Computeknoten
Eine Verteilung ist die Basiseinheit zum Speichern von verteilten Daten und Verarbeiten von parallelen Abfragen. Verteilungen werden zu Computeknoten gruppiert. Jeder Computeknoten verfolgt mindestens eine Verteilung nach.  

* Analytics Platform System verwendet Computeknoten als zentrale Komponente der Hardwarearchitektur und der Funktionen zum horizontalen Hochskalieren. Es werden immer acht Verteilungen pro Computeknoten verwendet. Dies ist im Diagramm für Tabellen mit Hashverteilung dargestellt. Die Anzahl von Computeknoten und somit auch die Anzahl von Verteilungen wird anhand der Anzahl von Computeknoten bestimmt, die Sie für die Appliance erwerben. Wenn Sie beispielsweise acht Computeknoten erwerben, erhalten Sie 64 Verteilungen (8 Computeknoten · 8 Verteilungen/Knoten). 
* SQL Data Warehouse verfügt über eine feste Anzahl von 60 Verteilungen und eine flexible Anzahl von Computeknoten. Die Computeknoten werden mit Azure-Computing- und -Speicherressourcen implementiert. Die Anzahl von Computeknoten kann sich je nach der Back-End-Dienstworkload und Computingkapazität (DWUs) ändern, die Sie für das Data Warehouse angeben. Wenn sich die Anzahl von Computeknoten ändert, ändert sich auch die Anzahl von Verteilungen pro Computeknoten. 

### <a name="can-i-view-the-compute-nodes"></a>Kann ich die Computeknoten anzeigen?
Jeder Computeknoten verfügt über eine Knoten-ID und ist in den Systemsichten von SQL Data Warehouse und Parallel Data Warehouse sichtbar.  Sie können die Computeknoten anzeigen, indem Sie in den Systemsichten, deren Namen mit „sys.pdw_nodes“ beginnen, nach der Spalte „node_id“ suchen. Eine Liste mit den Systemsichten finden Sie in der [Übersicht über die MPP-Systemsichten](sql-data-warehouse-reference-tsql-statements.md).

## <a name="a-namereplicatedareplicated-tables-for-parallel-data-warehouse"></a><a name="Replicated"></a>Replizierte Tabellen für Parallel Data Warehouse
Gilt für: Parallel Data Warehouse

Zusätzlich zur Verwendung von verteilten Tabellen verfügt Parallel Data Warehouse auch über eine Option zum Replizieren von Tabellen. Eine *replizierte Tabelle* ist eine Tabelle, die vollständig auf jedem Computeknoten gespeichert wird. Bei der Replikation einer Tabelle ist es nicht mehr nötig, die Tabellenzeilen zwischen Computeknoten zu übertragen, bevor die Tabelle in einer Verknüpfung oder Aggregation verwendet wird. Für die Replikation von Tabellen sind nur kleine Tabellen geeignet, weil mehr Speicherplatz zum Speichern der vollständigen Tabelle auf jedem Computeknoten erforderlich ist.  

Im folgenden Diagramm ist eine replizierte Tabelle dargestellt, die auf jedem Computeknoten gespeichert wird. Die replizierte Tabelle wird auf allen Datenträgern gespeichert, die dem Computeknoten zugewiesen sind. Diese Datenträgerstrategie wird implementiert, indem SQL Server-Dateigruppen verwendet werden.  

![Replizierte Tabelle](media/sql-data-warehouse-distributed-data/replicated-table.png "Replizierte Tabelle") 

## <a name="next-steps"></a>Nächste Schritte
Informationen zur effektiven Nutzung von verteilten Tabellen finden Sie unter [Verteilen von Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-distribute.md).  




<!--HONumber=Jan17_HO4-->


