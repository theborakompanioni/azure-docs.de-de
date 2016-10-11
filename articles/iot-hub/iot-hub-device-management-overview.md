<properties
 pageTitle="Überblick zur Geräteverwaltung | Microsoft Azure"
 description="Überblick über die Azure IoT Hub-Geräteverwaltung"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/16/2016"
 ms.author="bzurcher"/>



# Überblick über die Azure IoT Hub-Geräteverwaltung (Vorschau)

## Azure IoT Hub-Geräteverwaltung

Bei der Azure IoT Hub-Geräteverwaltung werden die Features und das Erweiterbarkeitsmodell für Geräte und Back-Ends bereitgestellt, um die IoT-Geräteverwaltung für die vielen unterschiedlichen Geräte und Protokolle im IoT-Bereich nutzen zu können. Die Geräte reichen bei IoT von einfachen Sensoren und zweckgebundenen Microcontrollern bis zu leistungsfähigeren Gateways, mit denen die Verwendung von anderen Geräten und Protokollen ermöglicht wird. IoT-Lösungen unterscheiden sich auch erheblich in Bezug auf vertikale Domänen und Anwendungen mit einzigartigen Anwendungsfällen für die Bediener in den unterschiedlichen Domänen. Für IoT-Lösungen können Funktionen, Muster und Codebibliotheken der IoT Hub-Geräteverwaltung genutzt werden, um eine Geräteverwaltung für die verschiedenen Geräte und Benutzer zu ermöglichen.

## Einführung

Ein entscheidender Punkt bei der Erstellung einer erfolgreichen IoT-Lösung ist die Festlegung einer Strategie, mit der Bediener die fortlaufende Verwaltung ihrer Geräteflotte bewältigen können. IoT-Bediener benötigen Tools und Anwendungen, die gleichzeitig einfach und zuverlässig sind und ihnen die Konzentration auf die strategischen Aspekte ihrer Arbeit ermöglichen. Mit Azure IoT Hub verfügen Sie über die Bausteine zum Erstellen von IoT-Anwendungen, mit denen die wichtigsten Muster der Geräteverwaltung abgedeckt werden können.

Geräte verfügen über eine IoT Hub-Verwaltung, wenn für sie eine einfache Anwendung (der Geräteverwaltungs-Agent) ausgeführt wird, um für das Gerät eine sichere Cloudverbindung herzustellen. Mit dem Agent-Code kann ein Bediener auf Anwendungsseite den Gerätestatus per Remotezugriff ermitteln und Verwaltungsvorgänge durchführen, z.B. das Anwenden von Änderungen der Netzwerkkonfiguration oder das Bereitstellen von Firmwareupdates.

## Prinzipien der IoT-Geräteverwaltung

IoT ist mit besonderen Verwaltungsanforderungen verbunden, und mit einer Lösung müssen die folgenden Prinzipien der IoT-Geräteverwaltung erfüllt werden:

![][img-dm_principles]

- **Skalierung und Automation**: Für IoT sind einfache Tools erforderlich, mit denen Routineaufgaben automatisiert werden können und eine relativ kleine Gruppe von Bedienern Millionen von Geräten verwalten kann. Bediener erwarten Tag für Tag, dass Gerätevorgänge per Remotezugriff und als Massenvorgang erledigt werden können und dass sie nur benachrichtigt werden, wenn Probleme direkte Aufmerksamkeit erfordern.

- **Flexibilität und Kompatibilität**: Das Ökosystem der IoT-Geräte ist äußerst vielfältig. Verwaltungstools müssen an eine Vielzahl von Geräteklassen, -plattformen und -protokolle angepasst werden. Die Bediener müssen alle Geräte unterstützen können – von einfachen eingebetteten Chips für Einzelvorgänge bis zu leistungsfähigen Computern mit vollem Funktionsumfang.

- **Kontexterkennung**: IoT-Umgebungen sind dynamisch und verändern sich ständig. Die Zuverlässigkeit des Diensts ist von entscheidender Bedeutung. Bei den Vorgängen der Geräteverwaltung müssen SLA-Wartungsfenster, Netzwerk- und Stromversorgungszustände, Bedingungen während der Nutzung und die Geolocation von Geräten berücksichtigt werden, um sicherzustellen, dass sich Ausfallzeiten aufgrund von Wartungsarbeiten nicht negativ auf wichtige Geschäftsvorgänge auswirken oder zu gefährlichen Situationen führen.

- **Verwaltung vieler Rollen**: Die Unterstützung der einzigartigen Workflows und Prozesse von IoT-Vorgängen ist sehr wichtig. Außerdem muss das Bedienpersonal trotz etwaiger Einschränkungen durch interne IT-Abteilungen harmonisch zusammenarbeiten und relevante Informationen zu Gerätevorgängen an Vorgesetzte und andere Personen mit Leitungsfunktion weitergeben.

## Lebenszyklus von IoT-Geräten 

IoT-Projekte unterscheiden sich zwar meist stark, aber es gibt für die Verwaltung von Geräten trotzdem allgemeingültige Muster. In Azure IoT lassen sich diese Muster innerhalb des IoT-Gerätelebenszyklus identifizieren, der aus fünf einzelnen Phasen besteht:

![][img-device_lifecycle]

