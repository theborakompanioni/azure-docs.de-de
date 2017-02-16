---
title: Erstellen eines VM-Images aus einem virtuellen Azure-Computer | Microsoft Docs
description: Es wird beschrieben, wie Sie ein generalisiertes VM-Image aus einer vorhandenen Azure-VM erstellen, die auf dem Resource Manager-Bereitstellungsmodell basiert.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 51ef4f51-0942-4249-afea-4a3f87ce1ff8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 349b8dc78ace79efca771efb78e453a47e51023e
ms.openlocfilehash: 6e80064adadcc542523acb1d2478e6b8f0163acb


---
# <a name="download-the-template-for-a-vm"></a>Herunterladen einer Vorlage für einen virtuellen Computer
Wenn Sie über das Portal oder mithilfe von PowerShell einen virtuellen Computer in Azure erstellen, wird automatisch eine Resource Manager-Vorlage für Sie erstellt. Sie können diese Vorlage verwenden, um eine Bereitstellung schnell zu duplizieren. Die Vorlage enthält Informationen über alle Ressourcen in einer Ressourcengruppe. Bei virtuellen Computern bedeutet dies, dass die Vorlage alle Elemente enthält, die zur Unterstützung des virtuellen Computers in dieser Ressourcengruppe erstellt wurden – einschließlich der Netzwerkressourcen.

## <a name="download-the-template-using-the-portal"></a>Herunterladen der Vorlage über das Portal
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Hubmenü die Option **Virtuelle Computer** aus.
3. Wählen Sie den gewünschten virtuellen Computer aus der Liste aus.
4. Wählen Sie **Automatisierungsskript** aus.
5. Wählen Sie **Herunterladen**, und speichern Sie die ZIP-Datei auf Ihrem lokalen Computer.
6. Öffnen Sie die ZIP-Datei, und extrahieren Sie die Dateien in einen Ordner. Die ZIP-Datei enthält Folgendes:
   
   * deploy.ps1
   * deploy.sh 
   * deployer.rb
   * DeploymentHelper.cs
   * parameters.json
   * template.json

Die Datei „template.json“ ist die Vorlage.

## <a name="download-the-template-using-powershell"></a>Herunterladen der Vorlage mithilfe von PowerShell
Sie können die JSON-Vorlagendatei auch mithilfe des Cmdlets [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) herunterladen. Sie können den Parameter `-path` verwenden, um den Dateinamen und den Pfad für die JSON-Datei anzugeben. Dieses Beispiel zeigt, wie Sie die Vorlage für die Ressourcengruppe mit dem Namen **myResourceGroup** in den Ordner **C:\users\public\downloads** auf Ihrem lokalen Computer herunterladen.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Bereitstellen von Ressourcen mithilfe von Vorlagen finden Sie unter [Resource Manager-Vorlage – exemplarische Vorgehensweise](../resource-manager-template-walkthrough.md).




<!--HONumber=Nov16_HO4-->


