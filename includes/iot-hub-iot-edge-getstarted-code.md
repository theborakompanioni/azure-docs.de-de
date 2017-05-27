## <a name="typical-output"></a>Typische Ausgabe

Im Anschluss sehen Sie ein Beispiel für die Ausgabe, die vom Hello World-Beispiel in die Protokolldatei geschrieben wird. Die Ausgabe ist für bessere Lesbarkeit formatiert:

```json
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Codeausschnitte

In diesem Abschnitt werden einige wichtige Teile des Codes im hello\_world-Beispiel beschrieben.

### <a name="iot-edge-gateway-creation"></a>IoT Edge-Gatewayerstellung

Der Entwickler muss den *Gatewayprozess*schreiben. Dieses Programm erstellt die interne Infrastruktur (den Broker), lädt die IoT Edge-Module und richtet alles ordnungsgemäß ein. IoT Edge stellt die Funktion **Gateway\_Create\_From\_JSON** bereit, mit der Sie einen Bootstrapvorgang für ein Gateway auf der Grundlage einer JSON-Datei ausführen können. Bei der Verwendung der Funktion **Gateway\_Create\_From\_JSON** müssen Sie ihr den Pfad zu einer JSON-Datei übergeben, die die zu ladenden IoT Edge-Module angibt.

Sie finden den Code für den Gatewayprozess im Hello World-Beispiel in der [main.c][lnk-main-c]-Datei. Zur besseren Lesbarkeit zeigt der folgende Ausschnitt eine verkürzte Version des Codes für den Gatewayprozess. Dieses Beispielprogramm erstellt ein Gateway und wartet dann darauf, dass der Benutzer die **EINGABETASTE** drückt, woraufhin es das Gateway entfernt.

```c
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

Die JSON-Einstellungsdatei enthält eine Liste zu ladender IoT Edge-Module und die Verknüpfungen zwischen den Modulen. Jedes IoT Edge-Modul muss Folgendes angeben:

* **Name:** Ein eindeutiger Name für das Modul.
* **Ladeprogramm:** Ein Ladeprogramm, das das gewünschte Modul laden kann. Ladeprogramme sind ein Erweiterungspunkt zum Laden von verschiedenen Modultypen. Wir stellen Ladeprogramme für die Verwendung mit Modulen bereit, die in nativem C, Node.js, Java und .Net geschrieben wurden. Das Hello World-Beispiel verwendet nur das native C-Ladeprogramm, da es sich bei allen Modulen in diesem Beispiel um dynamische, in C geschriebene Bibliotheken handelt. Weitere Informationen zur Verwendung von IoT Edge-Modulen in anderen Sprachen finden Sie in den Beispielen für [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample) und [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample).
    * **Name**: Der Name des Ladeprogramms, der für das Laden des Moduls verwendet wurde
    * **Einstiegspunkt:** Der Pfad zu der Bibliothek, die das Modul enthält Unter Linux ist diese Bibliothek eine SO-Datei, unter Windows eine DLL-Datei. Abhängig von der Art des verwendeten Ladeprogramms wird jeweils ein spezifischer Einstiegspunkt verwendet. Der Einstiegspunkt für das Node.js-Ladeprogramm ist eine JS-Datei. Der Einstiegspunkt des Java-Ladeprogramms ist eine Kombination aus Klassenpfad und Klassenname. Der Einstiegspunkt des .NET-Ladeprogramms ist eine Kombination aus Assembly- und Klassenname.

* **args**: alle Konfigurationsinformationen, die das Modul benötigt.

Der folgende Code zeigt den JSON-Code, der verwendet wurde, um alle IoT Edge-Module für das Hello World-Beispiel unter Linux zu deklarieren. Ob ein Modul ein Argument erfordert, hängt vom Modulentwurf ab. In diesem Beispiel akzeptiert das Protokollierungsmodul ein Argument (den Pfad zur Ausgabedatei), und das hello\_world-Modul verfügt über keine Argumente.

```json
"modules" :
[
    {
        "name" : "logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/logger/liblogger.so"
        }
        },
        "args" : {"filename":"log.txt"}
    },
    {
        "name" : "hello_world",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/hello_world/libhello_world.so"
        }
        },
        "args" : null
    }
]
```

Die JSON-Datei enthält auch die Verknüpfungen zwischen den Modulen, die an den Broker übergeben werden. Ein Link besitzt zwei Eigenschaften:

* **source:** Ein Modulname aus dem Abschnitt `modules` (oder „\*“).
* **sink:** Ein Modulname aus dem Abschnitt `modules`.

Jeder Link definiert eine Nachrichtenroute und eine Richtung. Nachrichten aus dem Modul `source` werden an das Modul `sink` übermittelt. `source` kann auf „\*“ festgelegt werden, um anzugeben, dass `sink` Nachrichten von jedem beliebigen Modul empfängt.

Der folgende Code zeigt den JSON-Code zum Konfigurieren der Verknüpfungen zwischen den Modulen, die im hello\_world-Beispiel unter Linux verwendet wurden. Jede vom Modul `hello_world` erzeugte Nachricht wird vom Modul `logger` verarbeitet.

```json
"links":
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="helloworld-module-message-publishing"></a>Veröffentlichen von Nachrichten durch das hello\_world-Modul

Den Code, der vom hello\_world-Modul zum Veröffentlichen von Nachrichten verwendet wird, finden Sie in der Datei [hello_world.c][lnk-helloworld-c]. Der folgende Codeausschnitt zeigt eine modifizierte Version des Codes mit zusätzlichen Kommentaren und entferntem Fehlerbehandlungscode zur besseren Lesbarkeit:

```c
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);

    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;

    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="helloworld-module-message-processing"></a>Nachrichtenverarbeitung im hello\_world-Modul

Das hello\_world-Modul verarbeitet keine Nachrichten, die andere IoT Edge-Module für den Broker veröffentlichen. Daher ist die Implementierung des Nachrichtenrückrufs im hello\_world-Modul eine no-op-Funktion.

```c
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Veröffentlichen und Verarbeiten von Nachrichten im Protokollierungsmodul

Das Protokollierungsmodul empfängt Nachrichten vom Broker und schreibt sie in eine Datei. Es veröffentlicht keine Nachrichten. Aus diesem Grund ruft der Code des Protokollierungsmoduls nie die Funktion **Broker_Publish** auf.

Die Funktion **Logger_Receive** in der Datei [logger.c][lnk-logger-c] ist der Rückruf, den der Broker aufruft, um Nachrichten an das Protokollierungsmodul zu übermitteln. Der folgende Codeausschnitt zeigt eine modifizierte Version mit zusätzlichen Kommentaren und entferntem Fehlerbehandlungscode zur besseren Lesbarkeit:

```c
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Azure IoT Edge finden Sie in den folgenden Artikeln:

* [IoT Edge – Senden von D2C-Nachrichten mit einem simulierten Gerät unter Linux][lnk-gateway-simulated].
* [Azure IoT Edge][lnk-iot-edge] auf GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/iot-edge/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/iot-edge/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/iot-edge/blob/master/modules/logger/src/logger.c
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md