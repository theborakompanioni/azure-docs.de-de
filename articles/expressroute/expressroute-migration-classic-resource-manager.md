---
title: "Migrieren von virtuellen Netzwerken für ExpressRoute vom klassischen Modell zu Resource Manager: Azure: PowerShell | Microsoft-Dokumentation"
description: Auf dieser Seite wird beschrieben, wie zugeordnete virtuelle Netzwerke zu Ressource Manager migriert werden, nachdem die Verbindung verschoben wurde.
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: ganesr;cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: f708e7d53983551c578486ded9c5481048c7ee8b
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="migrate-expressroute-associated-virtual-networks-from-classic-to-resource-manager"></a>Migrieren von virtuellen Netzwerken für ExpressRoute vom klassischen Modell zu Resource Manager

In diesem Artikel wird erläutert, wie virtuelle Netzwerke für Azure ExpressRoute vom klassischen Bereitstellungsmodell zum Azure Resource Manager-Bereitstellungsmodell migriert werden, nachdem die ExpressRoute-Verbindung verschoben wurde. 


## <a name="before-you-begin"></a>Voraussetzungen
* Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Module besitzen. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).
* Stellen Sie sicher, dass Sie vor Beginn der Konfiguration die Seiten [Voraussetzungen](expressroute-prerequisites.md), [Routinganforderungen](expressroute-routing.md) und [Workflows](expressroute-workflows.md) gelesen haben.
* Lesen Sie die unter [Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell](expressroute-move.md) bereitgestellten Informationen. Stellen Sie sicher, dass Sie die Grenzwerte und Einschränkungen verstehen.
* Vergewissern Sie sich, dass die Verbindung im klassischen Bereitstellungsmodell voll funktionsfähig ist.
* Stellen Sie sicher, dass Sie über eine Ressourcengruppe verfügen, die im Resource Manager-Bereitstellungsmodell erstellt wurde.
* Lesen Sie die folgende Dokumentation zur Ressourcenmigration:

    * [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
    * [FAQs: Platform-supported migration of IaaS resources from classic to Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md) (Häufig gestellte Fragen: Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager)
    * [Überprüfen der häufigsten Fehler bei der Migration und Gegenmaßnahmen](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="supported-and-unsupported-scenarios"></a>Unterstützte und nicht unterstützte Szenarien

* Eine ExpressRoute-Verbindung kann ohne Downtime von der klassischen Umgebung zur Resource Manager-Umgebung verschoben werden. Sie können jede ExpressRoute-Verbindung ohne Downtime von der klassischen Umgebung auf die Resource Manager-Umgebung umstellen. Befolgen Sie die Anweisungen zum [Verschieben von ExpressRoute-Verbindungen vom klassischen Bereitstellungsmodell zum Resource Manager-Bereitstellungsmodell mithilfe von PowerShell](expressroute-howto-move-arm.md). Dies ist eine Voraussetzung für das Verschieben von Ressourcen, die mit dem virtuellen Netzwerk verbunden sind.
* Virtuelle Netzwerke, Gateways und zugeordnete Bereitstellungen innerhalb des virtuellen Netzwerks, die einer ExpressRoute-Verbindung im selben Abonnement angefügt sind, können ohne Downtime zur Resource Manager-Umgebung migriert werden. Sie können die weiter unten beschriebenen Schritte ausführen, um Ressourcen wie virtuelle Netzwerke, Gateways und im virtuellen Netzwerk bereitgestellte virtuelle Computer zu migrieren. Sie müssen sicherstellen, dass die virtuellen Netzwerke ordnungsgemäß konfiguriert sind, bevor sie migriert werden. 
* Bei virtuellen Netzwerken, Gateways und zugeordneten Bereitstellungen innerhalb des virtuellen Netzwerks, die nicht im selben Abonnement wie die ExpressRoute-Verbindung enthalten sind, ist eine gewisse Downtime zum Abschließen der Migration erforderlich. Im letzten Abschnitt des Dokuments werden die Schritte zum Migrieren von Ressourcen beschrieben.
* Ein virtuelles Netzwerk mit einem ExpressRoute-Gateway und einem VPN-Gateway kann nicht migriert werden.

## <a name="move-an-expressroute-circuit-from-classic-to-resource-manager"></a>Umstellen einer ExpressRoute-Verbindung vom klassischen Modell auf das Resource Manager-Modell
Sie müssen eine ExpressRoute-Verbindung von der klassischen Umgebung in die Resource Manager-Umgebung verschieben, bevor Sie Ressourcen migrieren, die mit der ExpressRoute-Verbindung verknüpft sind. Informationen zum Ausführen dieser Aufgabe finden Sie in den folgenden Artikeln:

* Lesen Sie die unter [Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell](expressroute-move.md) bereitgestellten Informationen.
* [Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell mithilfe von PowerShell](expressroute-howto-move-arm.md)
* Verwenden Sie das Azure-Service-Verwaltungsportal. Sie können dem Workflow zum [Erstellen einer neuen ExpressRoute-Verbindung](expressroute-howto-circuit-portal-resource-manager.md) folgen und die Importoption auswählen. 

Bei diesem Vorgang kommt es zu keinerlei Downtime. Sie können während der Migration weiterhin Daten zwischen Ihrem Standort und Microsoft übertragen.

## <a name="prepare-your-virtual-network-for-migration"></a>Vorbereiten des virtuellen Netzwerks für die Migration
Sie müssen sicherstellen, dass das Netzwerk des zu migrierenden virtuellen Netzwerks keine unnötigen Artefakte enthält. Führen Sie das folgende PowerShell-Cmdlet aus, um die Konfiguration des virtuellen Netzwerks herunterzuladen und nach Bedarf zu aktualisieren:

```powershell
Add-AzureAccount
Select-AzureSubscription -SubscriptionName <VNET Subscription>
Get-AzureVNetConfig -ExportToFile C:\virtualnetworkconfig.xml
```
      
Stellen Sie sicher, dass alle Verweise auf &lt;ConnectionsToLocalNetwork&gt; aus den zu migrierenden virtuellen Netzwerken entfernt werden. Eine Beispielnetzwerkkonfiguration sehen Sie im folgenden Codeausschnitt:

```
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
```
 
Wenn &lt;ConnectionsToLocalNetwork&gt; nicht leer ist, löschen Sie die darunter aufgeführten Verweise, und senden Sie Ihre Netzwerkkonfiguration erneut. Hierzu können Sie das folgende PowerShell-Cmdlet ausführen:

```powershell
Set-AzureVNetConfig -ConfigurationPath c:\virtualnetworkconfig.xml
```

## <a name="migrate-virtual-networks-gateways-and-associated-deployments"></a>Migrieren von virtuellen Netzwerken, Gateways und zugehörigen Bereitstellungen

Die Schritte, die Sie für die Migration ausführen, hängen davon ab, ob sich Ihre Ressourcen in demselben Abonnement, in verschiedenen Abonnements oder sowohl in demselben als auch in verschiedenen Abonnements befinden.

### <a name="migrate-virtual-networks-gateways-and-associated-deployments-in-the-same-subscription-as-the-expressroute-circuit"></a>Migrieren von virtuellen Netzwerken, Gateways und zugeordneten Bereitstellungen im selben Abonnement wie die ExpressRoute-Verbindung
In diesem Abschnitt werden die Schritte beschrieben, die zum Migrieren eines virtuellen Netzwerks, eines Gateways und zugeordneter Bereitstellungen im selben Abonnement wie die ExpressRoute-Verbindung ausgeführt werden müssen. Bei dieser Migration kommt es zu keinerlei Downtime. Während des Migrationsvorgangs können alle Ressourcen weiterhin verwendet werden. Die Verwaltungsebene ist während der Ausführung der Migration gesperrt. 

1. Stellen Sie sicher, dass die ExpressRoute-Verbindung von der klassischen Umgebung auf die Resource Manager-Umgebung umgestellt wurde.
2. Stellen Sie sicher, dass das virtuelle Netzwerk ordnungsgemäß für die Migration vorbereitet wurde.
3. Registrieren Sie Ihr Abonnement für die Ressourcenmigration. Um Ihr Abonnement für die Ressourcenmigration zu registrieren, verwenden Sie den folgenden PowerShell-Codeausschnitt:

  ```powershell 
  Select-AzureRmSubscription -SubscriptionName <Your Subscription Name>
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  ```
4. Überprüfen, Vorbereiten und Migrieren: Verwenden Sie den folgenden PowerShell-Codeausschnitt zum Verschieben des virtuellen Netzwerks:

  ```powershell
  Move-AzureVirtualNetwork -Prepare $vnetName  
  Move-AzureVirtualNetwork -Commit $vnetName
  ```

  Sie können die Migration auch abbrechen, indem Sie das folgende PowerShell-Cmdlet ausführen:

  ```powershell
  Move-AzureVirtualNetwork -Abort $vnetName
  ```

### <a name="migrate-virtual-networks-gateways-and-associated-deployments-in-a-different-subscription-from-that-of-the-expressroute-circuit"></a>Migrieren von virtuellen Netzwerken, Gateways und zugeordneter Bereitstellungen in einem anderen Abonnement als die ExpressRoute-Verbindung

1. Stellen Sie sicher, dass die ExpressRoute-Verbindung von der klassischen Umgebung auf die Resource Manager-Umgebung umgestellt wurde.
2. Stellen Sie sicher, dass das virtuelle Netzwerk ordnungsgemäß für die Migration vorbereitet wurde.
3. Stellen Sie sicher, dass die ExpressRoute-Verbindung sowohl in der klassischen Umgebung als auch in der Resource Manager-Umgebung verwendet werden kann. Um die Verwendung der Verbindung in der klassischen Umgebung und in der Resource Manager-Umgebung zuzulassen, verwenden Sie das folgende PowerShell-Skript:

  ```powershell
  Login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName <My subscription>
  $circuit = Get-AzureRmExpressRouteCircuit -Name <CircuitName> -ResourceGroupName <ResourceGroup Name> 
  $circuit.AllowClassicOperations = $true
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
  ```
4. Erstellen Sie Autorisierungen in der Resource Manager-Umgebung. Informationen zum Erstellen von Autorisierungen finden Sie unter [Verbinden eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md). Verwenden Sie zum Erstellen einer Autorisierung den folgenden PowerShell-Codeausschnitt:

  ```powershell
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

  ```powershell
  $skey = Get-AzureDedicatedCircuit | select ServiceKey
  Remove-AzureDedicatedCircuitLink -ServiceKey $skey -VNetName $vnetName
  ```  

6. Registrieren Sie Ihr Abonnement für die Ressourcenmigration. Um Ihr Abonnement für die Ressourcenmigration zu registrieren, verwenden Sie den folgenden PowerShell-Codeausschnitt:

  ```powershell
  Select-AzureRmSubscription -SubscriptionName <Your Subscription Name>
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  ```
7. Überprüfen, Vorbereiten und Migrieren: Verwenden Sie den folgenden PowerShell-Codeausschnitt zum Verschieben des virtuellen Netzwerks:

  ```powershell
  Move-AzureVirtualNetwork -Prepare $vnetName  
  Move-AzureVirtualNetwork -Commit $vnetName
  ```

    Sie können die Migration auch abbrechen, indem Sie das folgende PowerShell-Cmdlet ausführen:

  ```powershell
  Move-AzureVirtualNetwork -Abort $vnetName
  ```
8. Stellen Sie wieder eine Verbindung zwischen dem virtuellen Netzwerk und der ExpressRoute-Verbindung her. Der folgende PowerShell-Codeausschnitt wird im Kontext des Abonnements ausgeführt, in dem das virtuelle Netzwerk erstellt wird. Sie dürfen den Codeausschnitt nicht in dem Abonnement ausführen, in dem die Verbindung erstellt wird. Verwenden Sie die Verbindungs-ID als Peer-ID und den Autorisierungsschlüssel, die Sie in Schritt 4 notiert haben.

  ```powershell
  Select-AzureRMSubscription –SubscriptionName <customer subscription>  
  $gw = Get-AzureRmVirtualNetworkGateway -Name $vnetName-Default-Gateway -ResourceGroupName ($vnetName + "-Migrated")
  $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroup  ($vnetName + "-Migrated")  

  New-AzureRmVirtualNetworkGatewayConnection -Name  ($vnetName + "-GwConn") -ResourceGroupName ($vnetName + "-Migrated")  -Location $vnet.Location -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey $key
  ```

## <a name="next-steps"></a>Nächste Schritte
* [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
* [FAQs: Platform-supported migration of IaaS resources from classic to Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md) (Häufig gestellte Fragen: Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager)
* [Überprüfen der häufigsten Fehler bei der Migration und Gegenmaßnahmen](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

