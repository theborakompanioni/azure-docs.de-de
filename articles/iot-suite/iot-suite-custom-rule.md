---
title: Erstellen einer benutzerdefinierten Regel in Azure IoT Suite | Microsoft-Dokumentation
description: "Erstellen einer benutzerdefinierten Regel in einer vorkonfigurierten IoT Suite-Lösung."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: 900f0b4c38f08d1e3017954db01cf32362765578
ms.contentlocale: de-de
ms.lasthandoff: 03/10/2017

---
# <a name="create-a-custom-rule-in-the-remote-monitoring-preconfigured-solution"></a>Erstellen einer benutzerdefinierten Regel in der vorkonfigurierten Remoteüberwachungslösung

## <a name="introduction"></a>Einführung

In den vorkonfigurierten Lösungen können Sie [Regeln konfigurieren, die ausgelöst werden, wenn ein Telemetriewert für ein Gerät einen bestimmten Schwellenwert erreicht][lnk-builtin-rule]. Unter [Verwenden dynamischer Telemetriedaten mit der vorkonfigurierten Lösung für die Remoteüberwachung][lnk-dynamic-telemetry] wird beschrieben, wie Sie Ihrer Lösung benutzerdefinierte Telemetriewerte wie *ExternalTemperature* hinzufügen. In diesem Artikel wird gezeigt, wie Sie eine benutzerdefinierte Regel für dynamische Telemetrietypen in Ihrer Lösung erstellen.

In diesem Tutorial wird ein einfaches simuliertes Node.js-Gerät zum Generieren dynamischer Telemetriedaten verwendet, die zum Back-End der vorkonfigurierten Lösung gesendet werden. Sie fügen anschließend der Visual Studio-Projektmappe **RemoteMonitoring** benutzerdefinierte Regeln hinzu und stellen dieses benutzerdefinierte Back-End Ihrem Azure-Abonnement bereit.

Für dieses Tutorial benötigen Sie Folgendes:

* Ein aktives Azure-Abonnement. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk_free_trial].
* [Node.js][lnk-node]-Version 0.12.x oder höher zum Erstellen eines simulierten Geräts.
* Visual Studio 2015 oder Visual Studio 2017 zum Ändern des Back-Ends der vorkonfigurierten Lösung mit Ihren neuen Regeln

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

Notieren Sie den Namen der Lösung, die Sie für Ihre Bereitstellung ausgewählt haben. Sie benötigen diesen Lösungsnamen im weiteren Verlauf dieses Tutorials.

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

Sie können die Node.js-Konsolen-App beenden, sobald Sie bestätigt haben, dass sie sendet Telemetriedaten zu **ExternalTemperature** an die vorkonfigurierte Lösung sendet. Lassen Sie das Konsolenfenster geöffnet, da Sie diese Node.js-Konsolen-App erneut ausführen, nachdem Sie die benutzerdefinierte Regel zur Lösung hinzugefügt haben.

## <a name="rule-storage-locations"></a>Speicherorte von Regeln

Informationen zu Regeln werden an zwei Orten gespeichert:

* In der Tabelle **DeviceRulesNormalizedTable**: In dieser Tabelle wird ein normalisierter Verweis auf die Regeln gespeichert, die vom Lösungsportal definiert werden. Zum Anzeigen von Geräteregeln im Lösungsportal wird diese Tabelle auf Regeldefinitionen abgefragt.
* Blob **DeviceRules**: In diesem Blob, das als Referenzeingabe in die Azure Stream Analytics-Aufträge definiert ist, werden alle Regeln gespeichert, die für alle registrierten Geräte definiert sind.
 
Wenn Sie im Lösungsportal eine vorhandene Regel aktualisieren oder eine neue Regel definieren, werden die Tabelle und das Blob entsprechend aktualisiert. Die im Portal angezeigte Regeldefinition stammt aus dem Tabellenspeicher. Die Regeldefinition, auf die die Stream Analytics-Aufträge verweisen, stammt aus dem Blob. 

## <a name="update-the-remotemonitoring-visual-studio-solution"></a>Aktualisieren der Visual Studio-Projektmappe „RemoteMonitoring“

Die folgenden Schritte veranschaulichen das Ändern der Visual Studio-Projektmappe „RemoteMonitoring“ durch Hinzufügen einer neuen Regel, die den vom simulierten Gerät gesendeten Telemetriewert **ExternalTemperature** verwendet:

1. Falls nicht bereits geschehen, klonen Sie das Repository **azure-iot-remote-monitoring** mithilfe des folgenden Git-Befehls an einen geeigneten Speicherort auf Ihrem lokalen Computer:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. Öffnen Sie in Visual Studio in der lokalen Kopie des Repository **azure-iot-remote-monitoring** die Datei „RemoteMonitoring.sln“.

3. Öffnen Sie die Datei „Infrastructure\Models\DeviceRuleBlobEntity.cs“, und fügen Sie die Eigenschaft **ExternalTemperature** wie folgt hinzu:

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. Fügen Sie in der gleichen Datei die **ExternalTemperatureRuleOutput**-Eigenschaft wie folgt hinzu:

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. Öffnen Sie die Datei „Infrastructure\Models\DeviceRuleDataFields.cs“, und fügen Sie die folgende **ExternalTemperature**-Eigenschaft hinter der vorhandenen **Humidity**-Eigenschaft hinzu:

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. Aktualisieren Sie in der gleichen Datei die **_availableDataFields**-Methode, in die Sie **ExternalTemperature** wie folgt einschließen:

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. Öffnen Sie die Datei „Infrastructure\Repository\DeviceRulesRepository.cs“, und ändern Sie die **BuildBlobEntityListFromTableRows**-Methode wie folgt:

    ```csharp
    else if (rule.DataField == DeviceRuleDataFields.Humidity)
    {
        entity.Humidity = rule.Threshold;
        entity.HumidityRuleOutput = rule.RuleOutput;
    }
    else if (rule.DataField == DeviceRuleDataFields.ExternalTemperature)
    {
      entity.ExternalTemperature = rule.Threshold;
      entity.ExternalTemperatureRuleOutput = rule.RuleOutput;
    }
    ```

