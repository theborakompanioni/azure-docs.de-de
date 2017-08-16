---
title: "Konfigurieren der Web Application Firewall – Azure Application Gateway | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Anleitungen zur Verwendung der Web Application Firewall auf einem vorhandenen oder neuen Anwendungsgateway."
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
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: ac6c629ceaf1a8036643f593ce3d7ef9ea096ef8
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a>Konfigurieren der Web Application Firewall auf einem neuen oder vorhandenen Anwendungsgateway mit der Azure CLI

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure-CLI](application-gateway-web-application-firewall-cli.md)

Erfahren Sie, wie Sie ein durch eine Web Application Firewall aktiviertes Anwendungsgateway erstellen oder einem vorhandenen Anwendungsgateway eine Web Application Firewall hinzufügen.

Die Web Application Firewall (WAF) in Azure Application Gateway schützt Webanwendungen vor gängigen webbasierten Angriffen wie der Einschleusung von SQL-Code, Angriffen durch websiteübergreifende Skripts und der Übernahme von Sitzungen.

Azure Application Gateway verwendet einen Load Balancer auf der Schicht 7 (Anwendungsschicht). Das Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Application Gateway bietet zahlreiche ADC-Features (Application Delivery Controller) wie HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität, SSL-Auslagerung (Secure Sockets Layer), benutzerdefinierte Integritätstests und Unterstützung mehrerer Standorte. Eine vollständige Liste der unterstützten Features finden Sie unter [Übersicht über Application Gateway](application-gateway-introduction.md).

Der folgende Artikel zeigt, wie Sie die [Web Application Firewall einem vorhandenen Anwendungsgateway hinzufügen](#add-web-application-firewall-to-an-existing-application-gateway) und [ein Anwendungsgateway erstellen](#create-an-application-gateway-with-web-application-firewall), das die Web Application Firewall verwendet.

![Szenarioabbildung][scenario]

## <a name="prerequisite-install-the-azure-cli-20"></a>Voraussetzung: Installieren der Azure CLI 2.0

Um die Schritte in diesem Artikel ausführen zu können, müssen Sie [die Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows (Azure CLI) installieren](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="waf-configuration-differences"></a>Unterschiede bei der WAF-Konfiguration

Wenn Sie [Erstellen eines Anwendungsgateways mithilfe der Azure CLI 2.0](application-gateway-create-gateway-cli.md) gelesen haben, kennen Sie die SKU-Einstellungen, die beim Erstellen eines Anwendungsgateways konfiguriert werden müssen. Die WAF bietet zusätzliche Einstellungen, die Sie beim Konfigurieren der SKU auf einem Anwendungsgateway definieren müssen. Am Anwendungsgateway selbst sind keine zusätzlichen Änderungen erforderlich.

| **Einstellung** | **Details**
|---|---|
|**SKU** |Ein normales Anwendungsgateway ohne WAF unterstützt die Größen **Standard\_Small**, **Standard\_Medium** und **Standard\_Large**. Mit der Einführung der WAF gibt es zwei zusätzliche SKUs: **WAF\_Medium** und **WAF\_Large**. Die WAF wird auf kleinen Anwendungsgateways nicht unterstützt.|
|**Mode** | Diese Einstellung ist der Modus der WAF. Zulässige Werte sind **Detection** (Erkennung) und **Prevention** (Schutz). Wenn die WAF im Erkennungsmodus eingerichtet wird, werden alle Bedrohungen in einer Protokolldatei gespeichert. Im Schutzmodus werden Ereignisse ebenfalls protokolliert, aber der Angreifer erhält zudem die Antwort 403 (nicht autorisiert) vom Anwendungsgateway.|

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Hinzufügen der Web Application Firewall zu einem vorhandenen Anwendungsgateway

Der folgende Befehl ändert ein vorhandenes Standardanwendungsgateway in ein Anwendungsgateway, für das WAF aktiviert ist.

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

Dieser Befehl aktualisiert das Anwendungsgateway mit der Web Application Firewall. Besuchen Sie [Application Gateway-Diagnose](application-gateway-diagnostics.md), um die Anzeige von Protokollen für Ihr Anwendungsgateway nachzuvollziehen. Aufgrund der Art der WAF-Sicherheit müssen Protokolle regelmäßig überprüft werden, um sich über die Sicherheitslage Ihrer Webanwendungen zu informieren.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Erstellen eines Anwendungsgateways mit der Web Application Firewall

Mit dem folgenden Befehl wird ein Anwendungsgateway mit Web Application Firewall erstellt.

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
> Anwendungsgateways, die mit der Basiskonfiguration der Web Application Firewall erstellt wurden, werden zum Schutz mit CRS 3.0 konfiguriert.

## <a name="get-application-gateway-dns-name"></a>Abrufen des DNS-Namens des Anwendungsgateways

Nach dem Erstellen des Gateways wird das Front-End für die Kommunikation konfiguriert. Wenn Sie eine öffentliche IP-Adresse verwenden, wird das Anwendungsgateway ein dynamisch zugewiesener DNS-Namen benötigt, der kein Anzeigename ist. Um sicherzustellen, dass die Endbenutzer auf das Anwendungsgateway zugreifen können, kann mit einem CNAME-Eintrag auf den öffentlichen Endpunkt des Anwendungsgateways verwiesen werden. [Konfigurieren eines benutzerdefinierten Domänennamens in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md) Rufen Sie zum Konfigurieren eines CNAME-Eintrags mithilfe des PublicIPAddress-Elements, das an das Anwendungsgateway angefügt ist, Details zum Anwendungsgateway und zum zugeordneten IP/DNS-Namen ab. Verwenden Sie den DNS-Namen des Anwendungsgateways zum Erstellen eines CNAME-Eintrags, der die beiden Webanwendungen an diesen DNS-Namen verweist. Die Verwendung von A-Einträgen wird nicht empfohlen, da sich die VIP beim Neustart des Anwendungsgateways möglicherweise verändert.

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

Informationen zum Anpassen von WAF-Regeln finden Sie unter [Customize web application firewall rules through the Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md) (Anpassen von Web Application Firewall-Regeln über die Azure CLI 2.0).

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png

