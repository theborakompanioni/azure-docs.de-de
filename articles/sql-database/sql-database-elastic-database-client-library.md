---
title: Erstellen skalierbarer Clouddatenbanken | Microsoft Docs
description: "Erstellen skalierbarer .NET-Datenbank-Apps mit der Clientbibliothek für elastische Datenbanken"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 1f11c52d-13c1-4994-b9b1-5b1ae2f9255f
ms.service: sql-database
ms.custom: multiple databases
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 509a77a5059a39f964a1d89099998a07fd2cbfd5


---
# <a name="building-scalable-cloud-databases"></a>Erstellen skalierbarer Clouddatenbanken
Das horizontale Skalieren von Datenbanken kann problemlos mithilfe von skalierbaren Tools und Features für die Azure SQL-Datenbank ausgeführt werden. Insbesondere können Sie die **Clientbibliothek für elastische Datenbanken** verwenden, um horizontal skalierte Datenbanken zu erstellen und zu verwalten. Mit diesem Feature können Sie ganz einfach Shardanwendungen mithilfe von Hunderten – oder sogar Tausenden – von Azure SQL-Datenbanken entwickeln. [Elastische Aufträge](sql-database-elastic-jobs-powershell.md) können dann verwendet werden, um die Verwaltung dieser Datenbanken zu vereinfachen.

Um die Bibliothek zu installieren, wechseln Sie zu [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). 

