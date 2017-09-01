---
title: Erstellen und Verwalten von virtuellen Windows-Computern mit dem Azure PowerShell-Modul | Microsoft-Dokumentation
description: 'Tutorial: Erstellen und Verwalten von virtuellen Windows-Computern mit dem Azure PowerShell-Modul'
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: a26f33247710431d433f3309ecdaca20e605c75a
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---

# <a name="create-and-manage-windows-vms-with-the-azure-powershell-module"></a>Erstellen und Verwalten von virtuellen Windows-Computern mit dem Azure PowerShell-Modul

Virtuelle Azure-Computer bieten eine vollständig konfigurierbare und flexible Computerumgebung. In diesem Tutorial werden grundlegende Vorgänge bei der Bereitstellung von virtuellen Azure-Computern behandelt, z.B. Auswählen einer VM-Größe, Auswählen eines VM-Images und Bereitstellen eines virtuellen Computers. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Computers und Herstellen einer Verbindung mit ihm
> * Auswählen und Verwenden von VM-Images
> * Anzeigen und Verwenden bestimmter VM-Größen
> * Ändern der Größe eines virtuellen Computers
> * Anzeigen und Verstehen des Status von virtuellen Computern

Für dieses Tutorial ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu.

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit dem Befehl [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe. 

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Vor dem virtuellen Computer muss eine Ressourcengruppe erstellt werden. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroupVM* in der Region *EastUS* erstellt. 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupVM -Location EastUS
```

Die Ressourcengruppe wird beim Erstellen oder Ändern eines virtuellen Computers angegeben und ist im gesamten Tutorial zu sehen.

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

Ein virtueller Computer muss mit einem virtuellen Netzwerk verbunden sein. Sie kommunizieren mit dem virtuellen Computer über eine öffentliche IP-Adresse durch eine Netzwerkschnittstellenkarte.

### <a name="create-virtual-network"></a>Virtuelles Netzwerk erstellen

Erstellen Sie mit [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) ein Subnetz:

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ein virtuelles Netzwerk:

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

Erstellen Sie mit [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) eine öffentliche IP-Adresse:

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

### <a name="create-network-interface-card"></a>Erstellen einer Netzwerkschnittstellenkarte

Erstellen Sie mit [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) eine Netzwerkschnittstellenkarte:

```powershell
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroupVM  `
  -Location EastUS `
  -Name myNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe

Eine Azure[Netzwerksicherheitsgruppe](../../virtual-network/virtual-networks-nsg.md) (NSG) steuert ein- und ausgehenden Datenverkehr für einen oder mehrere virtuelle Computer. Netzwerksicherheitsgruppen-Regeln dienen zum Zulassen oder Verweigern von Netzwerkdatenverkehr an einem bestimmten Port oder in einem Portbereich. Diese Regeln können auch ein Quelladresspräfix enthalten, sodass nur Datenverkehr aus einer vordefinierten Quelle mit einem virtuellen Computer kommunizieren kann. Um auf den IIS-Webserver zuzugreifen, den Sie installieren, müssen Sie eine NSG-Regel für eingehenden Datenverkehr hinzufügen.

Verwenden Sie zum Erstellen einer NSG-Regel für eingehenden Datenverkehr [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig). Das folgende Beispiel erstellt eine NSG-Regel mit dem Namen *myNSGRule*, die den Port *3389* für den virtuellen Computer öffnet:

```powershell
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow
```

Verwenden Sie [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup), um die NSG *myNSGRule* zu erstellen:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupVM `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRule
```

Fügen Sie mit [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) die NSG dem Subnetz im virtuellen Netzwerk hinzu:

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

Aktualisieren Sie mit [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) das virtuelle Netzwerk:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

Beim Erstellen eines virtuellen Computers stehen mehrere Optionen zur Verfügung, z.B. Betriebssystemimage, Festlegen der Datenträgergröße und Administratoranmeldeinformationen. In diesem Beispiel wird ein virtueller Computer mit dem Namen *myVM* erstellt, auf dem die neueste Version von Windows Server 2016 Datacenter ausgeführt wird.

Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) den Benutzernamen und das Kennwort für das Administratorkonto auf dem virtuellen Computer fest:

```powershell
$cred = Get-Credential
```

Erstellen Sie mit [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) die anfängliche Konfiguration für den virtuellen Computer:

```powershell
$vm = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1
```

Fügen Sie mit [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) Informationen zum Betriebssystem zur Konfiguration des virtuellen Computers hinzu:

```powershell
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

Fügen Sie mit [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) Informationen zum Image zur Konfiguration des virtuellen Computers hinzu:

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

Fügen Sie mit [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) Einstellungen des Betriebssystemdatenträgers zur Konfiguration des virtuellen Computers hinzu:

```powershell
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

