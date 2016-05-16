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

# Tutorial: Suchen nach Gerätezwillingen mithilfe von Abfragen (Vorschau)

Mit der Azure IoT-Geräteverwaltung können Sie Gerätezwillinge – Dienstdarstellungen eines physischen Geräts – mit Abfragen suchen. Sie können Abfragen basierend auf Geräteeigenschaften, Diensteigenschaften oder Tags im Gerätezwilling durchführen. Sie können Tags und Eigenschaften zur Abfrage verwenden:

-   Bei der Abfrage nach Gerätezwillingen mit Tags übergeben Sie ein Zeichenfolgenarray, und die Abfrage gibt den Satz von Geräten zurück, die mit allen diesen Zeichenfolgen gekennzeichnet sind.

-   Bei der Abfrage nach Gerätezwillingen mit Dienst- oder Geräteeigenschaften verwenden Sie einen JSON-Abfrageausdruck.

Weitere Informationen zu Gerätezwillingen und Abfragen finden Sie unter [Überblick über die Azure IoT Hub-Geräteverwaltung][lnk-dm-overview].

## Ausführen des Beispiels einer Geräteabfrage

Das folgende Beispiel legt die Funktionalität des Tutorials [Get started with Azure IoT Hub device management using C# (preview)][lnk-get-started] (Erste Schritte mit der Azure IoT Hub-Geräteverwaltung mit C# [Vorschau]) dar. Auf der Basis, dass die verschiedenen simulierten Geräte ausgeführt werden, wird eine Abfrage durchgeführt, um bestimmte Geräte zu suchen.

### Voraussetzungen 

Vor dem Ausführen dieses Beispiels müssen Sie die Schritte in [Get started with Azure IoT Hub device management using C# (preview)][lnk-get-started] (Erste Schritte mit der Azure IoT Hub-Geräteverwaltung mit C# [Vorschau]) abgeschlossen haben. Das bedeutet, dass Ihre simulierten Geräte ausgeführt werden müssen. Wenn Sie den Vorgang zuvor abgeschlossen haben, starten Sie Ihre simulierten Geräte jetzt neu.

### Starten des Beispiels

Um das Beispiel zu starten, müssen Sie den Prozess **Query.exe** ausführen. Damit werden ein paar unterschiedliche Abfragen ausgeführt. Folgen Sie den Schritten unten, um das Beispiel zu starten:

1.  Die simulierten Geräte sollten zunächst mindestens 1 Minute lang ausgeführt werden. So wird sichergestellt, dass die Geräteeigenschaften im Zwilling mit dem physischen Gerät synchronisiert werden. Weitere Informationen zur Synchronisierung finden Sie unter [Tutorial: Verwenden des Gerätezwillings][lnk-twin-tutorial]

2.  Navigieren Sie aus dem Stammordner, in dem Sie das Repository **azure-iot-sdks** geklont haben, zum Ordner **azure-iot-sdks\\csharp\\service\\samples\\bin**.

3.  Führen Sie `Query.exe <IoT Hub Connection String>` aus.

Im Befehlszeilenfenster sollte das Ergebnis von Abfragen für Geräteobjekte mit Tags, Dienst- und Geräteeigenschaften angezeigt werden.

## Abfragestruktur (JSON)

Abfragen, die Geräte- und Diensteigenschaften betreffen, werden mit einer JSON-Zeichenfolge ausgeführt, die die Abfrage selbst darstellt. Die JSON-Zeichenfolge besteht aus 4 Teilen. Im Folgenden finden Sie eine Erläuterung der einzelnen Teile und des C#-Objekts, das in die richtige JSON-Zeichenfolge serialisiert werden kann.

- **Project**: der Ausdruck, der die Felder aus dem Geräteobjekt angibt, die in das Abfrageresultset einbezogen werden (äquivalent zu SELECT in SQL):

  ```
  var query = JsonConvert.SerializeObject(
      project = new
      {
        all = false,
        properties = new []
        {
          new
          {
            name = "CustomerId",
            type = "service"
          },
          new
          {
            name = "Weight",
            type = "service"
          }
        }
      }
  );
```

- **Filter**: der Ausdruck, der die Geräteobjekte einschränkt, die in das Abfrageresultset einbezogen werden (äquivalent zu WHERE in SQL):

  ```
  var query = JsonConvert.SerializeObject(
      new
      {
        filter = new
        {
          property = new
          {
            name = "CustomerId",
            type = "service"
          },
          value = "123456",
          comparisonOperator = "eq",
          type = "comparison"
        }
      }
  );
  ```

- **Aggregate**: der Ausdruck, der die Gruppierung des Abfrageresultsets festlegt (äquivalent zu GROUPBY in SQL):

  ```
  var query = JsonConvert.SerializeObject(
      new
      {
      filter = new
        {
          property = new
          {
            name = "CustomerId",
            type = "service"
          },
          value = (string)null,
          comparisonOperator = "ne",
          type = "comparison"
        },
        aggregate = new
        {
          keys = new[]
          {
            new
            {
              name = "CustomerId",
              type = "service"
            }
          },
          properties = new[]
          {
            new
            {
              @operator = "avg",
              property = new
              {
                name = "Weight",
                type = "service"
              },
              columnName = "TotalWeight"
            }
          }
        }
      }
  );
  ```

- **Sort**: die Ausdrucksdefinition, welche Eigenschaft zum Sortieren des Abfrageresultsets verwendet werden sollte (äquivalent zu ORDER BY in SQL). Wenn „Sort“ den Wert NULL hat, wird **deviceID** standardmäßig verwendet:

  ```
  var query = JsonConvert.SerializeObject(
    new
    {
      sort = new[]
      {
        new
        {
          property = new
          {
            name = "QoS",
            type = "service"
          },
          order = "asc"
        }
      }
    }
  );
  ```

### Einschränkungen

Es gibt einige Einschränkungen der Implementierung von Abfragen in die öffentliche Vorschau.

-   Es erfolgt keine Überprüfung des Abfrageausdrucks.

-   Bei Abfragen wird die Groß-/Kleinschreibung unterschieden.

-   Bei der Verwendung von Abfrageausdrücken bei Abfragen nach Dienst- oder Geräteeigenschaften werden nur 100 Geräte zurückgegeben. Ein Beispiel zum Implementieren des Auslagerns ist in [unserer Abfragebibliothek][lnk-query-samples] verfügbar.

Ausführlichere Informationen über Syntax und verfügbare Felder für JSON sind [verfügbar][lnk-query-expression-guide]. Außerdem finden Sie Beispielabfragen in unserer [Abfrageausdrückebibliothek][lnk-query-samples].

### Abfrage nach Geräte- und Diensteigenschaften

Sobald Sie den JSON-Abfrageausdruck haben, können Sie Abfragen nach Gerätezwillingen durchführen. Rufen Sie **QueryDeviceJsonAsync** auf, und aktivieren Sie das Feld **AggregateResult** für Aggregatabfragen und das Feld **Result** für alle anderen Abfragen. **Result** enthält eine Liste von Geräteobjekten, die die Gerätezwillinge darstellen, die der Abfrage entsprechen. **AggregateResult** enthält ein Array von Wörterbüchern, von denen jedes die Ergebniszeile enthält.

Diese Abfragen werden in **Program.cs** des Projekts **Query** verwendet.

```
var foundDevices = (await registryManager.QueryDevicesJsonAsync(query)).Result;

var results = (await registryManager.QueryDevicesJsonAsync(query)).AggregateResult;
```

### Abfrage nach Tags

Mit Abfragen nach Tags können Sie Geräteobjekte suchen, ohne einen JSON-Abfrageausdruck zu verwenden. Wenn mehrere Tags übergeben werden, werden nur Geräteobjekte mit allen Tags zurückgegeben. Der zweite Parameter ist **maxCount**, die maximale Anzahl von Geräten, die zurückgegeben werden. Der maximale Wert für **maxCount** ist 1.000.

```
var foundDevices = await registryManager.QueryDevicesAsync(new[] { "bacon" }, 100);
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

<!---HONumber=AcomDC_0504_2016-->