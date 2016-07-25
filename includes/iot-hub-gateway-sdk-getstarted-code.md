## Typische Ausgabe

Hier sehen Sie ein Beispiel für die Ausgabe, die vom Hello World-Beispiel in die Protokolldatei geschrieben wird. Zur besseren Lesbarkeit wurden Zeilenvorschub- und Tabulatorzeichen eingefügt:

```
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

## Codeausschnitte

In diesem Abschnitt werden einige wichtige Teile des Codes im Hello World-Beispiel beschrieben.

### Gatewayerstellung

Der Entwickler muss den *Gatewayprozess* schreiben. Dieses Programm erstellt die interne Infrastruktur (den Nachrichtenbus), lädt die Module und richtet alle Elemente für eine ordnungsgemäße Funktionsweise ein. Das SDK stellt die Funktion **Gateway\_Create\_From\_JSON** bereit, mit der Sie ein Bootstrapping eines Gateways aus einer JSON-Datei durchführen können. Um die **Gateway\_Create\_From\_JSON**-Funktion zu verwenden, müssen Sie sie an den Pfad zu einer JSON-Datei übergeben, die die zu ladenden Module angibt.

Sie finden die Code für den Gatewayprozess im Hello World-Beispiel in der [main.c][lnk-main-c]-Datei. Zur besseren Lesbarkeit zeigt der unten stehende Codeausschnitt eine verkürzte Version des Codes für den Gatewayprozess. Dieses Programm erstellt ein Gateway und wartet dann darauf, dass der Benutzer die **EINGABETASTE** drückt, bevor es das Gateway entfernt.

```
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

Die JSON-Einstellungsdatei enthält eine Liste der zu ladenden Module. Jedes Modul muss Folgendes angeben:

- **module\_name**: ein eindeutiger Name für das Modul.
- **module\_path**: der Pfad zur Bibliothek, die das Modul enthält. Bei Linux ist dies eine SO-Datei, bei Windows eine DLL-Datei.
- **args**: alle Konfigurationsinformationen, die das Modul benötigt.

Das folgende Beispiel zeigt die JSON-Einstellungsdatei, die zum Konfigurieren des Hello World-Beispiels unter Linux verwendet wird. Ob ein Modul ein Argument erfordert, hängt vom Entwurf des Moduls ab. In diesem Beispiel akzeptiert das Protokollierungsmodul ein Argument, das den Pfad zur Ausgabedatei darstellt, und das Hello World-Beispiel akzeptiert keine Argumente:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger_hl",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "helloworld",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
			"args" : null
        }
    ]
}
```

### Veröffentlichen von Nachrichten durch das Hello World-Modul

Sie finden den Code, der vom Hello World-Modul zum Veröffentlichen von Nachrichten verwendet wird, in der Datei [hello\_world.c][lnk-helloworld-c]. Der Codeausschnitt unten zeigt eine verbesserte Version, aus dem zur besseren Lesbarkeit Fehlerbehandlungscode und zusätzliche Kommentare entfernt wurden.

```
int helloWorldThread(void *param)
{
    // Create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // Add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // Set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // Set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // Create a message based on the msgConfig structure
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
            // Publish the message to the bus
            (void)MessageBus_Publish(handleData->busHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### Nachrichtenverarbeitung im Hello World-Modul

Das Hello World-Modul muss niemals Nachrichten verarbeiten, die von anderen Modulen im Nachrichtenbus veröffentlicht werden. Daher ist die Implementierung des Nachrichtenrückrufs im Hello World-Modul eine no-op-Funktion.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### Veröffentlichen und Verarbeiten von Nachrichten im Protokollierungsmodul

Das Protokollierungsmodul empfängt Nachrichten aus dem Nachrichtenbus und schreibt sie in eine Datei. Es veröffentlicht niemals Nachrichten an den Nachrichtenbus. Aus diesem Grund ruft der Code des Protokollierungsmoduls nie die Funktion **MessageBus\_Publish** auf.

Die Funktion **Logger\_Recieve** in der Datei [logger.c][lnk-logger-c] ist der Rückruf, den der Nachrichtenbus aufruft, um Nachrichten an das Protokollierungsmodul zuzustellen. Der Codeausschnitt unten zeigt eine verbesserte Version, aus dem zur besseren Lesbarkeit Fehlerbehandlungscode und zusätzliche Kommentare entfernt wurden.

```
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
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{"time":"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "","properties":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ","content":"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, ""}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## Nächste Schritte

Weitere Informationen zur Verwendung des Gateway SDK finden Sie unter folgenden Links:

- [IoT Gateway SDK – Senden von D2C-Nachrichten mit einem simulierten Gerät unter Linux][lnk-gateway-simulated].
- [Azure IoT Gateway SDK][lnk-gateway-sdk] auf GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md

<!---HONumber=AcomDC_0713_2016-->