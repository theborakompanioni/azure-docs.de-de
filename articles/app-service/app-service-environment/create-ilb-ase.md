---
title: Erstellen und Verwenden eines internen Lastenausgleichs mit einer Azure App Service-Umgebung
description: Einzelheiten zum Erstellen und Verwenden einer vom Internet isolierten Azure App Service-Umgebung
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 8bc82a297ef8e3f1beac773a6a3b0c4a9b80334f
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017

---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a>Erstellen und Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung #

Die App Service-Umgebung (ASE) ist eine Bereitstellung des Azure App Service in einem Subnetz in Ihrem Azure Virtual Network (VNet). Es gibt zwei Möglichkeiten, eine ASE bereitzustellen: 

- Mit einer VIP unter einer externen IP-Adresse, die häufig als _externe ASE_ bezeichnet wird.
- Mit der VIP unter einer internen IP-Adresse, die häufig als _ILB-ASE_ bezeichnet wird, da der interne Endpunkt ein interner Lastenausgleich (ILB) ist. 

In diesem Artikel wird veranschaulicht, wie Sie eine _ILB-ASE_ erstellen.  Als Übersicht zur ASE empfiehlt sich für den Einstieg der Artikel [Einführung in die App Service-Umgebung][Intro]; wenn Sie erfahren möchten, wie eine externe ASE erstellt wird, beginnen Sie mit [Erstellen einer externen ASE][MakeExternalASE].

## <a name="overview"></a>Übersicht ##

Eine ASE kann mit einem Endpunkt, auf den über das Internet zugegriffen werden kann, oder einer IP-Adresse in Ihrem virtuellen Netzwerk bereitgestellt werden. Um die IP-Adresse auf eine Adresse des virtuellen Netzwerks festzulegen, muss die ASE mit einem ILB bereitgestellt werden. Beim Bereitstellen der ASE mit einem ILB müssen Sie Folgendes angeben:

-   Ihre eigene Domäne, mit der Apps erstellt werden
-   das für HTTPS verwendete Zertifikat
-   DNS-Verwaltung für die Domäne

Im Gegenzug können Sie z.B. Folgendes tun:

-   Intranetanwendungen sicher in der Cloud hosten, auf die Sie über ein Site-to-Site- oder ExpressRoute-VPN zugreifen
-   Apps in der Cloud hosten, die nicht im öffentlichen DNS-Server aufgeführt sind
-   vom Internet isolierte Back-End-Apps erstellen, in die sich Ihre Front-End-Apps sicher integrieren können

***Deaktivierte Funktionen***

Einige Möglichkeiten sind bei einer ILB-ASE nicht gegeben:

-   Verwenden von IP-basiertem SSL
-   Zuweisen von IP-Adressen an bestimmte Apps
-   Kaufen und Verwenden eines Zertifikats mit einer App über das Portal. Sie können weiterhin Zertifikate direkt bei einer Zertifizierungsstelle erhalten und sie mit Ihren Apps verwenden, nicht nur über das Azure-Portal.

## <a name="create-an-ilb-ase"></a>Erstellen einer ILB-ASE ##

So erstellen Sie eine ILB-ASE:

1.  Wählen Sie im Azure-Portal **Neu -&gt; Web und Mobil -&gt; App Service-Umgebung** aus.
2.  Wählen Sie Ihr Abonnement aus.
3.  Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie sie.
4.  Erstellen oder wählen Sie ein virtuelles Netzwerk aus.
5.  Erstellen Sie bei Auswahl eines virtuellen Netzwerks ein Subnetz. Die Größe des Subnetzes sollte auf einen ausreichend großen Wert festgelegt werden, um das zukünftige Wachstum Ihrer ASE abzudecken. Die empfohlene Größe ist `/25` mit 128 Adressen zur Verarbeitung einer ASE maximaler Größe. Sie können keine Subnetzgröße von `/29` oder kleiner verwenden.  In der Infrastruktur müssen mindestens 5 Adressen verwendet werden.  Selbst bei einem Wert von `/28` würden Sie über eine maximale Skalierung von 11 Instanzen in einem `/28`-Subnetz verfügen. Wenn Sie der Meinung sind, dass Sie später das standardmäßige Maximum von 100 Instanzen in Ihren App Service-Plänen überschreiten oder eine Skalierung auf nahezu 100 Instanzen bei schnellerer Front-End-Skalierung vornehmen werden, verwenden Sie ein /24-Subnetz mit 256 Adressen.
6.  Wählen Sie **Virtuelles Netzwerk/Speicherort -&gt; VNET-Konfiguration** aus, und legen Sie den **VIP-Typ** auf **Intern** fest.
7.  Geben Sie den Domänennamen an. Diese Domäne wird für Apps verwendet, die in dieser ASE erstellt werden. Es gelten einige Einschränkungen. Die folgenden Domänennamen sind unzulässig:
    - net
    - azurewebsites.net
    - p.azurewebsites.net
    - &lt;ASE-Name&gt;.p.azurewebsites.net

    Zudem gilt Folgendes: Wenn Sie einen benutzerdefinierten Domänennamen für die Apps angeben möchten, die mit dieser ASE gehostet werden, darf keine Überlappung zwischen dem benutzerdefinierten Domänennamen und dem von der ASE verwendeten Domänennamen vorliegen. Für eine ILB-ASE mit dem Domänennamen _contoso.com_ können Sie also keine benutzerdefinierten Domänennamen für die Apps verwenden, die wie folgt aussehen:
    - www.contoso.com
    - abcd.def.contoso.com
    - abcd.contoso.com

    Wenn Ihnen die benutzerdefinierten Domänennamen bekannt sind, die Sie mit den in der ILB-ASE bereitgestellten Apps verwenden möchten, wählen Sie eine Domäne für die ILB-ASE während der ILB-ASE-Erstellung aus, die keinen Konflikt bewirkt. In diesem Beispiel könnte der Domänenname etwa _contoso-internal.com_ lauten.
8.  Wählen Sie **OK** und dann **Erstellen** aus.

    ![][1]

Im Blatt „Virtuelles Netzwerk“ steht die Option **VNET-Konfiguration** zur Verfügung. Hier können Sie zwischen einer externen und einer internen VIP-Adresse wählen. Der Standardwert ist **Extern**. Wenn Sie **Extern** auswählen, verwendet Ihre ASE eine über das Internet zugängliche VIP. Wählen Sie hingegen **Intern** aus, wird die ASE mit einer ILB für eine IP-Adresse in Ihrem virtuellen Netzwerk konfiguriert.

Wenn Sie **Intern** ausgewählt haben, steht die Möglichkeit, der ASE weitere IP-Adressen hinzuzufügen, nicht mehr zur Verfügung; stattdessen müssen Sie die Domäne der ASE angeben. In einer ASE mit einer externen VIP-Adresse wird der Name der ASE in der Domäne für Apps verwendet, die in dieser ASE erstellt werden.

Wenn Sie **VIP-Typ** auf **Intern** festlegen, wird der ASE-Name nicht in der Domäne für die ASE verwendet. Sie geben die Domäne explizit an. Wenn die Domäne ***contoso.corp.net*** heißt und Sie in dieser ASE eine App mit dem Namen ***timereporting*** erstellen, lautet die URL für diese App ***timereporting.contoso.corp.net***.

## <a name="apps-in-an-ilb-ase"></a>Apps in einer ILB-ASE ##

Das Erstellen einer App in einer ILB-ASE entspricht dem normalen Erstellen einer App in einer ASE.

1.  Wählen Sie im Azure-Portal **Neu &gt; Web und Mobil &gt; Web** oder **Mobil** bzw. **API-App** aus.
2.  Geben Sie den Namen der App ein.
3.  Wählen Sie das Abonnement aus.
4.  Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine.
5.  Wählen Sie einen App Service-Plan aus, oder erstellen Sie einen. Wenn Sie einen neuen App Service-Plan erstellen möchten, wählen Sie Ihre ASE als Speicherort aus, und wählen Sie den Workerpool aus, in dem der App Service-Plan erstellt werden soll. Beim Erstellen des App Service-Plans wählen Sie die ASE als Speicherort und den Workerpool aus. Wenn Sie den Namen der App angeben, sehen Sie, dass die Domäne unter Ihrem App-Namen durch die Domäne für Ihre ASE ersetzt wird.
6.  Klicken Sie auf **Erstellen**. Wählen Sie das Kontrollkästchen **An das Dashboard anheften** aus, wenn die App auf dem Dashboard angezeigt werden soll.

    ![][2]

