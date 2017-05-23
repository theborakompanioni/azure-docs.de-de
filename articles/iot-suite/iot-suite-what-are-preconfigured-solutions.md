---
title: "Vorkonfigurierte Azure IoT-Lösungen | Microsoft Docs"
description: "Eine Beschreibung der vorkonfigurierten Azure IoT-Lösungen und ihrer Architektur mit Links zu zusätzlichen Ressourcen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 59009f37-9ba0-4e17-a189-7ea354a858a2
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/24/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 29e8639a6f1f0c2733d24dda78975ea7cfb6107a
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Was sind vorkonfigurierte Azure IoT Suite-Lösungen?
Bei den vorkonfigurierten Azure IoT Suite-Lösungen handelt es sich um Implementierungen allgemeiner IoT-Lösungsmuster, die Sie in Azure mit Ihrem Abonnement bereitstellen können. Sie können vorkonfigurierte Lösungen für Folgendes verwenden:

* Als Ausgangspunkt für eigene IoT-Lösungen.
* Zum Erlernen der allgemeinen Muster beim Entwerfen und Entwickeln von IoT-Lösungen.

Bei jeder vorkonfigurierten Lösung handelt es sich um eine vollständige End-to-End-Implementierung, bei der simulierte Geräte zum Generieren von Telemetriedaten verwendet werden.

Sie können nicht nur die Lösungen in Azure bereitstellen und ausführen, sondern auch den kompletten Quellcode herunterladen und die Lösung dann gemäß Ihren speziellen IoT-Anforderungen anpassen und erweitern.

> [!NOTE]
> Informationen zum Bereitstellen einer vorkonfigurierten Lösung finden Sie unter [Microsoft Azure IoT Suite][lnk-azureiotsuite]. Der Artikel [Erste Schritte mit den vorkonfigurierten IoT-Lösungen][lnk-getstarted-preconfigured] enthält weitere Informationen zum Bereitstellen und Ausführen der Lösungen.
> 
> 

In der folgenden Tabelle wird gezeigt, welchen IoT-Features die Lösungen zugeordnet sind:

| Lösung | Datenerfassung | Geräteidentität | Geräteverwaltung | Befehl und Steuerung | Regeln und Aktionen | Predictive Analytics |
| --- | --- | --- | --- | --- | --- | --- |
| [Remoteüberwachung][lnk-getstarted-preconfigured] |Ja |Ja |Ja |Ja |Ja |- |
| [Vorbeugende Wartung][lnk-predictive-maintenance] |Ja |Ja |- |Ja |Ja |Ja |
| [Verbundene Factory][lnk-getstarted-factory] |Ja |Ja |Ja |Ja |Ja |- |

* *Datenerfassung:*Skalierbarer Dateneingang in der Cloud
* *Geräteidentität:* Verwalten eindeutiger Geräteidentitäten und Steuern des Gerätezugriffs auf die Lösung.
* *Geräteverwaltung:* Verwalten von Gerätemetadaten und Ausführen von Vorgängen wie Geräteneustarts und Firmwareupgrades.
* *Befehl und Steuerung:* Senden von Nachrichten aus der Cloud an ein Gerät, um zu veranlassen, dass das Gerät eine Aktion ausführt.
* *Regeln und Aktionen:*Das Lösung-Back-End verwendet Regeln, um auf bestimmte D2C (Device-to-Cloud)-Daten zu reagieren.
* *Predictive Analytics:* Das Lösungs-Back-End analysiert D2C-Daten, um vorherzusagen, wann bestimmte Aktionen ausgeführt werden sollen. Beispiel: Analysieren von Telemetriedaten eines Flugzeugtriebwerks, um zu bestimmen, wann die Triebwerkwartung erforderlich ist.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Übersicht über die vorkonfigurierte Lösung zur Remoteüberwachung
Die vorkonfigurierte Lösung zur Remoteüberwachung wird in diesem Artikel beschrieben, weil sie zur Veranschaulichung zahlreicher gängiger Entwurfselemente dient, die auch in den anderen vorkonfigurierten Lösungen enthalten sind.

