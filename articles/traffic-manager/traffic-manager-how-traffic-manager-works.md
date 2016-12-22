---
title: Funktionsweise von Traffic Manager | Microsoft-Dokumente
description: "In diesem Artikel wird die Funktionsweise von Azure Traffic Manager erläutert."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: a6c9370d-e60d-440f-aa82-b6d3fa5416b0
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 8827793d771a2982a3dccb5d5d1674af0cd472ce
ms.openlocfilehash: 6ba1732840d094497a9e2cb7a7ea7825a7f4bc89

---

# <a name="how-traffic-manager-works"></a>Funktionsweise von Traffic Manager

Mit Azure Traffic Manager können Sie die Verteilung von Datenverkehr auf Anwendungsendpunkte steuern. Ein Endpunkt ist ein Dienst mit Internetzugriff, der innerhalb oder außerhalb von Azure gehostet wird.

Traffic Manager bietet zwei wesentliche Vorteile:

1. Verteilung von Datenverkehr gemäß einer von mehreren [Datenverkehrsrouting-Methoden](traffic-manager-routing-methods.md)
2. [Kontinuierliche Überwachung der Endpunktintegrität](traffic-manager-monitoring.md) und automatisches Failover bei einem Ausfall von Endpunkten

Wenn ein Client eine Verbindung mit einem Dienst herstellen möchte, muss zunächst der DNS-Name des Diensts in eine IP-Adresse aufgelöst werden. Anschließend stellt der Client eine Verbindung mit dieser IP-Adresse her, um auf den Dienst zuzugreifen.

**Wichtig: Traffic Manager operiert auf der DNS-Ebene.**  Traffic Manager verwendet DNS, um Clients anhand der Regeln der Routingmethode für den Datenverkehr an bestimmte Dienstendpunkte weiterzuleiten. Clients stellen mit dem ausgewählten Endpunkt eine **direkte Verbindung** her. Traffic Manager ist weder ein Proxy noch ein Gateway. Traffic Manager hat keinen Zugriff auf den Datenverkehr zwischen Client und Dienst.

## <a name="traffic-manager-example"></a>Traffic Manager-Beispiel

Contoso Corp hat ein neues Partnerportal entwickelt. Die URL für dieses Portal lautet https://partners.contoso.com/login.aspx. Die Anwendung wird in drei Bereichen von Azure gehostet. Traffic Manager wird zur Verteilung des Clientdatenverkehrs auf den nächstgelegenen verfügbaren Endpunkt verwendet, um die Verfügbarkeit zu verbessern und die allgemeine Leistung zu maximieren.

Hierzu geht das Unternehmen wie folgt vor:

* Es stellt drei Instanzen des Diensts bereit. Die DNS-Namen dieser Bereitstellungen lauten „contoso-us.cloudapp.net“, „contoso-eu.cloudapp.net“ und „contoso-asia.cloudapp.net“.
* Anschließend erstellt das Unternehmen ein Traffic Manager-Profil namens „contoso.trafficmanager.net“ und konfiguriert es für die Verwendung der leistungsorientierten Datenverkehrsrouting-Methode für die drei Endpunkte.
* Abschließend konfiguriert es die Vanity-Domänennamen namens „partners.contoso.com“ mit einem Verweis auf „contoso.trafficmanager.net“ (unter Verwendung eines DNS CNAME-Eintrags).

![Traffic Manager-DNS-Konfiguration][1]

> [!NOTE]
> Wenn Sie eine Vanity-Domäne mit Azure Traffic Manager verwenden, müssen Sie mithilfe eines CNAME-Eintrags von Ihrem Vanity-Domänennamen auf Ihren Traffic Manager-Domänennamen verweisen. DNS-Standards lassen nicht zu, dass ein CNAME auf der „Spitze“ (oder dem Stamm) einer Domäne erstellt wird. Daher kann für „contoso.com“ (gelegentlich auch als reiner Domänenname bezeichnet) kein CNAME erstellt werden. Ein CNAME kann nur für eine Domäne unter „contoso.com“ erstellt werden (beispielsweise für „www.contoso.com“). Zur Umgehung dieser Einschränkung empfiehlt sich die Verwendung einer einfachen HTTP-Umleitung, um an „contoso.com“ gerichtete Anforderungen an einen alternativen Namen (etwa „www.contoso.com“) umzuleiten.

