---
title: Erstellen einer Windows-VM mithilfe einer Vorlage in Azure | Microsoft Docs
description: Verwenden Sie eine Resource Manager-Vorlage und PowerShell, um ganz einfach einen neuen virtuellen Windows-Computer zu erstellen.
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 19129d61-8c04-4aa9-a01f-361a09466805
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: davidmu
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: acb8dda8819099595893f2ea65b23e76223507bd
ms.lasthandoff: 03/31/2017


---

# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a>Erstellen eines virtuellen Windows-Computer mit einer Resource Manager-Vorlage

In diesem Artikel wird gezeigt, wie Sie eine Azure Resource Manager-Vorlage mithilfe von PowerShell bereitstellen. Mit dieser [Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json) wird ein einzelner virtueller Computer mit Windows Server in einem neuen virtuellen Netzwerk mit einem einzelnen Subnetz bereitgestellt.

Eine ausführliche Beschreibung der Ressource des virtuellen Computers finden Sie unter [Virtuelle Computer in einer Azure Resource Manager-Vorlage](template-description.md). Weitere Informationen zu allen Ressourcen in einer Vorlage finden Sie unter [Exemplarische Vorgehensweise zu Azure Resource Manager-Vorlagen](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Das Ausführen der Schritte in diesem Artikel dauert ungefähr fünf Minuten.

## <a name="step-1-install-azure-powershell"></a>Schritt 1: Installieren von Azure PowerShell

Unter [Installieren und Konfigurieren von Azure PowerShell](../../powershell-install-configure.md) erfahren Sie, wie Sie die neueste Version von Azure PowerShell installieren, Ihr Abonnement auswählen und sich bei Ihrem Konto anmelden.

## <a name="step-2-create-a-resource-group"></a>Schritt 2: Erstellen einer Ressourcengruppe

Alle Ressourcen müssen in einer [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md) bereitgestellt werden.

1. Rufen Sie eine Liste mit den verfügbaren Standorten ab, an denen Ressourcen erstellt werden können.
   
    ```powershell   
    Get-AzureRmLocation | sort DisplayName | Select DisplayName
    ```

2. Erstellen Sie die Ressourcengruppe am ausgewählten Standort. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen **myResourceGroup** am Standort **USA, Mitte** erstellt:

    ```powershell   
    New-AzureRmResourceGroup -Name "myResourceGroup" -Location "Central US"
    ```
   
  Die Ausgabe sollte in etwa wie das folgende Beispiel aussehen:

    ```powershell 
    ResourceGroupName : myResourceGroup
    Location          : centralus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myResourceGroup
    ```

## <a name="step-3-create-the-resources"></a>Schritt 3: Erstellen der Ressourcen
Stellen Sie die Vorlage bereit, und geben Sie Parameterwerte an, wenn Sie dazu aufgefordert werden. In diesem Beispiel wird die Vorlage 101-vm-simple-windows in der von Ihnen erstellten Ressourcengruppe bereitgestellt:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myResourceGroup" -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json" 
```
Sie werden aufgefordert, den Namen des Administratorkontos auf dem virtuellen Computer, das Kennwort des Kontos und das DNS-Präfix anzugeben.

Die Ausgabe sollte in etwa wie das folgende Beispiel aussehen:

    DeploymentName    : azuredeploy
    ResourceGroupName : myResourceGroup
    ProvisioningState : Succeeded
    Timestamp         : 12/29/2016 8:11:37 PM
    Mode              : Incremental
    TemplateLink      :
       Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/
                        101-vm-simple-windows/azuredeploy.json
       ContentVersion : 1.0.0.0
    Parameters        :
      Name             Type                       Value
      ===============  =========================  ==========
      adminUsername    String                     myAdminUser
      adminPassword    SecureString
      dnsLabelPrefix   String                     myDomain
      windowsOSVersion String                     2016-Datacenter
    Outputs           :
      Name             Type                       Value
      ===============  =========================  ===========
      hostname         String                     myDomain.centralus.cloudapp.azure.com
    DeploymentDebugLogLevel :

> [!NOTE]
> Sie können auch Vorlagen und Parameter von lokalen Dateien bereitstellen. Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell mit Azure Storage](../../storage/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Nächste Schritte

- Falls bei der Bereitstellung Probleme aufgetreten sind, sollten Sie den Artikel [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../../resource-manager-common-deployment-errors.md) lesen.
- Erfahren Sie, wie Sie Azure PowerShell zum Erstellen eines virtuellen Computers verwenden können: [Erstellen einer Windows-VM mit dem Resource Manager und PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- Informationen zur Verwaltung des erstellten virtuellen Computers finden Sie unter [Verwalten von virtuellen Computern mit Azure Resource Manager und PowerShell](ps-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


