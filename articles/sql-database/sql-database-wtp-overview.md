---
title: "Mehrinstanzenfähige Beispiel-App für Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Einführung in die Wingtip Tickets-Beispiel-App (WTP) für Azure SQL-Datenbank"
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
ms.openlocfilehash: 265eab8104d8af7c510a88dffb9d70a2b3b37631
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="introduction-to-the-wingtip-tickets-platform-wtp-sample-saas-application"></a>Einführung in die Wingtip Tickets Platform-SaaS-Beispielanwendung (WTP)

Die Wingtip Tickets Platform-SaaS-Anwendung (WTP) ist eine mehrinstanzenfähige Beispiel-App, mit der die einzigartigen Vorteile von SQL-Datenbank veranschaulicht werden. Die App verwendet eine SaaS-Anwendungsmuster mit Datenbank pro Mandant, um mit mehreren Mandanten zu kommunizieren. Die WTP-App ist auf die Vorstellung der Funktionen von Azure SQL-Datenbank ausgelegt, die SaaS-Szenarien ermöglichen, einschließlich von SaaS-Entwurfs- und -Verwaltungsmustern. Ein schneller Einstieg ist sichergestellt, denn [die WTP-App wird in weniger als fünf Minuten bereitgestellt](sql-database-saas-tutorial.md)!

