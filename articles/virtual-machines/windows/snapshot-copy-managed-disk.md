---
title: "Erstellen einer Kopie eines verwalteten Azure-Datenträgers für die Sicherung | Microsoft-Dokumentation"
description: "Erfahren Sie, wie eine Kopie eines verwalteten Azure-Datenträgers für die Sicherung oder Behandlung von Datenträgerproblemen erstellen."
documentationcenter: 
author: cwatsonMSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 2/9/2017
ms.author: cwatson
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: bb913050fd3388d4632e6f75b36415006f370cbd
ms.lasthandoff: 04/27/2017


---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a>Erstellen einer Kopie einer als verwalteter Azure-Datenträger gespeicherten VHD mithilfe verwalteter Momentaufnahmen
Erstellen Sie eine Momentaufnahme eines verwalteten Datenträgers für die Sicherung oder einen verwalteten Datenträger aus der Momentaufnahme, und fügen Sie ihn an einen virtuellen Testcomputer an, um Probleme zu behandeln. Eine verwaltete Momentaufnahme ist eine vollständige zeitpunktabhängige Kopie eines verwalteten VM-Datenträgers. Standardmäßig wird eine schreibgeschützte Kopie Ihrer VHD erstellt, die als verwalteter Standard-Datenträger gespeichert wird. Weitere Informationen zu verwalteten Datenträgern finden Sie in der [Übersicht über Managed Disks](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Informationen zu Preisen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/managed-disks/). 

## <a name="before-you-begin"></a>Voraussetzungen
Wenn Sie PowerShell verwenden, vergewissern Sie sich, dass Sie die neueste Version des AzureRM.Compute-PowerShell-Moduls verwenden. Führen Sie den folgenden Befehl aus, um es zu installieren.

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Weitere Informationen finden Sie unter [Azure PowerShell-Versionsverwaltung](/powershell/azure/overview).

## <a name="copy-the-vhd-with-a-snapshot"></a>Kopieren der VHD mithilfe einer Momentaufnahme
Eine Momentaufnahme des verwalteten Datenträgers können Sie entweder im Azure-Portal oder mit PowerShell erstellen.

### <a name="use-azure-portal-to-take-a-snapshot"></a>Erstellen einer Momentaufnahme im Azure-Portal 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie zunächst links oben auf **Neu**, und suchen Sie nach **Momentaufnahme**.
3. Klicken Sie auf dem Blatt „Momentaufnahme“ auf **Erstellen**.
4. Geben Sie einen **Namen** für die Momentaufnahme ein.
5. Wählen Sie eine vorhandene [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md#resource-groups) aus, oder geben Sie den Namen für eine neue Ressourcengruppe ein. 
6. Wählen Sie den Standort eines Azure-Rechenzentrums aus.  
7. Wählen Sie für **Quelldatenträger** den verwalteten Datenträger aus, für den eine Momentaufnahme erstellt werden soll.
8. Wählen Sie den **Kontotyp** aus, der zum Speichern der Momentaufnahme verwendet werden soll. Wir empfehlen **Standard_LRS**, es sei denn, Sie benötigen eine Speicherung auf einem Hochleistungsdatenträger.
9. Klicken Sie auf **Erstellen**.

### <a name="use-powershell-to-take-a-snapshot"></a>Erstellen einer Momentaufnahme mit PowerShell
Die folgenden Schritte veranschaulichen das Abrufen des zu kopierenden VHD-Datenträgers, das Erstellen der Momentaufnahmenkonfigurationen und das Erstellen einer Momentaufnahme mithilfe des Cmdlets „New-AzureRmSnapshot“<!--Add link to cmdlet when available-->. 

1. Legen Sie einige Parameter fest. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'southeastasia' 
$dataDiskName = 'ContosoMD_datadisk1' 
$snapshotName = 'ContosoMD_datadisk1_snapshot1'  
```
  Ersetzen Sie die folgenden Parameterwerte:
  -  „myResourceGroup“ durch die Ressourcengruppe der VM.
  -  „southeastasia“ durch den geografischen Standort, an dem die verwaltete Momentaufnahme gespeichert werden soll. <!---How do you look these up? -->
  -  „ContosoMD_datadisk1“ durch den Namen des VHD-Datenträgers, den Sie kopieren möchten.
  -  „ContosoMD_datadisk1_snapshot1“ durch den Namen, den Sie für die neue Momentaufnahme verwenden möchten.

2. Rufen Sie den VHD-Datenträger ab, der kopiert werden soll.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. Erstellen Sie die Momentaufnahmenkonfigurationen. 

 ```powershell
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. Erstellen Sie die Momentaufnahme.

 ```powershell
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
Wenn Sie vorhaben, die Momentaufnahme zum Erstellen eines verwalteten Datenträgers zu nutzen und sie an eine VM anzufügen, die hohe Leistung benötigt, verwenden Sie mit dem Cmdlet „New-AzureRmSnapshot“ den Parameter `-AccountType Premium_LRS`. Der Parameter erstellt die Momentaufnahme so, dass sie als verwalteter Premium-Datenträger gespeichert wird. Verwaltete Premium-Datenträger sind teurer als die Standardvariante. Vergewissern Sie sich daher, dass Sie Premium benötigen, ehe Sie diesen Parameter angeben.



