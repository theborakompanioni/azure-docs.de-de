<properties
   pageTitle="Festlegen einer Gültigkeitsdauer für den Ablauf von Daten in DocumentDB | Microsoft Azure"
   description="Mit TTL bietet Microsoft Azure DocumentDB die Möglichkeit, im System vorhandene Dokumente nach einem bestimmten Zeitraum automatisch zu löschen."
   services="documentdb"
   documentationCenter=""
   keywords="Gültigkeitsdauer (Time To Live, TTL)"
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2016"
   ms.author="kipandya"/>

# Festlegen einer Gültigkeitsdauer für den automatischen Ablauf von Daten in DocumentDB-Sammlungen

Anwendungen können Unmengen an Daten generieren und speichern. Einige dieser Daten (etwa vom Computer generierte Ereignisdaten, Protokolle und Benutzersitzungsinformationen) sind allerdings nur für einen begrenzten Zeitraum relevant. Sobald die Daten von der Anwendung nicht mehr benötigt werden, können sie gefahrlos gelöscht werden, um den Speicherbedarf einer Anwendung zu verringern.

Mit der Gültigkeitsdauer (Time To Live, TTL) bietet Microsoft Azure DocumentDB die Möglichkeit, Dokumente nach einem bestimmten Zeitraum automatisch aus der Datenbank zu löschen. Die standardmäßige Gültigkeitsdauer kann auf Sammlungsebene festgelegt und für individuelle Dokumente überschrieben werden. Nach dem Festlegen der Gültigkeitsdauer (auf Sammlungs- oder Dokumentebene) werden Dokumente, die nach dem in Sekunden angegebenen Zeitraum (beginnend ab der letzten Änderung) vorhanden sind, von DocumentDB automatisch entfernt.

Die Gültigkeitsdauer in DocumentDB basiert auf einem Offset für den letzten Änderungszeitpunkt eines Dokuments. Zu diesem Zweck wird das für jedes Dokument vorhandene Feld „\_ts“ verwendet. Bei dem Feld „\_ts“ handelt es sich um einen Epochenzeitstempel im Unix-Format zur Darstellung von Datum und Uhrzeit. Das Feld „\_ts“ wird bei jeder Änderung eines Dokuments aktualisiert.

## TTL-Verhalten

Das TTL-Feature wird über TTL-Eigenschaften auf zwei Ebenen (Sammlungsebene und Dokumentebene) gesteuert. Die Werte werden in Sekunden festgelegt und fungieren als Delta für den letzten Änderungszeitpunkt des Dokuments (aus dem Feld „\_ts“).

 1.  DefaultTTL für die Sammlung
  * Wenn diese Angabe fehlt (oder auf Null festgelegt ist), werden Dokumente nicht automatisch gelöscht.
  
  * Ist die Angabe vorhanden und der Wert auf „-1“ (unendlich) festgelegt, laufen Dokumente standardmäßig nicht ab.
  
  * Ist die Angabe vorhanden und der Wert auf eine beliebige Zahl (n) festgelegt, laufen Dokumente n Sekunden nach der letzten Änderung ab.

 2.  TTL für die Dokumente
  * Die Eigenschaft wird nur angewendet, wenn DefaultTTL für die übergeordnete Auflistung vorhanden ist.
  
  * Überschreibt den DefaultTTL-Wert für die übergeordnete Auflistung.

Abgelaufene Dokumente (TTL + \_ts > = aktuelle Serverzeit) werden als abgelaufen markiert. Ab diesem Zeitpunkt können für diese Dokumente keine Vorgänge mehr ausgeführt werden, und sie werden in Abfrageergebnissen nicht mehr berücksichtigt. Die Dokumente werden im System physisch gelöscht und zu einem späteren Zeitpunkt opportunistisch im Hintergrund gelöscht. Dabei werden keinerlei [Anforderungseinheiten (Request Units, RUs)](documentdb-request-units.md) des Sammlungsbudgets verbraucht.

Die obige Logik wird in der folgenden Matrix veranschaulicht:

| | DefaultTTL fehlt/nicht für die Sammlung festgelegt | DefaultTTL = -1 für die Sammlung | DefaultTTL = n für die Sammlung|
| ------------- |:-------------|:-------------|:-------------|
| TTL für Dokument nicht vorhanden| Auf der Dokumentebene kann nichts überschrieben werden, da das TTL-Konzept weder auf der Dokument- noch auf der Sammlungsebene zur Anwendung kommt. | In dieser Sammlung laufen keine Dokumente ab. | Die Dokumente in dieser Sammlung laufen nach dem Intervall n ab. |
| TTL = -1 für das Dokument | Auf der Dokumentebene kann nichts überschrieben werden, da auf der Sammlungsebene keine DefaultTTL-Eigenschaft definiert ist, die ein Dokument überschreiben könnte. Der TTL-Wert für ein Dokument wird vom System nicht interpretiert. | In dieser Sammlung laufen keine Dokumente ab. | Das Dokument mit TTL = -1 in dieser Auflistung läuft nicht ab. Alle anderen Dokumente laufen nach dem Intervall n ab. |
| TTL = n für das Dokument | Auf der Dokumentebene kann nichts überschrieben werden. Der TTL-Wert für ein Dokument wird vom System nicht interpretiert. | Das Dokument mit TTL = n läuft nach dem Intervall n (in Sekunden) ab. Andere Dokumente erben das Intervall -1 und laufen nie ab. | Das Dokument mit TTL = n läuft nach dem Intervall n (in Sekunden) ab. Andere Dokumente erben das Intervall n von der Sammlung. |


