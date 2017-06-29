## <a name="create-a-device-identity"></a>Erstellen einer Geräteidentität

In diesem Abschnitt verwenden Sie ein Node.js-Tool mit dem Namen [iothub-explorer][iot-hub-explorer] zum Erstellen einer Geräteidentität für dieses Tutorial. Bei Geräte-IDs wird die Groß-/Kleinschreibung beachtet.

1. Führen Sie in der Befehlszeilenumgebung Folgendes aus:

    `npm install -g iothub-explorer@latest`

1. Führen Sie anschließend den folgenden Befehl aus, um sich bei Ihrem Hub anzumelden. Ersetzen Sie `{iot hub connection string}` durch die zuvor kopierte IoT Hub-Verbindungszeichenfolge:

    `iothub-explorer login "{iot hub connection string}"`

1. Erstellen Sie abschließend eine neue Geräteidentität mit dem Namen `myDeviceId`, indem Sie den folgenden Befehl verwenden:

    `iothub-explorer create myDeviceId --connection-string`

Notieren Sie sich die Verbindungszeichenfolge für das Gerät aus dem Ergebnis. Diese Geräteverbindungszeichenfolge wird von der Geräte-App verwendet, um die Verbindung mit Ihrem IoT Hub als Gerät herzustellen.

![][img-identity]

Informationen zum programmgesteuerten Erstellen von Geräteidentitäten finden Sie unter [Erste Schritte mit Azure IoT Hub][lnk-getstarted].

<!-- images and links -->
[img-identity]: media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