## <a name="how-clients-connect-using-traffic-manager"></a>Verbindungsherstellung von Clients über Traffic Manager

Wenn ein Client (das vorherige Beispiel fortgesetzt) die Seite „https://partners.contoso.com/login.aspx“ anfordert, führt er die folgenden Schritte durch, um den DNS-Namen aufzulösen und eine Verbindung herzustellen:

![Verbindungsherstellung mit Traffic Manager][2]

1. Der Client sendet eine DNS-Abfrage zur Auflösung des Namens „partners.contoso.com“ an den konfigurierten rekursiven DNS-Dienst. Ein rekursiver DNS-Dienst wird gelegentlich auch als lokaler DNS-Dienst bezeichnet und fungiert nicht als direkter Host für DNS-Domänen. Stattdessen wird er vom Client zum Abladen der Arbeit für die Kontaktaufnahme mit den verschiedenen autoritativen DNS-Diensten im Internet verwendet, die zum Auflösen eines DNS-Namens benötigt werden.
2. Zum Auflösen des DNS-Namens ermittelt der rekursive DNS-Dienst die Namenserver für die Domäne „contoso.com“. Anschließend fordert er von diesen Namenservern den DNS-Eintrag „partners.contoso.com“ an. Die „contoso.com“-DNS-Server geben den CNAME-Eintrag zurück, der auf „contoso.trafficmanager.net“ verweist.
3. Als Nächstes ermittelt der rekursive DNS-Dienst die Namenserver für die Domäne „trafficmanager net“, die vom Azure Traffic Manager-Dienst bereitgestellt werden. Anschließend sendet er eine Anforderung für den DNS-Eintrag „contoso.trafficmanager.net“ an diese DNS-Server.
4. Die Anforderung geht bei den Traffic Manager-Namenservern ein. Diese wählen einen Endpunkt. Dabei ist Folgendes ausschlaggebend:

    - Konfigurierter Zustand der einzelnen Endpunkte. (Deaktivierte Endpunkte werden nicht zurückgegeben.)
    - Aktuelle Integrität der einzelnen Endpunkte gemäß Traffic Manager-Integritätsüberprüfungen. Weitere Informationen finden Sie unter [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md).
    - Gewählte Datenverkehrsrouting-Methode. Weitere Informationen finden Sie unter [Traffic Manager-Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md).

5. Der ausgewählte Endpunkt wird als anderer DNS-CNAME-Eintrag zurückgegeben. Nehmen wir beispielsweise an, dass „contoso-us.cloudapp.net“ zurückgegeben wird.
6. Als Nächstes ermittelt nun der rekursive DNS-Dienst die Namenserver für die Domäne „cloudapp.net“. Von diesen Namenservern fordert er den DNS-Eintrag „contoso-us.cloudapp.net“ an. Ein DNS-A-Eintrag mit der IP-Adresse des Dienstendpunkts in den USA wird zurückgegeben.
7. Der rekursive DNS-Dienst konsolidiert die Ergebnisse und gibt an den Client eine DNS-Antwort zurück.
8. Der Client erhält die DNS-Ergebnisse und stellt eine Verbindung mit der angegebenen IP-Adresse her. Der Client stellt die Verbindung mit dem Dienstendpunkt der Anwendung direkt und nicht über Traffic Manager her. Da es sich um einen HTTPS-Endpunkt handelt, führt der Client erst den erforderlichen SSL/TLS-Handshake durch und fordert anschließend per HTTP GET-Anforderung die Seite „/login.aspx“ an.

