---
title: "Azure Cosmos DB-Automatisierung – Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Verwenden Sie die Azure CLI 2.0 zum Erstellen und Verwalten von Azure Cosmos DB-Konten. Azure Cosmos DB ist eine hochgradig verfügbare, global verteilte Datenbank."
services: cosmosdb
author: dmakwana
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: 6158c27f-6b9a-404e-a234-b5d48c4a5b29
ms.custom: quick start create
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 04/20/2017
ms.author: dimakwan
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 6150914e623cb6c54f1257c772be62150ab81e17
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="create-an-azure-cosmos-db-account-using-the-azure-cli"></a>Erstellen eines Azure Cosmos DB-Kontos mithilfe der Azure CLI

Der folgende Leitfaden beschreibt Befehle, mit denen Sie die Verwaltung Ihrer Azure Cosmos DB-Datenbankkonten mithilfe der Vorschaubefehle in Azure CLI 2.0 automatisieren können. Er enthält auch Befehle zum Verwalten von Kontoschlüsseln und Failoverprioritäten in [Datenbankkonten in mehreren Regionen][scaling-globally]. Wenn Sie Ihr Datenbankkonto aktualisieren, können Sie Konsistenzrichtlinien ändern und Regionen hinzufügen bzw. entfernen. Zur plattformübergreifenden Verwaltung Ihres Azure Cosmos DB-Datenbankkontos können Sie [Azure PowerShell](documentdb-manage-account-with-powershell.md), die [Ressourcenanbieter-REST-API][rp-rest-api] oder das [Azure-Portal](documentdb-create-account.md) verwenden.

## <a name="getting-started"></a>Erste Schritte

Befolgen Sie die Anweisungen unter [Installieren und Konfigurieren von Azure CLI 2.0][install-az-cli2], um Ihre Entwicklungsumgebung mit der Azure CLI 2.0 einzurichten.

Melden Sie sich bei Ihrem Azure-Konto an, indem Sie den folgenden Befehl ausführen und den auf dem Bildschirm gezeigten Schritten folgen.

```azurecli
az login
```

Erstellen Sie eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#resource-groups), wenn noch keine vorhanden ist:

```azurecli
az group create --name <resourcegroupname> --location <resourcegrouplocation>
az group list
```

