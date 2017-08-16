---
title: Beschleunigung dynamischer Websites durch Azure CDN
description: Deep-Dive-Analysen durch Beschleunigung dynamischer Websites
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: v-semcev
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: be2719e0e02c8bc69800ef4a3e7da3c3164cb9dd
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---
# <a name="dynamic-site-acceleration-via-azure-cdn"></a>Beschleunigung dynamischer Websites durch Azure CDN

Die explosionsartige Verbreitung von sozialen Medien, E-Commerce und hyperpersonalisierten Webinhalten führt dazu, dass eine rasant zunehmende Menge von Bereitstellungsinhalten für Endbenutzer in Echtzeit generiert wird. Benutzer erwarten schnelle, zuverlässige und personalisierte Weberlebnisse – unabhängig von ihrem Browser, Standort, Gerät oder Netzwerk. Genau jene Innovationen, die für diese hervorragenden Erfahrungen sorgen sollen, bringen jedoch auch lange Seitendownloadzeiten mit sich und beeinträchtigen die Qualität der Benutzererfahrung für den Kunden. 

Zu den Standardfunktionen von CDN zählt die Möglichkeit, Dateien näher beim Endbenutzer zwischenzuspeichern, um die Übermittlung statischer Dateien zu beschleunigen. Bei dynamischen Webanwendungen ist die Zwischenspeicherung dieser Inhalte an Edgestandorten jedoch nicht möglich, da der Server die Inhalte als Reaktion auf das Benutzerverhalten generiert. Die Beschleunigung der Übermittlung derartiger Inhalte ist komplexer als konventionelle Edgezwischenspeicherung und erfordert eine Komplettlösung, die jedes Element entlang des gesamten Datenpfads vom Beginn bis zur Übermittlung exakt optimiert. Durch die Beschleunigung dynamischer Websites (Dynamic Site Acceleration, DSA) von Azure CDN wird die Leistung von Webseiten mit dynamischen Inhalten merklich verbessert.

Azure CDN von Akamai und Verizon ermöglicht während der Erstellung des Endpunkts über das Menü **Optimiert für** eine Optimierung der Beschleunigung dynamischer Websites.

## <a name="configuring-cdn-endpoint-to-accelerate-delivery-of-dynamic-files"></a>Konfigurieren von CDN-Endpunkten zur Beschleunigung der Übermittlung dynamischer Dateien

Sie können Ihren CDN-Endpunkt für eine optimierte Übermittlung dynamischer Dateien über das Azure-Portal konfigurieren, indem Sie während der Erstellung des Endpunkts die Option **Beschleunigung dynamischer Websites** unter der Eigenschaftenauswahl **Optimiert für** auswählen. Sie können auch unsere REST-APIs oder die Client-SDKs verwenden, um dies programmgesteuert durchzuführen. 

### <a name="probe-path"></a>Testpfad
Der Testpfad ist eine spezielle Funktion für die Beschleunigung dynamischer Websites. Für die Erstellung ist ein gültiger Testpfad erforderlich. Bei der Beschleunigung dynamischer Websites wird eine kleine *Testpfad*-Datei verwendet, die am Ursprung eingefügt wird, um die Netzwerkroutingkonfiguration für CDN zu optimieren. Sie können unsere Beispieldatei herunterladen und in Ihre Website hochladen oder stattdessen eine vorhandene Ressource mit ungefähr 10 KB für den Testpfad in Ihrem Ursprung verwenden, sofern diese vorhanden ist.

> [!Note]
> Für die Beschleunigung dynamischer Websites fallen zusätzliche Gebühren an. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/cdn/).

Die folgenden Screenshots veranschaulichen die Vorgehensweise über das Azure-Portal.
 
![Hinzufügen eines neuen CDN-Endpunkts](./media/cdn-dynamic-site-acceleration/01_Endpoint_Profile.png) 

*Abbildung 1: Hinzufügen eines neuen CDN-Endpunkts aus dem CDN-Profil*
 
