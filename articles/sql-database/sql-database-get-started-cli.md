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
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f68f90c3aea337d7b61b43e637bcfda3c98f3ea
ms.openlocfilehash: df42834bca821b16e9dd1bc57d735d346f12ca1d
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="create-a-single-azure-sql-database-using-the-azure-cli"></a>Erstellen einer einzelnen Azure SQL-Datenbank mithilfe der Azure CLI.

Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In dieser Anleitung wird die Verwendung der Azure CLI zum Bereitstellen einer Azure SQL-Datenbank in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) auf einem [logischen SQL-Datenbankserver](sql-database-features.md) ausführlich beschrieben.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="define-variables"></a>Definieren von Variablen

Definieren Sie Variablen zur Verwendung in den Skripts dieses Schnellstarts.

```azurecli-interactive
# The data center and resource name for your resources
export resourcegroupname = myResourceGroup
export location = westeurope
# The logical server name: Use a random value or replace with your own value (do not capitalize)
export servername = server-$RANDOM
# Set an admin login and password for your database
export adminlogin = ServerAdmin
export password = ChangeYourAdminPassword1
# The ip address range that you want to allow to access your DB
export startip = "0.0.0.0"
export endip = "0.0.0.0"
# The database name
export databasename = mySampleDatabase
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `westeurope` erstellt.

```azurecli-interactive
az group create --name $resourcegroupname --location $location
```
## <a name="create-a-logical-server"></a>Erstellen eines logischen Servers

Erstellen Sie mit dem Befehl [az sql server create](/cli/azure/sql/server#create) einen [logischen Azure SQL-Datenbankserver](sql-database-features.md). Ein logischer Server enthält eine Gruppe von Datenbanken, die als Gruppe verwaltet werden. Im folgenden Beispiel wird in Ihrer Ressourcengruppe ein zufällig benannter Server mit einem Administrator namens `ServerAdmin` und dem Kennwort `ChangeYourAdminPassword1` erstellt. Ersetzen Sie ggf. diese vordefinierten Werte.

```azurecli-interactive
az sql server create --name $servername --resource-group $resourcegroupname --location $location \
    --admin-user $adminlogin --admin-password $password
```

## <a name="configure-a-server-firewall-rule"></a>Konfigurieren einer Serverfirewallregel

Erstellen Sie mit dem Befehl [az sql server firewall create](/cli/azure/sql/server/firewall-rule#create) eine [Azure SQL-Datenbank-Firewallregel auf Serverebene](sql-database-firewall-configure.md). Eine Firewallregel auf Serverebene lässt zu, dass eine externe Anwendung wie SQL Server Management Studio oder das SQLCMD-Hilfsprogramm über die Firewall des SQL-Datenbank-Dienstes eine Verbindung mit einer SQL-Datenbank herstellt. Im folgenden Beispiel wird die Firewall nur für andere Azure-Ressourcen geöffnet. Ändern Sie die IP-Adresse in eine für Ihre Umgebung geeignete Adresse, um die externe Konnektivität zu ermöglichen. Verwenden Sie 0.0.0.0 als IP-Startadresse und 255.255.255.255 als Endadresse, wenn Sie alle IP-Adressen öffnen möchten.  

```azurecli-interactive
az sql server firewall-rule create --resource-group $resourcegroupname --server $servername \
    -n AllowYourIp --start-ip-address $startip --end-ip-address $endip
```

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet.
>

## <a name="create-a-database-in-the-server-with-sample-data"></a>Erstellen einer Datenbank auf dem Server mit Beispieldaten

Erstellen Sie mit dem Befehl [az sql db create](/cli/azure/sql/db#create) auf dem Server eine Datenbank mit der [Leistungsstufe „S0“](sql-database-service-tiers.md). Im folgenden Beispiel wird eine Datenbank mit dem Namen `mySampleDatabase` erstellt, und die AdventureWorksLT-Beispieldaten werden in diese Datenbank geladen. Ersetzen Sie diese vordefinierten Werte wie gewünscht (andere Schnellstarts in dieser Sammlung bauen auf den Werten dieses Schnellstarts auf).

```azurecli-interactive
az sql db create --resource-group $resourcegroupname --server $servername \
    --name $databasename --sample-name AdventureWorksLT --service-objective S0
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstarts dieser Sammlung bauen auf diesem Schnellstart auf. 

> [!TIP]
> Wenn Sie planen, mit den nachfolgenden Schnellstarts fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.
>

```azurecli-interactive
az group delete --name $resourcegroupname
```

## <a name="next-steps"></a>Nächste Schritte

Sie besitzen nun eine Datenbank, können eine Verbindung herstellen und Abfragen mit Ihren bevorzugten Tools ausführen. Weitere Informationen erhalten Sie, indem Sie eines der folgenden Tools auswählen:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)


