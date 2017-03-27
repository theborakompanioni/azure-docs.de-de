---
title: 'Azure CLI: Erstellen einer SQL-Datenbank | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie einen logischen SQL-Datenbank-Server, eine Firewallregel auf Serverebene und Datenbanken mit der Azure CLI erstellen.
keywords: Tutorial zu SQL-Datenbank, Erstellen einer SQL-Datenbank
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: quick start
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: cli
ms.topic: hero-article
ms.date: 03/13/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: f1e07d232328c35a43497c5a0ed6661a4277423d
ms.lasthandoff: 03/18/2017

---

# <a name="create-a-single-azure-sql-database-using-the-azure-cli"></a>Erstellen einer einzelnen Azure SQL-Datenbank mithilfe der Azure CLI.

Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In dieser Anleitung wird die Verwendung der Azure CLI zum Bereitstellen einer Azure SQL-Datenbank in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) auf einem [logischen SQL-Datenbankserver](sql-database-features.md) ausführlich beschrieben.

Bevor Sie beginnen, stellen Sie sicher, dass die Azure CLI installiert wurde. Weitere Informationen finden Sie im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli). 

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `westeurope` erstellt.

```azurecli
az group create --name myResourceGroup --location westeurope
```
## <a name="create-a-logical-server"></a>Erstellen eines logischen Servers

Erstellen Sie mit dem Befehl [az sql server create](/cli/azure/sql/server#create) einen [logischen Azure SQL-Datenbankserver](sql-database-features.md). Ein logischer Server enthält eine Gruppe von Datenbanken, die als Gruppe verwaltet werden. Im folgenden Beispiel wird in Ihrer Ressourcengruppe ein zufällig benannter Server mit einem Administrator namens `ServerAdmin` und dem Kennwort `ChangeYourAdminPassword1` erstellt. Ersetzen Sie ggf. diese vordefinierten Werte.

```azurecli
servername=server-$RANDOM
az sql server create --name $servername --resource-group myResourceGroup --location westeurope \
    --admin-user ServerAdmin --admin-password ChangeYourAdminPassword1
```

## <a name="configure-a-server-firewall-rule"></a>Konfigurieren einer Serverfirewallregel

Erstellen Sie mit dem Befehl [az sql server firewall create](/cli/azure/sql/server/firewall#create) eine [Azure SQL-Datenbank-Firewallregel auf Serverebene](sql-database-firewall-configure.md). Eine Firewallregel auf Serverebene lässt zu, dass eine externe Anwendung wie SQL Server Management Studio oder das SQLCMD-Hilfsprogramm über die Firewall des SQL-Datenbank-Dienstes eine Verbindung mit einer SQL-Datenbank herstellt. Im folgenden Beispiel wird eine Firewallregel für einen vordefinierten Adressbereich erstellt, der in diesem Beispiel den gesamten möglichen Bereich von IP-Adressen umfasst. Ersetzen Sie diese vordefinierten Werte durch die Werte für Ihre externe IP-Adresse oder den IP-Adressbereich. 

```azurecli
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="create-a-database-in-the-server"></a>Erstellen einer Datenbank auf dem Server

Erstellen Sie mit dem Befehl [az sql db create](/cli/azure/sql/db#create) auf dem Server eine Datenbank mit der [Leistungsstufe „S0“](sql-database-service-tiers.md). Im folgenden Beispiel wird eine leere Datenbank mit dem Namen `mySampleDatabase` erstellt. Ersetzen Sie ggf. diesen vordefinierten Wert.

```azurecli
az sql db create --resource-group myResourceGroup --server $servername \
    --name mySampleDatabase --service-objective S0
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Die **Verbinden mit**-Schnellstarts in dieser Sammlung und die Tutorials der Tutorialsammlung bauen auf diesem Schnellstart auf. Wenn Sie planen, mit den nachfolgenden Schnellstarts oder Tutorials fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie den folgenden Befehl ausführen, um alle erstellten Ressourcen dieses Schnellstarts zu löschen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Herstellen einer Verbindung und Durchführen von Abfragen mit SQL Server Management Studio finden Sie unter [Verbinden und Abfragen mit SSMS](sql-database-connect-query-ssms.md).
- Informationen zum Herstellen einer Verbindung mit Visual Studio finden Sie unter [Verbinden und Abfragen mit Visual Studio](sql-database-connect-query.md).
- Eine technische Übersicht über SQL-Datenbank finden Sie unter [Was ist SQL Datenbank? Einführung in SQL-Datenbank](sql-database-technical-overview.md).

