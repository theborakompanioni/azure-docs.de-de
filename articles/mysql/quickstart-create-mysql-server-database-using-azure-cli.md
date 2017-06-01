---
title: "Schnellstart: Erstellen einer Azure-Datenbank für MySQL-Server – Azure CLI | Microsoft-Dokumentation"
description: "Dieser Schnellstart beschreibt die Verwendung der Azure CLI zum Erstellen einer Azure-Datenbank für MySQL-Server in einer Azure-Ressourcengruppe."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: portal
ms.workload: 
ms.date: 05/24/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: 9f78163e4ff1166a2abd94150d686256ee338286
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a>Erstellen einer Azure-Datenbank für MySQL-Server mithilfe der Azure CLI
Dieser Schnellstart beschreibt die Verwendung der Azure CLI zum Erstellen einer Azure-Datenbank für MySQL-Server in einer Azure-Ressourcengruppe in etwa fünf Minuten. Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts.

Für dieses Schnellstarttutorial muss die aktuelle Version von [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) installiert sein. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure
Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```azurecli
az login
```
Befolgen Sie die Anweisungen an der Eingabeaufforderung, um „https://aka.ms/devicelog“ in Ihrem Browser zu öffnen, und geben Sie dann den generierten Code in der **Eingabeaufforderung** ein.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie mit dem Befehl [az group create](https://docs.microsoft.com/cli/azure/group#create) eine [Azure-Ressourcengruppe](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `mycliresource` am Standort `westus` erstellt.

```azurecli
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Erstellen einer Azure-Datenbank für MySQL-Server
Erstellen Sie mit dem Befehl **az mysql server create** einen Azure-Datenbank für MySQL-Server. Ein Server kann mehrere Datenbanken verwalten. In der Regel wird für jedes Projekt oder jeden Benutzer eine separate Datenbank verwendet.

Das folgende Beispiel erstellt eine Azure-Datenbank für MySQL-Server in `westus` in der Ressourcengruppe `mycliresource` mit dem Namen `mycliserver`. Der Server verfügt über ein Administratorprotokoll mit dem Namen `myadmin` und dem Kennwort `Password01!`. Der Server wird mit der Leistungsebene **Basic** und **50** Compute-Einheiten erstellt, die von allen Datenbanken auf dem Server gemeinsam genutzt werden. Sie können Computeleistung und Speicher gemäß den Anwendungsanforderungen nach oben oder unten skalieren.

```azurecli
az mysql server create --resource-group mycliresource --name mycliserver --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

![Erstellen einer Azure-Datenbank für MySQL-Server mithilfe der Azure CLI](./media/quickstart-create-mysql-server-database-using-azure-cli/3_az-mysq-server-create.png)

## <a name="configure-firewall-rule"></a>Konfigurieren der Firewallregel
Erstellen Sie mit dem Befehl **az mysql server firewall-rule create** eine Firewallregel für die Azure-Datenbank für MySQL auf Serverebene. Eine Firewallregel auf Serverebene ermöglicht einer externen Anwendung wie z.B. dem Befehlszeilentool **mysql.exe** oder MySQL Workbench, über die Firewall des Azure-MySQL-Diensts eine Verbindung zu Ihrem Server herzustellen. 

Im folgenden Beispiel wird eine Firewallregel für einen vordefinierten Adressbereich erstellt, der in diesem Beispiel den gesamten möglichen Bereich von IP-Adressen umfasst.

```azurecli
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a>Konfigurieren der SSL-Einstellungen
Standardmäßig werden SSL-Verbindungen zwischen Ihrem Server und Clientanwendungen erzwungen.  Dadurch wird die Sicherheit von Daten während des Betriebs gewährleistet, indem der Datenstrom über das Internet verschlüsselt wird.  Um diesen Schnellstart zu vereinfachen, deaktivieren wir die SSL-Verbindungen für Ihren Server.  Dies wird für Produktionsserver jedoch nicht empfohlen.  Weitere Informationen finden Sie unter [Konfigurieren von SSL-Verbindungen in der Anwendung für eine sichere Verbindung mit Azure-Datenbank für MySQL](./howto-configure-ssl.md).

Im folgenden Beispiel wird das Erzwingen von SSL auf Ihrem MySQL-Server deaktiviert.
 
 ```azurecli
 az mysql server update --resource-group mycliresource --name mycliserver -g -n --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>Abrufen der Verbindungsinformationen

