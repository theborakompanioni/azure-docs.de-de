---
title: "Azure CLI-Skript: Erstellen einer Azure-Datenbank für PostgreSQL-Instanz | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel: Erstellt einen Azure-Datenbank für PostgreSQL-Server und konfiguriert eine Firewallregel auf Serverebene."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: postgresql-database
ms.tgt_pltfrm: portal
ms.devlang: azurecli
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 0f61358aa655a44ac7982c80eb1d796667c47bf4
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>Erstellen eines Azure-Datenbank für PostgreSQL-Servers und Konfigurieren einer Firewallregel mit der Azure CLI
Dieses Azure CLI-Beispielskript erstellt einen Azure-Datenbank für PostgreSQL-Server und konfiguriert eine Firewallregel auf Serverebene. Nach erfolgreicher Ausführung des Skripts können alle Azure-Dienste und die konfigurierte IP-Adresse auf den PostgreSQL-Server zugreifen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript
```azurecli
#!/bin/bash

# Create a resource group
az group create \
--name myresourcegroup \
--location westus

# Create a PostgreSQL server in the resource group
# Name of a server maps to DNS name and is thus required to be globally unique in Azure.
# Substitute the <server_admin_password> with your own value.
az postgres server create \
--name mypgserver-20170401 \
--resource-group myresourcegroup \
--location westus \
--admin-user mylogin \
--admin-password <server_admin_password> \
--performance-tier Basic \
--compute-units 50 \

# Configure a firewall rule for the server
# The ip address range that you want to allow to access your server
az postgres server firewall-rule create \
--resource-group myresourcegroup \
--server mypgserver-20170401 \
--name AllowIps \
--start-ip-address 0.0.0.0 \
--end-ip-address 255.255.255.255

# Default database ‘postgres’ gets created on the server.
```

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.
```azurecli
az group delete --name myresourcegroup
```

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).
- Weitere CLI-Skriptbeispiele für Azure-Datenbank für PostgreSQL finden Sie in der [Dokumentation zu Azure-Datenbank für PostgreSQL](../sample-scripts-azure-cli.md).

