<properties
   pageTitle="Funktionsweise von Traffic Manager | Microsoft Azure"
   description="Dieser Artikel enthält Informationen zur Funktionsweise von Traffic Manager."
   services="traffic-manager"
   documentationCenter=""
   authors="jtuliani"
   manager="carmonm"
   editor="tysonn"/>

<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/07/2016"
   ms.author="jonatul"/>

# Funktionsweise von Traffic Manager

Azure Traffic Manager ermöglicht die Steuerung der Verteilung von Datenverkehr auf Anwendungsendpunkte. Dabei kann es sich um einen beliebigen Endpunkt mit Internetzugriff handeln, der innerhalb oder außerhalb von Azure gehostet wird.

Traffic Manager bietet zwei wesentliche Vorteile:

1. Verteilung von Datenverkehr gemäß einer von mehreren [Datenverkehrsrouting-Methoden](traffic-manager-routing-methods.md)
2. [Kontinuierliche Überwachung der Endpunktintegrität](traffic-manager-monitoring.md) und automatisches Failover bei einem Ausfall von Endpunkten

Wenn ein Endbenutzer eine Verbindung mit einem Dienstendpunkt herstellen möchte, muss der Client (PC, Smartphone oder Ähnliches) zuerst den DNS-Namen an diesem Endpunkt zu einer IP-Adresse auflösen. Anschließend stellt der Client eine Verbindung mit dieser IP-Adresse her, um auf den Dienst zuzugreifen.

**Wichtig: Traffic Manager operiert auf der DNS-Ebene.** Endbenutzer werden von Traffic Manager auf der Grundlage der gewählten Datenverkehrsrouting-Methode und der aktuellen Endpunktintegrität mithilfe von DNS an bestimmte Dienstendpunkte umgeleitet. Clients stellen dann eine **direkte Verbindung** mit dem ausgewählten Endpunkt her. Traffic Manager ist kein Proxy und hat keinen Zugriff auf den Datenverkehr zwischen Client und Dienst.

## Traffic Manager-Beispiel

Contoso Corp hat ein neues Partnerportal entwickelt. Die URL für dieses Portal lautet https://partners.contoso.com/login.aspx. Die Anwendung wird in Azure gehostet. Zur Optimierung der Verfügbarkeit sowie zur Maximierung der weltweiten Leistung möchte das Unternehmen die Anwendung in drei Regionen auf der ganzen Welt bereitstellen und Endbenutzer dabei mithilfe von Traffic Manager an den jeweils nächstgelegenen Endpunkt weiterleiten.

Hierzu geht das Unternehmen wie folgt vor:

- Es stellt drei Instanzen des Diensts bereit. Die DNS-Namen dieser Bereitstellungen lauten „contoso-us.cloudapp.net“, „contoso-eu.cloudapp.net“ und „contoso-asia.cloudapp.net“.
- Anschließend erstellt das Unternehmen ein Traffic Manager-Profil namens „contoso.trafficmanager.net“ und konfiguriert es mit der leistungsorientierten Datenverkehrsrouting-Methode für die drei oben genannten Endpunkte.
- Abschließend konfiguriert es die Vanity-Domäne „partners.contoso.com“ mit einem Verweis auf „contoso.trafficmanager.net“ (unter Verwendung eines DNS CNAME-Eintrags).

![Traffic Manager-DNS-Konfiguration][1]

> [AZURE.NOTE] Wenn Sie eine Vanity-Domäne mit Azure Traffic Manager verwenden, müssen Sie mithilfe eines CNAME-Eintrags von Ihrem Vanity-Domänennamen auf Ihren Traffic Manager-Domänennamen verweisen.

> Aufgrund einer Einschränkung der DNS-Standards kann ein CNAME nicht an der Spitze (oder am Stamm) einer Domäne erstellt werden. Daher können Sie keinen CNAME für „contoso.com“ (gelegentlich auch als Naked-Domäne bezeichnet) erstellen. Ein CNAME kann nur für eine Domäne unter „contoso.com“ erstellt werden (beispielsweise für „www.contoso.com“).

> Traffic Manager kann also nicht direkt mit einer Naked-Domäne verwendet werden. Zur Umgehung dieser Einschränkung empfiehlt sich die Verwendung einer einfachen HTTP-Umleitung, um an „contoso.com“ gerichtete Anforderungen an einen alternativen Namen (etwa „www.contoso.com“) umzuleiten.

## Verbindungsherstellung von Clients über Traffic Manager

