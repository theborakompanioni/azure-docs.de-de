---
title: "Sichern und Wiederherstellen eines Servers in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie einen Server in Azure-Datenbank für PostgreSQL mit Azure CLI sichern und wiederherstellen."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 4cd77c4ae4d9487aad11ea790c5d88a4eaff6077
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017

---

# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-postgresql-by-using-the-azure-cli"></a>Sichern und Wiederherstellen eines Servers in Azure-Datenbank für PostgreSQL mit Azure CLI

Verwenden Sie Azure-Datenbank für PostgreSQL, um eine Serverdatenbank auf ein 7 bis 35 Tage früheres Datum wiederherzustellen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:
- [Einen Server und eine Datenbank für Azure-Datenbank für PostgreSQL](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Wenn Sie Azure CLI lokal installieren und verwenden möchten, müssen Sie für diesen Leitfaden die Azure CLI-Version 2.0 oder höher ausführen. Geben Sie zum Bestätigen der Version an der Eingabeaufforderung von Azure CLI `az --version` ein. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="back-up-happens-automatically"></a>Automatische Sicherung
Bei der Verwendung von Azure-Datenbank für PostgreSQL erstellt der Datenbankdienst automatisch alle fünf Minuten eine Sicherung des Diensts. 

Für den Basic-Tarif sind die Sicherungen 7 Tage lang verfügbar. Für den Standard-Tarif sind die Sicherungen 35 Tage lang verfügbar. Weitere Informationen finden Sie unter [Optionen und Leistung von Azure-Datenbank für PostgreSQL: Überblick über die verfügbaren Funktionen in den einzelnen Tarifen](concepts-service-tiers.md).

Mithilfe dieses automatischen Sicherungsfeatures können Sie einen Zustand des Servers und seiner Datenbanken zu einem früheren Datum oder Zeitpunkt wiederherstellen.

## <a name="restore-a-database-to-a-previous-point-in-time-by-using-the-azure-cli"></a>Wiederherstellen des Zustands einer Datenbank zu einem früheren Zeitpunkt über Azure CLI
Stellen Sie den Zustand des Servers zu einem früheren Zeitpunkt mit Azure-Datenbank für PostgreSQL wieder her. Die wiederhergestellten Daten werden auf einen neuen Server kopiert, und der vorhandene Server bleibt unverändert. Wenn zum Beispiel zur Mittagszeit am heutigen Tag eine Tabelle versehentlich gelöscht wird, können Sie den Zustand kurz vor Mittag wiederherstellen. Dann können Sie die fehlende Tabelle und die fehlenden Daten aus der wiederhergestellten Kopie des Servers abrufen. 

Verwenden Sie zur Wiederherstellung des Servers den Azure CLI-Befehl [az postgres server restore](/cli/azure/postgres/server#restore).

### <a name="run-the-restore-command"></a>Ausführen des Befehls „restore“

Geben Sie zum Wiederherstellen des Servers an der Azure CLI-Eingabeaufforderung den folgenden Befehl ein:

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

Für den Befehl `az postgres server restore` sind folgende Parameter erforderlich:
| Einstellung | Empfohlener Wert | Beschreibung  |
| --- | --- | --- |
| resource-group |  myResourceGroup |  Die Ressourcengruppe, in der sich der Quellserver befindet.  |
| name | mypgserver-restored | Der Name des neuen Servers, der durch den Befehl „restore“ erstellt wird. |
| restore-point-in-time | 2017-04-13T13:59:00Z | Wählen Sie einen Zeitpunkt aus, dessen Zustand wiederhergestellt werden soll. Dieses Datum und diese Urzeit müssen innerhalb des Aufbewahrungszeitraums für Sicherungen des Quellservers liegen. Verwenden Sie das Datums- und Zeitformat nach ISO 8601. Beispielsweise können Sie Ihre eigene lokale Zeitzone wie `2017-04-13T05:59:00-08:00` verwenden. Sie können z.B. auch das UTC-Zulu-Format verwenden, `2017-04-13T13:59:00Z`. |
| source-server | mypgserver-20170401 | Der Name oder die ID des Quellservers, über den die Wiederherstellung durchgeführt wird. |

Wenn Sie den Zustand eines Servers zu einem früheren Zeitpunkt wiederherstellen, wird ein neuer Server erstellt. Der ursprüngliche Server und seine Datenbanken aus dem angegebenen Zeitpunkt werden auf den neuen Server kopiert.

Die Werte zum Standort und Tarif des wiederhergestellten Servers bleiben mit denen des ursprünglichen Servers identisch. 

Der `az postgres server restore`-Befehl ist synchron. Nachdem der Server wiederhergestellt ist, können Sie ihn erneut verwenden, um den Prozess für einen anderen Zeitpunkt zu wiederholen. 

Suchen Sie nach Abschluss der Wiederherstellung den neuen Server, um zu überprüfen, ob die Daten wie erwartet wiederhergestellt wurden.

## <a name="next-steps"></a>Nächste Schritte
[Datenverbindungsbibliotheken für die Azure-Datenbank für PostgreSQL](concepts-connection-libraries.md)

