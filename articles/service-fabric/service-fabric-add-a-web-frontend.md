---
title: Erstellen eines Web-Front-End für Ihre Anwendung mithilfe von ASP.NET Core | Microsoft Docs
description: Stellen Sie die Service Fabric-Anwendung mithilfe eines ASP.NET Core-Web-API-Projekts sowie mit Kommunikation zwischen Diensten über ServiceProxy für das Web bereit.
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: ''

ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/11/2016
ms.author: seanmck

---
# Erstellen eines Webdienst-Front-Ends für Ihre Anwendung mithilfe von ASP.NET Core
Standardmäßig enthalten Azure Service Fabric-Dienste keine öffentliche Web-Schnittstelle. Um die Funktionalität Ihrer Anwendung für HTTP-Clients verfügbar zu machen, müssen Sie ein Webprojekt als Einstiegspunkt erstellen und dann darüber mit den einzelnen Diensten kommunizieren.

In diesem Tutorial knüpfen wir nahtlos an das Tutorial [Erstellen Ihrer ersten Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) an und fügen vor dem zustandsbehafteten Zählerdienst einen Webdienst hinzu. Falls Sie das Tutorial noch nicht absolviert haben, holen Sie dies nach, bevor Sie mit diesem Tutorial fortfahren.

## Hinzufügen eines ASP.NET Core-Diensts zu Ihrer Anwendung
ASP.NET Core ist ein einfaches, plattformübergreifendes Webentwicklungsframework, das Sie zur Erstellung von modernen Benutzeroberflächen und Web-APIs verwenden können. Nun fügen wir unserer vorhandenen Anwendung ein ASP.NET Web API-Projekt hinzu.

> [!NOTE]
> Für dieses Tutorial müssen Sie [.NET Core 1.0 installieren][dotnetcore-install].
> 
> 

1. Klicken Sie im Projektmappen-Explorer im Anwendungsprojekt mit der rechten Maustaste auf **Dienste**, und wählen Sie **Hinzufügen > Neuer Service Fabric-Dienst** aus.
   
    ![Hinzufügen eines neuen Dienstes zu einer vorhandenen Anwendung][vs-add-new-service]
2. Wählen Sie auf der Seite **Erstellen eines Diensts** die Option **ASP.NET Core** aus, und legen Sie einen Namen fest.
   
    ![Auswählen eines neuen ASP.NET-Webdienstes im Dialogfeld „Neuer Dienst“][vs-new-service-dialog]
3. Die nächste Seite umfasst eine Reihe von ASP.NET Core-Projektvorlagen. Hierbei handelt es sich um die gleichen Vorlagen wie beim Erstellen eines ASP.NET Core-Projekts außerhalb einer Service Fabric-Anwendung. In diesem Tutorial wählen wir **Web-API**. Sie können jedoch mit denselben Konzepten eine vollständige Webanwendung erstellen.
   
    ![Auswählen des ASP.NET-Projekttyps][vs-new-aspnet-project-dialog]
   
    Nach Erstellung Ihres Web-API-Projekts stehen Ihnen zwei Dienste in Ihrer Anwendung zur Verfügung. Bei der weiteren Erstellung Ihrer Anwendung fügen Sie weitere Dienste auf die gleiche Weise hinzu. Diese können unabhängig versioniert und aktualisiert werden.

