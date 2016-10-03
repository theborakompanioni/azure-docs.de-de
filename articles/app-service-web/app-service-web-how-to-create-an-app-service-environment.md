<properties 
	pageTitle="Erstellen einer App Service-Umgebung" 
	description="Beschreibung der Erstellung von App Service-Umgebungen" 
	services="app-service" 
	documentationCenter="" 
	authors="ccompy" 
	manager="stefsch" 
	editor=""/>

<tags 
	ms.service="app-service" 
	ms.workload="web" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/12/2016" 
	ms.author="ccompy"/>

# Erstellen einer App Service-Umgebung #

App Service-Umgebungen (App Service Environments, ASEs) sind eine Premium-Dienstoption von Azure App Service mit erweiterten Konfigurationsmöglichkeiten, die in den mehrinstanzenfähigen Verwaltungseinheiten nicht verfügbar sind. Mit dem ASE-Feature wird im Wesentlichen Azure App Service im virtuellen Netzwerk eines Kunden bereitgestellt. Um ein besseres Verständnis von den Möglichkeiten von App Service-Umgebungen zu erhalten, lesen Sie die Dokumentation [Was ist eine App Service-Umgebung][WhatisASE].


### Übersicht ###

Eine ASE besteht aus Front-End- und Worker-Computeressourcen. Die Front-Ends agieren als HTTP/HTTPS-Endpunkte und senden Datenverkehr an die Workers, d.h. die Rollen, die Ihre Apps hosten.

Für die ASE-Erstellung muss der Kunde die folgenden Informationen bereitstellen:

- Name der ASE
- Für die ASE zu verwendendes Abonnement
- Ressourcengruppe
- Virtuelles Azure-Netzwerk (VNet) mit 8 oder mehr Adressen und ein Subnetz zur Verwendung in der ASE
- VIP-Adresstyp, extern oder intern
- ASE-Ressourcenpooldefinition


Für jedes dieser Elemente gibt es einige wichtige Details.

- Der Name der ASE wird in der Unterdomäne für alle Apps verwendet, die in dieser ASE erstellt werden, sofern mit einer externen VIP-Adresse konfiguriert.
- Eine ASE mit einer externen VIP-Adresse hostet über das Internet zugängliche Apps. Eine ASE mit einer internen VIP-Adresse verwendet ein internes Lastenausgleichsmodul (Internal Load Balancer, ILB).
- Alle Apps, die in einer ASE erstellt werden, befinden sich in demselben Abonnement wie die ASE selbst.
- Wenn Sie nicht über Zugriff auf das Abonnement verfügen, mit dem die ASE erstellt wurde, können Sie die ASE nicht zum Erstellen von Apps verwenden.
- Zum Hosten einer ASE verwendete VNets müssen regionale VNets sein. Sie können entweder klassische oder Resource Manager-VNets verwenden.
- **Das für das Hosting der ASE verwendete Subnetz darf keine anderen Compute-Ressourcen enthalten.**
- In einem Subnetz kann nur eine ASE vorhanden sein.
- ASEs können nun in virtuellen Netzwerken bereitgestellt werden, die *entweder* öffentliche Adressbereiche *oder* RFC1918-Adressräume (d.h. private Adressen) verwenden. Um ein virtuelles Netzwerk mit einem öffentlichen Adressbereich zu verwenden, müssen Sie VNet und Subnetz vorab erstellen und dann das Subnetz auf der UX für die ASE-Erstellung auswählen.


Jede Bereitstellung einer App Service-Umgebung ist ein gehosteter Dienst, der von Azure verwaltet und gewartet wird. Die Compute-Ressourcen, welche die Systemrollen der App Service-Umgebung hosten, sind für den Kunden nicht zugänglich. Der Kunde verwaltet jedoch die Menge der Instanzen und deren Größen.

Es gibt zwei Möglichkeiten, auf die Benutzeroberfläche zur Erstellung der App Service-Umgebung zuzugreifen. Durchsuchen Sie den Azure Marketplace nach ***App Service-Umgebung***, oder wählen Sie "Neu" -> "Web + Mobil".

