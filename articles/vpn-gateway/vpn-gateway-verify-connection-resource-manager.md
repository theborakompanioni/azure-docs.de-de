---
title: "Überprüfen einer VPN Gateway-Verbindung | Microsoft-Dokumentation"
description: "Dieser Artikel zeigt, wie Sie eine Verbindung eines virtuellen Netzwerks mit einem VPN Gateway überprüfen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 7e3d1043-caa9-4472-96d3-832f4e2c91ee
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: b5bad71095e4b7e3b26df15780467526200ffa10
ms.openlocfilehash: 68d94a6402b1497f65c4d03fb987ba800e86c2a3


---
# <a name="verify-a-vpn-gateway-connection"></a>Überprüfen einer VPN Gateway-Verbindung
Sie können die Verbindung Ihres virtuellen Netzwerks mit einem VPN Gateway über das Portal und mithilfe von PowerShell überprüfen. Dieser Artikel enthält sowohl Schritte des Resource Manager- als auch des klassischen Bereitstellungsmodells.

## <a name="verify-using-the-azure-portal"></a>Überprüfen mithilfe des Azure-Portals

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="verify-using-powershell"></a>Überprüfen mithilfe von PowerShell

Installieren Sie zum Überprüfen mithilfe von PowerShell die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager. Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs). Weitere Informationen zur Verwendung von Resource Manager-Cmdlets finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### <a name="log-in-to-your-azure-account"></a>Melden Sie sich bei Ihrem Azure-Konto an.
1. Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her.
   
        Login-AzureRmAccount
2. Überprüfen Sie die Abonnements für das Konto.
   
        Get-AzureRmSubscription 
3. Geben Sie das Abonnement an, das Sie verwenden möchten.
   
        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### <a name="verify-your-connection"></a>Überprüfen der Verbindung

[!INCLUDE [Powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="verify-using-the-azure-portal-classic"></a>Überprüfen mithilfe des Azure-Portals (klassisch)
[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


## <a name="verify-using-powershell-classic"></a>Überprüfen mithilfe von PowerShell (klassisch)
Installieren Sie zum Überprüfen mithilfe von PowerShell die aktuellen Versionen der Azure PowerShell-Cmdlets. Achten Sie darauf, dass Sie sowohl die Resource Manager- als auch die Dienstverwaltungsversion (Service Management, SM) installieren und herunterladen. Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs). 

### <a name="log-in-to-your-azure-account"></a>Melden Sie sich bei Ihrem Azure-Konto an.
1. Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her.
   
        Login-AzureRmAccount
2. Überprüfen Sie die Abonnements für das Konto.
   
        Get-AzureRmSubscription 
3. Geben Sie das Abonnement an, das Sie verwenden möchten.
   
        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
4. Melden Sie sich für das klassische Bereitstellungsmodell bei den Dienstverwaltungs-Cmdlets an.

        Add-AzureAccount

### <a name="verify-your-connection"></a>Überprüfen der Verbindung
[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Nächste Schritte
* Sie können Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Für diese Schritte finden Sie Informationen unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) .




<!--HONumber=Jan17_HO5-->


