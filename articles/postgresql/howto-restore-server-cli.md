---
title: "Wiederherstellen eines Servers in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie mit der Azure CLI-Befehlszeile einen Server in Azure-Datenbank für PostgreSQL sichern und wiederherstellen."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f68f90c3aea337d7b61b43e637bcfda3c98f3ea
ms.openlocfilehash: fa63ccedb455e4972bfdd0e15069ad5e932b0fa3
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-cli"></a>Sichern und Wiederherstellen eines Servers in Azure-Datenbank für PostgreSQL mit Azure CLI

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- [Einen Server und eine Datenbank für Azure-Datenbank für PostgreSQL](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="backup-happens-automatically"></a>Automatische Sicherung
Bei der Verwendung von Azure-Datenbank für PostgreSQL erstellt der Datenbankdienst automatisch alle fünf Minuten eine Sicherung des Diensts. 

Die Sicherungen sind im Basic-Tarif sieben Tage und im Standard-Tarif 35 Tage lang verfügbar. Weitere Informationen finden Sie im Artikel zu den [Tarifen für Azure-Datenbank für PostgreSQL](concepts-service-tiers.md).

Mithilfe dieses automatischen Sicherungsfeatures können Sie einen früheren Zustand des Servers und aller seiner Datenbanken wiederherstellen.

## <a name="restore-a-database-to-a-previous-point-in-time-using-the-azure-cli"></a>Wiederherstellen des Zustands einer Datenbank zu einem früheren Zeitpunkt über Azure CLI
Mit Azure-Datenbank für PostgreSQL können Sie den Zustand des Servers zu einem früheren Zeitpunkt wiederherstellen. Die wiederhergestellten Daten werden auf einen neuen Server kopiert, und der vorhandene Server bleibt unverändert. Wenn zum Beispiel zur Mittagszeit am heutigen Tag eine Tabelle versehentlich gelöscht wurde, könnten Sie den Zustand kurz vor Mittag wiederherstellen. Rufen Sie dann die fehlende Tabelle und die fehlenden Daten aus der wiederhergestellten Kopie des Servers ab. 

Verwenden Sie den Azure CLI-Befehl [az postgres server restore](/cli/azure/postgres/server#restore) für die Wiederherstellung.

### <a name="run-the-restore-command"></a>Ausführen des Befehls „restore“
```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

Für den Befehl `az postgres server restore` sind folgende Parameter erforderlich:
| Einstellung | Empfohlener Wert | Beschreibung  |
| --- | --- | --- |
| resource-group |  myResourceGroup |  Die Ressourcengruppe, in der sich der Quellserver befindet.  |
| Name | mypgserver-restored | Der Name des neuen Servers, der durch den Befehl „restore“ erstellt wird. |
| restore-point-in-time | 2017-04-13T13:59:00Z | Wählen Sie einen Zeitpunkt für die Wiederherstellung aus. Datum und Zeit müssen innerhalb des Aufbewahrungszeitraums für Sicherungen des Quellservers liegen. Verwenden Sie das Datums- und Zeitformat nach ISO 8601. Beispielsweise können Sie Ihre eigene lokale Zeitzone wie etwa `2017-04-13T05:59:00-08:00` oder das UTC-Format Zulu `2017-04-13T13:59:00Z` verwenden. |
| source-server | mypgserver-20170401 | Der Name oder die ID des Quellservers, über den die Wiederherstellung durchgeführt wird. |

Bei der Wiederherstellung eines Servers zu einem früheren Zeitpunkt wird ein neuer Server erstellt. Dabei wird der ursprüngliche Server zu dem von Ihnen festgelegten Zeitpunkt kopiert. Die Werte zum Standort und Tarif des wiederhergestellten Servers sind mit denen des Quellservers identisch. Der Befehl ist synchron und gibt nach der Wiederherstellung des Servers Daten zurück. 

Sobald die Wiederherstellung abgeschlossen ist, suchen Sie nach dem neuen Server, der erstellt wurde. Stellen Sie sicher, dass die Daten wie erwartet wiederhergestellt wurden.

## <a name="next-steps"></a>Nächste Schritte
[Datenverbindungsbibliotheken für die Azure-Datenbank für PostgreSQL](concepts-connection-libraries.md)

