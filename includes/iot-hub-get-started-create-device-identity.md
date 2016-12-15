## <a name="create-a-device-identity"></a>Erstellen einer Geräteidentität
In diesem Abschnitt verwenden Sie ein Node.js-Tool mit dem Namen [IoT Hub-Explorer][iot-hub-explorer] zum Erstellen einer Geräteidentität für dieses Tutorial.

1. Führen Sie in der Befehlszeilenumgebung Folgendes aus:
   
    npm install -g iothub-explorer@latest
2. Führen Sie anschließend den folgenden Befehl aus, um sich an Ihrem Hub anzumelden, und ersetzen Sie dabei `{iot hub connection string}` durch die zuvor kopierte IoT Hub-Verbindungszeichenfolge:
   
    iothub-explorer login "{iot hub connection string}"
3. Erstellen Sie abschließend eine neue Geräteidentität mit dem Namen `myDeviceId`, indem Sie den folgenden Befehl verwenden:
   
    iothub-explorer create myDeviceId --connection-string

Notieren Sie sich die Verbindungszeichenfolge für das Gerät aus dem Ergebnis. Diese Geräteverbindungszeichenfolge wird von der Geräte-App verwendet, um die Verbindung mit Ihrem IoT Hub als Gerät herzustellen.

![][img-identity]

Informationen zur programmgesteuerten Erstellung von Geräteidentitäten finden Sie unter [Erste Schritte mit Azure IoT Hub][lnk-getstarted].

<!-- images and links -->
[img-identity]: media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md


<!--HONumber=Dec16_HO1-->


