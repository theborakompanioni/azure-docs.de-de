---
title: "Konfigurieren von Redis-Clustern für Azure Redis Cache vom Typ „Premium“ | Microsoft Docs"
description: "Erfahren Sie, wie Sie das Redis-Clustering für Azure Redis Cache-Instanzen im Premium-Tarif erstellen und verwalten."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 62208eec-52ae-4713-b077-62659fd844ab
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 02/14/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 8929a1697bf88da82fc027520d0126eaef872840
ms.openlocfilehash: ec7bdf6b27cc073324d0d3a79b268e9730a6016b
ms.lasthandoff: 02/09/2017


---
# <a name="how-to-configure-redis-clustering-for-a-premium-azure-redis-cache"></a>Konfigurieren von Redis-Clustern für Azure Redis Cache vom Typ "Premium"
Für Azure Redis Cache stehen verschiedene Cacheangebote bereit, die Flexibilität bei der Auswahl von Cachegröße und -features bieten, einschließlich Features des Premium-Tarifs wie die Unterstützung für Clustering, Persistenz und virtuelle Netzwerke. In diesem Artikel wird erläutert, wie Cluster in einer Azure Redis Cache-Instanz vom Typ "Premium" konfiguriert werden.

Weitere Informationen zu anderen Premium-Cache-Features finden Sie unter [Einführung in den Premium-Tarif von Azure Redis Cache](cache-premium-tier-intro.md).

