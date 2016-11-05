---
title: Informationen zu Images für virtuelle Windows-Computer | Microsoft Docs
description: Informationen zur Nutzung von Images mit virtuellen Windows-Computern in Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management

ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/21/2016
ms.author: cynthn

---
# Informationen zu Images für virtuelle Windows-Computer
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[!INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]

## Arbeiten mit Images
Das Azure PowerShell-Modul dient zum Verwalten der Images, die in Ihrem Azure-Abonnement zur Verfügung stehen. Sie können für einige Aufgaben für Images auch das klassische Azure-Portal nutzen, doch die Befehlszeile bieten Ihnen mehr Optionen.

* **Alle Images abrufen**: `Get-AzureVMImage` gibt eine Liste aller Images in Ihrem aktuellen Abonnement zurück, und zwar sowohl Ihre Images als auch von Azure oder Partnern bereitgestellte Images. Dies bedeutet, dass die Liste umfangreich sein kann. Die nächsten Beispiele zeigen, wie eine kürzere Liste abgerufen wird.
* **Image-Familien abrufen**: `Get-AzureVMImage | select ImageFamily` ruft eine Liste der Image-Familien ab, indem Zeichenfolgen mit der **ImageFamily**-Eigenschaft angezeigt werden.
* **Alle Images in einer bestimmten Familie abrufen**: `Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
* **VM-Images suchen**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName`. Hierbei erfolgt eine Filterung nach der „DataDiskConfiguration“-Eigenschaft, die nur für VM-Images gilt. Bei diesem Beispiel wird die Ausgabe auch nur nach der Bezeichnung und dem Namen des Images gefiltert.
* **Ein verallgemeinertes Image speichern**: `Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
* **Ein spezialisiertes Image speichern**: `Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
  >[Azure.Tip] Der "OSState"-Parameter ist erforderlich, wenn Sie ein VM-Image erstellen möchten, das neben dem Betriebssystem-Datenträger auch andere Datenträger enthält. Wenn Sie den Parameter nicht verwenden, erstellt das Cmdlet ein Betriebssystem-Image. Der Wert des Parameters gibt an, ob das Image verallgemeinert oder spezialisiert ist, was davon abhängt, ob der Betriebssystem-Datenträger auf die erneute Verwendung vorbereitet wurde.
* **Ein Image löschen**: `Remove-AzureVMImage –ImageName "MyOldVmImage"`

## Nächste Schritte
Sie können auch [einen Windows-Computer mit dem klassischen Portal erstellen](virtual-machines-windows-classic-tutorial.md).

<!---HONumber=AcomDC_0727_2016-->