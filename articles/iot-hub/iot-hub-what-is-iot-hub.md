<properties
 pageTitle="Übersicht über Azure IoT Hub | Microsoft Azure"
 description="Übersicht über den Azure IoT Hub-Dienst: Informationen zum IoT-Hub, Gerätekonnektivität, Kommunikationsmuster für das Internet der Dinge und dienstgestütztes Kommunikationsmuster."
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/25/2016"
 ms.author="dobett"/>

# Was ist Azure IoT Hub?

Willkommen bei Azure IoT Hub. In diesem Artikel erhalten Sie eine Übersicht über Azure IoT Hub. Es wird beschrieben, warum die Verwendung dieses Diensts empfehlenswert ist, wenn Sie eine IoT-Lösung (Internet of Things, Internet der Dinge) implementieren.

Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Lösungs-Back-End ermöglicht. Azure IoT Hub:

- Ermöglicht ein zuverlässiges Messaging zwischen Geräten und Cloud im großen Maßstab
- Eine sichere Kommunikation unter Verwendung von Zugriffssteuerung und Sicherheitsanmeldeinformationen auf Gerätebasis
- Bietet eine umfassende Überwachung der Ereignisse zur Verwaltung der Gerätekonnektivität und -identität.
- Gerätebibliotheken für die gängigsten Sprachen und Plattformen

Im Artikel zum [Vergleich von IoT Hub und Event Hubs][lnk-compare] werden die wesentlichen Unterschiede zwischen diesen beiden Diensten beschrieben. Besonderer Fokus liegt dabei auf den Vorteilen der Verwendung von IoT Hub in IoT-Lösungen.

![Azure IoT Hub als Cloudgateway in der IoT-Lösung][img-architecture]

> [AZURE.NOTE] Ausführliche Informationen zur IoT-Architektur finden Sie unter [Microsoft Azure IoT Reference Architecture][lnk-refarch] \(Microsoft Azure IoT-Referenzarchitektur).

## Anforderungen an die IoT-Gerätekonnektivität

Mit IoT Hub und den Gerätebibliotheken können Sie die Anforderungen erfüllen, die sich beim zuverlässigen und sicheren Verbinden von Geräten mit dem Back-End der Lösung ergeben. IoT-Geräte:

- Es handelt sich häufig um eingebettete Systeme ohne menschlichen Bediener.
- Sie können sich an Remotestandorten befinden, an denen physischer Zugriff teuer ist.
- Sie sind unter Umständen nur über das Back-End der Lösung erreichbar.
- Sie verfügen ggf. über eingeschränkte Stromversorgungs- und Verarbeitungsressourcen.
- Sie verfügen ggf. über intermittierende, langsame oder teure Netzwerkverbindungen.
- Sie müssen unter Umständen proprietäre, benutzerdefinierte oder branchenspezifische Anwendungsprotokolle verwenden.
- Sie können mit vielen gängigen Hardware- und Softwareplattformen erstellt werden.

Zusätzlich zu den obigen Anforderungen muss jede IoT-Lösung auch Skalierbarkeit, Sicherheit und Zuverlässigkeit bieten. Die sich ergebenden Verbindungsanforderungen sind schwierig und zeitaufwändig zu implementieren, wenn herkömmliche Technologie verwendet wird, z. B. Webcontainer und Nachrichtenbroker.

## Gründe für die Verwendung von Azure IoT Hub

Mit Azure IoT Hub werden die Verbindungsanforderungen für Geräte wie folgt erfüllt:

-   **Authentifizierung pro Gerät und sichere Verbindung:** Sie können jedes Gerät mit seinem eigenen [Sicherheitsschlüssel][lnk-devguide-security] bereitstellen, damit es eine Verbindung mit IoT Hub herstellen kann. Die [IoT Hub-Identitätsregistrierung][lnk-devguide-identityregistry] speichert Geräteidentitäten und Schlüssel in einer Lösung. Das Back-End einer Lösung kann einzelne Geräte einer Positiv- oder Negativliste hinzufügen und so die vollständige Kontrolle über den Gerätezugriff ermöglichen.

-   **Überwachung von Gerätekonnektivitätsvorgängen**. Sie können detaillierte Betriebsprotokolle zu Ereignissen im Zusammenhang mit Verwaltungsvorgängen für Geräteidentität und -konnektivität empfangen. Dadurch kann Ihre IoT-Lösung Verbindungsprobleme erkennen – etwa in Form von Geräten, die versuchen, sich mit den falschen Anmeldeinformationen anzumelden, Nachrichten zu häufig senden oder alle C2D-Nachrichten zurückweisen.

