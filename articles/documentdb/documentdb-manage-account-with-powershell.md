---
title: "Azure DocumentDB-Automatisierung – Verwaltung mit PowerShell | Microsoft Docs"
description: "Verwenden Sie Azure PowerShell zum Verwalten Ihrer DocumentDB-Datenbankkonten. DocumentDB ist eine cloudbasierte NoSQL-Datenbank für JSON-Daten."
services: documentdb
author: dmakwana
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: 
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: dimakwan
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 1d7691fd5248691f42ad31224f2fff9f7f0d4b9e
ms.lasthandoff: 04/03/2017


---
# <a name="automate-azure-documentdb-account-management-using-azure-powershell"></a>Automatisieren der Azure DocumentDB-Kontoverwaltung mit Azure PowerShell
> [!div class="op_single_selector"]
> * [Azure-Portal](documentdb-create-account.md)
> * [Azure CLI 1.0](documentdb-automation-resource-manager-cli-nodejs.md)
> * [Azure CLI 2.0](documentdb-automation-resource-manager-cli.md)
> * [Azure PowerShell](documentdb-manage-account-with-powershell.md)

Der folgende Leitfaden erläutert die Befehle zum Automatisieren der Verwaltung Ihrer DocumentDB-Datenbankkonten mithilfe von Azure PowerShell. Er enthält auch Befehle zum Verwalten von Kontoschlüsseln und Failoverprioritäten in [Datenbankkonten in mehreren Regionen][scaling-globally]. Wenn Sie Ihr Datenbankkonto aktualisieren, können Sie Konsistenzrichtlinien ändern und Regionen hinzufügen bzw. entfernen. Zur plattformübergreifenden Verwaltung Ihres DocumentDB-Datenbankkontos können Sie die [Azure-Befehlszeilenschnittstelle](documentdb-automation-resource-manager-cli.md), die [Ressourcenanbieter-REST-API][rp-rest-api] oder das [Azure-Portal](documentdb-create-account.md) verwenden.

## <a name="getting-started"></a>Erste Schritte

Befolgen Sie die Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell][powershell-install-configure], um Ihr Azure Resource Manager-Konto in Azure PowerShell zu installieren und sich bei diesem Konto anzumelden.

### <a name="notes"></a>Hinweise

* Wenn die folgenden Befehle ohne Benutzerbestätigung ausgeführt werden sollen, fügen Sie das Flag `-Force` an den Befehl an.
* Alle folgenden Befehle sind synchron.

## <a id="create-documentdb-account-powershell"></a> Erstellen eines DocumentDB-Datenbankkontos

Mit diesem Befehl können Sie ein DocumentDB-Datenbankkonto erstellen. Konfigurieren Sie das neue Datenbankkonto entweder mit einer einzelnen Region oder mit [mehreren Regionen][scaling-globally] mit einer bestimmten [Konsistenzrichtlinie](documentdb-consistency-levels.md).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $DocumentDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -PropertyObject $DocumentDBProperties
    
* `<write-region-location>`: Der Name des Standorts der Schreibregion des Datenbankkontos. Dieser Standort ist erforderlich, um einen Failoverprioritätswert von 0 anzugeben. Pro Datenbankkonto muss genau eine Schreibregion vorhanden sein.
* `<read-region-location>`: Der Name des Standorts der Leseregion des Datenbankkontos. Dieser Standort ist erforderlich, um einen Failoverprioritätswert größer 0 anzugeben. Für jedes Datenbankkonto können mehrere Leseregionen vorhanden sein.
* `<ip-range-filter>` gibt die Gruppe der IP-Adressen oder IP-Adressbereiche im CIDR-Format an, die als Liste der zulässigen Client-IPs für ein bestimmtes Datenbankkonto aufgenommen wird. IP-Adressen und -Adressbereiche müssen durch Kommas voneinander getrennt werden, und sie dürfen keine Leerzeichen enthalten. Weitere Informationen finden Sie unter [DocumentDB – Firewallunterstützung](documentdb-firewall-support.md).
* `<default-consistency-level>`: Die Standardkonsistenzebene des DocumentDB-Kontos. Weitere Informationen finden Sie unter [Konsistenzebenen in DocumentDB](documentdb-consistency-levels.md).
* `<max-interval>`: Bei Verwendung mit der Konsistenzebene „Begrenzte Veraltung“ stellt dieser Wert den tolerierten Veraltungszeitraum in Sekunden dar. Für diesen Wert ist ein Bereich von 1 bis 100 zulässig.
* `<max-staleness-prefix>`: Bei Verwendung mit der Konsistenzebene „Begrenzte Veraltung“ stellt dieser Wert die tolerierte Anzahl von Veraltungsanforderungen dar. Für diesen Wert ist ein Bereich von 1 bis 2.147.483.647 zulässig.
* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue DocumentDB-Datenbankkonto gehört.
* `<resource-group-location>`: Der Standort der Azure-Ressourcengruppe, zu der das neue DocumentDB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des zu erstellenden DocumentDB-Datenbankkontos. Der Name darf nur Kleinbuchstaben, Ziffern und den Bindestrich „-“ enthalten und muss zwischen 3 und 50 Zeichen lang sein.

