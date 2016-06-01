<properties
	pageTitle="Zwillingsabfragen mit der IoT Hub-Geräteverwaltung | Microsoft Azure"
	description="Tutorial zur Azure IoT Hub-Geräteverwaltung, in dem das Suchen von Gerätezwillingen mit Abfragen beschrieben wird."
	services="iot-hub"
	documentationCenter=".net"
	authors="ellenfosborne"
	manager="timlt"
	editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="elfarber"/>

# Tutorial: Gewusst wie: Suchen nach Gerätezwillingen mithilfe von Abfragen mit Node.js (Vorschau)

[AZURE.INCLUDE [iot-hub-device-management-query-selector](../../includes/iot-hub-device-management-query-selector.md)]

Mit der Azure IoT-Geräteverwaltung können Sie Gerätezwillinge – Dienstdarstellungen eines physischen Geräts – mit Abfragen suchen. Sie können Abfragen basierend auf Geräteeigenschaften, Diensteigenschaften oder Tags im Gerätezwilling durchführen. Sie können Tags und Eigenschaften zur Abfrage verwenden:

-   Bei der Abfrage nach Gerätezwillingen mit Tags übergeben Sie ein Zeichenfolgenarray, und die Abfrage gibt den Satz von Geräten zurück, die mit allen diesen Zeichenfolgen gekennzeichnet sind.

-   Bei der Abfrage nach Gerätezwillingen mit Dienst- oder Geräteeigenschaften verwenden Sie einen JSON-Abfrageausdruck.

Weitere Informationen zu Gerätezwillingen und Abfragen finden Sie unter [Überblick über die Azure IoT Hub-Geräteverwaltung][lnk-dm-overview].

## Ausführen des Beispiels einer Geräteabfrage

