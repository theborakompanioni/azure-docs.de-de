---
title: "Azure Cosmos DB-Automatisierung – Verwaltung mit PowerShell | Microsoft-Dokumentation"
description: Verwenden von Azure PowerShell zum Verwalten Ihrer Azure Cosmos DB-Konten.
services: cosmos-db
author: dmakwana
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: dimakwan
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: 1d5ea7263cc44a1873b5009edf4f106cc91e0067
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a>Erstellen eines Azure Cosmos DB-Kontos mithilfe von PowerShell

Der folgende Leitfaden erläutert die Befehle zum Automatisieren der Verwaltung Ihrer Azure Cosmos DB-Datenbankkonten mithilfe von Azure PowerShell. Er enthält auch Befehle zum Verwalten von Kontoschlüsseln und Failoverprioritäten in [Datenbankkonten in mehreren Regionen][scaling-globally]. Wenn Sie Ihr Datenbankkonto aktualisieren, können Sie Konsistenzrichtlinien ändern und Regionen hinzufügen bzw. entfernen. Zur plattformübergreifenden Verwaltung Ihres Azure Cosmos DB-Kontos können Sie die [Azure-Befehlszeilenschnittstelle](cli-samples.md), die [Ressourcenanbieter-REST-API][rp-rest-api] oder das [Azure-Portal](create-documentdb-dotnet.md#create-account) verwenden.

## <a name="getting-started"></a>Erste Schritte

Befolgen Sie die Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell][powershell-install-configure], um Ihr Azure Resource Manager-Konto in Azure PowerShell zu installieren und sich bei diesem Konto anzumelden.

### <a name="notes"></a>Hinweise

* Wenn die folgenden Befehle ohne Benutzerbestätigung ausgeführt werden sollen, fügen Sie das Flag `-Force` an den Befehl an.
* Alle folgenden Befehle sind synchron.

## <a id="create-documentdb-account-powershell"></a> Erstellen eines Azure Cosmos DB-Kontos

