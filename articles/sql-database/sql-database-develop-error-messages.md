---
title: SQL-Fehlercodes – Datenbankverbindungsfehler | Microsoft Docs
description: 'Erfahren Sie mehr über SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen, beispielsweise zu häufigen Datenbankverbindungsfehlern, Datenbankkopiefehlern und allgemeinen Fehlern. '
keywords: SQL-Fehlercodes,Zugriff auf SQL,Datenbankverbindungsfehler,SQL-Fehlercodes
services: sql-database
documentationcenter: ''
author: annemill
manager: jhubbard
editor: ''

ms.service: sql-database
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2016
ms.author: annemill

---
# SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Datenbankverbindungsfehler und andere Probleme
<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


In diesem Artikel werden SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen aufgeführt, darunter Datenbankverbindungsfehler, vorübergehende Fehler, Fehler zur Ressourcenkontrolle, Datenbankkopierfehler, Fehler in Zusammenhang mit elastischen Pools und andere. Die meisten Kategorien gelten speziell für Azure SQL-Datenbank und nicht für Microsoft SQL Server.

## Datenbankverbindungsfehler, vorübergehende Fehler und andere temporäre Fehler
Die folgende Tabelle enthält die SQL-Fehlercodes für Fehler bei Verbindungsverlust und andere vorübergehende Fehler, die auftreten können, wenn Ihre Anwendung versucht, auf SQL-Datenbank zuzugreifen. Tutorials für die ersten Schritte zum Herstellen einer Verbindung mit Azure SQL-Datenbank finden Sie unter [Herstellen einer Verbindung mit Azure SQL-Datenbank](sql-database-libraries.md).

### Häufigste Datenbankverbindungsfehler und vorübergehende Fehler
Die Azure-Infrastruktur verfügt über die Möglichkeit, Server dynamisch neu zu konfigurieren, wenn hohe Workloads im SQL-Datenbankdienst auftreten. Dieses dynamische Verhalten führt jedoch u.U. dazu, dass die Verbindung zwischen Ihrem Clientprogramm und der SQL-Datenbank getrennt wird. Diese Art Fehlerbedingung wird als *vorübergehender Fehler* bezeichnet.

Wenn das Clientprogramm über Wiederholungslogik verfügt, kann es versuchen, erneut eine Verbindung herzustellen, nachdem der vorübergehende Fehler Zeit hatte, sich selbst zu korrigieren. Es wird empfohlen, dass vor dem ersten Wiederholungsversuch eine Verzögerungszeit von fünf Sekunden verwendet wird. Wiederholungsversuche nach weniger als fünf Sekunden können den Clouddienst überfordern. Für jeden nachfolgenden Wiederholungsversuch sollte die Verzögerung exponentiell steigen, bis zu einem Maximum von 60 Sekunden.

Vorübergehende Fehler machen sich in der Regel in Form einer der folgenden Fehlermeldungen von Ihren Clientprogrammen bemerkbar:

* Datenbank <DB-Name> auf <Azure-Instanz>-Server ist zurzeit nicht verfügbar. Wiederholen Sie den Verbindungsversuch später. Falls das Problem weiterhin besteht, wenden Sie sich an den Kundensupport und geben als Ablaufverfolgungs-ID der Sitzung die <Sitzungs-ID> an.
* Datenbank <DB-Name> auf <Azure-Instanz>-Server ist zurzeit nicht verfügbar. Wiederholen Sie den Verbindungsversuch später. Falls das Problem weiterhin besteht, wenden Sie sich an den Kundensupport und geben als Ablaufverfolgungs-ID der Sitzung die <Sitzungs-ID> an. (Microsoft SQL Server, Fehler: 40613)
* Eine vorhandene Verbindung wurde erzwungenermaßen vom Remotehost geschlossen.
* System.Data.Entity.Core.EntityCommandExecutionException: Fehler beim Ausführen der Befehlsdefinition. Details siehe innere Ausnahme. ---> System.Data.SqlClient.SqlException: Beim Empfangen von Ergebnissen vom Server ist ein Fehler auf Übertragungsebene aufgetreten. (Anbieter: Sitzungsanbieter, Fehler: 19 – Physische Verbindung kann nicht verwendet werden)

Codebeispiele zur Wiederholungslogik finden Sie unter:

* [Connection Libraries for SQL Database and SQL Server (Verbindungsbibliotheken für SQL-Datenbanken und SQL Server, in englischer Sprache)](sql-database-libraries.md)
* [Maßnahmen zum Behandeln von Verbindungsfehlern und vorübergehenden Fehlern in SQL-Datenbank](sql-database-connectivity-issues.md)

