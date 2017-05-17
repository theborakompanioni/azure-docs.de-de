---
title: "Konfigurieren der Dienstparameter in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt, wie Sie die Dienstparameter in Azure-Datenbank für PostgreSQL konfigurieren."
services: postgresql
author: SaloniSonpal
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
ms.openlocfilehash: 7e46e3d588782ec0f2ef89c58001e997b8fc2910
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a>Anpassen der Serverkonfigurationsparameter mithilfe der Azure CLI
Sie können Konfigurationsparameter für einen Azure PostgreSQL-Server mithilfe der Befehlszeilenschnittstelle (Azure CLI) auflisten, anzeigen und aktualisieren. Auf Serverebene ist jedoch nur eine Teilmenge der Modulkonfigurationen verfügbar und kann geändert werden. 

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Einen Server und eine Datenbank, siehe [Erstellen einer Azure-Datenbank für PostgreSQL-Instanz](quickstart-create-server-database-azure-cli.md)
- Installiertes Befehlszeilenprogramm [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

## <a name="list-server-configuration-parameters-for-azure-postgresql-server"></a>Auflisten der Serverkonfigurationsparameter für einen Azure PostgreSQL-Server
Um eine Liste aller änderbaren Parameter eines Azure PostgreSQL-Servers und der zugehörigen Werte abzurufen, führen Sie den folgenden **az postgres server configuration**-Befehl wie hier gezeigt aus:
> az postgres server configuration list --resource-group <resource group name> --server <server name>

Sie können z.B. die Serverkonfigurationsparameter für den Azure PostgreSQL-Server **mypgserver.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup** auflisten.
```azurecli
az postgres server configuration list --resource-group myresourcegroup --server mypgserver
```
## <a name="show-server-configuration-parameter-details"></a>Anzeigen von Details zu Serverkonfigurationsparametern
Um Details zu einem bestimmten Konfigurationsparameter für einen Azure PostgreSQL-Server anzuzeigen, führen Sie den Befehl **az postgres server configuration** aus.
```azurecli
az postgres server configuration show --name <configuration name>
   --resource-group <resource group name> --server <server name>
```
Sie können z.B. Details des Serverkonfigurationsparameters **log\_min\_messages** für den Azure PostgreSQL-Server **mypgserver.postgres.database.azure.com** für diese Ressourcengruppe auflisten: **myresourcegroup **.
```azurecli
az postgres server configuration show --name log\_min\_messages --resource-group myresourcegroup --server mypgserver
```
## <a name="modify-server-configuration-parameter-value"></a>Ändern des Werts von Serverkonfigurationsparametern
Sie können den Wert von bestimmten Konfigurationsparametern eines Azure PostgreSQL-Servers ändern und dadurch den zugrunde liegenden Konfigurationswert für das PostgreSQL-Servermodul aktualisieren. Um den Konfigurationswert zu aktualisieren, führen Sie den folgenden **az postgres server configuration**-Befehl aus. 
```azurecli
az postgres server configuration update --name <configuration name>
   --resource-group <resource group name> --server <server name>[--value]
```
Sie können z.B. den Serverkonfigurationsparameter **log\_min\_messages** für den Azure PostgreSQL-Server **mypgserver.postgres.database.azure.com** für diese Ressourcengruppe aktualisieren: **myresourcegroup **.
```azurecli
az postgres server configuration show --name log\_min\_messages 
   --resource-group myresourcegroup --server mypgserver --value INFO
```
Wenn Sie den Wert eines Konfigurationsparameters zurücksetzen möchten, lassen Sie einfach den optionalen Parameter „--value“ weg. Dann wendet der Dienst den Standardwert an. Im obigen Beispiel könnte dies so aussehen:
```azurecli
az postgres server configuration show --name log\_min\_messages
   --resource-group myresourcegroup --server mypgserver
```
Dadurch wird die Konfiguration „log\_min\_messages“ auf den Standardwert „WARNING“ zurückgesetzt. Weitere Informationen zur Serverkonfiguration und zulässigen Werten finden Sie in der PostgreSQL-Dokumentation zur [Serverkonfiguration](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Nächste Schritte
- Informationen dazu, wie Sie Serverprotokolle konfigurieren und darauf zugreifen, finden Sie unter [Serverprotokolle in Azure-Datenbank für PostgreSQL](concepts-server-logs.md).
