<properties
 pageTitle="Aktivieren verwalteter Geräte hinter einem IoT-Gateway | Microsoft Azure"
 description="Leitfadenthema unter Verwendung eines IoT-Gateways, das mit dem Gateway-SDK erstellt wurde, zusammen mit Geräten, die von IoT Hub verwaltet werden."
 services="iot-hub"
 documentationCenter=""
 authors="chipalost"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="cstreet"/>
 
# Aktivieren verwalteter Geräte hinter einem IoT-Gateway

## Grundlegende Geräteisolierung

Organisationen verwenden häufig IoT-Gateways, um die allgemeine Sicherheit ihrer IoT-Lösungen zu erhöhen. Einige Geräte müssen Daten in die Cloud senden, sind jedoch nicht in der Lage, sich selbst gegen Bedrohungen aus dem Internet zu schützen. Sie können diese Geräte gegen externe Bedrohungen abschirmen, indem sie die Kommunikation mit der Außenwelt über ein Gateway abwickeln müssen.

Das Gateway befindet sich an der Grenze zwischen einer sicheren Umgebung und dem offenen Internet. Die Geräte kommunizieren mit dem Gateway, und das Gateway übergibt die Nachrichten an das richtige Ziel in der Cloud. Das Gateway ist gegen externe Bedrohungen verstärkt, verhindert unautorisierte Anforderungen, ermöglicht autorisierten eingehenden Datenverkehr und leitet diesen eingehenden Datenverkehr zu dem richtigen Gerät.

![][1]

Um eine zusätzliche Sicherheitsebene zu haben, können Sie auch Geräte einsetzen, die sich selbst hinter einem Gateway schützen können. In diesem Szenario müssen Sie nur darauf achten, dass das Gateway-OS stets gegen die neuesten Sicherheitslücken gepatcht ist, statt das Betriebssystem auf jedem Gerät zu aktualisieren.

## Isolierung plus Intelligenz

Ein gesicherter Router ist ein ausreichendes Gateway zum einfachen Isolieren von Geräten. IoT-Lösungen erfordern jedoch häufig, dass ein Gateway mehr Intelligenz bietet, als einfach nur Geräte zu isolieren. Beispielsweise möchten Sie vielleicht Ihre Geräte aus der Cloud verwalten. Sie können LWM2M, ein standardmäßiges Protokoll zur Geräteverwaltung für den Cloudverwaltungsteil der Lösung verwenden. Die Geräte senden jedoch mithilfe eines nicht TCP/IP-fähigen Protokolls Telemetriedaten. Darüber hinaus erzeugen die Geräte große Datenmengen, und Sie möchten nur eine gefilterte Teilmenge der Telemetriedaten hochladen. Sie können eine Lösung erstellen, die ein IoT-Gateway beinhaltet, das zwei unterschiedliche Datenströme verwalten kann. Das Gateway sollte:

-   die **Telemetriedaten** erkennen, filtern und dann über das Gateway in die Cloud hochladen. Das Gateway ist nicht mehr nur ein einfacher Router, der einfach Daten zwischen Gerät und Cloud überträgt.

-   Tauschen Sie einfach die **LWM2M-Geräteverwaltungsdaten** zwischen den Geräten und der Cloud aus. Das Gateway muss die eingehenden Daten nicht erkennen, und nur sicherstellen, dass die Daten zwischen den Geräten und der Cloud hin und her übertragen werden.

Die folgende Abbildung veranschaulicht dieses Szenario:

![][2]

## Die Lösung: Azure IoT-Geräteverwaltung und das Gateway-SDK 

Die öffentliche Vorschauversion der [Azure IoT-Geräteverwaltung][lnk-device-management] und die Betaversion des [Azure IoT-Gateway-SDK] ermöglichen dieses Szenario. Das Gateway behandelt jeden Datenstrom wie folgt:

-   **Telemetriedaten**: Sie können mit dem Gateway-SDK eine Pipeline erstellen, die Telemetriedaten erkennt, filtert und in die Cloud sendet. Das Gateway-SDK liefert Code, der Teile dieser Pipeline für den Entwickler implementiert. Weitere Informationen zur Architektur des SDK finden Sie im Tutorial [IoT Gateway SDK (beta) - Get started using Linux][lnk-gateway-get-started] \(IoT-Gateway-SDK [Beta] – erste Schritte mit Linux).

-   **Geräteverwaltung**: Die Azure-Geräteverwaltung stellt einen LWM2M-Client bereit, der auf dem Gerät ausgeführt wird, sowie eine Cloudschnittstelle zur Ausgabe von Verwaltungsbefehlen an das Gerät.
    
    Auf dem Gateway ist keine spezielle Logik erforderlich, da es die LWM2M-Daten, die zwischen dem Gerät und Ihrem IoT Hub ausgetauscht werden, nicht verarbeiten muss. Sie können ICS – ein Feature vieler moderner Betriebssysteme – auf dem Gateway aktivieren, um den Austausch von LWM2M-Daten zu ermöglichen. Sie können ein geeignetes Betriebssystem für dieses Szenario auswählen, da das Gateway-SDK zahlreiche Betriebssysteme unterstützt. Hier finden Sie Anweisungen zum Aktivieren von ICS unter [Windows 10] und [Ubuntu], zwei der vielen unterstützten Betriebssysteme.

Die folgende Abbildung zeigt die allgemeine Architektur zum Aktivieren dieses Szenarios mit [Azure IoT-Geräteverwaltung][lnk-device-management] und [Azure IoT-Gateway-SDK].

![][3]

## Nächste Schritte

Weitere Informationen zur Verwendung des Gateway-SDK finden Sie in folgenden Tutorials:

- [IoT Gateway SDK (beta) - Get started using Linux][lnk-gateway-get-started] \(IoT-Gateway-SDK [Beta] – erste Schritte mit Linux).
- [IoT Gateway SDK – Senden von D2C-Nachrichten mit einem simulierten Gerät unter Linux][lnk-gateway-simulated].

Informationen zur Geräteverwaltung mit IoT Hub finden Sie unter [Einführung in die Azure IoT Hub-Geräteverwaltungsbibliothek][lnk-library-c].

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

- [Entwerfen Ihrer Lösung][lnk-design]
- [Entwicklerhandbuch][lnk-devguide]
- [Simulieren eines Geräts mit dem Gateway SDK][lnk-gateway]
- [Verwenden des Azure-Portals zur Verwaltung von IoT Hub][lnk-portal]

<!-- Images and links -->
[1]: media/iot-hub-gateway-device-management/overview.png
[2]: media/iot-hub-gateway-device-management/manage.png
[Azure IoT-Gateway-SDK]: https://github.com/Azure/azure-iot-gateway-sdk/
[Windows 10]: http://windows.microsoft.com/de-DE/windows/using-internet-connection-sharing#1TC=windows-7
[Ubuntu]: https://help.ubuntu.com/community/Internet/ConnectionSharing
[3]: media/iot-hub-gateway-device-management/manage_2.png
[lnk-gateway-get-started]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-gateway-simulated]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-tutorial-twin]: iot-hub-device-management-device-twin.md
[lnk-tutorial-queries]: iot-hub-device-management-device-query.md
[lnk-tutorial-jobs]: iot-hub-device-management-device-jobs.md
[lnk-dm-gateway]: iot-hub-gateway-device-management.md
[lnk-library-c]: iot-hub-device-management-library.md

[lnk-design]: iot-hub-guidance.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-portal]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0713_2016-->