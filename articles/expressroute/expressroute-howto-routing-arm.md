---
title: "Konfigurieren des Routings (Peerings) für eine ExpressRoute-Verbindung: Resource Manager: PowerShell: Azure | Microsoft-Dokumentation"
description: "In diesem Artikel werden Sie durch die Schritte zum Erstellen und Bereitstellen des privaten, öffentlichen und Microsoft-Peerings einer ExpressRoute-Verbindung geführt. Außerdem wird veranschaulicht, wie Sie den Status überprüfen, Updates durchführen oder Peerings für die Verbindung löschen."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0a036d51-77ae-4fee-9ddb-35f040fbdcdf
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: ganesr;cherylmc
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: af68955b78239832e413e1b59e033d7d3da8d599
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-using-powershell"></a>Erstellen und Ändern des Peerings für eine ExpressRoute-Verbindung mithilfe von PowerShell

In diesem Artikel erfahren Sie, wie Sie die Routingkonfiguration einer ExpressRoute-Verbindung mithilfe von PowerShell im Resource Manager-Bereitstellungsmodell erstellen und verwalten. Sie können außerdem den Status prüfen, ein Update durchführen oder Peerings für eine ExpressRoute-Verbindung löschen bzw. deren Bereitstellung aufheben. Wenn Sie eine andere Methode für die Arbeit mit Ihrer Verbindung verwenden möchten, wählen Sie einen Artikel aus der folgenden Liste aus:

> [!div class="op_single_selector"]
> * [Azure-Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure-CLI](howto-routing-cli.md)
> * [Video – Privates Peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video – Öffentliches Peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video – Microsoft-Peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klassisch)](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a>Konfigurationsvoraussetzungen

* Sie müssen die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager installieren. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview). 
* Stellen Sie sicher, dass Sie vor Beginn der Konfiguration die Seiten [Voraussetzungen](expressroute-prerequisites.md), [Routinganforderungen](expressroute-routing.md) und [Workflows](expressroute-workflows.md) gelesen haben.
* Sie benötigen eine aktive ExpressRoute-Verbindung. Führen Sie die Schritte zum [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) aus, und lassen Sie sie vom Konnektivitätsanbieter aktivieren, bevor Sie fortfahren. Die ExpressRoute-Verbindung muss sich im Zustand „Provisioned“ und „Enabled“ befinden, damit Sie die Cmdlets in diesem Artikel ausführen können.

Diese Anweisungen gelten nur für Verbindungen, die über Service Provider erstellt wurden, von denen Layer 2-Konnektivitätsdienste angeboten werden. Wenn Sie einen Dienstanbieter nutzen, der verwaltete Layer 3-Dienste anbietet (meist ein IP-VPN, z.B. MPLS), übernimmt Ihr Konnektivitätsanbieter die Konfiguration und Verwaltung des Routings für Sie.

> [!IMPORTANT]
> Derzeit kündigen wir keine von Service Providern konfigurierten Peerings über das Service Management Portal an. Wir arbeiten daran, dass diese Funktion schnell bereitgestellt wird. Informieren Sie sich bei Ihrem Dienstanbieter, bevor Sie BGP-Peerings konfigurieren.
> 
> 

Sie können eine, zwei oder alle drei Peerings (Azure privat, Azure öffentlich und Microsoft) für eine ExpressRoute-Verbindung konfigurieren. Sie können Peerings in beliebiger Reihenfolge konfigurieren. Sie müssen jedoch sicherstellen, dass Sie die Konfiguration jedes Peerings einzeln nacheinander durchführen. 

## <a name="azure-private-peering"></a>Privates Azure-Peering

Dieser Abschnitt unterstützt Sie beim Erstellen, Abrufen, Aktualisieren und Löschen der privaten Azure-Peeringkonfiguration für eine ExpressRoute-Verbindung.

### <a name="to-create-azure-private-peering"></a>So erstellen Sie ein privates Azure-Peering

