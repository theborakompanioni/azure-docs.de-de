---
title: "Azure CLI-Beispiele zum Erstellen eines einzelnen Azure-Datenbank für MySQL-Servers und Konfigurieren einer Firewallregel | Microsoft-Dokumentation"
description: "Dieses CLI-Beispielskript erstellt einen einzelnen Azure-Datenbank für MySQL-Server und konfiguriert eine Firewallregel auf Serverebene."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.tgt_pltfrm: portal
ms.devlang: azurecli
ms.topic: article
ms.custom: sample
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: fa961f123dddc2e6243d1efbd1d72b994179eb79
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="create-a-mysql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>Erstellen eines MySQL-Servers und Konfigurieren einer Firewallregel mit der Azure CLI
Dieses CLI-Beispielskript erstellt einen einzelnen Azure-Datenbank für MySQL-Server und konfiguriert eine Firewallregel auf Serverebene. Nach der erfolgreichen Ausführung des Skripts ist der MySQL-Server für alle Azure-Dienste und die konfigurierte IP-Adresse erreichbar.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript
```bash
#!/bin/bash

# Create a resource group
az group create \
--name myresource \
--location westus

# Create a MySQL server in the resource group
# Name of a server maps to DNS name and is thus required to be globally unique in Azure.
# Substitute the <server_admin_password> with your own value.
az mysql server create \
--name mysqlserver4demo \
--resource-group myresource \
--location westus \
--admin-user myadmin \
--admin-password <server_admin_password> \
--performance-tier Basic \
--compute-units 50 \

# Configure a firewall rule for the server
# The ip address range that you want to allow to access your server
az mysql server firewall-rule create \
--resource-group myresource \
--server mysqlserver4demo \
--name AllowIps \
--start-ip-address 0.0.0.0 \
--end-ip-address 255.255.255.255

# Default database ‘postgres’ gets created on the server.
```

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
 Verwenden Sie nach Ausführung des Skriptbeispiels den folgenden Befehl, um die Ressourcengruppe und alle damit verbundenen Ressourcen zu entfernen.
```azurecli
az group delete --name myresource
```
## <a name="script-explanation"></a>Erläuterung des Skripts
Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| **Befehl** | **Hinweise** |
|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [az group create](https://docs.microsoft.com/en-us/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| az mysql server create | Erstellt einen MySQL-Server, der die Datenbanken hostet. |
| az mysql server firewall create | Erstellt eine Firewallregel, die vom eingegebenen IP-Adressbereich aus den Zugriff auf alle Server und darauf befindlichen Datenbanken ermöglicht. |
| [az group delete](https://docs.microsoft.com/en-us/cli/azure/resource#delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
[Azure CLI-Beispiele für Azure-Datenbank für MySQL](../sample-scripts-azure-cli.md)

