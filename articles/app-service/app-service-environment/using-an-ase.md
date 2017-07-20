---
title: Verwenden einer Azure App Service-Umgebung
description: "Anleitung für das Erstellen, Veröffentlichen und Skalieren in einer Azure App Service-Umgebung"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a22450c4-9b8b-41d4-9568-c4646f4cf66b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: c24f716d58f534d1439377234fa1263269961db7
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017

---
# <a name="using-an-app-service-environment"></a>Verwenden einer App Service-Umgebung #

## <a name="overview"></a>Übersicht ##

Eine App Service-Umgebung (ASE) ist eine Bereitstellung des Azure App Service in einem Subnetz im Azure Virtual Network (VNet) des Kunden. Sie besteht aus:

- Front-Ends: Hier endet HTTP/HTTPS in einer ASE
- Worker: Dabei handelt es sich um die Ressourcen, die Ihre Apps hosten.
- Datenbank: Die Datenbank enthält Informationen, durch die die Umgebung festgelegt ist.
- Speicher: Der Speicher wird zum Hosten der vom Kunden veröffentlichten Apps verwendet.

> [!NOTE]
> Es gibt zwei Versionen der App Service-Umgebung: ASEv1 und ASEv2. In ASEv1 müssen Sie die Ressourcen verwalten, bevor Sie sie verwenden können. Informationen zum Konfigurieren und Verwalten einer ASEv1 finden Sie unter [Konfigurieren einen App Service-Umgebung V1][ConfigureASEv1]. Der Rest dieses Dokuments konzentriert sich auf ASEv2.
>
>

Sie können eine ASE (ASEv1 und ASEv2) mit einer externen oder internen VIP für den App-Zugriff bereitstellen. Die Bereitstellung mit einer externen VIP wird im Allgemeinen als externe ASE bezeichnet, die interne Version aufgrund der Verwendung eines internen Lastenausgleichs (ILB) als ILB-ASE. Weitere Informationen zur ILB-ASE finden Sie unter [Verwenden einer ILB-ASE][MakeILBASE].

## <a name="create-a-web-app-in-an-ase"></a>Erstellen einer Web-App in einer ASE ##

Das Erstellen einer Web-App in einer ASE folgt demselben Prozess wie das normale Erstellen. Es gibt nur wenige kleine Unterschiede. Beim Erstellen eines neuen App Service-Plans:

- Anstatt einen geografischen Standort auszuwählen, an dem Sie Ihre App bereitstellen, wählen Sie eine ASE als Ihren Standort aus
- Alle in einer ASE erstellen App Service-Pläne müssen im Tarif „Isoliert“ enthalten sein.

Wenn Sie nicht über eine ASE verfügen, können Sie eine erstellen. Folgen Sie hierzu den Anweisungen in [Erstellen einer App Service-Umgebung][MakeExternalASE].

So erstellen Sie eine Web-App in einer ASE:

1. Klicken Sie auf **Neu &gt; Web und Mobil****Web-App**.
2. Geben Sie einen Namen für die Web-App an. Wenn Sie bereits einen App Service-Plan in einer ASE ausgewählt haben, entspricht der Domänenname der App dem Domänenname der ASE.

    ![][1]

1. Wählen Sie ein Abonnement aus.
2. Geben Sie einen Namen für eine neue Ressourcengruppe an, oder wählen Sie **Vorhandene verwenden**, und wählen Sie eine Ressourcengruppe aus der Dropdownliste aus.
3. Wählen Sie einen in Ihrer ASE vorhandenen App Service-Plan, oder erstellen Sie mit den folgenden Schritten einen neuen:
    1. Wählen Sie **Neu erstellen**.
    2. Geben Sie einen Namen für Ihren App Service-Plan ein.
    3. Wählen Sie Ihre ASE aus der Dropdownliste der **Speicherorte** aus.
    4. Wählen Sie einen **Isolierten** Tarif. Klicken Sie auf **Auswählen**.
    5. Klicken Sie auf **Okay**.
    
    ![][2]
1. Klicken Sie auf **Erstellen**.

## <a name="how-scale-works"></a>Skalieren ##

Jede App Service-App wird in einem App Service-Plan ausgeführt. Das Containermodell ist: Umgebungen beinhalten App Service-Pläne, und App Service-Pläne beinhalten Apps. Wenn Sie eine App skalieren, skalieren Sie den App Service-Plan und damit alle Apps in diesem Plan.

Wenn Sie in einer ASEv2 einen App Service-Plan skalieren, wird die erforderliche Infrastruktur automatisch hinzugefügt. Dies ist ein Unterschied zu ASEv1, wo die erforderliche Infrastruktur vor dem Erstellen oder Skalieren Ihres App Service-Plans hinzugefügt werden muss. Für ASEv2 bedeutet dies, dass es zu einer Zeitverzögerung bei der Skalierung von Vorgängen kommt, weil die Infrastruktur hinzugefügt wird.