Wenn ein Endbenutzer die Seite https://partners.contoso.com/login.aspx anfordert (wie im obigen Beispiel beschrieben), führt der Client die folgenden Schritte aus, um den DNS-Namen aufzulösen und eine Verbindung herzustellen:

![Verbindungsherstellung mit Traffic Manager][2]

1.	Der Client (PC, Smartphone oder Ähnliches) richtet eine DNS-Abfrage für „partners.contoso.com“ an den konfigurierten rekursiven DNS-Dienst. (Ein rekursiver DNS-Dienst wird gelegentlich auch als lokaler DNS-Dienst bezeichnet und fungiert nicht als direkter Host für DNS-Domänen. Stattdessen wird er vom Client zur Abladung der Arbeit für die Kontaktaufnahme mit den verschiedenen autoritativen DNS-Diensten im Internet verwendet, die zum Auflösen eines DNS-Namens benötigt werden.)
2.	Der rekursive DNS-Dienst löst den DNS-Namen „partners.contoso.com“ auf. Zunächst ermittelt der rekursive DNS-Dienst die Namenserver für die Domäne „contoso.com“. Anschließend fordert er von diesen Namenservern den DNS-Eintrag „partners.contoso.com“ an. Der CNAME für „contoso.trafficmanager.net“ wird zurückgegeben.
3.	Nun ermittelt der rekursive DNS-Dienst die Namenserver für die Domäne „trafficmanager net“, die vom Azure Traffic Manager-Dienst bereitgestellt werden. Von diesen Namenservern fordert er den DNS-Eintrag „contoso.trafficmanager.net“ an.
4.	Die Anforderung geht bei den Traffic Manager-Namenservern ein. Diese entscheiden, welcher Endpunkt zurückgegeben wird. Die Entscheidung basiert auf Folgendem: a. Aktivierungszustand der einzelnen Endpunkte. (Deaktivierte Endpunkte werden nicht zurückgegeben.) b. Aktuelle Integrität der einzelnen Endpunkte gemäß Traffic Manager-Integritätsüberprüfungen. Weitere Informationen finden Sie unter „Traffic Manager-Endpunktüberwachung“. c. Gewählte Datenverkehrsrouting-Methode. Weitere Informationen finden Sie unter „Traffic Manager-Routingmethoden für Datenverkehr“.
5.	Der gewählte Endpunkt wird als weiterer DNS CNAME-Eintrag zurückgegeben. (In unserem Fall nehmen wir an, dass „contoso-us.cloudapp.net“ zurückgegeben wird.)
6.	Der rekursive DNS-Dienst ermittelt nun die Namenserver für die Domäne „cloudapp.net“. Von diesen Namenservern fordert er den DNS-Eintrag „contoso-us.cloudapp.net“ an. Ein DNS A-Eintrag mit der IP-Adresse des Dienstendpunkts in den USA wird zurückgegeben.
7.	Der rekursive DNS-Dienst gibt an den Client die konsolidierten Ergebnisse der oben beschriebenen Namensauflösungssequenz zurück.
8.	Der Client erhält die DNS-Ergebnisse vom rekursiven DNS-Dienst und stellt eine Verbindung mit der angegebenen IP-Adresse her. Beachten Sie, dass die Verbindung mit dem Dienstendpunkt der Anwendung direkt und nicht über Traffic Manager hergestellt wird. Da es sich um einen HTTPS-Endpunkt handelt, wird erst der erforderliche SSL/TLS-Handshake durchgeführt und anschließend per HTTP GET-Anforderung die Seite „/login.aspx“ angefordert.

Beachten Sie, dass der rekursive DNS-Dienst die eingehenden DNS-Antworten zwischenspeichert. Gleiches gilt für den DNS-Client auf dem Gerät des Endbenutzers. Dadurch können nachfolgende DNS-Abfragen schneller abgewickelt werden, da keine anderen Namenserver abgefragt werden müssen, sondern die Daten aus dem Cache verwendet werden können. Wie lange die Daten zwischengespeichert werden, hängt von der TTL-Eigenschaft der einzelnen DNS-Einträge ab. Bei niedrigen Werten wird der Cache schneller ungültig, was mehr Roundtrips zu den Traffic Manager-Namensservern zur Folge hat. Bei hohen Werten kann es dagegen länger dauern, bis Datenverkehr nicht mehr an einen fehlerhaften Endpunkt geleitet wird. Mit Traffic Manager können Sie die TTL für Traffic Manager-DNS-Antworten konfigurieren und so einen Wert wählen, der für die Anforderungen Ihrer Anwendung am besten geeignet ist.

