---
title: "Ausführen von Ad-hoc-Analyseabfragen für mehrere Azure SQL-Datenbanken | Microsoft-Dokumentation"
description: "Ausführen von Ad-hoc-Analyseabfragen für mehrere Datenbanken in einer mehrinstanzenfähigen Anwendung"
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: dd41e7f1f131f6c18e03d2434982c3d681342b8b
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="run-ad-hoc-analytics-queries-across-all-wtp-tenants"></a>Ausführen von Ad-hoc-Analyseabfragen für alle WTP-Mandanten

In diesem Tutorial erstellen Sie eine Datenbank für Ad-hoc-Analysen und führen mehrere Abfragen für alle Mandanten aus. Diese Abfragen können Einblicke geben, die sonst in den tagtäglichen operativen Daten der Wingtip Tickets Platform-App (WTP) verborgen bleiben.

Zum Ausführen von Ad-hoc-Analyseabfragen (für mehrere Mandanten) verwendet die WTP-App eine [elastische Abfrage](sql-database-elastic-query-overview.md) zusammen mit einer Analysedatenbank.


In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]

> * Bereitstellen einer Ad-hoc-Analysedatenbank
> * Ausführen verteilter Abfragen für alle Mandantendatenbanken



Stellen Sie vor dem Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die WTP-App wurde bereitgestellt. Unter [Deploy and explore the WTP SaaS application (Bereitstellen und Erkunden der SaaS-Anwendung von WTP)](sql-database-saas-tutorial.md) finden Sie Informationen dazu, wie Sie die App in weniger als fünf Minuten bereitstellen.
* Azure PowerShell wurde installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).


## <a name="ad-hoc-analytics-pattern"></a>Ad-hoc-Analysemuster

Einer der großen Vorteile von SaaS-Anwendungen ist, dass Sie die riesige Menge von zentral gespeicherten Mandantendaten in der Cloud verwenden können. Sie können diese Daten verwenden, um Erkenntnisse über den Betrieb und die Verwendung Ihrer Anwendungen, Ihre Mandanten, deren Benutzer, deren Einstellungen und Verhalten zu erhalten. An den gewonnenen Erkenntnissen können Sie sich bei der Entwicklung von Funktionen, Verbesserungen der Benutzerfreundlichkeit und anderen Investitionen in Ihre Apps und Dienste orientieren.

Es ist einfach, auf diese Daten in einer Datenbank mit mehreren Mandanten zuzugreifen, aber weniger einfach bei einer Verteilung über potentiell Tausende von Datenbanken. Ein Ansatz besteht in der Verwendung flexibler Abfragen, wodurch Ad-hoc-Abfragen für einen verteilten Satz von Datenbanken mit gemeinsamem Schema ermöglicht werden. Flexible Abfragen verwenden eine einzige *Hauptdatenbank*, in der externe Tabellen definiert werden, die Tabellen in den verteilten Datenbanken (Mandantendatenbanken) spiegeln. Abfragen, die an diese Hauptdatenbank übermittelt werden, werden kompiliert, um einen Plan der verteilten Abfrage zu erzeugen, bei dem Teile der Abfrage nach Bedarf per Push an die Mandantendatenbanken übertragen werden. Flexible Abfragen verwenden die Shardzuordnung in der Katalogdatenbank, um den Speicherort der Mandantendatenbanken bereitzustellen. Setup und Abfragen verwenden einfach normales T-SQL und unterstützen Ad-hoc-Abfragen von Tools wie Power BI und Excel.

## <a name="get-the-wingtip-application-scripts"></a>Abrufen des Wingtip-Anwendungsskripts

Die Wingtip Tickets-Skripts und der Quellcode der Anwendung stehen im [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS)-GitHub-Repository zur Verfügung. Skriptdateien befinden sich im Ordner [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules) (Lernmodule). Laden Sie den Ordner **Learning Modules** auf den lokalen Computer herunter, und behalten Sie dessen Ordnerstruktur bei.

## <a name="deploy-the-database-used-for-ad-hoc-analytics-queries"></a>Bereitstellen der für Ad-hoc-Analyseabfragen verwendeten Datenbank

In dieser Übung wird die Ad-hoc-Analysedatenbank bereitgestellt, die das Schema enthält, das für das Ausstellen von Ad-hoc-Abfragen verwendet wird, die sich über alle Mandantendatenbanken erstrecken.

1. Öffnen Sie ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalytics.ps1* in der *PowerShell ISE*, und legen Sie folgende Werte fest:
   * **$DemoScenario** = 2, **Ad-hoc-Analysedatenbank bereitstellen**.

1. Drücken Sie **F5**, um das Skript auszuführen, eine neue SQL-­Datenbank für Ad-hoc-Analysen zu erstellen und sie zum WTP- Katalog hinzuzufügen. Die Tabellen TicketPurchases, Tickets und Venues werden als externe Tabellen hinzugefügt, die abgefragt werden können.
   Es ist in Ordnung, wenn hier die Warnung *Der RPC-Server ist nicht verfügbar* angezeigt wird.


Sie verfügen nun über eine *adhocanalytics*-Datenbank, mit der Sie verteilte Abfragen ausführen und Einblicke in alle Mandanten erhalten können.

## <a name="run-ad-hoc-analytics-queries"></a>Ausführen von Ad-hoc-Analyseabfragen

In dieser Übung werden Ad-hoc-Analyseabfragen ausgeführt, um Einblicke in die Mandanten der WTP-Anwendung zu erhalten.

1. Öffnen Sie ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalyticsQueries.sql* in SSMS.
1. Stellen Sie sicher, dass Sie mit der **adhocanalytics**-Datenbank verbunden sind.
1. Wählen Sie die Abfrage aus, die Sie ausführen möchten, und drücken Sie **F5**:

    ![query](media/sql-database-saas-tutorial-adhoc-analytics/query.png)




## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]

> * Bereitstellen einer Ad-hoc-Analysedatenbank
> * Ausführen verteilter Abfragen für alle Mandantendatenbanken

[Tutorial für Log Analytics (OMS) (in englischer Sprache)](sql-database-saas-tutorial-log-analytics.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Weitere Tutorials, die auf der ersten Wingtip Tickets Platform-Anwendungsbereitstellung (WTP) aufbauen](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Elastische Abfrage](sql-database-elastic-query-overview.md)

