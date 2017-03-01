---
title: Streamingprotokolle und Konsole
description: "Übersicht über Streamingprotokolle und die Konsole"
author: btardif
manager: erikre
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: 3e50a287-781b-4c6a-8c53-eec261889d7a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/12/2016
ms.author: byvinyal
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: 120ce6b115820728b9f853e9ff349beb0ef29c9d
ms.lasthandoff: 03/01/2017


---
# <a name="streaming-logs-and-the-console"></a>Streamingprotokolle und die Konsole
## <a name="streaming-logs"></a>Streamingprotokolle
Das **Azure-Portal** bietet ein integriertes Anzeigeprogramm für das Streamingprotokoll, über das Sie Ablaufverfolgungsereignisse von Ihren **App Service**-Apps in Echtzeit anzeigen können.  

Dieses Feature kann mit einigen einfachen Schritten eingerichtet werden:

* Schreiben Sie Ablaufverfolgungen in Ihren Code.
* Aktivieren Sie **Diagnoseprotokolle** für Ihre App.
* Zeigen Sie den Datenstrom in der im **Azure-Portal** integrierten Benutzeroberfläche für **Streamingprotokolle** an.

### <a name="how-to-write-traces-in-your-code"></a>So schreiben Sie Ablaufverfolgungen in den Code:
Ablaufverfolgungen in den Code zu schreiben ist einfach.  In C# könnte dieser Code z. B. wie folgt aussehen:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Die Ablaufverfolgungsklasse befindet sich in der System Diagnostics-Namespace.

In einer node.js-App können Sie folgenden Code schreiben, um das gleiche Ergebnis zu erhalten:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>Aktivieren und Anzeigen der Streamingprotokolle
![][BrowseSitesScreenshot] Diagnosen werden pro App aktiviert. Navigieren Sie zunächst zu der Website, für die Sie das Feature aktivieren möchten.  

![][DiagnosticsLogs] Scrollen Sie im Menü „Einstellungen“ nach unten zum Abschnitt **Überwachung**, und klicken Sie auf **(1) Diagnoseprotokolle**. **(2) Aktivieren** Sie dann **Anwendungsprotokollierung (Dateisystem)** oder **Anwendungsprotokollierung (BLOB)**. Mit der Option **Ebene** können Sie den Schweregrad der Ablaufverfolgungen ändern, die erfasst werden sollen. Legen Sie die Ebene auf **Ausführlich** fest, wenn es nur darum geht, sich mit dem Feature vertraut zu machen, um sicherzustellen, dass alle Ablaufverfolgungen erfasst werden.

Klicken Sie oben auf dem Blade auf **SPEICHERN** , um die Protokolle anzuzeigen.

> [!NOTE]
> Je höher der **Schweregrad**, desto mehr Ressourcen werden für die Protokollierung belegt und desto mehr Ablaufverfolgungen werden generiert. Stellen Sie sicher, dass **Schweregrad** für die richtige Ausführlichkeit für eine Produktionswebsite oder eine Website mit hohem Datenverkehr konfiguriert ist. 
> 
> 

![][StreamingLogsScreenshot] Zum Anzeigen der **Streamingprotokolle** im Azure-Portal klicken Sie auf **(1) Protokollstream**, ebenfalls im Abschnitt **Überwachung** des Menüs „Einstellungen“. Wenn Ihre App aktiv Ablaufverfolgungsanweisungen verzeichnet, sollten diese nahezu in Echtzeit in der **(2) Benutzeroberfläche für Streamingprotokolle** angezeigt werden.

## <a name="console"></a>Konsole
Das **Azure-Portal** bietet Konsolenzugriff auf Ihre App. Sie können das Dateisystem der App untersuchen und PowerShell- und Cmd-Skripts ausführen. Sie müssen sich beim Ausführen von Konsolenbefehlen an die gleichen Berechtigungen halten, die auch für den ausgeführten App-Code gelten. Der Zugriff auf geschützte Verzeichnisse oder ausgeführte Skripts, für die erweiterte Berechtigungen erforderlich sind, ist blockiert.  

![][ConsoleScreenshot] Scrollen Sie im Menü „Einstellungen“ nach unten zum Abschnitt **Entwicklungstools**, und klicken Sie auf **(1) Konsole**. Die Benutzeroberfläche der **(2) Konsole** wird dann rechts geöffnet.

Testen Sie z.B. folgende Befehle, um sich mit der **Konsole** vertraut zu machen:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png

