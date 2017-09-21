---
title: "Azure SQL-Datenbank: Beispiel für mehrinstanzenfähige App – Wingtip-SaaS | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie, wie Sie eine mehrinstanzenfähige Beispielanwendung nutzen, die Azure SQL-Datenbank verwendet – das Wingtip-SaaS-Beispiel."
keywords: Tutorial zur SQL-Datenbank
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: sstein
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 68a9d97a881f3a7628a08b66091c3feb4c4dbbfe
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="introduction-to-a-sql-database-multi-tenant-saas-app-example"></a>Einführung zum Beispiel einer mehrinstanzenfähigen SQL-Datenbank-SaaS-App

Bei der SaaS-Anwendung *Wingtip* handelt es sich um eine mehrinstanzenfähige Beispiel-App, mit der die einzigartigen Vorteile der SQL-Datenbank veranschaulicht werden. Die App verwendet eine SaaS-Anwendungsmuster mit Datenbank pro Mandant, um mit mehreren Mandanten zu kommunizieren. Die App ist auf die Vorstellung der Funktionen von Azure SQL-Datenbank ausgelegt, die SaaS-Szenarien ermöglichen, einschließlich verschiedener SaaS-Entwurfs- und -Verwaltungsmuster. Um einen schnellen Einstieg sicherzustellen, wird die SaaS-App Wingtip in weniger als fünf Minuten bereitgestellt.

