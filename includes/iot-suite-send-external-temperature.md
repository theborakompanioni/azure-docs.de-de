## <a name="configure-the-nodejs-simulated-device"></a>Konfigurieren des simulierten Node.js-Geräts
1. Klicken Sie im Dashboard für die Remoteüberwachung auf **+ Gerät hinzufügen** , und fügen Sie ein *benutzerdefiniertes Gerät* hinzu. Notieren Sie sich den IoT Hub-Hostnamen, die Geräte-ID und den Geräteschlüssel. Sie benötigen diese Informationen im weiteren Verlauf dieses Tutorials, wenn Sie die Geräteclientanwendung „remote_monitoring.js“ vorbereiten.
2. Stellen Sie sicher, dass auf dem Entwicklungscomputer mindestens die Node.js-Version 0.12.x installiert ist. Führen Sie an einer Eingabeaufforderung oder in der Shell `node --version` aus, um die Version zu überprüfen. Informationen zur Verwendung eines Paket-Managers zum Installieren von Node.js unter Linux finden Sie unter [Installieren von Node.js mithilfe eines Paket-Managers][node-linux].
3. Wenn Sie Node.js installiert haben, klonen Sie die neueste Version des Repositorys [azure-iot-sdk-node][lnk-github-repo] auf Ihren Entwicklungscomputer. Verwenden Sie stets die Verzweigung **master** , die die neueste Version der Bibliotheken und Beispiele bietet.
4. Kopieren Sie in Ihrer lokalen Kopie des Repositorys [azure-iot-sdk-node][lnk-github-repo] die beiden folgenden Dateien aus dem Ordner „node/device/samples“ in einen leeren Ordner auf Ihrem Entwicklungscomputer:
   
   * packages.json
   * remote_monitoring.js
5. Öffnen Sie die Datei „remote_monitoring.js“, und suchen Sie nach der folgenden Variablendefinition:
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. Ersetzen Sie **[IoT Hub device connection string]** durch Ihre Geräteverbindungszeichenfolge. Verwenden Sie die Werte für Ihren IoT Hub-Hostnamen, die Geräte-ID und den Geräteschlüssel, die Sie in Schritt 1 notiert haben. Eine Geräteverbindungszeichenfolge hat folgendes Format:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    Wenn der IoT Hub-Hostname **contoso** und die Geräte-ID **mydevice** lautet, sieht Ihre Verbindungszeichenfolge wie folgt aus:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Speichern Sie die Datei. Führen Sie die folgenden Befehle in einer Shell oder an einer Eingabeaufforderung im Ordner mit diesen Dateien aus, um die erforderlichen Pakete zu installieren. Führen Sie dann die Beispielanwendung aus:
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Beobachten dynamischer Telemetriedaten in Aktion
Das Dashboard zeigt die Telemetriedaten zu Temperatur und Feuchtigkeit von den vorhandenen simulierten Geräten:

![Das Standarddashboard][image1]

Wenn Sie das simulierte Node.js-Gerät auswählen, das Sie im vorherigen Abschnitt ausgeführt haben, werden Telemetriedaten zu Temperatur, Luftfeuchtigkeit und Außentemperatur angezeigt:

![Hinzufügen von Außentemperaturen zum Dashboard][image2]

Die Remoteüberwachungslösung erkennt automatisch die zusätzlichen Telemetriedaten zur Außentemperatur und fügt sie dem Diagramm im Dashboard hinzu.

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-send-external-temperature/image1.png
[image2]: media/iot-suite-send-external-temperature/image2.png