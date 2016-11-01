<properties 
	pageTitle="Problembehandlung für Azure Redis Cache | Microsoft Azure" 
	description="Erfahren Sie, wie Sie häufige Probleme bei Azure Redis Cache beheben." 
	services="redis-cache" 
	documentationCenter="" 
	authors="steved0x" 
	manager="douge" 
	editor=""/>

<tags 
	ms.service="cache" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="cache-redis" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/09/2016" 
	ms.author="sdanie"/>

# Problembehandlung für Azure Redis Cache

Dieser Artikel enthält Hinweise zur Behandlung der folgenden Kategorien von Azure Redis Cache-Problemen.

-	[Behandeln von clientseitigen Problemen](#client-side-troubleshooting): Dieser Abschnitt enthält Richtlinien zum Identifizieren und Beheben von Problemen, deren Ursache bei der mit Azure Redis Cache verbundenen Anwendung liegt.
-	[Behandeln von serverseitigen Problemen](#server-side-troubleshooting): Dieser Abschnitt enthält Richtlinien zum Identifizieren und Beheben von Problemen, deren Ursache auf der Serverseite von Azure Redis Cache liegt.
-	[StackExchange.Redis-Timeoutausnahmen](#stackexchangeredis-timeout-exceptions): Dieser Abschnitt enthält Informationen zur Behebung von Problemen bei Verwendung des StackExchange.Redis-Clients.


>[AZURE.NOTE] Etliche Problembehandlungsschritte in diesem Leitfaden enthalten Anleitungen zum Ausführen von Redis-Befehlen und zum Überwachen verschiedener Leistungsmetriken. Weitere Informationen und Anleitungen finden Sie in den Artikeln im Abschnitt [Weitere Informationen](#additional-information).

## Behandeln von clientseitigen Problemen


In diesem Abschnitt wird das Behandeln von Problemen beschrieben, die wegen eines Zustands in der Clientanwendung auftreten.

-	[Hohe Speicherauslastung auf dem Client](#memory-pressure-on-the-client)
-	[Datenverkehrsspitzen](#burst-of-traffic)
-	[Hohe Auslastung der Client-CPU](#high-client-cpu-usage)
-	[Bandbreitenüberschreitung auf der Clientseite](#client-side-bandwidth-exceeded)
-	[Große Anforderungen/große Antworten](#large-requestresponse-size)
-	[Was ist mit meinen Daten in Redis passiert?](#what-happened-to-my-data-in-redis)

### Hohe Speicherauslastung auf dem Client

#### Problem

Wenn der Arbeitsspeicher auf dem Clientcomputer sehr stark ausgelastet ist, führt dies zu Leistungsproblemen aller Art und unter Umständen auch zu einer verzögerten Verarbeitung der von der Redis-Instanz gesendeten Daten. Bei vollkommen ausgelastetem Speicher lagert das System in der Regel Daten aus dem physischen Speicher in den virtuellen Speicher aus, also auf den Datenträger. Diese sogenannten *Seitenfehler* bewirken eine deutliche Verlangsamung des Systems.

#### Messung 

1.	Überwachen Sie die Speicherauslastung auf dem Computer und stellen Sie sicher, dass sie den verfügbaren Speicher nicht überschreitet.
2.	Überwachen Sie den Leistungsindikator `Page Faults/Sec`. Bei den meisten Systemen treten selbst im normalen Betrieb einige Seitenfehler auf. Achten Sie deshalb bei diesem Leistungsindikator für Seitenfehler besonders auf die Spitzen, denn diese entsprechen Timeouts.

#### Lösung

Verwenden Sie für Ihren Client einen größeren virtuellen Computer mit mehr Arbeitsspeicher, oder analysieren Sie die Muster bei der Speichernutzung, um die Auslastung zu verringern.


### Datenverkehrsspitzen

#### Problem

Plötzliche Anstiege des Datenverkehrsvolumens in Verbindung mit unzureichenden `ThreadPool`-Einstellungen können zu Verzögerungen beim Verarbeiten der vom Redis-Server bereits gesendeten, aber auf der Clientseite noch nicht genutzten Daten führen.

#### Messung 

Überwachen Sie die Änderungen bei den `ThreadPool`-Statistiken im zeitlichen Verlauf. Verwenden Sie dazu Code [wie diesen](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Sie können sich auch die `TimeoutException`-Meldung von StackExchange.Redis ansehen. Beispiel:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

In der oben aufgeführten Meldung zeigen sich mehrere interessante Probleme:

 1. Beachten Sie, dass in den Abschnitten `IOCP` und `WORKER` der `Busy`-Wert größer als der `Min`-Wert ist. Das bedeutet, dass die `ThreadPool`-Einstellung angepasst werden muss.
 2. Außerdem wird `in: 64221` angegeben. Das bedeutet, dass 64211 Bytes auf der Kernelsocketebene empfangen wurden, aber noch nicht durch die Anwendung (z.B. StackExchange.Redis) gelesen wurden. In der Regel heißt das, dass Ihre Anwendung die Daten aus dem Netzwerk nicht so schnell liest, wie sie vom Server gesendet werden.

#### Lösung

Konfigurieren Sie Ihre [ThreadPool-Einstellungen](https://gist.github.com/JonCole/e65411214030f0d823cb) so, dass Ihr Threadpool bei Datenverkehrsspitzen schnell hochskaliert wird.


### Hohe Auslastung der Client-CPU

#### Problem

Eine hohe CPU-Auslastung auf dem Client weist darauf hin, dass das System das von ihm geforderte Arbeitsaufkommen nicht mehr bewältigen kann. Das bedeutet, dass der Client eine Antwort von Redis u.U. nicht mehr rechtzeitig verarbeiten kann, obwohl Redis die Antwort sehr schnell gesendet hat.

#### Messung

Überwachen Sie die systemweite CPU-Auslastung im Azure-Portal oder mithilfe des entsprechenden Leistungsindikators. Achten Sie darauf, dass Sie nicht die *Prozess*-CPU überwachen. Für einen einzelnen Prozess kann die CPU-Auslastung gering sein, während gleichzeitig die Gesamtauslastung der System-CPU hoch ist. Beobachten Sie die Spitzen bei der CPU-Auslastung, denn sie korrespondieren mit Timeouts. Als Folge einer hohen CPU-Auslastung sehen Sie in den `TimeoutException`-Fehlermeldungen möglicherweise auch hohe Werte für `in: XXX`, wie in [Datenverkehrsspitzen](#burst-of-traffic) beschrieben.

>[AZURE.NOTE] Ab StackExchange.Redis 1.1.603 ist in den `TimeoutException`-Fehlermeldungen die Metrik `local-cpu` enthalten. Verwenden Sie immer die neueste Version des [NuGet-Pakets für StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/). Es werden ständig Fehler im Code behoben, um ihn widerstandsfähiger gegen Timeouts zu machen. Das Nutzen der neuesten Version ist also wichtig.

#### Lösung

Führen Sie ein Upgrade auf einen größeren virtuellen Computer mit mehr CPU-Kapazität durch, oder untersuchen Sie, was die CPU-Spitzen verursacht.



### Bandbreitenüberschreitung auf der Clientseite

#### Problem

Für unterschiedliche Clientcomputergrößen bestehen jeweils eigene Einschränkungen im Hinblick auf die verfügbare Netzwerkbandbreite. Wenn bei Clients die verfügbare Bandbreite überschritten wird, werden die Daten auf der Clientseite nicht so schnell verarbeitet, wie sie vom Server gesendet werden. Dies kann zu Timeouts führen.

#### Messung

Überwachen Sie die Änderungen bei der Bandbreitennutzung im zeitlichen Verlauf. Verwenden Sie dazu Code [wie diesen](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Beachten Sie, dass dieser Code in einigen Umgebungen mit eingeschränkten Berechtigungen (z.B. Azure-Websites) u.U. nicht erfolgreich ausgeführt werden kann.

#### Lösung 

Vergrößern Sie den virtuellen Computer für den Client, oder verringern Sie die in Anspruch genommene Netzwerkbandbreite.


### Große Anforderungen/große Antworten

#### Problem

Eine große Anforderung oder eine große Antwort kann Timeouts verursachen. Nehmen Sie beispielsweise an, dass auf Ihrem Client ein Timeoutwert von einer Sekunde konfiguriert ist. Ihre Anwendung fordert zwei Schlüssel gleichzeitig an (z.B. „A“ und „B“) und verwendet dafür die gleiche physische Netzwerkverbindung. Die meisten Clients unterstützen das „Pipelining“ für solche Anforderungen. Das heißt: Die beiden Anforderungen „A“ und „B“ werden nacheinander übertragen, ohne dass auf Antworten gewartet wird. Der Server sendet die Antworten in der gleichen Reihenfolge zurück. Wenn die Antwort „A“ groß genug ist, kann sie den Großteil der Timeoutzeit für die folgenden Anforderungen in Anspruch nehmen.

Das folgende Beispiel veranschaulicht dieses Szenario. In diesem Szenario werden die Anforderungen „A“ und „B“ schnell gesendet, und der Server beginnt schnell mit dem Senden der Antworten „A“ und „B“. Infolge der Datenübertragungszeiten bleibt „B“ jedoch hinter der anderen Anforderung mit Timeout stecken, obwohl der Server schnell geantwortet hat.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### Messung

Diese Vorgänge lassen sich schwer messen. Im Grunde müssen Sie Ihren Clientcode für die Nachverfolgung von großen Anfragen und Antworten instrumentieren.

#### Lösung

1.	Redis ist für eine große Anzahl von kleinen Werten optimiert, nicht für wenige große Werte. Die bevorzugte Lösung besteht darin, Ihre Daten in verknüpfte kleinere Werte aufzuteilen. Ausführliche Informationen zu Gründen, warum kleinere Werte empfohlen werden, finden Sie im Beitrag [What is the ideal value size range for redis? Is 100KB too large?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) (Welche Größe ist für Werte bei Redis ideal? Sind 100 KB zu viel?).
2.	Vergrößern Sie Ihren virtuellen Computer (für den Client und den Redis Cache-Server), um höhere Bandbreitenkapazitäten zu erhalten und dadurch die Datenübertragungszeiten für größere Antworten zu verkürzen. Beachten Sie, dass es möglicherweise nicht ausreicht, nur für den Server oder nur für den Client mehr Bandbreite zu erhalten. Messen Sie die Bandbreitennutzung, und vergleichen Sie sie mit der Kapazität, die der Größe des gegenwärtig verwendeten virtuellen Computers zugeordnet ist.
3.	Erhöhen Sie die Anzahl der verwendeten `ConnectionMultiplexer`-Objekte und Roundrobinanforderungen über verschiedene Verbindungen.


### Was ist mit meinen Daten in Redis passiert?

#### Problem

Ich habe erwartet, dass sich bestimmte Daten in meiner Azure Redis Cache-Instanz befinden, aber sie sind scheinbar nicht dort.

##### Lösung

Mögliche Ursachen und Lösungen finden Sie unter [What happened to my data in Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) (Was ist mit meinen Daten in Redis passiert?)


## Behandeln von serverseitigen Problemen

In diesem Abschnitt wird das Behandeln von Problemen beschrieben, die wegen eines Zustands auf dem Cacheserver auftreten.

-	[Hohe Speicherauslastung auf dem Server](#memory-pressure-on-the-server)
-	[Hohe CPU-Auslastung/hohe Serverauslastung](#high-cpu-usage-server-load)
-	[Bandbreitenüberschreitung auf der Serverseite](#server-side-bandwidth-exceeded)

### Hohe Speicherauslastung auf dem Server

#### Problem

Wenn der Arbeitsspeicher auf der Serverseite sehr stark ausgelastet ist, führt dies zu Leistungsproblemen aller Art und unter Umständen auch zu einer verzögerten Verarbeitung von Anforderungen. Bei vollkommen ausgelastetem Speicher lagert das System in der Regel Daten aus dem physischen Speicher in den virtuellen Speicher aus, also auf den Datenträger. Diese sogenannten *Seitenfehler* bewirken eine deutliche Verlangsamung des Systems. Eine zu hohe Speicherauslastung kann verschiedene Ursachen haben:

1.	Sie haben den Cache bis zur Kapazitätsgrenze mit Daten gefüllt.
2.	Der Speicher in Redis ist stark fragmentiert. Dies wird meist durch das Speichern großer Objekte verursacht (Redis ist für kleine Objekte optimiert – siehe [What is the ideal value size range for redis? Is 100KB too large?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) (Welche Größe ist für Werte bei Redis ideal? Sind 100 KB zu viel?)).

#### Messung

Redis macht zwei Metriken verfügbar, mit deren Hilfe Sie das Problem identifizieren können. Bei der ersten handelt es sich um `used_memory` und bei der zweiten um `used_memory_rss`. [Diese Metriken](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) können im Azure-Portal oder über den Befehl [Redis INFO](http://redis.io/commands/info) genutzt werden.

#### Lösung

Sie können mehrere Änderungen vornehmen, um die Speicherauslastung in einem gesunden Rahmen zu halten:

1. [Konfigurieren Sie eine Speicherrichtlinie](cache-configure.md#maxmemory-policy-and-maxmemory-reserved), und legen Sie Ablauffristen für Ihre Schlüssel fest. Wenn Fragmentierung vorliegt, reicht dies möglicherweise nicht aus.
2. [Konfigurieren Sie einen Wert für „maxmemory-reserved“](cache-configure.md#maxmemory-policy-and-maxmemory-reserved), der groß genug ist, um die Speicherfragmentierung zu kompensieren.
3. Teilen Sie große zwischengespeicherte Objekte in kleinere verknüpfte Objekte auf.
4. [Skalieren](cache-how-to-scale.md) Sie auf einen größeren Cache.
5. Wenn Sie einen [Premium-Cache mit aktiviertem Redis-Cluster](cache-how-to-premium-clustering.md) verwenden, können Sie [die Anzahl von Shards erhöhen](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### Hohe CPU-Auslastung/hohe Serverauslastung

#### Problem

Eine hohe CPU-Auslastung kann bedeuten, dass die Clientseite eine Antwort von Redis u.U. nicht mehr rechtzeitig verarbeiten kann, obwohl Redis die Antwort sehr schnell gesendet hat.

#### Messung

Überwachen Sie die systemweite CPU-Auslastung im Azure-Portal oder mithilfe des entsprechenden Leistungsindikators. Achten Sie darauf, dass Sie nicht die *Prozess*-CPU überwachen. Für einen einzelnen Prozess kann die CPU-Auslastung gering sein, während gleichzeitig die Gesamtauslastung der System-CPU hoch ist. Beobachten Sie die Spitzen bei der CPU-Auslastung, denn sie korrespondieren mit Timeouts.

#### Lösung

[Skalieren](cache-how-to-scale.md) Sie auf einen höheren Cachetarif mit mehr CPU-Kapazität, oder untersuchen Sie, was die CPU-Spitzen verursacht.

### Bandbreitenüberschreitung auf der Serverseite

#### Problem

Für unterschiedliche große Cacheinstanzen bestehen jeweils eigene Einschränkungen im Hinblick auf die verfügbare Netzwerkbandbreite. Wenn der Server die verfügbare Bandbreite überschreitet, werden die Daten nicht so schnell an den Client gesendet wie erwartet. Dies kann zu Timeouts führen.

#### Messung

Sie können die Metrik `Cache Read` überwachen. Dies ist die Menge an Daten in Megabyte pro Sekunde (MB/s), die während des angegebenen Berichtsintervalls aus dem Cache gelesen wurde. Dieser Wert entspricht der von diesem Cache verwendeten Netzwerkbandbreite. Wenn Sie Warnungen für serverseitige Grenzwerte bei der Netzwerkbandbreite einrichten möchten, können Sie diese mithilfe des Leistungsindikators `Cache Read` erstellen. Vergleichen Sie die gemessenen Werte mit den Werten in [dieser Tabelle](cache-faq.md#cache-performance), in der die beobachteten Bandbreitengrenzwerte für verschiedene Cachetarife und -größen aufgelistet sind.

#### Lösung

Wenn bei Ihnen die Bandbreite regelmäßig in der Nähe der beobachteten maximalen Bandbreite für Ihren Tarif und Ihre Cachegröße liegt, sollten Sie eine [Skalierung](cache-how-to-scale.md) auf einen Tarif oder eine Größe mit mehr Netzwerkbandbreite ins Auge fassen. Nutzen Sie die Werte in [dieser Tabelle](cache-faq.md#cache-performance) als Richtschnur.


## StackExchange.Redis-Timeoutausnahmen

StackExchange.Redis verwendet für synchrone Vorgänge eine Konfigurationseinstellung mit dem Namen `synctimeout`. Der Standardwert für diese Einstellung ist 1000 ms. Wenn ein synchroner Aufruf nicht in der vorgesehenen Zeit abgeschlossen wurde, löst der StackExchange.Redis-Client einen Timeoutfehler ähnlich dem Folgenden aus.

	System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Diese Fehlermeldung enthält Metriken, mit deren Hilfe Sie die Ursache und die mögliche Lösung des Problems ermitteln können. In der folgenden Tabelle finden Sie Details zu den Metriken in der Fehlermeldung.

| Metrik in der Fehlermeldung | Details |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inst | In der letzten Zeitscheibe: 0 Befehle wurden ausgegeben. |
| mgr | Der Socket-Manager führt `socket.select` aus. Das heißt, er fordert das Betriebssystem auf, einen beschäftigten Socket anzugeben. Im Grunde: Der Reader liest nicht aktiv aus dem Netzwerk, weil er meint, dass nichts zu tun ist. |
| queue | Es sind insgesamt 73 Vorgänge in Bearbeitung. |
| qu | 6 der in Bearbeitung befindlichen Vorgänge befinden sich in der Warteschlange für „Nicht gesendet“ und wurden noch nicht in das Netzwerk für ausgehenden Datenverkehr geschrieben. |
| qs | 67 der in Bearbeitung befindlichen Vorgänge wurden an den Server gesendet, aber eine Antwort ist noch nicht verfügbar. Als Antwort ist Folgendes möglich: `Not yet sent by the server` oder `sent by the server but not yet processed by the client.`. |
| qc | Für 0 der in Bearbeitung befindlichen Vorgänge liegt eine Antwort vor, aber die Vorgänge wurden noch nicht als abgeschlossen markiert, weil noch auf die Abschlussschleife gewartet wird. |
| wr | Es ist ein aktiver Writer vorhanden (d.h., die 6 noch nicht gesendeten Anforderungen werden nicht ignoriert); Bytes/aktive Writer. |
| in | Es sind keine aktiven Reader vorhanden, und null Bytes sind zum Lesen auf der NIC (Network Interface Card, Netzwerkschnittstellenkarte) vorhanden; Bytes/aktive Reader |


### Untersuchungsschritte

1. Wenn Sie den StackExchange.Redis-Client verwenden, sollten Sie als bewährte Methode zum Herstellen der Verbindung das folgende Muster verwenden.


	    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
	    {
	        return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
	
	    });
	
	    public static ConnectionMultiplexer Connection
	    {
	        get
	        {
	            return lazyConnection.Value;
	        }
	    }


    Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit dem Cache mithilfe von StackExchange.Redis](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

2. Stellen Sie sicher, dass sich Ihr Azure Redis Cache und die Clientanwendung in der gleichen Azure-Region befinden. Wenn Ihr Cache z.B. in der Region „USA, Osten“ und Ihr Client in der Region „USA, Westen“ angesiedelt ist, erhalten Sie möglicherweise Timeouts, und die Anforderung wird nicht innerhalb des `synctimeout`-Intervalls abgeschlossen. Auch beim Debuggen über den lokalen Entwicklungscomputer können Timeouts auftreten.

    Es wird dringend empfohlen, den Cache und den Client in der gleichen Azure-Region anzusiedeln. Wenn in Ihrem Szenario regionsübergreifende Aufrufe vorkommen, sollten Sie das `synctimeout`-Intervall auf einen höheren Wert als die standardmäßigen 1000 ms festlegen. Nehmen Sie zu diesem Zweck eine `synctimeout`-Eigenschaft in die Verbindungszeichenfolge auf. Das folgende Beispiel zeigt einen Codeausschnitt aus einer Verbindungszeichenfolge für einen StackExchange.Redis-Cache, in der ein `synctimeout` von 2000 ms festgelegt ist.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...

3. Verwenden Sie immer die neueste Version des [NuGet-Pakets für StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/). Es werden ständig Fehler im Code behoben, um ihn widerstandsfähiger gegen Timeouts zu machen. Das Nutzen der neuesten Version ist also wichtig.

4. Wenn Anforderungen durch Bandbreiteneinschränkungen auf dem Server oder dem Client behindert werden, dauert deren Ausführung länger, und es können Timeouts auftreten. Um festzustellen, ob ein Timeout durch die verfügbare Netzwerkbandbreite auf dem Server verursacht wird, befolgen Sie die Anweisungen unter [Bandbreitenüberschreitung auf der Serverseite](#server-side-bandwidth-exceeded). Um festzustellen, ob ein Timeout durch die verfügbare Netzwerkbandbreite auf dem Client verursacht wird, befolgen Sie die Anweisungen unter [Bandbreitenüberschreitung auf der Clientseite](#client-side-bandwidth-exceeded).

6. Werden Sie durch die CPU auf dem Server oder auf dem Client behindert?
	-	Überprüfen Sie, ob Sie durch die CPU auf dem Client behindert werden. Wenn dies der Fall ist, werden Anforderungen u.U. nicht innerhalb des `synctimeout`-Intervalls verarbeitet, was zu Timeouts führt. Durch einen Wechsel zu einem größer dimensionierten Client oder durch eine Verteilung der Last können Sie Abhilfe schaffen.
	-	Überprüfen Sie, ob Sie durch die CPU auf dem Server behindert werden. Überwachen Sie dazu die [Cacheleistungsmetrik](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `CPU`. Wenn Anforderungen eingehen, während Redis durch die CPU behindert wird, können für diese Anforderungen Timeouts auftreten. Sie können Abhilfe schaffen, indem Sie die Last auf mehrere Shards verteilen (hierfür benötigen Sie einen Premium-Cache). Alternativ können Sie ein Upgrade zu einer größer dimensionierten CPU durchführen oder in einen höheren Tarif wechseln. Weitere Informationen finden Sie unter [Bandbreitenüberschreitung auf der Serverseite](#server-side-bandwidth-exceeded).

7. Dauert die Verarbeitung von Befehlen auf dem Server lange? Wenn die Verarbeitung und Ausführung von Befehlen auf dem Redis-Server lange dauert, kann dies zu Timeouts führen. Beispiele für Befehle mit langen Ausführungszeiten sind `mget` mit einer großen Anzahl von Schlüsseln, `keys *` oder schlecht geschriebene Lua-Skripts. Sie können mithilfe des „redis-cli“-Clients eine Verbindung mit Ihrer Azure Redis Cache-Instanz herstellen oder die [Redis-Konsole](cache-configure.md#redis-console) verwenden und den Befehl [SlowLog](http://redis.io/commands/slowlog) ausführen, um festzustellen, ob für Anforderungen mehr Zeit als erwartet benötigt wird. Der Redis-Server und StackExchange.Redis sind für viele kleine Anforderungen optimiert, nicht für wenige große. Durch eine Aufteilung Ihrer Daten in kleinere Blöcke können Sie u.U. Verbesserungen erzielen.

    Informationen zum Herstellen einer Verbindung mit dem Azure Redis Cache-SSL-Endpunkt mithilfe von „redis-cli“ und „stunnel“ finden Sie im Blogbeitrag [Announcing ASP.NET Session State Provider for Redis Preview Release](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) (Ankündigung des ASP.NET-Sitzungszustandsanbieters für Redis – Vorschauversion). Weitere Informationen finden Sie unter [SlowLog](http://redis.io/commands/slowlog).

8. Eine hohe Auslastung des Redis-Servers kann zu Timeouts führen. Sie können die Serverauslastung überwachen, indem Sie die [Cacheleistungsmetrik](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Redis Server Load` überwachen. Eine Serverauslastung von 100 (Maximalwert) bedeutet, dass der Redis-Server mit der Verarbeitung von Anforderungen voll ausgelastet war und keine Leerlaufzeiten aufgetreten sind. Um festzustellen, ob bestimmte Anforderungen die gesamte Serverkapazität in Anspruch nehmen, führen Sie den Befehl „SlowLog“ aus, wie im vorherigen Absatz beschrieben. Weitere Informationen finden Sie unter [Hohe CPU-Auslastung/hohe Serverauslastung](#high-cpu-usage-server-load).

9. Gab es ein anderes Ereignis auf der Clientseite, das ein Netzwerkflackern verursacht haben könnte? Überprüfen Sie auf dem Client (virtueller Computer für Web- oder Workerrolle oder IaaS), ob ein Ereignis wie das Hoch- oder Herunterskalieren der Anzahl von Clientinstanzen oder das Bereitstellen einer neuen Version des Clients stattgefunden hat oder ob die automatische Skalierung aktiviert ist. Bei unseren Tests haben wir festgestellt, dass beim automatischen Skalieren oder beim Hoch- oder Herunterskalieren die Verbindung mit dem Netzwerk für ausgehenden Datenverkehr für mehrere Sekunden verloren gehen kann. Der StackExchange.Redis-Code ist unempfindlich gegen solche Ereignisse und stellt die Verbindung anschließend wieder her. Allerdings kann für Anforderungen, die sich bei diesem Wiederherstellen der Verbindung in der Warteschlange befinden, ein Timeout auftreten.

10. Gab es vor mehreren kleineren Anforderungen eine große Anforderung an den Redis-Cache, für die ein Timeout auftrat? Der Parameter `qs` in der Fehlermeldung teilt Ihnen mit, wie viele Anforderungen vom Client an den Server gesendet wurden, für die aber bisher keine Antwort verarbeitet wurde. Dieser Wert kann fortlaufend wachsen, weil StackExchange.Redis eine einzige TCP-Verbindung nutzt und zu einem Zeitpunkt jeweils immer nur eine Antwort lesen kann. Obwohl für den ersten Vorgang ein Timeout aufgetreten ist, können weiterhin Daten an den Server und vom Server gesendet werden. Weitere Anforderungen werden allerdings blockiert, bis die erste vollständig verarbeitet ist, und dies führt zu weiteren Timeouts. Als mögliche Lösung können Sie die Wahrscheinlichkeit von Timeouts verringern. Stellen Sie zu diesem Zweck sicher, dass Ihr Cache für Ihre Workload groß genug ist, und teilen Sie große Werte in kleinere Blöcke auf. Eine weitere Lösung besteht darin, in Ihrem Client einen Pool von `ConnectionMultiplexer`-Objekten zu verwenden und beim Senden einer neuen Anforderung den am wenigsten ausgelasteten `ConnectionMultiplexer` zu verwenden. Dadurch können Sie verhindern, dass ein einzelnes Timeout weitere Timeouts für andere Anforderungen nach sich zieht.

11. Wenn Sie `RedisSessionStateprovider` verwenden, müssen Sie das Timeout für Wiederholungsversuche richtig festlegen. `retrytimeoutInMilliseconds` muss höher als `operationTimeoutinMilliseonds` sein, andernfalls werden keine Wiederholungsversuche vorgenommen. Im folgenden Beispiel ist `retrytimeoutInMilliseconds` auf 3000 festgelegt. Weitere Informationen finden Sie unter [ASP.NET-Sitzungszustandsanbieter für Azure Redis Cache](cache-aspnet-session-state-provider.md) und [How to use the configuration parameters of Session State Provider and Output Cache Provider](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration) (Verwenden der Konfigurationsparameter des Sitzungszustandsanbieters und des Ausgabecacheanbieters).


	<add name="AFRedisCacheSessionStateProvider" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="enbwcache.redis.cache.windows.net" port="6380" accessKey="…" ssl="true" databaseId="0" applicationName="AFRedisCacheSessionState" connectionTimeoutInMilliseconds = "5000" operationTimeoutInMilliseconds = "1000" retryTimeoutInMilliseconds="3000" />


12. Überprüfen Sie die Speicherauslastung auf dem Azure Redis Cache-Server, indem Sie `Used Memory RSS` und `Used Memory` [überwachen](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Wenn eine Entfernungsrichtlinie vorhanden ist und `Used_Memory` die Cachegröße erreicht, beginnt Redis mit dem Entfernen von Schlüsseln. Im Idealfall sollte `Used Memory RSS` nur geringfügig höher als `Used memory` sein. Ein großer Unterschied bedeutet, dass eine (interne oder externe) Speicherfragmentierung vorhanden ist. Wenn `Used Memory RSS` geringer als `Used Memory` ist, bedeutet das, dass ein Teil des Cachespeichers vom Betriebssystem ausgelagert wurde. In diesem Fall müssen Sie mit erheblichen Latenzen rechnen. Da Redis nicht steuern kann, wie seine Belegungen den Speicherseiten zugeordnet werden, ist ein hoher Wert für `Used Memory RSS` oft das Ergebnis einer Speicherauslastungsspitze. Wenn Redis Speicher freigibt, wird dieser Speicher dem Allocator zurückgegeben. Der Allocator kann den Speicher dem System zurückgeben, aber er muss dies nicht tun. Es kann zu einer Diskrepanz zwischen dem Wert für `Used Memory` und der vom Betriebssystem gemeldeten Speichernutzung kommen. Die Ursache dafür liegt möglicherweise darin, dass Speicher von Redis genutzt und freigegeben, aber nicht an das System zurückgegeben wurde. Um Speicherprobleme möglichst gering zu halten, können Sie die folgenden Schritte ausführen.
    -	Führen Sie ein Upgrade auf einen größeren Cache durch, damit in Ihrem System genügend Speicher vorhanden ist und entsprechende Einschränkungen wegfallen.
    -	Legen Sie Ablauffristen für die Schlüssel fest, damit ältere Werte proaktiv entfernt werden.
    -	Überwachen Sie die Cachemetrik `used_memory_rss`. Wenn sich dieser Wert der Größe Ihres Caches nähert, sind Leistungsprobleme absehbar. Falls Sie einen Premium-Cache nutzen, verteilen Sie die Daten über mehrere Shards. Andernfalls führen Sie ein Upgrade auf einen größeren Cache durch.

    Weitere Informationen finden Sie unter [Hohe Speicherauslastung auf dem Server](#memory-pressure-on-the-server).

## Zusätzliche Informationen

-	[Welches Redis Cache-Angebot und welche Redis Cache-Größe sollte ich verwenden?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
-	[Wie kann ich die Leistung meines Caches messen und testen?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-	[Wie führe ich Redis-Befehle aus?](cache-faq.md#how-can-i-run-redis-commands)
-	[Überwachen von Azure Redis Cache](cache-how-to-monitor.md)

<!---HONumber=AcomDC_0810_2016-->