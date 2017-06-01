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
ms.date: 04/28/2017
ms.author: vturecek
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c4d5e161c9f7af33609be53e7b82f156bb0e33f
ms.openlocfilehash: 182c3d02883ceae83c9ba12c0f27085d133ac47a
ms.contentlocale: de-de
ms.lasthandoff: 05/04/2017


---
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Erstellen eines Webdienst-Front-Ends für Ihre Anwendung mithilfe von ASP.NET Core
Standardmäßig enthalten Azure Service Fabric-Dienste keine öffentliche Web-Schnittstelle. Um die Funktionalität Ihrer Anwendung für HTTP-Clients verfügbar zu machen, müssen Sie ein Webprojekt als Einstiegspunkt erstellen und dann darüber mit den einzelnen Diensten kommunizieren.

In diesem Tutorial knüpfen wir nahtlos an das Tutorial [Erstellen Ihrer ersten Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) an und fügen vor dem zustandsbehafteten Zählerdienst einen Webdienst hinzu. Falls Sie das Tutorial noch nicht absolviert haben, holen Sie dies nach, bevor Sie mit diesem Tutorial fortfahren.

## <a name="add-an-aspnet-core-service-to-your-application"></a>Hinzufügen eines ASP.NET Core-Diensts zu Ihrer Anwendung
ASP.NET Core ist ein einfaches, plattformübergreifendes Webentwicklungsframework, das Sie zur Erstellung von modernen Benutzeroberflächen und Web-APIs verwenden können. Damit Sie die Integration von ASP.NET Core mit Service Fabric vollständig verstehen, wird dringend empfohlen, den Artikel [ASP.NET Core in zuverlässigen Service Fabric-Diensten](service-fabric-reliable-services-communication-aspnetcore.md) zu lesen, aber für den Moment ist es ausreichend, diese Anleitung für einen schnellen Einstieg zu befolgen.

Nun fügen wir unserer vorhandenen Anwendung ein ASP.NET Web API-Projekt hinzu.


> [!NOTE]
> Dieses Tutorial basiert auf den [ASP.NET Core-Tools für Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc). Die .NET Core-Tools für Visual Studio 2015 werden nicht mehr aktualisiert.


1. Klicken Sie im Projektmappen-Explorer im Anwendungsprojekt mit der rechten Maustaste auf **Dienste**, und wählen Sie **Hinzufügen > Neuer Service Fabric-Dienst** aus.
   
    ![Hinzufügen eines neuen Dienstes zu einer vorhandenen Anwendung][vs-add-new-service]
2. Wählen Sie auf der Seite **Dienst erstellen** die Option **ASP.NET Core aus**, und legen Sie einen Namen fest.
   
    ![Auswählen eines neuen ASP.NET-Webdienstes im Dialogfeld „Neuer Dienst“][vs-new-service-dialog]

3. Die nächste Seite umfasst eine Reihe von ASP.NET Core-Projektvorlagen. Beachten Sie, dass es sich hierbei um die gleichen Vorlagen wie beim Erstellen eines ASP.NET Core-Projekts außerhalb einer Service Fabric-Anwendung handelt. Sie enthalten außerdem zusätzlichen Code zum Registrieren des Diensts bei der Service Fabric-Runtime. In diesem Tutorial wählen wir **Web-API**. Sie können jedoch mit denselben Konzepten eine vollständige Webanwendung erstellen.
   
    ![Auswählen des ASP.NET-Projekttyps][vs-new-aspnet-project-dialog]
   
    Nach Erstellung Ihres Web-API-Projekts stehen Ihnen zwei Dienste in Ihrer Anwendung zur Verfügung. Bei der weiteren Erstellung Ihrer Anwendung fügen Sie weitere Dienste auf die gleiche Weise hinzu. Diese können unabhängig versioniert und aktualisiert werden.

