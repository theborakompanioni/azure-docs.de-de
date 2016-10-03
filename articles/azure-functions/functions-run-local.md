<properties
	pageTitle="Lokales Entwickeln und Ausführen von Azure Functions | Microsoft Azure"
	description="Erfahren Sie, wie Azure Functions in Visual Studio lokal codiert und getestet werden können, bevor Sie sie in Azure App Service ausführen."
	services="functions"
	documentationCenter="na"
	authors="tdykstra"
	manager="erikre"
	editor=""/>

<tags
	ms.service="functions"
	ms.workload="na"
	ms.tgt_pltfrm="multiple"
	ms.devlang="multiple"
	ms.topic="article"
	ms.date="08/22/2016"
	ms.author="glenga"/>

# Codieren und Testen von Azure-Funktionen in Visual Studio

## Übersicht

In diesem Artikel wird erläutert, wie Sie [Azure Functions](functions-overview.md) lokal ausführen, indem Sie das GitHub-Repository [WebJobs.Script](https://github.com/Azure/azure-webjobs-sdk-script/) herunterladen und die darin enthaltene Visual Studio-Projektmappe ausführen.

Die Laufzeit für Azure Functions ist eine Implementierung des Open Source-Projekts „WebJobs.Script“. Dieses Projekt basiert wiederum auf dem [Azure WebJobs-SDK](../app-service-web/websites-dotnet-webjobs-sdk.md), und beide Frameworks können lokal ausgeführt werden. Sie müssen sich jedoch mit einem Azure Storage-Konto verbinden, weil das WebJobs-SDK Speicherkontofunktionen verwendet, die vom Speicheremulator nicht unterstützt werden.

Functions ist im Azure-Portal einfach zu codieren und zu testen, aber manchmal ist es sinnvoll, lokal damit zu arbeiten, bevor es in Azure ausgeführt wird. Für einige der von Azure Functions unterstützten Sprachen ist es beispielsweise einfacher, Code in Visual Studio zu schreiben, da dort [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx) bereitsteht. Und auch wenn Sie eine Funktion remote debuggen können, kann das lokale Debuggen schneller und einfacher sein. Bei der lokalen Ausführung können Sie sowohl im Funktionscode als auch im Hostcode des WebJobs-Skripts debuggen und Haltepunkte festlegen.

>[AZURE.NOTE] Azure Functions befindet sich derzeit in der Vorschau, und die Benutzeroberfläche und die Tools sind noch in der Entwicklung. Die in diesem Artikel aufgeführten Verfahren spiegeln nicht die endgültige Entwicklungsoberfläche wider, und wir würden uns sehr über Ihr [Feedback](https://feedback.azure.com/forums/355860-azure-functions) freuen.

## Voraussetzungen

### Ein Azure-Konto mit einer Funktions-App

In diesem Artikel wird davon ausgegangen, dass Sie mit [Azure Functions](functions-overview.md) im Portal gearbeitet haben und mit den Konzepten von Azure Functions, wie z. B. [Triggern, Bindungen und JobHost](functions-reference.md), vertraut sind.

Wenn Sie Functions lokal ausführen, erhalten Sie Ausgabe im Konsolenfenster. Sie werden aber auch das von einer Live-Funktions-App gehostete Dashboard verwenden wollen, um Funktionsaufrufe und Protokolle anzuzeigen.

### Visual Studio 2015 mit dem neuesten Azure SDK für .NET

Wenn Sie nicht über Visual Studio 2015 oder das aktuelle Azure-SDK verfügen, [laden Sie das Azure-SDK für Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003) herunter. Visual Studio 2015 wird automatisch mit dem SDK installiert, wenn das Programm noch nicht vorhanden ist.

### Bedingt erforderliche Komponenten

Einige Azure-Ressourcen und Softwareinstallationen sind nur erforderlich, wenn Sie Funktionen ausführen möchten, die diese verwenden, beispielsweise:

* Azure-Ressourcen
	* Service Bus
	* Einfache Tabellen
	* DocumentDB
	* Ereignis-Hubs
	* Notification Hubs

* Compiler und Skriptmodule
	* F#
	* BASH
	* Python
	* PHP

Weitere Informationen zu diesen Anforderungen, einschließlich der Umgebungsvariablen, die Sie dafür festlegen müssen, finden Sie auf den [Wiki-Seiten für das Repository „WebJobs.Script“](https://github.com/Azure/azure-webjobs-sdk-script/wiki/home).

Wenn Sie beabsichtigen, sich am WebJobs.SDK-Projekt zu beteiligen, benötigen Sie alle bedingt erforderlichen Komponenten zum Ausführen vollständiger Tests.

## Lokale Ausführung

1. [Klonen](https://github.com/Azure/azure-webjobs-sdk-script/) Sie das WebJobs.Script-Repository, oder [laden Sie es herunter](https://github.com/Azure/azure-webjobs-sdk-script/archive/master.zip).

2. Legen Sie Umgebungsvariablen für Speicher-Verbindungszeichenfolgen fest.

	* AzureWebJobsStorage
	* AzureWebJobsDashboard

	Sie können diese Werte aus dem App Service-Portalblatt **Anwendungseinstellungen** für eine Funktions-App abrufen.

	a. Klicken Sie auf dem Blatt **Funktions-App** auf **Funktions-App-Einstellungen**.

	![Klicken Sie auf „Funktions-App-Einstellungen“](./media/functions-run-local/clickfuncappsettings.png)
 
	b. Klicken Sie auf dem Blatt **Funktions-App-Einstellungen** auf **Zu App Service-Einstellungen wechseln**.

	![Klicken Sie auf „App Service-Einstellungen“](./media/functions-run-local/clickappsvcsettings.png)
 
	c. Klicken Sie auf dem Blatt **Einstellungen** auf **Anwendungseinstellungen**.

	![Klicken Sie auf „Anwendungseinstellungen“.](./media/functions-run-local/clickappsettings.png)
 
	d. Scrollen Sie auf dem Blatt **Anwendungseinstellungen** nach unten zum Abschnitt **App-Einstellungen**, und suchen Sie die WebJobs-SDK-Einstellungen.

	![WebJobs-Einstellungen](./media/functions-run-local/wjsettings.png)

	e. Legen Sie eine Umgebungsvariable mit demselben Namen und Wert wie die `AzureWebJobsStorage`-App-Einstellung fest.

	f. Führen Sie dieselben Schritte für die `AzureWebJobsDashboard`-App-Einstellung durch.

2. Erstellen Sie eine Umgebungsvariable namens „AzureWebJobsServiceBus“, und legen Sie sie auf die Service Bus-Verbindungszeichenfolge fest.

	Diese Umgebungsvariable ist für Service Bus-Bindungen erforderlich, und es wird empfohlen, sie auch dann festzulegen, wenn Sie keine Service Bus-Bindungen verwenden. In einigen Szenarien werden unabhängig von den verwendeten Bindungen möglicherweise Ausnahmen angezeigt, wenn die Service Bus-Verbindungszeichenfolge nicht festgelegt ist.

3. Stellen Sie sicher, dass andere benötigte Umgebungsvariablen festgelegt sind. (Informationen hierzu finden Sie im vorangehenden Abschnitt [Bedingt erforderliche Komponenten](#conditional-prerequisites).)

4. Starten Sie Visual Studio, und öffnen Sie die Projektmappe „WebJobs.Script“.

6. Legen Sie das Startprojekt fest. Wenn Sie Funktionen ausführen möchten, die HTTP- oder WebHook-Trigger verwenden, wählen Sie **WebJobs.Script.WebHost**; andernfalls wählen Sie **WebJobs.Script.Host**.

4. Wenn Sie „WebJobs.Script.Host“ als Startprojekt verwenden:

	a. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt „WebJobs.Script.Host“, und klicken Sie dann auf **Eigenschaften**.

	b. Legen Sie im Fenster **Projekteigenschaften** auf der Registerkarte **Debuggen** die **Befehlszeilenargumente** auf `..\..\..\..\sample` fest.

	![Befehlszeilenargumente](./media/functions-run-local/cmdlineargs.png)

	Dies ist ein relativer Pfad zum Ordner *sample* im Repository. Der Ordner *sample* enthält eine Datei *host.json* mit globalen Einstellungen sowie einen Ordner für jede Beispielfunktion.

	Zum Einstieg ist es am einfachsten, den bereitgestellten Ordner *sample* zu verwenden. Später können Sie dem Ordner *sample* Ihre eigenen Funktionen hinzufügen oder einen beliebigen Ordner verwenden, der eine Datei *host.json* und Funktionsordner enthält.

5. Wenn Sie „WebJobs.Script.WebHost“ als Startprojekt verwenden:

	a. Legen Sie eine AzureWebJobsScriptRoot-Umgebungsvariable auf den vollständigen Pfad zum Ordner `sample` fest.

	b. Starten Sie Visual Studio neu, um den neuen Umgebungsvariablenwert zu übernehmen.

	Weitere Informationen über das Ausführen von HTTP-Triggerfunktionen finden Sie im Abschnitt [API-Schlüssel](#api-keys).

5. Öffnen Sie die Datei *sample\\host.json*, und fügen Sie eine `functions`-Eigenschaft hinzu, um anzugeben, welche Funktionen ausgeführt werden sollen.

	Beispielsweise veranlasst die folgende JSON den WebJobs-SDK-JobHost dazu, nur nach zwei Funktionen zu suchen.

		{
		  "functions": [ "TimerTrigger-CSharp", "QueueTrigger-CSharp"],
		  "id": "5a709861cab44e68bfed5d2c2fe7fc0c"
		}

	Wenn Sie anstelle des Ordners *sample* Ihren eigenen Ordner verwenden, legen Sie darin nur die Funktionen ab, die Sie ausführen möchten. Anschließend können Sie die `functions`-Eigenschaft in *host.json* auslassen.
 
6. Erstellen Sie das Projekt, und führen Sie es aus.

	Das Konsolenfenster zeigt, dass der JobHost nur die in der Datei `host.json` angegebenen Funktionen findet.

		Found the following functions:
		Host.Functions.QueueTrigger-CSharp
		Host.Functions.TimerTrigger-CSharp
		Job host started

	Wenn Sie das WebHost-Projekt starten, erhalten Sie eine leere Browserseite, da kein Inhalt vorhanden ist, der als Basis-URL des Projekts dienen kann. Informationen zu Verwenden von URLs für HTTP-Triggerfunktionen finden Sie im Abschnitt [API-Schlüssel](#apikeys).

## Anzeigen der Funktionsausgabe

Wechseln Sie zum Dashboard für Ihre Funktions-App, um Funktionsaufrufe anzuzeigen und Ausgabe für sie zu protokollieren.

Das Dashboard befindet sich unter folgender URL:

	https://{function app name}.scm.azurewebsites.net/azurejobs/#/functions

Die Seite **Funktionen** zeigt eine Liste der ausgeführten Funktionen und eine Liste der Funktionsaufrufe an.

![Aufrufdetail](./media/functions-run-local/invocationdetail.png)

Klicken Sie auf einen Aufruf, um die Seite **Aufrufdetails** anzuzeigen. Diese gibt den Zeitpunkt, zu dem die Funktion ausgelöst wurde, die ungefähre Laufzeit und den erfolgreichen Abschluss an. Klicken Sie auf die Schaltfläche **Ausgabe umschalten**, um vom Funktionscode geschriebene Protokolle anzuzeigen.

![Aufrufdetail](./media/functions-run-local/invocationdetail.png)

## <a id="apikeys"></a> API-Schlüssel für HTTP-Trigger

Zum Ausführen einer HTTP- oder Webhookfunktion benötigen Sie einen API-Schlüssel, oder Sie müssen `"authLevel": "anonymous"` in die Datei *function.json* aufnehmen.

Wenn der API-Schlüssel beispielsweise `12345` lautet, können Sie die *HttpTrigger*-Funktion mit der folgenden URL auslösen, wenn das Projekt „WebJobs.Script.WebHost“ ausgeführt wird.

	http://localhost:28549/api/httptrigger?code=12345

(Alternativ dazu können Sie den API-Schlüssel im `x-functions-key`-HTTP-Header einfügen.)

API-Schlüssel werden in `.json`-Dateien im Ordner [App\_Data/secrets](https://github.com/Azure/azure-webjobs-sdk-script/tree/master/src/WebJobs.Script.WebHost/App_Data/secrets) im Projekt „WebJobs.Script.WebHost“ gespeichert.

### API-Schlüssel, die für alle HTTP- und WebHook-Funktionen gelten

Die Datei *host.json* im Ordner *App\_Data/secrets* enthält zwei Schlüssel:

```json
{
  "masterKey": "hyexydhln844f2mb7hyexydhln844f2mb7",
  "functionKey": "7hyexydhn844f2mb7hyexydhln844f2mb7"
}
```

Die `functionKey`-Eigenschaft speichert einen Schlüssel, der für jede HTTP- oder Webhook-Funktion verwendet werden kann, wenn für diese bestimmte Funktion keine Außerkraftsetzung definiert ist. Dadurch entfällt die Notwendigkeit, immer neue API-Schlüssel für jede von Ihnen erstellte Funktion zu definieren.

Die `masterKey`-Eigenschaft speichert einen Schlüssel, der in einigen Testszenarios nützlich ist:

* Wenn Sie eine WebHook-Funktion mit einem Hauptschlüssel aufrufen, umgeht das WebJobs-SDK die Überprüfung der Signatur des WebHook-Anbieters.

* Wenn Sie eine HTTP- oder Webhook-Funktion mit einem Hauptschlüssel aufrufen, wird die Funktion auch dann ausgelöst, wenn sie in der Datei *function.json* deaktiviert ist. Im Azure-Portal steht hiermit die Schaltfläche **Ausführen** selbst für deaktivierte Funktionen zur Verfügung.
 
### API-Schlüssel, die für einzelne Funktionen gelten

Dateien mit dem Namen *{Funktionsname}.json* enthalten den API-Schlüssel für eine bestimmte Funktion. Beispielsweise wird durch den folgenden JSON-Beispielinhalt in *App\_Data/secrets/HttpTrigger.json* der API-Schlüssel für die `HttpTrigger`-Funktion festgelegt.

```json
{
  "key": "844f2mdhn844f2mb7hyexydhln844f2mb7"
}
```

## Verwenden von NuGet-Paketverweisen in Funktionen  

Aufgrund der Art, auf die NuGet-Verweise zurzeit verarbeitet werden, müssen Sie sicherstellen, dass Sie während der Ausführung des Hosts einen Schreibvorgang mit der Datei *project.json* ausführen („touch“). Der Host überwacht Dateiänderungen und löst eine Wiederherstellung aus, wenn Änderungen erkannt werden. *NuGet.exe* (3.3.0 empfohlen) muss sich außerdem in Ihrem Pfad befinden, oder die Umgebungsvariable „AzureWebJobs\_NuGetPath“ muss auf den Pfad zu *NuGet.exe* festgelegt werden.

## Problembehandlung

Änderungen an Umgebungsvariablen, die während der Ausführung von Visual Studio vorgenommen werden, werden nicht automatisch angewendet. Wenn Sie eine Umgebungsvariable nach dem Start von Visual Studio geändert oder hinzugefügt haben, fahren Sie Visual Studio herunter, und starten Sie es neu, um sicherzustellen, dass die aktuellen Werte übernommen werden.

Beim Debuggen erhalten Sie möglicherweise weitere Informationen zu Ausnahmen, indem Sie im Fenster **Ausnahmeeinstellungen** die Option **Common Language Runtime Exceptions** (Common Language Runtime-Ausnahmen) auswählen (STRG-ALT-E zum Öffnen des Fensters).

Eine andere Möglichkeit, während des Debuggens weitere Ausnahmeinformationen zu erhalten, besteht darin, einen Haltepunkt im `catch`-Block der Hauptschleife für den Skripthost festzulegen. Diesen finden Sie im Projekt „WebJobs.Script“ in *Host/ScriptHostManager.cs* in der `RunAndBlock`-Methode.

## Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Entwicklerreferenz zu Azure Functions](functions-reference.md)
* [C#-Entwicklerreferenz zu Azure Functions](functions-reference-csharp.md)
* [F#-Entwicklerreferenz zu Azure Functions](functions-reference-fsharp.md)
* [NodeJS-Entwicklerreferenz zu Azure Functions](functions-reference-node.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)

<!---HONumber=AcomDC_0921_2016-->