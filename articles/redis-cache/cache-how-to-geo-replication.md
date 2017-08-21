---
title: "Vorgehensweise zum Konfigurieren der Georeplikation für Azure Redis Cache | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Ihre Azure Redis Cache-Instanzen über geografische Regionen hinweg repliziert werden."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 375643dc-dbac-4bab-8004-d9ae9570440d
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: sdanie
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 71b0d4add7e642487f6d67cda692c500ee78b0e6
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# <a name="how-to-configure-geo-replication-for-azure-redis-cache"></a>Vorgehensweise zum Konfigurieren der Georeplikation für Azure Redis Cache

Die Georeplikation bietet einen Mechanismus zum Verknüpfen von zwei Azure Redis Cache-Instanzen im Premium-Tarif. Ein Cache wird als primärer verknüpfter Cache festgelegt und der andere als sekundärer verknüpfter Cache. Der sekundäre verknüpfte Cache ist schreibgeschützt, und die in den primären Cache geschriebenen Daten werden im sekundären verknüpften Cache repliziert. Über diese Funktion kann ein Cache über verschiedene Azure-Regionen hinweg repliziert werden. Dieser Artikel enthält eine Anleitung zum Konfigurieren der Georeplikation für Ihre Azure Redis Cache-Instanzen im Premium-Tarif.

## <a name="geo-replication-prerequisites"></a>Voraussetzungen für die Georeplikation

Um die Georeplikation zwischen zwei Caches zu konfigurieren, müssen die folgenden Voraussetzungen erfüllt sein:

- Beide Caches müssen den [Premium-Tarif](cache-premium-tier-intro.md) aufweisen.
- Beide Caches müssen sich im selben Azure-Abonnement befinden.
- Der verknüpfte sekundäre Cache muss entweder denselben Tarif wie der verknüpfte primäre Cache oder einen höheren Tarif aufweisen.
- Wenn für den primären verknüpften Cache das Clustering aktiviert ist, muss das Clustering für den sekundären verknüpften Cache mit derselben Anzahl von Shards wie beim primären verknüpften Cache aktiviert werden.
- Beide Caches müssen erstellt und ausgeführt werden.
- Für keinen der Caches darf Persistenz aktiviert sein.
- Es wird Unterstützung für die Georeplikation zwischen Caches im selben VNET geboten. Die Georeplikation zwischen Caches in verschiedenen VNETs wird ebenfalls unterstützt, solange beide VNETs so konfiguriert sind, dass Ressourcen in den VNETs über TCP-Verbindungen eine Verbindung herstellen können.

Nach der Konfiguration der Georeplikation gelten folgende Einschränkungen für Ihr verknüpftes Cachepaar:

