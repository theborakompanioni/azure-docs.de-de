---
title: "Übersicht über Azure IoT Hub | Microsoft Docs"
description: "Übersicht über den Azure IoT Hub-Dienst: Informationen zu IoT Hub, Gerätekonnektivität, Kommunikationsmuster für das Internet der Dinge, Gateways und dienstgestütztes Kommunikationsmuster"
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 7adde91586f5fbbffd0aeaf0efb0810cc891ac0b
ms.openlocfilehash: ed204c466c5cfb60e5ba250b9dacb2524ca384eb
ms.lasthandoff: 04/18/2017


---
# <a name="overview-of-the-azure-iot-hub-service"></a>Übersicht über den Azure IoT Hub-Dienst
Willkommen bei Azure IoT Hub. Dieser Artikel enthält eine Übersicht über Azure IoT Hub und erklärt, warum Sie diesen Dienst verwenden sollten, um eine IoT-Lösung (Internet of Things, Internet der Dinge) zu implementieren. Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Lösungs-Back-End ermöglicht. Azure IoT Hub:

* stellt zahlreiche D2C- und C2D-Kommunikationsoptionen (Gerät-an-Cloud und Cloud-an-Gerät) bereit, einschließlich unidirektionales Messaging, Dateiübertragungen und Anforderung-Antwort-Methoden.
* Bietet integriertes deklaratives Nachrichtenrouting zu anderen Azure-Diensten.
* Bietet einen abfragbaren Speicher für Gerätemetadaten und synchronisierte Zustandsinformationen.
* ermöglicht eine sichere Kommunikation und Zugriffssteuerung mithilfe von geräteabhängigen Sicherheitsschlüsseln oder X.509-Zertifikaten.
* Bietet eine umfassende Überwachung der Ereignisse zur Verwaltung der Gerätekonnektivität und -identität.
* Gerätebibliotheken für die gängigsten Sprachen und Plattformen

Im Artikel zum [Vergleich von IoT Hub und Event Hubs][lnk-compare] werden die wesentlichen Unterschiede zwischen diesen beiden Diensten beschrieben. Besonderer Fokus liegt dabei auf den Vorteilen der Verwendung von IoT Hub in Ihren IoT-Lösungen.

Weitere Informationen dazu, wie Azure und IoT Hub Ihre IoT-Lösung schützen können, finden Sie unter [Sicherheit im Internet der Dinge von Anfang an][lnk-security-ground-up].

![Azure IoT Hub als Cloudgateway in der IoT-Lösung][img-architecture]

> [!NOTE]
> Ausführliche Informationen zur IoT-Architektur finden Sie unter [Microsoft Azure IoT Reference Architecture (Architektur der Microsoft Azure IoT-Referenz)][lnk-refarch].
> 
> 

## <a name="iot-device-connectivity-challenges"></a>Anforderungen an die IoT-Gerätekonnektivität
Mit IoT Hub und den Gerätebibliotheken können Sie die Anforderungen erfüllen, die sich beim zuverlässigen und sicheren Verbinden von Geräten mit dem Back-End der Lösung ergeben. IoT-Geräte:

* Es handelt sich häufig um eingebettete Systeme ohne menschlichen Bediener.
* Sie können sich an Remotestandorten befinden, an denen physischer Zugriff teuer ist.
* Sie sind unter Umständen nur über das Back-End der Lösung erreichbar.
* Sie verfügen ggf. über eingeschränkte Stromversorgungs- und Verarbeitungsressourcen.
* Sie verfügen ggf. über intermittierende, langsame oder teure Netzwerkverbindungen.
* Sie müssen unter Umständen proprietäre, benutzerdefinierte oder branchenspezifische Anwendungsprotokolle verwenden.
* Sie können mit vielen gängigen Hardware- und Softwareplattformen erstellt werden.

Zusätzlich zu den obigen Anforderungen muss jede IoT-Lösung auch Skalierbarkeit, Sicherheit und Zuverlässigkeit bieten. Die sich ergebenden Verbindungsanforderungen sind schwierig und zeitaufwändig zu implementieren, wenn herkömmliche Technologie verwendet wird, z. B. Webcontainer und Nachrichtenbroker.

## <a name="why-use-azure-iot-hub"></a>Gründe für die Verwendung von Azure IoT Hub
Zusätzlich zu einer Bandbreite von [D2C][lnk-d2c-guidance]- und [C2D][lnk-c2d-guidance]-Kommunikationsoptionen, einschließlich Messaging, Dateiübertragungen und Anforderung-Antwort-Methoden geht Azure IoT Hub die Herausforderungen im Bereich Gerätekonnektivität auf folgende Weise an:

