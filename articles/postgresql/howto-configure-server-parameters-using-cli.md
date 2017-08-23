---
title: "Konfigurieren der Dienstparameter in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie mit der Azure CLI-Befehlszeile die Dienstparameter in Azure-Datenbank für PostgreSQL konfigurieren."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 9a575148a05843bef7524eff61407b377292ca3b
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017

---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a>Anpassen der Serverkonfigurationsparameter mithilfe der Azure CLI
Sie können Konfigurationsparameter für einen Azure PostgreSQL-Server mithilfe der Befehlszeilenschnittstelle (Azure CLI) auflisten, anzeigen und aktualisieren. Auf Serverebene ist jedoch nur eine Teilmenge der Modulkonfigurationen verfügbar und kann geändert werden. 

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Einen Server und eine Datenbank, siehe [Erstellen einer Azure-Datenbank für PostgreSQL-Instanz](quickstart-create-server-database-azure-cli.md)
- Installieren Sie das [Azure CLI 2.0](/cli/azure/install-azure-cli)-Befehlszeilenprogramm, oder verwenden Sie Azure Cloud Shell im Browser.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>Auflisten der Serverkonfigurationsparameter für Server für Azure-Datenbank für PostgreSQL
Führen Sie den Befehl [az postgres server configuration list](/cli/azure/postgres/server/configuration#list) aus, um alle änderbaren Parameter eines Servers mit ihren Werten aufzulisten.

Sie können die Serverkonfigurationsparameter für den Server **mypgserver-20170401.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup** auflisten.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="show-server-configuration-parameter-details"></a>Anzeigen von Details zu Serverkonfigurationsparametern
Um Details zu einem bestimmten Konfigurationsparameter für einen Server anzuzeigen, führen Sie den Befehl [az postgres server configuration show](/cli/azure/postgres/server/configuration#show) aus.

Dieses Beispiel zeigt Details des Serverkonfigurationsparameters **log\_min\_messages** für den Server **mypgserver-20170401.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup.**
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="modify-server-configuration-parameter-value"></a>Ändern des Werts von Serverkonfigurationsparametern
Sie können den Wert eines bestimmten Serverkonfigurationsparameters ändern und dadurch den zugrunde liegenden Konfigurationswert für das PostgreSQL-Servermodul aktualisieren. Um die Konfiguration zu aktualisieren, führen Sie den Befehl [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) aus. 

Aktualisieren Sie den Serverkonfigurationsparameter **log\_min\_messages** des Servers **mypgserver-20170401.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup**.
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401 --value INFO
```
Wenn Sie den Wert eines Konfigurationsparameters zurücksetzen möchten, lassen Sie einfach den optionalen Parameter `--value` weg. Dann wendet der Dienst den Standardwert an. Im obigen Beispiel könnte dies so aussehen:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
Dadurch wird die Konfiguration **log\_min\_messages** auf den Standardwert **WARNING** zurückgesetzt. Weitere Informationen zur Serverkonfiguration und zulässigen Werten finden Sie in der PostgreSQL-Dokumentation zur [Serverkonfiguration](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Nächste Schritte
- Informationen dazu, wie Sie Serverprotokolle konfigurieren und darauf zugreifen, finden Sie unter [Serverprotokolle in Azure-Datenbank für PostgreSQL](concepts-server-logs.md).

