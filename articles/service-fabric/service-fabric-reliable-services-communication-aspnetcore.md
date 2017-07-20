---
title: Dienstkommunikation mit ASP.NET Core | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie ASP.NET Core in zustandslosen und zustandsbehafteten zuverlässigen Diensten verwenden."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 8aa4668d-cbb6-4225-bd2d-ab5925a868f2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 05/02/2017
ms.author: vturecek
ms.translationtype: Human Translation
ms.sourcegitcommit: 3bbc9e9a22d962a6ee20ead05f728a2b706aee19
ms.openlocfilehash: 8ac4d409f7363e8b4ae98be659a627ac8db8d787
ms.contentlocale: de-de
ms.lasthandoff: 06/10/2017

---

# <a name="aspnet-core-in-service-fabric-reliable-services"></a>ASP.NET Core in zuverlässigen Service Fabric-Diensten

ASP.NET Core ist ein neues quelloffenes und plattformübergreifendes Framework zur Erstellung moderner cloudbasierter Anwendungen mit Internetverbindung. Hierzu zählen beispielsweise Web-Apps, IoT-Apps und mobile Back-Ends. 

Dieser Artikel ist eine ausführliche Anleitung zum Hosten von ASP.NET Core-Diensten in Service Fabric Reliable Services mithilfe der **Microsoft.ServiceFabric.AspNetCore.**-*Menge der NuGet-Pakete.

Ein einführendes Tutorial auf ASP.NET Core in Service Fabric und Anweisungen zum Abrufen Ihres Entwicklungsumgebungs-Setups, finden Sie unter [Erstelle eines Web-Front-End für Ihre Anwendung mithilfe von ASP.NET Core](service-fabric-add-a-web-frontend.md).