## Konfigurieren von TTL

Die Gültigkeitsdauer ist in DocumentDB-Sammlungen und für alle Dokumente standardmäßig deaktiviert.

## Aktivieren von TTL

Um TTL für eine Sammlung (oder für die Dokumente in einer Sammlung) zu aktivieren, müssen Sie die DefaultTTL-Eigenschaft einer Auflistung entweder auf „-1“ oder auf eine positive Zahl ungleich Null festlegen. Wenn DefaultTTL auf „-1“ festgelegt wird, laufen die Dokumente in der Sammlung standardmäßig nicht ab, der DocumentDB-Dienst überwacht die Sammlung aber auf Dokumente, bei denen diese Standardeinstellung überschrieben wurde.

## Konfigurieren einer Standardgültigkeitsdauer für eine Sammlung

Sie haben die Möglichkeit, eine Standardgültigkeitsdauer auf Sammlungsebene zu konfigurieren.

Wenn Sie die Gültigkeitsdauer für eine Sammlung festlegen möchten, müssen Sie mit einer positiven Zahl ungleich Null den Zeitraum angeben, nach dem alle in der Sammlung enthaltenen Dokumente ablaufen (in Sekunden ab dem Zeitstempel der letzten Änderung des Dokuments [\_ts]).

Alternativ können Sie den Standardwert auf „-1“ festlegen. In diesem Fall laufen die in die Sammlung eingefügten Dokumente niemals ab.

## Festlegen einer Gültigkeitsdauer für ein Dokument

Zusätzlich zu einer Standardgültigkeitsdauer für eine Sammlung können Sie eine spezifische Gültigkeitsdauer auf Dokumentebene festlegen. Dadurch wird die Standardeinstellung der Sammlung überschrieben.

Wenn Sie die Gültigkeitsdauer für ein Dokument festlegen möchten, müssen Sie mit einer positiven Zahl ungleich Null den Zeitraum angeben, nach dem das Dokument abläuft (in Sekunden ab dem Zeitstempel der letzten Änderung des Dokuments [\_ts]).

Konfigurieren Sie zum Festlegen dieses Ablauf-Offsets das TTL-Felds für das Dokument.

Für Dokumente ohne TTL-Feld gilt die Standardeinstellung der Sammlung.

Wenn die Gültigkeitsdauer auf der Sammlungsebene deaktiviert ist, wird das TTL-Feld für das Dokument ignoriert, bis die Gültigkeitsdauer für die Sammlung wieder aktiviert wird.


## Verlängern der Gültigkeitsdauer für ein vorhandenes Dokument

Die Gültigkeitsdauer für ein Dokument kann durch Ausführen eines beliebigen Schreibvorgangs für das Dokument zurückgesetzt werden. Dadurch wird „\_ts“ auf die aktuelle Zeit festgelegt, und der Countdown für den mittels TTL festgelegten Dokumentablauf beginnt von vorn.

Wenn Sie die Gültigkeitsdauer eines Dokuments ändern möchten, können Sie das Feld genau wie jedes andere festlegbare Feld aktualisieren.


## Entfernen der Gültigkeitsdauer von einem Dokument

Falls für ein Dokument eine Gültigkeitsdauer festgelegt wurde, das Dokument nun aber nicht mehr ablaufen soll, können Sie das Dokument abrufen, das TTL-Feld entfernen und das Dokument anschließend auf dem Server ersetzen.

Nach dem Entfernen des TTL-Felds aus dem Dokument gilt wieder der Standardwert der Sammlung.

Wenn ein Dokument nicht mehr ablaufen und auch nicht die Einstellung der Sammlung erben soll, muss der TTL-Wert auf „-1“ festgelegt werden.


## Deaktivieren von TTL

Wenn Sie TTL für eine Sammlung vollständig deaktivieren und den Hintergrundprozess für die Suche nach abgelaufenen Dokumenten beenden möchten, muss die DefaultTTL-Eigenschaft für die Sammlung gelöscht werden.

Das Löschen dieser Eigenschaft ist nicht das Gleiche wie das Festlegen der Eigenschaft auf „-1“. Bei Verwendung der Einstellung „-1“ laufen neue Dokumente, die der Sammlung hinzugefügt werden, nicht ab, dies kann jedoch für individuelle Dokumente in der Sammlung überschrieben werden.

