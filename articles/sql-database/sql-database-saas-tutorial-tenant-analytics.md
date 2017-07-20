---
title: "Ausführen von Analyseabfragen anhand mehrerer Azure SQL-Datenbanken | Microsoft-Dokumentation"
description: "Extrahieren von Daten aus Mandantendatenbanken in eine Analysedatenbank für die Offlineanalyse"
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 4e32407d5f321198358e07980907c3420aaf56c6
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---
# <a name="extract-data-from-tenant-databases-into-an-analytics-database-for-offline-analysis"></a>Extrahieren von Daten aus Mandantendatenbanken in eine Analysedatenbank für die Offlineanalyse

In diesem Tutorial verwenden Sie einen elastischen Auftrag zum Ausführen von Abfragen für jede Mandantendatenbank. Der Auftrag extrahiert Ticketverkaufsdaten und lädt diese zur Analyse in eine Analysedatenbank (oder Data Warehouse). Die Analysedatenbank wird dann abgefragt, um Einblicke über die tagtäglichen operativen Daten aller Mandanten zu erhalten.


In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen der Mandantenanalysedatenbank
> * Erstellen eines geplanten Auftrags zum Abrufen von Daten und Auffüllen der Analysedatenbank

Stellen Sie vor dem Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Wingtip-SaaS-App wird bereitgestellt. Unter [Bereitstellen und Kennenlernen einer mehrinstanzenfähigen SaaS-Anwendung, die Azure SQL-Datenbank verwendet](sql-database-saas-tutorial.md) finden Sie Informationen dazu, wie Sie die App in weniger als fünf Minuten bereitstellen.
* Azure PowerShell wurde installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* Die aktuelle Version von SQL Server Management Studio (SSMS) wurde installiert. [Herunterladen und Installieren von SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a>Muster der operativen Analyse der Mandanten

Einer der großen Vorteile von SaaS-Anwendungen ist, dass Sie die umfangreichen, in der Cloud gespeicherten Mandantendaten verwenden können. Mit diesen Daten können Sie Einblicke in den Betrieb und die Verwendung Ihrer Anwendung und in Ihre Mandanten erhalten. An diesen Daten können Sie sich bei der Entwicklung von Funktionen, Verbesserungen der Benutzerfreundlichkeit und anderen Investitionen in App und Plattform orientieren. Es ist einfach, auf diese Daten in einer Datenbank mit mehreren Mandanten zuzugreifen, aber weniger einfach bei einer Verteilung über potentiell Tausende von Datenbanken. Ein Ansatz besteht darin, auf diese Daten mit elastischen Aufträgen zuzugreifen, die Abfrageergebnisse von der Ausführung des Auftrags zurückgeben, die in einer Output-Datenbank und in einer Tabelle aufgezeichnet werden können.

## <a name="get-the-wingtip-application-scripts"></a>Abrufen der Wingtip-Anwendungsskripts

Die Wingtip-SaaS-Skripts und der Quellcode der Anwendung stehen im GitHub-Repository [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) zur Verfügung. [Schritte zum Herunterladen der Wingtip-SaaS-Skripts](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts)

## <a name="deploy-a-database-for-tenant-analytics-results"></a>Bereitstellen einer Datenbank für Mandantenanalyseergebnisse

Für dieses Tutorial ist es erforderlich, dass Sie eine Datenbank bereitstellen, um die Ergebnisse der Auftragsausführung von Skripts zu erfassen, die Abfrageergebnisse enthalten. Erstellen wir zu diesem Zweck eine Datenbank mit dem Namen „tenantanalytics“.

1. Öffnen Sie …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* in der *PowerShell ISE*, und legen Sie den folgenden Wert fest:
   * **$DemoScenario** = **2** *Operative Analysedatenbank bereitstellen*
1. Drücken Sie **F5** zum Ausführen des Demoskripts (das das Skript *Deploy-TenantAnalyticsDB.ps1* aufruft), wodurch die Mandantenanalysedatenbank erstellt wird.

## <a name="create-some-data-for-the-demo"></a>Erstellen einiger Daten für die Demo

1. Öffnen Sie …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* in der *PowerShell ISE*, und legen Sie den folgenden Wert fest:
   * **$DemoScenario** = **1** *Tickets für Events an allen Veranstaltungsorten kaufen*
1. Drücken Sie **F5**, um das Skript auszuführen und einen Ticketbestellverlauf zu erstellen.


## <a name="create-a-scheduled-job-to-retrieve-tenant-analytics-about-ticket-purchases"></a>Erstellen eines geplanten Auftrags zum Abrufen der Mandantenanalyse zu Ticketkäufen

Dieses Skript erstellt einen Auftrag, um Informationen zu Ticketkäufen von allen Mandanten abzurufen. Sobald die Daten in einer einzigen Tabelle aggregiert wurden, können Sie umfangreiche, aufschlussreiche Metriken zu Mustern bei Ticketkäufen für alle Mandanten erhalten.

1. Öffnen Sie SSMS, und stellen Sie eine Verbindung zum Server „catalog-&lt;user&gt;.database.windows.net“ her.
1. Öffnen Sie ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*
1. Ändern Sie &lt;User&gt;, verwenden Sie den Benutzernamen, den Sie bei der Bereitstellung der Wingtip-SaaS-App am Anfang des Skripts verwendet haben – **sp\_add\_target\_group\_member** und **sp\_add\_jobstep**.
1. Klicken Sie mit der rechten Maustaste, wählen Sie **Verbindung** aus, und stellen Sie eine Verbindung mit dem Server „catalog-&lt;User&gt;.database.windows.net“ her, falls dies noch nicht geschehen ist.
1. Stellen Sie sicher, dass Sie mit der **jobaccount**-Datenbank verbunden sind, und drücken Sie **F5**, um das Skript auszuführen.

* **sp\_add\_target\_group** erstellt den Zielgruppennamen *TenantGroup*, und nun müssen wir Zielelemente hinzufügen.
* **sp\_add\_target\_group\_member** fügt einen Zielmembertyp *server* hinzu, der alle Datenbanken auf diesem Server (dem Server „customer1-&lt;User&gt;“, der die Mandantendatenbanken enthält) zum Zeitpunkt der Ausführung des Auftrags in den Auftrag einschließt.
* **sp\_add\_job** erstellt einen neuen geplanten wöchentlichen Auftrag mit dem Namen „Ticket Purchases from all Tenants“ (Ticketkäufe von allen Mandanten).
* **sp\_add\_jobstep** erstellt den Auftragsschritt, der den T-SQL-Befehlstext enthält, um alle Informationen zu Ticketkäufen abzurufen und das zurückgegebene Resultset in eine Tabelle mit dem Namen *AllTicketsPurchasesfromAllTenants* zu kopieren.
* Die übrigen Ansichten im Skript zeigen das Vorhandensein der Objekte an und überwachen die Auftragsausführung. Überprüfen Sie den Statuswert aus der Spalte **Lebenszyklus**, um den Status zu überwachen. Wenn dies erfolgreich ausgeführt wurde, wurde der Auftrag für alle Mandantendatenbanken und die zwei zusätzlichen Datenbanken mit der Verweistabelle abgeschlossen.

Die erfolgreiche Ausführung des Skripts sollte zu ähnlichen Ergebnissen wie im Folgenden führen:

![results](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-to-retrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a>Erstellen eines Auftrags zum Abrufen der Gesamtzahl der Ticketkäufe von allen Mandanten

Dieses Skript erstellt einen Auftrag, um die Summe aller Ticketkäufe von allen Mandanten abzurufen.

1. Öffnen Sie SSMS, und stellen Sie eine Verbindung zum Server „*catalog-&lt;User&gt;.database.windows.net*“ her.
1. Öffnen Sie die Datei …\\Learning Modules\\Provision and Catalog\\Operational Analytics\\Tenant Analytics\\*Results-TicketPurchasesfromAllTenants.sql*
1. Ändern Sie &lt;User&gt;, verwenden Sie den Benutzernamen, den Sie bei der Bereitstellung der Wingtip-SaaS-App im Skript verwendet haben, in der gespeicherten Prozedur **sp\_add\_jobstep**.
1. Klicken Sie mit der rechten Maustaste, wählen Sie **Verbindung** aus, und stellen Sie eine Verbindung mit dem Server „catalog-&lt;User&gt;.database.windows.net“ her, falls dies noch nicht geschehen ist.
1. Stellen Sie sicher, dass Sie mit der **tenantanalytics**-Datenbank verbunden sind, und drücken Sie **F5**, um das Skript auszuführen.

Die erfolgreiche Ausführung des Skripts sollte zu ähnlichen Ergebnissen wie im Folgenden führen:

![results](media/sql-database-saas-tutorial-tenant-analytics/total-sales.png)



* **sp\_add\_job** erstellt einen neuen geplanten wöchentlichen Auftrag mit dem Namen „ResultsTicketsOrders“.

* **sp\_add\_jobstep** erstellt den Auftragsschritt, der den T-SQL-Befehlstext enthält, um alle Informationen zu Ticketkäufen abzurufen und das zurückgegebene Resultset in eine Tabelle mit dem Namen „CountofTicketOrders“ zu kopieren.

* Die übrigen Ansichten im Skript zeigen das Vorhandensein der Objekte an und überwachen die Auftragsausführung. Überprüfen Sie den Statuswert aus der Spalte **Lebenszyklus**, um den Status zu überwachen. Wenn dies erfolgreich ausgeführt wurde, wurde der Auftrag für alle Mandantendatenbanken und die zwei zusätzlichen Datenbanken mit der Verweistabelle abgeschlossen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellen einer Mandantenanalysedatenbank
> * Erstellen eines geplanten Auftrags zum Abrufen von Analysedaten von Mandanten

Glückwunsch!

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* Zusätzliche [Tutorials, die auf der Wingtip-SaaS-Anwendung aufbauen](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastische Aufträge](sql-database-elastic-jobs-overview.md)

