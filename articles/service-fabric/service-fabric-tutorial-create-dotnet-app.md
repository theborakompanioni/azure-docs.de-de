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
ms.date: 07/12/2017
ms.author: ryanwi, mikhegn
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: 8f824e451abd2ca87875c416184e4d7265e8c72b
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---

# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a>Erstellen und Bereitstellen einer Anwendung mit einem ASP.NET Core-Web-API-Front-End-Dienst und einem zustandsbehafteten Back-End-Dienst
In diesem Tutorial wird gezeigt, wie Sie eine Azure Service Fabric-Anwendung mit einem ASP.NET Core-Web-API-Front-End und einem zustandsbehafteten Back-End-Dienst zum Speichern Ihrer Daten erstellen.

![Anwendungsdiagramm](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines ASP.NET Core-Web-API-Diensts als zuverlässigen Dienst
> * Erstellen eines zustandsbehafteten zuverlässigen Diensts
> * Implementieren von Dienstremoting und Verwenden eines Dienstproxys

Das Tutorial ist in drei Artikel aufgeteilt, von denen dieser Artikel der erste in der Reihe ist.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:
- Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Installieren Sie Visual Studio 2017](https://www.visualstudio.com/) und die Workloads **Azure-Entwicklung** und **ASP.NET und Webentwicklung**.
- [Installieren Sie das Service Fabric SDK.](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a>Erstellen eines ASP.NET-Web-API-Diensts als zuverlässigen Dienst
ASP.NET Core ist ein einfaches, plattformübergreifendes Webentwicklungsframework, das Sie zur Erstellung von modernen Benutzeroberflächen und Web-APIs verwenden können. Damit Sie die Integration von ASP.NET Core mit Service Fabric vollständig verstehen, wird dringend empfohlen, den Artikel [ASP.NET Core in Service Fabric-Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) zu lesen. Für den Moment ist es aber ausreichend, dieses Tutorial für einen schnellen Einstieg zu befolgen. Weitere Informationen zu ASP.NET Core finden Sie in der [ASP.NET Core-Dokumentation](https://docs.microsoft.com/aspnet/core/).

> [!NOTE]
> Dieses Tutorial basiert auf den [ASP.NET Core-Tools für Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc). Die .NET Core-Tools für Visual Studio 2015 werden nicht mehr aktualisiert.

1. Starten Sie Visual Studio als **Administrator**.

2. Erstellen Sie mit **Datei**->**Neu**->**Projekt** ein Projekt.

3. Wählen Sie im Dialogfeld **Neues Projekt** die Option **Cloud > Service Fabric-Anwendung** aus.

4. Geben Sie der Anwendung den Namen **MyApplication**, und klicken Sie auf **OK**.

   ![Dialogfeld „Neues Projekt“ in Visual Studio](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. Wählen Sie auf der Seite **Neuer Service Fabric-Dienst** die Option **Zustandsloses ASP.NET Core** aus, und benennen Sie den Dienst mit **MyWebAPIFrontEnd**.
   
   ![Auswählen eines neuen ASP.NET-Webdienstes im Dialogfeld „Neuer Dienst“](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. Die nächste Seite umfasst eine Reihe von ASP.NET Core-Projektvorlagen. In diesem Tutorial wählen wir **Web-API**. Sie können jedoch mit denselben Konzepten eine vollständige Webanwendung erstellen.
   
   ![Auswählen des ASP.NET-Projekttyps](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   Visual Studio erstellt ein Anwendungs- und ein Dienstprojekt und zeigt sie im Projektmappen-Explorer an.

   ![Projektmappen-Explorer nach der Erstellung der Anwendung mit einem ASP.NET Core-Web-API-Dienst]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="deploy-and-debug-the-application-locally"></a>Lokales Bereitstellen und Debuggen der Anwendung
Sie können jetzt fortfahren und die Anwendung debuggen. Sehen Sie sich auch das Standardverhalten an, das die ASP.NET Core-Web-API-Vorlage bereitstellt.

Drücken Sie in Visual Studio `F5`, um die Anwendung für das Debuggen bereitzustellen. `F5` führt zu einem Fehler, wenn Sie nicht zuvor Visual Studio als **Administrator** geöffnet haben.

> [!NOTE]
> Wenn Sie die Anwendung zum ersten Mal lokal ausführen und bereitstellen, erstellt Visual Studio einen lokalen Cluster für das Debuggen. Dies kann etwas Zeit in Anspruch nehmen. Der Status der Clustererstellung wird im Ausgabefenster von Visual Studio angezeigt.

Wenn der Cluster bereit ist, erhalten Sie auf der Taskleiste eine Benachrichtigung vom lokalen Service Fabric-Cluster-Manager.

1. Drücken Sie in Visual Studio die Taste F5, um die Anwendung zu debuggen.
2. Nach Abschluss der Bereitstellung startet Visual Studio den Browser am Stamm des ASP.NET-Web-API-Diensts. Da die Web-API-Vorlage für ASP.NET Core kein Standardverhalten für den Stamm bereitstellt, wird im Browser eine Fehlermeldung angezeigt.
3. Fügen Sie im Browser `/api/values` an die URL an. Durch diese Anforderung wird die `Get`-Methode für den ValuesController in der Web-API-Vorlage aufgerufen. Dieser gibt die standardmäßige Antwort, die von der Vorlage bereitgestellt wird zurück – ein JSON-Array, das zwei Zeichenfolgen enthält:
   
![Von der Web-API-Vorlage für ASP.NET Core zurückgegebene Standardwerte](./media/service-fabric-tutorial-create-dotnet-app/browser-aspnet-template-values.png)

> [!NOTE]
> Um das Standardverhalten für Visual Studio 2017 beim Debuggen einer Anwendung zu ändern, können Sie die Eigenschaften des Service Fabric-Anwendungsprojekts **MyApplication** ändern.
> Legen Sie für dieses Tutorial den Debugmodus für die Anwendung auf **Anwendung aktualisieren** fest, und fügen Sie **„/api/values“** an die URL-Eigenschaft der Anwendung an, um das Debugverhalten zu optimieren.
> 

Zum Beenden des Debuggens der Anwendung wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT+F5**.

### <a name="understanding-the-service-fabric-application-and-service"></a>Grundlegendes zu Anwendungen und Diensten in Service Fabric
Die Visual Studio-Projektmappe enthält jetzt zwei Projekte.
1. Das Service Fabric-Anwendungsprojekt **MyApplication**
    - Dieses Projekt enthält nicht direkt Code. Stattdessen verweist es eine Reihe von Dienstprojekten. Darüber hinaus enthält es anderen Inhaltstypen, die angeben, wie die Anwendung erstellt und bereitgestellt wird.
2. Das Dienstprojekt **MyWebAPIFrontEnd**
    - Dieses Projekt ist das ASP.NET Core-Web-API-Projekt mit dem Code und der Konfiguration für Ihren Dienst. Wenn Sie sich die Codedatei „ValuesController.cs“ im Ordner „Controllers“ ansehen, können Sie erkennen, dass es sich um einen regulären ASP.NET Core-Web-API-Controller handelt. Es gibt keine speziellen Anforderungen an den Code, den Sie für Controller schreiben, wenn Sie eine ASP.NET Core-Web-API als zuverlässigen Dienst in Service Fabric ausführen.

Weitere Informationen zum Anwendungsmodell in Service Fabric finden Sie unter [Modellieren von Anwendungen in Service Fabric](service-fabric-application-model.md).

Weitere Informationen zum Inhalt des Dienstprojekts finden Sie unter [Erste Schritte mit Reliable Services](service-fabric-reliable-services-quick-start.md).

## <a name="add-a-stateful-back-end-service-to-your-application"></a>Hinzufügen eines zustandsbehafteten Back-End-Diensts zur Anwendung
Nachdem nun ein ASP.NET-Web-API-Dienst in der Anwendung ausgeführt wird, fügen Sie als Nächstes einen zustandsbehafteten zuverlässigen Dienst hinzu, um einige Daten in der Anwendung zu speichern.

Service Fabric ermöglicht eine konsistente und zuverlässige Speicherung von Daten direkt innerhalb des Diensts anhand von Reliable Collections. Reliable Collections sind ein einfacher Satz hochverfügbarer und zuverlässiger Sammungsklassen, die jedem vertraut sind, der bereits mit C#-Auflistungen gearbeitet hat.

In diesem Tutorial erstellen Sie einen Dienst, der einen Zählerwert in einer Reliable Collection gespeichert.

1. Klicken Sie im Projektmappen-Explorer im Anwendungsprojekt mit der rechten Maustaste auf **Dienste**, und wählen Sie **Hinzufügen > Neuer Service Fabric-Dienst** aus.
   
    ![Hinzufügen eines neuen Dienstes zu einer vorhandenen Anwendung](./media/service-fabric-tutorial-create-dotnet-app/vs-add-new-service.png)

2. Wählen Sie im Dialogfeld **Neuer Service Fabric-Dienst** die Option **Zustandsbehafteter Dienst** aus, benennen Sie den Dienst mit **MyStatefulService**, und wählen Sie **OK** aus.

    ![Dialogfeld „Neuer Dienst“ in Visual Studio](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    Nach Erstellung Ihres Dienstprojekts stehen Ihnen zwei Dienste in Ihrer Anwendung zur Verfügung. Bei der weiteren Erstellung Ihrer Anwendung können Sie weitere Dienste auf die gleiche Weise hinzufügen. Diese können unabhängig versioniert und aktualisiert werden.

### <a name="deploy-and-debug-the-application-locally"></a>Lokales Bereitstellen und Debuggen der Anwendung
Nachdem der neue Dienst der Anwendung hinzugefügt wurde, können Sie die vollständige Anwendung debuggen und sich das Standardverhalten des neuen Diensts ansehen.

Drücken Sie in Visual Studio die Taste F5, um die Anwendung zu debuggen.

> [!NOTE]
> Wenn Sie für die Anwendung als Debugmodus **Anwendung aktualisieren** verwenden, werden Sie aufgefordert, dem lokalen Service Fabric-Cluster Zugriff auf den Buildausgabeordner der Anwendung zu gewähren.
> 

Beide Dienste in Ihrer Anwendung werden in Ihrem lokalen Service Fabric-Cluster erstellt, bereitgestellt und angegeben. Sobald die Dienste gestartet wurden, startet Visual Studio Ihren Browser, aber auch automatisch die **Diagnoseereignisanzeige**, in der die Ausgabe der Ablaufverfolgung Ihrer Dienste angezeigt wird.
   
![Diagnoseereignisanzeige](./media/service-fabric-tutorial-create-dotnet-app/diagnostic-events-viewer.png)

Die Diagnoseereignisanzeige zeigt Meldungen der Ablaufverfolgung von allen Diensten, die Teil der debuggten Visual Studio-Projektmappe sind. Durch das Anhalten der Diagnoseereignisanzeige können Sie eine der Dienstmeldungen erweitern, um ihre Eigenschaften zu überprüfen. Auf diese Weise können Sie u.a. sehen, welcher Dienst im Cluster die Meldung ausgegeben hat und auf welchem Knoten die Dienstinstanz zurzeit ausgeführt wird.

![Diagnoseereignisanzeige](./media/service-fabric-tutorial-create-dotnet-app/expanded-diagnostics-viewer.png)

Sie sehen, dass die Dienstmeldungen von dem zustandsbehafteten Dienst stammen, den Sie erstellt haben, da der **serviceTypeName** den Wert **MyStatefulServiceType** aufweist. Sie können auch erkennen, dass er Meldungen mit dem Text „Current counter is...“ sendet. Diese Meldung wird durch diese hervorgehobene Codezeile in der `RunAsync`-Methode von **MyStatefulService.cs** ausgegeben, die Sie im folgenden Screenshot sehen.

![Dienstmeldungscode](./media/service-fabric-tutorial-create-dotnet-app/service-message-code.png)

Weitere Informationen zum Ausgeben von Diagnoseinformationen von Ihren Diensten und Anwendungen finden Sie unter [Überwachung und Diagnose für Azure Service Fabric](service-fabric-diagnostics-overview.md).

Zum Beenden des Debuggens der Anwendung wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT+F5**.

### <a name="understanding-the-mystatefulservice-code"></a>Grundlegendes zum MyStatefulService-Code
Um zu verstehen, wie ein Reliable Dictionary zum Speichern der Daten im Cluster verwendet wird, sehen Sie sich den Code im Dienst **MyStatefulService** an.

Fünf Codezeilen im Dienst beziehen sich auf das Erstellen, Aktualisieren und Lesen aus dem Reliable Dictionary.

![Reliable Dictionary-Code](./media/service-fabric-tutorial-create-dotnet-app/reliable-dictionary-code.png)

1. Wenn die `RunAsync`-Methode des Diensts ausgeführt wird (beim Dienststart), ruft diese Codezeile das Reliable Dictionary `myDictionary` ab oder fügt es dem Dienst hinzu.
2. Alle Interaktionen mit den Werten in einem Reliable Dictionary erfordern eine Transaktion, die durch diese using-Anweisung in dieser Codezeile erstellt werden.
3. Diese Codezeile ruft den Wert ab, der dem im Methodenaufruf angegebenen Schlüssel zugeordnet ist, z.B. `Counter`.
4. Diese Codezeile aktualisiert den Wert, der dem Schlüssel `Counter` zugeordnet ist, indem sie den Wert erhöht.
5. Dieser Methodenaufruf committet die Transaktion und wird beendet, wenn der aktualisierte Wert über ein Quorum von Knoten im Cluster gespeichert ist.

Ausführlichere Informationen zu Reliable Dictionarys und Reliable Collections finden Sie unter [Einführung in Reliable Collections in zustandsbehafteten Azure Service Fabric-Diensten](service-fabric-reliable-services-reliable-collections.md).

## <a name="connect-the-services"></a>Verbinden der Dienste
Im nächsten Schritt verbinden Sie die beiden Dienste und geben an, dass die Front-End-Web-API den aktuellen Wert aus dem Reliable Dictionary der Back-End-Dienste zurückgibt.

Service Fabric bietet absolute Flexibilität bei der Kommunikation mit Reliable Services. Sie können in einer Anwendung über Dienste verfügen, auf die über TCP zugegriffen werden kann. Auf andere Dienste wird möglicherweise über eine HTTP-REST-API zugegriffen, und bei wieder anderen Diensten erfolgt der Zugriff über Websockets. Ausführliche Informationen zu den verfügbaren Optionen und deren Vor- und Nachteilen finden Sie unter [Kommunizieren mit Diensten](service-fabric-connect-and-communicate-with-services.md).

In diesem Tutorial verwenden Sie das [Dienstremoting mit Reliable Services](service-fabric-reliable-services-communication-remoting.md).

Beim Dienstremoting (modelliert nach Remoteprozeduraufrufen oder RPCs) definieren Sie eine Schnittstelle, die als öffentlicher Vertrag für den Dienst fungiert. Anschließend verwenden Sie diese Schnittstelle zum Generieren einer Proxyklasse für die Interaktion mit dem Dienst.

### <a name="create-the-remoting-interface"></a>Erstellen der Remotingschnittstelle
Erstellen Sie zunächst die Schnittstelle, die als Vertrag zwischen den beiden Diensten fungiert. Auf die Schnittstelle muss von allen Diensten verwiesen werden, die sie verwenden. Deshalb erstellen Sie sie in einem separaten Klassenbibliotheksprojekt.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie dann **Hinzufügen** > **Neues Projekt** aus.
2. Wählen Sie im linken Navigationsbereich den Eintrag **Visual C#** und dann die Vorlage **Klassenbibliothek (.NET Framework)** aus. Vergewissern Sie sich, dass die .NET Framework-Version **4.5.2** oder höher verwendet wird.
   
    ![Erstellen eines Schnittstellenprojekts für Ihren zustandsbehafteten Dienst](./media/service-fabric-tutorial-create-dotnet-app/vs-add-class-library-project.png)

3. Benennen Sie die Klassenbibliothek mit **MyStatefulService.Interface**, und klicken Sie auf **OK**.

4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **NuGet-Pakete für Projektmappe verwalten** aus.

5. Wählen Sie **Durchsuchen** aus, und suchen Sie nach **Microsoft.ServiceFabric.Services.Remoting**. Wählen Sie die Installation für die drei Dienstprojekte in der Projektmappe aus: 
   
    ![Abrufen des Services NuGet-Pakets](./media/service-fabric-tutorial-create-dotnet-app/add-nuget.png)

6. Erstellen Sie in der Klassenbibliothek eine Schnittstelle mit einer einzelnen Methode, `GetCountAsync`, und erweitern Sie die Schnittstelle von `Microsoft.ServiceFabric.Services.Remoting.IService`. Um anzugeben, dass es sich um eine Dienstremoting-Schnittstelle handelt, muss sich die Dienstschnittstelle aus dieser Schnittstelle ableiten. 
   
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
7. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **MyStatefulService.Interface**, und wählen Sie **Eigenschaften** aus. Wählen Sie die Registerkarte **Erstellen** und dann in der Dropdownliste **Zielplattform** einen **x64**-Wert aus. 

8. Speichern Sie alle Änderungen.

### <a name="implement-the-interface-in-your-stateful-service"></a>Implementieren der Schnittstelle in Ihrem zustandsbehafteten Dienst
Nach dem Definieren der Schnittstelle müssen wir sie in den zustandsbehafteten Dienst implementieren.

1. Fügen Sie einen Verweis auf das Klassenbibliotheksprojekt hinzu, das die Schnittstelle aus dem Projekt **MyStatefulService** enthält.
   
    ![Hinzufügen eines Verweises auf das Klassenbibliotheksprojekt im zustandsbehafteten Dienst](./media/service-fabric-tutorial-create-dotnet-app/vs-add-class-library-reference.png)

    ![Hinzufügen eines Verweises auf das Klassenbibliotheksprojekt im zustandsbehafteten Dienst](./media/service-fabric-tutorial-create-dotnet-app/vs-add-class-library-reference-2.png)

2. Öffnen Sie die Datei `MyStatefulService.cs`, und erweitern Sie sie, um die `ICounter`-Schnittstelle zu implementieren, die Sie erstellt haben.
   
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

    - Diese Methode gibt den gespeicherten Wert des Schlüssels `Counter` im Reliable Dictionary `myDictionary` zurück. 

### <a name="expose-the-stateful-service-using-service-remoting"></a>Verfügbarmachen des zustandsbehafteten Diensts per Dienstremoting
Der letzte Schritt nach der Implementierung der `ICounter`-Schnittstelle ist das Öffnen des Dienstremoting-Endpunkts. Für zustandsbehaftete Dienste bietet Service Fabric eine überschreibbare Methode namens `CreateServiceReplicaListeners`. Mit dieser Methode können Sie eine oder mehrere Kommunikationslistener angeben, basierend auf der Art der Kommunikation, die Sie für ihren Dienst aktivieren möchten.

In diesem Fall ersetzen wir die vorhandene `CreateServiceReplicaListeners`-Methode und stellen eine Instanz von `ServiceRemotingListener` bereit. Diese erstellt einen RPC-Endpunkt, der von Clients über `ServiceProxy` aufgerufen werden kann.  

Ändern Sie die **CreateServiceReplicaListeners**-Methode in der Datei **MyStatefulService.cs**, und fügen Sie dem `Microsoft.ServiceFabric.Services.Remoting.Runtime`-Namespace eine using-Anweisung hinzu.

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

Der zustandsbehaftete Dienst ist nun bereit, per Dienstremoting Datenverkehr von anderen Diensten über RPC zu empfangen.

### <a name="call-the-stateful-back-end-service-from-the-front-end-service"></a>Aufrufen des zustandsbehafteten Back-End-Diensts vom Front-End-Dienst
Nachdem der Back-End-Dienst eine Schnittstelle verfügbar gemacht hat, müssen Sie nur noch Code für die Kommunikation mit dieser vom ASP.NET-Web-API-Dienst hinzufügen. Für die Kommunikation per Dienstremoting verwenden Sie einen Dienstproxy aus dem Values-Controller.

1. Erweitern Sie im Projektmappen-Explorer **MyWebAPIFrontEnd**, klicken Sie mit der rechten Maustaste auf **Abhängigkeiten**, und wählen Sie **Verweis hinzufügen** aus.  Wählen Sie **MyStatefulService.Interface** aus, und klicken Sie auf **OK**.
   
2. Öffnen Sie im Ordner **Controllers** die Datei `ValuesController.cs`. Fügen Sie der Datei die folgenden using-Anweisungen hinzu:

    ```c#
    using MyStatefulService.Interface;
    using Microsoft.ServiceFabric.Services.Client;
    using Microsoft.ServiceFabric.Services.Remoting.Client;
    ```
    
3. Die `Get`-Methode gibt derzeit nur ein hartcodiertes Zeichenfolgenarray mit „value1“ und „value2“ zurück, wie Sie vorhin im Browser gesehen haben. Ersetzen Sie diese Implementierung durch den folgenden Code:
   
    ```
    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));
   
        long count = await counter.GetCountAsync();
   
        return new string[] { count.ToString() };
    }
    ```
 
    Die erste Codezeile in dieser Methode erstellt das ServiceProxy-Objekt für den zustandsbehafteten Dienst über die ICounter-Schnittstelle. Beim Erstellen eines ServiceProxy-Objekts müssen Sie zwei Informationen angeben: die Partitions-ID und den Namen des Diensts.
   
    Zustandsbehaftete Dienste können partitioniert werden, um die Daten in Segmenten zu skalieren. Diese Aufteilung erfolgt basierend auf einem Schlüssel wie der Kunden-ID oder der Postleitzahl. In unserer einfachen Anwendung verfügt der zustandsbehaftete Dienst nur über eine Partition, sodass der Schlüssel keine Rolle spielt. Jeder Schlüssel, den Sie bereitstellen, wird auf dieselbe Partition führen. Weitere Informationen zum Partitionieren von Diensten finden Sie unter [Partitionieren von Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).
   
    Der Dienstname ist eine URI in folgender Form: „fabric:/&lt;Anwendungsname&gt;/&lt;Dienstname&gt;“.
   
    Mit diesen zwei Angaben kann Service Fabric den Computer eindeutig identifizieren, an den Anforderungen gesendet werden sollen. Die `ServiceProxy` -Klasse greift auch dann nahtlos ein, wenn der Computer ausfällt, der die Partition des zustandsbehafteten Diensts hostet, und ein anderer Computer dessen Stelle einnehmen muss. Diese Abstraktion vereinfacht das Schreiben des Clientcodes für den Umgang mit anderen Diensten.
   
    Sobald Sie den Proxy haben, rufen Sie einfach die `GetCountAsync`-Methode auf und geben das Ergebnis zurück.

4. Drücken Sie erneut F5, um die geänderte Anwendung auszuführen. Wie zuvor startet Visual Studio automatisch den Browser zum Stamm des Webprojekts. Fügen Sie den „api/Values“-Pfad hinzu und der aktuelle Zählerwert sollte zurückgegeben werden.
   
    ![Anzeige des zustandsbehafteten Zählerwerts im Browser](./media/service-fabric-tutorial-create-dotnet-app/browser-aspnet-counter-value.png)
   
    Aktualisieren Sie den Browser regelmäßig, um den aktuellen Zählerwert anzuzeigen.

Zum Beenden des Debuggens der Anwendung wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT+F5**.

## <a name="next-steps"></a>Nächste Schritte
In diesem Teil des Tutorials haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines ASP.NET Core-Web-API-Diensts als zuverlässigen Dienst
> * Erstellen eines zustandsbehafteten zuverlässigen Diensts
> * Implementieren von Dienstremoting und Verwenden eines Dienstproxys

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Bereitstellen der Anwendung für Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md)