## <a name="what-is-redis-cluster"></a>Was ist Redis Cluster?
Azure Redis Cache umfasst Redis Cluster entsprechend der [Implementierung in Redis](http://redis.io/topics/cluster-tutorial). Redis Cluster bringt Ihnen die folgenden Vorteile. 

* Die Möglichkeit, das DataSet automatisch zwischen mehreren Knoten aufzuteilen. 
* Die Möglichkeit, Vorgänge fortzusetzen, wenn bei einer Teilmenge der Knoten Fehler auftreten oder nicht alle Knoten im Cluster miteinander kommunizieren können. 
* Höherer Durchsatz: Der Durchsatz steigt linear, wenn Sie die Anzahl der Shards erhöht. 
* Höhere Speichergröße: Die Speichergröße erhöht sich linear, wenn Sie die Anzahl der Shards erhöht.  

Ausführliche Informationen zu Größe, Durchsatz und Bandbreite von Premium-Caches finden Sie in den [häufig gestellten Fragen zu Azure Redis Cache](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) . 

In Azure wird Redis Cluster als Modell aus primärem Cache und Replikatcache angeboten, in dem jeder Shard über ein Paar aus primärem Cache und Replikatcache mit Replikation verfügt und die Replikation mit dem Azure Redis Cache-Dienst verwaltet wird. 

## <a name="clustering"></a>Clustering
Clustering wird während der Erstellung des Caches auf dem Blatt **Neuer Redis Cache** aktiviert. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Das Clustering wird auf dem Blatt **Redis Cluster** konfiguriert.

![Clustering][redis-cache-clustering]

Sie können bis zu 10 Shards im Cluster festlegen. Klicken Sie auf **Aktiviert**, und schieben Sie den Schieberegler, oder geben Sie eine Zahl zwischen 1 und 10 für **Shard count** ein. Klicken Sie anschließend auf **OK**.

Jeder Shard ist ein Paar aus primärem Cache und Replikatcache, das in Azure verwaltet wird. Die Gesamtgröße des Caches wird durch Multiplikation der Anzahl der Shards mit der im Tarif ausgewählten Cachegröße berechnet. 

![Clustering][redis-cache-clustering-selected]

Nach der Erstellung des Caches können Sie eine Verbindung mit dem Cache herstellen und ihn genauso wie einen nicht gruppierten Cache verwenden. Redis verteilt die Daten auf alle Cache-Shards. Wenn die Diagnosefunktion [aktiviert](cache-how-to-monitor.md#enable-cache-diagnostics) ist, werden Metriken für jeden Shard separat erfasst und können auf dem Redis Cache-Blatt [angezeigt](cache-how-to-monitor.md) werden. 

> [!NOTE]
> 
> Es gibt einige geringfügige Unterschiede, die in Ihrer Clientanwendung erforderlich sind, wenn das Clustering konfiguriert ist. Weitere Informationen finden Sie unter [Muss ich Änderungen an meiner Clientanwendung vornehmen, um Clustering verwenden zu können?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 

Beispielcode zum Arbeiten mit Clustering mit dem StackExchange.Redis-Client finden Sie im [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs)-Teil des [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld)-Beispiels.

<a name="cluster-size"></a>

## <a name="change-the-cluster-size-on-a-running-premium-cache"></a>Ändern der Clustergröße auf einem ausgeführten Premium-Cache
Klicken Sie zum Ändern der Clustergröße auf einem ausgeführten Premium-Cache mit aktiviertem Clustering im **Ressourcenmenü** auf **(VORSCHAU) Größe des Redis-Clusters**.

> [!NOTE]
> Beachten Sie, dass sich trotz allgemeiner Verfügbarkeit der Azure Redis Cache in der Premium-Stufe das Feature Redis-Clustergröße derzeit in der Vorschau befindet.
> 
> 

![Redis-Clustergröße][redis-cache-redis-cluster-size]

Um die Clustergröße zu ändern, verwenden Sie den Schieberegler, oder geben Sie im Textfeld **Shardanzahl** eine Zahl zwischen 1 und 10 ein, und klicken Sie zum Speichern auf **OK**.

## <a name="clustering-faq"></a>Clustering – häufig gestellte Fragen
Die folgende Liste enthält Antworten auf häufig gestellte Fragen zum Clustering in Azure Redis Cache.

* [Muss ich Änderungen an meiner Clientanwendung vornehmen, um das Clustering verwenden zu können?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
* [Wie sind Schlüssel in einem Cluster verteilt?](#how-are-keys-distributed-in-a-cluster)
* [Was ist die maximale Cachegröße, die ich erstellen kann?](#what-is-the-largest-cache-size-i-can-create)
* [Wird das Clustering auf allen Redis-Clients unterstützt?](#do-all-redis-clients-support-clustering)
* [Wie stelle ich eine Verbindung mit dem Cache her, wenn das Clustering aktiviert ist?](#how-do-i-connect-to-my-cache-when-clustering-is-enabled)
* [Kann ich direkt eine Verbindung mit den einzelnen Shards des Caches herstellen?](#can-i-directly-connect-to-the-individual-shards-of-my-cache)
* [Kann ich das Clustering für einen bereits erstellten Cache konfigurieren?](#can-i-configure-clustering-for-a-previously-created-cache)
* [Kann ich das Clustering für einen Basic- oder Standard-Cache konfigurieren?](#can-i-configure-clustering-for-a-basic-or-standard-cache)
* [Kann ich das Clustering mit den Redis ASP.NET-Sitzungszustands- und -Ausgabezwischenspeicherungsanbietern verwenden?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)
* [Bei Verwenden von StackExchange.Redis und Clustering erhalte ich MOVE-Ausnahmen. Was soll ich tun?](#i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do)

### <a name="do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering"></a>Muss ich Änderungen an meiner Clientanwendung vornehmen, um das Clustering verwenden zu können?
* Wenn das Clustering aktiviert ist, ist nur Datenbank 0 verfügbar. Wenn die Clientanwendung mehrere Datenbanken verwendet und versucht, eine andere Datenbank als Datenbank 0 zu lesen oder Schreibvorgänge dafür auszuführen, wird die folgende Ausnahme ausgelöst: `Unhandled Exception: StackExchange.Redis.RedisConnectionException: ProtocolFailure on GET --->` `StackExchange.Redis.RedisCommandException: Multiple databases are not supported on this server; cannot switch to database: 6`
  
  Weitere Informationen finden Sie unter [Redis Cluster Specification - Implemented subset](http://redis.io/topics/cluster-spec#implemented-subset)(Spezifikation für Redis-Cluster – implementierte Teilmenge).
* Wenn Sie [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/)nutzen, müssen Sie Version 1.0.481 oder höher verwenden. Sie stellen eine Verbindung mit dem Cache mit den gleichen [Endpunkten, Ports und Schlüsseln](cache-configure.md#properties) her, die Sie auch für einen Cache verwenden, bei dem das Clustering nicht aktiviert ist. Der einzige Unterschied besteht darin, dass alle Lese- und Schreibvorgänge für Datenbank 0 erfolgen müssen.
  
  * Für andere Clients gelten möglicherweise andere Anforderungen. Weitere Informationen finden Sie unter [Wird das Clustering auf allen Redis-Clients unterstützt?](#do-all-redis-clients-support-clustering)
* Wenn für Ihre Anwendung mehrere Schlüsselvorgänge zu einem einzelnen Befehl zusammengefasst sind, müssen sich alle Schlüssel in demselben Shard befinden. Informationen dazu, wie Sie dies erreichen, finden Sie unter [Wie sind Schlüssel in einem Cluster verteilt?](#how-are-keys-distributed-in-a-cluster)
* Bei Verwendung des Redis ASP.NET-Sitzungszustandsanbieters müssen Sie Version 2.0.1 oder höher verwenden. Weitere Informationen finden Sie unter [Kann ich das Clustering mit den Redis ASP.NET-Sitzungszustands- und -Ausgabezwischenspeicherungsanbietern verwenden?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)

### <a name="how-are-keys-distributed-in-a-cluster"></a>Wie sind Schlüssel in einem Cluster verteilt?
Laut Redis-Dokumentation zum [Schlüsselverteilungsmodell](http://redis.io/topics/cluster-spec#keys-distribution-model) wird der Schlüsselbereich in 16384 Slots aufgeteilt. Jeder Schlüssel wird gehasht und einem dieser Slots zugewiesen, die auf die Knoten des Clusters verteilt werden. Sie können konfigurieren, welcher Teil des Schlüssels gehasht ist, um sicherzustellen, dass mehrere Schlüssel in demselben Shard angeordnet sind. Verwenden Sie hierfür Hashtags.

* Schlüssel mit einem Hashtag: Wenn ein Teil des Schlüssels in `{` und `}` gesetzt ist, wird nur dieser Teil des Schlüssels gehasht, um den Hashslot eines Schlüssels zu ermitteln. Die folgenden drei Schlüssel würden sich beispielsweise in demselben Shard befinden: `{key}1`, `{key}2` und `{key}3`, da nur der `key`-Teil des Namens gehasht ist. Eine vollständige Liste mit den Spezifikationen für Schlüssel-Hashtags finden Sie unter [Schlüssel-Hashtags](http://redis.io/topics/cluster-spec#keys-hash-tags).
* Schlüssel ohne Hashtag: Der gesamte Schlüsselname wird für das Hashing verwendet. Dies ergibt eine statistisch gleichmäßige Verteilung über die Shards des Caches.

Zum Erzielen einer optimalen Leistung und eines hohen Durchsatzes empfehlen wir die gleichmäßige Verteilung der Schlüssel. Wenn Sie Schlüssel mit Hashtag verwenden, muss von der Anwendung sichergestellt werden, dass die Schlüssel gleichmäßig verteilt werden.

Weitere Informationen finden Sie unter [Schlüsselverteilungsmodell](http://redis.io/topics/cluster-spec#keys-distribution-model), [Redis Cluster-Daten-Sharding](http://redis.io/topics/cluster-tutorial#redis-cluster-data-sharding) und [Schlüssel-Hashtags](http://redis.io/topics/cluster-spec#keys-hash-tags).

Beispielcode zum Arbeiten mit Clustering und Suchen von Schlüsseln im gleichen Shard mit dem StackExchange.Redis-Client finden Sie im [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs)-Teil des [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld)-Beispiels.

### <a name="what-is-the-largest-cache-size-i-can-create"></a>Was ist die maximale Cachegröße, die ich erstellen kann?
Die maximale Größe bei Premium-Cache beträgt 53 GB. Sie können bis zu 10 Shards mit einer maximalen Größe von 530 GB erstellen. Wenn Sie eine höhere Cachegröße benötigen, können Sie sie [anfordern](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Weitere Informationen finden Sie unter [Azure Redis Cache – Preise](https://azure.microsoft.com/pricing/details/cache/).

### <a name="do-all-redis-clients-support-clustering"></a>Wird das Clustering auf allen Redis-Clients unterstützt?
Zum gegenwärtigen Zeitpunkt unterstützen nicht alle Clients das Redis-Clustering. StackExchange.Redis ist einer der Clients, der das Clustering unterstützt. Weitere Informationen zu anderen Clients finden Sie im [Redis Cluster Tutorial](http://redis.io/topics/cluster-tutorial) im Abschnitt [Playing with the cluster (Arbeiten mit dem Cluster)](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster).

> [!NOTE]
> Wenn Sie StackExchange.Redis als Client verwenden, müssen Sie sicherstellen, dass Sie die neueste Version [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) 1.0.481 oder höher verwenden, damit das Clustering ordnungsgemäß ausgeführt wird. Wenn Sie Probleme mit MOVE-Ausnahmen haben, finden Sie unter [MOVE-Ausnahmen](#move-exceptions) weitere Informationen.
> 
> 

### <a name="how-do-i-connect-to-my-cache-when-clustering-is-enabled"></a>Wie stelle ich eine Verbindung mit dem Cache her, wenn das Clustering aktiviert ist?
Sie können eine Verbindung mit dem Cache mit den gleichen [Endpunkten](cache-configure.md#properties), [Ports](cache-configure.md#properties) und [Schlüsseln](cache-configure.md#access-keys) herstellen, die Sie auch für einen Cache verwenden, bei dem das Clustering nicht aktiviert ist. Redis verwaltet das Clustering auf dem Back-End, sodass es nicht über Ihren Client verwaltet werden muss.

### <a name="can-i-directly-connect-to-the-individual-shards-of-my-cache"></a>Kann ich direkt eine Verbindung mit den einzelnen Shards des Caches herstellen?
Dies wird offiziell nicht unterstützt. Allerdings besteht jeder Shard aus einem Paar aus primärem Cache und Replikatcache, die zusammen als Cacheinstanz bezeichnet werden. Mit dem Hilfsprogramm redis-cli in der [unstable](http://redis.io/download) -Verzweigung des Redis-Repositorys auf GitHub können Sie eine Verbindung mit diesen Cacheinstanzen herstellen. Diese Version implementiert grundlegende Unterstützung, wenn sie mit dem Switch `-c` gestartet wird. Weitere Informationen finden Sie im [Redis Cluster-Tutorial](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) unter [Playing with the cluster](http://redis.io) (Ausprobieren des Clusters) auf [http://redis.io](http://redis.io/topics/cluster-tutorial).

Ohne SSL verwenden Sie die folgenden Befehle.

    Redis-cli.exe –h <<cachename>> -p 13000 (to connect to instance 0)
    Redis-cli.exe –h <<cachename>> -p 13001 (to connect to instance 1)
    Redis-cli.exe –h <<cachename>> -p 13002 (to connect to instance 2)
    ...
    Redis-cli.exe –h <<cachename>> -p 1300N (to connect to instance N)

Ersetzen Sie für SSL `1300N` durch `1500N`.

### <a name="can-i-configure-clustering-for-a-previously-created-cache"></a>Kann ich das Clustering für einen bereits erstellten Cache konfigurieren?
Zurzeit können Sie das Clustering lediglich während der Erstellung eines Caches aktivieren. Sie können nach dem Erstellen des Caches die Clustergröße ändern, jedoch einem Premium-Cache kein Clustering hinzufügen oder das Clustering von einem Premium-Cache entfernen, nachdem der Cache erstellt wurde. Ein Premium-Cache mit aktivierter Clusterunterstützung und nur einem Shard unterscheidet sich von einem Premium-Cache der gleichen Größe ohne Clustering.

### <a name="can-i-configure-clustering-for-a-basic-or-standard-cache"></a>Kann ich das Clustering für einen Basic- oder Standard-Cache konfigurieren?
Clustering ist nur für Premium-Caches verfügbar.

### <a name="can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers"></a>Kann ich das Clustering mit den Redis ASP.NET-Sitzungszustands- und -Ausgabezwischenspeicherungsanbietern verwenden?
* **Redis-Ausgabecacheanbieter:** Keine Änderungen erforderlich.
* **Redis-Sitzungszustandsanbieter:** Zum Verwenden des Clusterings müssen Sie [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 oder höher verwenden. Andernfalls wird eine Ausnahme ausgelöst. Dies ist eine unterbrechende Änderung. Weitere Informationen finden Sie unter [v2.0.0 Breaking Change Details (v2.0.0 – Details zu unterbrechenden Änderungen)](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

<a name="move-exceptions"></a>

### <a name="i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do"></a>Bei Verwenden von StackExchange.Redis und Clustering erhalte ich MOVE-Ausnahmen. Was soll ich tun?
Wenn Sie StackExchange.Redis und Clustering verwenden und dabei `MOVE` -Ausnahmen erhalten, vergewissern Sie sich, dass Sie mindestens [StackExchange.Redis 1.1.603](https://www.nuget.org/packages/StackExchange.Redis/) verwenden. Anweisungen zum Konfigurieren Ihrer .NET-Anwendungen für die Verwendung von StackExchange.Redis finden Sie unter [Konfigurieren der Cacheclients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Verwendung weiterer Funktionen des Premium-Caches finden Sie in den folgenden Artikeln.

* [Einführung in den Premium-Tarif von Azure Redis Cache](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-clustering]: ./media/cache-how-to-premium-clustering/redis-cache-clustering.png

[redis-cache-clustering-selected]: ./media/cache-how-to-premium-clustering/redis-cache-clustering-selected.png

[redis-cache-redis-cluster-size]: ./media/cache-how-to-premium-clustering/redis-cache-redis-cluster-size.png