-   **Umfassender Satz von Gerätebibliotheken:** [Azure IoT-Geräte-SDKs][lnk-device-sdks] sind für die unterschiedlichsten Sprachen und Plattformen verfügbar und werden entsprechend unterstützt (C für viele Linux-Distributionen, Windows und Echtzeitbetriebssysteme). Azure IoT-Geräte-SDKs unterstützen auch verwaltete Sprachen wie C#, Java und JavaScript.

-   **IoT-Protokolle und -Erweiterbarkeit:** Falls Ihre Lösung die Gerätebibliotheken nicht nutzen kann, macht IoT Hub ein öffentliches Protokoll verfügbar, mit dem Geräte die Protokolle MQTT 3.1.1, HTTP 1.1 und AMQP 1.0 nativ verwenden können. Folgendermaßen können Sie IoT Hub erweitern, um benutzerdefinierte Protokolle zu unterstützen:

    - Erstellen eines Bereichsgateways mit dem [Azure IoT Gateway SDK][lnk-gateway-sdk], das Ihr benutzerdefiniertes Protokoll in eines der drei Protokolle umwandelt, die von IoT Hub gelesen werden können.
    - Anpassen des [Azure IoT-Protokollgateways][protocol-gateway], einer Open-Source-Komponente, die in der Cloud ausgeführt wird.

-   **Skalierung:** Azure IoT Hub kann auf Millionen von gleichzeitig verbundenen Geräten und Millionen von Ereignissen pro Sekunde skaliert werden.

Diese Vorteile gelten für viele Kommunikationsmuster. Mit IoT Hub haben Sie derzeit die Möglichkeit, die folgenden speziellen Kommunikationsmuster zu implementieren:

-   **Ereignisbasierte Device-to-Cloud (D2C)-Erfassung:** IoT Hub kann pro Sekunde zuverlässig Millionen von Ereignissen von Ihren Geräten empfangen. Anschließend können diese mit einem Ereignisverarbeitungsmodul unter Ihrem Hot Path verarbeitet werden. Zu Analysezwecken können sie auch unter Ihrem Cold Path gespeichert werden. IoT Hub behält die Ereignisdaten bis zu sieben Tage bei, um eine zuverlässige Verarbeitung zu garantieren und Auslastungsspitzen auszugleichen.

-   **Zuverlässiges Cloud-to-Device (C2D)-Messaging (bzw. *Befehle*):** Das Back-End der Lösung kann IoT Hub zum Senden von Nachrichten mit einer garantierten einmaligen Zustellung an einzelne Dienste verwenden. Jede Nachricht verfügt über eine individuelle Einstellung für die Lebensdauer, und das Back-End kann sowohl Zustellungs- als auch Ablaufnachweise anfordern. Durch diese Nachweise wird die vollständige Transparenz des Lebenszyklus einer C2D-Nachricht sichergestellt. Anschließend können Sie eine Geschäftslogik mit Vorgängen implementieren, die auf Geräten ausgeführt werden.

-   **Hochladen von Dateien und zwischengespeicherten Sensordaten in die Cloud:** Ihre Geräte können Dateien mit von IoT Hub verwalteten SAS-URIs in Azure Storage hochladen. IoT Hub kann Benachrichtigungen generieren, wenn Dateien in die Cloud übertragen wurden, um die Verarbeitung durch das Back-End zu ermöglichen.

## Gateways

Ein Gateway in einer IoT-Lösung ist i. d. R. entweder ein in der Cloud bereitgestelltes [Protokollgateway][lnk-gateway] oder ein [Bereichsgateway][lnk-field-gateway], das lokal mit Ihren Geräten bereitgestellt wird. Ein Protokollgateway führt die Protokollübersetzung, z. B. von MQTT in AMQP, durch. Ein Bereichsgateway kann Analysen am Rand durchführen, zeitkritische Entscheidungen treffen, die Latenzen reduzieren, Geräteverwaltungsdienste bereitstellen, zur Einhaltung von Sicherheits- und Datenschutzbestimmungen beitragen und die Protokollübersetzung durchführen. Beide Gatewaytypen fungieren als Vermittler zwischen Ihren Geräten und dem IoT Hub.