Untersuchen Sie nach dem Bereitstellen der WTP-App die [Sammlung von Tutorials](#sql-database-saas-tutorials), die auf der erstmaligen Bereitstellung aufbauen. Der Schwerpunkt jedes Lernprogramms liegt auf typischen Aufgaben, die in SaaS-Anwendungen implementiert werden. Aufgaben werden gemäß SaaS-Mustern implementiert, die die integrierten Funktionen von SQL-Datenbank nutzen. So werden u.a. folgende Muster beschrieben: Bereitstellen neuer Mandanten, Wiederherstellen von Mandantendatenbanken, Ausführen verteilter Abfragen für alle Mandanten und das Rollout von Schemaänderungen in allen Mandantendatenbanken. Jedes Tutorial enthält wiederverwendbare Skripts sowie ausführliche Erläuterungen, die das Verständnis und das Implementieren der gleichen SaaS-Verwaltungsmustern in Ihren Anwendungen erheblich erleichtern.

Die WTP-Anwendung mag zwar in sich abgeschlossen und für eine Beispielanwendung kompliziert wirken, es ist jedoch unerlässlich, sich auf die wichtigsten SaaS-Muster zu konzentrieren, da sich diese auf die Datenebene beziehen. Mit anderen Worten: Konzentrieren Sie sich auf die Datenebene, und nehmen Sie keine übermäßige Analyse der App selbst vor. Das Verständnis der Implementierung dieser wichtigen SaaS-Muster ist unverzichtbar, wenn Sie die Muster in Ihren Anwendungen implementieren und gleichzeitig nötige Modifizierungen für Ihre konkreten Geschäftsanforderungen berücksichtigen möchten.



## <a name="application-architecture"></a>Anwendungsarchitektur

Die WTP-App verwendet das Modell mit einer Datenbank pro Mandant sowie elastische SQL-Pools, um die Effizienz zu maximieren.
Verwenden Sie einen Mandantenkatalog zum Verwalten der Bereitstellung und Herstellen von Verbindungen.
Integrierte App-, Pool- und Datenbanküberwachung sowie Warnungen (OMS).
Mandantenübergreifende Verwaltung von Schema- und Verweisdaten (Aufträge für die elastische Datenbank).
Mandantenübergreifende Abfrage, operative Analyse (elastische Abfrage).
Verwendung geografisch verteilter Daten für eine größere Reichweite.
Geschäftskontinuität, Notfallwiederherstellung mit gestaffelter Wiederherstellung einzelner Mandanten (PITR) (geografische Wiederherstellung, Georeplikation, automatische Notfallwiederherstellung), Self-Service-Verwaltung von Mandanten (über Verwaltungs-APIs), PITR für Wiederherstellung nach selbst verursachten Zwischenfällen.

Die Wingtip-Kernanwendung verwendet einen Pool mit drei Beispielmandanten sowie eine Katalogdatenbank.

![WTP-Architektur](media/sql-database-wtp-overview/wtp-architecture.png)


## <a name="sql-database-wtp-saas-tutorials"></a>WTP-SaaS-Tutorials für SQL-Datenbank

Die folgenden Tutorials bauen auf der erstmaligen Bereitstellung der [Wingtip Tickets Platform-SaaS-Beispielanwendung](sql-database-saas-tutorial.md) auf:

| Bereich | Beschreibung | Speicherort des Skripts |
|:--|:--|:--|
|[Tutorial zum Bereitstellen und Katalogisieren von Mandanten](sql-database-saas-tutorial-provision-and-catalog.md)| Bereitstellen neuer Mandanten und Registrieren der Mandanten im Katalog | [Skripts auf GitHub](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Provision%20and%20Catalog) |
|[Tutorial zum Überwachen und Verwalten der Leistung](sql-database-saas-tutorial-performance-monitoring.md)| Überwachen und Verwalten der Datenbank- und Pool-Leistung | [Skripts auf GitHub](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Performance%20Monitoring%20and%20Management) |
|[Tutorial zum Wiederherstellen eines einzelnen Mandanten](sql-database-saas-tutorial-restore-single-tenant.md)| Wiederherstellen von Mandantendatenbanken | [Skripts auf GitHub](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Business%20Continuity%20and%20Disaster%20Recovery/RestoreTenant) |
|[Tutorial zum Verwalten von Mandantenschemas](sql-database-saas-tutorial-schema-management.md)| Ausführen von Abfragen für alle Mandanten  | [Skripts auf GitHub](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Schema%20Management) |
|[Tutorial zum Ausführen von Ad-hoc-Analysen](sql-database-saas-tutorial-adhoc-analytics.md) | Erstellen einer Datenbank für Ad-hoc-Analysen und Ausführen von Abfragen für alle Mandanten  | [Skripts auf GitHub](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Operational%20Analytics/Adhoc%20Analytics) |
|[Tutorial zum Verwalten mit Log Analytics (OMS)](sql-database-saas-tutorial-log-analytics.md) | Konfigurieren und Kennenlernen von Log Analytics | [Skripts auf GitHub](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Performance%20Monitoring%20and%20Management/LogAnalytics) |
|[Tutorial zum Ausführen von Mandantenanalysen](sql-database-saas-tutorial-tenant-analytics.md) | Einrichten und Ausführen von Abfragen zur Mandantenanalyse | [Skripts auf GitHub](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Operational%20Analytics/Tenant%20Analytics) |

## <a name="get-the-wingtip-application-scripts"></a>Abrufen der Wingtip-Anwendungsskripts

Die Wingtip Tickets-Skripts und der Quellcode der Anwendung stehen im [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS)-GitHub-Repository zur Verfügung. Skriptdateien befinden sich im Ordner [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Laden Sie den Ordner **Learning Modules** auf den lokalen Computer herunter, wobei Sie dessen Ordnerstruktur beibehalten.

## <a name="working-with-the-wtp-powershell-scripts"></a>Arbeiten mit den WTP-PowerShell-Skripts

Die Vorteile des Arbeitens mit der WTP-Anwendung lassen sich erschließen, indem Sie in die bereitgestellten Skripts eintauchen und untersuchen, wie die verschiedenen SaaS-Muster implementiert werden.

Verwenden Sie die [Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise), um die bereitgestellten Skripts und Module anzuzeigen und durchzuarbeiten, um ein umfassendes Verständnis zu erlangen. Da die meisten mit dem Präfix *Demo-* versehenen Skripts Variablen enthalten, die Sie vor dem Ausführen ändern können, erleichtert die Verwendung der PowerShell ISE das Arbeiten mit diesen Skripts.

Für jede WTP-App-Bereitstellung ist eine Datei **UserConfig.psm1** mit zwei Parametern vorhanden, mit denen die Werte für Ressourcengruppe und Benutzername festgelegt werden, die Sie während der Bereitstellung definiert haben. Bearbeiten Sie nach Abschluss der Bereitstellung das Modul **UserConfig.psm1**, indem Sie die Parameter _ResourceGroupName_ und _Name_ festlegen. Diese Werte werden von anderen Skripts für deren erfolgreiche Ausführung verwendet. Es wird daher dringend empfohlen, sie bei Abschluss der Bereitstellung festzulegen!



### <a name="execute-scripts-by-pressing-f5"></a>Ausführen von Skripts durch Drücken von F5

Verschiedene Skripts ermöglichen das Navigieren durch Ordner mit *$PSScriptRoot*, und diese Variable wird nur ausgewertet, wenn das Skript durch Drücken von **F5** ausgeführt wird.  Das Markieren und Ausführen einer Auswahl (**F8**) kann Fehler bewirken. Drücken Sie daher zum Ausführen von WTP-Skripts **F5**.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Schrittweises Durcharbeiten der Skripts zum Überprüfen der Implementierung

Am besten machen Sie sich mit den Skripts vertraut, indem Sie sie schrittweise durcharbeiten, um die ausgeführten Aktionen nachzuvollziehen. Sehen Sie die Skripts der ersten Ebene mit _Demo-_ im Namen durch, die einen allgemeinen leicht lesbaren Workflow bieten, in dem die einzelnen Schritte zum Ausführen der jeweiligen Aufgabe veranschaulicht werden. Untersuchen Sie die einzelnen Aufrufe eingehender, um Implementierungsdetails für die verschiedenen SaaS-Muster aufzurufen.

Tipps zum Arbeiten mit und zum [Debuggen von PowerShell-Skripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise):

* Öffnen und konfigurieren Sie Demo-Skripts in der PowerShell ISE.
* Starten bzw. führen Sie die Ausführung mit **F5** fort. Es wird davon abgeraten, **F8** zu drücken, da *$PSScriptRoot* beim Ausführen einer Auswahl aus einem Skript nicht ausgewertet wird.
* Platzieren Sie Haltepunkte, indem Sie auf eine Zeile klicken bzw. diese auswählen und **F9** drücken.
* Überspringen Sie eine Funktion oder einen Skriptaufruf durch Drücken von **F10**.
* Führen Sie eine Funktion oder einen Skriptaufruf als Einzelschritt aus, indem Sie **F11** drücken.
* Führen Sie die aktuelle Funktion bzw. den aktuellen Skriptaufruf bis zum Rücksprung aus, indem Sie **UMSCHALT+F11** drücken.




## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Untersuchen des Datenbankschemas und Ausführen von SQL-Abfragen mit SSMS

Verwenden Sie [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), um Verbindungen mit den WTP-Servern und -Datenbanken herzustellen und diese zu durchsuchen.

Die WTP-Beispiel-App enthält zunächst zwei SQL-Datenbankserver, mit denen eine Verbindung hergestellt werden kann – den Server *tenants1* und den Server *catalog*:


1. Öffnen Sie *SSMS*, und stellen Sie eine Verbindung mit dem Server *tenants1-&lt;User&gt;.database.windows.net* her.
2. Klicken Sie auf **Verbinden** > **Datenbankmodul...**:

   ![Katalogserver](media/sql-database-wtp-overview/connect.png)

1. Demo-Anmeldeinformationen: Benutzername = *developer*, Kennwort = *P@ssword1*

   ![connection](media\sql-database-wtp-overview\tenants1-connect.png)

1. Wiederholen Sie die Schritte 2 und 3, und stellen Sie eine Verbindung mit dem Server *catalog-&lt;User&gt;.database.windows.net* her.

Nachdem die Verbindung erfolgreich hergestellt wurde, werden beide Server angezeigt. Es wird u.U. eine größere oder kleinere Anzahl von Datenbanken angezeigt, je nachdem, wie viele Mandanten Sie bereitgestellt haben:

![Objekt-Explorer](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen des Wingtip Tickets SaaS-Anwendungsbeispiels](sql-database-saas-tutorial.md)