- Der sekundäre verknüpfte Cache ist schreibgeschützt: Sie können nur Daten lesen, jedoch keine Daten darin schreiben. 
- Alle Daten, die vor dem Hinzufügen der Verknüpfung im sekundären verknüpften Cache enthalten waren, werden entfernt. Wenn die Georeplikation jedoch anschließend entfernt wird, verbleiben die replizierten Daten im sekundären verknüpften Cache.
- Wenn für den Cache das Clustering aktiviert ist, können Sie für keinen der Caches einen [Skalierungsvorgang](cache-how-to-scale.md) initiieren oder die [Anzahl von Shards ändern](cache-how-to-premium-clustering.md).
- Für keinen der Caches kann Persistenz aktiviert werden.
- Sie können bei beiden Caches die Funktion [Exportieren](cache-how-to-import-export-data.md#export) verwenden, die Funktion [Importieren](cache-how-to-import-export-data.md#import) jedoch nur beim primären verknüpften Cache.
- Sie können die verknüpften Caches oder die Ressourcengruppe, die diese enthält, erst löschen, wenn Sie die Verknüpfung für die Georeplikation entfernt haben. Weitere Informationen finden Sie unter [Warum ist bei dem Versuch, meinen verknüpften Cache zu löschen, ein Fehler beim Vorgang aufgetreten?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- Wenn sich beide Caches in unterschiedlichen Regionen befinden, fallen Kosten für ausgehenden Netzwerkdatenverkehr für die Daten an, die über verschiedene Regionen hinweg im sekundären verknüpften Cache repliziert wurden. Weitere Informationen finden Sie unter [Wie viel kostet die Replikation meiner Daten über verschiedene Azure-Regionen hinweg?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- Es ist kein automatisches Failover zum sekundären verknüpften Cache vorhanden, wenn der primäre Cache (und sein Replikat) ausfällt. Um ein Failover für Clientanwendungen durchführen zu können, müssten Sie die Verknüpfung für die Georeplikation manuell entfernen und die Clientanwendungen auf den Cache verweisen, der zuvor als sekundärer verknüpfter Cache fungierte. Weitere Informationen finden Sie unter [Wie funktioniert ein Failover zum sekundären verknüpften Cache?](#how-does-failing-over-to-the-secondary-linked-cache-work)

## <a name="add-a-geo-replication-link"></a>Hinzufügen einer Verknüpfung für die Georeplikation

1. Um zwei Premium-Caches für die Georeplikation miteinander zu verknüpfen, klicken Sie im Ressourcenmenü des Caches, der als primärer verknüpfter Cache vorgesehen ist, auf **Georeplikation**, und klicken Sie dann auf dem Blatt **Georeplikation** auf **Cachereplikationsverknüpfung hinzufügen**.

    ![Hinzufügen einer Verknüpfung](./media/cache-how-to-geo-replication/cache-geo-location-menu.png)

2. Klicken Sie in der Liste **Kompatible Caches** auf den Namen des jeweiligen sekundären Caches. Wenn der gewünschte Cache nicht in der Liste angezeigt wird, stellen Sie sicher, dass die [Voraussetzungen für die Georeplikation](#geo-replication-prerequisites) für den gewünschten sekundären Cache erfüllt sind. Klicken Sie zum Filtern der Caches nach Region auf der Karte auf die gewünschte Region, um nur Caches in der Liste **Kompatible Caches** anzuzeigen.

    ![Für die Georeplikation kompatible Caches](./media/cache-how-to-geo-replication/cache-geo-location-select-link.png)
    
    Sie können mithilfe des Kontextmenüs auch den Verknüpfungsvorgang initiieren oder Details zum sekundären Cache anzeigen.

    ![Kontextmenü für die Georeplikation](./media/cache-how-to-geo-replication/cache-geo-location-select-link-context-menu.png)

3. Klicken Sie auf **Verknüpfen**, um zwei Caches zu verknüpfen und den Replikationsvorgang zu starten.

    ![Verknüpfen von Caches](./media/cache-how-to-geo-replication/cache-geo-location-confirm-link.png)

4. Auf dem Blatt **Georeplikation** können Sie den Status des Replikationsvorgangs sehen.

    ![Verknüpfungsstatus](./media/cache-how-to-geo-replication/cache-geo-location-linking.png)

    Auf dem Blatt **Übersicht** können Sie auch den Verknüpfungsstatus für den primären und sekundären Cache anzeigen.

    ![Cachestatus](./media/cache-how-to-geo-replication/cache-geo-location-link-status.png)

    Nach Abschluss des Replikationsvorgangs wechselt der **Verknüpfungsstatus** zu **Erfolgreich**.

    ![Cachestatus](./media/cache-how-to-geo-replication/cache-geo-location-link-successful.png)

    Während des Verknüpfungsvorgangs ist der verknüpfte primäre Cache nach wie vor für die Verwendung verfügbar. Der sekundäre verknüpfte Cache dagegen ist erst verfügbar, wenn der Verknüpfungsvorgang abgeschlossen ist.

## <a name="remove-a-geo-replication-link"></a>Entfernen einer Verknüpfung für die Georeplikation

1. Um die Verknüpfung zwischen zwei Caches zu entfernen und die Georeplikation zu beenden, klicken Sie auf dem Blatt **Georeplikation** auf **Verknüpfung von Caches aufheben**.
    
    ![Aufheben der Verknüpfung von Caches](./media/cache-how-to-geo-replication/cache-geo-location-unlink.png)

    Wenn der Vorgang zur Aufhebung der Verknüpfung abgeschlossen ist, ist der sekundäre Cache für Lese- und Schreibvorgänge verfügbar.

>[!NOTE]
>Wenn die Verknüpfung für die Georeplikation entfernt wird, bleiben die replizierten Daten aus dem primären verknüpften Cache im sekundären Cache.
>
>

## <a name="geo-replication-faq"></a>FAQs zur Georeplikation

- [Kann ich die Georeplikation bei einem Cache mit Standard- oder Basic-Tarif verwenden?](#can-i-use-geo-replication-with-a-standard-or-basic-tier-cache)
- [Ist mein Cache während des Vorgangs zur Verknüpfung oder Aufhebung der Verknüpfung verfügbar?](#is-my-cache-available-for-use-during-the-linking-or-unlinking-process)
- [Kann ich mehr als zwei Caches miteinander verknüpfen?](#can-i-link-more-than-two-caches-together)
- [Kann ich zwei Caches aus verschiedenen Azure-Abonnements verknüpfen?](#can-i-link-two-caches-from-different-azure-subscriptions)
- [Kann ich zwei Caches von unterschiedlicher Größe verknüpfen?](#can-i-link-two-caches-with-different-sizes)
- [Kann ich die Georeplikation mit aktiviertem Clustering verwenden?](#can-i-use-geo-replication-with-clustering-enabled)
- [Kann ich die Georeplikation bei meinen Caches in einem VNET verwenden?](#can-i-use-geo-replication-with-my-caches-in-a-vnet)
- [Kann ich die Georeplikation mit PowerShell oder der Azure CLI verwalten?](#can-i-use-powershell-or-azure-cli-to-manage-geo-replication)
- [Wie viel kostet die Replikation meiner Daten über verschiedene Azure-Regionen hinweg?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- [Warum ist bei dem Versuch, meinen verknüpften Cache zu löschen, ein Fehler beim Vorgang aufgetreten?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- [Welche Region sollte ich für meinen sekundären verknüpften Cache verwenden?](#what-region-should-i-use-for-my-secondary-linked-cache)
- [Wie funktioniert ein Failover zum sekundären verknüpften Cache?](#how-does-failing-over-to-the-secondary-linked-cache-work)

### <a name="can-i-use-geo-replication-with-a-standard-or-basic-tier-cache"></a>Kann ich die Georeplikation bei einem Cache mit Standard- oder Basic-Tarif verwenden?

Nein, die Georeplikation ist nur für Caches im Premium-Tarif verfügbar.

### <a name="is-my-cache-available-for-use-during-the-linking-or-unlinking-process"></a>Ist mein Cache während des Vorgangs zur Verknüpfung oder Aufhebung der Verknüpfung verfügbar?

- Während der Verknüpfung zweier Caches ist der verknüpfte primäre Cache nach wie vor für die Verwendung verfügbar. Der sekundäre verknüpfte Cache dagegen ist erst verfügbar, wenn der Verknüpfungsvorgang abgeschlossen ist.
- Wenn Sie die Verknüpfung für die Georeplikation zwischen zwei Caches entfernen, sind beide Caches nach wie vor für die Verwendung verfügbar.

### <a name="can-i-link-more-than-two-caches-together"></a>Kann ich mehr als zwei Caches miteinander verknüpfen?

Nein, bei der Georeplikation können nur zwei Caches miteinander verknüpft werden.

### <a name="can-i-link-two-caches-from-different-azure-subscriptions"></a>Kann ich zwei Caches aus verschiedenen Azure-Abonnements verknüpfen?

Nein, beide Caches müssen sich im selben Azure-Abonnement befinden.

### <a name="can-i-link-two-caches-with-different-sizes"></a>Kann ich zwei Caches von unterschiedlicher Größe verknüpfen?

Ja, solange der sekundäre verknüpfte Cache größer ist als der primäre verknüpfte Cache.

### <a name="can-i-use-geo-replication-with-clustering-enabled"></a>Kann ich die Georeplikation mit aktiviertem Clustering verwenden?

Ja, solange beide Caches über dieselbe Anzahl von Shards verfügen.

### <a name="can-i-use-geo-replication-with-my-caches-in-a-vnet"></a>Kann ich die Georeplikation bei meinen Caches in einem VNET verwenden?

Ja, die Georeplikation von Caches in VNETs wird unterstützt. 

- Es wird Unterstützung für die Georeplikation zwischen Caches im selben VNET geboten.
- Die Georeplikation zwischen Caches in verschiedenen VNETs wird ebenfalls unterstützt, solange beide VNETs so konfiguriert sind, dass Ressourcen in den VNETs über TCP-Verbindungen eine Verbindung herstellen können.

### <a name="can-i-use-powershell-or-azure-cli-to-manage-geo-replication"></a>Kann ich die Georeplikation mit PowerShell oder der Azure CLI verwalten?

Gegenwärtig kann die Georeplikation nur über das Azure-Portal verwaltet werden.

### <a name="how-much-does-it-cost-to-replicate-my-data-across-azure-regions"></a>Wie viel kostet die Replikation meiner Daten über verschiedene Azure-Regionen hinweg?

Beim Einsatz der Georeplikation werden Daten aus dem primären verknüpften Cache in den sekundären verknüpften Cache repliziert. Wenn sich beide verknüpfte Caches in derselben Azure-Region befinden, fallen keine Gebühren für die Datenübertragung an. Wenn sich beide verknüpfte Caches in verschiedenen Azure-Regionen befinden, fallen als Gebühr für die Datenübertragung bei der Georeplikation die Bandbreitenkosten für die Replikation dieser Daten in die andere Azure-Region an. Weitere Informationen finden Sie unter [Bandbreite – Preisdetails](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache"></a>Warum ist bei dem Versuch, meinen verknüpften Cache zu löschen, ein Fehler beim Vorgang aufgetreten?

Wenn zwei Caches miteinander verknüpft sind, können Sie die Caches oder die Ressourcengruppe, die diese enthält, erst löschen, wenn Sie die Verknüpfung für die Georeplikation entfernt haben. Wenn Sie versuchen, die Ressourcengruppe zu löschen, die einen oder beide verknüpfte Caches enthält, werden die anderen Ressourcen in der Ressourcengruppe gelöscht, die Ressourcengruppe bleibt jedoch im Status `deleting` und alle verknüpften Caches in der Ressourcengruppe im Status `running`. Um die Löschung der Ressourcengruppe und der darin enthaltenen verknüpften Caches durchzuführen, heben Sie die Verknüpfung für die Georeplikation auf, wie unter [Entfernen einer Verknüpfung für die Georeplikation](#remove-a-geo-replication-link) beschrieben wird.

### <a name="what-region-should-i-use-for-my-secondary-linked-cache"></a>Welche Region sollte ich für meinen sekundären verknüpften Cache verwenden?

Grundsätzlich sollte sich Ihr Cache in derselben Azure-Region befinden wie die der Anwendung, die darauf zugreift. Wenn Ihre Anwendung über eine primäre und eine Fallbackregion verfügt, sollten sich Ihre primären und sekundären Caches in denselben Regionen befinden. Weitere Informationen zu gekoppelten Regionen finden Sie unter [Best Practices – gekoppelte Azure-Regionen](../best-practices-availability-paired-regions.md).

### <a name="how-does-failing-over-to-the-secondary-linked-cache-work"></a>Wie funktioniert ein Failover zum sekundären verknüpften Cache?

Im ersten Release der Georeplikation bietet Azure Redis Cache keine Unterstützung für automatisches Failover über verschiedene Azure-Regionen hinweg. Die Georeplikation wird in erster Linie in einem Notfallwiederherstellungsszenario verwendet. In einem Notfallwiederherstellungsszenario sollten Kunden den gesamten Anwendungsstapel koordiniert in einer zusätzlichen Region platzieren, statt einzelnen Anwendungskomponenten die Entscheidung zu überlassen, wann der Wechsel zu ihren Sicherungen erfolgen soll. Dies ist insbesondere bei Redis von entscheidender Bedeutung. Einer der wesentlichen Vorteile bei Redis besteht darin, dass es sich um einen Speicher mit sehr geringen Wartezeiten handelt. Wenn für eine Redis-Instanz, die von einer Anwendung verwendet wird, ein Failover zu einer anderen Azure-Region durchgeführt wird, jedoch nicht für die Computeschicht, würde sich die zusätzliche Roundtripzeit deutlich auf die Leistung auswirken. Aus diesem Grund sollte ein automatisches Failover durch Redis aufgrund von vorübergehenden Verfügbarkeitsproblemen vermieden werden.

Um das Failover zu initiieren, müssen Sie gegenwärtig die Verknüpfung für die Georeplikation im Azure-Portal entfernen und dann den Verbindungsendpunkt im Redis-Client zwischen den primären verknüpften Cache und dem (ehemals verknüpften) sekundären Cache ändern. Wenn die Zuordnung beider Caches aufgehoben wurde, fungiert das Replikat wieder als Cache mit regulärem Lese-/Schreibzugriff und akzeptiert direkt Anforderungen von Redis-Clients.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [Premium-Tarif von Azure Redis Cache](cache-premium-tier-intro.md).


