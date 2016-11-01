<properties
	pageTitle="Erstellen und Verwenden einen internen Lastenausgleichs mit einer App Service-Umgebung | Microsoft Azure"
	description="Erstellen und Verwenden einer ASE mit einem ILB"
	services="app-service"
	documentationCenter=""
	authors="ccompy"
	manager="stefsch"
	editor=""/>

<tags
	ms.service="app-service"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/12/2016"
	ms.author="ccompy"/>

# Verwenden einen internen Lastenausgleichs mit einer App Service-Umgebung #

Das Feature App Service-Umgebungen (App Service Environment, ASE) ist eine Premium-Dienstoption von Azure App Service mit erweiterter Konfigurationsmöglichkeit, die in den mehrinstanzenfähigen Stamps nicht verfügbar ist. Mit dem ASE-Feature wird im Wesentlichen Azure App Service in Ihrem virtuellen Azure-Netzwerk (VNet) bereitgestellt. Um ein besseres Verständnis von den Möglichkeiten von App Service-Umgebungen zu erhalten, lesen Sie die Dokumentation [Was ist eine App Service-Umgebung][WhatisASE]. Wenn Sie die Vorteile des Betriebs in einem VNet nicht kennen, lesen Sie die [FAQs zu virtuellen Netzwerken][virtualnetwork].


## Übersicht ##


Eine ASE kann mit einem Endpunkt, auf den über das Internet zugegriffen werden kann, oder einer IP-Adresse in Ihrem VNet bereitgestellt werden. Um die IP-Adresse auf eine VNet-Adresse festzulegen, müssen Sie Ihre ASE mit einem internen Lastenausgleich (Internal Load Balancer, ILB) bereitstellen. Wenn Ihre ASE mit einem ILB konfiguriert ist, stellen Sie Folgendes bereit:

- Ihre eigene Domäne oder Unterdomäne. Zur Vereinfachung wird in diesem Dokument eine Unterdomäne vorausgesetzt, aber Sie können jeden Fall konfigurieren.
- das für HTTPS verwendete Zertifikat
- DNS-Verwaltung für die Unterdomäne.


Im Gegenzug können Sie z.B. Folgendes tun:

- Intranetanwendungen, z.B. Geschäftsanwendungen, sicher in der Cloud hosten, auf die Sie über ein Site-to-Site- oder ExpressRoute-VPN zugreifen.
- Apps in der Cloud hosten, die nicht im öffentlichen DNS-Server aufgeführt sind
- vom Internet isolierte Back-End-Apps erstellen, in die sich Ihre Front-End-Apps sicher integrieren können


#### Deaktivierte Funktionen ####

Es gibt einige Dinge, die Sie mit einer ILB-ASE nicht machen können. Hierzu zählen:

- Verwenden von IPSSL
- Zuweisen von IP-Adressen an bestimmte Apps
- Kaufen und Verwenden eines Zertifikats mit einer App über das Portal. Sie können natürlich weiterhin Zertifikate direkt bei einer Zertifizierungsstelle erhalten und sie mit Ihren Apps verwenden, nicht nur über das Azure-Portal.


## Erstellen einer ILB-ASE ##

Das Erstellen einer ILB-ASE unterscheidet sich nicht wesentlich vom Erstellen einer normalen ASE. Eine eingehendere Erläuterung zum Erstellen einer ASE finden Sie unter [Erstellen einer App Service-Umgebung][HowtoCreateASE]. Der Prozess zum Erstellen einer ILB-ASE ist beim Erstellen eines VNet während der ASE-Erstellung und bei der Auswahl eines vorhandenen VNet identisch. So erstellen Sie eine ILB-ASE:

1.	Wählen Sie im Azure-Portal **Neu > Web und Mobil > App Service-Umgebung** aus.
2.	Wählen Sie Ihr Abonnement aus.
3.	Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine.
4.	Wählen Sie ein VNet aus, oder erstellen Sie eines.
5.	Wählen Sie ein Subnetz aus. oder erstellen Sie eines, wenn Sie ein VNet auswählen.
6.	Wählen Sie **Virtuelles Netzwerk/Speicherort > VNet-Konfiguration**, und legen Sie „Intern“ für den VIP-Typ fest.
7.	Geben Sie den Unterdomänennamen an (diese Unterdomäne wird für Apps verwendet, die in dieser ASE erstellt werden).
8.	Wählen Sie „OK“ und dann „Erstellen“.


![][1]


Im Blatt „Virtuelles Netzwerk“ steht die Option „VNet-Konfiguration“ zur Verfügung. Hier können Sie zwischen einer externen und einer internen VIP-Adresse wählen. Die externe Adresse ist die Standardeinstellung. Wenn Sie die externe Adresse festgelegt haben, verwendet Ihre ASE eine über das Internet zugängliche VIP-Adresse. Wenn Sie die interne Adresse auswählen, wird Ihre ASE mit einem ILB auf einer IP-Adresse in Ihrem VNet konfiguriert.