## <a name="documentation"></a>Dokumentation
1. [Erste Schritte mit Tools für elastische Datenbanken](sql-database-elastic-scale-get-started.md)
2. [Features für elastische Datenbanken](sql-database-elastic-scale-introduction.md)
3. [Shard-Zuordnungsverwaltung](sql-database-elastic-scale-shard-map-management.md)
4. [Migrieren von vorhandenen Datenbanken zu horizontaler Hochskalierung](sql-database-elastic-convert-to-use-elastic-tools.md)
5. [Datenabhängiges Routing](sql-database-elastic-scale-data-dependent-routing.md)
6. [Multishardabfragen](sql-database-elastic-scale-multishard-querying.md)
7. [Hinzufügen eines Shards mithilfe der Tools für elastische Datenbanken](sql-database-elastic-scale-add-a-shard.md)
8. [Mehrinstanzenfähige Anwendungen mit elastischen Datenbanktools und zeilenbasierter Sicherheit](sql-database-elastic-tools-multi-tenant-row-level-security.md)
9. [Aktualisieren von Clientbibliothek-Apps](sql-database-elastic-scale-upgrade-client-library.md) 
10. [Übersicht über elastische Abfragen](sql-database-elastic-query-overview.md)
11. [Tools für elastische Datenbanken – Glossar](sql-database-elastic-scale-glossary.md)
12. [Clientbibliothek für elastische Datenbanken mit Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
13. [Clientbibliothek für elastische Datenbanken mit Dapper](sql-database-elastic-scale-working-with-dapper.md)
14. [Split-Merge-Tool](sql-database-elastic-scale-overview-split-and-merge.md)
15. [Leistungsindikatoren für den Shardzuordnungs-Manager](sql-database-elastic-database-client-library.md) 
16. [Häufig gestellte Fragen zu Tools für elastische Datenbanken](sql-database-elastic-scale-faq.md)

## <a name="client-capabilities"></a>Clientfunktionen
Horizontales Hochskalieren von Anwendungen mit *Sharding* konfrontiert Entwickler und Administrator gleichermaßen mit Herausforderungen. Die Clientbibliothek vereinfacht die Verwaltungsaufgaben durch Tools, mit denen sowohl Entwickler als auch Administratoren horizontal skalierte Datenbanken verwalten können. In einem typischen Beispiel gibt es viele Datenbanken zu verwalten, die als „Shards“ bezeichnet werden. Kunden werden in der gleichen Datenbank zusammengestellt, und es gibt eine Datenbank pro Kunde (ein Einzelmandantenschema). Die Clientbibliothek enthält die folgenden Features:

- **Shardzuordnungsverwaltung**: Eine spezielle Datenbank wird erstellt, der sogenannte „Shardzuordnungs-Manager“. Die Shardzuordnungsverwaltung gibt einer Anwendung die Möglichkeit, verschiedene Metadaten über die Shards zu verwalten. Entwickler können diese Funktion verwenden, um Datenbanken als Shards zu registrieren, Zuordnungen einzelner Sharding-Schlüssel oder -Schlüsselbereiche zu diesen Datenbanken zu beschreiben und diese Metadaten zu verwalten, während sich die Anzahl und Zusammensetzung der Datenbanken gemäß den Kapazitätsänderungen weiter entwickelt. Ohne die Clientbibliothek für elastische Datenbanken ist das Schreiben des Verwaltungscodes beim Implementieren von Sharding sehr zeitaufwendig. Details finden Sie unter [Shard-Zuordnungsverwaltung](sql-database-elastic-scale-shard-map-management.md).

- **Datenabhängiges Routing:**Angenommen, eine Anforderung geht bei der Anwendung ein. Anhand des Shardschlüsselwerts der Anforderung muss die Anwendung die richtige Datenbank basierend auf dem Schlüsselwert ermitteln. Anschließend öffnet sie eine Verbindung mit der Datenbank, um die Anforderung zu verarbeiten. Das datenabhängige Routing bietet die Möglichkeit, Verbindungen durch einen einfachen Aufruf über die Shard-Zuordnung der Anwendung zu öffnen. Das datenabhängige Routing ist weiterer Bereich des Infrastrukturcodes, der jetzt durch Funktionen der Clientbibliothek für elastische Datenbanken abgedeckt wird. Details finden Sie unter [Datenabhängiges Routing](sql-database-elastic-scale-data-dependent-routing.md).
- **Abfragen von mehreren Shards**: Das Abfragen mehrerer Shards erfolgt, wenn eine Anforderung mehrere (oder alle) Shards umfasst. In einer Abfrage mehrerer Shards wird derselbe T-SQL-Code für alle Shards oder eine Gruppe von Shards ausgeführt. Die Ergebnisse der beteiligten Shards werden unter Verwendung der UNION ALL-Semantik in einem Gesamtergebnis zusammengeführt. Die Funktionalität wird über die Clientbibliothek verfügbar gemacht. Sie verarbeitet u. a. Aufgaben wie Verbindungsverwaltung, Threadverwaltung, Fehlerbehandlung und Verarbeiten von Zwischenergebnissen. MSQ kann Hunderte von Shards abfragen. Weitere Einzelheiten finden Sie unter [Abfragen mehrerer Shards](sql-database-elastic-scale-multishard-querying.md).

Im Allgemeinen steht den Endkunden mit den Tools für elastische Datenbanken bei der Übermittlung lokaler Shard-Vorgänge die volle T-SQL-Funktionalität zur Verfügung, anders als bei Shard-übergreifenden Vorgängen, die eine eigene Semantik haben.

## <a name="next-steps"></a>Nächste Schritte
Probieren Sie die [Beispiel-App](sql-database-elastic-scale-get-started.md) aus, die die Clientfunktionen veranschaulicht. 

Rufen Sie zum Installieren der Bibliothek [Clientbibliothek für elastische Datenbanken](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)auf.

Informationen zur Verwendung des Split-Merge-Tools finden Sie in der [Übersicht über das Split-Merge-Tool](sql-database-elastic-scale-overview-split-and-merge.md).

[Die Clientbibliothek für elastische Datenbanken liegt nun als Open Source vor!](https://azure.microsoft.com/blog/elastic-database-client-library-is-now-open-sourced/)

Verwenden Sie [elastische Abfragen](sql-database-elastic-query-overview.md).

Die Bibliothek ist als Open Source-Software auf [GitHub](https://github.com/Azure/elastic-db-tools)verfügbar. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-database-client-library/glossary.png




<!--HONumber=Dec16_HO2-->


