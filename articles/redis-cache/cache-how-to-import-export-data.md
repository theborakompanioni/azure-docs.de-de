---
title: Importieren und Exportieren von Daten in Azure Redis Cache | Microsoft Docs
description: "Enthält Informationen zum Importieren und Exportieren von Daten in und aus dem Blob-Speicher mit Azure Redis Cache-Instanzen des Premium-Tarifs."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 4a68ac38-87af-4075-adab-569d37d7cc9e
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 65385aa918222837468f88246d0527c22c677ba7
ms.openlocfilehash: a2cf3abae986d2ab2243a4b2a9123e4c164d2ef1


---
# <a name="import-and-export-data-in-azure-redis-cache"></a>Importieren und Exportieren von Daten in Azure Redis Cache
Import/Export ist ein Vorgang der Azure Redis Cache-Datenverwaltung, bei dem Sie Daten in Azure Redis Cache importieren oder aus Azure Redis Cache exportieren können. Hierzu importieren bzw. exportieren Sie eine Momentaufnahme der Redis Cache-Datenbank (RDB) aus einem Premium-Cache in ein Seitenblob in einem Azure-Speicherkonto. So können Sie zwischen verschiedenen Azure Redis Cache-Instanzen migrieren oder den Cache vor der Nutzung mit Daten auffüllen.

Dieser Artikel enthält eine Anleitung zum Importieren und Exportieren von Daten mit Azure Redis Cache und Antworten auf häufig gestellte Fragen.

> [!IMPORTANT]
> Import/Export befindet sich in der Vorschauphase und ist nur für Caches des [Premium-Tarifs](cache-premium-tier-intro.md) verfügbar.
>
>

## <a name="import"></a>Importieren
Die Importfunktion kann verwendet werden, um Redis-kompatible RDB-Dateien von beliebigen Redis-Servern zu importieren, die in einer Cloud oder Umgebung ausgeführt werden, z.B. Redis unter Linux oder Windows oder bei einem Cloudanbieter wie Amazon Web Services und anderen. Das Importieren von Daten ist eine einfache Möglichkeit zum Erstellen eines Cache mit vorab aufgefüllten Daten. Während des Importvorgangs lädt Azure Redis Cache die RDB-Dateien aus dem Azure-Speicher in den Arbeitsspeicher und fügt die Schlüssel anschließend in den Cache ein.

