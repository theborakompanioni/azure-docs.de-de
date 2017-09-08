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
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: e7f85aaf2d940f114248d5925a1e97fe0f6bda6c
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a>Erstellen und Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung #

 Die Azure App Service-Umgebung ist eine Bereitstellung von Azure App Service in einem Subnetz in einem virtuellen Azure-Netzwerk (VNET). Es gibt zwei Möglichkeiten für die Bereitstellung einer App Service-Umgebung (App Service Environment, ASE): 

- Mit einer VIP unter einer externen IP-Adresse, die häufig als „externe ASE“ bezeichnet wird
- Mit einer VIP unter einer internen IP-Adresse, die häufig als „ILB-ASE“ bezeichnet wird, da der interne Endpunkt ein interner Lastenausgleich (ILB) ist. 

In diesem Artikel wird veranschaulicht, wie Sie eine ILB-ASE erstellen. Eine Übersicht über die ASE finden Sie unter [Einführung in App Service-Umgebungen][Intro]. Informationen zum Erstellen einer externen ASE finden Sie unter [Erstellen einer externen ASE][MakeExternalASE].

## <a name="overview"></a>Übersicht ##

Sie können eine ASE mit einem Endpunkt, auf den über das Internet zugegriffen werden kann, oder einer IP-Adresse in Ihrem VNET bereitstellen. Um die IP-Adresse auf eine VNET-Adresse festzulegen, muss die ASE mit einem ILB bereitgestellt werden. Beim Bereitstellen der ASE mit einem ILB müssen Sie Folgendes angeben:

-   Ihre eigene Domäne, die Sie für die Erstellung Ihrer Apps verwenden
-   Das für HTTPS verwendete Zertifikat
-   Die DNS-Verwaltung für Ihre Domäne

Im Gegenzug können Sie z.B. Folgendes tun:

-   Intranetanwendungen sicher in der Cloud hosten, auf die Sie über ein Site-to-Site- oder Azure ExpressRoute-VPN zugreifen
-   Apps in der Cloud hosten, die nicht in öffentlichen DNS-Servern aufgeführt sind
-   Vom Internet isolierte Back-End-Apps erstellen, in die Ihre Front-End-Apps sicher integriert werden können

### <a name="disabled-functionality"></a>Deaktivierte Funktionen ###

Es gibt einige Dinge, die Sie mit einer ILB-ASE nicht machen können:

-   IP-basiertes SSL verwenden
-   Bestimmten Apps IP-Adressen zuweisen
-   Ein Zertifikat mit einer App über das Azure-Portal kaufen und verwenden. Sie können Zertifikate direkt von einer Zertifizierungsstelle beziehen und mit Ihren Apps verwenden. Über das Azure-Portal können diese nicht bezogen werden.

## <a name="create-an-ilb-ase"></a>Erstellen einer ILB-ASE ##

So erstellen Sie eine ILB-ASE:

1. Wählen Sie im Azure-Portal **Neu** > **Web und mobil** > **App Service-Umgebung** aus.

2. Wählen Sie Ihr Abonnement aus.

3. Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie sie.

4. Wählen Sie ein VNET aus, oder erstellen Sie eines.

5. Wenn Sie ein vorhandenes VNET auswählen, müssen Sie ein Subnetz erstellen, das die ASE beinhaltet. Die Größe des Subnetzes sollte auf einen ausreichend großen Wert festgelegt werden, um das zukünftige Wachstum Ihrer ASE abzudecken. Empfohlen wird eine Größe von `/25` mit 128 Adressen zur Verarbeitung einer ASE in maximaler Größe. Die kleinste Größe, die Sie auswählen können, beträgt `/28`. Diese Größe kann je nach den Infrastrukturanforderungen auf maximal 11 Instanzen skaliert werden.

    * Skalieren Sie in Ihren App Service-Plänen über die maximale Anzahl von 100 Instanzen hinaus.

    * Skalieren Sie auf bis zu 100 Instanzen, profitieren Sie jedoch von einer schnelleren Front-End-Skalierung.

6. Wählen Sie **Virtuelles Netzwerk/Speicherort** >  **Konfigurationen virtueller Netzwerke**. Legen Sie den **VIP-Typ** auf **Intern** fest.