Beispiel: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $DocumentDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -PropertyObject $DocumentDBProperties

### <a name="notes"></a>Hinweise
* Das oben stehende Beispiel erstellt ein Datenbankkonto mit zwei Regionen. Es ist auch möglich, ein Datenbankkonto mit einer Region (die als Schreibregion festgelegt ist und den Failoverprioritätswert 0 aufweist) oder mit mehr als zwei Regionen zu erstellen. Weitere Informationen finden Sie unter [ Datenbankkonten für mehrere Regionen][scaling-globally].
* Bei den Standorten muss es sich um Regionen handeln, in denen DocumentDB allgemein verfügbar ist. Die aktuelle Liste der Regionen finden Sie auf der Seite [Azure Regionen](https://azure.microsoft.com/regions/#services).

## <a id="update-documentdb-account-powershell"></a> Aktualisieren eines DocumentDB-Datenbankkontos

Mit diesem Befehl können Sie die Eigenschaften Ihres DocumentDB-Datenbankkontos aktualisieren. Hierzu gehören z.B. die Konsistenzrichtlinie und die Standorte, in denen das Datenbankkonto vorhanden ist.

> [!NOTE]
> Mit diesem Befehl können Sie Regionen hinzufügen und entfernen, aber keine Failovereigenschaften ändern. Informationen zum Ändern von Failovereigenschaften finden Sie weiter [unten](#modify-failover-priority-powershell).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $DocumentDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -PropertyObject $DocumentDBProperties
    
* `<write-region-location>`: Der Name des Standorts der Schreibregion des Datenbankkontos. Dieser Standort ist erforderlich, um einen Failoverprioritätswert von 0 anzugeben. Pro Datenbankkonto muss genau eine Schreibregion vorhanden sein.
* `<read-region-location>`: Der Name des Standorts der Leseregion des Datenbankkontos. Dieser Standort ist erforderlich, um einen Failoverprioritätswert größer 0 anzugeben. Für jedes Datenbankkonto können mehrere Leseregionen vorhanden sein.
* `<default-consistency-level>`: Die Standardkonsistenzebene des DocumentDB-Kontos. Weitere Informationen finden Sie unter [Konsistenzebenen in DocumentDB](documentdb-consistency-levels.md).
* `<ip-range-filter>` gibt die Gruppe der IP-Adressen oder IP-Adressbereiche im CIDR-Format an, die als Liste der zulässigen Client-IPs für ein bestimmtes Datenbankkonto aufgenommen wird. IP-Adressen und -Adressbereiche müssen durch Kommas voneinander getrennt werden, und sie dürfen keine Leerzeichen enthalten. Weitere Informationen finden Sie unter [DocumentDB – Firewallunterstützung](documentdb-firewall-support.md).
* `<max-interval>`: Bei Verwendung mit der Konsistenzebene „Begrenzte Veraltung“ stellt dieser Wert den tolerierten Veraltungszeitraum in Sekunden dar. Für diesen Wert ist ein Bereich von 1 bis 100 zulässig.
* `<max-staleness-prefix>`: Bei Verwendung mit der Konsistenzebene „Begrenzte Veraltung“ stellt dieser Wert die tolerierte Anzahl von Veraltungsanforderungen dar. Für diesen Wert ist ein Bereich von 1 bis 2.147.483.647 zulässig.
* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue DocumentDB-Datenbankkonto gehört.
* `<resource-group-location>`: Der Standort der Azure-Ressourcengruppe, zu der das neue DocumentDB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des zu aktualisierenden DocumentDB-Datenbankkontos.

Beispiel: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $DocumentDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -PropertyObject $DocumentDBProperties

## <a id="delete-documentdb-account-powershell"></a> Löschen eines DocumentDB-Datenbankkontos

Mit diesem Befehl können Sie ein DocumentDB-Datenbankkonto löschen.

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue DocumentDB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des zu löschenden DocumentDB-Datenbankkontos.

Beispiel:

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="get-documentdb-properties-powershell"></a> Abrufen der Eigenschaften eines DocumentDB-Datenbankkontos

Mit diesem Befehl können Sie die Eigenschaften eines vorhandenen DocumentDB-Datenbankkontos abrufen.

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue DocumentDB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des DocumentDB-Datenbankkontos.

Beispiel:

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="update-tags-powershell"></a> Aktualisieren der Tags eines DocumentDB-Datenbankkontos

Das folgende Beispiel beschreibt, wie Sie [Azure-Ressourcentags][azure-resource-tags] für Ihr DocumentDB-Datenbankkonto festlegen.

> [!NOTE]
> Sie können diesen Befehl mit den Befehlen zum Erstellen oder Aktualisieren kombinieren, indem Sie das `-Tags`-Flag mit dem entsprechenden Parameter anfügen.

Beispiel:

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <a id="list-account-keys-powershell"></a> Auflisten von Kontoschlüsseln

Wenn Sie ein DocumentDB-Konto erstellen, generiert der Dienst zwei Hauptzugriffsschlüssel, die für die Authentifizierung verwendet werden können, wenn der Zugriff auf das DocumentDB-Konto erfolgt. Durch Bereitstellen von zwei Zugriffsschlüsseln ermöglicht DocumentDB Ihnen das erneute Generieren der Schlüssel ohne Unterbrechung des Zugriffs auf das DocumentDB-Konto. Schreibgeschützte Schlüssel für die Authentifizierung von schreibgeschützten Vorgängen sind ebenfalls verfügbar. Es gibt zwei Lese-Schreib-Schlüssel (primär und sekundär) und zwei schreibgeschützte Schlüssel (primär und sekundär).

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue DocumentDB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des DocumentDB-Datenbankkontos.

Beispiel:

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="list-connection-strings-powershell"></a>: Verbindungszeichenfolgen auflisten

Für MongoDB-Konten kann die Verbindungszeichenfolge zum Herstellen einer Verbindung Ihrer MongoDB-App mit dem Datenbankkonto mit dem folgenden Befehl abgerufen werden.

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue DocumentDB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des DocumentDB-Datenbankkontos.

Beispiel:

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="regenerate-account-key-powershell"></a> Erneutes Generieren eines Kontoschlüssels

Sie sollten regelmäßig die Zugriffsschlüssel für Ihr DocumentDB-Konto ändern, um dafür zu sorgen, dass Ihre Verbindungen möglichst sicher sind. Zwei Zugriffsschlüssel werden zugewiesen, damit Sie Verbindungen zum DocumentDB-Konto mit einem Zugriffsschlüssel aufrechterhalten können, während Sie den anderen Zugriffsschlüssel neu generieren.

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue DocumentDB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des DocumentDB-Datenbankkontos.
* `<key-kind>`: Einer von vier Schlüsseltypen, ["Primary"|"Secondary"|"PrimaryReadonly"|"SecondaryReadonly"], die Sie erneut generieren möchten.

Beispiel:

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <a id="modify-failover-priority-powershell"></a> Ändern der Failoverpriorität eines DocumentDB-Datenbankkontos

Bei Datenbankkonten mit mehreren Regionen können Sie die Failoverpriorität der verschiedenen Regionen ändern, in denen das DocumentDB-Datenbankkonto vorhanden ist. Weitere Informationen zum Failover in Ihrem DocumentDB-Datenbankkonto finden Sie unter [Globale Verteilung von Daten mit DocumentDB][distribute-data-globally].

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* `<write-region-location>`: Der Name des Standorts der Schreibregion des Datenbankkontos. Dieser Standort ist erforderlich, um einen Failoverprioritätswert von 0 anzugeben. Pro Datenbankkonto muss genau eine Schreibregion vorhanden sein.
* `<read-region-location>`: Der Name des Standorts der Leseregion des Datenbankkontos. Dieser Standort ist erforderlich, um einen Failoverprioritätswert größer 0 anzugeben. Für jedes Datenbankkonto können mehrere Leseregionen vorhanden sein.
* `<resource-group-name>`: Der Name der [Azure-Ressourcengruppe][azure-resource-groups], zu der das neue DocumentDB-Datenbankkonto gehört.
* `<database-account-name>`: Der Name des DocumentDB-Datenbankkontos.

Beispiel:

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/en-us/azure/powershell-install-configure
[scaling-globally]: https://azure.microsoft.com/en-us/documentation/articles/documentdb-distribute-data-globally/#scaling-across-the-planet
[distribute-data-globally]: https://docs.microsoft.com/en-us/azure/documentdb/documentdb-distribute-data-globally
[azure-resource-groups]: https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/en-us/rest/api/documentdbresourceprovider/
