---
title: "Verbinden eines Clouddiensts mit einem benutzerdefinierten Domänencontroller | Microsoft Docs"
description: "Hier erfahren Sie, wie Sie mithilfe von PowerShell und der AD-Domänenerweiterung eine Verbindung zwischen Ihren Web-/Workerrollen und einer benutzerdefinierten AD-Domäne herstellen."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 1e2d7c87-d254-4e7a-a832-67f84411ec95
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2017
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 5489762a7a392e4e4098d85cba22d560e9858267
ms.lasthandoff: 04/27/2017


---
# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Verbinden von Azure Cloud Services-Rollen mit einem in Azure gehosteten AD-Domänencontroller
Zunächst richten wir ein Virtual Network (VNet) in Azure ein. Anschließend fügen wir dem VNet einen Active Directory-Domänencontroller (gehostet auf einem virtuellen Azure-Computer) hinzu. Danach fügen wir dem erstellten VNet vorhandene Clouddienstrollen hinzu und verbinden diese mit dem Domänencontroller.

Bedenken Sie vor dem Start Folgendes:

1. In diesem Tutorial wird PowerShell verwendet. Vergewissern Sie sich daher, dass Azure PowerShell installiert und einsatzbereit ist. Unterstützung bei der Einrichtung von Azure PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).
2. Ihr AD-Domänencontroller und Ihre Web-/Workerrolleninstanzen müssen sich im VNet befinden.

Befolgen Sie diese Schrittanleitung, und fügen Sie unten einen Kommentar ein, falls Probleme auftreten sollten. Wir werden uns mit Ihnen in Verbindung setzen (wir lesen die Kommentare wirklich).

Beim Netzwerk, auf das vom Clouddienst verwiesen wird, muss es sich um ein **klassisches virtuelles Netzwerk** handeln.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Sie können ein virtuelles Netzwerk in Azure über das klassische Azure-Portal oder über PowerShell erstellen. In diesem Tutorial wird PowerShell verwendet. Informationen zum Erstellen eines virtuellen Netzwerks über das klassische Azure-Portal finden Sie unter [Erstellen eines virtuellen Netzwerks](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers
Nachdem Sie das Virtual Network fertig eingerichtet haben, müssen Sie einen AD-Domänencontroller erstellen. In diesem Lernprogramm richten wir einen AD-Domänencontroller auf einem virtuellen Azure-Computer ein.

Erstellen Sie zu diesem Zweck über PowerShell mithilfe der folgenden Befehle einen virtuellen Computer:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Heraufstufen des virtuellen Computers zu einem Domänencontroller
Sie müssen sich am virtuellen Computer anmelden und diesen konfigurieren, um ihn als AD-Domänencontroller zu konfigurieren.

Zur Anmeldung am virtuellen Computer können Sie die RDP-Datei über PowerShell mithilfe der unten stehenden Befehle abrufen.

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Nachdem Sie am virtuellen Computer angemeldet sind, richten Sie ihn als AD-Domänencontroller ein, indem Sie den schrittweisen Anleitungen unter [Einrichten eines benutzerdefinierten AD-Domänencontrollers](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx)folgen.

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Hinzufügen des Clouddiensts zu Virtual Network
Als Nächstes müssen Sie Ihre Clouddienstbereitstellung zu dem eben erstellten VNet hinzufügen. Bearbeiten Sie zu diesem Zweck die CSCFG-Datei Ihres Clouddiensts, indem Sie die relevanten Abschnitte mithilfe von Visual Studio oder einem Editor Ihrer Wahl hinzufügen.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Anschließend erstellen Sie Ihr Clouddienstprojekt und stellen es in Azure bereit. Unterstützung bei der Bereitstellung Ihres Clouddienstpakets in Azure erhalten Sie unter [Erstellen und Bereitstellen eines Clouddiensts](cloud-services-how-to-create-deploy.md#how-to-deploy-a-cloud-service)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Verbinden der Web- und Workerrollen mit der Domäne
Sobald Ihr Clouddienstprojekt auf Azure bereitgestellt ist, verbinden Sie Ihre Rolleninstanzen mithilfe der AD-Domänenerweiterung mit der benutzerdefinierten AD-Domäne. Führen Sie in PowerShell folgende Befehle aus, um die AD-Domänenerweiterung zu Ihrer vorhandenen Clouddienstbereitstellung hinzuzufügen und der benutzerdefinierten Domäne beizutreten:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

Das ist schon alles.

Ihre Clouddienste sollten nun Ihrem benutzerdefinierten Domänencontroller beigetreten sein. Wenn Sie mehr über die verschiedenen verfügbaren Optionen zur Konfiguration der AD-Domänenerweiterung erfahren möchten, nutzen Sie die PowerShell-Hilfe, wie unten gezeigt.

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```

