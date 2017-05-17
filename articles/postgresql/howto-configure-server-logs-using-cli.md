---
title: "Konfigurieren der und Zugreifen auf die Serverprotokolle für PostgreSQL mithilfe der Azure CLI | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt, wie Sie die Serverprotokolle in Azure-Datenbank für PostgreSQL konfigurieren und darauf zugreifen."
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
ms.openlocfilehash: dc047e6d5194035586931b3cacc4d5cd07f6cfc2
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe der Azure CLI
Sie können Azure PostgreSQL-Serverfehlerprotokolle mithilfe der Befehlszeilenschnittstelle (Azure CLI) auflisten und herunterladen. Der Zugriff auf Transaktionsprotokolle wird jedoch nicht unterstützt. 

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Einen [Azure-Datenbank für PostgreSQL-Server](quickstart-create-server-database-azure-cli.md)
- Installiertes Befehlszeilenprogramm [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Konfigurieren der Protokollierung für Azure-Datenbank für PostgreSQL
Sie können den Server für den Zugriff auf Abfrage- und Fehlerprotokolle konfigurieren. Fehlerprotokolle können Informationen über „auto-vacuum“, Verbindungen und Prüfpunkte enthalten.
1. Aktivieren Sie die Protokollierung.
2. Aktualisieren Sie „log\_statement“ und „log\_min\_duration\_statement“, um die Abfrageprotokollierung zu aktivieren.
3. Aktualisieren Sie den Aufbewahrungszeitraum.

Weitere Informationen finden Sie unter [Anpassen der Serverkonfigurationsparameter](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-postgresql-server"></a>Auflisten von Protokollen für Azure PostgreSQL-Server
Um die verfügbaren Protokolldateien für Ihren Server aufzulisten, führen Sie den Befehl **az postgres server-logs** aus, wie im folgenden Beispiel gezeigt:
```azurecli
az postgres server-logs list --resource-group <resource group name> --server <server name> [ --file-last-written --filename-contains --max-file-size ]
```
Sie können z.B. die Protokolldateien für den Azure PostgreSQL-Server **mypgserver.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup** auflisten und an diese Textdatei leiten: **log\_files\_list.txt **.
```azurecli
az postgres server-logs list --resource-group **myresourcegroup** --server **mypgserver** > log\_files\_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Herunterladen von Protokollen vom Server auf den lokalen Computer
Sie können auch einzelne Protokolldateien für Ihren Azure PostgreSQL-Server herunterladen. 
```azurecli
az postgres server-logs download –name <log file name> --resource-group <resource group name> --server <server name>
```
Dieses Beispiel lädt die spezifische Protokolldatei für den Azure PostgreSQL-Server **mypgserver.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup** in Ihre lokale Umgebung herunter.
```azurecli
az postgres server-logs download --name 20170414-mypgserver-postgresql.log --resource-group **myresourcegroup** --server **mypgserver**
```
## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu Serverprotokollen finden Sie unter [Serverprotokolle in Azure-Datenbank für PostgreSQL](concepts-server-logs.md).
- Weitere Informationen zu Serverparametern finden Sie unter [Anpassen der Serverkonfigurationsparameter über die Azure CLI](howto-configure-server-parameters-using-cli.md).