Zum Herstellen einer Verbindung zum Server müssen Sie Hostinformationen und Anmeldeinformationen für den Zugriff angeben.

```azurecli
az mysql server show --resource-group mycliresource --name mycliserver
```

Das Ergebnis liegt im JSON-Format vor. Notieren Sie sich **fullyQualifiedDomainName** und **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mycliserver.mysql.database.azure.com",
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

## <a name="connect-to-the-server-using-the-mysqlexe-command-line-tool"></a>Herstellen einer Verbindung zum Server mit dem Befehlszeilentool „mysql.exe“
Stellen Sie beim Herstellen einer Verbindung zu Ihrem Server mit dem Befehlszeilentool **mysql.exe** sicher, dass MySQL auf Ihrem Computer installiert ist.  Sie können MySQL [hier](https://dev.mysql.com/downloads/) herunterladen.

Öffnen Sie die Eingabeaufforderung und geben Sie Folgendes ein: 

1. Stellen Sie eine Verbindung zum Server mit dem Befehlszeilentool **mysql** her:
```dos
 mysql -h mycliserver.mysql.database.azure.com -u myadmin@mycliserver -p
```

2. Zeigen Sie den Serverstatus an:
```dos
 mysql> status
```
Wenn alles funktioniert, sollte das Befehlszeilentool Folgendes ausgeben:

```dos
C:\Users\v-chenyh>mysql -h mycliserver.mysql.database.azure.com -u myadmin@mycliserver -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.35, for Win64 (x86_64)

Connection id:          65512
Current database:
Current user:           myadmin@116.230.243.143
SSL:                    Not in use
Using delimiter:        ;
Server version:         5.6.26.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             mycliserver.mysql.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    gbk
Conn.  characterset:    gbk
TCP port:               3306
Uptime:                 2 days 9 hours 47 min 20 sec

Threads: 4  Questions: 34833  Slow queries: 2  Opens: 84  Flush tables: 4  Open tables: 1  Queries per second avg: 0.167
--------------

mysql>
```

> [!TIP]
> Zusätzliche Befehle finden Sie unter [MySQL 5.6-Referenzhandbuch – Kapitel 4.5.1 (in englischer Sprache)](https://dev.mysql.com/doc/refman/5.6/en/mysql.html).

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Herstellen einer Verbindung zum Server mit dem MySQL Workbench-GUI-Tool
1.    Starten Sie die Anwendung MySQL Workbench auf dem Clientcomputer. Sie können MySQL Workbench [hier](https://dev.mysql.com/downloads/workbench/) herunterladen und installieren.

2.    Geben Sie im Dialogfeld **Setup New Connection** (Neue Verbindung einrichten) auf der Registerkarte **Parameter** folgende Informationen ein:

| **Parameter** | **Beschreibung** |
|----------------|-----------------|
|    *Verbindungsname* | Geben Sie einen beliebigen Namen für diese Verbindung ein |
| *Verbindungsmethode* | Wählen Sie „Standard (TCP/IP)“ aus |
| *Hostname* | mycliserver.mysql.database.azure.com (der zuvor notierte Servername) |
| *Port* | 3306 |
| *Benutzername* | myadmin@mycliserver (die zuvor notierte Serveradministratoranmeldung) |
| *Kennwort* | Speichern Sie das Kennwort für das Administratorkonto. |

   ![Einrichten einer neuen Verbindung](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

Klicken Sie auf **Verbindung testen**, um zu testen, ob alle Parameter richtig konfiguriert wurden.
Nun können Sie auf die Verbindung klicken, die gerade erstellt wurde, um eine Verbindung zum Server herzustellen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht für einen anderen Schnellstart bzw. ein anderes Tutorial benötigen, können Sie sie wie folgt löschen: 

```azurecli
az group delete --name mycliresource
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Entwerfen einer MySQL-Datenbank mit der Azure CLI](./tutorial-design-database-using-cli.md)

