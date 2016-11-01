<properties 
   pageTitle="Azure SQL-Datenbankratgeber im Azure-Portal | Microsoft Azure" 
   description="Mit dem Azure SQL-Datenbankratgeber im Azure-Portal können Sie Empfehlungen für Ihre vorhandenen SQL-Datenbanken anzeigen und umsetzen und so die derzeitige Abfrageleistung verbessern." 
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
   ms.date="06/22/2016"
   ms.author="sstein"/>

# SQL-Datenbankratgeber

> [AZURE.SELECTOR]
- [SQL-Datenbankratgeber – Übersicht](sql-database-advisor.md)
- [Portal](sql-database-advisor-portal.md)

Mit dem Azure SQL-Datenbankratgeber im Azure-Portal können Sie Empfehlungen für Ihre vorhandenen SQL-Datenbanken anzeigen und umsetzen und so die derzeitige Abfrageleistung verbessern.

## Anzeigen von Empfehlungen

Auf der Seite „Empfehlungen“ können Sie die wichtigsten Empfehlungen basierend auf ihrer potenziellen Auswirkung zur Verbesserung der Leistung anzeigen. Außerdem können Sie hier die Verlaufsdaten der Vorgänge sehen. Wählen Sie eine Empfehlung oder einen Status aus, um Detailinformationen anzuzeigen.

Zum Anzeigen und Anwenden von Empfehlungen benötigen Sie die richtigen Berechtigungen für die [rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-configure.md) in Azure. Die Berechtigungen **Leser** und **SQL-DB-Mitwirkender** sind erforderlich, um Empfehlungen anzuzeigen, und die Berechtigungen **Besitzer** und **SQL-DB-Mitwirkender** sind erforderlich, um Aktionen ausführen zu können, Indizes zu erstellen oder zu löschen und die Indexerstellung abzubrechen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie auf **DURCHSUCHEN** > **SQL-Datenbanken**, und wählen Sie Ihre Datenbank aus.
5. Klicken Sie auf **Alle Einstellungen** > **Empfehlungen**, um die verfügbaren **Empfehlungen** für die ausgewählte Datenbank anzuzeigen.

> [AZURE.NOTE] Um Empfehlungen zu erhalten, muss eine Datenbank ungefähr eine Woche lang genutzt werden, und innerhalb dieser Woche müssen Aktivitäten stattfinden. Außerdem müssen auch konsistente Aktivitäten vorhanden sein. Der SQL-Datenbankratgeber kann leichter für konsistente Abfragemuster optimiert werden als für zufällige, unregelmäßige Aktivitätsspitzen. Wenn auf der Seite **Empfehlungen** keine Empfehlungen verfügbar sind, sollte der Grund hier in einer Meldung erläutert werden.

![Empfehlungen](./media/sql-database-advisor-portal/recommendations.png)

Hier sehen Sie eine Empfehlung zum Beheben von Schemaproblemen im Azure-Portal.

![Beheben von Schemaproblemen](./media/sql-database-advisor-portal/sql-database-advisor-schema-issue.png)

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

Der SQL-Datenbankratgeber gibt Ihnen vollständige Kontrolle darüber, wie Empfehlungen umgesetzt werden. Dazu stehen Ihnen die folgenden drei Optionen zur Verfügung:

- Aktivieren Sie einzelne Empfehlungen nacheinander.
- Ermöglichen Sie dem Ratgeber das automatische Anwenden von Empfehlungen (gilt derzeit für nur Indexempfehlungen).
- Führen Sie das empfohlene T-SQL-Skript manuell für Ihre Datenbank aus, um eine Empfehlung zu implementieren.

Wählen Sie eine beliebige Empfehlung aus, um die zugehörigen Detailinformationen anzuzeigen. Klicken Sie dann auf **Skript anzeigen**, um genaue Informationen dazu anzuzeigen, wie die Empfehlung erstellt wird.

Die Datenbank bleibt online, während der Ratgeber die Empfehlung anwendet – durch den SQL-Datenbankratgeber wird niemals eine Datenbank offline geschaltet.

