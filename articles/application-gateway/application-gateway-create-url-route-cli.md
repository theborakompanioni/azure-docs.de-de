---
title: "Erstellen eines Anwendungsgateways mit URL-Routingregeln – Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Diese Seite enthält Anweisungen zum Erstellen und Konfigurieren eines Anwendungsgateways mit URL-Routingregeln."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: c617006bcb122cd3191f5da3ff08191e3c55b04b
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="create-an-application-gateway-by-using-path-based-routing-with-azure-cli-20"></a>Erstellen eines Anwendungsgateways mit pfadbasiertem Routing mit der Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Mit Routing auf URL-Pfadbasis ordnen Sie Routen basierend auf dem URL-Pfad der HTTP-Anforderung zu. Es wird überprüft, ob eine Route zu einem Back-End-Serverpool für die URL konfiguriert ist, die im Anwendungsgateway aufgeführt wird, und der Netzwerkdatenverkehr wird dann an den definierten Pool gesendet. Ein gängiges Szenario für Routing auf URL-Pfadbasis ist der Lastenausgleich von Anforderungen für verschiedene Inhaltstypen auf verschiedene Back-End-Serverpools.

Azure Application Gateway nutzt zwei Regeltypen: Basisregeln und Regeln auf URL-Pfadbasis. Der Basisregeltyp stellt einen Roundrobin-Dienst für die Back-End-Pools bereit. Pfadbasierte Regeln nutzen für die Auswahl des entsprechenden Back-End-Pools zusätzlich zur Roundrobin-Verteilung das Pfadmuster der Anforderungs-URL.

## <a name="scenario"></a>Szenario

Im folgenden Beispiel verarbeitet das Anwendungsgateway Datenverkehr für „contoso.com“ mit zwei Back-End-Serverpools: einem Standardserverpool und einem Imageserverpool.

Anforderungen für „http://contoso.com/image*“ werden an den Imageserverpool (**imagesBackendPool**) geleitet. Wenn die Pfadmuster nicht übereinstimmen, wählt das Anwendungsgateway den Standardserverpool (**appGatewayBackendPool**).

![URL-Route](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Öffnen Sie die **Microsoft Azure-Eingabeaufforderung**, und melden Sie sich an:

```azurecli
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

## <a name="add-a-path-based-rule-to-an-existing-application-gateway"></a>Hinzufügen einer pfadbasierten Regel zu einem vorhandenen Anwendungsgateway

Die folgenden Schritte veranschaulichen das Hinzufügen einer pfadbasierten Regel zu einem vorhandenen Anwendungsgateway.
### <a name="create-a-new-back-end-pool"></a>Erstellen eines neuen Back-End-Pools

Konfigurieren Sie die Anwendungsgatewayeinstellung **imagesBackendPool** für den Netzwerkdatenverkehr mit Lastenausgleich im Back-End-Pool. In diesem Beispiel konfigurieren Sie verschiedene Back-End-Pool-Einstellungen für den neuen Back-End-Pool. Jeder Back-End-Pool kann eigene Einstellung aufweisen. Pfadbasierte Regeln verwenden Back-End-HTTP-Einstellungen, um Datenverkehr an die richtigen Mitglieder des Back-End-Pools weiterzuleiten. Die Einstellungen bestimmen das Protokoll und den Port, das bzw. der beim Senden von Datenverkehr an die Mitglieder des Back-End-Pools verwendet wird. Die Back-End-HTTP-Einstellungen steuern auch cookiebasierte Sitzungen.  Im aktivierten Zustand sendet die cookiebasierte Sitzungsaffinität Datenverkehr an das gleiche Back-End wie vorherige Anforderungen für das jeweilige Paket.

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port-for-an-application-gateway"></a>Erstellen eines neuen Front-End-Ports für ein Anwendungsgateway

Das Front-End-Port-Konfigurationsobjekt wird von einen Listener verwendet, um den Port zu definieren, an dem das Anwendungsgateway auf Datenverkehr des Listeners lauscht.

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a>Erstellen eines neuen Listeners

Dieser Schritt konfiguriert den Listener für die öffentliche IP-Adresse und den Port zum Empfangen von eingehendem Netzwerkverkehr. Im folgenden Beispiel wird der Listener mit der zuvor konfigurierten Front-End-IP- und Front-End-Portkonfiguration sowie mit einem Protokoll (http oder https, jeweils mit Beachtung der Groß-/Kleinschreibung) konfiguriert. In diesem Beispiel lauscht der Listener an Port 82 auf HTTP-Datenverkehr für die zuvor in diesem Szenario erstellte öffentliche IP-Adresse.

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-the-url-path-map"></a>Erstellen der URL-Pfadzuordnung

Dieser Schritt konfiguriert den relativen URL-Pfad, der vom Anwendungsgateway verwendet wird, um die Zuordnung zwischen dem Pfad und dem Back-End-Pool zu definieren, der zum Verarbeiten des eingehenden Datenverkehrs zugewiesen wurde.

> [!IMPORTANT]
> Jeder Pfad muss mit einem Schrägstrich (/) beginnen, und ein Sternchen ist nur am Ende zulässig. Gültige Beispiele sind „/xyz“, „/xyz*“ oder „/xyz/*“. Die Zeichenfolge, die in den Pfadabgleicher eingegeben wird, enthält keinen Text nach dem ersten „?“ oder „#“, und diese Zeichen sind nicht zulässig. 

Im folgenden Beispiel wird eine Regel für den Pfad „/images/*“ erstellt, um Datenverkehr an das Back-End **imagesBackendPool** zu leiten. Diese Regel stellt sicher, dass der Datenverkehr für jeden Satz URLs an das Back-End weitergeleitet wird. Beispiel: „http://adatum.com/images/figure1.jpg“ wird an **imagesBackendPool** weitergeleitet. Die Konfiguration der Regelpfadzuordnung konfiguriert auch dann einen Standard-Back-End-Adresspool, wenn der Pfad keiner der vordefinierten Pfadregeln entspricht. Beispiel: „http://adatum.com/shoppingcart/test.html“ wird an **pool1** weitergeleitet, da dieser als Standardpool für nicht übereinstimmenden Datenverkehr definiert wurde.

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

