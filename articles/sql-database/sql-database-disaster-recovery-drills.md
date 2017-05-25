---
title: "Notfallwiederherstellungsverfahren für SQL-Datenbank | Microsoft Docs"
description: "Hier erhalten Sie Anleitungen und bewährte Methoden für die Verwendung von Azure SQL-Datenbank zum Ausführen von Notfallwiederherstellungsverfahren, mit denen Sie wichtige Geschäftsanwendungen vor Fehlern und Ausfällen schützen."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: b44a269c-fe2a-404f-b013-290030860bd1
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/31/2016
ms.author: sashan
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 1b1d65a41a794a566287dcffe3581ac58e2a965f
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="performing-disaster-recovery-drill"></a>Ausführen von Notfallwiederherstellungsverfahren
Es wird empfohlen, in regelmäßigen Abständen Überprüfungen der Anwendungsbereitschaft für den Wiederherstellungsworkflow durchzuführen. Überprüfungen des Anwendungsverhaltens und der Auswirkungen von Datenverlusten und/oder Unterbrechungen durch Failover gehören zu einem angemessenen Entwicklungsverfahren. Solche Überprüfungen werden auch für die meisten Branchenstandards im Zuge von Zertifizierungen zur Geschäftskontinuität vorausgesetzt.

Notfallwiederherstellungsverfahren umfassen Folgendes:

* Simulieren von Ausfällen auf Datenebene
* Wiederherstellen
* Überprüfen der Anwendungsintegrität nach der Wiederherstellung

Abhängig vom [Entwurf Ihrer Anwendung für Geschäftskontinuität](sql-database-business-continuity.md)kann der verbundene Workflow variieren. Im Folgenden beschreiben wir bewährte Methoden zur Ausführung von Notfallwiederherstellungen im Kontext von Azure SQL-Datenbank.

## <a name="geo-restore"></a>Geografische Wiederherstellung
Um Datenverluste beim Durchführen von Notfallwiederherstellungsverfahren zu verhindern, empfehlen wir die Verwendung einer Testumgebung, indem Sie eine Kopie der Produktionsumgebung erstellen und diese zum Überprüfen des Failoverworkflows der Anwendung verwenden.

#### <a name="outage-simulation"></a>Simulieren von Ausfällen
Um die Ausfallzeit zu simulieren, können Sie die Quelldatenbank löschen oder umbenennen. Dadurch wird ein Verbindungsfehler für die Anwendung verursacht.

#### <a name="recovery"></a>Wiederherstellen
* Führen Sie die Geowiederherstellung der Datenbank auf einem anderen Server aus, wie [hier](sql-database-disaster-recovery.md)beschrieben.
* Ändern Sie die Anwendungskonfiguration zum Herstellen einer Verbindung mit der wiederhergestellten Datenbank, und folgen Sie der Anleitung unter [Konfigurieren einer Datenbank nach der Wiederherstellung](sql-database-disaster-recovery.md) zum Abschließen der Wiederherstellung.

#### <a name="validation"></a>Überprüfen
* Schließen Sie das Verfahren ab, indem Sie die Anwendungsintegrität nach der Wiederherstellung sicherstellen (einschließlich der Verbindungszeichenfolgen, Anmeldungen, grundlegenden Funktionstests oder sonstigen Überprüfungen, die Teil der Standardabmeldeverfahren von Anwendungen sind).

## <a name="geo-replication"></a>Georeplikation
Für eine durch Georeplikation geschützte Datenbank beinhaltet das Verfahren ein geplantes Failover auf die sekundäre Datenbank. Durch das geplante Failover wird sichergestellt, dass die primäre und die sekundäre Datenbank beim Wechseln der Rollen synchronisiert bleiben. Im Gegensatz zum ungeplanten Failover führt dieser Vorgang nicht zu Datenverlust, daher kann er auch in einer Produktionsumgebung ausgeführt werden.

#### <a name="outage-simulation"></a>Simulieren von Ausfällen
Um die Ausfallzeit zu simulieren, können Sie die Web-Anwendung oder den virtuellen Computer deaktivieren, die bzw. der mit der Datenbank verbunden ist. Dies führt zu Verbindungsfehlern für die Webclients.

#### <a name="recovery"></a>Wiederherstellen
* Stellen Sie sicher, dass die Anwendungskonfiguration in der Notfallwiederherstellungsregion auf die vorherige sekundäre Datenbank verweist. Diese Datenbank wird zur neuen primären Datenbank, auf die vollständig zugegriffen werden kann.
* Ausführen eines [geplanten Failovers](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) , um die sekundäre Datenbank als neue primäre Datenbank festzulegen
* Führen Sie die Anweisungen unter [Konfigurieren einer Datenbank nach der Wiederherstellung](sql-database-disaster-recovery.md) aus, um die Wiederherstellung abzuschließen.

#### <a name="validation"></a>Überprüfen
* Schließen Sie das Verfahren ab, indem Sie die Anwendungsintegrität nach der Wiederherstellung sicherstellen (einschließlich der Verbindungszeichenfolgen, Anmeldungen, grundlegenden Funktionstests oder sonstigen Überprüfungen, die Teil der Standardabmeldeverfahren von Anwendungen sind).

## <a name="next-steps"></a>Nächste Schritte
* Informationen über Szenarien für die Geschäftskontinuität finden Sie unter [Szenarien für die Geschäftskontinuität](sql-database-business-continuity.md)
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md)
* Informationen zu schnelleren Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md)  

