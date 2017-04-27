---
title: Verwenden von Azure Data Factory mit SQL Data Warehouse | Microsoft Docs
description: "Tipps für die Verwendung von Azure Data Factory (ADF) mit Azure SQL Data Warehouse für die Entwicklung von Lösungen."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 492de762-c7a2-4cdb-943f-3135230e94f1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 12743b3594a0a62b0b974fb6dd1c39972c1bd3fa
ms.lasthandoff: 04/03/2017


---
# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Verwenden von Azure Data Factory mit SQL Data Warehouse
Azure Data Factory bietet eine vollständig verwaltete Methode zur Orchestrierung der Datenübertragung und zur Ausführung von gespeicherten Prozeduren in SQL Data Warehouse.  Dadurch können Sie auf einfachere Weise komplexe Extraktions-, Umwandlungs- und Ladeprozeduren (Extract/Transform/Load, ETL) mit SQL Data Warehouse einrichten und planen. Eine vollständige Übersicht über Azure Data Factory finden Sie in der [Azure Data Factory-Dokumentation][Azure Data Factory documentation].

## <a name="data-movement"></a>Datenverschiebung
Azure Data Factory ermöglicht eine Datenverschiebung zwischen lokalen Datenquellen und anderen Azure-Diensten.  Die aktuelle Azure Data Factory-Integration unterstützt Datenverschiebung in die folgende Speicherorte und aus diesen:

* Azure Blob Storage
* Azure SQL-Datenbank
* Lokaler SQL Server
* SQL Server unter IaaS

Informationen zum Einrichten einer Datenkopieraktivität finden Sie unter [Kopieren von Daten mit Azure Data Factory][Copy data with Azure Data Factory]

## <a name="stored-procedures"></a>Gespeicherte Prozeduren
 Auf die gleiche Weise wie beim Planen von Datenübertragungen kann Azure Data Factory auch zum Orchestrieren der Ausführung von gespeicherten Prozeduren verwendet werden.  Dies ermöglicht die Erstellung komplexerer Pipelines und erweitert die Möglichkeiten von Azure Data Factory, sodass die Rechenleistung von SQL Data Warehouse genutzt wird.

## <a name="next-steps"></a>Nächste Schritte
Einen Überblick über die Integration finden Sie unter [SQL Data Warehouse-Integration (Übersicht)][SQL Data Warehouse integration overview].
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht für SQL Data Warehouse][SQL Data Warehouse development overview].

<!--Image references-->

<!--Article references-->

[Copy data with Azure Data Factory]: ../data-factory/data-factory-data-movement-activities.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory documentation]:https://azure.microsoft.com/documentation/services/data-factory/


