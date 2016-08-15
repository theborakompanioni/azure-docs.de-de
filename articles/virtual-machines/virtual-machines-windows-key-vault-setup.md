<properties
	pageTitle="Einrichten des Schlüsseltresors für virtuelle Computer in Azure Resource Manager | Microsoft Azure"
	description="Einrichten eines Schlüsseltresors, der für einen mit Azure Resource Manager erstellten virtuellen Computer verwendet werden soll."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="singhkays"
	manager="drewm"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/31/2016"
	ms.author="singhkay"/>

# Einrichten des Schlüsseltresors für virtuelle Computer in Azure Resource Manager

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] Klassisches Bereitstellungsmodell

Im Azure Resource Manager-Stapel sind geheime Schlüssel/Zertifikate als Ressourcen modelliert, die durch den Ressourcenanbieter des Schlüsseltresors bereitgestellt werden. Weitere Informationen über Schlüsseltresore finden Sie unter [Was ist der Azure-Schlüsseltresor?](../key-vault/key-vault-whatis.md).

>[AZURE.NOTE] 
>
>1. Damit ein Schlüsseltresor mit virtuellen Computern verwendet werden kann, die mit Azure Resource Manager bereitgestellt wurden, müssen Sie die **EnabledForDeployment**-Eigenschaft für den Schlüsseltresor auf „true“ festlegen. Dazu können Sie verschiedene Clients verwenden.
>
>2. Der Schlüsseltresor muss im selben Abonnement und Speicherort wie der virtuelle Computer erstellt werden.

## Verwenden von PowerShell zum Einrichten des Schlüsseltresors
Informationen zum Erstellen eines Schlüsseltresors mit PowerShell finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](../key-vault/key-vault-get-started.md#vault).

Für neue Schlüsseltresore können Sie dieses PowerShell-Cmdlet verwenden:

	New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Für vorhandene Schlüsseltresore können Sie dieses PowerShell-Cmdlet verwenden:

	Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## Verwenden der Befehlszeilenschnittstelle zum Einrichten des Schlüsseltresors
Informationen zum Erstellen eines Schlüsseltresors über die Befehlszeilenschnittstelle (CLI) finden Sie unter [Verwalten des Schlüsseltresors über die Befehlszeilenschnittstelle](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault).

Wenn Sie die Befehlszeilenschnittstelle verwenden, müssen Sie den Schlüsseltresor erstellen, bevor Sie die Bereitstellungsrichtlinie zuweisen. Hierfür können Sie den folgenden Befehl verwenden:

	azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## Verwenden von Vorlagen zum Einrichten des Schlüsseltresors
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

Weitere Optionen, die Sie beim Erstellen eines Schlüsseltresors mithilfe von Vorlagen konfigurieren können, finden Sie unter [Create a Key Vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/) (Erstellen eines Schlüsseltresors).

<!---HONumber=AcomDC_0803_2016-->