---
title: "Erstellen eines Azure-Anwendungsgateways – Azure CLI 2.0 | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie mithilfe der Azure CLI 2.0 in Resource Manager ein Anwendungsgateway erstellen.
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 14dab2197ff7c1eaff012066e321ef1b99f05bb3
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---
<a id="create-an-application-gateway-by-using-the-azure-cli-20" class="xliff"></a>

# Erstellen eines Anwendungsgateways mithilfe der Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Klassische Azure PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-Vorlage](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)

Azure Application Gateway verwendet einen Load Balancer auf der Schicht 7 (Anwendungsschicht). Das Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Application Gateway bietet folgende Funktionen für die Anwendungsbereitstellung: HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität, SSL-Auslagerung (Secure Sockets Layer), benutzerdefinierte Integritätstests und Unterstützung für mehrere Websites.

<a id="cli-versions-to-complete-the-task" class="xliff"></a>

## CLI-Versionen zum Durchführen dieser Aufgabe

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

* [Azure CLI 1.0:](application-gateway-create-gateway-cli-nodejs.md) Unsere Befehlszeilenschnittstelle für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
* [Azure CLI 2.0:](application-gateway-create-gateway-cli.md) Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell

<a id="prerequisite-install-the-azure-cli-20" class="xliff"></a>

## Voraussetzung: Installieren der Azure CLI 2.0