7. Geben Sie einen Domänennamen ein. Diese Domäne wird für Apps verwendet, die in dieser ASE erstellt werden. Es gelten einige Einschränkungen. Der Domänenname darf nicht folgendermaßen lauten:

    * net   

    * azurewebsites.net

    * p.azurewebsites.net

    * &lt;ASE-Name&gt;.p.azurewebsites.net

   Der für Apps verwendete benutzerdefinierte Domänenname und der von Ihrer ASE verwendete Domänenname dürfen sich nicht überschneiden. Für eine ILB-ASE mit dem Domänennamen _contoso.com_ können Sie keine benutzerdefinierten Domänennamen für beispielsweise folgende Apps verwenden:

    * www.contoso.com

    * abcd.def.contoso.com

    * abcd.contoso.com

   Wenn Sie die benutzerdefinierten Domänennamen für Ihre Apps kennen, wählen Sie eine Domäne für die ILB-ASE, die keinen Konflikt mit diesen benutzerdefinierten Domänennamen verursacht. In diesem Beispiel können Sie z.B. *contoso-internal.com* für die Domäne Ihrer ASE verwenden, da sie keinen Konflikt mit benutzerdefinierten Domänennamen, die auf *.contoso.com* enden, verursacht.

8. Wählen Sie **OK** und anschließend **Erstellen**.

    ![ASE-Erstellung][1]

Auf dem Blatt **Virtuelles Netzwerk** befindet sich die Option **Konfiguration virtueller Netzwerke**. Mit dieser können Sie eine externe oder interne VIP auswählen. Der Standardwert ist **Extern**. Wenn Sie **Extern** auswählen, verwendet Ihre ASE eine über das Internet zugängliche VIP. Wenn Sie **Intern** auswählen, wird Ihre ASE mit einem ILB unter einer IP-Adresse in Ihrem VNET konfiguriert.

Wenn Sie **Intern** auswählen, wird die Option zum Hinzuzufügen weiterer IP-Adressen zu Ihrer ASE ausgeblendet. Stattdessen müssen Sie die Domäne der ASE angeben. In einer ASE mit einer externen VIP-Adresse wird der Name der ASE in der Domäne für Apps verwendet, die in dieser ASE erstellt werden.

Wenn Sie **VIP-Typ** auf **Intern** festlegen, wird der ASE-Name nicht in der Domäne für die ASE verwendet. Sie geben die Domäne explizit an. Wenn Ihre Domäne *contoso.corp.net* heißt und Sie in dieser ASE eine App mit dem Namen *timereporting* erstellen, lautet die URL für diese App „timereporting.contoso.corp.net“.


## <a name="create-an-app-in-an-ilb-ase"></a>Erstellen einer App in einer ILB-ASE ##

Sie erstellen eine App in einer ILB-ASE auf dieselbe Weise, wie Sie eine App normalerweise in einer ASE erstellen würden.

1. Wählen Sie im Azure-Portal **Neu** > **Web und mobil** > **Web** oder **Mobil** bzw. **API-App** aus.

2. Geben Sie den Namen der App ein.

3. Wählen Sie das Abonnement aus.

4. Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie sie.

5. Wählen Sie einen App Service-Plan aus, oder erstellen Sie einen. Wenn Sie einen neuen App Service-Plan erstellen möchten, wählen Sie Ihre ASE als Speicherort aus. Wählen Sie den Workerpool aus, in dem Ihr App Service-Plan erstellt werden soll. Wählen Sie beim Erstellen des App Service-Plans die ASE als Speicherort und den Workerpool aus. Wenn Sie den Namen der App angeben, wird die Domäne unter Ihrem App-Namen durch die Domäne für Ihre ASE ersetzt.

6. Klicken Sie auf **Erstellen**. Wenn die App auf dem Dashboard angezeigt werden soll, aktivieren Sie das Kontrollkästchen **An Dashboard anheften**.

    ![Erstellung eines App Service-Plans][2]

    Unter **App-Name** wird der Domänenname entsprechend der Domäne Ihrer ASE aktualisiert.

## <a name="post-ilb-ase-creation-validation"></a>Überprüfung nach der ILB-ASE-Erstellung ##

