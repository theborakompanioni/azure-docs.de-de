---
title: Erstellen virtueller Azure-Computer mit beschleunigtem Netzwerkbetrieb | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen virtuellen Computer mit beschleunigtem Netzwerkbetrieb erstellen.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: 0d74a13968338d5dc88eab3353316c77c7544615
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="create-a-virtual-machine-with-accelerated-networking"></a>Erstellen eines virtuellen Computers mit beschleunigtem Netzwerkbetrieb

In diesem Tutorial erfahren Sie, wie Sie einen virtuellen Azure-Computer (VM) mit beschleunigtem Netzwerkbetrieb erstellen. Für den beschleunigten Netzwerkbetrieb liegt GA bei Windows und in der öffentlichen Vorschau für bestimmte Linux-Verteilungen vor. Der beschleunigte Netzwerkbetrieb ermöglicht die E/A-Virtualisierung mit Einzelstamm (Single Root I/O Virtualization, SR-IOV) in einer VM und somit eine erhebliche Steigerung der Netzwerkleistung. Über diesen Hochleistungspfad wird der Host des Datenpfads umgangen, um Wartezeiten, Jitter und CPU-Auslastung zu verringern. So können mit unterstützten VM-Typen die anspruchsvollsten Netzwerkworkloads genutzt werden. Die folgende Abbildung zeigt die Kommunikation zwischen zwei virtuellen Computern (VMs) mit und ohne beschleunigten Netzwerkbetrieb:

![Vergleich](./media/virtual-network-create-vm-accelerated-networking/image1.png)

