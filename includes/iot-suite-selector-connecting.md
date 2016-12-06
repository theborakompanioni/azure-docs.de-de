> [!div class="op_single_selector"]
> * [C unter Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [C unter Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [C auf mbed](../articles/iot-suite/iot-suite-connecting-devices-mbed.md)
> * [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> 
> 

## <a name="scenario-overview"></a>Übersicht über das Szenario
In diesem Szenario erstellen Sie ein Gerät, das die folgenden Telemetriedaten an die [vorkonfigurierte Lösung][lnk-what-are-preconfig-solutions] für die Remoteüberwachung sendet:

* Externe Temperatur
* Interne Temperatur
* Luftfeuchtigkeit

Zur Vereinfachung erzeugt der Code auf dem Gerät Beispielwerte. Sie sollten das Beispiel jedoch erweitern, indem Sie echte Sensoren an Ihr Gerät anschließen und tatsächliche Telemetriedaten senden.

Um dieses Tutorial abzuschließen, benötigen Sie ein aktives Azure-Konto. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [Azure – Kostenlose Testversion][lnk-free-trial].

## <a name="before-you-start"></a>Vorbereitung
Bevor Sie Code für Ihr Gerät schreiben, müssen Sie Ihre vorkonfigurierte Lösung für die Remoteüberwachung und anschließend ein neues benutzerdefiniertes Gerät in dieser Lösung bereitstellen.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Bereitstellen der vorkonfigurierten Lösung für die Remoteüberwachung
Das von Ihnen in diesem Tutorial erstellte Gerät sendet Daten an eine Instanz der vorkonfigurierten Lösung für die [Remoteüberwachung][lnk-remote-monitoring]. Wenn Sie die vorkonfigurierte Lösung für die Remoteüberwachung noch nicht in Ihrem Azure-Konto bereitgestellt haben, führen Sie die folgenden Schritte aus:

1. Klicken Sie auf der Seite <https://www.azureiotsuite.com/> auf **+**, um eine neue Lösung zu erstellen.
2. Klicken Sie im Bereich **Remoteüberwachung** auf **Auswählen**, um die neue Lösung zu erstellen.
3. Geben Sie auf der Seite **Create Remote monitoring solution** (Remoteüberwachungslösung erstellen) einen **Lösungsnamen** Ihrer Wahl ein, wählen Sie die gewünschte **Region** für die Bereitstellung aus, und wählen Sie dann das gewünschte Azure-Abonnement aus. Klicken Sie dann auf **Lösung erstellen**.
4. Warten Sie, bis der Bereitstellungsvorgang abgeschlossen ist.

> [!WARNING]
> Die vorkonfigurierten Lösungen verwenden abrechenbare Azure-Dienste. Achten Sie darauf, die vorkonfigurierte Lösung aus dem Abonnement zu entfernen, wenn Sie fertig sind, um unnötige Gebühren zu vermeiden. Sie können eine vorkonfigurierte Lösung vollständig aus Ihrem Abonnement entfernen, indem Sie auf die Seite <https://www.azureiotsuite.com/> zugreifen.
> 
> 

Wenn der Bereitstellungsprozess für die Remoteüberwachungslösung abgeschlossen ist, klicken Sie auf **Starten** , um das Lösungsdashboard im Browser zu öffnen.

![][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Bereitstellen des Geräts in der Remoteüberwachungslösung
> [!NOTE]
> Wenn Sie bereits ein Gerät in der Lösung bereits bereitgestellt haben, können Sie diesen Schritt überspringen. Für das Erstellen der Clientanwendung müssen Sie die Anmeldeinformationen des Geräts kennen.
> 
> 

Damit ein Gerät eine Verbindung mit der vorkonfigurierten Lösung herstellen kann, muss es sich mit gültigen Anmeldeinformationen bei IoT Hub identifizieren können. Sie können die Anmeldeinformationen für das Gerät aus dem Lösungsdashboard abrufen. Sie fügen die Anmeldeinformationen für das Gerät später in diesem Tutorial zu Ihrer Clientanwendung hinzu. 

Um Ihrer Remoteüberwachungslösung ein neues Gerät hinzuzufügen, führen Sie folgende Schritte im Lösungsdashboard durch:

1. Klicken Sie in der linken unteren Ecke des Dashboards auf **Gerät hinzufügen**.
   
   ![][1]
2. Klicken Sie im Bereich **Benutzerdefiniertes Gerät** auf **Neues Element hinzufügen**.
   
   ![][2]
3. Wählen Sie **Ich möchte meine eigene Geräte-ID definieren** aus, geben Sie eine Geräte-ID wie **mydevice** ein, und klicken Sie auf **ID prüfen**, um zu prüfen, ob der Name schon verwendet wird. Klicken Sie dann auf **Erstellen**, um das Gerät bereitzustellen.
   
   ![][3]
4. Notieren Sie die Anmeldeinformationen des Geräts (Geräte-ID, IoT Hub-Hostname und Geräteschlüssel). Ihre Clientanwendung benötigt diese Informationen zum Herstellen einer Verbindung mit der Remoteüberwachungslösung. Klicken Sie anschließend auf **Fertig**.
   
    ![][4]
5. Stellen Sie sicher, dass das Gerät im Abschnitt mit den Geräten angezeigt wird. Der Gerätestatus lautet **Ausstehend** bis das Gerät eine Verbindung mit der Remoteüberwachungslösung hergestellt hat.
   
    ![][5]

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png
[5]: ./media/iot-suite-selector-connecting/suite5.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

<!--HONumber=Nov16_HO3-->