Eine ILB-ASE unterscheidet sich etwas von einer Nicht-ILB-ASE. Wie bereits erwähnt müssen Sie Ihr eigenes DNS verwalten. Zudem müssen Sie Ihr eigenes Zertifikat für HTTPS-Verbindungen angeben.

Nachdem Sie Ihre ASE erstellt haben, wird der Domänenname entsprechend der von Ihnen angegebenen Domäne angezeigt. Im Menü **Einstellung** wird ein neues Element namens **ILB-Zertifikat** angezeigt. Die ASE wird mit einem Zertifikat erstellt, das nicht die ILB-ASE-Domäne angibt. Verwenden Sie die ASE mit diesem Zertifikat, teilt Ihnen Ihr Browser mit, dass das Zertifikat ungültig ist. Dieses Zertifikat ermöglicht ein einfacheres Testen von HTTPS, allerdings müssen Sie Ihr eigenes Zertifikat hochladen, das an Ihre ILB-ASE-Domäne gekoppelt ist. Dieser Schritt ist erforderlich, ganz gleich, ob das Zertifikat selbstsigniert ist oder von einer Zertifizierungsstelle bezogen wurde.

![ILB-ASE-Domänenname][3]

Für Ihre ILB-ASE ist ein gültiges SSL-Zertifikat erforderlich. Verwenden Sie interne Zertifizierungsstellen, erwerben Sie ein Zertifikat von einem externen Aussteller, oder verwenden Sie ein selbstsigniertes Zertifikat. Unabhängig von der Quelle des SSL-Zertifikats müssen die folgenden Zertifikatattribute richtig konfiguriert werden:

* **Antragsteller**: Dieses Attribut muss auf „*.ihre-stammdomäne-hier“ festgelegt werden.
* **Alternativer Antragstellername**: Dieses Attribut muss sowohl **.ihre-stammdomäne-hier* als auch **.scm.ihre-stammdomäne-hier* enthalten. SSL-Verbindungen mit der SCM/Kudu-Website, die jeder App zugeordnet ist, verwenden eine Adresse im Format *ihr-app-name.scm.ihre-stammdomäne-hier*.

Konvertieren Sie das SSL-Zertifikat in eine PFX-Datei, und speichern Sie es. Die PFX-Datei muss alle Zwischenzertifikate und Stammzertifikate enthalten. Sichern Sie es mit einem Kennwort.

Wenn Sie ein selbstsigniertes Zertifikat erstellen möchten, können Sie hier die PowerShell-Befehle verwenden. Sie müssen anstelle von *internal.contoso.com* den ILB-ASE-Domänennamen verwenden: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

Das mit diesen PowerShell-Befehlen generierte Zertifikat wird durch Browser gekennzeichnet, da es nicht von einer Zertifizierungsstelle erstellt wurde, die sich in der vom Browser verwendeten Zertifikatskette befindet. Ein Zertifikat, dem Ihr Browser vertraut, erhalten Sie von einer kommerziellen Zertifizierungsstelle in der Vertrauenskette Ihres Browsers. 

![Festlegen des ILB-Zertifikats][4]

So laden Sie eigene Zertifikate hoch und prüfen den Zugriff:

1. Wechseln Sie nach der Erstellung der ASE zur ASE-Benutzeroberfläche. Wählen Sie **ASE** > **Einstellungen** > **ILB-Zertifikat**.

2. Legen Sie das ILB-Zertifikat durch Auswahl der Zertifikat-PFX-Datei und Eingabe des Kennworts fest. Dieser Schritt kann eine Weile dauern. Es wird eine Meldung angezeigt, die besagt, dass ein Uploadvorgang ausgeführt wird.

3. Rufen Sie die ILB-Adresse für Ihre ASE ab. Wählen Sie **ASE** > **Eigenschaften** > **Virtuelle IP-Adresse** aus.

4. Erstellen Sie nach der ASE-Erstellung eine Web-App in der ASE.

5. Erstellen Sie eine VM, sofern im VNET noch keine vorhanden ist.

    > [!NOTE] 
    > Versuchen Sie nicht, diese VM im selben Subnetz wie die der ASE zu erstellen, da dies Fehler bzw. Probleme verursacht.
    >
    >

