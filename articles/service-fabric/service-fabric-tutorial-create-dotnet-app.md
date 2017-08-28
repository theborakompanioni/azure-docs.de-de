---
title: "Erstellen einer .NET-Anwendung für Service Fabric | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine Anwendung mit einem ASP.NET Core-Front-End und einem zustandsbehafteten zuverlässigen Dienst-Back-End erstellen und in einem Cluster bereitstellen."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi, mikhegn
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: ef50adf3af19bce494c3256308b443c8eaccdcea
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---

# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a>Erstellen und Bereitstellen einer Anwendung mit einem ASP.NET Core-Web-API-Front-End-Dienst und einem zustandsbehafteten Back-End-Dienst
Dieses Tutorial ist der erste Teil einer Serie.  Hier erfahren Sie, wie Sie eine Azure Service Fabric-Anwendung mit einem ASP.NET Core-Web-API-Front-End und einem zustandsbehafteten Back-End-Dienst zum Speichern Ihrer Daten erstellen. Am Ende verfügen Sie über eine Abstimmungsanwendung mit einem ASP.NET Core-Web-Front-End, mit der Abstimmungsergebnisse im Cluster in einem zustandsbehafteten Back-End-Dienst gespeichert werden. Wenn Sie die Abstimmungsanwendung nicht manuell erstellen möchten, können Sie den [Quellcode für die fertige Anwendung herunterladen](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) und mit [Durchlaufen der Beispielanwendung für die Abstimmung](#walkthrough_anchor) fortfahren.

![Anwendungsdiagramm](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Im ersten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines ASP.NET Core-Web-API-Diensts als zustandsbehafteter zuverlässiger Dienst
> * Erstellen eines ASP.NET Core-Webanwendungsdiensts als zustandsloser Webdienst
> * Kommunizieren mit dem zustandsbehafteten Dienst unter Verwendung des Reverseproxys

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen einer .NET Service Fabric-Anwendung
> * [Bereitstellen der Anwendung in einem Remotecluster](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Konfigurieren von CI/CD mit Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:
- Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Installieren Sie Visual Studio 2017](https://www.visualstudio.com/) und die Workloads **Azure-Entwicklung** und **ASP.NET und Webentwicklung**.
- [Installieren Sie das Service Fabric SDK.](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a>Erstellen eines ASP.NET-Web-API-Diensts als zuverlässigen Dienst
Erstellen Sie mithilfe von ASP.NET Core zunächst das Web-Front-End der Abstimmungsanwendung. ASP.NET Core ist ein einfaches, plattformübergreifendes Webentwicklungsframework, das Sie zur Erstellung von modernen Benutzeroberflächen und Web-APIs verwenden können. Damit Sie die Integration von ASP.NET Core mit Service Fabric vollständig verstehen, wird dringend empfohlen, den Artikel [ASP.NET Core in Service Fabric-Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) zu lesen. Für einen schnellen Einstieg reicht es jedoch, wenn Sie vorerst nur diesem Tutorial folgen. Weitere Informationen zu ASP.NET Core finden Sie in der [ASP.NET Core-Dokumentation](https://docs.microsoft.com/aspnet/core/).

> [!NOTE]
> Dieses Tutorial basiert auf den [ASP.NET Core-Tools für Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc). Die .NET Core-Tools für Visual Studio 2015 werden nicht mehr aktualisiert.

1. Starten Sie Visual Studio als **Administrator**.

2. Erstellen Sie mit **Datei**->**Neu**->**Projekt** ein Projekt.

3. Wählen Sie im Dialogfeld **Neues Projekt** die Option **Cloud > Service Fabric-Anwendung** aus.

4. Geben Sie der Anwendung den Namen **Voting**, und klicken Sie auf **OK**.

   ![Dialogfeld „Neues Projekt“ in Visual Studio](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. Wählen Sie auf der Seite **Neuer Service Fabric-Dienst** die Option **Zustandsloses ASP.NET Core** aus, und nennen Sie den Dienst **VotingWeb**.
   
   ![Auswählen eines neuen ASP.NET-Webdienstes im Dialogfeld „Neuer Dienst“](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. Die nächste Seite umfasst eine Reihe von ASP.NET Core-Projektvorlagen. Wählen Sie in diesem Tutorial die Option **Webanwendung** aus. 
   
   ![Auswählen des ASP.NET-Projekttyps](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   Visual Studio erstellt ein Anwendungs- und ein Dienstprojekt und zeigt sie im Projektmappen-Explorer an.

   ![Projektmappen-Explorer nach der Erstellung der Anwendung mit einem ASP.NET Core-Web-API-Dienst]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-to-the-votingweb-service"></a>Hinzufügen von „AngularJS“ zum Dienst „VotingWeb“
Fügen Sie [AngularJS](http://angularjs.org/) mithilfe der integrierten [Bower-Unterstützung](/aspnet/core/client-side/bower) zu Ihrem Dienst hinzu. Öffnen Sie *bower.json*, und fügen Sie Einträge für „angular“ und „angular-bootstrap“ hinzu. Speichern Sie Ihre Änderungen.

```json
{
  "name": "asp.net",
  "private": true,
  "dependencies": {
    "bootstrap": "3.3.7",
    "jquery": "2.2.0",
    "jquery-validation": "1.14.0",
    "jquery-validation-unobtrusive": "3.2.6",
    "angular": "v1.6.5",
    "angular-bootstrap": "v1.1.0"
  }
}
```
Wenn Sie die Datei *bower.json* speichern, wird Angular im Ordner *wwwroot/lib* Ihres Projekts installiert. Außerdem wird es im Ordner *Dependencies/Bower* aufgeführt.

### <a name="update-the-sitejs-file"></a>Aktualisieren der Datei „site.js“
Öffnen Sie die Datei *wwwroot/js/site.js*.  Ersetzen Sie den Inhalt der Datei durch den JavaScript-Code, der von den Home-Ansichten verwendet wird:

```javascript
var app = angular.module('VotingApp', ['ui.bootstrap']);
app.run(function () { });

app.controller('VotingAppController', ['$rootScope', '$scope', '$http', '$timeout', function ($rootScope, $scope, $http, $timeout) {

    $scope.refresh = function () {
        $http.get('api/Votes?c=' + new Date().getTime())
            .then(function (data, status) {
                $scope.votes = data;
            }, function (data, status) {
                $scope.votes = undefined;
            });
    };

    $scope.remove = function (item) {
        $http.delete('api/Votes/' + item)
            .then(function (data, status) {
                $scope.refresh();
            })
    };

    $scope.add = function (item) {
        var fd = new FormData();
        fd.append('item', item);
        $http.put('api/Votes/' + item, fd, {
            transformRequest: angular.identity,
            headers: { 'Content-Type': undefined }
        })
            .then(function (data, status) {
                $scope.refresh();
                $scope.item = undefined;
            })
    };
}]);
```

### <a name="update-the-indexcshtml-file"></a>Aktualisieren der Datei „Index.cshtml“
Öffnen Sie die Datei *Views/Home/Index.cshtml* (die spezifische Ansicht für den Home-Controller).  Ersetzen Sie den Inhalt durch Folgendes, und speichern Sie anschließend Ihre Änderungen:

```html
@{
    ViewData["Title"] = "Service Fabric Voting Sample";
}

<div ng-controller="VotingAppController" ng-init="refresh()">
    <div class="container-fluid">
        <div class="row">
            <div class="col-xs-8 col-xs-offset-2 text-center">
                <h2>Service Fabric Voting Sample</h2>
            </div>
        </div>

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <form class="col-xs-12 center-block">
                    <div class="col-xs-6 form-group">
                        <input id="txtAdd" type="text" class="form-control" placeholder="Add voting option" ng-model="item" />
                    </div>
                    <button id="btnAdd" class="btn btn-default" ng-click="add(item)">
                        <span class="glyphicon glyphicon-plus" aria-hidden="true"></span>
                        Add
                    </button>
                </form>
            </div>
        </div>

        <hr />

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <div class="row">
                    <div class="col-xs-4">
                        Click to vote
                    </div>
                </div>
                <div class="row top-buffer" ng-repeat="vote in votes.data">
                    <div class="col-xs-8">
                        <button class="btn btn-success text-left btn-block" ng-click="add(vote.key)">
                            <span class="pull-left">
                                {{vote.key}}
                            </span>
                            <span class="badge pull-right">
                                {{vote.value}} Votes
                            </span>
                        </button>
                    </div>
                    <div class="col-xs-4">
                        <button class="btn btn-danger pull-right btn-block" ng-click="remove(vote.key)">
                            <span class="glyphicon glyphicon-remove" aria-hidden="true"></span>
                            Remove
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

### <a name="update-the-layoutcshtml-file"></a>Aktualisieren der Datei „_Layout.cshtml“
Öffnen Sie die Datei *Views/Shared/_Layout.cshtml* (das Standardlayout für die ASP.NET-App).  Ersetzen Sie den Inhalt durch Folgendes, und speichern Sie anschließend Ihre Änderungen:

```html
<!DOCTYPE html>
<html ng-app="VotingApp" xmlns:ng="http://angularjs.org">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"]</title>

    <link href="~/lib/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet" />
    <link href="~/css/site.css" rel="stylesheet" />

</head>
<body>
    <div class="container body-content">
        @RenderBody()
    </div>

    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/lib/angular/angular.js"></script>
    <script src="~/lib/angular-bootstrap/ui-bootstrap-tpls.js"></script>
    <script src="~/js/site.js"></script>

    @RenderSection("Scripts", required: false)
</body>
</html>
```

### <a name="update-the-votingwebcs-file"></a>Aktualisieren der Datei „VotingWeb.cs“
Öffnen Sie die Datei *VotingWeb.cs* (erstellt den ASP.NET Core-Webhost innerhalb des zustandslosen Diensts unter Verwendung des WebListener-Webservers).  Fügen Sie am Anfang der Datei die Direktive `using System.Net.Http;` hinzu.  Ersetzen Sie die Funktion `CreateServiceInstanceListeners()` durch Folgendes, und speichern Sie anschließend Ihre Änderungen:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
            {
                ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting WebListener on {url}");

                return new WebHostBuilder().UseWebListener()
                            .ConfigureServices(
                                services => services
                                    .AddSingleton<StatelessServiceContext>(serviceContext)
                                    .AddSingleton<HttpClient>())
                            .UseContentRoot(Directory.GetCurrentDirectory())
                            .UseStartup<Startup>()
                            .UseApplicationInsights()
                            .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                            .UseUrls(url)
                            .Build();
            }))
    };
}
```

### <a name="add-the-votescontrollercs-file"></a>Hinzufügen der Datei „VotesController.cs“
Fügen Sie einen Controller zum Definieren von Abstimmungsaktionen hinzu. Klicken Sie mit der rechten Maustaste auf den Ordner **Controllers**, und wählen Sie **Hinzufügen > Neues Element > Klasse** aus.  Nennen Sie die Datei „VotesController.cs“, und klicken Sie auf **Hinzufügen**.  Ersetzen Sie den Inhalt der Datei durch Folgendes, und speichern Sie anschließend Ihre Änderungen:  In [Aktualisieren der Datei „VotesController.cs“](#updatevotecontroller_anchor) wird diese Datei später zum Lesen und Schreiben von Abstimmungsdaten aus dem Back-End-Dienst angepasst.  Vorläufig gibt der Controller statische Zeichenfolgendaten an die Ansicht zurück.

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using Newtonsoft.Json;
using System.Text;
using System.Net.Http;
using System.Net.Http.Headers;

namespace VotingWeb.Controllers
{
    [Produces("application/json")]
    [Route("api/Votes")]
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            List<KeyValuePair<string, int>> votes= new List<KeyValuePair<string, int>>();
            votes.Add(new KeyValuePair<string, int>("Pizza", 3));
            votes.Add(new KeyValuePair<string, int>("Ice cream", 4));

            return Json(votes);
        }
     }
}
```



### <a name="deploy-and-run-the-application-locally"></a>Lokales Bereitstellen und Ausführen der Anwendung
Sie können die Anwendung jetzt ausführen. Drücken Sie in Visual Studio `F5`, um die Anwendung für das Debuggen bereitzustellen. `F5` führt zu einem Fehler, wenn Sie nicht zuvor Visual Studio als **Administrator** geöffnet haben.

> [!NOTE]
> Wenn Sie die Anwendung zum ersten Mal lokal ausführen und bereitstellen, erstellt Visual Studio einen lokalen Cluster für das Debuggen.  Die Clustererstellung kann etwas dauern. Der Status der Clustererstellung wird im Ausgabefenster von Visual Studio angezeigt.

Ihre Web-App sollte nun wie folgt aussehen:

![ASP.NET Core-Front-End](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

Zum Beenden des Debuggens der Anwendung wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT+F5**.

## <a name="add-a-stateful-back-end-service-to-your-application"></a>Hinzufügen eines zustandsbehafteten Back-End-Diensts zur Anwendung
Nachdem nun ein ASP.NET-Web-API-Dienst in der Anwendung ausgeführt wird, fügen Sie als Nächstes einen zustandsbehafteten zuverlässigen Dienst hinzu, um einige Daten in der Anwendung zu speichern.

Service Fabric ermöglicht eine konsistente und zuverlässige Speicherung von Daten direkt innerhalb des Diensts anhand von Reliable Collections. Bei Reliable Collections handelt es sich um eine Gruppe hochverfügbarer und zuverlässiger Sammlungsklassen, die jedem vertraut sind, der bereits mit C#-Auflistungen gearbeitet hat.

In diesem Tutorial erstellen Sie einen Dienst, der einen Zählerwert in einer Reliable Collection speichert.

1. Klicken Sie im Projektmappen-Explorer im Anwendungsprojekt mit der rechten Maustaste auf **Dienste**, und wählen Sie **Hinzufügen > Neuer Service Fabric-Dienst** aus.
   
    ![Hinzufügen eines neuen Dienstes zu einer vorhandenen Anwendung](./media/service-fabric-tutorial-create-dotnet-app/vs-add-new-service.png)

2. Wählen Sie im Dialogfeld **Neuer Service Fabric-Dienst** die Option **Zustandsbehaftetes ASP.NET Core** aus, nennen Sie den Dienst **VotingData**, und wählen Sie **OK** aus.

    ![Dialogfeld „Neuer Dienst“ in Visual Studio](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    Nach Erstellung Ihres Dienstprojekts stehen Ihnen zwei Dienste in Ihrer Anwendung zur Verfügung. Bei der weiteren Erstellung Ihrer Anwendung können Sie weitere Dienste auf die gleiche Weise hinzufügen. Diese können unabhängig versioniert und aktualisiert werden.

3. Die nächste Seite umfasst eine Reihe von ASP.NET Core-Projektvorlagen. In diesem Tutorial wählen wir **Web-API**.

    ![Auswählen des ASP.NET-Projekttyps](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    Visual Studio erstellt ein Dienstprojekt und zeigt es im Projektmappen-Explorer an.

    ![Projektmappen-Explorer](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-the-votedatacontrollercs-file"></a>Hinzufügen der Datei „VoteDataController.cs“

Klicken Sie im Projekt **VotingData** mit der rechten Maustaste auf den Ordner **Controllers**, und wählen Sie **Hinzufügen > Neues Element > Klasse** aus. Nennen Sie die Datei „VoteDataController.cs“, und klicken Sie auf **Hinzufügen**. Ersetzen Sie den Inhalt der Datei durch Folgendes, und speichern Sie anschließend Ihre Änderungen:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.ServiceFabric.Data;
using System.Threading;
using Microsoft.ServiceFabric.Data.Collections;

namespace VotingData.Controllers
{
    [Route("api/[controller]")]
    public class VoteDataController : Controller
    {
        private readonly IReliableStateManager stateManager;

        public VoteDataController(IReliableStateManager stateManager)
        {
            this.stateManager = stateManager;
        }

        // GET api/VoteData
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            var ct = new CancellationToken();

            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                var list = await votesDictionary.CreateEnumerableAsync(tx);

                var enumerator = list.GetAsyncEnumerator();

                var result = new List<KeyValuePair<string, int>>();

                while (await enumerator.MoveNextAsync(ct))
                {
                    result.Add(enumerator.Current);
                }

                return Json(result);
            }
        }

        // PUT api/VoteData/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
                await tx.CommitAsync();
            }

            return new OkResult();
        }

        // DELETE api/VoteData/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                if (await votesDictionary.ContainsKeyAsync(tx, name))
                {
                    await votesDictionary.TryRemoveAsync(tx, name);
                    await tx.CommitAsync();
                    return new OkResult();
                }
                else
                {
                    return new NotFoundResult();
                }
            }
        }
    }
}
```


## <a name="connect-the-services"></a>Verbinden der Dienste
Im diesem Schritt verbinden Sie die beiden Dienste miteinander und sorgen dafür, dass die Front-End-Webanwendung Abstimmungsinformationen aus dem Back-End-Dienst abruft und festlegt.

Service Fabric bietet absolute Flexibilität bei der Kommunikation mit Reliable Services. Sie können in einer Anwendung über Dienste verfügen, auf die über TCP zugegriffen werden kann. Auf andere Dienste wird möglicherweise über eine HTTP-REST-API zugegriffen, und bei wieder anderen Diensten erfolgt der Zugriff über Websockets. Ausführliche Informationen zu den verfügbaren Optionen und deren Vor- und Nachteilen finden Sie unter [Kommunizieren mit Diensten](service-fabric-connect-and-communicate-with-services.md).

In diesem Tutorial verwenden wir die [ASP.NET Core-Web-API](service-fabric-reliable-services-communication-aspnetcore.md).

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-the-votescontrollercs-file"></a>Aktualisieren der Datei „VotesController.cs“
Öffnen Sie im Projekt **VotingWeb** die Datei *Controllers/VotesController.cs*.  Ersetzen Sie den Inhalt der Definition der Klasse `VotesController` durch Folgendes, und speichern Sie anschließend Ihre Änderungen:

```csharp
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;
        string serviceProxyUrl = "http://localhost:19081/Voting/VotingData/api/VoteData";
        string partitionKind = "Int64Range";
        string partitionKey = "0";

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            IEnumerable<KeyValuePair<string, int>> votes;

            HttpResponseMessage response = await this.httpClient.GetAsync($"{serviceProxyUrl}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            votes = JsonConvert.DeserializeObject<List<KeyValuePair<string, int>>>(await response.Content.ReadAsStringAsync());

            return Json(votes);
        }

        // PUT: api/Votes/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            string payload = $"{{ 'name' : '{name}' }}";
            StringContent putContent = new StringContent(payload, Encoding.UTF8, "application/json");
            putContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");

            string proxyUrl = $"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}";

            HttpResponseMessage response = await this.httpClient.PutAsync(proxyUrl, putContent);

            return new ContentResult()
            {
                StatusCode = (int)response.StatusCode,
                Content = await response.Content.ReadAsStringAsync()
            };
        }

        // DELETE: api/Votes/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            HttpResponseMessage response = await this.httpClient.DeleteAsync($"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            return new OkResult();

        }
    }
```
<a id="walkthrough" name="walkthrough_anchor"></a>

## <a name="walk-through-the-voting-sample-application"></a>Durchlaufen der Beispielanwendung für die Abstimmung
Die Abstimmungsanwendung besteht aus zwei Diensten:
- Web-Front-End-Dienst (VotingWeb): Ein ASP.NET Core-Web-Front-End-Dienst, der die Webseite bereitstellt und Web-APIs für die Kommunikation mit dem Back-End-Dienst verfügbar macht.
- Back-End-Dienst (VotingData): Ein ASP.NET Core-Web-Dienst, der eine API verfügbar macht, um die Abstimmungsergebnisse in einem zuverlässigen Wörterbuch zu speichern, das dauerhaft auf dem Datenträger vorhanden ist.

![Anwendungsdiagramm](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Beim Abstimmen in der Anwendung treten die folgenden Ereignisse ein:
1. Ein JavaScript sendet die Abstimmungsanforderung als HTTP PUT-Anforderung an die Web-API im Web-Front-End-Dienst.

2. Der Web-Front-End-Dienst nutzt einen Proxy, um eine HTTP PUT-Anforderung zu lokalisieren und an den Back-End-Dienst weiterzuleiten.

3. Der Back-End-Dienst erhält die eingehende Anforderung und speichert das aktualisierte Ergebnis in einem zuverlässigen Wörterbuch, das auf mehreren Knoten im Cluster repliziert und dauerhaft auf dem Datenträger gespeichert wird. Alle Daten der Anwendung werden im Cluster gespeichert, sodass keine Datenbank erforderlich ist.

## <a name="debug-in-visual-studio"></a>Debuggen in Visual Studio
Beim Debuggen der Anwendung in Visual Studio verwenden Sie einen lokalen Service Fabric-Entwicklungscluster. Sie haben die Möglichkeit, Ihre Oberfläche für das Debuggen an Ihr Szenario anzupassen. In dieser Anwendung speichern wir Daten in unserem Back-End-Dienst, indem wir ein zuverlässiges Wörterbuch verwenden. Standardmäßig wird die Anwendung von Visual Studio entfernt, wenn Sie den Debugger beenden. Die Entfernung der Anwendung führt dazu, dass auch die Daten im Back-End-Dienst entfernt werden. Um die Daten zwischen den Debugsitzungen beizubehalten, können Sie den **Debugmodus für die Anwendung** als Eigenschaft im Projekt **Voting** in Visual Studio ändern.

Führen Sie die folgenden Schritte aus, um zu ermitteln, was im Code passiert:
1. Öffnen Sie die Datei **VotesController.cs**, und legen Sie in der **Put**-Methode der Web-API (Zeile 47) einen Breakpoint fest. Sie können in Visual Studio im Projektmappen-Explorer nach der Datei suchen.

2. Öffnen Sie die Datei **VoteDataController.cs**, und legen Sie in der **Put**-Methode dieser Web-API (Zeile 50) einen Breakpoint fest.

3. Wechseln Sie zurück in den Browser, und klicken Sie auf eine Abstimmungsoption, oder fügen Sie eine neue Abstimmungsoption hinzu. Sie stoßen im API-Controller des Web-Front-Ends auf den ersten Breakpoint.
    
    1. An diesem Punkt sendet das JavaScript im Browser eine Anforderung an den Web-API-Controller im Front-End-Dienst.
    
    ![Front-End-Dienst „Stimme hinzufügen“](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. Zunächst erstellen wir die URL zum Reverseproxy für unseren Back-End-Dienst **(1)**.
    3. Anschließend senden wir die HTTP-Anforderung PUT an den Reverseproxy **(2)**.
    4. Zum Schluss geben wir die Antwort vom Back-End-Dienst an den Client zurück **(3)**.

4. Drücken Sie **F5**, um fortzufahren.
    1. Sie befinden sich jetzt am Breakpoint im Back-End-Dienst.
    
    ![Back-End-Dienst „Stimme hinzufügen“](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. In der ersten Zeile der Methode **(1)** verwenden wir das `StateManager`-Element, um ein zuverlässiges Wörterbuch mit dem Namen `counts` abzurufen bzw. hinzuzufügen.
    3. Für alle Interaktionen mit Werten in einem zuverlässigen Wörterbuch ist eine Transaktion erforderlich. Diese Transaktion wird mithilfe der Anweisung **(2)** erstellt.
    4. In der Transaktion aktualisieren wir dann den Wert des relevanten Schlüssels für die Abstimmungsoption und committen den Vorgang **(3)**. Nachdem die Rückgabe für die Commit-Methode durchgeführt wurde, werden die Daten im Wörterbuch aktualisiert und auf anderen Knoten im Cluster repliziert. Die Daten sind jetzt sicher im Cluster gespeichert, und der Back-End-Dienst kann das Failover auf andere Knoten durchführen, während die Daten weiterhin verfügbar sind.
5. Drücken Sie **F5**, um fortzufahren.

Drücken Sie **UMSCHALT+F5**, um die Debugsitzung zu beenden.


## <a name="next-steps"></a>Nächste Schritte
In diesem Teil des Tutorials haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines ASP.NET Core-Web-API-Diensts als zustandsbehafteter zuverlässiger Dienst
> * Erstellen eines ASP.NET Core-Webanwendungsdiensts als zustandsloser Webdienst
> * Kommunizieren mit dem zustandsbehafteten Dienst unter Verwendung des Reverseproxys

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Bereitstellen der Anwendung für Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md)