Fügen Sie mit [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) die zuvor erstellte Netzwerkschnittstellenkarte zur Konfiguration des virtuellen Computers hinzu:

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Erstellen Sie mit [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) den virtuellen Computer.

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```

## <a name="connect-to-vm"></a>Herstellen einer Verbindung mit dem virtuellen Computer

Nachdem die Bereitstellung abgeschlossen ist, stellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer her.

Führen Sie die folgenden Befehle aus, um die öffentliche IP-Adresse des virtuellen Computers zurückzugeben. Notieren Sie sich diese IP-Adresse. Sie wird in einem späteren Schritt über den Browser aufgerufen, um die Webkonnektivität zu testen.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupVM  | Select IpAddress
```

Erstellen Sie mit dem folgenden Befehl eine Remotedesktopsitzung mit dem virtuellen Computer. Ersetzen Sie die IP-Adresse mit dem *publicIPAddress*-Wert des virtuellen Computers. Geben Sie nach Aufforderung die beim Erstellen des virtuellen Computers verwendeten Anmeldeinformationen ein.

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a>Allgemeines zu VM-Images

Der Azure Marketplace umfasst viele VM-Images, die zum Erstellen eines neuen virtuellen Computers verwendet werden können. In den vorherigen Schritten wurde ein virtueller Computer mit dem Windows Server 2016 Datacenter-Image erstellt. In diesem Schritt wird der Marketplace mithilfe des PowerShell-Moduls nach weiteren Windows-Images durchsucht, die auch als Grundlage für neue virtuelle Computer dienen können. Bei diesem Vorgang werden der Herausgeber, das Angebot und der Imagename (SKU) gesucht. 

Führen Sie den Befehl [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) aus, um eine Liste mit Imageherausgebern abzurufen.  

```powersehll
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Führen Sie den Befehl [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) aus, um eine Liste mit Imageangeboten abzurufen. Mit diesem Befehl wird die zurückgegebene Liste nach dem angegebenen Herausgeber gefiltert. 

```powershell
Get-AzureRmVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"
```

```powershell
Offer             PublisherName          Location
-----             -------------          -------- 
Windows-HUB       MicrosoftWindowsServer EastUS 
WindowsServer     MicrosoftWindowsServer EastUS   
WindowsServer-HUB MicrosoftWindowsServer EastUS   
```

Mit dem Befehl [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) wird dann nach dem Herausgeber und dem Angebotsnamen gefiltert und eine Liste mit Imagenamen zurückgegeben.

```powershell
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

```powershell
Skus                                      Offer         PublisherName          Location
----                                      -----         -------------          --------
2008-R2-SP1                               WindowsServer MicrosoftWindowsServer EastUS  
2008-R2-SP1-smalldisk                     WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter                        WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter-smalldisk              WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core               WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core-smalldisk     WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers           WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers-smalldisk WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-RDSH                 WindowsServer MicrosoftWindowsServer EastUS
2016-Nano-Server                          WindowsServer MicrosoftWindowsServer EastUS
```

Mithilfe dieser Informationen kann ein virtueller Computer mit einem spezifischen Image bereitgestellt werden. In diesem Beispiel wird der Imagename für das VM-Objekt festgelegt. Die vollständigen Schritte für die Bereitstellung finden Sie in den vorherigen Beispielen in diesem Tutorial.

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a>Allgemeines zu VM-Größen

Die Größe eines virtuellen Computers bestimmt die Menge an Computeressourcen wie z.B. CPU, GPU und Arbeitsspeicher, die für den virtuellen Computer zur Verfügung gestellt werden. Virtuelle Computer müssen mit einer angemessenen Größe für die erwartete Workload erstellt werden. Wenn die Workload zunimmt, kann die Größe eines vorhandenen virtuellen Computers geändert werden.

### <a name="vm-sizes"></a>VM-Größen

In der folgenden Tabelle sind Größen in Anwendungsfällen kategorisiert.  

| Typ                     | Größen           |    Beschreibung       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Allgemeiner Zweck         |DSv2, Dv2, DS, D, Av2, A0-7| Ausgewogenes Verhältnis von CPU zu Arbeitsspeicher. Ideal für Entwicklung und Tests, kleine bis mittlere Anwendungen und Datenlösungen.  |
| Computeoptimiert      | Fs, F             | Hohes Verhältnis von CPU zu Arbeitsspeicher. Geeignet für Anwendungen, Network Appliances und Stapelverarbeitungsvorgänge mit mittlerer Auslastung.        |
| Arbeitsspeicheroptimiert       | GS, G, DSv2, DS, Dv2, D   | Hohes Verhältnis von Speicher zu Kern. Hervorragend geeignet für relationale Datenbanken, mittlere bis große Caches und In-Memory-Analysen.                 |
| Speicheroptimiert       | Ls                | Datenträgerdurchsatz und -E/A auf hohem Niveau. Ideal für Big Data sowie SQL- und NoSQL-Datenbanken.                                                         |
| GPU           | NV, NC            | Spezialisierte virtuelle Computer für intensives Grafikrendering und intensive Videobearbeitung.       |
| Hohe Leistung | H, A8-11          | Unsere virtuellen Computer mit den leistungsfähigsten CPUs, die optional über Netzwerkschnittstellen mit hohem Durchsatz (RDMA) verfügen. 


### <a name="find-available-vm-sizes"></a>Abrufen der verfügbaren VM-Größen

