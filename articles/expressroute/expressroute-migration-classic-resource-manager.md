---
title: 'Migrieren von ExpressRoute-Verbindungen und zugeordneten virtuellen Netzwerken vom klassischen Modell zu Resource Manager: Azure | Microsoft-Dokumentation'
description: Auf dieser Seite wird beschrieben, wie eine klassische Verbindung und zugeordnete virtuelle Netzwerke zu Ressource Manager migriert werden.
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 08152836-23e7-42d1-9a56-8306b341cd91
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: ganesr;cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 9a3ad5be7cc12f1c82eab4a8b3089276a434c234
ms.lasthandoff: 03/28/2017


---
# <a name="migrate-expressroute-circuits-and-associated-virtual-networks-from-the-classic-to-the-resource-manager-deployment-model"></a>Migrieren von ExpressRoute-Verbindungen und zugeordneten virtuellen Netzwerken vom klassischen Bereitstellungsmodell zum Resource Manager-Bereitstellungsmodell

In diesem Artikel wird erläutert, wie Azure ExpressRoute-Verbindungen und die zugeordneten virtuellen Netzwerke vom klassischen Bereitstellungsmodell zum Azure Resource Manager-Bereitstellungsmodell migriert werden. 


## <a name="before-you-begin"></a>Voraussetzungen
* Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Module besitzen. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).
* Stellen Sie sicher, dass Sie vor Beginn der Konfiguration die Seiten [Voraussetzungen](expressroute-prerequisites.md), [Routinganforderungen](expressroute-routing.md) und [Workflows](expressroute-workflows.md) gelesen haben.
* Lesen Sie die unter [Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell](expressroute-move.md) bereitgestellten Informationen. Stellen Sie sicher, dass Sie die Grenzwerte und Einschränkungen verstehen.
* Vergewissern Sie sich, dass die Verbindung im klassischen Bereitstellungsmodell voll funktionsfähig ist.
* Stellen Sie sicher, dass Sie über eine Ressourcengruppe verfügen, die im Resource Manager-Bereitstellungsmodell erstellt wurde.
* Lesen Sie die folgende Dokumentation zur Ressourcenmigration:

    * [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
    * [FAQs: Platform-supported migration of IaaS resources from classic to Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md) (Häufig gestellte Fragen: Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager)
    * [Common errors during classic to Azure Resource Manager migration](../virtual-machines/virtual-machines-migration-errors.md) (Häufige Fehler bei der Migration von einer klassischen Bereitstellung zu einer Azure Resource Manager-Bereitstellung)

## <a name="supported-and-unsupported-scenarios"></a>Unterstützte und nicht unterstützte Szenarien

* Eine ExpressRoute-Verbindung kann ohne Downtime von der klassischen Umgebung zur Resource Manager-Umgebung migriert werden. Sie können jede ExpressRoute-Verbindung ohne Downtime von der klassischen Umgebung auf die Resource Manager-Umgebung umstellen. Befolgen Sie die Anweisungen zum [Verschieben von ExpressRoute-Verbindungen vom klassischen Bereitstellungsmodell zum Resource Manager-Bereitstellungsmodell mithilfe von PowerShell](expressroute-howto-move-arm.md). Dies ist eine Voraussetzung für das Verschieben von Ressourcen, die mit dem virtuellen Netzwerk verbunden sind.
* Virtuelle Netzwerke, Gateways und zugeordnete Bereitstellungen innerhalb des virtuellen Netzwerks, die einer ExpressRoute-Verbindung im selben Abonnement angefügt sind, können ohne Downtime zur Resource Manager-Umgebung migriert werden. Sie können die weiter unten beschriebenen Schritte ausführen, um Ressourcen wie virtuelle Netzwerke, Gateways und im virtuellen Netzwerk bereitgestellte virtuelle Computer zu migrieren. Sie müssen sicherstellen, dass die virtuellen Netzwerke ordnungsgemäß konfiguriert sind, bevor sie migriert werden. 
* Bei virtuellen Netzwerken, Gateways und zugeordneten Bereitstellungen innerhalb des virtuellen Netzwerks, die nicht im selben Abonnement wie die ExpressRoute-Verbindung enthalten sind, ist eine gewisse Downtime zum Abschließen der Migration erforderlich. Im letzten Abschnitt des Dokuments werden die Schritte zum Migrieren von Ressourcen beschrieben.

## <a name="move-an-expressroute-circuit-from-classic-to-resource-manager"></a>Umstellen einer ExpressRoute-Verbindung vom klassischen Modell auf das Resource Manager-Modell
Sie müssen eine ExpressRoute-Verbindung von der klassischen Umgebung in die Resource Manager-Umgebung verschieben, bevor Sie Ressourcen migrieren, die mit der ExpressRoute-Verbindung verknüpft sind. Informationen zum Ausführen dieser Aufgabe finden Sie in den folgenden Artikeln:

* Lesen Sie die unter [Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell](expressroute-move.md) bereitgestellten Informationen.
* [Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell mithilfe von PowerShell](expressroute-howto-move-arm.md)
* Verwenden Sie das Azure-Service-Verwaltungsportal. Sie können dem Workflow zum [Erstellen einer neuen ExpressRoute-Verbindung](expressroute-howto-circuit-portal-resource-manager.md) folgen und die Importoption auswählen. 

Bei diesem Vorgang kommt es zu keinerlei Downtime. Sie können während der Migration weiterhin Daten zwischen Ihrem Standort und Microsoft übertragen.

## <a name="prepare-your-virtual-network-for-migration"></a>Vorbereiten des virtuellen Netzwerks für die Migration
Sie müssen sicherstellen, dass das Netzwerk des zu migrierenden virtuellen Netzwerks keine unnötigen Artefakte enthält. Führen Sie das folgende PowerShell-Cmdlet aus, um die Konfiguration des virtuellen Netzwerks herunterzuladen und nach Bedarf zu aktualisieren:

    Add-AzureAccount
    Select-AzureSubscription -SubscriptionName <VNET Subscription>
    Get-AzureVNetConfig -ExportToFile C:\virtualnetworkconfig.xml
      
Sie müssen sicherstellen, dass alle Verweise auf <ConnectionsToLocalNetwork> aus den zu migrierenden virtuellen Netzwerken entfernt werden. Eine Beispielnetzwerkkonfiguration sehen Sie im folgenden Codeausschnitt:

    <VirtualNetworkSite name="MyVNet" Location="East US">
        <AddressSpace>
            <AddressPrefix>10.0.0.0/8</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="Subnet-1">
                <AddressPrefix>10.0.0.0/11</AddressPrefix>
            </Subnet>
            <Subnet name="GatewaySubnet">
                <AddressPrefix>10.32.0.0/28</AddressPrefix>
            </Subnet>
        </Subnets>
        <Gateway>
            <ConnectionsToLocalNetwork>
            </ConnectionsToLocalNetwork>
        </Gateway>
    </VirtualNetworkSite>
 
Wenn <ConnectionsToLocalNetwork> nicht leer ist, löschen Sie die Verweise darunter, und senden Sie Ihre Netzwerkkonfiguration erneut. Hierzu können Sie das folgende PowerShell-Cmdlet ausführen:

    Set-AzureVNetConfig -ConfigurationPath c:\virtualnetworkconfig.xml

## <a name="migrate-virtual-networks-gateways-and-associated-deployments-in-the-same-subscription-as-the-expressroute-circuit"></a>Migrieren von virtuellen Netzwerken, Gateways und zugeordneten Bereitstellungen im selben Abonnement wie die ExpressRoute-Verbindung
In diesem Abschnitt werden die Schritte beschrieben, die zum Migrieren eines virtuellen Netzwerks, eines Gateways und zugeordneter Bereitstellungen im selben Abonnement wie die ExpressRoute-Verbindung ausgeführt werden müssen. Bei dieser Migration kommt es zu keinerlei Downtime. Während des Migrationsvorgangs können alle Ressourcen weiterhin verwendet werden. Die Verwaltungsebene ist während der Ausführung der Migration gesperrt. 

1. Stellen Sie sicher, dass die ExpressRoute-Verbindung von der klassischen Umgebung auf die Resource Manager-Umgebung umgestellt wurde.
2. Stellen Sie sicher, dass das virtuelle Netzwerk ordnungsgemäß für die Migration vorbereitet wurde.
3. Registrieren Sie Ihr Abonnement für die Ressourcenmigration. Um Ihr Abonnement für die Ressourcenmigration zu registrieren, verwenden Sie den folgenden PowerShell-Codeausschnitt: 
    ```
    Select-AzureRmSubscription -SubscriptionName <Your Subscription Name>
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
    ```
4. Überprüfen, Vorbereiten und Migrieren: Verwenden Sie den folgenden PowerShell-Codeausschnitt zum Verschieben des virtuellen Netzwerks:
    ```
    Move-AzureVirtualNetwork -Prepare $vnetName  
    Move-AzureVirtualNetwork -Commit $vnetName
    ```
    Sie können die Migration auch abbrechen, indem Sie das folgende PowerShell-Cmdlet ausführen:
    ```
    Move-AzureVirtualNetwork -Abort $vnetName
    ``` 
## <a name="migrate-virtual-networks-gateways-and-associated-deployments-in-a-different-subscription-from-that-of-the-expressroute-circuit"></a>Migrieren von virtuellen Netzwerken, Gateways und zugeordneter Bereitstellungen in einem anderen Abonnement als die ExpressRoute-Verbindung

1. Stellen Sie sicher, dass die ExpressRoute-Verbindung von der klassischen Umgebung auf die Resource Manager-Umgebung umgestellt wurde.
2. Stellen Sie sicher, dass das virtuelle Netzwerk ordnungsgemäß für die Migration vorbereitet wurde.
3. Stellen Sie sicher, dass die ExpressRoute-Verbindung sowohl in der klassischen Umgebung als auch in der Resource Manager-Umgebung verwendet werden kann. Um die Verwendung der Verbindung in der klassischen Umgebung und in der Resource Manager-Umgebung zuzulassen, verwenden Sie das folgende PowerShell-Skript: 
    ```
    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName <My subscription>
    $circuit = Get-AzureRmExpressRouteCircuit -Name <CircuitName> -ResourceGroupName <ResourceGroup Name> 
    $circuit.AllowClassicOperations = $true
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
    ```
4. Erstellen Sie Autorisierungen in der Resource Manager-Umgebung. Informationen zum Erstellen von Autorisierungen finden Sie unter [Verbinden eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md). Verwenden Sie zum Erstellen einer Autorisierung den folgenden PowerShell-Codeausschnitt:
    ```
    circuit = Get-AzureRmExpressRouteCircuit -Name <CircuitName> -ResourceGroupName <ResourceGroup Name> 
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "AuthorizationForMigration"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
    $circuit = Get-AzureRmExpressRouteCircuit -Name MigrateCircuit -ResourceGroupName MigrateRGWest

    $id = $circuit.id 
    $auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "AuthorizationForMigration"

    $key=$auth1.AuthorizationKey 
    ```
    Notieren Sie sich die Verbindungs-ID und den Autorisierungsschlüssel. Diese Elemente werden verwendet, um nach Abschluss der Migration die Verbindung mit dem virtuellen Netzwerk zu verbinden.
  
5. Löschen Sie die dedizierte Verbindungsverknüpfung, die dem virtuellen Netzwerk zugeordnet ist. Verwenden Sie zum Entfernen der Verbindungsverknüpfung aus der klassischen Umgebung das folgende Cmdlet: 
    ```
    $skey = Get-AzureDedicatedCircuit | select ServiceKey
    Remove-AzureDedicatedCircuitLink -ServiceKey $skey -VNetName $vnetName
    ```  

6. Registrieren Sie Ihr Abonnement für die Ressourcenmigration. Um Ihr Abonnement für die Ressourcenmigration zu registrieren, verwenden Sie den folgenden PowerShell-Codeausschnitt: 
    ```
    Select-AzureRmSubscription -SubscriptionName <Your Subscription Name>
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
    ```
7. Überprüfen, Vorbereiten und Migrieren: Verwenden Sie den folgenden PowerShell-Codeausschnitt zum Verschieben des virtuellen Netzwerks:
    ```
    Move-AzureVirtualNetwork -Prepare $vnetName  
    Move-AzureVirtualNetwork -Commit $vnetName
    ```
    Sie können die Migration auch abbrechen, indem Sie das folgende PowerShell-Cmdlet ausführen:
    ```
    Move-AzureVirtualNetwork -Abort $vnetName
    ```
8. Stellen Sie wieder eine Verbindung zwischen dem virtuellen Netzwerk und der ExpressRoute-Verbindung her. Der folgende PowerShell-Codeausschnitt wird im Kontext des Abonnements ausgeführt, in dem das virtuelle Netzwerk erstellt wird. Sie dürfen den Codeausschnitt nicht in dem Abonnement ausführen, in dem die Verbindung erstellt wird. Verwenden Sie die Verbindungs-ID als Peer-ID und den Autorisierungsschlüssel, die Sie in Schritt 4 notiert haben.
    ```
    Select-AzureRMSubscription –SubscriptionName <customer subscription>  
    $gw = Get-AzureRmVirtualNetworkGateway -Name $vnetName-Default-Gateway -ResourceGroupName ($vnetName + "-Migrated")
    $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroup  ($vnetName + "-Migrated")  

    New-AzureRmVirtualNetworkGatewayConnection -Name  ($vnetName + "-GwConn") -ResourceGroupName ($vnetName + "-Migrated")  -Location $vnet.Location -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey $key
    ```
## <a name="next-steps"></a>Nächste Schritte
* [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
* [FAQs: Platform-supported migration of IaaS resources from classic to Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md) (Häufig gestellte Fragen: Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager)
* [Common errors during classic to Azure Resource Manager migration](../virtual-machines/virtual-machines-migration-errors.md) (Häufige Fehler bei der Migration von einer klassischen Bereitstellung zu einer Azure Resource Manager-Bereitstellung)