1. **Planung**: Bediener müssen in die Lage versetzt werden, ein Schema der Geräteeigenschaften zu erstellen. Damit können sie eine Gruppe von Geräten einfach und präzise abfragen und Massenvorgänge zu Verwaltungszwecken durchführen.

    *Bausteine*: [Tutorial: Get started with device twins (preview)][lnk-twins-getstarted] \(Tutorial: Erste Schritte mit Gerätezwillingen (Vorschau)), [Tutorial: Use desired properties to configure devices (preview)][lnk-twin-properties] \(Tutorial: Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten (Vorschau))

2. **Bereitstellung**: Neue Geräte können gegenüber IoT Hub sicher authentifiziert werden, und Bediener können die Funktionen und den aktuellen Zustand eines Geräts sofort ermitteln.

    *Bausteine*: [Erste Schritte mit Azure IoT Hub für .NET][lnk-hub-getstarted], [Tutorial: Use desired properties to configure devices (preview)][lnk-twin-properties] \(Tutorial: Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten (Vorschau))

3. **Konfiguration**: Massenvorgänge zum Ändern der Konfiguration und Firmwareupdates für Geräte können durchgeführt werden, während gleichzeitig sowohl die Integrität als auch die Sicherheit aufrechterhalten wird.

    *Bausteine*: [Tutorial: Use desired properties to configure devices (preview)][lnk-twin-properties] \(Tutorial: Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten (Vorschau)), [Tutorial: Use direct methods][lnk-c2d-methods] \(Tutorial: Verwenden von direkten Methoden), [Tutorial: Schedule and broadcast jobs (preview)][lnk-jobs] \(Tutorial: Planen und Übertragen von Aufträgen (Vorschau))

4. **Überwachung**: Ermöglicht das Überwachen der Integrität der gesamten Geräteflotte und des Status von fortlaufenden Updaterollouts zum Warnen der Bediener vor Problemen, die ggf. Aufmerksamkeit erfordern.

    *Bausteine*: [Tutorial: Use desired properties to configure devices (preview)][lnk-twin-properties] \(Tutorial: Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten (Vorschau))

5. **Außerbetriebnahme**: Geräte werden nach einem Ausfall oder Upgradezyklus oder am Ende der Lebensdauer ausgetauscht oder außer Betrieb genommen.

    *Bausteine*:
    
## Muster der IoT Hub-Geräteverwaltung

IoT Hub ermöglicht die folgenden (anfänglichen) Muster für die Geräteverwaltung. Wie in den [Tutorials][lnk-get-started] veranschaulicht, können Sie diese Muster erweitern und genau an Ihr Szenario anpassen und basierend auf diesen Hauptmustern auch neue Muster für andere Fälle entwerfen.

1. **Neustart**: Die Back-End-Anwendung informiert das Gerät per D2C-Methode darüber, dass ein Neustart initiiert wurde. Das Gerät nutzt die vom Gerätezwilling gemeldeten Eigenschaften zum Aktualisieren des Neustartstatus des Geräts.

    ![][img-reboot_pattern]

2. **Zurücksetzung auf Werkseinstellungen**: Die Back-End-Anwendung informiert das Gerät per D2C-Methode darüber, dass eine Zurücksetzung auf die Werkseinstellungen initiiert wurde. Das Gerät nutzt die vom Gerätezwilling gemeldeten Eigenschaften zum Aktualisieren des Zurücksetzungsstatus des Geräts.

    ![][img-facreset_pattern]

3. **Konfiguration**: Die Back-End-Anwendung verwendet die vom Gerätezwilling gewünschten Eigenschaften zum Konfigurieren der Software, die auf dem Gerät ausgeführt wird. Das Gerät nutzt die vom Gerätezwilling gemeldeten Eigenschaften zum Aktualisieren des Konfigurationsstatus des Geräts.

    ![][img-config_pattern]

4. **Firmwareupdate**: Die Back-End-Anwendung informiert das Gerät per D2C-Methode darüber, dass ein Firmwareupdate initiiert wurde. Das Gerät initiiert einen Prozess mit mehreren Schritten, um das Firmwarepaket herunterzuladen, das Firmwarepaket anzuwenden und schließlich wieder eine Verbindung mit dem IoT Hub-Dienst herzustellen. Während dieses gesamten Prozesses mit mehreren Schritten verwendet das Gerät die vom Gerätezwilling gemeldeten Eigenschaften, um die Fortschrittsanzeige und den Status des Geräts zu aktualisieren.

    ![][img-fwupdate_pattern]

5. **Fortschritt und Status von Meldungen**: Das Anwendungs-Back-End führt Gerätezwillingabfragen für eine Gruppe von Geräten durch, um den Status und Fortschritt von Aktionen zu melden, die auf dem Gerät ausgeführt werden.

    ![][img-report_progress_pattern]

## Nächste Schritte

Mit den von Azure IoT Hub bereitgestellten Bausteinen können Entwickler IoT-Anwendungen erstellen, mit denen die besonderen Anforderungen von IoT-Bedienern in jeder Phase des Gerätelebenszyklus erfüllt werden.

Weitere Informationen zu den Features der Azure IoT Hub-Geräteverwaltung finden Sie im Tutorial [Erste Schritte mit der Azure IoT Hub-Geräteverwaltung][lnk-get-started].

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md

<!---HONumber=AcomDC_1005_2016-->