---
title: Bereitstellen einer App in VM-Skalierungsgruppen
description: Verwenden Sie Erweiterungen zum Bereitstellen einer App in Azure VM-Skalierungsgruppen.
services: virtual-machine-scale-sets
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: adegeo
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: fa7d9d3bef4cb326844ede76171e8c566e87116b
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Bereitstellen der App in VM-Skalierungsgruppen

Dieser Artikel beschreibt verschiedene Möglichkeiten zum Installieren der Software zum Bereitstellungszeitpunkt der Skalierungsgruppe.

Es empfiehlt sich, den Artikel [Übersicht über das Entwerfen von Skalierungsgruppen](virtual-machine-scale-sets-design-overview.md) zu lesen, in dem einige der Beschränkungen durch VM-Skalierungsgruppen beschrieben werden.

## <a name="capture-and-reuse-an-image"></a>Erfassen und Wiederverwenden eines Images

Sie können einen virtuellen Computer in Azure verwenden, um ein Basisimage für die Skalierungsgruppe vorzubereiten. Bei diesem Vorgang wird ein verwalteter Datenträger in Ihrem Speicherkonto erstellt, auf den Sie als Basisimage für Ihre Skalierungsgruppe verweisen können. 

Führen Sie folgende Schritte aus:

1. Erstellen eines virtuellen Azure-Computers
   * [Linux][linux-vm-create]
   * [Windows][windows-vm-create]

2. Melden Sie sich remote beim virtuellen Computer an, und passen Sie das System nach Wunsch an.

   Bei Bedarf können Sie Ihre Anwendung jetzt installieren. Beachten Sie jedoch, dass es in diesem Fall möglicherweise komplizierter wird, die Anwendung zu aktualisieren, da Sie sie eventuell erst entfernen müssen, wenn Sie sie jetzt installieren. Stattdessen können Sie in diesem Schritt alle Voraussetzungen für Ihre Anwendung installieren, z.B. eine bestimmte Runtime oder ein Betriebssystemfeature.

3. Befolgen Sie das Tutorial „Erfassen eines Computers“ für [Linux][linux-vm-capture] oder [Windows][windows-vm-capture].

4. Erstellen Sie eine [VM-Skalierungsgruppe][vmss-create] mit dem Bild-URI, den Sie im vorherigen Schritt erfasst haben.

Weitere Informationen über Datenträger finden Sie unter [Managed Disks – Übersicht](../virtual-machines/windows/managed-disks-overview.md) und [Verwenden angefügter Datenträger](virtual-machine-scale-sets-attached-disks.md).

## <a name="install-when-the-scale-set-is-provisioned"></a>Installieren nach der Bereitstellung der Skalierungsgruppe

Auf eine VM-Skalierungsgruppe können VM-Erweiterungen angewendet werden. Mit VM-Erweiterungen können Sie die virtuellen Computer in einer Skalierungsgruppe als ganze Gruppe anpassen. Weitere Informationen zu Erweiterungen finden Sie unter [VM-Erweiterungen](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Abhängig davon, ob Ihr Betriebssystem auf Linux oder Windows basiert, können Sie drei Haupterweiterungen verwenden.

### <a name="windows"></a>Windows

Für ein Windows-basiertes Betriebssystem verwenden Sie eine der Erweiterungen **Custom Script v1.8** oder **PowerShell DSC**.

#### <a name="custom-script"></a>Custom Script

Die benutzerdefinierte Skripterweiterung führt ein Skript auf jeder VM-Instanz in der Skalierungsgruppe aus. Eine Konfigurationsdatei oder Variable gibt an, welche Dateien auf den virtuellen Computer heruntergeladen werden und welcher Befehl dann ausgeführt wird. Auf diese Weise können Sie beispielsweise ein Installationsprogramm, ein Skript, eine Batchdatei oder eine ausführbare Datei ausführen.

PowerShell verwendet eine Hashtabelle für die Einstellungen. In diesem Beispiel wird die benutzerdefinierte Skripterweiterung zum Ausführen eines PowerShell-Skripts, das IIS installiert, konfiguriert.

```powershell
# Setup extension configuration hashtable variable
$customConfig = @{
  "fileUris" = @("https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1");
  "commandToExecute" = "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> `"%TEMP%\StartupLog.txt`" 2>&1";
};