Wenn Sie möchten, dass das VNet eine von der ASE separate Ressourcengruppe verwendet, müssen Sie das VNet zuerst separat erstellen und dann während der ASE-Erstellung auswählen. Auch wenn Sie während der Erstellung der ASE ein Subnetz in einem vorhandenen VNet erstellen möchten, muss sich die ASE in der gleichen Ressourcengruppe befinden wie das VNet.


### Schnellerfassung ###
Die Erstellungsoberfläche für eine ASE bietet eine Reihe von Standardeinstellungen, um eine rasche Erstellung zu ermöglichen. Sie können schnell eine ASE erstellen, indem Sie einfach einen Namen für die Bereitstellung eingeben. Dadurch wird wiederum eine ASE in der Ihnen am nächsten gelegenen Region erstellt mit:

- VNet mit 512 Adressen, die einen privaten RFC1918-Adressraum nutzen
- einem Subnetz mit 256 Adressen
- externe VIP-Adresse
- einem Front-End-Pool mit 2 P2-Computeressourcen
- einem Workerpool mit 2 P1-Computeressourcen
- einer einzelnen IP-Adresse zur Verwendung für IP-SSL

Für Front-End-Pools ist P2 oder größer erforderlich. Lassen Sie beim Auswählen des Abonnements, in dem sich die ASE befinden soll, Vorsicht walten. Die einzigen Konten, die die App Service-Umgebung zum Hosten von Inhalten verwenden können, müssen in dem Abonnement enthalten sein, das zur Erstellung verwendet wurde.

![][1]

Der Name, der für die ASE angegeben wird, wird für die in der ASE erstellten Apps verwendet. Wenn der Name der App Service-Umgebung „appsvcenvdemo“ ist, lautet der Unterdomänenname „.*appsvcenvdemo.p.azurewebsites.net*“. Wenn Sie also eine App namens *mytestapp* erstellt haben, wäre diese unter der Adresse *mytestapp.appsvcenvdemo.p.azurewebsites.net* aufrufbar. Sie dürfen im Namen Ihrer ASE keine Leerzeichen verwenden. Bei Verwendung von Großbuchstaben im Namen wird der entsprechende Domänenname dennoch vollständig in Kleinbuchstaben geschrieben. Bei Verwendung eines ILB wird Ihr ASE-Name nicht in Ihrer Unterdomäne verwendet, jedoch stattdessen explizit während der ASE-Erstellung angegeben.

Die Standardwerte sind für eine Reihe von Fällen sehr nützlich, aber häufig müssen Sie sie anpassen. Die nächsten Abschnitten führen Sie durch die einzelnen Konfigurationsabschnitte, die für die ASE relevant sind.


### Virtual Network ###
Der ASE-Erstellungsprozess unterstützt die Auswahl eines vorhandenen klassischen oder Resource Manager-VNet sowie die Erstellung eines neuen klassischen VNet.

Bei der Auswahl eines vorhandenes VNet sehen Sie, dass die klassischen und Resource Manager-VNets zusammen aufgelistet sind. Neben dem Speicherort der klassischen VNets steht das Wort „klassisch“. Wenn diese Angabe fehlt, ist es ein Resource Manager-VNet.

![][2]


In der Benutzeroberfläche zur VNET-Erstellung müssen Sie Folgendes angeben:

- VNet-Name
- VNet-Adressbereich in CIDR-Schreibweise
- Standort

Der Speicherort des VNet ist der Speicherort der ASE. Denken Sie daran, dass hiermit ein klassisches VNet erstellt wird, kein Resource Manager-VNet.

ASEs können in virtuellen Netzwerken bereitgestellt werden, die *entweder* öffentliche Adressbereiche *oder* RFC1918-Adressräume (d.h. private Adressen) verwenden. Um ein virtuelles Netzwerk mit einem öffentlichen Adressbereich zu verwenden, müssen Sie das Subnetz vorab erstellen und dann das Subnetz auf der UX für die ASE-Erstellung auswählen.

Wenn Sie ein bereits vorhandenes VNET auswählen, müssen Sie auch ein Subnetz angeben oder ein neues erstellen. Das Subnetz muss mindestens acht Adressen aufweisen, und es dürfen keine anderen Ressourcen bereits darin enthalten sein. Die ASE-Erstellung funktioniert nicht, wenn Sie ein Subnetz verwenden, dem bereits virtuelle Computer zugewiesen sind.