Im folgenden Diagramm sind die wichtigsten Elemente der Remoteüberwachungslösung dargestellt. Die folgenden Abschnitte enthalten weitere Informationen zu diesen Elementen.

![Architektur der vorkonfigurierten Lösung für Remoteüberwachung][img-remote-monitoring-arch]

## <a name="devices"></a>Geräte
Wenn Sie die vorkonfigurierte Lösung für die Remoteüberwachung bereitstellen, werden vier simulierte Geräte in der Lösung vorab bereitgestellt, mit denen ein Kühlgerät simuliert wird. Diese simulierte Geräte verfügen über ein integriertes Temperatur- und Luftfeuchtigkeitsmodell, das Telemetriedaten ausgibt. Die simulierten Geräte sind aus folgenden Gründen enthalten:
- Sie veranschaulichen den End-to-End-Datenfluss durch die Lösung.
- Sie sind eine praktische Quelle für Telemetriedaten.
- Sie bieten ein Ziel für Methoden oder Befehle, wenn Back-End-Entwickler die Lösung als Ausgangspunkt für eine benutzerdefinierte Implementierung nutzen.

Die simulierten Geräte in der Lösung können auf folgende Kommunikation zwischen Cloud und Gerät reagieren:

- *Methoden ([direkte Methoden][lnk-direct-methods]):* Eine bidirektionale Kommunikationsmethode, bei der eine umgehende Reaktion des verbundenen Geräts erwartet wird.
- *Befehle (C2D-Nachrichten):* Eine unidirektionale Kommunikationsmethode, bei der ein Gerät den Befehl aus einer permanenten Warteschlange abruft.

Einen Vergleich dieser unterschiedlichen Konzepte finden Sie im [Leitfaden zur C2D-Kommunikation][lnk-c2d-guidance].

Wenn ein Gerät erstmals eine Verbindung mit IoT Hub in der vorkonfigurierten Lösung herstellt, sendet es eine Geräteinformationsmeldung an den Hub, in der die Methoden aufgelistet sind, auf die das Gerät reagieren kann. In der vorkonfigurierten Lösung für die Remoteüberwachung werden von simulierten Geräten folgende Methoden unterstützt:

* *Initiate Firmware Update* (Firmwareupdate initiieren): Diese Methode initiiert auf dem Gerät eine asynchrone Aufgabe, um ein Firmwareupdate auszuführen. Die asynchrone Aufgabe verwendet gemeldete Eigenschaften, um Statusaktualisierungen auf dem Lösungsdashboard bereitzustellen.
* *Neustart*: Diese Methode bewirkt, dass das simulierte Gerät neu gestartet wird.
* *FactoryReset*: Diese Methode löst auf dem Gerät eine Zurücksetzung auf die Werkseinstellungen aus.

Wenn ein Gerät erstmals eine Verbindung mit IoT Hub in der vorkonfigurierten Lösung herstellt, sendet es eine Geräteinformationsmeldung an den Hub, in der die Befehle aufgelistet sind, auf die das Gerät reagieren kann. In der vorkonfigurierten Lösung für die Remoteüberwachung werden von simulierten Geräten folgende Befehle unterstützt:

* *Ping Device*(Gerät pingen): Das Gerät reagiert auf diesen Befehl mit einer Bestätigung. Mithilfe dieses Befehls kann überprüft werden, ob das Gerät noch aktiv ist und lauscht.
* *Start Telemetry*(Telemetrie starten): Weist das Gerät an, mit dem Senden von Telemetriedaten zu beginnen.
* *Stop Telemetry*(Telemetrie beenden): Weist das Gerät an, das Senden von Telemetriedaten zu beenden.
* *Change Set Point Temperature*(Festgelegte Punkttemperatur ändern): Steuert die simulierten Temperaturtelemetriewerte, die das Gerät sendet. Dieser Befehl ist hilfreich beim Testen der Back-End-Logik.
* *Diagnostic Telemetry*(Diagnosetelemetrie): Steuert, ob das Gerät die externen Temperaturtelemetriedaten sendet.
* *Change Device State*(Gerätestatus ändern): Legt die Metadateneigenschaft des Gerätezustands fest, den das Gerät meldet. Dieser Befehl ist hilfreich beim Testen der Back-End-Logik.

Sie können der Lösung weitere simulierte Geräte hinzufügen, die die gleichen Telemetriedaten ausgeben und auf die gleichen Methoden und Befehle reagieren.

Die Lösung reagiert nicht nur auf Befehle und Methoden, sondern verwendet auch [Gerätezwillinge][lnk-device-twin]. Gerätezwillinge werden von Geräten verwendet, um Eigenschaftswerte an das Lösungs-Back-End zu melden. Das Lösungsdashboard verwendet Gerätezwillinge, um neue gewünschte Eigenschaftswerte für Geräte festzulegen. Während des Firmwareupdates meldet das simulierte Gerät beispielsweise den Status des Updates unter Verwendung gemeldeter Eigenschaften.

## <a name="iot-hub"></a>IoT Hub
Bei dieser vorkonfigurierten Lösung entspricht die IoT Hub-Instanz dem *Cloud-Gateway* in einer typischen [IoT-Lösungsarchitektur][lnk-what-is-azure-iot].

Ein IoT Hub empfängt Telemetriedaten von den Geräten an einem Endpunkt. Ein IoT Hub verwaltet auch gerätespezifische Endpunkte, wobei jedes Gerät die Befehle abrufen kann, die an das Gerät gesendet werden.

Der IoT Hub macht die empfangenen Telemetriedaten über den dienstseitigen Endpunkt für gelesene Telemetriedaten verfügbar.

Mit der Geräteverwaltungsfunktion von IoT Hub können Sie Ihre Geräteeigenschaften über das Lösungsportal verwalten und Aufträge planen, die beispielsweise folgende Vorgänge ausführen:

- Neustarten von Geräten
- Ändern von Gerätezuständen
- Firmwareupdates

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Die vorkonfigurierte Lösung verwendet drei [Azure Stream Analytics][lnk-asa]-Aufträge (ASA), um den Telemetriedatenstrom von den Geräten zu filtern:

* *DeviceInfo-Auftrag* – Gibt Daten an einen Event Hub aus, der geräteregistrierungsspezifische Nachrichten an die Geräteregistrierung der Lösung (eine Azure Cosmos DB-Datenbank) weiterleitet. Die Nachricht wird gesendet, wenn ein Gerät erstmals eine Verbindung herstellt oder ein Befehl zum Ändern des Gerätestatus**** ausgeführt wurde.
* *Telemetrieauftrag* – Sendet alle Telemetrierohdaten zu Cold Storage-Zwecken an den Azure-Blobspeicher und berechnet Telemetrieaggregationen, die im Lösungsdashboard angezeigt werden.
* *Regelauftrag* – Filtert den Telemetriedatenstrom, um Werte zu identifizieren, die Regelschwellenwerte überschreiten, und gibt die Daten an einen Event Hub aus. Wenn eine Regel ausgelöst wird, wird das Ereignis in der Dashboardansicht des Lösungsportals als neue Zeile der Alarmverlaufstabelle angezeigt. Diese Regeln können auf der Grundlage der Einstellungen, die im Lösungsportal in den Ansichten **Regeln** und **Aktionen** definiert sind, auch eine Aktion auslösen.

