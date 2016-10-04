> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

Dieser Artikel beschreibt eine ausführliche exemplarische Vorgehensweise für den [Hello World-Beispielcode][lnk-helloworld-sample] zur Veranschaulichung der grundlegenden Komponenten der [Azure IoT Gateway SDK][lnk-gateway-sdk]-Architektur. Das Beispiel verwendet das Gateway SDK zum Erstellen eines einfachen Gateways, das alle fünf Sekunden eine „Hello World“-Nachricht in eine Datei protokolliert.

Diese Anleitung umfasst:

- **Konzepte**: eine konzeptionelle Übersicht über die Komponenten, aus denen jedes Gateway besteht, das Sie mit dem Gateway SDK erstellen.
- **Hello World-Beispielarchitektur**: beschreibt die Anwendung der Konzepte auf das Hello World-Beispiel und das Zusammenwirken der Komponenten.
- **Erstellen des Beispiels**: die zum Erstellen des Beispiels erforderlichen Schritte.
- **Ausführen des Beispiels**: die zum Ausführen Beispiels erforderlichen Schritte.
- **Typische Ausgabe**: Ein Beispiel der bei Ausführung des Beispiels zu erwartende Ausgabe.
- **Codeausschnitte**: Eine Sammlung von Codeausschnitten, die veranschaulicht, wie das Hello World-Beispiel die wichtigsten Gatewaykomponenten implementiert.

## Gateway SDK-Konzepte

Bevor Sie den Beispielcode untersuchen oder selbst mithilfe des Gateway SDK ein Bereichsgateway erstellen, sollten Sie die wichtigsten Konzepte kennen, die der Architektur des SDK zugrunde liegen.

### Module

Sie erstellen ein Gateway mit dem Azure IoT Gateway SDK, indem Sie *Module* erstellen und zusammenstellen. Module verwenden *Nachrichten*, um untereinander Daten auszutauschen. Ein Modul empfängt eine Nachricht, führt eine Aktion damit aus, transformiert sie optional in eine neue Nachricht und veröffentlicht sie dann zur Verarbeitung an andere Module, Einige Module erzeugen nur neue Nachrichten und verarbeiten niemals eingehende Nachrichten. Eine Modulkette erstellt eine Datenverarbeitungspipeline, wobei jedes Modul an einem bestimmten Punkt dieser Pipeline eine Transformation der Daten ausführt.

![][1]
 
Das SDK enthält Folgendes:

- Vordefinierte Module, die allgemeine Gatewayfunktionen ausführen.
- Die Schnittstellen, die ein Entwickler zum Schreiben benutzerdefinierter Module verwenden kann.
- Die Infrastruktur, die zur Bereitstellung und Ausführung eines Modulsatzes erforderlich ist.

Das SDK stellt eine Abstraktionsschicht bereit, mit deren Hilfe Sie Gateways erstellen können, die auf einer Vielzahl von Betriebssystemen und Plattformen ausgeführt werden können.

![][2]

### Meldungen

Die Vorstellung, dass Module einander Nachrichten übergeben, ist eine praktische Möglichkeit, die Funktionsweise eines Gateways zu verstehen, spiegelt stellt den tatsächlichen Vorgang aber nicht exakt wider. Module verwenden einen Broker, um miteinander zu kommunizieren. Sie veröffentlichen Nachrichten für den Broker (Bus, Pub/Sub oder anderes Messagingmuster), die dann vom Broker an die verbundenen Module weitergeleitet werden.

Ein Modul verwendet die Funktion **Broker\_Publish**, um eine Nachricht für den Broker zu veröffentlichen. Der Broker übermittelt Nachrichten durch Aufrufen einer Rückruffunktion an ein Modul. Eine Nachricht besteht aus einem Satz aus Schlüssel-Wert-Eigenschaften und Inhalten, der als Arbeitsspeicherblock übergeben wird.

![][3]

### Nachrichtenrouting und -filterung

Nachrichten können auf zwei Arten an die richtigen Module weitergeleitet werden: An den Broker kann eine Gruppe von Links übergeben werden, um den Broker über Quelle und Senke der einzelnen Module zu informieren. Alternativ kann das Modul eine Filterung auf der Grundlage der Nachrichteneigenschaften durchführen. Ein Modul soll nur auf Nachrichten reagieren, die für das Modul bestimmt sind. Die Links und die Messagingfilterung bilden im Grunde eine Nachrichtenpipeline.

## Hello World-Beispielarchitektur

Das Hello World-Beispiel veranschaulicht die Konzepte, die im vorherigen Abschnitt beschrieben wurden. Das Hello World-Beispiel implementiert ein Gateway, das über eine aus zwei Modulen bestehende Pipeline verfügt:

-	Das Modul *hello world* erstellt alle fünf Sekunden eine Nachricht und übergibt sie an das Modul „logger“.
-	Das Modul *logger* schreibt die empfangene Nachricht in eine Datei.

![][4]

Wie im vorherigen Abschnitt beschrieben, übergibt das Hello World-Modul Nachrichten nicht direkt an das Protokollierungsmodul. Stattdessen veröffentlicht es alle fünf Sekunden eine Nachricht für den Broker.

Das Modul „logger“ empfängt die Nachricht vom Broker und reagiert darauf, indem es den Inhalt der Nachricht in eine Datei schreibt.

Das Modul „logger“ verarbeitet nur Nachrichten des Brokers. Es veröffentlicht niemals neue Nachrichten für den Broker.

![][5]

Die Abbildung zeigt die Architektur des Hello World-Beispiels sowie die relativen Pfade zu den Quelldateien, die verschiedene Teile des Beispiels im [Repository][lnk-gateway-sdk] implementieren. Untersuchen Sie den Code selbst, oder verwenden Sie die unten stehenden Codeausschnitte als Leitfaden.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

<!---HONumber=AcomDC_0928_2016-->