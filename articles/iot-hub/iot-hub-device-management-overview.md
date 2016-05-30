<properties
 pageTitle="Überblick zur Geräteverwaltung | Microsoft Azure"
 description="Überblick über die Azure IoT Hub-Geräteverwaltung: Gerätezwillinge, Geräteabfragen, Geräteaufträge"
 services="iot-hub"
 documentationCenter=""
 authors="ellenfosborne"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="elfarber"/>

# Überblick über die Azure IoT Hub-Geräteverwaltung (Vorschau)

Die Azure IoT Hub-Geräteverwaltung ermöglicht Ihnen standardbasierte IoT-Geräteverwaltung, sodass Sie Ihre Geräte remote verwalten, konfigurieren und aktualisieren können.

Die drei wichtigsten Konzepte für die Geräteverwaltung in Azure IoT sind:

1.  **Gerätezwillinge**: die Darstellung des physischen Geräts in IoT Hub.

2.  **Geräteabfragen**: Sie können Gerätezwillinge suchen und aggregierte Kenntnisse von Gerätezwillingen generieren. Beispielsweise könnten Sie alle Gerätezwillinge mit Firmwareversion 1.0 ermitteln.

3.  **Geräteaufträge**: eine für ein oder mehrere physische Geräte durchgeführte Aktion, z.B. Firmwareupdate, Neustart und Zurücksetzung auf Werkseinstellungen.

## Gerätezwilling

Der Gerätezwilling ist die Darstellung eines physischen Geräts in Azure IoT. Das **Microsoft.Azure.Devices.Device**-Objekt wird verwendet, um den Gerätezwilling darzustellen.

![][img-twin]

Der Gerätezwilling besteht aus folgenden Komponenten:

1.  **Gerätefelder**: Gerätefelder sind vordefinierte Eigenschaften, die sowohl für IoT Hub-Messaging als auch für die Geräteverwaltung verwendet werden. Sie unterstützen IoT Hub beim Identifizieren von physischen Geräten und Verbinden mit ihnen. Gerätefelder sind nicht mit dem Gerät synchronisiert und werden ausschließlich im Gerätezwilling gespeichert. Gerätefelder enthalten die Geräte-ID und Authentifizierungsinformationen.

2.  **Geräteeigenschaften**: Geräteeigenschaften sind ein vordefiniertes Wörterbuch von Eigenschaften, die das physische Gerät beschreiben. Das physische Gerät ist der Master jeder Geräteeigenschaft und der autoritative Speicher jedes entsprechenden Werts. Eine konsistente Darstellung dieser Eigenschaften ist im Gerätezwilling und in der Cloud gespeichert. Die Kohärenz und Aktualität unterliegen den Synchronisierungseinstellungen, wie in [Tutorial: Verwenden des Gerätezwillings][lnk-tutorial-twin] beschrieben. Einige Beispiele für Geräteeigenschaften sind Firmwareversion, Akkukapazität und der Namen des Herstellers.

3.  **Diensteigenschaften**: Diensteigenschaften sind **&lt;Schlüssel, Wert&gt;**-Paare, die der Entwickler dem Diensteigenschaften-Wörterbuch hinzufügt. Diese Eigenschaften erweitern das Datenmodell für den Gerätezwilling, sodass Sie das Gerät besser charakterisieren können. Diensteigenschaften sind nicht mit dem Gerät synchronisiert und werden ausschließlich im Gerätezwilling in der Cloud gespeichert. Ein Beispiel für eine Diensteigenschaft ist **&lt;NextServiceDate, 11/12/2017&gt;**, die genutzt werden könnte, um Geräte nach ihrem nächsten Wartungsdatum zu suchen.

4.  **Tags**: Tags sind eine Teilmenge der Diensteigenschaften, die beliebige Zeichenfolgen statt Wörterbucheigenschaften sind. Mit Ihnen können Gerätezwillinge mit Anmerkungen versehen oder Geräte in Gruppen organisiert werden. Tags sind nicht mit dem Gerät synchronisiert und werden ausschließlich im Gerätezwilling gespeichert. Wenn Ihr Gerät z.B. einen physischen LKW darstellt, könnten Sie für jeden Typ von Ladung des LKWs ein Tag hinzufügen – **Äpfel**, **Orangen** und **Bananen**.

## Geräteabfragen

Im vorherigen Abschnitt haben Sie die verschiedenen Komponenten des Gerätezwillings kennen gelernt. Jetzt erfahren Sie, wie Sie Gerätezwillinge in der IoT Hub-Geräteregistrierung basierend auf Geräte- und Diensteigenschaften oder Tags suchen können. Sie würden z.B. eine Abfrage verwenden, um nach Geräten suchen, die aktualisiert werden müssen. Sie können Abfragen nach allen Geräten mit einer bestimmten Firmwareversion durchführen und das Ergebnis an eine bestimmte Aktion senden (in IoT Hub als Geräteauftrag bezeichnet, der im folgenden Abschnitt erläutert wird).

Sie können Tags und Eigenschaften zur Abfrage verwenden:

-   Bei der Abfrage nach Gerätezwillingen mit Tags übergeben Sie ein Zeichenfolgenarray, und die Abfrage gibt den Satz von Geräten zurück, die mit allen diesen Zeichenfolgen gekennzeichnet sind.

-   Bei der Abfrage nach Gerätezwillingen mit Dienst- oder Geräteeigenschaften verwenden Sie einen JSON-Abfrageausdruck. Das folgende Beispiel zeigt, wie Sie eine Abfrage für alle Geräte mit der Geräteeigenschaft mit dem Schlüssel **FirmwareVersion** und **1.0** durchführen könnten. Sie sehen, dass der **type** der Eigenschaft **device** ist, was darauf hinweist, dass die Abfrage basierend auf Geräte-, nicht Diensteigenschaften durchgeführt wird:

  ```
  {                           
      "filter": {                  
        "property": {                
          "name": "FirmwareVersion",   
          "type": "device"             
        },                           
        "value": "1.0",              
        "comparisonOperator": "eq",  
        "type": "comparison"         
      },                           
      "project": null,             
      "aggregate": null,           
      "sort": null                 
  }
  ```

