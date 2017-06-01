---
title: Trennen der Telemetrie von Entwicklung, Testen und Release in Azure Application Insights | Microsoft Docs
description: "Leiten Sie Telemetriedaten für Entwicklungs-, Test- und Produktionsabläufe an verschiedene Ressourcen."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 578e30f0-31ed-4f39-baa8-01b4c2f310c9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 125f05f5dce5a0e4127348de5b280f06c3491d84
ms.openlocfilehash: 0ee19b46a0e882571f6193471be69fa097d98936
ms.contentlocale: de-de
ms.lasthandoff: 05/22/2017


---
# <a name="separating-telemetry-from-development-test-and-production"></a>Trennen der Telemetriedaten von Entwicklung, Test und Produktion

Wenn Sie die nächste Version einer Webanwendung entwickeln, möchten Sie die [Application Insights](app-insights-overview.md)-Telemetriedaten der neuen Version nicht mit denen der bereits veröffentlichten Version verwechseln. Um Verwirrung zu vermeiden, senden Sie die Telemetriedaten aus verschiedenen Entwicklungsphasen mit separaten Instumentierungsschlüsseln (ikeys) an getrennte Application Insights-Ressourcen. Um das Ändern des Instrumentierungsschlüssels beim Fortschreiten einer Version von einer Phase zur nächsten zu erleichtern, kann es sinnvoll sein, den ikey im Code statt in der Konfigurationsdatei zu platzieren. 

(Wenn Ihr System ein Azure-Clouddienst ist, gibt es [eine weitere Methode zum Festlegen von separaten iKeys](app-insights-cloudservices.md).)

## <a name="about-resources-and-instrumentation-keys"></a>Informationen zu Ressourcen und Instrumentierungsschlüsseln

Wenn Sie Application Insights-Überwachung für Ihre Web App einrichten, erstellen Sie in Microsoft Azure eine Application Insights-*Ressource*. Sie öffnen diese Ressource im Azure-Portal, um die in Ihrer App gesammelten Telemetriedaten anzuzeigen und zu analysieren. Die Ressource wird durch einen *Instrumentierungsschlüssel* (ikey) identifiziert. Wenn Sie das Application Insights-Paket installieren, um Ihre App zu überwachen, konfigurieren Sie sie mit diesem Instrumentierungsschlüssel und teilen ihr so mit, wohin die Telemetriedaten gesendet werden sollen.

Normalerweise entscheiden Sie sich in unterschiedlichen Szenarien für die Verwendung von getrennten Ressourcen oder einer einzelnen, geteilten Ressource:

* Verschiedene, unabhängige Anwendungen: Verwenden Sie eine separate Ressource und einen separaten ikey für jede App.
* Mehrere Komponenten oder Rollen der gleichen Geschäftsanwendung – Verwenden Sie eine [einzelne geteilte Ressource](app-insights-monitor-multi-role-apps.md) für alle Komponentenanwendungen. Die Telemetrie kann anhand der cloud_RoleName-Eigenschaft gefiltert oder aufgeteilt werden.
* Entwicklung, Testen und Release: Verwenden Sie eine separate Ressource und separate ikeys für Versionen des Systems je nach Produktionsabschnitt oder -phase.
* A | B-Tests: Verwenden Sie eine einzelne Ressource. Erstellen Sie einen Telemetrie-Initialisierer, um den Telemetriedaten eine Eigenschaft zum Identifizieren der Varianten hinzuzufügen.


## <a name="dynamic-ikey"></a> Dynamischer Instrumentierungsschlüssel

Um das Ändern des ikeys beim Fortschreiten des Codes von einer Produktionsphase zur nächsten zu erleichtern, legen Sie ihn im Code statt in der Konfigurationsdatei fest.

Legen Sie den Schlüssel in einer Initialisierungsmethode fest, wie z. B. "global.aspx.cs" in einem ASP.NET-Dienst:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

In diesem Beispiel werden die iKeys für die verschiedenen Ressourcen in verschiedenen Versionen der Webkonfigurationsdatei platziert. Wenn Sie die Webkonfigurationsdatei austauschen – z.B. im Rahmen des Releaseskripts – wird die Zielressource ausgetauscht.

### <a name="web-pages"></a>Webseiten
Der iKey wird auch in Webseiten Ihrer App in dem [Skript verwendet, das Sie auf dem Blatt "Schnellstart" erhalten haben](app-insights-javascript.md). Statt ihn direkt im Skript zu programmieren, generieren Sie ihn über den Serverzustand. Beispielsweise in einer ASP.NET-App:

*JavaScript in Razor*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a>Erstellen zusätzlicher Application Insights-Ressourcen
Um die Telemetriedaten für verschiedene Anwendungskomponenten oder für verschiedene Einsatzzwecke der gleichen Komponente (Entwicklung/Test und Produktion) zu trennen, müssen Sie eine neue Application Insights-Ressource erstellen.

