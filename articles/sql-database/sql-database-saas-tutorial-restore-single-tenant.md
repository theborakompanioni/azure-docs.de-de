---
title: Wiederherstellen einer Datenbank eines einzelnen Mandanten (SaaS-Beispielanwendung unter Verwendung von Azure SQL-Datenbank) | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie die SQL-Datenbank-Instanz eines einzelnen Mandanten nach versehentlichem Löschen von Daten wiederherstellen."
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
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: billgib;sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: aa5759645713c5e5bc4c4f1d2b10f032efc7eae2
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="restore-a-single-tenant-database"></a>Wiederherstellen der Datenbank eines einzelnen Mandanten

Die Wingtip Tickets SaaS-App wird mittels eines Datenbank-pro-Mandant-Modells erstellt, wobei jeder Mandant seine eigene Datenbank besitzt. Einer der Vorteile dieses Modells ist, dass die Daten eines einzigen Mandanten mühelos isoliert – d.h. ohne Auswirkungen auf andere Mandanten – wiederhergestellt werden können.

In diesem Tutorial lernen Sie zwei Muster der Datenwiederherstellung kennen:

> [!div class="checklist"]

> * Wiederherstellen einer Datenbank in eine parallele Datenbank (Seite-an-Seite)
> * Direktes Wiederherstellen einer Datenbank


|||
|:--|:--|
| **Wiederherstellen des Mandanten mit dem Status eines früheren Zeitpunkts in eine parallele Datenbank** | Dieses Muster kann der Mandant zu Überprüfung, Überwachung, Konformität usw. verwenden. Die ursprüngliche Datenbank bleibt online und unverändert. |
| **Direktes Wiederherstellen des Mandanten** | Dieses Muster dient normalerweise zum Wiederherstellen eines Mandanten mit dem Status eines früheren Zeitpunkts, nachdem ein Mandant versehentlich Daten gelöscht hat. Die ursprüngliche Datenbank wird offline geschaltet und durch die wiederhergestellte Datenbank ersetzt. |
|||

Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die WTP-App wurde bereitgestellt. Unter [Bereitstellen und Kennenlernen der SaaS-Anwendung WTP](sql-database-saas-tutorial.md) finden Sie Informationen dazu, wie Sie die App in weniger als fünf Minuten bereitstellen.
* Azure PowerShell ist installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-tenant-restore-pattern"></a>Einführung in das SaaS-Muster zur Mandantenwiederherstellung

Es gibt zwei einfache Muster zum Wiederherstellen der Daten eines einzelnen Mandanten. Da Mandantendatenbanken voneinander isoliert sind, hat das Wiederherstellen eines Mandanten keine Auswirkungen auf Daten anderer Mandanten.

Im ersten Muster werden die Daten in eine neue Datenbank wiederhergestellt. Der Mandant erhält dann zusammen mit seinen Produktionsdaten Zugriff auf diese Datenbank. Anhand dieses Musters kann ein Mandantenadministrator die wiederhergestellten Daten prüfen und potenziell zum selektiven Überschreiben aktueller Datenwerte verwenden. Es liegt am SaaS-App-Designer, zu bestimmen, wie anspruchsvoll die Optionen für die Datenwiederherstellung sein sollen. In einigen Szenarien mag es ausreichen, die Daten einfach in dem Status überprüfen zu können, in dem sie sich zu einem bestimmten Zeitpunkt befanden. Wenn die Datenbank die [Georeplikation](sql-database-geo-replication-overview.md) verwendet, empfiehlt es sich, die erforderlichen Daten aus der wiederhergestellten Kopie in die ursprüngliche Datenbank zu kopieren. Wenn Sie die ursprüngliche Datenbank durch die wiederhergestellte Datenbank ersetzen, müssen Sie die Georeplikation neu konfigurieren und synchronisieren.

Im zweiten Muster, wo davon ausgegangen wird, dass der Mandant einen Verlust oder eine Beschädigung von Daten erlitten hat, wird seine Produktionsdatenbank in dem Status wiederhergestellt, in dem sie sich zu einem früheren Zeitpunkt befunden hat. Bei dem Muster mit direkter Wiederherstellung wird der Mandant für kurze Zeit offline geschaltet, während die Datenbank wiederhergestellt und wieder online geschaltet wird. Die ursprüngliche Datenbank wird gelöscht, kann aber immer noch wiederhergestellt werden, wenn Sie zum Status eines früheren Zeitpunkts zurückkehren müssen. Eine Variante dieses Muster könnte die Datenbank umbenennen, statt sie zu löschen, auch wenn die Umbenennung der Datenbank keinen zusätzlichen Vorteil im Hinblick auf Sicherheit bietet.

## <a name="get-the-wingtip-application-scripts"></a>Abrufen des Wingtip-Anwendungsskripts

