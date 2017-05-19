---
title: "Versionsanmerkungen für Application Insights | Microsoft Docs"
description: "Fügen Sie den Diagrammen im Metrik-Explorer in Application Insights Bereitstellungs- oder Buildmarker hinzu."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 2d568a38f66605da63054d1797ce23653e6204d4
ms.openlocfilehash: ff5d64bc00bc5fb3617160db852c7d08f8b3677b
ms.contentlocale: de-de
ms.lasthandoff: 01/20/2017


---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Anmerkungen zu Metrik-Diagrammen in Application Insights
Die Anmerkungen in den Diagrammen des [Metrik-Explorers](app-insights-metrics-explorer.md) geben an, wo Sie einen neuen Build oder ein anderes wichtiges Ereignis bereitgestellt haben. Dank dieser Anmerkungen sehen Sie auf einen Blick, ob Ihre Änderungen Auswirkungen auf die Leistung Ihrer Anwendung hatten. Sie können automatisch vom [Buildsystem von Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs) erstellt werden. Es können auch Anmerkungen erstellt werden, die alle gewünschten Ereignisse markieren, indem diese [aus PowerShell erstellt werden](#create-annotations-from-powershell).

![Beispiel für Anmerkungen mit sichtbarer Korrelation mit Serverantwortzeit](./media/app-insights-annotations/00.png)



## <a name="release-annotations-with-vsts-build"></a>Versionsanmerkungen mit VSTS-Build

Versionsanmerkungen sind ein Feature des cloudbasierten Build- und Versionsdiensts von Visual Studio Team Services. 

### <a name="install-the-annotations-extension-one-time"></a>Installieren der Erweiterung für Anmerkungen (einmalig)
Um Versionsanmerkungen erstellen zu können, müssen Sie eine der zahlreichen Team Service-Erweiterungen installieren, die im Visual Studio Marketplace zur Verfügung stehen.

1. Melden Sie sich bei Ihrem [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online)-Projekt an.
2. Laden Sie im Visual Studio-Marketplace die [Erweiterung Release Annotations](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations)herunter, und fügen Sie sie Ihrem Team Services-Konto hinzu.

![Öffnen Sie oben rechts auf der Team Services-Website den Marketplace. Wählen Sie „Visual Team Services“ und klicken Sie anschließend unter „Build und Version“ auf „Mehr Informationen“.](./media/app-insights-annotations/10.png)

Diesen Vorgang müssen Sie nur einmal für Ihr Visual Studio Team Services-Konto ausführen. Versionsanmerkungen können nun für jedes Projekt in Ihrem Konto konfiguriert werden. 

### <a name="configure-release-annotations"></a>Konfigurieren von Versionsanmerkungen

Es wird ein separater API-Schlüssel für jede VSTS-Versionsvorlage benötigt.

1. Melden Sie sich im [Microsoft Azure-Portal](https://portal.azure.com) an, und öffnen Sie die Application Insights-Ressource, mit der Sie Ihre Anwendung überwachen. (Oder [erstellen Sie jetzt eine Ressource](app-insights-overview.md), sofern noch nicht geschehen.)
2. Öffnen Sie **API-Zugriff**, **Application Insights-ID**.
   
    ![Öffnen Sie unter „portal.azure.com“ die Application Insights-Ressource, und wählen Sie „Einstellungen“. Öffnen Sie „API-Zugriff“. Kopieren der Anwendungs-ID](./media/app-insights-annotations/20.png)

4. Öffnen (oder erstellen) Sie in einem separaten Browserfenster die Versionsvorlage, mit der Ihre Bereitstellungen über Visual Studio Team Services verwaltet werden. 
   
    Fügen Sie eine Aufgabe hinzu, und wählen Sie im Menü die Aufgabe für Application Insights-Versionsanmerkungen aus.
   
    Fügen Sie die **Anwendungs-ID** ein, die Sie vom Blatt „API-Zugriff“ kopiert haben.
   
    ![Öffnen Sie in Visual Studio Team Services die Option „Version“, wählen Sie eine Versionsdefinition aus, und wählen Sie „Bearbeiten“. Klicken Sie auf „Aufgabe hinzufügen“, und wählen Sie die Option „Application Insights-Versionsanmerkung“. Fügen Sie die Application Insights-ID ein.](./media/app-insights-annotations/30.png)
4. Legen Sie das Feld **APIKey** auf eine Variable (`$(ApiKey)`) fest.

5. Wechseln Sie zurück ins Azure-Fenster, erstellen Sie einen neuen API-Schlüssel, und erstellen Sie eine Kopie davon.
   
    ![Klicken Sie im Azure-Fenster auf dem Blatt „API-Zugriff“ auf „API-Schlüssel erstellen“. Geben Sie einen Kommentar an, aktivieren Sie das Schreiben von Anmerkungen, und klicken Sie auf „Schlüssel generieren“. Kopieren Sie den neuen Schlüssel.](./media/app-insights-annotations/40.png)

6. Öffnen Sie die Registerkarte „Konfiguration“ der Versionsvorlage.
   
    Erstellen Sie eine Variablendefinition für `ApiKey`.
   
    Fügen Sie Ihren API-Schlüssel in die ApiKey-Variablendefinition ein.
   
    ![Wählen Sie im Team Services-Fenster die Registerkarte „Konfiguration“, und klicken Sie auf „Variable hinzufügen“. Legen Sie den Namen auf „ApiKey“ fest, und fügen Sie unter „Wert“ den gerade generierten Schlüssel ein. Klicken Sie anschließend auf das Schlosssymbol.](./media/app-insights-annotations/50.png)
7. **Speichern** Sie abschließend die Versionsdefinition.


## <a name="view-annotations"></a>Anmerkungen anzeigen
Wenn Sie nun diese Versionsvorlage zum Bereitstellen einer neuen Version verwenden, wird jedes Mal eine Anmerkung an Application Insights gesendet. Die Anmerkungen werden in Diagrammen im Metrik-Explorer angezeigt.

Klicken Sie auf einen Anmerkungsmarker, um Details zur Version, einschließlich Anfordernder, Quellcodeverwaltungsverzweigung, Versionsdefinition, Umgebung usw., anzuzeigen.

![Klicken Sie auf einen Versionsanmerkungsmarker.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a>Erstellen von benutzerdefinierten Anmerkungen in PowerShell
Sie können Anmerkungen auch von jedem beliebigen Prozess erstellen (ohne Visual Studio Team System zu verwenden). 


1. Erstellen Sie eine lokale Kopie des [PowerShell-Skript aus GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Rufen Sie die Anwendungs-ID ab und erstellen Sie auf dem Blatt „API-Zugriff“ einen API-Schlüssel .

3. Nennen Sie das Skript folgendermaßen:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

Änderungen am Skript können einfach vorgenommen werden, um beispielsweise frühere Anmerkungen zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Arbeitselements](app-insights-diagnostic-search.md#create-work-item)
* [Automation mit PowerShell](app-insights-powershell.md)

