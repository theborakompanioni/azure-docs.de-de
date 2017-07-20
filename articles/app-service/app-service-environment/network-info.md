---
title: "Überlegungen zum Netzwerkbetrieb in einer Azure App Service-Umgebung"
description: "In diesem Artikel wird der ASE-Netzwerkverkehr erläutert. Weiterhin wird darauf eingegangen, wie Sie mit Ihrer ASE NSGs und UDRs festlegen können"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ccompy
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: d19590c23da43e08ea42cd278347e01d87433a58
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017

---
# <a name="networking-considerations-for-an-app-service-environment"></a>Überlegungen zum Netzwerkbetrieb in einer App Service-Umgebung #

## <a name="overview"></a>Übersicht ##

Die [App Service-Umgebung][Intro] (ASE) ist eine Bereitstellung des Azure App Service in einem Subnetz in Ihrem Azure Virtual Network (VNet).  Es gibt zwei Bereitstellungstypen für die ASE:

- _Externe ASE_: macht die gehosteten Apps der ASE für eine über das Internet erreichbare IP-Adresse verfügbar. Weitere Informationen finden Sie unter [Erstellen einer externen ASE][MakeExternalASE].
- _ILB ASE_: macht die gehosteten Apps der ASE für eine IP-Adresse innerhalb Ihres Azure Virtual Networks (VNet) verfügbar.  Der interne Endpunkt ist ein Interner Load Balancer (ILB), daher der Name ILB-ASE. Weitere Informationen finden Sie unter [Erstellen und Verwenden einer ILB-ASE][MakeILBASE].

Es gibt mittlerweile zwei ASE-Versionen. Die erste Version der ASE heißt ASEv1, die neuere Version ASEv2. Informationen zur ASEv1 finden Sie unter [Einführung in die App Service-Umgebung v1][ASEv1Intro]. Eine ASEv1 kann in einem klassischen VNet oder einem Resource Manager-VNet bereitgestellt werden. Ein ASEv2 kann nur in einem Ressourcen-Manager-VNet bereitgestellt werden.

Alle Aufrufe, die von einer ASE ins Internet gehen, verlassen das VNet über eine der ASE zugewiesene VIP-Adresse. Die öffentliche IP dieser VIP ist die Quell-IP für alle Aufrufe, die von der ASE ins Internet gehen.  Wenn die Apps in Ihrer ASE Ressourcen in Ihrem VNet oder über ein VPN aufrufen, ist die Source-IP eine der IPs in dem von Ihrer ASE verwendeten Subnetz.  Da sich die ASE im VNet befindet, hat sie ohne zusätzliche Konfiguration auch Zugriff auf Ressourcen im VNet. Wenn das VNet mit Ihrem lokalen Netzwerk verbunden ist, hat es ohne zusätzliche Konfiguration der ASE oder Ihrer App ebenfalls Zugriff auf die dortigen Ressourcen.

![][1] 

Wenn Sie über eine externe ASE verfügen, ist diese öffentliche VIP auch der Endpunkt, auf den Ihre ASE-Apps für HTTP/S, FTP/S, Web Deploy und Remotedebuggen zurückgreifen.

![][2]

Wenn Sie über eine ILB-ASE verfügen, ist die IP-Adresse der Endpunkt für HTTP/S, FTP/S, Web Deploy und Remotedebuggen.

Die normalen App-Zugriffsports sind:

| Verwenden Sie | Aus | To |
|----------|---------|-------------|
|  HTTP/HTTPS  | Vom Benutzer konfigurierbar |  80, 443 |
|  FTP/FTPS    | Vom Benutzer konfigurierbar |  21, 990, 10001-10020 |
|  Remotedebuggen in Visual Studio  |  Vom Benutzer konfigurierbar |  4016, 4018, 4020, 4022 |

