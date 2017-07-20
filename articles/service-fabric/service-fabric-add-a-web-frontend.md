---
title: "Erstellen eines Web-Front-Ends für Ihre Azure Service Fabric-App mithilfe von ASP.NET Core | Microsoft-Dokumentation"
description: "Machen Sie Ihre Service Fabric-Anwendung im Web verfügbar, indem Sie ein ASP.NET Core-Projekt und über Dienstremoting die Kommunikation zwischen Diensten verwenden."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.translationtype: Human Translation
ms.sourcegitcommit: 3bbc9e9a22d962a6ee20ead05f728a2b706aee19
ms.openlocfilehash: b19aaa652f2c15573ded632ca1348e1a6752f080
ms.contentlocale: de-de
ms.lasthandoff: 06/10/2017


---
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Erstellen eines Webdienst-Front-Ends für Ihre Anwendung mithilfe von ASP.NET Core
Standardmäßig enthalten Azure Service Fabric-Dienste keine öffentliche Web-Schnittstelle. Um die Funktionalität Ihrer Anwendung für HTTP-Clients verfügbar zu machen, müssen Sie ein Webprojekt als Einstiegspunkt erstellen und dann darüber mit den einzelnen Diensten kommunizieren.

In diesem Tutorial knüpfen wir nahtlos an das Tutorial [Erstellen Ihrer ersten Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) an und fügen vor dem zustandsbehafteten Zählerdienst einen ASP.NET Core-Webdienst hinzu. Falls Sie das Tutorial noch nicht absolviert haben, holen Sie dies nach, bevor Sie mit diesem Tutorial fortfahren.

## <a name="set-up-your-environment-for-aspnet-core"></a>Einrichten der Umgebung für ASP.NET Core

