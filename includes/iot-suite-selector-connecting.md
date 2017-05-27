> [!div class="op_single_selector"]
> * [C unter Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [C unter Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> 
> 

## <a name="scenario-overview"></a>Übersicht über das Szenario
In diesem Szenario erstellen Sie ein Gerät, das die folgenden Telemetriedaten an die [vorkonfigurierte Lösung][lnk-what-are-preconfig-solutions] für die Remoteüberwachung sendet:

* Externe Temperatur
* Interne Temperatur
* Luftfeuchtigkeit

Zur Vereinfachung erzeugt der Code auf dem Gerät Beispielwerte. Sie sollten das Beispiel jedoch erweitern, indem Sie echte Sensoren an Ihr Gerät anschließen und tatsächliche Telemetriedaten senden.

Das Gerät kann auch auf Methoden antworten, die über das Lösungsdashboard aufgerufen werden, und auf die gewünschten Eigenschaftswerte, die im Lösungsdashboard festgelegt werden.

Um dieses Tutorial abzuschließen, benötigen Sie ein aktives Azure-Konto. Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk-free-trial].

## <a name="before-you-start"></a>Vorbereitung
Bevor Sie Code für Ihr Gerät schreiben, müssen Sie Ihre vorkonfigurierte Lösung für die Remoteüberwachung und ein neues benutzerdefiniertes Gerät in dieser Lösung bereitstellen.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Bereitstellen der vorkonfigurierten Lösung für die Remoteüberwachung
Das von Ihnen in diesem Tutorial erstellte Gerät sendet Daten an eine Instanz der vorkonfigurierten Lösung für die [Remoteüberwachung][lnk-remote-monitoring]. Führen Sie die folgenden Schritte aus, wenn Sie die vorkonfigurierte Lösung für die Remoteüberwachung noch nicht in Ihrem Azure-Konto bereitgestellt haben:

1. Klicken Sie auf der Seite <https://www.azureiotsuite.com/> auf **+**, um eine Lösung zu erstellen.
2. Klicken Sie im Bereich **Remoteüberwachung** auf **Auswählen**, um die Lösung zu erstellen.
3. Geben Sie auf der Seite **Create Remote monitoring solution** (Remoteüberwachungslösung erstellen) einen **Lösungsnamen** Ihrer Wahl ein, wählen Sie die gewünschte **Region** für die Bereitstellung aus, und wählen Sie dann das gewünschte Azure-Abonnement aus. Klicken Sie dann auf **Lösung erstellen**.
4. Warten Sie, bis der Bereitstellungsvorgang abgeschlossen ist.

> [!WARNING]
> Die vorkonfigurierten Lösungen verwenden abrechenbare Azure-Dienste. Achten Sie darauf, die vorkonfigurierte Lösung aus dem Abonnement zu entfernen, wenn Sie fertig sind, um unnötige Gebühren zu vermeiden. Sie können eine vorkonfigurierte Lösung vollständig aus Ihrem Abonnement entfernen, indem Sie auf die Seite <https://www.azureiotsuite.com/> zugreifen.
> 
> 

Wenn der Bereitstellungsprozess für die Remoteüberwachungslösung abgeschlossen ist, klicken Sie auf **Starten** , um das Lösungsdashboard im Browser zu öffnen.

![Lösungsdashboard][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Bereitstellen des Geräts in der Remoteüberwachungslösung
> [!NOTE]
> Wenn Sie bereits ein Gerät in der Lösung bereits bereitgestellt haben, können Sie diesen Schritt überspringen. Für das Erstellen der Clientanwendung müssen Sie die Anmeldeinformationen des Geräts kennen.
> 
> 

Damit ein Gerät eine Verbindung mit der vorkonfigurierten Lösung herstellen kann, muss es sich mit gültigen Anmeldeinformationen bei IoT Hub identifizieren können. Sie können die Anmeldeinformationen für das Gerät aus dem Lösungsdashboard abrufen. Sie fügen die Anmeldeinformationen für das Gerät später in diesem Tutorial zu Ihrer Clientanwendung hinzu.

Um Ihrer Remoteüberwachungslösung ein Gerät hinzuzufügen, führen Sie im Lösungsdashboard die folgenden Schritte aus:

1. Klicken Sie in der linken unteren Ecke des Dashboards auf **Gerät hinzufügen**.
   
   ![Hinzufügen eines Geräts][1]
2. Klicken Sie im Bereich **Benutzerdefiniertes Gerät** auf **Neue hinzufügen**.
   
   ![Hinzufügen eines benutzerdefinierten Geräts][2]
3. Wählen Sie die Option **Ich möchte meine eigene Geräte-ID definieren**. Geben Sie eine Geräte-ID wie **mydevice** ein, und klicken Sie auf **ID prüfen**, um zu prüfen, ob der Name bereits verwendet wird. Klicken Sie dann auf **Erstellen**, um das Gerät bereitzustellen.
   
   ![Hinzufügen einer Geräte-ID][3]
4. Notieren Sie sich die Anmeldeinformationen des Geräts (Geräte-ID, IoT Hub-Hostname und Geräteschlüssel). Ihre Clientanwendung benötigt diese Werte, um eine Verbindung mit der Remoteüberwachungslösung herzustellen. Klicken Sie anschließend auf **Fertig**.
   
    ![Anzeigen von Geräteanmeldeinformationen][4]
5. Wählen Sie Ihr Gerät im Lösungsdashboard in der Liste mit den Geräten aus. Klicken Sie anschließend im Bereich **Gerätedetails** auf **Gerät aktivieren**. Der Status Ihres Geräts lautet jetzt **Wird ausgeführt**. Die Remoteüberwachungslösung kann jetzt Telemetriedaten von Ihrem Gerät empfangen und Methoden auf dem Gerät aufrufen.

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/