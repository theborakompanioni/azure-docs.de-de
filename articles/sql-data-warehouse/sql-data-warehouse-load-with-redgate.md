---
title: Verwenden von Redgate zum Laden von Daten in eine Azure Data Warehouse-Instanz | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Data Platform Studio von Redgate in Data Warehousing-Szenarien verwenden.
services: sql-data-warehouse
documentationcenter: NA
author: twounder
manager: jhubbard
editor: 
ms.assetid: 670aef98-31f7-4436-86c0-cc989a39fe7f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: mausher;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2548f779767635865daf790d301d86feff573a29
ms.openlocfilehash: cb0b5489ccfabb0e2a4ee412162a18930073b309


---
# <a name="load-data-with-redgate-data-platform-studio"></a>Laden von Daten mit Redgate Data Platform Studio
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
> * [BCP](sql-data-warehouse-load-with-bcp.md)
> 
> 

In diesem Tutorial erfahren Sie, wie Sie mithilfe von [Data Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) von Redgate Daten aus einer lokalen SQL Server-Instanz in Azure SQL Data Warehouse verschieben. Data Platform Studio wendet optimal abgestimmte Kompatibilitätspatches und Optimierungen an und ermöglicht so den schnellsten Einstieg in die Verwendung von SQL Data Warehouse.