Die Wingtip Tickets-Skripts und der Quellcode der Anwendung stehen im [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS)-Github-Repository zur Verfügung. Skriptdateien befinden sich im Ordner [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules) (Lernmodule). Laden Sie den Ordner **Learning Modules** auf den lokalen Computer herunter, wobei Sie dessen Ordnerstruktur beibehalten.

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Simulieren des versehentlichen Löschens von Daten durch den Mandanten

Um diese Wiederherstellungsszenarien zu demonstrieren, müssen wir *versehentlich* einige Daten in einer der Mandantendatenbanken löschen. Sie können zwar jeden Datensatz löschen, aber der nächste Schritt richtet die Demo so ein, dass keine Blockade durch Verletzungen der referenziellen Integrität entstehen kann! Es werden auch einige Ticketkaufdaten hinzugefügt, die Sie weiter unten in den *WTP Analytics-Tutorials* verwenden können.

Führen Sie das Ticketgeneratorskript aus, und erstellen Sie zusätzliche Daten. Der Ticketgenerator kauft absichtlich nicht Tickets für das letzte Ereignis jedes Mandanten.

1. Öffnen Sie ...\\Learning Modules\\Utilities\\*Demo-TicketGenerator.ps1* in der *PowerShell ISE*.
1. Drücken Sie **F5**, um das Skript auszuführen und Kunden- und Ticketverkaufsdaten zu generieren.


### <a name="open-the-events-app-to-review-the-current-events"></a>Öffnen der Events-App, um die aktuellen Ereignisse zu prüfen

