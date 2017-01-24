---
title: "Konfigurieren von Datenpersistenz für Azure Redis Cache vom Typ &quot;Premium&quot;"
description: "Erfahren Sie, wie Sie die Datenpersistenz für Ihren Premium Azure Redis Cache mit Premium-Tarif konfigurieren und verwalten."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: b01cf279-60a0-4711-8c5f-af22d9540d38
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 65385aa918222837468f88246d0527c22c677ba7
ms.openlocfilehash: 628dfc48c00e61fe5c4883f6237a44e0f1309f0e


---
# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Konfigurieren von Datenpersistenz für Azure Redis Cache vom Typ "Premium"
Für Azure Redis Cache stehen verschiedene Cacheangebote bereit, die Flexibilität bei der Auswahl von Cachegröße und -features bieten, einschließlich des neuen Premium-Tarifs.

Der Premium-Tarif für Azure Redis Cache umfasst Clustering, Persistenz und Unterstützung virtueller Netzwerke. In diesem Artikel wird erläutert, wie die Persistenz in einer Azure Redis Cache-Instanz vom Typ "Premium" konfiguriert wird.

Weitere Informationen zu anderen Premium-Cache-Features finden Sie unter [Einführung in den Premium-Tarif von Azure Redis Cache](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Was ist Datenpersistenz?
Mithilfe der Redis-Persistenz können Sie die in Redis gespeicherten Daten dauerhaft speichern. Sie können zudem Momentaufnahmen erstellen und die Daten sichern, die Sie dann im Fall eines Hardwarefehlers laden können. Dies ist ein großer Vorteil gegenüber dem Basic- oder Standard-Tarif, bei denen alle Daten im Arbeitsspeicher gespeichert sind, sodass bei einem Fehler, bei dem Cacheknoten ausfallen, möglicherweise Daten verloren gehen. 

Azure Redis Cache bietet mit dem [RDB-Modell](http://redis.io/topics/persistence)Redis-Persistenz, bei der die Daten in einem Azure-Speicherkonto gespeichert werden. Wenn Persistenz konfiguriert ist, speichert Azure Redis Cache basierend auf einer wählbaren Sicherungshäufigkeit eine Momentaufnahme des Redis-Caches in einem binären Redis-Format dauerhaft auf dem Datenträger. Bei einem schwerwiegenden Fehler, bei dem der primäre sowie der Replikatcache deaktiviert werden, wird der Cache mithilfe der neuesten Momentaufnahme wiederhergestellt.

Persistenz kann auf dem Blatt **Neuer Redis-Cache** während der Erstellung des Caches sowie für vorhandene Premium-Caches auf dem Blatt **Einstellungen** konfiguriert werden.

## <a name="create-a-premium-cache"></a>Erstellen eines Premium-Caches
Um einen Cache zu erstellen und die Persistenz zu konfigurieren, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Neu**->**Daten und Speicher**>**Redis-Cache**.

![Erstellen eines Redis-Cache][redis-cache-new-cache-menu]

Zum Konfigurieren der Persistenz wählen Sie zunächst auf dem Blatt **Wählen Sie Ihren Tarif** eine der **Premium**-Optionen für den Cache aus.

![Tarif auswählen][redis-cache-premium-pricing-tier]

Wenn Sie einen Premium-Tarif ausgewählt haben, klicken Sie auf **Redis persistence**.

![Redis persistence][redis-cache-persistence]

Die Schritte im folgenden Abschnitt beschreiben, wie Sie Redis-Persistenz für Ihren neuen Premium-Cache konfigurieren. Nachdem Redis-Persistenz konfiguriert wurde, klicken Sie auf **Erstellen** , um den neuen Premium-Cache mit Redis-Persistenz zu erstellen.

## <a name="configure-redis-persistence"></a>Konfigurieren von Redis-Persistenz
Redis-Persistenz wird auf dem Blatt **Redis-Datenpersistenz** konfiguriert. Bei einem neuen Cache wird während der Erstellung des Caches auf dieses Blatt zugegriffen, wie im vorherigen Abschnitt beschrieben. Für vorhandene Caches erfolgt der Zugriff auf das Blatt **Redis-Datenpersistenz** über das Blatt **Einstellungen** für Ihren Cache.

![Redis-Einstellungen][redis-cache-settings]

Klicken Sie auf **Aktiviert** , um die Redis-Datenbanksicherung zu aktivieren. Um Redis-Persistenz für einen zuvor aktivierten Premium-Cache zu deaktivieren, klicken Sie auf **Deaktiviert**.

Zum Konfigurieren des Sicherungsintervalls wählen Sie in der Dropdownliste eine **Sicherungshäufigkeit** aus. Zur Auswahl stehen **15 Minuten**, **30 Minuten**, **60 Minuten**, **6 Stunden**, **12 Stunden** und **24 Stunden**. Dieses Intervall wird ab dem Moment rückwärts gezählt, an dem der vorherige Sicherungsvorgang erfolgreich abgeschlossen wird. Wenn das Intervall abgelaufen ist, wird eine neue Sicherung gestartet.

Klicken Sie auf **Speicherkonto**, um das Speicherkonto auszuwählen, und wählen Sie entweder den **primären** oder den **sekundären Schlüssel** aus der Dropdownliste **Speicherschlüssel** aus. Sie müssen ein Speicherkonto auswählen, das aus der gleichen Region wie der Cache stammt, und wir empfehlen ein **Storage Premium** -Konto, da dieses einen höheren Durchsatz aufweist. 

> [!IMPORTANT]
> Wenn der Speicherschlüssel für Ihr Persistenzkonto neu generiert wird, müssen Sie den gewünschten Schlüssel erneut in der Dropdownliste **Speicherschlüssel** auswählen.
> 
> 

![Redis persistence][redis-cache-persistence-selected]

Klicken Sie auf **OK** , um die Persistenzkonfiguration zu speichern.

Die nächste Sicherung (oder erste Sicherung bei neuen Caches) wird gestartet, sobald das Intervall für die Sicherungshäufigkeit abgelaufen ist.

## <a name="persistence-faq"></a>Persistenz – häufig gestellte Fragen
Die folgende Liste enthält Antworten auf häufig gestellte Fragen zur Persistenz in Azure Redis Cache.

* [Kann ich die Persistenz für einen bereits erstellten Cache aktivieren?](#can-i-enable-persistence-on-a-previously-created-cache)
* [Kann ich die Sicherungshäufigkeit ändern, nachdem ich den Cache erstellt habe?](#can-i-change-the-backup-frequency-after-i-create-the-cache)
* [Warum verstreichen mehr als 60 Minuten zwischen den Sicherungen, wenn ich eine Sicherungshäufigkeit von 60 Minuten festgelegt habe?](#why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
* [Was geschieht mit den alten Sicherungen, wenn eine neue Sicherung durchgeführt wird?](#what-happens-to-the-old-backups-when-a-new-backup-is-made)
* [Was geschieht, wenn ich auf eine andere Größe skaliert habe und eine Wiederherstellung aus einer Sicherung vorgenommen wird, die vor der Skalierung erstellt wurde?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Kann ich die Persistenz für einen bereits erstellten Cache aktivieren?
Ja, die Redis-Persistenz kann sowohl bei der Erstellung des Caches als auch für vorhandene Premium-Caches konfiguriert werden.

### <a name="can-i-change-the-backup-frequency-after-i-create-the-cache"></a>Kann ich die Sicherungshäufigkeit ändern, nachdem ich den Cache erstellt habe?
Ja, Sie können die Sicherungshäufigkeit auf dem Blatt **Redis-Datenpersistenz** ändern. Anweisungen dazu finden Sie unter [Konfigurieren von Redis-Persistenz](#configure-redis-persistence).

### <a name="why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Warum verstreichen mehr als 60 Minuten zwischen den Sicherungen, wenn ich eine Sicherungshäufigkeit von 60 Minuten festgelegt habe?
Das Intervall für die Sicherungshäufigkeit beginnt erst, nachdem der vorherige Sicherungsvorgang erfolgreich abgeschlossen wurde. Wenn für die Sicherungshäufigkeit 60 Minuten festgelegt sind und der Sicherungsvorgang nach 15 Minuten erfolgreich beendet wird, wird der nächste Sicherungsvorgang 75 Minuten nach der Startzeit des vorherigen Sicherungsvorgangs gestartet.

### <a name="what-happens-to-the-old-backups-when-a-new-backup-is-made"></a>Was geschieht mit den alten Sicherungen, wenn eine neue Sicherung durchgeführt wird?
Alle Sicherungen, mit Ausnahme der jeweils letzten Sicherung, werden automatisch gelöscht. Dieser Löschvorgang wird möglicherweise nicht sofort durchgeführt, ältere Sicherungen werden jedoch nicht dauerhaft gespeichert.

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Was geschieht, wenn ich auf eine andere Größe skaliert habe und eine Wiederherstellung aus einer Sicherung vorgenommen wird, die vor der Skalierung erstellt wurde?
* Eine Skalierung auf eine größere Größe hat keinerlei Auswirkungen.
* Wenn Sie auf eine kleinere Größe skaliert haben und eine benutzerdefinierte Einstellung für die [Datenbanken](cache-configure.md#databases) verwenden, die größer ist als der [Grenzwert für Datenbanken](cache-configure.md#databases) bei der neuen Größe, werden die Daten in diesen Datenbanken nicht wiederhergestellt. Weitere Informationen finden Sie unter [Hat die Skalierung Auswirkungen auf meine benutzerdefinierte Einstellung für Datenbanken?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
* Wenn Sie auf eine kleinere Größe skaliert haben und dort nicht genug Platz für alle Daten aus der letzten Sicherung ist, werden beim Wiederherstellungsvorgang Schlüssel entfernt. Diese Entfernung wird in der Regel mithilfe der Entfernungsrichtlinie [allkeys-lru](http://redis.io/topics/lru-cache) vorgenommen.

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Verwendung weiterer Funktionen des Premium-Caches finden Sie in den folgenden Artikeln.

* [Einführung in den Premium-Tarif von Azure Redis Cache](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-persistence/redis-cache-new-cache-menu.png

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-persistence-selected]: ./media/cache-how-to-premium-persistence/redis-cache-persistence-selected.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png



<!--HONumber=Jan17_HO2-->


