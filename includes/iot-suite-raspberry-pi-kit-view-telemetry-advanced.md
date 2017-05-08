## <a name="view-the-telemetry"></a>Anzeigen der Telemetrie

Der Raspberry Pi sendet nun Telemetriedaten an die Remoteüberwachungslösung. Die Telemetriedaten können auf dem Lösungsdashboard angezeigt werden. Über das Lösungsdashboard können Sie auch Nachrichten an Ihren Raspberry Pi senden.

- Navigieren Sie zum Lösungsdashboard.
- Wählen Sie in der Dropdownliste **Anzuzeigendes Gerät** Ihr Gerät aus.
- Die Telemetriedaten des Raspberry Pi werden auf dem Dashboard angezeigt.

![Anzeigen von Telemetriedaten des Raspberry Pi][img-telemetry-display]

## <a name="initiate-the-firmware-update"></a>Initiieren des Firmwareupdates

Der Firmwareupdateprozess lädt eine aktualisierte Version der Geräteclientanwendung herunter und installiert sie auf dem Raspberry Pi. Weitere Informationen zum Firmwareupdateprozess finden Sie unter der Beschreibung des Firmwareupdatemusters in der [Übersicht über die Geräteverwaltung mit IoT Hub][lnk-update-pattern].

Das Firmwareupdate wird durch Aufrufen einer Methode auf dem Gerät initiiert. Diese Methode ist asynchron und springt umgehend nach dem Start des Updateprozesses zurück. Das Gerät informiert die Lösung mithilfe gemeldeter Eigenschaften über den Fortschritt des Updates.

Methoden auf dem Raspberry Pi werden über das Lösungsdashboard aufgerufen. Wenn der Raspberry Pi zum ersten Mal eine Verbindung mit der Remoteüberwachungslösung herstellt, sendet er Informationen zu den unterstützten Methoden. 

[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry-advanced/telemetry.png
[lnk-update-pattern]: ../articles/iot-hub/iot-hub-device-management-overview.md
