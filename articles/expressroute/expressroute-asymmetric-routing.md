<properties
   pageTitle="Asymmetrisches Routing | Microsoft Azure"
   description="Dieser Artikel geht auf Probleme ein, die bei Kunden mit asymmetrischem Routing in einem Netzwerk mit mehreren Zielverbindungen auftreten können."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/23/2016"
   ms.author="osamazia"/>

# Asymmetrisches Routing mit mehreren Netzwerkpfaden

In diesem Artikel erfahren Sie, wie ausgehender und eingehender Datenverkehr über unterschiedliche Routen abgewickelt werden kann, wenn zwischen Quelle und Ziel mehrere Pfade verfügbar sind.

Zum besseren Verständnis des asymmetrischen Routings müssen wir uns mit zwei Konzepten vertraut machen: Da wären zum einen die Auswirkungen mehrerer Netzwerkpfade und zum anderen das Verhalten der Geräte mit Zustandsbeibehaltung (beispielsweise Firewalls). Diese Geräte werden als „zustandsbehaftet“ bezeichnet. Durch die Kombination dieser beiden Faktoren ergeben sich Szenarien, in denen der Datenverkehr von einem zustandsbehafteten Gerät verworfen wird, da der Datenverkehr nicht vom Gerät selbst stammt.

## Mehrere Netzwerkpfade

Wenn ein Unternehmensnetzwerk nur über eine einzelne Internetdienstanbieter-Verbindung mit dem Internet verfügt, wird der gesamte ein- und ausgehende Internetdatenverkehr über den gleichen Pfad abgewickelt. Zur Verbesserung der Netzwerkverfügbarkeit erwerben Unternehmen häufig mehrere Verbindungen als redundante Pfade. In solchen Fällen kann ausgehender Datenverkehr (also Datenverkehr, der das Netzwerk in Richtung des Internets verlässt) über eine andere Verbindung abgewickelt werden als eingehender Datenverkehr. Dies wird allgemein als asymmetrisches Routing bezeichnet, da für den eingehenden Datenverkehr ein anderer Pfad verwendet wird als für den ursprünglichen Datenfluss.

![Routing3](./media/expressroute-asymmetric-routing/AsymmetricRouting3.png)

Die obige Beschreibung gilt nicht nur für das Internet, sondern auch für andere Kombinationen mehrerer Pfade. Beispiele wären etwa ein Internetpfad und ein privater Pfad zum gleichen Ziel oder mehrere private Pfade zum gleichen Ziel.

Auf dem Weg zwischen Quelle und Ziel berechnen die einzelnen Router jeweils den optimalen Pfad zum Ziel. Die Bestimmung des optimalen Pfads basiert auf zwei zentralen Faktoren:

1.	Routing zwischen externen Netzwerken basiert auf einem Routingprotokoll namens BGP (Border Gateway-Protokoll). BGP nimmt Ankündigungen von Nachbarn entgegen, führt eine Reihe von Schritten aus, um den optimalen Pfad zum Ziel zu bestimmen, und nimmt ihn in die eigene Routingtabelle auf.
2.	Länge der Subnetzmaske, die einer Route zugeordnet ist. Wenn mehrere Ankündigungen für die gleiche IP-Adresse, aber für unterschiedliche Subnetzmasken empfangen werden, wird die Ankündigung mit der längeren Subnetzmaske bevorzugt, da sie als die spezifischere Route gilt.

## Zustandsbehaftete Geräte

