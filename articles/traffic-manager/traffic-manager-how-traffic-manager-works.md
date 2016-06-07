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
   ms.date="05/25/2016"
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

## Nächste Schritte

Informieren Sie sich ausführlicher über [Endpunktüberwachung und automatisches Failover](traffic-manager-monitoring.md) von Traffic Manager.

Informieren Sie sich ausführlicher über [Traffic Manager-Routingmethoden](traffic-manager-routing-methods.md).

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png

<!---HONumber=AcomDC_0525_2016-->