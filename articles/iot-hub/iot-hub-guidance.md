<properties
 pageTitle="IoT Hub-Lösung – Anleitung | Microsoft Azure"
 description="Themen mit Anleitungen zu Gateways, zur Gerätebereitstellung und zur Authentifizierung für die Entwicklung von IoT-Lösungen mithilfe von Azure IoT Hub."
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="dobett"/>

# Entwerfen der Lösung

Dieser Artikel enthält Anleitungen zum Entwerfen der folgenden Funktionen in der IoT-Lösung (Internet of Things, Internet der Dinge):

- Gerätebereitstellung
- Bereichsgateways
- Geräte-Authentifizierung

## Gerätebereitstellung

IoT-Lösungen speichern Daten zu einzelnen Geräten wie z. B.:

- Geräteidentität und Authentifizierungsschlüssel
- Typ und Version der Gerätehardware
- Gerätestatus
- Softwareversionen und -funktionen
- Befehlsverlauf des Geräts

Die Gerätedaten, die von einer bestimmten IoT-Lösung gespeichert werden, richten sich nach den jeweiligen Anforderungen der Lösung. Von einer Lösung müssen aber mindestens die Geräteidentitäten und Authentifizierungsschlüssel gespeichert werden. Azure IoT Hub enthält eine [Identitätsregistrierung][lnk-devguide-identityregistry], die Werte für jedes Gerät speichern kann, z. B. IDs, Authentifizierungsschlüssel und Statuscodes. Eine Lösung kann andere Azure-Dienste wie Tabellen, Blobs oder Azure DocumentDB nutzen, um zusätzliche Gerätedaten zu speichern.

Die *Gerätebereitstellung* ist der Prozess des Hinzufügens der ersten Gerätedaten zu den Speichern in Ihrer Lösung. Damit ein neues Gerät eine Verbindung mit Ihrem Hub herstellen kann, müssen Sie der [IoT Hub-Identitätsregistrierung][lnk-devguide-identityregistry] eine neue Geräte-ID und Schlüssel hinzufügen. Im Rahmen des Bereitstellungsprozesses müssen Sie unter Umständen gerätespezifische Daten in anderen Lösungsspeichern initialisieren.

Mit den [IoT Hub-Identitätsregistrierungs-APIs][lnk-devguide-identityregistry] können Sie IoT Hub in Ihren Bereitstellungsprozess integrieren.

## Bereichsgateways

Bei einer IoT-Lösung ist zwischen Ihren Geräten und Ihrem IoT Hub ein *Bereichsgateway* angeordnet. Es befindet sich normalerweise in der Nähe Ihrer Geräte. Ihre Geräte kommunizieren direkt mit dem Bereichsgateway, indem sie ein von den Geräten unterstütztes Protokoll nutzen. Das Bereichsgateway kommuniziert mit IoT Hub über ein Protokoll, das von IoT Hub unterstützt wird. Bei einem Bereichsgateway kann es sich um hochspezialisierte Hardware oder einen Computer mit niedriger Leistung handeln, auf dem Software für das End-to-End-Szenario ausgeführt wird, für das das Gateway vorgesehen ist.

Ein Bereichsgateway unterscheidet sich von einem einfachen Gerät für das Routing von Datenverkehr (z. B. einem Gerät für die Netzwerkadressübersetzung (NAT) oder einer Firewall), da es üblicherweise eine aktive Rolle bei der Verwaltung des Zugriffs und des Informationsflusses in der Lösung hat. Beispielsweise kann ein Bereichsgateway folgende Aufgaben ausführen:

- **Hinzufügen von Unterstützung für neue und ältere Geräte**: Es gibt Millionen von neuen und älteren Sensoren und Auslösern, die Daten nicht direkt in die Cloud senden können. Diese Geräte verwenden entweder ein nicht internetfähiges Protokoll, implementieren keine Verschlüsselung oder können keine Identitätszertifikate speichern. Die Verwendung eines Gateways verringert den Aufwand und die Kosten für die Verbindung dieser Geräte.
- **Ausführen von Edgeanalysen**: Es gibt viele Vorgänge, die lokal ausgeführt werden können, um die Menge an Daten zu reduzieren, die mit der Cloud ausgetauscht werden. Hierzu gehören beispielsweise die Filterung, Batchverarbeitung und Komprimierung von Daten. Es kann auch wünschenswert sein, einige Berechnungen wie etwa Datenbereinigung oder die Bewertung eines Machine Learning-Modells mit Echtzeitdaten lokal auszuführen.
- **Minimieren der Latenz**: Wenn Sie versuchen, das Herunterfahren von Produktionslinien zu verhindern oder die Stromversorgung wiederherzustellen, geht es um Millisekunden. Die Fähigkeit, Daten in der Nähe des Geräts zu analysieren, das die Daten erfasst, kann den Unterschied zwischen dem Verhindern einer Katastrophe und einem kaskadierenden Systemfehler bedeuten.
- **Einsparen von Netzwerkbandbreite**: Eine Bohrinsel generiert typischerweise zwischen 1 und 2 TB Daten pro Tag. In einer Boeing 787 entstehen pro Flug etwa 500 GB an Daten. Es ist nicht sinnvoll, große Datenmengen aus Tausenden oder sogar Hunderttausenden von Edgegeräten in die Cloud zu transportieren. Es ist auch nicht notwendig, denn viele kritische Analysen erfordern keine Verarbeitung und Speicherung im Cloudmaßstab.
- **Zuverlässiger Betrieb**: IoT-Daten werden zunehmend für Entscheidungen verwendet, die die Sicherheit der Bevölkerung und kritische Infrastrukturen betreffen. Integrität und Verfügbarkeit von Infrastruktur und Daten dürfen durch unterbrechungsanfällige Cloudverbindungen nicht beeinträchtigt werden. Mithilfe von Funktionen wie „Speichern und Weiterleiten“, um Daten lokal zu erfassen und zu verarbeiten und anschließend ggf. an die Cloud zu senden, können Sie zuverlässige Lösungen erstellen.
- **Beheben von Problemen mit Datenschutz und Sicherheit**: IoT-Geräte und die von ihnen generierten Daten müssen geschützt werden. Gateways erfüllen verschiedene Funktionen: Sie können Geräte vom offenen Internet isolieren und Verschlüsselungs- und Identitätsdienste für Geräte bieten, die diese Dienste nicht selbst bereitstellen können. Gateways können auch lokal gepufferte oder gespeicherte Daten sichern und personenbezogene Informationen entfernen, bevor die Daten über das Internet gesendet werden.

### Weitere Überlegungen

Sie können das [Azure IoT Gateway SDK][lnk-gateway-sdk] verwenden, um ein Bereichsgateway zu implementieren. Dieses SDK bietet bestimmte Funktionen, z.B. die Möglichkeit, die Kommunikation von mehreren Geräten über eine Verbindung mit IoT Hub im Multiplexverfahren zu übertragen.

## Benutzerdefinierte Geräteauthentifizierung

Mit der [Geräteidentitätsregistrierung][lnk-devguide-identityregistry] von IoT Hub können Sie Sicherheitsanmeldeinformationen und die Zugriffssteuerung pro Gerät konfigurieren. Auch wenn eine IoT-Lösung bereits erheblichen Anteil an einer benutzerdefinierten Geräteidentitätsregistrierung bzw. einem Authentifizierungsschema hat, können Sie diese vorhandene Infrastruktur in IoT Hub integrieren, indem ein *Tokendienst* erstellt wird. Auf diese Weise können Sie andere IoT-Features in der Lösung nutzen.

Ein Tokendienst ist ein benutzerdefinierter Clouddienst. Er nutzt eine *SAS-Richtlinie* von IoT Hub mit **DeviceConnect**-Berechtigungen, um Token mit *Gerätebereich* zu erstellen. Mit diesen Token kann ein Gerät eine Verbindung mit Ihrem IoT Hub herstellen.

  ![Schritte des Tokendienstmusters][img-tokenservice]

Dies sind die wichtigsten Schritte des Tokendienstmusters:

1. Erstellen Sie eine [IoT Hub-SAS-Richtlinie][lnk-devguide-security] mit **DeviceConnect**-Berechtigungen für IoT Hub. Sie können diese Richtlinie im [Azure-Portal][lnk-portal] oder programmgesteuert erstellen. Diese Richtlinie wird vom Tokendienst zum Signieren der von ihm erstellten Token verwendet.
2. Wenn ein Gerät auf IoT Hub zugreifen muss, fordert es von Ihrem Tokendienst ein signiertes Token an. Das Gerät kann mit Ihrer benutzerdefinierten Geräteidentitätsregistrierung bzw. mit dem Authentifizierungsschema authentifiziert werden, um die Geräteidentität zu ermitteln, die der Tokendienst zum Erstellen des Tokens verwendet.
3. Der Tokendienst gibt ein Token zurück. Das Token wird gemäß [Abschnitt über Sicherheit im IoT Hub-Entwicklerleitfaden][lnk-devguide-security] erstellt, indem `/devices/{deviceId}` als `resourceURI` verwendet wird. `deviceId` ist hierbei das zu authentifizierende Gerät. Der Tokendienst verwendet die SAS-Richtlinie, um das Token zu erstellen.
4. Das Gerät nutzt das Token direkt mit IoT Hub.

> [AZURE.NOTE] Sie können die .NET-Klasse [SharedAccessSignatureBuilder][lnk-dotnet-sas] oder die Java-Klasse [IotHubServiceSasToken][lnk-java-sas] zum Erstellen eines Tokens im Tokendienst verwenden.

Der Tokendienst kann die Gültigkeitsdauer für das Token wie gewünscht festlegen. Wenn das Token abläuft, trennt IoT Hub die Geräteverbindung. Das Gerät muss dann ein neues Token vom Tokendienst anfordern. Wenn Sie eine kurze Ablaufzeit verwenden, erhöht sich die Last für das Gerät und den Tokendienst gleichermaßen.

Damit ein Gerät eine Verbindung mit Ihrem Hub herstellen kann, müssen Sie es der IoT Hub-Geräteidentitätsregistrierung hinzufügen – auch wenn das Gerät für die Verbindung ein Token und keinen Geräteschlüssel verwendet. Aus diesem Grund können Sie weiterhin die Zugriffssteuerung pro Gerät nutzen, indem Sie Geräteidentitäten in der [IoT Hub-Identitätsregistrierung][lnk-devguide-identityregistry] aktivieren oder deaktivieren, wenn sich das Gerät mit einem Token authentifiziert. Dies verringert die Risiken der Verwendung von Token mit langen Ablaufzeiten.

### Vergleich mit einem benutzerdefinierten Gateway

Das Tokendienstmuster ist der empfohlene Weg zur Implementierung einer benutzerdefinierten Identitätsregistrierung bzw. eines Authentifizierungsschemas mit IoT Hub. Dies wird empfohlen, da der größte Teil des Lösungsdatenverkehrs weiterhin über IoT Hub abgewickelt wird. Es gibt aber auch Fälle, in denen das benutzerdefinierte Authentifizierungsschema so eng mit dem Protokoll verknüpft ist, dass ein Dienst zum Verarbeiten des gesamten Datenverkehrs (*benutzerdefiniertes Gateway*) erforderlich ist. Beispiele hierfür sind [Transport Layer Security (TLS) und vorinstallierte Schlüssel (PSKs)][lnk-tls-psk]. Weitere Informationen finden Sie im Thema [Protokollgateway][lnk-gateway].

## Gerätetakt <a id="heartbeat"></a>

Die [IoT Hub-Identitätsregistrierung][lnk-devguide-identityregistry] enthält das Feld **connectionState**. Sie sollten das Feld **connectionState** nur während der Entwicklung und des Debuggens verwenden. IoT-Lösungen sollten das Feld zur Laufzeit nicht abfragen (um beispielsweise eine Geräteverbindung zu prüfen und dann zu bestimmen, ob eine C2D-Nachricht oder SMS gesendet werden soll). Wenn Ihre IoT-Lösung wissen muss, ob ein Gerät verbunden ist (entweder zur Laufzeit oder mit höherer Genauigkeit als von der **connectionState**-Eigenschaft bereitgestellt), sollte Ihre Lösung das *Taktmuster* implementieren.