Router analysieren zu Routingzwecken den IP-Header des Pakets. Es gibt allerdings Geräte, die das Paket noch ausführlicher untersuchen. Diese Geräte analysieren in der Regel Layer4-Header (TCP/UDP) oder sogar Layer7-Header (Anwendungsschicht). Bei diesen Geräten handelt es sich entweder um Sicherheitsgeräte oder um Geräte zur Bandbreitenoptimierung. Die Firewall ist ein gängiges Beispiel für ein zustandsbehaftetes Gerät. Sie gewährt oder verhindert die Übermittlung eines Pakets über ihre Schnittstellen auf der Grundlage verschiedener Aspekte wie Protokoll, TCP/UDP-Port und URL-Header. Diese Paketüberprüfung ist für das Gerät mit einem hohen Verarbeitungsaufwand verbunden. Zur Verbesserung der Leistung prüft die Firewall das erste Paket eines Datenflusses. Ist das Paket zulässig, werden die Datenflussinformationen in der Zustandstabelle gespeichert. Alle nachfolgenden Pakete dieses Datenflusses werden auf der Grundlage der ursprünglichen Entscheidung zugelassen. Wenn also bei der Firewall ein Paket eingeht, das zu einem bereits vorhandenen Datenfluss gehört, und der Firewall noch keine dazugehörigen Zustandsinformationen vorliegen, wird das Paket von der Firewall verworfen.

## Asymmetrisches Routing mit ExpressRoute

Beim Herstellen einer ExpressRoute-Verbindung mit Microsoft ergeben sich die folgenden Änderungen für Ihr Netzwerk:

1.	Sie verfügen über mehrere Verbindungen mit Microsoft: Ihre vorhandene Internetverbindung und eine ExpressRoute-Verbindung. Ein Teil des Datenverkehrs an Microsoft kann über die Internetverbindung gesendet und über ExpressRoute empfangen werden (oder umgekehrt).
2.	Über ExpressRoute erhalten Sie spezifischere IP-Adressen. Daher wird bei Datenverkehr von Ihrem Netzwerk an Microsoft für Dienste, die über ExpressRoute angeboten werden, immer ExpressRoute bevorzugt.

Zum besseren Verständnis der Auswirkungen der beiden oben genannten Punkte sehen wir uns einige Szenarien an. Angenommen, Sie verfügen lediglich über eine einzelne Verbindung mit dem Internet und nutzen alle Microsoft-Dienste über die Internetverbindung. Der Datenverkehr zwischen Ihrem Netzwerk und Microsoft wird über die gleiche Internetverbindung abgewickelt und durchläuft die Firewall. Die Firewall erfasst den Datenfluss beim Kontakt mit dem ersten Paket und lässt zurückgegebene Pakete zu, da der Datenfluss in der Zustandstabelle enthalten ist.

![Routing1](./media/expressroute-asymmetric-routing/AsymmetricRouting1.png)


Nun aktivieren Sie ExpressRoute und nutzen Dienste, die von Microsoft über ExpressRoute angeboten werden. Alle anderen Dienste von Microsoft werden über die Internetverbindung genutzt. Sie stellen am Rand der ExpressRoute-Verbindung eine separate Firewall bereit. Für bestimmte Dienste kündigt Microsoft Ihrem Netzwerk gegenüber per ExpressRoute spezifischere Präfixe an. Für diese Präfixe verwendet Ihre Routinginfrastruktur ExpressRoute als bevorzugten Pfad. Wenn Sie Ihre öffentlichen IP-Adressen Microsoft gegenüber nicht per ExpressRoute ankündigen, kommuniziert Microsoft mit Ihren öffentlichen IP-Adressen über die Internetverbindung. Für an Microsoft gerichteten Datenverkehr Ihres Netzwerks wird also ExpressRoute verwendet, während eingehender Datenverkehr von Microsoft über die Internetverbindung abgewickelt wird. Falls bei der Edgefirewall ein Antwortpaket für einen Datenfluss eingeht, der nicht in der Zustandstabelle enthalten ist, wird der eingehende Datenverkehr verworfen.

