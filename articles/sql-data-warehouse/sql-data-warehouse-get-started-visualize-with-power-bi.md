---
title: Visualisieren von SQL Data Warehouse-Daten mit Power BI | Microsoft Azure
description: Visualisieren von SQL Data Warehouse-Daten mit Power BI
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: d7fb89d1-da1d-4788-a111-68d0e3fda799
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: c0e2324a2b2e6294df6e502f2e7a0ae36ff94158
ms.openlocfilehash: 8790bacecac0fa824189b5c212c2d803092ff4ed
ms.lasthandoff: 01/30/2017


---
# <a name="visualize-data-with-power-bi"></a>Visualisieren von Daten mit Power BI
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

In diesem Lernprogramm erfahren Sie, wie Sie Power BI zum Herstellen einer Verbindung mit SQL Data Warehouse verwenden und einige einfache Visualisierungen erstellen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Lernprogramm ist Folgendes erforderlich:

* Eine SQL Data Warehouse-Instanz, für die die AdventureWorksDW-Datenbank vorab geladen wurde. Informationen zur Bereitstellung finden Sie unter [Erstellen eines SQL Data Warehouse][Create a SQL Data Warehouse]. Wählen Sie die Option zum Laden der Beispieldaten. Wenn Sie bereits über ein Data Warehouse verfügen, aber noch keine Beispieldaten besitzen, können Sie [Beispieldaten manuell laden][load sample data manually].

## <a name="1-connect-to-your-database"></a>1. Verbinden mit der Datenbank
Gehen Sie wie folgt vor, um Power BI zu öffnen und eine Verbindung mit der AdventureWorksDW-Datenbank herzustellen:

1. Melden Sie sich beim [Azure-Portal][Azure portal] an.
2. Klicken Sie auf **SQL-Datenbanken** , und wählen Sie Ihre AdventureWorks SQL Data Warehouse-Datenbank aus.
   
    ![Datenbank suchen][1]
3. Klicken Sie auf die Schaltfläche „Open in Power BI“.
   
    ![Power BI-Schaltfläche][2]
4. Nun sollte die Seite mit der SQL Data Warehouse-Verbindung und der Webadresse Ihrer Datenbank angezeigt werden. Klicken Sie auf "Weiter".
   
    ![Power BI-Verbindung][3]
5. Geben Sie Ihren Azure SQL Server-Benutzernamen und das Kennwort ein. Es wird eine vollständige Verbindung mit Ihrer SQL Data Warehouse-Datenbank hergestellt.
   
    ![Power BI-Anmeldung][4]
6. Nachdem Sie sich bei Power BI angemeldet haben, klicken Sie auf dem linken Blatt auf das Dataset „AdventureWorksDW“. Die Datenbank wird geöffnet.
   
    ![Power BI – Öffnen von „AdventureWorksDW“][5]

## <a name="2-create-a-report"></a>2. Erstellen eines Berichts
Sie können Power BI jetzt zum Analysieren Ihrer AdventureWorksDW-Beispieldaten verwenden. Zum Durchführen der Analyse verfügt AdventureWorksDW über die Ansicht „AggregateSales“. Diese Ansicht enthält einige wichtige Metriken zum Analysieren des Umsatzes des Unternehmens.

1. Klicken Sie zum Erstellen einer Karte mit den Umsatzbeträgen nach Postleitzahl im rechten Bereich mit den Feldern auf die Ansicht „AggregateSales“, um sie zu erweitern. Klicken Sie dann auf die Spalten „PostalCode“ und „SalesAmount“, um sie auszuwählen.
   
    ![Power BI – Auswählen von „AggregateSales“][6]
   
    Power BI erkennt dies automatisch als geografische Daten und fügt sie in eine Karte ein.
   
    ![Power BI-Karte][7]
2. Durch diesen Schritt wird ein Balkendiagramm erstellt, in der Umsatz pro Kundeneinkommen angezeigt wird. Wechseln Sie zur Erstellung dieses Diagramms zur erweiterten Ansicht „AggregateSales“. Klicken Sie auf das Feld „SalesAmount“. Ziehen Sie das Feld „Customer Income“ auf die linke Seite, und legen Sie es auf „Axis“ ab.
   
    ![Power BI – Auswählen von „Axis“][8]
   
    Wir haben das Balkendiagramm nach links verschoben.
   
    ![Power BI-Balkendiagramm][9]
3. Durch diesen Schritt wird ein Liniendiagramm erstellt, das denn Umsatz pro Bestelldatum anzeigt. Wechseln Sie zur Erstellung dieses Diagramms zur erweiterten Ansicht „AggregateSales“. Klicken Sie auf „SalesAmount“ und „OrderDate“. Klicken Sie in der Spalte mit den Visualisierungen auf das Symbol für Liniendiagramme. Dies ist das erste Symbol in der zweiten Zeile unter „Visualisierungen“.
   
    ![Power BI – Auswählen des Liniendiagramms][10]
   
    Sie haben jetzt einen Bericht mit drei verschiedenen Visualisierungen der Daten.
   
    ![Power BI-Liniendiagramm][11]

Sie können Ihren Fortschritt jederzeit speichern, indem Sie auf **Datei** und auf **Speichern** klicken.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Gelegenheit hatten, sich ein wenig mit den Beispieldaten vertraut zu machen, können Sie sich nun mit dem [Entwickeln][develop], [Laden][load] oder [Migrieren][migrate] befassen. Alternativ können Sie sich auch die [Power BI-Website][Power BI website] ansehen.

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI website]: http://www.powerbi.com/

