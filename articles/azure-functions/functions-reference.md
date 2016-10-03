<properties
	pageTitle="Entwicklerreferenz zu Azure Functions | Microsoft Azure"
	description="Lernen Sie die Azure Functions-Konzepte und -Komponenten kennen, die allen Sprachen und Bindungen gemeinsam sind."
	services="functions"
	documentationCenter="na"
	authors="christopheranderson"
	manager="erikre"
	editor=""
	tags=""
	keywords="Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur"/>

<tags
	ms.service="functions"
	ms.devlang="multiple"
	ms.topic="reference"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="05/13/2016"
	ms.author="chrande"/>

# Entwicklerreferenz zu Azure Functions

Azure Functions nutzen einige wichtige gemeinsame technische Konzepte und Komponenten, unabhängig von der verwendeten Sprache oder Bindung. Bevor Sie sich mit den spezifischen Details einer bestimmten Sprache oder Bindung beschäftigen, sollten Sie diese Übersicht lesen, die für alle Funktionen gilt.

In diesem Artikel wird davon ausgegangen, dass Sie die [Übersicht zu Azure Functions](functions-overview.md) gelesen haben und mit [WebJobs SDK-Konzepten wie Triggern, Bindungen und der JobHost-Laufzeit](../app-service-web/websites-dotnet-webjobs-sdk.md) vertraut sind. Azure Functions basiert auf dem WebJobs SDK


## Funktionscode

Ein *Funktion* ist das primäre Konzept in Azure Functions. Sie schreiben Code in der Sprache Ihrer Wahl und speichern die Codedatei(en) sowie eine Konfigurationsdatei im gleichen Ordner. Die Konfiguration erfolgt in JSON, und die Datei heißt `function.json`. Es wird eine Vielzahl von Sprachen unterstützt, und für jede Sprache gibt es leicht voneinander abweichende Optionen, die sich für die jeweilige Sprache am besten eignen.

Die Datei `function.json` enthält die für eine Funktion spezifische Konfiguration, einschließlich der Bindungen. Die Runtime liest diese Datei, um zu ermitteln, welche Ereignisse ausgelöst, welche Daten beim Funktionsaufruf eingeschlossen und wohin die Daten gesendet werden müssen, die von der Funktion selbst übergeben werden.

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

Sie können verhindern, dass die Runtime die Funktion ausführt, indem Sie die Eigenschaft `disabled` auf `true` festlegen.

In der Eigenschaft `bindings` werden sowohl Trigger als auch Bindungen konfiguriert. Einige Einstellungen gelten für alle Bindungen, einige Einstellungen sind spezifisch für einen bestimmten Bindungstyp. Jede Bindung erfordert folgende Einstellungen:

|Eigenschaft|Werte/Typen|Kommentare|
|---|-----|------|
|`type`|string|Bindungstyp. Beispiel: `queueTrigger`.
|`direction`|'in', 'out'| Gibt an, ob die Bindung zum Empfangen von Daten in der Funktion oder zum Senden von Daten aus der Funktion dient.
| `name` | string | Der Name, der für die gebundenen Daten in der Funktion verwendet wird. Bei C# ist dies ein Argumentname, in JavaScript ist es der Schlüssel in einer Schlüssel-Wert-Liste.

## Funktionen-App

Eine Funktionen-App besteht aus einer oder mehreren individuellen Funktionen, die zusammen vom Azure App Service verwaltet werden. Der Tarif, die kontinuierliche Bereitstellung und die Laufzeitversion sind für alle Funktionen in einer Funktionen-App gleich. Funktionen, die in mehreren Sprachen geschrieben wurden, können dieselbe Funktionen-App nutzen. Eine Funktionen-App ist somit eine Möglichkeit, mit der Sie Ihre Funktionen organisieren und kollektiv verwalten können.

## Runtime (Skripthost und Webhost)

Die Runtime bzw. der Skripthost ist der zugrunde liegende WebJobs SDK-Host, der auf Ereignisse lauscht, Daten sammelt und sendet und letztendlich den Code ausführt.

Um HTTP-Trigger zu ermöglichen, gibt es auch einen Webhost, der in Produktionsszenarien vor dem Skripthost platziert ist. Auf diese Weise kann der Skripthost vom Front-End-Datenverkehr isoliert werden, der vom Webhost verwaltet wird.

## Ordnerstruktur

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Wenn Sie ein Projekt einrichten, um Funktionen für eine Funktionen-App in Azure App Service bereitzustellen, können Sie diese Ordnerstruktur als Sitecode behandeln. Sie können vorhandene Tools, wie z.B. die fortlaufende Integration und Bereitstellung, oder benutzerdefinierte Bereitstellungsskripts verwenden, um eine Paketinstallation zur Bereitstellungszeit oder eine Codetranspilierung umzusetzen.

## <a id="fileupdate"></a> Aktualisieren von Funktionen-App-Dateien

Der im Azure-Portal integrierte Funktionen-Editor ermöglicht das Aktualisieren der Datei *function.json* und der Codedatei für eine Funktion. Zum Hochladen oder Aktualisieren anderer Dateien wie *package.json* oder *project.json* oder von Abhängigkeiten müssen Sie andere Bereitstellungsmethoden nutzen.