Ohne beschleunigten Netzwerkbetrieb muss sämtlicher Netzwerkdatenverkehr zur und von der VM den Host und virtuellen Switch passieren. Der virtuelle Switch setzt alle Richtlinien für den Netzwerkdatenverkehr um, z.B. Netzwerksicherheitsgruppen, Zugriffssteuerungslisten, Isolation und andere virtualisierte Netzwerkdienste. Weitere Informationen zu virtuellen Switches finden Sie im Artikel mit einer [Übersicht über Hyper-V-Netzwerkvirtualisierung und virtuellen Switch](https://technet.microsoft.com/library/jj945275.aspx).

Mit beschleunigtem Netzwerkbetrieb gelangt der Netzwerkdatenverkehr zur Netzwerkkarte (NIC) der VM und wird dann an die VM weitergeleitet. Alle Netzwerkrichtlinien, die vom virtuellen Switch ohne beschleunigten Netzwerkbetrieb angewendet werden, werden ausgelagert und in der Hardware angewendet. Durch das Anwenden von Richtlinien in der Hardware kann die NIC Netzwerkdatenverkehr direkt an den virtuellen Computer weiterleiten und dabei den Host und den virtuellen Switch umgehen, während gleichzeitig sämtliche Richtlinien umgesetzt werden, die sonst auf dem Host angewendet worden wären.

Die Vorteile des beschleunigten Netzwerkbetriebs gelten nur für die VM, auf der dieses Feature aktiviert ist. Die besten Ergebnisse erzielen Sie, wenn Sie dieses Feature auf mindestens zwei VMs aktivieren, die mit demselben virtuellen Azure-Netzwerk (VNet) verbunden sind. Bei der Kommunikation über VNets oder bei lokalen Verbindungen hat dieses Feature nur minimale Auswirkungen auf die Wartezeit.

[!INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

## <a name="benefits"></a>Vorteile
* **Geringere Latenz/mehr Pakete pro Sekunde (pps):** Durch das Entfernen des virtuellen Switch aus dem Datenpfad wird die Zeit reduziert, die Pakete auf dem Host für die Verarbeitung von Richtlinien warten müssen. Gleichzeitig wird die Anzahl von Paketen erhöht, die im virtuellen Computer verarbeitet werden können.
* **Reduzierte Jitter:** Die Verarbeitung im virtuellen Switch hängt vom Umfang der anzuwendenden Richtlinien und von der Workload der CPU ab, die die Verarbeitung durchführt. Das Auslagern der Richtlinienerzwingung auf die Hardware entfernt diesen Umweg, indem die Pakete direkt an den virtuellen Computer gesendet werden. Damit werden der Host und alle Softwareinterrupts und Kontextwechsel aus der VM-Kommunikation entfernt.
* **Verringerte CPU-Auslastung:** Das Umgehen des virtuellen Switches auf dem Host führt zu weniger CPU-Auslastung für die Verarbeitung des Netzwerkdatenverkehrs.

## <a name="Limitations"></a>Einschränkungen
Die folgenden Einschränkungen gelten für die Verwendung dieser Funktion:

* **Erstellung von Netzwerkschnittstellen:** Der beschleunigte Netzwerkbetrieb kann nur für eine neue NIC aktiviert werden. Er kann nicht für eine vorhandene NIC aktiviert werden.
* **VM-Erstellung:** Eine NIC mit aktiviertem beschleunigten Netzwerkbetrieb kann nur an eine VM angefügt werden, während diese erstellt wird. Die NIC kann nicht an eine vorhandene VM angefügt werden.
* **Regionen:** Windows-VMs mit beschleunigtem Netzwerkbetrieb werden in die meisten Azure-Regionen angeboten. Linux-VMs mit beschleunigtem Netzwerkbetrieb werden in verschiedenen Regionen angeboten. Diese Funktion wird in weiteren Regionen angeboten. Aktuelle Informationen finden Sie im unten angegebenen Azure Virtual Networking Updates-Blog.   
* **Unterstützte Betriebssysteme:** Windows: Microsoft Windows Server 2012 R2 Datacenter und Windows Server 2016. Linux: Ubuntu Server 16.04 LTS mit dem Kernel 4.4.0-77 oder höher, SLES 12 SP2, RHEL 7.3 und CentOS 7.3 (veröffentlicht von „Rogue Wave Software“).
* **VM-Größe:** Allzweck- und für Computezwecke optimierte Instanzgrößen mit mindestens acht Kernen. Weitere Informationen finden Sie in den Artikeln zu VM-Größen von [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Die Anzahl von unterstützten VM-Instanzgrößen wird in Zukunft erweitert werden.

Änderungen an diesen Einschränkungen werden auf der Seite mit [Updates für virtuelle Azure-Netzwerke](https://azure.microsoft.com/updates/accelerated-networking-in-preview) bekanntgegeben.

## <a name="create-a-windows-vm"></a>Erstellen eines virtuellen Windows-Computers
Sie können das Azure-Portal oder Azure [PowerShell](#windows-powershell) verwenden, um die VM zu erstellen.

### <a name="windows-portal"></a>Portal

1. Navigieren Sie in einem Internetbrowser zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) registrieren.
2. Klicken Sie im Portal auf **Neu** > **Compute** > **Windows Server 2016 Datacenter**. 
3. Lassen Sie auf dem eingeblendeten Blatt **Windows Server 2016 Datacenter** unter **Bereitstellungsmodell auswählen** die Option *Resource Manager* ausgewählt, und klicken Sie auf **Erstellen**.
4. Geben Sie auf dem eingeblendeten Blatt **Grundeinstellungen** die folgenden Werte ein. Die restlichen Standardoptionen können Sie übernehmen oder dafür eigene Werte auswählen oder eingeben. Klicken Sie dann auf die Schaltfläche **OK**:

    |Einstellung|Wert|
    |---|---|
    |Name|MyVm|
    |Ressourcengruppe|Lassen Sie **Neu erstellen** ausgewählt, und geben Sie *MyResourceGroup* ein.|
    |Ort|USA, Westen 2|

    Wenn Sie noch nicht mit Azure vertraut sind, informieren Sie sich über [Ressourcengruppen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [Abonnements](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) und [Standorte](https://azure.microsoft.com/regions) (die auch als Regionen bezeichnet werden).
5. Geben Sie auf dem eingeblendeten Blatt **Wählen Sie eine Größe aus** in das Feld *Mindestanzahl von Kernen* den Wert **8** ein, und klicken Sie dann auf **Alle anzeigen**.
6. Klicken Sie auf **DS4_V2 Standard** oder eine unterstützte VM und dann auf die Schaltfläche **Auswählen**.
7. Lassen Sie auf dem eingeblendeten Blatt **Einstellungen** alle Einstellungen mit einer Ausnahme unverändert. Klicken Sie unter **Beschleunigter Netzwerkbetrieb** auf **Aktiviert** und dann auf die Schaltfläche **OK**. **Hinweis:** Wenn Sie in vorherigen Schritten Werte für VM-Größe, Betriebssystem oder Standort ausgewählt haben, die nicht im Abschnitt [Einschränkungen](#Limitations) dieses Artikels aufgeführt sind, ist **Beschleunigter Netzwerkbetrieb** nicht sichtbar.
8. Klicken Sie auf dem angezeigten Blatt **Zusammenfassung** auf die Schaltfläche **OK**. Azure beginnt mit der Erstellung der VM. Dieser Vorgang dauert einige Minuten.
9. Um den Windows-Treiber für beschleunigten Netzwerkbetrieb zu installieren, führen Sie die Schritte im Abschnitt [Konfigurieren von Windows](#configure-windows) dieses Artikels aus.

### <a name="windows-powershell"></a>PowerShell
1. Installieren Sie die neueste Version des Azure PowerShell-Moduls [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Azure PowerShell für Sie neu ist, lesen Sie den Artikel [Übersicht über Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Starten Sie eine PowerShell-Sitzung durch Klicken auf die Windows-Schaltfläche „Start“. Geben Sie **powershell** ein, und klicken Sie dann in den Suchergebnissen auf **PowerShell**.
3. Geben Sie im PowerShell-Fenster den Befehl `login-azurermaccount` ein, um sich mit Ihrem Azure-[Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) anzumelden. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) registrieren.
4. Kopieren Sie in Ihrem Browser das folgende Skript:
    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Windows `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id 

    # Create the virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    #
    ```
5. Klicken Sie im PowerShell-Fenster mit der rechten Maustaste, um das Skript einzufügen und seine Ausführung zu starten. Sie werden aufgefordert, einen Benutzernamen und ein Kennwort einzugeben. Verwenden Sie diese Anmeldeinformationen, um sich bei der VM anzumelden, wenn Sie mit dieser im nächsten Schritt eine Verbindung herstellen. Wenn das Skript fehlschlägt und Sie Werte im Skript vor seiner Ausführung geändert haben, bestätigen Sie, dass die Werte, die Sie für VM-Größe, Betriebssystem und Standort verwendet haben, im Abschnitt [Einschränkungen](#Limitations) dieses Artikels aufgeführt sind.
6. Um den Windows-Treiber für beschleunigten Netzwerkbetrieb zu installieren, führen Sie die Schritte im Abschnitt [Konfigurieren von Windows](#configure-windows) dieses Artikels aus.

### <a name="configure-windows"></a>Konfigurieren von Windows
Nachdem Sie die VM in Azure erstellt haben, müssen Sie den Windows-Treiber für beschleunigten Netzwerkbetrieb installieren. Bevor Sie die folgenden Schritte ausführen, müssen Sie eine Windows-VM mit beschleunigtem Netzwerkbetrieb gemäß den Schritten für das [Portal](#windows-portal) oder für [PowerShell](#windows-powershell) in diesem Artikel erstellt haben. 

1. Navigieren Sie in einem Internetbrowser zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) registrieren.
2. Geben Sie oben im Azure-Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *MyMVm* ein. Wenn **MyVm** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem eingeblendeten Blatt **MyVm** links oben auf die Schaltfläche **Verbinden**. **Hinweis:** Wenn **Wird erstellt** unter der Schaltfläche **Verbinden** angezeigt wird, hat Azure das Erstellen der VM noch nicht abgeschlossen. Klicken Sie erst auf **Verbinden**, sobald **Wird erstellt** unter der Schaltfläche **Verbinden** nicht mehr angezeigt wird.
4. Erlauben Sie Ihrem Browser das Herunterladen der Datei **MyVm.rdp**.  Klicken Sie auf die heruntergeladene Datei, um sie zu öffnen. 
5. Klicken Sie im angezeigten Feld **Remotedesktopverbindung**, das Sie benachrichtigt, dass der Herausgeber der Remoteverbindung nicht bestimmt werden kann, auf die Schaltfläche **Verbinden**.
6. Klicken Sie im eingeblendeten Feld **Windows-Sicherheit** auf **Auswahl** und dann auf **Anderes Konto verwenden**. Geben Sie den Benutzernamen und das Kennwort aus Schritt 4 ein, und klicken Sie auf die Schaltfläche **OK**.
7. Klicken Sie im Feld „Remotedesktopverbindung“, das mit dem Hinweis angezeigt wird, dass die Identität des Remotecomputers nicht überprüft werden kann, auf die Schaltfläche **Ja**.
8. Klicken Sie mit der rechten Maustaste auf die Windows-Schaltfläche „Start“, und klicken Sie auf **Geräte-Manager**. Erweitern Sie den Knoten **Netzwerkadapter**. Vergewissern Sie sich, dass **Mellanox ConnectX-3 Virtual Function Ethernet Adapter** (wie in der folgenden Abbildung gezeigt) angezeigt wird:
   
    ![Geräte-Manager](./media/virtual-network-create-vm-accelerated-networking/image2.png)

9. Der beschleunigte Netzwerkbetrieb ist nun für Ihre VM aktiviert.

## <a name="create-a-linux-vm"></a>Erstellen eines virtuellen Linux-Computers
Sie können das Azure-Portal oder Azure [PowerShell](#linux-powershell) verwenden, um eine Ubuntu- oder SLES-VM zu erstellen. Für RHEL- und CentOS-VMs wird ein anderer Workflow verwendet.  Befolgen Sie die nachstehenden Anweisungen.

### <a name="linux-portal"></a>Portal
1. Registrieren Sie sich für die Vorschauversion des beschleunigten Netzwerkbetriebs für Linux, indem Sie im Abschnitt [Erstellen einer Linux-VM: PowerShell](#linux-powershell) in diesem Artikel die Schritte 1-5 ausführen.  Sie können sich nicht im Portal für die Vorschauversion registrieren.
2. Führen Sie die im Abschnitt [Erstellen eine Windows-VM: Portal](#windows-portal) dieses Artikels die Schritte 1-8 aus. Klicken Sie in Schritt 2 auf **Ubuntu Server 16.04 LTS** anstatt auf **Windows Server 2016 Datacenter**. Wählen Sie für dieses Tutorial ein Kennwort anstelle eines SSH-Schlüssels, obwohl für Produktionsbereitstellungen beide verwendet werden können. Wenn **Beschleuniger Netzwerkbetrieb** bei Durchführen von Schritt 7 im Abschnitt [Erstellen einer Windows-VM: Portal](#windows-portal) dieses Artikels nicht angezeigt wird, ist einer der folgenden Gründe wahrscheinlich:
    - Sie sind noch nicht für die Vorschauversion registriert. Vergewissern Sie sich, dass Ihr Registrierungsstatus, wie in Schritt 4 des Abschnitts [Erstellen einer Linux-VM: PowerShell](#linux-powershell) dieses Artikels erläutert, **Registriert** ist. **Hinweis:** Wenn Sie an der Vorschauversion des beschleunigten Netzwerkbetriebs für Windows-VMs teilgenommen haben (es ist nicht mehr nötig, sich für das Verwenden des beschleunigten Netzwerkbetriebs für Windows-VMs zu registrieren), sind Sie nicht automatisch für die Vorschauversion des beschleunigten Netzwerkbetriebs für Linux-VMs registriert. Sie müssen sich für die Vorschauversion des beschleunigten Netzwerkbetriebs für Linux-VMs registrieren, um daran teilzunehmen.
    - Sie haben keine der im Abschnitt [Einschränkungen](#limitations) dieses Artikels erwähnten VM-Größen, Betriebssysteme oder Standorte ausgewählt.
3. Um den Linux-Treiber für beschleunigten Netzwerkbetrieb zu installieren, führen Sie die Schritte im Abschnitt [Konfigurieren von Linux](#configure-linux) dieses Artikels aus.

### <a name="linux-powershell"></a>PowerShell

>[!WARNING]
>Wenn Sie virtuelle Linux-Computer mit beschleunigtem Netzwerkbetrieb in einem Abonnement erstellen und dann versuchen, eine Windows-VM mit beschleunigtem Netzwerkbetrieb im gleichen Abonnement zu erstellen, kann das Erstellen der Windows-VM fehlschlagen. In dieser Vorschauphase wird empfohlen, Linux- und Windows-VMs mit beschleunigtem Netzwerkbetrieb in separaten Abonnements zu testen.
>

1. Installieren Sie die neueste Version des Azure PowerShell-Moduls [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Azure PowerShell für Sie neu ist, lesen Sie den Artikel [Übersicht über Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Starten Sie eine PowerShell-Sitzung durch Klicken auf die Windows-Schaltfläche „Start“. Geben Sie **powershell** ein, und klicken Sie dann in den Suchergebnissen auf **PowerShell**.
3. Geben Sie im PowerShell-Fenster den Befehl `login-azurermaccount` ein, um sich mit Ihrem Azure-[Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) anzumelden. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) registrieren.
4. Registrieren Sie sich für die Vorschauphase des beschleunigten Netzwerkbetriebs für Azure mithilfe der folgenden Schritte:
    - Senden Sie eine E-Mail an [axnpreview@microsoft.com](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e), die Ihre Azure-Abonnement-ID und die beabsichtigte Verwendung enthält. Warten Sie, bis Sie eine E-Mail-Bestätigung von Microsoft bezüglich der Aktivierung Ihres Abonnements erhalten.
    - Geben Sie den folgenden Befehl ein, um zu bestätigen, dass Sie für die Vorschau registriert sind:
    
        ```powershell
        Get-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingForLinux -ProviderNamespace Microsoft.Network
        ```

        Fahren Sie erst mit Schritt 5 fort, wenn in der Ausgabe **Registriert** angezeigt wird, nachdem Sie den vorherigen Befehl eingeben haben. Die Ausgabe muss der folgenden Ausgabe entsprechen, ehe Sie fortfahren:
    
        ```powershell
        FeatureName                        ProviderName      RegistrationState
        -----------                        ------------      -----------------
        AllowAcceleratedNetworkingForLinux Microsoft.Network Registered
        ```
        
      >[!NOTE]
      >Wenn Sie an der Vorschauversion des beschleunigten Netzwerkbetriebs für Windows-VMs teilgenommen haben (es ist nicht mehr nötig, sich für das Verwenden des beschleunigten Netzwerkbetriebs für Windows-VMs zu registrieren), sind Sie nicht automatisch für die Vorschauversion des beschleunigten Netzwerkbetriebs für Linux-VMs registriert. Sie müssen sich für die Vorschauversion des beschleunigten Netzwerkbetriebs für Linux-VMs registrieren, um daran teilzunehmen.
      >
5. Kopieren Sie das folgende Skript in Ihren Browser und ersetzen Sie dabei je nach Bedarf Ubuntu oder SLES.  Auch bei Redhat und CentOS wird ein anderer Workflow verwendet, der nachfolgend beschrieben wird:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Create a new Storage account and define the new VM’s OSDisk name and its URI
    # Must end with ".vhd" extension
    $OSDiskName = "MyOsDiskName.vhd"
    # Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.
    $OSDiskSAName = "thestorageaccountname"  
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $RgName -Name $OSDiskSAName -Type "Standard_GRS" -Location $Location
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName
 
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Linux `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName Canonical `
      -Offer UbuntuServer `
      -Skus 16.04-LTS `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk -Name $OSDiskName `
      -VhdUri $OSDiskUri `
      -CreateOption FromImage 

    # Create the virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    ```
    
6. Klicken Sie im PowerShell-Fenster mit der rechten Maustaste, um das Skript einzufügen und seine Ausführung zu starten. Sie werden aufgefordert, einen Benutzernamen und ein Kennwort einzugeben. Verwenden Sie diese Anmeldeinformationen, um sich bei der VM anzumelden, wenn Sie mit dieser im nächsten Schritt eine Verbindung herstellen. Wenn das Skript fehlschlägt, überprüfen Sie, ob:
    - Sie für die Vorschau registriert (wie in Schritt 4 erläutert) sind.
    - Sie vor der Ausführung des Skripts die zulässigen Werte für VM-Größe, Betriebssystem und Standort geändert haben, die im Abschnitt [Einschränkungen](#Limitations) dieses Artikels aufgeführt sind.
7. Um den Linux-Treiber für beschleunigten Netzwerkbetrieb zu installieren, führen Sie die Schritte im Abschnitt [Konfigurieren von Linux](#configure-linux) dieses Artikels aus.

### <a name="configure-linux"></a>Konfigurieren von Linux

Nachdem Sie die VM in Azure erstellt haben, müssen Sie den Linux-Treiber für beschleunigten Netzwerkbetrieb installieren. Bevor Sie die folgenden Schritte ausführen, müssen Sie eine Linux-VM mit beschleunigtem Netzwerkbetrieb gemäß den Schritten für das [Portal](#linux-portal) oder für [PowerShell](#linux-powershell) in diesem Artikel erstellt haben. 

1. Navigieren Sie in einem Internetbrowser zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem [Azure-Konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) registrieren.
2. Klicken Sie rechts oben im Portal auf die Leiste *Ressourcen suchen* und dann auf das Symbol **>_**, um eine Bash Cloud Shell (Vorschauversion) zu öffnen. Der Bash Cloud Shell-Bereich wird unten im Portal angezeigt. Nach wenigen Sekunden wird die Eingabeaufforderung **username@Azure:~$** angezeigt. Obwohl Sie auch über SSH anstatt über die Cloud Shell von Ihrem Computer aus eine Verbindung mit der VM herstellen können, setzen die Anweisungen in diesem Tutorial voraus, dass Sie die Cloud Shell verwenden.
3. Geben Sie oben im Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *MyMVm* ein. Wenn **MyVm** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
4. Klicken Sie auf dem eingeblendeten Blatt **MyVm** links oben auf die Schaltfläche **Verbinden**. **Hinweis:** Wenn **Wird erstellt** unter der Schaltfläche **Verbinden** angezeigt wird, hat Azure das Erstellen der VM noch nicht abgeschlossen. Klicken Sie erst auf **Verbinden**, sobald **Wird erstellt** unter der Schaltfläche **Verbinden** nicht mehr angezeigt wird.
5. Azure öffnet ein Feld, das Sie zum Eingeben von `ssh adminuser@<ipaddress>` auffordert. Geben Sie diesen Befehl in Cloud Shell ein (oder kopieren Sie ihn aus dem Feld in Schritt 4, und fügen Sie ihn in Cloud Shell ein), und drücken Sie die Eingabetaste.
6. Geben Sie **Ja** als Antwort auf die Frage ein, ob Sie das Herstellen der Verbindung fortsetzen möchten, und drücken Sie die EINGABETASTE.
7. Geben Sie das Kennwort ein, das Sie beim Erstellen der VM eingegeben haben. Nach erfolgreicher Anmeldung bei der VM sehen Sie die Eingabeaufforderung adminuser@MyVm:~ $. Sie sind jetzt in der Cloud Shell-Sitzung bei der VM angemeldet. **Hinweis:** Bei Cloud Shell-Sitzungen erfolgt nach 10 Minuten Inaktivität ein Timeout.

An dieser Stelle hängen die Anweisungen von der verwendeten Verteilung ab. 

#### <a name="ubuntusles"></a>Ubuntu/SLES

1. Geben Sie in die Eingabeaufforderung `uname -r` ein und stellen Sie bezüglich der Version Folgendes sicher:

    * Ubuntu weist die Version „4.4.0-77-generic“ oder höher auf.
    * SLES weist die Version „4.4.59-92.20-default“ oder höher auf.

2. Erstellen Sie eine Verbindung zwischen der standardmäßigen Netzwerk-vNIC und der vNIC für beschleunigten Netzwerkbetrieb durch Ausführen der folgenden Befehle. Für Netzwerkverkehr wird die vNIC für den beschleunigten Netzwerkbetrieb mit höherer Leistung verwendet, wobei die Verbindung zugleich sicherstellt, dass der Netzwerkdatenverkehr bei bestimmten Konfigurationsänderungen nicht unterbrochen wird.
          
     ```bash
     wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/configure_hv_sriov.sh
     chmod +x ./configure_hv_sriov.sh
     sudo ./configure_hv_sriov.sh
     ```
3. Nachdem Sie das Skript ausgeführt haben, wird die VM nach einer 60-sekündigen Pause neu gestartet.
4. Nachdem die VM neu gestartet wurde, stellen Sie über die Schritte 5 bis 7 die Verbindung wieder her.
5. Führen Sie den Befehl `ifconfig` aus, und bestätigen Sie, dass „bond0“ angezeigt wird und dass die Schnittstelle als „UP“ angezeigt wird. 
 
 >[!NOTE]
      >Anwendungen mit beschleunigtem Netzwerkbetrieb müssen über die Schnittstelle *bond0* und nicht über *eth0* kommunizieren.  Der Schnittstellenname kann sich ändern, bevor der beschleunigte Netzwerkbetrieb allgemein verfügbar wird.

#### <a name="rhelcentos"></a>RHEL/CentOS

Für die Erstellung einer Red Hat Enterprise Linux- oder CentOS 7.3-VM sind einige zusätzliche Schritte erforderlich, um die neuesten erforderlichen Treiber für SR-IOV und den Treiber für virtuelle Funktionen (Virtual Function, VF) für die Netzwerkkarte zu laden. In der ersten Phase der Anweisungen wird ein Image vorbereitet, durch das eine oder mehrere virtuelle Computer mit vorab geladenen Treibern erstellt werden können.

##### <a name="phase-one-prepare-a-red-hat-enterprise-linux-or-centos-73-base-image"></a>Phase 1: Bereitstellen eines Red Hat Enterprise Linux- oder CentOS 7.3-basierten Image 

1.  Stellen Sie eine CentOS 7.3-VM ohne SR-IOV in Azure bereit.

2.  Installieren Sie LIS 4.2.1:
    
    ```bash
    wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.1-1.tar.gz
    tar -xvf lis-rpms-4.2.1-1.tar.gz
    cd LISISO && sudo ./install.sh
    ```

3.  Laden Sie die Konfigurationsdateien herunter.
    
    ```bash
    cd /etc/udev/rules.d/  
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/60-hyperv-vf-name.rules 
    cd /usr/sbin/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/hv_vf_name 
    sudo chmod +x hv_vf_name
    cd /etc/sysconfig/network-scripts/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/ifcfg-vf1   
    ```

4.  Heben Sie die Bereitstellung dieser VM auf.

    ```bash
    sudo waagent -deprovision+user 
    ```

5.  Beenden Sie diese VM im Azure-Portal, wechseln Sie zum Abschnitt „Datenträger“ der VM, und ermitteln Sie den VHD-URI des Betriebssystemdatenträgers. Dieser URI enthält den VHD-Namen des Basisimage und das zugehörige Speicherkonto. 
 
##### <a name="phase-two-provision-new-vms-on-azure"></a>Phase 2: Bereitstellen der neuen VMs in Azure

1.  Aktivieren Sie „AcceleratedNetworking“ auf der vNIC und stellen Sie neue VMs basierend auf „New-AzureRMVMConfig“ mithilfe der in Phase 1 erfassten VHD des Basisimage bereit:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
     -Name $RgName `
     -Location $Location

    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
     -Name MySubnet `
     -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
     -ResourceGroupName $RgName `
     -Location $Location `
     -Name MyVnet `
     -AddressPrefix 10.0.0.0/16 `
     -Subnet $Subnet
    
    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
     -Name MyPublicIp `
     -ResourceGroupName $RgName `
     -Location $Location `
     -AllocationMethod Static
    
    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
     -Name MyNic `
     -ResourceGroupName $RgName `
     -Location $Location `
     -SubnetId $Vnet.Subnets[0].Id `
     -PublicIpAddressId $Pip.Id `
     -EnableAcceleratedNetworking
    
    # Specify the base image's VHD URI (from phase one step 5). 
    # Note: The storage account of this base image vhd should have "Storage service encryption" disabled
    # See more from here: https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption
    # This is just an example URI, you will need to replace this when running this script
    $sourceUri="https://myexamplesa.blob.core.windows.net/vhds/CentOS73-Base-Test120170629111341.vhd" 

    # Specify a URI for the location from which the new image binary large object (BLOB) is copied to start the virtual machine. 
    # Must end with ".vhd" extension
    $destOsDiskName = "MyOsDiskName.vhd" 
    $destOsDiskUri = "https://myexamplesa.blob.core.windows.net/vhds/" + $destOsDiskName
    
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential
    
    # Create a custom virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
     -VMName MyVM -VMSize Standard_DS4_v2 | `
    Set-AzureRmVMOperatingSystem `
     -Linux `
     -ComputerName myVM `
     -Credential $Cred | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk `
     -Name $OSDiskName `
     -SourceImageUri $sourceUri `
     -VhdUri $destOsDiskUri `
     -CreateOption FromImage `
     -Linux
    
    # Create the virtual machine.    
    New-AzureRmVM `
     -ResourceGroupName $RgName `
     -Location $Location `
     -VM $VmConfig
    ```

2.  Nachdem die VMs gestartet wurden, prüfen Sie das VF-Gerät anhand von „Ispci“ sowie den Mellanox-Eintrag. Beispielsweise sollte das folgende Element in der Ispci-Ausgabe angezeigt werden:
    
    ```
    0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
    ```
    
3.  Führen Sie das Bondingskript durch Folgendes aus:

    ```bash
    sudo bondvf.sh
    ```

4.  Starten Sie die neuen VMs neu:

    ```bash
    sudo reboot
    ```

Die VM sollte mit der konfigurierten bond0-Schnittstelle und dem aktiviertem Pfad des beschleunigten Netzwerkbetriebs gestartet werden.  Führen Sie zur Bestätigung `ifconfig` aus.