Nachdem Sie Ihr VNet angegeben oder ausgewählt haben, müssen Sie nach Bedarf ein Subnetz erstellen oder auswählen. Die hier anzugebenden Details sind in diesem Fall:

- Subnetzname
- Subnetzbereich in CIDR-Schreibweise

Falls Sie keine Erfahrung mit CIDR (Classless Inter-Domain Routing) besitzen: Es erfordert das Format einer IP-Adresse, die durch einen Schrägstrich vom CIDR-Wert getrennt wird. Es sieht folgendermaßen aus: *10.0.0.0/22*. Der CIDR-Wert gibt die Anzahl der führenden Bits an, die für die angezeigte IP-Adresse maskiert dargestellt werden. Einfacher ausgedrückt, stellt der CIDR-Wert einen IP-Adressbereich bereit. In diesem Beispiel kennzeichnet 10.0.0.0/22 einen Bereich von 1024 Adressen oder von 10.0.0.0 bis 10.0.3.255. /23 bedeutet 512 Adressen und so weiter.

Zur Erinnerung: Wenn Sie ein Subnetz in einem vorhandenen VNet erstellen möchten, befindet sich die ASE in der gleichen Ressourcengruppe wie das VNet. Damit Ihre ASE sich in einer vom VNet separaten Ressourcengruppe befindet, erstellen Sie VNet und Subnetz einfach getrennt von der Erstellung der ASE, und zwar davor.


#### Externe oder interne VIP-Adresse ####

Standardmäßig wird die VNet-Konfiguration mit einem externen VIP-Adresstyp und 1 IP-Adresse festgelegt. Wenn Sie einen ILB statt einer externen VIP-Adresse verwenden möchten, wechseln Sie zur VNet-Konfiguration, und ändern Sie den VIP-Adresstyp zu „intern“. Standardmäßig wird eine externe VIP-Adresse verwendet. Wenn Sie den VIP-Adresstyp zu „intern“ ändern, müssen Sie Ihre Unterdomäne für die ASE angeben. Bei Einsatz eines ILB als VIP-Adresse für eine ASE sind ein paar Kompromisse erforderlich. Mehr hierzu erfahren Sie unter [Using an Internal Load Balancer with an App Service Environment][ILBASE] \(Verwenden eines ILB mit einer App Service-Umgebung).

![][4]

### Compute-Ressourcenpools ###

Während der Erstellung der ASE können Sie die Anzahl der Ressourcen für die einzelnen Ressourcenpools sowie deren Größe festlegen. Die Größe der Ressourcenpools kann bei der Erstellung der ASE festgelegt werden, jedoch auch noch danach über die manuellen oder über die automatischen Skalierungsoptionen anpassen.

Wie bereits erwähnt, besteht eine ASE aus Front-End-Servern und Workern. Die Front-End-Server verarbeiten die Verbindungslast, und Worker führen den App-Code aus. Die Front-End-Server werden in einem dedizierten Compute-Ressourcenpool verwaltet. Die Worker werden wiederum in drei separaten Compute-Ressourcenpools verwaltet namens

- Workerpool 1
- Workerpool 2
- Workerpool 3

Wenn Sie über eine große Anzahl von Anforderungen für einfache Web-Apps verfügen, werden Sie Ihre Front-Ends wahrscheinlich zentral hochskalieren und weniger Worker verwenden. Falls Sie über CPU- oder arbeitsspeicherintensive Web-Apps mit wenig Datenverkehr verfügen, brauchen Sie nicht viele Front-Ends, jedoch höchstwahrscheinlich mehr oder größere Worker.

![][3]

Unabhängig von der Größe der Compute-Ressourcen sind mindestens zwei Front-End-Server und zwei Worker erforderlich. Eine ASE kann für die Verwendung von bis zu 55 Computeressourcen insgesamt konfiguriert werden. Von diesen 55 Compute-Ressourcen können nur 50 zum Hosten von Workloads eingesetzt werden. Dafür gibt es zwei Gründe. Es sind mindestens zwei Front-End-Compute-Ressourcen erforderlich. So bleiben bis zu 53 zur Unterstützung der Workerpoolzuweisung übrig. Um Fehlertoleranz bereitzustellen, muss nach den folgenden Regeln eine zusätzliche Computeressource zugewiesen werden:

- Jeder Workerpool benötigt mindestens eine weitere Compute-Ressource, der keine Workload zugewiesen werden kann
- Wenn die Menge von Compute-Ressourcen in einem Pool einen bestimmten Wert überschreitet, ist eine weitere Compute-Ressource erforderlich

Im Rahmen der Fehlertoleranzanforderungen gilt innerhalb eines einzelnen Workerpools Folgendes für einen bestimmten Wert von X Ressourcen, die einem Workerpool zugewiesen sind:

- Wenn X zwischen 2 und 20 liegt, beträgt die Anzahl der Compute-Ressourcen, die für Workloads verwendet werden können, X-1
- Wenn X zwischen 21 und 40 liegt, beträgt die Anzahl der Compute-Ressourcen, die für Workloads verwendet werden können, X-2
- Wenn X zwischen 41 und 53 liegt, beträgt die Anzahl der Compute-Ressourcen, die für Workloads verwendet werden können, X-3

Sie können nicht nur die Menge der Compute-Ressourcen verwalten, die einem bestimmten Pool zugewiesen werden können, Sie besitzen außerdem auch die Kontrolle über die Größe. In App Service-Umgebungen können Sie aus vier unterschiedlichen Größen mit den Namen P1 bis P4 auswählen. Ausführliche Informationen zu diesen Größen und Preisen finden Sie unter [App Service-Preise][AppServicePricing]. Die Compute-Ressourcengrößen P1 bis P3 entsprechen denen, die in den mehrinstanzenfähigen Umgebungen verfügbar sind. Die P4-Compute-Ressource bietet 8 Kerne mit 14 GB Arbeitsspeicher und steht nur in einer App Service-Umgebung zur Verfügung.

Die Preisgebung für App Service-Umgebungen erfolgt anhand der zugewiesenen Compute-Ressourcen. Sie bezahlen für die Compute-Ressourcen, die Ihrer App Service-Umgebung zugewiesen sind – unabhängig davon, ob diese Workloads hosten.

Standardmäßig umfasst eine App Service-Umgebung eine IP-Adresse, die für IP-SSL verfügbar ist. Wenn Sie wissen, dass Sie weitere benötigen werden, können Sie dies hier angeben oder nach der Erstellung verwalten.
  
### Nach Erstellung einer App Service-Umgebung ###

Nach der Erstellung einer App Service-Umgebung können Sie die folgenden Einstellungen anpassen:

- Anzahl der Front-Ends (Minimum: 2)
- Anzahl der Worker (Minimum: 2)
- Menge der für IP-SSL verfügbaren IP-Adressen
- Größen der Compute-Ressourcen, die von den Front-Ends oder den Workern verwendet werden (Mindestgröße für Front-Ends ist P2)

Folgende Einstellungen können Sie nicht ändern:

- Standort
- Abonnement
- Ressourcengruppe
- Verwendetes VNET
- Verwendetes Subnetz

Weitere Informationen zur manuellen Skalierung, Verwaltung und Überwachung von App Service-Umgebungen finden Sie unter [Konfigurieren einer App Service-Umgebung][ASEConfig].

Weitere Informationen zur automatischen Skalierung finden Sie in diesem Leitfaden: [Konfigurieren der automatischen Skalierung für eine App Service-Umgebung][ASEAutoscale].

Es gibt zusätzliche Abhängigkeiten, die nicht zur Anpassung zur Verfügung stehen, wie beispielsweise Datenbank und Speicher. Diese werden von Azure verwaltet und sind im System enthalten. Der Systemspeicher unterstützt bis zu 500 GB für die gesamte App Service-Umgebung, und die Datenbank wird von Azure nach Bedarf an die Staffelung des Systems angepasst.


## Erste Schritte
Alle Artikel und Anleitungen zu App Service-Umgebungen stehen in der [Dokumentation zur App Service-Umgebung](../app-service/app-service-app-service-environments-readme.md) zur Verfügung.

Informationen zum Einstieg in App Service-Umgebungen finden Sie unter [Einführung in App Service-Umgebungen][WhatisASE]

Weitere Informationen zur Azure App Service-Plattform finden Sie unter [Azure App Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png
[3]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-resources.png
[4]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-externalvip.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/

<!---HONumber=AcomDC_0713_2016-->