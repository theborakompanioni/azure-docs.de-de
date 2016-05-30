<properties
   pageTitle="Verwalten von Tabellen und Indizes in Azure SQL Data Warehouse | Microsoft Azure"
   description="Übersicht über Überlegungen, bewährte Methoden und Aufgaben für die Verwaltung von Tabellen und Indizes in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/02/2016"
   ms.author="jrj;barbkess;sonyama"/>

# Verwalten von Tabellen und Indizes in Azure SQL Data Warehouse

Übersicht über Überlegungen, bewährte Methoden und Aufgaben für die Verwaltung von Tabellen und Indizes in SQL Data Warehouse.



| Kategorie | Aufgabe oder Überlegungen | Beschreibung |
| :-----------------------| :---------------------------------------------- | :----------- |
| Columnstore-Indizes | Neuerstellung von Indizes nach dem Laden oder Einfügen von Daten | Standardmäßig speichert SQL Data Warehouse jede Tabelle als gruppierten Columnstore-Index. Dies bietet Ihnen eine bessere Leistung und Datenkomprimierung, insbesondere für große Tabellen. Je nachdem, wie Sie Daten im Columnstore-Index erfassen, können alle Daten möglicherweise nicht mit der Columnstore-Komprimierung gespeichert werden. In diesem Fall erhalten Sie möglicherweise nicht die Leistung, die Columnstore-Indizes bieten sollten. <br/><br/>Lesen Sie sich den Artikel [Verwalten von Columnstore-Indizes][] durch, um sicherzustellen, dass sich Columnstore-Indizes in einem ordnungsgemäßen Zustand befinden. |
| Tabellen mit Hashverteilung | Stellt sicher, dass die Daten gleichmäßig auf die Knoten verteilt sind | Tabellen mit Hashverteilung sind fast immer die beste Möglichkeit, Verknüpfungen und Aggregationen für große Tabellen zu optimieren. SQL Data Warehouse verteilte die Tabellen basierend auf einer angegebenen Verteilungsspalte. Einige Spalten stellen gute Verteilungsschlüssel dar und einige nicht. Sie möchten, dass die Zeilen gleichmäßig verteilt werden. Ein gewisser Grad an Ungleichheit bei der Verteilung oder Datenschiefe ist in Ordnung. Wird die Datenschiefe jedoch zu groß, gehen die Vorteile von MPP (Massively Parallel Process) verloren, die von SQL Data Warehouse geboten werden sollen. <br/><br/>Gehen Sie unter [Verwalten von Datenschiefe][], um sicherzustellen, dass die Tabellen mit Hashverteilung keine zu hohe Datenschiefe aufweisen. |





## Nächste Schritte

Weitere Verwaltungstipps finden Sie in der Übersicht über die [Verwaltung][].

<!--Image references-->

<!--Article references-->
[Verwalten von Columnstore-Indizes]: sql-data-warehouse-manage-columnstore-indexes.md
[Verwalten von Datenschiefe]: sql-data-warehouse-manage-distributed-data-skew.md
[Verwaltung]: sql-data-warehouse-overview-manage.md

<!--MSDN references-->


<!--Other Web references-->

<!---HONumber=AcomDC_0518_2016-->