Unter dem App-Namen wird der Name der Domäne aktualisiert, um die Domäne Ihrer ASE anzuzeigen.

## <a name="post-ilb-ase-creation-validation"></a>Überprüfung nach der ILB-ASE-Erstellung ##

Eine ILB-ASE unterscheidet sich etwas von einer Nicht-ILB-ASE. Wie bereits erwähnt, müssen Sie Ihren eigenen DNS verwalten und auch Ihr eigenes Zertifikat für HTTPS-Verbindungen bereitstellen.

Nach dem Erstellen Ihrer ASE werden Sie bemerken, dass der von Ihnen angegebene Domänenname angezeigt wird und dass das Menü **Einstellung** die neue Option **ILB-Zertifikat** enthält. Die ASE wird mit einem Zertifikat erstellt, das nicht die ILB-ASE-Domäne angibt. Verwenden Sie die ASE mit diesem Zertifikat, teil Ihnen Ihr Browser mit, dass das Zertifikat ungültig ist. Dieses Zertifikat erleichtert das Testen von HTTPS, es ist jedoch zu erwarten, dass Sie das eigene, an die ILB-ASE-Domäne gebundene Zertifikat hochladen müssen, wobei es keine Rolle spielt, ob es selbstsigniert ist oder von einer Zertifizierungsstelle bezogen wurde.

![][3]

Es gibt viele Möglichkeiten, ein gültiges SSL-Zertifikat zu beschaffen, z.B. interne Zertifizierungsstellen, Erwerb eines Zertifikats von einem externen Aussteller und Verwendung eines selbstsignierten Zertifikats. Unabhängig von der Quelle des SSL-Zertifikats müssen die folgenden Zertifikatattribute richtig konfiguriert werden:

-   _Antragsteller:_ Dieses Attribut muss auf _\*.ihre-stammdomäne-hier_ festgelegt werden.
-   _Alternativer Antragstellername:_ Dieses Attribut muss sowohl _\*.ihre-stammdomäne-hier_ als auch _\*.scm.ihre-stammdomäne-hier_ enthalten. Der Grund für den zweiten Eintrag ist, dass für SSL-Verbindungen mit der SCM/Kudu-Website, die jeder App zugeordnet ist, eine Adresse im Format _ihr-app-name.scm.ihre-stammdomäne-hier_ verwendet wird.

Wenn ein gültiges SSL-Zertifikat vorhanden ist, sind zwei weitere Vorbereitungsschritte erforderlich. Das SSL-Zertifikat muss in eine PFX-Datei konvertiert bzw. in diesem Format gespeichert werden. Beachten Sie, dass die PFX-Datei alle Zwischen- und Stammzertifikate enthalten und mit einem Kennwort geschützt werden muss.

Wenn Sie Ihr eigenes Zertifikat mithilfe von PowerShell erstellen möchten, können Sie die untenstehenden Befehle verwenden.  Sie müssen den ILB-ASE-Domänennamen anstelle von *internal.contoso.com* verwenden. 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

Das mit diesen PowerShell-Befehlen generierte Zertifikat wird immer noch durch Browser gekennzeichnet, da es nicht von einer Zertifizierungsstelle erstellt wurde, die sich in der vom Browser verwendeten Zertifikatskette befindet. Ein Zertifikat, das vom Browser nicht als problematisch angesehen wird, können Sie nur erhalten, wenn Sie es von einer kommerziellen Zertifizierungsstelle erwerben, die in der Zertifikatkette des Browsers enthalten ist.  

![][4]

So laden Sie eigene Zertifikate hoch und prüfen den Zugriff:

