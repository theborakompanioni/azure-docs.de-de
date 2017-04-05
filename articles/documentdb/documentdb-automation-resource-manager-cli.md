---
title: "DocumentDB-Automatisierung – Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Verwenden Sie Azure CLI 2.0. um DocumentDB-Datenbankkonten zu verwalten. DocumentDB ist eine cloudbasierte NoSQL-Datenbank für JSON-Daten."
services: documentdb
author: dmakwana
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: 6158c27f-6b9a-404e-a234-b5d48c4a5b29
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: dimakwan
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: b286a93d7cc5f962f969e877b2f487e56cbb1a95
ms.lasthandoff: 03/30/2017


---
# <a name="automate-azure-documentdb-account-management-using-azure-cli-20"></a>Automatisieren der Azure DocumentDB-Kontoverwaltung mit Azure CLI 2.0
> [!div class="op_single_selector"]
> * [Azure-Portal](documentdb-create-account.md)
> * [Azure CLI 1.0](documentdb-automation-resource-manager-cli-nodejs.md)
> * [Azure CLI 2.0](documentdb-automation-resource-manager-cli.md)
> * [Azure PowerShell](documentdb-manage-account-with-powershell.md)

Der folgende Leitfaden beschreibt Befehle, mit denen Sie die Verwaltung Ihrer DocumentDB-Datenbankkonten mithilfe der DocumentDB-Vorschaubefehle in Azure CLI 2.0 automatisieren können. Er enthält auch Befehle zum Verwalten von Kontoschlüsseln und Failoverprioritäten in [Datenbankkonten in mehreren Regionen][scaling-globally]. Wenn Sie Ihr Datenbankkonto aktualisieren, können Sie Konsistenzrichtlinien ändern und Regionen hinzufügen bzw. entfernen. Zur plattformübergreifenden Verwaltung Ihres DocumentDB-Datenbankkontos können Sie [Azure PowerShell](documentdb-manage-account-with-powershell.md), die [Ressourcenanbieter-REST-API][rp-rest-api] oder das [Azure-Portal](documentdb-create-account.md) verwenden.

## <a name="getting-started"></a>Erste Schritte

Befolgen Sie die Anweisungen unter [Installieren und Konfigurieren von Azure CLI 2.0][install-az-cli2], um Ihre Entwicklungsumgebung mit der Azure CLI 2.0 einzurichten.

Melden Sie sich bei Ihrem Azure-Konto an, indem Sie den folgenden Befehl ausführen und den auf dem Bildschirm gezeigten Schritten folgen.

    az login

Erstellen Sie eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#resource-groups), wenn noch keine vorhanden ist:

    az group create --name <resourcegroupname> --location <resourcegrouplocation>
    az group list