> [!TIP]
> Weitere Informationen zum Erstellen von ASP.NET Core-Diensten finden Sie in der [ASP.NET Core-Dokumentation](https://docs.asp.net).
> 
> 

## Ausführen der Anwendung
Um uns ein Bild von unserer Arbeit zu machen, stellen wir die neue Anwendung bereit und sehen uns das Standardverhalten der Web-API-Vorlage für ASP.NET Core an.

1. Drücken Sie in Visual Studio die Taste F5, um die App zu debuggen.
2. Nach Abschluss der Bereitstellung startet Visual Studio den Browser am Stamm des ASP.NET-Web-API-Diensts. Beispiel: http://localhost:33003. Die Portnummer wird nach dem Zufallsprinzip zugewiesen und kann auf Ihrem Computer anders sein. Da die Web-API-Vorlage für ASP.NET Core kein Standardverhalten für den Stamm bereitstellt, wird im Browser eine Fehlermeldung angezeigt.
3. Fügen Sie dem Speicherort im Browser `/api/values` hinzu. Dadurch wird die `Get`-Methode für den ValuesController in der Web-API-Vorlage aufgerufen. Dieser gibt die standardmäßige Antwort, die von der Vorlage bereitgestellt wird – ein JSON-Array, das zwei Zeichenfolgen enthält:
   
    ![Von der Web-API-Vorlage für ASP.NET Core zurückgegebene Standardwerte][browser-aspnet-template-values]
   
    Am Ende dieses Tutorials werden wir diese Standardwerte mit dem letzten Zählerwert aus unserem zustandsbehafteten Dienst ersetzen.

## Verbinden der Dienste
Service Fabric bietet absolute Flexibilität bei der Kommunikation mit Reliable Services. Innerhalb einer einzelnen Anwendung haben Sie womöglich Dienste, die über TCP zugänglich sind, andere Dienste, auf die über eine HTTP-REST-API zugegriffen werden kann und wiederum andere Dienste, auf die über WebSockets zugegriffen werden kann. Ausführliche Informationen zu den verfügbaren Optionen und deren Vor- und Nachteilen finden Sie unter [Kommunizieren mit Diensten](service-fabric-connect-and-communicate-with-services.md). In diesem Tutorial greifen wir auf einen einfacheren Ansatz zurück und verwenden die im SDK bereitgestellten `ServiceProxy`-/`ServiceRemotingListener`-Klassen.

Beim `ServiceProxy`-Ansatz (modelliert nach Remoteprozeduraufrufen oder RPCs) definieren Sie eine Schnittstelle, die als öffentlicher Vertrag für den Dienst fungiert. Anschließend verwenden Sie diese Schnittstelle zum Generieren einer Proxyklasse für die Interaktion mit dem Dienst.

### Erstellen der Benutzeroberfläche
Wir erstellen zunächst die Benutzeroberfläche, die als Vertrag zwischen dem zustandsbehafteten Dienst und dessen Clients fungiert (einschließlich des ASP.NET Core-Projekts).

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie dann **Hinzufügen** > **Neues Projekt** aus.
2. Wählen Sie im linken Navigationsbereich den Eintrag **Visual C#** und dann die Vorlage **Klassenbibliothek** aus. Vergewissern Sie sich, dass die .NET-Framework-Version **4.5.2** verwendet wird.
   
    ![Erstellen eines Schnittstellenprojekts für Ihren zustandsbehafteten Dienst][vs-add-class-library-project]
3. Damit eine Schnittstelle von `ServiceProxy` verwendet werden kann, muss sie von der IService-Schnittstelle abgeleitet werden. Diese Schnittstelle ist in einem der Service Fabric-NuGet-Pakete enthalten. Klicken Sie zum Hinzufügen des Pakets mit der rechten Maustaste auf das neue Klassenbibliotheksprojekt, und wählen Sie **NuGet-Pakete verwalten** aus.
4. Suchen Sie nach dem Paket **Microsoft.ServiceFabric.Services**, und installieren Sie es.
   
    ![Abrufen des Services NuGet-Pakets][vs-services-nuget-package]
5. Erstellen Sie in der Klassenbibliothek eine Schnittstelle mit einer einzelnen Methode, `GetCountAsync`, und erweitern Sie die Schnittstelle von IService.
   
    ```c#
    namespace MyStatefulService.Interfaces
    {
        using Microsoft.ServiceFabric.Services.Remoting;
   
        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```

### Implementieren der Schnittstelle in Ihrem zustandsbehafteten Dienst
Nach dem Definieren der Schnittstelle müssen wir sie in den zustandsbehafteten Dienst implementieren.

1. Fügen Sie Ihrem zustandsbehafteten Dienst einen Verweis auf das Klassenbibliotheksprojekt zu, das die Schnittstelle enthält.
   
    ![Hinzufügen eines Verweises auf das Klassenbibliotheksprojekt im zustandsbehafteten Dienst][vs-add-class-library-reference]
2. Suchen Sie die Klasse, die von `StatefulService` erbt (etwa `MyStatefulService`), und erweitern Sie sie, um die `ICounter`-Schnittstelle zu implementieren.
   
    ```c#
    using MyStatefulService.Interfaces;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
          // ...
    }
    ```
3. Implementieren Sie jetzt die einzelne Methode, die in der `ICounter`-Schnittstelle definiert ist: `GetCountAsync`.
   
    ```c#
    public async Task<long> GetCountAsync()
    {
      var myDictionary =
        await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
   
        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```

### Bereitstellen des zustandsbehafteten Diensts mithilfe eines Dienstremoting-Listeners
Nach dem Implementieren der `ICounter`-Schnittstelle besteht der letzte Schritt bei der Aktivierung des zustandsbehafteten Diensts darin, von anderen Diensten aufgerufen zu werden, um einen Kommunikationskanal zu öffnen. Für zustandsbehaftete Dienste bietet Service Fabric eine überschreibbare Methode namens `CreateServiceReplicaListeners`. Mit dieser Methode können Sie eine oder mehrere Kommunikationslistener angeben, basierend auf der Art der Kommunikation, die Sie in ihrem Dienst aktivieren möchten.

> [!NOTE]
> Die entsprechende Methode zum Öffnen eines Kommunikationskanals für zustandslose Dienste lautet `CreateServiceInstanceListeners`.
> 
> 

In diesem Fall ersetzen wir die vorhandene `CreateServiceReplicaListeners`-Methode und stellen eine Instanz von `ServiceRemotingListener` bereit. Diese erstellt einen RPC-Endpunkt, der von Clients über `ServiceProxy` aufgerufen werden kann.

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### Verwenden der ServiceProxy-Klasse für die Interaktion mit dem Dienst
Der zustandsbehaftete Dienst ist nun bereit, Datenverkehr von anderen Diensten zu empfangen. Es fehlt also nur noch das Hinzufügen des Codes, um durch ASP.NET Web Service mit dem Dienst zu kommunizieren.

1. Fügen Sie Ihrem ASP.NET-Projekt einen Verweis auf die Klassenbibliothek mit der `ICounter`-Schnittstelle hinzu.
2. Öffnen Sie über das Menü **Build** den **Konfigurations-Manager**. Die Ausgabe sollte folgendermaßen aussehen:
   
    ![Konfigurations-Manager mit Klassenbibliothek als AnyCPU][vs-configuration-manager]
   
    Beachten Sie, dass für die Klassenbibliothekprojekt **MyStatefulService.Interface** eine CPU-unabhängige Erstellung konfiguriert ist. Um eine problemlose Verwendung mit Service Fabric zu gewährleisten, muss es explizit auf x64 ausgerichtet sein. Klicken Sie auf die Dropdownliste „Plattform“, wählen Sie **Neu** aus, und erstellen Sie dann eine x64-Plattformkonfiguration.
   
    ![Erstellen einer neuen Plattform für die Klassenbibliothek][vs-create-platform]
3. Fügen Sie dem ASP.NET-Projekt das Microsoft.ServiceFabric.Services-Paket auf dieselbe Weise hinzu wie zuvor beim Klassenbibliotheksprojekt. Dadurch wird die `ServiceProxy`-Klasse bereitgestellt.
4. Öffnen Sie im Ordner **Controller** die `ValuesController`-Klasse. Beachten Sie, dass die `Get`-Methode derzeit nur ein hartcodiertes Zeichenfolgenarray mit „value1“ und „value2“ zurückgibt, wie wir vorhin im Browser gesehen haben. Ersetzen Sie diese Implementierung durch den folgenden Code:
   
    ```c#
    using MyStatefulService.Interfaces;
    using Microsoft.ServiceFabric.Services.Remoting.Client;
   
    ...
   
    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));
   
        long count = await counter.GetCountAsync();
   
        return new string[] { count.ToString() };
    }
    ```
   
    Die erste Codezeile ist der Schlüssel. Zum Erstellen des ICounter-Proxys für den zustandsbehafteten Dienst müssen Sie zwei Informationen angeben: die Partitions-ID und den Namen des Dienstes.
   
    Sie können durch die Partitionierung zustandsbehaftete Dienste skalieren, indem Sie ihren Status in verschiedene Buckets aufteilen, die auf einem von Ihnen definierten Schlüssel basieren, z. B. „Kunden-ID“ oder „PLZ“. In unserer einfachen Anwendung verfügt der zustandsbehaftete Dienst nur über eine Partition, sodass der Schlüssel keine Rolle spielt. Jeder Schlüssel, den Sie bereitstellen, wird auf dieselbe Partition führen. Weitere Informationen zum Partitionieren von Diensten finden Sie unter [Partitionieren von Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).
   
    Der Dienstname ist eine URI der Formular-Fabric: /&lt;application\_name&gt;/&lt;service\_name&gt;.
   
    Mit diesen zwei Angaben kann Service Fabric den Computer eindeutig identifizieren, an den Anforderungen gesendet werden sollen. Die `ServiceProxy`-Klasse greift auch dann nahtlos ein, wenn der Computer ausfällt, der die Partition des zustandsbehafteten Diensts hostet, und ein anderer Computer dessen Stelle einnehmen muss. Diese Abstraktion vereinfacht das Schreiben des Clientcodes für den Umgang mit anderen Diensten beträchtlich.
   
    Sobald wir den Proxy haben, rufen wir einfach die `GetCountAsync`-Methode auf und geben das Ergebnis zurück.
5. Drücken Sie erneut F5, um die geänderte Anwendung auszuführen. Wie zuvor startet Visual Studio automatisch den Browser zum Stamm des Webprojekts. Fügen Sie den „api/Values“-Pfad hinzu und der aktuelle Zählerwert sollte zurückgegeben werden.
   
    ![Anzeige des zustandsbehafteten Zählerwerts im Browser][browser-aspnet-counter-value]
   
    Aktualisieren Sie den Browser regelmäßig, um den aktuellen Zählerwert anzuzeigen.

> [!WARNING]
> Der in der Vorlage bereitgestellte, als Kestrel bekannte ASP.NET Core-Webserver wird [derzeit für die Verarbeitung direkten Internetverkehrs nicht unterstützt](https://docs.asp.net/en/latest/fundamentals/servers.html#kestrel). In Produktionsszenarien sollten Sie Ihre ASP.NET Core-Endpunkte hinter einem [API Management][api-management-landing-page]-Gateway oder einem anderen Gateway für den Internetzugriff hosten. Beachten Sie, dass Service Fabric für die Bereitstellung innerhalb von IIS nicht unterstützt wird.
> 
> 

## Was wird mit Akteuren verfahren?
Dieses Tutorial konzentriert sich auf das Hinzufügen des Web-Front-Ends, das mit einem zustandsbehafteten Dienst kommuniziert. Allerdings können Sie ein sehr ähnliches Modell befolgen, um mit Akteuren zu sprechen. Das ist sogar einfacher.

Wenn Sie ein Akteur-Projekt erstellen, generiert Visual Studio automatisch ein Schnittstellenprojekt für Sie. Diese Schnittstelle können Sie verwenden, um einen Akteur-Proxy im Webprojekt für die Kommunikation mit den Akteur zu generieren. Der Kommunikationskanal wird automatisch bereitgestellt. Es ist also nicht erforderlich, Vorgänge wie die `ServiceRemotingListener`-Einrichtung wie beim zustandsbehafteten Dienst in diesem Tutorial durchzuführen.

## Funktionsweise von Webdiensten auf Ihrem lokalen Cluster
Im Allgemeinen können Sie genau die gleiche Service Fabric-Anwendung in einem Cluster mit mehreren Computern bereitstellen, die Sie auf dem lokalen Cluster bereitgestellt haben, und sicher sein, dass es wie erwartet funktioniert. Dies ist der Fall, da der lokale Cluster einfach eine Konfiguration mit fünf Knoten ist, die auf einen einzelnen Computer reduziert ist.

Bei Webdiensten gibt es jedoch eine wichtige Nuance. Wenn sich Ihr Cluster wie in Azure hinter einem Load Balancer befindet, müssen Sie sicherstellen, dass Ihre Webdienste auf jedem Computer bereitgestellt werden, da der Load Balancer den Datenverkehr einfach auf die Computer verteilt. Legen Sie hierzu für den `InstanceCount` des Diensts den Sonderwert „-1“ fest.

Im Gegensatz dazu müssen Sie, wenn Sie einen Webdienst lokal ausführen, sicherstellen, dass nur eine Instanz des Diensts ausgeführt wird. Andernfalls entstehen Konflikten aus mehreren Prozessen, die den Pfad und den Port überwachen. Daher sollte bei lokalen Bereitstellungen der Wert für die Webdienstinstanzen „1“ lauten.

Informationen zum Konfigurieren verschiedener Werte für andere Umgebungen finden Sie unter [Verwalten von Anwendungsparametern für mehrere Umgebungen](service-fabric-manage-multiple-environment-app-configuration.md).

## Nächste Schritte
* [Erstellen eines Cluster in Azure zum Bereitstellen der Anwendung in der Cloud](service-fabric-cluster-creation-via-portal.md)
* [Weitere Informationen zum Kommunizieren mit Diensten](service-fabric-connect-and-communicate-with-services.md)
* [Weitere Informationen über die Partitionierung zustandsbehafteter Dienste](service-fabric-concepts-partitioning.md)

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-configuration-manager]: ./media/service-fabric-add-a-web-frontend/vs-configuration-manager.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
[api-management-landing-page]: https://azure.microsoft.com/de-DE/services/api-management/

<!---HONumber=AcomDC_0817_2016-->