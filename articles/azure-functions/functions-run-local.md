---
title: "Lokales Entwickeln und Ausführen von Azure Functions | Microsoft Docs"
description: "Erfahren Sie, wie Sie Azure-Funktionen auf dem lokalen Computer codieren und testen, bevor Sie sie in Azure Functions ausführen."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/08/2017
ms.author: donnam
ms.translationtype: Human Translation
ms.sourcegitcommit: 245ce9261332a3d36a36968f7c9dbc4611a019b2
ms.openlocfilehash: 478c2ffbb0bfe5b1595bc6ea1bebb2144aebc728
ms.contentlocale: de-de
ms.lasthandoff: 06/09/2017

---
# <a name="code-and-test-azure-functions-locally"></a>Lokales Codieren und Testen von Azure-Funktionen

Sie können die Azure Functions-Laufzeit lokal mit Ihrem bevorzugten Code-Editor und den lokalen Entwicklungstools ausführen. Lassen Sie Trigger bei Ereignissen in Azure auslösen und debuggen Sie C#- und JavaScript-Funktionen.

Installieren Sie zu Anfang [Azure Functions Core Tools] von npm. Azure Functions Core Tools ist eine lokale Version der Azure Functions-Laufzeit, die Sie auf Ihrem lokalen Windows-Computer ausführen können. Es ist kein Emulator oder Simulator. Es ist die gleiche Laufzeit, die in Azure ausgeführt wird.

[Azure Functions Core Tools] fügt die folgenden Befehlsaliase hinzu:
- `func`
- `azfun`
- `azurefunctions`