Im mehrinstanzenfähigen App Service erfolgt die Skalierung unmittelbar, weil es dort einen sofort verfügbaren Ressourcenpool für die Unterstützung der horizontalen Skalierung gibt. In einer ASE gibt es keinen solchen Puffer. Die Ressourcen werden nach Bedarf zugeordnet.

In einer ASE können bis zu 100 Instanzen skaliert werden. Diese 100 Instanzen können alle zu demselben App Service-Plan gehören oder über mehrere App Service-Pläne verteilt sein.

## <a name="ip-addresses"></a>IP-Adressen ##

In App Service gibt es die Möglichkeit, eine dedizierte IP-Adresse an eine App zu vergeben. Diese Funktion ist per Konfigurieren einer IP-basierten SSL verfügbar. Dies wird hier beschrieben: [Binden eines vorhandenen benutzerdefinierten SSL-Zertifikats an Azure-Web-Apps][ConfigureSSL]. Bei ASEs gibt es jedoch eine wichtige Ausnahme: In eine ILB-ASE kann keine zusätzliche IP-Adresse für die IP-basierte SSL hinzugefügt werden.

In ASEv1 müssen die IP-Adressen vor der Verwendung als Ressourcen zugewiesen werden. In ASEv2 können Sie die IP-Adressen einfach wie im mehrinstanzenfähigen App Service aus der App heraus verwenden. In einer ASEv2 mit bis zu 30 IP-Adressen ist immer eine freie Adresse vorhanden. Bei jeder Verwendung einer IP-Adresse wird eine weitere hinzugefügt, weswegen immer eine sofort verfügbare IP-Adresse zur Verfügung steht. Bei der Vergabe einer weiteren IP-Adresse kommt es zu einer zeitlichen Verzögerung, weswegen IP-Adressen nicht in schneller Abfolge hinzugefügt werden können.

## <a name="front-end-scaling"></a>Front-End-Skalierung ##

Beim horizontalen Skalieren Ihrer App Service-Pläne in einer ASEv2 werden die Worker zur Unterstützung der App Service-Pläne automatisch hinzugefügt. Jede ASE wird mit zwei Front-Ends erstellt, und diese werden mit einem Front-End pro 15 Instanzen automatisch horizontal skaliert. Bei 15 Instanzen bedeutet dies, dass Sie drei Front-Ends haben. Wenn Sie auf 30 Instanzen skalieren, haben Sie vier Front-Ends usw.

Für die meisten Szenarios sollte dies mehr als ausreichend sein. Wenn schneller horizontal skaliert werden muss, können Sie das Verhältnis in Ihren App Service-Plänen aber bis auf ein Front-End pro fünf Instanzen reduzieren. Für das Verändern des Verhältnisses fallen Gebühren an. Diese werden in [Azure App Service-Preise][Pricing] beschrieben.

Front-End-Ressourcen sind die HTTP/HTTPS-Endpunkte der ASE. Mit der standardmäßigen Front-End-Konfiguration beträgt der Speicherverbrauch pro Front-End konstant etwa 60 Prozent. Kundenworkloads können nicht in einem Front-End ausgeführt werden. Der wichtigste Skalierungsfaktor für ein Front-End ist die CPU, die hauptsächlich vom HTTPS-Datenverkehr getrieben ist.

## <a name="app-access"></a>App-Zugriff ##

In einer externen ASE unterscheidet sich die beim Erstellen der ASE verwendete Domäne von dem mehrinstanzenfähigen App Service und enthält den Namen der ASE. Weitere Informationen zum Erstellen einer externen ASE finden Sie in [Erstellen einer App Service-Umgebung][MakeExternalASE]. In einer externen ASE folgt der Domänenname dem folgenden Muster: *.&lt;Name der ASE&gt;.p.azurewebsites.net*. Wenn Sie z.B. in einer ASE mit dem Namen _external-ase_ arbeiten und in dieser ASE eine App mit dem Namen _contoso_ hosten, können Sie über die folgenden URLs auf diese zugreifen:

- contoso.external-ase.p.azurewebsites.net
- contoso.scm.external-ase.p.azurewebsites.net

Die URL *contoso.scm.external-ase.p.azurewebsites.net* wird für den Zugriff auf die Kudu-Konsole und zum Veröffentlichen Ihrer App mit Web Deploy verwendet. Informationen zur Kudu-Konsole finden Sie in [Kudu-Konsole für Azure App Service][Kudu]. Die Kudu-Konsole bietet Ihnen eine Web-Benutzeroberfläche für das Debuggen, Hochladen und Bearbeiten von Dateien und vieles mehr.

In einer ILB-ASE legen Sie die Domäne und die Bereitstellungszeit fest. Weitere Informationen zum Erstellen einer ILB-ASE finden Sie in [Erstellen und Verwenden einer ILB-ASE][MakeILBASE]. Wenn Sie z.B. den Domänennamen _Ilb-ase.info_ angeben, verwenden die Apps in dieser ASE während der App-Erstellung diese Domäne. Für die App mit dem Namen _contoso_ würden die URLs wie folgt lauten:

- contoso.ilb-ase.info
- contoso.scm.ilb-ase.info

## <a name="publishing"></a>Veröffentlichung ##

Neben dem mehrinstanzenfähigen App Service können Sie auch auf folgende Weise in einer ASE veröffentlichen:

- Web Deploy
- FTP
- Continuous Integration
- Drag and Drop in der Kudu-Konsole
- Eine IDE wie z.B. Visual Studio, Eclipse oder Intellij IDEA

In einer externen ASE ist das Verhalten jeweils dasselbe. Weitere Informationen finden Sie in [Bereitstellen in Azure App Service][AppDeploy]. 

Der große Unterschied bei der Veröffentlichung betrifft die ILB-ASEs. In einer ILB-ASE sind die Veröffentlichungsendpunkte nur über die ILB verfügbar. Die ILB liegt in einer privaten IP im ASE-Subnetz im virtuellen Netzwerk. Wenn Sie keinen Netzwerkzugriff auf die ILB haben, können Sie in der betreffenden ASE keine Apps veröffentlichen. Wie in [Erstellen und Verwenden einer ILB-ASE][MakeILBASE] erwähnt, müssen Sie für die Apps im System ein DNS konfigurieren. Dazu gehört der SCM-Endpunkt. Wenn DNS und SCM-Endpunkt nicht korrekt definiert sind, können Sie nicht veröffentlichen. Darüber hinaus müssen Ihre IDEs über Netzwerkzugriff auf den ILB verfügen, um direkt an ihn veröffentlichen zu können.

Internetbasierte CI-Systemen wie Github und VSTS können nicht mit einer ILB-ASE verwendet werden, weil der Veröffentlichungsendpunkt nicht über das Internet erreichbar ist. Stattdessen müssen Sie ein CI-System wie Dropbox verwenden, das ein Pull-Modell verwendet.

Die Veröffentlichungsendpunkte für die Apps in einer ILB-ASE verwenden die Domäne, mit der die ILB-ASE erstellt wurde. Dies wird im Veröffentlichungsprofil und im Portalblatt der App angezeigt (in **Übersicht** > **Zusammenfassung** und in den **Eigenschaften**). 

## <a name="pricing"></a>Preise ##

Es gibt eine neue Preis-SKU, die nur für ASEv2 gilt und **Isoliert** heißt. Alle App Service-Pläne, die in einer ASEv2 gehostet werden, befinden sich in der Preis-SKU „Isoliert“. Zusätzlich zum Preis Ihrer App Service-Pläne fällt eine Pauschalgebühr für die eigentliche ASE an. Dieser Preis richtet sich nicht nach der Größe Ihrer ASE. 

Die sonstigen potenziellen Gebühren beziehen sich auf die Anpassung des Front-End-Skalierungsverhältnisses oder der Größe des Front-Ends. Wenn Sie zwecks schnelleren Hinzufügens von Front-Ends das Skalierungsverhältnis erhöhen, zahlen Sie für die zusätzlichen Kerne, die sonst nicht automatisch zum System hinzugefügt worden wären. Analog dazu zahlen Sie bei Erhöhung Ihrer Front-End-Größe für die Kerne, die nicht automatisch zugeordnet wurden. Wenn Sie z.B. das Skalierungsverhältnis auf den Wert 10 anpassen, bedeutet dies, dass pro zehn Instanzen ein Front-End Ihren App Service-Plänen hinzugefügt wird. Die Pauschalgebühr deckt ein Skalierungsverhältnis von einem Front-End pro 15 Instanzen ab. Mit einem Skalierungsverhältnis von 10 zahlen Sie eine Gebühr für das dritte Front-End, das zu den zehn ASP-Instanzen hinzugefügt wird. Wenn 15 Instanzen erreicht werden, fällt dagegen keine Gebühr an, weil das Front-End automatisch hinzugefügt worden wäre.

Weitere Informationen finden Sie in [Azure App Service – Preise][Pricing].

## <a name="deleting-an-ase"></a>Löschen einer ASE ##

Wenn Sie eine App Service-Umgebung löschen möchten, verwenden Sie einfach die Aktion **Löschen** im oberen Bereich des Blatts „App Service-Umgebung“. Wenn Sie dies tun, werden Sie aufgefordert, den Namen Ihrer App Service-Umgebung einzugeben. Auf diese Weise bestätigen Sie, dass Sie diesen Schritt wirklich ausführen möchten. Beachten Sie Folgendes: Beim Löschen einer App Service-Umgebung wird auch ihr gesamter Inhalt gelöscht. 

![][3]

<!--Image references-->
[1]: ./media/using_an_app_service_environment/usingase-appcreate.png
[2]: ./media/using_an_app_service_environment/usingase-pricingtiers.png
[3]: ./media/using_an_app_service_environment/usingase-delete.png


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