6. Legen Sie das DNS für Ihre ASE-Domäne fest. Sie können einen Platzhalter mit Ihrer Domäne in Ihrem DNS verwenden. Um einige einfache Tests auszuführen, bearbeiten Sie die Hostdatei auf Ihrer VM, um den Namen der Web-App auf die virtuelle IP-Adresse festzulegen:

    a. Wenn Ihre ASE den Domänennamen _.ilbase.com_ aufweist und Sie die Web-App _mytestapp_ erstellen, wird sie unter _mytestapp.ilbase.com_ adressiert. Anschließend legen Sie _mytestapp.ilbase.com_ so fest, dass eine Auflösung in die ILB-Adresse erfolgt. (Unter Windows befindet sich die Hostdatei unter „_C:\Windows\System32\drivers\etc\_“.)

    b. Um Web Deploy-Publishing oder den Zugriff auf die erweiterte Konsole zu testen, erstellen Sie einen Datensatz für _mytestapp.scm.ilbase.com_.

7. Navigieren Sie in einem Browser auf dieser VM zu „http://mytestapp.ilbase.com“. (Navigieren Sie alternativ zum Namen Ihrer Web-App, der in Ihrer Domäne verwendet wird.)

8. Navigieren Sie in einem Browser auf dieser VM zu „https://mytestapp.ilbase.com“. Wenn Sie ein selbstsigniertes Zertifikat verwenden, müssen Sie eine Beeinträchtigung der Sicherheit in Kauf nehmen.

    Die IP-Adresse für Ihren ILB wird unter **IP-Adressen** aufgelistet. In dieser Liste sind auch die von der externen VIP und für eingehenden Datenverkehr für die Verwaltung verwendeten IP-Adressen aufgeführt.

    ![ILB-IP-Adresse][5]

## <a name="web-jobs-functions-and-the-ilb-ase"></a>Webaufträge, Funktionen und die ILB-ASE ##

Funktionen und Webaufträge werden auf einer ILB-ASE unterstützt. Damit das Portal jedoch mit ihnen arbeiten kann, benötigen Sie Netzwerkzugriff auf die SCM-Site.  Das bedeutet, dass Ihr Browser sich auf einem Host befinden muss, der sich im virtuellen Netzwerk befindet oder mit diesem verbunden ist.  

Bei Verwendung von Azure Functions für eine ILB-ASE kann ein Fehler mit folgender Meldung auftreten: „Wir können Ihre Funktionen zurzeit nicht abrufen. Versuchen Sie es später noch mal.“ Dieser Fehler tritt auf, da die Benutzeroberfläche von Functions über HTTPS auf die SCM-Site zugreift und das Stammzertifikat nicht in der Vertrauenskette des Browsers enthalten ist. Webaufträge weisen ein ähnliches Problem auf. Um dieses Problem zu vermeiden können Sie eine der folgenden Aktionen ausführen:

- Zertifikat dem Zertifikatspeicher für vertrauenswürdige Anbieter hinzufügen Dies hebt die Blockierung von Edge und Internet Explorer auf.
- Verwenden Sie Chrome, und rufen Sie zuerst die SCM-Website auf. Akzeptieren Sie das nicht vertrauenswürdige Zertifikat, und wechseln Sie anschließend zum Portal.
- Verwenden Sie ein kommerzielles Zertifikat, das in der Vertrauenskette Ihres Browsers enthalten ist.  Dies ist die beste Option.  

## <a name="dns-configuration"></a>DNS-Konfiguration ##

Wenn Sie eine externe VIP verwenden, wird das DNS von Azure verwaltet. Jede in Ihrer ASE erstellte App wird automatisch dem Azure-DNS – einem öffentlichen DNS – hinzugefügt. In einer ILB-ASE müssen Sie einen eigenen DNS verwalten. Für eine bestimmte Domäne, wie z.B. _contoso.net_, müssen Sie DNS-A-Einträge im DNS erstellen, die auf Ihre ILB-Adresse für Folgendes verweisen:

- *.contoso.net
- *.scm.contoso.net