Beim Taktmuster sendet das Gerät D2C-Nachrichten mindestens einmal pro festgelegtem Zeitraum (z. B. mindestens einmal pro Stunde). Dies bedeutet, dass selbst wenn ein Gerät keine zu sendenden Daten hat, es dennoch eine leere D2C-Nachricht sendet (in der Regel mit einer Eigenschaft, die sie als Takt identifiziert). Auf Dienstseite verwaltet die Lösung eine Zuordnung mit dem letzten für jedes Gerät empfangenen Takts und nimmt an, dass es ein Problem mit einem Gerät gibt, wenn es innerhalb des erwarteten Zeitraums keine Taktnachricht empfängt.

Eine komplexere Implementierung kann die Informationen aus der [Vorgangsüberwachung][lnk-devguide-opmon] enthalten, um Geräte zu bestimmen, die erfolglos versuchen, eine Verbindung herzustellen oder zu kommunizieren. Wenn Sie das Taktmuster implementieren, sollten Sie [IoT Hub-Kontingente und -Drosselungen][] überprüfen.

> [AZURE.NOTE] Wenn für eine IoT-Lösung der Geräteverbindungsstatus ausschließlich dazu benötigt wird, um zu bestimmen, ob C2D-Nachrichten gesendet werden müssen, und wenn Nachrichten nicht an große Gruppen von Geräten übertragen werden, sollten Sie in Betracht ziehen, eine kurze Ablaufzeit zu verwenden. Dadurch wird dasselbe, aber deutlich effizientere Ergebnis erzielt, als wenn die Registrierung des Geräteverbindungsstatus mit dem Taktmuster aufrechterhalten wird. Es ist auch durch die Anforderung von Nachrichtenbestätigungen möglich, vom IoT Hub darüber benachrichtigt zu werden, von welchen Geräten Nachrichten empfangen werden können und welche nicht online oder ausgefallen sind. Weitere Informationen zu C2D-Nachrichten finden Sie im [Entwicklungsleitfaden für IoT Hub][lnk-devguide-messaging].

## Nächste Schritte

Weitere Informationen zum Planen Ihrer IoT Hub-Bereitstellung finden Sie unter:

- [MQTT-Unterstützung][lnk-mqtt]
- [Unterstützte Geräte][lnk-devices]
- [Unterstützen zusätzlicher Protokolle][lnk-protocols]
- [Vergleich mit Event Hubs][lnk-compare]
- [Skalierung, hohe Verfügbarkeit und Notfallwiederherstellung][lnk-scaling]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

- [Entwicklerhandbuch][lnk-devguide]
- [Erkunden der Geräteverwaltung mithilfe der Beispielbenutzeroberfläche][lnk-dmui]
- [Simulieren eines Geräts mit dem Gateway SDK][lnk-gateway]
- [Verwenden des Azure-Portals zur Verwaltung von IoT Hub][lnk-portal-manage]

[img-tokenservice]: ./media/iot-hub-guidance/tokenservice.png

[lnk-devguide-identityregistry]: iot-hub-devguide.md#identityregistry
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-device-sdks]: iot-hub-sdks-summary.md
[lnk-devguide-security]: iot-hub-devguide.md#security
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-gateway]: iot-hub-protocol-gateway.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-what-is-hub]: iot-hub-what-is-iot-hub.md
[lnk-portal]: https://portal.azure.com
[lnk-throttles-quotas]: ../azure-subscription-service-limits.md/#iot-hub-limits
[lnk-devguide-antispoofing]: iot-hub-devguide.md#antispoofing
[lnk-devguide-protocol]: iot-hub-devguide.md#amqpvshttp
[lnk-devguide-messaging]: iot-hub-devguide.md#messaging
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/com/microsoft/azure/iot/service/auth/IotHubServiceSasToken.html
[IoT Hub-Kontingente und -Drosselungen]: iot-hub-devguide.md#throttling
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[lnk-mqtt]: iot-hub-mqtt-support.md
[lnk-devices]: iot-hub-tested-configurations.md
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-portal-manage]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0713_2016-->