Wenn Sie die Eigenschaft vollständig aus der Sammlung entfernen, läuft ebenfalls keines der Dokumente ab, dies gilt aber auch für Dokumente, bei denen explizit ein vorheriger Standardwert überschrieben wurde.


## Häufig gestellte Fragen

**Was kostet TTL?**

Die Verwendung von TTL für ein Dokument ist mit keinerlei Zusatzkosten verbunden.

**Wie lange dauert es nach dem Aktivieren von TTL, bis mein Dokument gelöscht wird?**

Abgelaufene Dokumente (TTL + \_ts > = aktuelle Serverzeit) werden umgehend als nicht verfügbar markiert. Ab diesem Zeitpunkt können für diese Dokumente keine Vorgänge mehr ausgeführt werden, und sie werden in Abfrageergebnissen nicht mehr berücksichtigt. Die Dokumente werden im Hintergrund physisch gelöscht. Dabei werden keinerlei Anforderungseinheiten (Request Units, RUs) des Sammlungsbudgets verbraucht.

**Wenn die Löschung der Dokumente einige Zeit dauert, werden die Dokumente bis zu ihrer Löschung auf mein Kontingent (und meine Rechnung) angerechnet?**

Nein. Nach dem Ablauf von Dokumenten wird Ihnen die Speicherung dieser Dokumente nicht weiter in Rechnung gestellt, und die Größe der Dokumente wird auch nicht auf das Speicherkontingent für eine Sammlung angerechnet.

**Hat die Gültigkeitsdauer für ein Dokument Auswirkung auf die Gebühren für Anforderungseinheiten?**

Nein. Vorgänge, die für ein Dokument in DocumentDB ausgeführt werden, haben keine Auswirkungen auf die Gebühren für Anforderungseinheiten.

**Hat das Löschen von Dokumenten Auswirkungen auf den Durchsatz, den ich für meine Sammlung bereitgestellt habe?**

Nein. Die Verarbeitung von Anforderungen für Ihre Sammlung hat Vorrang vor dem Hintergrundprozess zum Löschen Ihrer Dokumente. Das Hinzufügen einer Gültigkeitsdauer zu einem Dokument hat hierauf keine Auswirkungen.

**Wie lange dauert es, bis ein abgelaufenes Dokument aus meiner Sammlung gelöscht wird?**

Sobald ein Dokument abgelaufen ist, kann nicht mehr darauf zugegriffen werden. Wie lange es genau dauert, bis ein Dokument tatsächlich aus Ihrer Sammlung gelöscht wird, ist nicht deterministisch und hängt davon ab, wann das Dokument durch den Hintergrundprozess gelöscht werden kann.

**Werden abgelaufene Dokumente knotenübergreifend gelöscht oder wird hier ein Eventual Consistency-Ansatz verfolgt?**

Die Verfügbarkeit des Dokuments endet gleichzeitig auf allen Knoten und in allen Regionen.

**Fallen für Hintergrundaufgaben zur TTL-Überwachung Kosten für Anforderungseinheiten an?**

Nein, dafür fallen keine Kosten für Anforderungseinheiten an.

**Wie oft wird der TTL-Ablauf überprüft?**

Die Überprüfung des TTL Ablaufs findet nicht als Hintergrundprozess statt. Wenn der Back-End-Dienst auf eine Anforderung reagiert, führt er die Überprüfungen inline durch und schließt alle abgelaufenen Dokumente aus. Die Löschung des physischen Dokuments ist der einzige Prozess, der asynchron im Hintergrund ausgeführt wird. Die Häufigkeit dieses Prozesses hängt von den verfügbaren Anforderungseinheiten für die Sammlung ab.

**Gilt das TTL-Feature nur für vollständige Dokumente oder kann ich einen Ablauf für individuelle Dokumenteigenschaftswerte konfigurieren?**

TTL gilt für das gesamte Dokument. Soll nur ein Teil eines Dokuments ablaufen, empfiehlt es sich, den Teil aus dem Hauptdokument in ein separates, verknüpftes Dokument zu extrahieren und die Gültigkeitsdauer für das extrahierte Dokument zu verwenden.

**Gelten für das TTL-Feature bestimmte Indizierungsanforderungen?**

Ja. Die [Indizierungsrichtlinie](documentdb-indexing-policies.md) der Sammlung muss auf „Lazy“ oder „Consistent“ festgelegt sein. Beim Versuch, DefaultTTL für eine Sammlung mit der Indizierung „None“ festzulegen, tritt ein Fehler auf. Gleiches gilt, wenn Sie versuchen, die Indizierung für eine Auflistung zu deaktivieren, für die DefaultTTL bereits festgelegt wurde.


## Nächste Schritte

Weitere Informationen zu Azure DocumentDB finden Sie auf der [*Dokumentationsseite*](https://azure.microsoft.com/documentation/services/documentdb/) des Diensts.

<!---HONumber=AcomDC_0720_2016-->