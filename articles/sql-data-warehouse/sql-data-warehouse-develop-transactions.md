---
title: Transaktionen in SQL Data Warehouse | Microsoft Docs
description: "Tipps zum Implementieren von Transaktionen in Azure SQL Data Warehouse zum Entwickeln von Lösungen"
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: ae621788-e575-41f5-8bfe-fa04dc4b0b53
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: dde5397405b64d394cdff9c69c05a565c5427f56


---
# <a name="transactions-in-sql-data-warehouse"></a>Transaktionen in SQL Data Warehouse
Wie zu erwarten, unterstützt SQL Data Warehouse Transaktionen als Teil der Data Warehouse-Workload. Um allerdings eine angemessene Leistung von SQL Data Warehouse sicherzustellen, wurden einige Features im Vergleich zu SQL Server eingeschränkt. In diesem Artikel werden die Unterschiede hervorgehoben und die anderen Features aufgelistet. 

## <a name="transaction-isolation-levels"></a>Transaktionsisolationsstufen
SQL Data Warehouse implementiert ACID-Transaktionen. Die Isolation der Transaktionsunterstützung ist jedoch beschränkt auf `READ UNCOMMITTED` und kann nicht geändert werden. Sie können eine Reihe von Codemethoden implementieren, um fehlerhafte Datenlesevorgänge zu verhindern, wenn dies ein Problem für Sie darstellt. Die am häufigsten verwendeten Methoden nutzen sowohl CTAS als auch Wechsel von Partitionstabellen (oftmals gleitendes Fenstermuster genannt), um zu verhindern, dass Benutzer Daten abrufen, die noch vorbereitet werden. Sichten, die die Daten vorab filtern, sind auch ein beliebter Ansatz.  

## <a name="transaction-size"></a>Transaktionsgröße
Eine einzelne Transaktion zur Datenänderung ist in Bezug auf die Größe beschränkt. Der Grenzwert wird heute „pro Verteilung“ angewendet. Die Gesamtzuordnung kann also ermittelt werden, indem der Grenzwert mit der Verteilungsanzahl multipliziert wird. Um eine Annäherung für die maximale Zeilenanzahl in der Transaktion zu erhalten, teilen Sie die Verteilungsobergrenze durch die Gesamtgröße jeder Zeile. Bei Spalten mit variabler Länge können Sie erwägen, anstelle der maximalen Größe eine durchschnittliche Spaltenlänge zu verwenden.

Für die Tabelle unten gelten die folgenden Annahmen:

* Gleichmäßige Verteilung der Daten 
* Durchschnittliche Zeilenlänge beträgt 250 Byte

| [DWU][DWU] | Obergrenze pro Verteilung (GB) | Anzahl der Verteilungen | Max. Transaktionsgröße (GB) | # Zeilen pro Verteilung | Max. Zeilenzahl pro Transaktion |
| --- | --- | --- | --- | --- | --- |
| DW100 |1 |60 |60 |4.000.000 |240.000.000 |
| DW200 |1,5 |60 |90 |6.000.000 |360.000.000 |
| DW300 |2,25 |60 |135 |9.000.000 |540.000.000 |
| DW400 |3 |60 |180 |12.000.000 |720.000.000 |
| DW500 |3,75 |60 |225 |15.000.000 |900.000.000 |
| DW600 |4,5 |60 |270 |18.000.000 |1.080.000.000 |
| DW1000 |7,5 |60 |450 |30.000.000 |1.800.000.000 |
| DW1200 |9 |60 |540 |36.000.000 |2.160.000.000 |
| DW1500 |11,25 |60 |675 |45.000.000 |2.700.000.000 |
| DW2000 |15 |60 |900 |60.000.000 |3.600.000.000 |
| DW3000 |22,5 |60 |1.350 |90.000.000 |5.400.000.000 |
| DW6000 |45 |60 |2.700 |180.000.000 |10.800.000.000 |

Die Obergrenze für die Transaktionsgröße wird pro Transaktion oder Vorgang angewendet. Sie wird nicht übergreifend für alle gleichzeitigen Transaktionen angewendet. Daher ist es für jede Transaktion zulässig, diese Menge an Daten in das Protokoll zu schreiben. 

Informationen zum Optimieren und Minimieren der Datenmenge, die in das Protokoll geschrieben wird, finden Sie im Artikel [Bewährte Methoden für Transaktionen][Bewährte Methoden für Transaktionen].

> [!WARNING]
> Die maximale Transaktionsgröße kann nur für Tabellen mit HASH- oder ROUND_ROBIN-Verteilung erreicht werden, bei denen die Daten gleichmäßig verteilt werden. Wenn bei der Transaktion Daten auf verzerrte Weise in die Verteilungen geschrieben werden, wird die Obergrenze wahrscheinlich vor der maximalen Transaktionsgröße erreicht.
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a>Transaktionsstatus
SQL Data Warehouse verwendet die XACT_STATE()-Funktion, um eine fehlgeschlagene Transaktion mit dem Wert "-2" zu melden. Dies bedeutet, dass die Transaktion fehlgeschlagen und nur für den Rollback markiert ist.

