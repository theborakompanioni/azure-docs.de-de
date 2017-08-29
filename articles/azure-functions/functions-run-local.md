---
title: "Lokales Entwickeln und Ausführen von Azure Functions | Microsoft Docs"
description: "Erfahren Sie, wie Sie Azure-Funktionen auf dem lokalen Computer codieren und testen, bevor Sie sie in Azure Functions ausführen."
services: functions
documentationcenter: na
author: lindydonna
manager: cfowler
editor: 
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 07/12/2017
ms.author: glenga
ms.translationtype: HT
ms.sourcegitcommit: 49bc337dac9d3372da188afc3fa7dff8e907c905
ms.openlocfilehash: 07ad15c61bd4b3912dfa2f629218deebdebd6dc8
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="code-and-test-azure-functions-locally"></a>Lokales Codieren und Testen von Azure-Funktionen

Während das [Azure-Portal] eine vollständige Reihe von Tools zum Entwickeln und Testen von Azure-Funktionen bereitstellt, bevorzugen viele Entwickler eine lokale Entwicklungsumgebung. Mit Azure Functions können Sie einfach Ihren bevorzugten Code-Editor und Ihre bevorzugten Entwicklungstools zum Entwickeln und Testen Ihrer Funktionen auf Ihrem lokalen Computer verwenden. Ihre Funktionen können für Ereignisse in Azure ausgelöst werden, und Sie können Ihre C#- und JavaScript-Funktionen auf Ihrem lokalen Computer debuggen. 

Wenn Sie ein Visual Studio C#-Entwickler sind, ist Azure Functions auch [in Visual Studio 2017](functions-develop-vs.md) integrierbar.

## <a name="install-the-azure-functions-core-tools"></a>Installieren von Azure Functions Core Tools

Azure Functions Core Tools ist eine lokale Version der Azure Functions-Laufzeit, die Sie auf Ihrem lokalen Windows-Computer ausführen können. Es ist kein Emulator oder Simulator. Es ist die gleiche Laufzeit, über die Functions in Azure ausgeführt wird.