1. Öffnen Sie den *Events Hub* (http://events.wtp.&lt;user&gt;.trafficmanager.net), und klicken Sie auf **Contoso Concert Hall**:

   ![Events Hub](media/sql-database-saas-tutorial-restore-single-tenant/events-hub.png)

1. Scrollen Sie durch die Liste der Ereignisse, und notieren Sie sich das letzte Ereignis in der Liste:

   ![Letztes Ereignis](media/sql-database-saas-tutorial-restore-single-tenant/last-event.png)


### <a name="run-the-demo-scenario-to-accidentally-delete-the-last-event"></a>Ausführen des Demoszenarios, um das letzte Ereignis versehentlich zu löschen

1. Öffnen Sie ...\\Learning Modules\\Business Continuity and Disaster Recovery\\RestoreTenant\\*Demo-RestoreTenant.ps1* in der *PowerShell ISE*, und legen Sie den folgenden Wert fest:
   * **$DemoScenario** = **1** – Festlegen auf **1**: Ereignisse ohne Ticketverkäufe löschen.
1. Drücken Sie **F5**, um das Skript auszuführen, und löschen Sie das letzte Ereignis. Es wird eine Bestätigungsmeldung ähnlich der folgenden angezeigt:

   ```Console
   Deleting unsold events from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

1. Die Contoso-Ereignisseseite wird geöffnet. Scrollen Sie nach unten, und überprüfen Sie, ob das Ereignis nicht mehr vorhanden ist. Wenn sich das Ereignis weiterhin in der Liste befindet , klicken Sie auf „Aktualisieren“, und stellen Sie sicher, dass es nicht mehr vorhanden ist.

   ![Letztes Ereignis](media/sql-database-saas-tutorial-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Wiederherstellen einer Mandantendatenbank parallel mit der Produktionsdatenbank

In dieser Übung wird die Datenbank der Contoso Concert Hall mit dem Status eines bestimmten Zeitpunkts vor dem Löschen des Ereignisses wiederhergestellt. Nachdem das Ereignis in den vorherigen Schritten gelöscht wurde, möchten Sie es wiederherstellen und die gelöschten Daten anzeigen. Sie müssen die Produktionsdatenbank mit dem gelöschten Datensatz nicht wiederherstellen, jedoch müssen Sie die alte Datenbank wiederherstellen, um aus geschäftlichen Gründen auf die alten Daten zuzugreifen.

 Das Skript *Restore-TenantInParallel.ps1* erstellt eine parallele Mandantendatenbank und einen parallelen Katalogeintrag – beide mit dem Namen *ContosoConcertHall\_old*. Dieses Wiederherstellungsmuster eignet sich am besten zur Wiederherstellung nach einem geringfügigen Datenverlust oder für Wiederherstellungsszenarien in Verbindung mit Konformität und Überwachung. Es ist auch die empfohlene Vorgehensweise bei Verwendung der [Georeplikation](sql-database-geo-replication-overview.md).

1. Führen Sie den Abschnitt [Simulieren des versehentlichen Löschens von Daten durch den Mandanten](#simulate-a-tenant-accidentally-deleting-data) aus.
1. Öffnen Sie ...\\Learning Modules\\Business Continuity and Disaster Recovery\\RestoreTenant\\_Demo-RestoreTenant.ps1_ in der *PowerShell ISE*.
1. Legen Sie für **$DemoScenario** = **2** fest – mit **2** legen Sie die *parallele Wiederherstellung des Mandanten* fest.
1. Drücken Sie **F5** , um das Skript auszuführen.

Das Skript stellt die Mandantendatenbank (in einer parallelen Datenbank) mit dem Status eines früheren Zeitpunkts wieder her, der vor dem Zeitpunkt liegt, zu dem Sie das Ereignis im vorherigen Abschnitt gelöscht haben. Es erstellt eine zweite Datenbank, entfernt ggf. vorhandene Katalogmetadaten aus dieser Datenbank, und fügt die Datenbank unter dem Eintrag *ContosoConcertHall\_old* dem Katalog hinzu.

Das Demoskript öffnet die Ereignisseseite in Ihrem Browser. Beachten Sie bei der URL: ```http://events.wtp.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` zeigt Daten aus der wiederhergestellten Datenbank an, wobei *_old* dem Namen angefügt ist.

Scrollen Sie durch die im Browser aufgelisteten Ereignisse, um sich davon zu überzeugen, dass das im vorherigen Abschnitt gelöschte Ereignis wiederhergestellt wurde.

Beachten Sie: Sie würden einem Mandanten wohl kaum Zugriff auf wiederhergestellte Daten bieten, indem Sie den wiederhergestellten Mandanten als zusätzlichen Mandanten mit eigener Events-App zum Suchen nach Tickets verfügbar machen, aber dies veranschaulicht ideal das Wiederherstellungsmuster.

In der Praxis würden Sie wahrscheinlich nur diese wiederhergestellte Datenbank für einen festgelegten Zeitraum beibehalten. Zum Abschluss können Sie den wiederhergestellten Mandanteneintrag durch Aufruf des Skripts *Remove-RestoredTenant.ps1* löschen.

1. Legen Sie **$DemoScenario** auf **4** fest, um das Szenario zum *Entfernen des wiederhergestellten Mandanten* auszuwählen.
1. **Starten Sie** **die Ausführung mit** **F5.**
1. Der Eintrag *ContosoConcertHall\_old* ist jetzt aus dem Katalog gelöscht. Fahren Sie fort, und schließen Sie die Ereignisseseite für diesen Mandanten in Ihrem Browser.


## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Direktes Wiederherstellen eines Mandanten, Ersetzen der vorhandenen Mandantendatenbank

In dieser Übung wird der Mandant Contoso Concert Hall mit dem Status eines bestimmten Zeitpunkts vor dem Löschen des Ereignisses wiederhergestellt. Das *Restore-TenantInPlace*-Skript stellt die aktuelle Mandantendatenbank in einer neuen Datenbank mit dem Status eines früheren Zeitpunkts wieder her und löscht die ursprüngliche Datenbank. Dieses Wiederherstellungsmuster eignet sich optimal für die Wiederherstellung bei schwerwiegender Datenbeschädigung, da der Mandant in diesem Fall möglicherweise mit bedeutendem Datenverlust rechnen müsste.

1. Öffnen Sie die Datei **Demo-RestoreTenant.ps1** in der PowerShell ISE.
1. Legen Sie **$DemoScenario** auf **5** fest, um das *Szenario zum direkten Wiederherstellen des Mandanten* auszuwählen.
1. Starten Sie die Ausführung mit **F5**.

Das Skript stellt die Mandantendatenbank mit dem Status wieder her, der fünf Minuten vor dem Löschen des Ereignisses bestand. Hierzu wird der Mandant Contoso Concert Hall offline geschaltet, sodass keine weiteren Datenupdates durchgeführt werden. Dann wird eine parallele Datenbank aus dem Wiederherstellungspunkt wiederhergestellt und mit einem Zeitstempel benannt, um sicherzustellen, dass der Datenbankname nicht mit dem Namen der vorhandenen Mandantendatenbank in Konflikt tritt. Als Nächstes wird die alte Mandantendatenbank gelöscht, und die wiederhergestellte Datenbank wird mit dem Namen der ursprünglichen umbenannt. Schließlich wird Contoso Concert Hall wieder online geschaltet, um den App-Zugriff auf die wiederhergestellte Datenbank zu ermöglichen.

Sie haben die Datenbank erfolgreich mit dem Status zu einem bestimmten Zeitpunkt vor dem Löschen des Ereignisses wiederhergestellt. Die Ereignisseseite wird geöffnet, sodass Sie sich von der Wiederherstellung des letzten Ereignisses überzeugen können.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]

> * Wiederherstellen einer Datenbank in eine parallele Datenbank (Seite-an-Seite)
> * Direktes Wiederherstellen einer Datenbank

[Manage schema for multiple tenants in the WTP SaaS application](sql-database-saas-tutorial-schema-management.md) (Verwalten des Schemas für mehrere Mandanten in der SaaS-Anwendung WTP)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Weitere Tutorials, die auf der ersten Wingtip Tickets Platform-Anwendungsbereitstellung (WTP) aufbauen](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md)
* [Weitere Informationen zu Sicherungen für SQL-Datenbank](sql-database-automated-backups.md)
