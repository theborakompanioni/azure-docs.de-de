---
redirect_url: https://azure.microsoft.com/services/documentdb/
ROBOTS: NOINDEX, NOFOLLOW
translationtype: Human Translation
ms.sourcegitcommit: d59ebef3cda36ba048b844f0cd2326fff66b4aa5
ms.openlocfilehash: 27880401ccfca4c2c504eecc1aeb128d40c501a3


---
# <a name="storage-and-predictable-performance-provisioning-in-documentdb"></a>Speicherung und vorhersagbare Leistungsbereitstellung in DocumentDB
Azure DocumentDB ist ein vollständig verwalteter, skalierbarer dokumentorientierter NoSQL-Datenbankdienst für JSON-Dokumente. Mit DocumentDB müssen Sie keine virtuellen Computer mieten, Software bereitstellen oder Datenbanken überwachen. DocumentDB wird von Microsoft-Entwicklern betrieben und ständig überwacht, um erstklassige Verfügbarkeit, Leistung und Datensicherheit zu gewährleisten.  

Machen Sie sich mit DocumentDB durch [Erstellen eines Datenbankkontos](documentdb-create-account.md) und anschließender Erstellung einer [DocumentDB-Sammlung und -Datenbank](documentdb-create-collection.md) über das [Azure-Portal](https://portal.azure.com/) vertraut. DocumentDB-Datenbanken werden in Einheiten aus SSD-gestütztem Speicher und Durchsatz angeboten. Diese Speichereinheiten werden durch Erstellen von Datenbanksammlungen bereitgestellt. Dabei gilt für jede Sammlung ein reservierter Durchsatz, der jederzeit zentral hoch- oder herunterskaliert werden kann, damit die Anforderungen Ihrer Anwendung erfüllt sind.

Wenn Ihre Anwendung den reservierten Durchsatz für eine oder mehrere Sammlungen überschreitet, werden die Anforderungen pro Sammlung beschränkt. Dies bedeutet, dass einige Anforderungen erfolgreich sind, während andere aufgrund der Beschränkung fehlschlagen.

Dieser Artikel bietet eine Übersicht über die Ressourcen und Metriken zum Verwalten der Kapazität und Planen der Datenspeicherung.

## <a name="database-account"></a>Datenbankkonto
Als Azure-Abonnent können Sie ein oder mehrere DocumentDB-Datenbankkonten zur Verwaltung Ihrer Datenbankressourcen bereitstellen. Jedes Abonnement ist einem einzelnen Azure-Abonnement zugeordnet.

DocumentDB-Konten können über das [Azure-Portal](documentdb-create-account.md) oder mithilfe [einer ARM-Vorlage oder der Azure-CLI](documentdb-automation-resource-manager-cli.md) erstellt werden.

## <a name="databases"></a>Datenbanken
Eine einzelne DocumentDB-Datenbank kann eine nahezu unbegrenzte Menge an Dokumentspeicher enthalten, der anhand von Sammlungen gruppiert wird. Sammlungen ermöglichen eine Leistungsisolierung. Jede Sammlung kann mit einem Durchsatz bereitgestellt werden, der für andere Sammlungen in derselben Datenbank oder unter demselben Konto nicht freigegeben ist. Die Größe einer DocumentDB-Datenbank ist flexibel und reicht vom GB-Bereich bis hin zu SSD-gestütztem Dokumentspeicher im TB-Bereich mit bereitgestelltem Durchsatz. Im Gegensatz zu einer herkömmlichen RDBMS-Datenbank gilt der Bereich einer Datenbank in DocumentDB nicht nur für einen einzelnen Computer und kann mehrere Computer oder Cluster umfassen.  

Mit DocumentDB können Sie weitere Sammlungen oder Datenbanken bzw. beides erstellen, wenn Sie Ihre Anwendungen skalieren müssen. Datenbanken können über das [Azure-Portal](documentdb-create-database.md) oder eines der [DocumentDB-SDKs](documentdb-dotnet-samples.md) erstellt werden.   

## <a name="database-collections"></a>Datenbanksammlungen
Jede DocumentDB-Datenbank kann eine oder mehrere Auflistungen enthalten. Sammlungen dienen als hoch verfügbare Datenpartitionen für die Speicherung und Verarbeitung von Dokumenten. Jede Sammlung kann Dokumente mit heterogenem Schema speichern. Die automatischen Indizierungs- und Abfragefunktionen von DocumentDB ermöglichen Ihnen das einfache Filtern und Abrufen von Dokumenten. Eine Auflistung liefert den Umfang für Dokumentspeicher und Abfrageausführung. Eine Auflistung ist auch eine Transaktionsdomäne für ale Dokumente, die darin enthalten sind. Sammlungen werden dem Durchsatz ausgehend von dem im Azure-Portal oder über die SDKs festgelegten Wert zugeordnet.

Sammlungen werden von DocumentDB automatisch in einen oder mehrere physische Server partitioniert. Wenn Sie eine Sammlung erstellen, können Sie den bereitgestellten Durchsatz in Form von Anforderungseinheiten pro Sekunde und einer Partitionsschlüsseleigenschaft angeben. Der Wert dieser Eigenschaft wird von DocumentDB zum Verteilen von Dokumenten zwischen Partitionen und zum Weiterleiten von Anforderungen wie Abfragen verwendet. Der Partitionsschlüssel fungiert außerdem als die Transaktionsgrenze für gespeicherte Prozeduren und Trigger. Jede Sammlung hat eine reservierte Durchsatzmenge, die für diese Sammlung spezifisch ist und die nicht mit anderen Sammlungen im gleichen Konto gemeinsam genutzt wird. Daher können Sie Ihre Anwendung sowohl im Hinblick auf Speicher als auch Durchsatz horizontal hochskalieren.

Sammlungen können über das [Azure-Portal](documentdb-create-collection.md) oder eines der [DocumentDB-SDKs](documentdb-sdk-dotnet.md) erstellt werden.   

## <a name="request-units-and-database-operations"></a>Anforderungseinheiten und Datenbankvorgänge
Wenn Sie eine Sammlung erstellen, reservieren Sie Durchsatz hinsichtlich der [Anforderungseinheiten (request units, RUs)](documentdb-request-units.md) pro Sekunde. Anstatt sich Gedanken über Hardwareressourcen und deren Verwaltung zu machen, können Sie sich eine **Anforderungseinheit** als alleinige Maßeinheit für die Ressourcen vorstellen, die für das Durchführen der verschiedenen Datenbankvorgänge und das Ausführen einer Anwendungsanforderung erforderlich sind. Beim Lesen eines Dokuments mit 1 KB wird immer 1 RU verbraucht. Dies gilt unabhängig davon, wie viele Elemente in der Sammlung gespeichert sind oder wie viele gleichzeitige Anforderungen darin ausgeführt werden. Alle Anforderungen für DocumentDB, wie z.B. komplexe Vorgänge wie SQL-Abfragen, verfügen über einen vorhersagbaren RU-Wert, der zur Entwicklungszeit bestimmt werden kann. Wenn Sie die Größe der Dokumente und die Häufigkeit der einzelnen Vorgänge (Lese-, Schreib- und Abfragevorgänge) kennen, die für Ihre Anwendung unterstützt werden müssen, können Sie die genaue Durchsatzmenge bereitstellen, mit der die Anforderungen Ihrer Anwendung erfüllt werden. Sie können die Datenbank horizontal hochskalieren und zentral herunterskalieren, wenn sich die Leistungsanforderungen ändern.

Für jede Sammlung kann ein Durchsatz in Blöcken von 100 Anforderungseinheiten pro Sekunde reserviert werden. Diese Reservierung kann von Hunderten bis zu Millionen von Anforderungseinheiten pro Sekunde reichen. Der bereitgestellte Durchsatz kann während der gesamten Lebensdauer einer Sammlung angepasst werden, um sie auf die veränderten Verarbeitungsanforderungen und Zugriffsmuster der Anwendung abzustimmen. Weitere Informationen finden Sie unter [Leistungsebenen in DocumentDB](documentdb-performance-levels.md).

> [!IMPORTANT]
> Sammlungen sind fakturierbare Entitäten. Die Kosten werden anhand des bereitgestellten Durchsatzes der Sammlung in Anforderungseinheiten pro Sekunde zusammen mit dem insgesamt verbrauchten Speicher in GB ermittelt.
>
>

Wie viele Anforderungseinheiten werden bei einem bestimmten Vorgang wie dem Einfügen, Löschen, Abfragen oder dem Ausführen einer gespeicherten Prozedur verbraucht? Eine Anforderungseinheit ist eine normalisierte Kennzahl für die Anforderungsverarbeitungskosten. Ein Lesevorgang eines Dokuments mit 1 KB entspricht 1 RU, aber eine Anforderung zum Einfügen, Ersetzen oder Löschen des gleichen Dokuments verbraucht mehr Verarbeitungsleistung des Diensts und daher mehr Anforderungseinheiten. Jede Antwort des Diensts enthält einen benutzerdefinierten Header (`x-ms-request-charge`), mit dem die für die Anforderung verbrauchten Anforderungseinheiten gemeldet werden. Auf diesen Header kann auch über die [SDKs](documentdb-sdk-dotnet.md)zugegriffen werden. Im .NET SDK ist [RequestCharge](https://msdn.microsoft.com/library/azure/dn933057.aspx#P:Microsoft.Azure.Documents.Client.ResourceResponse`1.RequestCharge) eine Eigenschaft des [ResourceResponse](https://msdn.microsoft.com/library/azure/dn799209.aspx)-Objekts. Wenn Sie Ihre Anforderungen an den Durchsatz vor einem einzelnen Aufruf schätzen möchten, ist dies mithilfe des [Capacity Planner](documentdb-request-units.md#estimating-throughput-needs) möglich.

> [!NOTE]
> Der Grundwert von 1 Anforderungseinheit für ein Dokument mit 1 KB entspricht einem einfachen GET-Vorgang des Dokuments mit [Sitzungskonsistenz](documentdb-consistency-levels.md).
>
>

Es gibt verschiedene Faktoren, die sich auf die Anforderungseinheiten auswirken, die für einen Vorgang in einem DocumentDB-Datenbankkonto genutzt werden. Zu diesen Faktoren zählen folgende:

* Dokumentgröße. Je größer ein Dokument, desto mehr Einheiten werden für das Lesen und Schreiben der Daten genutzt.
* Anzahl der Eigenschaften. Eine standardmäßige Indizierung aller Eigenschaften vorausgesetzt, werden mehr Einheiten für das Schreiben eines Dokuments genutzt, wenn die Anzahl der Eigenschaften steigt.
* Datenkonsistenz. Bei Verwendung der Datenkonsistenzebenen "Strong" oder "Bounded Staleness" werden zusätzliche Einheiten zum Lesen von Dokumenten genutzt.
* Indizierte Eigenschaften. Eine Indexrichtlinie für jede Sammlung gibt an, welche Eigenschaften standardmäßig indiziert werden. Sie können die Nutzung von Anforderungseinheiten reduzieren, indem Sie die Anzahl der indizierten Eigenschaften begrenzen.
* Dokumentindizierung. Standardmäßig wird jedes Dokument automatisch indiziert. Sie nutzen weniger Anforderungseinheiten, wenn Sie einige Dokumente nicht indizieren.

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
            <td valign="top"><p>(500 * 1.3) + (100 * 7) = 1.350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1.3) + (500 * 7) = 4.150 RU/s</p></td>
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

## <a name="choice-of-consistency-level-and-throughput"></a>Auswahl der Konsistenzebene und Durchsatz
Die Auswahl der standardmäßigen Konsistenzebene hat Auswirkungen auf den Durchsatz und die Latenz. Sie können die standardmäßige Konsistenzebene sowohl programmgesteuert als auch über das Azure-Portal festlegen. Sie können die Konsistenzebene auch pro Anforderung überschreiben. Standardmäßig ist die Konsistenzebene auf **Session**festgelegt, wodurch monotone Lese-/Schreibvorgänge und Read-Your-Write-Garantie ermöglicht wird. Die Sitzungskonsistenz ist ideal für benutzerzentrierte Anwendungen geeignet und liefert einen optimalen Kompromiss zwischen Konsistenz und Leistung.    

Anleitungen zum Ändern Ihrer Konsistenzebene im Azure-Portal finden Sie unter [Verwalten eines DocumentDB-Kontos](documentdb-manage-account.md#consistency). Weitere Informationen zu Konsistenzebenen finden Sie unter [Verwenden von Konsistenzebenen](documentdb-consistency-levels.md).

## <a name="provisioned-document-storage-and-index-overhead"></a>Bereitgestellter Dokumentspeicher und Index-Overhead
DocumentDB unterstützt die Erstellung von einzelnen Partitionen und partitionierten Sammlungen. Für jede Partition in DocumentDB werden bis zu 10 GB an SSD-gestütztem Speicher unterstützt. Die 10 GB Dokumentspeicher umfassen die Dokumente plus den Speicher für den Index. Standardmäßig wird eine DocumentDB-Auflistung so konfiguriert, dass alle Dokumente automatisch indiziert werden, ohne dass explizit irgendwelche sekundären Indizes oder Schemas erforderlich wären. Basierend auf den Anwendungen, die DocumentDB nutzen, liegt der Index-Overhead normalerweise zwischen 2 und 20 %. Durch die von DocumentDB verwendete Indizierungstechnologie wird sichergestellt, dass der Index-Overhead, unabhängig von den Werten der Eigenschaften, bei den Standardeinstellungen 80 % der Größe der Dokumente nicht überschreitet.

Standardmäßig werden alle Dokumente von DocumentDB automatisch indiziert. Wenn Sie den Indizierungsaufwand jedoch optimieren möchten, können Sie beim Einfügen oder Ersetzen eines Dokuments bestimmte Dokumente aus der Indizierung ausschließen (siehe [Indizierungsrichtlinien für DocumentDB](documentdb-indexing-policies.md)). Sie können eine DocumentDB-Auflistung so konfigurieren, dass alle Dokumente innerhalb der Auflistung von der Indizierung ausgeschlossen werden. Sie können eine DocumentDB-Sammlung auch so konfigurieren, dass nur bestimmte Eigenschaften oder Pfade mit Platzhaltern in Ihren JSON-Dokumenten selektiv indiziert werden (siehe [Konfigurieren der Indizierungsrichtlinie einer Sammlung](documentdb-indexing-policies.md#CustomizingIndexingPolicy)). Durch das Ausschließen von Eigenschaften oder Dokumenten wird auch der Schreibdurchsatz verbessert, was bedeutet, dass Sie weniger Anforderungseinheiten verbrauchen.   

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu DocumentDB finden Sie unter [Partitionieren und Skalieren von Daten in DocumentDB](documentdb-partition-data.md).

Anweisungen zur Überwachung der Leistungsebenen im Azure-Portal finden Sie unter [Überwachen eines DocumentDB-Kontos](documentdb-monitor-accounts.md). Weitere Informationen zum Auswählen von Leistungsebenen für Sammlungen finden Sie unter [Leistungsebenen in DocumentDB](documentdb-performance-levels.md).



<!--HONumber=Jan17_HO3-->


