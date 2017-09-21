---
title: "Erstellen eines Anwendungsgateways – Azure CLI 2.0 | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie mithilfe der Azure CLI 2.0 in Resource Manager ein Anwendungsgateway erstellen.
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 9429fafa73392c3ab5d6af844453ba97f7d4f65b
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="create-an-application-gateway-by-using-the-azure-cli-20"></a>Erstellen eines Anwendungsgateways mithilfe der Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Klassische Azure PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-Vorlage](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)

Azure Application Gateway ist ein dediziertes virtuelles Gerät, das einen ADC (Application Delivery Controller) als Dienst bereitstellt und für Ihre Anwendung verschiedene Lastenausgleichsfunktionen der Ebene 7 bietet.

## <a name="cli-versions"></a>CLI-Versionen

Sie können zum Erstellen eines Anwendungsgateways eine der folgenden Versionen der Befehlszeilenschnittstelle (Command-Line Interface, CLI) verwenden:

* [Azure CLI 1.0:](application-gateway-create-gateway-cli-nodejs.md) Azure CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
* [Azure CLI 2.0:](application-gateway-create-gateway-cli.md) CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="prerequisite-install-the-azure-cli-20"></a>Voraussetzung: Installieren der Azure CLI 2.0

Um die Schritte in diesem Artikel ausführen zu können, müssen Sie [die Azure CLI für macOS, Linux und Windows installieren](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

> [!NOTE]
> Zum Erstellen eines Gateways benötigen Sie ein Azure-Konto. Falls Sie diese benötigen, können Sie sich für eine [kostenlose Testversion](../active-directory/sign-up-organization.md)registrieren.

## <a name="scenario"></a>Szenario

In diesem Szenario erfahren Sie, wie Sie ein Anwendungsgateway über das Azure-Portal erstellen.

Dieses Szenario umfasst Folgendes:

* Sie erstellen ein Anwendungsgateway für mittlere Nutzungsdauer mit zwei Instanzen.
* Sie erstellen ein virtuelles Netzwerk mit dem Namen AdatumAppGatewayVNET und dem reservierten CIDR-Block 10.0.0.0/16.
* Sie erstellen ein Subnetz mit dem Namen Appgatewaysubnet, für das 10.0.0.0/28 als CIDR-Block verwendet wird.

> [!NOTE]
> Zusätzliche Konfigurationsschritte für das Anwendungsgateway (u.a. benutzerdefinierte Integritätstests, Back-End-Pool-Adressen und zusätzliche Regeln) werden nicht während der Erstbereitstellung, sondern nach der Erstellung des Anwendungsgateways ausgeführt.

## <a name="before-you-begin"></a>Voraussetzungen

Für ein Anwendungsgateway ist ein eigenes Subnetz erforderlich. Stellen Sie beim Erstellen eines virtuellen Netzwerks sicher, dass der Adressbereich für mehrere Subnetze ausreicht. Nachdem Sie ein Anwendungsgateway in einem Subnetz bereitgestellt haben, können Sie nur zusätzliche Anwendungsgateways zu diesem Subnetz hinzufügen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Öffnen Sie die **Microsoft Azure-Eingabeaufforderung**, und melden Sie sich an:

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> Sie können `az login` auch ohne die Option für die Geräteanmeldung verwenden, die die Eingabe eines Codes unter „aka.ms/devicelogin“ erfordert.

Nach Eingabe des obigen Befehls erhalten Sie einen Code. Navigieren Sie in einem Browser zu https://aka.ms/devicelogin, und setzen Sie den Anmeldeprozess fort.

![Cmd mit Geräteanmeldung][1]

Geben Sie im Browser den Code ein, den Sie erhalten haben. Dadurch werden Sie zu einer Anmeldeseite umgeleitet.

![Browser zur Eingabe des Codes][2]

Geben Sie den Code ein, um sich anzumelden, und schließen Sie dann den Browser, um den Vorgang fortzusetzen.

![Erfolgreich angemeldet][3]

## <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie vor der Erstellung des Anwendungsgateways eine Ressourcengruppe für das Gateway. Verwenden Sie den folgenden Befehl:

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-the-application-gateway"></a>Erstellen des Anwendungsgateways

Verwenden Sie als IP-Adressen des Back-End-Servers die Back-End-IP-Adressen. Diese Werte können entweder private IPs im virtuellen Netzwerk, öffentliche IPs oder vollqualifizierte Domänennamen für Ihre Back-End-Server sein. Das folgende Beispiel erstellt ein Anwendungsgateway mit zusätzlichen Konfigurationen für HTTP-Einstellungen, Ports und Regeln:

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet" \
--subnet-address-prefix "10.0.0.0/28" \
--servers 10.0.0.4 10.0.0.5 \
--capacity 2 \
--sku Standard_Small \
--http-settings-cookie-based-affinity Enabled \
--http-settings-protocol Http \
--frontend-port 80 \
--routing-rule-type Basic \
--http-settings-port 80 \
--public-ip-address "pip2" \
--public-ip-address-allocation "dynamic" \

```

Das vorherige Beispiel zeigt verschiedene Eigenschaften, die während der Erstellung eines Anwendungsgateways nicht erforderlich sind. Das folgende Codebeispiel erstellt ein Anwendungsgateway mit den erforderlichen Informationen:

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet \
--subnet-address-prefix "10.0.0.0/28" \
--servers "10.0.0.5"  \
--public-ip-address pip
```
 
> [!NOTE]
> Um eine Liste mit Parametern zu erhalten, die während der Erstellung verwendet wird, führen Sie den folgenden Befehl aus: `az network application-gateway create --help`.

Mit diesem Beispiel wird ein einfaches Anwendungsgateway mit Standardeinstellungen für Listener, Back-End-Pool, Back-End-HTTP-Einstellungen und Regeln erstellt. Nach der erfolgreichen Bereitstellung können Sie diese Einstellungen an Ihre Anforderungen anpassen.

Wenn die Webanwendung mit dem Back-End-Pool in den vorherigen Schritten definiert wurde, beginnt der Lastenausgleich jetzt.

## <a name="get-the-application-gateway-dns-name"></a>Abrufen des DNS-Namens für das Anwendungsgateway
Nach dem Erstellen des Gateways konfigurieren Sie das Front-End für die Kommunikation. Wenn Sie eine öffentliche IP-Adresse verwenden, wird für das Anwendungsgateway ein dynamisch zugewiesener DNS-Name benötigt. Dieser Name ist kein Anzeigename. Um zu gewährleisten, dass Benutzer auf das Anwendungsgateway zugreifen können, verweisen Sie mit einem CNAME-Eintrag auf den öffentlichen Endpunkt des Anwendungsgateways. Weitere Informationen finden Sie unter [Bereitstellen von benutzerdefinierten Domäneneinstellungen für einen Azure-Dienst mit Azure DNS](../dns/dns-custom-domain.md).

Rufen Sie zum Konfigurieren eines Alias mithilfe des PublicIPAddress-Elements, das an das Anwendungsgateway angefügt ist, Details zum Anwendungsgateway sowie die zugeordnete IP-Adresse/den zugeordneten DNS-Namen ab. Verwenden Sie den DNS-Namen des Anwendungsgateways zum Erstellen eines CNAME-Eintrags, der die beiden Webanwendungen an diesen DNS-Namen verweist. Verwenden Sie keine A-Einträge, da sich die VIP beim Neustart des Anwendungsgateways möglicherweise ändert.


```azurecli-interactive
az network public-ip show --name "pip" --resource-group "AdatumAppGatewayRG"
```

```
{
  "dnsSettings": {
    "domainNameLabel": null,
    "fqdn": "8c786058-96d4-4f3e-bb41-660860ceae4c.cloudapp.net",
    "reverseFqdn": null
  },
  "etag": "W/\"3b0ac031-01f0-4860-b572-e3c25e0c57ad\"",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/publicIPAddresses/pip2",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "40.121.167.250",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/applicationGateways/AdatumAppGateway2/frontendIPConfigurations/appGatewayFrontendIP",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "AdatumAppGatewayRG",
    "subnet": null
  },
  "location": "eastus",
  "name": "pip2",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "AdatumAppGatewayRG",
  "resourceGuid": "3c30d310-c543-4e9d-9c72-bbacd7fe9b05",
  "tags": {
    "cli[2] owner[administrator]": ""
  },
  "type": "Microsoft.Network/publicIPAddresses"
}
```

## <a name="delete-all-resources"></a>Löschen aller Ressourcen

Führen Sie den folgenden Befehl aus, um alle in diesem Artikel erstellten Ressourcen zu löschen:

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a>Nächste Schritte

Informationen zum Erstellen benutzerdefinierter Tests finden Sie unter [Erstellen eines benutzerdefinierten Tests für ein Anwendungsgateway über das Portal](application-gateway-create-probe-portal.md).

Unter [Konfigurieren eines Anwendungsgateways für die SSL-Auslagerung mit Azure Resource Manager](application-gateway-ssl-arm.md) erfahren Sie, wie Sie die SSL-Abladung konfigurieren und die aufwendige Verschlüsselung für Webserver vermeiden.

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png