![Erstellen eines neuen CDN-Endpunkts durch die Beschleunigung dynamischer Websites](./media/cdn-dynamic-site-acceleration/02_Optimized_DSA.png)  

*Abbildung 2: Erstellen eines CDN-Endpunkts mit ausgewählter Option „Beschleunigung dynamischer Websites“*

Nachdem der CDN-Endpunkt erstellt wurde, werden die Optimierungen für die Beschleunigung dynamischer Websites auf alle Dateien angewendet, die bestimmte Kriterien erfüllen. Im folgenden Abschnitt wird die Optimierung der Beschleunigung dynamischer Websites ausführlich beschrieben.

## <a name="dsa-optimization-using-azure-cdn"></a>Optimierung für die Beschleunigung dynamischer Websites mit Azure CDN

Bei der Beschleunigung dynamischer Websites mit Azure CDN wird die Übermittlung dynamischer Ressourcen durch folgende Methoden beschleunigt:

-   Routenoptimierung
-   TCP-Optimierungen
-   Objektvorabruf (nur Akamai)
-   Mobile Bildkomprimierung (nur Akamai)

### <a name="route-optimization"></a>Routenoptimierung

Die Routenoptimierung spielt eine entscheidende Rolle, da das Internet von Dynamik geprägt ist und Datenverkehr sowie vorübergehende Ausfälle die Netzwerktopologie stetig verändern. Das Border Gateway Protocol (BGP) ist das Internet-Standardroutingprotokoll, aber es gibt unter Umständen schnellere Routen über zwischengeschaltete PoP-Server (Point of Presence). 

Die Routenoptimierung wählt den optimalen Pfad zum Ursprung, damit eine Website durchgängig zugänglich ist und dynamische Inhalte Endbenutzern über die schnellstmögliche und zuverlässigste Route übermittelt werden. 

Das Akamai-Netzwerk nutzt Techniken zum Sammeln von Echtzeitdaten und Vergleichen verschiedener Pfade über unterschiedliche Knoten auf dem Akamai-Server. Darüber hinaus kommt auch die BGP-Standardroute innerhalb des öffentlichen Internets zum Einsatz, um die schnellste Route zwischen dem Ursprung und dem CDN-Edge zu bestimmen. Mit diesen Verfahren werden Internet-Staufallen und lange Routen vermieden. 

Auf ähnliche Weise wird im Verizon-Netzwerk eine Kombination aus Anycast DNS, Unterstützungs-PoPs mit hoher Kapazität und Integritätsprüfungen verwendet, um die besten Gateways zu ermitteln, die für die Weiterleitung von Daten vom Client zum Ursprung am ehesten geeignet sind.
 
Das Ergebnis: Vollständig dynamische und transaktionale Inhalt werden schneller und zuverlässiger an Endbenutzer übermittelt – selbst dann, wenn sie nicht zwischengespeichert werden können. 

### <a name="tcp-optimizations"></a>TCP-Optimierungen

Das Transmission Control-Protokoll (TCP) ist das Standardprotokoll der Internetprotokollfamilie, um Informationen zwischen Anwendungen in einem IP-Netzwerk zu übermitteln.  Für die Einrichtung einer TCP-Verbindung müssen standardmäßig mehrere Anforderungen hin- und hergeleitet werden. Zudem sind auch Grenzwerte erforderlich, um Überlastungspunkte im Netzwerk zu vermeiden, die zu Ineffizienzen bei der Skalierung führen. Für Azure CDN von Akamai wird dieses Problem bewältigt, indem Optimierungen in drei Bereichen vorgenommen werden: 

 - Beseitigen von langsamen Starts
 - Nutzen von permanenten Verbindungen
 - Anpassen von TCP-Paketparametern (nur Akamai)

#### <a name="eliminating-slow-start"></a>Beseitigen von langsamen Starts

Der *langsame Start* ist ein Teil des TCP-Protokolls, der eine Netzwerküberlastung verhindert, indem die Menge der über das Netzwerk gesendeten Daten beschränkt wird. Er beginnt mit kleinen Überlastungsfenstergrößen zwischen Absender und Empfänger, bis der Höchstwert erreicht oder ein Paketverlust erkannt wird.

