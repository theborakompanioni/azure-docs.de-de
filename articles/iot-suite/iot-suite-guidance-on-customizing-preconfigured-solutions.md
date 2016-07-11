<properties
	pageTitle="Anpassen von vorkonfigurierten Lösungen | Microsoft Azure"
	description="Dieses Dokument bietet eine Anleitung zum Anpassen vorkonfigurierter Azure IoT Suite-Lösungen."
	services=""
    suite="iot-suite"
	documentationCenter=".net"
	authors="stevehob"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/27/2016"
     ms.author="stevehob"/>

# Anpassen vorkonfigurierter Lösungen

Die vorkonfigurierten Lösungen in der Azure IoT Suite veranschaulichen das Bereitstellen einer umfassenden Lösung basierend auf dem Zusammenwirken der Dienste in der Suite. Von hier ausgehend gibt es verschiedene Möglichkeiten zur Erweiterung und Anpassung der Lösung für bestimmte Szenarios. Diese allgemeinen Anpassungsmöglichkeiten werden in den folgenden Abschnitten beschrieben.

## Suchen des Quellcodes

Der Quellcode für die vorkonfigurierten Lösungen ist auf GitHub in den folgenden Repositorys verfügbar:

- Remoteüberwachung: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
- Vorhersagbarer Wartungsbedarf: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)

Der Quellcode für die vorkonfigurierten Lösungen wird bereitgestellt, um Muster und Verfahren zum Implementieren der umfassenden Funktionalität von IoT-Lösungen mithilfe von Azure IoT Suite zu veranschaulichen. Weitere Informationen zur Erstellung und Bereitstellung von Lösungen finden Sie in den GitHub-Repositorys.

## Ändern der vorkonfigurierten Regeln

Die Remoteüberwachungslösung enthält drei [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)-Aufträge, um die Geräteinformationen, Telemetriedaten und Regellogik für die Lösung zu implementieren.

Die drei Stream Analytics-Aufträge und ihre Syntax werden ausführlich unter [Exemplarische Vorgehensweise zur vorkonfigurierten Lösung für Remoteüberwachung](iot-suite-remote-monitoring-sample-walkthrough.md) beschrieben.

Sie können diese Aufträge direkt bearbeiten, um die Logik zu ändern oder spezifische Logik für das Szenario hinzuzufügen. Sie finden die Stream Analytics-Aufträge wie folgt:
 
1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zu der Ressourcengruppe mit demselben Namen wie Ihre IoT-Lösung.
3. Wählen Sie den Azure Stream Analytics-Auftrag aus, den Sie ändern möchten.
4. Beenden Sie den Auftrag, indem Sie aus den Befehlen **Beenden** auswählen.
5. Bearbeiten Sie die Eingaben, die Abfrage und die Ausgaben.

    Eine einfache Änderung besteht darin, die Abfrage für den Auftrag **Regeln** so zu ändern, dass **„<“** anstelle von **„>“** verwendet wird. Das Lösungsportal zeigt weiterhin **„>“** an, wenn Sie eine Regel bearbeiten. Sie werden allerdings feststellen, dass sich das Verhalten aufgrund der Änderung am zugrunde liegenden Auftrag umkehrt.

6. Starten des Auftrags

> [AZURE.NOTE] Das Remoteüberwachungs-Dashboard hängt von bestimmten Daten ab. Deshalb kann das Ändern der Aufträge dazu führen, dass das Dashboard fehlschlägt.

## Hinzufügen eigener Regeln

Sie können nicht nur die vorkonfigurierten Azure Stream Analytics-Aufträge ändern, sondern auch im Azure-Portal neue Aufträge hinzufügen oder vorhandenen Aufträgen neue Abfragen hinzufügen.

## Anpassen von Geräten

Eine der am häufigsten Erweiterungsaktivitäten ist das Arbeiten mit speziellen Geräten für Ihr Szenario. Es gibt mehrere Methoden zum Arbeiten mit Geräten. Dazu gehören das Anpassen eines simulierten Geräts an Ihr Szenario und das Verknüpfen des physischen Geräts mit der Lösung mithilfe des [IoT-Geräte-SDK][].

