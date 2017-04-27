---
title: "Öffentliche IP-Adressen (klassisch) auf Azure-Instanzebene | Microsoft-Dokumentation"
description: "Grundlegendes zu öffentlichen IP-Adressen auf Instanzebene (ILPIP) und zum Verwalten dieser Adressen mit PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 07eef6ec-7dfe-4c4d-a2c2-be0abfb48ec5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 773043f2841ec7539b0d49357dec6bcb9f4f78a1
ms.lasthandoff: 04/03/2017


---
# <a name="instance-level-public-ip-classic-overview"></a>Übersicht über die öffentliche IP-Adresse (klassisch) auf Instanzebene
Eine öffentliche IP-Adresse auf Instanzebene (Instance-Level Public IP, ILPIP) ist eine öffentliche IP-Adresse, die Sie einer VM oder Cloud Services-Rolleninstanz direkt zuweisen können, statt sie dem Clouddienst zuzuweisen, in dem sich Ihre VM oder Rolleninstanz befindet. Eine ILPIP tritt nicht an die Stelle der virtuellen IP-Adresse (VIP), die Ihrem Clouddienst zugeordnet ist. Es ist vielmehr eine zusätzliche IP-Adresse, mit der Sie direkt eine Verbindung mit der VM oder Rolleninstanz herstellen können.

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt, dass virtuelle Computer über Resource Manager erstellt werden. Stellen Sie sicher, dass Sie die Funktionsweise von [IP-Adressen](virtual-network-ip-addresses-overview-classic.md) in Azure verstehen.

![Unterscheidung zwischen ILPIP und VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Wie in Abbildung 1 dargestellt, erfolgt der Zugriff auf den Clouddienst über eine VIP-Adresse, während auf die einzelnen virtuellen Computer normalerweise unter Angabe von VIP:&lt;Portnummer&gt; zugegriffen wird. Die Zuweisung einer ILPIP zu einem bestimmten virtuellen Computer ermöglicht es, über diese IP-Adresse direkt auf diesen virtuellen Computer zuzugreifen.

Bei der Erstellung eines Clouddiensts in Azure werden automatisch entsprechende DNS A-Datensätze für den Zugriff auf den Dienst über einen vollständig qualifizierten Domänennamen (FQDN) anstelle der tatsächlichen VIP-Adresse erstellt. Dasselbe geschieht für eine ILPIP, um den Zugriff auf die VM oder Rolleninstanz über den FQDN statt der ILPIP zu ermöglichen. Wenn Sie beispielsweise einen Clouddienst namens *contosoadservice* erstellen und eine Webrolle namens *contosoweb* mit zwei Instanzen konfigurieren, registriert Azure die folgenden A-Datensätze für die Instanzen:

* contosoweb\_IN_0.contosoadservice.cloudapp.net
* contosoweb\_IN_1.contosoadservice.cloudapp.net 

> [!NOTE]
> Sie können einer VM- oder Rolleninstanz jeweils nur eine ILPIP zuweisen. Sie können bis zu 5 ILPIPs pro Abonnement verwenden. ILPIPs werden für Multi-NIC-VMs nicht unterstützt.
> 
> 

## <a name="why-would-i-request-an-ilpip"></a>Warum kann es sinnvoll sein, eine ILPIP anzufordern?
Wenn Sie über eine direkt zugewiesene IP-Adresse eine Verbindung mit Ihrer VM- oder Rolleninstanz herstellen möchten, anstatt VIP:&lt;Portnummer&gt; des Clouddiensts zu verwenden, fordern Sie eine ILPIP für die VM- oder Rolleninstanz an.

* **Aktives FTP**: Wird einer VM eine ILPIP zugewiesen, kann die VM Datenverkehr an beliebigen Ports empfangen. Die VM benötigt keine Endpunkte, um Datenverkehr empfangen zu können.  Einzelheiten zum FTP-Protokoll finden Sie unter (https://de.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview)[File Transfer Protocol].
* **Ausgehende IP**: Der vom virtuellen Computer ausgehende Datenverkehr wird der ILPIP als Quelle zugeordnet, und durch die ILPIP wird die VM gegenüber externen Entitäten eindeutig identifiziert.

> [!NOTE]
> Bisher wurde eine ILPIP-Adresse als PIP-Adresse (public IP, öffentliche IP) bezeichnet.
> 

## <a name="manage-an-ilpip-for-a-vm"></a>Verwalten einer ILPIP für einen virtuellen Computer
In den folgenden Aufgaben wird gezeigt, wie Sie ILPIPs für virtuelle Computer erstellen, zuweisen und entfernen.

### <a name="how-to-request-an-ilpip-during-vm-creation-using-powershell"></a>So fordern Sie eine ILPIP während der Erstellung des virtuellen Computers mit PowerShell an
Das folgende PowerShell-Skript erstellt einen Clouddienst namens *FTPService*, ruft ein Image aus Azure ab, erstellt mit dem abgerufenen Image einen virtuellen Computer namens *FTPInstance*, konfiguriert den virtuellen Computer für die Verwendung einer ILPIP und fügt den virtuellen Computer dem neuen Dienst hinzu:

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"} `
New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Abrufen von ILPIP-Informationen für einen virtuellen Computer
Führen Sie zum Anzeigen der ILPIP-Informationen für den mit dem vorherigen Skript erstellten virtuellen Computer den folgenden PowerShell-Befehl aus, und beobachten Sie die Werte für *PublicIPAddress* und *PublicIPName*:

```powershell
Get-AzureVM -Name FTPInstance -ServiceName FTPService
```

Erwartete Ausgabe:
 
    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            :     Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

### <a name="how-to-remove-an-ilpip-from-a-vm"></a>Entfernen einer ILPIP von einem virtuellen Computer
Führen Sie den folgenden PowerShell-Befehl aus, um die ILPIP zu entfernen, die dem virtuellen Computer im voerherigen Skript hinzugefügt wurde:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Hinzufügen einer ILPIP zu einem vorhandenen virtuellen Computer
Führen Sie den folgenden Befehl aus, um dem virtuellen Computer, der mit dem vorherigen Skript erstellt wurde, eine ILPIP hinzuzufügen:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a>Verwalten einer ILPIP für eine Cloud Services-Rolleninstanz

Um eine ILPIP zu einer Cloud Services-Rolleninstanz hinzuzufügen, führen Sie die folgenden Schritte aus:

1. Laden Sie die CSFG-Datei für den Clouddienst herunter, indem Sie Schritte ausführen, die im Artikel [Konfigurieren von Cloud-Diensten](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) aufgeführt sind.
2. Aktualisieren Sie die CSFG-Datei, indem Sie das `InstanceAddress`-Element hinzufügen. Im folgenden Beispiel wird eine ILPIP namens *MyPublicIP* zu einer Rolleninstanz namens *WebRole1* hinzugefügt: 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ILPIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
          <ConfigurationSettings>
        <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
          </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <InstanceAddress roleName="WebRole1">
        <PublicIPs>
          <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>
    ```
3. Laden Sie die CSFG-Datei für den Clouddienst hoch, indem Sie Schritte ausführen, die im Artikel [Konfigurieren von Cloud-Diensten](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) aufgeführt sind.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über die Funktionsweise der [IP-Adressierung](virtual-network-ip-addresses-overview-classic.md) im klassischen Bereitstellungsmodell.
* Informieren Sie sich über [reservierte IPs](virtual-networks-reserved-public-ip.md).