Nach Auswahl der internen Adresse können Sie Ihrer ASE keine weiteren IP-Adressen hinzufügen, und stattdessen müssen Sie die Unterdomäne der ASE bereitstellen. In einer ASE mit einer externen VIP-Adresse wird der Name der ASE in der Unterdomäne für Apps verwendet, die in dieser ASE erstellt werden. Wenn Ihre ASE als ***contosotest*** und Ihre App in dieser ASE als ***mytest*** bezeichnet wurde, hat die Unterdomäne das Format ***contosotest.p.azurewebsites.net***, und die URL für die App wäre ***mytest.contosotest.p.azurewebsites.net***. Wenn Sie „Intern“ für den VIP-Typ festgelegt haben, wird Ihr ASE-Name in der Unterdomäne für die ASE nicht verwendet. Sie geben die Unterdomäne explizit an. Wenn die Unterdomäne ***contoso.corp.net*** hieße, und Sie in dieser ASE eine App mit Namen ***timereporting*** erstellen würden, dann lautete die URL für diese App ***timereporting.contoso.corp.net***.


## Apps in einer ILB-ASE ##

Das Erstellen einer App in einer ILB-ASE entspricht dem normalen Erstellen einer App in einer ASE.

1. Wählen Sie im Azure-Portal **Neu > Web und Mobil > Web** oder **Mobil** bzw. **API-App** aus.
2. Geben Sie den Namen der App ein.
2. Wählen Sie das Abonnement aus.
3. Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine.
4. Wählen Sie einen App Service-Plan (ASP) aus, oder erstellen Sie einen. Wenn Sie einen neuen ASP erstellen, wählen Sie Ihre ASE als Speicherort, und wählen Sie den Workerpool, in dem Ihre ASP erstellt werden soll. Beim Erstellen des ASP wählen Sie Ihre ASE als Speicherort und den Workerpool. Wenn Sie den Namen der App angeben, sehen Sie, dass die Unterdomäne unter Ihrem App-Namen durch die Unterdomäne für Ihre ASE ersetzt wird.
5. Klicken Sie auf Erstellen. Wählen Sie das Kontrollkästchen **An das Dashboard anheften** aus, wenn die App auf dem Dashboard angezeigt werden soll.

![][2]


Unter dem App-Namen wird der Name der Unterdomäne aktualisiert, um die Unterdomäne Ihrer ASE anzuzeigen.


## Überprüfung nach der ILB-ASE-Erstellung ##

Eine ILB-ASE unterscheidet sich etwas von einer Nicht-ILB-ASE. Wie bereits erwähnt, müssen Sie Ihren eigenen DNS verwalten und auch Ihr eigenes Zertifikat für HTTPS-Verbindungen bereitstellen.


Nach dem Erstellen Ihrer ASE werden Sie bemerken, dass die Unterdomäne angezeigt wird, die Sie angegeben haben, und dass das Menü **Einstellung** eine neue Option **ILB-Zertifikat** enthält. Solange Sie kein Zertifikat für Ihre ASE eingerichtet haben, können Sie die Apps in Ihrer ASE über HTTPS nicht erreichen.

![][3]


Wenn Sie einfach experimentieren und nicht wissen, wie Sie ein Zertifikat erstellen, können Sie mit der IIS-MMC-Konsolenanwendung ein selbstsigniertes Zertifikat erstellen. Nach der Erstellung können Sie es als PFX-Datei exportieren und dann in die ILB-Zertifikatbenutzeroberfläche hochladen. Wenn Sie auf eine Website zugreifen, die mit einem selbstsignierten Zertifikat gesichert ist, erhalten Sie von Ihrem Browser eine Warnung, dass die Website, auf die Sie zugreifen, nicht sicher ist, weil das Zertifikat nicht validiert werden kann. Wenn Sie diese Warnung vermeiden möchten, benötigen Sie ein ordnungsgemäß signiertes Zertifikat, das Ihrer Unterdomäne entspricht und über eine Zertifikatkette verfügt, die von Ihrem Browser erkannt wird.

![][6]

Wenn Sie die HTTP- und HTTPS-Zugriff auf Ihre ASE testen möchten:

1.	Wechseln Sie nach der ASE-Erstellung zur ASE-Benutzeroberfläche: **ASE > Einstellungen > ILB-Zertifikate**.
2.	Legen Sie das ILB-Zertifikat durch Auswahl von Zertifikat-PFX-Datei und Angabe des Kennworts fest. Dieser Schritt nimmt etwas Zeit in Anspruch, und es wird die Meldung angezeigt, dass eine Skalierung ausgeführt wird.
3.	Rufen Sie die ILB-Adresse für Ihre ASE ab (**ASE > Eigenschaften >Virtuelle IP-Adresse**).
4.	Erstellen Sie nach der ASE-Erstellung eine Web-App in der ASE.
5.	Erstellen Sie einen virtuellen Computer, wenn in diesem VNET keiner vorhanden ist (nicht im gleichen Subnetz wie die ASE, da dies zum Absturz führt).
6.	Legen Sie den DNS für die Unterdomäne fest. Sie können einen Platzhalter mit Ihrer Unterdomäne in Ihrem DNS verwenden, oder wenn Sie einige einfache Tests durchführen möchten, bearbeiten Sie die Hostdatei auf Ihrem virtuellen Computer, um die VIP-Adresse für den Web-App-Namen festzulegen. Wenn Sie in einer ASE mit dem Unterdomänennamen „.ilbase.com“ die Web-App „mytestapp“ erstellen, sodass sie mit „mytestapp.ilbase.com“ adressiert wird, müssen Sie dies in der Hostdatei festlegen. (Unter Windows befindet sich die Hostdatei unter C:\\Windows\\System32\\drivers\\etc\\.)
7.	Wechseln Sie in einem Browser auf diesem virtuellen Computer zu http://mytestapp.ilbase.com (bzw. dem Namen Ihrer Web-App in Ihrer Unterdomäne).
8.	Wechseln Sie in einem Browser auf diesem virtuellen Computer zu https://mytestapp.ilbase.com. Sie müssen das Sicherheitsdefizit bei Verwendung eines selbstsignierten Zertifikats akzeptieren.


