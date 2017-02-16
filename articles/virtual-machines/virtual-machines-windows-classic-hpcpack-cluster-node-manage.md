---
title: Verwalten von HPC Pack-Clustercomputeknoten | Microsoft Docs
description: "Informationen zu PowerShell-Skripttools zum Hinzufügen, Entfernen, Starten und Beenden von HPC Pack 2012 R2-Clustercomputeknoten in Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 4193f03b-94e9-4704-a7ad-379abde063a9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: ff9fb5f0b2229a470ea3f5c736622ee1e9228c93
ms.openlocfilehash: 1941475ce97dbb5ab4e99b147f4d67e799590e7d


---
# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Verwalten der Anzahl und Verfügbarkeit von Computeknoten in einem HPC Pack-Cluster in Azure
Wenn Sie einen HPC Pack 2012 R2-Cluster auf virtuellen Azure-Computern erstellt haben, möchten Sie wahrscheinlich einige virtuelle Computer für Computeknoten im Cluster hinzufügen, entfernen, starten (bereitstellen) oder beenden (Bereitstellung aufheben). Zum Durchführen dieser Aufgaben können Sie Azure PowerShell-Skripts ausführen, die auf dem Hauptknoten des virtuellen Computers installiert sind. Mit diesen Skripts können Sie die Anzahl und Verfügbarkeit der HPC Pack-Clusterressourcen steuern und so auch Kosten senken.

> [!IMPORTANT] 
> Dieser Artikel gilt nur für HPC Pack 2012 R2-Cluster in Azure, die mit dem klassischen Bereitstellungsmodell erstellt wurden. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.
> Darüber hinaus sind die in diesem Artikel beschriebenen PowerShell-Skripts in HPC Pack 2016 nicht verfügbar.

## <a name="prerequisites"></a>Voraussetzungen
* **HPC Pack 2012 R2-Cluster auf virtuellen Azure-Computern**: Erstellen Sie einen HPC Pack 2012 R2-Cluster im klassischen Bereitstellungsmodell. Sie können beispielsweise die Bereitstellung automatisieren, indem Sie das HPC Pack 2012 R2-VM-Image in Azure Marketplace und ein Azure PowerShell-Skript verwenden. Informationen und Voraussetzungen finden Sie unter [Erstellen eines HPC-Clusters mit dem HPC Pack-IaaS-Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
  
    Nach der Bereitstellung finden Sie die Knotenverwaltungsskripts auf dem Hauptknoten im Ordner „%CCP\_HOME%bin“. Sie müssen alle Skripts als Administrator ausführen.
* **Azure-Veröffentlichungseinstellungsdatei oder -Verwaltungszertifikat:** Sie müssen einen der folgenden beiden Schritte auf dem Hauptknoten ausführen:
  
  * **Importieren der Azure-Veröffentlichungseinstellungsdatei**. Führen Sie hierzu die folgenden Azure PowerShell-Cmdlets auf dem Hauptknoten aus:
    
    ```PowerShell
    Get-AzurePublishSettingsFile
    
    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```
  * **Konfigurieren des Azure-Verwaltungszertifikats auf dem Hauptknoten**. Wenn Sie über die CER-Datei verfügen, importieren Sie sie in "CurrentUser\My certificate store", und führen Sie dann das folgende Azure PowerShell-Cmdlet für Ihre Azure-Umgebung (AzureCloud oder AzureChinaCloud) aus:
    
    ```PowerShell
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Hinzufügen von virtuellen Computern für Computeknoten
Fügen Sie Computeknoten mit dem Script **Add-HpcIaaSNode.ps1** hinzu.

### <a name="syntax"></a>Syntax
```PowerShell
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parameter
* **ServiceName**: Name des Clouddiensts, dem virtuelle Computer für neue Computeknoten hinzugefügt werden.
* **ImageName**: Name des Azure-VM-Images, der über das klassische Azure-Portal oder das Azure PowerShell-Cmdlet **Get-AzureVMImage** abgerufen werden kann. Das Image muss die folgenden Anforderungen erfüllen:
  
  1. Es muss ein Windows-Betriebssystem installiert sein.
  2. HPC Pack muss in der Computeknotenrolle installiert sein.
  3. Bei dem Image muss es sich um ein privates Image in der Kategorie "Benutzer" und nicht um ein öffentliches Azure-VM-Image handeln.
* **Quantity**: Anzahl der hinzufügenden virtuellen Computer für Computeknoten.
* **InstanceSize**: Größe der virtuellen Computer für Computeknoten.
* **DomainUserName**: Domänenbenutzername, der zum Verknüpfen der neuen virtuellen Computer mit der Domäne verwendet wird.
* **DomainUserPassword**: Kennwort des Domänenbenutzers.
* **NodeNameSeries** (optional): Benennungsmuster für die Computeknoten. Das Format muss &lt;*Root\_Name*&gt;&lt;*Start\_Zahl*&gt;% entsprechen. "MyCN%10%" gibt beispielsweise mehrere Namen für Computeknoten, beginnend ab "MyCN11" an. Wenn kein Wert angegeben ist, verwendet das Skript die im HPC-Cluster konfigurierten Benennungsreihen für Knoten.

### <a name="example"></a>Beispiel
Im folgenden Beispiel werden dem Clouddienst *hpcservice1* 20 virtuelle Computer für Computeknoten mit der Größe „Large“ basierend auf dem VM-Image *hpccnimage1* hinzugefügt.

```PowerShell
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Entfernen von virtuellen Computern für Computeknoten
Entfernen Sie Computeknoten mit dem Script **Remove-HpcIaaSNode.ps1** .

### <a name="syntax"></a>Syntax
```PowerShell
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parameter
* **Name**: Namen der zu entfernenden Clusterknoten. Platzhalter werden unterstützt. Der Name des Parametersatzes ist "Name". Sie können nicht beide Parameter **Name** und **Node** angeben.
* **Node**: Das „HpcNode“-Objekt für die zu entfernenden Knoten, das über das HPC PowerShell-Cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)abgerufen werden kann. Der Name des Parametersatzes ist "Node". Sie können nicht beide Parameter **Name** und **Node** angeben.
* **DeleteVHD** (optional): Einstellung zum Löschen der Datenträger, die den zu entfernenden virtuellen Computern zugeordnet sind.
* **Force** (optional): Einstellung, mit der HPC-Knoten offline geschaltet werden, bevor sie entfernt werden.
* **Confirm** (optional): Aufforderung zur Bestätigung vor dem Ausführen des Befehls.
* **WhatIf**: Einstellung, mit der beschrieben wird, was geschehen würde, wenn Sie den Befehl ausführen würden, ohne ihn tatsächlich auszuführen.

