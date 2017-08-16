---
title: "Erstellen eines Anwendungsgateways mit URL-Routingregeln – Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Diese Seite enthält Anweisungen zum Erstellen und Konfigurieren eines Azure Application Gateways mit URL-Routingregeln."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: 958049830d6753ec26635f18f8f8b2fabdec0733
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---
# <a name="create-an-application-gateway-using-path-based-routing-with-azure-cli-20"></a>Erstellen eines Anwendungsgateways mit pfadbasiertem Routing mit Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Mit Routing auf URL-Pfadbasis können Sie Routen basierend auf dem URL-Pfad einer HTTP-Anforderung zuordnen. Es wird überprüft, ob eine Route zu einem Back-End-Pool für die URL konfiguriert ist, die im Anwendungsgateway angezeigt wird, und der Netzwerkverkehr wird an den definierten Back-End-Pool gesendet. Ein gängiges Szenario für URL-basiertes Routing ist der Lastenausgleich von Anforderungen für verschiedene Inhaltstypen auf verschiedene Back-End-Serverpools.

Mit dem URL-basierten Routing wird ein neuer Regeltyp für Anwendungsgateways eingeführt. Anwendungsgateways verfügen über zwei Regeltypen: Basisrouting und PathBasedRouting. Der Basisregeltyp bietet einen Roundrobindienst für die Back-End-Pools, während PathBasedRouting neben der Roundrobinverteilung auch Pfadmuster der Anforderungs-URL beim Auswählen des Back-End-Pools berücksichtigt.

## <a name="scenario"></a>Szenario

Im folgenden Beispiel verarbeitet das Anwendungsgateway Datenverkehr für „contoso.com“ mit zwei Back-End-Serverpools: einem Standardserverpool und einem Imageserverpool.

Anforderungen für „http://contoso.com/image*“ werden an den Imageserverpool (imagesBackendPool) weitergeleitet. Wenn das Pfadmuster nicht übereinstimmt, wird ein Standardserverpool (appGatewayBackendPool) ausgewählt.

![URL-Route](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="log-in-to-azure"></a>Anmelden an Azure

Öffnen Sie die **Microsoft Azure-Eingabeaufforderung**, und melden Sie sich an. 

```azurecli
az login -u "username"
```

> [!NOTE]
> Sie können `az login` auch ohne die Option für die Geräteanmeldung verwenden, die die Eingabe eines Codes unter „aka.ms/devicelogin“ erfordert.

Wenn Sie das vorherige Beispiel eingeben, erhalten Sie einen Code. Navigieren Sie in einem Browser zu „https://aka.ms/devicelogin“, und setzen Sie den Anmeldeprozess fort.

![CMD mit Geräteanmeldung][1]

Geben Sie im Browser den Code ein, den Sie erhalten haben. Sie werden zu einer Anmeldeseite umgeleitet.

![Browser zur Eingabe des Codes][2]

Wenn Sie den Code eingegeben haben, sind Sie angemeldet. Schließen Sie dann den Browser, um mit dem Szenario fortzufahren.

![Erfolgreich angemeldet][3]

## <a name="add-a-path-based-rule-to-an-existing-application-gateway"></a>Hinzufügen einer pfadbasierten Regel zu einem vorhandenen Anwendungsgateway

Erstellen eines Anwendungsgateways mit einer definierten Pfadregel

### <a name="create-a-new-back-end-pool"></a>Erstellen eines neuen Back-End-Pools

Konfigurieren Sie die Anwendungsgatewayeinstellung **imagesBackendPool** für den Netzwerkdatenverkehr mit Lastenausgleich im Back-End-Pool. In diesem Beispiel konfigurieren Sie verschiedene Back-End-Pool-Einstellungen für den neuen Back-End-Pool. Jeder Back-End-Pool kann eine eigene Back-End-Pool-Einstellung aufweisen.  Back-End-HTTP-Einstellungen werden von Regeln verwendet, um Datenverkehr an die richtigen Mitglieder des Back-End-Pools weiterzuleiten. Die Einstellungen bestimmen das Protokoll und den Port, das bzw. der beim Senden von Datenverkehr an die Mitglieder des Back-End-Pools verwendet wird. Cookiebasierte Sitzungen werden ebenfalls durch die Back-End-HTTP-Einstellungen gesteuert.  Im aktivierten Zustand sendet die cookiebasierte Sitzungsaffinität Datenverkehr an das gleiche Back-End wie vorherige Anforderungen für das jeweilige Paket.

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port"></a>Erstellen eines neuen Front-End-Ports

Konfigurieren Sie den Front-End-Port für ein Application Gateway. Das Front-End-Port-Konfigurationsobjekt wird von einen Listener verwendet, um den Port zu definieren, an dem das Anwendungsgateway auf Datenverkehr des Listeners lauscht.

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a>Erstellen eines neuen Listeners

Konfigurieren Sie den Listener. Dieser Schritt konfiguriert den Listener für die öffentliche IP-Adresse und den Port zum Empfangen von eingehendem Netzwerkverkehr. Im folgenden Beispiel wird der Listener mit der zuvor konfigurierten Front-End-IP- und Front-End-Portkonfiguration sowie mit einem Protokoll (HTTP oder HTTPS) konfiguriert. In diesem Beispiel lauscht der Listener an Port 82 auf HTTP-Datenverkehr für die zuvor erstellte öffentliche IP-Adresse.

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-the-url-path-map"></a>Erstellen der URL-Pfadzuordnung

Konfigurieren Sie URL-Regelpfade für die Back-End-Pools. Dieser Schritt konfiguriert den relativen Pfad, der vom Anwendungsgateway verwendet wird, um die Zuordnung zwischen dem URL-Pfad und dem Back-End-Pool zu definieren, der zum Verarbeiten des eingehenden Datenverkehrs zugewiesen wurde.

> [!IMPORTANT]
> Jeder Pfad muss mit „/“ beginnen, und ein „\*“ ist nur am Ende zulässig. Gültige Beispiele sind „/xyz“, „/xyz*“ oder „/xyz/*“. Die Zeichenfolge, die in den Pfadabgleicher eingegeben wird, enthält keinen Text nach dem ersten „?“ oder „#“, und diese Zeichen sind nicht zulässig. 

Im folgenden Beispiel wird eine Regel für den Pfad „/images/*“ erstellt, um Datenverkehr an das Back-End „imagesBackendPool“ zu leiten. Diese Regel stellt sicher, dass der Datenverkehr für jeden Satz URLs an das Back-End weitergeleitet wird. Beispiel: „http://adatum.com/images/figure1.jpg“ wird an „imagesBackendPool“ weitergeleitet. Die Konfiguration der Regelpfadzuordnung konfiguriert auch dann einen Standard-Back-End-Adresspool, wenn der Pfad keiner der vordefinierten Pfadregeln entspricht. Beispiel: „http://adatum.com/shoppingcart/test.html“ wird an „pool1“ weitergeleitet, da dieser als Standardpool für nicht übereinstimmenden Datenverkehr definiert wurde.

```azurecli-interactive
az network application-gateway url-path-map create \
--gateway-name AdatumAppGateway \
--name imagespathmap \
--paths /images/* \
--resource-group myresourcegroup2 \
--address-pool imagesBackendPool \
--default-address-pool appGatewayBackendPool \
--default-http-settings appGatewayBackendHttpSettings \
--http-settings appGatewayBackendHttpSettings \
--rule-name images
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zur SSL-Auslagerung (Secure Sockets Layer) finden Sie unter [Konfigurieren eines Anwendungsgateways für die SSL-Auslagerung](application-gateway-ssl-cli.md).


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png

