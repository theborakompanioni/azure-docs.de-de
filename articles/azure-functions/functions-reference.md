---
title: Leitfaden zur Entwicklung von Azure Functions | Microsoft-Dokumentation
description: "Lernen Sie die Konzepte und Techniken der Azure Functions kennen, die Sie benötigen, um alle Programmiersprachen und Bindungen übergreifend Funktionen in Azure zu entwickeln."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: Entwicklerhandbuch, Azure Functions, Funktionen, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: chrande
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: 879be48150cfe13e31064475aa637f13f5f5f9d5
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="azure-functions-developers-guide"></a>Azure Functions: Entwicklerhandbuch
In Azure Functions nutzen bestimmte Funktionen einige wichtige technische Konzepte und Komponenten gemeinsam, unabhängig von der verwendeten Sprache oder Bindung. Bevor Sie sich mit den spezifischen Details einer bestimmten Sprache oder Bindung beschäftigen, sollten Sie diese Übersicht lesen, die für alle Funktionen gilt.

In diesem Artikel wird davon ausgegangen, dass Sie die [Übersicht zu Azure Functions](functions-overview.md) gelesen haben und mit [WebJobs SDK-Konzepten wie Triggern, Bindungen und der JobHost-Laufzeit](../app-service-web/websites-dotnet-webjobs-sdk.md) vertraut sind. Azure Functions basiert auf dem WebJobs SDK 

## <a name="function-code"></a>Funktionscode
Ein *Funktion* ist das primäre Konzept in Azure Functions. Sie schreiben Code für eine Funktion in der Sprache Ihrer Wahl und speichern die Code- und Konfigurationsdateien im gleichen Ordner. Die Konfigurationsdatei heißt `function.json` und enthält JSON-Konfigurationsdaten. Verschiedene Sprachen werden unterstützt, und für jede Sprache gibt es leicht voneinander abweichende Optionen, die sich für die jeweilige Sprache am besten eignen. 

Die Datei „function.json“ definiert die Funktionsbindungen und weitere Konfigurationseinstellungen. Die Laufzeit verwendet diese Datei, um zu ermitteln, welche Ereignisse überwacht werden sollen und wie Daten in die Funktionsausführung übergeben und aus dieser zurückgegeben werden. Im Folgenden finden Sie eine function.json-Beispieldatei.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Legen Sie die `disabled`-Eigenschaft auf `true` fest, um die Ausführung der Funktion zu verhindern.

In der Eigenschaft `bindings` werden sowohl Trigger als auch Bindungen konfiguriert. Einige Einstellungen gelten für alle Bindungen, einige Einstellungen sind spezifisch für einen bestimmten Bindungstyp. Jede Bindung erfordert folgende Einstellungen:

| Eigenschaft | Werte/Typen | Kommentare |
| --- | --- | --- |
| `type` |string |Bindungstyp. Beispiel: `queueTrigger`. |
| `direction` |'in', 'out' |Gibt an, ob die Bindung zum Empfangen von Daten in der Funktion oder zum Senden von Daten aus der Funktion dient. |
| `name` |string |Der Name, der für die gebundenen Daten in der Funktion verwendet wird. In C# ist dies ein Argumentname, in JavaScript ist es der Schlüssel in einer Schlüssel-Wert-Liste. |

## <a name="function-app"></a>Funktionen-App
Eine Funktionen-App besteht aus einer oder mehreren individuellen Funktionen, die zusammen vom Azure App Service verwaltet werden. Der Tarif, die kontinuierliche Bereitstellung und die Laufzeitversion sind für alle Funktionen in einer Funktionen-App gleich. Funktionen, die in mehreren Sprachen geschrieben wurden, können dieselbe Funktionen-App nutzen. Eine Funktionen-App ist somit eine Möglichkeit, mit der Sie Ihre Funktionen organisieren und kollektiv verwalten können. 

## <a name="runtime-script-host-and-web-host"></a>Runtime (Skripthost und Webhost)
Die Laufzeit bzw. der Skripthost ist der zugrunde liegende WebJobs SDK-Host, der auf Ereignisse lauscht, Daten sammelt und sendet und letztendlich den Code ausführt. 

Um HTTP-Trigger zu ermöglichen, gibt es auch einen Webhost, der in Produktionsszenarien vor dem Skripthost platziert ist. Durch die Verwendung von zwei Hosts kann der Skripthost vom Front-End-Datenverkehr isoliert werden, der vom Webhost verwaltet wird.

## <a name="folder-structure"></a>Ordnerstruktur
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Wenn Sie ein Projekt einrichten, um Funktionen für eine Funktionen-App in Azure App Service bereitzustellen, können Sie diese Ordnerstruktur als Sitecode behandeln. Sie können vorhandene Tools, wie z.B. die fortlaufende Integration und Bereitstellung, oder benutzerdefinierte Bereitstellungsskripts verwenden, um eine Paketinstallation zur Bereitstellungszeit oder eine Codetranspilierung umzusetzen.

> [!NOTE]
> Stellen Sie sicher, dass Sie Ihre `host.json`-Datei und die Funktionsordner direkt im Ordner `wwwroot` bereitstellen. Schließen Sie den Ordner `wwwroot` nicht in Ihre Bereitstellungen ein. Andernfalls erhalten Sie `wwwroot\wwwroot`-Ordner. 
> 
> 