> [!NOTE]
> Stellen Sie vor Beginn des Importvorgangs sicher, dass die Redis-Datenbankdatei(en) in Seitenblobs in einen Azure-Speicher hochgeladen wurde(n), der sich in derselben Region und im gleichen Abonnement wie Ihre Azure Redis Cache-Instanz befindet. Weitere Informationen finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](../storage/storage-dotnet-how-to-use-blobs.md). Wenn Sie die RDB-Datei mit der [Azure Redis Cache-Exportfunktion](#export) exportiert haben, ist Ihre RDB-Datei bereits in einem Seitenblob gespeichert und für den Import bereit.
>
>

1. Um ein oder mehrere exportierte Cacheblobs zu importieren, [navigieren Sie im Azure-Portal zum Cache](cache-configure.md#configure-redis-cache-settings), und klicken Sie auf dem Blatt **Einstellungen** der Cacheinstanz auf **Daten importieren**.

    ![Daten importieren][cache-import-data]
2. Klicken Sie auf **Blob(s) auswählen** , und wählen Sie das Speicherkonto aus, das die zu importierenden Daten enthält.

    ![Speicherkonto auswählen][cache-import-choose-storage-account]
3. Klicken Sie auf den Container, der die zu importierenden Daten enthält.

    ![Container auswählen][cache-import-choose-container]
4. Wählen Sie ein oder mehrere Blobs für den Import aus, indem Sie jeweils auf den Bereich links vom Blobnamen und dann auf **Auswählen**klicken.

    ![Blobs auswählen][cache-import-choose-blobs]
5. Klicken Sie auf **Importieren** , um den Importvorgang zu starten.

   > [!IMPORTANT]
   > Während des Importvorgangs können Cacheclients nicht auf den Cache zugreifen, und alle im Cache enthaltenen Daten werden gelöscht.
   >
   >

    ![Importieren][cache-import-blobs]

    Sie können den Status des Importvorgangs überwachen, indem Sie die Benachrichtigungen im Azure-Portal verfolgen oder die Ereignisse im Überwachungsprotokoll anzeigen. Weitere Informationen finden Sie im Abschnitt „Einstellungen zu Support und Problembehandlung“ des Artikels [Gewusst wie: Konfigurieren von Azure Redis Cache](cache-configure.md).

    ![Importstatus][cache-import-data-import-complete]

## <a name="export"></a>Export
Mit der Exportfunktion können Sie die in Azure Redis Cache gespeicherten Daten als Redis-kompatible RDB-Dateien exportieren. Sie können diese Funktion nutzen, um Daten von einer Azure Redis Cache-Instanz zur anderen oder auf einen anderen Redis-Server zu verschieben. Während des Exportvorgangs wird auf dem virtuellen Computer, der die Azure Redis Cache-Serverinstanz hostet, eine temporäre Datei erstellt, die dann in das angegebene Speicherkonto hochgeladen wird. Nachdem der Exportvorgang mit dem Status „Erfolg“ oder „Fehler“ abgeschlossen wurde, wird die temporäre Datei gelöscht.

1. Um den aktuellen Inhalt des Caches in den Speicher zu exportieren, [navigieren Sie im Azure-Portal zum Cache](cache-configure.md#configure-redis-cache-settings) und klicken im Blatt **Einstellungen** der Cacheinstanz auf **Daten exportieren**.

    ![Speichercontainer auswählen][cache-export-data-choose-storage-container]
2. Klicken Sie auf **Speichercontainer auswählen** , und wählen Sie das gewünschte Speicherkonto aus. Das Speicherkonto muss sich in demselben Abonnement und derselben Region wie der Cache befinden.

   > [!IMPORTANT]
   > Der Import/Export funktioniert in Verbindung mit Seitenblobs. Diese werden von klassischen Speicherkonten und ARM-Speicherkonten unterstützt, aber derzeit noch nicht von [Blob Storage-Konten](../storage/storage-blob-storage-tiers.md#blob-storage-accounts).
   >
   >

    ![Speicherkonto][cache-export-data-choose-account]
3. Markieren Sie den gewünschten Blobcontainer, und klicken Sie auf **Auswählen**. Klicken Sie zum Verwenden eines neuen Containers auf **Container hinzufügen** , damit Sie diesen zuerst hinzufügen und dann in der Liste auswählen können.

    ![Speichercontainer auswählen][cache-export-data-container]
4. Geben Sie ein **Blobnamenspräfix** ein, und klicken Sie auf **Exportieren**, um den Exportvorgang zu starten. Das Blobnamenpräfix wird als Präfix für die Namen der Dateien verwendet, die während des Exportvorgangs generiert werden.

    ![Export][cache-export-data]

    Sie können den Status des Exportvorgangs überwachen, indem Sie die Benachrichtigungen im Azure-Portal verfolgen oder die Ereignisse im Überwachungsprotokoll anzeigen. Weitere Informationen finden Sie im Abschnitt „Einstellungen zu Support und Problembehandlung“ des Artikels [Gewusst wie: Konfigurieren von Azure Redis Cache](cache-configure.md).

    ![][cache-export-data-export-complete]

    Caches können während des Exportvorgangs weiterhin genutzt werden.

## <a name="importexport-faq"></a>Import/Export – Häufig gestellte Fragen
Dieser Abschnitt enthält häufig gestellte Fragen zum Import/Export-Feature.

* [Bei welchen Tarifen kann Import/Export genutzt werden?](#what-pricing-tiers-can-use-importexport)
* [Kann ich Daten von beliebigen Redis-Servern importieren?](#can-i-import-data-from-any-redis-server)
* [Ist der Cache während eines Import- oder Exportvorgangs verfügbar?](#will-my-cache-be-available-during-an-importexport-operation)
* [Kann ich Import/Export mit dem Redis-Cluster nutzen?](#can-i-use-importexport-with-redis-cluster)
* [Wie funktioniert Import/Export bei einer benutzerdefinierten Einstellung für Datenbanken?](#how-does-importexport-work-with-a-custom-databases-setting)
* [Wie unterscheidet sich Import/Export von der Redis-Persistenz?](#how-is-importexport-different-from-redis-persistence)
* [Kann ich Import/Export mit PowerShell, per CLI oder mit anderen Verwaltungsclients automatisieren?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [Ich habe während des Import/Export-Vorgangs einen Zeitüberschreitungsfehler erhalten. Was bedeutet das?](#i-received-a-timeout-error-during-my-importexport-operation.-what-does-it-mean)
* [Ich habe beim Exportieren der Daten in Azure Blob Storage einen Fehler erhalten. Was ist passiert?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage.-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a>Bei welchen Tarifen kann Import/Export genutzt werden?
Import/Export ist nur für den Premium-Tarif verfügbar.

### <a name="can-i-import-data-from-any-redis-server"></a>Kann ich Daten von beliebigen Redis-Servern importieren?
Ja. Zusätzlich zum Importieren von Daten, die aus Azure Redis Cache-Instanzen exportiert wurden, können Sie RDB-Dateien von Redis-Servern in beliebigen Clouds oder Umgebungen importieren, z.B. Linux, Windows oder von Cloudanbietern wie Amazon Web Services. Laden Sie hierzu die RDB-Datei vom gewünschten Redis-Server in ein Seitenblob eines Azure Storage-Kontos hoch, und importieren Sie sie dann in Ihre Premium-Instanz von Azure Redis Cache. Es kann beispielsweise sein, dass Sie die Daten aus dem Produktionscache exportieren und in einen Cache importieren möchten, der als Teil der Stagingumgebung zum Testen oder für die Migration verwendet wird.

### <a name="will-my-cache-be-available-during-an-importexport-operation"></a>Ist der Cache während eines Import- oder Exportvorgangs verfügbar?
* **Export:** Caches bleiben verfügbar, und Sie können Sie den Cache während eines Exportvorgangs weiter nutzen.
* **Import:** Caches sind ab dem Start eines Importvorgangs nicht mehr verfügbar und können wieder genutzt werden, nachdem der Importvorgang abgeschlossen ist.

### <a name="can-i-use-importexport-with-redis-cluster"></a>Kann ich Import/Export mit dem Redis-Cluster nutzen?
Ja. Sie können einen Import bzw. Export auch zwischen einem gruppierten Cache und einem nicht gruppierten Cache durchführen. Da der Redis-Cluster [nur Datenbank 0 unterstützt](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), werden Daten ausschließlich in die Datenbank 0 importiert. Wenn gruppierte Cachedaten importiert werden, werden die Schlüssel auf die Shards des Clusters verteilt.

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Wie funktioniert Import/Export bei einer benutzerdefinierten Einstellung für Datenbanken?
Bei einigen Tarifen gibt es unterschiedliche [Grenzwerte für Datenbanken](cache-configure.md#databases). Wenn Sie bei der Cacheerstellung einen benutzerdefinierten Wert für die `databases`-Einstellung konfiguriert haben, müssen Sie beim Importieren einige Punkte beachten.

* Beim Importieren in einen Tarif mit einem niedrigeren `databases` -Grenzwert als bei dem Tarif, aus dem Sie exportieren:
  * Wenn Sie die Standardanzahl für `databases` verwenden (sie beträgt bei allen Tarifen 16), gehen keine Daten verloren.
  * Wenn Sie eine benutzerdefinierte Anzahl für `databases` verwenden, die innerhalb der Grenzwerte für den Tarif liegt, in den Sie importieren, gehen keine Daten verloren.
  * Wenn Ihre exportierten Daten Daten in einer Datenbank enthalten, die die Grenzwerte des neuen Tarifs überschreitet, werden die Daten aus diesen höheren Datenbanken nicht importiert.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Wie unterscheidet sich Import/Export von der Redis-Persistenz?
Mit der Azure Redis Cache-Persistenz können Sie unter Redis gespeicherte Daten dauerhaft in Azure Storage speichern. Wenn Persistenz konfiguriert ist, speichert Azure Redis Cache basierend auf einer wählbaren Sicherungshäufigkeit eine Momentaufnahme des Redis-Caches in einem binären Redis-Format dauerhaft auf dem Datenträger. Bei einem schwerwiegenden Fehler, bei dem sowohl der primäre Cache als auch der Replikatcache deaktiviert werden, werden die Cachedaten automatisch wiederhergestellt, indem die aktuellste Momentaufnahme verwendet wird. Weitere Informationen finden Sie unter [Konfigurieren von Datenpersistenz für Azure Redis Cache vom Typ „Premium“](cache-how-to-premium-persistence.md).

Mit Import/Export können Sie Daten in Azure Redis Cache importieren oder daraus exportieren. Die Funktion dient nicht zum Konfigurieren der Sicherung oder für die Wiederherstellen per Redis-Persistenz.

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Kann ich Import/Export mit PowerShell, per CLI oder mit anderen Verwaltungsclients automatisieren?
Ja. Anweisungen für PowerShell finden Sie unter [Importieren eines Redis-Cache](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) und [Exportieren eines Redis-Cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Ich habe während des Import/Export-Vorgangs einen Zeitüberschreitungsfehler erhalten. Was bedeutet das?
Wenn Sie sich vor dem Initiieren des Vorgangs länger als 15 Minuten auf dem Blatt **Daten importieren** bzw. **Daten exportieren** aufhalten, wird ein Fehler der folgenden Art angezeigt.

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Initiieren Sie den Import- oder Exportvorgang, bevor die 15 Minuten abgelaufen sind.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Ich habe beim Exportieren der Daten in Azure Blob Storage einen Fehler erhalten. Was ist passiert?
Import/Export funktioniert nur mit RDB-Dateien, die als Seitenblobs gespeichert sind. Andere Blobtypen werden derzeit nicht unterstützt. Dies gilt auch für Blob-Speicherkonten mit Ebenen vom Typ „Hot“ und „Cool“.

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Verwendung weiterer Funktionen des Premium-Caches finden Sie in den folgenden Artikeln.

* [Einführung in den Premium-Tarif von Azure Redis Cache](cache-premium-tier-intro.md)    

<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png



<!--HONumber=Jan17_HO2-->


