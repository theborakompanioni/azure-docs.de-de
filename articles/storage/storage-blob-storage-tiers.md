<properties
	pageTitle="Azure-Blobspeicher: Tarife für selten genutzte Daten (Cool Data) und häufig genutzte Daten (Hot Data) | Microsoft Azure"
	description="Eine Übersicht über Azure-Blobspeichertarife zur kostengünstigen Speicherung von Objektdaten auf der Grundlage von Zugriffsmustern."
	services="storage"
	documentationCenter=""
	authors="sribhat-msft"
	manager="vamshik"
	editor="tysonn"/>

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="04/24/2016"
	ms.author="sribhat"/>


# Azure-Blobspeicher: Tarife für selten genutzte Daten (Cool Data) und häufig genutzte Daten (Hot Data)

## Übersicht

Azure-Blobspeicher ist eine kostengünstige und skalierbare Lösung zum Speichern großer unstrukturierter Datenmengen in der Cloud. Dank gewaltiger Speicherkapazität und erstklassiger Skalierbarkeit lassen sich mit Blobspeicher einfach und kostengünstig mehrere Petabytes an Daten und Milliarden von Objekten pro Kunde speichern. Ausführlichere Informationen finden Sie unter [Einführung in Microsoft Azure Storage](storage-introduction.md).

Die Menge der in der Cloud gespeicherten Daten nimmt heutzutage exponentiell zu. Ein wichtiger Aspekt bei der Verwaltung der Kosten Ihres wachsenden Speicherbedarfs ist die Aufteilung der Daten auf der Grundlage von Attributen wie Zugriffshäufigkeit, Aufbewahrungszeitrum und Ähnlichem. Anhand von Kundenfeedback und Verwendungsmustern haben wir ermittelt, dass sich die in der Cloud gespeicherten Daten in puncto Generierung, Verarbeitung und Zugriff während ihrer Lebensdauer stark unterscheiden können. Einige Daten werden während ihrer Lebensdauer aktiv verwendet und geändert. Auf andere wird zu Beginn des Lebenszyklus sehr häufig zugegriffen, mit zunehmendem Alter der Daten aber immer seltener. Und wieder andere Daten werden nach der Speicherung in der Cloud nur ganz selten (oder gar nicht) verwendet. Im Allgemeinen unterscheiden wir zwischen häufig genutzten Daten, die ein hohes Maß an Stabilität und Verfügbarkeit erfordern (**Hot Data**), und selten genutzten Daten mit hoher Lebensdauer (**Cool Data**). Bei selten genutzten Daten können geringfügige Abstriche bei der Verfügbarkeit gemacht werden, Haltbarkeit, Zugriffszeit und Durchsatz müssen sich jedoch auf einem ähnlich hohen Niveau befinden wie bei häufig genutzten Daten. Daher kann bei selten genutzten Daten eine Kombination aus einem SLA mit etwas niedrigerer Verfügbarkeit und höheren Zugriffskosten in Kauf genommen werden, um im Gegenzug die Speicherkosten deutlich zu verringern.

Jedes der oben beschriebenen Datenzugriffsszenarien profitiert von einem differenzierten, für ein bestimmtes Zugriffsmuster optimierten Speichertarif. Azure-Blobspeicher bietet nun differenzierte Speichertarife für Daten mit unterschiedlichem Zugriffsmuster und Preismodell.

## Blob-Speicherkonten

**Blob-Speicherkonten** sind spezielle Speicherkonten und dienen dazu, unstrukturierte Daten als Blobs (Objekte) in Azure Storage zu speichern. Mit Blob-Speicherkonten haben Sie nun die Wahl zwischen Zugriffstarifen für selten und häufig genutzte Daten. Dadurch können Sie bei seltener genutzten Daten (Cool Data) die Speicherkosten und bei häufig genutzten Daten (Hot Data) die Zugriffskosten senken. Blob-Speicherkonten sind mit Ihren bereits vorhandenen allgemeinen Speicherkonten vergleichbar und besitzen die gleichen Haltbarkeits-, Verfügbarkeits-, Skalierbarkeits- und Leistungseigenschaften, die Sie schon heute verwenden – einschließlich vollständiger API-Konsistenz für Blockblobs und Anfügeblobs.

Mit dem für Blob-Speicherkonten verfügbaren Attribut **Access Tier** können Sie den Zugriffstarif abhängig von den im Konto gespeicherten Daten als **Hot** oder **Cool** angeben. Bei einer Änderung des Nutzungsmusters der Daten können Sie jederzeit zwischen den beiden Zugriffstarifen wechseln.