Für Azure CDN von Akamai und Verizon wird das langsame Startverhalten mithilfe von drei Schritten beseitigt:

1.  In den Netzwerken von Akamai und Verizon wird die Integritäts- und Bandbreitenüberwachung eingesetzt, um die Bandbreite von Verbindungen zwischen PoP-Edgeservern zu messen.
2. Die Metriken werden von PoP-Edgeservern gemeinsam genutzt, sodass jeder Server über die Netzwerkbedingungen und die Serverintegrität der anderen PoP-Komponenten in der Nähe informiert ist.  
3. Die CDN-Edgeserver sind nun in der Lage, Annahmen über einige Übertragungsparameter zu treffen, z.B. über die optimale Fenstergröße, den Zeitpunkt, an dem die Kommunikation mit anderen CDN-Edgeservern in dessen Nähe stattfinden soll. Dieser Schritt bedeutet, dass die anfängliche Überlastungsfenstergröße erhöht werden kann, wenn die Integrität der Verbindung zwischen den CDN-Edgeservern höheren Paketdatenübertragungen standhalten kann.  

#### <a name="leveraging-persistent-connections"></a>Nutzen von permanenten Verbindungen

Mit einer CDN-Instanz stellen vergleichsweise weniger eindeutige Computergruppen eine direkte Verbindung mit Ihrem Ursprungsserver her als Benutzer. Im Azure CDN von Akamai und Verizon werden Benutzeranforderungen zudem in Pools zusammengefasst, um weniger Verbindungen mit dem Ursprungsserver herzustellen.

Wie bereits erwähnt wurde, leiten TCP-Verbindungen mehrere Anforderungen in einen Handshake hin und her, um eine neue Verbindung herstellen. Permanente Verbindungen, auch als „HTTP-Keep-Alive“ bezeichnet, nutzen die Wiederverwendung von vorhandenen TCP-Verbindungen für mehrere HTTP-Anforderungen, um Roundtripzeiten zu sparen und die Übermittlung zu beschleunigen. 

Für das Verizon-Netzwerk werden außerdem regelmäßige Keep-Alive-Pakete über die TCP-Verbindung gesendet, um zu verhindern, dass eine offene Verbindung geschlossen wird.

#### <a name="tuning-tcp-packet-parameters"></a>Anpassen von TCP-Paketparametern

Azure CDN von Akamai passt auch Parameter an, die Verbindungen zwischen Servern regeln, und verringert die Menge langer Roundtrips, die erforderlich sind, um die auf der Website eingebetteten Inhalte abzurufen. Dies geschieht mit den folgenden Verfahren:

1.  Durch Vergrößern des anfänglichen Überlastungsfensters, damit eine größere Anzahl von Paketen gesendet werden kann, ohne auf eine Bestätigung zu warten.
2.  Durch Verringern des anfänglichen Neuübertragungstimeouts, damit ein Verlust schneller erkannt und die Übertragung beschleunigt wird.
3.  Durch Verringern des minimalen und maximalen Neuübertragungstimeouts, um die Wartezeit zu verkürzen, bevor die Annahme getroffen werden muss, dass bei der Übertragung Pakete verloren gegangen sind.

### <a name="object-prefetch-akamai-only"></a>Objektvorabruf (nur Akamai)

Die meisten Websites bestehen aus einer HTML-Seite, die auf verschiedene andere Ressourcen wie Bilder und Skripts verweist. Wenn ein Client eine Webseite anfordert, lädt der Browser in der Regel zunächst das HTML-Objekt herunter und analysiert es. Anschließend werden zusätzliche Anforderungen an verknüpfte Ressourcen gesendet, die zum vollständigen Laden der Seite erforderlich sind. 

Der *Vorabruf* ist eine Methode zum Abrufen von Bildern und Skripts, die auf der HTML-Seite eingebettet sind, während die HTML-Seite im Browser verarbeitet wird und sogar bevor der Browser diese Objektanforderungen stellt. 

