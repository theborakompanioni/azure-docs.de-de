<properties
	pageTitle="Aktivieren von Stretch-Datenbank für eine Tabelle | Microsoft Azure"
	description="Erfahren Sie, wie Sie eine Tabelle für Stretch-Datenbank konfigurieren."
	services="sql-server-stretch-database"
	documentationCenter=""
	authors="douglaslMS"
	manager=""
	editor=""/>

<tags
	ms.service="sql-server-stretch-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/17/2016"
	ms.author="douglasl"/>

# Aktivieren von Stretch-Datenbank für eine Tabelle

Um eine Tabelle für Stretch-Datenbank zu konfigurieren, wählen Sie **Stretch | Aktivieren** für eine Tabelle in SQL Server Management Studio, um den Assistenten zum **Aktivieren einer Tabelle für Stretch** zu öffnen. Sie können Transact-SQL auch verwenden, um Stretch-Datenbank für eine vorhandene Tabelle zu aktivieren, oder um eine neue Tabelle zu erstellen, in der Stretch-Datenbank aktiviert ist.

-   Wenn Sie Ihre historischen Daten in einer separaten Tabelle speichern, können Sie die ganze Tabelle migrieren.

-   Wenn die Tabelle alte und aktuelle Daten enthält, können Sie ein Filterprädikat zum Auswählen der Zeilen zum Migrieren angeben.

**Voraussetzungen** Wenn Sie **Stretch | Aktivieren** für eine Tabelle auswählen, Stretch-Datenbank bisher jedoch noch nicht für die Datenbank aktiviert war, konfiguriert der Assistent zuerst die Datenbank für Stretch-Datenbank. Führen Sie anstelle der Schritte in diesem Thema die Schritte unter [Erste Schritte mit dem Assistenten zum Aktivieren einer Datenbank für Stretch](sql-server-stretch-database-wizard.md) aus.

**Berechtigungen** Für das Aktivieren von Stretch-Datenbank für eine Datenbank oder eine Tabelle sind db\_owner-Berechtigungen erforderlich. Zum Aktivieren von Stretch-Datenbank für eine Tabelle sind auch ALTER-Berechtigungen für die Tabelle erforderlich.

## <a name="EnableWizardTable"></a>Verwenden des Assistenten zum Aktivieren von Stretch-Datenbank für eine Tabelle
**Assistenten starten**

1.  Wählen Sie in SQL Server Management Studio im Objekt-Explorer die Tabelle aus, für die Stretch aktiviert werden soll.

2.  Klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Stretch** aus. Wählen Sie dann **Aktivieren** aus, um den Assistenten zu starten.

**Einführung**

Überprüfen Sie den Zweck des Assistenten und die Voraussetzungen.

**Datenbanktabellen auswählen**

Bestätigen Sie, dass die Tabelle, die Sie aktivieren möchten, angezeigt wird und ausgewählt ist.

Sie können eine ganze Tabelle migrieren oder ein einfaches Filterprädikat im Assistenten festlegen. Führen Sie einen der folgenden Schritte aus, wenn Sie eine andere Art von Filterprädikat für die Auswahl der zu migrierenden Zeilen verwenden möchten.

-   Beenden Sie den Assistenten, und führen Sie die ALTER TABLE-Anweisung aus, um Stretch für die Tabelle zu aktivieren und ein Prädikat anzugeben.

