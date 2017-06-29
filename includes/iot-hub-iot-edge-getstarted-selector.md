> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-get-started.md)
> 
> 

Dieser Artikel beschreibt eine ausführliche exemplarische Vorgehensweise für den [Hello World-Beispielcode][lnk-helloworld-sample] zur Veranschaulichung der grundlegenden Komponenten der [Azure IoT Edge][lnk-iot-edge]-Architektur. In dem Beispiel wird Azure IoT Edge verwendet, um ein einfaches Gateway zu erstellen, das alle fünf Sekunden eine Hello World-Nachricht in einer Datei protokolliert.

Diese Anleitung umfasst:

* **„Hallo Welt“-Beispielarchitektur:** beschreibt die Anwendung der [Azure IoT Edge-Architekturkonzepte][lnk-edge-concepts] auf das „Hallo Welt“-Beispiel und das Zusammenwirken der Komponenten.
* **Erstellen des Beispiels**: die zum Erstellen des Beispiels erforderlichen Schritte.
* **Ausführen des Beispiels**: die zum Ausführen Beispiels erforderlichen Schritte. 
* **Typische Ausgabe**: Ein Beispiel der bei Ausführung des Beispiels zu erwartende Ausgabe.
* **Codeausschnitte**: eine Sammlung von Codeausschnitten, die veranschaulichen, wie das Hello World-Beispiel die wichtigsten IoT Edge-Gatewaykomponenten implementiert.


## <a name="hello-world-sample-architecture"></a>Hello World-Beispielarchitektur
Das Hello World-Beispiel veranschaulicht die Konzepte, die im vorherigen Abschnitt beschrieben wurden. Das Hello World-Beispiel implementiert ein IoT Edge-Gateway, das über eine aus zwei IoT Edge-Modulen bestehende Pipeline verfügt:

* Das Modul *hello world* erstellt alle fünf Sekunden eine Nachricht und übergibt sie an das Modul „logger“.
* Das Modul *logger* schreibt die empfangene Nachricht in eine Datei.

![Mit Azure IoT Edge erstellte Architektur des Hello World-Beispiels][4]

Wie im vorherigen Abschnitt beschrieben, übergibt das Hello World-Modul Nachrichten nicht direkt an das Protokollierungsmodul. Stattdessen veröffentlicht es alle fünf Sekunden eine Nachricht für den Broker.

Das Modul „logger“ empfängt die Nachricht vom Broker und reagiert darauf, indem es den Inhalt der Nachricht in eine Datei schreibt.

Das Modul „logger“ verarbeitet nur Nachrichten des Brokers. Es veröffentlicht niemals neue Nachrichten für den Broker.

![Funktionsweise der Weiterleitung von Nachrichten zwischen Modulen in Azure IoT Edge durch den Broker][5]

Die Abbildung zeigt die Architektur des Hello World-Beispiels sowie die relativen Pfade zu den Quelldateien, die verschiedene Teile des Beispiels im [Repository][lnk-iot-edge] implementieren. Untersuchen Sie den Code selbst, oder verwenden Sie die unten stehenden Codeausschnitte als Leitfaden.

<!-- Images -->
[4]: media/iot-hub-iot-edge-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-iot-edge-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/iot-edge/tree/master/samples/hello_world
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-edge-concepts]: ../articles/iot-hub/iot-hub-iot-edge-overview.md