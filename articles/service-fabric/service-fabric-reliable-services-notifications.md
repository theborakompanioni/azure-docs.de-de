<properties
   pageTitle="Reliable Service-Benachrichtigungen | Microsoft Azure"
   description="Dokumentation zu Reliable Service-Benachrichtigungen mit Service Fabric"
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
   ms.workload="na"
   ms.date="06/24/2016"
   ms.author="mcoskun"/>

# Reliable Service-Benachrichtigungen

Mit Benachrichtigungen können Clients Änderungen an einem Objekt verfolgen, an dem sie interessiert sind. Es gibt zwei Objekttypen, die Benachrichtigungen unterstützen: **Reliable State Manager** und **Reliable Dictionary**.

Einige Gründe für die Verwendung von Benachrichtigungen:

- Das Erstellen von materialisierten Sichten wie sekundäre Indizes oder aggregierte, gefilterte Ansichten des Replikatstatus. Beispielsweise ein sortierter Index aller Schlüssel in einem Reliable Dictionary.
- Das Senden von Überwachungsdaten, wie z.B. die Anzahl der in der letzten Stunde hinzugefügten Benutzer.

Benachrichtigungen werden während der Anwendung von Vorgängen ausgelöst. Da Benachrichtigungen Teil der Anwendung von Vorgängen sind, muss ihre Bearbeitung unbedingt so schnell wie möglich abgeschlossen werden, und es sollten keine aufwendigen Vorgänge in synchronen Ereignissen ausgeführt werden.

## Reliable State Manager-Benachrichtigungen

Der Reliable State Manager liefert Benachrichtigungen für folgende Ereignisse:

- Transaktion
    - Commit
- Zustands-Manager
    - Neu erstellen
    - Hinzufügen eines Reliable State
    - Entfernen eines Reliable State

Der Reliable State Manager verfolgt die aktuellen In-Flight-Transaktionen. Die einzige Änderung des Transaktionsstatus, bei der eine Benachrichtigung ausgelöst wird, ist der Commit einer Transaktion.

Der Reliable State Manager verwaltet eine Auflistung von Reliable States wie Reliable Dictionary und Reliable Queue. Der Reliable State Manager löst Benachrichtigungen aus, wenn sich diese Auflistung folgendermaßen verändert: Ein Reliable State wird hinzugefügt oder entfernt bzw. die gesamte Auflistung wird neu erstellt. Die Auflistung des Reliable State Manager wird in drei Fällen neu erstellt.

- Wiederherstellung: Wird ein Replikat gestartet, stellt es den vorherigen Zustand vom Datenträger wieder her. Nach Abschluss der Wiederherstellung wird ein Ereignis mit **NotifyStateManagerChangedEventArgs** ausgelöst, das den Satz wiederhergestellter **IReliableState**-Elemente enthält.
- Vollständige Kopie: Bevor ein Replikat dem Konfigurationssatz hinzugefügt werden kann, muss es zunächst erstellt werden. In einigen Fällen muss möglicherweise eine vollständige Kopie des Reliable State Manager-Zustands vom primären Replikat auf das inaktive sekundäre Replikat angewendet werden. Der Reliable State Manager löst auf dem sekundären Replikat ein Ereignis mit **NotifyStateManagerChangedEventArgs** aus, das den **IReliableState**-Satz enthält, der vom primären Replikat übernommen wurde.
- Wiederherstellen: Bei einer Notfallwiederherstellung kann der Replikatzustand mit **RestoreAsync** aus einer Sicherung wiederhergestellt werden. In solchen Fällen löst der Reliable State Manager auf dem primären Replikat ein Ereignis mit **NotifyStateManagerChangedEventArgs** aus, das den **IReliableState**-Satz enthält, der aus der Sicherung wiederhergestellt wurde.

### Verwenden von Reliable State Manager-Benachrichtigungen
Um sich für Transaktionsbenachrichtigungen bzw. State Manager-Benachrichtigungen zu registrieren, müssen Benutzer sich jeweils bei den Ereignissen **TransactionChanged** oder **StateManagerChanged** im Reliable State Manager registrieren. Üblicherweise wird die Registrierung bei diesen Ereignishandlern im Konstruktor Ihres StatefulService durchgeführt. Denn durch die Registrierung im Konstruktor stellt der Client sicher, dass keine Benachrichtigungen verpasst werden, die durch Veränderungen während der Lebensdauer von **IReliableStateManager** entstehen.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