## <a name="rebuild-and-redeploy-the-solution"></a>Erstellen Sie die Projektmappe neu, und stellen Sie sie erneut bereit.

Sie können jetzt die aktualisierte Projektmappe Ihrem Azure-Abonnement bereitstellen.

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, und navigieren Sie zum Stammverzeichnis Ihrer lokalen Kopie des Repositorys „azure-iot-remote-monitoring“.

2. Um die aktualisierte Projektmappe bereitstellen, führen Sie den folgenden Befehl aus, wobei Sie **{deployment name}** durch den Namen Ihrer vorkonfigurierten Lösungsbereitstellung ersetzen, den Sie zuvor notiert haben:

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-the-stream-analytics-job"></a>Aktualisieren des Stream Analytics-Auftrags

Sobald die Bereitstellung abgeschlossen ist, können Sie den Stream Analytics-Auftrag mit den neuen Regeldefinitionen aktualisieren.

1. Navigieren Sie im Azure-Portal zur Ressourcengruppe, die Ihre vorkonfigurierten Lösungsressourcen enthält. Diese Ressourcengruppe hat den Namen, den Sie für die Lösung während der Bereitstellung angegeben haben.

2. Navigieren Sie zum Stream Analytics-Auftrag „{deployment name}-Rules“. 

3. Klicken Sie auf **Beenden**, um den Stream Analytics-Auftrag zu beenden. (Sie müssen warten, bis der Stream Analytics-Auftrag beendet ist, ehe Sie die Abfrage bearbeiten können).

4. Klicken Sie auf **Abfrage**. Ändern Sie die Abfrage durch Hinzufügen der **SELECT**-Anweisung für **ExternalTemperature**. Das folgende Beispiel zeigt die vollständige Abfrage mit der neuen **SELECT**-Anweisung:

    ```
    WITH AlarmsData AS 
    (
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Temperature' as ReadingType,
         Stream.Temperature as Reading,
         Ref.Temperature as Threshold,
         Ref.TemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Humidity' as ReadingType,
         Stream.Humidity as Reading,
         Ref.Humidity as Threshold,
         Ref.HumidityRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'ExternalTemperature' as ReadingType,
         Stream.ExternalTemperature as Reading,
         Ref.ExternalTemperature as Threshold,
         Ref.ExternalTemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.ExternalTemperature IS NOT null AND Stream.ExternalTemperature > Ref.ExternalTemperature
    )
     
    SELECT *
    INTO DeviceRulesMonitoring
    FROM AlarmsData
     
    SELECT *
    INTO DeviceRulesHub
    FROM AlarmsData
    ```

5. Klicken Sie auf **Speichern**, um die aktualisierte Regelabfrage zu ändern.

6. Klicken Sie auf **Starten**, um den Stream Analytics-Auftrag erneut zu starten.

## <a name="add-your-new-rule-in-the-dashboard"></a>Hinzufügen der neuen Regel zum Dashboard

Sie können nun die Regel **ExternalTemperature** einem Gerät im Lösungsdashboard hinzufügen.

1. Navigieren Sie zum Lösungsportal.

2. Navigieren Sie zum Bereich **Geräte**.

3. Wechseln Sie zum von Ihnen erstellten Gerät, das **ExternalTemperature**-Telemetriedaten sendet, und klicken Sie im Bereich **Gerätedetails** auf **Regel hinzufügen**.

4. Wählen Sie **ExternalTemperature** in **Datenfeld** aus.

5. Legen Sie **Schwellenwert** auf 56 fest. Klicken Sie dann auf **Regeln anzeigen und speichern**.

6. Kehren Sie zum Dashboard zurück, um den Alarmverlauf anzuzeigen.

7. Starten Sie im geöffnet gehaltenen Konsolenfenster die Node.js-Konsolen-App, damit sie mit dem Senden von **ExternalTemperature**-Telemetriedaten beginnt.

8. Beachten Sie, dass in der Tabelle **Alarmverlauf** neue Alarme gezeigt werden, wenn die neue Regel ausgelöst wird.
 
## <a name="additional-information"></a>Zusätzliche Informationen

Das Ändern des Operators **>** ist komplexer und geht über die in diesem Tutorial behandelten Schritte hinaus. Wenngleich Sie den Stream Analytics-Auftrag für die Verwendung eines gewünschten Operators ändern können, ist die Wiedergabe dieses Operators im Lösungsportal eine komplexere Aufgabe. 

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun wissen, wie benutzerdefinierte Regeln erstellt werden, können Sie mehr zu den vorkonfigurierten Lösungen erfahren:

- [Verbinden einer Logik-App mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite][lnk-logic-app]
- [Geräteinformationen-Metadaten in der vorkonfigurierten Lösung für die Remoteüberwachung][lnk-devinfo]

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-logic-apps-tutorial.md