[Azure Functions Core Tools] wird als npm-Paket bereitgestellt. Sie müssen zuerst [NodeJS installieren](https://docs.npmjs.com/getting-started/installing-node), das npm enthält.  

>[!NOTE]
>Momentan kann das Azure Functions Core Tools-Paket nur auf Windows-Computern installiert werden. Diese Einschränkung ist auf eine temporäre Einschränkung im Functions-Host zurückzuführen.

[Azure Functions Core Tools] fügt die folgenden Befehlsaliase hinzu:
* **func**
* **azfun**
* **azurefunctions**

Alle diese Alias können anstelle von `func` verwendet werden, was in den Beispielen in diesem Thema gezeigt wird.

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
  "IsEncrypted": false,   
  "Values": {
    "AzureWebJobsStorage": "<connection string>", 
    "AzureWebJobsDashboard": "<connection string>", 
  },
  "Host": {
    "LocalHttpPort": 7071, 
    "CORS": "*" 
  },
  "ConnectionStrings": {
    "SQLConnectionString": "Value"
  }
}
```
| Einstellung      | Beschreibung                            |
| ------------ | -------------------------------------- |
| **IsEncrypted** | Wenn dies auf **TRUE** festgelegt wird, werden alle Werte mithilfe eines Schlüssels des lokalen Computers verschlüsselt. Wird mit `func settings`-Befehlen verwendet. Der Standardwert ist **false**. |
| **Werte** | Eine Sammlung von Anwendungseinstellungen, die bei der lokalen Ausführung verwendet wird. Fügen Sie Ihre Anwendungseinstellungen zu diesem Objekt hinzu.  |
| **AzureWebJobsStorage** | Legt die Verbindungszeichenfolge auf das Azure Storage-Konto fest, das intern von der Azure Functions-Laufzeit verwendet wird. Das Speicherkonto unterstützt die Trigger Ihrer Funktion. Diese Verbindungseinstellung des Speicherkontos wird für alle Funktionen außer die Funktionen benötigt, die per HTTP ausgelöst werden.  |
| **AzureWebJobsDashboard** | Legt die Verbindungszeichenfolge auf das Azure Storage-Konto fest, das zum Speichern der Funktionsprotokolle verwendet wird. Dieser optionale Wert stellt die Protokolle im Portal zur Verfügung.|
| **Host** | Die Einstellungen in diesem Abschnitt passen den Hostprozess von Functions bei der lokalen Ausführung an. | 
| **LocalHttpPort** | Legt den Standardport fest, der bei der Ausführung des lokalen Functions-Host verwendet wird (`func host start` und `func run`). Die Befehlszeilenoption `--port` hat Vorrang vor diesem Wert. |
| **CORS** | Definiert die für die [Ressourcenfreigabe zwischen verschiedenen Ursprüngen (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) zulässigen Ursprünge. Ursprünge werden als durch Trennzeichen getrennte Liste ohne Leerzeichen bereitgestellt. Den Platzhalterwert (**\***) wird unterstützt, wodurch Anforderungen von einem beliebigen Ursprung zulässig sind. |
| **ConnectionStrings** | Enthält die Datenbank-Verbindungszeichenfolgen für Ihre Funktionen. Verbindungszeichenfolgen in diesem Objekt werden der Umgebung mit dem Anbietertyp **System.Data.SqlClient** hinzugefügt.  | 

Die meisten Trigger und Bindungen verfügen über eine **Connection**-Eigenschaft, die dem Namen einer Umgebungsvariablen oder App-Einstellung entspricht. Für jede Verbindungseigenschaft muss eine App-Einstellung in der Datei „local.settings.json“ definiert sein. 

Diese Einstellungen können in Ihrem Code auch als Umgebungsvariablen gelesen werden. Verwenden Sie in C# [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) oder [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx). Verwenden Sie in JavaScript `process.env`. Einstellungen, die als Systemumgebungsvariable angegeben werden, haben Vorrang vor Werten in der Datei „local.settings.json“. 

Einstellungen in der Datei „local.settings.json“ werden nur bei der lokalen Ausführung von Functions-Tools verwendet. Standardmäßig werden diese Einstellungen nicht automatisch migriert, wenn das Projekt in Azure veröffentlicht wird. Verwenden Sie den Switch `--publish-local-settings` [bei der Veröffentlichung](#publish), um sicherzustellen, dass diese Einstellungen zur Funktionen-App in Azure hinzugefügt werden.

Wenn keine gültige Speicherverbindungszeichenfolge für **AzureWebJobsStorage** festgelegt ist, wird die folgende Fehlermeldung angezeigt:  

>Missing value for AzureWebJobsStorage in local.settings.json (Fehlender Wert für AzureWebJobsStorage in local.settings.json). Dies ist für alle Nicht-HTTP-Trigger erforderlich. Sie können „func azure functionary fetch-app-settings <functionAppName>“ ausführen oder eine Verbindungszeichenfolge in „local.settings.json“ angeben.
  
[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a>Konfigurieren von App-Einstellungen

Um einen Wert für Verbindungszeichenfolgen festzulegen, können Sie eine der folgenden Aktionen ausführen:
* Geben Sie die Verbindungszeichenfolge aus [Azure-Speicher-Explorer](http://storageexplorer.com/) ein.
* Verwenden Sie einen der folgenden Befehle:

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure functionapp storage fetch-connection-string <StorageAccountName>
    ```
    Für beide Befehle müssen Sie sich zunächst bei Azure anmelden.

## <a name="create-a-function"></a>Erstellen einer Funktion

Um eine Funktion zu erstellen, führen Sie den folgenden Befehl aus:

```
func new
``` 
`func new` unterstützt die folgenden optionalen Argumente:

| Argument     | Beschreibung                            |
| ------------ | -------------------------------------- |
| **`--language -l`** | Die Vorlagenprogrammiersprache, z.B. C#, F# oder JavaScript. |
| **`--template -t`** | Der Name der Vorlage. |
| **`--name -n`** | Der Funktionsname. |

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

`func host start` unterstützt die folgenden Optionen:

| Option     | Beschreibung                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | Der lokale Port, auf dem gelauscht werden soll. Standardwert: 7071. |
| **`--debug <type>`** | Die Optionen sind `VSCode` und `VS`. |
| **`--cors`** | Eine durch Trennzeichen getrennte Liste der CORS-Ursprünge ohne Leerzeichen. |
| **`--nodeDebugPort -n`** | Der Port, den der Knotendebugger verwendet. Standard: Ein Wert von „launch.json“ oder 5858. |
| **`--debugLevel -d`** | Die Konsolen-Ablaufverfolgungsebene (aus, ausführlich, Info, Warnung oder Fehler). Standard: Info.|
| **`--timeout -t`** | Das Timeout für den zu startenden Functions-Host in Sekunden. Standard: 20 Sekunden.|
| **`--useHttps`** | An https://localhost:{Port} statt an http://localhost:{Port} binden. Standardmäßig erstellt diese Option ein vertrauenswürdiges Zertifikat auf Ihrem Computer.|
| **`--pause-on-error`** | Vor Beenden des Prozesses für zusätzliche Eingabe anhalten. Ist beim Starten von Azure Functions Core Tools in einer integrierten Entwicklungsumgebung (IDE) nützlich.|

