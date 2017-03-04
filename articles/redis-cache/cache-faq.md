---
title: FAQ zu Azure Redis Cache | Microsoft Docs
description: "In diesem Artikel erhalten Sie Antworten auf häufig gestellte Fragen sowie Informationen zu Mustern und Best Practices für Azure Redis Cache."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: c2c52b7d-b2d1-433a-b635-c20180e5cab2
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 02/14/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 256d215deb2224dca188e373a1bf2cf7fc55be1f
ms.openlocfilehash: 9089a14f1f0b1fa5265f616e0bd9f0f30aff0c60
ms.lasthandoff: 02/17/2017


---
# <a name="azure-redis-cache-faq"></a>Azure Redis Cache – häufig gestellte Fragen
In diesem Artikel erhalten Sie Antworten auf häufig gestellte Fragen sowie Informationen zu Mustern und Best Practices für Azure Redis Cache.

## <a name="what-if-my-question-isnt-answered-here"></a>Was kann ich tun, wenn meine Frage hier nicht beantwortet wird?
Wenn Ihre Frage hier nicht aufgeführt wird, informieren Sie uns, und wir helfen Ihnen dabei, eine Antwort zu finden.

* Sie können am Ende dieser FAQ in den Kommentaren eine Frage stellen und mit dem Azure Cache-Team und anderen Mitgliedern der Community über diesen Artikel diskutieren.
* Um eine größere Benutzergruppe zu erreichen, können Sie eine Frage im [MSDN-Forum zu Azure Cache](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) stellen und mit dem Azure Cache-Team und anderen Mitgliedern der Community diskutieren.
* Featureanfragen und Anregungen können an [Azure Redis Cache User Voice](https://feedback.azure.com/forums/169382-cache)gesendet werden.
* Sie können auch eine E-Mail an unsere Adresse für [externes Feedback zu Azure Cache](mailto:azurecache@microsoft.com)senden.

## <a name="azure-redis-cache-basics"></a>Azure Redis Cache – Grundlagen
Die häufig gestellten Fragen in diesem Abschnitt beziehen sich auf die Grundlagen von Azure Redis Cache.

* [Was ist Azure Redis Cache?](#what-is-azure-redis-cache)
* [Erste Schritte mit Azure Redis Cache](#how-can-i-get-started-with-azure-redis-cache)

Im Folgenden finden Sie häufig gestellte Fragen zu grundlegenden Konzepten. Fragen zu Azure Redis Cache werden in einem der anderen Abschnitte mit häufig gestellten Fragen beantwortet.

* [Welches Redis Cache-Angebot und welche Redis Cache-Größe sollte ich verwenden?](#what-redis-cache-offering-and-size-should-i-use)
* [Welche Redis Cache-Clients kann ich verwenden?](#what-redis-cache-clients-can-i-use)
* [Gibt es einen lokalen Emulator für Azure Redis Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
* [Wie überwache ich die Integrität und Leistung meines Caches?](#how-do-i-monitor-the-health-and-performance-of-my-cache)

## <a name="planning-faqs"></a>Häufig gestellte Fragen zur Planung
* [Welches Redis Cache-Angebot und welche Redis Cache-Größe sollte ich verwenden?](#what-redis-cache-offering-and-size-should-i-use)
* [Azure Redis Cache-Leistung](#azure-redis-cache-performance)
* [Ich welcher Region sollte ich meinen Cache platzieren?](#in-what-region-should-i-locate-my-cache)
* [Wie wird Azure Redis Cache abgerechnet?](#how-am-i-billed-for-azure-redis-cache)
* [Kann ich Azure Redis Cache in Verbindung mit der Azure Government Cloud oder der Azure-Cloud in China verwenden?](#can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud)

## <a name="development-faqs"></a>Häufig gestellte Fragen zur Entwicklung
* [Was bewirken die Konfigurationsoptionen für "StackExchange.Redis"?](#what-do-the-stackexchangeredis-configuration-options-do)
* [Welche Redis Cache-Clients kann ich verwenden?](#what-redis-cache-clients-can-i-use)
* [Gibt es einen lokalen Emulator für Azure Redis Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
* [Wie führe ich Redis-Befehle aus?](#how-can-i-run-redis-commands)
* [Warum gibt es für Azure Redis Cache keinen MSDN-Klassenbibliothekverweis wie für einige andere Azure-Dienste?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
* [Kann ich Azure Redis Cache als PHP-Sitzungscache verwenden?](#can-i-use-azure-redis-cache-as-a-php-session-cache)

## <a name="security-faqs"></a>Häufig gestellte Fragen zur Sicherheit
* [Wann sollte ich den Nicht-SSL-Port für die Verbindungsherstellung mit Redis verwenden?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)

## <a name="production-faqs"></a>Häufig gestellte Fragen zur Produktion
* [Welche Best Practices gelten für die Produktion?](#what-are-some-production-best-practices)
* [Was muss bei der Verwendung gängiger Redis-Befehle beachtet werden?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
* [Wie kann ich die Leistung meines Caches messen und testen?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Wichtige Details zum Threadpool-Wachstum](#important-details-about-threadpool-growth)
* [Aktivieren der Garbage Collection auf dem Server, um bei Verwenden von „StackExchange.Redis“ mehr Durchsatz auf dem Client zu erzielen](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)

## <a name="monitoring-and-troubleshooting-faqs"></a>Häufig gestellte Fragen zur Überwachung und Problembehandlung
In den häufig gestellten Fragen in diesem Abschnitt werden allgemeine Fragen zur Überwachung und Problembehandlung behandelt. Weitere Informationen zur Überwachung und Problembehandlung Ihrer Azure Redis Cache-Instanzen finden Sie unter [Überwachen von Azure Redis Cache](cache-how-to-monitor.md) und [Problembehandlung für Azure Redis Cache](cache-how-to-troubleshoot.md).

* [Wie überwache ich die Integrität und Leistung meines Caches?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
* [Die Einstellungen meines Speicherkontos für die Cachediagnose haben sich geändert. Was ist passiert?](#my-cache-diagnostics-storage-account-settings-changed-what-happened)
* [Warum ist die Diagnose für einige neue Caches aktiviert, für andere dagegen nicht?](#why-are-diagnostics-enabled-for-some-new-caches-but-not-others)
* [Warum kommt es zu Timeouts?](#why-am-i-seeing-timeouts)
* [Warum wurde mein Client vom Cache getrennt?](#why-was-my-client-disconnected-from-the-cache)

## <a name="prior-cache-offering-faqs"></a>Häufig gestellte Fragen zu vorherigen Cache-Angeboten
* [Welches Azure-Cache-Angebot ist das Richtige für mich?](#which-azure-cache-offering-is-right-for-me)

### <a name="what-is-azure-redis-cache"></a>Was ist Azure Redis Cache?
Azure Redis Cache basiert auf dem beliebten Open Source- [Redis Cache](http://redis.io). Sie erhalten Zugriff auf einen sicheren, dedizierten Redis Cache, der von Microsoft verwaltet wird und in Azure von einer beliebigen Anwendung aus aufgerufen werden kann. Eine ausführlichere Übersicht finden Sie auf der [Azure Redis Cache](https://azure.microsoft.com/services/cache/) -Produktseite auf Azure.com.

### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Erste Schritte mit Azure Redis Cache
Es gibt verschiedene Möglichkeiten, mit Azure Redis Cache zu starten.

* Sie können sich eines unserer Tutorials ansehen, verfügbar für [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md) und [Python](cache-python-get-started.md).
* Sie können sich das Video [How to Build High Performance Apps Using Microsoft Azure Redis Cache](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/) (So erstellen Sie mit Microsoft Azure Redis Cache hochleistungsfähige Apps) ansehen.
* Sie können sich die Clientdokumentation für die Clients für die jeweilige Entwicklersprache Ihres Projekts ansehen, um weitere Informationen zur Verwendung von Redis zu erhalten. Es gibt viele Redis-Clients, die mit Azure Redis Cache verwendet werden können. Eine Liste mit Redis-Clients finden Sie unter [http://redis.io/clients](http://redis.io/clients).

Wenn Sie noch kein Azure-Konto besitzen, haben Sie folgende Möglichkeiten:

* [Kostenloses Anlegen eines Azure-Kontos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). Sie erhalten ein Guthaben, mit dem Sie andere kostenpflichtige Azure-Dienste ausprobieren können. Auch nachdem Sie das Guthaben aufgebraucht haben, können Sie das Konto behalten und kostenlose Azure-Dienste und -Features nutzen.
* [Aktivieren Sie Visual Studio-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Ihr MSDN-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste verwenden können.

<a name="cache-size"></a>

### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Welches Redis Cache-Angebot und welche Redis Cache-Größe sollte ich verwenden?
Jedes Azure Redis Cache-Angebot umfasst unterschiedliche Optionen in Bezug auf **Größe**, **Bandbreite**, **Hochverfügbarkeit** und **SLA**.

Nachfolgend sind verschiedene Aspekte aufgeführt, die Ihnen bei der Wahl helfen können.

* **Speicher**: Der Basic-Tarif und der Standard-Tarif bieten 250 MB bis 53 GB. Der Premium-Tarif bietet bis zu 530 GB, die [auf Anfrage](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase)aufgestockt werden können. Weitere Informationen finden Sie unter [Azure Redis Cache – Preise](https://azure.microsoft.com/pricing/details/cache/).
* **Netzwerkleistung**: Bei einer Workload, die einen hohen Durchsatz erfordert, bietet der Premium-Tarif im Vergleich zum Standard- oder Basic-Tarif eine größere Bandbreite. Zudem haben die größeren Caches aufgrund des zugrunde liegenden virtuellen Computers, der den Cache hostet, bei jedem Tarif eine höhere Bandbreite. Ausführlichere Informationen finden Sie in der [folgenden Tabelle](#cache-performance).
* **Durchsatz**: Der Premium-Tarif bietet den maximal verfügbaren Durchsatz. Wenn Cacheserver oder -clients die Bandbreitengrenzwerte erreichen, können Timeouts auf der Clientseite auftreten. Ausführlichere Informationen finden Sie in der unten stehenden Tabelle.
* **Hohe Verfügbarkeit/SLA**: Azure Redis Cache garantiert, dass ein Standard-/Premium-Cache mindestens 99,9% der Zeit zur Verfügung steht. Weitere Informationen zu unserer SLA finden Sie unter [Azure Redis Cache Preise](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). Die SLA deckt nur die Konnektivität zu den Cache-Endpunkten ab. Sie bezieht sich dagegen nicht auf Schutz vor Datenverlusten. Es wird empfohlen, das Redis-Feature für Datenpersistenz im Premium-Tarif zu verwenden, um den Schutz vor Datenverlusten zu erhöhen.
* **Redis-Datenpersistenz**: Der Premium-Tarif ermöglicht die Persistenz der Cachedaten in einem Azure-Speicherkonto. In einem Basic- oder Standard-Cache werden alle Daten nur im Arbeitsspeicher gespeichert. Bei Problemen mit der zugrunde liegenden Infrastruktur kann es zu Datenverlusten kommen. Es wird empfohlen, das Redis-Feature für Datenpersistenz im Premium-Tarif zu verwenden, um den Schutz vor Datenverlusten zu erhöhen. Azure Redis Cache bietet RDB- und AOF-Optionen (demnächst verfügbar) bei der Redis-Persistenz. Weitere Informationen finden Sie unter [Konfigurieren von Persistenz für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-persistence.md).
* **Redis Cluster**: Zum Erstellen von Caches mit einer Größe von über 53 GB oder zum horizontalen Partitionieren („Sharding“) von Daten über mehrere Redis-Knoten hinweg können Sie das im Premium-Tarif verfügbare Redis-Clustering verwenden. Für die hohe Verfügbarkeit besteht jeder Knoten aus einem Paar aus primärem Cache und Replikatcache. Weitere Informationen finden Sie unter [Konfigurieren von Clustern für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-clustering.md).
* **Erhöhte Sicherheit und Isolation**: Die Bereitstellung von Azure Virtual Network (VNET) bietet verbesserte Sicherheit und Isolation für den Azure Redis Cache sowie Subnetze, Zugriffssteuerungsrichtlinien und andere Features, mit denen der Zugriff weiter eingeschränkt wird. Weitere Informationen finden Sie unter [Konfigurieren der Unterstützung virtueller Netzwerke für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-vnet.md).
* **Konfigurieren von Redis**: Sowohl im Standard- als auch im Premium-Tarif können Sie Redis für Keyspace-Benachrichtigungen konfigurieren.
* **Maximale Anzahl von Clientverbindungen**: Der Premium-Tarif bietet die maximale Anzahl von Clients, die eine Verbindung mit Redis herstellen können, mit einer größeren Anzahl an Verbindungen für größere Caches. Weitere Informationen finden Sie unter [Azure Redis Cache – Preise](https://azure.microsoft.com/pricing/details/cache/).
* **Dedizierter Kern für Redis Server**: Im Premium-Tarif verfügen alle Cachegrößen über einen dedizierten Kern für Redis. Im Basic- oder Standard-Tarif verfügen alle Cachegrößen ab C1 über einen dedizierten Kern für Redis-Server.
* **Redis verwendet Single-Threading**, deshalb bietet der Einsatz von mehr als zwei Kernen keine zusätzlichen Vorteile gegenüber der Verwendung von nur zwei Kernen. Größere virtuelle Computer besitzen jedoch in der Regel mehr Bandbreite als kleinere. Wenn Cacheserver oder -client die Bandbreitengrenzwerte erreichen, kommt es zu Timeouts auf Clientseite.
* **Leistungsoptimierungen**: Caches im Premium-Tarif werden auf Hardware mit schnelleren Prozessoren bereitgestellt, die im Vergleich zu den Tarifen Basic oder Standard eine bessere Leistung bieten. Caches im Premium-Tarif erreichen einen höheren Durchsatz und geringere Wartezeiten.

<a name="cache-performance"></a>

### <a name="azure-redis-cache-performance"></a>Azure Redis Cache-Leistung
In der folgenden Tabelle sind die maximalen Bandbreitenwerte beim Testen verschiedener Standard- und Premium-Cachegrößen bei Verwendung von `redis-benchmark.exe` aus einem virtuellen IaaS-Computer mit dem Azure Redis Cache-Endpunkt angegeben. 

>[!NOTE] 
>Diese Werte werden nicht garantiert, und es gibt keine SLA für diese Werte; sie sollten jedoch typischerweise erreicht werden. Führen Sie Auslastungstests für Ihre Anwendung durch, um die geeignete Cachegröße für Ihre Anwendung zu ermitteln.
>
>

Aus dieser Tabelle können folgende Schlussfolgerungen gezogen werden:

* Der Durchsatz für Caches gleicher Größe ist im Premium-Tarif besser als im Standard-Tarif. Beispielsweise liegt bei einem Cache mit 6 GB der Durchsatz von P1 bei 140.000 RPS im Vergleich zu 49.000 RPS bei C3.
* Mit dem Redis-Clustering steigt der Durchsatz linear, je mehr Shards (Knoten) Sie im Cluster verwenden. Wenn Sie beispielsweise einen P4-Cluster mit 10 Shards erstellen, beträgt der verfügbare Durchsatz 250.000 × 10 = 2,5 Millionen RPS.
* Der Durchsatz für größere Schlüsselgrößen ist im Premium-Tarif höher als im Standard-Tarif.

| Tarif | Größe | CPU-Kerne | Verfügbare Bandbreite | 1 KB Schlüsselgröße |
| --- | --- | --- | --- | --- |
| **Standard-Cachegröße** | | |**Megabits pro Sekunde (MBit/s)/Megabyte pro Sekunde (MB/s)** |**Anforderungen pro Sekunde (RPS)** |
| C0 |250 MB |Shared
 |5 / 0,625 |600 |
| C1 |1 GB |1 |100 / 12,5 |12.200 |
| C2 |2,5 GB |2 |200 / 25 |24.000 |
| C3 |6 GB |4 |400 / 50 |49.000 |
| C4 |13 GB |2 |500 / 62,5 |61.000 |
| C5 |26 GB |4 |1.000 / 125 |115.000 |
| C6 |53 GB |8 |2.000 / 250 |150.000 |
| **Premium-Cachegröße** | |**CPU-Kerne pro Shard** | |**Anforderungen pro Sekunde (RPS), pro Shard** |
| P1 |6 GB |2 |1.000 / 125 |140.000 |
| P2 |13 GB |4 |2.000 / 250 |220.000 |
| P3 |26 GB |4 |2.000 / 250 |220.000 |
| P4 |53 GB |8 |4.000 / 500 |250.000 |

Anweisungen zum Herunterladen von Redis-Tools wie beispielsweise `redis-benchmark.exe`finden Sie im Abschnitt [Wie führe ich Redis-Befehle aus?](#cache-commands) .

<a name="cache-region"></a>

### <a name="in-what-region-should-i-locate-my-cache"></a>Ich welcher Region sollte ich meinen Cache platzieren?
Um eine optimale Leistung und die niedrigste Latenz zu erzielen, sollten Sie Ihren Azure Redis Cache in derselben Region platzieren wie Ihre Cacheclientanwendung.

<a name="cache-billing"></a>

### <a name="how-am-i-billed-for-azure-redis-cache"></a>Wie wird Azure Redis Cache abgerechnet?
Die Preise von Azure Redis Cache finden Sie [hier](https://azure.microsoft.com/pricing/details/cache/). Auf der Seite mit den Preisen sind die Kosten als Stundensatz ausgewiesen. Caches werden auf Minutenbasis ab dem Zeitpunkt der Erstellung des Caches bis zu dem Zeitpunkt abgerechnet, an dem ein Cache gelöscht wird. Es gibt keine Möglichkeit, die Abrechnung eines Caches zu beenden oder anzuhalten.

## <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud"></a>Kann ich Azure Redis Cache in Verbindung mit der Azure Government Cloud oder der Azure-Cloud in China verwenden?
Ja, Azure Redis Cache ist sowohl für die Azure Government Cloud als auch für die Azure-Cloud in China verfügbar. Die URLs für den Zugriff und die Verwaltung von Azure Redis Cache in der Azure Government Cloud und der Azure-Cloud in China unterscheiden sich von den URLs der öffentlichen Azure-Cloud. Weitere Informationen zu Überlegungen, wann Azure Redis Cache in Verbindung mit der Azure Government Cloud und der Azure-Cloud in China verwendet werden sollte, finden Sie unter [Azure Government-Datenbanken – Azure Redis Cache](../azure-government/documentation-government-services-database.md#azure-redis-cache) und [Azure China Cloud – Azure Redis Cache](https://www.azure.cn/documentation/services/redis-cache/).

Informationen zur Nutzung von Azure Redis Cache in Verbindung mit PowerShell in der Azure Government Cloud und der Azure-Cloud in China finden Sie unter [Herstellen einer Verbindung mit der Azure Government-Cloud oder Azure China](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).

<a name="cache-configuration"></a>

### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Was bewirken die Konfigurationsoptionen für "StackExchange.Redis"?
Für "StackExchange.Redis" stehen zahlreiche Optionen zur Verfügung. In diesem Abschnitt werden einige der gängigsten Optionen erläutert. Ausführlichere Informationen zu den StackExchange.Redis-Optionen finden Sie unter [StackExchange.Redis-Konfiguration](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md).

| ConfigurationOptions | Beschreibung | Empfehlung |
| --- | --- | --- |
| AbortOnConnectFail |Wenn Sie diese Option auf "true" setzen, wird die Verbindung nach einem Netzwerkausfall nicht wiederhergestellt. |Bei Festlegung auf "false" kann "StackExchange.Redis" die Verbindung automatisch wiederherstellen. |
| ConnectRetry |Die Anzahl von Versuchen für die Verbindungsherstellung bei der anfänglichen Verbindung. |Die folgenden Informationen bieten eine Orientierung. |
| ConnectTimeout |Timeout in Millisekunden für Verbindungsvorgänge. |Die folgenden Informationen bieten eine Orientierung. |

In der Regel reichen die Standardwerte des Clients aus. Sie können die Optionen basierend auf Ihrer Workload optimieren.

* **Wiederholungsversuche**
  * Für „ConnectRetry“ und „ConnectTimeout“ lautet die allgemeine Empfehlung, nach einem Fehler schnell abzubrechen und den Vorgang zu wiederholen. Diese Empfehlung hängt von Ihrer Workload und davon ab, wie lange es auf Ihrem Client durchschnittlich dauert, einen Redis-Befehl auszugeben und eine Antwort zu empfangen.
  * Ermöglichen Sie eine automatische Neuverbindung durch "StackExchange.Redis", statt die Prüfung des Verbindungsstatus und die erneute Verbindungsherstellung selbst zu prüfen. **Vermeiden Sie die Verwendung der ConnectionMultiplexer.IsConnected-Eigenschaft**.
  * Schneeballeffekt: Gelegentlich kann es zu Problemen kommen, bei denen Wiederholungsversuche zu nicht behebbaren, immer wiederkehrenden Problemen führen. Wenn der Schneeballeffekt auftritt, sollten Sie die Verwendung eines exponentiellen Backoff/Retry-Algorithmus in Erwägung ziehen, wie er im Artikel [Allgemeiner Leitfaden zum Wiederholen von Vorgängen](../best-practices-retry-general.md) der Microsoft Patterns & Practices-Gruppe beschrieben wird.
* **Timeoutwerte**
  * Berücksichtigen Sie Ihre Workload, und legen Sie die Werte entsprechend fest. Wenn Sie große Werte speichern, legen Sie die Timeouteinstellung auf einen höheren Wert fest.
  * Legen Sie `AbortOnConnectFail` auf „false“ fest, und überlassen Sie StackExchange.Redis die erneute Verbindungsherstellung.
  * Verwenden Sie eine einzelne ConnectionMultiplexer-Instanz für die Anwendung. Sie können "LazyConnection" verwenden, um eine einzelne Instanz zu erstellen, die über eine Connection-Eigenschaft zurückgegeben wird, wie gezeigt in [Verbindungsherstellung mit dem Cache unter Verwendung der ConnectionMultiplexer-Klasse](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
  * Legen Sie die `ConnectionMultiplexer.ClientName` -Eigenschaft zu Diagnosezwecken auf einen eindeutigen App-Instanznamen fest.
  * Verwenden Sie für benutzerdefinierte Workloads mehrere `ConnectionMultiplexer` -Instanzen.
      * Sie können diesem Modell folgen, wenn die Last in Ihrer Anwendung variiert. Beispiel:
      * Sie können einen Multiplexer zur Verarbeitung großer Schlüssel verwenden.
      * Sie können einen Multiplexer zur Verarbeitung kleiner Schlüssel verwenden.
      * Sie können verschiedene Werte für Verbindungstimeouts und eine retry-Logik für jede verwendete ConnectionMultiplexer-Instanz verwenden.
      * Legen Sie die `ClientName` -Eigenschaft für jeden Multiplexer fest, um die Diagnose zu vereinfachen.
      * Diese Empfehlung kann zu einer optimierten Latenz pro `ConnectionMultiplexer` führen.

### <a name="what-redis-cache-clients-can-i-use"></a>Welche Redis Cache-Clients kann ich verwenden?
Einer der großen Vorteile von Redis ist, dass es viele Clients gibt, die viele verschiedene Programmiersprachen unterstützen. Eine aktuelle Liste von Clients finden Sie unter [Redis-Clients](http://redis.io/clients). Tutorials, in denen mehrere verschiedene Sprachen und Clients behandelt werden, finden Sie unter [Verwenden von Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md). Klicken Sie oben im Artikel auf die gewünschte Sprache.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>

### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Gibt es einen lokalen Emulator für Azure Redis Cache?
Für Azure Redis Cache ist kein lokaler Emulator verfügbar. Sie können jedoch die MSOpenTech-Version von „redis-server.exe“ über die [Redis-Befehlszeilentools](https://github.com/MSOpenTech/redis/releases/) auf dem lokalen Computer ausführen und eine Verbindung herstellen, um ein ähnliches Verhalten wie bei einem lokalen Cache-Emulator zu erhalten. Dies wird im folgenden Beispiel gezeigt:

    private static Lazy<ConnectionMultiplexer>
          lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });

        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


Optional können Sie die Datei [redis.conf](http://redis.io/topics/config) konfigurieren, um bei Bedarf eine bessere Abstimmung mit den [standardmäßigen Cacheeinstellungen](cache-configure.md#default-redis-server-configuration) für Ihren Online-Azure Redis Cache zu erreichen.

<a name="cache-commands"></a>

### <a name="how-can-i-run-redis-commands"></a>Wie führe ich Redis-Befehle aus?
Sie können alle aufgelisteten [Redis-Befehle](http://redis.io/commands#) mit Ausnahme der Befehle verwenden, die unter [Redis-Befehle, die in Azure Redis Cache nicht unterstützt werden](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache) aufgeführt sind. Für die Ausführung von Redis-Befehlen haben Sie mehrere Optionen.

* Wenn Sie über einen Standard- oder Premium-Cache verfügen, können Sie Redis-Befehle über die [Redis-Konsole](cache-configure.md#redis-console)ausführen. Die Redis-Konsole ist eine sichere Methode zum Ausführen von Redis-Befehlen im Azure-Portal.
* Sie können auch die Redis-Befehlszeilentools verwenden. Führen Sie hierzu die folgenden Schritte aus:
* Laden Sie die [Redis-Befehlszeilentools](https://github.com/MSOpenTech/redis/releases/) herunter.
* Stellen Sie über `redis-cli.exe`eine Verbindung mit dem Cache her. Übergeben Sie den Cacheendpunkt mithilfe des Schalters „-h“ und dem Schlüssel mit Verwendung von „-a“, wie im folgenden Beispiel gezeigt:
* `redis-cli -h <your cache="" name="">
  .redis.cache.windows.net -a <key>
  `

> [!NOTE]
> Die Redis-Befehlszeilentools funktionieren nicht mit dem SSL-Port, aber Sie können ein Hilfsprogramm wie `stunnel` verwenden, um eine sichere Verbindung zwischen den Tools und dem SSL-Port herzustellen. Anweisungen hierzu finden Sie im Blogbeitrag [Announcing ASP.NET Session State Provider for Redis Preview Release](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) (Ankündigung des ASP.NET-Sitzungsstatusanbieters für Redis-Vorschauversion).
>
>

<a name="cache-reference"></a>

### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Warum gibt es für Azure Redis Cache keinen MSDN-Klassenbibliothekverweis wie für einige andere Azure-Dienste?
Microsoft Azure Redis Cache basiert auf dem beliebten Open Source-Redis Cache. Der Zugriff kann über eine Vielzahl von [Redis-Clients](http://redis.io/clients) für zahlreiche Programmiersprachen erfolgen. Jeder Client verfügt über eine eigene API, die unter Verwendung von [Redis-Befehlen](http://redis.io/commands) Aufrufe an den Redis Cache sendet.

Da jeder Client anders ist, gibt es nicht nur einen zentralen Klassenverweis in MSDN, und jeder Client verwaltet seine eigene Verweisdokumentation. Zusätzlich zur Referenzdokumentation gibt es mehrere Tutorials, die Ihnen den Einstieg in die Verwendung von Azure Redis Cache mit verschiedenen Sprachen und Cacheclients erleichtern. Diese Tutorials finden Sie unter [Verwenden von Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md). Klicken Sie oben im Artikel auf die gewünschte Sprache.

### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Kann ich Azure Redis Cache als PHP-Sitzungscache verwenden?
Ja. Um Azure Redis Cache als PHP-Sitzungscache zu verwenden, geben Sie die Verbindungszeichenfolge zu Ihrer Azure Redis Cache-Instanz in `session.save_path` an.

> [!IMPORTANT]
> Bei der Verwendung von Azure Redis Cache als PHP-Sitzungscache müssen Sie – wie im folgenden Beispiel dargestellt – eine URL-Codierung des Sicherheitsschlüssels durchführen, der zum Herstellen der Verbindung mit dem Cache verwendet wird:
>
> `session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
> Wurde keine URL-Codierung des Schlüssels vorgenommen, erhalten Sie ggf. eine Ausnahmemeldung ähnlich der folgenden: `Failed to parse session.save_path`
>
>

Weitere Informationen zur Nutzung von Redis Cache als PHP-Sitzungscache mit dem PhpRedis-Client finden Sie unter [PHP Session handler](https://github.com/phpredis/phpredis#php-session-handler)(PHP-Sitzungshandler).

<a name="cache-ssl"></a>

### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Wann sollte ich den Nicht-SSL-Port für die Verbindungsherstellung mit Redis verwenden?
Der Redis-Server bietet keine native SSL-Unterstützung, Azure Redis Cache hingegen schon. Wenn Sie eine Verbindung mit Azure Redis Cache herstellen und Ihr Client SSL unterstützt (z. B. StackExchange.Redis), dann sollten Sie SSL verwenden.

>[!NOTE]
>Der Nicht-SSL-Port für neue Azure Redis Cache-Instanzen ist standardmäßig deaktiviert. Wenn Ihr Client keine SSL-Unterstützung bietet, müssen Sie den nicht SSL-fähigen Port mithilfe der Anweisungen im Abschnitt [Zugriffsports](cache-configure.md#access-ports) des Artikels [Konfigurieren eines Caches in Azure Redis Cache](cache-configure.md) aktivieren.
>
>

Redis-Tools wie z.B. `redis-cli` funktionieren nicht mit dem SSL-Port, aber Sie können ein Hilfsprogramm wie `stunnel` verwenden, um eine sichere Verbindung zwischen den Tools und dem SSL-Port herzustellen. Anweisungen hierzu finden Sie im Blogbeitrag [Announcing ASP.NET Session State Provider for Redis Preview Release](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) (Ankündigung des ASP.NET-Sitzungsstatusanbieters für Redis-Vorschauversion).

Anweisungen zum Herunterladen der Redis-Tools finden Sie im Abschnitt [Wie führe ich Redis-Befehle aus?](#cache-commands) .

### <a name="what-are-some-production-best-practices"></a>Welche Best Practices gelten für die Produktion?
* [Best Practices für StackExchange.Redis](#stackexchangeredis-best-practices)
* [Konfiguration und Konzepte](#configuration-and-concepts)
* [Leistungstests](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>Best Practices für StackExchange.Redis
* Legen Sie `AbortConnect` auf „false“ fest, und lassen Sie dann den ConnectionMultiplexer automatisch eine neue Verbindung herstellen. [Ausführliche Informationen finden Sie hier](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
* Verwenden Sie den ConnectionMultiplexer wieder – erstellen Sie nicht für jede Anforderung einen neuen ConnectionMultiplexer. Sie sollten das [hier gezeigte](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) `Lazy<ConnectionMultiplexer>`-Muster verwenden.
* Redis funktioniert am besten mit kleineren Werten, deshalb sollten Sie die Aufteilung großer Daten in mehrere Schlüssel erwägen. In [dieser Diskussion zu Redis](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) werden 100KB als groß betrachtet. Lesen Sie [diesen Artikel](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) über ein beispielhaftes Problem, das durch große Werte verursacht werden kann.
* Konfigurieren Sie Ihre [ThreadPool-Einstellungen](#important-details-about-threadpool-growth) , um Timeouts zu verhindern.
* Verwenden Sie mindestens den Standardwert von 5 Sekunden für connectTimeout. Dieses Intervall gibt StackExchange.Redis bei einer Netzwerkunterbrechung genügend Zeit zur erneuten Verbindungsherstellung.
* Berücksichtigen Sie die Leistungskosten für andere Vorgänge, die Sie ausführen. Beispielsweise ist der `KEYS` -Befehl ein O(n)-Vorgang und sollte vermieden werden. Die [redis.io-Website](http://redis.io/commands/) umfasst Details zur Zeitkomplexität für jeden unterstützten Vorgang. Klicken Sie auf jeden Befehl, um die Komplexität für jeden Vorgang anzuzeigen.

#### <a name="configuration-and-concepts"></a>Konfiguration und Konzepte
* Verwenden Sie den Standard- oder Premium-Tarif für Produktionssysteme. Der Basic-Tarif ist ein System mit einem einzelnen Knoten, ohne Datenreplikation und ohne SLA. Verwenden Sie mindestens einen C1-Cache. C0-Caches werden in der Regel für einfache Entwicklungs-/Testszenarien verwendet.
* Beachten Sie, dass Redis ein **In-Memory** -Datenspeicher ist. Lesen Sie [diesen Artikel](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) , um zu erfahren, in welchen Szenarien es zu einem Datenverlust kommen kann.
* Entwickeln Sie Ihr System so, dass es Verbindungsunterbrechungen [aufgrund von Patching- und Failovervorgängen](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md)behandeln kann.

#### <a name="performance-testing"></a>Leistungstests
* Starten Sie mit der Verwendung von `redis-benchmark.exe` , um den möglichen Durchsatz einschätzen zu können, bevor Sie Ihre eigenen Leistungstests schreiben. Weil `redis-benchmark` nicht SSL unterstützt, müssen Sie [den Nicht-SSL-Port über das Azure-Portal aktivieren](cache-configure.md#access-ports), bevor Sie den Test ausführen. Beispiele finden Sie unter [Wie kann ich die Leistung meines Caches messen und testen?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* Die für den Test verwendete Client-VM sollte sich in derselben Region befinden wie Ihre Redis-Cache-Instanz.
* Es wird empfohlen, die Dv2-VM-Serie für Ihren Client zu verwenden, da sie über bessere Hardware verfügt und die besten Ergebnisse liefern dürfte.
* Stellen Sie sicher, dass die von Ihnen ausgewählte Client-VM mindestens über die gleiche Computing- und Bandbreitenkapazität verfügt wie der getestete Cache.
* Aktivieren Sie VRSS auf dem Clientcomputer, wenn Sie unter Windows arbeiten. [Ausführliche Informationen finden Sie hier](https://technet.microsoft.com/library/dn383582.aspx).
* Redis-Instanzen im Premium-Tarif verfügen über eine bessere Netzwerklatenz und einen höheren Durchsatz, weil sowohl die CPU als auch das Netzwerk auf besserer Hardware ausgeführt werden.

<a name="cache-redis-commands"></a>

### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Was muss bei der Verwendung gängiger Redis-Befehle beachtet werden?
* Sie sollten bestimmte Redis-Befehle mit langer Ausführungszeit nur dann einsetzen, wenn Sie deren Auswirkung verstehen.
  * Führen Sie beispielsweise den Befehl [KEYS](http://redis.io/commands/keys) nicht in der Produktion aus, da die Rückgabe in Abhängigkeit von der Anzahl von Schlüsseln sehr viel Zeit in Anspruch nehmen kann. Redis ist ein Server mit Single-Threading, d. h. Befehle werden nacheinander verarbeitet. Wenn Sie nach KEYS weitere Befehle ausgeben, werden diese erst ausgeführt, wenn Redis den KEYS-Befehl verarbeitet hat. Die [redis.io-Website](http://redis.io/commands/) umfasst Details zur Zeitkomplexität für jeden unterstützten Vorgang. Klicken Sie auf jeden Befehl, um die Komplexität für jeden Vorgang anzuzeigen.
* Schlüsselgrößen – sollte ich kleine Schlüssel/Werte oder große Schlüssel/Werte verwenden? Dies hängt im Allgemeinen vom Szenario ab. Wenn Ihre Szenarios größere Schlüssel erfordern, können Sie die ConnectionTimeout- und retry-Werte sowie die Logik für erneute Verbindungsversuche anpassen. In Bezug auf den Redis-Server führen kleinere Werte zu einer besseren Leistung.
* Diese Überlegungen bedeuten jedoch nicht, dass Sie in Redis keine größeren Werte speichern können – Sie müssen sich lediglich der folgenden Punkte bewusst sein. Latenzen sind höher. Wenn Sie über einen größeren und einen kleineren Datensatz verfügen, können Sie mehrere ConnectionMultiplexer-Instanzen verwenden und jede mit einem anderen Satz an Werten für Timeout und erneute Verbindungsherstellung konfigurieren – wie im vorangegangenen Abschnitt [Was bewirken die StackExchange.Redis-Konfigurationsoptionen?](#cache-configuration) beschrieben.

<a name="cache-benchmarking"></a>

### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Wie kann ich die Leistung meines Caches messen und testen?
* [Aktivieren Sie die Cachediagnose](cache-how-to-monitor.md#enable-cache-diagnostics), damit Sie die Integrität Ihres Caches [überwachen](cache-how-to-monitor.md) können. Sie können die Metriken im Azure-Portal anzeigen und anschließend mit einem Tool Ihrer Wahl [herunterladen und prüfen](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) .
* Mit "redis-benchmark.exe" können Sie Auslastungstests für Ihren Redis-Server durchführen.
* Stellen Sie sicher, dass sich Client und Redis Cache für die Auslastungstests in derselben Region befinden.
* Verwenden Sie "redis-cli.exe", und überwachen Sie den Cache unter Verwendung des INFO-Befehls.
* Wenn Ihre Datenlast zu einer hohen Arbeitsspeicherfragmentierung führt, sollte Sie eine zentrale Skalierung auf einen größeren Cache durchführen.
* Anweisungen zum Herunterladen der Redis-Tools finden Sie im Abschnitt [Wie führe ich Redis-Befehle aus?](#cache-commands) .

Die folgenden Befehle sind ein Beispiel für die Verwendung von „redis-benchmark.exe“. Führen Sie diese Befehle von einem virtuellen Computer in der gleichen Region wie Ihr Cache aus, um genaue Ergebnisse zu erhalten.

* Test von SET-Anforderungen in der Pipeline mithilfe einer 1-K-Nutzlast

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t SET -n 1000000 -d 1024 -P 50`
* Test von GET-Anforderungen in der Pipeline mithilfe einer 1-K-Nutzlast
  HINWEIS: Führen Sie zunächst den oben gezeigten SET-Test aus, um den Cache zu füllen.

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t GET -n 1000000 -d 1024 -P 50`

<a name="threadpool"></a>

### <a name="important-details-about-threadpool-growth"></a>Wichtige Details zum Threadpool-Wachstum
Der CLR-Threadpool enthält zwei Typen von Threads: Workerthreads und E/A-Abschlussportthreads (oder IOCP-Threads, I/O Completion Port-Threads).

* Workerthreads werden für Vorgänge wie das Verarbeiten der Methoden `Task.Run(…)` oder `ThreadPool.QueueUserWorkItem(…)` verwendet. Diese Threads werden auch von verschiedenen Komponenten in der CLR verwendet, wenn Arbeitsvorgänge in einem Hintergrundthread ausgeführt werden müssen.
* IOCP-Threads werden verwendet, wenn asynchrone E/A-Vorgänge ausgeführt werden (z. B. das Lesen aus dem Netzwerk).

Der Threadpool stellt neue Workerthreads oder E/A-Abschlussthreads nach Bedarf (und ohne Drosselung) bereit, bis die Einstellung für das Minimum des jeweiligen Threadtyps erreicht ist. Standardmäßig entspricht die minimale Anzahl von Threads der Anzahl von Prozessoren in einem System.

Wenn die Anzahl der vorhandenen (ausgelasteten) Threads die „minimale“ Anzahl von Threads erreicht, drosselt der Threadpool die Rate, mit der er neue Threads hinzufügt, auf einen Thread pro 500 Millisekunden. Wenn bei Ihrem System eine Arbeitsspitze eingeht, die einen IOCP-Thread benötigt, werden diese Arbeitsvorgänge in der Regel sehr schnell verarbeitet. Wenn für die Arbeitsspitze jedoch mehr Threads erforderlich sind als in der konfigurierten Einstellung für das Minimum vorgesehen, tritt eine gewisse Verzögerung bei der Verarbeitung einiger Arbeitsvorgänge auf. Der Threadpool wartet darauf, dass zwei Dinge geschehen:

1. Ein vorhandener Thread wird frei, um die Arbeitsvorgänge zu verarbeiten.
2. Es wird 500 Millisekunden lang kein vorhandener Thread frei, und daraufhin wird ein neuer Thread erstellt.

Im Grunde genommen heißt dass: Wenn die Anzahl von ausgelasteten Threads größer ist als die minimale Anzahl von Threads, muss wahrscheinlich eine Verzögerung von 500 ms in Kauf genommen werden, bevor der Netzwerkdatenverkehr von der Anwendung verarbeitet wird. Außerdem gilt es zu beachten, dass ein Thread nach einem längeren Leerlauf als 15 Sekunden (soweit ich mich erinnere) bereinigt wird. Danach kann der Zyklus aus Wachstum und Schrumpfung erneut beginnen.

Betrachten wir eine Beispielfehlermeldung aus StackExchange.Redis (Build 1.0.450 oder höher). Sie sehen, dass nun Threadpool-Statistiken ausgegeben werden (siehe die Details für IOCP und WORKER unten).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive,
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0,
    IOCP: (Busy=6,Free=994,Min=4,Max=1000),
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

Im obigen Beispiel sehen Sie, dass bei den IOCP-Threads sechs Threads ausgelastet sind, und dass das System für ein Minimum von vier Threads konfiguriert ist. In diesem Fall wird der Client wahrscheinlich zwei Verzögerungen von 500 ms hingenommen haben, weil 6 größer als 4 ist.

Beachten Sie, dass für StackExchange.Redis ein Timout eintreten kann, wenn IOCP- oder WORKER-Threads gedrosselt werden.

### <a name="recommendation"></a>Empfehlung
Vor dem Hintergrund dieser Informationen empfehlen wir dringend, als minimalen Wert für IOCP- und WORKER-Threads einen höheren Wert als den Standardwert zu konfigurieren. Wir können keine allgemeingültigen Richtlinien zur Größe dieses Werts geben, denn der richtige Wert für eine Anwendung kann für eine andere Anwendung zu hoch oder zu niedrig sein. Diese Einstellung kann sich auch auf die Leistung von anderen Teilen komplexer Anwendungen auswirken. Daher muss jeder Kunde diese Einstellung gemäß seinen eigenen Anforderungen optimieren. Ein guter Ausgangspunkt sind 200 oder 300 Threads. Testen Sie diese Einstellung, und passen Sie sie nach Bedarf an.

So konfigurieren Sie diese Einstellung:

* Verwenden Sie in ASP.NET die [Konfigurationseinstellung „minIoThreads“]["minIoThreads" configuration setting] unter dem Konfigurationselement `<processModel>` in „web.config“. Wenn die Ausführung in Azure Websites stattfindet, steht diese Einstellung nicht in den Konfigurationsoptionen zur Verfügung. Sie sollten dennoch in der Lage sein, diese Einstellung programmgesteuert über die „Application_Start“-Methode in „global.asax.cs“ zu konfigurieren (siehe unten).

  > [!NOTE] 
  > Der in diesem Konfigurationselement angegebene Wert ist eine Einstellung *pro Kern*. Wenn Ihr Computer z.B. über vier Kerne verfügt und Sie eine minIOThreads-Einstellung von 200 zur Laufzeit festlegen möchten, müssen Sie `<processModel minIoThreads="50"/>` verwenden.
  >

* Außerhalb von ASP.NET verwenden Sie die [ThreadPool.SetMinThreads(…)](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) -API.

<a name="server-gc"></a>

### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Aktivieren der Garbage Collection auf dem Server-, um bei Verwenden von „StackExchange.Redis“ mehr Durchsatz auf dem Client zu erzielen
Das Aktivieren der Garbage Collection auf dem Server kann den Client optimieren und bei Verwenden von „StackExchange.Redis“ mehr Leistung und Durchsatz ermöglichen. Weitere Informationen zur Garbage Collection auf dem Server und ihrer Aktivierung finden Sie in den folgenden Artikeln:

* [So aktivieren Sie die Garbage Collection auf dem Server](https://msdn.microsoft.com/library/ms229357.aspx)
* [Grundlagen der Garbage Collection](https://msdn.microsoft.com/library/ee787088.aspx)
* [Garbage Collection und Leistung](https://msdn.microsoft.com/library/ee851764.aspx)

<a name="cache-monitor"></a>

### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Wie überwache ich die Integrität und Leistung meines Caches?
Microsoft Azure Redis Cache-Instanzen können im [Azure-Portal](https://portal.azure.com)überwacht werden. Sie können Metriken anzeigen, Metrikdiagramme an das Startmenü anheften, Daten- und Zeitbereiche für Überwachungsdiagramme anpassen, Metriken aus Diagrammen hinzufügen und entfernen sowie Warnungen festlegen, die ausgelöst werden, wenn bestimmte Bedingungen erfüllt sind. Weitere Informationen finden Sie unter [Überwachen von Azure Redis Cache](cache-how-to-monitor.md).

Das **Ressourcenmenü** von Redis Cache enthält auch mehrere Tools zur Überwachung und Problembehandlung für Ihre Caches.

* **Diagnose und Problembehandlung** bietet Informationen zu häufigen Problemen sowie Strategien zu deren Behebung.
* **Ressourcenintegrität** dienen zum Überwachen Ihrer Ressource und informieren Sie darüber, ob sie wie erwartet ausgeführt wird. Weitere Informationen zum Azure Resource Health-Dienst finden Sie in der [Übersicht über Azure Resource Health](../resource-health/resource-health-overview.md).
* **Neue Supportanfrage** bietet Optionen, um eine Supportanfrage für Ihren Cache zu öffnen.

Diese Tools ermöglichen es Ihnen, die Integrität Ihrer Azure Redis Cache-Instanzen zu überwachen und unterstützen Sie beim Verwalten Ihrer Cachinganwendungen. Weitere Informationen finden Sie im Abschnitt „Einstellungen zu Support und Problembehandlung“ des Artikels [Gewusst wie: Konfigurieren von Azure Redis Cache](cache-configure.md).

### <a name="my-cache-diagnostics-storage-account-settings-changed-what-happened"></a>Die Einstellungen meines Speicherkontos für die Cachediagnose haben sich geändert. Was ist passiert?
Für Caches innerhalb der gleichen Region und des gleichen Abonnements gelten die gleichen Diagnosespeichereinstellungen. Wenn die Konfiguration geändert wird (durch Aktivieren/Deaktivieren der Diagnose oder Ändern des Speicherkontos), gilt diese Änderung für alle Caches dieses Abonnements in dieser Region. Wenn sich die Diagnoseeinstellungen für Ihren Cache geändert haben, überprüfen Sie, ob sich die Diagnoseeinstellungen für einen anderen Cache im selben Abonnement und derselben Region geändert haben. Eine Möglichkeit besteht darin, die Überwachungsprotokolle für Ihren Cache auf ein `Write DiagnosticSettings`-Ereignis zu überprüfen. Weitere Informationen zum Arbeiten mit Überwachungsprotokollen finden Sie unter [Anzeigen von Ereignissen und Überwachungsprotokollen](../monitoring-and-diagnostics/insights-debugging-with-events.md) und [Überwachen von Vorgängen mit Resource Manager](../azure-resource-manager/resource-group-audit.md). Weitere Informationen zum Überwachen von Azure Redis Cache-Ereignissen finden Sie unter [Vorgänge und Warnungen](cache-how-to-monitor.md#operations-and-alerts).

### <a name="why-are-diagnostics-enabled-for-some-new-caches-but-not-others"></a>Warum ist die Diagnose für einige neue Caches aktiviert, für andere dagegen nicht?
Caches in derselben Region und im selben Abonnement haben die gleichen Diagnosespeichereinstellungen. Wenn Sie einen neuen Cache in derselben Region und im selben Abonnement wie einen anderen Cache erstellen, für den die Diagnose aktiviert ist, wird die Diagnose für den neuen Cache mit denselben Einstellungen aktiviert.

<a name="cache-timeouts"></a>

### <a name="why-am-i-seeing-timeouts"></a>Warum kommt es zu Timeouts?
Timeouts treten auf dem Client auf, der mit Redis kommuniziert. Wenn ein Befehl an den Redis-Server gesendet wird, wird dieser in die Warteschlange eingereiht, bis der Redis-Server den Befehl auswählt und ausführt. Auf dem Client kann es bei diesem Vorgang zu einem Timeout kommen. In diesem Fall wird auf der aufrufenden Seite eine Ausnahme ausgelöst. Weitere Informationen zur Behandlung von Timeoutproblemen finden Sie unter [Behandeln von clientseitigen Problemen](cache-how-to-troubleshoot.md#client-side-troubleshooting) und [StackExchange.Redis-Timeoutausnahmen](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).

<a name="cache-disconnect"></a>

### <a name="why-was-my-client-disconnected-from-the-cache"></a>Warum wurde mein Client vom Cache getrennt?
Nachfolgend werden einige Gründe dafür aufgeführt, warum ein Cache getrennt wird.

* Gründe auf Clientseite
  * Die Clientanwendung wurde neu bereitgestellt.
  * Die Clientanwendung hat eine Skalierung ausgeführt.
    * Bei Cloud Services oder Web-Apps kann dies aufgrund einer automatischen Skalierung der Fall sein.
  * Die Netzwerkschicht auf Clientseite wurde geändert.
  * Vorübergehende Fehler auf dem Client oder in den Netzwerkknoten zwischen Client und Server.
  * Die Bandbreitenschwellenwerte wurden erreicht.
  * Die Ausführung CPU-bezogener Vorgänge hat zu viel Zeit in Anspruch genommen.
* Gründe auf Serverseite
  * Im Standard-Cacheangebot initiiert der Azure Redis Cache-Dienst ein Failover vom primären Knoten auf den sekundären Knoten.
  * Azure hat ein Patching der Instanz durchgeführt, auf dem der Cache bereitgestellt wurde.
    * Dies kann bei Redis-Serverupdates oder im Rahmen der allgemeinen VM-Wartung der Fall sein.

### <a name="which-azure-cache-offering-is-right-for-me"></a>Welches Azure-Cache-Angebot ist das Richtige für mich?
> [!IMPORTANT]
> Gemäß einer [Ankündigung](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/) im letzten Jahr wurden die Dienste Azure Managed Cache Service und Azure In-Role Cache am 30. November 2016 **deaktiviert**. Es wird empfohlen, [Azure Redis Cache](https://azure.microsoft.com/services/cache/) zu verwenden. Informationen zur Migration finden Sie unter [Migrieren von Managed Cache Service zu Azure Redis Cache](cache-migrate-to-redis.md).
>
>

### <a name="azure-redis-cache"></a>Azure Redis Cache
Azure Redis Cache ist allgemein verfügbar in Größen bis zu 53 GB mit einer Verfügbarkeits-SLA von 99,9 %. Der neue [Premium-Tarif](cache-premium-tier-intro.md) bietet Größen bis zu 530 GB und unterstützt Clustering, VNET und Persistenz mit einer SLA von 99,9 %.

Azure Redis Cache ermöglicht den Kunden die Verwendung eines gesicherten, dedizierten Redis Cache, der von Microsoft verwaltet wird. Mit diesem Angebot können Sie die umfangreichen Features und die Umgebung von Redis nutzen und dabei von zuverlässigem Hosting und Überwachung durch Microsoft profitieren.

Im Gegensatz zu traditionellen Caches, die nur mit Schlüssel-Wert-Paaren arbeiten, wird Redis besonders für seine äußerst leistungsfähigen Datentypen geschätzt. Redis unterstützt außerdem die Ausführung atomarer Vorgänge für diese Typen, z. B. das Anhängen an eine Zeichenfolge, das Erhöhen des Werts in einem Hash, das Einfügen per Push in eine Liste, das Berechnen der Schnittmenge, der Gesamtmenge und der Differenz oder das Abrufen des Elements mit der höchsten Rangfolge in einem sortierten Satz. Weitere Funktionen sind unter anderem die Unterstützung von Transaktionen, Pub/Sub, Lua-Skripts, Schlüssel mit begrenzter Lebensdauer und Konfigurationseinstellungen, mit denen sich Redis wie ein herkömmlicher Cache verhält.

Ein weiterer wichtiger Aspekt für den Erfolg von Redis ist das gesunde, lebendige Open Source-Ökosystem. Dies spiegelt sich in den verschiedenen verfügbaren Redis-Clients in mehreren Sprachen wider. Dieses Ökosystem und eine breite Palette von Clients ermöglichen, dass Azure Redis Cache von nahezu allen Workloads verwendet werden kann, die Sie in Azure erstellen können.

Weitere Informationen zu den ersten Schritten mit Azure Redis Cache finden Sie unter [Verwenden von Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md) und in der [Azure Redis Cache-Dokumentation](index.md).

### <a name="managed-cache-service"></a>Managed Cache Service
[Managed Cache Service wurde am 30. November 2016 außer Betrieb gesetzt.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

### <a name="in-role-cache"></a>In-Role Cache
[In-Role Cache wurde am 30. November 2016 außer Betrieb gesetzt.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

["minIoThreads" configuration setting]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx

