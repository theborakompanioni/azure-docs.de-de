---
title: "Überprüfen einer Gatewayverbindung | Microsoft Docs"
description: "In diesem Artikel erfahren Sie, wie Sie eine Gatewayverbindung im Resource Manager-Bereitstellungsmodell überprüfen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 7e3d1043-caa9-4472-96d3-832f4e2c91ee
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/14/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 3fe204c09eebf7d254a1bf2bb130e2d3498b6b45
ms.openlocfilehash: d1b6d12b1976b317e9ed496857439c86e99253f9


---
# <a name="verify-a-gateway-connection"></a>Überprüfen einer Gatewayverbindung
Es gibt verschiedene Möglichkeiten zum Überprüfen der Gatewayverbindung. In diesem Artikel wird gezeigt, wie Sie den Status einer Resource Manager-Gatewayverbindung mithilfe des Azure-Portals und mithilfe von PowerShell überprüfen.

## <a name="verify-using-powershell"></a>Überprüfen mithilfe von PowerShell
Sie müssen die aktuelle Version der PowerShell-Cmdlets für Azure-Ressourcen-Manager installieren. Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs). Weitere Informationen zur Verwendung von Resource Manager-Cmdlets finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1-log-in-to-your-azure-account"></a>Schritt 1: Melden Sie sich bei Ihrem Azure-Konto an.
1. Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her.
   
        Login-AzureRmAccount
2. Überprüfen Sie die Abonnements für das Konto.
   
        Get-AzureRmSubscription 
3. Geben Sie das Abonnement an, das Sie verwenden möchten.
   
        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### <a name="step-2-verify-your-connection"></a>Schritt 2: Überprüfen Sie die Verbindung.
[!INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="verify-using-the-azure-portal"></a>Überprüfen mithilfe des Azure-Portals
[!INCLUDE [verify connection portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="next-steps"></a>Nächste Schritte
* Sie können Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Für diese Schritte finden Sie Informationen unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) .




<!--HONumber=Dec16_HO1-->