Um die Schritte in diesem Artikel ausführen zu können, müssen Sie [die Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows (Azure CLI) installieren](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

> [!NOTE]
> Wenn Sie über kein Azure-Konto verfügen, müssen Sie eines erstellen. Sie können sich [hier](../active-directory/sign-up-organization.md)für eine kostenlose Testversion registrieren.

<a id="scenario" class="xliff"></a>

## Szenario

In diesem Szenario erfahren Sie, wie Sie ein Anwendungsgateway über das Azure-Portal erstellen.

Dieses Szenario umfasst Folgendes:

* Sie erstellen ein Anwendungsgateway für mittlere Nutzungsdauer mit zwei Instanzen.
* Sie erstellen ein virtuelles Netzwerk mit dem Namen AdatumAppGatewayVNET und dem reservierten CIDR-Block 10.0.0.0/16.
* Sie erstellen ein Subnetz mit dem Namen Appgatewaysubnet, für das 10.0.0.0/28 als CIDR-Block verwendet wird.
* Sie konfigurieren ein Zertifikat für die SSL-Auslagerung.

![Beispielszenario][scenario]

> [!NOTE]
> Zusätzliche Konfigurationsschritte für das Anwendungsgateway (u.a. benutzerdefinierte Integritätstests, Back-End-Pool-Adressen und zusätzlichen Regeln) werden nicht während der Erstbereitstellung, sondern nach der Konfiguration des Anwendungsgateways ausgeführt.

<a id="before-you-begin" class="xliff"></a>

## Voraussetzungen

Für Azure Application Gateway ist ein eigenes Subnetz erforderlich. Stellen Sie beim Erstellen eines virtuellen Netzwerks sicher, dass der Adressbereich für mehrere Subnetze ausreicht. Sobald Sie ein Anwendungsgateway in einem Subnetz bereitstellen, können nur zusätzliche Anwendungsgateways zum Subnetz hinzugefügt werden.

<a id="log-in-to-azure" class="xliff"></a>

## Anmelden an Azure

Öffnen Sie die **Microsoft Azure-Eingabeaufforderung**, und melden Sie sich an. 

```azurecli
az login -u "username"
```

> [!NOTE]
> Sie können auch `az login` ohne die Option für die Geräteanmeldung verwenden, die die Eingabe eines Codes unter aka.ms/devicelogin erfordert.

Wenn Sie das vorherige Beispiel eingeben, erhalten Sie einen Code. Navigieren Sie in einem Browser zu „https://aka.ms/devicelogin“, und setzen Sie den Anmeldeprozess fort.

![CMD mit Geräteanmeldung][1]

Geben Sie im Browser den Code ein, den Sie erhalten haben. Sie werden zu einer Anmeldeseite umgeleitet.

![Browser zur Eingabe des Codes][2]

Wenn Sie den Code eingegeben haben, sind Sie angemeldet. Schließen Sie dann den Browser, um mit dem Szenario fortzufahren.

![Erfolgreich angemeldet][3]

<a id="create-the-resource-group" class="xliff"></a>

## Ressourcengruppe erstellen

Vor dem Erstellen des Anwendungsgateways wird eine Ressourcengruppe erstellt, die das Anwendungsgateway enthält. Hier sehen Sie den Befehl:

```azurecli
az resource group create --name myresourcegroup --location "West US"
```

<a id="create-a-virtual-network-and-subnet" class="xliff"></a>

## Erstellen eines virtuellen Netzwerks und des Subnetzes

Sobald die Ressourcengruppe erstellt wurde, wird ein virtuelles Netzwerk für das Anwendungsgateway erstellt.  Im folgenden Beispiel ist der Adressraum 10.0.0.0/16 für das virtuelle Netzwerk definiert, und 10.0.0.0/28 wird für das Subnetz verwendet, wie in den vorangehenden Szenariohinweisen erläutert.

```azurecli
az network vnet create \
--name AdatumAppGatewayVNET \
--address-prefix 10.0.0.0/16 \
--subnet-name Appgatewaysubnet \
--subnet-prefix 10.0.0.0/28 \
--resource-group AdatumAppGateway \
--location eastus
```

<a id="create-the-application-gateway" class="xliff"></a>

## Erstellen des Anwendungsgateways

Sobald das virtuelle Netzwerk und das Subnetz erstellt wurden, sind die Voraussetzungen für das Anwendungsgateway erfüllt. Für den folgenden Schritt sind darüber hinaus ein zuvor exportiertes PFX-Zertifikat und das Kennwort für das Zertifikat erforderlich: Bei den für das Back-End verwendeten IP-Adressen handelt es sich um die IP-Adressen für Ihren Back-End-Server. Diese Werte können entweder private IPs im virtuellen Netzwerk, öffentliche IPs oder vollqualifizierte Domänennamen für Ihre Back-End-Server sein.

```azurecli
az network application-gateway create \
--name AdatumAppGateway \
--location eastus \
--resource-group AdatumAppGatewayRG \
--vnet-name AdatumAppGatewayVNET \
---vnet-address-prefix 10.0.0.0/16 \
--subnet Appgatewaysubnet \
---subnet-address-prefix 10.0.0.0/28 \
--servers 10.0.0.4 10.0.0.5 \
--cert-file /mnt/c/Users/username/Desktop/application-gateway/fabrikam.pfx \
--cert-password P@ssw0rd \
--capacity 2 \
--sku Standard_Small \
--http-settings-cookie-based-affinity Enabled \
--http-settings-protocol Http \
--public-ip-address AdatumAppGatewayPIP \
--frontend-port 443 \
--routing-rule-type Basic \
--http-settings-port 80

```

> [!NOTE]
> Um eine Liste mit Parametern zu erhalten, die während der Erstellung bereitgestellt werden kann, führen Sie den folgenden Befehl aus: **az network application-gateway create --help**.

Mit diesem Beispiel wird ein einfaches Anwendungsgateway mit Standardeinstellungen für Listener, Back-End-Pool, Back-End-HTTP-Einstellungen und Regeln erstellt. Darüber hinaus wird die SSL-Auslagerung konfiguriert. Nach der erfolgreichen Bereitstellung können Sie diese Einstellungen an Ihre Anforderungen anpassen.
Wenn Sie Ihre Webanwendung bereits mit dem Back-End-Pool in den vorherigen Schritten definiert haben, beginnt nach dem Erstellen der Lastenausgleich.

<a id="delete-all-resources" class="xliff"></a>

## Löschen aller Ressourcen

Führen Sie die folgenden Schritte aus, um alle Ressourcen zu löschen, die in diesem Artikel erstellt wurden:

```azurecli
az group delete --name AdatumAppGatewayRG
```
 
<a id="next-steps" class="xliff"></a>

## Nächste Schritte

Unter [Erstellen eines benutzerdefinierten Integritätstests](application-gateway-create-probe-portal.md)

Unter [Konfigurieren der SSL-Auslagerung](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png