Der rekursive DNS-Dienst speichert die erhaltenen DNS-Antworten im Cache. Von der DNS-Auflösung auf dem Clientgerät wird das Ergebnis ebenfalls im Cache gespeichert. Durch das Speichern im Cache können nachfolgende DNS-Abfragen schneller abgewickelt werden, da keine anderen Namenserver abgefragt werden müssen, sondern die Daten aus dem Cache verwendet werden können. Wie lange die Daten zwischengespeichert werden, hängt von der TTL-Eigenschaft der einzelnen DNS-Einträge ab. Kleinere Werte bedeuten, dass der Cache schneller abläuft und somit mehr Roundtrips zu den Traffic Manager-Nameservern erforderlich sind. Größere Werte bedeuten, dass es möglicherweise länger dauert, bis der Datenverkehr von einem fehlerhaften Endpunkt weggeleitet wird. Mit Traffic Manager können Sie die TTL für Traffic Manager-DNS-Antworten konfigurieren und so einen Wert wählen, der für die Anforderungen Ihrer Anwendung am besten geeignet ist.

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="what-ip-address-does-traffic-manager-use"></a>Welche IP-Adresse verwendet Traffic Manager?

Traffic Manager arbeitet, wie unter „Funktionsweise von Traffic Manager“ erläutert, auf der DNS-Ebene. Clients werden mithilfe von DNS-Antworten an den geeigneten Dienstendpunkt umgeleitet. Anschließend stellen Clients dann eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager).

Traffic Manager bietet daher keinen Endpunkt und keine IP-Adresse für die Verbindungsherstellung von Clients. Wenn also für einen Dienst eine statische IP-Adresse benötigt wird, muss diese im Dienst und nicht in Traffic Manager konfiguriert werden.

### <a name="does-traffic-manager-support-sticky-sessions"></a>Unterstützt Traffic Manager persistente Sitzungen?