> [AZURE.NOTE] Die Änderung des Zugriffstarifs kann mit zusätzlichen Kosten verbunden sein. Ausführlichere Informationen finden Sie weiter unten im Abschnitt [Preise und Abrechnung](storage-blob-storage-tiers.md#pricing-and-billing).

Beispielszenarien für die Verwendung des Zugriffstarifs für häufig genutzte Daten (Hot):

- Daten, die aktiv verwendet werden oder bei denen eine hohe Zugriffshäufigkeit (Lese- und Schreibvorgänge) zu erwarten ist
- Daten, die zur Verarbeitung und späteren Migration in den Tarif für selten genutzte Daten (Cool) bereitgestellt werden

Beispielszenarien für die Verwendung des Zugriffstarifs für selten genutzte Daten (Cool):

- Datasets für Sicherung, Archivierung oder Notfallwiederherstellung
- Ältere Medieninhalte, die nur noch selten angezeigt werden, dann aber umgehend verfügbar sein müssen
- Umfangreiche Datasets, die kostengünstig gespeichert werden müssen, während weitere Daten zur zukünftigen Verarbeitung gesammelt werden (*beispielsweise* langfristige Speicherung wissenschaftlicher Daten oder Telemetrie-Rohdaten einer Produktionsstätte)
- Originaldaten (Rohdaten), die auch nach der Umwandlung in ein endgültiges verwendbares Format erhalten bleiben müssen (*beispielsweise* Medien-Rohdateien nach der Transcodierung in andere Formate)
- Compliance- und Archivdaten, die über einen langen Zeitraum gespeichert werden müssen und selten verwendet werden (*beispielsweise* Überwachungsvideos, alte Röntgenbilder/MRT-Scans für Unternehmen aus dem Gesundheitswesen, Audioaufnahmen und Protokolle von Kundenanrufen bei Finanzdienstleistern)

Weitere Informationen zu Speicherkonten finden Sie unter [Informationen zu Azure-Speicherkonten](storage-create-storage-account.md).

Bei Anwendungen, die nur Block- oder Anfügeblobspeicher benötigen, empfiehlt sich die Verwendung von Blob-Speicherkonten, um vom differenzierten Preismodell des gestaffelten Speichers profitieren zu können. Es gibt allerdings bestimmte Situationen, in denen dies nicht möglich ist und stattdessen allgemeine Speicherkonten verwendet werden müssen. Hierzu zählen etwa folgende Szenarien:

- Sie müssen Tabellen, Warteschlangen oder Dateien verwenden, und die Blobs sollen im gleichen Speicherkonto gespeichert werden. Neben der Verwendung der gleichen Schlüssel hat diese Vorgehensweise allerdings keine weiteren technischen Vorteile.
- Sie müssen weiterhin das klassische Bereitstellungsmodell verwenden. Blob-Speicherkonten sind nur im Rahmen des Azure Resource Manager-Bereitstellungsmodells verfügbar.
- Sie müssen Seitenblobs verwenden. Wir empfehlen grundsätzlich die Verwendung von Blockblobs, es sei denn, Sie benötigen explizit Seitenblobs.
- Sie verwenden eine ältere Version der [REST-API für Speicherdienste](https://msdn.microsoft.com/library/azure/dd894041.aspx) (vor 2014-02-14) oder eine Clientbibliothek mit einer niedrigeren Version als 4.x und können kein Upgrade für Ihre Anwendung durchführen.

> [AZURE.NOTE] Blob-Speicherkonten werden aktuell in den meisten Azure-Regionen unterstützt, und das Angebot wird in Zukunft noch erweitert. Die aktuelle Liste mit verfügbaren Regionen finden Sie auf der Seite [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services).

## Vergleich der Zugriffstarife

In der folgenden Tabelle werden die beiden Zugriffstarife miteinander verglichen:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250"> <col width="250"> <col width="250">
<tbody>
<tr>
	<td><strong><center></center></strong></td>
	<td><strong><center>Hot-Zugriffstarif</center></strong></td>
	<td><strong><center>Cool-Zugriffstarif</center></strong>&lt;/td
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
	<td><strong><center>Leistung und Skalierbarkeit<center></strong></td>
	<td colspan="2"><center>Identisch mit allgemeinen Speicherkonten</center></td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] Blob-Speicherkonten unterstützen die gleichen Leistungs- und Skalierbarkeitsziele wie allgemeine Speicherkonten. Weitere Informationen finden Sie unter [Microsoft Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) (Speicherskalierbarkeit und Leistungsziele von Azure, in englischer Sprache).

## Preise und Abrechnung

Blob-Speicherkonten verwenden ein neues, zugriffstarifbasiertes Preismodell für Blobspeicher. Bei Verwendung eines Blob-Speicherkontos sind folgende Abrechnungsaspekte zu berücksichtigen:

- **Speicherkosten**: Die Kosten für die Datenspeicherung hängen nicht nur von der gespeicherten Datenmenge ab, sondern auch vom Zugriffstarif. Der Zugriffstarif für selten genutzte Daten (Cool) bietet geringere Kosten pro Gigabyte als der Zugriffstarif für häufig genutzte Daten (Hot).
- **Kosten für den Datenzugriff**: Im Rahmen des Zugriffstarifs für selten genutzte Daten (Cool) fallen pro Gigabyte Zugriffsgebühren für Lese- und Schreibvorgänge an.
- **Transaktionskosten**: Transaktionsgebühren fallen bei beiden Tarifen an. Beim Zugriffstarif für selten genutzte Daten (Cool) sind die Transaktionskosten jedoch höher als beim Zugriffstarif für häufig genutzte Daten (Hot).
- **Datenübertragungskosten bei Georeplikation**: Gilt nur für Konten mit konfigurierter Georeplikation (einschließlich GRS und RA-GRS). Die Datenübertragung für die Georeplikation wird pro Gigabyte abgerechnet.
- **Kosten für ausgehende Datenübertragungen**: Ausgehende Datenübertragungen (Daten, die aus einer Azure-Region übertragen werden) werden genau wie bei allgemeinen Speicherkonten nach Bandbreitennutzung pro Gigabyte abgerechnet.
- **Änderung des Zugriffstarifs**: Bei einem Zugriffstarifwechsel von „Cool“ zu „Hot“ fällt bei jedem Übergang eine Gebühr an, die den Kosten entspricht, die durch das Lesen aller im Speicherkonto vorhandenen Daten entstehen. Der Wechsel von „Hot“ zu „Cool“ ist dagegen kostenlos.

> [AZURE.NOTE] Damit Benutzer die neuen Speichertarife und deren Funktionen vorab testen können, ist der Wechsel von „Cool“ zu „Hot“ bis zum 30. Juni 2016 kostenlos. Ab dem 1. Juli 2016 wird die Gebühr für alle Übergänge von „Cool“ zu „Hot“ erhoben. Ausführlichere Informationen zum Preismodell für Blob-Speicherkonten finden Sie auf der Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/). Ausführlichere Informationen zu den Kosten für ausgehende Datenübertragungen finden Sie auf der Seite [Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/).