Mit diesem Befehl können Sie ein Azure Cosmos DB-Datenbankkonto erstellen. Konfigurieren Sie das neue Datenbankkonto entweder mit einer einzelnen Region oder mit [mehreren Regionen][scaling-globally] mit einer bestimmten [Konsistenzrichtlinie](consistency-levels.md).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>`: Der Name des Standorts der Schreibregion des Datenbankkontos. Dieser Standort ist erforderlich, um einen Failoverprioritätswert von 0 anzugeben. Pro Datenbankkonto muss genau eine Schreibregion vorhanden sein.
* `<read-region-location>`: Der Name des Standorts der Leseregion des Datenbankkontos. Dieser Standort ist erforderlich, um einen Failoverprioritätswert größer 0 anzugeben. Für jedes Datenbankkonto können mehrere Leseregionen vorhanden sein.
* `<ip-range-filter>` gibt die Gruppe der IP-Adressen oder IP-Adressbereiche im CIDR-Format an, die als Liste der zulässigen Client-IPs für ein bestimmtes Datenbankkonto aufgenommen wird. IP-Adressen und -Adressbereiche müssen durch Kommas voneinander getrennt werden, und sie dürfen keine Leerzeichen enthalten. Weitere Informationen finden Sie unter [Azure Cosmos DB-Firewallunterstützung](firewall-support.md).
* `<default-consistency-level>`: Die Standardkonsistenzebene des Azure Cosmos DB-Kontos. Weitere Informationen finden Sie unter [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md).
* `<max-interval>`: Bei Verwendung mit der Konsistenzebene „Begrenzte Veraltung“ stellt dieser Wert den tolerierten Veraltungszeitraum in Sekunden dar. Für diesen Wert ist ein Bereich von 1 bis 100 zulässig.
* `<max-staleness-prefix>`: Bei Verwendung mit der Konsistenzebene „Begrenzte Veraltung“ stellt dieser Wert die tolerierte Anzahl von Veraltungsanforderungen dar. Für diesen Wert ist ein Bereich von 1 bis 2.147.483.647 zulässig.
* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue Azure Cosmos DB-Datenbankkonto gehört.
* `<resource-group-location>`: Der Speicherort der Azure-Ressourcengruppe, zu der das neue Azure Cosmos DB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des zu erstellenden Azure Cosmos DB-Datenbankkontos. Der Name darf nur Kleinbuchstaben, Ziffern und den Bindestrich „-“ enthalten und muss zwischen 3 und 50 Zeichen lang sein.

Beispiel: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a>Hinweise
* Das oben stehende Beispiel erstellt ein Datenbankkonto mit zwei Regionen. Es ist auch möglich, ein Datenbankkonto mit einer Region (die als Schreibregion festgelegt ist und den Failoverprioritätswert 0 aufweist) oder mit mehr als zwei Regionen zu erstellen. Weitere Informationen finden Sie unter [ Datenbankkonten für mehrere Regionen][scaling-globally].
* Die Standorte müssen Regionen entsprechen, in denen Azure Cosmos DB allgemein verfügbar ist. Die aktuelle Liste der Regionen finden Sie auf der Seite [Azure Regionen](https://azure.microsoft.com/regions/#services).

## <a id="update-documentdb-account-powershell"></a> Aktualisieren eines DocumentDB-Datenbankkontos

Mit diesem Befehl können Sie die Eigenschaften Ihres Azure Cosmos DB-Datenbankkontos aktualisieren. Hierzu gehören z.B. die Konsistenzrichtlinie und die Standorte, in denen das Datenbankkonto vorhanden ist.

> [!NOTE]
> Mit diesem Befehl können Sie Regionen hinzufügen und entfernen, aber keine Failovereigenschaften ändern. Informationen zum Ändern von Failovereigenschaften finden Sie weiter [unten](#modify-failover-priority-powershell).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>`: Der Name des Standorts der Schreibregion des Datenbankkontos. Dieser Standort ist erforderlich, um einen Failoverprioritätswert von 0 anzugeben. Pro Datenbankkonto muss genau eine Schreibregion vorhanden sein.
* `<read-region-location>`: Der Name des Standorts der Leseregion des Datenbankkontos. Dieser Standort ist erforderlich, um einen Failoverprioritätswert größer 0 anzugeben. Für jedes Datenbankkonto können mehrere Leseregionen vorhanden sein.
* `<default-consistency-level>`: Die Standardkonsistenzebene des Azure Cosmos DB-Kontos. Weitere Informationen finden Sie unter [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md).
* `<ip-range-filter>` gibt die Gruppe der IP-Adressen oder IP-Adressbereiche im CIDR-Format an, die als Liste der zulässigen Client-IPs für ein bestimmtes Datenbankkonto aufgenommen wird. IP-Adressen und -Adressbereiche müssen durch Kommas voneinander getrennt werden, und sie dürfen keine Leerzeichen enthalten. Weitere Informationen finden Sie unter [Azure Cosmos DB-Firewallunterstützung](firewall-support.md).
* `<max-interval>`: Bei Verwendung mit der Konsistenzebene „Begrenzte Veraltung“ stellt dieser Wert den tolerierten Veraltungszeitraum in Sekunden dar. Für diesen Wert ist ein Bereich von 1 bis 100 zulässig.
* `<max-staleness-prefix>`: Bei Verwendung mit der Konsistenzebene „Begrenzte Veraltung“ stellt dieser Wert die tolerierte Anzahl von Veraltungsanforderungen dar. Für diesen Wert ist ein Bereich von 1 bis 2.147.483.647 zulässig.
* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue Azure Cosmos DB-Datenbankkonto gehört.
* `<resource-group-location>`: Der Speicherort der Azure-Ressourcengruppe, zu der das neue Azure Cosmos DB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des zu aktualisierenden Azure Cosmos DB-Datenbankkontos.

Beispiel: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <a id="delete-documentdb-account-powershell"></a> Löschen eines DocumentDB-Datenbankkontos

Mit diesem Befehl können Sie ein Azure Cosmos DB-Datenbankkonto löschen.

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue Azure Cosmos DB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des zu löschenden Azure Cosmos DB-Datenbankkontos.

Beispiel:

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="get-documentdb-properties-powershell"></a> Abrufen der Eigenschaften eines DocumentDB-Datenbankkontos

Mit diesem Befehl können Sie die Eigenschaften für ein vorhandenes Azure Cosmos DB-Datenbankkonto abrufen.

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue Azure Cosmos DB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des Azure Cosmos DB-Datenbankkontos.

Beispiel:

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="update-tags-powershell"></a> Aktualisieren von Tags eines Azure Cosmos DB-Datenbankkontos