1. Importieren Sie das PowerShell-Modul für ExpressRoute.

  Sie müssen den neuesten PowerShell-Installer aus dem [PowerShell-Katalog](http://www.powershellgallery.com/) installieren und die Azure-Ressourcen-Manager-Module in die PowerShell-Sitzung importieren, um die ExpressRoute-Cmdlets verwenden zu können. Sie müssen PowerShell als Administrator ausführen.

  ```powershell
  Install-Module AzureRM
  Install-AzureRM
  ```

  Importieren Sie alle AzureRM.*-Module im bekannten semantischen Versionsbereich.

  ```powershell
  Import-AzureRM
  ```

  Sie können auch lediglich ein ausgewähltes Modul im bekannten semantischen Versionsbereich importieren.

  ```powershell
  Import-Module AzureRM.Network 
  ```

  Melden Sie sich bei Ihrem Konto an.

  ```powershell
  Login-AzureRmAccount
  ```

  Wählen Sie das Abonnement aus, mit dem die ExpressRoute-Verbindung erstellt werden soll.

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. Erstellen Sie eine ExpressRoute-Verbindung.

  Führen Sie die Schritte zum Erstellen einer [ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) aus, und lassen Sie sie vom Konnektivitätsanbieter bereitstellen.

  Wenn Ihr Konnektivitätsanbieter verwaltete Layer 3-Dienste im Angebot hat, können Sie anfordern, dass der Anbieter für Sie das private Azure-Peering aktiviert. In diesem Fall müssen Sie die Anweisungen in den nächsten Abschnitten nicht befolgen. Falls Ihr Konnektivitätsanbieter das Routing jedoch nicht für Sie verwaltet, setzen Sie Ihre Konfiguration nach dem Erstellen der Verbindung mit den nachfolgenden Schritten fort.
3. Überprüfen Sie, ob die ExpressRoute-Verbindung bereitgestellt und auch aktiviert wurde. Nehmen Sie das folgende Beispiel:

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  Die Antwort ähnelt dem folgenden Beispiel:

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. Konfigurieren Sie das private Azure-Peering für die Verbindung. Stellen Sie sicher, dass Sie über die folgenden Elemente verfügen, bevor Sie mit den nächsten Schritten fortfahren:

  * Ein /30-Subnetz für die primäre Verknüpfung. Dieses Subnetz darf nicht Teil eines Adressraums sein, der für virtuelle Netzwerke reserviert ist.
  * Ein /30-Subnetz für die sekundäre Verknüpfung. Dieses Subnetz darf nicht Teil eines Adressraums sein, der für virtuelle Netzwerke reserviert ist.
  * Eine gültige VLAN-ID zum Einrichten dieses Peerings. Stellen Sie sicher, dass kein anderes Peering der Verbindung die gleiche VLAN-ID verwendet.
  * AS-Nummer für Peering. Sie können sowohl AS-Nummern mit 2 Byte als auch mit 4 Byte verwenden. Sie können eine private AS-Nummer für dieses Peering verwenden. Achten Sie darauf, dass Sie nicht 65515 verwenden.
  * **Optional** – Einen MD5-Hash, wenn Sie sich für dessen Einsatz entscheiden.

  Verwenden Sie das folgende Beispiel, um das private Azure-Peering für Ihre Verbindung zu konfigurieren:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  Wenn Sie sich für den Einsatz eines MD5-Hash entscheiden, verwenden Sie das folgende Beispiel:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > Stellen Sie sicher, dass Sie Ihre AS-Nummer als Peering-ASN angeben, nicht als Kunden-ASN.
  > 
  >

### <a name="to-view-azure-private-peering-details"></a>So zeigen Sie Details zum privaten Azure-Peering an

Sie können Konfigurationsdetails anhand des folgenden Beispiels abrufen:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt
```

### <a name="to-update-azure-private-peering-configuration"></a>So aktualisieren Sie die Konfiguration für privates Azure-Peering

Sie können einen beliebigen Teil der Konfiguration anhand des folgenden Beispiels aktualisieren. In diesem Beispiel wird die VLAN-ID der Verbindung von 100 in 500 geändert.

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-azure-private-peering"></a>So löschen Sie ein privates Azure-Peering

Sie können Ihre Peeringkonfiguration entfernen, indem Sie das folgende Beispiel ausführen:

> [!WARNING]
> Sie müssen sicherstellen, dass die Verknüpfungen aller virtuellen Netzwerke mit der ExpressRoute-Verbindung aufgehoben werden, bevor Sie dieses Beispiel ausführen. 
> 
> 

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="azure-public-peering"></a>Öffentliches Azure-Peering

Dieser Abschnitt unterstützt Sie beim Erstellen, Abrufen, Aktualisieren und Löschen der öffentlichen Azure-Peeringkonfiguration für eine ExpressRoute-Verbindung.

### <a name="to-create-azure-public-peering"></a>So erstellen Sie ein öffentliches Azure-Peering

1. Importieren Sie das PowerShell-Modul für ExpressRoute.

  Sie müssen den neuesten PowerShell-Installer aus dem [PowerShell-Katalog](http://www.powershellgallery.com/) installieren und die Azure-Ressourcen-Manager-Module in die PowerShell-Sitzung importieren, um die ExpressRoute-Cmdlets verwenden zu können. Sie müssen PowerShell als Administrator ausführen.

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
```

  Importieren Sie alle AzureRM.*-Module im bekannten semantischen Versionsbereich.

  ```powershell
  Import-AzureRM
  ```

  Sie können auch lediglich ein ausgewähltes Modul im bekannten semantischen Versionsbereich importieren.

  ```powershell
  Import-Module AzureRM.Network
```

  Melden Sie sich bei Ihrem Konto an.

  ```powershell
  Login-AzureRmAccount
  ```

  Wählen Sie das Abonnement aus, mit dem die ExpressRoute-Verbindung erstellt werden soll.

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. Erstellen Sie eine ExpressRoute-Verbindung.

  Führen Sie die Schritte zum Erstellen einer [ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) aus, und lassen Sie sie vom Konnektivitätsanbieter bereitstellen.

  Wenn Ihr Konnektivitätsanbieter verwaltete Layer 3-Dienste im Angebot hat, können Sie anfordern, dass der Anbieter für Sie das private Azure-Peering aktiviert. In diesem Fall müssen Sie die Anweisungen in den nächsten Abschnitten nicht befolgen. Falls Ihr Konnektivitätsanbieter das Routing jedoch nicht für Sie verwaltet, setzen Sie Ihre Konfiguration nach dem Erstellen der Verbindung mit den nachfolgenden Schritten fort.
3. Überprüfen Sie, ob die ExpressRoute-Verbindung bereitgestellt und auch aktiviert wurde. Nehmen Sie das folgende Beispiel:

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  Die Antwort ähnelt dem folgenden Beispiel:

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                      "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. Konfigurieren Sie das öffentliche Azure-Peering für die Verbindung. Stellen Sie sicher, dass die folgenden Informationen vorliegen, bevor Sie fortfahren:

  * Ein /30-Subnetz für die primäre Verknüpfung. Dies muss ein gültiges öffentliches IPv4-Präfix sein.
  * Ein /30-Subnetz für die sekundäre Verknüpfung. Dies muss ein gültiges öffentliches IPv4-Präfix sein.
  * Eine gültige VLAN-ID zum Einrichten dieses Peerings. Stellen Sie sicher, dass kein anderes Peering der Verbindung die gleiche VLAN-ID verwendet.
  * AS-Nummer für Peering. Sie können sowohl AS-Nummern mit 2 Byte als auch mit 4 Byte verwenden.
  * **Optional** – Einen MD5-Hash, wenn Sie sich für dessen Einsatz entscheiden.

  Führen Sie das folgende Beispiel aus, um das öffentliche Azure-Peering für Ihre Verbindung zu konfigurieren:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  Wenn Sie sich für den Einsatz eines MD5-Hash entscheiden, verwenden Sie das folgende Beispiel:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  > [!IMPORTANT]
  > Stellen Sie sicher, dass Sie Ihre AS-Nummer als Peering-ASN angeben, nicht als Kunden-ASN.
  > 
  >

### <a name="to-view-azure-public-peering-details"></a>So zeigen Sie Details zum öffentlichen Azure-Peering an

Sie können die Konfigurationsdetails mit dem folgenden Cmdlet abrufen:

```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt
  ```

### <a name="to-update-azure-public-peering-configuration"></a>Aktualisieren der Konfiguration für öffentliches Azure-Peering

Sie können einen beliebigen Teil der Konfiguration anhand des folgenden Beispiels aktualisieren. In diesem Beispiel wird die VLAN-ID der Verbindung von 200 in 600 geändert.

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-azure-public-peering"></a>So löschen Sie ein öffentliches Azure-Peering

Sie können Ihre Peeringkonfiguration entfernen, indem Sie das folgende Beispiel ausführen:

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="microsoft-peering"></a>Microsoft-Peering

Dieser Abschnitt unterstützt Sie beim Erstellen, Abrufen, Aktualisieren und Löschen der Microsoft-Peeringkonfiguration für eine ExpressRoute-Verbindung.

> [!IMPORTANT]
> Beim Microsoft-Peering von ExpressRoute-Verbindungen, die vor dem 1. August 2017 konfiguriert wurden, werden alle Dienstpräfixe über das Microsoft-Peering angekündigt, auch wenn keine Routenfilter definiert sind. Beim Microsoft-Peering von ExpressRoute-Verbindungen, die am oder nach dem 1. August 2017 konfiguriert wurden, werden Präfixe erst angekündigt, wenn der Verbindung ein Routenfilter hinzugefügt wurde. Weitere Informationen finden Sie unter [Konfigurieren eines Routenfilters für das Microsoft-Peering](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>So erstellen Sie Microsoft-Peering

1. Importieren Sie das PowerShell-Modul für ExpressRoute.

  Sie müssen den neuesten PowerShell-Installer aus dem [PowerShell-Katalog](http://www.powershellgallery.com/) installieren und die Azure-Ressourcen-Manager-Module in die PowerShell-Sitzung importieren, um die ExpressRoute-Cmdlets verwenden zu können. Sie müssen PowerShell als Administrator ausführen.

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
  ```

  Importieren Sie alle AzureRM.*-Module im bekannten semantischen Versionsbereich.

  ```powershell
  Import-AzureRM
  ```

  Sie können auch lediglich ein ausgewähltes Modul im bekannten semantischen Versionsbereich importieren.

  ```powershell
  Import-Module AzureRM.Network
  ```

  Melden Sie sich bei Ihrem Konto an.

  ```powershell
  Login-AzureRmAccount
  ```

  Wählen Sie das Abonnement aus, mit dem die ExpressRoute-Verbindung erstellt werden soll.

  ```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. Erstellen Sie eine ExpressRoute-Verbindung.

  Führen Sie die Schritte zum Erstellen einer [ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) aus, und lassen Sie sie vom Konnektivitätsanbieter bereitstellen.

  Wenn Ihr Konnektivitätsanbieter verwaltete Layer 3-Dienste im Angebot hat, können Sie anfordern, dass der Anbieter für Sie das private Azure-Peering aktiviert. In diesem Fall müssen Sie die Anweisungen in den nächsten Abschnitten nicht befolgen. Falls Ihr Konnektivitätsanbieter das Routing jedoch nicht für Sie verwaltet, setzen Sie Ihre Konfiguration nach dem Erstellen der Verbindung mit den nachfolgenden Schritten fort.
3. Überprüfen Sie, ob die ExpressRoute-Verbindung bereitgestellt und auch aktiviert wurde. Nehmen Sie das folgende Beispiel:

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  Die Antwort ähnelt dem folgenden Beispiel:

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. Konfigurieren Sie das Microsoft-Peering für die Verbindung. Stellen Sie vorab sicher, dass die folgenden Informationen vorliegen:

  * Ein /30-Subnetz für die primäre Verknüpfung. Dies muss ein gültiges öffentliches IPv4-Präfix sein, das sich in Ihrem Besitz befindet und über eine RIR/IRR-Registrierung verfügt.
  * Ein /30-Subnetz für die sekundäre Verknüpfung. Dies muss ein gültiges öffentliches IPv4-Präfix sein, das sich in Ihrem Besitz befindet und über eine RIR/IRR-Registrierung verfügt.
  * Eine gültige VLAN-ID zum Einrichten dieses Peerings. Stellen Sie sicher, dass kein anderes Peering der Verbindung die gleiche VLAN-ID verwendet.
  * AS-Nummer für Peering. Sie können sowohl AS-Nummern mit 2 Byte als auch mit 4 Byte verwenden.
  * Angekündigte Präfixe: Sie müssen eine Liste mit allen Präfixen bereitstellen, die Sie über die BGP-Sitzung ankündigen möchten. Nur öffentliche IP-Adresspräfixe werden akzeptiert. Wenn Sie planen, einen Satz mit Präfixen zu senden, können Sie eine durch Komma getrennte Liste senden. Diese Präfixe müssen über eine RIR/IRR-Registrierung für Sie verfügen.
  * **Optional** – Kunden-ASN: Wenn Sie Präfixe ankündigen, die nicht für die Peering-AS-Nummer registriert sind, können Sie die AS-Nummer angeben, unter der sie registriert sind.
  * Routing-Registrierungsname: Sie können den RIR/IRR-Wert angeben, unter dem die AS-Nummer und die Präfixe registriert sind.
  * **Optional** – Einen MD5-Hash, wenn Sie sich für dessen Einsatz entscheiden.

   Verwenden Sie das folgende Beispiel, um das Microsoft-Peering für Ihre Verbindung zu konfigurieren:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

### <a name="to-get-microsoft-peering-details"></a>So rufen Sie Details zum Microsoft-Peering ab

Sie können Konfigurationsdetails anhand des folgenden Beispiels abrufen:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt
```

### <a name="to-update-microsoft-peering-configuration"></a>So aktualisieren Sie die Konfiguration des Microsoft-Peerings

Sie können einen beliebigen Teil der Konfiguration anhand des folgenden Beispiels aktualisieren:

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-microsoft-peering"></a>So löschen Sie das Microsoft-Peering

Sie können Ihre Peeringkonfiguration entfernen, indem Sie das folgende Cmdlet ausführen:

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="next-steps"></a>Nächste Schritte

[Verknüpfen Sie als Nächstes ein VNET mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md).

* Weitere Informationen zu ExpressRoute-Workflows finden Sie unter [ExpressRoute-Workflows](expressroute-workflows.md).
* Weitere Informationen zum Verbindungspeering finden Sie unter [ExpressRoute-Verbindungen und Routingdomänen](expressroute-circuit-peerings.md).
* Weitere Informationen zur Arbeit mit virtuellen Netzwerken finden Sie unter [Virtuelle Netzwerke im Überblick](../virtual-network/virtual-networks-overview.md).

