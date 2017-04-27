---
title: 'Gewusst wie: Skalieren von Azure Redis Cache | Microsoft Docs'
description: Erfahren Sie, wie Sie Azure Redis Cache-Instanzen skalieren
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 350db214-3b7c-4877-bd43-fef6df2db96c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 91b3580491a1e3504a3891b66606a9bd18c0638f
ms.lasthandoff: 04/12/2017


---
# <a name="how-to-scale-azure-redis-cache"></a>Skalieren von Azure Redis Cache
Für Azure Redis Cache stehen verschiedene Cacheangebote bereit, die Flexibilität bei der Auswahl von Cachegröße und -funktionen bieten. Nach dem Erstellen des Caches können Sie Größe und Tarif des Caches skalieren, wenn sich die Anforderungen Ihrer Anwendung ändern. In diesem Artikel wird erläutert, wie Sie Ihren Cache im Azure-Portal und mithilfe von Tools wie Azure PowerShell und Azure-CLI skalieren.

## <a name="when-to-scale"></a>Zeitpunkt für die Skalierung
Sie können mithilfe der [Überwachungsfunktionen](cache-how-to-monitor.md) von Azure Redis Cache die Integration und Leistung des Caches überwachen und ermitteln, ob der Cache skaliert werden muss. 

Überwachen Sie die folgenden Metriken überwachen, um herauszufinden, ob eine Skalierung notwendig ist.

* Redis-Serverlast
* Speicherauslastung
* Netzwerkbandbreite
* CPU-Auslastung

Wenn Sie feststellen, dass der Cache die Anforderungen der Anwendung nicht mehr erfüllt, können Sie in einen höheren oder niedrigeren Tarif wechseln, der zu Ihrer Anwendung passt. Weitere Informationen dazu, wie Sie ermitteln, welcher Cachetarif geeignet ist, finden Sie unter [Welches Angebot und welche Größe eignet sich für meinen Redis Cache](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)?

