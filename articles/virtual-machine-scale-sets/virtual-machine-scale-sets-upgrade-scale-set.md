<properties
	pageTitle="Bereitstellen einer App in Skalierungsgruppen für virtuelle Computer | Microsoft Azure"
	description="Bereitstellen einer App in Skalierungsgruppen für virtuelle Computer"
	services="virtual-machine-scale-sets"
	documentationCenter=""
	authors="gbowerman"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machine-scale-sets"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/13/2016"
	ms.author="guybo"/>


# Aktualisieren einer VM-Skalierungsgruppe

Dieser Artikel beschreibt, wie Sie ohne Ausfallzeiten ein Betriebssystemupdate für eine Azure VM-Skalierungsgruppe einführen können. In diesem Kontext bedeutet ein Betriebssystemupdate, dass entweder die Version oder die SKU des Betriebssystems oder der URI eines benutzerdefinierten Images geändert wird. Ein Update ohne Ausfallzeiten heißt, dass virtuelle Computer nicht alle gleichzeitig, sondern einzeln oder in Gruppen aktualisiert werden (beispielsweise durch Aktualisieren einer Fehlerdomäne nach der anderen). Auf diese Weise können virtuelle Computer, die gerade nicht aktualisiert werden, weiter ausgeführt werden.

Um Missverständnissen vorzubeugen, unterscheiden wir drei Arten von Betriebssystemupdates, die Sie ausführen können:

1. Sie ändern die Version oder SKU eines Plattformimages. Hierunter fällt z.B. die Änderung der Ubuntu 14.04.2-LTS-Version von 14.04.201506100 zu 14.04.201507060 oder die Änderung der neuesten SKU von Ubuntu 15.10 zur neuesten SKU von 16.04.0-LTS. Dieses Szenario wird in diesem Artikel beschrieben.

2. Sie erstellen eine neue Version eines benutzerdefinierten Images und möchten den URI ändern, der auf das Image zeigt (properties->virtualMachineProfile->storageProfile->osDisk->image->uri). Dieses Szenario wird in diesem Artikel beschrieben.

3. Sie patchen das Betriebssystem aus einem virtuellen Computer heraus (z.B. indem Sie ein Sicherheitspatch installieren, die Windows-Updatefunktion verwenden oder ähnliches). Dieses Szenario wird unterstützt, in diesem Artikel jedoch nicht behandelt.

Die ersten beiden Szenarien sind unterstützte Anforderungen. Für das dritte müssen Sie (zumindest im Moment) eine neue Skalierungsgruppe erstellen. Dieser Artikel beschreibt die Optionen 1 und 2. Hinweis: VM-Skalierungsgruppen, die im Rahmen eines [Service Fabric-Clusters](https://azure.microsoft.com/services/service-fabric/) bereitgestellt werden, werden in diesem Artikel nicht behandelt.

Bei der Änderung einer Betriebssystemversion bzw. einer Betriebssystem-SKU oder des URI eines benutzerdefinierten Images gilt folgender grundlegender Ablauf:

* Rufen Sie das VMSS-Modell (VM Scale Set, VM-Skalierungsgruppe) ab.

* Ändern Sie den Wert für Version, SKU oder URI im Modell.

* Aktualisieren Sie das Modell.

* Führen Sie einen manualUpgrade-Aufruf für die virtuellen Computer in der Skalierungsgruppe aus. Dieser Schritt ist nur relevant, wenn die upgradePolicy-Eigenschaft Ihrer Skalierungsgruppe auf „Manuell“ festgelegt ist. Wenn die Eigenschaft auf „Automatisch“ festgelegt ist, werden alle virtuellen Computer gleichzeitig aktualisiert, was zu Ausfallzeiten führt.


Behalten Sie diese Hintergrundinformationen im Kopf. Wir schauen uns jetzt an, wie Sie die Version einer Skalierungsgruppe in PowerShell und unter Verwendung der REST-API aktualisieren können. Diese Beispiele gelten für Plattformimages. Wir haben Ihnen jedoch hoffentlich genügend Informationen bereitgestellt, dass Sie diesen Vorgang auch für ein benutzerdefiniertes Image anpassen können.

## PowerShell

Mit diesem Beispiel wird eine Windows-VM-Skalierungsgruppe auf die neue Version 4.0.20160229 aktualisiert. Nach der Aktualisierung des Modells aktualisiert das Modell einen virtuellen Computer nach dem anderen.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the VMSS model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Wenn Sie den URI für ein benutzerdefiniertes Image aktualisieren, anstatt eine Plattformimageversion zu ändern, ersetzen Sie die Zeile „set the new version...“ durch eine Zeile ähnlich der folgenden:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```


## REST-API

Hier finden Sie einige Python-Beispiele, in denen die Azure-REST-API verwendet wird, um eine Betriebssystemversion zu aktualisieren. Beide verwenden die schlanke [azurerm-Bibliothek](https://pypi.python.org/pypi/azurerm) der Azure-REST-API-Funktionen, um einen GET-Befehl für das Skalierungsgruppenmodell und dann einen PUT-Befehl mit einem aktualisierten Modell auszuführen. Die Beispiele beziehen auch VM-Instanzsichten ein, um die virtuellen Computer gemäß ihrer Updatedomäne zu identifizieren.

### vmssupgrade

vmssupgrade ist ein Python-Skript zur Einführung eines Betriebssystemupgrades in eine ausgeführte VM-Skalierungsgruppe, wobei eine Updatedomäne nach der anderen aktualisiert wird. Sie finden das Skript [hier](https://github.com/gbowerman/vmsstools).

![vmssupgrade – Screenshot](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Mit diesem Skript können Sie bestimmte virtuelle Computer für die Aktualisierung auswählen oder eine Updatedomäne angeben. Das Skript unterstützt auch die Änderung einer Plattformimageversion ODER die Änderung des URI eines benutzerdefinierten Images.

### vmsseditor

Dieses Tool ist ein allgemeiner Editor für VM-Skalierungsgruppen, der den VM-Status als Heatmap anzeigt, in der eine Zeile eine Updatedomäne darstellt. Sie können u.a. das Modell für eine VM-Skalierungsgruppe mit einer neuen Version, einer neuen SKU oder einem neuen benutzerdefinierten Image-URI aktualisieren und dann Fehlerdomänen für die Aktualisierung auswählen. In diesem Fall werden alle virtuellen Computer in dieser Updatedomäne auf das neue Modell aktualisiert. Alternativ dazu könnten Sie ein paralleles Upgrade basierend auf der Batchgröße Ihrer Wahl ausführen. Sie finden den vmsseditor in diesem [GitHub-Repository](https://github.com/gbowerman/vmssdashboard).

In diesem Beispiel habe ich das Modell einer Skalierungsgruppe auf die Ubuntu 14.04-2LTS-Version 14.04.201507060 aktualisiert. Beachten Sie, dass dieser Screenshot veraltet ist – in der Zwischenzeit wurden dem Tool viele weitere Optionen hinzugefügt.

![vmsseditor – Screenshot 1](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Nachdem Sie erneut auf „Upgrade“ und „Details abrufen“ geklickt haben, beginnt die Aktualisierung der virtuellen Computer in UD 0.

![vmsseditor – Screenshot 2](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

<!---HONumber=AcomDC_0921_2016-->