---
title: Konfigurieren von Azure Redis Cache | Microsoft Docs
description: "Grundlagen der Redis-Standardkonfiguration für Azure Redis Cache und Informationen zur Konfiguration Ihrer Azure Redis Cache-Instanzen"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: d0bf2e1f-6a26-4e62-85ba-d82b35fc5aa6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/11/2017
ms.author: sdanie
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: fefa78f7e0ba4bd40e6c0985080403237d6eb916
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="how-to-configure-azure-redis-cache"></a>Gewusst wie: Konfigurieren von Azure Redis Cache
In diesem Thema wird beschrieben, wie Sie die Konfiguration für Ihre Azure Redis Cache-Instanzen überprüfen und aktualisieren. Außerdem wird die standardmäßige Redis-Serverkonfiguration für Azure Redis Cache-Instanzen beschrieben.

> [!NOTE]
> Weitere Informationen zur Konfiguration und Verwendung von Premium-Cache-Features finden Sie unter [Konfigurieren von Persistenz](cache-how-to-premium-persistence.md), [Konfigurieren von Clustern](cache-how-to-premium-clustering.md) und [Konfigurieren der Virtual Network-Unterstützung](cache-how-to-premium-vnet.md).
> 
> 

## <a name="configure-redis-cache-settings"></a>Konfigurieren von Redis Cache-Einstellungen
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure Redis Cache-Einstellungen werden auf dem Blatt **Redis Cache** über das **Ressourcenmenü** angezeigt und konfiguriert.

![Redis Cache: Einstellungen](./media/cache-configure/redis-cache-settings.png)

Sie können die folgenden Einstellungen über das **Ressourcenmenü** anzeigen und konfigurieren.

