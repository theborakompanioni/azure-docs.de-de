## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Neu** > **Internet der Dinge** > **IoT Hub**.

   ![Erstellen eines IoT Hubs im Azure-Portal](../articles/iot-hub/media/iot-hub-create-hub-and-device/1_create-azure-iot-hub-portal.png)
1. Geben Sie im Bereich **IoT Hub** die folgenden Informationen für Ihren IoT Hub ein:

   **Name**: Der Name Ihres IoT Hubs. Wenn der eingegebene Name gültig ist, wird ein grünes Häkchen angezeigt.

   **Tarif und Skalierung**: Wählen Sie den kostenlosen Tarif F1 aus. Diese Option ist für diese Demo ausreichend. Siehe [Preis- und Skalierung](https://azure.microsoft.com/pricing/details/iot-hub/).

   **Ressourcengruppe**: Erstellen Sie eine Ressourcengruppe zum Hosten des IoT Hubs, oder verwenden Sie eine vorhandene. Siehe [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../articles/azure-resource-manager/resource-group-portal.md).

   **Standort**: Wählen Sie den nächstgelegenen Standort aus, an dem der IoT Hub erstellt wird.

   **An Dashboard anheften**: Aktivieren Sie diese Option für den leichteren Zugriff auf Ihren IoT Hub über das Dashboard.

   ![Ausfüllen der Felder zum Erstellen des Azure IoT Hubs](../articles/iot-hub/media/iot-hub-create-hub-and-device/2_fill-in-fields-for-azure-iot-hub-portal.png)

1. Klicken Sie auf **Erstellen**. Die Erstellung Ihres IoT Hubs kann einige Minuten dauern. Sie sehen den Fortschritt im Bereich **Benachrichtigungen**.

   ![Anzeigen von Benachrichtigungen zum Status der IoT Hub-Erstellung](../articles/iot-hub/media/iot-hub-create-hub-and-device/3_notification-azure-iot-hub-creation-progress-portal.png)

1. Nach dem Erstellen Ihres IoT Hubs klicken Sie im Dashboard darauf. Notieren Sie sich den **Hostnamen**, und klicken Sie anschließend auf **Richtlinien für gemeinsamen Zugriff**.

   ![Abrufen des Hostnamens Ihres IoT Hubs](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

1. Klicken Sie im Bereich **Freigegebene Zugriffsrichtlinien** auf die Richtlinie **iothubowner**, und kopieren und notieren Sie dann die **Verbindungszeichenfolge** Ihres IoT Hubs. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf IoT Hub](../articles/iot-hub/iot-hub-devguide-security.md).

   ![Abrufen der IoT Hub-Verbindungszeichenfolge](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-the-iot-hub-for-the-your-device"></a>Registrieren eines Geräts im IoT Hub für Ihr Gerät

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) Ihren IoT-Hub.
1. Klicken Sie auf **Geräte-Explorer**.
1. Klicken Sie im Geräte-Explorer-Bereich auf **Hinzufügen**, um Ihrem IoT Hub ein Gerät hinzuzufügen.

   **Geräte-ID**: Die ID des neuen Geräts.

   **Authentifizierungstyp**: Wählen Sie die Option **Symmetrischer Schlüssel**.

   **Auto Generate Keys** (Schlüssel automatisch generieren): Aktivieren Sie dieses Feld.

   **Connect device to IoT Hub** (Gerät mit IoT Hub verbinden): Klicken Sie auf **Aktivieren**.

   ![Hinzufügen eines Geräts im Geräte-Explorer Ihres IoT Hubs](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-device-explorer-portal.png)

1. Klicken Sie auf **Speichern**.
1. Öffnen Sie das Gerät nach der Erstellung im Bereich **Geräte-Explorer**.
1. Notieren Sie sich den Primärschlüssel der Verbindungszeichenfolge.

   ![Abrufen der Geräte-Verbindungszeichenfolge](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_get-device-connection-string-in-device-explorer-portal.png)