## Geräteaufträge

Das nächste Konzept in der Geräteverwaltung sind Geräteaufträge, die die Koordination mehrstufiger Orchestrierungen auf mehreren Geräten ermöglichen.

Es gibt sechs Typen von Geräteaufträgen, die zurzeit von der Azure IoT Hub-Geräteverwaltung bereitgestellt werden (es werden zusätzliche Aufträge hinzugefügt, sobald Kunden sie benötigen):

- **Firmwareupdate**: Dient zum Aktualisieren der Firmware (bzw. des Betriebssystemimage) auf dem physischen Gerät.
- **Neustart**: Startet das physische Gerät neu.
- **Zurücksetzung auf Werkseinstellungen**: Setzt die Firmware (bzw. das Betriebssystemimage) des physischen Geräts auf ein ab Werk bereitgestelltes Sicherungsimage zurück, das auf dem Gerät gespeichert ist.
- **Konfigurationsaktualisierung**: Konfiguriert den IoT Hub-Client-Agent, der auf dem physischen Gerät ausgeführt wird.
- **Geräteeigenschaft lesen**: Dient zum Abrufen des aktuellsten Werts einer Geräteeigenschaft auf dem physischen Gerät.
- **Geräteeigenschaft schreiben**: Ändert eine Geräteeigenschaft auf dem physischen Gerät.

Weitere Informationen zur Verwendung jedes dieser Aufträge finden Sie in der [API-Dokumentation für C# und node.js][lnk-apidocs].

Ein Auftrag kann für mehrere Geräte ausgeführt werden. Beim Starten eines Auftrags wird ein untergeordneter Auftrag für jedes dieser Geräte erstellt. Ein untergeordneter Auftrag wird auf einem einzelnen Gerät ausgeführt. Jeder untergeordnete Auftrag verfügt über einen Zeiger auf den übergeordneten Auftrag. Der übergeordnete Auftrag ist nur ein Container für die untergeordneten Aufträge, er implementiert keine Logik zur Unterscheidung zwischen Gerätetypen (z.B. Unterscheidung zwischen Aktualisieren eines Intel Edison-Geräts und Aktualisieren eines Raspberry Pi-Geräts). Das folgende Diagramm veranschaulicht die Beziehung zwischen einem übergeordneten Auftrag, den ihm untergeordneten Aufträgen und den zugehörigen physischen Geräten.

![][img-jobs]

Sie können den Auftragsverlauf abfragen, um den Status von Aufträgen zu erkennen, die Sie gestartet haben. Einige Beispiele für Abfragen finden Sie in [unserer Abfragebibliothek][lnk-query-samples].

## Geräteimplementierung

Wir haben die dienstseitigen Konzepte behandelt und wenden uns jetzt dem Erstellen eines verwalteten physischen Geräts zu. Mit der Clientbibliothek der Azure IoT Hub-Geräteverwaltung können Sie Ihre IoT-Geräte mit Azure IoT Hub verwalten. Das „Verwalten“ umfasst Aktionen wie das Neustarten, Zurücksetzen auf die Werkseinstellung und Aktualisieren der Firmware. Derzeit stellen wir eine plattformunabhängige C-Bibliothek bereit, aber wir werden in Kürze Unterstützung für weitere Sprachen hinzufügen.

Die Clientbibliothek der Geräteverwaltung hat zwei Hauptaufgaben:

- Synchronisieren von Eigenschaften auf dem physischen Gerät mit dem entsprechenden Gerätezwilling in IoT Hub
- Choreographieren von Geräteaufträgen, die von IoT Hub an das Gerät gesendet werden

Weitere Informationen zu diesen Aufgaben und der Implementierung auf dem physischen Gerät finden Sie unter [Einführung in die Clientbibliothek der Azure IoT Hub-Geräteverwaltung für C][lnk-library-c].

## Nächste Schritte

Weitere Informationen zu den Geräteverwaltungsfeatures von Azure IoT Hub erhalten Sie in den folgenden Tutorials:

- [Erste Schritte mit der Azure IoT Hub-Geräteverwaltung][lnk-get-started]

- [Verwenden des Gerätezwillings][lnk-tutorial-twin]

- [Suchen nach Gerätezwillingen mithilfe von Abfragen][lnk-tutorial-queries]

- [Aktualisieren der Gerätefirmware mithilfe von Geräteaufträgen][lnk-tutorial-jobs]

<!-- Images and links -->
[img-twin]: media/iot-hub-device-management-overview/image1.png
[img-jobs]: media/iot-hub-device-management-overview/image2.png
[img-client]: media/iot-hub-device-management-overview/image3.png

[lnk-lwm2m]: http://technical.openmobilealliance.org/Technical/technical-information/release-program/current-releases/oma-lightweightm2m-v1-0
[lnk-library-c]: iot-hub-device-management-library.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-tutorial-twin]: iot-hub-device-management-device-twin.md
[lnk-tutorial-queries]: iot-hub-device-management-device-query.md
[lnk-tutorial-jobs]: iot-hub-device-management-device-jobs.md
[lnk-apidocs]: http://azure.github.io/azure-iot-sdks/
[lnk-query-samples]: https://github.com/Azure/azure-iot-sdks/blob/dmpreview/doc/get_started/dm_queries/query-samples.md

<!---HONumber=AcomDC_0518_2016-->