Der Quellcode und die Verwaltungsskripts der Anwendung stehen im GitHub-Repository [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) zur Verfügung. Um die Skripts auszuführen, [laden Sie den Ordner „Learning Modules“](#download-and-unblock-the-wingtip-saas-scripts) auf den lokalen Computer herunter.

## <a name="sql-database-wingtip-saas-tutorials"></a>Tutorials zur SaaS-App Wingtip für die SQL-Datenbank

Untersuchen Sie nach der Bereitstellung der App die folgenden Tutorials, die auf der erstmaligen Bereitstellung aufbauen. In diesen Tutorials werden allgemeine SaaS-Muster behandelt, die die integrierten Funktionen der SQL-Datenbank sowie der von SQL Data Warehouse und anderen Azure-Dienste nutzen. Die Tutorials enthalten PowerShell-Skripts sowie ausführliche Erläuterungen, die das Verständnis und das Implementieren derselben SaaS-Verwaltungsmustern in Ihren Anwendungen erheblich erleichtern.


| Tutorial | Beschreibung |
|:--|:--|
|[Bereitstellen und Kennenlernen der SaaS-Anwendung Wingtip](sql-database-saas-tutorial.md)| **BEGINNEN SIE HIER.** Stellen Sie die SaaS-Anwendung Wingtip in Ihrem Azure-Abonnement bereit, und machen Sie sich mit ihr vertraut. |
|[Bereitstellen und Katalogisieren von Mandanten](sql-database-saas-tutorial-provision-and-catalog.md)| Hier erfahren Sie, wie die Anwendung eine Verbindung mit einer Katalogdatenbank herstellt und wie den Mandanten im Katalog die entsprechenden Daten zugeordnet werden. |
|[Überwachen und Verwalten der Leistung](sql-database-saas-tutorial-performance-monitoring.md)| Hier erfahren Sie, wie Überwachungsfunktionen der SQL-Datenbank verwendet und Benachrichtigungen festgelegt werden, wenn Leistungsschwellenwerte überschritten werden. |
|[Überwachen mit Log Analytics (OMS)](sql-database-saas-tutorial-log-analytics.md) | Hier erfahren Sie, wie mithilfe von [Log Analytics](../log-analytics/log-analytics-overview.md) große Mengen von Ressourcen über mehrere Pools hinweg überwacht werden. |
|[Wiederherstellen einzelner Mandanten](sql-database-saas-tutorial-restore-single-tenant.md)| Hier erfahren Sie, wie Sie eine Mandantendatenbank zu einem früheren Zeitpunkt wiederherstellen. Die Schritte zum Wiederherstellen einer parallelen Datenbank und die Onlineschaltung der vorhandenen Mandantendatenbank werden ebenfalls erläutert. |
|[Verwalten von Mandantenschemas](sql-database-saas-tutorial-schema-management.md)| Hier erfahren Sie, wie Sie Schemas und Verweisdaten über alle Wingtip-SaaS-Mandanten hinweg aktualisieren. |
|[Ausführen von Ad-hoc-Analysen](sql-database-saas-tutorial-adhoc-analytics.md) | Erstellen Sie eine Datenbank für Ad-hoc-Analysen, und führen Sie verteilte Echtzeitabfragen für alle Mandanten aus.  |
|[Ausführen von Mandantenanalysen](sql-database-saas-tutorial-tenant-analytics.md) | Extrahieren Sie für die Offlineausführung von Analyseabfragen Mandantendaten in eine Analysedatenbank oder ein Data Warehouse. |



## <a name="application-architecture"></a>Anwendungsarchitektur

Die SaaS-App Wingtip verwendet das Modell mit einer Datenbank pro Mandant sowie elastische SQL-Pools, um die Effizienz zu maximieren. Für die Bereitstellung von Mandanten und deren Zuordnung mit den jeweiligen Daten wird eine Katalogdatenbank verwendet. Die SaaS-Kernanwendung Wingtip verwendet einen Pool mit drei Beispielmandanten sowie eine Katalogdatenbank. Um die zahlreichen Tutorials zur SaaS-Anwendung Wingtip zu absolvieren, sind bei der ersten Bereitstellung Add-ons erforderlich, wenn analytische Datenbanken, datenbankübergreifende Schemaverwaltung usw. eingeführt werden.


![Architektur der SaaS-Anwendung Wingtip](media/sql-database-wtp-overview/app-architecture.png)


Wenn Sie die Tutorials durchgehen und mit der App arbeiten, ist es wichtig, sich auf die SaaS-Muster zu konzentrieren, da sie im Zusammenhang mit der Datenebene stehen. Mit anderen Worten: Konzentrieren Sie sich auf die Datenebene, und nehmen Sie keine übermäßige Analyse der App selbst vor. Es ist von entscheidender Bedeutung, einen klaren Überblick über die Implementierung dieser SaaS-Muster zu besitzen, wenn Sie die Muster in Ihren Anwendungen implementieren und gleichzeitig nötige Modifizierungen für Ihre konkreten Geschäftsanforderungen berücksichtigen möchten.

## <a name="download-and-unblock-the-wingtip-saas-scripts"></a>Herunterladen und Aufheben der Blockierung der Wingtip-SaaS-Skripts

Wenn ZIP-Dateien von einer externen Quelle heruntergeladen und extrahiert werden, können ausführbare Inhalte (Skripts, DLLs) von Windows eventuell blockiert werden. Führen Sie bei der Extraktion der Skripts aus einer ZIP-Datei ***die nachfolgenden Schritte durch, um die Blockierung der ZIP-Datei vor der Extraktion aufzuheben***. Dadurch wird sichergestellt, dass die Ausführung der Skripts zugelassen ist.

1. Navigieren Sie zum [GitHub-Repository der Wingtip-SaaS-Anwendung](https://github.com/Microsoft/WingtipSaaS).
1. Klicken Sie auf **Klonen oder herunterladen**.
1. Klicken Sie auf **ZIP herunterladen**, und speichern Sie die Datei.
1. Klicken Sie mit der rechten Maustaste auf die Datei **WingtipSaaS-master.zip**, und wählen Sie **Eigenschaften** aus.
1. Wählen Sie auf der Registerkarte **Allgemein** die Option **Blockierung aufheben**.
1. Klicken Sie auf **OK**.
1. Extrahieren Sie die Dateien.

Die Skripts befinden sich im Ordner *..\\WingtipSaaS-master\\Learning Modules*.


## <a name="working-with-the-wingtip-saas-powershell-scripts"></a>Arbeiten mit PowerShell-Skripts für die SaaS-Anwendung Wingtip

Um das Beispiel optimal nutzen zu können, müssen Sie sich mit den bereitgestellten Skripts auseinandersetzen. Verwenden Sie Haltepunkte, und gehen Sie die Skripts durch, indem Sie sich die Einzelheiten bezüglich der Implementierung der unterschiedlichen SaaS-Muster ansehen. Um mühelos die bereitgestellten Skripts und Module durchzugehen und auf diese Weise bestmöglich zu verstehen, wird die Verwendung der [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise) empfohlen.

### <a name="update-the-configuration-file-for-your-deployment"></a>Aktualisieren der Konfigurationsdatei für Ihre Bereitstellung

Bearbeiten Sie die Datei **UserConfig.psm1** mit dem Ressourcengruppen- und Benutzerwert, den Sie bei der Bereitstellung festlegen:

1. Öffnen Sie die *PowerShell ISE*, und laden Sie ...\\Learning Modules\\*UserConfig.psm1*. 
1. Aktualisieren Sie *ResourceGroupName* und *Name* mit den jeweiligen Werten für Ihre Bereitstellung (nur in den Zeilen 10 und 11).
1. Speichern Sie die Änderungen.

Indem Sie diese Werte hier festlegen, wird es Ihnen erspart, diese bereitstellungsspezifischen Werte in jedem Skript aktualisieren zu müssen.

### <a name="execute-scripts-by-pressing-f5"></a>Ausführen von Skripts durch Drücken von F5

Zur Navigation in Ordnern verwenden die verschiedenen Skripts *$PSScriptRoot*. *$PSScriptRoot* wird nur ausgewertet, wenn die Skripts durch Drücken der Taste **F5** ausgeführt werden.  Das Markieren und Ausführen einer Auswahl (**F8**) kann zu Fehlern führen. Drücken Sie daher zum Ausführen von Skripts **F5**.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Schrittweises Durcharbeiten der Skripts zum Überprüfen der Implementierung

Die beste Möglichkeit, die Skripts zu verstehen, besteht darin, die jeweiligen Funktionen durchzugehen. Sehen Sie sich die enthaltenen **Demo-**-Skripts an, die einen einfachen allgemeinen Workflow darstellen. Die **Demo -**-Skripts veranschaulichen die erforderlichen Schritte zur Durchführung der einzelnen Aufgaben. Legen Sie daher die Haltepunkte fest, und führen Sie einen Drilldown auf die einzelnen Aufrufe durch, um Implementierungsdetails für die verschiedenen SaaS-Muster anzuzeigen.

Tipps zum Prüfen und schrittweisen Ausführen der PowerShell-Skripts:

* Öffnen Sie die **Demo-**-Skripts in der PowerShell ISE.
* Drücken Sie zum Ausführen oder Fortfahren die Taste **F5**. (Es wird davon abgeraten, **F8** zu drücken, da *$PSScriptRoot* beim Ausführen einer Auswahl aus einem Skript nicht ausgewertet wird.)
* Platzieren Sie Haltepunkte, indem Sie auf eine Zeile klicken bzw. diese auswählen und **F9** drücken.
* Überspringen Sie eine Funktion oder einen Skriptaufruf durch Drücken von **F10**.
* Führen Sie eine Funktion oder einen Skriptaufruf als Einzelschritt aus, indem Sie **F11** drücken.
* Führen Sie die aktuelle Funktion bzw. den aktuellen Skriptaufruf bis zum Rücksprung aus, indem Sie **UMSCHALT+F11** drücken.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Untersuchen des Datenbankschemas und Ausführen von SQL-Abfragen mit SSMS

Verwenden Sie [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), um eine Verbindung mit den Anwendungsservern und -datenbanken herzustellen und diese zu durchsuchen.

Die Bereitstellung enthält zunächst zwei SQL-Datenbankserver, mit denen eine Verbindung mit den Servern *tenants1-&lt;User&gt;* und *catalog-&lt;User&gt;* hergestellt werden kann. Um sicherzustellen, dass erfolgreich eine Verbindung mit der Demo hergestellt wird, wird beiden Servern eine [Firewallregel](sql-database-firewall-configure.md) zugewiesen, die alle IP-Adressen zulässt.


1. Öffnen Sie *SSMS*, und stellen Sie eine Verbindung mit dem Server *tenants1-&lt;User&gt;.database.windows.net* her.
1. Klicken Sie auf **Verbinden** > **Datenbankmodul...**:

   ![Katalogserver](media/sql-database-wtp-overview/connect.png)

1. Demo-Anmeldeinformationen: Benutzername = *developer*, Kennwort = *P@ssword1*

   ![connection](media\sql-database-wtp-overview\tenants1-connect.png)

1. Wiederholen Sie die Schritte 2 und 3, und stellen Sie eine Verbindung mit dem Server *catalog-&lt;User&gt;.database.windows.net* her.

Nachdem die Verbindung erfolgreich hergestellt wurde, werden beide Server angezeigt. Ihre Liste der Datenbanken kann variieren, je nachdem, welche Mandanten Sie bereitgestellt haben:

![Objekt-Explorer](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen der SaaS-Anwendung Wingtip](sql-database-saas-tutorial.md)
