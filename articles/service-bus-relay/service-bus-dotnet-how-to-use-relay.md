<properties
	pageTitle="Verwenden von Service Bus Relay mit .NET | Microsoft Azure"
	description="Erfahren Sie mehr über die Verwendung des Azure Service Bus Relay-Diensts zum Herstellen einer Verbindung zwischen zwei Anwendungen, die an unterschiedlichen Standorten gehostet sind."
	services="service-bus-relay"
	documentationCenter=".net"
	authors="sethmanheim"
	manager="timlt"
	editor=""/>

<tags
	ms.service="service-bus-relay"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="09/16/2016"
	ms.author="sethm"/>


# Verwenden des Azure Service Bus Relay-Diensts

In diesem Artikel wird die Verwendung des Service Bus Relay-Diensts beschrieben. Die Beispiele sind in C# geschrieben und nutzen die Windows Communication Foundation (WCF)-API mit den Erweiterungen, die in der Service Bus-Assembly enthalten sind. Weitere Informationen zu Service Bus Relay finden Sie in der Übersicht [Service Bus-Relaymessaging](service-bus-relay-overview.md).

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## Was ist Service Bus Relay?

Der [Service Bus *Relay*-Dienst](service-bus-relay-overview.md) ermöglicht Ihnen die Erstellung von Hybridanwendungen, die sowohl in einem Azure-Datencenter als auch in Ihrer eigenen lokalen Unternehmensumgebung ausgeführt werden. Der Service Bus Relay-Dienst erleichtert dies, indem er Ihnen die Möglichkeit bietet, WCF-Dienste (Windows Communication Foundation), die sich in einem Unternehmensnetzwerk befinden, sicher in der öffentlichen Cloud bereitzustellen, ohne dass eine Firewallverbindung eröffnet werden muss oder intrusive Änderungen an der unternehmensinternen Netzwerkinfrastruktur erforderlich werden.