Dies gilt für externe ASEs und ILB-ASEs. In einer externen ASE sind dies die Ports, die Sie mit der öffentlichen VIP wählen können. In einer ILB-ASE treffen Sie diese Ports im ILB. Beachten Sie, dass die Sperrung von Port 443 Auswirkungen auf einige im Portal verfügbaren Features haben kann. Weitere Informationen finden Sie unter [Portalabhängigkeiten](#portaldep).

## <a name="ase-dependencies"></a>ASE-Abhängigkeiten ##

Eine eingehende ASE-Zugriffsabhängigkeit ist:

| Verwenden Sie | Aus | To |
|-----|------|----|
| Verwaltung | Internet | ASE-Subnetz: 454, 455 |
|  Interne ASE-Kommunikation | ASE-Subnetz: alle Ports | ASE-Subnetz: alle Ports
|  Azure Load Balancer eingehend zulassen | Azure Load Balancer | Beliebig

Zusätzlich zur Systemüberwachung ermöglicht der eingehende Datenverkehr Befehl und Steuerung der ASE. Die Quell-IP-Adressen für diesen Datenverkehr sind nicht konstant. Dies bedeutet, dass die Netzwerksicherheitskonfiguration den Zugriff über alle IP-Adressen auf den Ports 454 und 455 zulassen muss.

Eine ASE hängt beim ausgehenden Zugriff von mehreren externen Systemen ab. Diese Systemabhängigkeiten werden über DNS-Namen festgelegt und lassen sich keiner festen IP-Adressengruppe zuordnen. Dies bedeutet, dass die ASE den ausgehenden Zugriff aus dem ASE-Subnetz auf alle externen IP-Adressen über eine Vielzahl von Ports erfordert. Eine ASE weist die folgenden ausgehenden Abhängigkeiten auf:

| Verwenden Sie | Aus | To |
|-----|------|----|
| Azure Storage | ASE-Subnetz | table.core.windows.net, blob.core.windows.net, queue.core.windows.net, file.core.windows.net: 80, 443, 445 (445 wird nur in ASEv1 benötigt) |
| SQL-Datenbank | ASE-Subnetz | database.windows.net: 1433, 11000-11999, 14000-14999 (weitere Informationen finden Sie im Artikel zum [Verwenden der Ports von SQL-Datenbank V12](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md))|
| Azure-Verwaltung | ASE-Subnetz | management.core.windows.net, management.azure.com: 443 
| SSL-Zertifikatprüfung |  ASE-Subnetz            |  ocsp.msocsp.com, mscrl.microsoft.com, crl.microsoft.com: 443
| Azure Active Directory        | ASE-Subnetz            |  Internet: 443
| App Service-Verwaltung        | ASE-Subnetz            |  Internet: 443
| Azure DNS                     | ASE-Subnetz            | Internet: 53
| Interne ASE-Kommunikation    | ASE-Subnetz: alle Ports | ASE-Subnetz: alle Ports

Wenn die ASE nicht mehr auf diese Abhängigkeiten zugreifen kann, ist die ASE nicht mehr funktionsfähig. Nach einer gewissen Zeit wird die ASE angehalten.

**Kunden-DNS**

Wenn das VNet mit einem kundendefinierten DNS-Server konfiguriert ist, wird es von den Workloads des Mandanten verwendet. Zu Verwaltungszwecken muss die ASE auch weiterhin mit dem Azure-DNS kommunizieren können. 

Wenn das VNet mit einem Kunden-DNS auf der anderen Seite eines VPN konfiguriert ist, muss der DNS-Server aus dem Subnetz erreichbar sein, das die ASE enthält.

<a name="portaldep"></a>
## <a name="portal-dependencies"></a>Portalabhängigkeiten ##

Neben den funktionalen Abhängigkeiten einer ASE gibt es einige weitere Elemente in Zusammenhang mit der Portalverwendung. Einige der Funktionen im Azure-Portal hängen vom direkten Zugriff auf den _SCM-Standort_ ab. Zu jeder App im Azure App Service gibt es zwei URLs. Die erste URL dient dem Zugriff auf Ihre App. Die zweite URL dient dem Zugriff auf den SCM-Standort, der auch als _Kudu-Konsole_ bezeichnet wird. Einige der Funktionen, die den SCM-Standort nutzen, sind:

-   Webaufträge
-   Functions
-   Logstream
-   Kudu
-   Erweiterungen
-   Prozess-Explorer
-   Konsole

Dies ist bei der Verwendung einer ILB-ASE höchst anspruchsvoll, da der SCM-Standort außerhalb des VNets nicht über das Internet erreichbar ist. Wenn Ihre App in einer ILB-ASE gehostet wird, sind diese Funktionen, die vom Zugriff auf den SCM-Standort abhängig sind, aus diesem Grund im Azure-Portal ausgegraut.

Viele dieser vom SCM-Standort abhängigen Funktionen sind auch direkt in der Kudu-Konsole verfügbar. Sie können sich mit der Konsole direkt verbinden, anstatt das Portal zu verwenden. Wenn Ihre App in einer ILB-ASE gehostet wird, müssen Sie sich mit Ihren Veröffentlichungsanmeldeinformationen anmelden. Die URL für den Zugriff auf den SCM-Standort einer in einer ILB-ASE gehosteten App besitzt das folgende Format: 

```
<appname>.scm.<domain name the ILB ASE was created with> 
```

Es gilt also: Wenn Ihre ILB-ASE der Domänenname *contoso.net* ist und der Name der App *testapp* lautet, ist „meine App“ auf *testapp.contoso.net* erreichbar, und der zugehörige SCM-Standort ist auf *testapp.scm.contoso.net* erreichbar.

## <a name="ase-ip-addresses"></a>ASE-IP-Adressen ##

Eine ASE muss viele IP-Adressen berücksichtigen. Sie lauten wie folgt:

- Öffentliche eingehende IP-Adresse: für den App-Datenverkehr in einer externen ASE und für den Verwaltungsdatenverkehr sowohl in einer externen ASE als auch in einer ILB-ASE.
- Ausgehende öffentliche IP-Adresse: als „Absender“ für von der ASE ausgehende und das VNet verlassende Verbindungen, die nicht durch ein VPN weitergeleitet werden.
- ILB-IP-Adresse: bei Verwendung einer ILB-ASE.
- Von der App zugewiesene IP-basierte SSL-Adressen: kann nur in einer externen ASE verwendet werden, wenn gleichzeitig auch IP-basiertes SSL konfiguriert ist.

Alle diese IP-Adressen sind von der ASE-Benutzeroberfläche aus auf einfache Weise in der ASEv2 im Azure-Portal sichtbar. Wenn Sie eine ILB-ASE verwenden, wird die IP der ILB angezeigt.

![][3]

**Von der App zugewiesene IP-Adressen**

Mit einer externen ASE können Sie einzelnen Apps IP-Adressen zuweisen. Dies ist mit einer ILB-ASE nicht möglich. Weitere Informationen zum Konfigurieren einer eigenen IP-Adresse für Ihre App finden Sie in [Binden eines vorhandenen benutzerdefinierten SSL-Zertifikats an Azure-Web-Apps](../../app-service-web/app-service-web-tutorial-custom-ssl.md).

Wenn eine App über eine eigene IP-basierte SSL-Adresse verfügt, reserviert die ASE zwei Ports für die Zuordnung zu dieser IP-Adresse. Ein Port wird für den HTTP-Datenverkehr verwendet, der andere für den HTTPS-Datenverkehr. Diese Ports werden im Bereich „IP-Adressen“ der ASE-Benutzeroberfläche angezeigt. Der Datenverkehr muss diese Ports von der VIP aus erreichen können, da sonst nicht auf die Apps zugegriffen werden kann. Beim Konfigurieren der Netzwerksicherheitsgruppen darf dieser wichtige Punkt nicht vergessen werden.

## <a name="network-security-groups"></a>Netzwerksicherheitsgruppen ##

[Netzwerksicherheitsgruppen][NSGs] (NSGs) bieten die Möglichkeit, den Netzwerkzugriff innerhalb eines VNets zu steuern. Wenn Sie das Portal verwenden, gibt es auf der niedrigsten Prioritätsstufe eine implizite Ablehnungsregel, durch die alles abgelehnt wird. Sie erstellen also Ihre eigenen Zulassungsregeln.

In einer ASE haben Sie keinen Zugriff auf die VMs, die zum Hosten der eigentlichen ASE verwendet werden. Diese sind in einem Microsoft-Abonnement enthalten. Wenn Sie den Zugriff auf die Apps in der ASE beschränken möchten, müssen Sie im ASE-Subnetz Netzwerksicherheitsgruppen festlegen. In diesem Fall müssen Sie sorgfältig auf die ASE-Abhängigkeiten achten. Wenn Sie Abhängigkeiten blockieren, ist die ASE nicht mehr funktionsfähig.

NSGs können über das Azure-Portal und über PowerShell konfiguriert werden. Der Einfachheit halber beziehen sich die folgenden Informationen auf das Azure-Portal. NSGs können Sie unter **Netzwerk** im Portal als Ressource der obersten Ebene erstellen.

Unter Berücksichtigung der Anforderungen an den eingehenden und ausgehenden Datenverkehr sollten die NSGs wie folgt aussehen. In diesem Beispiel ist der VNet-Adressbereich _192.168.250.0/16_. Das Subnetz, in dem sich die ASE befindet, ist _192.168.251.128/25_.

Die ersten beiden Anforderungen an den eingehenden Datenverkehr, ohne die die ASE nicht funktionsfähig ist, sind oben in diesem Beispiel genannt. Sie ermöglichen die Verwaltung der ASE und die Kommunikation der ASE mit sich selbst. Die anderen Einträge sind alle vom Mandanten konfigurierbar und können den Netzwerkzugriff auf die in der ASE gehosteten Anwendungen steuern.   

![][4]

Zusätzlich zu einer Standardregel, die den IPs im VNet die Kommunikation mit dem ASE-Subnetz ermöglicht, gibt es eine Standardregel, die das Lastenausgleichsmodul und damit die öffentliche VIP aktiviert, um mit der ASE zu kommunizieren.  Sie können sich die Standardregeln per Klick auf *Standardregeln* neben dem Symbol *Hinzufügen* anzeigen lassen. Wenn Sie nach den angezeigten NSG-Regeln noch eine Ablehnungsregel für alles andere hinzufügen würden, würden Sie den Datenverkehr zwischen der VIP und der ASE unterbinden. Wenn Sie den Datenverkehr aus dem VNet unterbinden möchten, müssen Sie Ihre eigene Regel hinzufügen, die eingehenden Datenverkehr aus einer Quelle ähnlich dem Azure Load Balancer mit einem beliebigen Ziel und einem Portbereich von \* zulässt.  Da die NSG-Regel nur auf das ASE-Subnetz angewendet wird, müssen Sie kein spezifisches Ziel angeben.

Wenn Sie Ihrer App eine IP-Adresse zugewiesen haben, müssen Sie außerdem sicherstellen, dass die entsprechenden Ports geöffnet bleiben. Die verwendeten Ports werden in der **App Service-Umgebung** in der Benutzeroberfläche der  > **IP-Adressen** angezeigt.  

In den unten aufgeführten Regeln für den ausgehenden Datenverkehr werden alle Einträge außer dem letzten benötigt. Durch sie ist der Netzwerkzugriff auf die weiter oben im vorliegenden Dokument beschriebenen ASE-Abhängigkeiten möglich. Wenn Sie eine beliebige Abhängigkeit blockieren, ist die ASE nicht mehr funktionsfähig. Das letzte Listenelement ermöglicht der ASE die Kommunikation mit anderen Ressourcen in Ihrem VNet.

![][5]

Nach der Festlegung Ihrer NSGs müssen diese dem Subnetz zugewiesen werden, in dem sich Ihre ASE befindet. Wenn Sie das ASE-VNet oder -Subnetz vergessen haben, können Sie es sich im Verwaltungsportal der ASE anzeigen lassen. Um die NSG Ihrem Subnetz zuzuweisen, öffnen Sie die Benutzeroberfläche des Subnetzes, und wählen Sie die NSG aus.

## <a name="routes"></a>Routen ##

Routen erweisen sich meistens dann als problematisch, wenn Sie Ihr VNet mit ExpressRoute konfigurieren. In Azure VNet gibt es drei Routentypen. Dabei handelt es sich um:

-   Systemrouten
-   BGP-Routen
-   Benutzerdefinierte Routen (UDR)

BGP-Routen überschreiben Systemrouten. UDRs überschreiben BGP-Routen. Weitere Informationen zu Routen in Azure Virtual Networks finden Sie unter [Benutzerdefinierte Routen – Übersicht][UDRs]

Die von ASE für die Systemverwaltung verwendete SQL-Datenbank verfügt über eine Whitelist und setzt voraus, dass die Kommunikation aus der öffentlichen VIP der ASE stammt. Wenn die Antworten auf die eingehenden Verwaltungsanforderungen an ExpressRoute gesendet werden, unterscheiden sich die Antwort- und die Empfängeradresse, was zum Abbruch der TCP-Kommunikation führt.

Aus diesen Überlegungen ergibt sich die einfachste Vorgehensweise, um eine funktionsfähige ASE zu erhalten, wenn Ihr VNet mit einer ExpressRoute konfiguriert ist:

-   Konfigurieren Sie ExpressRoute so, dass _0.0.0.0/0_ angekündigt wird, wodurch standardmäßig der gesamte ausgehende Datenverkehr zwangsweise lokal getunnelt wird.
-   Erstellen Sie eine UDR, und wenden Sie diese in dem Subnetz an, in dem die App Service-Umgebung liegt. Verwenden Sie hierbei das Adresspräfix _0.0.0.0/0_ sowie _Internet_ als nächsten Hop-Typ.

Wenn Sie diese beiden Änderungen vornehmen, wird der aus dem ASE-Subnetz stammende Datenverkehr ins Internet nicht zwingend über ExpressRoute geleitet, und die ASE ist funktionsfähig. 

> [!IMPORTANT]
> Die in einer UDR definierten Routen müssen ausreichend spezifisch sein, damit sie Vorrang vor allen von der ExpressRoute-Konfiguration angekündigten Routen erhalten. Im letzten Beispiel wird der allgemeine Adressbereich 0.0.0.0/0 verwendet, der durch Routenankündigungen mit spezifischeren Adressbereichen versehentlich überschrieben werden kann.
>

App Service-Umgebungen werden nicht mit ExpressRoute-Konfigurationen unterstützt, die Routen „über Kreuz“ vom öffentlichen Peeringpfad zum privaten Peeringpfad ankündigen. ExpressRoute-Konfigurationen, für die öffentliches Peering konfiguriert ist, erhalten Routenankündigungen von Microsoft für zahlreiche Microsoft Azure-IP-Adressbereiche. Werden diese Adressbereiche über Kreuz auf dem privaten Peeringpfad angekündigt, führt dies dazu, dass alle ausgehenden Netzwerkpakete aus dem Subnetz der App Service-Umgebung zwangsweise zur lokalen Netzwerkinfrastruktur eines Kunden getunnelt werden. Dieser Netzwerkdatenfluss wird derzeit für App Service-Umgebungen nicht unterstützt. Eine Lösung für dieses Problem besteht darin, „Über-Kreuz-Ankündigungen“ von Routen vom öffentlichen Peeringpfad zum privaten Peeringpfad zu verhindern.

So erstellen Sie eine UDR wie beschrieben:

1.  Wechseln Sie im Azure-Portal zu **Netzwerk** > **Routentabellen**.
2.  Erstellen Sie eine neue Routentabelle in derselben Region wie Ihr VNet.
3.  Wählen Sie in der Benutzeroberfläche Ihrer Routentabelle **Routen** > **Hinzufügen**.
4.  Legen Sie als **Typ des nächsten Hops** **Internet** und als **Adresspräfix** _0.0.0.0/0_ fest, und klicken Sie auf **Speichern**.

Nun sollte in etwa Folgendes angezeigt werden:

![][6]

Wechseln Sie nach dem Erstellen der neuen Routentabelle in das Subnetz, in dem sich Ihre ASE befindet. Wählen Sie aus der im Portal angezeigten Liste Ihre Routentabelle aus. Nachdem Sie die Änderung gespeichert haben, sollten die für Ihr Subnetz vermerkten NSGs und Routen angezeigt werden.

![][7]

### <a name="deploying-into-existing-azure-virtual-networks-that-are-integrated-with-expressroute"></a>Bereitstellen in vorhandenen und in ExpressRoute integrierten virtuellen Azure-Netzwerken ###

Wenn Sie Ihre ASE in einem bereits in ExpressRoute integrierten VNet bereitstellen möchten, müssen Sie das Subnetz vorkonfigurieren, in dem die ASE bereitgestellt werden soll. Für die Bereitstellung müssen Sie dann eine Resource Manager-Vorlage verwenden. So erstellen Sie eine ASE in einem Vnet, für das ExpressRoute bereits konfiguriert ist:

- Erstellen Sie ein Subnetz, um die ASE zu hosten.

    > [!NOTE]
    > Das Subnetz darf außer der ASE keine weiteren Elemente enthalten. Die Adressgröße kann später nicht mehr geändert werden und muss daher ausreichend groß für nachfolgende Erweiterungen gewählt werden. Die empfohlene Größe ist `/25` mit 128 Adressen.
- Erstellen Sie UDRs (d.h. Routentabellen) wie zuvor beschrieben, und legen Sie dies im Subnetz entsprechend fest.
- Erstellen Sie die ASE mit einer Resource Manager-Vorlage wie hier beschrieben: [Creating an ASE using an ARM Template (Erstellen einer ASE mit einer ARM-Vorlage)][MakeASEfromTemplate].

<!--Image references-->
[1]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow.png
[2]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow2.png
[3]: ./media/network_considerations_with_an_app_service_environment/networkase-ipaddresses.png
[4]: ./media/network_considerations_with_an_app_service_environment/networkase-inboundnsg.png
[5]: ./media/network_considerations_with_an_app_service_environment/networkase-outboundnsg.png
[6]: ./media/network_considerations_with_an_app_service_environment/networkase-udr.png
[7]: ./media/network_considerations_with_an_app_service_environment/networkase-subnet.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md

