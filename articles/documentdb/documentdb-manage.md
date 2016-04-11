<properties
	pageTitle="DocumentDB-Speicherung und -Leistung | Microsoft Azure" 
	description="Informationen zur Speicherung von Daten und Dokumenten in DocumentDB und zum Skalieren von DocumentDB zum Erfüllen der Kapazitätsanforderungen Ihrer Anwendung." 
	keywords="Speicherung von Dokumenten"
	services="documentdb" 
	authors="mimig1" 
	manager="jhubbard" 
	editor="cgronlun" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="03/30/2016" 
	ms.author="mimig"/>

# Speicherung und vorhersagbare Leistungsbereitstellung in DocumentDB
DocumentDB ist ein vollständig verwalteter, skalierbarer dokumentorientierter NoSQL-Datenbankdienst für JSON-Dokumente. Mit DocumentDB müssen Sie keine virtuellen Computer mieten, Software bereitstellen, Datenbanken überwachen oder sich Gedanken über die Notfallwiederherstellung machen. DocumentDB wird von Microsoft-Entwicklern betrieben und ständig überwacht, um erstklassige Verfügbarkeit, Leistung und Datensicherheit zu gewährleisten.

Sie können sich mit DocumentDB vertraut machen, indem Sie über das [Azure-Portal](documentdb-create-account.md) ein [Datenbankkonto erstellen](https://portal.azure.com/). DocumentDB wird in Einheiten aus SSD-gestütztem Speicher und Durchsatz angeboten. Diese Einheiten werden bereitgestellt, indem in Ihrem Datenbankkonto Datenbanksammlungen erstellt werden. Für jede Sammlung gilt ein reservierter Durchsatz. Wenn sich die Durchsatzanforderungen Ihrer Anwendung ändern, können Sie diese Einstellung dynamisch ändern, indem Sie die [Leistungsebene](documentdb-performance-levels.md) für jede Sammlung festlegen.

Wenn Ihre Anwendung die Leistungsebene für eine oder mehrere Sammlungen überschreitet, werden die Anforderungen pro Sammlung beschränkt. Dies bedeutet, dass einige Anforderungen erfolgreich sind, während andere aufgrund der Beschränkung fehlschlagen.

Dieser Artikel bietet eine Übersicht über die Ressourcen und Metriken zum Verwalten der Kapazität und Planen der Datenspeicherung.

## Datenbankkonto und Verwaltungsressourcen
Als Azure-Abonnent können Sie ein oder mehrere DocumentDB-Datenbankkonten bereitstellen. Jedes Datenbankonto verfügt über ein Kontingent von administrativen Ressourcen wie Datenbanken, Benutzern und Berechtigungen. Diese Ressourcen unterliegen [Grenzen und Kontingenten](documentdb-limits.md). [Wenden Sie sich an den Support](documentdb-increase-limits.md), wenn Sie zusätzliche administrative Ressourcen benötigen.

Das Azure-Verwaltungsportal bietet Nutzungsmetriken für die Überwachung von Datenbankkonten, Datenbanken und Sammlungen. Weitere Informationen finden Sie unter [Überwachen eines DocumentDB-Kontos](documentdb-monitor-accounts.md).

## Datenbanken
Eine einzelne DocumentDB-Datenbank kann eine nahezu unbegrenzte Menge Dokumentspeicher enthalten, der anhand von Sammlungen gruppiert wird. Sammlungen ermöglichen eine Leistungsisolierung. Jede Sammlung kann mit einem Durchsatz bereitgestellt werden, der für andere Sammlungen in derselben Datenbank oder unter demselben Konto nicht freigegeben ist. Die Größe einer DocumentDB-Datenbank ist flexibel und reicht vom GB-Bereich bis hin zu SSD-gestütztem Dokumentspeicher im TB-Bereich mit bereitgestelltem Durchsatz. Im Gegensatz zu einer herkömmlichen RDBMS-Datenbank gilt der Bereich einer Datenbank in DocumentDB nicht nur für einen einzelnen Computer und kann mehrere Computer oder Cluster umfassen.

Mit DocumentDB können Sie weitere Sammlungen oder Datenbanken bzw. beides erstellen, wenn Sie Ihre Anwendungen skalieren müssen.

## Datenbanksammlungen
Jede DocumentDB-Datenbank kann eine oder mehrere Auflistungen enthalten. Sammlungen dienen als hoch verfügbare Datenpartitionen für die Speicherung und Verarbeitung von Dokumenten. Jede Sammlung kann Dokumente mit heterogenem Schema speichern. Die automatischen Indizierungs- und Abfragefunktionen von DocumentDB ermöglichen Ihnen das einfache Filtern und Abrufen von Dokumenten. Eine Auflistung liefert den Umfang für Dokumentspeicher und Abfrageausführung. Eine Auflistung ist auch eine Transaktionsdomäne für ale Dokumente, die darin enthalten sind. Sammlungen wird der Durchsatz basierend auf der dafür angegebenen Leistungsebene zugeordnet. Dies kann über das Azure-Portal oder eines der SDKs dynamisch angepasst werden.

Sammlungen werden von DocumentDB automatisch in einen oder mehrere physische Server partitioniert. Wenn Sie eine Sammlung erstellen, können Sie den bereitgestellten Durchsatz in Form von Anforderungseinheiten pro Sekunde und einer Partitionsschlüsseleigenschaft angeben. Der Wert dieser Eigenschaft wird von DocumentDB zum Verteilen von Dokumenten zwischen Partitionen und zum Weiterleiten von Anforderungen wie Abfragen verwendet. Der Partitionsschlüssel fungiert außerdem als die Transaktionsgrenze für gespeicherte Prozeduren und Trigger. Jede Sammlung hat eine reservierte Durchsatzmenge, die für diese Sammlung spezifisch ist und die nicht mit anderen Sammlungen im gleichen Konto gemeinsam genutzt wird. Daher können Sie Ihre Anwendung sowohl im Hinblick auf Speicher als auch auf Durchsatz horizontal hochskalieren.

Sammlungen können über das [Azure-Portal](https://portal.azure.com/) oder eines der [DocumentDB SDKs](documentdb-sdk-dotnet.md) erstellt werden.
 
## Anforderungseinheiten und Datenbankvorgänge

Wenn Sie eine Sammlung erstellen, reservieren Sie den Durchsatz hinsichtlich der Anforderungseinheiten (RUs) pro Sekunde. Anstatt sich Gedanken über Hardwareressourcen und deren Verwaltung zu machen, können Sie sich eine **Anforderungseinheit** als alleinige Maßeinheit für die Ressourcen vorstellen, die für das Durchführen der verschiedenen Datenbankvorgänge und das Ausführen einer Anwendungsanforderung erforderlich sind. Beim Lesen eines Dokuments mit 1 KB wird immer 1 RU verbraucht. Dies gilt unabhängig davon, wie viele Elemente in der Sammlung gespeichert sind oder wie viele gleichzeitige Anforderungen darin ausgeführt werden. Alle Anforderungen für DocumentDB, z.B. auch komplexe Vorgänge wie SQL-Abfragen, verfügen über einen vorhersagbaren RU-Wert, der zur Entwicklungszeit bestimmt werden kann. Wenn Sie die Größe der Dokumente und die Häufigkeit der einzelnen Vorgänge (Lese-, Schreib- und Abfragevorgänge) kennen, die für Ihre Anwendung unterstützt werden müssen, können Sie die genaue Durchsatzmenge bereitstellen, mit der die Anforderungen Ihrer Anwendung erfüllt werden. Sie können die Datenbank horizontal hochskalieren und zentral herunterskalieren, wenn sich die Leistungsanforderungen ändern.

Für jede Sammlung kann ein Durchsatz in Blöcken von 100 Anforderungseinheiten pro Sekunde reserviert werden. Diese Reservierung kann von Hunderten bis zu Millionen von Anforderungseinheiten pro Sekunde reichen. Der bereitgestellte Durchsatz kann während der gesamten Lebensdauer einer Sammlung angepasst werden, um sie auf die veränderten Verarbeitungsanforderungen und Zugriffsmuster der Anwendung abzustimmen. Weitere Informationen finden Sie unter [Leistungsebenen in DocumentDB](documentdb-performance-levels.md).

>[AZURE.IMPORTANT] Sammlungen sind fakturierbare Entitäten. Die Kosten werden anhand des bereitgestellten Durchsatzes der Sammlung in Anforderungseinheiten pro Sekunde zusammen mit dem insgesamt verbrauchten Speicher in GB ermittelt.

Wie viele Anforderungseinheiten werden bei einem bestimmten Vorgang wie dem Einfügen, Löschen, Abfragen oder dem Ausführen einer gespeicherten Prozedur verbraucht? Eine Anforderungseinheit ist eine normalisierte Kennzahl für die Anforderungsverarbeitungskosten. Ein Lesevorgang eines Dokuments mit 1 KB entspricht 1 RU, aber eine Anforderung zum Einfügen, Ersetzen oder Löschen des gleichen Dokuments verbraucht mehr Verarbeitungsleistung des Diensts und daher mehr Anforderungseinheiten. Jede Antwort des Diensts enthält einen benutzerdefinierten Header (`x-ms-request-charge`), mit dem die für die Anforderung verbrauchten Anforderungseinheiten gemeldet werden. Auf diesen Header kann auch über die [SDKs](documentdb-sdk-dotnet.md) zugegriffen werden. Im .Net SDK ist „RequestCharge“ eine Eigenschaft des ResourceResponse-Objekts.

>[AZURE.NOTE] Der Grundwert von 1 Anforderungseinheit für ein Dokument mit 1 KB entspricht einem einfachen GET-Vorgang des Dokuments mit Sitzungskonsistenz.

Es gibt verschiedene Faktoren, die sich auf die Anforderungseinheiten auswirken, die für einen Vorgang in einem DocumentDB-Datenbankkonto genutzt werden. Zu diesen Faktoren zählen folgende:

- Dokumentgröße. Je größer ein Dokument, desto mehr Einheiten werden für das Lesen und Schreiben der Daten genutzt.
- Anzahl der Eigenschaften. Eine standardmäßige Indizierung aller Eigenschaften vorausgesetzt, werden mehr Einheiten für das Schreiben eines Dokuments genutzt, wenn die Anzahl der Eigenschaften steigt.
- Datenkonsistenz. Bei Verwendung der Datenkonsistenzebenen "Strong" oder "Bounded Staleness" werden zusätzliche Einheiten zum Lesen von Dokumenten genutzt.
- Indizierte Eigenschaften. Eine Indexrichtlinie für jede Sammlung gibt an, welche Eigenschaften standardmäßig indiziert werden. Sie können die Nutzung von Anforderungseinheiten reduzieren, indem Sie die Anzahl der indizierten Eigenschaften begrenzen. 
- Dokumentindizierung. Standardmäßig wird jedes Dokument automatisch indiziert. Sie nutzen weniger Anforderungseinheiten, wenn Sie einige Dokumente nicht indizieren.

Weitere Informationen finden Sie unter [DocumentDB-Anforderungseinheiten](documentdb-request-units.md).

In der Tabelle unten ist beispielsweise angegeben, wie viele Anforderungseinheiten bei drei unterschiedlichen Dokumentgrößen (1 KB, 4 KB und 64 KB) und bei zwei unterschiedlichen Leistungsebenen (500 Lesevorgänge/Sekunde + 100 Schreibvorgänge/Sekunde und 500 Lesevorgänge/Sekunde + 500 Schreibvorgänge/Sekunde) bereitgestellt werden sollten. Für die Datenkonsistenz wurde „Session“ konfiguriert, und die Indizierungsrichtlinie wurde auf „None“ festgelegt.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Dokumentgröße</strong></p></td>
            <td valign="top"><p><strong>Lesevorgänge/Sekunde</strong></p></td>
            <td valign="top"><p><strong>Schreibvorgänge/Sekunde</strong></p></td>
            <td valign="top"><p><strong>Anforderungseinheiten</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1) + (100 * 5) = 1.000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 5) + (100 * 5) = 3.000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1,3) + (100 * 7) = 1.350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1,3) + (500 * 7) = 4.150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 10) + (100 * 48) = 9.800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 10) + (500 * 48) = 29.000 RU/s</p></td>
        </tr>
    </tbody>
