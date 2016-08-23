## Erstellen eines IoT Hubs

Erstellen Sie einen IoT Hub, mit dem Ihr simuliertes Gerät verbunden werden kann. Die folgenden Schritte veranschaulichen, wie Sie diese Aufgabe mit dem Azure-Portal ausführen.

1. Melden Sie sich auf dem [Azure-Portal][lnk-portal] an.

2. Klicken Sie in der Navigationsleiste auf **Neu** > **Internet der Dinge (IoT)** > **Azure IoT Hub**.

    ![Navigationsleiste im Azure-Portal][1]

3. Wählen Sie auf dem Blatt **IoT Hub** die gewünschte Konfiguration für Ihren IoT Hub.

    ![Blatt „IoT Hub“][2]

    * Geben Sie im Feld **Name** einen Namen für Ihren IoT Hub ein. Wenn der **Name** gültig und verfügbar ist, wird im Feld **Name** ein grünes Häkchen angezeigt.
    * Wählen Sie eine [Preis- und Skalierungsstufe][lnk-pricing] aus. Für dieses Tutorial ist keine bestimmte Stufe erforderlich. Verwenden Sie für dieses Tutorial daher die kostenlose Stufe F1.
    * Erstellen Sie in **Ressourcengruppe** eine neue Ressourcengruppe, oder wählen Sie eine vorhandene aus. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen][lnk-resource-groups].
    * Wählen Sie in **Standort** den Standort aus, an dem Ihr IoT Hub gehostet werden soll. Wählen Sie für dieses Tutorial Ihren nächstgelegenen Standort aus.

4. Wenn Sie die Konfigurationsoptionen für Ihren IoT Hub ausgewählt haben, klicken Sie auf **Erstellen**. Die Erstellung des IoT Hubs kann einige Minuten dauern. Im Startmenü oder im Benachrichtigungsbereich können Sie den Fortschritt überwachen und den Status überprüfen.

    ![Neuer IoT Hub-Status][3]

5. Wenn der IoT Hub erfolgreich erstellt wurde, klicken Sie im Portal auf die neue Kachel für Ihren IoT Hub, um das Blatt für den neuen IoT Hub zu öffnen. Notieren Sie sich den Hostnamen, und klicken Sie anschließend auf **Freigegebene Zugriffsrichtlinien**.

    ![Blatt für neuen IoT Hub][4]

6. Klicken Sie im Blatt **Freigegebene Zugriffsrichtlinien** auf die Richtlinie **iothubowner**, und kopieren Sie (oder notieren Sie sich) die auf dem Blatt **iothubowner** angegebene Verbindungszeichenfolge. Weitere Informationen finden Sie im „Entwicklungsleitfaden für Azure IoT Hub“ unter [Zugriffssteuerung][lnk-access-control].

    ![Blatt „Freigegebene Zugriffsrichtlinien“][5]


<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-portal/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide.md#accesscontrol

<!---HONumber=AcomDC_0817_2016-->