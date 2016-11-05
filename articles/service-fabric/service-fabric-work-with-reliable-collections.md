---
title: Arbeiten mit Reliable Collections | Microsoft Docs
description: Lernen Sie die bewährten Methoden für die Arbeit mit Reliable Collections kennen.
services: service-fabric
documentationcenter: .net
author: JeffreyRichter
manager: timlt
editor: ''

ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/28/2016
ms.author: jeffreyr

---
# Arbeiten mit Reliable Collections
Service Fabric bietet .NET-Entwicklern über Reliable Collections ein zustandsbehaftetes Programmiermodell. Service Fabric umfasst z. B. Reliable Dictionary- und Reliable Queue-Klassen. Wenn Sie diese Klassen verwenden, wird Ihr Zustand partitioniert (für Skalierbarkeit), repliziert (für Verfügbarkeit) und innerhalb einer Partition durchgeführt (für ACID-Semantik). Sehen wir uns nun die typische Nutzung eines Reliable Dictionary-Objekts an.

~~~
retry:
try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to  
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) { 
   await Task.Delay(100, cancellationToken); goto retry; 
}
~~~

Alle Vorgänge für Reliable Dictionary-Objekte (mit Ausnahme von ClearAsync, das nicht annullierbar ist), benötigen ein ITransaction-Objekt. Diesem Objekt sind sämtliche Änderungen zugeordnet, die Sie an Reliable Dictionary- und/oder Reliable Queue-Objekten innerhalb einer einzelnen Partition vornehmen möchten. Sie erhalten ein ITransaction-Objekt, indem Sie die CreateTransaction-Methode des Zustands-Managers der Partition aufrufen.

Im obigen Code wird das ITransaction-Objekt an eine AddAsync-Methode eines Reliable Dictionarys übergeben. Dictionary-Methoden, die einen Schlüssel akzeptieren, wenden intern eine dem Schlüssel zugeordnete Lese-/Schreibsperre an. Wenn die Methode den Wert des Schlüssels ändert, nimmt die Methode eine Schreibsperre für den Schlüssel vor. Wenn die Methode nur aus dem Wert des Schlüssels liest, wird eine Lesesperre für den Schlüssel vorgenommen. Da AddAsync den Wert des Schlüssels auf den neuen, übergebenen Wert ändert, wird die Schreibsperre des Schlüssels aktiviert. Wenn also 2 (oder mehr) Threads gleichzeitig versuchen, Werte mit demselben Schlüssel hinzuzufügen, aktiviert ein Thread die Schreibsperre und blockiert die anderen Threads. In der Standardeinstellung blockieren Methoden bis zu 4 Sekunden zum Aktivieren der Sperre. Nach 4 Sekunden lösen die Methoden eine TimeoutException aus. Methodenüberladungen ermöglichen Ihnen, auf Wunsch einen expliziten Timeout-Wert festzulegen.

In der Regel schreiben Sie Ihren Code, um auf eine TimeoutException zu reagieren, indem Sie diese abfangen und den gesamten Vorgang wiederholen (wie im obigen Code gezeigt). In meinem einfachen Code gebe ich für Task.Delay stets 100 Millisekunden ein. In der Praxis ist jedoch eine exponentielle Backoffverzögerung eher zu empfehlen.

Sobald die Sperre abgerufen wurde, fügt AddAsync die Schlüssel- und Wertobjektverweise einem internen, temporären Dictionary hinzu, das dem ITransaction-Objekt zugeordnet ist. Dies geschieht, um Sie Ihnen eine Semantik zum Lesen eigener Schreibvorgänge bereitzustellen. Nach dem Aufrufen von AddAsync wird durch Aufrufen von TryGetValueAsync (mit dem gleichen ITransaction-Objekt) der Wert zurückgegeben, auch wenn Sie noch keinen Commit für die Transaktion ausgeführt haben. Als Nächstes serialisiert AddAsync Ihre Schlüssel- und Wertobjekte zu Bytearrays und wendet diese Bytearrays auf eine Protokolldatei auf dem lokalen Knoten an. Schließlich sendet AddAsync Bytearrays an alle sekundären Replikate, sodass diese die gleichen Schlüssel-/Wertinformationen aufweisen. Obwohl die Schlüssel-/Wertinformationen in eine Protokolldatei geschrieben wurden, werden die Informationen erst als Teil des Dictionarys erachtet, wenn ein Commit für die ihnen zugeordnete Transaktion ausgeführt wurde.

Im obigen Code wird durch den Aufruf von CommitAsync ein Commit für alle Vorgänge der Transaktion ausgeführt. Dabei werden Commit-Informationen an die Protokolldatei auf dem lokalen Knoten angehängt und der Commit-Datensatz außerdem an alle sekundären Replikate gesendet. Nachdem ein Quorum (Mehrheit) der Replikate geantwortet hat, werden alle Datenänderungen als dauerhaft erachtet, und alle Sperren, die mit über das ITransaction-Objekt bearbeiteten Schlüsseln verknüpft sind, werden aufgehoben. So können andere Threads/Transaktionen dieselben Schlüssel und deren Werte bearbeiten.