Wenn Sie für ExpressRoute und Internet den gleichen NAT-Pool verwenden, treten ähnliche Probleme für die Clients mit privaten IP-Adressen in Ihrem Netzwerk auf. Anforderungen für Dienste wie Windows Update werden über die Internetverbindung abgewickelt, da IP-Adressen für diese Dienste nicht per ExpressRoute angekündigt werden. Der eingehende Datenverkehr wird hingegen über ExpressRoute zurückgegeben. Wenn Microsoft über die Internet- und die ExpressRoute-Verbindung eine IP-Adresse mit der gleichen Subnetzmaske erhält, wird ExpressRoute der Internetverbindung vorgezogen. Wenn einer Firewall oder einem anderen zustandsbehafteten Gerät an dem ExpressRoute zugewandten Netzwerkrand noch keine Informationen zu dem Datenfluss vorliegen, werden die Pakete des betreffenden Datenflusses verworfen.

## Lösungen für asymmetrisches Routing

Asymmetrisches Routing kann grundsätzlich auf zwei Arten behandelt werden: mittels Routing oder mittels quellenbasierter NAT (Source NAT, SNAT).

1. Routing

    - Stellen Sie sicher, dass Ihre öffentlichen IP-Adressen gegenüber den entsprechenden WAN-Verbindungen angekündigt werden. Angenommen, Sie möchten die Internetverbindung für Authentifizierungsdatenverkehr verwenden und Ihren E-Mail-Datenverkehr über ExpressRoute abwickeln. In diesem Fall dürfen Sie Ihre öffentlichen AD FS-IP-Adressen nicht über ExpressRoute ankündigen. Analog dazu darf der lokale AD FS-Server nicht für IP-Adressen verfügbar gemacht werden, die über ExpressRoute empfangen werden. Über ExpressRoute eingehende Routen sind spezifischer, sodass ExpressRoute als Pfad für Authentifizierungsdatenverkehr an Microsoft bevorzugt wird, was asymmetrisches Routing zur Folge hat.

    - Wenn Sie ExpressRoute für die Authentifizierung verwenden möchten, müssen öffentliche AD FS-IP-Adressen über ExpressRoute ohne NAT angekündigt werden. Dadurch wird von Microsoft stammender Datenverkehr für den lokalen AD FS-Server über ExpressRoute abgewickelt, und Datenverkehr, der vom Kunden an Microsoft zurückgegeben wird, wird ebenfalls über ExpressRoute abgewickelt, da ExpressRoute gegenüber der Internetverbindung bevorzugt wird.

2. Quellenbasierte NAT

	Eine weitere Möglichkeit zur Behandlung von Problemen durch asymmetrisches Routing ist die Quell-NAT (Source NAT, SNAT). Angenommen, Sie haben die öffentliche IP-Adresse des lokalen SMTP-Servers nicht über ExpressRoute angekündigt und möchten für diese Kommunikation die Internetverbindung verwenden. Eine an Ihren lokalen SMTP-Server gerichtete Anforderung von Microsoft durchläuft das Internet. Die eingehende Anforderung wird von der Quell-NAT verarbeitet an eine interne IP-Adresse weitergeleitet. Der vom SMTP-Server zurückgegebene Datenverkehr wird nicht an ExpressRoute, sondern zur Durchführung der NAT an die Edgefirewall übermittelt. Dadurch wird der zurückgegebene Datenverkehr wieder über die Internetverbindung abgewickelt.


![Routing2](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## Erkennung von asymmetrischem Routing

Wenn Sie sich vergewissern möchten, dass der Datenverkehr den erwarteten Pfad durchläuft, verwenden Sie am besten Traceroute. Soll an Microsoft gerichteter Datenverkehr von Ihrem lokalen SMTP-Server über den Internetpfad abgewickelt werden, führen Sie Traceroute zwischen SMTP-Server und Office 365 durch. Das Ergebnis gibt Aufschluss darüber, ob der Datenverkehr Ihr Netzwerk tatsächlich in Richtung des Internets (und nicht Richtung ExpressRoute) verlässt.

<!---HONumber=AcomDC_0824_2016-->