Wenn der Functions-Host startet, gibt er die URL der HTTP-ausgelösten Funktionen aus:

```
Found the following functions:
Host.Functions.MyHttpTrigger

ob host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug-in-vs-code-or-visual-studio"></a>Debuggen in VS Code oder Visual Studio

Um einen Debugger anzufügen, übergeben Sie das `--debug`-Argument. Verwenden Sie Visual Studio Code zum Debuggen von JavaScript-Funktionen. Verwenden Sie Visual Studio für C#-Funktionen.

Verwenden Sie `--debug vs` zum Debuggen von C#-Funktionen. Sie können auch [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) verwenden. 

Um den Host zu starten und JavaScript-Debuggen einzurichten, führen Sie Folgendes aus:

```
func host start --debug vscode
```

Wählen Sie dann in Visual Studio Code in der Ansicht **Debuggen** **Anfügen an Azure Functions**. Sie können Haltepunkte anfügen, Variablen prüfen und Code schrittweise ausführen.

![JavaScript-Debuggen mit Visual Studio Code](./media/functions-run-local/vscode-javascript-debugging.png)

### <a name="passing-test-data-to-a-function"></a>Übergeben von Testdaten an eine Funktion

Sie können eine Funktion auch direkt aufrufen, indem Sie `func run <FunctionName>` verwenden, und Eingabedaten für die Funktion bereitstellen. Dieser Befehl ähnelt der Ausführung einer Funktion mithilfe der Registerkarte **Test** im Azure-Portal. Dieser Befehl startet den gesamten Functions-Host.

`func run` unterstützt die folgenden Optionen:

| Option     | Beschreibung                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Inlineinhalt. |
| **`--debug -d`** | Anfügen eines Debuggers an den Hostprozess vor dem Ausführen der Funktion.|
| **`--timeout -t`** | Wartezeit (in Sekunden), bis der lokale Functions-Host bereit ist.|
| **`--file -f`** | Der als Inhalt zu verwendende Dateiname.|
| **`--no-interactive`** | Fordert keine Eingabe an. Für Automatisierungsszenarien nützlich.|

Um z.B. eine HTTP-ausgelöste Funktion aufzurufen und Inhaltstext zu übergeben, führen Sie folgenden Befehl aus:

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <a name="publish"></a>Veröffentlichen in Azure

Um ein Functions-Projekt in einer Funktions-App in Azure zu veröffentlichen, verwenden Sie den `publish`-Befehl:

```
func azure functionapp publish <FunctionAppName>
```

Sie können die folgenden Optionen verwenden:

| Option     | Beschreibung                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Einstellungen zur Veröffentlichung in Azure in „local.settings.json“. Wenn die Einstellung bereits vorhanden ist, werden Sie gefragt, ob sie überschrieben werden soll.|
| **`--overwrite-settings -y`** | Muss zusammen mit `-i` verwendet werden. Überschreibt AppSettings in Azure mit dem lokalen Wert, falls abweichend. Standard ist die Eingabeaufforderung.|

Der `publish`-Befehl lädt den Inhalt des Functions-Projektverzeichnisses hoch. Wenn Sie Dateien lokal löschen, werden sie mit dem Befehl `publish` nicht aus Azure gelöscht. Sie können Dateien in Azure löschen, indem Sie das [Kudu-Tool](functions-how-to-use-azure-function-app-settings.md#kudu) im [Azure-Portal] verwenden.

## <a name="next-steps"></a>Nächste Schritte

Azure Functions Core Tools ist ein [Open Source-Programm und wird auf GitHub gehostet](https://github.com/azure/azure-functions-cli).  
Um einen Fehler zu melden oder ein Feature anzufordern, [öffnen Sie ein GitHub-Problem](https://github.com/azure/azure-functions-cli/issues). 

<!-- LINKS -->

[Azure Functions Core Tools]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure-Portal]: https://portal.azure.com 

