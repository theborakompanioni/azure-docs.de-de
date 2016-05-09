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
	ms.date="04/07/2016"
	ms.author="chrande"/>

# Entwicklerreferenz zu Azure Functions

Azure Functions nutzen einige wichtige gemeinsame technische Konzepte und Komponenten, unabhängig von der verwendeten Sprache oder Bindung. Bevor Sie sich mit den spezifischen Details einer bestimmten Sprache oder Bindung beschäftigen, sollten Sie diese Übersicht lesen, die für alle Funktionen gilt.

In diesem Artikel wird davon ausgegangen, dass Sie die [Übersicht zu Azure Functions](functions-overview.md) gelesen haben und mit [WebJobs SDK-Konzepten wie Triggern, Bindungen und der JobHost-Laufzeit](../app-service-web/websites-dotnet-webjobs-sdk.md) vertraut sind. Azure Functions basiert auf dem WebJobs SDK

## Funktion

Ein *Funktion* ist das primäre Konzept in Azure Functions. Sie schreiben Code in der Sprache Ihrer Wahl und speichern die Codedatei(en) sowie eine Konfigurationsdatei im gleichen Ordner. Die Konfiguration erfolgt in JSON, und die Datei heißt `function.json`. Es wird eine Vielzahl von Sprachen unterstützt, und für jede Sprache gibt es leicht voneinander abweichende Optionen, die sich für die jeweilige Sprache am besten eignen. Beispiel für eine Ordnerstruktur:

```
mynodefunction
| - function.json
| - index.js
| - node_modules
| | - ... packages ...
| - package.json
mycsharpfunction
| - function.json
| - run.csx
```

## function.json und Bindungen

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

## Runtime (Skripthost und Webhost)

Die Runtime, die auch als Skripthost bezeichnet wird, ist der zugrunde liegende WebJobs SDK-Host, der auf Ereignisse lauscht, Daten sammelt und sendet und letztendlich den Code ausführt.

Um HTTP-Trigger zu ermöglichen, gibt es auch einen Webhost, der in Produktionsszenarien vor dem Skripthost platziert ist. Auf diese Weise kann der Skripthost vom Front-End-Datenverkehr isoliert werden, der vom Webhost verwaltet wird.

## Ordnerstruktur

Ein Skripthost verweist auf einen Ordner, der eine Konfigurationsdatei und ein oder mehrere Funktionen enthält.

```
parentFolder (for example, wwwroot)
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

Die Datei *host.json* enthält die für den Skripthost spezifische Konfiguration und befindet sich im übergeordneten Ordner.

Jede Funktion verfügt über einen Ordner, der die Codedatei(en), *function.json*, sowie weitere Abhängigkeiten enthält.

Wenn Sie ein Projekt einrichten, um Funktionen für eine Funktions-App in Azure App Service bereitzustellen, können Sie diese Ordnerstruktur als Sitecode behandeln. Sie können vorhandene Tools, wie z.B. die fortlaufende Integration und Bereitstellung, oder benutzerdefinierte Bereitstellungsskripts verwenden, um eine Paketinstallation zur Bereitstellungszeit oder eine Codetranspilierung umzusetzen.

## Parallele Ausführung

Wenn die Auslösung mehrerer Ereignisse schneller erfolgt als die Runtime einer Singlethreadfunktion sie verarbeiten kann, kann die Runtime die Funktion mehrmals parallel aufrufen. Wenn eine Funktions-App den [dynamischen Serviceplan](functions-scale.md#dynamic-service-plan) verwendet, könnte die App automatisch horizontal auf bis zu 10 gleichzeitige Instanzen hochskaliert werden. Jede Instanz der Funktions-App – unabhängig davon, ob die App im dynamischen Serviceplan oder einem regulären [App Services-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) ausgeführt wird – kann dann gleichzeitige Funktionsaufrufe über mehrere Threads parallel verarbeiten. Die maximale Anzahl gleichzeitiger Funktionsaufrufe in jeder Instanz der Funktions-App variiert je nach Größe des Arbeitsspeichers der Funktions-App.

## Azure Functions Pulse  

Pulse ist ein Liveereignisstream, der anzeigt, wie häufig Ihre Funktion ausgeführt wird und welche Ausführungen erfolgreich oder fehlerhaft waren. Sie können auch die durchschnittliche Ausführungszeit überwachen. Wir werden im Lauf der Zeit weitere Features und Anpassungen hinzufügen. Sie können über die Registerkarte **Überwachung** auf die Seite **Pulse** zugreifen.

## Bindungen

Hier finden Sie eine Tabelle aller unterstützten Bindungen.

[AZURE.INCLUDE [Dynamisches Compute](../../includes/functions-bindings.md)]

## Melden von Problemen

[AZURE.INCLUDE [Melden von Problemen](../../includes/functions-reporting-issues.md)]

## Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [C#-Entwicklerreferenz zu Azure Functions](functions-reference-csharp.md)
* [NodeJS-Entwicklerreferenz zu Azure Functions](functions-reference-node.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)

<!---HONumber=AcomDC_0427_2016-->