### Anwenden einzelner Empfehlungen

Sie können Empfehlungen nacheinander anzeigen und akzeptieren.

1. Klicken Sie auf dem Blatt **Empfehlungen** auf eine Empfehlung.
2. Klicken Sie auf dem Blatt **Details** auf **Anwenden**.

    ![Anwenden einer Empfehlung](./media/sql-database-advisor-portal/apply.png)

### Aktivieren der automatischen Indexverwaltung

Sie können den SQL-Datenbankratgeber so konfigurieren, dass Empfehlungen automatisch implementiert werden. Sobald Empfehlungen zur Verfügung stehen, werden sie automatisch angewendet. Wie bei allen vom Dienst verwalteten Indexvorgängen wird eine Empfehlung zurückgesetzt, wenn sie sich negativ auf die Leistung auswirkt.

1. Klicken Sie auf dem Blatt **Empfehlungen** auf **Automatisieren**:

    ![Ratgebereinstellungen](./media/sql-database-advisor-portal/settings.png)

2. Konfigurieren Sie den Ratgeber für das automatische **Erstellen** oder **Löschen** von Indizes:

    ![Empfohlene Indizes](./media/sql-database-advisor-portal/automation.png)


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

![Empfohlene Indizes](./media/sql-database-advisor-portal/operations.png)


### Zurücksetzen einer Empfehlung

Wenn Sie die Empfehlung mithilfe des Ratgebers angewendet haben (also nicht das T-SQL-Skript manuell ausgeführt haben), wird die Empfehlung automatisch zurückgesetzt, wenn sie sich negativ auf die Leistung auswirkt. Wenn Sie aus irgendeinem Grund eine Empfehlung zurücksetzen möchten, können Sie Folgendes tun.


1. Wählen Sie eine erfolgreich angewendete Empfehlung im Bereich **Optimierungsverlauf** aus.
2. Klicken Sie auf dem Blatt **Details zur Empfehlung** auf **Zurücksetzen**.

![Empfohlene Indizes](./media/sql-database-advisor-portal/details.png)


## Überwachen der Auswirkung von Indexempfehlungen auf die Leistung

Nachdem Empfehlungen erfolgreich implementiert wurden (zurzeit nur Indexvorgänge und Empfehlungen zum Parametrisieren von Abfragen), können Sie auf dem Blatt „Details zur Empfehlung“ auf **Details abfragen** klicken, um über [Query Performance Insight](sql-database-query-performance.md) die Statistik zur Abfrageleistung zu öffnen und die Auswirkung Ihrer häufigsten Abfragen auf die Leistung anzuzeigen.

![Überwachen der Auswirkung auf die Leistung](./media/sql-database-advisor-portal/query-insights.png)



## Zusammenfassung

Der SQL-Datenbankratgeber bietet Empfehlungen zur Leistungsverbesserung für die SQL-Datenbank. Durch das Bereitstellen von T-SQL-Skripts sowie individueller und vollständig automatisierter Optionen bietet der Ratgeber wertvolle Unterstützung bei der Optimierung Ihrer Datenbank und damit Ihrer Abfrageleistung.



## Nächste Schritte

Überwachen Sie Ihre Empfehlungen, und wenden Sie sie weiterhin an, um die Leistung zu optimieren. Datenbankworkloads sind dynamisch und ändern sich ständig. Der SQL-Datenbankratgeber setzt die Überwachung fort und bietet Empfehlungen, durch die die Leistung Ihrer Datenbank erhöht werden kann.

 - Unter [SQL-Datenbankratgeber](sql-database-advisor.md) finden Sie eine Übersicht zum SQL-Datenbankratgeber.
 - Unter [Query Performance Insight](sql-database-query-performance.md) erfahren Sie, wie Sie die Auswirkungen Ihrer wichtigsten Abfragen auf die Leistung untersuchen können.

## Zusätzliche Ressourcen

- [Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx)
- [CREATE INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
- [Rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-configure.md)

<!---HONumber=AcomDC_0629_2016-->