> [!NOTE]
> Die Verwendung von "-2" in der XACT_STATE-Funktion zum Kennzeichnen einer fehlgeschlagenen Transaktion stellt für SQL Server unterschiedliche Verhalten dar. SQL Server verwendet den Wert "-1" für eine Transaktion, für die kein Commit durchgeführt werden kann. SQL Server kann einige Fehler innerhalb einer Transaktion tolerieren, ohne als nicht commitfähig gekennzeichnet zu werden. `SELECT 1/0` würde beispielsweise einen Fehler verursachen, aber keinen Transaktionszustand erzwingen, der keinen Commit zulässt. SQL Server lässt auch Lesevorgänge in der Transaktion zu, für die kein Commit möglich ist. SQL Data Warehouse dagegen lässt dies nicht zu. Wenn in einer SQL Data Warehouse-Transaktion ein Fehler auftritt, wird die Transaktion sofort in den Zustand „-2“ versetzt, und Sie können erst dann weitere SELECT-Anweisungen ausführen, wenn ein Rollback für die ursprüngliche Anweisung durchgeführt wurde. Es ist daher wichtig, zu überprüfen, ob in Ihrem Anwendungscode XACT_STATE() verwendet wird, da Sie den Code möglicherweise ändern müssen.
> 
> 

In SQL Server kann z.B. eine Transaktion wie die folgende angezeigt werden:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Wenn Sie den Code unverändert lassen, erhalten Sie folgende Fehlermeldung:

Meldung 111233, Ebene 16, Status 1, Zeile 1 111233;Die aktuelle Transaktion wurde abgebrochen, und alle ausstehenden Änderungen wurden zurückgesetzt. Ursache: Eine Transaktion in einem Nur-Rollback-Status wurde vor einer DDL-, DML- oder SELECT-Anweisung nicht explizit zurückgesetzt.

Sie erhalten auch keine Ausgabe der ERROR_*-Funktionen.

In SQL Data Warehouse muss der Code leicht geändert werden:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Jetzt werden Sie das erwartete Verhalten feststellen. Der Fehler in der Transaktion wird verwaltet, und die ERROR_*-Funktionen geben wie erwartet Werte zurück.

Die einzige Änderung besteht darin, dass die `ROLLBACK`-Ausführung für die Transaktion vor dem Lesen der Fehlerinformationen im `CATCH`-Block erfolgen muss.

## <a name="errorline-function"></a>Error_Line()-Funktion
Es ist auch erwähnenswert, dass SQL Data Warehouse die ERROR_LINE()-Funktion nicht implementiert oder unterstützt. Wenn diese in Ihrem Code enthalten ist, müssen Sie sie entfernen, um die Kompatibilität mit SQL Data Warehouse zu gewährleisten. Verwenden Sie stattdessen Abfragebezeichnungen in Ihrem Code, um entsprechende Funktionalität zu implementieren. Weitere Informationen zu dieser Funktion finden Sie im Artikel [LABEL][LABEL].

## <a name="using-throw-and-raiserror"></a>Verwenden von THROW und RAISERROR
THROW ist die modernere Implementierung zum Auslösen von Ausnahmen in SQL Data Warehouse, RAISERROR wird jedoch ebenfalls unterstützt. Es gibt aber einige erwähnenswerte Unterschiede.

* Benutzerdefinierte Fehlermeldungsnummern können für THROW nicht im Bereich 100.000 bis 150.000 liegen.
* RAISERROR-Fehlermeldungen sind bei 50.000 festgelegt.
* Die Verwendung von "sys.messages" wird nicht unterstützt.

## <a name="limitiations"></a>Einschränkungen
SQL Data Warehouse verfügt über einige weitere Einschränkungen in Bezug auf Transaktionen.

Dies sind:

* Keine verteilten Transaktionen
* Keine geschachtelten Transaktionen zulässig
* Keine Speicherpunkte zulässig
* Keine benannten Transaktionen
* Keine markierten Transaktionen
* Keine Unterstützung für DDL wie z.B. `CREATE TABLE` innerhalb von benutzerdefinierten Transaktionen

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Optimieren von Transaktionen finden Sie unter [Bewährte Methoden für Transaktionen][Bewährte Methoden für Transaktionen].  Informationen zu weiteren bewährten Methoden für SQL Data Warehouse finden Sie unter [Bewährte Methoden für SQL Data Warehouse][Bewährte Methoden für SQL Data Warehouse].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Entwicklungsübersicht]: ./sql-data-warehouse-overview-develop.md
[Bewährte Methoden für Transaktionen]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Bewährte Methoden für SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->



<!--HONumber=Nov16_HO3-->


