---
title: Was ist Traffic Manager? | Microsoft Docs
description: "In diesem Artikel erfahren Sie, worum es sich bei Traffic Manager handelt und ob Traffic Manager die richtige Datenverkehrsrouting-Lösung für Ihre Anwendung ist."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: kumud
ms.translationtype: HT
ms.sourcegitcommit: bde1bc7e140f9eb7bb864c1c0a1387b9da5d4d22
ms.openlocfilehash: 50d7f14d0d4234ee98d8a46e903b5f916cb02fab
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---

# <a name="overview-of-traffic-manager"></a>Traffic Manager – Übersicht

Microsoft Azure Traffic Manager ermöglicht die Verteilung von Benutzerdatenverkehr für Dienstendpunkte in unterschiedlichen Rechenzentren. Zu den von Traffic Manager unterstützten Dienstendpunkten zählen virtuelle Azure-Computer, Web-Apps und Clouddienste. Darüber hinaus kann Traffic Manager auch mit externen, Azure-fremden Endpunkten verwendet werden.

Traffic Manager verwendet das Domain Name System (DNS), um Clientanforderungen auf der Grundlage einer Datenverkehrsrouting-Methode und der Integrität der Endpunkte an den optimalen Endpunkt weiterzuleiten. Traffic Manager bietet eine Reihe von [Datenverkehrsrouting-Methoden](traffic-manager-routing-methods.md) und [Endpunktüberwachungsoptionen](traffic-manager-monitoring.md), die verschiedene Anwendungsanforderungen erfüllen sowie automatisches Failover ermöglichen. Traffic Manager zeichnet sich durch eine geringe Fehleranfälligkeit aus, selbst wenn es zu einem Ausfall einer ganzen Azure-Region kommt.

## <a name="traffic-manager-benefits"></a>Vorteile von Traffic Manager

Traffic Manager unterstützt Sie bei folgenden Aufgaben:

* **Verbessern der Verfügbarkeit kritischer Anwendungen**

    Mit Traffic Manager können Sie die Verfügbarkeit wichtiger Anwendungen gewährleisten, indem Sie Ihre Endpunkte überwachen und beim Ausfall eines Endpunkts automatisches Failover bereitstellen.

* **Verbessern der Reaktionsfähigkeit hochleistungsfähiger Anwendungen**

    Azure ermöglicht die Ausführung von Cloud-Diensten oder Websites in Datencentern auf der ganzen Welt. Traffic Manager kann die Reaktionsfähigkeit Ihrer Anwendungen verbessern und Endbenutzer an den Endpunkt mit der niedrigsten Netzwerklatenz für den Client weiterleiten.

* **Ausführen von Upgrades oder Dienstwartungsarbeiten ohne Ausfallzeiten**

    Geplante Wartungsarbeiten für Ihre Anwendungen können ohne Ausfallzeiten für Ihre Anwendungen durchgeführt werden. Hierzu wird der Datenverkehr während der Wartung von Traffic Manager an andere Endpunkte weitergeleitet.

* **Kombinieren von lokalen und cloudbasierten Anwendungen**

    Traffic Manager unterstützt externe, Azure-fremde Endpunkte und kann somit in hybriden, cloudbasierten bzw. lokalen Bereitstellungen einschließlich „burst-to-cloud“ (Ausweichen in die Cloud bei Nachfragespitzen), „migrate-to-cloud“ (Migration in die Cloud) und „failover-to-cloud“ (Failover in die Cloud) verwendet werden.

* **Verteilen des Datenverkehrs für große, komplexe Bereitstellungen**

    Datenverkehrsrouting-Methoden können mit [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md) zu komplexen und flexiblen Datenverkehrsrouting-Regeln kombiniert werden, die auch den Anforderungen umfangreicher und komplexer Bereitstellungen gerecht werden.

## <a name="how-traffic-manager-works"></a>Funktionsweise von Traffic Manager

Mit Azure Traffic Manager können Sie die Verteilung von Datenverkehr auf Anwendungsendpunkte steuern. Ein Endpunkt ist ein Dienst mit Internetzugriff, der innerhalb oder außerhalb von Azure gehostet wird.

Traffic Manager bietet zwei wesentliche Vorteile:

1. Verteilung von Datenverkehr gemäß einer von mehreren [Datenverkehrsrouting-Methoden](traffic-manager-routing-methods.md)
2. [Kontinuierliche Überwachung der Endpunktintegrität](traffic-manager-monitoring.md) und automatisches Failover bei einem Ausfall von Endpunkten

Wenn ein Client eine Verbindung mit einem Dienst herstellen möchte, muss zunächst der DNS-Name des Diensts in eine IP-Adresse aufgelöst werden. Anschließend stellt der Client eine Verbindung mit dieser IP-Adresse her, um auf den Dienst zuzugreifen.