Der **TransactionChanged**-Ereignishandler verwendet **NotifyTransactionChangedEventArgs**, um Details zu dem Ereignis bereitzustellen. Er enthält die Action-Eigenschaft (z.B. **NotifyTransactionChangedAction.Commit**), die den Änderungstyp angibt, sowie die Transaktionseigenschaft, die einen Verweis auf die geänderte Transaktion bereitstellt.

>[AZURE.NOTE] Aktuell werden TransactionChanged-Ereignisse nur ausgelöst, wenn für die Transaktion ein Commit ausgeführt wurde und daher die Aktion gleich NotifyTransactionChangedAction.Commit ist. Allerdings können in zukünftigen Updates weitere Aktionen hinzugefügt werden. Daher empfehlen wir, dass Sie die Aktion überprüfen und das Ereignis nur verarbeiten lassen, wenn es das erwartete ist.

Es folgt ein Beispiel für den **TransactionChanged**-Ereignishandler

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

Der **StateManagerChanged**-Ereignishandler verwendet **NotifyStateManagerChangedEventArgs**, um Details zu dem Ereignis bereitzustellen. **NotifyStateManagerChangedEventArgs** verfügt über zwei Unterklassen: NotifyStateManagerRebuildEventArgs und NotifyStateManagerSingleEntityChangedEventArgs. Die Action-Eigenschaft in **NotifyStateManagerChangedEventArgs** sollte verwendet werden, um **NotifyStateManagerChangedEventArgs** für die richtige Unterklasse umzuwandeln:

- NotifyStateManagerChangedAction.Rebuild: NotifyStateManagerRebuildEventArgs
- NotifyStateManagerChangedAction.Add und NotifyStateManagerChangedAction.Remove: NotifyStateManagerSingleEntityChangedEventArgs

Es folgt ein Beispiel für einen **StateManagerChanged**-Benachrichtigungshandler.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## Reliable Dictionary-Benachrichtigungen

Das Reliable Dictionary liefert Benachrichtigungen für folgende Ereignisse:

- Neu erstellen: Wird aufgerufen, wenn **ReliableDictionary** den Zustand von dem lokalem Datenträger oder aus der Kopie des primären Replikats oder der Sicherung wiederhergestellt hat.
- Löschen: Wird aufgerufen, wenn der **ReliableDictionary**-Zustand mit der **ClearAsync**-Methode gelöscht wurde.
- Hinzufügen: Wird aufgerufen, wenn **ReliableDictionary** ein Element hinzugefügt wurde.
- Aktualisieren: Wird aufgerufen, wenn ein Element in **IReliableDictionary** aktualisiert wurde.
- Entfernen: Wird aufgerufen, wenn ein Element aus **IReliableDictionary** entfernt wurde.

### Verwenden von Reliable Dictionary-Benachrichtigungen
Um sich für Reliable Dictionary-Benachrichtigungen zu registrieren, müssen Benutzer sich beim **DictionaryChanged**-Ereignishandler in **IReliableDictionary** registrieren. Üblicherweise wird die Registrierung bei diesen Ereignishandlern in der **ReliableStateManager.StateManagerChanged**-Hinzufügungsbenachrichtigung durchgeführt. Eine Registrierung zum Zeitpunkt des Hinzufügens von **IReliableDictionary** zu **IReliableStateManager** stellt sicher, dass Sie keine Benachrichtigungen verpassen.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

>[AZURE.NOTE] ProcessStateManagerSingleEntityNotification ist die Beispielmethode, die vom OnStateManagerChangedHandler-Beispiel oben aufgerufen wird.

Beachten Sie, dass der oben aufgeführte Code sowohl IReliableNotificationAsyncCallback als auch **DictionaryChanged** festlegt. Da **NotifyDictionaryRebuildEventArgs** ein **IAsyncEnumerable** enthält, das asynchron aufgelistet werden muss, werden Neuerstellungsbenachrichtigungen durch **RebuildNotificationAsyncCallback** anstelle durch **OnDictionaryChangedHandler** ausgelöst.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