Die IP-Adresse der ILB wird in den Eigenschaften als virtuelle IP-Adresse aufgeführt.

![][4]


## Verwenden einer ILB-ASE ##

#### Netzwerksicherheitsgruppen ####

Eine ILB-ASE ermöglicht Netzwerkisolation für Ihre Apps, da nicht auf die Apps zugegriffen werden kann, bzw. die Apps nicht einmal im Internet bekannt sind. Dies eignet sich hervorragend zum Hosten von Intranetsites, z.B. Branchenanwendungen. Wenn Sie den Zugriff noch weiter einschränken müssen, können Sie noch Netzwerksicherheitsgruppen (NSGs) zum Steuern des Zugriffs auf Netzwerkebene verwenden.


Wenn Sie NSGs verwenden möchten, um den Zugriff weiter einzuschränken, müssen Sie sicherstellen, dass Sie nicht die Kommunikation unterbrechen, die die ASE für den Betrieb benötigt. Obwohl der HTTP/HTTPS-Zugriff nur über den von der ASE verwendeten ILB erfolgt, ist die ASE weiterhin von Ressourcen außerhalb des VNet abhängig. Um festzustellen, welcher Netzwerkzugriff noch erforderlich ist, lesen Sie das Dokument [Steuern von eingehendem Datenverkehr in eine App Service-Umgebung][ControlInbound] und das Dokument [Details zur Netzwerkkonfiguration für App Service-Umgebungen mit ExpressRoute][ExpressRoute].


Um Ihre Netzwerksicherheitsgruppen zu konfigurieren, müssen Sie die IP-Adresse kennen, die Azure verwendet, um Ihre ASE zu verwalten. Diese IP-Adresse ist auch die ausgehende IP-Adresse Ihrer ASE für Internetanfrage. Zum Suchen dieser IP-Adresse wechseln Sie zu **Einstellungen > Eigenschaften**, und suchen Sie die **Ausgehende IP-Adresse**.

![][5]


#### Allgemeine ILB-ASE-Verwaltung ####

Das Verwalten einer ILB-ASE ist weitgehend identisch mit dem normalen Verwalten einer ASE. Sie müssen Ihre Workerpools zentral hochskalieren, um weitere ASP-Instanzen zu hosten, und Ihre Front-End-Server zentral hochskalieren, um zusätzlichen HTTP/HTTPS-Datenverkehr abzuwickeln. Allgemeine Informationen zur Verwaltung der Konfiguration einer ASE finden Sie in dem Dokument [Konfigurieren einer App Service-Umgebung][ASEConfig].


Die zusätzlichen Verwaltungselemente sind Zertifikat- und DNS-Verwaltung. Sie müssen das für HTTPS verwendete Zertifikat nach der ILB-ASE-Erstellung abrufen und hochladen und vor Ablauf ersetzen. Da Azure die Basisdomäne besitzt, können wir Zertifikate für ASEs mit einer externen VIP-Adresse bereitstellen. Da die von einer ILB-ASE verwendete Unterdomäne alles sein kann, müssen Sie Ihr eigenes Zertifikat für HTTPS bereitstellen.


#### DNS-Konfiguration ####

Wenn eine externe VIP-Adresse des DNS mithilfe von Azure verwaltet wird. Jede in Ihrer ASE erstellte App wird automatisch dem Azure-DNS – einem öffentlichen DNS – hinzugefügt. In einer ILB-ASE müssen Sie einen eigenen DNS verwalten. Für eine bestimmte Unterdomäne, wie z.B. „contoso.corp.net“, müssen Sie DNS-A-Einträge erstellen, die auf Ihre ILB-Adresse für Folgendes verweisen:

	* 
	*.scm 
	ftp 
	publish 


## Erste Schritte
Alle Artikel und Anleitungen zu App Service-Umgebungen stehen in der [Dokumentation zur App Service-Umgebung](../app-service/app-service-app-service-environments-readme.md) zur Verfügung.

Informationen zum Einstieg in App Service-Umgebungen finden Sie unter [Einführung in App Service-Umgebungen][WhatisASE]

Weitere Informationen zur Azure App Service-Plattform finden Sie unter [Azure App Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/

<!---HONumber=AcomDC_0713_2016-->