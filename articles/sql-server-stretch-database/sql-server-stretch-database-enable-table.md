---
title: Aktivieren von Stretch-Datenbank für eine Tabelle | Microsoft Docs
description: Erfahren Sie, wie Sie eine Tabelle für Stretch-Datenbank konfigurieren.
services: sql-server-stretch-database
documentationcenter: ''
author: douglaslMS
manager: ''
editor: ''

ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/05/2016
ms.author: douglasl

---
# Aktivieren von Stretch-Datenbank für eine Tabelle
Um eine Tabelle für Stretch-Datenbank zu konfigurieren, wählen Sie **Stretch | Aktivieren** für eine Tabelle in SQL Server Management Studio, um den Assistenten zum **Aktivieren einer Tabelle für Stretch** zu öffnen. Sie können Transact-SQL auch verwenden, um Stretch-Datenbank für eine vorhandene Tabelle zu aktivieren, oder um eine neue Tabelle zu erstellen, in der Stretch-Datenbank aktiviert ist.

* Wenn Sie inaktive Daten in einer separaten Tabelle speichern, können Sie die ganze Tabelle migrieren.
* Wenn die Tabelle sowohl aktive als auch inaktive Daten enthält, können Sie eine Filterfunktion zum Auswählen der zu migrierenden Zeilen angeben.

**Voraussetzungen** Wenn Sie **Stretch | Aktivieren** für eine Tabelle auswählen, Stretch-Datenbank bisher jedoch noch nicht für die Datenbank aktiviert war, konfiguriert der Assistent zuerst die Datenbank für Stretch-Datenbank. Führen Sie anstelle der Schritte in diesem Thema die Schritte unter [Erste Schritte mit dem Assistenten zum Aktivieren einer Datenbank für Stretch](sql-server-stretch-database-wizard.md) aus.

**Berechtigungen** Für das Aktivieren von Stretch-Datenbank für eine Datenbank oder eine Tabelle sind db\_owner-Berechtigungen erforderlich. Zum Aktivieren von Stretch-Datenbank für eine Tabelle sind auch ALTER-Berechtigungen für die Tabelle erforderlich.

> [!NOTE]
> Bedenken Sie später beim Deaktivieren von Stretch-Datenbank Folgendes: Wenn Sie Stretch-Datenbank für eine Tabelle oder eine Datenbank deaktivieren, wird das Remoteobjekt nicht gelöscht. Wenn Sie die Remotetabelle oder die Remotedatenbank löschen möchten, müssen Sie sie mithilfe des Azure-Verwaltungsportals entfernen. Für die Remoteobjekte fallen weiterhin Azure-Kosten an, bis Sie die Objekte manuell löschen.
> 
> 

## <a name="EnableWizardTable"></a>Verwenden des Assistenten zum Aktivieren von Stretch-Datenbank für eine Tabelle
**Assistenten starten**

1. Wählen Sie in SQL Server Management Studio im Objekt-Explorer die Tabelle aus, für die Stretch aktiviert werden soll.
2. Klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Stretch** aus. Wählen Sie dann **Aktivieren**, um den Assistenten zu starten.

**Einführung**

Überprüfen Sie den Zweck des Assistenten und die Voraussetzungen.

**Datenbanktabellen auswählen**

Bestätigen Sie, dass die Tabelle, die Sie aktivieren möchten, angezeigt wird und ausgewählt ist.

Sie können eine ganze Tabelle migrieren oder eine einfache Filterfunktion im Assistenten festlegen. Führen Sie einen der folgenden Schritte aus, wenn Sie eine andere Art von Filterfunktion für die Auswahl der zu migrierenden Zeilen verwenden möchten.