Fügen Sie unter [portal.azure.com](https://portal.azure.com)eine neue Application Insights-Ressource hinzu:

![Klicken Sie auf "Neu > Application Insights"](./media/app-insights-separate-resources/01-new.png)

* **Anwendungstyp** bestimmt den Inhalt des Blatts "Übersicht" und die im [Metrik-Explorer](app-insights-metrics-explorer.md)verfügbaren Eigenschaften. Wenn Ihr App-Typ nicht angezeigt wird, wählen Sie einen der Webtypen für Webseiten aus.
* **Ressourcengruppe** ist eine benutzerfreundliche Möglichkeit zum Verwalten von Eigenschaften wie der [Zugriffssteuerung](app-insights-resources-roles-access-control.md)gespeichert und verarbeitet. Sie können separate Ressourcengruppen für Entwicklungs-, Test- und Produktionsumgebungen verwenden.
* **Abonnement** ist Ihr Zahlungskonto in Azure.
* **Speicherort** ist der Ort, an dem Ihre Daten aufbewahrt werden. Dieser kann derzeit nicht geändert werden. 
* **Zu Dashboard hinzufügen** legt eine Schnellzugriffskachel für die Ressource auf Ihrer Azure-Startseite ab. 

Das Erstellen der Ressource dauert einige Sekunden. Wenn es abgeschlossen ist, sehen Sie eine Warnung.

(Sie können ein [PowerShell-Skript](app-insights-powershell-script-create-resource.md) schreiben, um eine Ressource automatisch zu erstellen.)

### <a name="getting-the-instrumentation-key"></a>Abrufen des Instrumentierungsschlüssels
Der Instrumentierungsschlüssel identifiziert die Ressource, die Sie erstellt haben. 

![Klicken Sie auf "Essentials", klicken Sie auf den Instrumentierungsschlüssel, STRG+C.](./media/app-insights-separate-resources/02-props.png)

Sie benötigen die Instrumentierungsschlüssel aller Ressourcen, an die Ihre App Daten sendet.

## <a name="filter-on-build-number"></a>Filtern nach Buildnummer
Wenn Sie eine neue Version Ihrer App veröffentlichen, sollten Sie die Telemetrie aus verschiedenen Builds trennen können.

Sie können die Eigenschaft „Anwendungsversion“ festlegen, sodass Sie die Ergebnisse in der [Suche](app-insights-diagnostic-search.md) und im [Metrik-Explorer](app-insights-metrics-explorer.md) filtern können.

![Filtern nach einer Eigenschaft](./media/app-insights-separate-resources/050-filter.png)

Es gibt verschiedene Methoden, um die Eigenschaft "Anwendungsversion" festzulegen.

* Direktes Festlegen:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* Umschließen Sie diese Zeile in einem [Telemetrieinitialisierer](app-insights-api-custom-events-metrics.md#defaults) , um sicherzustellen, dass alle TelemetryClient-Instanzen einheitlich festgelegt werden.
* [ASP.NET] Legen Sie die Version in `BuildInfo.config`fest. Das Webmodul übernimmt die Version aus dem Knoten "BuildLabel". Schließen Sie diese Datei in Ihr Projekt ein, und denken Sie daran, die Eigenschaft "Immer kopieren" im Projektmappen-Explorer festzulegen.

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] Generieren Sie "BuildInfo.config" automatisch in MSBuild. Fügen Sie zu diesem Zweck Ihrer `.csproj`-Datei einige Zeilen hinzu:

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Hierdurch wird eine Datei mit dem Namen " *yourProjectName*.BuildInfo.config" generiert. Der Veröffentlichungsprozess benennt diese Datei in "BuildInfo.config" um.

    Die Buildbezeichnung enthält einen Platzhalter (AutoGen_...), wenn Sie für die Erstellung Visual Studio verwenden. Bei der Erstellung mit MSBuild wird der Name jedoch mit der richtigen Versionsnummer aufgefüllt.

    Um das Generieren von Versionsnummern in MSBuild zu ermöglichen, legen Sie in "AssemblyReference.cs" die Version fest, z. B. `1.0.*`.

## <a name="version-and-release-tracking"></a>Versionsnachverfolgung
Stellen Sie für die Nachverfolgung der Anwendungsversion sicher, dass `buildinfo.config` über den Prozess Ihres Microsoft-Buildmoduls generiert wird. Fügen Sie in Ihrer CSPROJ-Datei Folgendes hinzu:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Wenn das Webmodul Application Insights über die Buildinformationen verfügt, fügt es jedem Telemetrieelement automatisch die **Anwendungsversion** als Eigenschaft hinzu. Dies ermöglicht es Ihnen, nach der Version zu filtern, wenn Sie [Diagnosesuchen](app-insights-diagnostic-search.md) durchführen oder [Metriken untersuchen](app-insights-metrics-explorer.md).

Beachten Sie aber, dass die Buildversionsnummer nur vom Microsoft-Buildmodul generiert wird, und nicht vom Entwicklerbuild in Visual Studio.

### <a name="release-annotations"></a>Versionsanmerkungen
Bei Verwendung von Visual Studio Team Services können Sie Ihren Diagrammen einen [Anmerkungsmarker](app-insights-annotations.md) hinzufügen lassen, wenn Sie eine neue Version veröffentlichen. In der folgenden Abbildung ist dargestellt, wie dieser Marker angezeigt wird.

![Screenshot eines Beispiels für eine Versionsanmerkung in einem Diagramm](./media/app-insights-asp-net/release-annotation.png)
## <a name="next-steps"></a>Nächste Schritte

* [Freigegebene Ressourcen für mehrere Rollen](app-insights-monitor-multi-role-apps.md)
* [Erstellen eines Telemetrie-Initialisierers zur Unterscheidung von A|B-Varianten](app-insights-api-filtering-sampling.md#add-properties)

