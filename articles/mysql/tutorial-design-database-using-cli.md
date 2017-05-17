---
title: "Entwerfen Ihrer ersten Azure-Datenbank für MySQL mithilfe von Azure CLI | Microsoft-Dokumentation"
description: "In diesem Tutorial wird das Erstellen und Verwalten von Azure-Datenbank für MySQL-Server und der Datenbank mithilfe von Azure CLI 2.0 erläutert."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: portal
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 99238b9c0bcc7fa80e8c72cda25f7ed809cc5195
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="design-your-first-azure-database-for-mysql-database"></a>Entwerfen Ihrer ersten Azure-Datenbank für MySQL

Azure-Datenbank für MySQL ist ein relationaler Datenbankdienst in der Microsoft Cloud, der auf dem MySQL Community Edition-Datenbankmodul basiert. In diesem Tutorial verwenden Sie die Azure CLI (Befehlszeilenschnittstelle) und andere Hilfsprogramme, um zu lernen, wie Sie Folgendes ausführen:

> [!div class="checklist"]
> * Erstellen einer Azure-Datenbank für MySQL
> * Konfigurieren der Serverfirewall Verwenden des [mysql-Befehlszeilentools](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) zum Erstellen einer Datenbank
> * Laden von Beispieldaten
> * Abfragen von Daten
> * Aktualisieren von Daten
> * Wiederherstellen von Daten

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```azurecli
az login
```
Befolgen Sie die Anweisungen an der Eingabeaufforderung, um die URL „https://aka.ms/devicelog“ in Ihrem Browser zu öffnen, und geben Sie dann den generierten Code in der **Eingabeaufforderung** ein.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie mit dem Befehl [az group create](https://docs.microsoft.com/cli/azure/group#create) eine [Azure-Ressourcengruppe](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `mycliresource` am Standort `westus` erstellt.

```azurecli
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Erstellen einer Azure-Datenbank für MySQL-Server
Erstellen Sie mit dem Befehl „az mysql server create“ einen Azure-Datenbank für MySQL-Server. Ein Server kann mehrere Datenbanken verwalten. In der Regel wird für jedes Projekt oder jeden Benutzer eine separate Datenbank verwendet.

Das folgende Beispiel erstellt einen Azure-Datenbank für MySQL-Server, der sich in `westus` in der Ressourcengruppe `mycliresource` mit dem Namen `mycliserver` befindet. Der Server hat ein Administratorprotokoll mit dem Namen `myadmin` und dem Kennwort `Password01!`. Der Server wird mit der Leistungsebene **Basic** und **50** Compute-Einheiten erstellt, die von allen Datenbanken auf dem Server gemeinsam genutzt werden. Sie können Computeleistung und Speicher gemäß den Anwendungsanforderungen nach oben oder unten skalieren.

```azurecli
az mysql server create --resource-group mycliresource --name mycliserver
--location westus --user myadmin --password Password01!
--performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Konfigurieren der Firewallregel
Erstellen Sie mit dem Befehl „az mysql server firewall-rule create“ eine Firewallregel auf Azure-Datenbank für MySQL-Serverebene. Eine Firewallregel auf Serverebene ermöglicht einer externen Anwendung wie z.B. dem **mysql**-Befehlszeilentool oder MySQL Workbench, über die Firewall des Azure-MySQL-Diensts eine Verbindung mit Ihrem Server herzustellen. 

Das folgende Beispiel erstellt eine Firewallregel für einen vordefinierten Adressbereich. Dieses Beispiel zeigt den gesamten möglichen Bereich von IP-Adressen.

```azurecli
az mysql server firewall-rule create --resource-group mycliresource
--server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0
--end-ip-address 255.255.255.255
```

## <a name="get-the-connection-information"></a>Abrufen der Verbindungsinformationen

Zum Herstellen einer Verbindung mit dem Server müssen Sie Hostinformationen und Anmeldeinformationen für den Zugriff angeben.
```azurecli
az mysql server show --resource-group mycliresource --name mycliserver
```

Das Ergebnis liegt im JSON-Format vor. Notieren Sie sich die Werte für **fullyQualifiedDomainName** und **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mycliserver.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mycliresource/providers/Microsoft.DBforMySQL/servers/mycliserver",
  "location": "westus",
  "name": "mycliserver",
  "resourceGroup": "mycliresource",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "MYSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