Eine Erörterung der *Sperrfrist* für Clients, die ADO.NET verwenden, finden Sie unter [SQL Server-Verbindungspooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### Fehlercodes für vorübergehende Fehler
Die folgenden Fehler sind vorübergehend, und in der Anwendungslogik sollte ein neuer Versuch unternommen werden.

| Fehlercode | Schweregrad | Beschreibung |
| ---:| ---:|:--- |
| 4060 |16 |Die von der Anmeldung angeforderte „%.&#x2a;ls“-Datenbank kann nicht geöffnet werden. Fehler bei der Anmeldung. |
| 40197 |17 |Dienstfehler beim Verarbeiten Ihrer Anforderung. Wiederholen Sie den Vorgang. Fehlercode %d.<br/><br/>Sie erhalten diesen Fehler, wenn der Dienst aufgrund von Software- oder Hardwareupgrades, Hardwarefehlern oder sonstigen Failoverproblemen ausgefallen ist. Der Fehlercode (%d), der in der Meldung zum Fehler 40197 enthalten ist, liefert weitere Informationen zur Art des aufgetretenen Fehlers oder Failovers. Beispiele für Fehlercodes, die in die Meldung zum Fehler 40197 eingebettet sind, lauten 40020, 40143, 40166 und 40540.<br/><br/>Wenn Sie erneut eine Verbindung mit Ihrem SQL-Datenbank-Server herstellen, werden Sie automatisch mit einer intakten Kopie Ihrer Datenbank verbunden. Ihre Anwendung muss den Fehler 40197 abfangen, den für die Problembehandlung in der Meldung enthaltenen Fehlercode (%d) protokollieren und versuchen, eine neue Verbindung mit SQL-Datenbank herzustellen, bis die Ressourcen verfügbar sind, damit Ihre Verbindung wiederhergestellt wird. |
| 40501 |20 |Der Dienst ist derzeit ausgelastet. Wiederholen Sie die Anforderung in 10 Sekunden. Vorgangs-ID: %ls. Code: %d.<br/><br/>*Hinweis:* Weitere Informationen finden Sie unter: <br/>• [Ressourceneinschränkungen für Azure SQL-Datenbanken](sql-database-resource-limits.md). |
| 40613 |17 |Die „%.&#x2a;ls“-Datenbank auf Server „%.&#x2a;ls“ ist zurzeit nicht verfügbar. Wiederholen Sie den Verbindungsversuch später. Falls das Problem weiterhin besteht, wenden Sie sich an den Kundensupport und geben als Ablaufverfolgungs-ID der Sitzung „%.&#x2a;ls“ an. |
| 49918 |16 |Anforderung kann nicht verarbeitet werden. Zum Verarbeiten der Anforderung sind nicht genügend Ressourcen vorhanden.<br/><br/>Der Dienst ist derzeit ausgelastet. Versuchen Sie die Anforderung später erneut. |
| 49919 |16 |Die Erstellung oder Aktualisierung der Anforderung kann nicht verarbeitet werden. Für das Abonnement „%ld“ werden derzeit zu viele Erstell- oder Aktualisierungsvorgänge ausgeführt.<br/><br/>Der Dienst ist mit der Verarbeitung mehrerer Erstell- oder Aktualisierungsvorgänge für Ihr Abonnement oder Ihren Server ausgelastet. Zur Ressourcenoptimierung werden Anforderungen derzeit blockiert. Fragen Sie [sys.dm\_operation\_status](https://msdn.microsoft.com/library/dn270022.aspx) auf ausstehende Vorgänge ab. Warten Sie, bis ausstehende Erstell- oder Aktualisierungsanforderungen abgeschlossen sind, oder löschen Sie eine Ihrer ausstehenden Anforderungen, und wiederholen Sie die Anforderung später. |
| 49920 |16 |Anforderung kann nicht verarbeitet werden. Für das Abonnement „%ld“ werden derzeit zu viele Vorgänge ausgeführt.<br/><br/>Der Dienst ist mit der Verarbeitung mehrerer Vorgänge für dieses Abonnement ausgelastet. Zur Ressourcenoptimierung werden Anforderungen derzeit blockiert. Fragen Sie [sys.dm\_operation\_status](https://msdn.microsoft.com/library/dn270022.aspx) auf den Vorgangsstatus ab. Warten Sie, bis ausstehende Anforderungen abgeschlossen sind, oder löschen Sie eine Ihrer ausstehenden Anforderungen, und wiederholen Sie die Anforderung später. |

## Fehler beim Kopieren von Datenbanken
Die folgenden Fehler können beim Kopieren einer Datenbank in Azure SQL-Datenbank auftreten. Weitere Informationen finden Sie unter [Kopieren einer Azure SQL-Datenbank](sql-database-copy.md).

| Fehlercode | Schweregrad | Beschreibung |
| ---:| ---:|:--- |
| 40635 |16 |Der Client mit der IP-Adresse „%.&#x2a;ls“ ist vorübergehend deaktiviert. |
| 40637 |16 |Das Kopieren von Datenbanken ist derzeit deaktiviert. |
| 40561 |16 |Fehler beim Kopieren der Datenbank. Die Quell- oder die Zieldatenbank ist nicht vorhanden. |
| 40562 |16 |Fehler beim Kopieren der Datenbank. Die Quelldatenbank wurde gelöscht. |
| 40563 |16 |Fehler beim Kopieren der Datenbank. Die Zieldatenbank wurde gelöscht. |
| 40564 |16 |Interner Fehler beim Kopieren der Datenbank. Löschen Sie die Zieldatenbank, und wiederholen Sie den Vorgang. |
| 40565 |16 |Fehler beim Kopieren der Datenbank. Es darf jeweils nur eine Datenbankkopie von derselben Quelle erstellt werden. Löschen Sie die Zieldatenbank, und versuchen Sie es später erneut. |
| 40566 |16 |Interner Fehler beim Kopieren der Datenbank. Löschen Sie die Zieldatenbank, und wiederholen Sie den Vorgang. |
| 40567 |16 |Interner Fehler beim Kopieren der Datenbank. Löschen Sie die Zieldatenbank, und wiederholen Sie den Vorgang. |
| 40568 |16 |Fehler beim Kopieren der Datenbank. Die Quelldatenbank ist nicht mehr verfügbar. Löschen Sie die Zieldatenbank, und wiederholen Sie den Vorgang. |
| 40569 |16 |Fehler beim Kopieren der Datenbank. Die Zieldatenbank ist nicht mehr verfügbar. Löschen Sie die Zieldatenbank, und wiederholen Sie den Vorgang. |
| 40570 |16 |Interner Fehler beim Kopieren der Datenbank. Löschen Sie die Zieldatenbank, und versuchen Sie es später erneut. |
| 40571 |16 |Interner Fehler beim Kopieren der Datenbank. Löschen Sie die Zieldatenbank, und versuchen Sie es später erneut. |

## Fehler bei der Ressourcenkontrolle
Die folgenden Fehler treten beim Arbeiten mit Azure SQL-Datenbank bei der übermäßigen Nutzung von Ressourcen auf. Beispiel:

* Eine Transaktion war zu lange geöffnet.
* Eine Transaktion enthält zu viele Sperren.
* Eine Anwendung verbraucht zu viel Arbeitsspeicher.
* Eine Anwendung verbraucht zu viel `TempDb`-Speicher.

Verwandte Themen:

* Ausführlichere Informationen finden Sie hier: [Ressourceneinschränkungen für Azure SQL-Datenbanken](sql-database-resource-limits.md).

| Fehlercode | Schweregrad | Beschreibung |
| ---:| ---:|:--- |
| 10928 |20 |Ressourcen-ID: %d. Das %s-Limit für die Datenbank beträgt %d und wurde erreicht. Weitere Informationen finden Sie unter [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>Mit der Ressourcen-ID wird die Ressource angegeben, für die das Limit erreicht wurde. Bei Arbeitsthreads lautet die Ressourcen-ID „1“. Bei Sitzungen lautet die Ressourcen-ID „2“.<br/><br/>*Hinweis:* Weitere Informationen zu diesem Fehler und zu seiner Behebung finden Sie unter: <br/>• [Ressourceneinschränkungen für Azure SQL-Datenbanken](sql-database-resource-limits.md). |
| 10929 |20 |Ressourcen-ID: %d. Die %s-Mindestgarantie beträgt %d, der maximale Wert beträgt %d und die aktuelle Nutzung für die Datenbank beträgt %d. Der Server ist jedoch derzeit zu stark ausgelastet, um Anforderungen über %d für diese Datenbank zu unterstützen. Weitere Informationen finden Sie unter [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). Versuchen Sie es andernfalls später noch einmal.<br/><br/>Mit der Ressourcen-ID wird die Ressource angegeben, für die das Limit erreicht wurde. Bei Arbeitsthreads lautet die Ressourcen-ID „1“. Bei Sitzungen lautet die Ressourcen-ID „2“.<br/><br/>*Hinweis:* Weitere Informationen zu diesem Fehler und zu seiner Behebung finden Sie unter: <br/>• [Ressourceneinschränkungen für Azure SQL-Datenbanken](sql-database-resource-limits.md). |
| 40544 |20 |Das Datenbankkontingent wurde erreicht. Partitionieren oder löschen Sie Daten, löschen Sie Indizes, oder informieren Sie sich in der Dokumentation über mögliche Lösungen. |
| 40549 |16 |Die Sitzung wird aufgrund einer Transaktion mit langer Laufzeit beendet. Verkürzen Sie die Transaktion. |
| 40550 |16 |Die Sitzung wurde beendet, da zu viele Sperren abgerufen wurden. Reduzieren Sie die Anzahl der in einer einzelnen Transaktion gelesenen oder geänderten Zeilen. |
| 40551 |16 |Die Sitzung wurde aufgrund übermäßiger `TEMPDB`-Auslastung beendet. Ändern Sie die Abfrage, um die Nutzung des temporären Tabellenbereichs zu verringern.<br/><br/>*Tipp:* Wenn Sie temporäre Objekte verwenden, können Sie Speicherplatz in der `TEMPDB`-Datenbank sparen, indem Sie die temporären Objekte verwerfen, die von der Sitzung nicht mehr benötigt werden. |
| 40552 |16 |Die Sitzung wurde aufgrund übermäßiger Verwendung des Speicherplatzes für das Transaktionsprotokoll beendet. Reduzieren Sie die Anzahl der in einer einzelnen Transaktion geänderten Zeilen.<br/><br/>*Tipp:* Versuchen Sie beim Durchführen von Masseneinfügungen mit dem `bcp.exe`-Hilfsprogramm oder der `System.Data.SqlClient.SqlBulkCopy`-Klasse, die Option `-b batchsize` oder `BatchSize` zu verwenden, um die Anzahl von Zeilen zu beschränken, die bei jeder Transaktion auf den Server kopiert werden. Versuchen Sie es mit der Option `REBUILD WITH ONLINE = ON`, wenn Sie einen Index mit der `ALTER INDEX`-Anweisung neu erstellen. |
| 40553 |16 |Die Sitzung wurde aufgrund übermäßiger Speicherauslastung beendet. Ändern Sie die Abfrage, damit weniger Zeilen verarbeitet werden. <br/><br/>*Tipp:* Wenn Sie die Anzahl von `ORDER BY`- und `GROUP BY`-Vorgängen im Transact-SQL-Code reduzieren, verringern sich auch die Arbeitsspeicheranforderungen Ihrer Abfrage. |

## Fehler im Zusammenhang mit elastischen Pools
Die folgenden Fehler beziehen sich auf die Erstellung und Verwendung elastischer Pools.

| ErrorNumber | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
|:--- |:--- |:--- |:--- |:--- |:--- |
| 1132 |EX\_RESOURCE |Der Speichergrenzwert des elastischen Pools wurde erreicht. Die Speicherauslastung für der elastischen Pool darf (%d) MB nicht überschreiten. |Speicherplatzbeschränkung des elastischen Pools in MB. |Es wurde versucht, Daten in eine Datenbank zu schreiben, während die Speicherbegrenzung des elastischen Pools erreicht wurde. |Erhöhen Sie nach Möglichkeit die DTUs des elastischen Pools, um die Speicherkapazität zu erweitern, reduzieren Sie die Speichernutzung der einzelnen Datenbanken innerhalb des elastischen Pools, oder entfernen Sie Datenbanken aus dem elastischen Pool. |
| 10929 |EX\_USER |Die %s-Mindestgarantie beträgt %d, der maximale Wert beträgt %d und die aktuelle Nutzung für die Datenbank beträgt %d. Der Server ist jedoch derzeit zu stark ausgelastet, um Anforderungen über %d für diese Datenbank zu unterstützen. Weitere Informationen dazu finden Sie unter [Ressourcenkontrolle für Azure SQL-Datenbank](http://go.microsoft.com/fwlink/?LinkId=267637). Bitte versuchen Sie es andernfalls später noch einmal. |DTU-Minimum pro Datenbank; DTU-Maximum pro Datenbank |Die Gesamtanzahl der gleichzeitigen Worker (Anforderungen) in allen Datenbanken im elastischen Pool hat versucht, den Poolgrenzwert zu überschreiten. |Bitte erhöhen Sie nach Möglichkeit die DTU-Anzahl des elastischen Pools, um die Begrenzung für Worker zu steigern, oder entfernen Sie Datenbanken aus dem elastischen Pool. |
| 40844 |EX\_USER |Datenbank "%Ls" auf Server "%ls" ist eine Datenbank der "%ls"-Edition in einem elastischen Pool und kann nicht über eine fortlaufende Kopierbeziehung verfügen. |Datenbankname, Datenbank-Edition, Servername |Ein StartDatabaseCopy-Befehl wurde für eine Nicht-Premium-Datenbank in einem elastischen Pool ausgegeben. |In Kürze verfügbar |
| 40857 |EX\_USER |Der elastische Pool wurde für folgenden Server nicht gefunden: "%ls", Name des elastischen Pools: "%ls". |Name des Servers; Name des elastisches Pools |Der angegebene elastische Pool ist nicht auf dem angegebenen Server vorhanden. |Geben Sie einen gültigen Namen für den elastischen Pool an. |
| 40858 |EX\_USER |Der elastische Pool "%ls" ist bereits auf folgendem Server vorhanden: "%ls" |Name des elastischen Pools, Servername |Der angegebene elastische Pool existiert bereits auf dem angegebenen logischen Server. |Geben Sie einen neuen Namen für den elastischen Pool an. |
| 40859 |EX\_USER |Der elastische Pool unterstützt Dienstebene "%ls" nicht. |Dienstebene des elastischen Pools |Die angegebene Dienstebene wird für die Bereitstellung elastischer Pools nicht unterstützt. |Geben Sie die richtige Edition an, oder lassen Sie die Dienstebene leer, um die Standarddienstebene zu verwenden. |
| 40860 |EX\_USER |Die Kombination aus elastischem Pool "%ls" und Dienstziel "%ls" ist ungültig. |Name des elastischen Pools; Name des Dienstebenenziels |Der elastische Pool und das Dienstziel können gemeinsam angegeben werden, wenn das Dienstziel als "ElasticPool" angegeben wird. |Geben Sie die richtige Kombination aus elastischem Pool und Dienstziel an. |
| 40861 |EX\_USER |Die Datenbankedition „%.*ls“ darf sich nicht von der Dienstebene des elastischen Pools („%.*ls“) unterscheiden. |Datenbankedition, Dienstebene des elastischen Pools |Die Datenbankedition unterscheidet sich von der Dienstebene des elastischen Pools. |Geben Sie keine Datenbankedition an, die sich von der Dienstebene des elastischen Pools unterscheidet. Beachten Sie, dass die Datenbankedition nicht angegeben werden muss. |
| 40862 |EX\_USER |Der Name des elastischen Pools muss angegeben werden, wenn das Dienstziel des elastischen Pools angegeben wurde. |Keine |Das Dienstziel des elastischen Pools identifiziert einen elastischen Pool nicht eindeutig. |Geben Sie den Namen des elastischen Pools an, wenn Sie das Dienstziel des elastischen Pools verwenden. |
| 40864 |EX\_USER |Die DTU-Anzahl für den elastischen Pool muss mindestens (%d) DTUs für die Dienstebene "%.*ls" betragen. |DTUs für elastischen Pool; Dienstebene des elastischen Pools. |Es wurde versucht, eine DTU-Anzahl für den elastischen Pool unterhalb des unteren Grenzwerts festzulegen. |Legen Sie die DTU-Einstellung für den elastischen Pool mindestens auf die Untergrenze fest. |
| 40865 |EX\_USER |Die DTU-Anzahl für den elastischen Pool darf höchstens (%d) DTUs für die Dienstebene "%.*ls" betragen. |DTUs für elastischen Pool; Dienstebene des elastischen Pools. |Es wurde versucht, eine DTU-Anzahl für den elastischen Pool oberhalb des oberen Grenzwerts festzulegen. |Legen Sie die DTU-Einstellung für den elastischen Pool höchstens auf die Obergrenze fest. |
| 40867 |EX\_USER |Die maximalen DTUs pro Datenbank müssen mindestens (%d) für Dienstebene "%.*ls" betragen. |Maximale DTUs pro Datenbank; Dienstebene des elastischen Pools |Es wurde versucht, eine maximale DTU-Anzahl pro Datenbank festzulegen, die unter der unterstützten Grenze liegt. |Sie sollten in Betracht ziehen, die Dienstebene des elastischen Pools zu verwenden, die die gewünschte Einstellung unterstützt. |
| 40868 |EX\_USER |Die maximale DTU-Anzahl pro Datenbank kann nicht mehr als (%d) für Dienstebene "%.*ls" betragen. |Maximale DTUs pro Datenbank; Dienstebene des elastischen Pools. |Es wurde versucht, eine maximale DTU-Anzahl pro Datenbank festzulegen, die über der unterstützten Grenze liegt. |Sie sollten in Betracht ziehen, die Dienstebene des elastischen Pools zu verwenden, die die gewünschte Einstellung unterstützt. |
| 40870 |EX\_USER |Die minimale DTU-Anzahl pro Datenbank kann nicht mehr als (%d) für Dienstebene "%.*ls" betragen. |Minimale DTUs pro Datenbank; Dienstebene des elastischen Pools. |Es wurde versucht, eine minimale DTU-Anzahl pro Datenbank festzulegen, die über der unterstützten Grenze liegt. |Sie sollten in Betracht ziehen, die Dienstebene des elastischen Pools zu verwenden, die die gewünschte Einstellung unterstützt. |
| 40873 |EX\_USER |Die Anzahl der Datenbanken (%d) und minimalen DTUs pro Datenbank (%d) darf die DTU-Anzahl des elastischen Pools (%d) nicht überschreiten. |Anzahl der Datenbanken im elastischen Pool; minimale DTUs pro Datenbank; DTUs im elastischen Pool. |Es wurde versucht, eine Mindestanzahl von DTUs für den elastischen Pool anzugeben, die die DTU-Anzahl des elastischen Pools überschreitet. |Sie sollten in Betracht ziehen, die Anzahl der DTUs im elastischen Pool zu erhöhen oder die Mindestanzahl der DTUs pro Datenbank zu verringern, oder Sie verringern die Anzahl der Datenbanken im elastischen Pool. |
| 40877 |EX\_USER |Ein elastischer Pool kann nur gelöscht werden, wenn er keine Datenbanken enthält. |Keine |Der elastische Pool enthält eine oder mehrere Datenbanken und kann nicht gelöscht werden. |Entfernen Sie Datenbanken aus dem elastischen Pool, um ihn zu löschen. |
| 40881 |EX\_USER |Der elastische Pool "%.*ls" hat den Grenzwert für die Anzahl an Datenbanken erreicht. Der Grenzwert für die Datenbankanzahl im elastischen Pool darf (%d) für einen elastischen Pool mit (%d) DTUs nicht überschreiten. |Name des elastischen Pools; Grenzwert für die Datenbankanzahl im elastischen Pool; DTUs für Ressourcenpool. |Es wurde versucht, eine Datenbank zu erstellen oder zum elastischen Pool hinzuzufügen, während der Grenzwert für die Datenbankanzahl des elastischen Pools erreicht wurde. |Bitte erhöhen Sie nach Möglichkeit die DTU-Anzahl des elastischen Pools, um die Begrenzung für Datenbanken zu steigern, oder entfernen Sie Datenbanken aus dem elastischen Pool. |
| 40889 |EX\_USER |Die Begrenzung für DTUs oder Speicher für den elastischen Pool "%.*ls" kann nicht verkleinert werden, da nicht genügend Speicherplatz für die Datenbanken verfügbar wäre. |Name der elastischen Pools. |Es wurde versucht, die Speicherbegrenzung des elastischen Pools unter die Speicherauslastung zu verringern. |Reduzieren Sie die Speicherauslastung der einzelnen Datenbanken im elastischen Pool, oder entfernen Sie Datenbanken aus dem Pool, um die DTUs oder Speicherbegrenzung zu verringern. |
| 40891 |EX\_USER |Die Mindestanzahl von DTUs pro Datenbank (%d) darf die Höchstanzahl von DTUs pro Datenbank (%d) nicht überschreiten. |DTU-Mindestanzahl pro Datenbank; DTU-Höchstanzahl pro Datenbank. |Es wurde versucht, die DTU-Mindestanzahl pro Datenbank höher festzulegen, als die maximale DTU-Anzahl pro Datenbank. |Stellen Sie sicher, dass die Mindestanzahl von DTUs pro Datenbank nicht die Höchstanzahl von DTUs pro Datenbank überschreitet. |
| TBD |EX\_USER |Die Speichergröße für eine einzelne Datenbank in einem elastischen Pool darf die maximal zulässige Größe für die Dienstebene des elastischen Pools "%.*ls" nicht überschreiten. |Dienstebene des elastischen Pools |Die maximale Größe der Datenbank überschreitet die maximale Größe, die von der Dienstebene des elastische Pools zugelassen wird. |Legen Sie die maximale Größe der Datenbank höchstens auf die maximal zulässige Größe der Dienstebene des elastische Pools fest. |

Verwandte Themen:

* [Erstellen eines Pools für elastische Datenbanken (C#)](sql-database-elastic-pool-create-csharp.md)
* [Verwalten eines Pools für elastische Datenbanken (C#)](sql-database-elastic-pool-manage-csharp.md)
* [Erstellen eines Pools für elastische Datenbanken (PowerShell)](sql-database-elastic-pool-create-powershell.md)
* [Überwachen und Verwalten eines Pools für elastische Datenbanken (PowerShell)](sql-database-elastic-pool-manage-powershell.md)

## Allgemeine Fehler
Die folgenden Fehler fallen in keine der vorherigen Kategorien.

| Fehlercode | Schweregrad | Beschreibung |
| ---:| ---:|:--- |
| 15006 |16 |<Administratoranmeldung> ist kein gültiger Name, da er ungültige Zeichen enthält. |
| 18452 |14 |Anmeldefehler. Die Anmeldung stammt aus einer nicht vertrauenswürdigen Domäne und kann mit der Windows-Authentifizierung nicht verwendet werden.%.&#x2a;ls (Windows-Anmeldungen werden in dieser SQL Server-Version nicht unterstützt.) |
| 18456 |14 |Fehler bei der Anmeldung für den Benutzer „%.&#x2a;ls“.%.&#x2a;ls%.&#x2a;ls(Fehler bei der Anmeldung für den Benutzer „%.&#x2a;ls“. Fehler bei der Kennwortänderung. Kennwortänderungen während der Anmeldung werden in dieser SQL Server-Version nicht unterstützt.) |
| 18470 |14 |Fehler bei der Anmeldung für den Benutzer „%.&#x2a;ls“. Ursache: Das Konto ist deaktiviert.%.&#x2a;ls |
| 40014 |16 |Es können nicht mehrere Datenbanken in derselben Transaktion verwendet werden. |
| 40054 |16 |Tabellen ohne gruppierten Index werden in dieser SQL Server-Version nicht unterstützt. Erstellen Sie einen gruppierten Index, und wiederholen Sie den Vorgang. |
| 40133 |15 |Dieser Vorgang wird in dieser SQL Server-Version nicht unterstützt. |
| 40506 |16 |Die angegebene SID ist für diese SQL Server-Version ungültig. |
| 40507 |16 |„%.&#x2a;ls“ kann in dieser SQL Server-Version nicht mit Parametern aufgerufen werden. |
| 40508 |16 |Die USE-Anweisung wird zum Wechseln zwischen Datenbanken nicht unterstützt. Verwenden Sie eine neue Verbindung, um eine Verbindung mit einer anderen Datenbank herzustellen. |
| 40510 |16 |Die „%.&#x2a;ls“-Anweisung wird in dieser SQL Server-Version nicht unterstützt. |
| 40511 |16 |Die integrierte „%.&#x2a;ls“-Funktion wird in dieser SQL Server-Version nicht unterstützt. |
| 40512 |16 |Die als veraltet markierte „%ls“-Funktion wird in dieser SQL Server-Version nicht unterstützt. |
| 40513 |16 |Die „%.&#x2a;ls“-Servervariable wird in dieser SQL Server-Version nicht unterstützt. |
| 40514 |16 |„%ls“ wird in dieser SQL Server-Version nicht unterstützt. |
| 40515 |16 |In dieser SQL Server-Version werden keine Verweise auf eine Datenbank und/oder einen Servernamen in „%.&#x2a;ls“ unterstützt. |
| 40516 |16 |Globale temporäre Objekte werden in dieser SQL Server-Version nicht unterstützt. |
| 40517 |16 |Die Schlüsselwort- oder Anweisungsoption „%.&#x2a;ls“ wird in dieser SQL Server-Version nicht unterstützt. |
| 40518 |16 |Der DBCC-Befehl „%.&#x2a;ls“ wird in dieser SQL Server-Version nicht unterstützt. |
| 40520 |16 |Die sicherungsfähige Klasse „%S\_MSG“ wird in dieser SQL Server-Version nicht unterstützt. |
| 40521 |16 |Die sicherungsfähige Klasse „%S\_MSG“ wird in dieser SQL Server-Version im Serverbereich nicht unterstützt. |
| 40522 |16 |Der „%.&#x2a;ls“-Datenbankprinzipaltyp wird in dieser SQL Server-Version nicht unterstützt. |
| 40523 |16 |Das implizite Erstellen des Benutzers „%.&#x2a;ls“ wird in dieser SQL Server-Version nicht unterstützt. Erstellen Sie den Benutzer vor der Verwendung explizit. |
| 40524 |16 |Der „%.&#x2a;ls“-Datentyp wird in dieser SQL Server-Version nicht unterstützt. |
| 40525 |16 |WITH „%.ls“ wird in dieser SQL Server-Version nicht unterstützt. |
| 40526 |16 |Der Rowsetanbieter „%.&#x2a;ls“ wird in dieser SQL Server-Version nicht unterstützt. |
| 40527 |16 |Verbindungsserver werden in dieser SQL Server-Version nicht unterstützt. |
| 40528 |16 |In dieser SQL Server-Version können Benutzer keinen Zertifikaten, asymmetrischen Schlüsseln oder Windows-Anmeldungen zugeordnet werden. |
| 40529 |16 |Die integrierte „%.&#x2a;ls“-Funktion wird in dieser SQL Server-Version im Identitätswechselkontext nicht unterstützt. |
| 40532 |11 |Der bei der Anmeldung angeforderte Server „%.&#x2a;ls“ kann nicht geöffnet werden. Fehler bei der Anmeldung. |
| 40553 |16 |Die Sitzung wurde aufgrund übermäßiger Speicherauslastung beendet. Ändern Sie die Abfrage, damit weniger Zeilen verarbeitet werden.<br/><br/>*Hinweis:* Wenn Sie die Anzahl von `ORDER BY`- und `GROUP BY`-Vorgängen im Transact-SQL-Code reduzieren, verringern sich auch die Arbeitsspeicheranforderungen Ihrer Abfrage. |
| 40604 |16 |CREATE/ALTER DATABASE war nicht möglich, da das Serverkontingent überschritten würde. |
| 40606 |16 |Das Anfügen von Datenbanken wird in dieser SQL Server-Version nicht unterstützt. |
| 40607 |16 |Windows-Anmeldungen werden in dieser SQL Server-Version nicht unterstützt. |
| 40611 |16 |Für Server können maximal 128 Firewallregeln definiert werden. |
| 40614 |16 |Die IP-Endadresse der Firewallregel darf nicht vor der IP-Startadresse liegen. |
| 40615 |16 |Der bei der Anmeldung angeforderte Server „{0}“ kann nicht geöffnet werden. Der Client mit der IP-Adresse „{1}“ hat keine Zugriffsberechtigung für den Server. Aktivieren Sie den Zugriff über das SQL Azure-Portal, oder indem Sie „sp\_set\_firewall\_rule“ für die master-Datenbank ausführen, um eine Firewallregel für diese IP-Adresse bzw. diesen IP-Adressbereich zu erstellen. Es kann bis zu fünf Minuten dauert, bis diese Änderung wirksam wird. |
| 40617 |16 |Der mit <Regelname> beginnende Name der Firewallregel ist zu lang. Die maximale Länge beträgt 128. |
| 40618 |16 |Der Name für die Firewallregel darf nicht leer sein. |
| 40620 |16 |Fehler bei der Anmeldung für den Benutzer „%.&#x2a;ls“. Fehler bei der Kennwortänderung. Kennwortänderungen während der Anmeldung werden in dieser SQL Server-Version nicht unterstützt. |
| 40627 |20 |Der Vorgang für Server „{0}“ und die „{1}“-Datenbank wird ausgeführt. Warten Sie vor dem Wiederholen des Vorgangs einige Minuten. |
| 40630 |16 |Fehler bei der Kennwortüberprüfung. Das Kennwort ist zu kurz und erfüllt daher nicht die Richtlinienanforderungen. |
| 40631 |16 |Das Kennwort, das Sie angegeben haben, ist zu lang. Das Kennwort sollte nicht mehr als 128 Zeichen aufweisen. |
| 40632 |16 |Fehler bei der Kennwortüberprüfung. Das Kennwort ist nicht komplex genug und erfüllt daher nicht die Richtlinienanforderungen. |
| 40636 |16 |Der reservierte Datenbankname „%.&#x2a;ls“ kann in diesem Vorgang nicht verwendet werden. |
| 40638 |16 |Ungültige Abonnement-ID <Abonnement-ID>. Das Abonnement ist nicht vorhanden. |
| 40639 |16 |Anforderung entspricht nicht dem Schema: <Schemafehler>. |
| 40640 |20 |Der Server hat eine unerwartete Ausnahme erkannt. |
| 40641 |16 |Der angegebene Pfad ist ungültig. |
| 40642 |17 |Der Server ist derzeit überlastet. Bitte versuchen Sie es später erneut. |
| 40643 |16 |Der angegebene Headerwert „x-ms-version“ ist ungültig. |
| 40644 |14 |Fehler beim Autorisieren des Zugriffs auf das angegebene Abonnement. |
| 40645 |16 |Servername <Servername> darf nicht leer oder NULL sein. Er darf nur aus den Kleinbuchstaben „a“ - „z“, den Zahlen 0 - 9 und Bindestrichen bestehen. Der Bindestrich darf nicht am Anfang oder Ende des Namens stehen. |
| 40646 |16 |Die Abonnement-ID darf nicht leer sein. |
| 40647 |16 |Der Server <Servername> ist im <Abonnement-ID>-Abonnement nicht enthalten. |
| 40648 |17 |Zu viele Anforderungen wurden durchgeführt. Versuchen Sie es später erneut. |
| 40649 |16 |Es wurde ein ungültiger „content-type“ angegeben. Nur „application“ oder „xml“ wird unterstützt. |
| 40650 |16 |Abonnement <Abonnement-ID> ist nicht vorhanden oder nicht betriebsbereit. |
| 40651 |16 |Fehler beim Erstellen des Servers, weil das Abonnement <Abonnement-ID> deaktiviert ist. |
| 40652 |16 |Der Server kann nicht verschoben oder erstellt werden, Abonnement <Abonnement-ID> überschreitet das Serverkontingent. |
| 40671 |17 |Kommunikationsfehler zwischen dem Gateway und dem Verwaltungsdienst. Versuchen Sie es später erneut. |
| 40852 |16 |Die von der Anmeldung angeforderte Datenbank „%.*ls“ auf Server „%.*ls“ kann nicht geöffnet werden. Der Zugriff auf die Datenbank ist nur mit einer Verbindungszeichenfolge mit aktivierter Sicherheit zulässig. Um auf diese Datenbank zuzugreifen, ändern Sie die Verbindungszeichenfolgen so, dass der Server-FQDN „secure“ enthält – „<Servername>.database.windows.net“ muss in „<Servername>.database.`secure`.windows.net“ geändert werden. |
| 45168 |16 |Das SQL Azure-System ist ausgelastet und richtet eine Obergrenze bei den gleichzeitigen DB CRUD-Vorgängen für einen Server ein (z. B. CREATE DATABASE). Für den in der Fehlermeldung angegebenen Server wurde die maximale Anzahl von gleichzeitigen Verbindungen überschritten. Versuchen Sie es später erneut. |
| 45169 |16 |Das SQL Azure-System ist ausgelastet und richtet eine Obergrenze bei der Anzahl gleichzeitiger CRUD-Servervorgänge für ein Abonnement ein (z. B.CREATE SERVER). Für das in der Fehlermeldung angegebene Abonnement wurde die maximale Anzahl von gleichzeitigen Verbindungen überschritten, und die Anforderung wurde verweigert. Versuchen Sie es später erneut. |

## Verwandte Links
* [Azure SQL-Datenbanken – Allgemeine Einschränkungen und Leitlinien](sql-database-general-limitations.md)
* [Ressourceneinschränkungen für Azure SQL-Datenbanken](sql-database-resource-limits.md)

<!---HONumber=AcomDC_0803_2016-->