# Add the extension to the config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Compute -Type CustomScriptExtension -TypeHandlerVersion 1.8 -Name "customscript1" -Setting $customConfig

# Send the new config to Azure
Update-AzureRmVmss -ResourceGroupName $rg -Name "MyVmssTest143"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>Verwenden Sie den Schalter `-ProtectedSetting` für Einstellungen, die möglicherweise vertrauliche Informationen enthalten.

---------


Die Azure-Befehlszeilenschnittstelle verwendet eine JSON-Datei für die Einstellungen. In diesem Beispiel wird die benutzerdefinierte Skripterweiterung zum Ausführen eines PowerShell-Skripts, das IIS installiert, konfiguriert. Speichern Sie die folgende JSON-Datei als _settings.json_.

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1"
  ],
  "commandToExecute": "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> \"%TEMP%\StartupLog.txt\" 2>&1"
}
```

Führen Sie dann diesen Azure CLI-Befehl aus.

```azurecli
az vmss extension set --publisher Microsoft.Compute --version 1.8 --name CustomScriptExtension --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Verwenden Sie den Schalter `--protected-settings` für Einstellungen, die möglicherweise vertrauliche Informationen enthalten.

### <a name="powershell-dsc"></a>PowerShell DSC

Sie können PowerShell DSC zum Anpassen der VM-Instanzen für Skalierungsgruppen verwenden. Die **DSC**-Erweiterung, die von **Microsoft.Powershell** veröffentlicht wird, stellt die bereitgestellte DSC-Konfiguration auf jeder VM-Instanz bereit und führt sie aus. Eine Konfigurationsdatei oder Variable teilt der Erweiterung mit, wo sich das *ZIP*-Paket befindet und welche Kombination aus _Skriptfunktionen_ ausgeführt werden soll.

PowerShell verwendet eine Hashtabelle für die Einstellungen. In diesem Beispiel wird ein DSC-Paket bereitgestellt, das IIS installiert.

```powershell
# Setup extension configuration hashtable variable
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Add the extension to the config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Powershell -Type DSC -TypeHandlerVersion 2.24 -Name "dsc1" -Setting $dscConfig

# Send the new config to Azure
Update-AzureRmVmss -ResourceGroupName $rg -Name "myscaleset1"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>Verwenden Sie den Schalter `-ProtectedSetting` für Einstellungen, die möglicherweise vertrauliche Informationen enthalten.

-----------

Die Azure-Befehlszeilenschnittstelle verwendet eine JSON-Datei für die Einstellungen. In diesem Beispiel wird ein DSC-Paket bereitgestellt, das IIS installiert. Speichern Sie die folgende JSON-Datei als _settings.json_.

```json
{
  "wmfVersion": "latest",
  "configuration": {
    "url": "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip",
    "script": "configure-http.ps1",
    "function": "WebsiteTest"
  }
}
```

Führen Sie dann diesen Azure CLI-Befehl aus.

```azurecli
az vmss extension set --publisher Microsoft.Powershell --version 2.24 --name DSC --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Verwenden Sie den Schalter `--protected-settings` für Einstellungen, die möglicherweise vertrauliche Informationen enthalten.

### <a name="linux"></a>Linux

Linux kann bei der Erstellung die Erweiterung **Custom Script v2.0** oder **cloud-init** verwenden.

Ein benutzerdefiniertes Skript ist eine einfache Erweiterung, die Dateien in die Instanzen des virtuellen Computers herunterlädt und einen Befehl ausführt.

#### <a name="custom-script"></a>Custom Script

Speichern Sie die folgende JSON-Datei als _settings.json_.

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
  ],
  "commandToExecute": "bash installserver.sh"
}
```

Verwenden Sie die Azure-Befehlszeilenschnittstelle, um diese Erweiterung einer vorhandenen VM-Skalierungsgruppe hinzuzufügen. Jeder virtuelle Computer in der Skalierungsgruppe führt die Erweiterung automatisch aus.

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Verwenden Sie den Schalter `--protected-settings` für Einstellungen, die möglicherweise vertrauliche Informationen enthalten.

#### <a name="cloud-init"></a>cloud-init

cloud-init wird beim Erstellen der Skalierungsgruppe verwendet. Erstellen Sie zunächst eine lokale Datei mit dem Namen _cloud-init.txt_, und fügen Sie ihr die Konfiguration hinzu. Ein Beispiel finden Sie in [dieser Übersicht](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt).

Verwenden Sie die Azure-Befehlszeilenschnittstelle, um eine Skalierungsgruppe zu erstellen. Das Feld `--custom-data` akzeptiert den Dateinamen eines cloud-init-Skripts.

```azurecli
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image Canonical:UbuntuServer:14.04.4-LTS:latest \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

