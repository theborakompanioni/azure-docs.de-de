<properties 
	pageTitle="Konfigurieren von Azure Redis Cache | Microsoft Azure"
	description="Grundlagen der Redis-Standardkonfiguration für Azure Redis Cache und Informationen zur Konfiguration Ihrer Azure Redis Cache-Instanzen"
	services="redis-cache"
	documentationCenter="na"
	authors="steved0x"
	manager="douge"
	editor="tysonn" />
<tags 
	ms.service="cache"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="cache-redis"
	ms.workload="tbd"
	ms.date="07/25/2016"
	ms.author="sdanie" />

# Gewusst wie: Konfigurieren von Azure Redis Cache

In diesem Thema wird beschrieben, wie Sie die Konfiguration für Ihre Azure Redis Cache-Instanzen überprüfen und aktualisieren. Außerdem wird die standardmäßige Redis-Serverkonfiguration für Azure Redis Cache-Instanzen beschrieben.

>[AZURE.NOTE] Weitere Informationen zur Konfiguration und Verwendung von Premium-Cache-Features finden Sie unter [Konfigurieren von Persistenz für Azure Redis Cache vom Typ „Premium“](cache-how-to-premium-persistence.md), [Konfigurieren von Clustern für Azure Redis Cache vom Typ „Premium“](cache-how-to-premium-clustering.md) und [Konfigurieren der Unterstützung virtueller Netzwerke für Azure Redis Cache vom Typ „Premium“](cache-how-to-premium-vnet.md).

## Konfigurieren von Redis Cache-Einstellungen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure Redis Cache ermöglicht auf dem Blatt **Einstellungen** die folgenden Einstellungen.

![Redis Cache: Einstellungen](./media/cache-configure/redis-cache-settings.png)