Das folgende Beispiel erweitert die Funktionalität des Tutorials [Erste Schritte mit der Azure IoT Hub-Geräteverwaltung mithilfe von C# (Vorschau)][lnk-get-started]. Auf der Basis, dass die verschiedenen simulierten Geräte ausgeführt werden, wird eine Abfrage durchgeführt, um bestimmte Geräte zu suchen.

### Voraussetzungen 

Vor dem Ausführen dieses Beispiels müssen Sie die Schritte in [Erste Schritte mit der Azure IoT Hub-Geräteverwaltung mithilfe von C# (Vorschau)][lnk-get-started] abgeschlossen haben. Das bedeutet, dass Ihre simulierten Geräte ausgeführt werden müssen. Wenn Sie den Vorgang zuvor abgeschlossen haben, starten Sie Ihre simulierten Geräte jetzt neu.

### Starten des Beispiels

Um das Beispiel zu starten, müssen Sie `registry_queryDevices.js` ausführen. Damit werden ein paar unterschiedliche Abfragen ausgeführt. Folgen Sie den Schritten unten, um das Beispiel zu starten:

1.  Die simulierten Geräte sollten zunächst mindestens 1 Minute lang ausgeführt werden. So wird sichergestellt, dass die Geräteeigenschaften im Zwilling mit dem physischen Gerät synchronisiert werden. Weitere Informationen zur Synchronisierung finden Sie unter [Tutorial: Verwenden des Gerätezwillings][lnk-twin-tutorial].

2.  Navigieren Sie aus dem Stammordner, in dem Sie das Repository **azure-iot-sdks** geklont haben, zum Verzeichnis **azure-iot-sdks/node/service/samples**.

3.  Öffnen Sie **registry\_queryDevices.js**, und ersetzen Sie den Platzhalter durch Ihre IoT Hub-Verbindungszeichenfolge.

4.  Führen Sie `node registry_queryDevices.js` aus.

Im Befehlszeilenfenster sollte das Ergebnis von Abfragen für Geräteobjekte mit Tags, Dienst- und Geräteeigenschaften angezeigt werden.

## Abfragestruktur (JSON)

Abfragen, die Geräte- und Diensteigenschaften betreffen, werden mit einer JSON-Zeichenfolge ausgeführt, die die Abfrage selbst darstellt. Die JSON-Zeichenfolge besteht aus 4 Teilen. Im Folgenden finden Sie eine Erläuterung der einzelnen Teile und des C#-Objekts, das in die richtige JSON-Zeichenfolge serialisiert werden kann.

- **Project**: der Ausdruck, der die Felder aus dem Geräteobjekt angibt, die in das Abfrageresultset einbezogen werden (äquivalent zu SELECT in SQL):

	```
	var projectQuery = {
	    "project": {
	        "all": false,
	        "properties": [{
	            "name": "CustomerId",
	            "type": "service"
	        }, {
	            "name": "Weight",
	            "type": "service"
	        }]
	    }
	}
	```

- **Filter**: der Ausdruck, der die Geräteobjekte einschränkt, die in das Abfrageresultset einbezogen werden (äquivalent zu WHERE in SQL):

	```
	var filterQuery = {
	  filter: {
	    property: {
	      name: "CustomerId",
	      type: "service"
	    },
	    value: "123456",
	    comparisonOperator: "eq",
	    type: "comparison"
	  },
	  project: null,
	  aggregate: null,
	  sort: null
	};
	```

- **Aggregate**: der Ausdruck, der die Gruppierung des Abfrageresultsets festlegt (äquivalent zu GROUPBY in SQL):

	```
	var aggregateQuery = {
	filter: null,
	project: null,
	aggregate: {
	keys: [
	  {
	    name: "CustomerId",
	    type: "service"
	  }
	],
	properties: [
	  {
	    operator: "sum",
	    property: {
	      name: "Weight",
	      type: "service"
	    },
	    columnName: "TotalWeight"
	  }
	]
	},
	sort: null
	};
	```

- **Sort**: die Ausdrucksdefinition, welche Eigenschaft zum Sortieren des Abfrageresultsets verwendet werden sollte (äquivalent zu ORDER BY in SQL). Wenn „Sort“ den Wert NULL hat, wird **deviceID** standardmäßig verwendet:

```
  var sortQuery = {
  filter: null,
  project: null,
  aggregate: null,
  sort: [
      {
          order: "asc",
          property: {
              name: "QoS",
              type: "service"
          }
      }
  ]
  };
```

### Einschränkungen

Es gibt einige Einschränkungen der Implementierung von Abfragen in die öffentliche Vorschau.

-   Es erfolgt keine Überprüfung des Abfrageausdrucks.

-   Bei Abfragen wird die Groß-/Kleinschreibung unterschieden.

-   Bei der Verwendung von Abfrageausdrücken bei Abfragen nach Dienst- oder Geräteeigenschaften werden nur 100 Geräte zurückgegeben. Ein Beispiel zum Implementieren des Auslagerns ist in [unserer Abfragebibliothek][lnk-query-samples] verfügbar.

Ausführlichere Informationen über Syntax und verfügbare Felder für JSON sind [verfügbar][lnk-query-expression-guide]. Außerdem finden Sie Beispielabfragen in unserer [Bibliothek mit Abfrageausdrücken][lnk-query-samples].

### Abfrage nach Geräte- und Diensteigenschaften

Sobald Sie den JSON-Abfrageausdruck haben, können Sie Abfragen nach Gerätezwillingen durchführen. Rufen Sie **queryDevices** auf, und überprüfen Sie das Objekt **result** auf Aggregatabfragen und das Objekt **devices** auf alle anderen Abfragen.

```
registry.queryDevices(query, done)
```

### Abfrage nach Tags

Mit Abfragen nach Tags können Sie Geräteobjekte suchen, ohne einen JSON-Abfrageausdruck zu verwenden. Wenn mehrere Tags übergeben werden, werden nur Geräteobjekte mit allen Tags zurückgegeben. Der zweite Parameter ist **maxCount**, die maximale Anzahl von Geräten, die zurückgegeben werden. Der maximale Wert für **maxCount** ist 1.000.

```
registry.queryDevicesByTags(['bacon'], 100, done)
```

### Geräteimplementierung

Die Abfrage wird durch die Clientbibliothek der Azure IoT Hub-Geräteverwaltung aktiviert. Solange die Geräteeigenschaften synchronisiert werden (wie in [Tutorial: Verwenden des Gerätezwillings][lnk-twin-tutorial]) beschrieben, können Sie sie abfragen. Geräteeigenschaften sind nur verfügbar, nachdem das physische Gerät eine Verbindung mit IoT Hub hergestellt hat und Anfangswerte bereitstellt.

## Nächste Schritte

Weitere Informationen zu den Geräteverwaltungsfeatures von Azure IoT Hub erhalten Sie in den folgenden Tutorials:

- [Verwenden des Gerätezwillings][lnk-twin-tutorial]
- [Aktualisieren der Gerätefirmware mithilfe von Geräteaufträgen][lnk-jobs-tutorial]



<!-- images and links -->
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twin-tutorial]: iot-hub-device-management-device-twin.md
[lnk-jobs-tutorial]: iot-hub-device-management-device-jobs.md
[lnk-query-spec]: https://github.com/Azure/azure-iot-sdks/blob/dmpreview/node/service/devdoc/query_expression_requirements.md
[lnk-query-samples]: https://github.com/Azure/azure-iot-sdks/blob/dmpreview/doc/get_started/dm_queries/query-samples.md
[lnk-query-expression-guide]: https://github.com/Azure/azure-iot-sdks/blob/dmpreview/node/service/devdoc/query_expression_requirements.md

<!---HONumber=AcomDC_0518_2016-->