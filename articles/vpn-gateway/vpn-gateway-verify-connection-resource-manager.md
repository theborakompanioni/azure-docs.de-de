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
ms.date: 04/24/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 0e9fa1b1397c60985de9d2e60b3f01146036801f
ms.lasthandoff: 04/26/2017


---
# <a name="verify-a-vpn-gateway-connection"></a>Überprüfen einer VPN Gateway-Verbindung

In diesem Artikel erfahren Sie, wie Sie Ihre VPN-Gatewayverbindung im Resource Manager- und klassischen Bereitstellungsmodell überprüfen.

## <a name="verify-using-the-azure-portal"></a>Überprüfen mithilfe des Azure-Portals

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="verify-using-powershell"></a>Überprüfen mithilfe von PowerShell

Installieren Sie zum Überprüfen mithilfe von PowerShell die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager. Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs). Weitere Informationen zur Verwendung von Resource Manager-Cmdlets finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md).

### <a name="log-in-to-your-azure-account"></a>Melden Sie sich bei Ihrem Azure-Konto an.
1. Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her.

  ```powershell
  Login-AzureRmAccount
  ```
2. Überprüfen Sie die Abonnements für das Konto.

  ```powershell
  Get-AzureRmSubscription
  ``` 
3. Geben Sie das Abonnement an, das Sie verwenden möchten.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```

### <a name="verify-your-connection"></a>Überprüfen der Verbindung

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="verify-using-the-azure-cli"></a>Überprüfen über die Azure CLI

Zum Überprüfen mithilfe der Azure CLI installieren Sie die neueste Version der CLI-Befehle (ab 2.0). Informationen zur Installation der CLI-Befehle finden Sie unter [Installieren von Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="log-in-to-your-azure-account"></a>Melden Sie sich bei Ihrem Azure-Konto an.

1. Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

  ```azurecli
  az login
  ```
2. Listen Sie die Abonnements für das Konto auf, wenn Sie über mehr als ein Azure-Abonnement verfügen.

  ```azurecli
  Az account list --all
  ```
3. Geben Sie das Abonnement an, das Sie verwenden möchten.

  ```azurecli
  Az account set --subscription
  <replace_with_your_subscription_id>
  ```

### <a name="verify-your-connection"></a>Überprüfen der Verbindung

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <a name="verify-using-the-azure-portal-classic"></a>Überprüfen mithilfe des Azure-Portals (klassisch)
[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


## <a name="verify-using-powershell-classic"></a>Überprüfen mithilfe von PowerShell (klassisch)
Installieren Sie zum Überprüfen mithilfe von PowerShell die aktuellen Versionen der Azure PowerShell-Cmdlets. Achten Sie darauf, dass Sie sowohl die Resource Manager- als auch die Dienstverwaltungsversion (Service Management, SM) installieren und herunterladen. Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs). 

### <a name="log-in-to-your-azure-account"></a>Melden Sie sich bei Ihrem Azure-Konto an.
1. Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her.

  ```powershell
  Login-AzureRmAccount
  ```
2. Überprüfen Sie die Abonnements für das Konto.

  ```powershell
  Get-AzureRmSubscription 
  ```
3. Geben Sie das Abonnement an, das Sie verwenden möchten.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
4. Melden Sie sich für das klassische Bereitstellungsmodell bei den Dienstverwaltungs-Cmdlets an.

  ```powershell
  Add-AzureAccount
  ```

### <a name="verify-your-connection"></a>Überprüfen der Verbindung
[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Nächste Schritte
* Sie können Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Für diese Schritte finden Sie Informationen unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) .


