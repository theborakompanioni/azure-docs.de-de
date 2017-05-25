---
title: Erstellen eines benutzerdefinierten Azure DevTest Labs-Images aus einer VHD-Datei mithilfe von PowerShell | Microsoft Docs
description: Automatisieren Sie das Erstellen von benutzerdefinierten Azure DevTest Labs-Images aus VHD-Dateien mithilfe von PowerShell.
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.translationtype: Human Translation
ms.sourcegitcommit: 38d70ed302eeab912ce4fe33272f85e96f1b1eda
ms.openlocfilehash: 6c85df6bbcf25e687881380fd68bb3ee861098e3
ms.contentlocale: de-de
ms.lasthandoff: 01/11/2017


---

# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a>Erstellen eines benutzerdefinierten Images aus einer VHD-Datei mithilfe von PowerShell

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Schrittweise Anweisungen

Die folgenden Schritte führen Sie durch die Erstellung eines benutzerdefinierten Images aus einer VHD-Datei mithilfe von PowerShell:

1. Melden Sie sich an einer PowerShell-Eingabeaufforderung mit dem folgenden Aufruf des Cmdlets **Login-AzureRmAccount** bei Ihrem Azure-Konto an.  
    
    ```PowerShell
    Login-AzureRmAccount
    ```

1.    Wählen Sie das gewünschte Azure-Abonnement aus, indem Sie das Cmdlet **Select-AzureRmSubscription** aufrufen. Ersetzen Sie den folgenden Platzhalter für die Variable **$subscriptionId** durch eine gültige Azure-Abonnement-ID. 

    ```PowerShell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    ```

1.    Rufen Sie das Lab-Objekt durch Aufrufen des Cmdlets **Get-AzureRmResource** ab. Ersetzen Sie die folgenden Platzhalter für die Variablen **$labRg** und **$labName** durch die entsprechenden Werte für Ihre Umgebung. 

    ```PowerShell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```
 
1.    Rufen Sie das Lab-Speicherkonto und die zugehörigen Schlüsselwerte aus dem Lab-Objekt ab. 

    ```PowerShell
    $labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
    $labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value
    ```

1.    Ersetzen Sie den folgenden Platzhalter für die Variable **$vhdUri** durch den URI zu der hochgeladenen VHD-Datei. Sie finden den URI der VHD-Datei im Azure-Portal auf dem Blatt „Blob“ des Speicherkontos.

    ```PowerShell
    $vhdUri = '<Specify the VHD URI here>'
    ```

1.    Erstellen Sie das benutzerdefinierte Image mit dem Cmdlet **New-AzureRmResourceGroupDeployment**. Ersetzen Sie die folgenden Platzhalter für die Variablen **$customImageName** und **$customImageDescription** durch aussagekräftige Namen für Ihre Umgebung.

    ```PowerShell
    $customImageName = '<Specify the custom image name>'
    $customImageDescription = '<Specify the custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-to-create-a-custom-image-from-a-vhd-file"></a>PowerShell-Skript zum Erstellen eines benutzerdefinierten Images aus einer VHD-Datei

Das folgende PowerShell-Skript kann zum Erstellen eines benutzerdefinierten Images aus einer VHD-Datei verwendet werden. Ersetzen Sie die Platzhalter (beginnend und endend mit spitzen Klammern) durch die entsprechenden Werte für Ihre Anforderungen. 

```PowerShell
# Log in to your Azure account.  
Login-AzureRmAccount

# Select the desired Azure subscription. 
$subscriptionId = '<Specify your subscription ID here>'
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Get the lab object.
$labRg = '<Specify your lab resource group name here>'
$labName = '<Specify your lab name here>'
$lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)

# Get the lab storage account and lab storage account key values.
$labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
$labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value

# Set the URI of the VHD file.    
$vhdUri = '<Specify the VHD URI here>'

# Set the custom image name and description values.
$customImageName = '<Specify the custom image name>'
$customImageDescription = '<Specify the custom image description>'

# Set up the parameters object.
$parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

# Create the custom image. 
New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
```

## <a name="related-blog-posts"></a>Verwandte Blogbeiträge

- [Custom images or formulas? (Benutzerdefinierte Images oder Formeln?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copying Custom Images between Azure DevTest Labs (Kopieren benutzerdefinierter Images zwischen Azure DevTest Labs)](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Nächste Schritte

- [Hinzufügen eines virtuellen Computers zum Lab](./devtest-lab-add-vm-with-artifacts.md)