## Schnellstart

In diesem Abschnitt werden die folgenden Szenarien unter Verwendung des Azure-Portals veranschaulicht:

- Erstellen eines Blob-Speicherkontos
- Verwalten eines Blob-Speicherkontos

### Verwenden des Azure-Portals

#### Erstellen eines Blob-Speicherkontos mithilfe des Azure-Portals

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie im Menü „Hub“ die Option **Neu** -> **Daten und Speicher** -> **Speicherkonto** aus.

3. Geben Sie einen Namen für Ihr Speicherkonto ein.

4. Wählen Sie als Bereitstellungsmodell die Option **Resource Manager** aus.

5. Wählen Sie als Speicherkontotyp die Option **Blob Storage** aus.

6. Wählen Sie den Zugriffstarif (**Hot** oder **Cool**) aus. Der Standardwert ist **Hot**.

7. Wählen Sie die Replikationsoption für das Speicherkonto aus: **LRS**, **GRS** oder **RA-GRS**. Die Standardeinstellung ist **RA-GRS**. Weitere Details zu den Replikationsoptionen für Azure Storage finden Sie unter [Azure Storage-Replikation](storage-redundancy.md).

8. Wählen Sie das Abonnement aus, in dem Sie das neue Speicherkonto erstellen möchten.

9. Geben Sie eine neue Ressourcengruppe an, oder wählen Sie eine vorhandene Ressourcengruppe aus. Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen](../azure-portal/resource-group-portal.md).

10. Wählen Sie den geografischen Standort für das Speicherkonto aus.

11. Klicken Sie auf **Erstellen**, um das Speicherkonto zu erstellen.

