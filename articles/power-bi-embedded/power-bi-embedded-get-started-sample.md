<properties
   pageTitle="Erste Schritte mit dem Beispiel"
   description=""
   services="power-bi-embedded"
   documentationCenter=""
   authors="dvana"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="03/29/2016"
   ms.author="derrickv"/>

# Erste Schritte mit dem Beispiel zu Microsoft Power BI Embedded

Mit der **Vorschau von Microsoft Power BI Embedded** können Sie Power BI-Berichte in Ihre Web- oder mobilen Anwendungen integrieren, sodass Sie keine benutzerdefinierten Lösungen zum Visualisieren von Daten für Ihre Benutzer erstellen müssen. Die folgenden Ressourcen helfen Ihnen bei den ersten Schritten zur Integration von Power BI-Berichten in Ihre App.

 -	[Beispiel für eine Dashboard-Web-App](http://go.microsoft.com/fwlink/?LinkId=761493)
 -	[Power BI Embedded-API-Referenz](https://msdn.microsoft.com/library/mt712303.aspx)
 -	[Power BI Embedded .NET SDK (verfügbar über NuGet)](http://go.microsoft.com/fwlink/?LinkId=746472)

In diesem Artikel erhalten Sie eine Einführung in das Beispiel für den Einstieg in **Power BI Embedded**. Beginnen wir zunächst mit der Konfiguration der Beispiel-App, damit Sie die Beispiel-Web-App ausführen können.

> [AZURE.NOTE] Bevor Sie das Beispiel zu den ersten Schritten mit Power BI Embedded konfigurieren und ausführen können, müssen Sie mindestens eine **Arbeitsbereichssammlung** in Ihrem Azure-Abonnement erstellen. Informationen zum Erstellen einer **Arbeitsbereichssammlung** im Azure-Portal finden Sie unter [Erste Schritte mit der Vorschau von Power BI Embedded](power-bi-embedded-get-started.md).

## Konfigurieren der Beispiel-App

Im Folgenden werden Sie durch die Einrichtung ihrer Visual Studio-Entwicklungsumgebung geführt, um auf die Vorschaukomponenten zuzugreifen, die zum Ausführen der Beispiel-App erforderlich sind.

1. Laden Sie das Beispiel [Power BI Embedded – Integrate a report into a web app](http://go.microsoft.com/fwlink/?LinkId=761493) (Integrieren eines Berichts in eine Web-App) von GitHub herunter, und extrahieren Sie die Datei.

2. Öffnen Sie **PowerBI embedded.sln** in Visual Studio.

3. Erstellen Sie die Projektmappe.

4. Führen Sie die Konsolen-App **ProvisionSample** aus. In der Beispiel-App-Konsole stellen Sie einen Arbeitsbereich bereit und importieren eine PBIX-Datei.

5. Wählen Sie zum Bereitstellen eines neuen **Arbeitsbereichs** die Option **5. Bereitstellen eines neuen Arbeitsbereichs in einer vorhandenen Arbeitsbereichssammlung**.

    ![](media\powerbi-embedded-get-started-sample\console-option-5.png)

6. Geben Sie den Namen Ihrer **Arbeitsbereichssammlung** und den **Zugriffsschlüssel** ein. Sie können diese im **Azure-Portal** abrufen. Weitere Informationen zum Abrufen Ihres **Zugriffsschlüssels** finden Sie unter [Anzeigen von Power BI-API-Zugriffsschlüsseln](power-bi-embedded-get-started-sample.md#view-access-keys) in „Erste Schritte mit der Vorschau von Microsoft Power BI Embedded“.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

7. Kopieren und speichern Sie die neu erstellte **Arbeitsbereichs-ID**, um sie später in diesem Artikel zu verwenden. Nach der Erstellung ist die **Arbeitsbereichs-ID** im **Azure-Portal** zu finden.

    ![](media\powerbi-embedded-get-started-sample\workspace-id.png)

8. Zum Importieren einer PBIX-Datei in Ihren **Arbeitsbereich** wählen Sie die Option **6. Importieren Sie die PBIX-Desktopdatei in einen vorhandenen Arbeitsbereich**. Falls Sie keine PBIX-Datei parat haben, können Sie das [PBIX-Beispiel „Retail Analysis“](http://go.microsoft.com/fwlink/?LinkID=780547) herunterladen.

9. Wenn Sie dazu aufgefordert werden, geben Sie einen Anzeigenamen für Ihr **Dataset** ein.

Sie sollten in etwa folgende Rückgabe erhalten:

````
Checking import state... Publishing
Checking import state... Succeeded
```

> [AZURE.NOTE] Wenn Ihre PBIX-Datei DirectQuery-Verbindungen enthält, führen Sie Option 7 aus, um die Verbindungszeichenfolgen zu aktualisieren.

Jetzt haben Sie einen Power BI-PBIX-Bericht in Ihren **Arbeitsbereich** importiert. Im nächsten Abschnitt erfahren Sie, wie Sie die Beispiel-Web-App für die ersten Schritte in **Power BI Embedded** ausführen. Im nächsten Abschnitt erfahren Sie, wie Sie die Beispiel-Web-App ausführen.

## Ausführen der Beispiel-Web-App

Das Beispiel für die Web-App ist ein Beispieldashboard, das die in den **Arbeitsbereich** importierten Berichte rendert.

So konfigurieren Sie das Web-App-Beispiel

1. Klicken Sie in der Visual Studio-Projektmappe **PowerBI-embedded** mit der rechten Maustaste auf die Webanwendung **EmbedSample**, und wählen Sie **Als Startprojekt festlegen**.
2. Bearbeiten Sie in **web.config** in der Webanwendung **EmbedSample** die **appSettings**: **AccessKey**, **WorkspaceCollection**-Name und **WorkspaceId**.

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. Führen Sie die Webanwendung **EmbedSample** aus.

Nach dem Ausführen der Webanwendung **EmbedSample** enthält der Navigationsbereich auf der linken Seite ein Menü **Berichte**. Erweitern Sie zum Anzeigen des importierten Berichts den Eintrag **Berichte**, und klicken Sie auf einen Bericht. Wenn Sie das [PBIX-Beispiel „Retail Analysis“](http://go.microsoft.com/fwlink/?LinkID=780547) importiert haben, sieht die Beispiel-Web-App wie folgt aus:

![](media\powerbi-embedded-get-started-sample\power-bi-embedded-sample-left-nav.png)

Nachdem Sie auf einen Bericht geklickt haben, sieht die Webanwendung **EmbedSample** in etwa wie folgt aus:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)

Im nächsten Abschnitt wird der **Power BI Embedded**-Beispielcode untersucht.

## Untersuchen des Beispielcodes
Das **Microsoft Power BI Embedded**-Vorschaubeispiel ist eine Dashboard-Beispiel-Web-App, die zeigt, wie Sie **Power BI**-Berichte in Ihre App integrieren. Dabei werden die bewährten Methoden anhand eines MVC-Entwurfsmusters (Model-View-Controller) veranschaulicht. In diesem Abschnitt werden Teile des Beispielcodes hervorgehoben, die Sie innerhalb der Web-App-Projektmappe **PowerBI-embedded** untersuchen können. Das MVC-Muster (Model-View-Controller) trennt die Modellierung der Domäne, die Präsentation und die Aktionen basierend auf der Benutzereingabe in drei separate Klassen: „Model“, „View“ und „Control“. Weitere Informationen zu MVC finden Sie unter [Learn About ASP.NET](http://www.asp.net/mvc) (Informationen zu ASP.NET).

Der Beispielcode der **Microsoft Power BI Embedded**-Vorschau ist wie folgt unterteilt. Jeder Abschnitt enthält den Dateinamen in der Projektmappe „PowerBI-embedded.sln“, damit Sie den Code im Beispiel leicht finden können.

> [AZURE.NOTE] Dieser Abschnitt ist eine Zusammenfassung des Beispielcodes, die zeigt, wie der Code geschrieben wurde. Die Beschreibung des Beispiels wird erweitert, bis wir es demnächst allgemein verfügbar machen. Laden Sie die Projektmappe „PowerBI-embedded.sln“ in Visual Studio, um das vollständige Beispiel anzuzeigen.

### Modell
Das Beispiel umfasst ein **ReportsViewModel** und ein **ReportViewModel**.

**ReportsViewModel.cs**: Stellt die Power BI-Berichte dar.

    public class ReportsViewModel
    {
        public List<Report> Reports { get; set; }
    }

**ReportViewModel.cs**: Stellt einen Power BI-Bericht dar.

    public classReportViewModel
    {
        public IReport Report { get; set; }

        public string AccessToken { get; set; }
    }

### Sicht
In der **Sicht** wird die Anzeige von Power BI-**Berichten** und eines Power BI-**Berichts** verwaltet.

**Reports.cshtml**: Durchlaufen Sie **Model.Reports**, um einen **ActionLink** zu erstellen. Der **ActionLink** ist wie folgt zusammengesetzt:

|Teil|Beschreibung
|---|---
|Titel| Der Name des Berichts.
|QueryString| Ein Link zur Berichts-ID.

    <div id="reports-nav" class="panel-collapse collapse">
        <div class="panel-body">
            <ul class="nav navbar-nav">
                @foreach (var report in Model.Reports)
                {
                    var reportClass = Request.QueryString["reportId"] == report.Id ? "active" : "";
                    <li class="@reportClass">
                        @Html.ActionLink(report.Name, "Report", new { reportId = report.Id })
                    </li>
                }
            </ul>
        </div>
    </div>

Report.cshtml: Legen Sie das **Model.AccessToken** und den Lambda-Ausdruck für **PowerBIReportFor** fest.

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### Controller

**DashboardController.cs**: Erstellt einen PowerBIClient, der ein **App-Token** übergibt. Ein JSON-Webtoken (JWT) wird aus dem **Signaturschlüssel** generiert, um die **Anmeldeinformationen** zu erhalten. Die **Anmeldeinformationen** dienen zum Erstellen einer Instanz von **PowerBIClient**. Weitere Informationen zu **App-Token** finden Sie unter [Funktionsweise des App-Tokenflusses](#key-flow). Sobald Sie eine Instanz von **PowerBIClient** haben, können Sie GetReports() und GetReportsAsync() aufrufen.

CreatePowerBIClient()

    private IPowerBIClient CreatePowerBIClient(PowerBIToken token)
    {
        var jwt = token.Generate(accessKey);
        var credentials = new TokenCredentials(jwt, "AppToken");
        var client = new PowerBIClient(credentials)
        {
            BaseUri = new Uri(apiUrl)
        };

        return client;
    }

ActionResult Reports()

    public ActionResult Reports()
    {
        var devToken = PowerBIToken.CreateDevToken(this.workspaceCollection, this.workspaceId);
        using (var client = this.CreatePowerBIClient(devToken))
        {
            var reportsResponse = client.Reports.GetReports(this.workspaceCollection, this.workspaceId);

            var viewModel = new ReportsViewModel
            {
                Reports = reportsResponse.Value.ToList()
            };

            return PartialView(viewModel);
        }
    }


Task<ActionResult> Report(string reportId)

    public async Task<ActionResult> Report(string reportId)
    {
        var devToken = PowerBIToken.CreateDevToken(this.workspaceCollection, this.workspaceId);
        using (var client = this.CreatePowerBIClient(devToken))
        {
            var reportsResponse = await client.Reports.GetReportsAsync(this.workspaceCollection, this.workspaceId);
            var report = reportsResponse.Value.FirstOrDefault(r => r.Id == reportId);
            var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, Guid.Parse(report.Id));

            var viewModel = new ReportViewModel
            {
                Report = report,
                AccessToken = embedToken.Generate(this.accessKey)
            };

            return View(viewModel);
        }
    }

### Integrieren eines Berichts in Ihre App

Sobald Sie über einen **Bericht** verfügen, verwenden Sie einen **IFrame**, um den Power BI-**Bericht** einzubetten. Hier sehen Sie einen Codeausschnitt aus „powerbi.js“ im Vorschaubeispiel zu **Microsoft Power BI Embedded**.

![](media\powerbi-embedded-get-started-sample\power-bi-embedded-iframe-code.png)


### Filtern von in die Anwendung eingebetteten Berichten

Sie können einen eingebetteten Bericht mit einer URL-Syntax filtern. Fügen Sie hierfür der iFrame-SRC-URL einen Abfragezeichenfolgenparameter mit angegebenem Filter hinzu. Dies ist die Syntax der Filterabfrage:

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-0694-4c70-9673-ee9655d54a4a&
$filter={tableName/fieldName} eq '{fieldValue}'
```

> [AZURE.NOTE] {tableName/fieldName} darf keine Leerzeichen oder Sonderzeichen enthalten. Für {fieldValue} wird ein einzelner Kategoriewert akzeptiert.


## Weitere Informationen

- [Was ist Microsoft Power BI Embedded?](power-bi-embedded-what-is-power-bi-embedded.md)
- [Häufige Szenarien bei der Vorschau von Microsoft Power BI Embedded](power-bi-embedded-scenarios.md)
- [Erste Schritte mit der Vorschau von Microsoft Power BI Embedded](power-bi-embedded-get-started.md)
- [Informationen zum App-Tokenfluss in Power BI Embedded](power-bi-embedded-app-token-flow.md)

<!---HONumber=AcomDC_0413_2016-->