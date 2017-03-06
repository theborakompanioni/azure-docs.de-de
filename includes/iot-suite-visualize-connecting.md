## <a name="view-device-telemetry-in-the-dashboard"></a>Anzeigen der Gerätetelemetrie im Dashboard
Das Dashboard in der Remoteüberwachungslösung ermöglicht Ihnen das Anzeigen der Telemetrie, die Ihre Geräte an IoT Hub senden.

1. Wechseln Sie in Ihrem Browser zum Dashboard der Remoteüberwachungslösung, und klicken Sie im linken Bereich auf **Geräte**, um zur **Geräteliste** zu gelangen.
2. In der **Geräteliste** wird angezeigt, dass das Gerät den Status **Wird ausgeführt** hat. Wenn nicht, können Sie im Bereich **Gerätedetails** auf **Gerät aktivieren**.
   
    ![Anzeigen des Gerätestatus][18]
3. Klicken Sie auf **Dashboard**, um zu diesem zurückzukehren. Wählen Sie in der Dropdownliste **Anzuzeigendes Gerät** Ihr Gerät aus, um dessen Telemetrie anzuzeigen. Die Telemetrie der Beispielanwendung ist mit 50 Einheiten für die Innentemperatur, 55 Einheiten für die Außentemperatur und 50 Einheiten für die Luftfeuchtigkeit konfiguriert.
   
    ![Anzeigen der Gerätetelemetrie][img-telemetry]

## <a name="invoke-a-method-on-your-device"></a>Aufrufen einer Methode auf Ihrem Gerät
Das Dashboard in der Remoteüberwachungslösung ermöglicht es Ihnen, über IoT Hub Methoden auf Ihren Geräten aufzurufen. Beispielsweise können Sie in der Remoteüberwachungslösung eine Methode zum Simulieren des Neustarts eines Gerät aufrufen.

1. Klicken Sie im Dashboard der Remoteüberwachungslösung im linken Bereich auf **Geräte**, um zur **Geräteliste** zu gelangen.
2. Klicken Sie in der **Geräteliste** auf die **Geräte-ID** für Ihr Gerät.
3. Klicken Sie im Bereich **Gerätedetails** auf **Methoden**.
   
    ![Gerätemethoden][13]
4. Wählen Sie in der Dropdownliste **Methode** die Option **InitiateFirmwareUpdate**, und geben Sie dann unter **FWPACKAGEURI** eine Pseudo-URL ein. Klicken Sie auf **Invoke Method** (Methode aufrufen), um die Methode auf dem Gerät aufzurufen.
   
    ![Aufrufen einer Gerätemethode][14]
   

5. In der Konsole wird bei der Ausführung Ihres Gerätecodes eine Meldung angezeigt, wenn das Gerät die Methode verarbeitet. Die Ergebnisse der Methode werden dem Verlauf im Lösungsportal hinzugefügt:

    ![Anzeigen des Methodenverlaufs][img-method-history]

## <a name="next-steps"></a>Nächste Schritte
Im Artikel [Anpassen vorkonfigurierter Lösungen][lnk-customize] werden einige Möglichkeiten zum Erweitern dieses Beispiels beschrieben. Mögliche Erweiterungen umfassen die Verwendung echter Sensoren und die Implementierung zusätzlicher Befehle.

Weitere Informationen finden Sie unter [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[img-method-history]: ./media/iot-suite-visualize-connecting/history.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