* **Gerätezwillinge**. Mithilfe von [Gerätezwillingen][lnk-twins] können Sie die Metadaten und Statusinformationen von Geräten speichern, synchronisieren und abfragen. Gerätezwillinge sind JSON-Dokumente, in denen Gerätestatusinformationen (Metadaten, Konfigurationen und Bedingungen) gespeichert werden. IoT Hub speichert einen Gerätezwilling für jedes Gerät, das Sie mit IoT Hub verbinden. 
* **Authentifizierung pro Gerät und sichere Verbindung:** Sie können für jedes Gerät einen eigenen [Sicherheitsschlüssel][lnk-devguide-security] bereitstellen, damit es eine Verbindung mit IoT Hub herstellen kann. Die [IoT Hub-Identitätsregistrierung][lnk-devguide-identityregistry] speichert Geräteidentitäten und -schlüssel in einer Lösung. Das Back-End einer Lösung kann einzelne Geräte einer Positiv- oder Negativliste hinzufügen und so die vollständige Kontrolle über den Gerätezugriff ermöglichen.
* **Weiterleitung von D2C-Nachrichten an Azure-Dienste auf der Grundlage deklarativer Regeln**. Mit IoT Hub können Sie Nachrichtenrouten auf der Grundlage von Routingregeln definieren und so steuern, wohin D2C-Nachrichten gesendet werden. Routingregeln können ganz ohne Programmieraufwand erstellt und anstelle von benutzerdefinierten Nachrichtenverteilern nach der Erfassung verwendet werden.
* **Überwachung von Gerätekonnektivitätsvorgängen:** Sie können detaillierte Betriebsprotokolle zu Ereignissen im Zusammenhang mit Verwaltungsvorgängen für Geräteidentität und -konnektivität empfangen. Dadurch kann Ihre IoT-Lösung Verbindungsprobleme erkennen – etwa in Form von Geräten, die versuchen, sich mit den falschen Anmeldeinformationen anzumelden, Nachrichten zu häufig senden oder alle C2D-Nachrichten zurückweisen.
* **Umfassender Satz von Gerätebibliotheken:** [Azure IoT-Geräte-SDKs][lnk-device-sdks] sind für die unterschiedlichsten Sprachen und Plattformen verfügbar und werden entsprechend unterstützt (C für viele Linux-Distributionen, Windows und Echtzeitbetriebssysteme). Azure IoT-Geräte-SDKs unterstützen auch verwaltete Sprachen wie C#, Java und JavaScript.
* **IoT-Protokolle und -Erweiterbarkeit:** Falls Ihre Lösung die Gerätebibliotheken nicht nutzen kann, macht IoT Hub ein öffentliches Protokoll verfügbar, mit dem Geräte die Protokolle MQTT 3.1.1, HTTP 1.1 und AMQP 1.0 nativ verwenden können. Folgendermaßen können Sie IoT Hub erweitern, um benutzerdefinierte Protokolle zu unterstützen:
  
  * Durch das Erstellen eines Bereichsgateways mit dem [Azure IoT-Gateway SDK][lnk-gateway-sdk], das Ihr benutzerdefiniertes Protokoll in eines der drei Protokolle umwandelt, die von IoT Hub gelesen werden können. 
  * durch Anpassen des [Azure IoT-Protokollgateways][protocol-gateway], einer Open-Source-Komponente, die in der Cloud ausgeführt wird.
* **Skalierung:**Bei einer Anwendung für Hunderttausende oder Millionen von Mandanten sind Ansätze mit gemeinsamer Datenbanknutzung vorteilhaft, z.B. Azure IoT Hub kann auf Millionen von gleichzeitig verbundenen Geräten und Millionen von Ereignissen pro Sekunde skaliert werden.

## <a name="gateways"></a>Gateways
Ein Gateway in einer IoT-Lösung ist i.d.R. entweder ein in der Cloud bereitgestelltes [Protokollgateway][lnk-gateway] oder ein [Bereichsgateway][lnk-field-gateway], das lokal mit Ihren Geräten bereitgestellt wird. Ein Protokollgateway führt die Protokollübersetzung, z. B. von MQTT in AMQP, durch. Ein Bereichsgateway kann Analysen am Rand durchführen, zeitkritische Entscheidungen treffen, die Latenzen reduzieren, Geräteverwaltungsdienste bereitstellen, zur Einhaltung von Sicherheits- und Datenschutzbestimmungen beitragen und die Protokollübersetzung durchführen. Beide Gatewaytypen fungieren als Vermittler zwischen Ihren Geräten und dem IoT Hub.

