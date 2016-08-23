<properties
   pageTitle="Einführung in Application Gateway | Microsoft Azure"
   description="Diese Seite bietet eine Übersicht über den Application Gateway-Dienst für Lastenausgleich der Ebene 7, einschließlich Gatewaygrößen, HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität und SSL-Auslagerung."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="gwallace"/>

# Übersicht über Application Gateway

Microsoft Azure Application Gateway bietet eine von Azure verwaltete HTTP-Lastenausgleichslösung, die auf Lastenausgleich der HTTP-Ebene 7 basiert.

Durch Anwendungslastenausgleich können IT-Administratoren und Entwickler Routingregeln für Netzwerkverkehr basierend auf HTTP erstellen. Der Application Gateway-Dienst bietet hohe Verfügbarkeit und gute Kontrolle. Die Vereinbarung zum Servicelevel und die Preise finden Sie auf den Seiten [SLA](https://azure.microsoft.com/support/legal/sla/) und [Preise](https://azure.microsoft.com/pricing/details/application-gateway/).

Application Gateway unterstützt derzeit die Anwendungsbereitstellung der Ebene 7 für Folgendes:

- HTTP-Lastenausgleich
- Cookiebasierte Sitzungsaffinität
- [Secure Sockets Layer (SSL)-Auslagerung](application-gateway-ssl-arm.md)
- [URL-basiertes Inhaltsrouting](application-gateway-url-route-overview.md)
- [Multisiterouting](application-gateway-multi-site-overview.md)

## Lastenausgleich der HTTP-Ebene 7

Azure bietet einen Ebene-4-Lastenausgleich über Azure Load Balancer, der auf der Transportebene (TCP/UDP) eingesetzt wird und den Lastenausgleich des gesamten eingehenden Netzwerkverkehrs für den Application Gateway-Dienst übernimmt. Das Anwendungsgateway wendet die Routingregeln auf den HTTP-Datenverkehr an und ermöglicht so Lastenausgleich auf Ebene 7 (HTTP). Wenn Sie ein Anwendungsgateway erstellen, wird ein Endpunkt (VIP) zugeordnet und als öffentliche IP-Adresse für eingehenden Netzwerkverkehr verwendet.

Das Anwendungsgateway leitet den HTTP-Datenverkehr auf Grundlage der Konfiguration weiter: virtueller Computer, Clouddienst, Web-App oder eine externe IP-Adresse.

Lastenausgleich der HTTP-Ebene 7 eignet sich für:

- Anwendungen, bei denen Anforderungen einer Benutzer-/Clientsitzung den gleichen virtuellen Back-End-Computer erreichen müssen. Beispiele für solche Anwendungen wären Einkaufswagen-Apps und E-Mail-Webserver.
- Anwendungen, die für Webserverfarmen den Mehraufwand für die SSL-Beendigung unterbinden möchten.
- Anwendungen, wie z.B. ein Content Delivery Network, für die mehrere HTTP-Anforderungen über die gleiche lange bestehende TCP-Verbindung an verschiedene Back-End-Server weitergeleitet werden bzw. für die dort ein Lastenausgleich erfolgen muss.

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]


## Gatewaygrößen und -instanzen

Application Gateway wird derzeit in drei Größen angeboten: klein, mittel und groß. Kleine Instanzen sind für Entwicklungs- und Testszenarien vorgesehen.

Sie können bis zu 50 Application Gateways pro Abonnement erstellen, und jedes Application Gateway kann jeweils bis zu 10 Instanzen aufweisen. Der Application Gateway-Lastenausgleich als ein von Azure verwalteter Dienst ermöglicht die Bereitstellung eines Lastenausgleichs der Ebene 7 hinter dem Azure Load Balancer.

Die folgende Tabelle zeigt einen durchschnittlichen Leistungsdurchsatz für jede Anwendungsgatewayinstanz:


| Back-End-Seitenantwort | Klein | Mittel | Groß|
|---|---|---|---|
| 6000 | 7,5 MBit/s | 13 MBit/s | 50 MBit/s |
|100\.000 | 35 MBit/s | 100 MBit/s| 200 MBit/s |


>[AZURE.NOTE] Hierbei handelt es sich um ungefähre Werte für den Durchsatz des Anwendungsgateways. Der tatsächliche Durchsatz ist abhängig von verschiedenen Umgebungsdetails wie etwa durchschnittliche Seitengröße, Speicherort der Back-End-Instanzen und Verarbeitungszeit für die Seitenbereitstellung.


## Systemüberwachung

Azure Application Gateway überprüft die Integrität der Back-End-Instanzen automatisch. Weitere Informationen finden Sie unter [Systemüberwachung von Application Gateway – Übersicht](application-gateway-probe-overview.md).





## Konfigurieren und Verwalten

Sie können ein Application Gateway mithilfe von REST-APIs und PowerShell-Cmdlets erstellen und verwalten.


## Nächste Schritte

Nachdem Sie sich mit Anwendungsgateways vertraut gemacht haben, können Sie [ein Anwendungsgateway erstellen](application-gateway-create-gateway-portal.md) oder [ein Anwendungsgateway für SSL-Auslagerung erstellen](application-gateway-ssl-arm.md), um einen Lastenausgleich für HTTPS-Verbindungen durchzuführen.

Weitere Informationen zum Erstellen eines Anwendungsgateways mit URL-basiertem Inhaltsrouting finden Sie unter [Erstellen eines Anwendungsgateways mit URL-basiertem Routing](application-gateway-create-url-route-arm-ps.md).

<!---HONumber=AcomDC_0810_2016-->