</table>

Abfragen, gespeicherte Prozeduren und Trigger nutzen Anforderungseinheiten basierend auf der Komplexität des ausgeführten Vorgangs. Untersuchen Sie während der Entwicklung Ihrer Anwendung den "x-ms-request-charge"-Header, um herauszufinden, wie viel Anforderungseinheiten die einzelnen Vorgänge verbrauchen.


## Auswahl der Konsistenzebene und Durchsatz
Die Auswahl der standardmäßigen Konsistenzebene hat Auswirkungen auf den Durchsatz und die Latenz. Sie können die standardmäßige Konsistenzebene sowohl programmgesteuert als auch über das Azure-Portal festlegen. Sie können die Konsistenzebene auch pro Anforderung überschreiben. Standardmäßig ist die Konsistenzebene auf "Session" festgelegt, wodurch monotone Lese-/Schreibvorgänge und Read-Your-Write-Garantie ermöglicht wird. Die Sitzungskonsistenz ist ideal für benutzerzentrierte Anwendungen geeignet und liefert einen optimalen Kompromiss zwischen Konsistenz und Leistung.

Anleitungen zum Ändern Ihrer Konsistenzebene im Azure-Portal finden Sie unter [Verwalten eines DocumentDB-Kontos](documentdb-manage-account.md#consistency). Weitere Informationen zu Konsistenzebenen finden Sie unter [Verwenden von Konsistenzebenen](documentdb-consistency-levels.md).

## Bereitgestellter Dokumentspeicher und Index-Overhead
DocumentDB unterstützt die Erstellung von einzelnen Partitionen und partitionierten Sammlungen. Für jede Partition in DocumentDB werden bis zu 10 GB an SSD-gestütztem Speicher unterstützt. Die 10 GB Dokumentspeicher umfassen die Dokumente plus den Speicher für den Index. Standardmäßig wird eine DocumentDB-Auflistung so konfiguriert, dass alle Dokumente automatisch indiziert werden, ohne dass explizit irgendwelche sekundären Indizes oder Schemas erforderlich wären. Basierend auf den Anwendungen, die DocumentDB nutzen, liegt der Index-Overhead normalerweise zwischen 2 und 20 %. Durch die von DocumentDB verwendete Indizierungstechnologie wird sichergestellt, dass der Index-Overhead, unabhängig von den Werten der Eigenschaften, bei den Standardeinstellungen 80 % der Größe der Dokumente nicht überschreitet.

Standardmäßig werden alle Dokumente von DocumentDB automatisch indiziert. Wenn Sie den Indizierungsaufwand jedoch optimieren möchten, können Sie beim Einfügen oder Ersetzen eines Dokuments bestimmte Dokumente aus der Indizierung ausschließen (siehe [Indizierungsrichtlinien für DocumentDB](documentdb-indexing-policies.md)). Sie können eine DocumentDB-Auflistung so konfigurieren, dass alle Dokumente innerhalb der Auflistung von der Indizierung ausgeschlossen werden. Sie können eine DocumentDB-Sammlung auch so konfigurieren, dass nur bestimmte Eigenschaften oder Pfade mit Platzhaltern in Ihren JSON-Dokumenten selektiv indiziert werden (siehe [Konfigurieren der Indizierungsrichtlinie einer Sammlung](documentdb-indexing-policies.md#configuring-the-indexing-policy-of-a-collection)). Durch das Ausschließen von Eigenschaften oder Dokumenten wird auch der Schreibdurchsatz verbessert, was bedeutet, dass Sie weniger Anforderungseinheiten verbrauchen.
## Nächste Schritte
Anweisungen zur Überwachung der Leistungsebenen im Azure-Portal finden Sie unter [Überwachen eines DocumentDB-Kontos](documentdb-monitor-accounts.md).

Weitere Informationen zum Auswählen von Leistungsebenen für Sammlungen finden Sie unter [Leistungsebenen in DocumentDB](documentdb-performance-levels.md).
 

<!---HONumber=AcomDC_0330_2016-->