Bei dieser vorkonfigurierten Lösung bilden die ASA-Aufträge einen Teil des **IoT-Lösungs-Back-Ends** in einer typischen [IoT-Lösungsarchitektur][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Ereignisprozessor
Bei dieser vorkonfigurierten Lösung bildet der Ereignisprozessor einen Teil des **IoT-Lösungs-Back-Ends** in einer typischen [IoT-Lösungsarchitektur][lnk-what-is-azure-iot].

Die ASA-Aufträge **DeviceInfo** und **Rules** senden ihre Ausgabe an Event Hubs für die Weitergabe an andere Back-End-Dienste. Die Lösung verwendet eine [EventProcessorHost][lnk-event-processor]-Instanz, die in einem [WebJob][lnk-web-job] ausgeführt wird, um die Nachrichten von diesen Event Hubs zu lesen. **EventProcessorHost** verwendet Folgendes:
- Die Daten vom Typ **DeviceInfo**, um die Gerätedaten in der Cosmos DB-Datenbank zu aktualisieren.
- Die**** Regeldaten, um die Logik-App aufzurufen und die Warnungsanzeige im Lösungsportal zu aktualisieren.

## <a name="device-identity-registry-device-twin-and-cosmos-db"></a>Geräteidentitätsregistrierung, Gerätezwilling und Cosmos DB
Jede IoT Hub-Instanz verfügt über eine [Geräteidentitätsregistrierung][lnk-identity-registry] zum Speichern von Geräteschlüsseln. IoT Hub verwendet diese Informationen zum Authentifizieren von Geräten – ein Gerät muss registriert sein und einen gültigen Schlüssel haben, bevor es eine Verbindung mit dem Hub herstellen kann.

Bei einem [Gerätezwilling][lnk-device-twin] handelt es sich um ein von IoT Hub verwaltetes JSON-Dokument. Ein Gerätezwilling enthält Folgendes:

- Gemeldete Eigenschaften, die vom Gerät an den Hub gesendet wurden. Diese Eigenschaften können im Lösungsportal angezeigt werden.
- Gewünschte Eigenschaften, die an das Gerät gesendet werden sollen. Diese Eigenschaften können im Lösungsportal festgelegt werden.
- Tags, die nur im Gerätezwilling (und nicht auf dem Gerät) vorhanden sind. Mithilfe dieser Tags können Gerätelisten im Lösungsportal gefiltert werden.

Diese Lösung verwendet Gerätezwillinge zum Verwalten von Gerätemetadaten. Die Lösung verwendet auch eine Cosmos DB-Datenbank, um zusätzliche lösungsspezifische Gerätedaten (beispielsweise die von den einzelnen Geräten unterstützten Befehle und den Befehlsverlauf) zu speichern.

Die Lösung muss zudem die Informationen in der Geräteidentitätsregistrierung mit dem Inhalt der Cosmos DB-Datenbank synchron halten. **EventProcessorHost** verwendet die Daten aus dem **DeviceInfo**-Datenstromanalyseauftrag zum Verwalten der Synchronisierung.

## <a name="solution-portal"></a>Lösungsportal
![Lösungsportal][img-dashboard]

Das Lösungsportal ist eine webbasierte Benutzeroberfläche, die in der Cloud als Teil der vorkonfigurierten Lösung bereitgestellt wird. Hiermit haben Sie folgende Möglichkeiten:

* Anzeigen von Telemetriedaten und Alarmverlauf in einem Dashboard
* Bereitstellen von neuen Geräten
* Verwalten und Überwachen von Geräten
* Senden von Befehlen an bestimmte Geräte
* Aufrufen von Methoden auf bestimmten Geräten
* Verwalten von Regeln und Aktionen
* Planen von Aufträgen, die auf einem einzelnen Gerät oder auf mehreren Geräten ausgeführt werden sollen

Bei dieser vorkonfigurierten Lösung bildet das Lösungsportal einen Teil des **IoT-Lösungs-Back-Ends** und einen Teil der **Verarbeitungsverbindung und Business Connectivity** in der typischen [IoT-Lösungsarchitektur][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den IoT-Lösungsarchitekturen finden Sie unter [Microsoft Azure IoT services: Reference Architecture][lnk-refarch] (Microsoft Azure IoT-Dienste: Referenzarchitektur).

Sie wissen nun, worum es sich bei einer vorkonfigurierten Lösung handelt, und können beginnen, indem Sie die vorkonfigurierte Lösung für die *Remoteüberwachung* bereitstellen: [Erste Schritte mit den vorkonfigurierten Lösungen][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-getstarted-factory]: iot-suite-connected-factory-overview.md