Im weiteren Verlauf dieses Artikels wird vorausgesetzt, dass Sie bereits mit ASP.NET Core vertraut sind. Wenn nicht, empfehlen wir Ihnen [ASP.NET Core Grundlagen](https://docs.microsoft.com/aspnet/core/fundamentals/index) zu lesen.

## <a name="aspnet-core-in-the-service-fabric-environment"></a>ASP.NET Core in der Service Fabric-Umgebung

ASP.NET Core-Apps können unter .NET Core oder im gesamten .NET Framework ausgeführt werden. Im Gegensatz dazu können Service Fabric-Dienste derzeit nur im gesamten .NET Framework ausgeführt werden. Wenn Sie also einen ASP.NET Core Service Fabric-Dienst erstellen, müssen Sie ihn auf das gesamte .NET Framework ausrichten.

ASP.NET Core kann in Service Fabric auf zwei unterschiedliche Arten verwendet werden:
 - **Gehostet als ausführbare Gastanwendungsdatei:** Diese Methode wird hauptsächlich verwendet, um bereits vorhandene ASP.NET Core-Anwendungen ohne Codeänderungen unter Service Fabric auszuführen.
 - **Im Rahmen eines zuverlässigen Diensts:** Diese Methode ermöglicht eine bessere Integration in die Service Fabric-Laufzeit und die Verwendung zustandsbehafteter ASP.NET Core-Dienste.

Im weiteren Verlauf dieses Artikels erfahren Sie, wie Sie ASP.NET Core mithilfe der im Service Fabric SDK enthaltenen ASP.NET Core-Integrationskomponenten in einem zuverlässigen Dienst verwenden. 

## <a name="service-fabric-service-hosting"></a>Service Fabric-Diensthosting

In Service Fabric werden einzelne oder mehrere Instanzen und/oder Replikate Ihres Diensts in einem *Diensthostprozess* in Form einer ausführbaren Datei mit Ihrem Dienstcode ausgeführt. Als Dienstautor sind Sie für den Diensthostprozess zuständig. Dieser wird von Service Fabric für Sie aktiviert und überwacht.

Die herkömmliche ASP.NET-Version (bis MVC 5) ist über „System.Web.dll“ eng an IIS gekoppelt. Bei ASP.NET Core sind der Webserver und Ihre Webanwendung getrennt. Dadurch sind Webanwendungen zwischen verschiedenen Webservern übertragbar, und es ermöglicht die Verwendung *selbst gehosteter* Webserver, sodass Sie einen Webserver in Ihrem eigenen Prozess starten können (im Gegensatz zu einem Prozess, für den eine dedizierte Webserversoftware wie etwa IIS zuständig ist). 

Wenn Sie einen Service Fabric-Dienst und ASP.NET als ausführbare Gastanwendungsdatei oder in einem zuverlässigen Dienst kombinieren möchten, müssen Sie innerhalb Ihres Diensthostprozesses ASP.NET starten können. Dies ist dank des Selbsthostings von ASP.NET Core möglich.

## <a name="hosting-aspnet-core-in-a-reliable-service"></a>Hosten von ASP.NET Core in einem zuverlässigen Dienst
In der Regel erstellen selbst gehostete ASP.NET Core Anwendungen einen Webhost im Einstiegspunkt einer Anwendung (wie etwa die `static void Main()`-Methode in `Program.cs`). In diesem Fall ist der Lebenszyklus des Webhosts an den Lebenszyklus des Prozesses gebunden.

![Hosten von ASP.NET Core in einen Prozess][0]

Der Einstiegspunkt der Anwendung ist jedoch nicht der richtige Ort, um einen Webhost in einem zuverlässigen Dienst zu erstellen, da der Einstiegspunkt der Anwendung nur dazu dient, einen Diensttyp bei der Service Fabric-Laufzeit zu registrieren, um Instanzen des Diensttyps erstellen zu können. Der Webhost muss stattdessen direkt in einem zuverlässigen Dienst erstellt werden. Innerhalb des Diensthostprozesses können Dienstinstanzen und/oder -replikate mehrere Lebenszyklen durchlaufen. 

Eine zuverlässige Dienstinstanz wird durch Ihre von `StatelessService` oder `StatefulService` abgeleitete Dienstklasse dargestellt. Der Kommunikationsstapel für einen Dienst befindet sich in Ihrer Dienstklasse in einer `ICommunicationListener`-Implementierung. Die NuGet-Pakete vom Typ `Microsoft.ServiceFabric.Services.AspNetCore.*` enthalten Implementierungen von `ICommunicationListener`, die den ASP.NET Core-Webhost für Kestrel oder WebListener in einem zuverlässigen Dienst starten und verwalten.

![Hosten von ASP.NET Core in einem zuverlässigen Dienst][1]

## <a name="aspnet-core-icommunicationlisteners"></a>ICommunicationListener-Implementierungen für ASP.NET Core
Die `ICommunicationListener`-Implementierungen für Kestrel und WebListener in den NuGet-Paketen vom Typ `Microsoft.ServiceFabric.Services.AspNetCore.*` haben ähnliche Verwendungsmuster, führen aber für jeden Webserver geringfügig andere Aktionen aus. 

Beide Kommunikationslistener bieten einen Konstruktor, der folgende Argumente akzeptiert:
 - **`ServiceContext serviceContext`**: Das `ServiceContext`-Objekt mit Informationen zum ausgeführten Dienst.
 - **`string endpointName`**: Der Name einer `Endpoint`-Konfiguration in „ServiceManifest.xml“. Hier liegt der Hauptunterschied zwischen den beiden Kommunikationslistenern: WebListener **erfordert** eine `Endpoint`-Konfiguration, Kestrel hingegen nicht.
 - **`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: Ein von Ihnen implementiertes Lambda, in dem Sie ein `IWebHost`-Element erstellen und zurückgeben. Dadurch können Sie `IWebHost` in einer ASP.NET Core-Anwendung wie gewohnt konfigurieren. Das Lambda stellt eine URL bereit, die für Sie in Abhängigkeit von den verwendeten Service Fabric-Integrationsoptionen und der angegebenen `Endpoint`-Konfiguration generiert wird. Diese URL kann anschließend geändert oder unverändert verwendet werden, um den Webserver zu starten.

## <a name="service-fabric-integration-middleware"></a>Middleware für die Service Fabric-Integration
Das NuGet-Paket `Microsoft.ServiceFabric.Services.AspNetCore` enthält die `UseServiceFabricIntegration`-Erweiterungsmethode für `IWebHostBuilder`, der Service Fabric-fähigen Middleware hinzufügt. Diese Middleware konfiguriert die `ICommunicationListener`-Implementierung von Kestrel oder WebListener, um eine eindeutige Dienst-URL beim Service Fabric Naming Service zu registrieren, und stellt durch anschließende Überprüfung von Clientanforderungen sicher, dass Clients eine Verbindung mit dem richtigen Dienst herstellen. Das ist in einer Umgebung mit gemeinsam genutztem Host (beispielsweise Service Fabric) erforderlich, in der unter Umständen mehrere Webanwendungen auf dem gleichen physischen oder virtuellen Computer ausgeführt, aber keine eindeutigen Hostnamen verwendet werden, und verhindert, dass Clients irrtümlich eine Verbindung mit dem falschen Dienst herstellen. Dieses Szenario wird im nächsten Abschnitt ausführlicher beschrieben.

### <a name="a-case-of-mistaken-identity"></a>Ein Fall von Identitätsverwechslung
Dienstreplikate lauschen unabhängig vom Protokoll an einer eindeutigen Kombination aus IP-Adresse und Port. Nachdem ein Dienstreplikat damit begonnen hat, an einem IP:Port-Endpunkt zu lauschen, meldet es die entsprechende Endpunktadresse an den Service Fabric Naming Service, wo sie von Clients oder anderen Diensten ermittelt werden kann. Wenn Dienste dynamisch zugewiesene Anwendungsports verwenden, kann ein Dienstreplikat zufällig den gleichen IP:Port-Endpunkt eines anderen Diensts verwenden, der sich zuvor auf dem gleichen physischen oder virtuellen Computer befand. Das kann dazu führen, dass ein Client irrtümlich eine Verbindung mit dem falschen Dienst herstellt. Dieser Fall kann bei folgendem Ereignisablauf eintreten:

 1. Dienst A lauscht an 10.0.0.1:30000 über HTTP. 
 2. Der Client löst Dienst A auf und erhält die Adresse 10.0.0.1:30000.
 3. Dienst A wird auf einen anderen Knoten verschoben.
 4. Dienst B wird unter 10.0.0.1 platziert und verwendet zufällig ebenfalls den Port 30000.
 5. Der Client versucht, mit Dienst A unter Verwendung der zwischengespeicherten Adresse 10.0.0.1:30000 eine Verbindung herzustellen.
 6. Der Client ist nun mit Dienst B verbunden und weiß nicht, dass es sich dabei um den falschen Dienst handelt.

Das kann zu gelegentlich auftretenden Fehlern führen, die sich nur schwer diagnostizieren lassen. 

### <a name="using-unique-service-urls"></a>Verwenden eindeutiger Dienst-URLs
Um das zu verhindern, können Dienste für den Naming Service einen Endpunkt mit einem eindeutigen Bezeichner veröffentlichen und dann bei Clientanforderungen den eindeutigen Bezeichner überprüfen. Hierbei handelt es sich um einen kooperativen Vorgang zwischen Diensten in einer vertrauenswürdigen Umgebung ohne schädliche Mandanten. Die Methode bietet keine sichere Dienstauthentifizierung in einer Umgebung mit schädlichen Mandanten.

In einer vertrauenswürdigen Umgebung fügt die durch die `UseServiceFabricIntegration`-Methode hinzugefügte Middleware automatisch einen eindeutigen Bezeichner an die Adresse an, die für den Naming Service veröffentlicht wird, und überprüft diesen Bezeichner bei jeder Anforderung. Stimmt der Bezeichner nicht überein, gibt die Middleware sofort eine Antwort vom Typ „HTTP 410 Fehlend“ zurück.

Dienste mit dynamisch zugewiesenem Port sollten diese Middleware verwenden.

Bei Diensten mit einem festen eindeutigen Port tritt dieses Problem in einer kooperativen Umgebung nicht auf. Ein fester eindeutiger Port wird üblicherweise für extern zugängliche Dienste verwendet, die einen bekannten Port benötigen, mit dem Clientanwendungen eine Verbindung herstellen können. So verwenden beispielsweise die meisten Webanwendungen mit Internetzugriff den Port 80 oder 443 für Webbrowserverbindungen. In diesem Fall sollte der eindeutige Bezeichner nicht aktiviert werden.

Das folgende Diagramm zeigt die Abfolge der Anforderungen bei aktivierter Middleware:

![Service Fabric ASP.NET Core-integration][2]

Die `ICommunicationListener`-Implementierungen von Kestrel und WebListener verwenden diesen Mechanismus auf die gleiche Weise. Mithilfe des zugrunde liegenden *http.sys*-Portfreigabefeatures kann WebListener Anforderungen zwar intern auf der Grundlage eindeutiger URL-Pfade unterscheiden, die `ICommunicationListener`-Implementierung von WebListener verwendet diese Funktion jedoch *nicht*, da dies im zuvor beschriebenen Szenario zu Fehlerstatuscodes vom Typ „HTTP 503“ und „HTTP 404“ führt. Clients können dadurch wiederum nur sehr schwer den Zweck des Fehlers bestimmen, da „HTTP 503“ und „HTTP 404“ bereits häufig zur Angabe anderer Fehler verwendet werden. Daher wird bei `ICommunicationListener`-Implementierungen von Kestrel und WebListener die durch die `UseServiceFabricIntegration`-Erweiterungsmethode bereitgestellte Middleware als Standard verwendet, sodass Clients nur bei Antworten vom Typ „HTTP 410“ eine erneute Auflösung für einen Dienstendpunkt ausführen müssen.

## <a name="weblistener-in-reliable-services"></a>WebListener in zuverlässigen Diensten
WebListener kann in einem zuverlässigen Dienst durch Importieren des NuGet-Pakets **Microsoft.ServiceFabric.AspNetCore.WebListener** verwendet werden. Dieses Paket enthält `WebListenerCommunicationListener` (eine Implementierung von `ICommunicationListener`), mit der Sie einen ASP.NET Core-Webhost innerhalb eines zuverlässigen Diensts mit WebListener als Webserver erstellen können.

WebListener basiert auf der [Windows-HTTP-Server-API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx). Dabei kommt der *http.sys*-Kerneltreiber zum Einsatz, der von IIS verwendet wird, um HTTP-Anforderungen zu verarbeiten und an Prozesse weiterzuleiten, die Webanwendungen ausführen. Dadurch können mehrere Prozesse auf dem gleichen physischen oder virtuellen Computer Webanwendungen am gleichen Port hosten. Die Unterscheidung erfolgt entweder anhand eines eindeutigen URL-Pfads oder anhand eines eindeutigen Hostnamens. Mit diesen Features können in Service Fabric mehrere Websites im gleichen Cluster gehostet werden.

Das folgende Diagramm veranschaulicht für WebListener die Verwendung des *http.sys*-Kerneltreibers unter Windows für die Portfreigabe:

![http.sys][3]

### <a name="weblistener-in-a-stateless-service"></a>WebListener in einem zustandslosen Dienst
Wenn Sie `WebListener` in einem zustandslosen Dienst verwenden möchten, müssen Sie die `CreateServiceInstanceListeners`-Methode überschreiben und eine `WebListenerCommunicationListener`-Instanz zurückgeben:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseWebListener()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build()))
    };
}
```

### <a name="weblistener-in-a-stateful-service"></a>WebListener in einem zustandsbehafteten Dienst

Aufgrund von Schwierigkeiten mit dem zugrunde liegenden *http.sys*-Portfreigabefeature ist `WebListenerCommunicationListener` derzeit nicht für die Verwendung in zustandsbehafteten Diensten geeignet. Weitere Informationen finden Sie im folgenden Abschnitt zur dynamischen Portzuordnung mit WebListener. Für zustandsbehaftete Dienste wird Kestrel als Webserver empfohlen.

### <a name="endpoint-configuration"></a>Endpunktkonfiguration

Eine `Endpoint`-Konfiguration ist für Webserver erforderlich, die die Windows-HTTP-Server-API verwenden. Hierzu zählt auch WebListener. Webserver, die die Windows-HTTP-Server-API verwenden, müssen ihre URL zuerst mit *http.sys* reservieren. (Hierzu wird üblicherweise das Tool [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) verwendet.) Für diese Aktion sind erhöhte Rechte erforderlich, über die Ihre Dienste standardmäßig nicht verfügen. Mithilfe der Optionen „http“ und „https“ für die `Protocol`-Eigenschaft der `Endpoint`-Konfiguration in *ServiceManifest.xml* wird die Service Fabric-Laufzeit spezifisch angewiesen, in Ihrem Auftrag eine URL mit *http.sys* zu registrieren und dabei das URL-Präfix für einen [*starken Platzhalter*](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) zu verwenden.

Wenn Sie also beispielsweise `http://+:80` für einen Dienst reservieren möchten, verwenden Sie in „ServiceManifest.xml“ die folgende Konfiguration:

```xml
<ServiceManifest ... >
    ...
    <Resources>
        <Endpoints>
            <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>

</ServiceManifest>
```

Und der Endpunktname muss an den `WebListenerCommunicationListener`-Konstruktor übergeben werden:

```csharp
 new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     return new WebHostBuilder()
         .UseWebListener()
         .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
         .UseUrls(url)
         .Build();
 })
```

#### <a name="use-weblistener-with-a-static-port"></a>Verwenden von WebListener mit einem statischen Port
Wenn Sie WebListener mit einem statischen Port verwenden möchten, geben Sie in der `Endpoint`-Konfiguration die Portnummer an:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-weblistener-with-a-dynamic-port"></a>Verwenden von WebListener mit einem dynamischen Port
Wenn Sie WebListener mit einem dynamisch zugewiesenen Port verwenden möchten, lassen Sie die `Port`-Eigenschaft in der `Endpoint`-Konfiguration weg:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

Hinweis: Ein von einer `Endpoint`-Konfiguration zugeordneter dynamischer Port stellt nur einen einzelnen Port *pro Hostprozess* bereit. Im Rahmen des aktuellen Service Fabric-Hostingmodells können mehrere Dienstinstanzen und/oder -replikate im gleichen Prozess gehostet werden. Bei der Zuordnung durch die `Endpoint`-Konfiguration teilen sie sich also jeweils den gleichen Port. Mit dem zugrunde liegenden *http.sys*-Portfreigabefeature können mehrere WebListener-Instanzen einen Port gemeinsam nutzen. Dies wird von `WebListenerCommunicationListener` jedoch aufgrund von Schwierigkeiten bei Clientanforderungen nicht unterstützt. Für die Verwendung eines dynamischen Ports wird Kestrel als Webserver empfohlen.

## <a name="kestrel-in-reliable-services"></a>Kestrel in zuverlässigen Diensten
Kestrel kann in einem zuverlässigen Dienst durch Importieren des NuGet-Pakets **Microsoft.ServiceFabric.AspNetCore.Kestrel** verwendet werden. Dieses Paket enthält `KestrelCommunicationListener` (eine Implementierung von `ICommunicationListener`), mit der Sie einen ASP.NET Core-Webhost innerhalb eines zuverlässigen Diensts mit Kestrel als Webserver erstellen können.

Kestrel ist ein plattformübergreifender Webserver für ASP.NET Core und basiert auf libuv, einer plattformübergreifenden asynchronen E/A-Bibliothek. Im Gegensatz zu WebListener verwendet Kestrel keinen zentralen Endpunkt-Manager wie *http.sys*. Und im Gegensatz zu WebListener unterstützt Kestrel keine Portfreigabe zwischen mehreren Prozessen. Jede Instanz von Kestrel muss einen eindeutigen Port verwenden.

![Kestrel][4]

### <a name="kestrel-in-a-stateless-service"></a>Kestrel in einem zustandslosen Dienst
Wenn Sie `Kestrel` in einem zustandslosen Dienst verwenden möchten, müssen Sie die `CreateServiceInstanceListeners`-Methode überschreiben und eine `KestrelCommunicationListener`-Instanz zurückgeben:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

### <a name="kestrel-in-a-stateful-service"></a>Kestrel in einem zustandsbehafteten Dienst
Wenn Sie `Kestrel` in einem zustandsbehafteten Dienst verwenden möchten, müssen Sie die `CreateServiceReplicaListeners`-Methode überschreiben und eine `KestrelCommunicationListener`-Instanz zurückgeben:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new ServiceReplicaListener[]
    {
        new ServiceReplicaListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                         services => services
                             .AddSingleton<StatefulServiceContext>(serviceContext)
                             .AddSingleton<IReliableStateManager>(this.StateManager))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

In diesem Beispiel wird eine Singletoninstanz von `IReliableStateManager` für den Webhost-Abhängigkeitsinjektionscontainer bereitgestellt. Das ist zwar nicht zwingend erforderlich, ermöglicht aber die Verwendung von `IReliableStateManager` und zuverlässigen Auflistungen in Ihren MVC-Controlleraktionsmethoden.

Beachten Sie, dass in einem zustandsbehafteten Dienst für `KestrelCommunicationListener` **kein** `Endpoint`-Konfigurationsname bereitgestellt wird. Dies wird im folgenden Abschnitt ausführlicher erläutert.

### <a name="endpoint-configuration"></a>Endpunktkonfiguration
Eine `Endpoint`-Konfiguration ist für Kestrel nicht erforderlich. 

Kestrel ist ein einfacher eigenständiger Webserver. Im Gegensatz zu WebListener (oder HttpListener) benötigt er keine `Endpoint`-Konfiguration in *ServiceManifest.xml*, da vor dem Start keine URL-Registrierung erforderlich ist. 

#### <a name="use-kestrel-with-a-static-port"></a>Verwenden von Kestrel mit einem statischen Port
Ein statischer Port kann in der `Endpoint`-Konfiguration von „ServiceManifest.xml“ für die Verwendung mit Kestrel konfiguriert werden. Das ist zwar nicht unbedingt nötig, hat jedoch zwei potenzielle Vorteile:
 1. Wenn der Port nicht im Anwendungsportbereich liegt, wird er von Service Fabric durch die Betriebssystemfirewall geöffnet.
 2. Die durch `KestrelCommunicationListener` bereitgestellte URL verwendet diesen Port.

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

Wenn ein `Endpoint`-Element konfiguriert ist, muss dessen Name an den `KestrelCommunicationListener`-Konstruktor übergeben werden: 

```csharp
new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) => ...
```

Falls keine `Endpoint`-Konfiguration verwendet wird, lassen Sie den Namen im `KestrelCommunicationListener`-Konstruktor weg. In diesem Fall wird ein dynamischer Port verwendet. Weitere Informationen finden Sie im nächsten Abschnitt.

#### <a name="use-kestrel-with-a-dynamic-port"></a>Verwenden von Kestrel mit einem dynamischen Port
Kestrel kann die automatische Portzuweisung aus der `Endpoint`-Konfiguration in „ServiceManifest.xml“ nicht verwenden, da die automatische Portzuweisung aus einer `Endpoint`-Konfiguration einen eindeutigen Port pro *Hostprozess* zuweist und ein einzelner Hostprozess mehrere Kestrel-Instanzen enthalten kann. Da Kestrel die Portfreigabe nicht unterstützt und jede Kestrel-Instanz an einem eindeutigen Port geöffnet werden muss, ist das nicht möglich.

Wenn Sie die dynamische Portzuweisung mit Kestrel verwenden möchten, lassen Sie einfach die `Endpoint`-Konfiguration in „ServiceManifest.xml“ vollständig weg, und übergeben Sie keinen Endpunktnamen an den `KestrelCommunicationListener`-Konstruktor:

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

In dieser Konfiguration wählt `KestrelCommunicationListener` automatisch einen nicht verwendeten Port aus dem Anwendungsportbereich aus.

## <a name="scenarios-and-configurations"></a>Szenarien und Konfigurationen
Dieser Abschnitt enthält eine Beschreibung der folgenden Szenarien und gibt die empfohlene Kombination aus Webserver, Portkonfiguration, Service Fabric-Integrationsoptionen und sonstigen Einstellungen an, die für einen ordnungsgemäß funktionierenden Dienst benötigt wird:
 - Zustandsloser ASP.NET Core-Dienst (extern verfügbar gemacht)
 - Zustandsloser ASP.NET Core-Dienst (nur intern)
 - Zustandsbehafteter ASP.NET Core-Dienst (nur intern)

Ein **extern verfügbar gemachter** Dienst macht einen Endpunkt verfügbar, auf den von außerhalb des Clusters zugegriffen werden kann (üblicherweise durch einen Load Balancer).

Bei einem **rein internen** Dienst ist der Endpunkt nur innerhalb des Clusters erreichbar.

> [!NOTE]
> Zustandsbehaftete Dienstendpunkte sollten grundsätzlich nicht für das Internet verfügbar gemacht werden. Cluster hinter einem Load Balancer ohne Informationen zur Service Fabric-Dienstauflösung (beispielsweise Azure Load Balancer) können keine zustandsbehafteten Dienste verfügbar machen, da der Load Balancer das entsprechende zustandsbehaftete Dienstreplikat nicht finden und keinen Datenverkehr dorthin weiterleiten kann. 

### <a name="externally-exposed-aspnet-core-stateless-services"></a>Zustandslose ASP.NET Core-Dienste (extern verfügbar gemacht)
WebListener ist der empfohlene Webserver für Front-End-Dienste, die unter Windows externe HTTP-Endpunkte mit Internetzugriff verfügbar machen. Er bietet einen besseren Schutz vor Angriffen und unterstützt Features, die von Kestrel nicht unterstützt werden. Hierzu zählen beispielsweise die Windows-Authentifizierung und die Portfreigabe. 

Kestrel wird derzeit nicht als Edgeserver (Server mit Internetzugriff) unterstützt. Zur Abwicklung von Datenverkehr aus dem öffentlichen Internet muss ein Reverseproxyserver wie IIS oder Nginx verwendet werden.
 
Ein für das Internet verfügbar gemachter zustandsloser Dienst muss einen bekannten und stabilen Endpunkt verwenden, der über einen Load Balancer erreichbar ist. Das ist die URL, die Sie den Benutzern Ihrer Anwendung zur Verfügung stellen. Empfohlene Konfiguration:

|  |  | **Hinweise** |
| --- | --- | --- |
| Webserver | WebListener | Kestrel kann verwendet werden, wenn der Dienst nur für ein vertrauenswürdiges Netzwerk (beispielsweise ein Intranet) verfügbar gemacht wird. Andernfalls ist WebListener die bevorzugte Option. |
| Portkonfiguration | Statisch | In der `Endpoints`-Konfiguration von „ServiceManifest.xml“ muss ein bekannter statischer Port konfiguriert werden – beispielsweise 80 für HTTP oder 443 für HTTPS. |
| ServiceFabricIntegrationOptions | Keine | Die Option `ServiceFabricIntegrationOptions.None` muss verwendet werden, wenn Sie Middleware für die Service Fabric-Integration konfigurieren, damit der Dienst nicht versucht, eingehende Anforderungen auf einen eindeutigen Bezeichner zu prüfen. Die eindeutig identifizierbaren Informationen, die von der Middleware verwendet werden, sind externen Benutzern Ihrer Anwendung nicht bekannt. |
| Anzahl der Instanzen | -1 | In typischen Anwendungsfällen muss die Instanzenanzahl auf „-1“ festgelegt werden, damit auf allen Knoten, die Datenverkehr von einem Load Balancer erhalten, eine Instanz zur Verfügung steht. |

Wenn sich mehrere extern verfügbar gemachte Dienste die gleichen Knoten teilen, muss ein eindeutiger, aber stabiler URL-Pfad verwendet werden. Hierzu kann die URL geändert werden, die beim Konfigurieren von IWebHost angegeben wurde. Hinweis: Dies gilt nur für WebListener.

 ```csharp
 new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     url += "/MyUniqueServicePath";
 
     return new WebHostBuilder()
         .UseWebListener()
         ...
         .UseUrls(url)
         .Build();
 })
 ```

### <a name="internal-only-stateless-aspnet-core-service"></a>Zustandsloser ASP.NET Core-Dienst (nur intern)
Zustandslose Dienste, die nur innerhalb des Clusters aufgerufen werden, sollten eindeutige URLs und dynamisch zugewiesene Ports verwenden, um die Zusammenarbeit zwischen mehreren Diensten zu gewährleisten. Empfohlene Konfiguration:

|  |  | **Hinweise** |
| --- | --- | --- |
| Webserver | Kestrel | Für interne zustandslose Dienste kann zwar WebListener verwendet werden, als Server wird jedoch Kestrel empfohlen, um die gemeinsame Verwendung eines Hosts durch mehrere Dienstinstanzen zu ermöglichen.  |
| Portkonfiguration | Dynamisch zugewiesen | Mehrere Replikate eines zustandsbehafteten Diensts können gemeinsam einen Hostprozess oder ein Hostbetriebssystem verwenden und benötigen daher eindeutige Ports. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Bei dynamischer Portzuweisung verhindert diese Einstellung das weiter oben beschriebene Verwechslungsproblem. |
| InstanceCount | beliebig | Die Einstellung für die Instanzenanzahl kann auf einen beliebigen Wert festgelegt werden, der für den Betrieb des Diensts erforderlich ist. |

### <a name="internal-only-stateful-aspnet-core-service"></a>Zustandsbehafteter ASP.NET Core-Dienst (nur intern)
Zustandsbehaftete Dienste, die nur innerhalb des Clusters aufgerufen werden, sollten dynamisch zugewiesene Ports verwenden, um die Zusammenarbeit zwischen mehreren Diensten zu gewährleisten. Empfohlene Konfiguration:

|  |  | **Hinweise** |
| --- | --- | --- |
| Webserver | Kestrel | `WebListenerCommunicationListener` ist nicht für die Verwendung durch zustandsbehaftete Dienste konzipiert, in denen sich Replikate einen Hostprozess teilen. |
| Portkonfiguration | Dynamisch zugewiesen | Mehrere Replikate eines zustandsbehafteten Diensts können gemeinsam einen Hostprozess oder ein Hostbetriebssystem verwenden und benötigen daher eindeutige Ports. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Bei dynamischer Portzuweisung verhindert diese Einstellung das weiter oben beschriebene Verwechslungsproblem. |

## <a name="next-steps"></a>Nächste Schritte
[Debuggen der Service Fabric-Anwendung mithilfe von Visual Studio](service-fabric-debugging-your-application.md)

<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png