#### Ändern des Zugriffstarifs für ein Blob-Speicherkonto mithilfe des Azure-Portals

1.	Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem Speicherkonto.

2.	Klicken Sie auf **Alle Einstellungen** und anschließend auf **Konfiguration**, um die Kontokonfiguration anzuzeigen und/oder zu ändern.

3.	Geben Sie den gewünschten Zugriffstarif (**Hot** oder **Cool**) an.

## Migrieren zu Blob-Speicherkonten

Dieser Abschnitt dient dazu, Benutzern einen reibungslosen Übergang zu Blob-Speicherkonten zu ermöglichen. Ein Blob-Speicherkonto ist ein spezielles Konto, in dem nur Blobs gespeichert werden. Bereits vorhandene allgemeine Speicherkonten sind auch zum Speichern von Tabellen, Warteschlangen und Dateien geeignet und können nicht in Blob-Speicherkonten konvertiert werden. Daher müssen Sie neue Blob-Speicherkonten mit dem entsprechenden Zugriffstarif erstellen und Ihre vorhandenen Daten in die neu erstellten Konten migrieren, um die Features von Blob-Speicherkonten nutzen zu können.

### Planen der Migration vorhandener Daten

Wenn Sie Ihre Daten in ein Blob-Speicherkonto verschieben, möchten Sie wahrscheinlich den Zugriffstarif für selten genutzte Daten (Cool Data) verwenden, um Speicherkosten für diese Daten zu sparen. Ermitteln Sie vor Migration von Daten zu einem Blob-Speicherkonto mit dem Zugriffstarif für selten genutzte Daten zunächst auf der Grundlage Ihres aktuellen Verwendungsmusters, ob die Migration zu einem Blob-Speicherkonto für Sie sinnvoll ist. Dazu benötigen Sie im Allgemeinen folgende Informationen:

- Ihr Speicherverwendungsmuster: Welche Datenmengen werden gespeichert, und wie verändert sich dies von Monat zu Monat?
- Ihre Speicherzugriffsmuster: Welche Datenmengen werden im Rahmen des Kontos gelesen und geschrieben (einschließlich neuer Daten)? Wie viele und welche Transaktionen werden für den Datenzugriff verwendet?

Informationen zum Überwachen Ihrer vorhandenen Speicherkonten und zum Sammeln der entsprechenden Daten finden Sie unter [Aktivieren der Azure-Speichermetriken und Anzeigen von Metrikdaten](storage-enable-and-view-metrics.md). Auf der Grundlage dieser Daten können Sie dann mithilfe des [Azure Storage-Preisrechners](https://azure.microsoft.com/pricing/calculator/?scenario=data-management) Ihre Kosten bestimmen.

### Migrieren vorhandener Daten

Mithilfe der folgenden Methoden können Sie vorhandene Daten aus einer lokalen Speicherlösung, aus dem Cloudspeicher eines Drittanbieters oder aus Ihren vorhandenen allgemeinen Azure-Speicherkonten in Blob-Speicherkonten migrieren:

#### AzCopy

AzCopy ist ein Windows-Befehlszeilenprogramm, mit dem sehr effizient Daten in und aus Azure Storage kopiert werden können. Mit AzCopy können Sie Daten aus Ihren vorhandenen allgemeinen Speicherkonten in Ihr Blob-Speicherkonto kopieren oder Daten aus Ihrem lokalen Speichersystem in Ihr Blob-Speicherkonto hochladen.

Ausführlichere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md).

#### Data Movement-Bibliothek

Die Azure Storage Data Movement-Bibliothek für .NET basiert auf dem Kernframework für die Datenverschiebung, das AzCopy zugrunde liegt. Die Bibliothek ist ähnlich wie AzCopy auf leistungsfähige, zuverlässige und einfache Datenübertragungsvorgänge ausgelegt. Dies ermöglicht die native Verwendung aller Vorteile der AzCopy-Features in Ihrer Anwendung, ohne externe Instanzen von AzCopy ausführen und überwachen zu müssen.

