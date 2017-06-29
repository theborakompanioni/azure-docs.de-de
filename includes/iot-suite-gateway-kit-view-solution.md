## <a name="view-the-solution-dashboard"></a>Anzeigen des Lösungsdashboards

Im Lösungsdashboard können Sie die bereitgestellte Lösung verwalten. Sie können beispielsweise Telemetriedaten anzeigen, Geräte hinzufügen und Methoden aufrufen.

1. Wenn die Bereitstellung abgeschlossen ist und auf der Kachel für die vorkonfigurierte Lösung **Bereit** angezeigt wird, wählen Sie **Starten** aus, um eine neue Registerkarte mit dem Lösungsportal für die Remoteüberwachung zu öffnen.

    ![Vorkonfigurierte Lösung starten][img-launch-solution]

1. Standardmäßig zeigt das Lösungsportal das *Dashboard* an. Navigieren Sie über das Menü auf der linken Seite zu anderen Bereichen des Lösungsportals.

    ![Lösungsdashboard der vorkonfigurierten Lösung für die Remoteüberwachung][img-menu]

## <a name="add-a-device"></a>Hinzufügen eines Geräts

Damit ein Gerät eine Verbindung mit der vorkonfigurierten Lösung herstellen kann, muss es sich mit gültigen Anmeldeinformationen bei IoT Hub identifizieren können. Sie können die Anmeldeinformationen für das Gerät aus dem Lösungsdashboard abrufen. Sie fügen die Anmeldeinformationen für das Gerät später in diesem Tutorial zu Ihrer Clientanwendung hinzu.

Um Ihrer Remoteüberwachungslösung ein Gerät hinzuzufügen, führen Sie im Lösungsdashboard die folgenden Schritte aus:

1. Klicken Sie in der linken unteren Ecke des Dashboards auf **Gerät hinzufügen**.

   ![Hinzufügen eines Geräts][1]

1. Klicken Sie im Bereich **Benutzerdefiniertes Gerät** auf **Neue hinzufügen**.

   ![Hinzufügen eines benutzerdefinierten Geräts][2]

1. Wählen Sie die Option **Ich möchte meine eigene Geräte-ID definieren**. Geben Sie eine Geräte-ID ein (beispielsweise **device01**), und klicken Sie auf **ID prüfen**, um sich zu vergewissern, dass Sie den Namen noch nicht in Ihrer Lösung verwendet haben. Klicken Sie anschließend auf **Erstellen**, um das Gerät bereitzustellen.

   ![Hinzufügen einer Geräte-ID][3]

1. Notieren Sie sich die Anmeldeinformationen des Geräts (**Geräte-ID**, **IoT Hub-Hostname** und **Geräteschlüssel**). Ihre Clientanwendung auf dem Intel NUC benötigt diese Werte, um eine Verbindung mit der Remoteüberwachungslösung herzustellen. Klicken Sie anschließend auf **Fertig**.

    ![Anzeigen von Geräteanmeldeinformationen][4]

1. Wählen Sie Ihr Gerät im Lösungsdashboard in der Liste mit den Geräten aus. Klicken Sie anschließend im Bereich **Gerätedetails** auf **Gerät aktivieren**. Der Status Ihres Geräts lautet jetzt **Wird ausgeführt**. Die Remoteüberwachungslösung kann jetzt Telemetriedaten von Ihrem Gerät empfangen und Methoden auf dem Gerät aufrufen.

[img-launch-solution]: media/iot-suite-gateway-kit-view-solution/launch.png
[img-menu]: media/iot-suite-gateway-kit-view-solution/menu.png
[1]: media/iot-suite-gateway-kit-view-solution/suite0.png
[2]: media/iot-suite-gateway-kit-view-solution/suite1.png
[3]: media/iot-suite-gateway-kit-view-solution/suite2.png
[4]: media/iot-suite-gateway-kit-view-solution/suite3.png