* [Übersicht](#overview)
* [Aktivitätsprotokoll](#activity-log)
* [Zugriffssteuerung (IAM)](#access-control-iam)
* [Tags](#tags)
* [Diagnose und Problembehandlung](#diagnose-and-solve-problems)
* [Einstellungen](#settings)
    * [Zugriffsschlüssel](#access-keys)
    * [Erweiterte Einstellungen](#advanced-settings)
    * [Redis Cache Advisor](#redis-cache-advisor)
    * [Skalieren](#scale)
    * [Redis-Clustergröße](#cluster-size)
    * [Redis-Datenpersistenz](#redis-data-persistence)
    * [Planen von Updates](#schedule-updates)
    * [Virtual Network](#virtual-network)
    * [Firewall](#firewall)
    * [Eigenschaften](#properties)
    * [Sperren](#locks)
    * [Automatisierungsskript](#automation-script)
* [Verwaltung](#administration)
    * [Daten importieren](#importexport)
    * [Daten exportieren](#importexport)
    * [Neustart](#reboot)
* [Überwachung](#monitoring)
    * [Redis-Metriken](#redis-metrics)
    * [Warnregeln](#alert-rules)
    * [Diagnose](#diagnostics)
* [Einstellungen für Support und Problembehandlung](#support-amp-troubleshooting-settings)
    * [Ressourcenintegrität](#resource-health)
    * [Neue Supportanfrage](#new-support-request)


## <a name="overview"></a>Übersicht

**Übersicht** enthält grundlegende Informationen zu Ihrem Cache, z.B. Name, Ports, Tarif und ausgewählte Cachemetriken.

### <a name="activity-log"></a>Aktivitätsprotokoll

Klicken Sie auf **Aktivitätsprotokoll** , um in Ihrem Cache ausgeführte Aktionen anzuzeigen. Sie können auch filtern, um diese Ansicht zum Einbeziehen anderer Ressourcen zu erweitern. Weitere Informationen zur Arbeit mit Überwachungsprotokollen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](../azure-resource-manager/resource-group-audit.md). Weitere Informationen zum Überwachen von Azure Redis Cache-Ereignissen finden Sie unter [Vorgänge und Warnungen](cache-how-to-monitor.md#operations-and-alerts).

### <a name="access-control-iam"></a>Zugriffssteuerung (IAM)

Der Abschnitt **Zugriffssteuerung (IAM)** bietet Unterstützung für die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) über das Azure-Portal, damit Organisationen ihre Zugriffsverwaltungsanforderungen einfach und präzise erfüllen können. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung im Azure-Portal](../active-directory/role-based-access-control-configure.md).

### <a name="tags"></a>Tags

Im Abschnitt **Tags** können Sie Ihre Ressourcen organisieren. Weitere Informationen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../azure-resource-manager/resource-group-using-tags.md).


### <a name="diagnose-and-solve-problems"></a>Diagnose und Problembehandlung

Klicken Sie auf **Diagnose und Problembehandlung** , um gängige Probleme und Strategien zu deren Behebung anzuzeigen.



## <a name="settings"></a>Einstellungen
Der Abschnitt **Einstellungen** ermöglicht den Zugriff auf die folgenden Cacheeinstellungen und deren Konfiguration.

![Einstellungen](./media/cache-configure/redis-cache-general-settings.png)

* [Zugriffsschlüssel](#access-keys)
* [Erweiterte Einstellungen](#advanced-settings)
* [Redis Cache Advisor](#redis-cache-advisor)
* [Skalieren](#scale)
* [Redis-Clustergröße](#cluster-size)
* [Redis-Datenpersistenz](#redis-data-persistence)
* [Planen von Updates](#schedule-updates)
* [Virtual Network](#virtual-network)
* [Firewall](#firewall)
* [Eigenschaften](#properties)
* [Sperren](#locks)
* [Automatisierungsskript](#automation-script)



### <a name="access-keys"></a>Zugriffsschlüssel
Klicken Sie auf **Zugriffsschlüssel** , um die Zugriffsschlüssel für Ihren Cache anzuzeigen oder neu zu generieren. Diese Schlüssel werden von den Clients verwendet, die eine Verbindung mit dem Cache herstellen.

![Redis Cache: Zugriffsschlüssel](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a>Erweiterte Einstellungen
Die folgenden Einstellungen werden auf dem Blatt **Erweiterte Einstellungen** konfiguriert.

* [Zugriffsports](#access-ports)
* [maxmemory-policy und maxmemory-reserved](#maxmemory-policy-and-maxmemory-reserved)
* [Keyspacebenachrichtigungen (Erweiterte Einstellungen)](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a>Zugriffsports
Der Zugriff ohne SSL ist für neue Caches standardmäßig deaktiviert. Zum Aktivieren eines nicht SSL-fähigen Ports klicken Sie auf dem Blatt **Erweiterte Einstellungen** bei **Nur Zugriff über SSL zulassen** auf **Nein**, und klicken Sie dann auf **Speichern**.

![Redis Cache: Zugriffsports](./media/cache-configure/redis-cache-access-ports.png)

#### <a name="maxmemory-policy-and-maxmemory-reserved"></a>maxmemory-policy und maxmemory-reserved
Mit den Einstellungen **maxmemory-policy** und **maxmemory-reserved** auf dem Blatt **Erweiterte Einstellungen** konfigurieren Sie die Arbeitsspeicherrichtlinien für den Cache. Mit der Einstellung **maxmemory-policy** wird die Entfernungsrichtlinie für den Cache konfiguriert, und mit **maxmemory-reserved** wird der Arbeitsspeicher konfiguriert, der für andere Prozesse als Cacheprozesse reserviert ist.

![Redis Cache: Maxmemory-Richtlinie](./media/cache-configure/redis-cache-maxmemory-policy.png)

Unter **Maxmemory-Richtlinie** können Sie eine der folgenden Entfernungsrichtlinien auswählen:

* `volatile-lru`: Dies ist die Standardoption.
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

Weitere Informationen zu `maxmemory`-Richtlinien finden Sie unter [Eviction policies](http://redis.io/topics/lru-cache#eviction-policies) (Entfernungsrichtlinien).

Mit der Einstellung **maxmemory-reserved** wird die Arbeitsspeichermenge in MB konfiguriert, die für andere Prozesse als Cacheprozesse reserviert ist, z.B. die Replikation während eines Failovers. Sie kann auch verwendet werden, wenn Sie über ein hohes Fragmentierungsverhältnis verfügen. Mit dem Festlegen dieses Werts können Sie dafür sorgen, dass Sie bei wechselnden Auslastungen eine konsistentere Redis-Servererfahrung erzielen. Für Workloads, die mit hohem Schreibaufwand verbunden sind, sollte ein höherer Wert festgelegt werden. Wenn Arbeitsspeicher für Vorgänge dieser Art reserviert ist, ist er nicht für die Speicherung zwischengespeicherter Daten verfügbar.

> [!IMPORTANT]
> Die Einstellung **maxmemory-reserved** ist nur für Standard- und Premium-Caches verfügbar.
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a>Keyspacebenachrichtigungen (Erweiterte Einstellungen)
Redis-Keyspacebenachrichtigungen werden auf dem Blatt **Erweiterte Einstellungen** konfiguriert. Mit Keyspacebenachrichtigungen können Clients Benachrichtigungen empfangen, wenn bestimmte Ereignisse eintreten.

![Redis Cache: Erweiterte Einstellungen](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> Keyspacebenachrichtigungen und die Einstellung **notify-keyspace-events** sind nur für Caches vom Typ "Standard" und "Premium" verfügbar.
> 
> 

Weitere Informationen finden Sie unter [Redis-Keyspacebenachrichtigungen](http://redis.io/topics/notifications). Beispielcode finden Sie in der Datei [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) im [Hello world-Beispiel](https://github.com/rustd/RedisSamples/tree/master/HelloWorld).


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis Cache Advisor
Das Blatt **Redis Cache Advisor** zeigt Empfehlungen für Ihren Cache an. Während des normalen Betriebs werden keine Empfehlungen angezeigt. 

![Empfehlungen](./media/cache-configure/redis-cache-no-recommendations.png)

Wenn während in Ihrem Cache Bedingungen wie hohe Speicherauslastung, Netzwerkbandbreite oder Serverauslastung auftreten, wird auf dem Blatt **Redis Cache** eine Warnung angezeigt.

![Empfehlungen](./media/cache-configure/redis-cache-recommendations-alert.png)

Weitere Informationen finden Sie auf dem Blatt **Empfehlungen** .

![Empfehlungen](./media/cache-configure/redis-cache-recommendations.png)

Sie können diese Metriken auf dem Blatt [Redis Cache](cache-how-to-monitor.md#monitoring-charts) in den Abschnitten [Überwachungsdiagramme](cache-how-to-monitor.md#usage-charts) und **Nutzungsdiagramme** überwachen.

Jeder Tarif hat verschiedene Limits für Clientverbindungen, Speicher und Bandbreite. Wenn Ihr Cache über einen längeren Zeitraum maximale Kapazität für diese Metriken erreicht, wird eine Empfehlung erstellt. Weitere Informationen zu den Metriken und Grenzwerten, die vom Tool **Empfehlungen** überprüft werden, finden Sie in der folgenden Tabelle:

| Redis Cache-Metrik | Weitere Informationen |
| --- | --- |
| Netzwerkbandbreiten-Nutzung |[Cacheleistung – verfügbare Bandbreite](cache-faq.md#cache-performance) |
| Verbundene Clients |[Standardmäßige Redis-Serverkonfiguration – maxclients](#maxclients) |
| Serverauslastung |[Nutzungsdiagramme – Arbeitsauslastung des Redis-Servers](cache-how-to-monitor.md#usage-charts) |
| Speicherauslastung |[Cacheleistung – Größe](cache-faq.md#cache-performance) |

Klicken Sie zum Upgraden Ihres Caches auf **Jetzt aktualisieren**, um den [Tarif](#scale) zu ändern und Ihren Cache zu skalieren. Weitere Informationen zur Tarifauswahl finden Sie unter [Welches Redis Cache-Angebot und welche Redis Cache-Größe sollte ich verwenden?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)


### <a name="scale"></a>Skalieren
Klicken Sie auf **Staffelung**, um den Tarif für Ihren Cache anzuzeigen oder zu ändern. Weitere Informationen zur Skalierung finden Sie unter [Skalieren von Azure Redis Cache](cache-how-to-scale.md).

![Redis-Cache: Tarif](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a>Redis-Clustergröße
Klicken Sie zum Ändern der Clustergröße aus einem ausgeführten Premium-Cache mit aktivierter Clusterunterstützung auf **(PREVIEW) Redis Cluster Size** .

> [!NOTE]
> Beachten Sie, dass sich trotz allgemeiner Verfügbarkeit der Azure Redis Cache in der Premium-Stufe das Feature Redis-Clustergröße derzeit in der Vorschau befindet.
> 
> 

![Redis-Clustergröße](./media/cache-configure/redis-cache-redis-cluster-size.png)

Um die Clustergröße zu ändern, verwenden Sie den Schieberegler, oder geben Sie im Textfeld **Shardanzahl** eine Zahl zwischen 1 und 10 ein, und klicken Sie zum Speichern auf **OK**.

> [!IMPORTANT]
> Redis-Clustering ist nur für Premium-Caches verfügbar. Weitere Informationen finden Sie unter [Konfigurieren von Clustern für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-clustering.md).
> 
> 


### <a name="redis-data-persistence"></a>Redis-Datenpersistenz
Klicken Sie auf **Redis-Datenpersistenz** zum Aktivieren, Deaktivieren oder Konfigurieren der Datenpersistenz für den Premium-Cache konfigurieren.

![Redis-Datenpersistenz](./media/cache-configure/redis-cache-persistence-settings.png)

Klicken Sie auf **Aktiviert** , um die Redis-Datenbanksicherung zu aktivieren. Klicken Sie auf **Deaktiviert**, um die Redis-Persistenz zu deaktivieren.

Wählen Sie zum Konfigurieren des Sicherungsintervalls in der Dropdownliste unter **Sicherungshäufigkeit** einen der folgenden Einträge aus. 

- **15 Minuten**
- **30 Minuten**
- **60 Minuten**
- **6 Stunden**
- **12 Stunden**
- **24 Stunden**

Das Sicherungsintervall wird ab dem Moment rückwärts gezählt, an dem der vorherige Sicherungsvorgang erfolgreich abgeschlossen wird. Wenn das Intervall abgelaufen ist, wird eine neue Sicherung gestartet.

Klicken Sie auf **Speicherkonto**, um das Speicherkonto auszuwählen, und wählen Sie entweder den **primären** oder den **sekundären Schlüssel** aus der Dropdownliste **Speicherschlüssel** aus. Sie müssen ein Speicherkonto auswählen, das aus der gleichen Region wie der Cache stammt, und wir empfehlen ein **Storage Premium** -Konto, da dieses einen höheren Durchsatz aufweist. Wenn der Speicherschlüssel für Ihr Persistenzkonto neu generiert wird, müssen Sie den gewünschten Schlüssel erneut in der Dropdownliste **Speicherschlüssel** auswählen.

Klicken Sie auf **OK** , um die Persistenzkonfiguration zu speichern.

> [!IMPORTANT]
> Redis-Datenpersistenz ist nur für Premium-Caches verfügbar. Weitere Informationen finden Sie unter [Konfigurieren von Persistenz für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-persistence.md).
> 
> 

### <a name="schedule-updates"></a>Planen von Updates
Auf dem Blatt **Updates planen** können Sie ein Wartungsfenster für Redis-Serverupdates für Ihren Cache bestimmen. 

> [!IMPORTANT]
> Das Wartungsfenster bezieht sich nur auf Redis-Serverupdates und nicht auf Azure-Updates oder Updates des Betriebssystems der virtuellen Computer, die den Cache hosten.
> 
> 

![Planen von Updates](./media/cache-configure/redis-schedule-updates.png)

Aktivieren Sie zum Angeben eines Wartungsfensters die Kontrollkästchen der gewünschten Tage, geben Sie jeweils die Startzeit des Wartungsfensters an, und klicken Sie auf **OK**. Beachten Sie, dass die Zeit im Wartungsfenster als UTC angegeben ist. 

> [!IMPORTANT]
> Die Funktion **Zeitplanaktualisierungen** ist nur für Caches im Premium-Tarif verfügbar. Weitere Informationen und Anweisungen finden Sie unter [Azure Redis Cache-Verwaltung – Planen von Updates](cache-administration.md#schedule-updates).
> 
> 



### <a name="virtual-network"></a>Virtuelles Netzwerk
Im Abschnitt **Virtuelles Netzwerk** können Sie die Einstellungen des virtuellen Netzwerks für Ihren Cache konfigurieren. Weitere Informationen zum Erstellen eines Premium-Caches mit VNET-Unterstützung sowie zum Aktualisieren der Einstellungen finden Sie unter [Konfigurieren der Unterstützung virtueller Netzwerke für Azure Redis Cache vom Typ „Premium“](cache-how-to-premium-vnet.md).

> [!IMPORTANT]
> Einstellungen des virtuellen Netzwerks stehen nur für Premium-Caches zur Verfügung, die während der Erstellung des Caches mit VNET-Unterstützung konfiguriert wurden. 
> 
> 

### <a name="firewall"></a>Firewall

Klicken Sie auf **Firewall**, um Firewallregeln für Ihr Azure Redis Cache vom Typ „Premium“ anzuzeigen und zu konfigurieren.

![Firewall](./media/cache-configure/redis-firewall-rules.png)

Sie können Firewallregeln mit einem Start- und End-IP-Adressbereich angeben. Wenn Firewallregeln konfiguriert werden, können nur Clientverbindungen aus dem angegebenen IP-Adressbereich eine Verbindung mit dem Cache herstellen. Wenn eine Firewallregel gespeichert wird, gibt es eine kurze Verzögerung, bevor die Regel in Kraft tritt. Diese Verzögerung beträgt in der Regel weniger als eine Minute.

> [!IMPORTANT]
> Verbindungen von Azure Redis Cache-Überwachungssystemen werden immer zugelassen, auch wenn Firewallregeln konfiguriert sind.
> 
> Firewallregeln sind nur für Caches im Premium-Tarif verfügbar.
> 
> 

### <a name="properties"></a>Eigenschaften
Klicken Sie auf **Eigenschaften**, um Informationen zu Ihrem Cache anzuzeigen, z.B. den Endpunkt und die Ports des Caches.

![Redis Cache: Eigenschaften](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a>Sperren
Im Abschnitt **Sperren** können Sie ein Abonnement, eine Ressourcengruppe oder eine Ressource sperren, um zu verhindern, dass andere Benutzer in Ihrer Organisation versehentlich wichtige Ressourcen löschen oder ändern. Weitere Informationen finden Sie unter [Sperren von Ressourcen mit dem Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-lock-resources.md).

### <a name="automation-script"></a>Automatisierungsskript

Klicken Sie auf **Automatisierungsskript**, um für künftige Bereitstellungen eine Vorlage Ihrer bereitgestellten Ressourcen zu erstellen und zu exportieren. Weitere Informationen zum Arbeiten mit Vorlagen finden Sie unter [Bereitstellen von Ressourcen mit einer Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="administration-settings"></a>Verwaltungseinstellungen
Mit den Einstellungen im Abschnitt **Verwaltung** können Sie folgende Verwaltungsaufgaben für Ihren Premium-Cache durchführen. 

![Verwaltung](./media/cache-configure/redis-cache-administration.png)

* [Daten importieren](#importexport)
* [Daten exportieren](#importexport)
* [Neustart](#reboot)


> [!IMPORTANT]
> Die Einstellungen in diesem Abschnitt sind nur für Caches im Premium-Tarif verfügbar.
> 
> 

### <a name="importexport"></a>Import/Export
Import/Export ist ein Vorgang der Azure Redis Cache-Datenverwaltung, bei dem Sie Daten in den Cache importieren oder aus dem Cache exportieren können. Hierzu importieren bzw. exportieren Sie eine Momentaufnahme der Redis Cache-Datenbank (RDB) aus einem Premium-Cache in ein Seitenblob in einem Azure Storage-Konto. So können Sie zwischen verschiedenen Azure Redis Cache-Instanzen migrieren oder den Cache vor der Nutzung mit Daten auffüllen.

Die Importfunktion kann verwendet werden, um Redis-kompatible RDB-Dateien von beliebigen Redis-Servern zu importieren, die in einer Cloud oder Umgebung ausgeführt werden, z.B. Redis unter Linux oder Windows oder bei einem Cloudanbieter wie Amazon Web Services und anderen. Das Importieren von Daten ist eine einfache Möglichkeit zum Erstellen eines Cache mit vorab aufgefüllten Daten. Während des Importvorgangs lädt Azure Redis Cache die RDB-Dateien aus Azure Storage in den Arbeitsspeicher und fügt die Schlüssel anschließend in den Cache ein.

Mit der Exportfunktion können Sie die in Azure Redis Cache gespeicherten Daten als Redis-kompatible RDB-Dateien exportieren. Sie können diese Funktion nutzen, um Daten von einer Azure Redis Cache-Instanz zur anderen oder auf einen anderen Redis-Server zu verschieben. Während des Exportvorgangs wird auf dem virtuellen Computer, der die Azure Redis Cache-Serverinstanz hostet, eine temporäre Datei erstellt, die dann in das angegebene Speicherkonto hochgeladen wird. Nachdem der Exportvorgang mit dem Status „Erfolg“ oder „Fehler“ abgeschlossen wurde, wird die temporäre Datei gelöscht.

> [!IMPORTANT]
> Import/Export ist nur für Caches im Premium-Tarif verfügbar. Weitere Informationen und Anweisungen finden Sie unter [Importieren und Exportieren von Daten in Azure Redis Cache](cache-how-to-import-export-data.md).
> 
> 

### <a name="reboot"></a>Neustart
Auf dem Blatt **Neustart** können Sie die Knoten Ihres Caches neu starten. Mit dieser Neustartfunktion können Sie Ihre Anwendung bei einem Ausfall eines Cacheknotens auf Resilienz testen.

![Neustart](./media/cache-configure/redis-cache-reboot.png)

Wenn Sie über einen Premium-Cache mit aktiviertem Clustering verfügen, können Sie die Shards des Caches auswählen, die neu gestartet werden sollen.

![Neustart](./media/cache-configure/redis-cache-reboot-cluster.png)

Zum Neustarten eines oder mehrerer Knoten Ihres Caches wählen Sie die gewünschten Knoten aus und klicken auf **Neustart**. Wenn Sie über einen Premium-Cache mit aktiviertem Clustering verfügen, wählen Sie die Shards aus, die neu gestartet werden sollen, und klicken Sie dann auf **Neustart**. Nach einigen Minuten werden die ausgewählten Knoten neu gestartet, die paar Minuten später wieder online sind.

> [!IMPORTANT]
> Ein Neustart ist nur für Caches im Premium-Tarif verfügbar. Weitere Informationen und Anweisungen finden Sie unter [Azure Redis Cache-Verwaltung – Neustart](cache-administration.md#reboot).
> 
> 


## <a name="monitoring"></a>Überwachung

Im Abschnitt **Überwachung** können Sie die Diagnose und Überwachung für Ihren Redis Cache konfigurieren. Weitere Informationen zur Azure Redis Cache-Diagnose und -Überwachung finden Sie unter [Überwachen von Azure Redis Cache](cache-how-to-monitor.md).

![Diagnose](./media/cache-configure/redis-cache-diagnostics.png)

* [Redis-Metriken](#redis-metrics)
* [Warnregeln](#alert-rules)
* [Diagnose](#diagnostics)

### <a name="redis-metrics"></a>Redis-Metriken
Klicken Sie auf **Redis Metriken**, um für Ihren Cache [Metriken anzuzeigen](cache-how-to-monitor.md#how-to-view-metrics-and-customize-charts).

### <a name="alert-rules"></a>Warnregeln

Klicken Sie auf **Warnregeln**, um Warnungen basierend auf Redis Cache-Metriken zu konfigurieren. Weitere Informationen finden Sie unter [Vorgänge und Warnungen](cache-how-to-monitor.md#operations-and-alerts).

### <a name="diagnostics"></a>Diagnose

Klicken Sie zum **Konfigurieren des Speicherkontos** für Cachediagnosedaten auf [Diagnose](cache-how-to-monitor.md#enable-cache-diagnostics) .

![Redis Cache: Diagnose](./media/cache-configure/redis-cache-diagnostics-settings.png)

## <a name="support--troubleshooting-settings"></a>Einstellungen für Support und Problembehandlung
Die Einstellungen im Abschnitt **Support und Problembehandlung** bieten Optionen zur Behandlung von Problemen mit dem Cache.

![Support und Problembehandlung](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [Ressourcenintegrität](#resource-health)
* [Neue Supportanfrage](#new-support-request)

### <a name="resource-health"></a>Ressourcenintegrität
**Ressourcenintegrität** dienen zum Überwachen Ihrer Ressource und informieren Sie darüber, ob sie wie erwartet ausgeführt wird. Weitere Informationen zum Azure Resource Health-Dienst finden Sie in der [Übersicht über Azure Resource Health](../resource-health/resource-health-overview.md).

> [!NOTE]
> Die Ressourcenintegrität kann derzeit keine Informationen zur Integrität der in einem virtuellen Netzwerk gehosteten Azure Redis Cache-Instanzen liefern. Weitere Informationen finden Sie unter [Funktionieren alle Cachefeatures beim Hosten eines Cache in einem VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)
> 
> 

### <a name="new-support-request"></a>Neue Supportanfrage
Klicken Sie auf **Neue Supportanfrage** , um eine Supportanfrage für Ihren Cache zu erstellen.





## <a name="default-redis-server-configuration"></a>Standardmäßige Redis-Serverkonfiguration
Neue Azure Redis Cache-Instanzen werden mit den folgenden standardmäßigen Redis-Konfigurationswerten konfiguriert.

> [!NOTE]
> Die Einstellungen in diesem Abschnitt können mit der `StackExchange.Redis.IServer.ConfigSet`-Methode nicht geändert werden. Wenn diese Methode mit einem der Befehle in diesem Abschnitt aufgerufen wird, wird eine Ausnahme ausgelöst, die in etwa wie folgt lautet:  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> Alle Werte, die konfigurierbar sind, z.B. **maxmemory-policy**, können über das Azure-Portal oder Befehlszeilenverwaltungstools wie die Azure-Befehlszeilenschnittstelle oder PowerShell konfiguriert werden.
> 
> 

| Einstellung | Standardwert | Beschreibung |
| --- | --- | --- |
| `databases` |16 |Die Standardanzahl von Datenbanken ist 16, Sie können aber basierend auf dem Tarif eine andere Anzahl konfigurieren.<sup>1</sup> Die Standarddatenbank ist „DB 0“. Sie können mithilfe von `connection.GetDatabase(dbid)` pro Verbindung eine andere Datenbank auswählen. Hierbei steht `dbid` für eine Zahl zwischen `0` und `databases - 1`. |
| `maxclients` |Tarifabhängig<sup>2</sup> |Dies ist die maximale Anzahl von verbundenen Clients, die gleichzeitig zulässig sind. Sobald der Grenzwert erreicht ist, schließt Redis alle neuen Verbindungen und gibt den Fehler „max number of clients reached“ (Maximale Anzahl von Clients erreicht) zurück. |
| `maxmemory-policy` |`volatile-lru` |Mit der Einstellung „maxmemory-policy“ wird festgelegt, was in Redis entfernt werden soll, wenn der Wert für `maxmemory` (Größe des Caches, die Sie beim Erstellen des Caches ausgewählt haben) erreicht ist. Bei Azure Redis Cache lautet die Standardeinstellung `volatile-lru`. Hierbei werden die Schlüssel anhand eines Ablaufverfahrens mit LRU-Algorithmus entfernt. Diese Einstellung kann im Azure-Portal konfiguriert werden. Weitere Informationen finden Sie unter [maxmemory-policy und maxmemory-reserved](#maxmemory-policy-and-maxmemory-reserved). |
| `maxmemory-sample` |3 |Zur Einsparung von Arbeitsspeicher sind LRU- und minimale TTL-Algorithmen keine präzisen Algorithmen, sondern angenäherte Algorithmen. Standardmäßig werden von Redis drei Schlüssel geprüft, und es wird der Schlüssel ausgewählt, der vor längerer Zeit verwendet wurde. |
| `lua-time-limit` |5.000 |Maximale Ausführungszeit eines Lua-Skripts in Millisekunden. Wenn die maximale Ausführungszeit erreicht wird, protokolliert Redis, dass ein Skript nach der maximal zulässigen Ausführungszeit weiterhin ausgeführt wird. Es wird dann damit begonnen, auf Abfragen mit einem Fehler zu antworten. |
| `lua-event-limit` |500 |Maximale Größe der Skriptereigniswarteschlange. |
| `client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub` |0 0 032mb 8mb 60 |Die Clientausgabepuffer-Grenzwerte können verwendet werden, um die Verbindungstrennung für Clients zu erzwingen, die aus einem bestimmten Grund Daten nicht schnell genug vom Server lesen. (Ein häufiger Grund ist, dass ein Pub/Sub-Client Nachrichten nicht so schnell verarbeiten kann, wie sie von der veröffentlichenden Stelle produziert werden.) Weitere Informationen finden Sie unter [http://redis.io/topics/clients](http://redis.io/topics/clients). |

<a name="databases"></a> 
<sup>1</sup>Der Grenzwert für `databases` ist für jeden Azure Redis Cache-Tarif unterschiedlich und kann bei der Erstellung des Caches festgelegt werden. Wenn bei der Cacheerstellung keine Einstellung für `databases` angegeben wird, wird der Standardwert (16) verwendet.

* Caches vom Typ „Basic“ und „Standard“
  * C0 (250 MB) – bis zu 16 Datenbanken
  * C1 (1 GB) – bis zu 16 Datenbanken
  * C2 (2,5 GB) – bis zu 16 Datenbanken
  * C3 (6 GB) – bis zu 16 Datenbanken
  * C4 (13 GB) – bis zu 32 Datenbanken
  * C5 (26 GB) – bis zu 48 Datenbanken
  * C6 (53 GB) – bis zu 64 Datenbanken
* Premium-Caches
  * P1 (6 bis 60 GB) – bis zu 16 Datenbanken
  * P2 (13 bis 130 GB) – bis zu 32 Datenbanken
  * P3 (26 bis 260 GB) – bis zu 48 Datenbanken
  * P4 (53 bis 530 GB) – bis zu 64 Datenbanken
  * Alle Premium-Caches mit aktiviertem Redis-Cluster: Ein Redis-Cluster unterstützt nur die Nutzung der Datenbank 0. Deshalb ist der `databases`-Grenzwert für Premium-Caches mit aktiviertem Redis-Cluster effektiv 1, und der Befehl [Auswählen](http://redis.io/commands/select) ist nicht zulässig. Weitere Informationen finden Sie unter [Muss ich Änderungen an meiner Clientanwendung vornehmen, um Clustering verwenden zu können?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)

Weitere Informationen zu Datenbanken finden Sie unter [Was sind Redis-Datenbanken?](cache-faq.md#what-are-redis-databases)

> [!NOTE]
> Mit den Einstellungen `databases` kann nur bei der Erstellung des Caches konfiguriert und nur mit PowerShell, der Befehlszeilenschnittstelle oder anderen Verwaltungsclients verwendet werden. Ein Beispiel für das Konfigurieren von `databases` bei der Cacheerstellung mithilfe von PowerShell finden Sie unter [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).
> 
> 

<a name="maxclients"></a> 
<sup>2</sup>`maxclients` ist für jeden Azure Redis Cache-Tarif unterschiedlich.

* Caches vom Typ „Basic“ und „Standard“
  * Cache C0 (250 MB) – bis zu 256 Verbindungen
  * Cache C1 (1 GB) – bis zu 1.000 Verbindungen
  * Cache C2 (2,5 GB) – bis zu 2.000 Verbindungen
  * Cache C3 (6 GB) – bis zu 5.000 Verbindungen
  * Cache C4 (13 GB) – bis zu 10.000 Verbindungen
  * Cache C5 (26 GB) – bis zu 15.000 Verbindungen
  * Cache C6 (53 GB) – bis zu 20.000 Verbindungen
* Premium-Caches
  * P1 (6 GB - 60 GB) – bis zu 7.500 Verbindungen
  * P2 (13 GB - 130 GB) – bis zu 15.000 Verbindungen
  * P3 (26 GB - 260 GB) – bis zu 30.000 Verbindungen
  * P4 (53 GB - 530 GB) – bis zu 40.000 Verbindungen

> [!NOTE]
> Obgleich jede Cachegröße eine *bestimmte* Anzahl von Verbindungen zulässt, fällt für jede Verbindung ein Mehraufwand an. Ein Beispiel für einen solchen Aufwand ist die CPU- und Arbeitsspeicherauslastung aufgrund der TLS-/SSL-Verschlüsselung. Das maximale Verbindungslimit für eine angegebene Cachegröße geht von einem geringfügig ausgelasteten Cache aus. Wenn die Last des Verbindungsaufwands *plus* die Last von Clientvorgängen die Systemkapazität überschreiten, können im Cache Kapazitätsprobleme entstehen, auch wenn Sie das Verbindungslimit für die aktuelle Cachegröße nicht überschritten haben.
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Redis-Befehle, die in Azure Redis Cache nicht unterstützt werden.
> [!IMPORTANT]
> Da die Konfiguration und Verwaltung von Azure Redis Cache-Instanzen von Microsoft verwaltet wird, sind die folgenden Befehle deaktiviert. Wenn Sie versuchen, sie aufzurufen, erhalten Sie etwa folgende Fehlermeldung: `"(error) ERR unknown command"`.
> 
> * BGREWRITEAOF
> * BGSAVE
> * CONFIG
> * DEBUG
> * MIGRATE
> * Speichern
> * SHUTDOWN
> * SLAVEOF
> * CLUSTER: Clusterschreibbefehle sind deaktiviert, aber schreibgeschützte Clusterbefehle sind zulässig.
> 
> 

Weitere Informationen zu Redis-Befehlen finden Sie unter [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Redis-Konsole
Über die **Redis-Konsole**, die für Caches vom Typ "Standard" und "Premium" zur Verfügung steht, können Sie Befehle sicher auf Ihre Azure Redis Cache-Instanzen anwenden.

> [!IMPORTANT]
> Die Redis-Konsole kann nicht mit [VNET](cache-how-to-premium-vnet.md) verwendet werden. Wenn der Cache zu einem virtuellen Netzwerk gehört, haben nur Clients in diesem virtuellen Netzwerk Zugriff auf den Cache. Da die Redis-Konsole in Ihrem lokalen Browser ausgeführt wird, der sich außerhalb des VNET befindet, kann sie keine Verbindung mit Ihrem Cache herstellen.
> 
> 

Um auf die Redis-Konsole zuzugreifen, klicken Sie auf dem Blatt **Redis-Cache** auf **Konsole**.

![Redis-Konsole](./media/cache-configure/redis-console-menu.png)

Zum Anwenden von Befehlen auf Ihre Cache-Instanz geben Sie einfach den gewünschten Befehl in die Konsole ein.

![Redis-Konsole](./media/cache-configure/redis-console.png)

Eine Liste der Redis-Befehle, die für Azure Redis Cache deaktiviert sind, finden Sie im vorherigen Abschnitt [Redis-Befehle, die in Azure Redis Cache nicht unterstützt werden](#redis-commands-not-supported-in-azure-redis-cache) . Weitere Informationen zu Redis-Befehlen finden Sie unter [http://redis.io/commands](http://redis.io/commands). 

## <a name="move-your-cache-to-a-new-subscription"></a>Verschieben Ihres Caches in ein neues Abonnement
Sie können Ihren Cache in ein neues Abonnement verschieben, indem Sie auf **Verschieben**klicken.

![Verschieben von Redis Cache](./media/cache-configure/redis-cache-move.png)

Informationen zum Verschieben von Ressourcen zwischen Ressourcengruppen und zwischen Abonnements finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md).

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Verwenden von Redis-Befehlen finden Sie unter [Wie führe ich Redis-Befehle aus?](cache-faq.md#how-can-i-run-redis-commands)