Sofern die Option **Vorabrufen** aktiviert ist, wenn das CDN die HTML-Basisseite im Browser des Clients verarbeitet, analysiert das CDN die HTML-Datei, sendet zusätzliche Anforderungen für verknüpfte Ressourcen und speichert diese im jeweiligen Cache. Wenn der Client die Anforderungen für die verknüpften Ressourcen sendet, verfügt der CDN-Edgeserver bereits über die angeforderten Objekte und kann sie sofort ohne Roundtrip zum Ursprung verarbeiten. Diese Optimierung ist sowohl für zwischenspeicherbare als auch nicht zwischenspeicherbare Inhalte von Vorteil.

### <a name="adaptive-image-compression-akamai-only"></a>Adaptive Bildkomprimierung (nur Akamai)

Bei einigen Geräten, insbesondere mobilen Geräten, kommt es bisweilen zu langsameren Netzwerkgeschwindigkeiten. In diesen Szenarien ist es für den Benutzer besser, schneller kleinere Bilder auf der Webseite zu sehen, als lange auf die Anzeige von Bildern mit voller Auflösung zu warten.

Diese Funktion überwacht automatisch die Netzwerkqualität und wendet bei langsamen Netzwerkgeschwindigkeiten standardmäßige JPEG-Komprimierungsmethoden an, um die Übermittlungszeit zu verkürzen.

Adaptive Bildkomprimierung | Dateierweiterungen  
--- | ---  
JPEG-Komprimierung | .jpg, .jpeg, .jpe, .jig, .jgig, .jgi

## <a name="caching"></a>Caching

Für die Beschleunigung dynamischer Websites ist die Zwischenspeicherung im CDN standardmäßig deaktiviert. Dies gilt auch, wenn vom Ursprung Cache-Control/Expires-Header in die Antwort eingefügt werden. Diese Standardeinstellung ist deaktiviert, da die Beschleunigung dynamischer Websites normalerweise für dynamische Ressourcen verwendet wird, die nicht zwischengespeichert werden sollten. Sie sind für jeden Client eindeutig, und die Aktivierung der Zwischenspeicherung als Standardeinstellung kann dazu führen, dass dieses Verhalten beeinträchtigt wird.

Wenn Sie über eine Website mit einer Mischung aus statischen und dynamischen Ressourcen verfügen, erzielen Sie die beste Leistung, indem Sie sich für einen Hybridansatz entscheiden. 

Bei Verwendung des ADN mit Verizon Premium können Sie die Zwischenspeicherung für bestimmte Fälle mit dem Regelmodul wieder aktivieren.  

Eine Alternative hierzu ist die Verwendung von zwei CDN-Endpunkten. Ein Endpunkt mit Beschleunigung dynamischer Websites zur Bereitstellung von dynamischen Ressourcen und ein anderer mit einem statischen Optimierungstyp, z.B. allgemeine Webbereitstellung, für die Bereitstellung von zwischenspeicherbaren Ressourcen. Um dies zu erreichen, ändern Sie Ihre Webseiten-URLs so, dass eine direkte Verknüpfung zur Ressource auf dem CDN-Endpunkt hergestellt wird, den Sie verwenden möchten. 

Beispiel: `mydynamic.azureedge.net/index.html` ist eine dynamische Seite, die vom Endpunkt mit Beschleunigung dynamischer Websites geladen wird.  Die HTML-Seite verweist auf mehrere statische Ressourcen, z.B. JavaScript-Bibliotheken oder Images, die vom statischen CDN-Endpunkt geladen werden, z.B. `mystatic.azureedge.net/banner.jpg` und `mystatic.azureedge.net/scripts.js`. 

[Hier](https://docs.microsoft.com/azure/cdn/cdn-cloud-service-with-cdn#controller) finden Sie ein Beispiel dazu, wie Sie Controller in einer ASP.NET-Webanwendung nutzen, um Inhalt über eine bestimmte CDN-URL bereitzustellen.