-   Führen Sie die ALTER TABLE-Anweisung aus, um ein Prädikat anzugeben, nachdem Sie den Assistenten beendet haben. Die erforderlichen Schritte finden Sie unter [Hinzufügen eines Filterprädikats nach dem Ausführen des Assistenten](sql-server-stretch-database-predicate-function.md#addafterwiz).

Die ALTER TABLE-Syntax wird weiter unten in diesem Thema beschrieben.

**Zusammenfassung**

Überprüfen Sie die Werte, die Sie eingegeben haben, und die im Assistenten ausgewählten Optionen. Wählen Sie dann **Fertig stellen**, um Stretch zu aktivieren.

**Ergebnisse**

Überprüfen Sie die Ergebnisse.

## <a name="EnableTSQLTable"></a>Verwenden von Transact-SQL zum Aktivieren von Stretch-Datenbank für eine Tabelle
Sie können Stretch-Datenbank mithilfe von Transact-SQL für eine vorhandene Tabelle aktivieren oder eine neue Tabelle erstellen, in der Stretch-Datenbank aktiviert ist.

### Optionen
Verwenden Sie die folgenden Optionen beim Ausführen von CREATE TABLE oder ALTER TABLE, um Stretch-Datenbank für eine Tabelle zu aktivieren.

-   Geben Sie optional über die `FILTER_PREDICATE = <predicate>`-Klausel ein Prädikat an, mit dem Sie die zu migrierenden Zeilen auswählen können, wenn die Tabelle sowohl historische als auch aktuelle Daten enthält. Das Prädikat muss eine Inline-Tabellenwertfunktion aufrufen. Weitere Informationen finden Sie unter [Auswählen von Zeilen für die Migration mit einem Filterprädikat](sql-server-stretch-database-predicate-function.md). Wenn Sie kein Filterprädikat angeben, wird die gesamte Tabelle migriert.

    >   [AZURE.NOTE] Falls Sie ein Filterprädikat mit schlechter Leistung angeben, wird die Datenmigration ebenfalls mit schlechter Leistung durchgeführt. Stretch-Datenbank wendet das Filterprädikat mithilfe des CROSS APPLY-Operators auf die Tabelle an.

-   Geben Sie `MIGRATION_STATE = OUTBOUND` an, um die Datenmigration sofort zu starten, oder `MIGRATION_STATE = PAUSED`, um den Start der Datenmigration zu verschieben.

### Aktivieren von Stretch-Datenbank für eine vorhandene Tabelle
Um eine vorhandene Tabelle für Stretch-Datenbank zu konfigurieren, führen Sie den Befehl ALTER TABLE aus.

Hier ist ein Beispiel, bei dem die gesamte Tabelle migriert wird und die Datenmigration unmittelbar beginnt.

```tsql
ALTER TABLE <table name>
    SET ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;
```
Es folgt ein Beispiel, in dem nur die durch die Inline-Tabellenwertfunktion `dbo.fn_stretchpredicate` markierten Zeilen migriert werden und der Start der Datenmigration verschoben wird. Weitere Informationen zum Filterprädikat finden Sie unter [Auswählen von Zeilen für die Migration mit einem Filterprädikat](sql-server-stretch-database-predicate-function.md).

```tsql
ALTER TABLE <table name>
    SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
        MIGRATION_STATE = PAUSED ) );
```

Weitere Informationen finden Sie unter [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).

### Erstellen einer neue Tabelle, für die Stretch-Datenbank aktiviert ist
Führen Sie zum Erstellen einer neuen Tabelle, für die Stretch-Datenbank aktiviert ist, den Befehl CREATE TABLE aus.

Hier ist ein Beispiel, bei dem die gesamte Tabelle migriert wird und die Datenmigration unmittelbar beginnt.

```tsql
CREATE TABLE <table name> ...
    WITH ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;
```
Es folgt ein Beispiel, in dem nur die durch die Inline-Tabellenwertfunktion `dbo.fn_stretchpredicate` markierten Zeilen migriert werden und der Start der Datenmigration verschoben wird. Weitere Informationen zum Filterprädikat finden Sie unter [Auswählen von Zeilen für die Migration mit einem Filterprädikat](sql-server-stretch-database-predicate-function.md).

```tsql
CREATE TABLE <table name> ...
    WITH ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
        MIGRATION_STATE = PAUSED ) );
```

Weitere Informationen finden Sie unter [CREATE TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms174979.aspx).


## Weitere Informationen

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)

[CREATE TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms174979.aspx)

<!---HONumber=AcomDC_0608_2016-->