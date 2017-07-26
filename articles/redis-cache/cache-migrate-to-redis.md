---
title: "Migrieren von Managed Cache Service-Anwendungen zu Redis – Azure | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie Managed Cache Service- und In-Role Cache-Anwendungen zu Azure Redis Cache migrieren.
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 041f077b-8c8e-4d7c-a3fc-89d334ed70d6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/30/2017
ms.author: sdanie
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: 0fbfb945c66926794721f2ce8cc183dac51ecb27
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="migrate-from-managed-cache-service-to-azure-redis-cache"></a>Migrieren von Managed Cache Service zu Azure Redis Cache
Zum Migrieren von Anwendungen, die Azure Managed Cache Service verwenden, zu Azure Redis Cache, sind je nach den von Ihrer Cachinganwendung verwendeten Features nur minimale Änderungen an der Anwendung erforderlich. Sie APIs sind zwar nicht identisch, aber doch sehr ähnlich. Der vorhandene Code für die Verwendung von Managed Cache Service kann nach minimalen Änderungen für den Zugriff auf einen Cache wiederverwendet werden. In diesem Thema wird beschrieben, wie Sie die erforderlichen Änderungen an der Konfiguration und der Anwendung vornehmen, um Managed Cache Service-Anwendungen zur Verwendung von Azure Redis Cache zu migrieren, und wie Sie mit einigen der Features von Azure Redis Cache die Funktionen eines Managed Cache Service-Cache implementieren.

>[!NOTE]
>Managed Cache Service und In-Role Cache wurden am 30. November 2016 [eingestellt](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/). Wenn Sie über In-Role Cache-Bereitstellungen verfügen, die Sie zu Azure Redis Cache migrieren möchten, können Sie die Schritte in diesem Artikel ausführen.

## <a name="migration-steps"></a>Schritte bei der Migration
Die folgenden Schritte sind zum Migrieren einer Managed Cache Service-Anwendung zur Verwendung von Azure Redis Cache erforderlich.

* Zuordnen der Managed Cache Service-Features zu Azure Redis Cache
* Auswählen eines Cacheangebots
* Erstellen eines Caches
* Konfigurieren der Cacheclients
  * Entfernen der Managed Cache Service-Konfiguration
  * Konfigurieren eines Cacheclients mithilfe des StackExchange.Redis-NuGet-Pakets
* Migrieren des Managed Cache Service-Codes
  * Herstellen einer Verbindung mit dem Cache mithilfe der ConnectionMultiplexer-Klasse
  * Zugreifen auf primitive Datentypen im Cache
  * Arbeiten mit .NET-Objekten im Cache
* Migrieren des ASP.NET-Sitzungszustands und des Ausgabecaches zu Azure Redis Cache 

## <a name="map-managed-cache-service-features-to-azure-redis-cache"></a>Zuordnen der Managed Cache Service-Features zu Azure Redis Cache
Azure Managed Cache Service und Azure Redis Cache sind zwar ähnlich, bei der Implementierung der Features gibt es jedoch Unterschiede. In diesem Abschnitt werden einige der Unterschiede beschrieben und Anleitungen zur Implementierung der Funktionen von Managed Cache Service in Azure Redis Cache ausgeführt.

