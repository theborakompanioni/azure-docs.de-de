<properties 
   pageTitle="Azure SQL-Datenbank-Ratgeber" 
   description="Der Azure SQL-Datenbank-Ratgeber stellt Empfehlungen für Ihre vorhandenen SQL-Datenbanken bereit, die die aktuelle Abfrageleistung verbessern können." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="05/10/2016"
   ms.author="sstein"/>

# SQL-Datenbank-Ratgeber

Der Azure SQL-Datenbank-Indexratgeber wurde aktualisiert und enthält nun nicht mehr nur Empfehlungen für das Erstellen und Löschen von Indizes, sondern auch Empfehlungen für das Parametrisieren von Abfragen und für das Beheben von Schemaproblemen. Mit diesen zusätzlichen Empfehlungen heißt der Indexratgeber jetzt **SQL-Datenbank-Ratgeber**.

Der SQL-Datenbank-Ratgeber bewertet die Leistung durch eine Analyse des Nutzungsverlaufs Ihrer SQL-Datenbank. Die Empfehlungen, die sich am besten für die Ausführung der typischen Workload Ihrer Datenbank eignen, werden angezeigt.

Die folgenden Empfehlungen sind für V12-Server verfügbar. (Für V11-Server stehen keine Empfehlungen zur Verfügung.) Derzeit können Sie festlegen, dass die Empfehlungen zum Erstellen und Löschen von Indizes automatisch angewendet werden. Informationen hierzu finden Sie unter [Automatische Indexverwaltung](#enable-automatic-index-management).

## Empfehlungen zum Erstellen eines Index 

Empfehlungen zum **Erstellen eines Index** werden angezeigt, wenn der SQL-Datenbankdienst erkennt, dass ein Index fehlt, von dem die Workload Ihrer Datenbanken profitieren könnte (nur nicht gruppierte Indizes).


## Empfehlungen zum Löschen eines Index

Empfehlungen zum **Löschen eines Index** werden angezeigt, wenn der SQL-Datenbankdienst doppelt vorhandene Indizes erkennt (derzeit in der Vorschau, gilt nur für doppelt vorhandene Indizes).

## Empfehlungen zum Parametrisieren von Abfragen

Empfehlungen zum **Parametrisieren von Abfragen** werden angezeigt, wenn der SQL-Datenbankdienst erkennt, dass eine oder mehrere Abfragen ständig neu kompiliert werden, aber am Ende immer derselbe Abfrageausführungsplan daraus hervorgeht. Dadurch ergibt sich die Möglichkeit zum Anwenden einer erzwungenen Parametrisierung, sodass Abfragepläne für eine verbesserte Leistung und eine geringere Ressourcennutzung zwischengespeichert und wiederverwendet werden.

## Empfehlungen zum Beheben von Schemaproblemen

Empfehlungen zum **Beheben von Schemaproblemen** werden angezeigt, wenn der SQL-Datenbankdienst eine Anomalie in der Anzahl von schemabezogenen SQL-Fehlern erkennt, die in Ihrer Azure SQL-Datenbank auftreten. Diese Empfehlung tritt in der Regel auf, wenn Ihre Datenbank innerhalb einer Stunde mehrere schemabezogene Fehler erkennt (ungültiger Spaltenname, ungültiger Objektname usw.).


## Anzeigen von Empfehlungen

Auf der Seite „Empfehlungen“ können Sie die wichtigsten Empfehlungen basierend auf ihrer potenziellen Auswirkung zur Verbesserung der Leistung anzeigen. Außerdem können Sie hier die Verlaufsdaten der Vorgänge sehen. Wählen Sie eine Empfehlung oder einen Status aus, um Detailinformationen anzuzeigen.

Zum Anzeigen und Anwenden von Empfehlungen benötigen Sie die richtigen Berechtigungen für die [rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-configure.md) in Azure. Die Berechtigungen **Leser** und **SQL-DB-Mitwirkender** sind erforderlich, um Empfehlungen anzuzeigen, und die Berechtigungen **Besitzer** und **SQL-DB-Mitwirkender** sind erforderlich, um Aktionen ausführen zu können, Indizes zu erstellen oder zu löschen und die Indexerstellung abzubrechen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie auf **DURCHSUCHEN** > **SQL-Datenbanken**, und wählen Sie Ihre Datenbank aus.
5. Klicken Sie auf **Alle Einstellungen** > **Empfehlungen**, um die verfügbaren **Empfehlungen** für die ausgewählte Datenbank anzuzeigen.

> [AZURE.NOTE] Um Empfehlungen zu erhalten, muss eine Datenbank ungefähr eine Woche lang genutzt werden, und innerhalb dieser Woche müssen Aktivitäten stattfinden. Außerdem müssen auch konsistente Aktivitäten vorhanden sein. Der SQL-Datenbank-Ratgeber kann leichter für konsistente Abfragemuster optimiert werden als für zufällige, unregelmäßige Aktivitätsspitzen. Wenn auf der Seite **Empfehlungen** keine Empfehlungen verfügbar sind, sollte der Grund hier in einer Meldung erläutert werden.

![Recommendations](./media/sql-database-index-advisor/recommendations.png)

Empfehlungen werden nach möglichen Auswirkungen auf die Leistung in die folgenden vier Kategorien unterteilt:

| Auswirkung | Beschreibung |
| :--- | :--- |
| Hoch | Empfehlungen für hohe Auswirkungen sollten den größten Einfluss auf die Leistung haben. |
| Mittel | Empfehlungen für mittlere Auswirkungen sollten die Leistung verbessern, jedoch nicht wesentlich. |
| Niedrig | Empfehlungen für geringe Auswirkungen sollten eine bessere Leistung bieten, die Verbesserungen sind möglicherweise jedoch nicht signifikant. 


### Entfernen von Empfehlungen aus der Liste

Wenn die Liste der Empfehlungen Einträge enthält, die Sie aus der Liste entfernen möchten, können Sie die Empfehlung verwerfen:

1. Wählen Sie in der Liste **Empfehlungen** eine Empfehlung aus.
2. Klicken Sie auf dem Blatt **Details** auf **Verwerfen**.


Falls gewünscht, können Sie verworfene Einträge wieder zur Liste **Empfehlungen** hinzufügen:

1. Klicken Sie auf dem Blatt **Empfehlungen** auf **Verworfene anzeigen**.
1. Wählen Sie einen verworfenen Eintrag aus der Liste, um dessen Details anzuzeigen.
1. Klicken Sie optional auf **„Verwerfen“ rückgängig machen**, um den Eintrag wieder der Hauptliste der **Empfehlungen** hinzuzufügen.



## Anwenden von Empfehlungen

Der SQL-Datenbank-Ratgeber gibt Ihnen vollständige Kontrolle darüber, wie Empfehlungen umgesetzt werden. Dazu stehen Ihnen die folgenden drei Optionen zur Verfügung:

- Aktivieren Sie einzelne Empfehlungen nacheinander.
- Ermöglichen Sie dem Ratgeber das automatische Anwenden von Empfehlungen (gilt derzeit für nur Indexempfehlungen).
- Führen Sie das empfohlene T-SQL-Skript manuell für Ihre Datenbank aus, um eine Empfehlung zu implementieren.

Wählen Sie eine beliebige Empfehlung aus, um die zugehörigen Detailinformationen anzuzeigen. Klicken Sie dann auf **Skript anzeigen**, um genaue Informationen dazu anzuzeigen, wie die Empfehlung erstellt wird.

Die Datenbank bleibt online, während der Ratgeber die Empfehlung anwendet – durch den SQL-Datenbank-Ratgeber wird niemals eine Datenbank offline geschaltet.

### Anwenden einzelner Empfehlungen

Sie können Empfehlungen nacheinander anzeigen und akzeptieren.

1. Klicken Sie auf dem Blatt **Empfehlungen** auf eine Empfehlung.
2. Klicken Sie auf dem Blatt **Details** auf **Anwenden**.

    ![Anwenden einer Empfehlung](./media/sql-database-index-advisor/apply.png)


### Aktivieren der automatischen Indexverwaltung

Sie können den SQL-Datenbank-Ratgeber so konfigurieren, dass Empfehlungen automatisch implementiert werden. Sobald Empfehlungen zur Verfügung stehen, werden sie automatisch angewendet. Wie bei allen vom Dienst verwalteten Indexvorgängen wird eine Empfehlung zurückgesetzt, wenn sie sich negativ auf die Leistung auswirkt.

1. Klicken Sie auf dem Blatt **Empfehlungen** auf **Automatisieren**.

    ![Ratgebereinstellungen](./media/sql-database-index-advisor/settings.png)

2. Konfigurieren Sie den Ratgeber für das automatische **Erstellen** oder **Löschen** von Indizes:

    ![Empfohlene Indizes](./media/sql-database-index-advisor/automation.png)




### Manuelles Ausführen des empfohlenen T-SQL-Skripts

Wählen Sie eine beliebige Empfehlung aus, und klicken Sie auf **Skript anzeigen**. Führen Sie dieses Skript für Ihre Datenbank aus, um die Empfehlung manuell anzuwenden.

*Indizes, die manuell erstellt wurden, werden nicht durch den Dienst überwacht und auf ihre tatsächlichen Auswirkungen auf die Leistung überprüft*. Es empfiehlt sich daher, diese Indizes nach der Erstellung zu überwachen. So können Sie sicherstellen, dass sie Leistungssteigerungen bieten, und sie gegebenenfalls anpassen oder löschen. Ausführliche Informationen zum Erstellen von Indizes finden Sie unter [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).


### Abbrechen von Empfehlungen

Empfehlungen, die den Status **Ausstehend**, **Wird geprüft** oder **Erfolgreich** aufweisen, können abgebrochen werden. Empfehlungen mit dem Status **Wird ausgeführt** können nicht abgebrochen werden.

1. Wählen Sie im Bereich **Optimierungsverlauf** eine Empfehlung aus, um das Blatt **Empfehlungsdetails** zu öffnen.
2. Klicken Sie auf **Abbrechen**, um den Vorgang zum Anwenden der Empfehlung abzubrechen.



## Überwachen von Vorgängen

Eine Empfehlung wird möglicherweise nicht umgehend angewendet. Im Portal finden Sie ausführliche Informationen zum Status der Empfehlungsvorgänge. Indizes können die folgenden Zustände aufweisen:

| Status | Beschreibung |
| :--- | :--- |
| Ausstehend | Der Befehl zum Anwenden der Empfehlung wurde empfangen und ist für die Ausführung geplant. |
| Wird ausgeführt | Die Empfehlung wird angewendet. |
| Erfolgreich | Die Empfehlung wurde erfolgreich angewendet. |
| Fehler | Beim Vorgang zum Anwenden der Empfehlung ist ein Fehler aufgetreten. Dies kann ein vorübergehendes Problem sein, oder es handelt sich möglicherweise um eine Schemaänderung an der Tabelle, und das Skript ist nicht mehr gültig. |
| Wird zurückgesetzt | Die Empfehlung wurde angewendet, wird jedoch als nicht leistungsfähig erachtet und automatisch zurückgesetzt. |
| Zurückgesetzt | Die Empfehlung wurde zurückgesetzt. |

Klicken Sie auf eine in Bearbeitung befindliche Empfehlung in der Liste, um die zugehörigen Detailinformationen anzuzeigen:

![Empfohlene Indizes](./media/sql-database-index-advisor/operations.png)



### Zurücksetzen einer Empfehlung

Wenn Sie die Empfehlung mithilfe des Ratgebers angewendet haben (also nicht das T-SQL-Skript manuell ausgeführt haben), wird die Empfehlung automatisch zurückgesetzt, wenn sie sich negativ auf die Leistung auswirkt. Wenn Sie aus irgendeinem Grund eine Empfehlung zurücksetzen möchten, können Sie Folgendes tun.


1. Wählen Sie eine erfolgreich angewendete Empfehlung im Bereich **Optimierungsverlauf** aus.
2. Klicken Sie auf dem Blatt **Empfehlungsdetails** auf **Zurücksetzen**.

![Empfohlene Indizes](./media/sql-database-index-advisor/details.png)


## Überwachen der Auswirkung von Indexempfehlungen auf die Leistung

Nachdem Empfehlungen erfolgreich implementiert wurden (zurzeit nur Indexvorgänge und Empfehlungen zum Parametrisieren von Abfragen), können Sie auf dem Blatt „Empfehlungsdetails“ auf **Details abfragen** klicken, um die [Statistik zur Abfrageleistung](sql-database-query-performance.md) zu öffnen und die Auswirkung Ihrer häufigsten Abfragen auf die Leistung anzuzeigen.

![Überwachen der Auswirkung auf die Leistung](./media/sql-database-index-advisor/query-insights.png)



## Zusammenfassung

Der SQL-Datenbank-Ratgeber bietet Empfehlungen zur Leistungsverbesserung für die SQL-Datenbank. Durch das Bereitstellen von T-SQL-Skripts sowie individueller und vollständig automatisierter Optionen bietet der Ratgeber wertvolle Unterstützung bei der Optimierung Ihrer Datenbank und damit Ihrer Abfrageleistung.



## Nächste Schritte

Überwachen Sie Ihre Empfehlungen, und wenden Sie sie weiterhin an, um die Leistung zu optimieren. Datenbankworkloads sind dynamisch und ändern sich ständig. Der SQL-Datenbank-Ratgeber setzt die Überwachung fort und bietet Empfehlungen, durch die die Leistung Ihrer Datenbank erhöht werden kann.

<!---HONumber=AcomDC_0518_2016-->