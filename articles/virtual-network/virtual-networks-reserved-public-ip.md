---
title: Verwalten reservierter IP-Adressen (klassisch) mit PowerShell | Microsoft Docs
description: "Erfahren Sie mehr über reservierte IP-Adressen (klassisch) und wie man sie mithilfe von PowerShell verwaltet."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 34652a55-3ab8-4c2d-8fb2-43684033b191
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: c934f78e514230958fad8b2aa9be4d2e56a3a835
ms.openlocfilehash: 55071ffe1bcc17b2181b4c52f51d28571a2f0eff


---
# <a name="reserved-ip-addresses-classic"></a>Reservierten IP-Adressen (klassisch)

> [!div class="op_single_selector"]
- [Azure-Portal](virtual-network-deploy-static-pip-arm-portal.md)
- [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
- [Azure-Befehlszeilenschnittstelle](virtual-network-deploy-static-pip-arm-cli.md)
- [Vorlage](virtual-network-deploy-static-pip-arm-template.md)
- [PowerShell (klassisch)](virtual-networks-reserved-public-ip.md)

In Azure fallen IP-Adressen in zwei Kategorien: dynamisch und reserviert. Öffentliche IP-Adressen, die von Azure verwaltet werden, sind standardmäßig dynamische IP-Adressen. Daher kann sich die IP-Adresse, die für einen bestimmten Clouddienst (VIP) oder zum direkten Zugreifen auf einen virtuellen Computer oder eine Rolleninstanz (ILPIP) genutzt wird, von Zeit zu Zeit ändern, wenn Ressourcen heruntergefahren oder neu zugeordnet werden.

Sie können eine IP-Adresse reservieren, um zu verhindern, dass sie sich ändert. Reservierte IPs können nur als VIP verwendet werden. So wird sichergestellt, dass die IP-Adresse für den Clouddienst sich auch dann nicht ändert, wenn Ressourcen heruntergefahren oder neu zugeordnet werden. Außerdem können Sie vorhandene dynamische IP-Adressen, die als VIP verwendet werden, in eine reservierte IP-Adresse konvertieren.

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Erfahren Sie, wie Sie eine statische öffentliche IP-Adresse mit dem [Resource Manager-Bereitstellungsmodell](virtual-network-ip-addresses-overview-arm.md) reservieren.

Lesen Sie für weitere Informationen zu IP-Adressen in Azure den Artikel [IP-Adressen](virtual-network-ip-addresses-overview-classic.md).

## <a name="when-do-i-need-a-reserved-ip"></a>Wann benötige ich eine reservierte IP-Adresse?
* **Sie möchten sicherstellen, dass die IP-Adresse in Ihrem Abonnement reserviert ist**. Wenn Sie eine IP-Adresse reservieren möchten, die Ihr Abonnement unter keinen Umständen verlässt, sollten Sie eine reservierte öffentliche IP-Adresse verwenden.  
* **Sie möchten, dass die IP-Adresse Ihrem Clouddienst auch im beendeten oder neu zugeordneten Zustand (VMs) zugewiesen bleibt**. Auf Ihren Dienst soll mit einer IP-Adresse zugegriffen werden, die sich auch dann nicht ändert, wenn virtuelle Computer im Clouddienst beendet oder neu zugeordnet werden.
* **Sie möchten sicherstellen, dass für ausgehenden Datenverkehr von Azure eine vorhersagbare IP-Adresse verwendet wird**. Unter Umständen haben Sie Ihre lokale Firewall so konfiguriert, dass nur Datenverkehr von bestimmten IP-Adressen zugelassen wird. Bei Reservierung einer IP-Adresse kennen Sie die IP-Quelladresse und müssen Ihre Firewallregeln bei einer IP-Änderung nicht aktualisieren.

## <a name="faq"></a>Häufig gestellte Fragen
1. Kann ich eine reservierte IP-Adresse für alle Azure-Dienste verwenden?  
   * Reservierte IPs können nur für virtuelle Computer und Clouddienst-Instanzenrollen, die über eine VIP verfügbar gemacht werden, verwendet werden.
2. Wie viele reservierte IP-Adressen können verwendet werden?  
   * Siehe Artikel [Einschränkungen bei Azure](../azure-subscription-service-limits.md#networking-limits).
3. Fällt für reservierte IPs eine Gebühr an?
   * Preisinformationen finden Sie unter [Preise für reservierte IP-Adressen](http://go.microsoft.com/fwlink/?LinkID=398482) .
4. Wie wird eine IP-Adresse reserviert?
   * Sie können PowerShell, die [Azure-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn722420.aspx) oder das [Azure-Portal](https://portal.azure.com) nutzen, um eine IP-Adresse in einer bestimmten Region zu reservieren. Diese reservierte IP-Adresse wird Ihrem Abonnement zugeordnet.
5. Ist die Verwendung mit auf Affinitätsgruppen basierenden VNets möglich?
   * Reservierte IP-Adressen werden nur in regionalen VNets unterstützt. Für VNets, die Affinitätsgruppen zugeordnet sind, besteht keine Unterstützung. Weitere Informationen zum Zuordnen eines VNets zu einer Region oder Affinitätsgruppe finden Sie unter [Informationen zu regionalen VNets und Affinitätsgruppen](virtual-networks-migrate-to-regional-vnet.md).

## <a name="manage-reserved-vips"></a>Verwalten von reservierten VIPs

Stellen Sie sicher, dass PowerShell durch Ausführen der Schritte im Artikel [Install and configure PowerShell (Installieren und Konfigurieren von Azure PowerShell)](/powershell/azureps-cmdlets-docs) installiert und konfiguriert wurde. 

Bevor Sie reservierte IPs verwenden können, müssen Sie diese Ihrem Abonnement hinzufügen. Führen Sie den folgenden Befehl aus, um eine reservierte IP aus dem Pool mit den öffentlichen IP-Adressen zu erstellen, die für den Standort *USA, Mitte* verfügbar sind:

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
```

Beachten Sie dabei, dass Sie nicht angeben können, welche IP-Adresse reserviert wird. Führen Sie den folgenden PowerShell-Befehl aus, und notieren Sie sich die Werte für *ReservedIPName* und *Address*, um anzuzeigen, welche IP-Adressen in Ihrem Abonnement reserviert sind:

```powershell
Get-AzureReservedIP
```

Erwartete Ausgabe:

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

Nachdem eine IP-Adresse reserviert wurde, bleibt sie Ihrem Abonnement so lange zugeordnet, bis Sie sie löschen. Führen Sie den folgenden PowerShell-Befehl aus, um die obige reservierte IP zu löschen:

```powershell
Remove-AzureReservedIP -ReservedIPName "MyReservedIP"
```

## <a name="reserve-the-ip-address-of-an-existing-cloud-service"></a>Reservieren der IP-Adresse eines vorhandenen Clouddiensts
Sie können die IP-Adresse eines vorhandenen Clouddiensts durch Hinzufügen des `-ServiceName`-Parameters reservieren. Um beispielsweise die IP-Adresse des Clouddiensts *TestService* am Standort *USA, Mitte* zu reservieren, führen Sie den folgenden PowerShell-Befehl aus:

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService
```

## <a name="associate-a-reserved-ip-to-a-new-cloud-service"></a>Zuordnen einer reservierten IP zu einem neuen Clouddienst
Mit dem unten angegebenen Skript wird eine neue reservierte IP erstellt, die dann einem neuen Clouddienst mit dem Namen *TestService*zugeordnet wird.

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"
```

> [!NOTE]
> Wenn Sie eine reservierte IP für die Verwendung mit einem Clouddienst erstellen, müssen Sie auf den virtuellen Computer für die eingehende Kommunikation weiterhin mit *VIP:&lt;Portnummer>* verweisen. Das Reservieren einer IP-Adresse bedeutet nicht, dass Sie eine direkte Verbindung mit dem virtuellen Computer herstellen können. Die reservierte IP wird dem Clouddienst zugewiesen, für den der virtuelle Computer bereitgestellt wurde. Wenn Sie direkt per IP eine Verbindung mit dem virtuellen Computer herstellen möchten, müssen Sie eine öffentliche IP auf Instanzebene konfigurieren. Eine öffentliche IP auf Instanzebene ist eine Art von öffentlicher IP (als ILPIP bezeichnet), die Ihrem virtuellen Computer direkt zugewiesen ist. Sie kann nicht reserviert werden. Weitere Informationen finden Sie unter [Öffentliche IP auf Instanzebene (ILPIP)](virtual-networks-instance-level-public-ip.md) .
> 

## <a name="remove-a-reserved-ip-from-a-running-deployment"></a>Entfernen einer reservierten IP aus einer aktiven Bereitstellung
Führen Sie den folgenden PowerShell-Befehl aus, um die reservierte IP, die dem oben per Skript erstellten neuen Dienst hinzugefügt wurde, zu entfernen:

```powershell
Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService
```

> [!NOTE]
> Wenn Sie eine reservierte IP aus einer aktiven Bereitstellung entfernen, wird die Reservierung nicht aus Ihrem Abonnement entfernt. Die IP-Adresse wird lediglich frei und kann von einer anderen Ressource in Ihrem Abonnement verwendet werden.
> 

## <a name="associate-a-reserved-ip-to-a-running-deployment"></a>Zuordnen einer reservierten IP zu einer aktiven Bereitstellung
Mit den folgenden Befehlen wird ein neuer Clouddienst mit dem Namen *TestService2* und dem neuen virtuellen Computer *TestVM2* erstellt. Anschließend wird die vorhandene reservierte IP mit dem Namen *MyReservedIP* dem Clouddienst zugeordnet:

```powershell
$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService2 -Location "Central US"

Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2
```

## <a name="associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Zuordnen einer reservierten IP zu einem Clouddienst mit einer Dienstkonfigurationsdatei
Sie können eine reservierte IP einem Clouddienst auch zuordnen, indem Sie eine Dienstkonfigurationsdatei (CSCFG) verwenden. Im folgenden XML-Beispielcode wird veranschaulicht, wie Sie einen Clouddienst für die Verwendung einer reservierten VIP mit dem Namen *MyReservedIP*konfigurieren:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über die Funktionsweise der [IP-Adressierung](virtual-network-ip-addresses-overview-classic.md) im klassischen Bereitstellungsmodell.
* Erfahren Sie mehr zu [reservierten privaten IP-Adressen](virtual-networks-reserved-private-ip.md).
* Erfahren Sie mehr zu [öffentlichen IP-Adressen auf Instanzebene (ILPIP)](virtual-networks-instance-level-public-ip.md).




<!--HONumber=Feb17_HO1-->


