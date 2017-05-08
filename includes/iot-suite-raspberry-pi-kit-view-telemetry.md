## <a name="view-the-telemetry"></a>Anzeigen der Telemetrie

Der Raspberry Pi sendet nun Telemetriedaten an die Remoteüberwachungslösung. Die Telemetriedaten können auf dem Lösungsdashboard angezeigt werden. Über das Lösungsdashboard können Sie auch Nachrichten an Ihren Raspberry Pi senden.

- Navigieren Sie zum Lösungsdashboard.
- Wählen Sie in der Dropdownliste **Anzuzeigendes Gerät** Ihr Gerät aus.
- Die Telemetriedaten des Raspberry Pi werden auf dem Dashboard angezeigt.

![Anzeigen von Telemetriedaten des Raspberry Pi][img-telemetry-display]

## <a name="act-on-the-device"></a>Ausführen von Aktionen für das Gerät

Über das Lösungsdashboard können Sie Methoden auf Ihrem Raspberry Pi aufrufen. Wenn der Raspberry Pi eine Verbindung mit der Remoteüberwachungslösung herstellt, sendet er Informationen zu den unterstützten Methoden.

- Klicken Sie im Lösungsdashboard auf **Geräte**, um die Seite **Geräte** aufzurufen. Wählen Sie in der **Geräteliste** Ihren Raspberry Pi aus. Wählen Sie anschließend **Methoden** aus:

    ![Auflisten von Geräten im Dashboard][img-list-devices]

- Wählen Sie auf der Seite **Methode aufrufen** in der Dropdownliste **Methode** die Option **LightBlink** aus.

- Wählen Sie **InvokeMethod** aus. Die mit dem Raspberry Pi verbundene LED blinkt mehrmals. Die App auf dem Raspberry Pi gibt eine Bestätigung an das Lösungsdashboard zurück:

    ![Anzeigen des Methodenverlaufs][img-method-history]

- Die LED kann mithilfe der Methode **ChangeLightStatus** ein- und ausgeschaltet werden. Legen Sie dazu **LightStatusValue** auf **1** (Ein) bzw. **0** (Aus) fest.

> [!WARNING]
> Wenn Sie die Ausführung der Remoteüberwachungslösung in Ihrem Azure-Konto nicht beenden, wird Ihnen die Ausführungszeit in Rechnung gestellt. Weitere Informationen zur Senkung des Verbrauchs während der Ausführung der Remoteüberwachungslösung finden Sie unter [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config] (Konfigurieren vorkonfigurierter Azure IoT Suite-Lösungen zu Demonstrationszwecken). Löschen Sie die vorkonfigurierte Lösung aus Ihrem Azure-Konto, wenn Sie sie nicht mehr benötigen.


[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry/telemetry.png
[img-list-devices]: media/iot-suite-raspberry-pi-kit-view-telemetry/listdevices.png
[img-method-history]: media/iot-suite-raspberry-pi-kit-view-telemetry/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md