## Häufig gestellte Fragen

### Welche IP-Adresse verwendet Traffic Manager?

Traffic Manager arbeitet, wie unter „Funktionsweise von Traffic Manager“ erläutert, auf der DNS-Ebene. Clients werden mithilfe von DNS-Antworten an den geeigneten Dienstendpunkt umgeleitet. Anschließend stellen Clients dann eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager).

Traffic Manager bietet daher keinen Endpunkt und keine IP-Adresse für die Verbindungsherstellung von Clients. Wenn also etwa eine statische IP-Adresse benötigt wird, muss diese im Dienst und nicht in Traffic Manager konfiguriert werden.

### Unterstützt Traffic Manager persistente Sitzungen?

Wie bereits [weiter oben](#how-clients-connect-using-traffic-manager) erwähnt, arbeitet Traffic Manager auf der DNS-Ebene. Clients werden mithilfe von DNS-Antworten an den geeigneten Dienstendpunkt umgeleitet. Anschließend stellen Clients dann eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager). HTTP-Datenverkehr zwischen Client und Server (einschließlich Cookies) wird also nicht über Traffic Manager abgewickelt.

Beachten Sie außerdem, dass es sich bei der IP-Quelladresse der von Traffic Manager empfangenen DNS-Abfrage um die IP-Adresse des rekursiven DNS-Diensts (und nicht um die IP-Adresse des Clients) handelt.

Aus diesem Grund kann Traffic Manager keine individuellen Clients identifizieren oder nachverfolgen und somit keine persistenten Sitzungen implementieren. Dies ist bei allen DNS-basierten Datenverkehrsverwaltungs-Systemen der Fall, nicht nur bei Verwendung von Traffic Manager.

### Warum tritt bei mir ein HTTP-Fehler auf, wenn ich Traffic Manager verwende?

Wie bereits [weiter oben](#how-clients-connect-using-traffic-manager) erwähnt, arbeitet Traffic Manager auf der DNS-Ebene. Clients werden mithilfe von DNS-Antworten an den geeigneten Dienstendpunkt umgeleitet. Anschließend stellen Clients dann eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager).

HTTP-Datenverkehr zwischen Client und Server wird also nicht über Traffic Manager abgewickelt. Das bedeutet, dass von Traffic Manager auch keine HTTP-Fehler verursacht werden können. Gegebenenfalls auftretende HTTP-Fehler müssen daher auf Ihre Anwendung zurückzuführen sein. Da der Client eine Verbindung mit der Anwendung herstellt, muss die DNS-Auflösung (einschließlich der Rolle von Traffic Manager) abgeschlossen sein.

Konzentrieren Sie sich daher bei der weiteren Untersuchung auf die Anwendung.

Bei Verwendung von Traffic Manager liegt häufig folgendes Problem vor: Der vom Browser an die Anwendung übergebene HTTP-Hostheader enthält den im Browser verwendeten Domänennamen. Dabei kann es sich um den Traffic Manager-Domänennamen (etwa „myprofile.trafficmanager.net“) handeln, wenn Sie diesen zu Testzwecken verwenden, oder um die Vanity-Domäne „CNAME“, die als Verweis auf den Traffic Manager-Domänennamen konfiguriert ist. Überprüfen Sie in beiden Fällen, ob die Anwendung für die Annahme dieses Hostheaders konfiguriert ist.

Falls Ihre Anwendung in Azure App Service gehostet wird, lesen Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für eine Web-App in Azure App Services, der Traffic Manager verwendet](../app-service-web/web-sites-traffic-manager-custom-domain-name.md) weiter.

### Wie wirkt sich die Verwendung von Traffic Manager auf die Leistung aus?

Wie bereits [weiter oben](#how-clients-connect-using-traffic-manager) erwähnt, arbeitet Traffic Manager auf der DNS-Ebene. Clients werden mithilfe von DNS-Antworten an den geeigneten Dienstendpunkt umgeleitet. Anschließend stellen Clients dann eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager).

Da Clients eine direkte Verbindung mit Ihren Endpunkten herstellen, hat die Verwendung von Traffic Manager keinerlei Auswirkungen auf die Leistung, sobald die Verbindung hergestellt wurde.

Die Anwendungsintegration von Traffic Manager erfolgt auf der DNS-Ebene, sodass in die DNS-Auflösungskette kein zusätzliches DNS-Lookup eingefügt werden muss (siehe [Traffic Manager-Beispiel](#traffic-manager-example)). Die DNS-Auflösungszeit wird durch Traffic Manager nur minimal beeinträchtigt. Traffic Manager verwendet ein globales Netzwerk von Namenservern sowie Anycastnetzwerke, um sicherzustellen, dass DNS-Abfragen immer an den nächstgelegenen verfügbaren Namenserver weitergeleitet werden. Dank der Zwischenspeicherung von DNS-Antworten wirkt sich die zusätzliche, durch die Verwendung von Traffic Manager bedingte DNS-Latenz lediglich auf einen Bruchteil der Sitzungen aus.

Die Leistungseinbußen durch die Implementierung von Traffic Manager in Ihre Anwendung dürften daher minimal sein.

Bei Verwendung der [leistungsorientierten Methode für das Datenverkehrsrouting](traffic-manager-routing-methods.md#performance-traffic-routing-method) von Traffic Manager wird die Erhöhung der DNS-Latenz durch die verbesserte Leistung aufgrund der Weiterleitung von Endbenutzern an den nächstgelegenen Endpunkt mehr als wettgemacht.

### Welche Anwendungsprotokolle kann ich mit Traffic Manager verwenden?
Wie bereits [weiter oben](#how-clients-connect-using-traffic-manager) erwähnt, arbeitet Traffic Manager auf der DNS-Ebene. Nach Abschluss des DNS-Lookups stellen Clients eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager). Für diese Verbindung kann daher ein beliebiges Anwendungsprotokoll verwendet werden.

Für die Endpunkt-Integritätsprüfungen von Traffic Manager wird jedoch entweder ein HTTP- oder ein HTTPS-Endpunkt benötigt. Dies kann von den Anwendungsendpunkten getrennt werden, mit denen Clients eine Verbindung herstellen. Geben Sie hierzu in den Integritätsprüfungseinstellungen des Traffic Manager-Profils einen anderen TCP-Port oder URI-Pfad an.

### Kann ich Traffic Manager mit einem Naked-Domänennamen (ohne „www“) verwenden?

Derzeit ist dies nicht möglich.

Der DNS-CNAME-Eintragstyp wird verwendet, um eine Zuordnung zwischen einem DNS-Namen und einem anderen Namen zu erstellen. Wie im [Traffic Manager-Beispiel](#traffic-manager-example) erläutert, benötigt Traffic Manager einen DNS-CNAME-Eintrag, um den Vanity-DNS-Namen (etwa „www.contoso.com“) dem DNS-Namen des Traffic Manager-Profils (etwa „contoso.trafficmanager.net“) zuzuordnen. Darüber hinaus gibt das Traffic Manager-Profil selbst einen zweiten DNS-CNAME-Eintrag zurück, um anzugeben, mit welchem Endpunkt der Client eine Verbindung herstellen soll.

Die DNS-Standards lassen keine Koexistenz von CNAME-Einträgen und anderen DNS-Einträgen des gleichen Typs zu. Da die oberste Ebene (Stamm) einer DNS-Zone immer zwei vorab vorhandene DNS-Einträge (den SOA- und den autoritativen NS-Eintrag) enthält, kann ein CNAME-Eintrag nicht auf der obersten Ebene der Zone erstellt werden, ohne gegen die DNS-Standards zu verstoßen.

Zur Umgehung dieses Problems empfiehlt es sich, den Datenverkehr für Dienste mit einer Naked-Domäne (ohne „www“), für die Traffic Manager verwendet werden soll, mithilfe einer HTTP-Umleitung aus der Naked-Domäne an eine andere URL umzuleiten, für die Traffic Manager verwendet werden kann. So kann beispielsweise die Naked-Domäne „contoso.com“ Benutzer an „www.contoso.com“ umleiten, wofür dann Traffic Manager verwendet werden kann.

Die uneingeschränkte Unterstützung von Naked-Domänen in Traffic Manager ist Teil unseres Feature-Backlogs. Sollten Sie sich für dieses Feature interessieren, [stimmen Sie auf unserer Community-Feedbackwebsite dafür ab](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).

## Nächste Schritte

Informieren Sie sich ausführlicher über [Endpunktüberwachung und automatisches Failover](traffic-manager-monitoring.md) von Traffic Manager.

Informieren Sie sich ausführlicher über [Traffic Manager-Routingmethoden](traffic-manager-routing-methods.md).

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png

<!---HONumber=AcomDC_0608_2016-->