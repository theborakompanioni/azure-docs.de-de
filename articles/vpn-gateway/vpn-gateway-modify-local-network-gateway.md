---
title: "Ändern der IP-Adresspräfixe eines lokalen Netzwerkgateways und der Gateway-IP | Microsoft Docs"
description: "In diesem Artikel wird erläutert, wie Sie die IP-Adresspräfixe für Ihr lokales Netzwerkgateway ändern."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 8c7db48f-d09a-44e7-836f-1fb6930389df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/08/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e88511058b1de79ca222d87ada6c9abdf4daf11c


---
# <a name="modify-local-network-gateway-settings-using-powershell"></a>Ändern der Einstellungen des lokalen Netzwerkgateways mit PowerShell
Manchmal ändern sich die Einstellungen für „AddressPrefix“ oder „GatewayIPAddress“ Ihres lokalen Netzwerkgateways. Die folgenden Anweisungen unterstützen Sie beim Ändern der Einstellungen für Ihr lokales Netzwerkgateway. Sie können diese Einstellungen auch im Azure-Portal ändern.

## <a name="before-you-begin"></a>Voraussetzungen
Sie müssen die aktuelle Version der PowerShell-Cmdlets für Azure-Ressourcen-Manager installieren. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) .

## <a name="to-modify-ip-address-prefixes"></a>So ändern Sie IP-Adresspräfixe
[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="to-modify-the-gateway-ip-address"></a>So ändern Sie die Gateway-IP-Adresse
[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Sie können die Gatewayverbindung überprüfen. Informationen finden Sie unter [Überprüfen einer Gatewayverbindung](vpn-gateway-verify-connection-resource-manager.md).




<!--HONumber=Nov16_HO3-->