> [!TIP]
> Weitere Informationen zu ASP.NET Core finden Sie in der [ASP.NET Core-Dokumentation](https://docs.microsoft.com/aspnet/core/).
> 

## <a name="run-the-application"></a>Ausführen der Anwendung
Um uns ein Bild von unserer Arbeit zu machen, stellen wir die neue Anwendung bereit und sehen uns das Standardverhalten der Web-API-Vorlage für ASP.NET Core an.

1. Drücken Sie in Visual Studio die Taste F5, um die App zu debuggen.
2. Nach Abschluss der Bereitstellung startet Visual Studio den Browser am Stamm des ASP.NET-Web-API-Diensts, der standardmäßig auf Port 8966 lauscht. Da die Web-API-Vorlage für ASP.NET Core kein Standardverhalten für den Stamm bereitstellt, wird im Browser eine Fehlermeldung angezeigt.
3. Fügen Sie dem Speicherort im Browser `/api/values` hinzu. Dadurch wird die `Get` -Methode für den ValuesController in der Web-API-Vorlage aufgerufen. Dieser gibt die standardmäßige Antwort, die von der Vorlage bereitgestellt wird – ein JSON-Array, das zwei Zeichenfolgen enthält:
   
    ![Von der Web-API-Vorlage für ASP.NET Core zurückgegebene Standardwerte][browser-aspnet-template-values]
   
    Am Ende dieses Tutorials werden wir diese Standardwerte mit dem letzten Zählerwert aus unserem zustandsbehafteten Dienst ersetzen.

## <a name="connect-the-services"></a>Verbinden der Dienste
Service Fabric bietet absolute Flexibilität bei der Kommunikation mit Reliable Services. Innerhalb einer einzelnen Anwendung haben Sie womöglich Dienste, die über TCP zugänglich sind, andere Dienste, auf die über eine HTTP-REST-API zugegriffen werden kann und wiederum andere Dienste, auf die über WebSockets zugegriffen werden kann. Ausführliche Informationen zu den verfügbaren Optionen und deren Vor- und Nachteilen finden Sie unter [Kommunizieren mit Diensten](service-fabric-connect-and-communicate-with-services.md). In diesem Tutorial greifen wir auf einen einfacheren Ansatz zurück und verwenden die im SDK bereitgestellten `ServiceProxy`/`ServiceRemotingListener` -Klassen.

Beim `ServiceProxy` -Ansatz (modelliert nach Remoteprozeduraufrufen oder RPCs) definieren Sie eine Schnittstelle, die als öffentlicher Vertrag für den Dienst fungiert. Anschließend verwenden Sie diese Schnittstelle zum Generieren einer Proxyklasse für die Interaktion mit dem Dienst.

### <a name="create-the-interface"></a>Erstellen der Benutzeroberfläche
Wir erstellen zunächst die Benutzeroberfläche, die als Vertrag zwischen dem zustandsbehafteten Dienst und dessen Clients fungiert (einschließlich des ASP.NET Core-Projekts).

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie dann **Fügen Sie dem Speicherort im Browser** > **Neues Projekt**.
2. Wählen Sie im linken Navigationsbereich den Eintrag **Visual C#** und dann die Vorlage **Klassenbibliothek** aus. Vergewissern Sie sich, dass die .NET-Framework-Version **4.5.2**verwendet wird.
   
    ![Erstellen eines Schnittstellenprojekts für Ihren zustandsbehafteten Dienst][vs-add-class-library-project]

3. Damit eine Schnittstelle von `ServiceProxy`verwendet werden kann, muss sie von der IService-Schnittstelle abgeleitet werden. Diese Schnittstelle ist in einem der Service Fabric-NuGet-Pakete enthalten. Klicken Sie zum Hinzufügen des Pakets mit der rechten Maustaste auf das neue Klassenbibliotheksprojekt, und wählen Sie **NuGet-Pakete verwalten**aus.
4. Suchen Sie nach dem Paket **Microsoft.ServiceFabric.Services.Remoting**, und installieren Sie es.
   
    ![Abrufen des Services NuGet-Pakets][vs-services-nuget-package]

5. Erstellen Sie in der Klassenbibliothek eine Schnittstelle mit einer einzelnen Methode, `GetCountAsync`, und erweitern Sie die Schnittstelle von IService.
   
    ```c#
    namespace MyStatefulService.Interface
    {
        using Microsoft.ServiceFabric.Services.Remoting;
   
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

### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Bereitstellen des zustandsbehafteten Diensts mithilfe eines Dienstremoting-Listeners
Nach dem Implementieren der `ICounter` -Schnittstelle besteht der letzte Schritt bei der Aktivierung des zustandsbehafteten Diensts darin, von anderen Diensten aufgerufen zu werden, um einen Kommunikationskanal zu öffnen. Für zustandsbehaftete Dienste bietet Service Fabric eine überschreibbare Methode namens `CreateServiceReplicaListeners`. Mit dieser Methode können Sie eine oder mehrere Kommunikationslistener angeben, basierend auf der Art der Kommunikation, die Sie in ihrem Dienst aktivieren möchten.

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


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>Verwenden der ServiceProxy-Klasse für die Interaktion mit dem Dienst
Der zustandsbehaftete Dienst ist nun bereit, Datenverkehr von anderen Diensten zu empfangen. Es fehlt also nur noch das Hinzufügen des Codes, um durch ASP.NET Web Service mit dem Dienst zu kommunizieren.

1. Fügen Sie Ihrem ASP.NET-Projekt einen Verweis auf die Klassenbibliothek mit der `ICounter` -Schnittstelle hinzu.

2. Fügen Sie dem ASP.NET-Projekt das Microsoft.ServiceFabric.Services.Remoting-Paket auf dieselbe Weise hinzu wie zuvor beim Klassenbibliotheksprojekt. Dadurch wird die `ServiceProxy` -Klasse bereitgestellt.

4. Öffnen Sie im Ordner **Controller** die `ValuesController`-Klasse. Beachten Sie, dass die `Get` -Methode derzeit nur ein hartcodiertes Zeichenfolgenarray mit „value1“ und „value2“ zurückgibt, wie wir vorhin im Browser gesehen haben. Ersetzen Sie diese Implementierung durch den folgenden Code:
   
    ```c#
    using MyStatefulService.Interface;
    using Microsoft.ServiceFabric.Services.Client;
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

## <a name="connecting-to-a-reliable-actors-service"></a>Herstellen einer Verbindung mit einem Reliable Actors-Dienst
Dieses Tutorial konzentriert sich auf das Hinzufügen des Web-Front-Ends, das mit einem zustandsbehafteten Dienst kommuniziert. Allerdings können Sie ein sehr ähnliches Modell befolgen, um mit Akteuren zu sprechen. Das ist sogar einfacher.

Wenn Sie ein Akteur-Projekt erstellen, generiert Visual Studio automatisch ein Schnittstellenprojekt für Sie. Diese Schnittstelle können Sie verwenden, um einen Akteur-Proxy im Webprojekt für die Kommunikation mit den Akteur zu generieren. Der Kommunikationskanal wird automatisch bereitgestellt. Es ist also nicht erforderlich, Vorgänge wie die `ServiceRemotingListener` -Einrichtung wie beim zustandsbehafteten Dienst in diesem Tutorial durchzuführen.

## <a name="how-web-services-work-on-your-local-cluster"></a>Funktionsweise von Webdiensten auf Ihrem lokalen Cluster
Im Allgemeinen können Sie genau die gleiche Service Fabric-Anwendung in einem Cluster mit mehreren Computern bereitstellen, die Sie auf dem lokalen Cluster bereitgestellt haben, und sicher sein, dass es wie erwartet funktioniert. Dies ist der Fall, da der lokale Cluster einfach eine Konfiguration mit fünf Knoten ist, die auf einen einzelnen Computer reduziert ist.

Bei Webdiensten gibt es jedoch eine wichtige Nuance. Wenn sich Ihr Cluster wie in Azure hinter einem Load Balancer befindet, müssen Sie sicherstellen, dass Ihre Webdienste auf jedem Computer bereitgestellt werden, da der Load Balancer den Datenverkehr einfach auf die Computer verteilt. Legen Sie hierzu für den `InstanceCount` des Diensts den Sonderwert „-1“ fest.

Im Gegensatz dazu müssen Sie, wenn Sie einen Webdienst lokal ausführen, sicherstellen, dass nur eine Instanz des Diensts ausgeführt wird. Andernfalls entstehen Konflikten aus mehreren Prozessen, die den Pfad und den Port überwachen. Daher sollte bei lokalen Bereitstellungen der Wert für die Webdienstinstanzen „1“ lauten.

Informationen zum Konfigurieren verschiedener Werte für andere Umgebungen finden Sie unter [Verwalten von Anwendungsparametern für mehrere Umgebungen](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie ein Web-Front-End für Ihre Anwendung mit ASP.NET Core eingerichtet haben, erhalten Sie weitere Informationen, wie ASP.NET Core mit Service Fabric integriert wird, in diesem Artikel zu [ASP.NET Core in zuverlässigen Service Fabric-Diensten](service-fabric-reliable-services-communication-aspnetcore.md).

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

