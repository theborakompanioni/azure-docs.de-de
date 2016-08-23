<properties
	pageTitle="Verschieben eines virtuellen Linux-Computers | Microsoft Azure"
	description="Verschieben Sie einen virtuellen Linux-Computer im Resource Manager-Bereitstellungsmodell in ein anderes Azure-Abonnement oder eine andere Ressourcengruppe."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/08/2016"
	ms.author="cynthn"/>

	


# Verschieben einer Linux-VM in ein anderes Abonnement oder eine andere Ressourcengruppe

In diesem Artikel wird beschrieben, wie Sie eine Linux-VM zwischen Ressourcengruppen oder Abonnements verschieben. Das Verschieben einer VM zwischen Abonnements kann nützlich sein, wenn Sie eine VM in einem persönlichen Abonnement erstellt haben und sie nun in das Abonnement Ihres Unternehmens verschieben möchten.

> [AZURE.NOTE] Im Rahmen der Verschiebung werden neue Ressourcen-IDs erstellt. Nachdem die VM verschoben wurde, müssen Sie Ihre Tools und Skripts aktualisieren, damit die neuen Ressourcen-IDs verwendet werden.


## Verwenden der Azure-Befehlszeilenschnittstelle zum Verschieben einer VM 

Um eine VM erfolgreich verschieben zu können, müssen Sie die VM und alle unterstützenden Ressourcen verschieben. Verwenden Sie den Befehl **azure group show**, um alle Ressourcen einer Ressourcengruppe und die dazugehörigen IDs aufzulisten. Es ist hilfreich, die Ausgabe dieses Befehls per Pipe-Vorgang an eine Datei zu übergeben, damit Sie die IDs später kopieren und in Befehle einfügen können.

	azure group show <resourceGroupName>

Verwenden Sie den CLI-Befehl **azure resource move**, um eine VM und die dazugehörigen Ressourcen in eine andere Ressourcengruppe zu verschieben. Im folgenden Beispiel wird gezeigt, wie Sie eine VM und die wichtigsten Ressourcen dafür verschieben. Wir verwenden den Parameter **-i** und übergeben eine kommagetrennte Liste (ohne Leerstellen) mit IDs für die zu verschiebenden Ressourcen.

	
    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
	nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
	nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
	pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
	vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
	diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
	storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>  	
	
	azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"
	
Wenn Sie die VM und ihre Ressourcen in ein anderes Abonnement verschieben möchten, fügen Sie den Parameter **--destination-subscriptionId &#60;Zielabonnement-ID&#62;** hinzu, um das Zielabonnement anzugeben.

Wenn Sie die Eingabeaufforderung auf einem Windows-Computer verwenden, müssen Sie vor den Variablennamen beim Deklarieren das Zeichen **$** hinzufügen. Dies ist unter Linux nicht erforderlich.

Sie werden aufgefordert zu bestätigen, dass die angegebene Ressource verschoben werden soll. Geben Sie **Y** (J) ein, um zu bestätigen, dass die Ressourcen verschoben werden sollen.
	

[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]

## Nächste Schritte

Sie können viele verschiedene Arten von Ressourcen zwischen Ressourcengruppen und Abonnements verschieben. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../resource-group-move-resources.md).

<!---HONumber=AcomDC_0810_2016-->