**Wichtig: Traffic Manager operiert auf der DNS-Ebene.**  Traffic Manager verwendet DNS, um Clients anhand der Regeln der Routingmethode für den Datenverkehr an bestimmte Dienstendpunkte weiterzuleiten. Clients stellen mit dem ausgewählten Endpunkt eine **direkte Verbindung** her. Traffic Manager ist weder ein Proxy noch ein Gateway. Traffic Manager hat keinen Zugriff auf den Datenverkehr zwischen Client und Dienst.

### <a name="traffic-manager-example"></a>Traffic Manager-Beispiel

Contoso Corp hat ein neues Partnerportal entwickelt. Die URL für dieses Portal lautet https://partners.contoso.com/login.aspx. Die Anwendung wird in drei Bereichen von Azure gehostet. Traffic Manager wird zur Verteilung des Clientdatenverkehrs auf den nächstgelegenen verfügbaren Endpunkt verwendet, um die Verfügbarkeit zu verbessern und die allgemeine Leistung zu maximieren.

Um diese Konfiguration zu erzielen, werden die folgenden Schritte ausgeführt:

1. Drei Instanzen des Diensts werden bereitgestellt. Die DNS-Namen dieser Bereitstellungen lauten „contoso-us.cloudapp.net“, „contoso-eu.cloudapp.net“ und „contoso-asia.cloudapp.net“.
2. Ein Traffic Manager-Profil namens „contoso.trafficmanager.net“ wird erstellt und für die Verwendung der leistungsorientierten Datenverkehrsrouting-Methode für die drei Endpunkte konfiguriert.
* Der Vanity-Domänenname „partners.contoso.com“ mit einem Verweis auf „contoso.trafficmanager.net“ (unter Verwendung eines DNS CNAME-Eintrags) wird konfiguriert.

![Traffic Manager-DNS-Konfiguration][1]

> [!NOTE]
> Wenn Sie eine Vanity-Domäne mit Azure Traffic Manager verwenden, müssen Sie mithilfe eines CNAME-Eintrags von Ihrem Vanity-Domänennamen auf Ihren Traffic Manager-Domänennamen verweisen. DNS-Standards lassen nicht zu, dass ein CNAME auf der „Spitze“ (oder dem Stamm) einer Domäne erstellt wird. Daher kann für „contoso.com“ (gelegentlich auch als reiner Domänenname bezeichnet) kein CNAME erstellt werden. Ein CNAME kann nur für eine Domäne unter „contoso.com“ erstellt werden (beispielsweise für „www.contoso.com“). Zur Umgehung dieser Einschränkung empfiehlt sich die Verwendung einer einfachen HTTP-Umleitung, um an „contoso.com“ gerichtete Anforderungen an einen alternativen Namen (etwa „www.contoso.com“) umzuleiten.

### <a name="how-clients-connect-using-traffic-manager"></a>Verbindungsherstellung von Clients über Traffic Manager

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

Der rekursive DNS-Dienst speichert die erhaltenen DNS-Antworten im Cache. Von der DNS-Auflösung auf dem Clientgerät wird das Ergebnis ebenfalls im Cache gespeichert. Durch das Speichern im Cache können nachfolgende DNS-Abfragen schneller abgewickelt werden, da keine anderen Namenserver abgefragt werden müssen, sondern die Daten aus dem Cache verwendet werden können. Wie lange die Daten zwischengespeichert werden, hängt von der TTL-Eigenschaft der einzelnen DNS-Einträge ab. Kleinere Werte bedeuten, dass der Cache schneller abläuft und somit mehr Roundtrips zu den Traffic Manager-Nameservern erforderlich sind. Größere Werte bedeuten, dass es möglicherweise länger dauert, bis der Datenverkehr von einem fehlerhaften Endpunkt weggeleitet wird. Mit Traffic Manager können Sie die TTL für Traffic Manager-DNS-Antworten von 0 bis 2.147.483.647 Sekunden (maximale Länge, die [RFC-1035](https://www.ietf.org/rfc/rfc1035.txt) entspricht)konfigurieren und so einen Wert wählen, der für die Anforderungen Ihrer Anwendung am besten geeignet ist.

## <a name="pricing"></a>Preise

Preisinformationen finden Sie unter [Traffic Manager – Preise](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="faq"></a>Häufig gestellte Fragen

Häufig gestellte Fragen zu Traffic Manager finden Sie unter [Häufig gestellte Fragen zu Traffic Manager](traffic-manager-FAQs.md).

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich ausführlicher über [Endpunktüberwachung und automatisches Failover](traffic-manager-monitoring.md)von Traffic Manager.

Informieren Sie sich ausführlicher über [Traffic Manager-Routingmethoden](traffic-manager-routing-methods.md).

Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) von Azure.

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png


