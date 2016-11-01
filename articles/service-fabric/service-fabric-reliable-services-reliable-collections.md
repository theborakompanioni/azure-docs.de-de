<properties
   pageTitle="Reliable Collections | Microsoft Azure"
   description="Zustandsbehaftete Service Fabric-Dienste bieten zuverlässige Auflistungen, die Ihnen das Schreiben hochverfügbarer, skalierbarer Cloudanwendungen mit kurzer Latenz ermöglichen."
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/28/2016"
   ms.author="mcoskun"/>

# Einführung in Reliable Collections in zustandsbehafteten Azure Service Fabric-Diensten

Reliable Collections ermöglichen es Ihnen, hoch verfügbare, skalierbare Cloudanwendungen mit geringer Latenz auf die gleiche Weise wie Anwendungen für einen einzelnen Computer zu schreiben. Die Klassen im Namespace **Microsoft.ServiceFabric.Data.Collections** bieten eine Reihe sofort verwendbarer Auflistungen, die automatisch einen hochverfügbaren Zustand ermöglichen. Als Entwickler programmieren Sie lediglich die Reliable Collections-APIs. Reliable Collections verwalten anschließend den replizierten lokalen Zustand automatisch.

Der Hauptunterschied zwischen zuverlässigen Auflistungen und anderen Hochverfügbarkeitstechnologien (z. B. Redis, Azure-Tabellendienst und Azure-Warteschlangendienst) ist, dass der Zustand lokal in der Dienstinstanz gespeichert wird und gleichzeitig hoch verfügbar ist. Dies bedeutet Folgendes:

- Alle Lesevorgänge erfolgen lokal, was eine geringe Latenz und einen hohen Lesedurchsatz zur Folge hat.
- Alle Schreibvorgänge lösen die Mindestanzahl von Netzwerk-E/As aus, was eine geringe Latenz und einen hohen Schreibdurchsatz zur Folge hat.

![Abbildung der Weiterentwicklung von Auflistungen.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Reliable Collections können als natürliche Weiterentwicklung der **System.Collections**-Klassen betrachtet werden: Der neue Satz von Auflistungen wurde für Cloudanwendungen und Umgebungen mit mehreren Computern konzipiert, ohne die Komplexität für den Entwickler zu erhöhen. Reliable Collections sind damit:

- Repliziert: Zustandsänderungen werden für hohe Verfügbarkeit repliziert.
- Persistent gespeichert: Daten werden persistent auf Datenträgern gespeichert, um sie vor größeren Ausfällen (z. B. einem Stromausfall im Rechenzentrum) zu schützen.
- Asynchron: APIs sind asynchron, um sicherzustellen, dass Threads bei einer E/A nicht blockiert werden.
- Transaktional: APIs nutzen die Abstraktion von Transaktionen, damit Sie mehrere Reliable Collections auf einfache Weise in einem Dienst verwalten können.

Zuverlässige Auflistungen zeichnen sich durch von Beginn an starke Konsistenzgarantien aus, was die Argumentation hinsichtlich Anwendungszuständen erleichtert. Eine starke Konsistenz wird erreicht, indem Transaktionscommits erst abgeschlossen werden, nachdem die gesamte Transaktion in einem Mehrheitsquorum von Replikaten (einschließlich des primären Replikats) protokolliert wurde. Um eine schwächere Konsistenz zu erreichen, können Anwendungen eine Bestätigung zurück an den Client/Antragsteller senden, bevor der asynchrone Commit zurückgegeben wird.

Die Reliable Collections-APIs sind eine Weiterentwicklung der APIs für gleichzeitige Auflistungen (im Namespace **System.Collections.Concurrent**):

- Asynchron: Gibt eine Aufgabe zurück, da die Vorgänge im Gegensatz zu gleichzeitigen Auflistungen repliziert und persistent gespeichert werden.
- Keine out-Parameter: Gibt mithilfe von `ConditionalValue<T>` anstelle von out-Parametern "Bool" und einen Wert zurück. `ConditionalValue<T>` entspricht `Nullable<T>`, erfordert aber für "struct" nicht "T".
- Transaktionen: Verwendet ein Transaktionsobjekt, um dem Benutzer Gruppenaktionen für mehrere zuverlässige Auflistungen in einer Transaktion zu ermöglichen.

Aktuell enthält **Microsoft.ServiceFabric.Data.Collections** zwei Auflistungen:

- [Zuverlässiges Wörterbuch](https://msdn.microsoft.com/library/azure/dn971511.aspx): stellt eine replizierte, transaktionale und asynchrone Auflistung von Schlüsselwertpaaren dar. Ähnlich wie bei **ConcurrentDictionary** können der Schlüssel und der Wert von beliebigem Typ sein.
- [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx): Stellt eine replizierte, transaktionale und asynchrone strenge FIFO (First In, First Out)-Warteschlange dar. Ähnlich wie bei **ConcurrentQueue** kann der Wert von beliebigem Typ sein.

## Isolationsgrade
Isolationsstufen definiert den Grad, zu dem eine Transaktion von Änderungen isoliert werden muss, die von anderen Transaktionen ausgeführt werden. Es gibt zwei Isolationsstufen, die von zuverlässigen Auflistungen unterstützt werden:

- **Wiederholbarer Lesevorgang**: Gibt an, dass Anweisungen keine Daten lesen können, die geändert wurden, für die aber von anderen Transaktionen noch kein Commit ausgeführt wurde. Darüber hinaus können von der aktuellen Transaktion gelesene Daten erst nach Abschluss dieser Transaktion von anderen Transaktionen geändert werden. Weitere Informationen finden Sie unter [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).
- **Momentaufnahme**: Gibt an, dass von Anweisungen in einer Transaktion gelesene Daten der im Hinblick auf Transaktionen konsistenten Version der Daten entsprechen, die zu Beginn der Transaktion vorhanden waren. Die Transaktion kann nur Datenänderungen erkennen, die vor dem Start der Transaktion festgeschrieben wurden. Nach dem Start der aktuellen Transaktion von anderen Transaktionen vorgenommene Datenänderungen sind für Anweisungen, die in der aktuellen Transaktion ausgeführt werden, nicht sichtbar. Es erscheint daher, als ob die Anweisungen in einer Transaktion eine Momentaufnahme der festgeschriebenen Daten erhalten, die zu Beginn der Transaktion vorhanden waren. Momentaufnahmen sind über zuverlässige Sammlungen hinweg konsistent. Weitere Informationen finden Sie unter [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).

Reliable Collections wählen die Isolationsstufe für einen bestimmten Lesevorgang automatisch je nach Vorgang und nach Rolle des Replikats zum Zeitpunkt der Erstellung der Transaktion. Im Folgenden finden Sie eine Tabelle, die Standardwerte für Isolationsstufen für Reliable Dictionary- und Reliable Queue-Vorgänge veranschaulicht.

| Vorgang\\Rolle | Primär | Sekundär |
| --------------------- | :--------------- | :--------------- |
| Lesevorgang für eine einzelne Entität | Wiederholbarer Lesevorgang | Momentaufnahme |
| Aufzählung\\Anzahl | Momentaufnahme | Momentaufnahme |

>[AZURE.NOTE] Typische Beispiele für Vorgänge mit einer einzigen Entität sind `IReliableDictionary.TryGetValueAsync` und `IReliableQueue.TryPeekAsync`.

Das zuverlässige Wörterbuch und die zuverlässige Warteschlange unterstützen beide "Read Your Writes". Mit anderen Worten sind jegliche Schreibvorgänge innerhalb einer Transaktion für den nachfolgenden Lesevorgang sichtbar, wenn dieser derselben Transaktion angehört.

## Sperren
In Reliable Collections bestehen alle Transaktionen aus zwei Phasen: Die von einer Transaktion angeforderten Sperren werden erst aufgehoben, wenn die Transaktion durch einen Abbruch oder einen Commit beendet wird.

Reliable Dictionary verwendet Sperren auf Zeilenebene für alle Vorgänge mit nur einer Entität. Reliable Queue wägt Parallelität gegen die strikte transaktionsbezogene FIFO-Eigenschaft ab. Reliable Queue verwendet Sperren auf Vorgangsebene und lässt gleichzeitig eine Transaktion mit `TryPeekAsync` und/oder `TryDequeueAsync` und eine Transaktion mit `EnqueueAsync` zu. Beachten Sie Folgendes: Wenn ein `TryPeekAsync`- oder ein `TryDequeueAsync`-Vorgang feststellen, dass die Reliable Queue leer ist, sperren sie auch `EnqueueAsync`, um das FIFO-Prinzip beizubehalten.

Schreibvorgänge verwenden immer exklusive Sperren. Bei Lesevorgängen richtet sich die Sperrung nach verschiedenen Faktoren. Jegliche Lesevorgänge, die mithilfe der Momentaufnahmeisolierung ausgeführt wurden, sind frei von Sperren. Bei allen wiederholbaren Lesevorgängen werden standardmäßig freigegebene Sperren angewendet. Bei Lesevorgängen, die wiederholbare Lesevorgänge unterstützen, können Benutzer anstelle der freigegebenen Sperre eine Aktualisierungssperre anfordern. Eine Aktualisierungssperre ist eine asymmetrische Sperre, mit der eine häufig auftretende Form von Deadlock verhindert wird. Der Deadlock tritt auf, wenn mehrere Transaktionen Ressourcen für potenzielle Updates zu einem späteren Zeitpunkt sperren.

Die Kompatibilitätsmatrix für Sperren finden Sie unten:

| Anforderung\\Gewährt | Keine | Shared | Aktualisieren | Exklusiv |
| ----------------- | :----------- | :----------- | :---------- | :----------- |
| Shared | Kein Konflikt | Kein Konflikt | Konflikt: | Konflikt: |
| Aktualisieren | Kein Konflikt | Kein Konflikt | Konflikt: | Konflikt: |
| Exklusiv | Kein Konflikt | Konflikt: | Konflikt: | Konflikt: |

Hinweis: Zum Erkennen von Deadlocks werden in den Reliable Collections-APIs Timeoutargumente verwendet. Angenommen, zwei Transaktionen (T1 und T2) versuchen, K1 zu lesen und zu aktualisieren. Beide können ein Deadlock durchführen, da im Endeffekt beide die freigegebene Sperre haben. In diesem Fall tritt bei einem oder beiden Vorgängen ein Timeout auf.

Das vorangegangene Deadlockszenario ist ein hervorragendes Beispiel, wie Aktualisierungssperren Deadlocks verhindern können.

## Persistenzmodell
Reliable State Manager und Reliable Collections basieren auf einem Persistenzmodell, das als Protokoll und Prüfpunkt bezeichnet wird. Bei diesem Modell wird jede Zustandsänderung auf dem Datenträger protokolliert und nur im Arbeitsspeicher angewendet. Der vollständige Zustand selbst wird nur gelegentlich persistent gespeichert (auch als Prüfpunkt bezeichnet). Der Vorteil ist: Deltas werden zur Leistungsverbesserung in sequenzielle Nur-Anfügen-Schreibvorgänge auf dem Datenträger umgewandelt.

Zum besseren Verständnis des Protokoll- und Prüfpunktmodells sehen wir uns zunächst das Szenario des Endlosdatenträgers an. Reliable State Manager protokolliert jeden Vorgang, bevor dieser repliziert wird. Dies ermöglicht es Reliable Collections, den Vorgang nur im Arbeitsspeicher anzuwenden. Da Protokolle persistent sind, verfügt der zuverlässige Zustands-Manager selbst dann, wenn das Replikat aufgrund eines Fehlers neu gestartet werden muss, über ausreichend Informationen in seinen Protokollen, um alle im Replikat verloren gegangenen Vorgänge zu wiederholen. Da es sich um einen Endlosdatenträger handelt, müssen Protokolleinträge nie entfernt werden. Reliable Collections verwalten lediglich den Zustand im Speicher.

Sehen wir uns jetzt das Szenario mit begrenzten Datenträgern an. Da sich Protokolldatensätze akkumulieren, ist der gesamte Speicherplatz auf dem Datenträger von Reliable State Manager irgendwann belegt. Bevor dies geschieht, muss Reliable State Manager sein Protokoll kürzen, um Platz für neuere Datensätze zu schaffen. Er fordert die Reliable Collections auf, ihren Zustand im Arbeitsspeicher auf dem Datenträger zu prüfen. Die Reliable Collections sind dafür zuständig, den Zustand bis zu diesem Punkt persistent zu speichern. Sobald die zuverlässigen Auflistungen ihre Prüfpunkte abgeschlossen haben, kann der zuverlässige Zustands-Manager das Protokoll kürzen, um Speicherplatz freizugeben. Wenn das Replikat neu gestartet werden muss, stellen zuverlässige Auflistungen den Zustand ihres Prüfpunkts wieder her. Zudem stellt der zuverlässige Zustands-Manager alle seit dem Prüfpunkt vorgenommenen Zustandsänderungen wieder her.

>[AZURE.NOTE] Ein weiterer Vorteil der Verwendung von Prüfpunkten ist die Verbesserung der Wiederherstellungsleistung im Normalfall. Dies liegt daran, dass Prüfpunkte nur die neuesten Versionen enthalten.

## Recommendations

- Ändern Sie kein benutzerdefiniertes Objekt, das von Lesevorgängen (z.B. `TryPeekAsync` oder `TryGetValueAsync`) zurückgegeben wurde. Zuverlässige Auflistungen geben ebenso wie gleichzeitige Auflistungen anstelle einer Kopie einen Verweis auf die Objekte zurück.
- Tiefenkopieren Sie zurückgegebene benutzerdefinierte Objekte, bevor Sie diese ändern. Da bei Strukturen und integrierten Typen eine Wertübergabe erfolgt, ist hier keine Tiefenkopie erforderlich.
- Verwenden Sie `TimeSpan.MaxValue` nicht für Timeouts. Timeouts sollten verwendet werden, um Deadlocks zu erkennen.
- Verwenden Sie keine Transaktion, nachdem für sie ein Commit ausgeführt bzw. sie verworfen oder abgebrochen wurde.
- Verwenden Sie eine Enumeration nicht außerhalb des Transaktionsbereichs, in dem sie erstellt wurde.
- Erstellen Sie keine Transaktion innerhalb der `using`-Anweisung einer anderen Transaktion, da dies zu Deadlocks führen kann.
- Stellen Sie sicher, dass Ihre `IComparable<TKey>`-Implementierung richtig ist. Dies ist erforderlich, damit das System Prüfpunkte zusammenfügen kann.
- Verwenden Sie Aktualisierungssperren beim Lesen eines Elements, das aktualisiert werden soll, um eine bestimmte Klasse von Deadlocks zu vermeiden.
- Sie sollten zwecks Notfallwiederherstellung die Verwendung der Funktionen „Backup“ und „Wiederherstellung“ in Betracht ziehen.
- Verwenden Sie Vorgänge mit einer einzigen Entität und Vorgänge mit mehreren Entitäten (z.B. `GetCountAsync` und `CreateEnumerableAsync`) aufgrund der unterschiedlichen Isolationsstufen nicht in der gleichen Transaktion.

Hier folgen einige Punkte, die es zu beachten gilt:

- Das Standardtimeout beträgt 4 Sekunden für alle Reliable Collections-APIs. Die meisten Benutzer sollten diesen Wert nicht überschreiben.
- Das Standardabbruchtoken ist `CancellationToken.None` in allen APIs für zuverlässige Auflistungen.
- Der Schlüsseltyp-Parameter (*TKey*) für ein zuverlässiges Wörterbuch muss `GetHashCode()` und `Equals()` ordnungsgemäß implementieren. Schlüssel müssen unveränderlich sein.
- Zum Erreichen einer hohen Verfügbarkeit der zuverlässigen Auflistungen sollte jeder Dienst mindestens ein Ziel und eine Mindestgröße von 3 bei der Replikatgruppe haben.
- Lesevorgänge auf dem sekundären Replikat dürfen Versionen lesen, die nicht im Quorum committet wurden. Dies bedeutet, dass Datenversionen, die von einem einzelnen sekundären Replikat gelesen werden, falsch weiterverarbeitet werden können. Da Lesevorgänge von primären Replikaten immer stabil sind, können hier nie fehlerhafte Versionen auftreten.

## Nächste Schritte

- [Reliable Services – Schnellstart](service-fabric-reliable-services-quick-start.md)
- [Arbeiten mit Reliable Collections](service-fabric-work-with-reliable-collections.md)
- [Reliable Services – Benachrichtigungen](service-fabric-reliable-services-notifications.md)
- [Sichern und Wiederherstellen von Reliable Services (Notfallwiederherstellung)](service-fabric-reliable-services-backup-restore.md)
- [Konfigurieren des Reliable State Managers](service-fabric-reliable-services-configuration.md)
- [Erste Schritte mit Web-API-Diensten von Service Fabric](service-fabric-reliable-services-communication-webapi.md)
- [Erweiterte Verwendung des Reliable Services-Programmiermodells](service-fabric-reliable-services-advanced-usage.md)
- [Entwicklerreferenz für zuverlässige Auflistungen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

<!---HONumber=AcomDC_0803_2016-->