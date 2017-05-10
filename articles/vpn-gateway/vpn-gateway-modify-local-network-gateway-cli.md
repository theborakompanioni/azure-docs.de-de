---
title: "Ändern der IP-Adresspräfixe des lokalen Netzwerkgateways und der IP-Adresse des VPN-Gateways | Azure| CLI| Microsoft-Dokumentation"
description: "In diesem Artikel wird erläutert, wie Sie die IP-Adresspräfixe für Ihr lokales Netzwerkgateway mithilfe der Azure CLI ändern."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c7db48f-d09a-44e7-836f-1fb6930389df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/24/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 81c1a6e2357c8d336449224fac841be8dcc2a3cd
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="modify-local-network-gateway-settings-using-the-azure-cli"></a>Ändern der Einstellungen des lokalen Netzwerkgateways mithilfe der Azure CLI

Manchmal ändern sich die Einstellungen für „AddressPrefix“ oder „GatewayIPAddress“ Ihres lokalen Netzwerkgateways. In diesem Artikel wird gezeigt, wie Sie die Einstellungen Ihres lokalen Netzwerkgateways ändern. Sie können diese Einstellungen auch im Azure-Portal oder mithilfe von PowerShell ändern.

## <a name="before-you-begin"></a>Voraussetzungen

Installieren Sie die aktuelle Version der CLI-Befehle (2.0 oder höher). Informationen zur Installation der CLI-Befehle finden Sie unter [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (Installieren von Azure CLI 2.0).

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="modify-ip-address-prefixes"></a>Ändern von IP-Adresspräfixen

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <a name="modify-the-gateway-ip-address"></a>Ändern der Gateway-IP-Adresse

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können die Gatewayverbindung überprüfen. Informationen finden Sie unter [Überprüfen einer Gatewayverbindung](vpn-gateway-verify-connection-resource-manager.md).