## <a name="how-do-i-manage-application-updates"></a>Wie verwalte ich Anwendungsupdates?

Wenn Sie Ihre Anwendung über eine Erweiterung bereitgestellt haben, ändern Sie die Definition leicht. Diese Änderung bewirkt, dass die Erweiterung für alle Instanzen der virtuellen Computer erneut bereitgestellt wird. Es **muss** ein Element der Erweiterung geändert werden, z.B. indem eine Datei umbenannt wird, andernfalls erkennt Azure nicht, dass die Erweiterung geändert wurde.

Wenn Sie die Anwendung in Ihr eigenes Betriebssystemimage integriert haben, verwenden Sie für Anwendungsupdates eine Pipeline für automatische Bereitstellungen. Entwerfen Sie Ihre Architektur so, dass der schnelle Austausch einer bereitgestellten Skalierungsgruppe in der Produktion erleichtert wird. Ein gutes Beispiel dieses Verfahrens ist die [Azure Spinnaker-Treibermethode](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

[Packer](https://www.packer.io/) und [Terraform](https://www.terraform.io/) unterstützen Azure Resource Manager, sodass Sie Images auch „als Code“ definieren und in Azure erstellen und dann die VHD in der Skalierungsgruppe verwenden können. Dadurch entstehen jedoch Probleme mit Marketplace-Images, in denen Erweiterungen/benutzerdefinierte Skripts wichtiger werden, da Sie Marketplace-Elemente nicht direkt bearbeiten.

## <a name="what-happens-when-a-scale-set-scales-out"></a>Was geschieht, wenn eine Skalierungsgruppe horizontal hochskaliert wird?
Wenn Sie einer Skalierungsgruppe virtuelle Computer hinzufügen, wird die Anwendung automatisch installiert. Wenn z.B. Erweiterungen für die Skalierungsgruppe definiert wurden, werden sie auf einem neuen virtuellen Computer jedes Mal ausgeführt, wenn er erstellt wird. Wenn die Skalierungsgruppe auf einem benutzerdefinierten Image basiert, ist jeder neue virtuelle Computer eine Kopie des benutzerdefinierten Quellimages. Wenn die virtuellen Computer der Skalierungsgruppe Containerhosts sind, können Sie beispielsweise Startcode einrichten, der die Container in einer benutzerdefinierten Skripterweiterung lädt. Oder eine Erweiterung kann einen Agent installieren, der bei einem Clusterorchestrator registriert wird, z.B. bei Azure Container Service.


## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Wie verteilen Sie ein Betriebssystemupdate auf Updatedomänen?
Angenommen, Sie möchten das Betriebssystemimage aktualisieren, während gleichzeitig die VM-Skalierungsgruppe weiterhin ausgeführt wird. PowerShell und die Azure-Befehlszeilenschnittstelle können Images des virtuellen Computers nacheinander aktualisieren. Der Artikel [Upgraden einer VM-Skalierungsgruppe](./virtual-machine-scale-sets-upgrade-scale-set.md) bietet ebenfalls weitere Informationen zu den verfügbaren Optionen zum Ausführen von Betriebssystemupgrades in einer VM-Skalierungsgruppe.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten einer Skalierungsgruppe mithilfe von PowerShell](virtual-machine-scale-sets-windows-manage.md)
* [Erstellen einer Skalierungsgruppenvorlage](virtual-machine-scale-sets-mvss-start.md)


[linux-vm-create]: ../virtual-machines/linux/tutorial-manage-vm.md
[windows-vm-create]: ../virtual-machines/windows/tutorial-manage-vm.md
[linux-vm-capture]: ../virtual-machines/linux/capture-image.md
[windows-vm-capture]: ../virtual-machines/windows/capture-image.md 
[vmss-create]: virtual-machine-scale-sets-create.md


