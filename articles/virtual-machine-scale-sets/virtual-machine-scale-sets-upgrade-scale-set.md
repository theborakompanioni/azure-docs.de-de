---
title: Aktualisieren einer Azure-VM-Skalierungsgruppe | Microsoft-Dokumentation
description: Aktualisieren einer Azure-VM-Skalierungsgruppe
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: guybo
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: bbc04cfb1145f3be2957d11f2ed6253428c4b9c3
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="upgrade-a-virtual-machine-scale-set"></a>Upgraden einer VM-Skalierungsgruppe
In diesem Artikel erfahren Sie, wie Sie ohne Ausfallzeiten ein Rollout eines Betriebssystemupdates für eine Skalierungsgruppe mit virtuellen Azure-Computern durchführen. In diesem Kontext wird bei einem Betriebssystemupdate entweder die Version oder die SKU des Betriebssystems oder der URI eines benutzerdefinierten Images geändert. Ein Update ohne Ausfallzeiten heißt, dass virtuelle Computer nicht alle gleichzeitig, sondern einzeln oder gruppenweise aktualisiert werden (beispielsweise eine Fehlerdomäne nach der anderen). Dadurch können virtuelle Computer, die gerade nicht aktualisiert werden, weiter ausgeführt werden.

Sie können drei Arten von Betriebssystemupdates ausführen:

* Ändern die Version oder SKU eines Plattformimages. Hierzu zählt beispielsweise das Ändern der Ubuntu 14.04.2-LTS-Version von 14.04.201506100 in 14.04.201507060 oder das Ändern der SKU „Ubuntu 15.10/latest“ in „16.04.0-LTS/latest“. Dieses Szenario wird in diesem Artikel beschrieben.
* Ändern des URIs, der auf eine neue Version eines benutzerdefinierten, von Ihnen erstellten Images verweist (**Eigenschaften** > **virtualMachineProfile** (Profil des virtuellen Computers) > **storageProfile** (Speicherprofil) > **Betriebssystemdatenträger** > **Image** > **URI**). Dieses Szenario wird in diesem Artikel beschrieben.
* Patchen des Betriebssystems über einen virtuellen Computer. (Beispiele hierfür wären etwa das Installieren eines Sicherheitspatches und das Ausführen von Windows Update.) Dieses Szenario wird unterstützt, in diesem Artikel jedoch nicht behandelt.

Die ersten beiden Optionen sind unterstützte Anforderungen und werden in diesem Artikel behandelt. Zum Ausführen der dritten Option müssen Sie eine neue Skalierungsgruppe erstellen.

VM-Skalierungsgruppen, die im Rahmen eines [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) -Clusters bereitgestellt werden, werden hier nicht behandelt.

Die grundlegende Vorgehensweise zum Ändern der Betriebssystemversion/SKU eines Plattformimages oder des URIs eines benutzerdefinierten Images umfasst folgende Schritte:

1. Rufen Sie das Modell der VM-Skalierungsgruppe ab.
2. Ändern Sie im Modell den Wert für Version, SKU oder URI.
3. Aktualisieren Sie das Modell.
4. Führen Sie einen *manualUpgrade* -Aufruf für die virtuellen Computer in der Skalierungsgruppe aus. Dieser Schritt ist nur relevant, wenn *upgradePolicy* in Ihrer Skalierungsgruppe auf **Manuell** festgelegt ist. Im automatischen Modus ****werden alle virtuellen Computer gleichzeitig aktualisiert, was zu Ausfallzeiten führt.

Behalten Sie diese Hintergrundinformationen im Kopf. Wir sehen uns nun an, wie Sie die Version einer Skalierungsgruppe in PowerShell und unter Verwendung der REST-API aktualisieren können. Diese Beispiele gelten zwar für Plattformimages, anhand der Informationen in diesem Artikel können Sie den Vorgang jedoch auch für ein benutzerdefiniertes Image anpassen.

## <a name="powershell"></a>PowerShell
Dieses Beispiel aktualisiert eine Skalierungsgruppe mit virtuellen Windows-Computern auf die neue Version 4.0.20160229. Mit dem aktualisierten Modell werden die einzelnen Instanzen der virtuellen Computer nacheinander aktualisiert.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Wenn Sie keine Plattformimageversion ändern, sondern den URI für ein benutzerdefiniertes Image aktualisieren möchten, ersetzen Sie die Zeile „set the new version...“ durch eine Zeile wie die folgende:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```


## <a name="the-rest-api"></a>REST-API
Hier finden Sie einige Python-Beispiele, in denen die Azure-REST-API verwendet wird, um eine Betriebssystemversion zu aktualisieren. Beide verwenden die einfache [azurerm-Bibliothek](https://pypi.python.org/pypi/azurerm) mit Azure-REST-API-Wrapper-Funktionen, um einen GET-Befehl für das Skalierungsgruppenmodell und anschließend einen PUT-Befehl mit einem aktualisierten Modell auszuführen. Die Beispiele berücksichtigen auch Ansichten für Instanzen virtueller Computer, um die virtuellen Computer anhand ihrer Updatedomäne zu identifizieren.

### <a name="vmssupgrade"></a>Vmssupgrade
 [Vmssupgrade](https://github.com/gbowerman/vmsstools) ist ein Python-Skript zum Durchführen eines Betriebssystemupgrade-Rollouts in einer aktiven VM-Skalierungsgruppe, bei dem jeweils eine Updatedomäne nach der anderen aktualisiert wird.

![Vmssupgrade-Skript zum Auswählen von virtuellen Computern oder einer Updatedomäne](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Bei diesem Skript können Sie bestimmte virtuelle Computer für die Aktualisierung auswählen oder eine Updatedomäne angeben. Es unterstützt das Ändern einer Plattformimageversion sowie das Ändern des URIs eines benutzerdefinierten Images.

### <a name="vmsseditor"></a>Vmsseditor
[Vmsseditor](https://github.com/gbowerman/vmssdashboard) ist ein vielseitiger Editor für VM-Skalierungsgruppen, der den Status virtueller Computer als Heatmap anzeigt, in der eine Zeile jeweils eine Updatedomäne darstellt. Sie können unter anderem das Modell für eine VM-Skalierungsgruppe mit einer neuen Version, einer neuen SKU oder einem neuen URI eines benutzerdefinierten Images aktualisieren und dann Fehlerdomänen für das Upgrade auswählen. Dadurch wird für alle virtuellen Computer in dieser Updatedomäne ein Upgrade auf das neue Modell durchgeführt. Alternativ können Sie ein paralleles Upgrade durchführen, das auf einer Batchgröße Ihrer Wahl basiert.  

Der folgende Screenshot zeigt ein Modell einer Skalierungsgruppe der Ubuntu 14.04-2LTS-Version 14.04.201507060. Das Tool verfügt inzwischen über zahlreiche weitere Optionen, die zum Zeitpunkt der Screenshoterstellung noch nicht vorhanden waren.

![Vmsseditor-Modell einer Skalierungsgruppe für Ubuntu 14.04-2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Nach dem Klicken auf **Upgrade** > **Get Details** (Details abrufen) werden die virtuellen Computer in „UD 0“ aktualisiert.

![Vmsseditor mit einer aktiven Aktualisierung](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)