## <a id="fileupdate"></a> Aktualisieren von Funktionen-App-Dateien
Der im Azure-Portal integrierte Funktionen-Editor ermöglicht das Aktualisieren der Datei *function.json* und der Codedatei für eine Funktion. Zum Hochladen oder Aktualisieren anderer Dateien wie *package.json* oder *project.json* oder von Abhängigkeiten müssen Sie andere Bereitstellungsmethoden nutzen.

Funktionen-Apps basieren auf App Service. Daher stehen alle [für Standard-Web-Apps verfügbaren Bereitstellungsoptionen](../app-service-web/web-sites-deploy.md) auch für Funktionen-Apps zur Verfügung. Es folgen einige Methoden, die Sie zum Hochladen oder Aktualisieren von Funktionen-App-Dateien befolgen können. 

#### <a name="to-use-app-service-editor"></a>So verwenden Sie den App Service-Editor
1. Klicken Sie im Azure Functions-Portal auf **Funktionen-App-Einstellungen**.
2. Klicken Sie im Abschnitt **Erweiterte Einstellungen** auf **Zu App Service-Einstellungen** wechseln.
3. Klicken Sie im App-Menü „Nav“ unter **ENTWICKLUNGSTOOLS** auf **App Service-Editor**.
4. Klicken Sie auf **Start**.
   
   Nachdem der App Service-Editor geladen wurde, sehen Sie die Datei *host.json* und die Funktionenordner unter *wwwroot*. 
5. Öffnen Sie die Dateien, um Sie zu bearbeiten, oder laden Sie Dateien per Drag & Drop von Ihrem Entwicklungscomputer hoch.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>So verwenden Sie den SCM-Endpunkt (Kudu) der Funktionen-App
1. Navigieren Sie zu `https://<function_app_name>.scm.azurewebsites.net`.
2. Klicken Sie auf **Debugkonsole > CMD**.
3. Navigieren Sie zu `D:\home\site\wwwroot\`, um *host.json* zu aktualisieren, oder zu `D:\home\site\wwwroot\<function_name>`, um die Dateien einer Funktion zu aktualisieren.
4. Verschieben Sie eine Datei per Drag & Drop, die Sie im Dateiraster in den entsprechenden Ordner hochladen möchten. Es gibt zwei Bereiche im Dateiraster, in denen Sie eine Datei ablegen können. Für *ZIP* -Dateien wird ein Feld mit der Bezeichnung „Zum Hochladen und Entzippen hier ablegen“ angezeigt. Legen Sie Dateien anderer Dateitypen im Dateiraster ab, jedoch außerhalb des Felds zum Entzippen.

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --DonnaM -->

#### <a name="to-use-continuous-deployment"></a>Verwenden der kontinuierlichen Bereitstellung
Befolgen Sie die Anweisungen im Thema [Continuous deployment for Azure Functions](functions-continuous-deployment.md)(Kontinuierliche Bereitstellung für Azure Functions).

## <a name="parallel-execution"></a>Parallele Ausführung
Wenn die Auslösung mehrerer Ereignisse schneller erfolgt als die Runtime einer Singlethreadfunktion sie verarbeiten kann, kann die Runtime die Funktion mehrmals parallel aufrufen.  Wenn eine Funktionen-App den [verbrauchsbasierten Hostingplan](functions-scale.md#how-the-consumption-plan-works) verwendet, kann die App automatisch horizontal hochskaliert werden.  Jede Instanz der Funktionen-App – unabhängig davon, ob die App im verbrauchsbasierten Hostingplan oder einem regulären [App Service-Hostingplan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) ausgeführt wird – kann gleichzeitige Funktionsaufrufe über mehrere Threads parallel verarbeiten.  Die maximale Anzahl gleichzeitiger Funktionsaufrufe in jeder Funktionen-App-Instanz variiert je nach Art des verwendeten Triggers sowie je nach den Ressourcen, die von anderen Funktionen innerhalb der Funktionen-App verwendet werden.

## <a name="functions-runtime-versioning"></a>Versionsverwaltung der Functions-Runtime

Sie können die Version der Functions-Runtime mit der App-Einstellung `FUNCTIONS_EXTENSION_VERSION` konfigurieren. Der Wert „~1“ bedeutet beispielsweise, dass für Ihre Funktionen-App „1“ als deren Hauptversion verwendet wird. Funktionen-Apps werden auf jede neue Nebenversion aktualisiert, wenn sie freigegeben werden. Die genaue Version Ihrer Funktionen-App können Sie auf der Registerkarte **Einstellungen** im Azure-Portal anzeigen.

## <a name="repositories"></a>Repositorys
Der Code für Azure Functions ist Open Source und in GitHub-Repositorys gespeichert:

* [Azure Functions-Laufzeit](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure Functions-Portal](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure Functions-Vorlagen](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK-Erweiterungen](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Bindungen
Hier finden Sie eine Tabelle aller unterstützten Bindungen.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Melden von Problemen
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [C#-Entwicklerreferenz zu Azure Functions](functions-reference-csharp.md)
* [F#-Entwicklerreferenz zu Azure Functions](functions-reference-fsharp.md)
* [NodeJS-Entwicklerreferenz zu Azure Functions](functions-reference-node.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)
* [Azure Functions: The Journey](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) im Blog des Azure App Service-Teams. Überblick, wie Azure Functions entwickelt wurde.