| Managed Cache Service-Feature | Managed Cache Service-Unterstützung | Azure Redis Cache-Unterstützung |
| --- | --- | --- |
| Benannte Caches |Es ist ein Standardcache konfiguriert. Bei den Cacheangeboten „Standard“ und „Premium“ können bei Bedarf bis zu neun zusätzliche benannte Caches konfiguriert werden. |Für Azure Redis-Caches können 16 Datenbanken konfiguriert werden, die verwendet werden können, um Funktionalität ähnlich benannter Caches zu implementieren. Weitere Informationen finden Sie unter [Was sind Redis-Datenbanken?](cache-faq.md#what-are-redis-databases) und [Standardmäßige Redis-Serverkonfiguration](cache-configure.md#default-redis-server-configuration). |
| Hohe Verfügbarkeit |Bietet bei den Cacheangeboten „Standard“ und „Premium“ hohe Verfügbarkeit für Elemente im Cache. Wenn Elemente aufgrund eines Fehlers verloren gehen, sind weiterhin Sicherungskopien der Elemente im Cache verfügbar. Schreibvorgänge im sekundären Cache werden synchron ausgeführt. |Hohe Verfügbarkeit ist bei den Cacheangeboten „Standard“ und „Premium“ erhältlich, die eine Primär/Replikat-Konfiguration mit zwei Knoten aufweisen. (Jeder Shard in einem Premium-Cache verfügt über ein Paar aus Primär-/Replikatknoten.) Schreibvorgänge im Replikat erfolgen asynchron. Weitere Informationen finden Sie unter [Azure Redis Cache – Preise](https://azure.microsoft.com/pricing/details/cache/). |
| Benachrichtigungen |Ermöglicht Clients den Empfang von asynchronen Benachrichtigungen, wenn verschiedene Cachevorgänge für einen benannten Cache auftreten. |Clientanwendungen können Redis-Pub/Sub- oder [Keyspace-Benachrichtigungen](cache-configure.md#keyspace-notifications-advanced-settings) verwenden, um eine ähnliche Benachrichtigungsfunktionalität zu erzielen. |
| Lokaler Cache |Speichert eine Kopie der zwischengespeicherten Objekte lokal auf dem Client, um einen extrem schnellen Zugriff zu ermöglichen. |Clientanwendungen müssen diese Funktionalität mit einem Wörterbuch oder einer ähnlichen Datenstruktur implementieren. |
| Entfernungsrichtlinie |Keine oder LRU. Die Standardrichtlinie ist LRU. |Azure Redis Cache unterstützt die folgenden Entfernungsrichtlinien: „volatile-lru“, „allkeys-lru“, „volatile-random“, „allkeys-random“, „volatile-ttl“ und „noeviction“. Die Standardrichtlinie ist „volatile-lru“. Weitere Informationen finden Sie unter [Standardmäßige Redis-Serverkonfiguration](cache-configure.md#default-redis-server-configuration). |
| Ablaufrichtlinie |Die Standardablaufrichtlinie ist „Absolut“, und das Standardablaufintervall beträgt 10 Minuten. Außerdem sind die Richtlinien „Gleitend“ und „Nie“ verfügbar. |Standardmäßig laufen Elemente im Cache nicht ab, es kann jedoch ein Ablaufdatum pro Schreibvorgang mithilfe von Cachesatzüberladungen konfiguriert werden. Weitere Informationen finden Sie unter [Hinzufügen zu und Abrufen von Objekten aus dem Cache](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache). |
| Bereiche und Tagging |Bereiche sind Untergruppen für zwischengespeicherte Elemente. Bereiche unterstützen auch Anmerkungen für zwischengespeicherte Elemente mithilfe zusätzlicher beschreibender Zeichenfolgen, den sogenannten Tags. Bereiche unterstützen die Möglichkeit, Suchvorgänge für alle Elemente mit Tags in diesem Bereich auszuführen. Alle Elemente in einem Bereich befinden sich in einem einzelnen Knoten des Cacheclusters. |Ein Redis-Cache besteht aus einem einzelnen Knoten (sofern der Redis-Cluster nicht aktiviert ist), sodass das Konzept von Managed Cache Service-Bereichen nicht gilt. Redis unterstützt beim Abrufen von Schlüsseln Such- und Platzhaltervorgänge, damit beschreibende Tags in die Schlüsselnamen eingebettet und die Elemente später abgerufen werden können. Ein Beispiel für die Implementierung einer Tagginglösung mit Redis finden Sie unter [Implementieren des Cache-Taggings mit Redis](http://stackify.com/implementing-cache-tagging-redis/). |
| Serialisierung |Managed Cache unterstützt NetDataContractSerializer, BinaryFormatter und die Verwendung von benutzerdefinierten Serialisierungsprogrammen. Der Standardwert ist NetDataContractSerializer. |Das Serialisieren von .NET Objekten vor dem Platzieren im Cache ist Aufgabe der Clientanwendung, wobei die Entscheidung für ein Serialisierungsprogramm dem Client-Anwendungsentwickler überlassen ist. Weitere Informationen und Beispielcode finden Sie unter [Arbeiten mit .NET-Objekten im Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache). |
| Cache-Emulator |Managed Cache bietet einen lokalen Cache-Emulator. |Azure Redis Cache enthält keinen Emulator, Sie können jedoch [das MSOpenTech-Build von „redis-server.exe“ lokal ausführen](cache-faq.md#cache-emulator) , um Emulatorfunktionen bereitzustellen. |

## <a name="choose-a-cache-offering"></a>Auswählen eines Cacheangebots
Microsoft Azure Redis Cache ist in den folgenden Ebenen verfügbar:

* **Basic** – Einzelner Knoten. Mehrere Größen bis zu 53 GB.
* **Standard** – Zwei Knoten: Primär/Replikat. Mehrere Größen bis zu 53 GB. 99,9 % SLA
* **Premium** – Zwei Knoten (Primär/Replikat) mit bis zu 10 Shards. Mehrere Größen von 6 GB bis 530 GB. Alle Funktionen der Standard-Ebene und weitere umfassen die Unterstützung für [Redis-Cluster](cache-how-to-premium-clustering.md), [Redis-Persistenz](cache-how-to-premium-persistence.md), und [Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9 % SLA

Diese Ausführungen unterscheiden sich hinsichtlich der Features und des Preises. Die Features werden weiter unten in diesem Leitfaden behandelt, weitere Informationen zu den Preisen finden Sie unter [Cache – Preisdetails](https://azure.microsoft.com/pricing/details/cache/).

Als Ausgangspunkt für die Migration wählen Sie zunächst die Größe aus, die der Größe des bisherigen Managed Cache Service-Caches entspricht. Skalieren Sie die Größe dann je nach Anforderungen der Anwendung. Weitere Hilfe bei der Auswahl des richtigen Azure Redis Cache-Angebots finden Sie unter [Welches Redis Cache-Angebot und welche Redis Cache-Größe sollte ich verwenden?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Erstellen eines Caches
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>Konfigurieren der Cacheclients
Nachdem der Cache erstellt und konfiguriert wurde, besteht der nächste Schritt darin, die Managed Cache Service-Konfiguration zu entfernen und die Azure Redis Cache-Konfiguration und -Verweise hinzuzufügen, damit die Cacheclients auf den Cache zugreifen können.

* Entfernen der Managed Cache Service-Konfiguration
* Konfigurieren eines Cacheclients mithilfe des StackExchange.Redis-NuGet-Pakets

### <a name="remove-the-managed-cache-service-configuration"></a>Entfernen der Managed Cache Service-Konfiguration
Bevor die Clientanwendungen für Azure Redis Cache konfiguriert werden können, müssen die vorhandene Managed Cache Service-Konfiguration und die Assemblyverweise entfernt werden, indem Sie das Managed Cache Service-NuGet-Paket deinstallieren.

Klicken Sie zum Deinstallieren des Managed Cache Service-NuGet-Pakets im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus. Wählen Sie den Knoten **Installierte Pakete** aus, und geben Sie im Suchfeld für installierte Pakete **WindowsAzure.Caching** ein. Wählen Sie **Windows** **Azure Cache** (oder **Windows** **Azure Caching**, je nach Version des NuGet-Pakets) aus, klicken Sie auf **Deinstallieren** und dann auf **Schließen**.

![Deinstallieren des Azure Managed Cache Service NuGet-Pakets](./media/cache-migrate-to-redis/IC757666.jpg)

Wenn Sie das Managed Cache Service-NuGet-Paket deinstallieren, werden die Managed Cache Service-Assemblys und die Managed Cache Service-Einträge in „app.config“ oder „web.config“ der Clientanwendung entfernt. Da einige angepasste Einstellungen nicht entfernt werden können, wenn Sie das NuGet-Paket deinstallieren, öffnen Sie „web.config“ oder „app.config“, und vergewissern Sie sich, dass die folgenden Elemente vollständig entfernt wurden.

Vergewissern Sie sich, dass der Eintrag `dataCacheClients` aus dem Element `configSections` entfernt wurde. Entfernen Sie nicht das gesamte Element `configSections`, sondern nur den Eintrag `dataCacheClients`, falls vorhanden.

```xml
<configSections>
  <!-- Existing sections omitted for clarity. -->
  <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
</configSections>
```

Vergewissern Sie sich, dass der Abschnitt `dataCacheClients` entfernt wurde. Der Abschnitt `dataCacheClients` sieht etwa wie im folgenden Beispiel aus.

```xml
<dataCacheClients>
  <dataCacheClientname="default">
    <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
    <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
    <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

    <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
    <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
    <!--<securityProperties mode="Message" sslEnabled="true">
      <messageSecurity authorizationInfo="[Authentication Key]" />
    </securityProperties>-->
  </dataCacheClient>
</dataCacheClients>
```

Nachdem die Managed Cache Service-Konfiguration entfernt wurde, können Sie den Cacheclient konfigurieren, wie im folgenden Abschnitt beschrieben.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>Konfigurieren eines Cacheclients mithilfe des StackExchange.Redis-NuGet-Pakets
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Migrieren des Managed Cache Service-Codes
Die API für den StackExchange.Redis-Cacheclient ist ähnlich dem für Managed Cache Service. Dieser Abschnitt enthält eine Übersicht über die Unterschiede.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>Herstellen einer Verbindung mit dem Cache mithilfe der ConnectionMultiplexer-Klasse
In Managed Cache Service werden Verbindungen mit dem Cache von den Klassen `DataCacheFactory` und `DataCache` behandelt. Bei Azure Redis Cache werden diese Verbindungen von der `ConnectionMultiplexer` -Klasse verwaltet.

Fügen Sie folgende using-Anweisung am Anfang jeder Datei ein, von der aus Sie auf den Cache zugreifen möchten.

```c#
using StackExchange.Redis
```

Wenn dieser Namespace nicht aufgelöst werden kann, vergewissern Sie sich, dass Sie das StackExchange.Redis-NuGet-Paket wie unter [Konfigurieren der Cacheclients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients)beschrieben hinzugefügt haben.

> [!NOTE]
> Für den StackExchange.Redis-Client ist .NET Framework 4 oder höher erforderlich.
> 
> 

Rufen Sie die statische `ConnectionMultiplexer.Connect` -Methode auf, und übergeben Sie den Endpunkt und den Schlüssel, um eine Verbindung mit einer Azure Redis Cache-Instanz herzustellen. Ein Ansatz zur Freigabe einer `ConnectionMultiplexer` -Instanz in Ihrer Anwendung ist das Verwenden einer statischen Eigenschaft, die wie im folgenden Beispiel eine verbundene Instanz zurückgibt. Dies ist eine threadsichere Möglichkeit, nur eine einzige verbundene `ConnectionMultiplexer` -Instanz zu initialisieren. In diesem Beispiel ist `abortConnect` auf „false“ festgelegt, was bedeutet, dass der Aufruf erfolgreich ist, auch wenn eine Verbindung mit dem Cache nicht hergestellt wird. Eine wichtige Funktion von `ConnectionMultiplexer` ist, dass die Verbindung mit dem Cache automatisch wiederhergestellt wird, sobald das Netzwerkproblem oder andere Ursachen beseitigt wurden.

```c#
private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
{
    return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
});

public static ConnectionMultiplexer Connection
{
    get
    {
        return lazyConnection.Value;
    }
}
```

Cacheendpunkt, Schlüssel und Ports finden Sie auf dem Blatt **Redis Cache** der Cacheinstanz. Weitere Informationen finden Sie unter [Redis Cache: Eigenschaften](cache-configure.md#properties).

Nach dem Verbindungsaufbau können Sie einen Verweis auf diese Redis-Cachedatenbank zurückgeben, indem Sie die `ConnectionMultiplexer.GetDatabase` -Methode aufrufen. Das von der `GetDatabase` -Methode zurückgegebene Objekt ist ein einfaches Durchgangsobjekt und muss nicht gespeichert werden.

```c#
IDatabase cache = Connection.GetDatabase();

// Perform cache operations using the cache object...
// Simple put of integral data types into the cache
cache.StringSet("key1", "value");
cache.StringSet("key2", 25);

// Simple get of data types from the cache
string key1 = cache.StringGet("key1");
int key2 = (int)cache.StringGet("key2");
```

Der StackExchange.Redis-Client verwendet die Typen `RedisKey` und `RedisValue`, um Elemente im Cache zu speichern und auf diese zuzugreifen. Für diese Typen sind Zuordnungen zu den meisten primitiven Sprachtypen wie Zeichenfolgen vorhanden, und sie werden häufig nicht direkt verwendet. Redis-Zeichenfolgen stellen die grundlegendste Art eines Redis-Werts dar und können viele Datentypen wie serialisierte Binärdatenströme enthalten. Sie dürfen den Typ zwar nicht direkt verwenden, verwenden jedoch die Methoden, deren Name `String` enthält. Bei den meisten primitiven Datentypen erfolgen das Speichern und Abrufen von Elementen aus dem Cache mithilfe der Methoden `StringSet` und `StringGet`, es sei denn, Sie speichern Sammlungen oder andere Redis-Datentypen im Cache. 

`StringSet` und `StringGet` sind mit den Managed Cache Service-Methoden `Put` und `Get` vergleichbar mit dem wichtigen Unterschied, dass ein .NET-Objekt vor dem Abrufen oder Festlegen erst serialisiert werden muss. 

Beim Aufruf von `StringGet` wird das Objekt zurückgegeben, sofern es vorhanden ist. Andernfalls wird NULL zurückgegeben. In diesem Fall können Sie den Wert aus der gewünschten Datenquelle abrufen und für die zukünftige Verwendung im Cache speichern. Dies wird auch als Cache-Aside-Muster bezeichnet.

Um die Ablaufzeit eines Elements im Cache anzugeben, verwenden Sie den `TimeSpan`-Parameter von `StringSet`.

```c#
cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));
```

Azure Redis Cache kann .NET-Objekte und primitive Datentypen verarbeiten. .NET-Objekte müssen jedoch zunächst serialisiert werden. Dies liegt in der Verantwortung des Anwendungsentwicklers. Dadurch erhalten Entwickler Flexibilität bei der Wahl des Serialisierungsprogramms. Weitere Informationen und Beispielcode finden Sie unter [Arbeiten mit .NET-Objekten im Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-redis-cache"></a>Migrieren des ASP.NET-Sitzungszustands und des Ausgabecaches zu Azure Redis Cache
Azure-Redis-Cache verfügt über Anbieter für den ASP.NET-Sitzungszustand und das Caching der Seitenausgabe. Um die Anwendung zu migrieren, die Managed Cache Service-Versionen dieser Anbieter verwendet, entfernen Sie zunächst die vorhandenen Abschnitte aus „web.config“ und konfigurieren die Azure Redis Cache-Versionen der Anbieter. Anweisungen zur Verwendung von Azure Redis Cache-ASP.NET-Anbieter finden Sie unter [ASP.NET-Sitzungszustandsanbieter für Azure Redis Cache](cache-aspnet-session-state-provider.md) und [ASP.NET-Ausgabecacheanbieter für Azure Redis Cache](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Nächste Schritte
In der [Azure Redis Cache-Dokumentation](https://azure.microsoft.com/documentation/services/cache/) finden Sie Tutorials, Beispiele, Videos und vieles mehr.