### <a name="example"></a>Beispiel
Das folgende Beispiel erzwingt das Offlineschalten der Knoten, deren Namen mit *HPCNode-CN-* beginnen, und entfernt die Knoten und die zugeordneten Datenträger anschließend.

```PowerShell
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Starten von virtuellen Computern für Computeknoten
Starten Sie Computeknoten mit dem Script **Start-HpcIaaSNode.ps1** .

### <a name="syntax"></a>Syntax
```PowerShell
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parameter
* **Name**: Namen der zu startenden Clusterknoten. Platzhalter werden unterstützt. Der Name des Parametersatzes ist "Name". Sie können nicht beide Parameter **Name** und **Node** angeben.
* **Node**: Das HpcNode-Objekt für die zu startenden Knoten, das über das HPC PowerShell-Cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)abgerufen werden kann. Der Name des Parametersatzes ist "Node". Sie können nicht beide Parameter **Name** und **Node** angeben.

### <a name="example"></a>Beispiel
Im folgenden Beispiel werden Knoten gestartet, deren Namen mit *HPCNode-CN-*beginnen.

```PowerShell
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Beenden von virtuellen Computern für Computeknoten
Beenden Sie Computeknoten mit dem Skript **Stop-HpcIaaSNode.ps1** .

### <a name="syntax"></a>Syntax
```PowerShell
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parameter
* **Name**: Namen der zu beendenden Clusterknoten. Platzhalter werden unterstützt. Der Name des Parametersatzes ist "Name". Sie können nicht beide Parameter **Name** und **Node** angeben.
* **Node**: Das HpcNode-Objekt für die zu beendenden Knoten, das über das HPC PowerShell-Cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)abgerufen werden kann. Der Name des Parametersatzes ist "Node". Sie können nicht beide Parameter **Name** und **Node** angeben.
* **Force** (optional): Einstellung, mit der HPC-Knoten offline geschaltet werden, bevor sie beendet werden.

### <a name="example"></a>Beispiel
Im folgenden Beispiel werden Knoten offline geschaltet und anschließend beendet, deren Namen mit *HPCNode-CN-* beginnen.

```PowerShell
Stop-HPCIaaSNode.ps1 –Name HPCNodeCN-* -Force
```

## <a name="next-steps"></a>Nächste Schritte
* Um die Clusterknoten entsprechend der aktuellen Workload der Aufträge und Aufgaben im Cluster automatisch zu vergrößern oder zu verkleinern, finden Sie unter [Automatisches Vergrößern oder Verkleinern der HPC Pack-Clusterressourcen in Azure gemäß der Clusterworkload](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) entsprechende Informationen.




<!--HONumber=Jan17_HO1-->