`<resourcegrouplocation>` muss einer der Regionen entsprechen, in denen Azure Cosmos DB allgemein verfügbar ist. Die aktuelle Liste der Regionen finden Sie auf der Seite [Azure Regionen](https://azure.microsoft.com/regions/#services).

### <a name="notes"></a>Hinweise

* Führen Sie „az documentdb -h“ aus, um eine vollständige Liste der verfügbaren Befehle zu erhalten, oder besuchen Sie die [Referenzseite][az-documentdb-ref].
* Führen Sie „az documentdb &lt;Befehl&gt; -h“ aus, um eine Liste mit Details zu den erforderlichen und optionalen Parametern für jeden Befehl zu erhalten.

## <a name="register-your-subscription-to-use-azure-cosmos-db"></a>Registrieren Ihres Abonnements zur Verwendung von Azure Cosmos DB

Mit diesem Befehl wird Ihr Abonnement zur Verwendung von Azure Cosmos DB über die CLI registriert.

```azurecli
az provider register -n Microsoft.DocumentDB 
```

## <a id="create-documentdb-account-cli"></a> Erstellen eines Azure Cosmos DB-Datenbankkontos

Mit diesem Befehl können Sie ein Azure Cosmos DB-Datenbankkonto erstellen. Konfigurieren Sie das neue Datenbankkonto entweder mit einer einzelnen Region oder mit [mehreren Regionen][scaling-globally] mit einer bestimmten [Konsistenzrichtlinie](documentdb-consistency-levels.md).

```
Arguments
    --name -n           [Required]: Name of the Azure Cosmos DB database account. The account 
                                    name must be unique.
    --resource-group -g [Required]: Name of the resource group.
    --default-consistency-level   : Default consistency level of the Azure Cosmos DB database account.
                                    Allowed values: BoundedStaleness, Eventual, Session, Strong.
    --ip-range-filter             : Firewall support. Specifies the set of IP addresses or IP
                                    address ranges in CIDR form to be included as the allowed list
                                    of client IPs for a given database account. IP addresses/ranges
                                    must be comma separated and must not contain any spaces.
    --kind                        : The type of Azure Cosmos DB database account to create.  Allowed
                                    values: GlobalDocumentDB, MongoDB, Parse.  Default:
                                    GlobalDocumentDB.
    --locations                   : Space separated locations in 'regionName=failoverPriority'
                                    format. E.g "East US"=0 "West US"=1. Failover priority values
                                    are 0 for write regions and greater than 0 for read regions. A
                                    failover priority value must be unique and less than the total
                                    number of regions. Default: single region account in the
                                    location of the specified resource group.
    --max-interval                : When used with Bounded Staleness consistency, this value
                                    represents the time amount of staleness (in seconds) tolerated.
                                    Accepted range for this value is 1 - 100.  Default: 5.
    --max-staleness-prefix        : When used with Bounded Staleness consistency, this value
                                    represents the number of stale requests tolerated. Accepted
                                    range for this value is 1 - 2,147,483,647.  Default: 100.
```

```azurecli
az documentdb create -g <resourcegroupname> -n <uniquedocumentdbaccountname> --kind <typeofdatabaseaccount>
```

Beispiele: 

    az documentdb create -g rg-test -n docdb-test
    az documentdb create -g rg-test -n docdb-test --kind MongoDB
    az documentdb create -g rg-test -n docdb-test --locations "East US"=0 "West US"=1 "South Central US"=2
    az documentdb create -g rg-test -n docdb-test --ip-range-filter "13.91.6.132,13.91.6.1/24"
    az documentdb create -g rg-test -n docdb-test --locations "East US"=0 "West US"=1 --default-consistency-level BoundedStaleness --max-interval 10 --max-staleness-prefix 200

### <a name="notes"></a>Hinweise 
* Die Standorte müssen Regionen entsprechen, in denen Azure Cosmos DB allgemein verfügbar ist. Die aktuelle Liste der Regionen finden Sie auf der Seite [Azure Regionen](https://azure.microsoft.com/regions/#services).
* Um den Portalzugriff zu aktivieren, schließen Sie die IP-Adresse für das Azure-Portal für Ihre Region in den IP-Bereichsfilter ein; befolgen Sie dazu die Anleitungen in [Konfigurieren der IP-Access Control-Richtlinie](documentdb-firewall-support.md#configure-ip-policy).

## <a id="update-documentdb-account-cli"></a> Aktualisieren eines Azure Cosmos DB-Datenbankkontos

Mit diesem Befehl können Sie die Eigenschaften Ihres Azure Cosmos DB-Datenbankkontos aktualisieren. Hierzu gehören z.B. die Konsistenzrichtlinie und die Standorte, in denen das Datenbankkonto vorhanden ist.

> [!NOTE]
> Mit diesem Befehl können Sie Regionen hinzufügen und entfernen, aber keine Failoverprioritäten ändern. Informationen zum Ändern von Failovereigenschaften finden Sie weiter [unten](#modify-failover-priority-powershell).

```
Arguments
    --name -n           [Required]: Name of the Azure Cosmos DB database account.
    --resource-group -g [Required]: Name of the resource group.
    --default-consistency-level   : Default consistency level of the Azure Cosmos DB database account.
                                    Allowed values: BoundedStaleness, Eventual, Session, Strong.
    --ip-range-filter             : Firewall support. Specifies the set of IP addresses or IP address
                                    ranges in CIDR form to be included as the allowed list of client
                                    IPs for a given database account. IP addresses/ranges must be comma
                                    separated and must not contain any spaces.
    --locations                   : Space separated locations in 'regionName=failoverPriority' format.
                                    E.g "East US"=0. Failover priority values are 0 for write regions
                                    and greater than 0 for read regions. A failover priority value must
                                    be unique and less than the total number of regions.
    --max-interval                : When used with Bounded Staleness consistency, this value represents
                                    the time amount of staleness (in seconds) tolerated. Accepted range
                                    for this value is 1 - 100.
    --max-staleness-prefix        : When used with Bounded Staleness consistency, this value represents
                                    the number of stale requests tolerated. Accepted range for this
                                    value is 1 - 2,147,483,647.
```

Beispiele: 

    az documentdb update -g rg-test -n docdb-test --locations "East US"=0 "West US"=1 "South Central US"=2
    az documentdb update -g rg-test -n docdb-test --ip-range-filter "13.91.6.132,13.91.6.1/24"
    az documentdb update -g rg-test -n docdb-test --default-consistency-level BoundedStaleness --max-interval 10 --max-staleness-prefix 200

## <a id="add-remove-region-documentdb-account-cli"></a> Hinzufügen einer Region zu einem bzw. Entfernen einer Region aus einem Azure Cosmos DB-Datenbankkonto

Um eine oder mehrere Regionen zu Ihrem vorhandenen Azure Cosmos DB-Datenbankkonto hinzuzufügen oder daraus zu entfernen, verwenden Sie den Befehl [update](#update-documentdb-account-cli) mit dem Flag `--locations`. Das Beispiel unten zeigt, wie Sie ein neues Konto erstellen und danach Regionen zu diesem Konto hinzufügen oder daraus entfernen.

Beispiel:

    az documentdb create -g rg-test -n docdb-test --locations "East US"=0 "West US"=1
    az documentdb update -g rg-test -n docdb-test --locations "East US"=0 "North Europe"=1 "South Central US"=2


## <a id="delete-documentdb-account-cli"></a> Löschen eines Azure Cosmos DB-Datenbankkontos

Mit diesem Befehl können Sie ein Azure Cosmos DB-Datenbankkonto löschen.

```
Arguments
    --name -n           [Required]: Name of the Azure Cosmos DB database account.
    --resource-group -g [Required]: Name of the resource group.
```

Beispiel:

    az documentdb delete -g rg-test -n docdb-test

## <a id="get-documentdb-properties-cli"></a> Abrufen der Eigenschaften eines Azure Cosmos DB-Datenbankkontos

Mit diesem Befehl können Sie die Eigenschaften für ein vorhandenes Azure Cosmos DB-Datenbankkonto abrufen.

```
Arguments
    --name -n           [Required]: Name of the Azure Cosmos DB database account.
    --resource-group -g [Required]: Name of the resource group.
```

Beispiel:

    az documentdb show -g rg-test -n docdb-test

## <a id="list-account-keys-cli"></a> Auflisten von Kontoschlüsseln

Wenn Sie ein Azure Cosmos DB-Konto erstellen, generiert der Dienst zwei Hauptzugriffsschlüssel, die für die Authentifizierung verwendet werden können, wenn der Zugriff auf das Azure Cosmos DB-Konto erfolgt. Durch Bereitstellen von zwei Zugriffsschlüsseln ermöglicht Azure Cosmos DB Ihnen das erneute Generieren der Schlüssel ohne Unterbrechung des Zugriffs auf das Azure Cosmos DB-Konto. Schreibgeschützte Schlüssel für die Authentifizierung von schreibgeschützten Vorgängen sind ebenfalls verfügbar. Es gibt zwei Lese-Schreib-Schlüssel (primär und sekundär) und zwei schreibgeschützte Schlüssel (primär und sekundär).

```
Arguments
    --name -n           [Required]: Name of the Azure Cosmos DB database account.
    --resource-group -g [Required]: Name of the resource group.
```

Beispiel:

    az documentdb list-keys -g rg-test -n docdb-test

## <a id="list-connection-strings-cli"></a>: Verbindungszeichenfolgen auflisten

Für MongoDB-Konten kann die Verbindungszeichenfolge zum Herstellen einer Verbindung Ihrer MongoDB-App mit dem Datenbankkonto mit dem folgenden Befehl abgerufen werden.

```
Arguments
    --name -n           [Required]: Name of the Azure Cosmos DB database account.
    --resource-group -g [Required]: Name of the resource group.
```

Beispiel:

    az documentdb list-connection-strings -g rg-test -n docdb-test

## <a id="regenerate-account-key-cli"></a> Erneutes Generieren eines Kontoschlüssels

Sie sollten regelmäßig die Zugriffsschlüssel für Ihr Azure Cosmos DB-Konto ändern, um dafür zu sorgen, dass Ihre Verbindungen möglichst sicher sind. Zwei Zugriffsschlüssel werden zugewiesen, damit Sie Verbindungen zum Azure Cosmos DB-Konto mit einem Zugriffsschlüssel aufrechterhalten können, während Sie den anderen Zugriffsschlüssel neu generieren.

```
Arguments
    --name -n           [Required]: Name of the Azure Cosmos DB database account.
    --resource-group -g [Required]: Name of the resource group.
    --key-kind          [Required]: The access key to regenerate.  Allowed values: primary, primaryReadonly,
                                    secondary, secondaryReadonly.
```

Beispiel:

    az documentdb regenerate-key -g rg-test -n docdb-test --key-kind secondary

## <a id="modify-failover-priority-cli"></a> Ändern der Failoverpriorität eines Azure Cosmos DB-Datenbankkontos

Bei Datenbankkonten mit mehreren Regionen können Sie die Failoverpriorität der verschiedenen Regionen ändern, in denen das Azure Cosmos DB-Datenbankkonto vorhanden ist. Weitere Informationen zum Failover in Ihrem Azure Cosmos DB-Datenbankkonto finden Sie unter [Globale Verteilung von Daten mit Azure Cosmos DB](documentdb-distribute-data-globally.md).

```
Arguments
    --name -n           [Required]: Name of the Azure Cosmos DB database account.
    --resource-group -g [Required]: Name of the resource group.
    --failover-policies [Required]: Space separated failover policies in 'regionName=failoverPriority' format.
                                    E.g "East US"=0 "West US"=1.
```

Beispiel:

    az documentdb failover-priority-change "East US"=1 "West US"=0 "South Central US"=2

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Herstellen einer Verbindung mit .NET finden Sie unter [Verbinden und Abfragen mit .NET](../cosmos-db/create-documentdb-dotnet.md).
* Informationen zum Herstellen einer Verbindung mit .NET Core finden Sie unter [Verbinden und Abfragen mit .NET Core](../cosmos-db/create-documentdb-dotnet-core.md).
* Informationen zum Herstellen einer Verbindung mit Node.js finden Sie unter [Verbinden und Abfragen mit Node.js und einer MongoDB-App](../cosmos-db/create-mongodb-nodejs.md).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[scaling-globally]: https://azure.microsoft.com/documentation/articles/documentdb-distribute-data-globally/#scaling-across-the-planet
[install-az-cli2]: https://docs.microsoft.com/cli/azure/install-az-cli2
[az-documentdb-ref]: https://docs.microsoft.com/cli/azure/documentdb
[az-documentdb-create-ref]: https://docs.microsoft.com/cli/azure/documentdb#create
[rp-rest-api]: https://docs.microsoft.com/rest/api/documentdbresourceprovider/

