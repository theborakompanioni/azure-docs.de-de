<properties
   pageTitle="Ändern der IP-Adresspräfixe eines lokalen Netzwerkgateways und der Gateway-IP | Microsoft Azure"
   description="In diesem Artikel wird erläutert, wie Sie die IP-Adresspräfixe für Ihr lokales Netzwerkgateway ändern."
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
   ms.date="08/08/2016"
   ms.author="cherylmc"/>

# Ändern der Einstellungen des lokalen Netzwerkgateways mit PowerShell

Manchmal ändern sich die Einstellungen für „AddressPrefix“ oder „GatewayIPAddress“ Ihres lokalen Netzwerkgateways. Die folgenden Anweisungen unterstützen Sie beim Ändern der Einstellungen für Ihr lokales Netzwerkgateway. Sie können diese Einstellungen auch im Azure-Portal ändern.

## Voraussetzungen
	
Sie müssen die aktuelle Version der PowerShell-Cmdlets für Azure-Ressourcen-Manager installieren. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## So ändern Sie IP-Adresspräfixe

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## So ändern Sie die Gateway-IP-Adresse

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## Nächste Schritte

Sie können die Gatewayverbindung überprüfen. Informationen finden Sie unter [Überprüfen einer Gatewayverbindung](vpn-gateway-verify-connection-resource-manager.md).

<!---HONumber=AcomDC_0810_2016-->