![Relaykonzepte](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Der Service Bus Relay-Dienst bietet Ihnen die Möglichkeit, WCF-Dienste in Ihrer bestehenden Unternehmensumgebung zu hosten. Sie können die Überwachung auf eingehende Sitzungen und Anforderungen für diese WCF-Dienste dann an den Service Bus-Dienst unter Azure delegieren. Auf diese Weise erhalten Sie die Möglichkeit, diese Dienste für Anwendungscode unter Azure, für mobile Mitarbeiter oder für Extranetpartnerumgebungen bereitzustellen. Mit Service Bus können Sie auf sichere Weise steuern, welche Personen auf welcher Optimierungsstufe Zugriff auf diese Dienste erhalten. Der Dienst stellt ein leistungsstarkes und sicheres Verfahren zur Verfügung, um Anwendungsfunktionen und Daten aus bestehenden Unternehmenslösungen bereitzustellen und in der Cloud davon zu profitieren.

In diesem Artikel wird gezeigt, wie mit dem Service Bus Relay-Dienst ein mithilfe der TCP-Channelbindung bereitgestellter WCF-Webdienst erstellt wird, der eine geschützte Unterhaltung zwischen zwei Parteien implementiert.

## Erstellen eines Dienstnamespaces

Wenn Sie mit der Verwendung des Service Bus Relays in Azure beginnen möchten, müssen Sie zuerst einen Namespace erstellen. Ein Namespace ist ein Bereichscontainer für die Adressierung von Service Bus-Ressourcen innerhalb Ihrer Anwendung.

So erstellen Sie einen Dienstnamespace:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## Abrufen des NuGet-Pakets "Service Bus"

Das [Service Bus-NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus) stellt die einfachste Möglichkeit zum Abrufen der Service Bus-API und zum Konfigurieren der Anwendung mit allen Service Bus-Abhängigkeiten dar. Gehen Sie wie folgt vor, um das NuGet-Paket in Ihrem Projekt zu installieren:

1.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Verweise**, und klicken Sie dann auf **NuGet-Pakete verwalten**.
2.  Suchen Sie nach „Service Bus“, und wählen Sie das Element **Microsoft Azure Service Bus** aus. Klicken Sie auf **Installieren**, um die Installation durchzuführen. Schließen Sie danach das folgende Dialogfeld:

	![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## Verwenden von Service Bus zum Bereitstellen und Nutzen eines SOAP-Webdiensts mit TCP

Um einen bestehenden WCF SOAP-Webdienst für die externe Nutzung bereitzustellen, müssen Sie Änderungen an den Dienstbindungen und Adressen vornehmen. Dies kann Änderungen an der Konfigurationsdatei oder Codeänderungen erforderlich machen, je nachdem, wie Ihre WCF-Dienste eingerichtet und konfiguriert sind. Beachten Sie, dass WCF Ihnen die Möglichkeit bietet, mehrere Netzwerkendpunkte über denselben Dienst zu verwalten, sodass Sie die bestehenden internen Endpunkte beibehalten können, während Sie gleichzeitig Service Bus-Endpunkte für den externen Zugriff hinzufügen.

Bei dieser Aufgabe erstellen Sie einen einfachen WCF-Dienst und fügen ihm einen Service Bus-Listener hinzu. Für diese Übung wird eine gewisse Vertrautheit mit Visual Studio vorausgesetzt. Es werden daher nicht alle Details der Projekterstellung erläutert. Stattdessen steht der Code im Vordergrund.

Richten Sie vor dem Ausführen dieser Schritte zunächst mit dem folgenden Verfahren Ihre Umgebung ein:

1.  Erstellen Sie in Visual Studio eine Konsolenanwendung, die die beiden Projekte "Client" und "Service" innerhalb der Lösung enthält.
2.  Fügen Sie das Paket Microsoft Azure Service Bus NuGet beiden Projekte hinzu. Mit diesem Paket werden den Projekten alle benötigten Assemblyverweise hinzugefügt.

### Erstellen des Diensts

Erstellen Sie zunächst den Dienst selbst Ein WCF-Dienst besteht aus mindestens drei verschiedenen Teilen:

-   Definition eines Vertrags, der beschreibt, welche Nachrichten ausgetauscht werden und welche Operationen aufgerufen werden sollen
-   Implementierung des besagten Vertrags
-   Dem Host, der diesen WCF-Dienst hostet und mehrere Endpunkte bereitstellt

Die Codebeispiele in diesem Abschnitt beziehen sich auf jede dieser Komponenten.

Der Vertrag definiert einen einzelnen Vorgang (`AddNumbers`), der zwei Zahlen hinzufügt und das Ergebnis zurückgibt. Die Schnittstelle `IProblemSolverChannel` ermöglicht dem Client die einfachere Verwaltung der Proxylebensdauer. Das Erstellen einer solchen Schnittstelle wird als eine bewährte Methode betrachtet. Es ist sinnvoll, die Vertragsdefinition in einer separaten Datei abzulegen, sodass Sie von beiden Projekten aus ("Client" und "Service") auf diese Datei verweisen können. Sie können den Code aber auch in beide Projekte kopieren.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Nachdem der Vertrag etabliert wurde, ist die Implementierung ein Kinderspiel.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### Programmgesteuertes Konfigurieren eines Diensthosts

Nachdem der Vertrag erstellt und die Implementierung durchgeführt wurde, kann nun der Dienst gehostet werden. Das Hosting erfolgt innerhalb eines [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx)-Objekts, das die Verwaltung der Dienstinstanzen übernimmt und die Endpunkte hostet, die auf Nachrichten warten. Der folgende Code konfiguriert den Dienst sowohl mit einem regulären lokalen Endpunkt als auch mit einem Service Bus-Endpunkt, um interne und externe Endpunkte nebeneinander zu veranschaulichen. Ersetzen Sie die Zeichenfolge *namespace* durch Ihren Namespacenamen und *yourKey* durch den SAS-Schlüssel, den Sie im vorherigen Setupschritt erhalten haben.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

In dem Beispiel erstellen Sie zwei Endpunkte für dieselbe Vertragsimplementierung. Einer ist lokal und einer wird über Service Bus projiziert. Die zentralen Unterschiede sind die Bindungen. [NetTcpBinding](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) für den lokalen Endpunkt und [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) für den Service Bus-Endpunkt und die Adressen. Der lokale Endpunkt verfügt über eine lokale Netzwerkadresse mit einem bestimmten Port. Der Service Bus-Endpunkt hat eine Endpunktadresse, die sich aus der Zeichenfolge `sb`, Ihrem Namespacenamen und dem Pfad „solver“ zusammensetzt. Dies führt zum URI `sb://[serviceNamespace].servicebus.windows.net/solver`, mit dem der Dienstendpunkt als Service Bus-TCP-Endpunkt mit vollqualifiziertem externem DNS-Namen identifiziert wird. Wenn Sie Code, der die Platzhalter ersetzt, in die `Main`-Funktion der Anwendung **Service** einfügen, erhalten Sie einen funktionierenden Dienst. Wenn Ihr Dienst exklusiv für Service Bus lauschen soll, entfernen Sie die lokale Endpunktdeklaration.

### Konfigurieren eines Diensthosts in der Datei „App.config“

Sie können den Host auch mithilfe der Datei "App.config" konfigurieren. Der Diensthostingcode wird in diesem Fall im nächsten Beispiel angezeigt.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Die Endpunktdefinitionen werden in die Datei "App.config" verschoben. Das NuGet-Paket hat der Datei „App.config“ bereits eine Reihe von Definitionen hinzugefügt, die die erforderlichen Konfigurationserweiterungen für Service Bus darstellen. Das folgende Beispiel, das mit dem vorherigen Beispiel identisch ist, sollte direkt unter dem **system.serviceModel**-Element angezeigt werden. In dem Codebeispiel wird davon ausgegangen, dass der C#-Namespace des Projekts den Namen **Service** hat. Ersetzen Sie die Platzhalter durch den Namespace und den SAS-Schlüssel des Service Bus-Diensts.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Nachdem Sie diese Änderungen vorgenommen haben, startet der Dienst wie zuvor, jedoch mit zwei aktiven Endpunkten: einem lokalen Endpunkt und einem, der in der Cloud lauscht.

### Erstellen des Clients

#### Programmgesteuertes Konfigurieren eines Clients

Um den Dienst zu nutzen, können Sie einen WCF-Client mit einem [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx)-Objekt erstellen. Service Bus verwendet ein Token-basiertes Sicherheitsmodell, das mit SAS implementiert wird. Die [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx)-Klasse stellt einen Sicherheitstokenanbieter mit integrierten Factory-Methoden dar, die einige bekannte Tokenanbieter zurückgeben. Im folgenden Beispiel wird die [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx)-Methode zum Behandeln der Beschaffung des richtigen SAS-Tokens verwendet. Die Name und der Schlüssel wurden aus dem Portal abgerufen, wie im vorherigen Abschnitt beschrieben wurde.

Verweisen Sie zuerst auf den `IProblemSolver`-Vertragscode, oder kopieren Sie ihn aus dem Dienst in Ihr Clientprojekt.

Ersetzen Sie anschließend den Code in der `Main`-Methode des Clients, und ersetzen Sie dabei den Platzhaltertext erneut durch Ihren Service Bus-Namespace und SAS-Schlüssel.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Sie können jetzt den Client und den Dienst erstellen und ausführen (führen Sie zuerst den Dienst aus). Der Client ruft den Dienst auf und gibt **9** aus. Der Client und der Server können auf verschiedenen Computern ausgeführt werden (selbst über Netzwerke hinweg). Die Kommunikation funktioniert weiterhin. Der Clientcode kann ebenfalls in der Cloud oder lokal ausgeführt werden.

#### Konfigurieren eines Clients in der Datei „App.config“

Der folgende Code zeigt, wie Sie den Client auch mithilfe der Datei "App.config" konfigurieren können.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Die Endpunktdefinitionen werden in die Datei "App.config" verschoben. Das folgende Beispiel, das mit dem vorherigen Code identisch ist, sollte direkt unter dem **system.serviceModel**-Element angezeigt werden. Wie zuvor müssen Sie auch hier die Platzhalter durch den Service Bus-Namespace und SAS-Schlüssel ersetzen.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## Nächste Schritte

Nachdem Sie nun mit den Grundlagen des Service Bus Relay-Diensts vertraut sind, können Sie auf die Informationen unter den folgenden Links zugreifen und sich weiter informieren.

- [Übersicht über Service Bus-Relaymessaging](service-bus-relay-overview.md)
- [Übersicht über die Architektur von Azure Service Bus](../service-bus/service-bus-fundamentals-hybrid-solutions.md)
- Laden Sie Service Bus-Beispiele unter [Azure-Beispiele][] herunter, oder sehen Sie sich die [Übersicht über Service Bus-Beispiele][] an.

  [Shared Access Signature Authentication with Service Bus]: ../service-bus/service-bus-shared-access-signature-authentication.md
  [Azure-Beispiele]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [Übersicht über Service Bus-Beispiele]: ../service-bus/service-bus-samples.md

<!---HONumber=AcomDC_0928_2016-->