`<resourcegrouplocation>` muss einer der Regionen entsprechen, in denen DocumentDB allgemein verfügbar ist. Die aktuelle Liste der Regionen finden Sie auf der Seite [Azure Regionen](https://azure.microsoft.com/regions/#services).

### <a name="notes"></a>Hinweise

* Führen Sie „az documentdb -h“ aus, um eine vollständige Liste der verfügbaren Befehle zu erhalten, oder besuchen Sie die [Referenzseite][az-documentdb-ref].
* Führen Sie „az documentdb <command> -h“ aus, um eine Liste mit Details zu den erforderlichen und optionalen Parametern für jeden Befehl zu erhalten.

## <a id="create-documentdb-account-cli"></a> Erstellen eines DocumentDB-Datenbankkontos

Mit diesem Befehl können Sie ein DocumentDB-Datenbankkonto erstellen. Konfigurieren Sie das neue Datenbankkonto entweder mit einer einzelnen Region oder mit [mehreren Regionen][scaling-globally] mit einer bestimmten [Konsistenzrichtlinie](documentdb-consistency-levels.md). 

```
Arguments
    --name -n           [Required]: Name of the DocumentDB database account.
    --resource-group -g [Required]: Name of the resource group.
    --default-consistency-level   : Default consistency level of the DocumentDB database account.
                                    Allowed values: BoundedStaleness, Eventual, Session, Strong.
    --ip-range-filter             : Firewall support. Specifies the set of IP addresses or IP
                                    address ranges in CIDR form to be included as the allowed list
                                    of client IPs for a given database account. IP addresses/ranges
                                    must be comma separated and must not contain any spaces.
                                    To enable portal access, include 104.42.195.92.
    --kind                        : The type of DocumentDB database account to create.  Allowed
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

Beispiele: 

    az documentdb create -g rg-test -n docdb-test
    az documentdb create -g rg-test -n docdb-test --kind MongoDB
    az documentdb create -g rg-test -n docdb-test --locations "East US"=0 "West US"=1 "South Central US"=2
    az documentdb create -g rg-test -n docdb-test --ip-range-filter "13.91.6.132,13.91.6.1/24"
    az documentdb create -g rg-test -n docdb-test --locations "East US"=0 "West US"=1 --default-consistency-level BoundedStaleness --max-interval 10 --max-staleness-prefix 200

### <a name="notes"></a>Hinweise
* Bei den Standorten muss es sich um Regionen handeln, in denen DocumentDB allgemein verfügbar ist. Die aktuelle Liste der Regionen finden Sie auf der Seite [Azure Regionen](https://azure.microsoft.com/regions/#services).

## <a id="update-documentdb-account-cli"></a> Aktualisieren eines DocumentDB-Datenbankkontos

Mit diesem Befehl können Sie die Eigenschaften Ihres DocumentDB-Datenbankkontos aktualisieren. Hierzu gehören z.B. die Konsistenzrichtlinie und die Standorte, in denen das Datenbankkonto vorhanden ist.

> [!NOTE]
> Mit diesem Befehl können Sie Regionen hinzufügen und entfernen, aber keine Failoverprioritäten ändern. Informationen zum Ändern von Failovereigenschaften finden Sie weiter [unten](#modify-failover-priority-powershell).

```
Arguments
    --name -n           [Required]: Name of the DocumentDB database account.
    --resource-group -g [Required]: Name of the resource group.
    --default-consistency-level   : Default consistency level of the DocumentDB database account.
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

## <a id="add-remove-region-documentdb-account-cli"></a>Hinzufügen/Entfernen einer Region zu bzw. aus einem DocumentDB-Datenbankkonto

Um eine oder mehrere Regionen zu Ihrem vorhandenen DocumentDB-Datenbankkonto hinzuzufügen oder daraus zu entfernen, verwenden Sie den Befehl [update](#update-documentdb-account-cli) mit dem Flag `--locations`. Das Beispiel unten zeigt, wie Sie ein neues Konto erstellen und danach Regionen zu diesem Konto hinzufügen oder daraus entfernen.

Beispiel:

    az documentdb create -g rg-test -n docdb-test --locations "East US"=0 "West US"=1
    az documentdb update -g rg-test -n docdb-test --locations "East US"=0 "North Europe"=1 "South Central US"=2


## <a id="delete-documentdb-account-cli"></a> Löschen eines DocumentDB-Datenbankkontos

Mit diesem Befehl können Sie ein DocumentDB-Datenbankkonto löschen.

```
Arguments
    --name -n           [Required]: Name of the DocumentDB database account.
    --resource-group -g [Required]: Name of the resource group.
```

Beispiel:

    az documentdb delete -g rg-test -n docdb-test

## <a id="get-documentdb-properties-cli"></a> Abrufen der Eigenschaften eines DocumentDB-Datenbankkontos

Mit diesem Befehl können Sie die Eigenschaften eines vorhandenen DocumentDB-Datenbankkontos abrufen.

```
Arguments
    --name -n           [Required]: Name of the DocumentDB database account.
    --resource-group -g [Required]: Name of the resource group.
```

Beispiel:

    az documentdb show -g rg-test -n docdb-test

## <a id="list-account-keys-cli"></a> Auflisten von Kontoschlüsseln

Wenn Sie ein DocumentDB-Konto erstellen, generiert der Dienst zwei Hauptzugriffsschlüssel, die für die Authentifizierung verwendet werden können, wenn der Zugriff auf das DocumentDB-Konto erfolgt. Durch Bereitstellen von zwei Zugriffsschlüsseln ermöglicht DocumentDB Ihnen das erneute Generieren der Schlüssel ohne Unterbrechung des Zugriffs auf das DocumentDB-Konto. Schreibgeschützte Schlüssel für die Authentifizierung von schreibgeschützten Vorgängen sind ebenfalls verfügbar. Es gibt zwei Lese-Schreib-Schlüssel (primär und sekundär) und zwei schreibgeschützte Schlüssel (primär und sekundär).

```
Arguments
    --name -n           [Required]: Name of the DocumentDB database account.
    --resource-group -g [Required]: Name of the resource group.
```

Beispiel:

    az documentdb list-keys -g rg-test -n docdb-test

## <a id="regenerate-account-key-cli"></a> Erneutes Generieren eines Kontoschlüssels

Sie sollten regelmäßig die Zugriffsschlüssel für Ihr DocumentDB-Konto ändern, um dafür zu sorgen, dass Ihre Verbindungen möglichst sicher sind. Zwei Zugriffsschlüssel werden zugewiesen, damit Sie Verbindungen zum DocumentDB-Konto mit einem Zugriffsschlüssel aufrechterhalten können, während Sie den anderen Zugriffsschlüssel neu generieren.

```
Arguments
    --name -n           [Required]: Name of the DocumentDB database account.
    --resource-group -g [Required]: Name of the resource group.
    --key-kind          [Required]: The access key to regenerate.  Allowed values: primary, primaryReadonly,
                                    secondary, secondaryReadonly.
```

Beispiel:

    az documentdb regenerate-key -g rg-test -n docdb-test --key-kind secondary

## <a id="modify-failover-priority-cli"></a> Ändern der Failoverpriorität eines DocumentDB-Datenbankkontos

Bei Datenbankkonten mit mehreren Regionen können Sie die Failoverpriorität der verschiedenen Regionen ändern, in denen das DocumentDB-Datenbankkonto vorhanden ist. Weitere Informationen zum Failover in Ihrem DocumentDB-Datenbankkonto finden Sie unter [Globale Verteilung von Daten mit DocumentDB](documentdb-distribute-data-globally.md).

```
Arguments
    --name -n           [Required]: Name of the DocumentDB database account.
    --resource-group -g [Required]: Name of the resource group.
    --failover-policies [Required]: Space separated failover policies in 'regionName=failoverPriority' format.
                                    E.g "East US"=0 "West US"=1.
```

Beispiel:

    az documentdb failover-priority-change "East US"=1 "West US"=0 "South Central US"=2

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun über ein DocumentDB-Datenbankkonto verfügen, besteht der nächste Schritt darin, eine DocumentDB-Datenbank zu erstellen. Mit einer der folgenden Methoden können Sie eine Datenbank erstellen:

* Im Azure-Portal, wie unter [Erstellen einer DocumentDB-Sammlung und -Datenbank mit dem Azure-Portal](documentdb-create-collection.md) beschrieben
* Mit den C# .NET-Beispielen im Projekt [DatabaseManagement](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/DatabaseManagement) des Repositorys [azure-documentdb-net](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples) auf GitHub.
* Die [DocumentDB-SDKs](documentdb-sdk-dotnet.md). DocumentDB verfügt über SDKs für .NET, Java, Python, Node.js und JavaScript-API.

Nach dem Erstellen der Datenbank müssen Sie der Datenbank [mindestens eine Sammlung hinzufügen](documentdb-create-collection.md) und den Sammlungen dann [Dokumente hinzufügen](documentdb-view-json-document-explorer.md).


Wenn eine Sammlung Dokumente enthält, können Sie in [DocumentDB SQL](documentdb-sql-query.md) an den Dokumenten [Abfragen ausführen](documentdb-sql-query.md#ExecutingSqlQueries), indem Sie den [Abfrage-Explorer](documentdb-query-collections-query-explorer.md) im Portal, die [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder eines der [SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx) verwenden.

Weitere Informationen zu DocumentDB finden Sie in folgenden Ressourcen:

* [Lernpfad für DocumentDB](https://azure.microsoft.com/documentation/learning-paths/documentdb/)
* [Ressourcenmodell und Konzepte von DocumentDB](documentdb-resources.md)


<!--Reference style links - using these makes the source content way more readable than using inline links-->
[scaling-globally]: https://azure.microsoft.com/en-us/documentation/articles/documentdb-distribute-data-globally/#scaling-across-the-planet
[install-az-cli2]: https://docs.microsoft.com/en-us/cli/azure/install-az-cli2
[az-documentdb-ref]: https://docs.microsoft.com/en-us/cli/azure/documentdb
[az-documentdb-create-ref]: https://docs.microsoft.com/en-us/cli/azure/documentdb#create
[rp-rest-api]: https://docs.microsoft.com/en-us/rest/api/documentdbresourceprovider/

