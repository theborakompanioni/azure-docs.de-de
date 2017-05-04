---
title: "Einrichten von Key Vault für virtuelle Windows-Computer in Azure Resource Manager | Microsoft-Dokumentation"
description: "Einrichten eines Schlüsseltresors, der für einen mit Azure Resource Manager erstellten virtuellen Computer verwendet werden soll."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: kasing
translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: a5083a5216efbfd76fd912ec48c2f0ec3b30c4a1
ms.lasthandoff: 05/03/2017


---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Einrichten des Schlüsseltresors für virtuelle Computer in Azure Resource Manager

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

Im Azure Resource Manager-Stapel sind geheime Schlüssel/Zertifikate als Ressourcen modelliert, die durch den Ressourcenanbieter des Schlüsseltresors bereitgestellt werden. Weitere Informationen über Schlüsseltresore finden Sie unter [Was ist der Azure-Schlüsseltresor?](../../key-vault/key-vault-whatis.md)

> [!NOTE]
> 1. Damit ein Schlüsseltresor mit virtuellen Computern verwendet werden kann, die mit Azure Resource Manager bereitgestellt wurden, müssen Sie die **EnabledForDeployment** -Eigenschaft für den Schlüsseltresor auf „true“ festlegen. Dazu können Sie verschiedene Clients verwenden.
> 2. Der Schlüsseltresor muss im gleichen Abonnement und Speicherort wie der virtuelle Computer erstellt werden.
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>Verwenden von PowerShell zum Einrichten des Schlüsseltresors
Informationen zum Erstellen eines Schlüsseltresors mit PowerShell finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](../../key-vault/key-vault-get-started.md#vault).

Für neue Schlüsseltresore können Sie dieses PowerShell-Cmdlet verwenden:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Für vorhandene Schlüsseltresore können Sie dieses PowerShell-Cmdlet verwenden:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a>Verwenden der Befehlszeilenschnittstelle zum Einrichten des Schlüsseltresors
Informationen zum Erstellen eines Schlüsseltresors über die Befehlszeilenschnittstelle (CLI) finden Sie unter [Verwalten des Schlüsseltresors über die Befehlszeilenschnittstelle](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

Wenn Sie die Befehlszeilenschnittstelle verwenden, müssen Sie den Schlüsseltresor erstellen, bevor Sie die Bereitstellungsrichtlinie zuweisen. Hierfür können Sie den folgenden Befehl verwenden:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Verwenden von Vorlagen zum Einrichten des Schlüsseltresors
Wenn Sie eine Vorlage verwenden, müssen Sie die `enabledForDeployment`-Eigenschaft für die Schlüsseltresorressource auf `true` festlegen.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Weitere Optionen, die Sie beim Erstellen eines Schlüsseltresors mithilfe von Vorlagen konfigurieren können, finden Sie unter [Create a Key Vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)(Erstellen eines Schlüsseltresors).

