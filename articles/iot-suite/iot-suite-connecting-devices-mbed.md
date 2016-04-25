<properties
   pageTitle="Verbinden eines Geräts mit C unter mbed | Microsoft Azure"
   description="Beschreibt, wie Sie ein Gerät mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite verbinden, indem Sie eine in C geschriebene Anwendung auf einem mbed-Gerät ausführen."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/05/2016"
   ms.author="dobett"/>


# Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung von IoT Suite

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## Erstellen und Ausführen der C-Beispielprojektmappe unter mbed

Im Folgenden werden die Schritte zum Verbinden eines [mbed-fähigen Freescale FRDM-K64F-Geräts][lnk-mbed-home] mit der Remoteüberwachungslösung beschrieben.

### Verbinden des Geräts mit Ihrem Netzwerk und Desktopcomputer

1. Verbinden Sie das mbed-Gerät mithilfe eines Ethernet-Kabels mit Ihrem Netzwerk. Dieser Schritt ist erforderlich, da die Beispielanwendung Zugriff auf das Internet benötigt.

2. Informationen zum Verbinden des mbed-Geräts mit Ihrem Desktop-PC finden Sie unter [Erste Schritte mit mbed][lnk-mbed-getstarted].

3. Wenn auf Ihrem Desktop-PC Windows ausgeführt wird, finden Sie unter [PC-Konfiguration][lnk-mbed-pcconnect] weitere Informationen zum Konfigurieren des mbed-Gerätezugriffs über den seriellen Anschluss.

### Erstellen eines mbed-Projekts und Importieren des Beispielcodes