## <a name="connect-to-the-server-using-mysql"></a>Herstellen einer Verbindung mit dem Server unter Verwendung von mysql
Verwenden Sie das [mysql-Befehlszeilentool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) zum Herstellen einer Verbindung mit Ihrem Azure-Datenbank für MySQL-Server. In diesem Beispiel lautet der Befehl:
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>Leere Datenbank erstellen
Sobald Sie mit dem Server verbunden sind, erstellen Sie eine leere Datenbank.
```sql
mysql> CREATE DATABASE mysampledb;
```

Führen Sie an der Eingabeaufforderung den folgenden Befehl zum Wechseln der Verbindung zu dieser neu erstellten Datenbank aus:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Erstellen von Tabellen in der Datenbank
Da Sie nun wissen, wie Sie eine Verbindung mit Azure-Datenbank für MySQL herstellen, können wir die Ausführung einiger grundlegender Aufgaben erörtern.

Zunächst können wir eine Tabelle erstellen und mit einigen Daten füllen. Wir erstellen eine Tabelle, die Bestandsinformationen speichert.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Laden von Daten in die Tabellen
Jetzt können wir einige Daten in die Tabelle einfügen. Führen Sie im geöffneten Eingabeaufforderungsfenster die folgende Abfrage zum Einfügen einiger Datenzeilen aus.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Die Tabelle, die Sie zuvor erstellt haben, enthält nun zwei Zeilen mit Beispieldaten.

## <a name="query-and-update-the-data-in-the-tables"></a>Abfragen und Aktualisieren der Daten in den Tabellen
Führen Sie die folgende Abfrage aus, um Informationen aus der Datenbanktabelle abzurufen.
```sql
SELECT * FROM inventory;
```

Sie können auch die Daten in den Tabellen aktualisieren.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Die Zeile wird entsprechend aktualisiert, wenn Sie Daten abrufen.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Wiederherstellen eines früheren Zustands einer Datenbank
Stellen Sie sich vor, Sie haben versehentlich diese Tabelle gelöscht. Dies ist eine Situation, in der Sie die Datenbank nicht einfach wiederherstellen können. Azure-Datenbank für MySQL ermöglicht Ihnen, zu einem beliebigen Zeitpunkt innerhalb der letzten 35 Tage zurückzugehen und diesen Zeitpunkt in einem neuen Server wiederherzustellen. Sie können diesen neuen Server zur Wiederherstellung gelöschter Daten verwenden. Mithilfe der folgenden Schritte wird der Status des Beispielservers zu einem Zeitpunkt wiederhergestellt, der vor dem Hinzufügen der Tabelle liegt.

Zum Wiederherstellen benötigen Sie die folgenden Informationen:

- Wiederherstellungspunkt: Wählen Sie einen Zeitpunkt vor der Änderung des Servers aus. Er darf nicht weiter zurückliegen als das älteste Sicherungsdatum der Quelldatenbank.
- Zielserver: Geben Sie einen neuen Servernamen für die Wiederherstellung ein.
- Quellserver: Geben Sie den Namen des Servers ein, von dem aus wiederhergestellt werden soll.
- Speicherort: Sie können nicht die Region wählen. Standardmäßig ist dieser Wert mit dem Quellserver identisch.
- Tarif: Sie können diesen Wert beim Wiederherstellen eines Servers nicht ändern. Er ist mit dem Wert für den Quellserver identisch. 

```azurecli
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

Mit diesem Verfahren werden der Server und der Status des Servers [für einen Zeitpunkt wiederhergestellt](./howto-restore-server-portal.md), der vor dem Löschen der Tabellen liegt. Beim Wiederherstellen eines Servers im Zustand eines anderen Zeitpunkts wird ein Duplikat des ursprünglichen Servers im Zustand des von Ihnen angegebenen Zeitpunkts als neuer Server erstellt, vorausgesetzt, dass dieser Zeitpunkt innerhalb der für Ihre [Dienstebene](./concepts-service-tiers.md) geltenden Beibehaltungsdauer liegt.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:
> [!div class="checklist"]
> * Erstellen einer Azure-Datenbank für MySQL
> * Konfigurieren der Serverfirewall Verwenden des [mysql-Befehlszeilentools](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) zum Erstellen einer Datenbank
> * Laden von Beispieldaten
> * Abfragen von Daten
> * Aktualisieren von Daten
> * Wiederherstellen von Daten

[Azure-Datenbank für MySQL – Azure-CLI-Beispiele](./sample-scripts-azure-cli.md)