* Beenden Sie den Assistenten, und führen Sie die ALTER TABLE-Anweisung aus, um Stretch für die Tabelle zu aktivieren und eine Filterfunktion anzugeben.
* Führen Sie die ALTER TABLE-Anweisung aus, um eine Filterfunktion anzugeben, nachdem Sie den Assistenten beendet haben. Die erforderlichen Schritte finden Sie unter [Hinzufügen einer Filterfunktion nach dem Ausführen des Assistenten](sql-server-stretch-database-predicate-function.md#addafterwiz).

Die ALTER TABLE-Syntax wird weiter unten in diesem Thema beschrieben.

**Zusammenfassung**

Überprüfen Sie die Werte, die Sie eingegeben haben, und die im Assistenten ausgewählten Optionen. Wählen Sie dann **Fertig stellen** aus, um Stretch zu aktivieren.

**Ergebnisse**

Überprüfen Sie die Ergebnisse.

## <a name="EnableTSQLTable"></a>Verwenden von Transact-SQL zum Aktivieren von Stretch-Datenbank für eine Tabelle
Sie können Stretch-Datenbank mithilfe von Transact-SQL für eine vorhandene Tabelle aktivieren oder eine neue Tabelle erstellen, in der Stretch-Datenbank aktiviert ist.

### Options
Verwenden Sie die folgenden Optionen beim Ausführen von CREATE TABLE oder ALTER TABLE, um Stretch-Datenbank für eine Tabelle zu aktivieren.

* Geben Sie optional über die `FILTER_PREDICATE = <function>`-Klausel eine Funktion an, mit der Sie die zu migrierenden Zeilen auswählen können, wenn die Tabelle sowohl aktive als auch inaktive Daten enthält. Das Prädikat muss eine Inline-Tabellenwertfunktion aufrufen. Weitere Informationen finden Sie unter [Auswählen von Zeilen für die Migration mit einer Filterfunktion](sql-server-stretch-database-predicate-function.md). Wenn Sie keine Filterfunktion angeben, wird die gesamte Tabelle migriert.
  
  > [!NOTE]
  > Falls Sie eine Filterfunktion mit schlechter Leistung angeben, wird die Datenmigration ebenfalls mit schlechter Leistung durchgeführt. Stretch-Datenbank wendet die Filterfunktion mithilfe des CROSS APPLY-Operators auf die Tabelle an.
  > 
  > 
* Geben Sie `MIGRATION_STATE = OUTBOUND` an, um die Datenmigration sofort zu starten, oder `MIGRATION_STATE = PAUSED`, um den Start der Datenmigration zu verschieben.

### Aktivieren von Stretch-Datenbank für eine vorhandene Tabelle
Um eine vorhandene Tabelle für Stretch-Datenbank zu konfigurieren, führen Sie den Befehl ALTER TABLE aus.

Hier ist ein Beispiel, bei dem die gesamte Tabelle migriert wird und die Datenmigration unmittelbar beginnt.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <table name>  
    SET ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;  
GO
```
Es folgt ein Beispiel, in dem nur die durch die Inline-Tabellenwertfunktion `dbo.fn_stretchpredicate` markierten Zeilen migriert werden und der Start der Datenmigration verschoben wird. Weitere Informationen zur Filterfunktion finden Sie unter [Auswählen von Zeilen für die Migration mit einer Filterfunktion](sql-server-stretch-database-predicate-function.md).

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <table name>  
    SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(),  
        MIGRATION_STATE = PAUSED ) ) ;  
 GO
```

Weitere Informationen finden Sie unter [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).

### Erstellen einer neue Tabelle, für die Stretch-Datenbank aktiviert ist
Führen Sie zum Erstellen einer neuen Tabelle, für die Stretch-Datenbank aktiviert ist, den Befehl CREATE TABLE aus.

Hier ist ein Beispiel, bei dem die gesamte Tabelle migriert wird und die Datenmigration unmittelbar beginnt.

```tsql
USE <Stretch-enabled database name>;
GO
CREATE TABLE <table name>
    ( ... )  
    WITH ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;  
GO
```

Es folgt ein Beispiel, in dem nur die durch die Inline-Tabellenwertfunktion `dbo.fn_stretchpredicate` markierten Zeilen migriert werden und der Start der Datenmigration verschoben wird. Weitere Informationen zur Filterfunktion finden Sie unter [Auswählen von Zeilen für die Migration mit einer Filterfunktion](sql-server-stretch-database-predicate-function.md).

```tsql
USE <Stretch-enabled database name>;
GO
CREATE TABLE <table name>
    ( ... )  
    WITH ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(),  
        MIGRATION_STATE = PAUSED ) ) ;  
GO  
```

Weitere Informationen finden Sie unter [CREATE TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms174979.aspx).

## Siehe auch
[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)

[CREATE TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms174979.aspx)

<!---HONumber=AcomDC_0810_2016-->