Wird CommitAsync (in der Regel aufgrund einer ausgelösten Ausnahme) nicht aufgerufen, wird das ITransaction-Objekt verworfen. Wenn ein ITransaction-Objekt ohne ausgeführten Commit verworfen wird, fügt Service Fabric der Protokolldatei des lokalen Knotens Abort-Informationen hinzu, und nichts muss an die sekundären Replikate gesendet werden. Dann werden alle Sperren aufgehoben, die mit über die Transaktion bearbeiteten Schlüsseln verknüpft sind.

## Häufige Probleme und deren Vermeidung
Nachdem Sie nun wissen, wie Reliable Collections intern funktionieren, sehen wir einige Fälle von missbräuchlicher Verwendung an. Sehen Sie den folgenden Code an:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes, 
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
~~~

Bei der Arbeit mit einem regulären .NET-Dictionary können Sie dem Dictionary einen Schlüssel/Wert hinzufügen und dann den Wert einer Eigenschaft ändern (z. B. LastLogin). Dieser Code funktioniert jedoch mit einem Reliable Dictionary nicht ordnungsgemäß . Wir erinnern uns: Der Aufruf von AddAsync serialisiert die Schlüssel-/Wertobjekte in Bytearrays, speichert dann die Arrays in einer lokalen Datei und sendet diese an die sekundären Replikate. Wenn Sie später eine Eigenschaft ändern, wird der Wert der Eigenschaft nur im Speicher geändert. Die Änderung wirkt sich nicht auf die lokale Datei oder die an die Replikaten gesendeten Daten aus. Wenn der Prozess abstürzt, wird alles im Speicher verworfen. Wenn ein neuer Prozess gestartet oder ein anderes Replikat zum primären Element wird, ist der alte Wert der Eigenschaft verfügbar.

Der oben gezeigte Fehler kann Benutzern wirklich unglaublich schnell unterlaufen. Noch dazu bemerken Sie den Fehler erst dann, wenn bzw. falls der Prozess ausfällt. Die richtige Vorgehensweise zum Schreiben des Codes besteht darin, die beiden Zeilen umzukehren:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync(); 
}
~~~

Hier sehen Sie ein weiteres Beispiel für einen häufigen Fehler:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync(); 
   }
}
~~~

Auch hier funktioniert der Code mit regulären .NET-Dictionarys und entspricht einem gängigen Muster: Der Entwickler verwendet einen Schlüssel, um einen Wert zu suchen. Wenn der Wert vorhanden ist, ändert der Entwickler den Wert einer Eigenschaft. In Reliable Collections weist der Code jedoch dasselbe Problem auf wie zuvor beschrieben: **Sie dürfen ein Objekt in einer Reliable Collection NICHT ändern.**

Um einen Wert in einer Reliable Collection ordnungsgemäß zu aktualisieren, rufen Sie einen Verweis auf den vorhandenen Wert ab und betrachten das Objekt als unveränderlich. Anschließend erstellen Sie ein neues Objekt, das eine exakte Kopie des ursprünglichen Objekts ist. Nun können Sie den Status des neuen Objekts ändern und das neue Objekt in die Auflistung schreiben, damit es in Bytearrays serialisiert, an die lokale Datei angefügt und an die Replikate gesendet wird. Nach dem Commit der Änderung(en) weisen die Objekte im Arbeitsspeicher, in der lokalen Datei und in allen Replikaten genau denselben Zustand auf. Alles in Ordnung!

Der folgende Code zeigt die richtige Methode, um einen Wert in einer Reliable Collection zu aktualisieren:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire 
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync(); 
   }
}
~~~

## Definieren von unveränderlichen Datentypen, um Programmierfehler zu verhindern
Im Idealfall sollte der Compiler Fehler melden, wenn Sie versehentlich Code erzeugen, der den Zustand eines Objekts ändert, das nicht geändert werden darf. Leider ist dies mit dem C#-Compiler nicht möglich. Um potenzielle Programmierfehler zu vermeiden, raten wir Ihnen dringend dazu, die mit Reliable Collections verwendeten Typen als unveränderliche Typen zu definieren. Dazu halten Sie sich an Hauptwerttypen (z. B. Zahlen [Int32, UInt64 usw.], DateTime, Guid, TimeSpan und ähnliche Werte). Natürlich können sie auch String verwenden. Vermeiden Sie im Idealfall Auflistungseigenschaften, da deren Serialisierung und Deserialisierung die Leistung beeinträchtigen kann. Wenn Sie jedoch Auflistungseigenschaften verwenden möchten, raten wir dringend dazu, die .NET-Bibliothek der unveränderlichen Auflistungen (System.Collections.Immutable) zu verwenden. Diese Bibliothek können Sie unter http://nuget.org herunterladen. Zudem empfehlen wir nach Möglichkeit, Klassen zu versiegeln und schreibgeschützte Felder zu verwenden.

