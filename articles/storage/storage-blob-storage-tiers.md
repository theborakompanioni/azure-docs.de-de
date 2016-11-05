---
title: Azure Storage vom Typ „Cool“ für Blobs | Microsoft Docs
description: Speicherebenen für Azure-Blobspeicher ermöglichen eine kostengünstige Speicherung für Objektdaten basierend auf Zugriffsmustern. Die Speicherebene vom Typ „Cool“ ist für Daten optimiert, auf die weniger häufig zugegriffen wird.
services: storage
documentationcenter: ''
author: michaelhauss
manager: vamshik
editor: tysonn

ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/20/2016
ms.author: mihauss;robinsh

---
# Azure Blob Storage: „Heiße“ und „kalte“ Speicherebenen
## Übersicht
Azure Storage verfügt jetzt über zwei Speicherebenen für Blob Storage (Objektspeicher), damit Sie Ihre Daten je nach Nutzung möglichst kostengünstig speichern können. Die **Speicherebene „Heiß“ (Hot)** von Azure ist für die Speicherung von Daten optimiert, auf die häufig zugegriffen wird. Die **Speicherebene „Kalt“ (Cool)** von Azure ist für die Speicherung von Daten optimiert, auf die weniger häufig zugegriffen wird und die langlebiger sind. Bei den Daten der Speicherebene „Cool“ können geringfügige Abstriche bei der Verfügbarkeit gemacht werden, aber Haltbarkeit, Zugriffszeit und Durchsatz müssen sich auf einem ähnlich hohen Niveau wie bei häufig genutzten Daten befinden. Daher kann bei selten genutzten Daten eine Kombination aus einem SLA mit etwas niedrigerer Verfügbarkeit und höheren Zugriffskosten in Kauf genommen werden, um im Gegenzug die Speicherkosten deutlich zu verringern.

Die Speicherung von Daten in der Cloud nimmt immer mehr zu. Um die Kosten für die zunehmenden Speicheranforderungen im Blick zu behalten, ist es hilfreich, die Daten anhand von Attributen wie der Zugriffshäufigkeit und geplanten Aufbewahrungsdauer zu organisieren. In der Cloud gespeicherte Daten können sich stark darin unterscheiden, wie sie über ihre Lebensdauer hinweg generiert, verarbeitet und genutzt werden. Auf einen Teil der Daten wird aktiv zugegriffen, und sie werden während ihrer Lebensdauer geändert. Auf andere Daten wird zu Beginn ihrer Lebensdauer sehr häufig zugegriffen, und die Zugriffe fallen dann deutlich ab, je älter die Daten werden. Außerdem gibt es Daten, die in der Cloud lediglich vorgehalten werden und auf die nach der Speicherung nur sehr selten oder gar nicht zugegriffen wird.

Jedes der oben beschriebenen Datenzugriffsszenarien profitiert von einer differenzierten Speicherebene, die für ein bestimmtes Zugriffsmuster optimiert ist. Dank der Einführung der Speicherebenen vom Typ „Hot“ und „Cool“ können für Azure Blob Storage jetzt diese benötigten unterschiedlichen Speicherebenen mit separaten Preismodellen angeboten werden.

## Blob-Speicherkonten
**Blob-Speicherkonten** sind spezielle Speicherkonten und dienen dazu, unstrukturierte Daten als Blobs (Objekte) in Azure Storage zu speichern. Mit Blob-Speicherkonten haben Sie nun die Wahl zwischen den Speicherebenen „Hot“ und „Cool“. Dadurch können Sie bei seltener genutzten Daten (Cool Data) die Speicherkosten und bei häufig genutzten Daten (Hot Data) die Zugriffskosten senken. Blob-Speicherkonten sind mit Ihren bereits vorhandenen allgemeinen Speicherkonten vergleichbar und besitzen die gleichen Haltbarkeits-, Verfügbarkeits-, Skalierbarkeits- und Leistungseigenschaften, die Sie schon heute verwenden – einschließlich vollständiger API-Konsistenz für Blockblobs und Anfügeblobs.

> [!NOTE]
> Blob-Speicherkonten unterstützen nur Block- und Anfügeblobs, keine Seitenblobs.
> 
> 

Mit dem für Blob-Speicherkonten verfügbaren Attribut **Access Tier** (Zugriffstarif) können Sie die Speicherebene abhängig von den im Konto gespeicherten Daten als **Hot** oder **Cool** angeben. Bei einer Änderung des Nutzungsmusters der Daten können Sie jederzeit zwischen den beiden Speicherebenen wechseln.

