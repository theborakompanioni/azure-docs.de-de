---
title: "Schnellstart: Erstellen einer Azure-Datenbank für MySQL-Server – Azure CLI | Microsoft-Dokumentation"
description: "Dieser Schnellstart beschreibt die Verwendung der Azure CLI zum Erstellen einer Azure-Datenbank für MySQL-Server in einer Azure-Ressourcengruppe."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: hero-article
ms.date: 06/13/2017
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f68f90c3aea337d7b61b43e637bcfda3c98f3ea
ms.openlocfilehash: 04fc441aee7a4c8adc4f02d5e51b2d9e64400f55
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a>Erstellen einer Azure-Datenbank für MySQL-Server mithilfe der Azure CLI
Dieser Schnellstart beschreibt die Verwendung der Azure CLI zum Erstellen einer Azure-Datenbank für MySQL-Server in einer Azure-Ressourcengruppe in etwa fünf Minuten. Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

Wenn Sie über mehrere Abonnements verfügen, wählen Sie das entsprechende Abonnement aus, in dem die Ressource vorhanden ist oder in Rechnung gestellt wird. Wählen Sie eine bestimmte Abonnement-ID unter Ihrem Konto mit dem Befehl [az account set](/cli/azure/account#set) aus.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie mit dem Befehl [az group create](https://docs.microsoft.com/cli/azure/group#create) eine [Azure-Ressourcengruppe](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myresourcegroup` am Standort `westus` erstellt.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Erstellen einer Azure-Datenbank für MySQL-Server
Erstellen Sie mit dem Befehl **az mysql server create** einen Azure-Datenbank für MySQL-Server. Ein Server kann mehrere Datenbanken verwalten. In der Regel wird für jedes Projekt oder jeden Benutzer eine separate Datenbank verwendet.

Das folgende Beispiel erstellt eine Azure-Datenbank für MySQL-Server in `westus` in der Ressourcengruppe `myresourcegroup` mit dem Namen `myserver4demo`. Der Server verfügt über ein Administratorprotokoll mit dem Namen `myadmin` und dem Kennwort `Password01!`. Der Server wird mit der Leistungsebene **Basic** und **50** Compute-Einheiten erstellt, die von allen Datenbanken auf dem Server gemeinsam genutzt werden. Sie können Computeleistung und Speicher gemäß den Anwendungsanforderungen nach oben oder unten skalieren.

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name myserver4demo --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Konfigurieren der Firewallregel
Erstellen Sie mit dem Befehl **az mysql server firewall-rule create** eine Firewallregel für die Azure-Datenbank für MySQL auf Serverebene. Eine Firewallregel auf Serverebene ermöglicht einer externen Anwendung wie z.B. dem Befehlszeilentool **mysql.exe** oder MySQL Workbench, über die Firewall des Azure-MySQL-Diensts eine Verbindung zu Ihrem Server herzustellen. 

Im folgenden Beispiel wird eine Firewallregel für einen vordefinierten Adressbereich erstellt, der in diesem Beispiel den gesamten möglichen Bereich von IP-Adressen umfasst.

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server myserver4demo --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a>Konfigurieren der SSL-Einstellungen
Standardmäßig werden SSL-Verbindungen zwischen Ihrem Server und Clientanwendungen erzwungen.  Dadurch wird die Sicherheit von Daten während des Betriebs gewährleistet, indem der Datenstrom über das Internet verschlüsselt wird.  Um diesen Schnellstart zu vereinfachen, deaktivieren wir die SSL-Verbindungen für Ihren Server.  Dies wird für Produktionsserver jedoch nicht empfohlen.  Weitere Informationen finden Sie unter [Konfigurieren von SSL-Verbindungen in der Anwendung für eine sichere Verbindung mit Azure-Datenbank für MySQL](./howto-configure-ssl.md).

Im folgenden Beispiel wird das Erzwingen von SSL auf Ihrem MySQL-Server deaktiviert.
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name myserver4demo -g -n --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>Abrufen der Verbindungsinformationen

Zum Herstellen einer Verbindung zum Server müssen Sie Hostinformationen und Anmeldeinformationen für den Zugriff angeben.

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name myserver4demo
```

Das Ergebnis liegt im JSON-Format vor. Notieren Sie sich **fullyQualifiedDomainName** und **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "myserver4demo.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/servers/myserver4demo",
  "location": "westus",
  "name": "myserver4demo",
  "resourceGroup": "myresourcegroup",
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
Herstellen einer Serververbindung mit dem Befehlszeilentool **mysql.exe** Sie können MySQL [hier](https://dev.mysql.com/downloads/) herunterladen und auf Ihrem Computer installieren. Stattdessen können Sie auch in den Codebeispielen auf die Schaltfläche **Ausprobieren** oder im Azure-Portal auf der Symbolleiste ganz oben rechts auf die Schaltfläche `>_` klicken und **Azure Cloud Shell** öffnen.

Geben Sie die nächsten Befehle ein: 

1. Stellen Sie eine Verbindung zum Server mit dem Befehlszeilentool **mysql** her:
```azurecli-interactive
 mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. Zeigen Sie den Serverstatus an:
```sql
 mysql> status
```
Wenn alles funktioniert, sollte das Befehlszeilentool den folgenden Text ausgeben:

```dos
C:\Users\>mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
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
Connection:             myserver4demo.mysql.database.azure.com via TCP/IP
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
> Zusätzliche Befehle finden Sie unter [MySQL 5.7-Referenzhandbuch – Kapitel 4.5.1 (in englischer Sprache)](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Herstellen einer Verbindung zum Server mit dem MySQL Workbench-GUI-Tool
1.  Starten Sie die Anwendung MySQL Workbench auf dem Clientcomputer. Sie können MySQL Workbench [hier](https://dev.mysql.com/downloads/workbench/) herunterladen und installieren.

2.  Geben Sie im Dialogfeld **Setup New Connection** (Neue Verbindung einrichten) auf der Registerkarte **Parameter** folgende Informationen ein:

   ![Einrichten einer neuen Verbindung](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| **Einstellung** | **Empfohlener Wert** | **Beschreibung** |
|---|---|---|
|   Verbindungsname | Eigene Verbindung | Geben Sie eine beliebige Bezeichnung für diese Verbindung ein. |
| Verbindungsmethode | Wählen Sie „Standard (TCP/IP)“ aus | Verwenden Sie das TCP/IP-Protokoll zum Herstellen einer Verbindung mit Azure-Datenbank für MySQL> |
| Hostname | myserver4demo.mysql.database.azure.com | Der zuvor notierte Servername. |
| Port | 3306 | Der Standardport für MySQL wird verwendet. |
| Benutzername | myadmin@myserver4demo | Die zuvor notierte Serveradministratoranmeldung. |
| Kennwort | **** | Verwenden Sie das Administratorkontokennwort, das Sie zuvor konfiguriert haben. |

Klicken Sie auf **Verbindung testen**, um zu testen, ob alle Parameter richtig konfiguriert wurden.
Nun können Sie auf die Verbindung klicken, um eine Verbindung mit dem Server herzustellen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie diese Ressourcen nicht für einen anderen Schnellstart bzw. ein anderes Tutorial benötigen, können Sie sie mit dem folgenden Befehl löschen: 

```azurecli-interactive
az group delete --name myresourcegroup
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Entwerfen einer MySQL-Datenbank mit der Azure CLI](./tutorial-design-database-using-cli.md)

