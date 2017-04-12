---
title: "Ändern der Größe eines virtuellen Windows-Computers im klassischen Bereitstellungsmodell – Azure | Microsoft-Dokumentation"
description: "Ändern der Größe eines virtuellen Windows-Computers, der im klassischen Bereitstellungsmodell erstellt wurde, mithilfe von Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e3038215-001c-406e-904d-e0f4e326a4c7
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 4277bc8394c7ba140291e9dc776162e87deab96b
ms.lasthandoff: 03/31/2017


---
# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>Ändern der Größe eines virtuellen Windows-Computers, der im klassischen Bereitstellungsmodell erstellt wurde
In diesem Artikel erfahren Sie, wie die Größe eines virtuellen Windows-Computers, der im klassischen Bereitstellungsmodell erstellt wurde, mithilfe von Azure Powershell geändert wird.

Wenn Sie die Möglichkeit zum Ändern der Größe eines virtuellen Computers in Betracht ziehen, gibt es zwei Konzepte, die den Bereich der verfügbaren Größe für den virtuellen Computer bestimmen. Das erste Konzept ist die Region, in der Ihr virtueller Computer bereitgestellt wird. Die Liste der Größen virtueller Computer, die in der Region verfügbar sind, finden Sie auf der Azure-Regionen Webseite auf der Registerkarte Dienste. Das zweite Konzept ist die physische Hardware, die derzeit den virtuellen Computer hostet. Die physischen Server, die virtuelle Computer hosten, werden in Clustern gemeinsamer physischer Hardware gruppiert. Die Methode zum Ändern der Größe eines virtuellen Computers hängt davon ab, ob die gewünschte neue Größe des virtuellen Computers von dem Hardwarecluster unterstützt wird, das derzeit den virtuellen Computer hostet.

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Sie können auch die [Größe eines virtuellen Computers ändern, der im Resource Manager-Bereitstellungsmodell erstellt wurde](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="add-your-account"></a>Fügen Sie Ihr Konto hinzu
Sie müssen Azure PowerShell zum Arbeiten mit klassischen Azure-Ressourcen konfigurieren. Gehen Sie folgendermaßen vor, Azure PowerShell zur Verwaltung der klassischen Ressourcen zu konfigurieren.

1. Geben Sie an der PowerShell-Eingabeaufforderung `Add-AzureAccount` ein, und drücken Sie die **EINGABETASTE**. 
2. Geben Sie die mit Ihrem Azure-Abonnement verknüpfte E-Mail-Adresse ein, und klicken Sie auf **Weiter**. 
3. Geben Sie das Kennwort für Ihr Konto ein. 
4. Klicken Sie auf **Anmelden**. 

## <a name="resize-in-the-same-hardware-cluster"></a>Ändern der Größe im gleichen Hardwarecluster
Führen Sie die folgenden Schritte zum Ändern der Größe eines virtuellen Computers auf eine im Hardwarecluster, das den virtuellen Computer hostet, verfügbare Größe durch.

1. Führen Sie den folgenden PowerShell-Befehl zum Auflisten der Größen virtueller Computer auf, die im Hardwarecluster verfügbar sind, das den Clouddienst hostet, der den virtuellen Computer enthält.
   
    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```
2. Führen Sie die folgenden Befehle zum Ändern der Größe des virtuellen Computers aus.
   
    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Ändern der Größe in einem neuen Hardwarecluster
Zum Ändern der Größe eines virtuellen Computers auf eine Größe, die nicht im Hardwarecluster verfügbar ist, das den Clouddienst hostet, der den virtuellen Computer enthält, müssen der Clouddienst und alle virtuellen Computer neu erstellt werden. Jeder Clouddienst wird auf einem einzelnen Hardwarecluster gehostet, daher müssen alle virtuellen Computer im Clouddienst eine Größe aufweisen, die in einem Hardwarecluster unterstützt wird. Die folgenden Schritte beschreiben, wie die Größe eines virtuellen Computers geändert wird, indem der Clouddienst gelöscht und neu erstellt wird.

1. Führen Sie den folgenden PowerShell-Befehl zum Auflisten der verfügbaren Größen virtueller Computer in der Region aus. 
   
    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```
2. Notieren Sie alle Konfigurationseinstellungen für die einzelnen virtuellen Computer in dem Clouddienst, der den virtuellen Computer enthält, der geändert werden soll. 
3. Löschen Sie alle virtuellen Computer in dem Clouddienst mit ausgewählter Option, die Datenträger für jeden virtuellen Computer beizubehalten.
4. Erstellen Sie den virtuellen Computer, dessen Größe geändert werden soll, mit der gewünschten Größe neu.
5. Erstellen Sie alle anderen virtuellen Computer neu, die in dem Clouddienst waren, mit einer Größe der virtuellen Computer, die im Hardwarecluster verfügbar ist, das jetzt den Clouddienst hostet.

Ein Beispielskript zum Löschen und Neuerstellen eines Clouddiensts mit einer neuen Größe des virtuellen Computers finden Sie [hier](https://github.com/Azure/azure-vm-scripts). 

## <a name="next-steps"></a>Nächste Schritte
* [Ändern der Größe eines virtuellen Computers, der in dem Ressourcen-Manager-Bereitstellungsmodell erstellt wurde](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