1.  Wechseln Sie nach der ASE-Erstellung zur ASE-Benutzeroberfläche (**ASE > Einstellungen > ILB-Zertifikate**).
2.  Legen Sie das ILB-Zertifikat durch Auswahl der Zertifikat-PFX-Datei und Angabe des Kennworts fest. Dieser Schritt nimmt etwas Zeit in Anspruch, und es wird die Meldung angezeigt, dass eine Uploadvorgang ausgeführt wird.
3.  Rufen Sie die ILB-Adresse für Ihre ASE ab (**ASE > Eigenschaften > Virtuelle IP-Adresse**).
4.  Erstellen Sie nach der ASE-Erstellung eine Web-App in der ASE.
5.  Erstellen Sie einen virtuellen Computer, sofern im virtuellen Netzwerk noch keiner vorhanden ist.

    > [!NOTE] 
    > Erstellen Sie diesen virtuellen Computer nicht im selben Subnetz wie die ASE. Andernfalls bewirkt dies Fehler im Setup.
    >
    >

6.  Legen Sie das DNS für Ihre ASE-Domäne fest. Sie können einen Platzhalter mit Ihrer Domäne im DNS verwenden, oder wenn Sie einige einfache Tests durchführen möchten, bearbeiten Sie die Hostdatei auf Ihrem virtuellen Computer, um den Web-App-Namen auf die VIP-IP-Adresse festzulegen. Wenn Ihre ASE den Domänennamen _.ilbase.com_ aufweist und Sie die Web-App _mytestapp_ erstellen, wird sie unter _mytestapp.ilbase.com_ adressiert. Anschließen legen Sie _mytestapp.ilbase.com_ so fest, dass eine Auflösung in die ILB-Adresse erfolgt. (Unter Windows befindet sich die Hostdatei unter „_C:\Windows\System32\drivers\etc\_“.) Wenn Sie Web Deploy-Publishing oder den Zugriff auf die erweiterte Konsole testen möchten, müssen Sie auch einen Eintrag für _mytestapp.scm.ilbase.com_ erstellen.
7.  Navigieren Sie in einem Browser auf dieser VM zu ***http://mytestapp.ilbase.com*** (bzw. zum festgelegten Web-App-Namen der Domäne).
8.  Navigieren Sie in einem Browser auf dieser VM zu ***https://mytestapp.ilbase.com***. Bei Verwendung eines selbstsignierten Zertifikats müssen Sie eine Sicherheitsbeeinträchtigung hinnehmen.

Die IP-Adresse für Ihre ILB wird unter **IP-Adressen** aufgelistet. Hier sind auch die von der internen VIP und für eingehenden Datenverkehr für die Verwaltung verwendeten IP-Adressen aufgeführt.

![][5]

### <a name="functions-and-the-ilb-ase"></a>Funktionen und die ILB-ASE

Bei Verwendung von Funktionen für eine ILB-ASE kann ein Fehler mit folgender Meldung auftreten: *„Wir können Ihre Funktionen zurzeit nicht abrufen.  Versuchen Sie es später erneut.“*  Dies liegt daran, dass die Funktionen-Benutzeroberfläche die SCM-Site über HTTPS nutzt.  Eine solche Situation kann eintreten, wenn Sie ein HTTP-Zertifikat ohne Stammzertifikat im Browser für Ihre ASE verwenden.  Zudem wird die Einstellung *accept-invalid-cert* in den Browsern Internet Explorer und Edge nicht für mehrere Registerkarten genutzt. Sie haben jedoch folgende Möglichkeiten:

- Sie können das Zertifikat dem vertrauenswürdigen Zertifikatspeicher hinzufügen, 
- oder Sie können Chrome verwenden; in diesem Fall müssen Sie jedoch zuerst die SCM-Site aufrufen, das nicht vertrauenswürdige Zertifikat akzeptieren und anschließend zum Portal wechseln.

## <a name="dns-configuration"></a>DNS-Konfiguration ##

Bei Verwendung einer externen VIP-Adresse wird das DNS mithilfe von Azure verwaltet. Jede in Ihrer ASE erstellte App wird automatisch dem Azure-DNS – einem öffentlichen DNS – hinzugefügt. In einer ILB-ASE müssen Sie einen eigenen DNS verwalten. Für eine bestimmte Domäne, wie z.B. _contoso.net_, müssen Sie DNS-A-Einträge im DNS erstellen, die auf Ihre ILB-Adresse für Folgendes verweisen:

- *.contoso.net
- *.scm.contoso.net

Wenn Ihre ILB-ASE-Domäne außerhalb dieser ASE für verschiedene Zwecke verwendet wird, müssen Sie DNS möglicherweise nach einzelnen App-Namen verwalten. Dies ist sehr viel aufwändiger, da Sie den Namen jeder neuen App bei der Erstellung im DNS hinzufügen müssten. Daher wird die Verwendung einer dedizierten Domäne empfohlen.

