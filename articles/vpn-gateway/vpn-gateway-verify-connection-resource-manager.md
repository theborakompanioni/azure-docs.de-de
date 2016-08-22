<properties
   pageTitle="Überprüfen einer Gatewayverbindung | Microsoft Azure"
   description="In diesem Artikel erfahren Sie, wie Sie eine Gatewayverbindung im Resource Manager-Bereitstellungsmodell überprüfen."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/03/2016"
   ms.author="cherylmc"/>

# Überprüfen einer Gatewayverbindung

Es gibt verschiedene Möglichkeiten zum Überprüfen der Gatewayverbindung. In diesem Artikel wird gezeigt, wie Sie den Status einer Resource Manager-Gatewayverbindung mithilfe des Azure-Portals und mithilfe von PowerShell überprüfen.


## Voraussetzungen

Wenn Sie PowerShell verwenden möchten, müssen Sie die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager installieren. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Weitere Informationen zur Verwendung von Resource Manager-Cmdlets finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

1. Öffnen Sie die PowerShell-Konsole, und stellen Sie eine Verbindung mit Ihrem Konto her.

		Login-AzureRmAccount

2. Überprüfen Sie die Abonnements für das Konto.

		Get-AzureRmSubscription 

3. Geben Sie das Abonnement an, das Sie verwenden möchten.

		Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## Überprüfen der Verbindung


[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## Nächste Schritte

- Sie können Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Für diese Schritte finden Sie Informationen unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

<!---HONumber=AcomDC_0810_2016-->