Eine Liste der in einer bestimmten Region verfügbaren VM-Größen können Sie mit dem Befehl [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) abrufen.

```powershell
Get-AzureRmVMSize -Location EastUS
```

## <a name="resize-a-vm"></a>Ändern der Größe eines virtuellen Computers

Nach der Bereitstellung eines virtuellen Computers kann dessen Größe geändert werden, um die Ressourcenzuordnung zu erhöhen oder zu verringern.

Prüfen Sie vor der Größenänderung eines virtuellen Computers, ob die gewünschte Größe im aktuellen VM-Cluster verfügbar ist. Mit dem Befehl [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) wird eine Liste der Größen zurückgegeben. 

```powershell
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

Wenn die gewünschte Größe verfügbar ist, kann die Größe des virtuellen Computers im eingeschalteten Zustand geändert werden, er muss jedoch während des Vorgangs neu gestartet werden.

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
```

Wenn die gewünschte Größe im aktuellen Cluster nicht verfügbar ist, muss die Zuordnung des virtuellen Computers aufgehoben werden, damit die Größenänderung erfolgen kann. Beachten Sie, dass beim Wiedereinschalten des virtuellen Computers alle Daten auf dem temporären Datenträger entfernt werden und sich die öffentliche IP-Adresse ändert, sofern keine statische IP-Adresse verwendet wird. 

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
Start-AzureRmVM -ResourceGroupName myResourceGroupVM  -Name $vm.name
```

## <a name="vm-power-states"></a>Betriebszustände von virtuellen Computern

Ein virtueller Azure-Computer kann einen von mehreren Betriebszuständen aufweisen. Dieser Zustand entspricht dem aktuellen Zustand des virtuellen Computers im Hypervisor. 

### <a name="power-states"></a>Betriebszustände

| Betriebszustand | Beschreibung
|----|----|
| Wird gestartet | Gibt an, dass der virtuelle Computer gestartet wird. |
| Wird ausgeführt | Gibt an, dass der virtuelle Computer ausgeführt wird. |
| Wird beendet | Gibt an, dass der virtuelle Computer beendet wird. | 
| Beendet | Gibt an, dass der virtuelle Computer beendet ist. Beachten Sie, dass für virtuelle Computer in beendetem Zustand weiterhin Computegebühren anfallen.  |
| Zuordnung wird aufgehoben | Gibt an, dass die Zuordnung des virtuellen Computers aufgehoben wird. |
| Zuordnung aufgehoben | Gibt an, dass der virtuelle Computer vollständig aus dem Hypervisor entfernt, auf Steuerungsebene jedoch weiterhin verfügbar ist. Für virtuelle Computer, deren Zuordnung aufgehoben ist, fallen keine Computegebühren an. |
| - | Gibt an, dass der Betriebszustand des virtuellen Computers nicht bekannt ist. |

### <a name="find-power-state"></a>Abrufen des Betriebszustands

Verwenden Sie zum Abrufen des Zustands eines bestimmten virtuellen Computers den Befehl [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm). Achten Sie darauf, dass Sie einen gültigen Namen für einen virtuellen Computer und eine Ressourcengruppe angeben. 

```powershell
Get-AzureRmVM `
    -ResourceGroupName myResourceGroupVM `
    -Name myVM `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

Ausgabe:

```powershell
Status
------
PowerState/running
```

## <a name="management-tasks"></a>Verwaltungsaufgaben

Während der Lebensdauer eines virtuellen Computers können Sie Verwaltungsaufgaben wie das Starten, Beenden oder Löschen eines virtuellen Computers ausführen. Darüber hinaus empfiehlt es sich, Skripts zum Automatisieren von wiederkehrenden oder komplexen Aufgaben zu erstellen. Mit Azure PowerShell können viele allgemeine Verwaltungsaufgaben über die Befehlszeile oder in Skripts ausgeführt werden.

### <a name="stop-virtual-machine"></a>Beenden des virtuellen Computers

Verwenden Sie [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm), um einen virtuellen Computer zu beenden und freizugeben:

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
```

Wenn der virtuelle Computer bereitgestellt bleiben soll, verwenden Sie den Parameter „-StayProvisioned“.

### <a name="start-virtual-machine"></a>Starten des virtuellen Computers

```powershell
Start-AzureRmVM -ResourceGroupName myResourceGroupVM -Name myVM
```

### <a name="delete-resource-group"></a>Ressourcengruppe löschen

Beim Löschen einer Ressourcengruppe werden auch alle darin enthaltenen Ressourcen gelöscht.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroupVM -Force
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zur grundlegenden Erstellung und Verwaltung von virtuellen Computern erhalten, darunter:

> [!div class="checklist"]
> * Erstellen eines virtuellen Computers und Herstellen einer Verbindung mit ihm
> * Auswählen und Verwenden von VM-Images
> * Anzeigen und Verwenden bestimmter VM-Größen
> * Ändern der Größe eines virtuellen Computers
> * Anzeigen und Verstehen des Status von virtuellen Computern

Im nächsten Tutorial erhalten Sie Informationen zu VM-Datenträgern.  

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von VM-Datenträgern](./tutorial-manage-data-disk.md)