-	[Einstellungen zu Support und Problembehandlung](#support-amp-troubleshooting-settings)
-	[Allgemeine Einstellungen](#general-settings)
	-	[Eigenschaften](#properties)
	-	[Zugriffsschlüssel](#access-keys)
	-	[Zugriffsports](#access-ports)
	-	[Maxmemory-Richtlinie](#maxmemory-policy-and-maxmemory-reserved)
	-	[Erweiterte Einstellungen (Keyspacebenachrichtigungen)](#keyspace-notifications-advanced-settings)
	-	[Redis Cache Advisor](#redis-cache-advisor)
-	[Skalierungseinstellungen](#scale-settings)
	-	[Tarif](#pricing-tier)
	-	[Redis-Clustergröße](#cluster-size)
-	[Datenverwaltungseinstellungen](#data-management-settings)
	-	[Redis-Datenpersistenz](#redis-data-persistence)
	-	[Import/Export](#importexport)
-	[Verwaltungseinstellungen](#administration-settings)
	-	[Neustart](#reboot)
	-	[Planen von Updates](#schedule-updates)
-	[Diagnoseeinstellungen](#diagnostics-settings)
-	[Netzwerkeinstellungen](#network-settings)
-	[Ressourcenverwaltungseinstellungen](#resource-management-settings)

## Einstellungen zu Support und Problembehandlung

Die Einstellungen im Abschnitt **Support und Problembehandlung** bieten Optionen zur Behandlung von Problemen mit dem Cache.

![Support und Problembehandlung](./media/cache-configure/redis-cache-support-troubleshooting.png)

Klicken Sie auf **Problembehandlung**, um gängige Probleme und Strategien zu deren Behebung anzuzeigen.

Klicken Sie auf **Überwachungsprotokolle**, um in Ihrem Cache ausgeführte Aktionen anzuzeigen. Sie können auch filtern, um diese Ansicht zum Einbeziehen anderer Ressourcen zu erweitern. Weitere Informationen zum Arbeiten mit Überwachungsprotokollen finden Sie unter [Anzeigen von Ereignissen und Überwachungsprotokollen](../azure-portal/insights-debugging-with-events.md) und [Überwachen von Vorgängen mit Resource Manager](../resource-group-audit.md). Weitere Informationen zum Überwachen von Azure Redis Cache-Ereignissen finden Sie unter [Vorgänge und Warnungen](cache-how-to-monitor.md#operations-and-alerts).

Die Informationen zur **Ressourcenintegrität** dienen zum Überwachen Ihrer Ressource, d.h. ob sie wie erwartet ausgeführt wird. Weitere Informationen zum Azure Resource Health-Dienst finden Sie in der [Übersicht über Azure Resource Health](../resource-health/resource-health-overview.md).

>[AZURE.NOTE] Die Ressourcenintegrität kann derzeit keine Informationen zur Integrität der in einem virtuellen Netzwerk gehosteten Azure Redis Cache-Instanzen liefern. Weitere Informationen finden Sie unter [Funktionieren alle Cachefeatures beim Hosten eines Cache in einem VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet).

Klicken Sie auf **Neue Supportanfrage**, um eine Supportanfrage für Ihren Cache zu erstellen.

## Allgemeine Einstellungen

Die Einstellungen im Abschnitt **Allgemein** ermöglichen den Zugriff auf die folgenden Cache-Einstellungen und deren Konfiguration:

![Allgemeine Einstellungen](./media/cache-configure/redis-cache-general-settings.png)

-	[Eigenschaften](#properties)
-	[Zugriffsschlüssel](#access-keys)
-	[Zugriffsports](#access-ports)
-	[Maxmemory-Richtlinie](#maxmemory-policy-and-maxmemory-reserved)
-	[Erweiterte Einstellungen (Keyspacebenachrichtigungen)](#keyspace-notifications-advanced-settings)
-	[Redis Cache Advisor](#redis-cache-advisor)

### Eigenschaften

Klicken Sie auf **Eigenschaften**, um Informationen zu Ihrem Cache anzuzeigen, z. B. den Endpunkt und die Ports des Caches.

![Redis Cache: Eigenschaften](./media/cache-configure/redis-cache-properties.png)

### Zugriffsschlüssel

Klicken Sie auf **Zugriffsschlüssel**, um die Zugriffsschlüssel für Ihren Cache anzuzeigen oder neu zu generieren. Diese Schlüssel werden von den Clients, die eine Verbindung mit Ihrem Cache herstellen, zusammen mit dem Hostnamen und den Ports aus dem Blatt **Eigenschaften** verwendet.

![Redis Cache: Zugriffsschlüssel](./media/cache-configure/redis-cache-manage-keys.png)

### Zugriffsports

Der Zugriff ohne SSL ist für neue Caches standardmäßig deaktiviert. Klicken Sie zum Aktivieren des Nicht-SSL-Ports auf das Blatt **Zugriffsports** und dann auf **Nein**.

![Redis Cache: Zugriffsports](./media/cache-configure/redis-cache-access-ports.png)

### maxmemory-policy und maxmemory-reserved

Klicken Sie auf **Maxmemory-Richtlinie**, um die Arbeitsspeicherrichtlinien für den Cache zu konfigurieren. Mit der Einstellung **maxmemory-policy** wird die Entfernungsrichtlinie für den Cache konfiguriert, und mit **maxmemory-reserved** wird der Arbeitsspeicher konfiguriert, der für andere Prozesse als Cacheprozesse reserviert ist.

![Redis Cache: Maxmemory-Richtlinie](./media/cache-configure/redis-cache-maxmemory-policy.png)

Unter **Maxmemory-Richtlinie** können Sie unter den folgenden Entfernungsrichtlinien wählen:

-	volatile-lru (Standardeinstellung)
-	allkeys-lru
-	volatile-random
-	allkeys-random
-	volatile-ttl
-	noeviction

Weitere Informationen zu Maxmemory-Richtlinien finden Sie unter [Entfernungsrichtlinien](http://redis.io/topics/lru-cache#eviction-policies).

Mit der Einstellung **maxmemory-reserved** wird die Arbeitsspeichermenge in MB konfiguriert, die für andere Prozesse als Cacheprozesse reserviert ist, z. B. die Replikation während eines Failovers. Sie kann auch verwendet werden, wenn Sie über ein hohes Fragmentierungsverhältnis verfügen. Mit dem Festlegen dieses Werts können Sie dafür sorgen, dass Sie bei wechselnden Auslastungen eine konsistentere Redis-Servererfahrung erzielen. Dieser Wert sollte für Workloads, die mit hohem Schreibaufwand verbunden sind, höher gewählt werden. Wenn Arbeitsspeicher für Vorgänge dieser Art reserviert ist, ist er nicht für die Speicherung zwischengespeicherter Daten verfügbar.

>[AZURE.IMPORTANT] Die Einstellung **maxmemory-reserved** ist nur für Standard- und Premium-Caches verfügbar.

### Keyspacebenachrichtigungen (Erweiterte Einstellungen)

Klicken Sie auf **Erweiterte Einstellungen**, um Redis-Keyspacebenachrichtigungen zu konfigurieren. Mit Keyspacebenachrichtigungen können Clients Benachrichtigungen empfangen, wenn bestimmte Ereignisse eintreten.

![Redis Cache: Erweiterte Einstellungen](./media/cache-configure/redis-cache-advanced-settings.png)

>[AZURE.IMPORTANT] Keyspacebenachrichtigungen und die Einstellung **notify-keyspace-events** sind nur für Caches vom Typ "Standard" und "Premium" verfügbar.

Weitere Informationen finden Sie unter [Redis-Keyspacebenachrichtigungen](http://redis.io/topics/notifications). Beispielcode finden Sie in der Datei [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) im [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld)-Beispiel.

<a name="recommendations"></a>
## Redis Cache Advisor

Auf dem Blatt **Empfehlungen** finden Sie Empfehlungen für Ihren Cache. Während des normalen Betriebs werden keine Empfehlungen angezeigt.

![Recommendations](./media/cache-configure/redis-cache-no-recommendations.png)

Wenn während der Vorgänge in Ihrem Cache Bedingungen wie hohe Speicherauslastung, Netzwerkbandbreite oder Serverauslastung auftreten, wird auf dem Blatt **Redis Cache** eine Warnung angezeigt.

![Recommendations](./media/cache-configure/redis-cache-recommendations-alert.png)

Weitere Informationen finden Sie auf dem Blatt **Empfehlungen**.

![Empfehlungen](./media/cache-configure/redis-cache-recommendations.png)

Sie können diese Metriken in den Abschnitten [Überwachungsdiagramme](cache-how-to-monitor.md#monitoring-charts) und [Nutzungsdiagramme](cache-how-to-monitor.md#usage-charts) des Blatts **Redis Cache** überwachen.

Jeder Tarif hat verschiedene Limits für Clientverbindungen, Speicher und Bandbreite. Wenn Ihr Cache über einen längeren Zeitraum maximale Kapazität für diese Metriken erreicht, wird eine Empfehlung erstellt. Weitere Informationen zu den Metriken und Grenzwerten, die vom Tool **Empfehlungen** überprüft werden, finden Sie in der folgenden Tabelle.

| Redis Cache-Metrik | Weitere Informationen finden Sie unter |
|-------------------------|---------------------------------------------------------------------------|
| Netzwerkbandbreiten-Nutzung | [Cacheleistung – verfügbare Bandbreite](cache-faq.md#cache-performance) |
| Verbundene Clients | [Standardmäßige Redis-Serverkonfiguration – maxclients](#maxclients) |
| Serverauslastung | [Nutzungsdiagramme – Arbeitsauslastung des Redis-Servers](cache-how-to-monitor.md#usage-charts) |
| Speicherauslastung | [Cacheleistung – Größe](cache-faq.md#cache-performance) |

Klicken Sie zum Upgraden Ihres Caches auf **Jetzt aktualisieren**, um den [Tarif](#pricing-tier) zu ändern und Ihren Cache zu skalieren. Weitere Informationen zur Tarifauswahl finden Sie unter [Welches Redis Cache-Angebot und welche Redis Cache-Größe sollte ich verwenden?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## Skalierungseinstellungen

Die Einstellungen im Abschnitt **Staffelung** ermöglichen den Zugriff auf die folgenden Cache-Einstellungen und deren Konfiguration:

![Netzwerk](./media/cache-configure/redis-cache-scale.png)

-	[Tarif](#pricing-tier)
-	[Redis-Clustergröße](#cluster-size)

### Tarif

Klicken Sie auf **Tarif**, um den Tarif für Ihren Cache anzuzeigen oder zu ändern. Weitere Informationen zur Skalierung finden Sie unter [Skalieren von Azure Redis Cache](cache-how-to-scale.md).

![Redis-Cache: Tarif](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>
### Redis-Clustergröße

Klicken Sie zum Ändern der Clustergröße aus einem ausgeführten Premium-Cache mit aktivierter Clusterunterstützung auf **(PREVIEW) Redis Cluster Size**.

>[AZURE.NOTE] Beachten Sie, dass sich trotz allgemeiner Verfügbarkeit der Azure Redis Cache in der Premium-Stufe das Feature Redis-Clustergröße derzeit in der Vorschau befindet.

![Redis-Clustergröße](./media/cache-configure/redis-cache-redis-cluster-size.png)

Um die Clustergröße zu ändern, verwenden Sie den Schieberegler oder geben Sie eine Zahl zwischen 1 und 10 im Textfeld **Shardanzahl** ein und klicken Sie zum Speichern auf **OK**.

>[AZURE.IMPORTANT] Redis-Clustering ist nur für Premium-Caches verfügbar. Weitere Informationen finden Sie unter [Konfigurieren von Clustern für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-clustering.md).


## Datenverwaltungseinstellungen

Die Einstellungen im Abschnitt **Datenverwaltung** ermöglichen den Zugriff auf die folgenden Cache-Einstellungen und deren Konfiguration:

![Datenverwaltung](./media/cache-configure/redis-cache-data-management.png)

-	[Redis-Datenpersistenz](#redis-data-persistence)
-	[Import/Export](#importexport)

### Redis-Datenpersistenz

Klicken Sie auf **Redis-Datenpersistenz** zum Aktivieren, Deaktivieren oder Konfigurieren der Datenpersistenz für den Premium-Cache konfigurieren.

![Redis-Datenpersistenz](./media/cache-configure/redis-cache-persistence-settings.png)

Klicken Sie auf **Aktiviert**, um die Redis-Datenbanksicherung zu aktivieren. Klicken Sie auf **Deaktiviert**, um die Redis-Persistenz zu deaktivieren.

Zum Konfigurieren des Sicherungsintervalls, wählen Sie eine **Sicherungshäufigkeit** aus der Dropdownliste. Zur Auswahl stehen **15 Minuten**, **30 Minuten**, **60 Minuten**, **6 Stunden**, **12 Stunden** und **24 Stunden**. Dieses Intervall wird ab dem Moment rückwärts gezählt, an dem der vorherige Sicherungsvorgang erfolgreich abgeschlossen wird. Wenn das Intervall abgelaufen ist, wird eine neue Sicherung gestartet.

Klicken Sie auf **Speicherkonto**, um das Speicherkonto auszuwählen, und wählen Sie entweder **Primärschlüssel** oder **Sekundärschlüssel** aus der Dropdownliste **Speicherschlüssel** aus. Sie müssen ein Speicherkonto auswählen, das aus der gleichen Region wie der Cache stammt, und wir empfehlen ein **Storage Premium**-Konto, da dieses einen höheren Durchsatz aufweist. Wenn der Speicherschlüssel für Ihr Persistenzkonto neu generiert wird, müssen Sie den gewünschten Schlüssel erneut aus der Dropdownliste **Speicherschlüssel** auswählen.

Klicken Sie auf **OK**, um die Persistenzkonfiguration zu speichern.

>[AZURE.IMPORTANT] Redis-Datenpersistenz ist nur für Premium-Caches verfügbar. Weitere Informationen finden Sie unter [Konfigurieren von Persistenz für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-persistence.md).

### Import/Export

Import/Export ist ein Vorgang der Azure Redis Cache-Datenverwaltung, bei dem Sie Daten in Azure Redis Cache importieren oder aus Azure Redis Cache exportieren können. Hierzu importieren bzw. exportieren Sie eine Redis Cache-Datenbankmomentaufnahme (RDB) aus einem Premium-Cache in ein Seitenblob in einem Azure-Speicherkonto. So können Sie zwischen verschiedenen Azure Redis Cache-Instanzen migrieren oder den Cache vor der Nutzung mit Daten auffüllen.

Die Importfunktion kann verwendet werden, um Redis-kompatible RDB-Dateien von beliebigen Redis-Servern zu importieren, die in einer Cloud oder Umgebung ausgeführt werden, z.B. Redis unter Linux oder Windows oder bei einem Cloudanbieter wie Amazon Web Services und anderen. Das Importieren von Daten ist eine einfache Möglichkeit zum Erstellen eines Cache mit vorab aufgefüllten Daten. Während des Importvorgangs lädt Azure Redis Cache die RDB-Dateien aus dem Azure-Speicher in den Arbeitsspeicher und fügt die Schlüssel anschließend in den Cache ein.

Mit der Exportfunktion können Sie die in Azure Redis Cache gespeicherten Daten als Redis-kompatible RDB-Dateien exportieren. Sie können diese Funktion nutzen, um Daten von einer Azure Redis Cache-Instanz zur anderen oder auf einen anderen Redis-Server zu verschieben. Während des Exportvorgangs wird auf dem virtuellen Computer, von dem die Azure Redis Cache-Serverinstanz gehostet wird, eine temporäre Datei erstellt, und die Datei wird in das angegebene Speicherkonto hochgeladen. Nachdem der Exportvorgang mit dem Status „Erfolg“ oder „Fehler“ abgeschlossen wurde, wird die temporäre Datei gelöscht.

>[AZURE.IMPORTANT] Import/Export ist nur für Caches im Premium-Tarif verfügbar. Weitere Informationen und Anweisungen finden Sie unter [Importieren und Exportieren von Daten in Azure Redis Cache](cache-how-to-import-export-data.md).


## Verwaltungseinstellungen

Die Einstellungen im Abschnitt **Verwaltung** ermöglichen das Erledigen der folgenden Verwaltungsaufgaben für Ihren Premium-Cache:

![Verwaltung](./media/cache-configure/redis-cache-administration.png)

-	[Neustart](#reboot)
-	[Planen von Updates](#schedule-updates)

>[AZURE.IMPORTANT] Die Einstellungen in diesem Abschnitt sind nur für Caches im Premium-Tarif verfügbar.

### Neustart

Auf dem Blatt **Neustart** können Sie einen oder mehrere Knoten Ihres Caches neu starten. Dies ermöglicht das Testen Ihrer Anwendung auf Stabilität im Fall eines Ausfalls.

![Neustart](./media/cache-configure/redis-cache-reboot.png)

Wenn Sie über einen Premium-Cache mit aktiviertem Clustering verfügen, können Sie die Shards des Caches auswählen, die neu gestartet werden sollen.

![Neustart](./media/cache-configure/redis-cache-reboot-cluster.png)

Wählen Sie zum Neustarten eines oder mehrerer Knoten Ihres Caches die gewünschten Knoten aus, und klicken Sie anschließend auf **Neustart**. Wenn Sie über einen Premium-Cache mit aktiviertem Clustering verfügen, wählen Sie die Shards aus, die neu gestartet werden sollen, und klicken Sie dann auf **Neustart**. Nach einigen Minuten werden die ausgewählten Knoten neu gestartet, die paar Minuten später wieder online sind.

>[AZURE.IMPORTANT] Ein Neustart ist nur für Caches im Premium-Tarif verfügbar. Weitere Informationen und Anweisungen finden Sie unter [Azure Redis Cache-Verwaltung – Neustart](cache-administration.md#reboot).

### Planen von Updates

Auf dem Blatt **Updates planen** können Sie ein Wartungsfenster für Redis-Serverupdates für Ihren Cache bestimmen.

>[AZURE.IMPORTANT] Beachten Sie, dass das Wartungsfenster nur für Redis-Serverupdates und nicht für Azure-Updates oder Updates des Betriebssystems der virtuellen Computer gilt, die den Cache hosten.

![Planen von Updates](./media/cache-configure/redis-schedule-updates.png)

Aktivieren Sie zum Angeben eines Wartungsfensters die Kontrollkästchen der gewünschten Tage, geben Sie jeweils die Startzeit des Wartungsfensters an, und klicken Sie auf **OK**. Beachten Sie, dass die Zeit im Wartungsfenster als UTC angegeben ist.

>[AZURE.IMPORTANT] Das Planen von Updates ist nur für Caches im Premium-Tarif verfügbar. Weitere Informationen und Anweisungen finden Sie unter [Azure Redis Cache-Verwaltung – Planen von Updates](cache-administration.md#schedule-updates).

## Diagnoseeinstellungen

Im Abschnitt **Diagnose** können Sie die Diagnose für Ihren Redis Cache konfigurieren.

![Diagnose](./media/cache-configure/redis-cache-diagnostics.png)

Klicken Sie zum [Konfigurieren des Speicherkontos](cache-how-to-monitor.md#enable-cache-diagnostics) für Cachediagnosedaten auf **Diagnose**.

![Redis Cache: Diagnose](./media/cache-configure/redis-cache-diagnostics-settings.png)

Klicken Sie auf **Redis-Metriken**, um für Ihren Cache [Metriken anzuzeigen](cache-how-to-monitor.md#how-to-view-metrics-and-customize-charts), und auf **Warnungsregeln**, um [Warnungsregeln einzurichten](cache-how-to-monitor.md#operations-and-alerts).

Weitere Informationen zur Azure Redis Cache-Diagnose finden Sie unter [Überwachen von Azure Redis Cache](cache-how-to-monitor.md).


## Netzwerkeinstellungen

Die Einstellungen im Abschnitt **Netzwerk** ermöglichen den Zugriff auf die folgenden Cache-Einstellungen und deren Konfiguration:

![Netzwerk](./media/cache-configure/redis-cache-network.png)

>[AZURE.IMPORTANT] Einstellungen des virtuellen Netzwerks stehen nur für Premium-Caches zur Verfügung, die während der Erstellung des Caches mit VNET-Unterstützung konfiguriert wurden. Weitere Informationen zum Erstellen eines Premium-Caches mit VNET-Unterstützung sowie zum Aktualisieren der Einstellungen finden Sie unter [Konfigurieren der Unterstützung virtueller Netzwerke für Azure Redis Cache vom Typ „Premium“](cache-how-to-premium-vnet.md).

## Ressourcenverwaltungseinstellungen

![Ressourcenverwaltung](./media/cache-configure/redis-cache-resource-management.png)

Im Abschnitt **Tags** können Sie Ihre Ressourcen organisieren. Weitere Informationen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../resource-group-using-tags.md).

Im Abschnitt **Sperren** können Sie ein Abonnement, eine Ressourcengruppe oder eine Ressource sperren, um zu verhindern, dass andere Benutzer in Ihrer Organisation versehentlich wichtige Ressourcen löschen oder ändern. Weitere Informationen finden Sie unter [Sperren von Ressourcen mit dem Azure-Ressourcen-Manager](../resource-group-lock-resources.md).

Der Abschnitt **Benutzer** bietet Unterstützung für die rollenbasierte Access Control (RBAC) über das Azure-Portal, damit Organisationen ihre Zugriffsverwaltungsanforderungen einfach und präzise erfüllen können. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Active Directory](../active-directory/role-based-access-control-configure.md).

Klicken Sie auf **Vorlage exportieren**, um für künftige Bereitstellungen eine Vorlage Ihrer bereitgestellten Ressourcen zu erstellen und zu exportieren. Weitere Informationen zum Verwenden von Vorlagen finden Sie unter [Bereitstellen von Ressourcen mit einer Azure Resource Manager-Vorlage](../resource-group-template-deploy.md).

## Standardmäßige Redis-Serverkonfiguration

Neue Azure Redis Cache-Instanzen werden mit den folgenden standardmäßigen Redis-Konfigurationswerten konfiguriert.

>[AZURE.NOTE] Die Einstellungen in diesem Abschnitt können mit der `StackExchange.Redis.IServer.ConfigSet`-Methode nicht geändert werden. Wenn diese Methode mit einem der Befehle in diesem Abschnitt aufgerufen wird, wird eine Ausnahme ausgelöst, die in etwa wie folgt lautet:
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>Alle Werte, die konfigurierbar sind, z. B. **max-memory-policy**, können über das Azure-Portal oder Befehlszeilenverwaltungstools wie die Azure-Befehlszeilenschnittstelle oder PowerShell konfiguriert werden.

|Einstellung|Standardwert|Beschreibung|
|---|---|---|
|Datenbanken|16|Die Standardanzahl von Datenbanken ist 16, Sie können aber basierend auf dem Tarif eine andere Anzahl konfigurieren.<sup>1</sup> Die Standarddatenbank ist „DB 0“. Sie können pro Verbindung eine andere Datenbank auswählen, indem Sie `connection.GetDatabase(dbid)` verwenden. Hierbei steht „dbid“ für eine Zahl zwischen `0` und `databases - 1`.|
|maxclients|Tarifabhängig<sup>2</sup>|Dies ist die maximale Anzahl von verbundenen Clients, die gleichzeitig zulässig sind. Sobald der Grenzwert erreicht ist, schließt Redis alle neuen Verbindungen und sendet den Fehler „max number of clients reached“ (Maximale Anzahl von Clients erreicht).|
|maxmemory-policy|volatile-lru|Mit der Einstellung „maxmemory-policy“ wählt Redis aus, was entfernt werden soll, wenn der Wert für „maxmemory“ (Größe des Cacheangebots, die Sie beim Erstellen des Caches ausgewählt haben) erreicht ist. Bei Azure Redis Cache lautet die Standardeinstellung „volatile-lru“. Hierbei werden die Schlüssel anhand eines Ablaufverfahrens mit LRU-Algorithmus entfernt. Diese Einstellung kann im Azure-Portal konfiguriert werden. Weitere Informationen finden Sie unter [maxmemory-policy und maxmemory-reserved](#maxmemory-policy-and-maxmemory-reserved).|
|maxmemory-samples|3|LRU- und minimale TTL-Algorithmen sind keine präzisen Algorithmen, sondern angenäherte Algorithmen (um Arbeitsspeicher zu sparen). Sie können also auch die Größe der zu prüfenden Stichproben auswählen. Standardmäßig werden von Redis beispielsweise drei Schlüssel geprüft, und es wird der Schlüssel ausgewählt, der vor längerer Zeit verwendet wurde.|
|lua-time-limit|5\.000|Maximale Ausführungszeit eines Lua-Skripts in Millisekunden. Wenn die maximale Ausführungszeit erreicht wird, wird von Redis protokolliert, dass ein Skript nach der maximal zulässigen Ausführungszeit weiterhin ausgeführt wird. Es wird dann damit begonnen, auf Abfragen mit einem Fehler zu antworten.|
|lua-event-limit|500|Dies ist die maximale Größe der Skriptereigniswarteschlange.|
|client-output-buffer-limit normalclient-output-buffer-limit pubsub|0 0 032mb 8mb 60|Die Clientausgabepuffer-Grenzwerte können verwendet werden, um die Verbindungstrennung für Clients zu erzwingen, die aus einem bestimmten Grund Daten nicht schnell genug vom Server lesen. (Ein häufiger Grund ist, dass ein Pub/Sub-Client Nachrichten nicht so schnell verarbeiten kann, wie sie von der veröffentlichenden Stelle produziert werden.) Weitere Informationen finden Sie unter [http://redis.io/topics/clients](http://redis.io/topics/clients).|

<a name="databases"></a> <sup>1</sup>Der Grenzwert für `databases` ist für jeden Azure Redis Cache-Tarif unterschiedlich und kann bei der Erstellung des Caches festgelegt werden. Wenn bei der Cacheerstellung keine `databases`-Einstellung angegeben wird, wird der Standardwert (16) verwendet.

-	Caches vom Typ „Basic“ und „Standard“
	-	C0 (250 MB) – bis zu 16 Datenbanken
	-	C1 (1 GB) – bis zu 16 Datenbanken
	-	C2 (2,5 GB) – bis zu 16 Datenbanken
	-	C3 (6 GB) – bis zu 16 Datenbanken
	-	C4 (13 GB) – bis zu 32 Datenbanken
	-	C5 (26 GB) – bis zu 48 Datenbanken
	-	C6 (53 GB) – bis zu 64 Datenbanken
-	Premium-Caches
	-	P1 (6 bis 60 GB) – bis zu 16 Datenbanken
	-	P2 (13 bis 130 GB) – bis zu 32 Datenbanken
	-	P3 (26 bis 260 GB) – bis zu 48 Datenbanken
	-	P4 (53 bis 530 GB) – bis zu 64 Datenbanken
	-   Alle Premium-Caches mit aktivierten Redis-Cluster: Ein Redis-Cluster unterstützt nur die Nutzung der Datenbank 0. Deshalb ist der `databases`-Grenzwert für Premium-Caches mit aktiviertem Redis-Cluster effektiv 1, und der Befehl [Auswählen](http://redis.io/commands/select) ist nicht zulässig. Weitere Informationen finden Sie unter [Muss ich Änderungen an meiner Clientanwendung vornehmen, um Clustering verwenden zu können?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering).


>[AZURE.NOTE] Die Einstellung `databases` kann nur bei der Erstellung des Caches konfiguriert und nur mit PowerShell, der Befehlszeilenschnittstelle oder anderen Verwaltungsclients verwendet werden. Ein Beispiel für das Konfigurieren von `databases` bei der Cacheerstellung mithilfe von PowerShell finden Sie unter [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).


<a name="maxclients"></a> <sup>2</sup>`maxclients` ist für jeden Azure Redis Cache-Tarif unterschiedlich.

-	Caches vom Typ "Basic" und "Standard"
	-	Cache C0 (250 MB) – bis zu 256 Verbindungen
	-	Cache C1 (1 GB) – bis zu 1.000 Verbindungen
	-	Cache C2 (2,5 GB) – bis zu 2.000 Verbindungen
	-	Cache C3 (6 GB) – bis zu 5.000 Verbindungen
	-	Cache C4 (13 GB) – bis zu 10.000 Verbindungen
	-	Cache C5 (26 GB) – bis zu 15.000 Verbindungen
	-	Cache C6 (53 GB) – bis zu 20.000 Verbindungen
-	Premium-Caches
	-	P1 (6 GB - 60 GB) – bis zu 7.500 Verbindungen
	-	P2 (13 GB - 130 GB) – bis zu 15.000 Verbindungen
	-	P3 (26 GB - 260 GB) – bis zu 30.000 Verbindungen
	-	P4 (53 GB - 530 GB) – bis zu 40.000 Verbindungen

## Redis-Befehle, die in Azure Redis Cache nicht unterstützt werden.

>[AZURE.IMPORTANT] Da die Konfiguration und Verwaltung von Azure Redis Cache-Instanzen von Microsoft verwaltet wird, sind die folgenden Befehle deaktiviert. Wenn Sie versuchen, sie aufzurufen, erhalten Sie etwa folgende Fehlermeldung: `"(error) ERR unknown command"`.
>
>-	BGREWRITEAOF
>-	BGSAVE
>-	CONFIG
>-	DEBUG
>-	MIGRATE
>-	SAVE
>-	SHUTDOWN
>-	SLAVEOF
>-	CLUSTER: Clusterschreibbefehle sind deaktiviert, aber schreibgeschützte Clusterbefehle sind zulässig.

Weitere Informationen zu Redis-Befehlen finden Sie unter [http://redis.io/commands](http://redis.io/commands).

## Redis-Konsole

Über die **Redis-Konsole**, die für Caches vom Typ "Standard" und "Premium" zur Verfügung steht, können Sie Befehle sicher auf Ihre Azure Redis Cache-Instanzen anwenden.

>[AZURE.IMPORTANT] Die Redis-Konsole kann nicht mit VNET, Clustering und anderen Datenbanken als 0 verwendet werden.
>
>-	[VNET](cache-how-to-premium-vnet.md): Wenn der Cache Teil eines VNET ist, haben nur Clients im VNET Zugriff auf den Cache. Da die Redis-Konsole den Client "Redis-cli.exe" verwendet, der auf virtuellen Computern gehostet wird, die nicht Teil des VNET sind, kann sie keine Verbindung mit dem Cache herstellen.
>-	[Clustering](cache-how-to-premium-clustering.md): Die Redis-Konsole verwendet den Client "Redis-cli.exe", der Clustering derzeit nicht unterstützt. Das Hilfsprogramm redis-cli in der [unstable](http://redis.io/download)-Verzweigung des Redis-Repositorys auf GitHub implementiert grundlegende Unterstützung, wenn es mit dem Switch `-c` gestartet wird. Weitere Informationen finden Sie im [Redis Cluster Tutorial](http://redis.io/topics/cluster-tutorial) unter [Playing with the Cluster](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) (in englischer Sprache) auf [http://redis.io](http://redis.io).
>-	Die Redis-Konsole stellt jedes Mal, wenn Sie einen Befehl senden, eine neue Verbindung der Datenbank 0 her. Sie können den Befehl `SELECT` nicht verwenden, um eine andere Datenbank auszuwählen, da die Datenbank bei jedem Befehl auf 0 zurückgesetzt wird. Informationen zum Ausführen von Redis-Befehlen, einschließlich des Wechsels zu einer anderen Datenbank, finden Sie unter [Wie führe ich Redis-Befehle aus?](cache-faq.md#how-can-i-run-redis-commands).

Um auf die Redis-Konsole zuzugreifen, klicken Sie auf dem Blatt **Redis-Cache** auf **Konsole**.

![Redis-Konsole](./media/cache-configure/redis-console-menu.png)

Zum Anwenden von Befehlen auf Ihre Cache-Instanz geben Sie einfach den gewünschten Befehl in die Konsole ein.

![Redis-Konsole](./media/cache-configure/redis-console.png)

Eine Liste der Redis-Befehle, die für Azure Redis Cache deaktiviert sind, finden Sie im vorherigen Abschnitt [Redis-Befehle, die in Azure Redis Cache nicht unterstützt werden](#redis-commands-not-supported-in-azure-redis-cache). Weitere Informationen zu Redis-Befehlen finden Sie unter [http://redis.io/commands](http://redis.io/commands).

## Verschieben Ihres Caches in ein neues Abonnement

Sie können Ihren Cache in ein neues Abonnement verschieben, indem Sie auf **Verschieben** klicken.

![Verschieben von Redis Cache](./media/cache-configure/redis-cache-move.png)

Informationen zum Verschieben von Ressourcen zwischen Ressourcengruppen und zwischen Abonnements finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../resource-group-move-resources.md).

## Nächste Schritte
-	Weitere Informationen zum Verwenden von Redis-Befehlen finden Sie unter [Wie führe ich Redis-Befehle aus?](cache-faq.md#how-can-i-run-redis-commands).

<!---HONumber=AcomDC_0727_2016-->