Eine schrittweise Anleitung zum Hinzufügen von Geräten zur vorkonfigurierten Remoteüberwachungslösung finden Sie unter [Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung von IoT Suite](iot-suite-connecting-devices.md) und im [C-SDK-Beispiel zur Remoteüberwachung](https://github.com/Azure/azure-iot-sdks/tree/master/c/serializer/samples/remote_monitoring), das für den Einsatz mit der vorkonfigurierten Remoteüberwachungslösung konzipiert ist.

### Erstellen eines eigenen simulierten Geräts

Der Quellcode der Remoteüberwachungslösung (auf den oben verwiesen wird) enthält einen .NET-Simulator. Dieser Simulator wird als Teil der Lösung bereitgestellt und kann geändert werden, um andere Metadaten oder Telemetrie zu senden oder auf andere Befehle zu reagieren.

Der vorkonfigurierte Simulator in der vorkonfigurierten Lösung zur Remoteüberwachung ist ein Kühlgerät, das Telemetriedaten zu Temperatur und Feuchtigkeit ausgibt. Sie können den Simulator im Projekt [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) ändern, wenn Sie das GitHub-Repository verzweigt haben.

### Verfügbare Standorte für simulierte Geräte

Die Standorte befinden sich standardmäßig in Seattle/Redmond, Washington, USA. Diese Standorte können in [SampleDeviceFactory.cs][lnk-sample-device-factory] geändert werden.


### Erstellen und Verwenden eines eigenen (physischen) Geräts

Die [Azure IoT-SDKs](https://github.com/Azure/azure-iot-sdks) bieten Bibliotheken zum Verbinden zahlreicher Gerätetypen (Sprachen und Betriebssysteme) mit IoT-Lösungen.

## Ändern von Dashboardgrenzwerten

### Anzahl angezeigter Geräte im Dropdownfeld des Dashboards

Standardwert: 200. Diese Zahl kann in [DashboardController.cs][lnk-dashboard-controller] geändert werden.

### Anzahl angezeigter Pins im Bing Karten-Steuerelement

Standardwert: 200. Diese Zahl kann in [TelemetryApiController.cs][lnk-telemetry-api-controller-01] geändert werden.

### Zeitraum der Telemetriegrafik

Standardwert: 10 Minuten. Dieser Wert kann in [TelemetryApiController.cs][lnk-telemetry-api-controller-02] geändert werden.

## Manuelles Einrichten der Anwendungsrollen

Das folgende Verfahren beschreibt das Hinzufügen von **Admin**- und **ReadOnly**-Anwendungsrollen zu einer vorkonfigurierten Lösung. In von „azureiotsuite.com“ bereitgestellten, vorkonfigurierten Lösungen sind die **Admin**- und die **ReadOnly**-Rolle bereits enthalten.

Mitglieder der **ReadOnly**-Rolle können das Dashboard und die Geräteliste anzeigen, dürfen jedoch keine Geräte hinzufügen, Geräteattribute ändern oder Befehle senden. Mitglieder der **Admin**-Rolle haben uneingeschränkten Zugriff auf alle Funktionen in der Lösung.

1. Rufen Sie das [klassische Azure-Portal][lnk-classic-portal] auf.

2. Wählen Sie **Active Directory** aus.

3. Klicken Sie auf den Namen des AAD-Mandanten, den Sie bei der Bereitstellung Ihrer Lösung verwendet haben.

4. Klicken Sie auf **Anwendungen**.

5. Klicken Sie auf den Namen der Anwendung, der mit dem Namen der vorkonfigurierten Lösung übereinstimmt. Sollte Ihre Anwendung nicht in der Liste angezeigt werden, wählen Sie in der Dropdownliste **Anzeigen** die Option **Anwendungen im Besitz meines Unternehmens** aus, und klicken Sie auf das Häkchen.

6.  Klicken Sie unten auf der Seite auf **Manifest verwalten** und dann auf **Manifest herunterladen**.

7. Dadurch wird eine JSON-Datei auf Ihren lokalen Computer heruntergeladen. Öffnen Sie diese Datei zur Bearbeitung in einem Text-Editor Ihrer Wahl.

8. In der dritten Zeile der JSON-Datei steht:

  ```
  "appRoles" : [],
  ```
  Ersetzen Sie dies durch Folgendes:

  ```
  "appRoles": [
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Administrator access to the application",
  "displayName": "Admin",
  "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
  "isEnabled": true,
  "value": "Admin"
  },
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Read only access to device information",
  "displayName": "Read Only",
  "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
  "isEnabled": true,
  "value": "ReadOnly"
  } ],
  ```

9. Speichern Sie die aktualisierte JSON-Datei (Sie können die vorhandene Datei überschreiben).

10.  Wählen Sie im Azure-Verwaltungsportal am unteren Seitenrand die Option **Manifest verwalten** und anschließend **Manifest hochladen** aus, um die zuvor gespeicherte JSON-Datei hochzuladen.

11. Sie haben nun die **Admin**- und die **ReadOnly**-Rolle Ihrer Anwendung hinzugefügt.

12. Unter [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions] erfahren Sie, wie Sie einem Benutzer in Ihrem Verzeichnis eine dieser Rollen zuweisen.

## Feedback

Möchten Sie Anpassungsvorschläge zu diesem Dokument machen? Nutzen Sie [UserVoice](https://feedback.azure.com/forums/321918-azure-iot), um neue Features vorzuschlagen, oder verfassen Sie weiter unten einen Kommentar zu diesem Artikel.

## Nächste Schritte

Weitere Informationen zu IoT-Geräten finden Sie auf der [Azure IoT-Entwicklerwebsite](https://azure.microsoft.com/develop/iot/), die entsprechende Links und Dokumente enthält.

[IoT-Geräte-SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com

<!---HONumber=AcomDC_0629_2016-->