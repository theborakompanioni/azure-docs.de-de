## <a name="create-a-device-identity"></a>Erstellen einer Geräteidentität

In diesem Abschnitt verwenden Sie das [Azure-Portal][lnk-azure-portal], um in der Identitätsregistrierung im IoT Hub eine Geräteidentität zu erstellen. Ein Gerät kann nur eine Verbindung mit IoT Hub herstellen, wenn in der Identitätsregistrierung ein Eintrag für dieses Gerät vorhanden ist. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch][lnk-devguide-identity] im Abschnitt zur Identitätsregistrierung. Mit dem **Device Explorer** im Portal können Sie eine eindeutige Geräte-ID und einen Schlüssel erstellen. Mit diesen Daten kann sich das Gerät beim Herstellen der Verbindung mit dem IoT Hub identifizieren. Bei Geräte-IDs wird die Groß-/Kleinschreibung beachtet.

1. Stellen Sie sicher, dass Sie am [Azure-Portal][lnk-azure-portal] angemeldet sind.

1. Klicken Sie in der Navigationsleiste auf **Alle Ressourcen**, und suchen Sie nach Ihrer IoT Hub-Ressource.

    ![Navigieren zum IoT Hub][img-find-iothub]

1. Klicken Sie bei geöffneter IoT Hub-Ressource auf das Tool **Device Explorer** und dann oben auf **Hinzufügen**. Geben Sie den Namen für Ihr neues Gerät an, z.B. **myDeviceId**, und klicken Sie auf **Speichern**.

    ![Erstellen der Geräteidentität im Portal][img-create-device]

   Es wird eine neue Geräteidentität für Ihren IoT Hub erstellt.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. Klicken Sie in der Geräteliste von **Device Explorer** auf das neu erstellte Gerät, und notieren Sie sich **Verbindungszeichenfolge und Primärschlüssel**. 

    ![Geräte-Verbindungszeichenfolge][img-connection-string]

> [!NOTE]
> Die Identitätsregistrierung in IoT Hub speichert nur Geräteidentitäten, um einen sicheren Zugriff auf den IoT-Hub zu ermöglichen. Sie speichert Geräte-IDs und Schlüssel, die als Sicherheitsanmeldeinformationen verwendet werden, sowie ein Aktiviert/Deaktiviert-Kennzeichen, mit dem Sie den Zugriff für ein einzelnes Gerät deaktivieren können. Wenn Ihre Anwendung das Speichern weiterer gerätespezifischer Metadaten erfordert, sollte dafür ein anwendungsspezifischer Speicher verwendet werden. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch][lnk-devguide-identity].

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