Wenn Ihre ILB-ASE-Domäne außerhalb dieser ASE für verschiedene Zwecke verwendet wird, müssen Sie das DNS möglicherweise nach einzelnen App-Namen verwalten. Diese Methode ist mit Aufwand verbunden, da Sie den Namen jeder neuen App bei der Erstellung im DNS hinzufügen müssen. Aus diesem Grund wird empfohlen, eine dedizierte Domäne zu verwenden.

## <a name="publish-with-an-ilb-ase"></a>Veröffentlichen mit einer ILB-ASE ##

Für jede erstellte App sind zwei Endpunkte vorhanden. In einer ILB-ASE sind sowohl *&lt;App-Name>.&lt;ILB-ASE-Domäne>* als auch *&lt;App-Name>.scm.&lt;ILB-ASE-Domäne>* vorhanden. 

Über den Namen der SCM-Website gelangen Sie zur Kudu-Konsole, die im Azure-Portal als **Erweitertes Portal** bezeichnet wird. An der Kudu-Konsole können Sie u.a. die Umgebungsvariablen anzeigen, den Datenträger untersuchen und eine Konsole verwenden. Weitere Informationen hierzu finden Sie unter [Kudu-Konsole für Azure App Service][Kudu]. 

Im mehrinstanzenfähigen App Service und in einer externen ASE ist einmaliges Anmelden zwischen dem Azure-Portal und der Kudu-Konsole eingerichtet. Für die ILB-ASE müssen Sie jedoch beim Anmelden bei der Kudu-Konsole Ihre Anmeldeinformationen für die Veröffentlichung verwenden.

Internetbasierte CI-Systeme wie GitHub und Visual Studio Team Services können nicht mit einer ILB-ASE verwendet werden, da der Veröffentlichungsendpunkt nicht über das Internet erreichbar ist. Stattdessen müssen Sie ein CI-System wie Dropbox verwenden, das ein Pull-Modell verwendet.

Die Veröffentlichungsendpunkte für die Apps in einer ILB-ASE verwenden die Domäne, mit der die ILB-ASE erstellt wurde. Diese Domäne wird im Veröffentlichungsprofil und auf dem Portalblatt der App angezeigt (unter **Übersicht** > **Zusammenfassung** sowie unter **Eigenschaften**). Wenn Sie über eine ILB-ASE mit der Unterdomäne *contoso.net* und einer App mit dem Namen *mytest* verfügen, verwenden Sie *mytest.contoso.net* als FTP-Verbindung und *mytest.scm.contoso.net* für den Web Deploy-Vorgang.

## <a name="couple-an-ilb-ase-with-a-waf-device"></a>Koppeln einer ILB-ASE mit einem WAF-Gerät ##

Azure App Service bietet zahlreiche Sicherheitsmaßnahmen zum Schutz des Systems. Außerdem bietet es Unterstützung bei der Feststellung, ob eine App gehackt wurde. Der beste Schutz für eine Webanwendung besteht darin, eine Hostingplattform wie Azure App Service mit einer Web Application Firewall (WAF) zu koppeln. Da die ILB-ASE über einen vom Netzwerk isolierten Anwendungsendpunkt verfügt, ist sie für eine solche Nutzung geeignet.

Weitere Informationen zum Konfigurieren Ihrer ILB-ASE mit einem WAF-Gerät finden Sie unter [Konfigurieren einer Web Application Firewall (WAF) für eine App Service-Umgebung][ASEWAF]. In diesem Artikel wird die Verwendung eines virtuellen Barracuda-Geräts mit Ihrer ASE erläutert. Eine weitere Möglichkeit besteht darin, Azure Application Gateway zu verwenden. Application Gateway sichert mithilfe der OWASP-Kernregeln alle dahinter platzierten Anwendungen. Weitere Informationen zu Application Gateway finden Sie unter [Einführung zur Web Application Firewall (WAF) von Azure][AppGW].

## <a name="get-started"></a>Erste Schritte ##

Alle Artikel und Anweisungen zu ASEs stehen in der [Infodatei für App Service-Umgebungen][ASEReadme] zur Verfügung.

* Informationen zum Einstieg in ASEs finden Sie unter [Einführung in App Service-Umgebungen][Intro].
* Weitere Informationen zur Azure App Service-Plattform finden Sie unter [Azure App Service](http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/).
 
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