1. Wechseln Sie in Ihrem Webbrowser zur [Entwicklerwebsite](https://developer.mbed.org/) von mbed.org. Wenn Sie sich noch nicht registriert haben, können Sie die Option zum Erstellen eines neuen Kontos verwenden (kostenlos). Melden Sie sich andernfalls mit Ihren Anmeldeinformationen an. Klicken Sie anschließend rechts oben auf der Seite auf **Compiler**. Sie gelangen auf die Benutzeroberfläche für die Arbeitsbereichsverwaltung.

2. Stellen Sie sicher, dass die von Ihnen verwendete Hardwareplattform in der oberen rechten Ecke des Fensters angezeigt wird, oder klicken Sie auf das Symbol in der rechten Ecke, um Ihre Hardwareplattform auszuwählen.

3. Klicken Sie im Hauptmenü auf **Importieren**. Klicken Sie anschließend auf **Hier klicken**, um den Import über den URL-Link neben dem mbed-Logo mit dem Globus durchzuführen.

    ![][6]

4. Geben Sie im Popupfenster den Link für den Beispielcode ein (https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/), und klicken Sie dann auf **Importieren**.

    ![][7]

5. Im Fenster des mbed-Compilers sehen Sie, dass der Import dieses Projekts verschiedene Bibliotheken umfasst. Einige werden vom Azure IoT-Team verwaltet ([azureiot\_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub\_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub\_amqp\_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure\_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), und bei anderen handelt es sich um Bibliotheken von Drittanbietern, die im Katalog mit den mbed-Bibliotheken verfügbar sind.

    ![][8]

6. Öffnen Sie die Datei „remote\_monitoring\\remote\_monitoring.c“, und suchen Sie in der Datei nach dem folgenden Code:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. Ersetzen Sie [Device Id] und [Device Key] durch Ihre Gerätedaten, damit das Beispielprogramm eine Verbindung mit Ihrem IoT Hub herstellen kann. Verwenden Sie den Wert für „IoT Hub Hostname“, um die Platzhalter [IoTHub Name] und [IoTHub Suffix, d. h. „azure-devices.net“] zu ersetzen. Wenn der IoT Hub-Hostname also beispielsweise „contoso.azure-devices.net“ lautet, ist „Contoso“ der **hubName**, und der restliche Text ist das **hubSuffix**.

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### Einführung in den Code

Wenn Sie daran interessiert sind, wie das Programm funktioniert, können Sie sich diesen Abschnitt durchlesen, in dem einige wichtige Teile des Beispielcodes beschrieben werden. Falls Sie nur den Code ausführen möchten, können Sie mit [Erstellen und Ausführen des Programms](#buildandrun) fortfahren.

#### Definieren des Modells

In diesem Beispiel wird die Serialisierungsbibliothek ([serializer][lnk-serializer]) zum Definieren eines Modells verwendet, mit dem die Nachrichten angegeben werden, die vom Gerät an den IoT Hub gesendet und vom IoT Hub empfangen werden können. In diesem Beispiel definiert der **Contoso**-Namespace das Modell **Thermostat**, mit dem die Telemetriedaten **Temperature** (Temperatur), **ExternalTemperature** (Außentemperatur) und **Humidity** (Luftfeuchtigkeit) sowie Metadaten angegeben werden, z.B. die Geräte-ID, Geräteeigenschaften und die Befehle, auf die das Gerät reagiert:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
);

DECLARE_STRUCT(DeviceProperties,
ascii_char_ptr, DeviceID,
_Bool, HubEnabledState
);

DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
);

END_NAMESPACE(Contoso);
```

Zur Modelldefinition gehören die Definitionen für die Befehle **SetTemperature** und **SetHumidity**, auf die das Gerät reagiert:

```
EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
{
    (void)printf("Received temperature %d\r\n", temperature);
    thermostat->Temperature = temperature;
    return EXECUTE_COMMAND_SUCCESS;
}

EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
{
    (void)printf("Received humidity %d\r\n", humidity);
    thermostat->Humidity = humidity;
    return EXECUTE_COMMAND_SUCCESS;
}
```

#### Herstellen einer Verbindung zwischen Modell und Bibliothek

Die Funktionen **sendMessage** und **IoTHubMessage** sind Codebausteine zum Senden von Telemetriedaten vom Gerät und Verbinden von Nachrichten vom IoT Hub mit den Befehlshandlern.

#### Funktion „remote\_monitoring\_run“

Die Funktion **main** des Programms ruft die Funktion **remote\_monitoring\_run** auf, wenn die Anwendung gestartet wird, um das Verhalten des Geräts als IoT Hub-Geräteclient auszuführen. Die Funktion **remote\_monitoring\_run** besteht hauptsächlich aus verschachtelten Funktionspaaren:

- Mit **platform\_init** und **platform\_deinit** werden plattformspezifische Vorgänge für die Initialisierung und zum Herunterfahren durchgeführt.
- Mit **serializer\_init** und **serializer\_deinit** wird die Serialisierungsbibliothek initialisiert und deren Initialisierung aufgehoben.
- Mit **IoTHubClient\_Create** und **IoTHubClient\_Destroy** wird ein Clienthandle (**iotHubClientHandle**) erstellt, indem die Geräteanmeldeinformationen zum Herstellen der Verbindung mit Ihrem IoT Hub verwendet werden.

Im Hauptteil der Funktion **remote\_monitoring\_run** führt das Programm die folgenden Vorgänge mit dem Handle **iotHubClientHandle** durch:

- Erstellt eine Instanz des Contoso-Thermostat-Modells und richtet die Nachrichtenrückrufe für die beiden Befehle ein.
- Sendet über die Serialisierungsbibliothek Informationen zum Gerät selbst, einschließlich der unterstützten Befehle, an Ihren IoT Hub. Wenn der Hub diese Nachricht empfängt, wird der Gerätestatus im Dashboard von **Ausstehend** in **Wird ausgeführt** geändert.
- Startet eine **while**-Schleife, mit der Werte zu Temperatur, Außentemperatur und Luftfeuchtigkeit jede Sekunde an den IoT Hub gesendet werden.

Zu Referenzzwecken finden Sie nachstehend ein Beispiel für die **DeviceInfo**-Nachricht, die beim Start an IoT Hub gesendet wurde:

```
{
  "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties":
  {
    "DeviceID":"mydevice01", "HubEnabledState":true
  }, 
  "Commands":
  [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

Zu Referenzzwecken finden Sie nachstehend ein Beispiel für die **Telemetry**-Nachricht, die an IoT Hub gesendet wurde:

```
{"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
```

Zu Referenzzwecken finden Sie nachstehend ein Beispiel für einen **Befehl**, der vom IoT Hub empfangen wurde:

```
{
  "Name":"SetHumidity",
  "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
  "CreatedTime":"2016-03-11T15:09:44.2231295Z",
  "Parameters":{"humidity":23}
}
```

<a id="buildandrun"/>
### Erstellen und Ausführen des Programms

1. Klicken Sie auf **Kompilieren**, um das Programm zu erstellen. Sie können etwaige Warnungen ignorieren, aber wenn während der Erstellung Fehler auftreten, sollten Sie diese vor dem Fortfahren beheben.

2. Wenn die Erstellung erfolgreich ist, generiert die mbed-Compilerwebsite eine BIN-Datei mit dem Namen Ihres Projekts und lädt sie auf Ihren lokalen Computer herunter. Kopieren Sie die BIN-Datei auf das Gerät. Durch das Speichern der BIN-Datei auf dem Gerät wird dieses neu gestartet, und das in der BIN-Datei enthaltene Programm wird ausgeführt. Sie können das Programm zu einem beliebigen Zeitpunkt starten, indem Sie die Schaltfläche zum Zurücksetzen am mbed-Gerät drücken.

3. Stellen Sie mit einer SSH-Clientanwendung eine Verbindung mit dem Gerät her, z. B. PuTTY. Sie können ermitteln, welcher serielle Port von Ihrem Gerät verwendet wird, indem Sie dies im Windows-Geräte-Manager überprüfen:

    ![][11]

4. Klicken Sie in PuTTY auf den Verbindungstyp **Seriell**. Das Gerät stellt i.d.R. eine Verbindung mit 115.200 Baud her. Geben Sie also in das Feld **Geschwindigkeit** den Wert „115200“ ein. Klicken Sie anschließend auf **Öffnen**.

5. Das Programm wird gestartet. Unter Umständen müssen Sie das Board zurücksetzen (drücken Sie STRG+UNTBR, oder drücken Sie die Reset-Taste des Boards), falls das Programm nach dem Herstellen der Verbindung nicht automatisch gestartet wird.

    ![][10]

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
[lnk-serializer]: https://azure.microsoft.com/documentation/articles/iot-hub-device-sdk-c-intro/#serializer

<!---HONumber=AcomDC_0413_2016-->