Ausführlichere Informationen finden Sie unter [Azure Storage Data Movement-Bibliothek für .NET](https://github.com/Azure/azure-storage-net-data-movement).

#### REST-API oder Clientbibliothek

Sie können eine benutzerdefinierte Anwendung erstellen, um Ihre Daten mithilfe einer der Azure-Clientbibliotheken oder mithilfe der REST-API der Azure Storage-Dienste in ein Blob-Speicherkonto zu migrieren. Azure Storage bietet umfassende Clientbibliotheken für mehrere Programmiersprachen und Plattformen wie .NET, Java, C++, Node.JS, PHP, Ruby und Python. Die Clientbibliotheken bieten erweiterte Funktionen, beispielsweise Wiederholungslogik, Protokollierung und parallele Uploads. Die Entwicklung kann direkt mit der REST-API erfolgen. Diese API lässt sich mithilfe jeder Sprache aufrufen, die HTTP/HTTPS-Anforderungen verarbeitet.

Ausführlichere Informationen finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md).

## Häufig gestellte Fragen

1.	**Bleiben bereits vorhandene Speicherkonten weiterhin verfügbar?**

    Ja. Bereits vorhandene Speicherkonten sind weiterhin verfügbar (zu gleichen Preisen und mit unveränderten Funktionen). Für sie kann kein Zugriffstarif ausgewählt werden, und diese Möglichkeit ist auch in Zukunft nicht geplant.

2.	**Warum und wann sollte ich Blob-Speicherkonten verwenden?**

    Blob-Speicherkonten sind speziell für die Speicherung von Blobs konzipiert und ermöglichen die Einführung neuer, bloborientierter Features. In Zukunft wird für die Speicherung von Blobs die Verwendung von Blob-Speicherkonten empfohlen, und es werden Funktionen wie hierarchische Speicherung und Tarife für diesen Kontotyp eingeführt. Es liegt jedoch bei Ihnen, wann Sie die Migration (abhängig von Ihren geschäftlichen Anforderungen) vornehmen möchten.

3.	**Kann ich mein vorhandenes Speicherkonto in ein Blob-Speicherkonto umwandeln?**

    Nein. Blob-Speicherkonten sind eine andere Art von Speicherkonto und müssen neu erstellt und die Daten anschließend migriert werden.

4.	**Kann ich Objekte innerhalb des gleichen Kontos in beiden Zugriffstarifen speichern?**

    Derzeit leider nicht. Das Zugriffstarifattribut wird auf Kontoebene festgelegt und gilt für alle Objekte dieses Kontos.

5.	**Kann ich den Zugriffstarif für mein Blob-Speicherkonto ändern?**

    Ja. Der Zugriffstarif für das Speicherkonto kann geändert werden. Die Änderung des Zugriffstarifs auf Kontoebene gilt für alle im Konto gespeicherten Objekte. Der Wechsel von „Hot“ zu „Cool“ ist kostenlos. Beim Wechsel von „Cool“ zu „Hot“ wird dagegen eine GB-basierte Gebühr für das Lesen aller Daten des Kontos erhoben.

6.	**Wie oft kann ich den Zugriffstarif für mein Blob-Speicherkonto ändern?**

    Die Anzahl der Zugriffstarifänderungen wird von uns zwar nicht begrenzt, bedenken Sie aber, dass ein Wechsel von „Cool“ zu „Hot“ mit erheblichen Kosten verbunden ist. Wir raten daher von häufigen Zugriffstarifwechseln ab.

7.	**Muss ich zur Verwendung von Blob-Speicherkonten meine vorhandenen Anwendungen ändern?**

    Blob-Speicherkonten sind vollständig mit allgemeinen Speicherkonten konsistent – vorausgesetzt, Sie verwenden die neueste Version (2014-02-14) der [REST-API für Speicherdienste](https://msdn.microsoft.com/library/azure/dd894041.aspx). Bei Verwendung einer älteren Protokollversion müssen Sie Ihre Anwendung aktualisieren, sodass sie die neue Version verwendet und beide Arten von Speicherkonten problemlos verwendet werden können. Unabhängig von der Art des verwendeten Speicherkontos empfehlen wir grundsätzlich die Verwendung der neuesten Version.

8.	**Ändert sich etwas für die Benutzer?**

    Abgesehen davon, dass in einem Blob-Speicherkonto nur Blobs gespeichert werden, ist alles ähnlich wie bei allgemeinen Speicherkonten. Ein Blob-Speicherkonto bietet alle wesentlichen Funktionen von Azure Storage – einschließlich hohe Stabilität und Verfügbarkeit, Skalierbarkeit, Leistung und Sicherheit. Mit Ausnahme der spezifischen Features und Einschränkungen für Blob-Speicherkonten und der weiter oben erläuterten Zugriffstarife ändert sich nichts.

<!---HONumber=AcomDC_0427_2016-->