Der folgende UserInfo-Typ veranschaulicht die Definition eines unveränderlichen Typs mithilfe der oben genannten Empfehlungen.

~~~
[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;

   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }

   [DataMember]
   public readonly String Email;

   // Ideally, this would be a readonly field but it can't be because OnDeserialized 
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
~~~

Der ItemID-Typ ist ebenfalls ein unveränderlicher Typ, wie hier gezeigt:

~~~
[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
~~~

## Schema-Versionsverwaltung (Upgrades)
Intern serialisieren die Reliable Collections Ihre Objekte mit dem .NET DataContractSerializer. Die serialisierten Objekte werden auf der lokalen Festplatte des primären Replikats übernommen und auch an die sekundären Replikate übermittelt. Im Zuge der Entwicklung Ihres Dienstes möchten Sie möglicherweise die Art der erforderlichen Daten (Schema) ändern. Lassen Sie bei der Versionsverwaltung Ihrer Daten große Vorsicht walten. Höchste Priorität hat, dass Sie stets in der Lage sein müssen, alte Daten zu deserialisieren. Ihr Deserialisierungscode muss daher unendlich abwärtskompatibel sein: Version 333 Ihres Dienstcodes muss mit Daten arbeiten können, die vor 5 Jahren mit der Version 1 Ihres Dienstcodes zu einer Reliable Collection hinzugefügt wurden.

Zudem wird der Dienstcode auf jeder Upgradedomäne einzeln aktualisiert. Während der Aktualisierung müssen also zwei verschiedene Versionen Ihres Dienstcodes gleichzeitig ausgeführt werden. Sie müssen vermeiden, dass die neue Version Ihres Dienstcodes das neue Schema wie alte Versionen Ihres Dienstcodes verwendet, da dieser das neue Schema möglicherweise nicht verarbeiten kann. Nach Möglichkeit sollten Sie jede Version des Diensts um 1 Version aufwärtskompatibel entwickeln. Insbesondere bedeutet dies, dass V1 des Dienstcodes einfach alle Elemente des Schemas ignorieren sollte, die sie nicht explizit behandelt. Allerdings muss sie alle Daten, die ihr nicht explizit bekannt sind, speichern und einfach zurückschreiben können, wenn ein Dictionary-Schlüssel oder -Wert aktualisiert wird.

> [!WARNING]
> Sie können Sie das Schema eines Schlüssels ändern, müssen jedoch sicherstellen, dass die Hashcode- und Gleichungsalgorithmen Ihres Schlüssels stabil sind. Wenn Sie die Funktionsweise der folgenden Algorithmen ändern, können Sie den Schlüssel nicht mehr im Reliable Dictionary abfragen.
> 
> 

Alternativ können Sie einen Vorgang ausführen, der als 2-Phasen-Upgrade bezeichnet wird. Mit einem 2-Phasen-Upgrade aktualisieren Sie den Dienst aus V1 auf V2: V2 enthält den Code, der mit der neuen Schemaänderung umzugehen weiß, aber dieser Code wird nicht ausgeführt. Wenn der V2-Code V1-Daten liest, verwendet und schreibt er V1-Daten. Wenn das Upgrade auf allen Upgradedomänen abgeschlossen ist, können Sie den laufenden V2-Instanzen signalisieren, dass das Upgrade abgeschlossen ist. (Eine Möglichkeit, dies zu signalisieren, besteht darin, ein Konfigurationsupgrade durchzuführen. Daher wird dieser Vorgang als 2-Phasen-Upgrade bezeichnet.) Jetzt können die V2-Instanzen V1-Daten lesen, in V2-Daten konvertieren, diese verwenden und als V2-Daten schreiben. Wenn andere Instanzen V2-Daten lesen, müssen sie diese nicht konvertieren. Sie verwenden sie einfach und schreiben V2-Daten.

## Nächste Schritte
Informationen zum Erstellen von aufwärtskompatiblen Datenverträgen finden Sie unter [Aufwärtskompatible Datenverträge](https://msdn.microsoft.com/library/ms731083.aspx).

Bewährte Methoden zur Versionsverwaltung von Datenverträgen finden Sie unter [Datenvertragsversionsverwaltung](https://msdn.microsoft.com/library/ms731138.aspx).

Informationen zum Implementieren von versionstoleranten Datenverträgen finden Sie unter [Versionstolerante Serialisierungsrückrufe](https://msdn.microsoft.com/library/ms733734.aspx).

Weitere Informationen zum Erstellen einer Datenstruktur, zum Interagieren über mehrere Versionen finden Sie unter [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).

<!---HONumber=AcomDC_0406_2016-->