Ein Bereichsgateway unterscheidet sich von einem einfachen Gerät für das Routing von Datenverkehr (etwa einem Gerät für die Netzwerkadressenübersetzung oder einer Firewall), da es üblicherweise eine aktive Rolle bei der Verwaltung des Zugriffs und des Informationsflusses in der Lösung übernimmt.

Eine Lösung kann sowohl Protokoll- als auch Bereichsgateways enthalten.

## <a name="how-does-iot-hub-work"></a>Funktionsweise von IoT Hub
Azure IoT Hub implementiert das Muster für die [dienstgestützte Kommunikation][lnk-service-assisted-pattern], um die Interaktionen zwischen Ihren Geräten und dem Back-End der Lösung zu vermitteln. Das Ziel der dienstgestützten Kommunikation ist die Einrichtung von vertrauenswürdigen, bidirektionalen Kommunikationspfaden zwischen Steuersystemen, z. B. IoT Hub, und speziellen Geräten, die im nicht vertrauenswürdigen physischen Raum bereitgestellt werden. Für das Muster gelten die folgenden Grundsätze:

* Sicherheit hat Vorrang vor allen anderen Funktionen.
* Geräte akzeptieren keine unerwünschten Netzwerkinformationen. Ein Gerät stellt alle Verbindungen und Routen nur in ausgehender Richtung her. Damit ein Gerät einen Befehl vom Lösungs-Back-End erhalten kann, muss das Gerät regelmäßig eine Verbindung initiieren, um eine Prüfung auf ausstehende Befehle durchzuführen, die verarbeitet werden müssen.
* Geräte sollten nur Verbindungen mit bekannten Peerdiensten wie IoT Hub herstellen bzw. Routen dafür einrichten.
* Der Kommunikationspfad zwischen Gerät und Dienst oder Gerät und Gateway wird auf Anwendungsprotokollebene geschützt.
* Die Autorisierung und Authentifizierung auf Systemebene basiert auf gerätebezogenen Identitäten. So wird dafür gesorgt, dass Anmeldeinformationen und Berechtigungen nahezu sofort widerrufbar sind.
* Die bidirektionale Kommunikation für Geräte, die aufgrund von Stromversorgungs- oder Verbindungsaspekten nur selten eine Verbindung herstellen, wird wie folgt erreicht: Befehle und Gerätebenachrichtigungen werden beibehalten, bis ein Gerät eine Verbindung herstellt und diese Informationen empfängt. IoT Hub verfügt über spezielle Gerätewarteschlangen für die gesendeten Befehle.
* Die Nutzlastdaten der Anwendung werden für die geschützte Übertragung über Gateways an einen bestimmten Dienst separat geschützt.

Das dienstgestützte Kommunikationsmuster wurde in der Mobilgerätebranche in großem Umfang eingesetzt, um Pushbenachrichtigungsdienste wie [Windows-Pushbenachrichtigungsdienste (WNS)][lnk-wns], [Google Cloud Messaging][lnk-google-messaging] und [Apple Push Notification Service][lnk-apple-push] zu implementieren.

IoT Hub wird über den öffentlichen Peeringpfad von ExpressRoute unterstützt.

## <a name="next-steps"></a>Nächste Schritte
Unter [Senden und Empfangen von Nachrichten mit IoT Hub][lnk-send-messages] erfahren Sie, wie Sie Nachrichten von einem Gerät senden und Nachrichten von IoT Hub empfangen. Außerdem erfahren Sie dort, wie Sie Nachrichtenrouten konfigurieren.

Informationen dazu, wie IoT Hub die standardbasierte Geräteverwaltung ermöglicht, sodass Sie Ihre Geräte remote verwalten, konfigurieren und aktualisieren können, finden Sie unter [Übersicht über die Geräteverwaltung mit IoT Hub][lnk-device-management].

Mit den Azure IoT-Geräte SDKs können Sie Clientanwendungen auf einer Vielzahl von Gerätehardwareplattformen und Betriebssystemen implementieren. Die Geräte-SDKs enthalten Bibliotheken, die das Senden von Telemetriedaten an einen IoT-Hub und das Empfangen von C2D-Nachrichten ermöglichen. Wenn Sie die Geräte SDKs verwenden, können Sie für die Kommunikation mit IoT Hub aus verschiedenen Netzwerkprotokollen auswählen. Erfahren Sie mehr in den [Informationen zu Geräte SDKs (in englischer Sprache)][lnk-device-sdks].

Hilfe zu den ersten Schritten beim Schreiben von Code und dem Ausführen von Beispielen finden Sie im Tutorial [Erste Schritte mit IoT Hub][lnk-get-started].

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Dienstgestützte Kommunikation, Blogbeitrag von Clemens Vasters"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-send-messages]: iot-hub-devguide-messaging.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-security-ground-up]: iot-hub-security-ground-up.md

