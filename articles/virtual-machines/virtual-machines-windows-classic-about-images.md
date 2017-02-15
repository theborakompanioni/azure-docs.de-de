---
title: "Informationen zu Images für virtuelle Windows-Computer | Microsoft Docs"
description: Informationen zur Nutzung von Images mit virtuellen Windows-Computern in Azure.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 66ff3fab-8e7f-4dff-b8da-ab1c9c9c9af8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/21/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: c118f9ebb8da496ad33b6e24e80d16ccf6557928


---
# <a name="about-images-for-windows-virtual-machines"></a>Informationen zu Images für virtuelle Windows-Computer
> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Informationen zum Suchen und Verwenden von Images im Resource Manager-Modell finden Sie [hier](virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a>Arbeiten mit Images
Das Azure PowerShell-Modul dient zum Verwalten der Images, die in Ihrem Azure-Abonnement zur Verfügung stehen. Sie können für einige Aufgaben für Images auch das klassische Azure-Portal nutzen, doch die Befehlszeile bieten Ihnen mehr Optionen.

* **Abrufen aller Images**: `Get-AzureVMImage` gibt eine Liste aller in Ihrem aktuellen Abonnement verfügbaren Images zurück, und zwar sowohl Ihre eigenen Images als auch die von Azure oder Partnern bereitgestellten Images. Dies bedeutet, dass die Liste umfangreich sein kann. Die nächsten Beispiele zeigen, wie eine kürzere Liste abgerufen wird.
* **Abrufen von Imagefamilien**: `Get-AzureVMImage | select ImageFamily` ruft eine Liste der Imagefamilien ab, indem Zeichenfolgen mit der **ImageFamily**-Eigenschaft angezeigt werden.
* **Abrufen aller Images in einer bestimmten Familie **: `Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
* **Suchen nach VM-Images**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName`. Hierbei erfolgt eine Filterung nach der DataDiskConfiguration-Eigenschaft, die nur für VM-Images gilt. Bei diesem Beispiel wird die Ausgabe auch nur nach der Bezeichnung und dem Namen des Images gefiltert.
* **Speichern eines generalisierten Images**: `Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
* **Speichern eines spezialisierten Images**: `Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
  >[Azure.Tip] Der "OSState"-Parameter ist erforderlich, wenn Sie ein VM-Image erstellen möchten, das neben dem Betriebssystem-Datenträger auch andere Datenträger enthält. Wenn Sie den Parameter nicht verwenden, erstellt das Cmdlet ein Betriebssystem-Image. Der Wert des Parameters gibt an, ob das Image verallgemeinert oder spezialisiert ist, was davon abhängt, ob der Betriebssystem-Datenträger auf die erneute Verwendung vorbereitet wurde.
* **Löschen eines Images**: `Remove-AzureVMImage –ImageName "MyOldVmImage"`

## <a name="next-steps"></a>Nächste Schritte
Sie können auch [einen Windows-Computer mit dem klassischen Portal erstellen](virtual-machines-windows-classic-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).




<!--HONumber=Feb17_HO2-->