Azure Functions Core Tools ist ein [Open Source-Programm und wird auf GitHub gehostet](https://github.com/azure/azure-functions-cli). Um einen Fehler zu melden oder ein Feature anzufordern, [öffnen Sie ein GitHub-Problem](https://github.com/azure/azure-functions-cli/issues).

## <a name="create-a-local-functions-project"></a>Erstellen eines lokalen Functions-Projekts

Bei lokaler Ausführung ist ein Functions-Projekt ein Verzeichnis mit den Dateien „host.json“ und „local.settings.json“. Dieses Verzeichnis ist das Äquivalent zu einer Funktions-App in Azure. Weitere Informationen zur Azure Functions-Ordnerstruktur finden Sie in [Azure Functions: Entwicklerhandbuch](functions-reference.md#folder-structure).

Führen Sie an der Eingabeaufforderung folgenden Befehl aus:

```
func init MyFunctionProj
```

Die Ausgabe sieht in etwa wie folgt aus:

```
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in D:/Code/Playground/MyFunctionProj/.git/
```

Verwenden Sie die Option `--no-source-control [-n]`, um das Erstellen eines Git-Repositorys abzulehnen.

<a name="local-settings"></a>

## <a name="local-settings-file"></a>Datei für lokale Einstellungen

Die Datei „local.settings.json“ speichert App-Einstellungen, Verbindungszeichenfolgen und Einstellungen für Azure Functions Core Tools. Sie hat folgende Struktur:

```json
{
  "IsEncrypted": false,   // If set to true, all values are encrypted by using a local machine key. Use with "func settings" commands.
  "Values": {
    "AzureWebJobsStorage": "<connection string>",   // This is required for all triggers except HTTP.
    "AzureWebJobsDashboard": "<connection string>", // Optional, controls whether to log to the Monitor tab in the portal.
  },
  "Host": {
    "LocalHttpPort": 7071, // If specified, this is the default port for "host start" and "run". Can be overridden by using the --port command-line option.
    "CORS": "*"            // Origins to allow in the CORS setting.
  },
  "ConnectionStrings": {
    "SQLConnectionString": "Value"
  }
}
```

Geben Sie die App-Einstellungen in der **Values**-Sammlung an. Diese Einstellungen können dann als Umgebungsvariablen gelesen werden. Verwenden Sie in C# `System.Environment.GetEnvironmentVariable` oder `ConfigurationManager.AppSettings`. Verwenden Sie in JavaScript `process.env`. Wenn die gleiche Einstellung als Systemumgebungsvariable angegeben wird, hat dies Vorrang vor Werten in „local.settings.json“.

Die App-Einstellung **AzureWebJobsStorage** ist eine spezielle Einstellung, die die Azure Functions-Laufzeit für alle Nicht-HTTP-Trigger benötigt. Intern erstellt die Laufzeit Warteschlangen zum Verwalten von Triggern in diesem Speicherkonto. Wenn kein Wert für **AzureWebJobsStorage** angegeben wird, und Sie Nicht-HTTP-Trigger verwenden, wird folgende Meldung angezeigt:

*Wert für AzureWebJobsStorage in „local.settings.json“ fehlt. Dies ist für alle Nicht-HTTP-Trigger erforderlich. Sie können „func azure functionapp fetch-app-settings <functionAppName>“ ausführen oder eine Verbindungszeichenfolge in „local.settings.json“ eingeben.*

> [!NOTE]
> Über die Verbindungszeichenfolge „AzureWebJobsStorage“: „UseDevelopmentStorage=true“ können Sie den Emulator für lokalen Speicher verwenden. Möglicherweise treten jedoch im Vergleich zum Azure Storage-Dienst Unterschiede im Verhalten auf.

Die folgenden Einstellungen passen den lokalen Functions-Host an:
- `LocalHttpPort`. Der für `func host start` `func run` zu verwendende Standardport. Die Befehlszeilenoption `--port` hat Vorrang vor diesem Wert.
- `CORS`. Die in CORS zulässigen Ursprünge als durch Trennzeichen getrennte Liste ohne Leerzeichen. Mit „*“ lassen Sie alle zu.

Sie können die Verbindungszeichenfolgen im `ConnectionStrings`-Objekt bereitstellen. Sie werden der Umgebung mit dem Anbieternamen **System.Data.SqlClient** hinzugefügt.

Die meisten Trigger und Bindungen verfügen über eine Eigenschaft **Connection**, die der Name einer Umgebungsvariablen oder App-Einstellung in „local.settings.json“ ist. Wenn der App-Einstellungswert fehlt, wird die folgende Meldung angezeigt:

*Warnung: Wert mit dem Namen „MyStorageConnection“ in „local.settings.json“, der der in „BlobTriggerCSharp\function.json“ für „blobTrigger“ festgelegten Eigenschaft „Connection“ entspricht, kann nicht gefunden werden. Sie können „func azure functionapp fetch-app-settings <functionAppName>“ ausführen oder eine Verbindungszeichenfolge in „local.settings.json“ eingeben.*

Die Datei „local.settings.json“ wird nur von Azure Functions Core Tools verwendet. Verwenden Sie zum Festlegen von App-Einstellungen und Verbindungszeichenfolgen in Azure das Blatt **Anwendungseinstellungen**.

### <a name="configure-app-settings"></a>Konfigurieren von App-Einstellungen

Um einen Wert für Verbindungszeichenfolgen festzulegen, können Sie eine der folgenden Aktionen ausführen:
- Geben Sie manuell eine Verbindungszeichenfolge im [Azure Storage Explorer](http://storageexplorer.com/) ein.
- Verwenden Sie **func azure functionapp fetch-app-settings \<Funktions-App-Name\>**. **Azure-Anmeldung** erforderlich.
- Verwenden Sie **func azure functionapp storage fetch-connection-string \<Speicherkontoname\>**. **Azure-Anmeldung** erforderlich.

## <a name="create-a-function"></a>Erstellen einer Funktion

Führen Sie zum Erstellen einer Funktion `func new` aus. Dieser Befehl hat folgende optionale Argumente:

- `--language [-l]`. Die Vorlagenprogrammiersprache, z.B. C#, F# oder JavaScript.
- `--template [-t]`. Der Name der Vorlage.
- `--name [-n]`. Der Funktionsname.

Führen Sie z.B. zum Erstellen eines JavaScript-HTTP-Triggers Folgendes aus:

```
func new --language JavaScript --template HttpTrigger --name MyHttpTrigger
```

Führen Sie zum Erstellen einer durch die Warteschlange ausgelösten Funktion Folgendes aus:

```
func new --language JavaScript --template QueueTrigger --name QueueTriggerJS
```

## <a name="run-functions-locally"></a>Lokales Ausführen von Funktionen

Um ein Functions-Projekt auszuführen, führen Sie den Functions-Host aus. Der Host aktiviert Trigger für alle Funktionen im Projekt:

```
func host start
```

Sie können die folgenden Optionen mit `func host start` verwenden:

- `--port [-p]`. Der lokale Port, auf dem gelauscht werden soll. Standardwert: 7071.
- `--debug <type>`. Die Optionen sind VSCode und VS.
- `--cors`. Eine durch Trennzeichen getrennte Liste der CORS-Ursprünge ohne Leerzeichen.
- `--nodeDebugPort [-n]`. Der Port, den der Knotendebugger verwendet. Standard: Ein Wert von „launch.json“ oder 5858.
- `--debugLevel [-d]`. Die Konsolen-Ablaufverfolgungsebene (aus, ausführlich, Info, Warnung oder Fehler). Standard: Info.
- `--timeout [-t]`. Das Timeout für den zu startenden Functions-Host in Sekunden. Standard: 20 Sekunden.
- `--useHttps`. An https://localhost:{Port} statt an http://localhost:{Port} binden. Standardmäßig erstellt diese Option ein vertrauenswürdiges Zertifikat auf Ihrem Computer.
- `--pause-on-error`. Vor Beenden des Prozesses für zusätzliche Eingabe anhalten. Ist beim Starten von Azure Functions Core Tools in einer integrierten Entwicklungsumgebung (IDE) nützlich.

Wenn der Functions-Host startet, gibt er die URL der HTTP-ausgelösten Funktionen aus:

```
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug"></a>Debuggen

Um einen Debugger anzufügen, übergeben Sie das `--debug`-Argument. Verwenden Sie Visual Studio Code zum Debuggen von JavaScript-Funktionen. Verwenden Sie Visual Studio für C#-Funktionen.

Verwenden Sie `--debug vs` zum Debuggen von C#-Funktionen. Verwenden Sie alternativ [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/). 

Um den Host zu starten und JavaScript-Debuggen einzurichten, führen Sie Folgendes aus:

```
func host start --debug vscode
```

Wählen Sie dann in Visual Studio Code in der Ansicht **Debuggen** **Anfügen an Azure Functions**. Sie können Haltepunkte anfügen, Variablen prüfen und Code schrittweise ausführen.

![JavaScript-Debuggen mit Visual Studio Code](./media/functions-run-local/vscode-javascript-debugging.png)

### <a name="call-a-function-by-using-test-data"></a>Aufrufen einer Funktion mit Testdaten

Sie können eine Funktion auch direkt mithilfe von `func run <FunctionName>` aufrufen. Dieser Befehl ähnelt der Registerkarte **Test** im Azure-Portal, wo Sie Daten für die Funktion eingeben können. Dieser Befehl startet den gesamten Functions-Host.

Sie können die folgenden Optionen mit `func run` verwenden:

- `--content [-c]`. Inlineinhalt.
- `--debug [-d]`. Anfügen eines Debuggers an den Hostprozess vor dem Ausführen der Funktion.
- `--timeout [-t]`. Wartezeit (in Sekunden), bis der lokale Functions-Host bereit ist.
- `--file [-f]`. Der als Inhalt zu verwendende Dateiname.
- `--no-interactive`. Fordert keine Eingabe an. Für Automatisierungsszenarien nützlich.

Um z.B. eine HTTP-ausgelöste Funktion aufzurufen und Inhaltstext zu übergeben, führen Sie folgenden Befehl aus:

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <a name="publish-a-function-app"></a>Veröffentlichen einer Funktions-App

Um ein Functions-Projekt in einer Funktions-App in Azure zu veröffentlichen, verwenden Sie den `publish`-Befehl:

```
func azure functionapp publish <FunctionAppName>
```

Sie können die folgenden Optionen verwenden:

- `--publish-local-settings [-i]`.  Einstellungen zur Veröffentlichung in Azure in „local.settings.json“. Wenn die Einstellung bereits vorhanden ist, werden Sie gefragt, ob sie überschrieben werden soll.
- `--overwrite-settings [-y]`. Muss zusammen mit `-i` verwendet werden. Überschreibt AppSettings in Azure mit dem lokalen Wert, falls abweichend. Standard ist die Eingabeaufforderung.

Der `publish`-Befehl lädt den Inhalt des Functions-Projektverzeichnisses hoch. Wenn Sie Dateien lokal löschen, werden sie mit diesem Befehl nicht aus Azure gelöscht. Verwenden Sie zum Löschen dieser Dateien im Azure Functions-Portal Kudu. Wählen Sie zum Starten von Kudu im Azure Functions-Portal **Plattformfeatures** > **Erweiterte Tools (Kudu)**. 


<!-- LINKS -->

[Azure Functions Core Tools]: https://www.npmjs.com/package/azure-functions-core-tools