Wie bereits [weiter oben](#how-clients-connect-using-traffic-manager) erwähnt, arbeitet Traffic Manager auf der DNS-Ebene. Clients werden mithilfe von DNS-Antworten an den geeigneten Dienstendpunkt umgeleitet. Anschließend stellen Clients dann eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager). Traffic Manager hat somit keinen Zugriff auf den HTTP-Datenverkehr zwischen Client und Server.

Zudem handelt es sich bei der IP-Quelladresse der von Traffic Manager empfangenen DNS-Abfrage um die IP-Adresse des rekursiven DNS-Diensts (und nicht um die IP-Adresse des Clients). Aus diesem Grund kann Traffic Manager keine individuellen Clients nachverfolgen und keine persistenten Sitzungen implementieren. Diese Einschränkung besteht bei allen DNS-basierten Systemen zur Datenverkehrsverwaltung, nicht nur bei Verwendung von Traffic Manager.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Warum tritt bei mir ein HTTP-Fehler auf, wenn ich Traffic Manager verwende?

Wie bereits [weiter oben](#how-clients-connect-using-traffic-manager) erwähnt, arbeitet Traffic Manager auf der DNS-Ebene. Clients werden mithilfe von DNS-Antworten an den geeigneten Dienstendpunkt umgeleitet. Anschließend stellen Clients dann eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager). HTTP-Datenverkehr zwischen Client und Server wird nicht über Traffic Manager abgewickelt. Daher müssen auftretende HTTP-Fehler auf Ihre Anwendung zurückzuführen sein. Damit der Client eine Verbindung mit der Anwendung herstellen kann, werden alle Schritte zur DNS-Auflösung durchgeführt. Hierzu gehört auch die Interaktion von Traffic Manager im Datenverkehrsfluss der Anwendung.

Konzentrieren Sie sich daher bei der weiteren Untersuchung auf die Anwendung.

Der vom Browser des Clients gesendete HTTP-Hostheader ist die häufigste Ursache für Probleme. Konfigurieren Sie die Anwendung so, dass der richtige Hostheader für den verwendeten Domänennamen akzeptiert wird. Informationen zu Endpunkten, die den Azure App Service nutzen, finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für eine Web-App in Azure App Services, der Traffic Manager verwendet](../app-service-web/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-the-performance-impact-of-using-traffic-manager"></a>Wie wirkt sich die Verwendung von Traffic Manager auf die Leistung aus?

Wie bereits [weiter oben](#how-clients-connect-using-traffic-manager) erwähnt, arbeitet Traffic Manager auf der DNS-Ebene. Da Clients eine direkte Verbindung mit Ihren Endpunkten herstellen, hat die Verwendung von Traffic Manager keinerlei Auswirkungen auf die Leistung, sobald die Verbindung hergestellt wurde.

Die Anwendungsintegration von Traffic Manager erfolgt auf der DNS-Ebene, sodass in die DNS-Auflösungskette kein zusätzliches DNS-Lookup eingefügt werden muss (siehe [Traffic Manager-Beispiel](#traffic-manager-example)). Die DNS-Auflösungszeit wird durch Traffic Manager nur minimal beeinträchtigt. Traffic Manager verwendet ein globales Netzwerk von Namenservern sowie [Anycastnetzwerke](https://en.wikipedia.org/wiki/Anycast), um sicherzustellen, dass DNS-Abfragen immer an den nächstgelegenen verfügbaren Namenserver weitergeleitet werden. Dank der Zwischenspeicherung von DNS-Antworten wirkt sich die zusätzliche, durch die Verwendung von Traffic Manager bedingte DNS-Latenz lediglich auf einen Bruchteil der Sitzungen aus.

Mit der leistungsorientierten Methode wird der Datenverkehr an den nächstgelegenen verfügbaren Endpunkt geleitet. Das führt dazu, dass die mit dieser Methode verbundenen Auswirkungen auf die Gesamtleistung minimal sind. Eine höhere DNS-Latenz muss durch eine geringere Netzwerklatenz für den Endpunkt ausgeglichen werden.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Welche Anwendungsprotokolle kann ich mit Traffic Manager verwenden?

Wie bereits [weiter oben](#how-clients-connect-using-traffic-manager) erwähnt, arbeitet Traffic Manager auf der DNS-Ebene. Nach Abschluss des DNS-Lookups stellen Clients eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager). Daher kann für diese Verbindung ein beliebiges Anwendungsprotokoll verwendet werden. Für die Endpunkt-Integritätsprüfungen von Traffic Manager wird jedoch entweder ein HTTP- oder ein HTTPS-Endpunkt benötigt. Für die Integritätsprüfung kann ein anderer Endpunkt als der Anwendungsendpunkt verwendet werden, mit dem Clients eine Verbindung herstellen.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Kann ich Traffic Manager mit einem reinen Domänennamen verwenden?

Nein. Die DNS-Standards lassen keine Koexistenz von CNAME-Einträgen und anderen DNS-Einträgen mit demselben Namen zu. Die Spitze (oder der Stamm) einer DNS-Zone enthält immer zwei bereits vorhandene DNS-Einträge: den SOA-Eintrag und den autoritativen NS-Eintrag. Das bedeutet, dass im Zonenspitze kein CNAME-Eintrag erstellt werden kann, ohne gegen die DNS-Standards zu verstoßen.

Wie im [Traffic Manager-Beispiel](#traffic-manager-example) beschrieben, benötigt Traffic Manager einen DNS-CNAME-Eintrag zum Zuordnen des Vanity-DNS-Namens. „www.contoso.com“ wird beispielsweise dem DNS-Namen des Traffic Manager-Profils „contoso.trafficmanager.net“ zugeordnet. Darüber hinaus gibt das Traffic Manager-Profil selbst einen zweiten DNS-CNAME-Eintrag zurück, um anzugeben, mit welchem Endpunkt der Client eine Verbindung herstellen soll.

Um dieses Problem zu umgehen, wird empfohlen, eine HTTP-Umleitung zu verwenden, um Datenverkehr von einem reinen Domänennamen an eine andere URL umzuleiten, die Traffic Manager verwenden kann. So kann beispielsweise die reine Domäne „contoso.com“ Benutzer an den CNAME „www.contoso.com“ weiterleiten, der auf den DNS-Namen von Traffic Manager verweist.

Die uneingeschränkte Unterstützung von Naked-Domänen in Traffic Manager ist Teil unseres Feature-Backlogs. Sie können die Unterstützung für diese Funktionsanforderung registrieren, indem Sie [auf unserer Community-Feedbackwebsite dafür abstimmen](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich ausführlicher über [Endpunktüberwachung und automatisches Failover](traffic-manager-monitoring.md)von Traffic Manager.

Informieren Sie sich ausführlicher über [Traffic Manager-Routingmethoden](traffic-manager-routing-methods.md).

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png



<!--HONumber=Nov16_HO5-->


