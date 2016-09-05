<properties
  pageTitle="Azure IoT Suite und Logik-Apps | Microsoft Azure"
  description="Tutorial zum Einbinden von Logik-Apps in Azure IoT Suite für Geschäftsprozesse."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="08/16/2016"
  ms.author="araguila"/>
  
# Tutorial: Verbinden einer Logik-App mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite

Die vorkonfigurierte Remoteüberwachungslösung von [Microsoft Azure IoT Suite][lnk-internetofthings] eignet sich besonders gut für die ersten Schritte mit einem lückenlosen Featuresatz zum Veranschaulichen einer IoT-Lösung. In diesem Tutorial wird erläutert, wie Sie Ihrer vorkonfigurierten Remoteüberwachungslösung von Microsoft Azure IoT Suite eine Logik-App hinzufügen. Diese Schritte veranschaulichen, wie Sie mit Ihrer IoT-Lösung noch mehr erreichen können, indem Sie sie mit einem Geschäftsprozess verbinden.

_Wenn Sie eine exemplarische Vorgehensweise zum Bereitstellen einer vorkonfigurierten Remoteüberwachungslösung suchen, siehe das [Tutorial: Erste Schritte mit den vorkonfigurierten IoT-Lösungen][lnk-getstarted]._

Bevor Sie mit diesem Tutorial beginnen, sollten Sie zunächst:

- eine vorkonfigurierte Lösung für die Remoteüberwachung in Ihrem Azure-Abonnement bereitstellen

- ein SendGrid-Konto erstellen, um eine E-Mail zu senden, die Ihren Geschäftsprozess auslöst Sie können sich bei [SendGrid](https://sendgrid.com/) durch Klicken auf **Jetzt loslegen** für ein kostenloses Testkonto registrieren. Nachdem Sie sich für Ihr kostenloses Testkonto registriert haben, müssen Sie in SendGrid einen [API-Schlüssel](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) erstellen, der Berechtigungen zum Senden von E-Mail erteilt. Sie benötigen diesen API-Schlüssel im weiteren Verlauf des Tutorials.

Wenn Sie Ihre vorkonfigurierte Remoteüberwachungslösung bereits bereitgestellt haben, navigieren Sie im [Azure-Portal][lnk-azureportal] zur Ressourcengruppe dieser Lösung. Die Ressourcengruppe hat den gleichen Namen wie die Lösung, den Sie gewählt haben, als Sie Ihre Remoteüberwachungslösung bereitgestellt haben. In der Ressourcengruppe finden Sie alle bereitgestellten Azure-Ressourcen für Ihre Lösung mit Ausnahme der Azure Active Directory-Anwendung, die Sie im klassischen Azure-Portal finden. Der folgende Screenshot zeigt ein Beispiel des Blatts **Ressourcengruppe** für eine vorkonfigurierte Remoteüberwachungslösung:

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

Richten Sie zunächst die Logik-App ein, die mit der vorkonfigurierten Lösung verwendet werden soll.

## Einrichten der Logik-App

1. Klicken Sie im Azure-Portal oben auf dem Blatt „Ressourcengruppe“ auf __Hinzufügen__.

2. Suchen Sie __Logik-App__, wählen Sie diese Option aus, und klicken Sie dann auf **Erstellen**.

3. Geben Sie den __Namen__ ein, und wählen Sie für **Abonnement** und **Ressourcengruppe** die Angaben, die Sie bei der Bereitstellung Ihrer Remoteüberwachungslösung verwendet haben. Klicken Sie auf __Erstellen__.

    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)

4. Sobald die Bereitstellung abgeschlossen ist, sehen Sie, dass die Logik-App als Ressource in der Ressourcengruppe aufgeführt ist.

5. Klicken Sie auf die Logik-App, um zum Logik-App-Blatt zu navigieren, und wählen Sie die Vorlage **Leere Logik-App** aus, um den **Designer für Logik-Apps** zu öffnen.

    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)

6. Wählen Sie __Anfordern__ aus. Diese Aktion gibt an, dass eine eingehende HTTP-Anforderung mit einer bestimmten JSON-formatierten Nutzlast als Trigger fungiert.

7. Fügen Sie Folgendes in den Anforderungstext des JSON-Schemas ein:

    ```
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
    
    > [AZURE.NOTE] Sie können die URL für den HTTP-Post-Befehl kopieren, nachdem Sie die Logik-App gespeichert haben. Zuvor muss jedoch eine Aktion hinzugefügt werden.

8. Klicken Sie unter Ihrem manuellen Trigger auf __+ Neuer Schritt __. Klicken Sie dann auf **Aktion hinzufügen**.

    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)

9. Suchen Sie **SendGrid – E-Mail senden**, und klicken Sie darauf.

    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)

10. Geben Sie einen Namen für die Verbindung ein, z. B. **SendGridConnection**, geben Sie den **SendGrid-API-Schlüssel** ein, der erstellt wurde, als Sie Ihr SendGrid-Konto eingerichtet haben, und klicken Sie auf **Erstellen**.

    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)

11. Fügen Sie den Feldern **Von** und **An** E-Mail-Adressen hinzu. Fügen Sie **Remoteüberwachungswarnung [DeviceId]** dem Feld **Betreff** hinzu. Fügen Sie dem Feld **E-Mail-Text** Folgendes hinzu: **Gerät [DeviceId] hat [measurementName] mit dem Wert [measuredValue] gemeldet.** Sie können **[DeviceId]**, **[measurementName]** und **[measuredValue]** hinzufügen, indem Sie auf den Abschnitt **Sie können Daten aus vorherigen Schritten einfügen** klicken.

    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)

12. Klicken Sie in der oberen Menüleiste auf __Speichern__.

13. Klicken Sie auf den Trigger **Anfordern**, und kopieren Sie den Wert von__HTTP Post an diese URL__. Sie benötigen diese URL im weiteren Verlauf dieses Tutorials.

> [AZURE.NOTE] Logik-Apps ermöglichen Ihnen das Ausführen [vieler verschiedener Aktionstypen][lnk-logic-apps-actions], einschließlich Aktionen in Office 365.

## Einrichten des EventProcessor-Webauftrags

In diesem Abschnitt verbinden Sie Ihre vorkonfigurierte Lösung mit der Logik-App, die Sie erstellt haben. Zu diesem Zweck fügen Sie die URL hinzu, um die Logik-App für die Aktion auszulösen, die eingeleitet wird, wenn ein Gerätesensorwert einen Schwellenwert überschreitet.

1. Verwenden Sie Ihren Git-Client zum Klonen der neuesten Version des Github-Repositorys [azure-iot-remote-monitoring][lnk-rmgithub]. Beispiel:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. Öffnen Sie in Visual Studio __RemoteMonitoring.sln__ in der lokalen Kopie des Repositorys.

3. Öffnen Sie im Ordner **Infrastructure\\Repository** die Datei __ActionRepository.cs__.

4. Aktualisieren Sie das Wörterbuch **ActionIds** mit __HTTP Post an diese URL__, die Sie folgendermaßen in Ihrer Logik-App notiert haben:

    ```
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this UR>" },
        { "Raise Alarm", "<Http Post to this UR> }
    };
    ```

5. Speichern Sie die Änderungen in der Projektmappe, und beenden Sie Visual Studio.

## Über die Befehlszeile bereitstellen

In diesem Abschnitt stellen Sie die aktualisierte Version der Remoteüberwachungslösung so bereit, dass die derzeit in Azure ausgeführte Version ersetzt wird.

1. Befolgen Sie die [Anweisungen zum Einrichten der Entwicklungsumgebung][lnk-devsetup] für die Bereitstellung.

2.  Befolgen Sie für eine [lokale Bereitstellung][lnk-localdeploy] die entsprechenden Anweisungen.

3.  Für eine Bereitstellung in der Cloud und Aktualisierung Ihrer vorhandenen Cloudbereitstellung befolgen Sie die Anweisung zur [Cloudbereitstellung][lnk-clouddeploy]. Verwenden Sie den Namen der ursprünglichen Bereitstellung als Bereitstellungsnamen. Wenn beispielsweise die ursprüngliche Bereitstellung **demologicapp** hieß, verwenden Sie den folgenden Befehl:

    ``
    build.cmd cloud release demologicapp
    ``
    
    Wenn das Buildskript ausgeführt wird, vergewissern Sie sich, dass Sie dasselbe Azure-Konto und -Abonnement bzw. dieselbe Region und Active Directory-Instanz wie bei der Bereitstellung der Lösung verwenden.

## Ihre Logik-App in Aktion

In der vorkonfigurierten Remoteüberwachungslösung sind standardmäßig zwei Regeln eingerichtet, wenn Sie eine Lösung bereitstellen. Beide Regeln gelten auf dem Gerät **SampleDevice001**:

* Temperatur > 38,00
* Luftfeuchtigkeit > 48,00

Die Temperaturregel löst die Aktion **Alarm auslösen** aus. Die Luftfeuchtigkeitsregel löst die Aktion **Nachricht senden** aus. Wenn Sie die gleiche URL für beide Aktionen der **ActionRepository**-Klasse verwenden, wird Ihre Logik-App bei jeder der Regeln ausgelöst. Beide Regeln verwenden SendGrid, um eine E-Mail mit den Details zu dieser Warnung an die **An**-Adresse zu senden.

> [AZURE.NOTE] Die Logik-App wird weiterhin bei jedem Erreichen des Schwellenwerts ausgelöst. Um unnötige E-Mails zu vermeiden, können Sie entweder die Regeln in Ihrem Lösungsportal oder die Logik-App im [Azure-Portal][lnk-azureportal] deaktivieren.

Zusätzlich zum Empfangen von E-Mails können Sie auch prüfen, wann die Logik-App im Portal ausgeführt wird:

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## Nächste Schritte

Nachdem Sie eine Logik-App zum Verbinden der vorkonfigurierten Lösung mit einem Geschäftsprozess genutzt haben, können Sie mehr über die Optionen zum Anpassen der vorkonfigurierten Lösungen erfahren:

- [Verwenden dynamischer Telemetriedaten mit der vorkonfigurierten Lösung für die Remoteüberwachung][lnk-dynamic]
- [Geräteinformationen-Metadaten in der vorkonfigurierten Lösung für die Remoteüberwachung][lnk-devinfo]

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md

<!---HONumber=AcomDC_0824_2016-->