> [!NOTE]
> Die Änderung der Speicherebene kann mit zusätzlichen Kosten verbunden sein. Ausführlichere Informationen finden Sie im Abschnitt [Preise und Abrechnung](storage-blob-storage-tiers.md#pricing-and-billing).
> 
> 

Beispielszenarien für die Verwendung der Speicherebene „Hot“:

* Daten, die aktiv verwendet werden oder bei denen eine hohe Zugriffshäufigkeit (Lese- und Schreibvorgänge) zu erwarten ist
* Daten, die zur Verarbeitung und späteren Migration zur Speicherebene „Cool“ bereitgestellt werden

Beispielszenarien für die Verwendung der Speicherebene „Cool“:

* Datasets für Sicherung, Archivierung oder Notfallwiederherstellung
* Ältere Medieninhalte, die nur noch selten angezeigt werden, dann aber umgehend verfügbar sein müssen
* Umfangreiche Datasets, die kostengünstig gespeichert werden müssen, während weitere Daten zur zukünftigen Verarbeitung gesammelt werden (z.B. langfristige Speicherung wissenschaftlicher Daten oder Telemetrie-Rohdaten einer Produktionsstätte)
* Originaldaten (Rohdaten), die auch nach der Umwandlung in ein endgültiges verwendbares Format erhalten bleiben müssen (z.B. Medien-Rohdateien nach der Transcodierung in andere Formate)
* Compliance- und Archivdaten, die über einen langen Zeitraum gespeichert werden müssen und selten verwendet werden (z.B. Überwachungsvideos, alte Röntgenbilder/MRT-Scans für Unternehmen aus dem Gesundheitswesen, Audioaufnahmen und Protokolle von Kundenanrufen bei Finanzdienstleistern)

Weitere Informationen zu Speicherkonten finden Sie unter [Informationen zu Azure-Speicherkonten](storage-create-storage-account.md).

Bei Anwendungen, die nur Block- oder Anfügeblobspeicher benötigen, empfiehlt sich die Verwendung von Blob-Speicherkonten, um vom differenzierten Preismodell des gestaffelten Speichers profitieren zu können. Es gibt allerdings bestimmte Situationen, in denen dies nicht möglich ist und stattdessen allgemeine Speicherkonten verwendet werden müssen. Hierzu zählen etwa folgende Szenarien:

* Sie müssen Tabellen, Warteschlangen oder Dateien verwenden, und die Blobs sollen im gleichen Speicherkonto gespeichert werden. Neben der Verwendung der gleichen Schlüssel hat diese Vorgehensweise allerdings keine weiteren technischen Vorteile.
* Sie müssen weiterhin das klassische Bereitstellungsmodell verwenden. Blob-Speicherkonten sind nur im Rahmen des Azure Resource Manager-Bereitstellungsmodells verfügbar.
* Sie müssen Seitenblobs verwenden. Blob-Speicherkonten unterstützen keine Seitenblobs. Wir empfehlen grundsätzlich die Verwendung von Blockblobs, es sei denn, Sie benötigen explizit Seitenblobs.
* Sie verwenden eine ältere Version der [REST-API für Speicherdienste](https://msdn.microsoft.com/library/azure/dd894041.aspx) (vor 2014-02-14) oder eine Clientbibliothek mit einer niedrigeren Version als 4.x und können kein Upgrade für Ihre Anwendung durchführen.

> [!NOTE]
> Blob-Speicherkonten werden aktuell in den meisten Azure-Regionen unterstützt, und das Angebot wird in Zukunft noch erweitert. Die aktuelle Liste mit verfügbaren Regionen finden Sie auf der Seite [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services).
> 
> 

## Vergleich zwischen Speicherebenen
In der folgenden Tabelle werden die beiden Speicherebenen miteinander verglichen:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250"> <col width="250"> <col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Speicherebene „Hot“</center></strong></td>
    <td><strong><center>Speicherebene „Cool“</center></strong>&lt;/td
</tr>
<tr>
    <td><strong><center>Verfügbarkeit</center></strong></td>
    <td><center>99,9 %</center></td>
    <td><center>99 %</center></td>
</tr>
<tr>
    <td><strong><center>Verfügbarkeit<br>(RA-GRS-Lesevorgänge)</center></strong></td>
    <td><center>99,99 %</center></td>
    <td><center>99,9 %</center></td>
</tr>
<tr>
    <td><strong><center>Nutzungsgebühren</center></strong></td>
    <td><center>Höhere Speicherkosten<br>Geringere Zugriffs- und Transaktionskosten</center></td>
    <td><center>Geringere Speicherkosten<br>Höhere Zugriffs- und Transaktionskosten</center></td>
</tr>
<tr>
    <td><strong><center>Minimale Objektgröße<center></strong></td>
    <td colspan="2"><center>–</center></td>
</tr>
<tr>
    <td><strong><center>Minimale Speicherdauer<center></strong></td>
    <td colspan="2"><center>N/V</center></td>
</tr>
<tr>
    <td><strong><center>Latenz<br>(Zeit bis zum ersten Byte)<center></strong></td>
    <td colspan="2"><center>Millisekunden</center></td>
</tr>
<tr>
    <td><strong><center>Skalierbarkeits- und Leistungsziele<center></strong></td>
    <td colspan="2"><center>Identisch mit allgemeinen Speicherkonten</center></td>
</tr>
</tbody>
</table>

> [!NOTE]
> Blob-Speicherkonten unterstützen die gleichen Leistungs- und Skalierbarkeitsziele wie allgemeine Speicherkonten. Weitere Informationen finden Sie unter [Microsoft Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) (Speicherskalierbarkeit und Leistungsziele von Azure, in englischer Sprache).
> 
> 

## Preise und Abrechnung
Blob-Speicherkonten verwenden ein neues Preismodell für Blobspeicher, das auf der Speicherebene basiert. Bei Verwendung eines Blob-Speicherkontos sind folgende Abrechnungsaspekte zu berücksichtigen:

* **Speicherkosten**: Die Kosten für die Datenspeicherung hängen nicht nur von der gespeicherten Datenmenge ab, sondern auch von der Speicherebene. Die Speicherebene für selten genutzte Daten (Cool) bietet geringere Kosten pro Gigabyte als die Speicherebene für häufig genutzte Daten (Hot).
* **Kosten für den Datenzugriff**: Im Rahmen der Speicherebene für selten genutzte Daten (Cool) fallen pro Gigabyte Zugriffsgebühren für Lese- und Schreibvorgänge an.
* **Transaktionskosten**: Transaktionsgebühren fallen bei beiden Tarifen an. Bei der Speicherebene für selten genutzte Daten (Cool) sind die Transaktionskosten aber höher als bei der Speicherebene für häufig genutzte Daten (Hot).
* **Datenübertragungskosten bei Georeplikation**: Gilt nur für Konten mit konfigurierter Georeplikation (einschließlich GRS und RA-GRS). Die Datenübertragung für die Georeplikation wird pro Gigabyte abgerechnet.
* **Kosten für ausgehende Datenübertragungen**: Ausgehende Datenübertragungen (Daten, die aus einer Azure-Region übertragen werden) werden genau wie bei allgemeinen Speicherkonten nach Bandbreitennutzung pro Gigabyte abgerechnet.
* **Änderung der Speicherebene**: Bei einem Speicherebenenwechsel von „Cool“ zu „Hot“ fällt bei jedem Übergang eine Gebühr an, die den Kosten entspricht, die durch das Lesen aller im Speicherkonto vorhandenen Daten entstehen. Der Wechsel von „Hot“ zu „Cool“ ist dagegen kostenlos.

> [!NOTE]
> Damit Benutzer die neuen Speicherebenen und deren Funktionen vorab testen können, ist der Wechsel von „Cool“ zu „Hot“ bis zum 30. Juni 2016 kostenlos. Ab dem 1. Juli 2016 wird die Gebühr für alle Übergänge von „Cool“ zu „Hot“ erhoben. Ausführlichere Informationen zum Preismodell für Blob-Speicherkonten finden Sie auf der Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/). Ausführlichere Informationen zu den Kosten für ausgehende Datenübertragungen finden Sie auf der Seite [Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/).
> 
> 

## Schnellstart
In diesem Abschnitt werden die folgenden Szenarien unter Verwendung des Azure-Portals veranschaulicht:

* Erstellen eines Blob-Speicherkontos
* Verwalten eines Blob-Speicherkontos

### Verwenden des Azure-Portals
#### Erstellen eines Blob-Speicherkontos mithilfe des Azure-Portals
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü „Hub“ die Option **Neu** > **Daten und Speicher** > **Speicherkonto** aus.
3. Geben Sie einen Namen für Ihr Speicherkonto ein.
   
    Dieser Name muss global eindeutig sein. Er wird als Teil der URL verwendet, über die auf die Objekte im Speicherkonto zugegriffen wird.
4. Wählen Sie als Bereitstellungsmodell die Option **Resource Manager** aus.
   
    Mehrstufiger Speicher kann nur mit Resource Manager-Speicherkonten verwendet werden. Dies ist das empfohlene Bereitstellungsmodell für die neuen Ressourcen. Weitere Informationen finden Sie unter [Übersicht über Azure Resource Manager](../resource-group-overview.md).
5. Wählen Sie in der Dropdownliste „Account Kind“ (Kontoart) die Option **Blob Storage**.
   
    Hier wählen Sie den Speicherkontotyp aus. Mehrstufiger Speicher ist nicht im allgemeinen Speicher verfügbar, sondern nur im Speicher vom Typ „Blobspeicher“.
   
    Beachten Sie, dass die Leistungsebene bei dieser Auswahl auf „Standard“ festgelegt ist. Mehrstufiger Speicher ist für die Leistungsebene „Premium“ nicht verfügbar.
6. Wählen Sie die Replikationsoption für das Speicherkonto aus: **LRS**, **GRS** oder **RA-GRS**. Die Standardeinstellung ist **RA-GRS**.
   
    LRS = lokal redundanter Speicher, GRS = georedundanter Speicher (2 Regionen), RA-GRS = georedundanter Speicher mit Lesezugriff (2 Regionen mit Lesezugriff für die zweite Region).
   
    Weitere Details zu den Replikationsoptionen für Azure Storage finden Sie unter [Azure Storage-Replikation](storage-redundancy.md).
7. Wählen Sie die richtige Speicherebene für Ihre Zwecke aus: Legen Sie **Zugriffstarif** entweder auf **Kalte Daten** oder **Heiße Daten** fest. Der Standardwert ist **Heiße Daten**.
8. Wählen Sie das Abonnement aus, in dem Sie das neue Speicherkonto erstellen möchten.
9. Geben Sie eine neue Ressourcengruppe an, oder wählen Sie eine vorhandene Ressourcengruppe aus. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure-Ressourcen-Manager](../resource-group-overview.md).
10. Wählen Sie die Region für Ihr Speicherkonto aus.
11. Klicken Sie auf **Erstellen**, um das Speicherkonto zu erstellen.