## <a name="scale-a-cache"></a>Skalieren eines Caches
Zum Skalieren Ihres Caches [navigieren Sie zum Cache](cache-configure.md#configure-redis-cache-settings) im [Azure-Portal](https://portal.azure.com) und klicken im Menü **Ressource** auf **Skalieren**.

![Skalieren](./media/cache-how-to-scale/redis-cache-scale-menu.png)

Wählen Sie auf dem Blatt **Tarif auswählen** den gewünschten Tarif aus, und klicken Sie auf **Auswählen**.

![Tarif][redis-cache-pricing-tier-blade]


Sie können mit den folgenden Einschränkungen zu einem anderen Tarif wechseln:

* Sie können keine Skalierung von einem höheren Tarif auf einen niedrigeren Tarif vornehmen.
  * Eine Skalierung von einem **Premium**-Cache auf einen niedrigeren **Standard**- oder **Basic**-Cache ist nicht möglich.
  * Ein **Standard**-Cache kann nicht auf einen niedrigeren **Basic**-Cache skaliert werden.
* Ein **Basic**-Cache kann auf einen **Standard**-Cache skaliert werden, die Größe kann jedoch nicht gleichzeitig geändert werden. Wenn Sie eine andere Größe benötigen, können Sie anschließend einen Skalierungsvorgang auf die gewünschte Größe durchführen.
* Ein **Basic**-Cache kann nicht direkt auf einen **Premium**-Cache skaliert werden. Stattdessen müssen Sie in einem ersten Skalierungsvorgang von **Basic** auf **Standard** skalieren und dann in einem anschließenden Skalierungsvorgang von **Standard** auf **Premium**.
* Von einer größeren Größe kann nicht auf **C0 (250 MB)** herunterskaliert werden.
 
Während der Cache in den neuen Tarif skaliert wird, wird auf dem Blatt **Redis Cache** der Status **Wird skaliert** angezeigt.

![Skalieren][redis-cache-scaling]

Wenn die Skalierung abgeschlossen ist, ändert sich der Status von **Wird skaliert** zu **Wird ausgeführt**.

## <a name="how-to-automate-a-scaling-operation"></a>Automatisieren eines Skalierungsvorgangs
Sie können Ihre Cache-Instanzen nicht nur über das Azure-Portal skalieren, sondern auch mithilfe der PowerShell-Cmdlets, über die Azure-Befehlszeilenschnittstelle (Azure-CLI) und mithilfe der Microsoft Azure-Verwaltungsbibliotheken (MAML). 

* [Skalieren mithilfe von PowerShell](#scale-using-powershell)
* [Skalieren über die Azure-Befehlszeilenschnittstelle](#scale-using-azure-cli)
* [Skalieren mithilfe der Microsoft Azure-Verwaltungsbibliotheken](#scale-using-maml)

### <a name="scale-using-powershell"></a>Skalieren mithilfe von PowerShell
Sie können die Azure Redis Cache-Instanzen mithilfe von PowerShell skalieren. Dazu verwenden Sie das Cmdlet [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) und ändern die Eigenschaften `Size`, `Sku` oder `ShardCount`. Das folgende Beispiel veranschaulicht, wie ein Cache mit Namen `myCache` zu einem 2,5-GB-Cache skaliert wird. 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Weitere Informationen zum Skalieren mithilfe von PowerShell finden Sie unter [Skalieren eines Redis-Caches mit PowerShell](cache-howto-manage-redis-cache-powershell.md#scale).

### <a name="scale-using-azure-cli"></a>Skalieren über die Azure-Befehlszeilenschnittstelle
Wenn Sie die Azure Redis Cache-Instanzen über die Azure-Befehlszeilenschnittstelle (CLI) skalieren möchten, rufen Sie den Befehl `azure rediscache set` auf und übergeben die gewünschten Konfigurationsänderungen. Abhängig vom gewünschten Skalierungsvorgang zählen dazu die neue Größe, die SKU oder die Clustergröße.

Weitere Informationen zum Skalieren über die Azure-Befehlszeilenschnittstelle finden Sie unter [Ändern der Einstellungen eines vorhandenen Redis-Cache](cache-manage-cli.md#scale).

### <a name="scale-using-maml"></a>Skalieren mithilfe der Microsoft Azure-Verwaltungsbibliotheken
Wenn Sie die Azure Redis Cache-Instanzen mithilfe der [Microsoft Azure-Verwaltungsbibliotheken](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/) (MAML, Microsoft Azure Management Libraries) skalieren möchten, rufen Sie die Methode `IRedisOperations.CreateOrUpdate` auf und übergeben die neue Größe für `RedisProperties.SKU.Capacity`.

    static void Main(string[] args)
    {
        // For instructions on getting the access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // To scale, set a new size for the redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

Weitere Informationen finden Sie im Beispiel [Verwalten von Redis Cache mithilfe von Microsoft Azure-Verwaltungsbibliotheken](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) .

## <a name="scaling-faq"></a>Häufig gestellte Fragen zur Skalierung
Die folgende Liste enthält Antworten auf häufig gestellte Fragen zur Skalierung von Azure Redis Cache-Instanzen.

* [Kann ich eine Skalierung auf einen Premium-Cache, aus diesem oder innerhalb von diesem vornehmen?](#can-i-scale-to-from-or-within-a-premium-cache)
* [Muss ich nach dem Skalieren den Namen oder die Zugriffsschlüssel für den Cache ändern?](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [Wie funktioniert die Skalierung?](#how-does-scaling-work)
* [Gehen während der Skalierung Daten aus dem Cache verloren?](#will-i-lose-data-from-my-cache-during-scaling)
* [Hat die Skalierung Auswirkungen auf meine benutzerdefinierte Einstellung für Datenbanken?](#is-my-custom-databases-setting-affected-during-scaling)
* [Ist der Cache während der Skalierung verfügbar?](#will-my-cache-be-available-during-scaling)
* [Nicht unterstützte Vorgänge](#operations-that-are-not-supported)
* [Wie lange dauert die Skalierung?](#how-long-does-scaling-take)
* [Woher weiß ich, dass die Skalierung abgeschlossen ist?](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a>Kann ich eine Skalierung auf einen Premium-Cache, aus diesem oder innerhalb von diesem vornehmen?
* Eine Skalierung von einem **Premium**-Cache auf die niedrigeren Tarife **Basic** oder **Standard** ist nicht möglich.
* Eine Skalierung von einem bestimmten **Premium** -Cachetarif zu einem anderen ist jedoch möglich.
* Ein **Basic**-Cache kann nicht direkt auf einen **Premium**-Cache skaliert werden. Stattdessen müssen Sie in einem ersten Skalierungsvorgang von **Basic** auf **Standard** skalieren und dann in einem folgenden Skalierungsvorgang von **Standard** auf **Premium**.
* Wenn Sie beim Erstellen des **Premium** -Caches die Clusterunterstützung aktiviert haben, können Sie die [Clustergröße ändern](cache-how-to-premium-clustering.md#cluster-size). Wenn der Cache ohne aktiviertes Clustering erstellt wurde, können Sie das Clustering nicht zu einem späteren Zeitpunkt konfigurieren.
  
  Weitere Informationen finden Sie unter [Konfigurieren von Clustern für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-clustering.md).

### <a name="after-scaling-do-i-have-to-change-my-cache-name-or-access-keys"></a>Muss ich nach dem Skalieren den Namen oder die Zugriffsschlüssel für den Cache ändern?
Nein, Cachename und -schlüssel bleiben während eines Skalierungsvorgangs unverändert.

### <a name="how-does-scaling-work"></a>Wie funktioniert die Skalierung?
* Wenn ein **Basic** -Cache auf eine andere Größe skaliert wird, wird er heruntergefahren, und es wird ein neuer Cache mit der neuen Größe bereitgestellt. Während dieser Zeit ist der Cache nicht verfügbar, und alle Daten im Cache gehen verloren.
* Wenn ein **Basic**-Cache auf einen **Standard**-Cache skaliert wird, wird ein Replikatcache bereitgestellt, und die Daten werden aus dem primären Cache in den Replikatcache kopiert. Der Cache bleibt während des Skalierungsvorgangs verfügbar.
* Wenn ein **Standard**-Cache auf eine andere Größe oder auf einen **Premium**-Cache skaliert wird, wird eines der Replikate heruntergefahren und mit der neuen Größe erneut bereitgestellt, und die Daten werden übertragen. Anschließend führt das andere Replikat ein Failover aus, bevor es erneut bereitgestellt wird. Dieser Vorgang ähnelt dem, der beim Ausfall eines Cacheknotens durchgeführt wird.

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a>Gehen während der Skalierung Daten aus dem Cache verloren?
* Wenn ein **Basic** -Cache auf eine neue Größe skaliert wird, gehen alle Daten verloren, und der Cache ist während des Skalierungsvorgangs nicht verfügbar.
* Wenn ein **Basic**-Cache auf einen **Standard**-Cache skaliert wird, werden die Daten im Cache in der Regel beibehalten.
* Wenn ein **Standard**-Cache auf eine größere Größe oder auf einen höheren Tarif bzw. ein **Premium**-Cache auf eine größere Größe skaliert wird, bleiben normalerweise alle Daten erhalten. Wenn Sie einen **Standard**-Cache oder einen **Premium**-Cache auf eine kleinere Größe herunterskalieren, können Daten verloren gehen, je nachdem, ob der neue Cache groß genug für alle bisher enthaltenen Daten ist. Wenn Daten beim Herunterskalieren verloren gehen, werden die Schlüssel mithilfe der Entfernungsrichtlinie [allkeys-lru](http://redis.io/topics/lru-cache) entfernt. 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a>Hat die Skalierung Auswirkungen auf meine benutzerdefinierte Einstellung für Datenbanken?
Bei einigen Tarifen gibt es unterschiedliche [Grenzwerte für Datenbanken](cache-configure.md#databases). Wenn Sie bei der Cacheerstellung einen benutzerdefinierten Wert für die `databases`-Einstellung konfiguriert haben, müssen Sie beim Herunterskalieren einige Punkte beachten.

* Wenn Sie auf einen Tarif mit einem niedrigeren `databases` -Grenzwert skalieren als beim aktuellen Tarif:
  * Wenn Sie die Standardanzahl für `databases` verwenden (sie beträgt bei allen Tarifen 16), gehen keine Daten verloren.
  * Wenn Sie eine benutzerdefinierte Anzahl für `databases` verwenden, die innerhalb der Grenzwerte für den Tarif liegt, auf den Sie skalieren, wird diese `databases`-Einstellung beibehalten, und es gehen keine Daten verloren.
  * Wenn Sie eine benutzerdefinierte Anzahl für `databases` verwenden, die die Grenzwerte des neuen Tarifs überschreitet, wird die `databases`-Einstellung auf die Grenzwerte des neuen Tarifs herabgesetzt, und alle Daten in den entfernten Datenbanken gehen verloren.
* Wenn Sie auf einen Tarif mit dem gleichen oder einem höheren `databases`-Grenzwert skalieren wie beim aktuellen Tarif, wird Ihre `databases`-Einstellung beibehalten, und es gehen keine Daten verloren.

Hinweis: Für Caches der Tarife „Standard“ und „Premium“ gilt zwar eine SLA (Service Level Agreement, Vereinbarung zum Servicelevel) von 99,9 % für Verfügbarkeit, aber keine SLA für Datenverlust.

### <a name="will-my-cache-be-available-during-scaling"></a>Ist der Cache während der Skalierung verfügbar?
* **Standard**- und **Premium**-Caches bleiben während des Skalierungsvorgangs verfügbar.
* **Basic**-Caches sind während der Skalierung auf eine andere Größe offline, bleiben jedoch bei der Skalierung von **Basic** auf **Standard** verfügbar.

### <a name="operations-that-are-not-supported"></a>Nicht unterstützte Vorgänge
* Sie können keine Skalierung von einem höheren Tarif auf einen niedrigeren Tarif vornehmen.
  * Eine Skalierung von einem **Premium**-Cache auf einen niedrigeren **Standard**- oder **Basic**-Cache ist nicht möglich.
  * Ein **Standard**-Cache kann nicht auf einen niedrigeren **Basic**-Cache skaliert werden.
* Ein **Basic**-Cache kann auf einen **Standard**-Cache skaliert werden, die Größe kann jedoch nicht gleichzeitig geändert werden. Wenn Sie eine andere Größe benötigen, können Sie anschließend einen Skalierungsvorgang auf die gewünschte Größe durchführen.
* Ein **Basic**-Cache kann nicht direkt auf einen **Premium**-Cache skaliert werden. Stattdessen müssen Sie in einem ersten Skalierungsvorgang von **Basic** auf **Standard** skalieren und dann in einem folgenden Skalierungsvorgang von **Standard** auf **Premium**.
* Von einer größeren Größe kann nicht auf **C0 (250 MB)** herunterskaliert werden.

Wenn bei einem Skalierungsvorgang ein Fehler auftritt, versucht der Dienst, den Vorgang rückgängig zu machen, und der Cache wird auf die ursprüngliche Größe zurückgesetzt.

### <a name="how-long-does-scaling-take"></a>Wie lange dauert die Skalierung?
Die Skalierung dauert ca. 20 Minuten, je nachdem, wie viele Daten sich im Cache befinden.

### <a name="how-can-i-tell-when-scaling-is-complete"></a>Woher weiß ich, dass die Skalierung abgeschlossen ist?
Im Azure-Portal können Sie den Fortschritt der Skalierung anzeigen. Wenn die Skalierung abgeschlossen ist, ändert sich der Status des Caches zu **Wird ausgeführt**.

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png




