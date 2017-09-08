---
title: 'Konfigurieren einer Web Application Firewall: Azure Application Gateway | Microsoft-Dokumentation'
description: "Dieser Artikel enthält Anleitungen zur Verwendung einer Web Application Firewall für ein vorhandenes oder neues Anwendungsgateway."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: gwallace
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: 999d1ad3ee54d84e155254655dbb7a39ac60572c
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="configure-a-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a>Konfigurieren einer Web Application Firewall auf einem neuen oder vorhandenen Anwendungsgateway mit der Azure CLI

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure-CLI](application-gateway-web-application-firewall-cli.md)

In diesem Artikel wird erläutert, wie ein Anwendungsgateway, für das eine Web Application Firewall (WAF) aktiviert ist, erstellt wird. Zudem wird das Hinzufügen von WAF zu einem vorhandenen Anwendungsgateway erläutert.

WAF in Azure Application Gateway schützt Webanwendungen vor gängigen webbasierten Angriffen wie der Einschleusung von SQL-Befehlen, Angriffen durch websiteübergreifende Skripts und der Übernahme von Sitzungen.

 Application Gateway ist ein Lastenausgleich auf Schicht 7 (Anwendungsschicht). Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Application Gateway stellt viele ADC-Features (Application Delivery Controller) bereit:

 * HTTP-Lastenausgleich 
 * Cookiebasierte Sitzungsaffinität 
 * SSL-Auslagerung (Secure Sockets Layer) 
 * Benutzerdefinierte Integritätstests 
 * Unterstützung der Funktion für mehrere Standorte
 
 Eine vollständige Liste der unterstützten Features finden Sie unter [Übersicht über Application Gateway](application-gateway-introduction.md).

Dieser Artikel veranschaulicht das [Hinzufügen einer Web Application Firewall zu einem vorhandenen Anwendungsgateway](#add-web-application-firewall-to-an-existing-application-gateway). Er erläutert außerdem das [Erstellen eines Anwendungsgateways, das eine Web Application Firewall nutzt](#create-an-application-gateway-with-web-application-firewall).

![Szenarioabbildung][scenario]

## <a name="prerequisite-install-the-azure-cli-20"></a>Voraussetzung: Installieren der Azure CLI 2.0

Um die Schritte in diesem Artikel ausführen zu können, müssen Sie [die Azure-Befehlszeilenschnittstelle (Azure CLI) für Mac, Linux und Windows installieren](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="waf-configuration-differences"></a>Unterschiede bei der WAF-Konfiguration

Wenn Sie [Erstellen eines Anwendungsgateways mithilfe der Azure CLI 2.0](application-gateway-create-gateway-cli.md) gelesen haben, kennen Sie die SKU-Einstellungen, die beim Erstellen eines Anwendungsgateways konfiguriert werden müssen. WAF bietet zusätzliche Einstellungen, die Sie beim Konfigurieren der SKU für ein Anwendungsgateway definieren müssen. Am Anwendungsgateway selbst sind keine zusätzlichen Änderungen erforderlich.

| **Einstellung** | **Details**
|---|---|
|**SKU** |Ein normales Anwendungsgateway ohne WAF unterstützt die Größen **Standard\_Small**, **Standard\_Medium** und **Standard\_Large**. Mit der Einführung von WAF gibt es zwei zusätzliche SKUs: **WAF\_Medium** und **WAF\_Large**. WAF wird für kleine Anwendungsgateways nicht unterstützt.|
|**Mode** | Diese Einstellung ist der WAF-Modus. Zulässige Werte sind **Detection** (Erkennung) und **Prevention** (Schutz). Wenn WAF im **Erkennungsmodus** eingerichtet wird, werden alle Bedrohungen in einer Protokolldatei gespeichert. Im **Schutzmodus** werden Ereignisse ebenfalls protokolliert, der Angreifer erhält vom Anwendungsgateway jedoch zusätzlich die Antwort 403 (nicht autorisiert).|

## <a name="add-a-web-application-firewall-to-an-existing-application-gateway"></a>Hinzufügen einer Web Application Firewall zu einem vorhandenen Anwendungsgateway

Der folgende Befehl ändert ein vorhandenes Standardanwendungsgateway in ein Anwendungsgateway, für das WAF aktiviert ist:

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

Dieser Befehl aktualisiert das Anwendungsgateway mit WAF. Informationen zur Anzeige von Protokollen für Ihr Anwendungsgateway finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md). Aufgrund der Art der WAF-Sicherheit müssen Sie die Protokolle regelmäßig überprüfen, um die Sicherheitslage Ihrer Webanwendungen zu kennen.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Erstellen eines Anwendungsgateways mit einer Web Application Firewall

Mit dem folgenden Befehl wird ein Anwendungsgateway mit WAF erstellt:

```azurecli-interactive
#!/bin/bash

az network application-gateway create \
  --name "AdatumAppGateway2" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet2" \
  --subnet-address-prefix "10.0.0.0/28" \
 --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 
  --sku "WAF_Medium" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip2" \
  --public-ip-address-allocation "dynamic" \
  --tags "cli[2] owner[administrator]"
```

> [!NOTE]
> Anwendungsgateways, die mit der WAF-Basiskonfiguration erstellt wurden, werden zum Schutz mit CRS 3.0 konfiguriert.

## <a name="get-an-application-gateway-dns-name"></a>Abrufen des DNS-Namens eines Anwendungsgateways

Nach dem Erstellen des Gateways wird das Front-End für die Kommunikation konfiguriert. Wenn Sie eine öffentliche IP-Adresse verwenden, wird für das Anwendungsgateway ein dynamisch zugewiesener DNS-Name benötigt. Dieser Name ist nicht benutzerfreundlich. Um zu gewährleisten, dass Benutzer auf das Anwendungsgateway zugreifen können, verweisen Sie mit einem CNAME-Eintrag auf den öffentlichen Endpunkt des Anwendungsgateways. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für einen Azure-Clouddienst](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Rufen Sie zum Konfigurieren eines CNAME-Eintrags mithilfe des PublicIPAddress-Elements, das an das Anwendungsgateway angefügt ist, Details zum Anwendungsgateway sowie die zugeordnete IP-Adresse/den zugeordneten DNS-Namen ab. Verwenden Sie den DNS-Namen des Anwendungsgateways zum Erstellen eines CNAME-Eintrags, der die beiden Webanwendungen an diesen DNS-Namen verweist. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart des Anwendungsgateways möglicherweise ändert.

```azurecli-interactive
#!/bin/bash

az network public-ip show \
  --name pip2 \
  --resource-group "AdatumAppGatewayRG"
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

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Anpassen von WAF-Regeln finden Sie unter [Anpassen von Web Application Firewall-Regeln mit Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png