Das folgende Beispiel beschreibt, wie Sie [Azure-Ressourcentags][azure-resource-tags] für Ihr Azure Cosmos DB-Datenbankkonto festlegen.

> [!NOTE]
> Sie können diesen Befehl mit den Befehlen zum Erstellen oder Aktualisieren kombinieren, indem Sie das `-Tags`-Flag mit dem entsprechenden Parameter anfügen.

Beispiel:

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <a id="list-account-keys-powershell"></a> Auflisten von Kontoschlüsseln

Wenn Sie ein Azure Cosmos DB-Konto erstellen, generiert der Dienst zwei Hauptzugriffsschlüssel, die für die Authentifizierung verwendet werden können, wenn der Zugriff auf das Azure Cosmos DB-Konto erfolgt. Durch Bereitstellen von zwei Zugriffsschlüsseln ermöglicht Azure Cosmos DB Ihnen das erneute Generieren der Schlüssel ohne Unterbrechung des Zugriffs auf das Azure Cosmos DB-Konto. Schreibgeschützte Schlüssel für die Authentifizierung von schreibgeschützten Vorgängen sind ebenfalls verfügbar. Es gibt zwei Lese-Schreib-Schlüssel (primär und sekundär) und zwei schreibgeschützte Schlüssel (primär und sekundär).

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue Azure Cosmos DB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des Azure Cosmos DB-Datenbankkontos.

Beispiel:

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="list-connection-strings-powershell"></a>: Verbindungszeichenfolgen auflisten

Für MongoDB-Konten kann die Verbindungszeichenfolge zum Herstellen einer Verbindung Ihrer MongoDB-App mit dem Datenbankkonto mit dem folgenden Befehl abgerufen werden.

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue Azure Cosmos DB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des Azure Cosmos DB-Datenbankkontos.

Beispiel:

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="regenerate-account-key-powershell"></a> Erneutes Generieren eines Kontoschlüssels

Sie sollten regelmäßig die Zugriffsschlüssel für Ihr Azure Cosmos DB-Konto ändern, um dafür zu sorgen, dass Ihre Verbindungen möglichst sicher sind. Zwei Zugriffsschlüssel werden zugewiesen, damit Sie Verbindungen zum Azure Cosmos DB-Konto mit einem Zugriffsschlüssel aufrechterhalten können, während Sie den anderen Zugriffsschlüssel neu generieren.

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue Azure Cosmos DB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des Azure Cosmos DB-Datenbankkontos.
* `<key-kind>`: Einer von vier Schlüsseltypen, ["Primary"|"Secondary"|"PrimaryReadonly"|"SecondaryReadonly"], die Sie erneut generieren möchten.

Beispiel:

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <a id="modify-failover-priority-powershell"></a>: Ändern der Failoverpriorität eines Azure Cosmos DB-Datenbankkontos

Bei Datenbankkonten mit mehreren Regionen können Sie die Failoverpriorität der verschiedenen Regionen ändern, in denen das Azure Cosmos DB-Datenbankkonto vorhanden ist. Weitere Informationen zum Failover in Ihrem Azure Cosmos DB-Datenbankkonto finden Sie unter [Globale Verteilung von Daten mit Azure Cosmos DB][distribute-data-globally].

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* `<write-region-location>`: Der Name des Standorts der Schreibregion des Datenbankkontos. Dieser Standort ist erforderlich, um einen Failoverprioritätswert von 0 anzugeben. Pro Datenbankkonto muss genau eine Schreibregion vorhanden sein.
* `<read-region-location>`: Der Name des Standorts der Leseregion des Datenbankkontos. Dieser Standort ist erforderlich, um einen Failoverprioritätswert größer 0 anzugeben. Für jedes Datenbankkonto können mehrere Leseregionen vorhanden sein.
* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue Azure Cosmos DB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des Azure Cosmos DB-Datenbankkontos.

Beispiel:

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Herstellen einer Verbindung mit .NET finden Sie unter [Verbinden und Abfragen mit .NET](create-documentdb-dotnet.md).
* Informationen zum Herstellen einer Verbindung mit .NET Core finden Sie unter [Verbinden und Abfragen mit .NET Core](create-documentdb-dotnet-core.md).
* Informationen zum Herstellen einer Verbindung mit Node.js finden Sie unter [Verbinden und Abfragen mit Node.js und einer MongoDB-App](create-mongodb-nodejs.md).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/documentdbresourceprovider/