Funktionen-Apps basieren auf App Service. Daher stehen alle [für Standard-Web-Apps verfügbaren Bereitstellungsoptionen](../app-service-web/web-sites-deploy.md) auch für Funktionen-Apps zur Verfügung. Es folgen einige Methoden, die Sie zum Hochladen oder Aktualisieren von Funktionen-App-Dateien befolgen können.

#### So verwenden Sie den App Service-Editor

1. Klicken Sie im Azure Functions-Portal auf **Funktionen-App-Einstellungen**.

2. Klicken Sie im Abschnitt **Erweiterte Einstellungen** auf **Zu App Service-Einstellungen wechseln**.

3. Klicken Sie im App-Menü „Nav“ unter **ENTWICKLUNGSTOOLS** auf **App Service-Editor**.

4.  Klicken Sie auf **Start**.

	Nachdem der App Service-Editor geladen wurde, sehen Sie die Datei *host.json* und die Funktionenordner unter *wwwroot*.

5. Öffnen Sie die Dateien, um Sie zu bearbeiten, oder laden Sie Dateien per Drag & Drop von Ihrem Entwicklungscomputer hoch.

#### So verwenden Sie den SCM-Endpunkt (Kudu) der Funktionen-App

1. Navigieren Sie zu `https://<function_app_name>.scm.azurewebsites.net`.

2. Klicken Sie auf **Debugkonsole > CMD**.

3. Navigieren Sie zu `D:\home\site\wwwroot`, um *host.json* zu aktualisieren, oder zu `D:\home\site\wwwroot<function_name>`, um die Dateien einer Funktion zu aktualisieren.

4. Verschieben Sie eine Datei per Drag & Drop, die Sie im Dateiraster in den entsprechenden Ordner hochladen möchten. Es gibt zwei Bereiche im Dateiraster, in denen Sie eine Datei ablegen können. Für *ZIP*-Dateien wird ein Feld mit der Bezeichnung „Zum Hochladen und Entzippen hier ablegen“ angezeigt. Legen Sie Dateien anderer Dateitypen im Dateiraster ab, jedoch außerhalb des Felds zum Entzippen.

#### So verwenden Sie FTP

1. Führen Sie die [hier](../app-service-web/web-sites-deploy.md#ftp) aufgeführten Schritte aus, um FTP zu konfigurieren.

2. Wenn Sie mit der Website der Funktionen-App verbunden sind, kopieren Sie die aktualisierte Datei *host.json* in `/site/wwwroot`, oder kopieren Sie Funktionsdateien in `/site/wwwroot/<function_name>`.

#### Verwenden der kontinuierlichen Bereitstellung

Befolgen Sie die Anweisungen im Thema [Continuous deployment for Azure Functions](functions-continuous-deployment.md) (Kontinuierliche Bereitstellung für Azure Functions).

## Parallele Ausführung

Wenn die Auslösung mehrerer Ereignisse schneller erfolgt als die Runtime einer Singlethreadfunktion sie verarbeiten kann, kann die Runtime die Funktion mehrmals parallel aufrufen. Wenn eine Funktionen-App den [dynamischen Serviceplan](functions-scale.md#dynamic-service-plan) verwendet, kann die App automatisch horizontal hochskaliert werden. Jede Instanz der Funktionen-App – unabhängig davon, ob die App im dynamischen Serviceplan oder einem regulären [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) ausgeführt wird – kann dann gleichzeitige Funktionsaufrufe über mehrere Threads parallel verarbeiten. Die maximale Anzahl gleichzeitiger Funktionsaufrufe in jeder Instanz der Funktionen-App variiert je nach Größe des Arbeitsspeichers der Funktionen-App.

## Azure Functions Pulse  

Pulse ist ein Liveereignisstream, der anzeigt, wie häufig Ihre Funktion ausgeführt wird und welche Ausführungen erfolgreich oder fehlerhaft waren. Sie können auch die durchschnittliche Ausführungszeit überwachen. Wir werden im Lauf der Zeit weitere Features und Anpassungen hinzufügen. Sie können über die Registerkarte **Überwachung** auf die Seite **Pulse** zugreifen.

## Repositorys

Der Code für Azure Functions ist Open Source und in GitHub-Repositorys gespeichert:

* [Azure Functions-Laufzeit](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure Functions-Portal](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure Functions-Vorlagen](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK-Erweiterungen](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## Bindungen

Hier finden Sie eine Tabelle aller unterstützten Bindungen.

[AZURE.INCLUDE [Dynamisches Compute](../../includes/functions-bindings.md)]

## Melden von Problemen

[AZURE.INCLUDE [Melden von Problemen](../../includes/functions-reporting-issues.md)]

## Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [C#-Entwicklerreferenz zu Azure Functions](functions-reference-csharp.md)
* [F#-Entwicklerreferenz zu Azure Functions](functions-reference-fsharp.md)
* [NodeJS-Entwicklerreferenz zu Azure Functions](functions-reference-node.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)
* [Azure Functions: The Journey](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) im Blog des Azure App Service-Teams. Überblick, wie Azure Functions entwickelt wurde.

<!---HONumber=AcomDC_0921_2016-->