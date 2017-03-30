---
title: Verwalten Ihrer virtuellen Computer mit Azure PowerShell | Microsoft Docs
description: "Lernen Sie Befehle kennen, mit denen Sie Aufgaben bei der Verwaltung Ihrer virtuellen Computer verwenden können."
services: virtual-machines-windows
documentationcenter: windows
author: singhkays
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 7cdf9bd3-6578-4069-8a95-e8585f04a394
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2016
ms.author: kasing
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: 44235e32b7cf5b2be90f6d460ac22d7c0350f894
ms.lasthandoff: 03/27/2017


---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Verwalten Ihrer virtuellen Computer mithilfe von Azure PowerShell
> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Informationen zu allgemeinen PowerShell-Befehlen im Resource Manager-Modell finden Sie [hier](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Viele Aufgaben, die Sie jeden Tag zur Verwaltung der virtuellen Computer ausführen, können mithilfe von Azure PowerShell-Cmdlets automatisiert werden. In diesem Artikel finden Sie Beispiele für Befehle für einfachere Aufgaben und Links zu Artikeln, die die Befehle für komplexere Aufgaben behandeln.

> [!NOTE]
> Wenn Sie Azure PowerShell noch nicht installiert und konfiguriert haben, erhalten Sie die Anweisungen dazu im Artikel [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).
> 
> 

## <a name="how-to-use-the-example-commands"></a>So verwenden Sie die Beispielbefehle
Sie müssen einen Teil des Texts in den Befehlen durch Text ersetzen, der für Ihre Umgebung geeignet ist. Die Symbole < und > geben Text an, der ersetzt werden muss. Wenn Sie den Text ersetzen, entfernen Sie die Symbole, behalten jedoch die Anführungszeichen bei.

## <a name="get-a-vm"></a>Abrufen eines virtuellen Computers
Dies ist eine einfache Aufgabe, die Sie häufig verwenden. Verwenden Sie sie zum Abrufen von Informationen zu einem virtuellen Computer, Ausführen von Aufgaben auf diesem oder Abrufen von Ausgaben zur Speicherung in einer Variablen.

Um Informationen über den virtuellen Computer zu erhalten, führen Sie diesen Befehl aus und ersetzen den Text in Anführungszeichen, einschließlich der Zeichen < und >:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Führen Sie zum Speichern der Ausgabe in eine $vm-Variable folgenden Code aus:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Anmelden bei einem virtuellen Computer auf Windows-Basis
Führen Sie diese Befehle aus.

> [!NOTE]
> Sie erhalten den Namen des virtuellen Computers und Clouddiensts über die Anzeige des **Get-AzureVM** Befehls.
> 
> $svcName = "<cloud service name>" $vmName = "<virtual machine name>" $localPath = "<Laufwerk und Ordnerpfad zum Speichern der heruntergeladenen RDP-Datei. Beispiel: c:\temp>" $localFile = $localPath + "\" + $vmname + ".rdp" Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch
> 
> 

## <a name="stop-a-vm"></a>Anhalten eines virtuellen Computers
Führen Sie den folgenden Befehl aus:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> Verwenden Sie diesen Parameter, um die virtuelle IP-Adresse des Clouddiensts beizubehalten, falls es sich um den letzten virtuellen Computer in diesem Clouddienst handelt. <br><br> Wenn Sie diesen Parameter verwenden, wird der virtuelle Computer dennoch in Rechnung gestellt.
> 
> 

## <a name="start-a-vm"></a>Starten eines virtuellen Computers
Führen Sie den folgenden Befehl aus:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Datenträger anfügen
Diese Aufgabe erfordert einige Schritte. Zunächst verwenden Sie das Cmdlet ****Add-AzureDataDisk**** zum Hinzufügen des Datenträgers zum $vm-Objekt. Dann verwenden Sie das Cmdlet **Update-AzureVM** , um die Konfiguration des virtuellen Computers zu aktualisieren.

Sie müssen auch entscheiden, ob Sie einen neuen Datenträger anfügen oder einen, der Daten enthält. Für einen neuen Datenträger erstellt der Befehl die VHD-Datei und fügt sie an.

Um einen neuen Datenträger anzufügen, führen Sie den folgenden Befehl aus:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Um einen vorhandenen Datenträger anzufügen, führen Sie den folgenden Befehl aus:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Führen Sie den folgenden Befehl zum Anfügen von Datenträgern aus einer vorhandenen VHD-Datei im Blob-Speicher aus:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Erstellen eines virtuellen Computers auf Windows-Basis
Zum Erstellen eines neuen auf Windows basierenden virtuellen Computers in Azure folgen Sie den Anweisungen unter [Verwenden von Azure PowerShell zum Erstellen und Vorabkonfigurieren Windows-basierter virtueller Computer](create-powershell.md). In diesem Thema wird schrittweise erläutert, wie Sie einen Azure PowerShell-Befehlssatz erstellen, mit dem Sie einen virtuellen Windows-Computer generieren, der mit Folgendem vorkonfiguriert werden kann:

* Mit einer Mitgliedschaft in einer Active Directory-Domäne.
* Mit zusätzlichen Datenträgern.
* Als Mitglied in einer vorhandenen Lastenausgleichsgruppe.
* Mit einer statischen IP-Adresse.