Für dieses Tutorial benötigen Sie Visual Studio 2017. Alle Editionen sind geeignet. 

 - [Installieren Sie Visual Studio 2017](https://www.visualstudio.com/)

Um ASP.NET Core Service Fabric-Anwendungen entwickeln zu können, müssen die folgenden Workloads installiert sein:
 - **Azure-Entwicklung** (unter *Web & Cloud*)
 - **ASP.NET- und Webentwicklung** (unter *Web & Cloud*)
 - **Plattformübergreifende Entwicklung mit .NET Core**  (unter *sonstige Toolsets*)

>[!Note] 
>Die .NET Core-Tools für Visual Studio 2015 werden nicht mehr aktualisiert. Wenn Sie noch mit Visual Studio 2015 arbeiten, muss [.NET Core VS 2015 Tools Preview 2](https://www.microsoft.com/net/download/core) installiert sein.

## <a name="add-an-aspnet-core-service-to-your-application"></a>Hinzufügen eines ASP.NET Core-Diensts zu Ihrer Anwendung
ASP.NET Core ist ein einfaches, plattformübergreifendes Webentwicklungsframework, das Sie zur Erstellung von modernen Benutzeroberflächen und Web-APIs verwenden können. 

Nun fügen wir unserer vorhandenen Anwendung ein ASP.NET Web API-Projekt hinzu.

1. Klicken Sie im Projektmappen-Explorer im Anwendungsprojekt mit der rechten Maustaste auf **Dienste**, und wählen Sie **Hinzufügen > Neuer Service Fabric-Dienst** aus.
   
    ![Hinzufügen eines neuen Dienstes zu einer vorhandenen Anwendung][vs-add-new-service]
2. Wählen Sie auf der Seite **Dienst erstellen** die Option **ASP.NET Core aus**, und legen Sie einen Namen fest.
   
    ![Auswählen eines neuen ASP.NET-Webdienstes im Dialogfeld „Neuer Dienst“][vs-new-service-dialog]

3. Die nächste Seite umfasst eine Reihe von ASP.NET Core-Projektvorlagen. Beachten Sie, dass es sich hierbei um die gleichen Vorlagen wie beim Erstellen eines ASP.NET Core-Projekts außerhalb einer Service Fabric-Anwendung handelt. Sie enthalten außerdem zusätzlichen Code zum Registrieren des Diensts bei der Service Fabric-Runtime. In diesem Tutorial wählen wir **Web-API**. Sie können jedoch mit denselben Konzepten eine vollständige Webanwendung erstellen.
   
    ![Auswählen des ASP.NET-Projekttyps][vs-new-aspnet-project-dialog]
   
    Nach Erstellung Ihres Web-API-Projekts müssten Ihnen zwei Dienste in Ihrer Anwendung zur Verfügung stehen. Bei der weiteren Erstellung Ihrer Anwendung können Sie weitere Dienste auf die gleiche Weise hinzufügen. Diese können unabhängig versioniert und aktualisiert werden.

## <a name="run-the-application"></a>Ausführen der Anwendung
Um uns ein Bild von unserer Arbeit zu machen, stellen wir die neue Anwendung bereit und sehen uns das Standardverhalten der Web-API-Vorlage für ASP.NET Core an.

1. Drücken Sie in Visual Studio die Taste F5, um die App zu debuggen.
2. Nach Abschluss der Bereitstellung startet Visual Studio den Browser am Stamm des Web-API-Diensts von ASP.NET. Da die Web-API-Vorlage für ASP.NET Core kein Standardverhalten für den Stamm bereitstellt, ,müsste im Browser die Fehlermeldung 404 angezeigt werden.
3. Fügen Sie dem Speicherort im Browser `/api/values` hinzu. Dadurch wird die `Get`-Methode für den ValuesController in der Web-API-Vorlage aufgerufen. Dieser gibt die standardmäßige Antwort, die von der Vorlage bereitgestellt wird -- ein JSON-Array, das zwei Zeichenfolgen enthält:
   
    ![Von der Web-API-Vorlage für ASP.NET Core zurückgegebene Standardwerte][browser-aspnet-template-values]
   
    Am Ende dieses Tutorials wird auf dieser Seite anstatt der Standardzeichenfolgen der letzte Zählerwert aus unserem zustandsbehafteten Dienst angezeigt.

## <a name="connect-the-services"></a>Verbinden der Dienste
Service Fabric bietet absolute Flexibilität bei der Kommunikation mit Reliable Services. Innerhalb einer einzelnen Anwendung haben Sie womöglich Dienste, die über TCP zugänglich sind, andere Dienste, auf die über eine HTTP-REST-API zugegriffen werden kann und wiederum andere Dienste, auf die über WebSockets zugegriffen werden kann. Ausführliche Informationen zu den verfügbaren Optionen und deren Vor- und Nachteilen finden Sie unter [Kommunizieren mit Diensten](service-fabric-connect-and-communicate-with-services.md). In diesem Tutorial verwenden wir das [Service Fabric-Dienstremoting](service-fabric-reliable-services-communication-remoting.md) aus dem SDK.

Beim Dienstremoting-Ansatz (modelliert nach Remoteprozeduraufrufen oder RPCs) definieren Sie eine Schnittstelle, die als öffentlicher Vertrag für den Dienst fungiert. Anschließend verwenden Sie diese Schnittstelle zum Generieren einer Proxyklasse für die Interaktion mit dem Dienst.

### <a name="create-the-remoting-interface"></a>Erstellen der Remotingschnittstelle
Wir erstellen zunächst die Benutzeroberfläche, die als Vertrag zwischen dem zustandsbehafteten Dienst und anderen Diensten fungiert, in diesem Fall dem ASP.NET Core-Projekt. Diese Schnittstelle muss von allen Diensten gemeinsam genutzt werden, die die Schnittstelle für RPC-Aufrufe verwenden. Daher erstellen wir die Schnittstellen in einem eigenen Class Library-Projekt.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie dann **Fügen Sie dem Speicherort im Browser** > **Neues Projekt**.

2. Wählen Sie im linken Navigationsbereich den Eintrag **Visual C#** und dann die Vorlage **Klassenbibliothek** aus. Vergewissern Sie sich, dass die .NET-Framework-Version **4.5.2**verwendet wird.
   
    ![Erstellen eines Schnittstellenprojekts für Ihren zustandsbehafteten Dienst][vs-add-class-library-project]

3. Installieren des NuGet-Pakets **Microsoft.ServiceFabric.Services.Remoting**. Suchen Sie im NuGet-Paket-Manager nach **Microsoft.ServiceFabric.Services.Remoting**, und installieren sie es für alle Projekte der Lösung, die Dienstremoting verwenden, darunter auch:
   - Das Class Library-Projekt, das die Dienstschnittstelle enthält
   - Das Stateful Service-Projekt
   - Das ASP.NET Core-Webdienstprojekt
   
    ![Abrufen des Services NuGet-Pakets][vs-services-nuget-package]

4. Erstellen Sie in der Klassenbibliothek eine Schnittstelle mit einer einzelnen Methode, `GetCountAsync`, und erweitern Sie die Schnittstelle von `Microsoft.ServiceFabric.Services.Remoting.IService`. Um anzugeben, dass es sich um eine Dienstremoting-Schnittstelle handelt, muss sich die Dienstschnittstelle aus dieser Schnittstelle ableiten.
   
    ```c#
    using Microsoft.ServiceFabric.Services.Remoting;
    using System.Threading.Tasks;
        
    ...

    namespace MyStatefulService.Interface
    {
        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```

### <a name="implement-the-interface-in-your-stateful-service"></a>Implementieren der Schnittstelle in Ihrem zustandsbehafteten Dienst
Nach dem Definieren der Schnittstelle müssen wir sie in den zustandsbehafteten Dienst implementieren.

1. Fügen Sie Ihrem zustandsbehafteten Dienst einen Verweis auf das Klassenbibliotheksprojekt zu, das die Schnittstelle enthält.
   
    ![Hinzufügen eines Verweises auf das Klassenbibliotheksprojekt im zustandsbehafteten Dienst][vs-add-class-library-reference]
2. Suchen Sie die Klasse, die von `StatefulService` erbt (etwa `MyStatefulService`), und erweitern Sie sie, um die `ICounter`-Schnittstelle zu implementieren.
   
    ```c#
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
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

### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Bereitstellen des zustandsbehafteten Diensts mithilfe eines Dienstremoting-Listeners
Der letzte Schritt nach der Implementierung der `ICounter`-Schnittstelle ist das Öffnen des Dienstremoting-Kommunikationskanals. Für zustandsbehaftete Dienste bietet Service Fabric eine überschreibbare Methode namens `CreateServiceReplicaListeners`. Mit dieser Methode können Sie eine oder mehrere Kommunikationslistener angeben, basierend auf der Art der Kommunikation, die Sie für ihren Dienst aktivieren möchten.

> [!NOTE]
> Die entsprechende Methode zum Öffnen eines Kommunikationskanals für zustandslose Dienste lautet `CreateServiceInstanceListeners`.

In diesem Fall ersetzen wir die vorhandene `CreateServiceReplicaListeners`-Methode und stellen eine Instanz von `ServiceRemotingListener` bereit. Diese erstellt einen RPC-Endpunkt, der von Clients über `ServiceProxy` aufgerufen werden kann.  

Die Erweiterungsmethode `CreateServiceRemotingListener` für die `IService`-Schnittstelle ermöglicht es Ihnen, auf einfache Weise einen `ServiceRemotingListener` mit allen Standardeinstellungen zu erstellen. Um diese Erweiterungsmethode verwenden zu können, müssen Sie sicherstellen, dass Sie den `Microsoft.ServiceFabric.Services.Remoting.Runtime`-Namespace importiert haben. 

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


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>Verwenden der ServiceProxy-Klasse für die Interaktion mit dem Dienst
Der zustandsbehaftete Dienst ist nun bereit, Datenverkehr von anderen Diensten über RPC zu empfangen. Es fehlt also nur noch das Hinzufügen des Codes, um durch ASP.NET Web Service mit dem Dienst zu kommunizieren.

1. Fügen Sie Ihrem ASP.NET-Projekt einen Verweis auf die Klassenbibliothek mit der `ICounter` -Schnittstelle hinzu.

2. Zuvor haben Sie das NuGet-Paket **Microsoft.ServiceFabric.Services.Remoting** zum ASP.NET-Projekt hinzugefügt. Dieses Paket enthält die `ServiceProxy`-Klasse, die Sie für RPC-Aufrufe an den zustandsbehafteten Dienst verwenden. Stellen Sie sicher, dass dieses NuGet-Paket im ASP.NET Core-Webdienstprojekt installiert ist.

4. Öffnen Sie im Ordner **Controller** die `ValuesController`-Klasse. Beachten Sie, dass die `Get` -Methode derzeit nur ein hartcodiertes Zeichenfolgenarray mit „value1“ und „value2“ zurückgibt, wie wir vorhin im Browser gesehen haben. Ersetzen Sie diese Implementierung durch den folgenden Code:
   
    ```c#
    using MyStatefulService.Interface;
    using Microsoft.ServiceFabric.Services.Client;
    using Microsoft.ServiceFabric.Services.Remoting.Client;
   
    ...

    [HttpGet]
    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));
   
        long count = await counter.GetCountAsync();
   
        return new string[] { count.ToString() };
    }
    ```
   
    Die erste Codezeile ist der Schlüssel. Zum Erstellen des ICounter-Proxys für den zustandsbehafteten Dienst müssen Sie zwei Informationen angeben: die Partitions-ID und den Namen des Dienstes.
   
    Sie können durch die Partitionierung zustandsbehaftete Dienste skalieren, indem Sie ihren Status in verschiedene Buckets aufteilen, die auf einem von Ihnen definierten Schlüssel basieren, z. B. „Kunden-ID“ oder „PLZ“. In unserer einfachen Anwendung verfügt der zustandsbehaftete Dienst nur über eine Partition, sodass der Schlüssel keine Rolle spielt. Jeder Schlüssel, den Sie bereitstellen, wird auf dieselbe Partition führen. Weitere Informationen zum Partitionieren von Diensten finden Sie unter [Partitionieren von Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).
   
    Der Dienstname ist eine URI in folgender Form: „fabric:/&lt;Anwendungsname&gt;/&lt;Dienstname&gt;“.
   
    Mit diesen zwei Angaben kann Service Fabric den Computer eindeutig identifizieren, an den Anforderungen gesendet werden sollen. Die `ServiceProxy` -Klasse greift auch dann nahtlos ein, wenn der Computer ausfällt, der die Partition des zustandsbehafteten Diensts hostet, und ein anderer Computer dessen Stelle einnehmen muss. Diese Abstraktion vereinfacht das Schreiben des Clientcodes für den Umgang mit anderen Diensten beträchtlich.
   
    Sobald wir den Proxy haben, rufen wir einfach die `GetCountAsync` -Methode auf und geben das Ergebnis zurück.

5. Drücken Sie erneut F5, um die geänderte Anwendung auszuführen. Wie zuvor startet Visual Studio automatisch den Browser zum Stamm des Webprojekts. Fügen Sie den „api/Values“-Pfad hinzu und der aktuelle Zählerwert sollte zurückgegeben werden.
   
    ![Anzeige des zustandsbehafteten Zählerwerts im Browser][browser-aspnet-counter-value]
   
    Aktualisieren Sie den Browser regelmäßig, um den aktuellen Zählerwert anzuzeigen.

## <a name="kestrel-and-weblistener"></a>Kestrel und WebListener

Der standardmäßige, als Kestrel bekannte ASP.NET Core-Webserver wird [derzeit für die Verarbeitung direkten Internetverkehrs nicht unterstützt](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel). Daher verwendet die zustandslose Dienstvorlage von ASP.NET Core standardmäßig [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) für Service Fabric. 

Weitere Informationen zu „Kestrel“ und „WebListener“ in Service Fabric-Diensten finden Sie unter [ASP.NET Core in zuverlässigen Service Fabric-Diensten](service-fabric-reliable-services-communication-aspnetcore.md).

## <a name="connecting-to-a-reliable-actor-service"></a>Herstellen einer Verbindung mit einem Reliable Actor-Dienst
Dieses Tutorial konzentriert sich auf das Hinzufügen des Web-Front-Ends, das mit einem zustandsbehafteten Dienst kommuniziert. Allerdings können Sie ein sehr ähnliches Modell befolgen, um mit Akteuren zu sprechen. Wenn Sie ein Reliable Actor-Projekt erstellen, generiert Visual Studio automatisch ein Schnittstellenprojekt für Sie. Diese Schnittstelle können Sie verwenden, um einen Akteur-Proxy im Webprojekt für die Kommunikation mit den Akteur zu generieren. Der Kommunikationskanal wird automatisch bereitgestellt. Es ist also nicht erforderlich, Vorgänge wie die `ServiceRemotingListener` -Einrichtung wie beim zustandsbehafteten Dienst in diesem Tutorial durchzuführen.

## <a name="how-web-services-work-on-your-local-cluster"></a>Funktionsweise von Webdiensten auf Ihrem lokalen Cluster
Im Allgemeinen können Sie genau die gleiche Service Fabric-Anwendung in einem Cluster mit mehreren Computern bereitstellen, die Sie auf dem lokalen Cluster bereitgestellt haben, und sicher sein, dass es wie erwartet funktioniert. Dies ist der Fall, da der lokale Cluster einfach eine Konfiguration mit fünf Knoten ist, die auf einen einzelnen Computer reduziert ist.

Bei Webdiensten gibt es jedoch eine wichtige Nuance. Wenn sich Ihr Cluster wie in Azure hinter einem Load Balancer befindet, müssen Sie sicherstellen, dass Ihre Webdienste auf jedem Computer bereitgestellt werden, da der Load Balancer den Datenverkehr einfach auf die Computer verteilt. Legen Sie hierzu für den `InstanceCount` des Diensts den Sonderwert „-1“ fest.

Im Gegensatz dazu müssen Sie, wenn Sie einen Webdienst lokal ausführen, sicherstellen, dass nur eine Instanz des Diensts ausgeführt wird. Andernfalls entstehen Konflikten aus mehreren Prozessen, die den Pfad und den Port überwachen. Daher sollte bei lokalen Bereitstellungen der Wert für die Webdienstinstanzen „1“ lauten.

Informationen zum Konfigurieren verschiedener Werte für andere Umgebungen finden Sie unter [Verwalten von Anwendungsparametern für mehrere Umgebungen](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie ein Web-Front-End für Ihre Anwendung mit ASP.NET Core eingerichtet haben, erhalten Sie weitere Informationen über [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md). Dadurch können Sie ein tieferes Verständnis dafür entwickeln, wie ASP.NET Core mit Service Fabric zusammenarbeitet.

Als Nächstes [erfahren Sie mehr über die Kommunikation mit Diensten](service-fabric-connect-and-communicate-with-services.md) im Allgemeinen, um ein vollständiges Bild zu erhalten, wie die Dienstkommunikation in Service Fabric funktioniert.

Nachdem Sie ein gutes Verständnis der Funktionsweise der Dienstkommunikation haben, [erstellen Sie einen Cluster in Azure, und stellen Sie Ihre Anwendung in der Cloud bereit](service-fabric-cluster-creation-via-portal.md).

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows

