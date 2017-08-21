---
title: "Entwerfen von hochverfügbaren Anwendungen mit georedundantem Azure Storage mit Lesezugriff (RA-GRS) | Microsoft Docs"
description: "Verwenden Sie Azure-RA-GRS-Speicher, um eine hochverfügbare Anwendung so flexibel zu gestalten, dass sie Ausfälle verarbeiten kann."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 8f040b0f-8926-4831-ac07-79f646f31926
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 1/19/2017
ms.author: robinsh
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: adc7e23d8c9f869f2951490020e3d0f1a2b2e81c
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---
# <a name="designing-highly-available-applications-using-ra-grs"></a>Entwerfen hochverfügbarer Anwendungen mithilfe von RA-GRS

Ein Feature von cloudbasierten Infrastrukturen ist, dass sie eine hochverfügbare Plattform zum Hosten von Anwendungen bereitstellen. Entwickler von cloudbasierten Anwendungen müssen genau überlegen, wie sie diese Plattform zum Bereitstellen hochverfügbarer Anwendungen für ihre Benutzer verwenden. Dieser Artikel konzentriert sich insbesondere darauf, wie Entwickler georedundanten Azure Storage mit Lesezugriff (RA-GRS) verwenden können, um die Verfügbarkeit ihrer Anwendungen zu erhöhen.

Es gibt vier Redundanzoptionen: lokal redundanten Speicher (LRS), zonenredundanten Speicher (ZRS), georedundanten Speicher (GRS) und georedundanten Speicher mit Lesezugriff (RA-GRS). Wir befassen uns in diesem Artikel mit GRS und RA-GRS. Bei GRS werden drei Kopien Ihrer Daten in der primären Region gespeichert, die Sie beim Einrichten des Speicherkontos ausgewählt haben. Drei weitere Kopien werden asynchron in einer sekundären Region verwaltet, die von Azure festgelegt wird. RA-GRS ist abgesehen davon, dass Sie über Lesezugriff auf die sekundäre Kopie verfügen, identisch mit GRS. Weitere Informationen zu den verschiedenen Azure Storage-Redundanzoptionen finden Sie unter [Azure Storage-Replikation](https://docs.microsoft.com/en-us/azure/storage/storage-redundancy). Im Artikel über Replikation sind auch Kombinationen der primären und sekundären Regionen aufgeführt.

Der Artikel enthält Codeausschnitte sowie am Ende einen Link zu einem vollständigen Beispiel, das Sie herunterladen und ausführen können.

## <a name="key-features-of-ra-grs"></a>Wichtige Features von RA-GRS

Bevor wir erläutern, wie Sie RA-GRS-Speicher verwenden, beschäftigen wir uns mit den Eigenschaften und dem Verhalten.

* Azure Storage verwaltet eine schreibgeschützte Kopie der Daten, die Sie in Ihrer primären Region in einer sekundären Region speichern. Wie oben bereits erwähnt, bestimmt der Speicherdienst den Speicherort der sekundären Region.

* Die schreibgeschützte Kopie ist [letztendlich konsistent](https://en.wikipedia.org/wiki/Eventual_consistency) mit den Daten in der primären Region.

* Für Blobs, Tabellen und Warteschlangen können Sie den Wert für den *Zeitpunkt der letzten Synchronisierung* von der sekundären Region abfragen, der anzeigt, wann die letzte Replikation von der primären zur sekundären Region ausgeführt wurde. (Dies wird für Azure File Storage nicht unterstützt, da RA-GRS-Redundanz derzeit nicht verfügbar ist.)

* Sie können die Speicherclientbibliothek für die Interaktion mit den Daten in der primären oder sekundären Region verwenden. Sie können Anforderungen auch automatisch an die sekundäre Region leiten, wenn bei einer Leseanforderung an die primäre Region eine Zeitüberschreitung auftritt.

* Wenn ein schwerwiegendes Problem beim Zugriff auf die Daten in der primären Region vorliegt, kann das Azure-Team ein geografisches Failover auslösen. Ab diesem Zeitpunkt werden die auf die primäre Region verweisenden DNS-Einträge so geändert, dass sie auf die sekundäre Region verweisen.

* Wenn ein geografisches Failover auftritt, wählt Azure einen neuen sekundären Standort aus, repliziert die Daten an diesem Speicherort und verweist anschließend die sekundären DNS-Einträge auf diesen. Der sekundäre Endpunkt steht nicht zur Verfügung, bis die Replikation des Speicherkontos abgeschlossen ist. Weitere Informationen finden Sie unter [Vorgehensweise beim Ausfall von Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-disaster-recovery-guidance).

## <a name="application-design-considerations-when-using-ra-grs"></a>Überlegungen zum Anwendungsentwurf bei der Verwendung von RA-GRS

Der Hauptzweck dieses Artikels besteht darin, zu veranschaulichen, wie Sie eine Anwendung entwerfen, die auch im Fall eines schwerwiegenden Zwischenfalls im primären Rechenzentrum weiterhin funktioniert (wenn auch nur mit begrenzter Kapazität). Dazu muss Ihre Anwendung vorübergehende oder langwierige Probleme verarbeiten, indem zum Lesen aus der sekundären Region gewechselt wird, wenn ein Problem vorliegt, und anschließend zurück gewechselt wird, wenn die primäre Region wieder verfügbar ist.

### <a name="using-eventually-consistent-data"></a>Verwenden von letztendlich konsistenten Daten

Bei dieser vorgeschlagenen Lösung wird davon ausgegangen, dass es akzeptabel ist, an die aufrufende Anwendung möglicherweise veraltete Daten zurückzugeben. Da die sekundären Daten letztendlich konsistent sind, ist es möglich, dass die Daten in die primäre Region geschrieben wurden, die Replikation des Updates auf die sekundäre Region jedoch nicht abgeschlossen war, als nicht mehr auf die primäre Region zugegriffen werden konnte.

Beispiel: Ihr Kunde konnte ein erfolgreiches Update übermitteln. Die primäre Region ist jedoch ausgefallen, bevor das Update zur sekundären Region weitergegeben wurde. Wenn der Kunde dann das Zurücklesen der Daten anfordert, empfängt er in diesem Fall die veralteten Daten anstelle der aktualisierten Daten. Sie müssen entscheiden, ob dies akzeptabel ist und wie Sie den Kunden ggf. darüber informieren. Weiter unten in diesem Artikel wird dargestellt, wie Sie den Zeitpunkt der letzten Synchronisierung der sekundären Daten überprüfen, um festzustellen, ob die sekundäre Region auf dem neuesten Stand ist.

### <a name="handling-services-separately-or-all-together"></a>Einzelnes oder gemeinsames Verarbeiten von Services

Auch wenn es unwahrscheinlich ist, kann es vorkommen, dass ein Dienst nicht mehr verfügbar ist, während die anderen Dienste weiterhin voll funktionsfähig sind. Sie können die Wiederholungen und den schreibgeschützten Modus für jeden Dienst (Blobs, Warteschlangen, Tabellen) einzeln verarbeiten, oder Sie können Wiederholungen generisch für alle Speicherdienste zusammen verarbeiten.

Wenn Sie beispielsweise Warteschlangen und Blobs in Ihrer Anwendung verwenden, können Sie sich dazu entschließen, separaten Code für die Verarbeitung von Wiederholungen nach einem Fehler für jeden Fehler hinzufügen. Bei einem Wiederholungsversuch des Blob-Diensts, während der Warteschlangendienst weiterhin funktioniert, wird nur der Teil der Anwendung beeinflusst, der Blobs verarbeitet. Wenn alle Speicherdienstwiederholungen verarbeitet werden sollen und ein Aufruf an den Blob-Dienst einen wiederholbaren Fehler zurückgibt, werden Anforderungen an den Blob-Dienst und den Warteschlangendienst beeinflusst.

Dies hängt letztendlich von der Komplexität Ihrer Anwendung ab. Sie können sich dazu entschließen, die Fehler nicht nach Dienst zu verarbeiten, sondern stattdessen Leseanforderungen für alle Speicherdienste an die sekundäre Region umzuleiten und die Anwendung im schreibgeschützten Modus auszuführen, wenn ein Problem mit dem Speicherdienst in der primären Region auftritt.

### <a name="other-considerations"></a>Weitere Überlegungen

Dies sind die weiteren Überlegungen, die im weiteren Verlauf dieses Artikels beschrieben werden.

*   Verarbeiten von Wiederholungen von Leseanforderungen mit dem Trennschalter-Muster

*   Letztendlich konsistente Daten und Zeitpunkt der letzten Synchronisierung

*   Testen

## <a name="running-your-application-in-read-only-mode"></a>Ausführen der Anwendung im schreibgeschützten Modus

Um RA-GRS-Speicher zu verwenden, müssen Sie in der Lage sein, sowohl Leseanforderungen als auch Aktualisierungsanforderungen mit Fehlern zu verarbeiten (Aktualisierungen sind in diesem Fall Einfügungen, Updates und Löschungen). Wenn beim primären Rechenzentrum ein Fehler auftritt, können Leseanforderungen zum sekundären Rechenzentrum umgeleitet werden, aber für Aktualisierungsanforderungen ist das nicht möglich, da die sekundäre Region schreibgeschützt ist. Aus diesem Grund benötigen Sie eine Methode zum Ausführen der Anwendung im schreibgeschützten Modus.

Sie können beispielsweise ein Flag festlegen, das vor dem Übermitteln von Aktualisierungsanforderungen an den Speicherdienst geprüft wird. Wenn eine der Aktualisierungsanforderungen übermittelt wird, können Sie sie überspringen und eine entsprechende Antwort an den Kunden zurückgeben. Sie können auch bestimmte Features insgesamt deaktivieren, bis das Problem gelöst ist, und die Benutzer darüber benachrichtigen, dass diese Funktionen vorübergehend nicht verfügbar sind.

Wenn Sie sich entscheiden, Fehler für jeden Dienst einzeln zu verarbeiten, müssen Sie auch die Möglichkeit zum Ausführen der Anwendung im schreibgeschützten Modus nach Dienst verarbeiten. Sie können Schreibschutz-Flags für jeden Dienst verwenden, der aktiviert und deaktiviert werden kann, und das entsprechende Flag an den entsprechenden Stellen im Code verarbeiten.

Ein weiterer Vorteil der Anwendungsausführung im schreibgeschützten Modus liegt in der Möglichkeit, während eines umfangreichen Anwendungsupgrades eingeschränkte Funktionalität sicherzustellen. Sie können die Anwendungsausführung im schreibgeschützten Modus auslösen, auf das sekundäre Rechenzentrum verweisen und damit sicherstellen, dass niemand auf die Daten in der primären Region zugreift, während Sie Upgrades vornehmen.

## <a name="handling-updates-when-running-in-read-only-mode"></a>Verarbeiten von Aktualisierungen bei der Ausführung im schreibgeschützten Modus

Es gibt viele Möglichkeiten, Aktualisierungsanforderungen bei der Ausführung im schreibgeschützten Modus zu verarbeiten. Dies wird hier nicht umfassend beschrieben, aber es gibt im Allgemeinen eine Reihe von Mustern, die Sie berücksichtigen sollten.

1.  Sie können auf Ihren Benutzer reagieren und ihm erläutern, dass derzeit keine Aktualisierungen akzeptiert werden. Mit einem Kontaktverwaltungssystem könnten Kunden beispielsweise auf Kontaktinformationen zugreifen, jedoch keine Aktualisierungen vornehmen.

2.  Sie können die Aktualisierungen in einer anderen Region der Warteschlange hinzufügen. In diesem Fall schreiben Sie die ausstehende Aktualisierungsanforderung in eine Warteschlange einer anderen Region und haben dadurch eine Möglichkeit, diese Anforderungen zu verarbeiten, wenn das primäre Rechenzentrum wieder online geschaltet wird. In diesem Szenario sollten Sie den Kunden darüber informieren, dass sich die angeforderte Aktualisierung für die spätere Verarbeitung in der Warteschlange befindet.

3.  Sie können Ihre Aktualisierungen in ein Speicherkonto in einer anderen Region schreiben. Wenn das primäre Rechenzentrum wieder online geschaltet wird, haben Sie die Möglichkeit, diese Aktualisierungen je nach Datenstruktur mit den primären Daten zusammenzuführen. Wenn Sie beispielsweise separate Dateien mit einem Datums-/Zeitstempel im Namen erstellen, können Sie die Dateien zur primären Region kopieren. Dies funktioniert für einige Workloads wie z.B. Protokollierung und IoT-Daten.

## <a name="handling-retries"></a>Verarbeiten von Wiederholungsversuchen

Woher können Sie wissen, welche Fehler wiederholbar sind? Dies wird durch die Speicherclientbibliothek bestimmt. Ein 404-Fehler (Ressource nicht gefunden) ist beispielsweise nicht wiederholbar, da eine Wiederholung wahrscheinlich nicht erfolgreich ist. Andererseits ist ein 500-Fehler wiederholbar, da es sich um einen Serverfehler handelt und möglicherweise einfach ein vorübergehendes Problem vorliegt. Weitere Informationen finden Sie im [Open-Source-Code für die ExponentialRetry-Klasse ](https://github.com/Azure/azure-storage-net/blob/87b84b3d5ee884c7adc10e494e2c7060956515d0/Lib/Common/RetryPolicies/ExponentialRetry.cs) in der Speicherclientbibliothek für .NET. (Suchen Sie nach der ShouldRetry-Methode.)

### <a name="read-requests"></a>Leseanforderungen

Leseanforderungen können in den sekundären Speicher umgeleitet werden, wenn ein Problem mit dem primären Speicher vorliegt. Wie oben unter [Verwenden von letztendlich konsistenten Daten](#using-eventually-consistent-data) beschrieben, muss Ihre Anwendung das Lesen möglicherweise veralteter Daten zulassen. Wenn Sie die Speicherclientbibliothek für RA-GRS-Datenzugriff verwenden, können Sie das Wiederholungsverhalten einer Leseanforderung angeben, indem Sie einen der folgenden Werte für die **LocationMode**-Eigenschaft festlegen:

*   **PrimaryOnly** (Standard)

*   **PrimaryThenSecondary**

*   **SecondaryOnly**

*   **SecondaryThenPrimary**

Wenn Sie **LocationMode** auf **PrimaryThenSecondary** festlegen und bei der ursprünglichen Leseanforderung an den primären Endpunkt ein wiederholbarer Fehler auftritt, führt der Client automatisch eine weitere Leseanforderung an den sekundären Endpunkt aus. Wenn es sich beim Fehler um ein Servertimeout handelt, muss der Client warten, bis das Timeout abläuft, bevor er einen wiederholbaren Fehler vom Dienst empfängt.

Es gibt im Grunde zwei zu berücksichtigende Szenarien bei der Reaktion auf einen wiederholbaren Fehler:

*   Dies ist ein isoliertes Problem, und nachfolgende Anforderungen an den primären Endpunkt geben keinen wiederholbaren Fehler zurück. Dies kann beispielsweise vorkommen, wenn ein vorübergehender Netzwerkfehler vorliegt.

    In diesem Szenario entstehen keine signifikanten Leistungseinbußen dadurch, dass **LocationMode** auf **PrimaryThenSecondary** festgelegt wird, da dies nur selten vorkommt.

*   Es handelt sich um ein Problem mit mindestens einem der Speicherdienste in der primären Region, und alle nachfolgenden Anforderungen an diesen Dienst in der primären Region geben wahrscheinlich für eine bestimmte Zeit einen wiederholbaren Fehler zurück. Dies kann beispielsweise vorkommen, wenn auf die primäre Region absolut nicht zugegriffen werden kann.

    In diesem Szenario treten Leistungseinbußen auf, da alle Leseanforderungen zunächst auf dem primären Endpunkt ausgeführt werden, dann warten, bis das Timeout abläuft, und anschließend zum sekundären Endpunkt wechseln.

Für diese Szenarien sollten Sie feststellen, ob es ein fortlaufendes Problem mit dem primären Endpunkt gibt, und alle Leseanforderungen direkt an den sekundären Endpunkt senden, indem Sie die **LocationMode**-Eigenschaft auf **SecondaryOnly** festlegen. Zu diesem Zeitpunkt sollten Sie die Anwendung auch auf die Ausführung im schreibgeschützten Modus festlegen. Dieser Ansatz wird als [Trennschalter-Muster](https://msdn.microsoft.com/library/dn589784.aspx) bezeichnet.

### <a name="update-requests"></a>Aktualisieren von Anforderungen

Das Trennschalter-Muster kann auch auf Aktualisierungsanforderungen angewendet werden. Aktualisierungsanforderungen können jedoch nicht in den sekundären Speicher umgeleitet werden, da dieser schreibgeschützt ist. Sie sollten für diese Anforderungen den **LocationMode**-Eigenschaftensatz auf **PrimaryOnly** (Standard) belassen. Um diese Fehler zu verarbeiten, können Sie eine Metrik auf diese Anforderungen anwenden, z.B. 10 Fehler hintereinander, und wenn der Schwellenwert erreicht wird, überführen Sie die Anwendung in den schreibgeschützten Modus. Sie können die im nächsten Abschnitt über Trennschalter-Muster beschriebenen Methoden auch zum Zurückkehren in den Aktualisierungsmodus verwenden.

## <a name="circuit-breaker-pattern"></a>Trennschalter-Muster

Durch die Verwendung eines Trennschalter-Musters in Ihrer Anwendung können Sie verhindern, dass ein Vorgang wiederholt wird, bei dem vermutlich erneut ein Fehler auftritt. Die Anwendung kann weiterhin ausgeführt werden, anstatt Zeit in Anspruch zu nehmen, während der Vorgang fortlaufend exponentiell wiederholt wird. Außerdem erkennt das Muster, wenn der Fehler behoben wurde. Zu diesem Zeitpunkt kann die Anwendung den Vorgang erneut versuchen.

### <a name="how-to-implement-the-circuit-breaker-pattern"></a>So implementieren Sie das Trennschalter-Muster

Um festzustellen, ob ein fortlaufendes Problem mit einem primären Endpunkt vorliegt, können Sie überwachen, wie häufig der Client Fehlerwiederholungen erkennt. Da jeder Fall unterschiedlich ist, müssen Sie den Schwellenwert für den Wechsel zum sekundären Endpunkt und zum Ausführen der Anwendung im schreibgeschützten Modus festlegen. Beispielsweise können Sie den Wechsel ausführen, wenn 10 Fehler hintereinander aufgetreten sind. Ein weiteres Beispiel ist, zu wechseln, wenn bei 90 % der Anforderungen in einem Zeitraum von 2 Minuten Fehler auftreten.

Für das erste Szenario können Sie einfach die Anzahl der Fehler verwenden. Bei einem Erfolg vor Erreichen des Maximalwerts wird der Zähler auf null zurückgesetzt. Für das zweite Szenario besteht die Möglichkeit zur Implementierung des MemoryCache-Objekts (in .NET). Für jede Anforderung fügen Sie dem Cache ein CacheItem hinzu, bestimmen den Wert für Erfolg (1) oder Fehler (0) und legen die Ablaufzeit auf 2 Minuten ab dem jetzigen Zeitpunkt fest (je nach gewünschter Zeiteinschränkung). Wenn ein Eintrag abläuft, wird der Eintrag automatisch entfernt. Dadurch erhalten Sie ein laufendes 2-Minuten-Fenster. Jedes Mal, wenn Sie eine Anforderung an den Speicherdienst übermitteln, stellen Sie zuerst eine Linq-Abfrage an das MemoryCache-Objekt, um den Erfolg in Prozent zu berechnen, indem die Werte addiert und die Summe durch die Anzahl geteilt wird. Wenn der Erfolg in Prozent unter einen Schwellenwert (z.B. 10 %) fällt, legen Sie die **LocationMode**-Eigenschaft für Leseanforderungen auf **SecondaryOnly** fest und überführen die Anwendung in den schreibgeschützten Modus, bevor Sie fortfahren.

Die Fehlerschwellenwerte, die zum Bestimmen des Wechsels verwendet werden, unterscheiden sich je nach den Diensten Ihrer Anwendung. Daher sollten Sie in Erwägung ziehen, diese zu konfigurierbaren Parametern zu machen. An dieser Stelle entscheiden Sie auch, ob wiederholbare Fehler von jedem Dienst getrennt oder gemeinsam verarbeitet werden, wie zuvor erläutert.

Ein weiterer Aspekt ist, wie mehrere Instanzen einer Anwendung verarbeitet werden und wie vorgegangen wird, wenn in den einzelnen Instanzen wiederholbare Fehler erkannt werden. Gehen wir beispielsweise davon aus, dass 20 VMs mit der gleichen geladenen Anwendung ausführen. Verarbeiten Sie jede Instanz separat? Wenn in einer Instanz Probleme auftreten, möchten Sie die Reaktion auf nur eine Instanz beschränken, oder sollen alle Instanzen gleichartig reagieren? Die getrennte Verarbeitung von Instanzen ist deutlich einfacher als eine koordinierte Reaktion. Die Vorgehensweise hängt jedoch von der Architektur Ihrer Anwendung ab.

### <a name="options-for-monitoring-the-error-frequency"></a>Optionen zur Überwachung der Fehlerhäufigkeit

Sie haben drei Hauptoptionen für die Überwachung der Fehlerhäufigkeit in der primären Region, mit denen Sie bestimmen können, wann der Wechsel zur sekundären Region und in den schreibgeschützten Modus der Anwendung stattfindet.

*   Fügen Sie dem [**OperationContext**](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.operationcontext.aspx)-Objekt, das Sie an die Speicheranforderungen übergeben, einen Handler für das [**Wiederholungsereignis**](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.operationcontext.retrying.aspx) hinzu – dies ist die Methode, die in diesem Artikel erläutert und im zugehörigen Beispiel verwendet wird. Diese Ereignisse werden ausgelöst, wenn der Client eine Anforderung wiederholt. Dadurch können Sie verfolgen, wie oft der Client wiederholbare Fehler auf einem primären Endpunkt feststellt.

    ```csharp 
    operationContext.Retrying += (sender, arguments) =>
    {
        // Retrying in the primary region
        if (arguments.Request.Host == primaryhostname)
            ...
    };
    ```

*   In der Methode [**Evaluieren**](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.evaluate.aspx) in einer benutzerdefinierten Wiederholungsrichtlinie können Sie benutzerdefinierten Code ausführen, sobald ein erneuter Versuch stattfindet. Zusätzlich zur Erfassung des Zeitpunkts einer Wiederholung erhalten Sie auch die Möglichkeit, das Wiederholungsverhalten zu ändern.

    ```csharp 
    public RetryInfo Evaluate(RetryContext retryContext,
    OperationContext operationContext)
    {
        var statusCode = retryContext.LastRequestResult.HttpStatusCode;
        if (retryContext.CurrentRetryCount >= this.maximumAttempts
        || ((statusCode &gt;= 300 && statusCode &lt; 500 && statusCode != 408)
        || statusCode == 501 // Not Implemented
        || statusCode == 505 // Version Not Supported
            ))
        {
        // Do not retry
            return null;
        }

        // Monitor retries in the primary location
        ...

        // Determine RetryInterval and TargetLocation
        RetryInfo info =
            CreateRetryInfo(retryContext.CurrentRetryCount);

        return info;
    }
    ```

*   Der dritte Ansatz besteht darin, eine benutzerdefinierte Komponente in der Anwendung zu implementieren, die fortlaufend Ping-Nachrichten mit leeren Leseanforderungen (z.B. das Lesen eines kleinen Blobs) an den primären Speicherendpunkt sendet, um den Systemzustand zu ermitteln. Dies nimmt zwar einige Ressourcen in Anspruch, jedoch in einem unerheblichen Maß. Wenn ein Problem festgestellt wird, durch das der Schwellenwert erreicht wird, führen Sie einen Wechsel in den schreibgeschützten Modus **SecondaryOnly** durch.

Zu einem späteren Zeitpunkt möchten Sie wahrscheinlich zum primären Endpunkt zurückwechseln und Aktualisierungen wieder zulassen. Wenn Sie eine der ersten beiden oben aufgeführten Methoden verwenden, können Sie einfach zurück zum primären Endpunkt wechseln und den Aktualisierungsmodus nach einem beliebig gewählten Zeitraum oder einer Anzahl von Vorgängen aktivieren. Anschließend können Sie die Wiederholungslogik erneut ausführen lassen. Wenn das Problem behoben wurde, wird der primäre Endpunkt weiterhin verwendet, und Aktualisierungen werden zugelassen. Wenn weiterhin ein Problem vorliegt, wird erneut zum sekundären Endpunkt und in den schreibgeschützten Modus gewechselt, nachdem die festgelegten Fehlerkriterien nicht erfüllt wurden.

Wenn im dritten Szenario die Ping-Nachricht an den primären Speicherendpunkt erfolgreich ist, können Sie den Wechsel zurück zu **PrimaryOnly** auslösen und weiterhin Aktualisierungen zulassen.

## <a name="handling-eventually-consistent-data"></a>Verarbeiten von letztendlich konsistenten Daten

RA-GRS funktioniert durch die Replikation von Transaktionen von der primären zur sekundären Region. Dieser Replikationsprozess garantiert, dass die Daten in der sekundären Region *letztendlich konsistent* sind. Das heißt, dass alle Transaktionen in der primären Region letztendlich in der sekundären Region angezeigt werden, wobei jedoch möglicherweise eine Verzögerung vor der Anzeige auftritt, und es gibt keine Garantie dafür, dass die Transaktionen in der sekundären Region in der gleichen Reihenfolge wie in der ursprünglichen Reihenfolge der primären Region eintreffen. Wenn die Transaktionen nicht in der richtigen Reihenfolge in der sekundären Region eintreffen, können Sie *möglicherweise* davon ausgehen, dass die Daten in der sekundären Region inkonsistent sind, bis der Dienst auf dem aktuellen Stand ist.

Die folgende Tabelle zeigt ein Beispiel dafür, was passieren kann, wenn Sie die Details einer Mitarbeiterin aktualisieren, um sie zu einem Mitglied der Rolle *Administratoren* zu machen. Für dieses Beispiel müssen Sie die **Mitarbeiterentität** und eine **Administratorrollenentität** mit der Gesamtanzahl der Administratoren aktualisieren. Beachten Sie, dass die Aktualisierungen in der sekundären Region in falscher Reihenfolge angewendet werden.

| **Time** | **Transaktion**                                            | **Replikation**                       | **Zeitpunkt der letzten Synchronisierung** | **Ergebnis** |
|----------|------------------------------------------------------------|---------------------------------------|--------------------|------------| 
| T0       | Transaktion A: <br> Mitarbeiterentität <br> in primäre Region einfügen |                                   |                    | Transaktion A in primäre Region eingefügt,<br> noch nicht repliziert. |
| T1       |                                                            | Transaktion A <br> in sekundäre Region<br> repliziert | T1 | Transaktion A in sekundäre Region repliziert. <br>Zeitpunkt der letzten Synchronisierung aktualisiert.    |
| T2       | Transaktion B:<br>Aktualisieren<br> Mitarbeiterentität<br> in primärer Region  |                                | T1                 | Transaktion B in primäre Region geschrieben,<br> noch nicht repliziert.  |
| T3       | Transaktion C:<br> Aktualisieren <br>administrator<br>Rollenentität in<br>primary |                    | T1                 | Transaktion C in primäre Region geschrieben,<br> noch nicht repliziert.  |
| *T4*     |                                                       | Transaktion C <br>in sekundäre Region<br> repliziert | T1         | Transaktion C in sekundäre Region repliziert.<br>LastSyncTime nicht aktualisiert, da <br>Transaktion B noch nicht repliziert wurde.|
| *T5*     | Entitäten aus sekundärer <br>Region gelesen                           |                                  | T1                 | Sie erhalten den veralteten Wert für die Mitarbeiterentität, <br> da die Transaktion B noch <br> nicht repliziert wurde. Sie erhalten den neuen Wert für<br> die Administratorrollenentität, da C<br> repliziert wurde. Zeitpunkt der letzten Synchronisierung wurde noch nicht<br> aktualisiert, da die Transaktion B noch nicht<br> repliziert wurde. Sie können erkennen, dass die<br>Administratorrollenentität inkonsistent ist, da <br>Datum und Uhrzeit der Entität nach dem Zeitpunkt <br>der letzten Synchronisierung liegen. |
| *T6*     |                                                      | Transaktion B<br> in sekundäre Region<br> repliziert | T6                 | *T6* – alle Transaktionen bis C wurden <br>repliziert, Zeitpunkt der letzten Synchronisierung<br> wurde aktualisiert. |

In diesem Beispiel wird davon ausgegangen, dass der Client bei T5 zum Lesen aus der sekundären Region wechselt. Er kann die **Administratorrollenentität** zu diesem Zeitpunkt erfolgreich lesen, die Entität enthält jedoch einen Wert für die Anzahl der Administratoren, die nicht konsistent mit der Anzahl der **Mitarbeiterentitäten** ist, die zu diesem Zeitpunkt in der sekundären Region als Administratoren gekennzeichnet sind. Der Client konnte diesen Wert anzeigen, es besteht jedoch das Risiko, dass es sich um inkonsistente Informationen handelt. Wahlweise kann der Client versuchen, festzustellen, ob die **Administratorrolle** möglicherweise inkonsistent ist, da die Aktualisierungen nicht in der richtigen Reihenfolge vorliegen, und anschließend können die Benutzer über diese Tatsache informiert werden.

Um möglicherweise inkonsistente Daten zu erkennen, kann der Client den Wert des *Zeitpunkts der letzten Synchronisierung* verwenden, den Sie jederzeit durch eine Abfrage eines Speicherdiensts abrufen können. Dadurch können Sie den Zeitpunkt der letzten Datenkonsistenz in der sekundären Region und den Zeitpunkt feststellen, zu dem der Dienst alle Transaktionen vor diesem Zeitpunkt angewendet hat. Im Beispiel oben wird nach dem Einfügen der **Mitarbeiterentität** in der sekundären Region durch den Dienst der Zeitpunkt der letzten Synchronisierung auf *T1* festgelegt. Der Wert bleibt *T1*, bis der Dienst die **Mitarbeiterentität** in der sekundären Region aktualisiert, sobald sie auf *T6* festgelegt wird. Wenn der Client den Zeitpunkt der letzten Synchronisierung beim Lesen der Entität bei *T5* abruft, kann dieser ihn mit dem Zeitstempel der Entität vergleichen. Wenn der Zeitstempel der Entität nach dem Zeitpunkt der letzten Synchronisierung liegt, ist die Entität möglicherweise inkonsistent, und Sie können die entsprechende Aktion für Ihre Anwendung ausführen. Die Verwendung dieses Felds erfordert, dass Sie wissen, wann die letzte Aktualisierung des primären Replikats abgeschlossen wurde.

## <a name="testing"></a>Testen

Es ist wichtig, zu testen, ob Ihre Anwendung sich erwartungsgemäß verhält, wenn wiederholbare Fehler auftreten. Beispielsweise müssen Sie testen, ob die Anwendung zur sekundären Region und in den schreibgeschützten Modus wechselt, wenn ein Problem entdeckt wird, und zurückwechselt, wenn die primäre Region wieder verfügbar ist. Zu diesem Zweck benötigen Sie eine Möglichkeit zum Simulieren von wiederholbaren Fehlern und deren Auftrittshäufigkeit.

Sie können [Fiddler](http://www.telerik.com/fiddler) zum Abfangen und Bearbeiten von HTTP-Antworten in einem Skript verwenden. Dieses Skript kann Antworten vom primären Endpunkt identifizieren und den HTTP-Statuscode in einen Statuscode ändern, den die Speicherclientbibliothek als wiederholbaren Fehler erkennt. Dieser Codeausschnitt zeigt ein einfaches Beispiel eines Fiddler-Skripts, das Antworten auf Leseanforderungen an die **employeedata**-Tabelle abfängt, um einen 502-Status zurückzugeben:

```java
static function OnBeforeResponse(oSession: Session) {
    ...
    if ((oSession.hostname == "\[yourstorageaccount\].table.core.windows.net")
      && (oSession.PathAndQuery.StartsWith("/employeedata?$filter"))) {
        oSession.responseCode = 502;
    }
}
```

Sie könnten dieses Beispiel erweitern, um ein größeres Spektrum an Anforderungen abzufangen, und nur den **responseCode** für einige Anforderungen ändern, um ein realistischeres Szenario zu simulieren. Weitere Informationen zum Anpassen von Fiddler-Skripts finden Sie unter [Modifying a Request or Response](http://docs.telerik.com/fiddler/KnowledgeBase/FiddlerScript/ModifyRequestOrResponse) (Ändern einer Anforderung oder Antwort) in der Fiddler-Dokumentation.

Wenn Sie die Schwellenwerte für den Wechsel Ihrer Anwendung in den schreibgeschützten Modus konfigurierbar gemacht haben, ist das Testen des Verhaltens anhand von Transaktionsvolumes außerhalb von Produktionsumgebungen einfacher.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu georedundantem Lesezugriff, einschließlich eines weiteren Beispiels zum Festlegen von LastSyncTime, finden Sie unter [Windows Azure Storage Redundancy Options and Read Access Geo Redundant Storage](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) (Windows Azure Storage-Redundanzoptionen und georedundanter Speicherlesezugriff).

* Ein vollständiges Beispiel zur Erläuterung des Wechsels zwischen den primären und sekundären Endpunkten finden Sie unter [Azure Samples – Using the Circuit Breaker Pattern with RA-GRS storage](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs) (Beispiele für Azure – Verwenden des Trennschalter-Musters mit RA-GRS-Speicher).