> [!NOTE]
> [Redgate](http://www.red-gate.com) ist seit langem Microsoft-Partner und Anbieter verschiedener SQL Server-Tools. Dieses Feature in Data Platform Studio wird sowohl für gewerbliche also auch für nicht gewerbliche Zwecke kostenlos zur Verfügung gestellt.
> 
> 

## <a name="before-you-begin"></a>Voraussetzungen
### <a name="create-or-identify-resources"></a>Erstellen oder Identifizieren von Ressourcen
Für dieses Tutorial benötigen Sie Folgendes:

* **Lokale SQL Server-Datenbank-Instanz**: Die Daten, die in SQL Data Warehouse importiert werden sollen, müssen aus einer lokalen SQL Server-Instanz (ab Version 2008 R2) stammen. Data Platform Studio kann Daten nicht direkt aus einer Azure SQL-Datenbank-Instanz oder aus Textdateien importieren.
* **Azure Storage-Konto**: Data Platform Studio stellt die Daten in Azure Blob Storage bereit, bevor sie in SQL Data Warehouse geladen werden. Anstelle des klassischen Bereitstellungsmodells muss das Speicherkonto das standardmäßige Resource Manager-Bereitstellungsmodell verwenden. Sollten Sie noch nicht über ein Speicherkonto verfügen, machen Sie sich mit der Erstellung eines Speicherkontos vertraut. 
* **SQL Data Warehouse**: Da die Daten in diesem Tutorial aus einer lokalen SQL Server-Instanz in SQL Data Warehouse verschoben werden, benötigen Sie ein Online-Data Warehouse. Sollten Sie noch nicht über ein Data Warehouse verfügen, machen Sie sich mit der Erstellung einer Azure SQL Data Warehouse-Instanz vertraut.

> [!NOTE]
> Zur Verbesserung der Leistung sollten sich das Speicherkonto und das Data Warehouse in der gleichen Region befinden.
> 
> 

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Schritt 1: Anmelden bei Data Platform Studio mit Ihrem Azure-Konto
Navigieren Sie in Ihrem Webbrowser zur Website [Data Platform Studio](https://www.dataplatformstudio.com/). Melden Sie sich mit dem Azure-Konto an, das Sie auch zum Erstellen des Speicherkontos und des Data Warehouse verwenden. Sollte Ihre E-Mail-Adresse sowohl mit einem Geschäfts-, Schul- oder Unikonto als auch mit einem Microsoft-Konto verknüpft sein, wählen Sie das Konto aus, das Zugriff auf Ihre Ressourcen hat.

> [!NOTE]
> Bei erstmaliger Verwendung von Data Platform Studio werden Sie aufgefordert, der Anwendung die Berechtigung zum Verwalten Ihrer Azure-Ressourcen zu gewähren.
> 
> 

## <a name="step-2-start-the-import-wizard"></a>Schritt 2: Starten des Import-Assistenten
Wählen Sie im DPS-Hauptbildschirm den Link für Azure SQL Data Warehouse aus, um den Import-Assistenten zu starten.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>Schritt 3: Installieren des Data Platform Studio-Gateways
Installieren Sie das DPS-Gateway, um eine Verbindung mit Ihrer lokalen SQL Server-Datenbank herzustellen. Bei dem Gateway handelt es sich um einen Client-Agent, der den Zugriff auf Ihre lokale Umgebung ermöglicht, die Daten extrahiert und sie in Ihr Speicherkonto hochlädt. Ihre Daten werden nicht über die Server von Redgate geleitet. So installieren Sie das Gateway:

1. Klicken Sie auf den Link **Gateway erstellen**.
2. Laden Sie das Gateway herunter, und installieren Sie es mithilfe des bereitgestellten Installationsprogramms.

![][2]

> [!NOTE]
> Das Gateway kann auf einem beliebigen Computer installiert werden, der über Netzwerkzugriff auf die SQL Server-Quelldatenbank verfügt. Beim Zugriff auf die SQL Server-Datenbank verwendet es die Windows-Authentifizierung mit den Anmeldeinformationen des aktuellen Benutzers.
> 
> 

Nach der Installation ändert sich der Gatewaystatus in „Verbunden“, und Sie können „Weiter“ auswählen.

## <a name="step-4-identify-the-source-database"></a>Schritt 4: Angeben der Quelldatenbank
Geben Sie im Textfeld *Servernamen eingeben* den Namen des Servers ein, der Ihre Datenbank hostet, und wählen Sie **Weiter** aus. Wählen Sie anschließend im Dropdownmenü die Datenbank aus, aus der Sie Daten importieren möchten.

![][3]

DPS untersucht die ausgewählte Datenbank auf importierbare Tabellen. Standardmäßig importiert DPS alle Tabellen in der Datenbank. Durch Erweitern des Links „Alle Tabellen“ können Sie Tabellen auswählen oder deren Auswahl aufheben. Wählen Sie die Schaltfläche „Weiter“ aus, um den Vorgang fortzusetzen.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Schritt 5: Auswählen eines Speicherkontos zum Bereitstellen der Daten
DPS fordert Sie auf, einen Bereitstellungsort für die Daten anzugeben. Wählen Sie ein vorhandenes Speicherkonto aus Ihrem Abonnement und anschließend **Weiter** aus.

> [!NOTE]
> DPS erstellt im ausgewählten Speicherkonto einen neuen Blobcontainer und verwendet für jeden Import einen eigenen Ordner.
> 
> 

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Schritt 6: Auswählen eines Data Warehouse
Als Nächstes wählen Sie eine Onlinedatenbank vom Typ [Azure SQL Data Warehouse](http://aka.ms/sqldw) aus, in die die Daten importiert werden sollen. Geben Sie nach dem Auswählen der Datenbank die Anmeldeinformationen für die Verbindung mit der Datenbank ein, und wählen Sie anschließend **Weiter** aus.

![][5]

> [!NOTE]
> DPS führt die Quelldatentabellen im Data Warehouse zusammen. Sollten im Data Warehouse aufgrund des Tabellennamens vorhandene Tabellen überschrieben werden, gibt DPS eine entsprechende Warnung aus. Durch Aktivieren des entsprechenden Kontrollkästchens können vor dem Importieren optional alle vorhandenen Objekte im Data Warehouse gelöscht werden.
> 
> 

## <a name="step-7-import-the-data"></a>Schritt 7: Importieren der Daten
DPS bestätigt, dass die Daten importiert werden sollen. Klicken Sie einfach auf die Schaltfläche zum Starten des Importvorgangs, um den Datenimport zu starten.

![][6]

DPS zeigt den Fortschritt beim Extrahieren und Hochladen der Daten aus der lokalen SQL Server-Instanz und den Fortschritt beim Importieren in SQL Data Warehouse an.

![][7]

Nach Abschluss des Importvorgangs zeigt DPS eine Zusammenfassung des Datenimports und einen Änderungsbericht der angewendeten Kompatibilitätspatches an.

![][8]

## <a name="next-steps"></a>Nächste Schritte
Untersuchen Sie die Daten in SQL Data Warehouse. Weitere Informationen finden Sie in den folgenden Themen:

* [Abfragen von Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]
* [Visualisieren von Daten mit Power BI][Visualize data with Power BI]

Weitere Informationen zu Data Platform Studio von Redgate finden Sie hier:

* [DPS-Startseite](http://www.dataplatformstudio.com/)
* [DPS-Demo auf Channel&9;](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Eine Übersicht über weitere Methoden zum Migrieren und Laden von Daten in SQL Data Warehouse finden Sie hier:

* [Migrieren Ihrer Lösung zu SQL Data Warehouse][Migrate your solution to SQL Data Warehouse]
* [Laden von Daten in Azure SQL Data Warehouse](sql-data-warehouse-overview-load.md)

Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht für SQL Data Warehouse](sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Query Azure SQL Data Warehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Visualize data with Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrate your solution to SQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md



<!--HONumber=Jan17_HO4-->