Ein Bereichsgateway unterscheidet sich von einem einfachen Gerät für das Routing von Datenverkehr (etwa einem Gerät für die Netzwerkadressenübersetzung oder einer Firewall), da es üblicherweise eine aktive Rolle bei der Verwaltung des Zugriffs und des Informationsflusses in der Lösung übernimmt.

Eine Lösung kann sowohl Protokoll- als auch Bereichsgateways enthalten.

## Funktionsweise von IoT Hub

Azure IoT Hub implementiert das Muster für die [dienstgestützte Kommunikation][lnk-service-assisted-pattern], um die Interaktionen zwischen Ihren Geräten und dem Back-End der Lösung zu vermitteln. Das Ziel der dienstgestützten Kommunikation ist die Einrichtung von vertrauenswürdigen, bidirektionalen Kommunikationspfaden zwischen Steuersystemen, z. B. IoT Hub, und speziellen Geräten, die im nicht vertrauenswürdigen physischen Raum bereitgestellt werden. Für das Muster gelten die folgenden Grundsätze:

- Sicherheit hat Vorrang vor allen anderen Funktionen.
- Geräte akzeptieren keine unerwünschten Netzwerkinformationen. Ein Gerät stellt alle Verbindungen und Routen nur in ausgehender Richtung her. Damit ein Gerät einen Befehl vom Back-End erhalten kann, muss das Gerät regelmäßig eine Verbindung initiieren, um eine Prüfung auf ausstehende Befehle durchzuführen, die verarbeitet werden müssen.
- Geräte sollten nur Verbindungen mit bekannten Peerdiensten wie IoT Hub herstellen bzw. Routen dafür einrichten.
- Der Kommunikationspfad zwischen Gerät und Dienst oder Gerät und Gateway wird auf Anwendungsprotokollebene geschützt.
- Die Autorisierung und Authentifizierung auf Systemebene basiert auf gerätebezogenen Identitäten. So wird dafür gesorgt, dass Anmeldeinformationen und Berechtigungen nahezu sofort widerrufbar sind.
- Die bidirektionale Kommunikation für Geräte, die aufgrund von Stromversorgungs- oder Verbindungsaspekten nur selten eine Verbindung herstellen, wird wie folgt erreicht: Befehle und Gerätebenachrichtigungen werden beibehalten, bis ein Gerät eine Verbindung herstellt und diese Informationen empfängt. IoT Hub verfügt über spezielle Gerätewarteschlangen für die gesendeten Befehle.
- Die Nutzlastdaten der Anwendung werden für die geschützte Übertragung über Gateways an einen bestimmten Dienst separat geschützt.

Das dienstgestützte Kommunikationsmuster wurde in der Mobilgerätebranche in großem Umfang eingesetzt, um Pushbenachrichtigungsdienste wie [Windows-Pushbenachrichtigungsdienste (WNS)][lnk-wns], [Google Cloud Messaging][lnk-google-messaging] und [Apple Push Notification Service][lnk-apple-push] zu implementieren.

## Nächste Schritte

Informationen dazu, wie Azure IoT Hub die standardbasierte IoT-Geräteverwaltung ermöglicht, sodass Sie Ihre Geräte remote verwalten, konfigurieren und aktualisieren können, finden Sie unter [Überblick über die Azure IoT Hub-Geräteverwaltung][lnk-device-management].

Mit den IoT-Geräte-SDKs können Sie Clientanwendungen auf einer Vielzahl von Gerätehardwareplattformen und Betriebssystemen implementieren. Die IoT-Geräte-SDKs enthalten Bibliotheken, die das Senden von Telemetriedaten an einen IoT Hub und das Empfangen von Cloud-zu-Gerät-Befehlen ermöglichen. Wenn Sie die SDKs verwenden, können Sie für die Kommunikation mit IoT Hub unter verschiedenen Netzwerkprotokollen wählen. Erfahren Sie mehr in den [Informationen zu Geräte-SDKs][lnk-device-sdks].

Informationen zu den ersten Schritten zum Schreiben von Code und Ausführen von Beispielen finden Sie im Tutorial [Erste Schritte mit IoT Hub][lnk-get-started].

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-d2c]: iot-hub-csharp-csharp-process-d2c.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Dienstgestützte Kommunikation, Blogbeitrag von Clemens Vasters"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-guidance.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide.md#identityregistry
[lnk-devguide-security]: iot-hub-devguide.md#security
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md

<!---HONumber=AcomDC_0831_2016-->