>[AZURE.NOTE] Als Teil der Verarbeitung der Neuerstellungsbenachrichtigung wird im oben aufgeführten Code zuerst der beibehaltene aggregierte Zustand gelöscht. Alle vorherigen Benachrichtigungen sind irrelevant, da die Reliable Collection mit neuem Zustand wiederhergestellt wird.

Der **DictionaryChanged**-Ereignishandler verwendet **NotifyDictionaryChangedEventArgs**, um Details zu dem Ereignis bereitzustellen. **NotifyDictionaryChangedEventArgs** verfügt über fünf Unterklassen. Die Action-Eigenschaft in **NotifyDictionaryChangedEventArgs** sollte verwendet werden, um **NotifyDictionaryChangedEventArgs** für die richtige Unterklasse umzuwandeln:

- NotifyDictionaryChangedAction.Rebuild: NotifyDictionaryRebuildEventArgs
- NotifyDictionaryChangedAction.Clear: NotifyDictionaryClearEventArgs
- NotifyDictionaryChangedAction.Add und NotifyStateManagerChangedAction.Remove: NotifyDictionaryItemAddedEventArgs
- NotifyDictionaryChangedAction.Update: NotifyDictionaryItemUpdatedEventArgs
- NotifyDictionaryChangedAction.Remove: NotifyDictionaryItemRemovedEventArgs

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## Empfehlungen

- Führen Sie Benachrichtigungsereignisse so schnell wie möglich durch.
- Führen Sie KEINE aufwendigen Vorgänge (z.B. E/A-Vorgänge) im Rahmen der synchronen Ereignisse aus.
- Überprüfen Sie vor Bearbeitung des Ereignisses den Aktionstyp. Neue Aktionstypen können in Zukunft hinzugefügt werden.

Hier folgen einige Punkte, die es zu beachten gilt:
- Benachrichtigungen werden während der Ausführung eines Vorgangs ausgelöst. Eine Wiederherstellungsbenachrichtigung wird beispielsweise als letzter Schritt der Wiederherstellung ausgelöst, und die Wiederherstellung kann erst abgeschlossen werden, wenn das Benachrichtigungsereignis verarbeitet wurde.
- Da Benachrichtigungen als Teil der Anwendung von Vorgängen ausgelöst werden, werden Clients nur Benachrichtigungen für Vorgänge mit lokaler Commitausführung angezeigt. Beachten Sie, dass Vorgänge in Zukunft eventuell rückgängig gemacht werden können, da für sie nur ein lokaler Commit (d.h. Protokollierung) garantiert wird.
- Im Pfad „Wiederholen“ wird eine einzelne Benachrichtigung für jeden angewendeten Vorgang ausgelöst. Dies bedeutet, wenn eine Transaktion T1 die Elemente Create(X), Delete(X), Create(X) enthält, erhalten Sie Benachrichtigungen in dieser Reihenfolge: eine für die Erstellung von X, eine für den Löschvorgang und erneut eine für die Erstellung.
- Für Transaktionen, die mehrere Vorgänge enthalten, werden Vorgänge in der Reihenfolge angewendet, in der sie auf dem primären Replikat durch den Benutzer empfangen wurden.
- Im Rahmen der Verarbeitung von falschen Fortschritten können einige Vorgänge rückgängig gemacht werden. Für solche Vorgänge zum Rückgängigmachen, die den Zustand des Replikats auf einen stabilen Punkt zurücksetzen, werden Benachrichtigungen ausgelöst. Ein wichtiger Unterschied bei Benachrichtigungen zu Vorgängen zum Rückgängigmachen ist es, dass Ereignisse mit doppelten Schlüsseln aggregiert werden. Wird beispielsweise die oben aufgeführte T1-Transaktion rückgängig gemacht, wird dem Benutzer eine einzelne Löschbenachrichtigung (X) angezeigt.

## Nächste Schritte

- [Reliable Services – Schnellstart](service-fabric-reliable-services-quick-start.md)
- [Sichern und Wiederherstellen von Reliable Services (Notfallwiederherstellung)](service-fabric-reliable-services-backup-restore.md)
- [Entwicklerreferenz für zuverlässige Auflistungen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

<!---HONumber=AcomDC_0629_2016-->