## <a name="publishing-with-an-ilb-ase"></a>Veröffentlichung mit einer ILB-ASE ##

Für jede erstellte App sind zwei Endpunkte vorhanden. In einer ILB-ASE sind sowohl *&lt;App-Name>.&lt;ILB-ASE-Domäne>* als auch *&lt;App-Name>.scm.&lt;ILB-ASE-Domäne>* vorhanden. 

Über den Namen der SCM-Site gelangen Sie zur Kudu-Konsole, die im Azure-Portal als **Advanced Portal** (Erweitertes Portal) bezeichnet wird. An der Kudu-Konsole können Sie eine Vielzahl von Vorgängen ausführen; Sie können u.a. die Umgebungsvariablen anzeigen, den Datenträger untersuchen und eine Konsole verwenden. Weitere Informationen hierzu finden Sie unter [Kudu-Konsole für Azure App Service][Kudu]. 

Im mehrinstanzenfähigen App Service und in einer externen ASE gilt einmaliges Anmelden zwischen Azure-Portal und der Kudu-Konsole. Für die ILB-ASE müssen Sie jedoch beim Anmelden bei der Kudu-Konsole stattdessen Ihre Anmeldeinformationen für die Veröffentlichung verwenden.

Internetbasierte CI-Systemen wie Github und VSTS können nicht mit einer ILB-ASE verwendet werden, weil der Veröffentlichungsendpunkt nicht über das Internet erreichbar ist. Stattdessen müssen Sie ein CI-System wie Dropbox verwenden, das ein Pull-Modell verwendet.

Die Veröffentlichungsendpunkte für die Apps in einer ILB-ASE verwenden die Domäne, mit der die ILB-ASE erstellt wurde. Dies wird im Veröffentlichungsprofil und im Portalblatt der App angezeigt (in **Übersicht** > **Zusammenfassung** und in den **Eigenschaften**). Wenn Sie über eine ILB-ASE mit der Unterdomäne *contoso.net* und einer App mit dem Namen *mytest* verfügen, stellen Sie eine FTP-Verbindung mit *mytest.contoso.net* her und führen den Web Deploy-Vorgang in *mytest.scm.contoso.net* aus.

## <a name="coupling-an-ilb-ase-with-a-waf-device"></a>Koppeln einer ILB-ASE mit einem WAF-Gerät ##

Azure App Service bietet eine Vielzahl von Sicherheitsmaßnahmen, mit denen das System geschützt und festgestellt wird, ob eine App gehackt wurde. Der beste Schutz für eine Web-App besteht darin, eine Hosting-Plattform wie Azure App Service mit einer Web Application Firewall (WAF) zu koppeln. Da die ILB-ASE über einen vom Netzwerk isolierten Anwendungsendpunkt verfügt, eignet sie sich hervorragend für eine solche Nutzung.  

Weitere Informationen zum Konfigurieren Ihrer ILB-ASE mit einem WAF-Gerät finden Sie unter [Konfigurieren einer Web Application Firewall (WAF) für eine App Service-Umgebung][ASEWAF]. In diesem Artikel wird die Verwendung eines virtuellen Barracuda-Geräts mit Ihrer ASE erläutert. Eine weitere Möglichkeit besteht darin, das Azure Application Gateway zu verwenden. Das Application Gateway sichert mithilfe der OWASP-Kernregeln alle dahinter platzierten Anwendungen. Weitere Informationen zum Application Gateway von Azure finden Sie in der Einführung zur [Web Application Firewall (WAF)][AppGW].

### <a name="getting-started"></a>Erste Schritte ###

Alle Artikel und Anleitungen zu App Service-Umgebungen stehen in der [Dokumentation zur App Service-Umgebung][ASEReadme] zur Verfügung.

Informationen zu den ersten Schritten mit App Service-Umgebungen finden Sie unter [Einführung in App Service-Umgebungen][Intro].

Weitere Informationen zur Azure App Service-Plattform finden Sie unter [Azure App Service](http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/).
 
<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
[3]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate.png
[4]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate2.png
[5]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-ipaddresses.png

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