#### Ändern der Speicherebene für ein Blob-Speicherkonto mithilfe des Azure-Portals
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Navigieren Sie zu Ihrem Speicherkonto, wählen Sie „Alle Ressourcen“, und wählen Sie dann Ihr Speicherkonto aus.
3. Klicken Sie im Blatt „Einstellungen“ auf **Konfiguration**, um die Kontokonfiguration anzuzeigen bzw. zu ändern.
4. Wählen Sie die richtige Speicherebene für Ihre Zwecke aus: Legen Sie **Zugriffstarif** entweder auf **Kalte Daten** oder **Heiße Daten** fest.
5. Klicken Sie oben im Blatt auf „Speichern“.

> [!NOTE]
> Die Änderung der Speicherebene kann mit zusätzlichen Kosten verbunden sein. Ausführlichere Informationen finden Sie im Abschnitt [Preise und Abrechnung](storage-blob-storage-tiers.md#pricing-and-billing).
> 
> 

## Evaluieren von und Migrieren zu Blob-Speicherkonten
Dieser Abschnitt dient dazu, Benutzern einen reibungslosen Übergang zur Nutzung von Blob-Speicherkonten zu ermöglichen. Es gibt zwei Benutzerszenarien:

* Sie verfügen über ein vorhandenes allgemeines Speicherkonto und möchten eine Umstellung auf ein Blob-Speicherkonto mit der richtigen Speicherebene evaluieren.
* Sie haben sich für die Nutzung eines Blob-Speicherkontos entschieden oder besitzen bereits ein Konto dieser Art und möchten evaluieren, ob Sie die Speicherebene für „heiße Daten“ (Hot) oder „kalte Daten“ (Cool) verwenden sollen.

In beiden Fällen sollten Sie zuerst die Kosten für die Speicherung und den Zugriff auf Ihre Daten in einem Blob-Speicherkonto schätzen und diesen Betrag mit Ihren derzeitigen Kosten vergleichen.

### Evaluieren von Blob-Speicherkontoebenen
Zum Schätzen der Kosten für die Speicherung und den Zugriff auf die Daten, die in einem Blob-Speicherkonto gespeichert sind, müssen Sie Ihr vorhandenes Nutzungsmuster evaluieren oder eine Annäherung für das erwartete Nutzungsmuster erstellen. Dazu benötigen Sie im Allgemeinen folgende Informationen:

* Ihr Speicherverbrauch: Welche Datenmengen werden gespeichert, und wie verändert sich dies von Monat zu Monat?
* Ihre Speicherzugriffsmuster: Welche Datenmengen werden im Rahmen des Kontos gelesen und geschrieben (einschließlich neuer Daten)? Wie viele Transaktionen werden für den Datenzugriff verwendet, und welche Arten von Transaktionen fallen an?

#### Überwachen von vorhandenen Speicherkonten
Um Ihre vorhandenen Speicherkonten zu überwachen und diese Daten zu sammeln, können Sie Azure-Speicheranalysen nutzen. Damit wird eine Protokollierung durchgeführt, und es werden Metrikdaten für ein Speicherkonto bereitgestellt. Mit Speicheranalysen können Metriken gespeichert werden, die aggregierte Transaktionsstatistiken und Kapazitätsdaten zu Anforderungen an den Blob-Speicherdienst enthalten – sowohl für allgemeine Speicherkonten als auch für Blob-Speicherkonten. Diese Daten werden in bekannten Tabellen in demselben Speicherkonto gespeichert.

Weitere Informationen finden Sie unter [Informationen zu Metriken der Speicheranalyse](https://msdn.microsoft.com/library/azure/hh343258.aspx) und [Schema der Tabellen für Speicheranalysemetriken](https://msdn.microsoft.com/library/azure/hh343264.aspx).

> [!NOTE]
> Blob-Speicherkonten machen den Tabellenspeicherdienst-Endpunkt nur zum Speichern und Zugreifen auf die Metrikdaten für das Konto verfügbar.
> 
> 

Zum Überwachen des Speicherverbrauchs für den Blob-Speicherdienst müssen Sie die Kapazitätsmetriken aktivieren. Wenn dies aktiviert ist, werden täglich Kapazitätsdaten für den Blob-Dienst eines Speicherkontos aufgezeichnet. Sie werden als Tabelleneintrag aufgezeichnet, der in die Tabelle *$MetricsCapacityBlob* desselben Speicherkontos geschrieben wird.

Um das Datenzugriffsmuster für den Blob-Speicherdienst zu überwachen, müssen Sie die stündlichen Transaktionsmetriken auf API-Ebene aktivieren. Wenn dies aktiviert ist, werden jede Stunde die Transaktionen pro API aggregiert und als Tabelleneintrag aufgezeichnet, der in die Tabelle *$MetricsHourPrimaryTransactionsBlob* desselben Speicherkontos geschrieben wird. In der Tabelle *$MetricsHourSecondaryTransactionsBlob* werden für RA-GRS-Speicherkonten die Transaktionen zum sekundären Endpunkt aufgezeichnet.

> [!NOTE]
> Falls Sie über ein allgemeines Speicherkonto verfügen, in dem Sie neben Block- und Anfügeblob-Daten Seitenblobs und Datenträger virtueller Computer gespeichert haben, ist dieser Schätzungsprozess nicht geeignet. Es gibt nämlich keine Möglichkeit, die Kapazitäts- und Transaktionsmetriken basierend auf dem Blob-Typ nur für Block- und Anfügeblobs zu unterscheiden, die zu einem Blob-Speicherkonto migriert werden können.
> 
> 

Um eine gute Annäherung des Datenverbrauchs und der Zugriffsmuster zu erhalten, empfehlen wir Ihnen die Auswahl eines Aufbewahrungszeitraums für die Metriken, der für die reguläre Nutzung repräsentativ ist und den Sie dann extrapolieren können. Eine Option besteht darin, die Metrikdaten für sieben Tage aufzubewahren und die Daten für eine Analyse am Monatsende jede Woche zu erfassen. Eine andere Möglichkeit ist die Aufbewahrung der Metrikdaten der letzten 30 Tage, um sie dann am Ende der 30 Tage zu erfassen und zu analysieren.

Weitere Informationen zum Aktivieren, Erfassen und Anzeigen von Metrikdaten finden Sie unter [Aktivieren der Azure-Speichermetriken und Anzeigen von Metrikdaten](storage-enable-and-view-metrics.md).

> [!NOTE]
> Die Speicherung, der Zugriff und der Download von Analysedaten wird genauso wie für reguläre Benutzerdaten berechnet.
> 
> 

#### Verwenden von Nutzungsmetriken zum Schätzen von Kosten
##### Speicherkosten
Der letzte Eintrag in der Kapazitätsmetrikentabelle *$MetricsCapacityBlob* mit dem Zeilenschlüssel *'data'* enthält die von den Benutzerdaten verbrauchte Speicherkapazität. Der letzte Eintrag in der Kapazitätsmetrikentabelle *$MetricsCapacityBlob* mit dem Zeilenschlüssel *'analytics'* enthält die von den Analyseprotokollen verbrauchte Speicherkapazität.

Diese gesamte Kapazität, die von Benutzerdaten und Analyseprotokollen (falls aktiviert) verbraucht wird, kann dann verwendet werden, um die Kosten für das Speichern der Daten im Speicherkonto zu schätzen. Die gleiche Methode kann auch zum Schätzen der Speicherkosten für Block- und Anfügeblobs in allgemeinen Speicherkonten verwendet werden.

##### Transaktionskosten
Die Summe von *'TotalBillableRequests'* über alle Einträge für eine API in der Transaktionsmetrikentabelle hinweg gibt die Gesamtzahl der Transaktionen für die jeweilige API an. Beispielsweise kann die Gesamtzahl von *'GetBlob'*-Transaktionen in einem Zeitraum anhand der Summe der gesamten abrechenbaren Anforderungen für alle Einträge mit dem Zeilenschlüssel *'user;GetBlob'* berechnet werden.

Um die Transaktionskosten für Blob-Speicherkonten zu schätzen, müssen Sie die Transaktionen in drei Gruppen unterteilen, da unterschiedliche Preise gelten.

* Schreibtransaktionen wie *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* und *'CopyBlob'*.
* Löschtransaktionen wie *'DeleteBlob'* und *'DeleteContainer'*.
* Alle anderen Transaktionen.

Um die Transaktionskosten für allgemeine Speicherkonten zu schätzen, müssen Sie alle Transaktionen unabhängig vom Vorgang bzw. von der API aggregieren.

##### Datenübertragungskosten für Datenzugriff und Georeplikation
Die Speicheranalyse liefert zwar nicht die Menge der Daten, die aus einem Speicherkonto gelesen und in das Speicherkonto geschrieben wird, aber dieser Wert kann grob geschätzt werden, indem die Tabelle mit den Transaktionsmetriken verwendet wird. Die Summe von *'TotalIngress'* über alle Einträge für eine API in der Transaktionsmetrikentabelle hinweg gibt die Gesamtmenge der Eingangsdaten in Byte für die jeweilige API an. Analog dazu gibt die Summe von *'TotalEgress'* die Gesamtmenge der Ausgangsdaten in Byte an.

Um die Datenzugriffskosten für Blob-Speicherkonten zu schätzen, müssen Sie die Transaktionen in zwei Gruppen unterteilen.

* Die Menge der Daten, die aus dem Speicherkonto abgerufen werden, kann geschätzt werden, indem vor allem für die Vorgänge *'GetBlob'* und *'CopyBlob'* die Summe von *'TotalEgress'* geprüft wird.
* Die Menge der Daten, die in das Speicherkonto geschrieben werden, kann anhand der Summe von *'TotalIngress'* für die Vorgänge *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* und *'AppendBlock'* geschätzt werden.

Die Datenübertragungskosten bei Georeplikation für Blob-Speicherkonten können auch berechnet werden, indem die Schätzung für die Menge der geschriebenen Daten verwendet wird, wenn ein GRS- oder RA-GRS-Speicherkonto genutzt wird.

> [!NOTE]
> Ein ausführlicheres Beispiel zur Berechnung der Kosten für die Verwendung der Speicherebene „Heiße Daten“ (Hot) oder „Kalte Daten“ (Cool) finden Sie auf der [Seite mit den Preisen für Azure Storage](https://azure.microsoft.com/pricing/details/storage/) im FAQ-Bereich mit dem Titel *Was sind die Zugriffsebenen „Heiß“ und „Kalt“, und wie bestimme ich, welche Zugriffsebene ich wählen sollte?*.
> 
> 

### Migrieren vorhandener Daten
Ein Blob-Speicherkonto ist ein spezielles Konto, in dem nur Blockblobs und Anfügeblobs gespeichert werden. Bereits vorhandene allgemeine Speicherkonten, die Ihnen das Speichern von Tabellen, Warteschlangen, Dateien und Datenträgern sowie Blobs ermöglichen, können nicht in Blob-Speicherkonten konvertiert werden. Zum Verwenden der Speicherebenen müssen Sie neue Blob-Speicherkonten erstellen und Ihre vorhandenen Daten in die neu erstellten Konten migrieren. Mithilfe der folgenden Methoden können Sie vorhandene Daten von lokalen Speichergeräten, aus dem Cloudspeicher von Drittanbietern oder aus Ihren vorhandenen allgemeinen Azure-Speicherkonten zu Blob-Speicherkonten migrieren:

#### AzCopy
AzCopy ist ein Windows-Befehlszeilenprogramm, mit dem sehr effizient Daten in und aus Azure Storage kopiert werden können. Mit AzCopy können Sie Daten aus Ihren vorhandenen allgemeinen Speicherkonten in das Blob-Speicherkonto kopieren oder Daten von Ihren lokalen Speichergeräten in das Blob-Speicherkonto hochladen.

Ausführlichere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md).

#### Data Movement-Bibliothek
Die Azure Storage Data Movement-Bibliothek für .NET basiert auf dem Kernframework für die Datenverschiebung, das AzCopy zugrunde liegt. Die Bibliothek ist ähnlich wie AzCopy auf leistungsfähige, zuverlässige und einfache Datenübertragungsvorgänge ausgelegt. Dies ermöglicht die native Verwendung aller Vorteile der AzCopy-Features in Ihrer Anwendung, ohne externe Instanzen von AzCopy ausführen und überwachen zu müssen.

Ausführlichere Informationen finden Sie unter [Azure Storage Data Movement-Bibliothek für .NET](https://github.com/Azure/azure-storage-net-data-movement).

#### REST-API oder Clientbibliothek
Sie können eine benutzerdefinierte Anwendung erstellen, um Ihre Daten mithilfe einer der Azure-Clientbibliotheken oder mithilfe der REST-API der Azure Storage-Dienste in ein Blob-Speicherkonto zu migrieren. Azure Storage bietet umfassende Clientbibliotheken für mehrere Programmiersprachen und Plattformen wie .NET, Java, C++, Node.JS, PHP, Ruby und Python. Die Clientbibliotheken bieten erweiterte Funktionen, beispielsweise Wiederholungslogik, Protokollierung und parallele Uploads. Die Entwicklung kann direkt mit der REST-API erfolgen. Diese API lässt sich mithilfe jeder Sprache aufrufen, die HTTP/HTTPS-Anforderungen verarbeitet.

Ausführlichere Informationen finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md).

> [!NOTE]
> Für Blobs, für die die clientseitige Verschlüsselung verwendet wird, werden auf die Verschlüsselung bezogene Metadaten des Blobs gespeichert. Es ist äußerst wichtig, dass für alle Kopiermechanismen sichergestellt wird, dass die Blob-Metadaten – und vor allem die auf die Verschlüsselung bezogenen Metadaten – beibehalten werden. Wenn Sie die Blobs ohne diese Metadaten kopieren, ist der Blob-Inhalt nicht mehr abrufbar. Weitere Informationen zu den auf die Verschlüsselung bezogenen Metadaten finden Sie unter [Clientseitige Azure Storage-Verschlüsselung](storage-client-side-encryption.md).
> 
> 

## Häufig gestellte Fragen
1. **Bleiben bereits vorhandene Speicherkonten weiterhin verfügbar?**
   
    Ja. Bereits vorhandene Speicherkonten sind weiterhin verfügbar (zu gleichen Preisen und mit unveränderten Funktionen). Für sie kann keine Speicherebene ausgewählt werden, und diese Möglichkeit ist auch in Zukunft nicht geplant.
2. **Warum und wann sollte ich Blob-Speicherkonten verwenden?**
   
    Blob-Speicherkonten sind speziell für die Speicherung von Blobs konzipiert und ermöglichen die Einführung neuer, bloborientierter Features. In Zukunft wird für die Speicherung von Blobs die Verwendung von Blob-Speicherkonten empfohlen, und es werden Funktionen wie hierarchische Speicherung und Tarife für diesen Kontotyp eingeführt. Es liegt jedoch bei Ihnen, wann Sie die Migration (abhängig von Ihren geschäftlichen Anforderungen) vornehmen möchten.
3. **Kann ich mein vorhandenes Speicherkonto in ein Blob-Speicherkonto umwandeln?**
   
    Nein. Blob-Speicherkonten sind eine andere Art von Speicherkonto und müssen neu erstellt und die Daten anschließend migriert werden.
4. **Kann ich Objekte innerhalb des gleichen Kontos auf beiden Speicherebenen speichern?**
   
    Das Attribut *Zugriffstarif*, mit dem die Speicherebene angegeben wird, wird auf Kontoebene festgelegt und gilt für alle Objekte dieses Kontos. Sie können das Attribut „Zugriffstarif“ nicht auf einer Objektebene festlegen.
5. **Kann ich die Speicherebene meines Blob-Speicherkontos ändern?**
   
    Ja. Sie können die Speicherebene ändern, indem Sie für das Speicherkonto das Attribut *Zugriffstarif* festlegen. Die Änderung der Speicherebene gilt für alle Objekte, die im Konto gespeichert sind. Der Wechsel von „Hot“ zu „Cool“ ist kostenlos. Beim Wechsel von „Cool“ zu „Hot“ wird dagegen eine GB-basierte Gebühr für das Lesen aller Daten des Kontos erhoben.
6. **Wie oft kann ich die Speicherebene für mein Blob-Speicherkonto ändern?**
   
    Die Anzahl von Speicherebenenänderungen wird von uns zwar nicht begrenzt, aber Sie sollten bedenken, dass ein Wechsel von „Cool“ zu „Hot“ mit erheblichen Kosten verbunden ist. Wir raten daher von häufigen Speicherebenenwechseln ab.
7. **Verhalten sich die Blobs bei der Speicherebene „Cool“ anders als die Blobs bei der Speicherebene „Hot“?**
   
    Blobs der Speicherebene „Hot“ weisen die gleiche Latenz wie Blobs von allgemeinen Speicherkonten auf. Blobs der Speicherebene „Cool“ verfügen über eine ähnliche Latenz (in Millisekunden) wie Blobs von allgemeinen Speicherkonten.
   
    Für Blobs der Speicherebene „Cool“ gilt in Bezug auf die Verfügbarkeit ein etwas geringerer Servicelevel (SLA) als für Blobs, die unter der Speicherebene „Hot“ gespeichert sind. Weitere Details finden Sie unter [SLA für Speicher](https://azure.microsoft.com/support/legal/sla/storage).
8. **Kann ich Seitenblobs und Datenträger virtueller Computer unter Blob-Speicherkonten speichern?**
   
    Blob-Speicherkonten unterstützen nur Block- und Anfügeblobs, keine Seitenblobs. Datenträger virtueller Azure-Computer werden durch Seitenblobs unterstützt. Daher können Blob-Speicherkonten nicht zum Speichern der Datenträger von virtuellen Computern verwendet werden. Es ist aber möglich, Sicherungen der Datenträger virtueller Computer als Blockblobs unter einem Blob-Speicherkonto zu speichern.
9. **Muss ich zur Verwendung von Blob-Speicherkonten meine vorhandenen Anwendungen ändern?**
   
    Blob-Speicherkonten sind zu 100% API-konsistent mit allgemeinen Speicherkonten für Block- und Anfügeblobs. Solange Sie für Ihre Anwendung Blockblobs oder Anfügeblobs verwenden und Version 2014-02-14 (oder höher) der [REST-API für Speicherdienste](https://msdn.microsoft.com/library/azure/dd894041.aspx) nutzen, sollte Ihre Anwendung problemlos funktionieren. Bei Verwendung einer älteren Protokollversion müssen Sie Ihre Anwendung aktualisieren, sodass sie die neue Version verwendet und beide Arten von Speicherkonten problemlos verwendet werden können. Unabhängig von der Art des verwendeten Speicherkontos empfehlen wir grundsätzlich die Verwendung der neuesten Version.
10. **Ändert sich etwas für die Benutzer?**
    
    Blob-Speicherkonten weisen eine hohe Ähnlichkeit mit allgemeinen Speicherkonten zum Speichern von Block- und Anfügeblobs auf und unterstützen alle wesentlichen Funktionen von Azure Storage, z.B. hohe Stabilität und Verfügbarkeit, Skalierbarkeit, Leistung und Sicherheit. Mit Ausnahme der spezifischen Features und Einschränkungen für Blob-Speicherkonten und der weiter oben erläuterten Speicherebenen ändert sich nichts.

## Nächste Schritte
### Auswerten von Blob-Speicherkonten
[Überprüfen der Verfügbarkeit von Blob-Speicherkonten nach Region unter Azure-Regionen](https://azure.microsoft.com/regions/#services)

[Auswerten der Nutzung vorhandener Speicherkonten durch Aktivierung von Azure Storage-Metriken](storage-enable-and-view-metrics.md)

[Überprüfen der Preise für Blobspeicher nach Region unter Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)

[Überprüfen der Preise für Datenübertragungen unter Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/)

### Beginnen mit der Nutzung von Blob-Speicherkonten
[Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md)

[Verschieben von Daten in und aus Azure Storage](storage-moving-data.md)

[Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md)

[Durchsuchen und Erkunden von Speicherkonten](http://storageexplorer.com/)

<!---HONumber=AcomDC_0921_2016-->