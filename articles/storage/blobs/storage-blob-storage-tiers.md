---
title: "Azure-Speicherebenen „Heiß“ (Hot), „Kalt“ (Cool) und „Archiv“ für Blobs | Microsoft-Dokumentation"
description: "Speicherebenen „Heiß“ (Hot), „Kalt“ (Cool) und „Archiv“ für Azure Blob Storage-Konten."
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: eb33ed4f-1b17-4fd6-82e2-8d5372800eef
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/05/2017
ms.author: mihauss
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: 544b11d74a926fe62b8ceca51570ce9d2ee7e6e7
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="azure-blob-storage-hot-cool-and-archive-preview-storage-tiers"></a>Azure Blob Storage: Speicherebenen „Heiß“ (Hot), „Kalt“ (Cool) und „Archiv“ (Vorschau)

## <a name="overview"></a>Übersicht

Azure Storage bietet drei Speicherebenen für Blobobjektspeicher, damit Sie Ihre Daten je nach Nutzung möglichst kostengünstig speichern können. Die **Speicherebene „Heiß“ (Hot)** von Azure ist für die Speicherung von Daten optimiert, auf die häufig zugegriffen wird. Die **Speicherebene „Kalt“ (Cool)** von Azure ist für die Speicherung von Daten optimiert, auf die selten zugegriffen wird und die mindestens einen Monat lang gespeichert werden. Die [Speicherebene „Archiv“ (Vorschau)](https://azure.microsoft.com/blog/announcing-the-public-preview-of-azure-archive-blob-storage-and-blob-level-tiering) ist für die Speicherung von Daten optimiert, auf die äußerst selten zugegriffen wird und die mindestens sechs Monate lang mit flexiblen Latenzanforderungen (in der Größenordnung von Stunden) gespeichert werden. Die Speicherebene *Archiv* kann nicht für auf das gesamte Speicherkonto, sondern nur auf der Blobebene verwendet werden. Bei den Daten der Speicherebene „Cool“ können geringfügige Abstriche bei der Verfügbarkeit gemacht werden, aber Haltbarkeit, Zugriffszeit und Durchsatz müssen sich auf einem ähnlich hohen Niveau wie bei häufig genutzten Daten befinden. Bei Daten der Speicherebenen „Cool“ und „Archiv“ ist unter Umständen ein SLA mit einer etwas geringeren Verfügbarkeit und höheren Zugriffskosten akzeptabel, um im Gegenzug die Speicherkosten deutlich zu senken.

Die Speicherung von Daten in der Cloud nimmt immer mehr zu. Um die Kosten für die zunehmenden Speicheranforderungen im Blick zu behalten, ist es hilfreich, die Daten anhand von Attributen wie der Zugriffshäufigkeit und geplanten Aufbewahrungsdauer zu organisieren. In der Cloud gespeicherte Daten können sich darin unterscheiden, wie sie über ihre Lebensdauer hinweg generiert, verarbeitet und genutzt werden. Auf einen Teil der Daten wird aktiv zugegriffen, und sie werden während ihrer Lebensdauer geändert. Auf andere Daten wird zu Beginn ihrer Lebensdauer häufig zugegriffen, und die Zugriffe fallen dann deutlich ab, je älter die Daten werden. Außerdem gibt es Daten, die in der Cloud lediglich vorgehalten werden und auf die nach der Speicherung nur sehr selten oder gar nicht zugegriffen wird.

Jedes dieser Datenzugriffsszenarien profitiert von einer differenzierten Speicherebene, die für ein bestimmtes Zugriffsmuster optimiert ist. Mit den Speicherebenen „Hot“, „Cool“ und „Archiv“ bietet Azure Blob Storage unterschiedliche Speicherebenen mit separaten Preismodellen.

## <a name="blob-storage-accounts"></a>Blob-Speicherkonten

**Blob-Speicherkonten** sind spezielle Speicherkonten und dienen dazu, unstrukturierte Daten als Blobs (Objekte) in Azure Storage zu speichern. Mit Blob-Speicherkonten können Sie jetzt auf der Grundlage von Zugriffsmustern zwischen den Speicherebenen „Hot“ und „Cool“ (auf der Kontoebene) bzw. zwischen den Ebenen „Hot“, „Cool“ und „Archiv“ (auf der Blobebene) wählen. Speichern Sie äußerst selten verwendete Daten zu geringstmöglichen Speicherkosten, selten verwendete Daten zu geringeren Speicherkosten als häufig verwendete Daten und häufig verwendete Daten zu geringstmöglichen Zugriffskosten. Blob-Speicherkonten sind mit Ihren bereits vorhandenen allgemeinen Speicherkonten vergleichbar und besitzen die gleichen Eigenschaften für Dauerhaftigkeit, Verfügbarkeit, Skalierbarkeit und Leistung, die Sie schon heute verwenden – einschließlich vollständiger API-Konsistenz für Blockblobs und Anfügeblobs.

> [!NOTE]
> BLOB-Speicherkonten unterstützen nur Block- und Anfügeblobs, keine Seitenblobs.

Mit dem für Blob-Speicherkonten verfügbaren Attribut **Access Tier** (Zugriffstarif) können Sie die Speicherebene abhängig von den im Konto gespeicherten Daten als **Hot** oder **Cool** angeben. Bei einer Änderung des Nutzungsmusters der Daten können Sie jederzeit zwischen den beiden Speicherebenen wechseln. Die Ebene „Archiv“ (Vorschau) steht ausschließlich auf der Blobebene zur Verfügung.

> [!NOTE]
> Die Änderung der Speicherebene kann mit zusätzlichen Kosten verbunden sein. Ausführlichere Informationen finden Sie im Abschnitt [Preise und Abrechnung](#pricing-and-billing).

### <a name="hot-access-tier"></a>Zugriffsebene „Hot“

Beispielszenarien für die Verwendung der Speicherebene „Hot“:

* Daten, die aktiv verwendet werden oder bei denen eine hohe Zugriffshäufigkeit (Lese- und Schreibvorgänge) zu erwarten ist
* Daten, die zur Verarbeitung und späteren Migration zur Speicherebene „Cool“ bereitgestellt werden

### <a name="cool-access-tier"></a>Zugriffsebene „Cool“

Beispielszenarien für die Verwendung der Speicherebene „Cool“:

* Kurzfristige Sicherung und Datasets für die Notfallwiederherstellung
* Ältere Medieninhalte, die nur noch selten angezeigt werden, dann aber umgehend verfügbar sein müssen
* Große Datasets, die kostengünstig gespeichert werden müssen, während weitere Daten für die zukünftige Verarbeitung gesammelt werden (*Beispiel:* langfristige Speicherung von wissenschaftlichen Daten oder Telemetrie-Rohdaten einer Produktionsstätte)

### <a name="archive-access-tier-preview"></a>Zugriffsebene „Archiv“ (Vorschau)

[Archivspeicher](https://azure.microsoft.com/blog/announcing-the-public-preview-of-azure-archive-blob-storage-and-blob-level-tiering) zeichnet sich im Vergleich zu den Speicherebenen „Hot“ und „Cool“ durch geringstmögliche Speicherkosten und höhere Datenabrufkosten aus.

Ein Blob im Archivspeicher kann nicht gelesen, kopiert, überschrieben oder geändert werden. Auch können Sie keine Momentaufnahmen von einem Blob im Archivspeicher erstellen. Sie können jedoch vorhandene Vorgänge verwenden, um das Blob zu löschen, aufzulisten, Eigenschaften/Metadaten des Blobs abzurufen oder die Ebene des Blobs zu ändern. Wenn Sie Daten aus dem Archivspeicher lesen möchten, müssen Sie zuerst die Ebene des Blobs in „Hot“ oder „Cool“ ändern. Dieser Prozess wird als Aktivierung bezeichnet und kann für Blobs mit einer Größe von bis zu 50 GB bis zu 15 Stunden dauern. Bei größeren Blobs dauert der Vorgang länger. Die zusätzlich benötigte Zeit ist abhängig vom Durchsatzlimit des Blobs.

Während der Aktivierung können Sie anhand der Archivstatuseigenschaft des Blobs ermitteln, ob die Ebene geändert wurde. Je nach Zielebene lautet der Status entweder „rehydrate-pending-to-hot“ (Aktivierung für Ebene „Hot“ ausstehend) oder „rehydrate-pending-to-cool“ (Aktivierung für Ebene „Cool“ ausstehend). Nach Abschluss des Vorgangs wird die Archivstatuseigenschaft des Blobs entfernt, und die Zugriffsebeneneigenschaft des Blobs spiegelt die Ebene „Hot“ oder „Cool“ wider.  

Beispielszenarien für die Verwendung der Speicherebene „Archiv“:

* Langfristige Sicherung, Archivierung und Datasets für die Notfallwiederherstellung
* Originaldaten (Rohdaten), die auch nach der Umwandlung in ein endgültiges verwendbares Format erhalten bleiben müssen (*Beispiel:* Medien-Rohdateien nach der Transcodierung in andere Formate)
* Compliance- und Archivdaten, die über einen langen Zeitraum gespeichert werden müssen und selten verwendet werden (*Beispiele:* Überwachungsvideos, alte Röntgenbilder/MRT-Scans für Unternehmen aus dem Gesundheitswesen, Audioaufnahmen und Protokolle von Kundenanrufen bei Finanzdienstleistern)

### <a name="recommendations"></a>Recommendations

Weitere Informationen zu Speicherkonten finden Sie unter [Informationen zu Azure-Speicherkonten](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) .

Bei Anwendungen, die nur Block- oder Anfügeblobspeicher benötigen, empfiehlt sich die Verwendung von Blob-Speicherkonten, um vom differenzierten Preismodell des gestaffelten Speichers profitieren zu können. Allerdings gibt es bestimmte Situationen, in denen dies nicht möglich ist und stattdessen allgemeine Speicherkonten verwendet werden müssen. Hierzu zählen etwa folgende Szenarien:

* Sie müssen Tabellen, Warteschlangen oder Dateien verwenden, und die Blobs sollen im gleichen Speicherkonto gespeichert werden. Neben der Verwendung der gleichen Schlüssel hat diese Vorgehensweise allerdings keine weiteren technischen Vorteile.

* Sie müssen weiterhin das klassische Bereitstellungsmodell verwenden. Blob-Speicherkonten sind nur im Rahmen des Azure Resource Manager-Bereitstellungsmodells verfügbar.

* Sie müssen Seitenblobs verwenden. Blob-Speicherkonten unterstützen keine Seitenblobs. Wir empfehlen grundsätzlich die Verwendung von Blockblobs, es sei denn, Sie benötigen explizit Seitenblobs.

* Sie verwenden eine ältere Version der [REST-API für Speicherdienste](https://msdn.microsoft.com/library/azure/dd894041.aspx) (vor 2014-02-14) oder eine Clientbibliothek mit einer niedrigeren Version als 4.x und können kein Upgrade für Ihre Anwendung durchführen.

> [!NOTE]
> Blob-Speicherkonten werden derzeit in allen Azure-Regionen unterstützt.
 

## <a name="blob-level-tiering-feature-preview"></a>Tiering auf Blobebene (Vorschau)

Mit dem Tiering auf Blobebene können Sie nun mithilfe eines einzelnen Vorgangs namens [Set Blob Tier](/rest/api/storageservices/set-blob-tier) (Blobebene festlegen) die Ebene Ihrer Daten auf der Objektebene ändern. So können Sie flexibel auf Nutzungsänderungen reagieren und problemlos zwischen den Blobzugriffsebenen „Hot“, „Cool“ und „Archiv“ wechseln, ohne Daten zwischen Konten zu verschieben. Alle Ebenenänderungen werden umgehend umgesetzt (mit Ausnahme der Aktivierung aus dem Archiv). Ein Konto kann Blobs aus allen drei Speicherebenen enthalten. Blobs, denen keine explizite Ebene zugewiesen ist, erben die Ebene von der Zugriffsebeneneinstellung des Kontos.

Gehen Sie zur Verwendung dieser Vorschaufeatures wie in der [Ankündigung der Archivebene und des Tierings auf Blobebene](https://azure.microsoft.com/blog/announcing-the-public-preview-of-azure-archive-blob-storage-and-blob-level-tiering) beschrieben vor.

Im Anschluss folgen einige Einschränkungen für die Vorschauversion des Tierings auf Blobebene:

* Der Archivspeicher wird nur von neuen Blob-Speicherkonten unterstützt, die nach erfolgreicher Registrierung für die Vorschauversion in der Region „USA, Osten 2“ erstellt werden.

* Das Tiering auf Blobebene wird nur von neuen Blob-Speicherkonten unterstützt, die nach erfolgreicher Registrierung für die Vorschauversion in öffentlichen Regionen erstellt werden.

* Tiering auf Blobebene und Archivspeicher werden nur in [LRS] (../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#locally-redundant-storage) unterstützt. Die Unterstützung von [GRS](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#geo-redundant-storage) und [RA-GRS](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#read-access-geo-redundant-storage) folgt später.

* Die Ebene eines Blobs mit Momentaufnahmen kann nicht geändert werden.

* Von einem Blob im Archivspeicher kann keine Kopie oder Momentaufnahme erstellt werden.

## <a name="comparison-of-the-storage-tiers"></a>Vergleich der Speicherebenen

Die folgende Tabelle enthält eine Gegenüberstellung der Speicherebenen „Hot“ und „Cool“. Da sich der Archivspeicher auf Blobebene noch in der Vorschauphase befindet, stehen dafür keine SLAs zur Verfügung.

| | **Speicherebene „Hot“** | **Speicherebene „Cool“** |
| ---- | ----- | ----- |
| **Verfügbarkeit** | 99,9 % | 99 % |
| **Verfügbarkeit** <br> **(RA-GRS-Lesevorgänge)**| 99,99 % | 99,9 % |
| **Nutzungsgebühren** | Höhere Speicherkosten, geringere Zugriffs- und Transaktionskosten | Geringere Speicherkosten, höhere Zugriffs- und Transaktionskosten |
| **Mindestobjektgröße** | N/V | N/V |
| **Mindestspeicherdauer** | N/V | N/V |
| **Latenz** <br> **(Zeit bis zum ersten Byte)** | Millisekunden | Millisekunden |
| **Skalierbarkeits- und Leistungsziele** | Identisch mit allgemeinen Speicherkonten | Identisch mit allgemeinen Speicherkonten |

> [!NOTE]
> Blob-Speicherkonten unterstützen die gleichen Leistungs- und Skalierbarkeitsziele wie allgemeine Speicherkonten. Weitere Informationen finden Sie unter [Microsoft Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Speicherskalierbarkeit und Leistungsziele von Azure, in englischer Sprache).


## <a name="pricing-and-billing"></a>Preise und Abrechnung
Blob-Speicherkonten verwenden ein auf der Speicherebene basierendes Preismodell für Blobspeicher. Bei Verwendung eines Blob-Speicherkontos sind folgende Abrechnungsaspekte zu berücksichtigen:

* **Speicherkosten**: Die Kosten für die Datenspeicherung hängen nicht nur von der gespeicherten Datenmenge ab, sondern auch von der Speicherebene. Die Speicherebene für selten genutzte Daten (Cool) bietet geringere Kosten pro Gigabyte als die Speicherebene für häufig genutzte Daten (Hot).

* **Kosten für den Datenzugriff**: Im Rahmen der Speicherebene für selten genutzte Daten (Cool) fallen pro Gigabyte Zugriffsgebühren für Lese- und Schreibvorgänge an.

* **Transaktionskosten**: Transaktionsgebühren fallen bei beiden Tarifen an. Bei der Speicherebene für selten genutzte Daten (Cool) sind die Transaktionskosten aber höher als bei der Speicherebene für häufig genutzte Daten (Hot).

* **Datenübertragungskosten bei Georeplikation**: Gilt nur für Konten mit konfigurierter Georeplikation (einschließlich GRS und RA-GRS). Die Datenübertragung für die Georeplikation wird pro Gigabyte abgerechnet.

* **Kosten für ausgehende Datenübertragungen**: Ausgehende Datenübertragungen (Daten, die aus einer Azure-Region übertragen werden) werden genau wie bei allgemeinen Speicherkonten nach Bandbreitennutzung pro Gigabyte abgerechnet.

* **Änderung der Speicherebene**: Bei einem Speicherebenenwechsel von „Cool“ zu „Hot“ fällt bei jedem Übergang eine Gebühr an, die den Kosten entspricht, die durch das Lesen aller im Speicherkonto vorhandenen Daten entstehen. Der Wechsel von „Hot“ zu „Cool“ ist hingegen kostenlos.

> [!NOTE]
> Ausführlichere Informationen zum Preismodell für Blob-Speicherkonten finden Sie auf der Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/). Ausführlichere Informationen zu den Kosten für ausgehende Datenübertragungen finden Sie auf der Seite [Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="quickstart"></a>Schnellstart

In diesem Abschnitt werden unter Verwendung des Azure-Portals die folgenden Szenarien veranschaulicht:

* Erstellen eines Blob-Speicherkontos
* Verwalten eines Blob-Speicherkontos

In den folgenden Beispielen kann die Zugriffsebene nicht auf „Archiv“ festgelegt werden, da diese Einstellung für das gesamte Speicherkonto gilt. „Archiv“ kann nur für ein bestimmtes Blob festgelegt werden.

### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Erstellen eines Blob-Speicherkontos mithilfe des Azure-Portals

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.

2. Wählen Sie im Menü „Hub“ die Option **Neu** > **Daten und Speicher** > **Speicherkonto** aus.

3. Geben Sie einen Namen für Ihr Speicherkonto ein.
   
    Dieser Name muss global eindeutig sein. Er wird als Teil der URL verwendet, über die auf die Objekte im Speicherkonto zugegriffen wird.  

4. Wählen Sie als Bereitstellungsmodell die Option **Resource Manager** aus.
   
    Mehrstufiger Speicher kann nur mit Resource Manager-Speicherkonten verwendet werden. Dies ist das empfohlene Bereitstellungsmodell für die neuen Ressourcen. Weitere Informationen finden Sie unter [Übersicht über Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).  

5. Wählen Sie in der Dropdownliste „Account Kind“ (Kontoart) die Option **Blob Storage**.
   
    Hier wählen Sie den Speicherkontotyp aus. Mehrstufiger Speicher ist nicht im allgemeinen Speicher verfügbar, sondern nur im Speicher vom Typ „Blobspeicher“.     
   
    Beachten Sie, dass die Leistungsebene bei dieser Auswahl auf „Standard“ festgelegt ist. Mehrstufiger Speicher ist für die Leistungsebene „Premium“ nicht verfügbar.

6. Wählen Sie die Replikationsoption für das Speicherkonto aus: **LRS**, **GRS** oder **RA-GRS**. Die Standardeinstellung ist **RA-GRS**.
   
    LRS = lokal redundanter Speicher, GRS = georedundanter Speicher (2 Regionen), RA-GRS = georedundanter Speicher mit Lesezugriff (2 Regionen mit Lesezugriff für die zweite Region).
   
    Weitere Details zu den Replikationsoptionen für Azure Storage finden Sie unter [Azure Storage-Replikation](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

7. Wählen Sie die richtige Speicherebene für Ihre Zwecke aus: Legen Sie **Zugriffstarif** entweder auf **Kalte Daten** oder **Heiße Daten** fest. Der Standardwert ist **Heiße Daten**. 

8. Wählen Sie das Abonnement aus, in dem Sie das neue Speicherkonto erstellen möchten.

9. Geben Sie eine neue Ressourcengruppe an, oder wählen Sie eine vorhandene Ressourcengruppe aus. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

10. Wählen Sie die Region für Ihr Speicherkonto aus.

11. Klicken Sie auf **Erstellen** , um das Speicherkonto zu erstellen.

### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>Ändern der Speicherebene für ein Blob-Speicherkonto mithilfe des Azure-Portals

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Navigieren Sie zu Ihrem Speicherkonto, wählen Sie „Alle Ressourcen“, und wählen Sie dann Ihr Speicherkonto aus.

3. Klicken Sie auf dem Blatt „Einstellungen“ auf **Konfiguration**, um die Kontokonfiguration anzuzeigen bzw. zu ändern.

4. Wählen Sie die passende Speicherebene für Ihre Zwecke aus: Legen Sie **Zugriffsebene** entweder auf **Kalte Daten** oder **Heiße Daten** fest.

5. Klicken Sie oben im Blatt auf „Speichern“.

> [!NOTE]
> Die Änderung der Speicherebene kann mit zusätzlichen Kosten verbunden sein. Ausführlichere Informationen finden Sie im Abschnitt [Preise und Abrechnung](#pricing-and-billing).


## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Evaluieren von und Migrieren zu Blob-Speicherkonten
Dieser Abschnitt dient dazu, Benutzern einen reibungslosen Übergang zur Nutzung von Blob-Speicherkonten zu ermöglichen. Es gibt zwei Benutzerszenarien:

* Sie verfügen über ein vorhandenes allgemeines Speicherkonto und möchten eine Umstellung auf ein Blob-Speicherkonto mit der richtigen Speicherebene evaluieren.
* Sie haben sich für die Nutzung eines Blob-Speicherkontos entschieden oder besitzen bereits ein Konto dieser Art und möchten evaluieren, ob Sie die Speicherebene für „heiße Daten“ (Hot) oder „kalte Daten“ (Cool) verwenden sollen.

In beiden Fällen sollten Sie zuerst die Kosten für die Speicherung und den Zugriff auf Ihre Daten in einem Blob-Speicherkonto schätzen und diesen Betrag mit Ihren derzeitigen Kosten vergleichen.

## <a name="evaluating-blob-storage-account-tiers"></a>Evaluieren von Blob-Speicherkontoebenen

Zur Ermittlung der ungefähren Kosten für die Speicherung und den Zugriff auf die Daten, die in einem Blob-Speicherkonto gespeichert sind, müssen Sie Ihr vorhandenes Nutzungsmuster evaluieren oder Ihr voraussichtliches Nutzungsmuster einschätzen. Dazu benötigen Sie im Allgemeinen folgende Informationen:

* Ihr Speicherverbrauch: Welche Datenmengen werden gespeichert, und wie verändert sich dies von Monat zu Monat?

* Ihre Speicherzugriffsmuster: Welche Datenmengen werden im Rahmen des Kontos gelesen und geschrieben (einschließlich neuer Daten)? Wie viele Transaktionen werden für den Datenzugriff verwendet, und welche Arten von Transaktionen fallen an?

## <a name="monitoring-existing-storage-accounts"></a>Überwachen von vorhandenen Speicherkonten

Um Ihre vorhandenen Speicherkonten zu überwachen und diese Daten zu sammeln, können Sie Azure-Speicheranalysen nutzen. Damit wird eine Protokollierung durchgeführt, und es werden Metrikdaten für ein Speicherkonto bereitgestellt. Mit Speicheranalysen können Metriken gespeichert werden, die aggregierte Transaktionsstatistiken und Kapazitätsdaten zu Anforderungen an den Blob-Speicherdienst enthalten – sowohl für allgemeine Speicherkonten als auch für Blob-Speicherkonten. Diese Daten werden in bekannten Tabellen in demselben Speicherkonto gespeichert.

Weitere Informationen finden Sie unter [Informationen zu Metriken der Speicheranalyse](https://msdn.microsoft.com/library/azure/hh343258.aspx) sowie unter [Schema der Tabellen für Speicheranalysemetriken](https://msdn.microsoft.com/library/azure/hh343264.aspx).

> [!NOTE]
> Blob-Speicherkonten machen den Tabellenspeicherdienst-Endpunkt nur zum Speichern und Zugreifen auf die Metrikdaten für das Konto verfügbar.

Zur Überwachung des Speicherbedarfs für den Blob-Speicherdienst müssen Sie die Kapazitätsmetriken aktivieren.
Wenn dies aktiviert ist, werden täglich Kapazitätsdaten für den Blob-Dienst eines Speicherkontos aufgezeichnet. Sie werden als Tabelleneintrag aufgezeichnet, der in die Tabelle *$MetricsCapacityBlob* desselben Speicherkontos geschrieben wird.

Zur Überwachung des Datenzugriffsmusters für den Blob-Speicherdienst müssen Sie die stündlichen Transaktionsmetriken auf der API-Ebene aktivieren. Wenn dies aktiviert ist, werden jede Stunde die Transaktionen pro API aggregiert und als Tabelleneintrag aufgezeichnet, der in die Tabelle *$MetricsHourPrimaryTransactionsBlob* desselben Speicherkontos geschrieben wird. In der Tabelle *$MetricsHourSecondaryTransactionsBlob* werden bei Verwendung von RA-GRS-Speicherkonten die Transaktionen für den sekundären Endpunkt aufgezeichnet.

> [!NOTE]
> Falls Sie über ein allgemeines Speicherkonto verfügen, in dem Sie neben Block- und Anfügeblob-Daten Seitenblobs und Datenträger virtueller Computer gespeichert haben, ist dieser Schätzungsprozess nicht geeignet. Es gibt nämlich keine Möglichkeit, die Kapazitäts- und Transaktionsmetriken basierend auf dem Blobtyp nur für Block- und Anfügeblobs zu ermitteln, die zu einem Blob-Speicherkonto migriert werden können.

Um eine gute Annäherung des Datenverbrauchs und der Zugriffsmuster zu erhalten, empfehlen wir Ihnen die Auswahl eines Aufbewahrungszeitraums für die Metriken, der für die reguläre Nutzung repräsentativ ist und den Sie dann extrapolieren können. Eine Option besteht darin, die Metrikdaten für sieben Tage aufzubewahren und die Daten für eine Analyse am Monatsende jede Woche zu erfassen. Eine andere Möglichkeit ist die Aufbewahrung der Metrikdaten der letzten 30 Tage, um sie dann am Ende der 30 Tage zu erfassen und zu analysieren.

Weitere Informationen zum Aktivieren, Erfassen und Anzeigen von Metrikdaten finden Sie unter [Aktivieren der Azure-Speichermetriken und Anzeigen von Metrikdaten](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE]
> Speicherung, Verwendung und Download von Analysedaten werden genau wie bei regulären Benutzerdaten in Rechnung gestellt.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Verwenden von Nutzungsmetriken zum Schätzen von Kosten

### <a name="storage-costs"></a>Speicherkosten

Der letzte Eintrag in der Kapazitätsmetrikentabelle *$MetricsCapacityBlob* mit dem Zeilenschlüssel *'data'* enthält die von den Benutzerdaten verbrauchte Speicherkapazität. Der letzte Eintrag in der Kapazitätsmetrikentabelle *$MetricsCapacityBlob* mit dem Zeilenschlüssel *'analytics'* enthält die von den Analyseprotokollen verbrauchte Speicherkapazität.

Diese gesamte Kapazität, die von Benutzerdaten und Analyseprotokollen (falls aktiviert) verbraucht wird, kann dann verwendet werden, um die Kosten für das Speichern der Daten im Speicherkonto zu schätzen. Die gleiche Methode kann auch zum Schätzen der Speicherkosten für Block- und Anfügeblobs in allgemeinen Speicherkonten verwendet werden.

### <a name="transaction-costs"></a>Transaktionskosten

Die Summe von *'TotalBillableRequests'*über alle Einträge für eine API in der Transaktionsmetrikentabelle hinweg gibt die Gesamtzahl der Transaktionen für die jeweilige API an. *Beispiel:* Die Gesamtanzahl von Transaktionen vom Typ *GetBlob* eines bestimmten Zeitraums kann anhand der Gesamtsumme von abrechenbaren Anforderungen für alle Einträge mit dem Zeilenschlüssel *user;GetBlob* berechnet werden.

Zur Ermittlung der ungefähren Transaktionskosten für Blob-Speicherkonten müssen die Transaktionen in drei Gruppen unterteilt werden, da jeweils unterschiedliche Preise gelten.

* Schreibtransaktionen wie *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* und *'CopyBlob'*.
* Löschtransaktionen wie *'DeleteBlob'* und *'DeleteContainer'*.
* Alle anderen Transaktionen.

Um die Transaktionskosten für allgemeine Speicherkonten zu schätzen, müssen Sie alle Transaktionen unabhängig vom Vorgang bzw. von der API aggregieren.

### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Datenübertragungskosten für Datenzugriff und Georeplikation

Die Speicheranalyse liefert zwar nicht die Menge der Daten, die aus einem Speicherkonto gelesen und in das Speicherkonto geschrieben wird, aber dieser Wert kann grob geschätzt werden, indem die Tabelle mit den Transaktionsmetriken verwendet wird. Die Summe von *'TotalIngress'* über alle Einträge für eine API in der Transaktionsmetrikentabelle hinweg gibt die Gesamtmenge der Eingangsdaten in Byte für die jeweilige API an. Analog dazu gibt die Summe von *'TotalEgress'* die Gesamtmenge der Ausgangsdaten in Byte an.

Zur Ermittlung der ungefähren Datenzugriffskosten für Blob-Speicherkonten müssen die Transaktionen in zwei Gruppen unterteilt werden. 

* Die Menge der Daten, die aus dem Speicherkonto abgerufen werden, kann geschätzt werden, indem vor allem für die Vorgänge *'GetBlob'* und *'CopyBlob'* die Summe von *'TotalEgress'* geprüft wird.

* Die Menge der Daten, die in das Speicherkonto geschrieben werden, kann anhand der Summe von *'TotalIngress'* für die Vorgänge *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* und *'AppendBlock'* geschätzt werden.

Bei Verwendung eines GRS- oder RA-GRS-Speicherkontos können die Datenübertragungskosten mit Georeplikation für Blob-Speicherkonten auch auf der Grundlage der Schätzung für die Menge an geschriebenen Daten berechnet werden.

> [!NOTE]
> Ein ausführlicheres Beispiel zur Berechnung der Kosten für die Verwendung der Speicherebene „Hot“ oder „Cool“ finden Sie unter *Was sind die Zugriffsebenen „Heiß“ und „Kalt“, und wie bestimme ich, welche Zugriffsebene ich wählen sollte?* im FAQ-Bereich mit dem Titel [Was sind die Zugriffsebenen „Heiß“ und „Kalt“, und wie bestimme ich, welche Zugriffsebene ich wählen sollte?](https://azure.microsoft.com/pricing/details/storage/)an.
 
## <a name="migrating-existing-data"></a>Migrieren vorhandener Daten

Ein Blob-Speicherkonto ist ein spezielles Konto, in dem nur Blockblobs und Anfügeblobs gespeichert werden. Bereits vorhandene allgemeine Speicherkonten, die Ihnen das Speichern von Tabellen, Warteschlangen, Dateien und Datenträgern sowie Blobs ermöglichen, können nicht in Blob-Speicherkonten konvertiert werden. Zum Verwenden der Speicherebenen müssen Sie neue Blob-Speicherkonten erstellen und Ihre vorhandenen Daten zu den neu erstellten Konten migrieren.

Mithilfe der folgenden Methoden können Sie vorhandene Daten von lokalen Speichergeräten, aus dem Cloudspeicher von Drittanbietern oder aus Ihren vorhandenen allgemeinen Azure-Speicherkonten zu Blob-Speicherkonten migrieren:

### <a name="azcopy"></a>AzCopy

AzCopy ist ein Windows-Befehlszeilenprogramm, mit dem sehr effizient Daten in und aus Azure Storage kopiert werden können. Mit AzCopy können Sie Daten aus Ihren vorhandenen allgemeinen Speicherkonten in das Blob-Speicherkonto kopieren oder Daten von Ihren lokalen Speichergeräten in das Blob-Speicherkonto hochladen.

Ausführlichere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Data Movement-Bibliothek

Die Azure Storage Data Movement-Bibliothek für .NET basiert auf dem Kernframework für die Datenverschiebung, das AzCopy zugrunde liegt. Die Bibliothek ist ähnlich wie AzCopy auf leistungsfähige, zuverlässige und einfache Datenübertragungsvorgänge ausgelegt. Dies ermöglicht die native Verwendung aller Vorteile der AzCopy-Features in Ihrer Anwendung, ohne externe Instanzen von AzCopy ausführen und überwachen zu müssen.

Ausführlichere Informationen finden Sie unter [Azure Storage Data Movement-Bibliothek für .NET](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>REST-API oder Clientbibliothek

Sie können eine benutzerdefinierte Anwendung erstellen, um Ihre Daten mithilfe einer der Azure-Clientbibliotheken oder mithilfe der REST-API der Azure Storage-Dienste in ein Blob-Speicherkonto zu migrieren. Azure Storage bietet umfassende Clientbibliotheken für mehrere Programmiersprachen und Plattformen wie .NET, Java, C++, Node.JS, PHP, Ruby und Python. Die Clientbibliotheken bieten erweiterte Funktionen, beispielsweise Wiederholungslogik, Protokollierung und parallele Uploads. Die Entwicklung kann direkt mit der REST-API erfolgen. Diese API lässt sich mithilfe jeder Sprache aufrufen, die HTTP/HTTPS-Anforderungen verarbeitet.

Ausführlichere Informationen finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md).

> [!NOTE]
> Für Blobs, für die die clientseitige Verschlüsselung verwendet wird, werden auf die Verschlüsselung bezogene Metadaten des Blobs gespeichert. Es ist äußerst wichtig, dass für alle Kopiermechanismen sichergestellt wird, dass die Blob-Metadaten – und vor allem die auf die Verschlüsselung bezogenen Metadaten – beibehalten werden. Wenn Sie die Blobs ohne diese Metadaten kopieren, ist der Blobinhalt nicht mehr abrufbar. Weitere Informationen zu den auf die Verschlüsselung bezogenen Metadaten finden Sie unter [Clientseitige Azure Storage-Verschlüsselung](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
 
## <a name="faq"></a>Häufig gestellte Fragen

1. **Bleiben bereits vorhandene Speicherkonten weiterhin verfügbar?**
   
    Ja. Bereits vorhandene Speicherkonten sind weiterhin verfügbar (zu gleichen Preisen und mit unveränderten Funktionen).  Für sie kann keine Speicherebene ausgewählt werden, und diese Möglichkeit ist auch in Zukunft nicht geplant.

2. **Warum und wann sollte ich Blob-Speicherkonten verwenden?**
   
    Blob-Speicherkonten sind speziell für die Speicherung von Blobs konzipiert und ermöglichen die Einführung neuer, bloborientierter Features. In Zukunft wird für die Speicherung von Blobs die Verwendung von Blob-Speicherkonten empfohlen, und es werden Funktionen wie hierarchische Speicherung und Tarife für diesen Kontotyp eingeführt. Es liegt jedoch bei Ihnen, wann Sie die Migration (abhängig von Ihren geschäftlichen Anforderungen) vornehmen möchten.

3. **Kann ich mein vorhandenes Speicherkonto in ein Blob-Speicherkonto umwandeln?**
   
    Nein. Blob-Speicherkonten sind eine andere Art von Speicherkonto und müssen neu erstellt und die Daten anschließend migriert werden, wie weiter oben erläutert.

4. **Kann ich Objekte innerhalb des gleichen Kontos auf beiden Speicherebenen speichern?**
   
    Das Attribut *Zugriffsebene* gibt die Speicherebene an, wird auf der Kontoebene festgelegt und gilt für alle Objekte dieses Kontos. Mit dem Tiering auf Blobebene (Vorschau) können Sie die Zugriffsebene jedoch für bestimmte Blobs festlegen und so die Zugriffsebene für das Konto überschreiben. 

5. **Kann ich die Speicherebene meines Blob-Speicherkontos ändern?**
   
    Ja. Sie können die Speicherebene ändern, indem Sie für das Speicherkonto das Attribut *Zugriffsebene* festlegen. Die Änderung der Speicherebene gilt für alle im Konto gespeicherten Objekte. Der Wechsel von „Hot“ zu „Cool“ ist kostenlos. Beim Wechsel von „Cool“ zu „Hot“ wird hingegen eine GB-basierte Gebühr für das Lesen aller Daten des Kontos erhoben.

6. **Wie oft kann ich die Speicherebene für mein Blob-Speicherkonto ändern?**
   
    Die Anzahl von Speicherebenenänderungen wird von uns zwar nicht begrenzt, aber Sie sollten bedenken, dass ein Wechsel von „Cool“ zu „Hot“ mit erheblichen Kosten verbunden ist. Wir raten daher von häufigen Speicherebenenwechseln ab.

7. **Verhalten sich die Blobs der Speicherebene „Cool“ anders als die Blobs der Speicherebene „Hot“?**
   
    Blobs der Speicherebene „Hot“ weisen die gleiche Latenz wie Blobs von allgemeinen Speicherkonten auf. Blobs der Speicherebene „Cool“ verfügen über eine ähnliche Latenz (in Millisekunden) wie Blobs von allgemeinen Speicherkonten.
   
    Für Blobs der Speicherebene „Cool“ gilt in Bezug auf die Verfügbarkeit ein etwas geringerer Servicelevel (SLA) als für Blobs, die auf der Speicherebene „Hot“ gespeichert sind. Weitere Details finden Sie unter [SLA für Speicher](https://azure.microsoft.com/support/legal/sla/storage).

8. **Kann ich Seitenblobs und Datenträger virtueller Computer unter Blob-Speicherkonten speichern?**
   
    Blob-Speicherkonten unterstützen nur Block- und Anfügeblobs, keine Seitenblobs. Datenträger virtueller Azure-Computer werden durch Seitenblobs unterstützt. Daher können Blob-Speicherkonten nicht zum Speichern der Datenträger von virtuellen Computern verwendet werden. Es ist aber möglich, Sicherungen der Datenträger virtueller Computer als Blockblobs unter einem Blob-Speicherkonto zu speichern.

9. **Muss ich zur Verwendung von Blob-Speicherkonten meine vorhandenen Anwendungen ändern?**
   
    Blob-Speicherkonten sind zu 100% API-konsistent mit allgemeinen Speicherkonten für Block- und Anfügeblobs. Solange Sie für Ihre Anwendung Blockblobs oder Anfügeblobs verwenden und Version 2014-02-14 (oder höher) der [REST-API für Speicherdienste](https://msdn.microsoft.com/library/azure/dd894041.aspx) nutzen, sollte Ihre Anwendung funktionieren. Bei Verwendung einer älteren Protokollversion müssen Sie Ihre Anwendung aktualisieren, sodass sie die neue Version verwendet und beide Arten von Speicherkonten problemlos genutzt werden können. Unabhängig von der Art des verwendeten Speicherkontos empfehlen wir grundsätzlich die Verwendung der neuesten Version.

10. **Ändert sich etwas für die Benutzer?**
    
    Blob-Speicherkonten weisen eine hohe Ähnlichkeit mit allgemeinen Speicherkonten zum Speichern von Block- und Anfügeblobs auf und unterstützen alle wesentlichen Funktionen von Azure Storage, z.B. hohe Stabilität und Verfügbarkeit, Skalierbarkeit, Leistung und Sicherheit. Mit Ausnahme der spezifischen Features und Einschränkungen für Blob-Speicherkonten und der weiter oben erläuterten Speicherebenen ändert sich nichts.

## <a name="next-steps"></a>Nächste Schritte

### <a name="evaluate-blob-storage-accounts"></a>Auswerten von Blob-Speicherkonten

[Überprüfen der Verfügbarkeit von Blob-Speicherkonten nach Region unter Azure-Regionen](https://azure.microsoft.com/regions/#services)

[Auswerten der Nutzung vorhandener Speicherkonten durch Aktivierung von Azure Storage-Metriken](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Überprüfen der Preise für Blobspeicher nach Region unter Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)

[Überprüfen der Preise für Datenübertragungen unter Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Beginnen mit der Nutzung von Blob-Speicherkonten

[Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md)

[Verschieben von Daten in und aus Azure Storage](../common/storage-moving-data.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Durchsuchen und Erkunden von Speicherkonten](http://storageexplorer.com/)
