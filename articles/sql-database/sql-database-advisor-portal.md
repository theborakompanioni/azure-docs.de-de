---
title: "Anwenden von Empfehlungen zur Leistung für Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Sie können im Azure-Portal Empfehlungen zur Leistungsverbesserung erhalten, die die Leistung Ihrer Azure SQL-Datenbank verbessern können, oder Sie können in Ihrer Workload erkannte Probleme beheben."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: cda8a646-0584-4368-b28a-85cdd9b54fcd
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: 018afaa8b08bd001e55693390e80c8e2c4f33a30
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017



---
# <a name="find-and-apply-performance-recommendations"></a>Suchen und Anwenden von Empfehlungen zur Leistung

Sie können im Azure-Portal Empfehlungen zur Leistungsverbesserung erhalten, die die Leistung Ihrer Azure SQL-Datenbank verbessern können, oder Sie können in Ihrer Workload erkannte Probleme beheben. Mit der Seite **Empfehlungen zur Leistung** im Azure-Portal können Sie die besten Empfehlungen basierend auf deren möglichen Auswirkungen ermitteln. 

## <a name="viewing-recommendations"></a>Anzeigen von Empfehlungen

Zum Anzeigen und Anwenden von Empfehlungen zur Leistung benötigen Sie die richtigen Berechtigungen für die [rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-what-is.md) in Azure. Die Berechtigungen **Leser** und **SQL-DB-Mitwirkender** sind erforderlich, um Empfehlungen anzuzeigen, und die Berechtigungen **Besitzer** und **SQL-DB-Mitwirkender** sind erforderlich, um Aktionen ausführen zu können, Indizes zu erstellen oder zu löschen und die Indexerstellung abzubrechen.

Verwenden Sie die folgenden Schritte, um Empfehlungen zur Leistung im Azure-Portal zu suchen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie auf **Weitere Dienste** > **SQL-Datenbanken**, und klicken Sie auf Ihre Datenbank.
3. Navigieren Sie zu **Empfehlungen zur Leistung**, um die verfügbaren Empfehlungen für die ausgewählte Datenbank anzuzeigen.

Empfehlungen zur Leistung werden in der Tabelle angezeigt, so ähnlich wie in folgender Abbildung dargestellt:

![Empfehlungen](./media/sql-database-advisor-portal/recommendations.png)

Empfehlungen werden nach möglichen Auswirkungen auf die Leistung in die folgenden Kategorien unterteilt:

| Auswirkung | Beschreibung |
|:--- |:--- |
| Hoch |Empfehlungen für hohe Auswirkungen sollten den größten Einfluss auf die Leistung haben. |
| Mittel |Empfehlungen für mittlere Auswirkungen sollten die Leistung verbessern, jedoch nicht wesentlich. |
| Niedrig |Empfehlungen für geringe Auswirkungen sollten eine bessere Leistung bieten, die Verbesserungen sind möglicherweise jedoch nicht signifikant. |


> [!NOTE]
> Azure SQL-Datenbank muss Aktivitäten mindestens einen Tag lang überwachen, um einige Empfehlungen aufstellen zu können. Der Azure SQL-Datenbank kann leichter für konsistente Abfragemuster optimiert werden als für zufällige, unregelmäßige Aktivitätsspitzen. Wenn gerade keine Empfehlungen verfügbar sind, sollte der Grund in einer Meldung auf der Seite **Empfehlungen zur Leistung** erläutert werden.
> 

Außerdem können Sie hier die Verlaufsdaten der Vorgänge sehen. Wählen Sie eine Empfehlung oder einen Status aus, um weitere Details anzuzeigen.

Hier sehen Sie die Empfehlung „Index erstellen“ im Azure-Portal.

![Erstellen des Index](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a>Anwenden von Empfehlungen
Azure SQL-Datenbank gibt Ihnen vollständige Kontrolle darüber, wie Empfehlungen umgesetzt werden. Dazu stehen Ihnen die folgenden drei Optionen zur Verfügung: 

* Aktivieren Sie einzelne Empfehlungen nacheinander.
* Aktivieren Sie die automatische Optimierung, um die Empfehlungen automatisch anzuwenden.
* Führen Sie das empfohlene T-SQL-Skript für Ihre Datenbank aus, um eine Empfehlung manuell zu implementieren.

Wählen Sie eine beliebige Empfehlung aus, um die zugehörigen Details anzuzeigen. Klicken Sie dann auf **Skript anzeigen**, um genaue Informationen dazu anzuzeigen, wie die Empfehlung erstellt wird.

Die Datenbank bleibt online, während die Empfehlung angewendet wird. Eine Empfehlungen zur Leistung bzw. die automatischen Optimierung führen nie dazu, dass eine Datenbank offline geschaltet wird.

### <a name="apply-an-individual-recommendation"></a>Anwenden einzelner Empfehlungen
Sie können Empfehlungen nacheinander anzeigen und akzeptieren.

1. Klicken Sie auf dem Blatt **Empfehlungen** auf eine Empfehlung.
2. Klicken Sie auf dem Blatt **Details** auf **Anwenden**.
   
    ![Anwenden einer Empfehlung](./media/sql-database-advisor-portal/apply.png)

Die ausgewählte Empfehlung wird auf die Datenbank angewendet.

### <a name="removing-recommendations-from-the-list"></a>Entfernen von Empfehlungen aus der Liste
Wenn die Liste der Empfehlungen Einträge enthält, die Sie aus der Liste entfernen möchten, können Sie die Empfehlung verwerfen:

1. Wählen Sie in der Liste **Empfehlungen** eine Empfehlung aus, um die Details zu öffnen.
2. Klicken Sie auf dem Blatt **Details** auf **Verwerfen**.

Falls gewünscht, können Sie verworfene Einträge wieder zur Liste **Empfehlungen** hinzufügen:

1. Klicken Sie auf dem Blatt **Empfehlungen** auf **Verworfene anzeigen**.
2. Wählen Sie einen verworfenen Eintrag aus der Liste, um dessen Details anzuzeigen.
3. Klicken Sie optional auf **"Verwerfen" rückgängig machen**, um den Eintrag wieder der Hauptliste der **Empfehlungen** hinzuzufügen.


### <a name="enable-automatic-tuning"></a>Aktivieren der automatischen Optimierung
Sie können den Azure SQL-Datenbank so konfigurieren, dass Empfehlungen automatisch implementiert werden. Sobald Empfehlungen zur Verfügung stehen, werden sie automatisch angewendet. Wie bei allen vom Dienst verwalteten Empfehlungen wird eine Empfehlung rückgängig gemacht, wenn sie sich negativ auf die Leistung auswirkt.

1. Klicken Sie auf dem Blatt **Empfehlungen** auf **Automatisieren**:
   
    ![Ratgebereinstellungen](./media/sql-database-advisor-portal/settings.png)
2. Zu automatisierende Aktionen auswählen:
   
    ![Empfohlene Indizes](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-the-recommended-t-sql-script"></a>Manuelles Ausführen des empfohlenen T-SQL-Skripts
Wählen Sie eine beliebige Empfehlung aus, und klicken Sie auf **Skript anzeigen**. Führen Sie dieses Skript für Ihre Datenbank aus, um die Empfehlung manuell anzuwenden.

*Indizes, die manuell erstellt wurden, werden nicht durch den Dienst überwacht und auf ihre tatsächlichen Auswirkungen auf die Leistung überprüft*. Es empfiehlt sich daher, diese Indizes nach der Erstellung zu überwachen. So können Sie sicherstellen, dass sie Leistungssteigerungen bieten, und sie gegebenenfalls anpassen oder löschen. Ausführliche Informationen zum Erstellen von Indizes finden Sie unter [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).

### <a name="canceling-recommendations"></a>Abbrechen von Empfehlungen
Empfehlungen, die den Status **Ausstehend**, **Wird geprüft** oder **Erfolg** aufweisen, können abgebrochen werden. Empfehlungen mit dem Status **Wird ausgeführt** können nicht abgebrochen werden.

1. Wählen Sie im Bereich **Optimierungsverlauf** eine Empfehlung aus, um das Blatt **Empfehlungsdetails** zu öffnen.
2. Klicken Sie auf **Abbrechen** , um den Vorgang zum Anwenden der Empfehlung abzubrechen.

## <a name="monitoring-operations"></a>Überwachen von Vorgängen
Eine Empfehlung wird möglicherweise nicht umgehend angewendet. Im Portal finden Sie ausführliche Informationen zum Status der Empfehlungen. Indizes können die folgenden Zustände aufweisen:

| Status | Beschreibung |
|:--- |:--- |
| Ausstehend |Der Befehl zum Anwenden der Empfehlung wurde empfangen und ist für die Ausführung geplant. |
| Wird ausgeführt |Die Empfehlung wird angewendet. |
| Überprüfen |Die Empfehlung wurde erfolgreich angewendet, und der Dienst berechnet die Vorteile. |
| Erfolgreich |Die Empfehlung wurde erfolgreich angewendet, und die Vorteile wurden berechnet. |
| Error |Beim Vorgang zum Anwenden der Empfehlung ist ein Fehler aufgetreten. Dies kann ein vorübergehendes Problem sein, oder es handelt sich möglicherweise um eine Schemaänderung an der Tabelle, und das Skript ist nicht mehr gültig. |
| Wird zurückgesetzt |Die Empfehlung wurde angewendet, wird jedoch als nicht leistungsfähig erachtet und automatisch zurückgesetzt. |
| Zurückgesetzt |Die Empfehlung wurde zurückgesetzt. |

Klicken Sie auf eine in Bearbeitung befindliche Empfehlung in der Liste, um weitere Details anzuzeigen:

![Empfohlene Indizes](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a>Zurücksetzen einer Empfehlung
Wenn Sie die Empfehlungen zur Leistung verwendet habe, um die Empfehlung anzuwenden (also nicht das T-SQL-Skript manuell ausgeführt haben), wird die Empfehlung automatisch rückgängig gemacht, wenn sie sich negativ auf die Leistung auswirkt. Wenn Sie aus irgendeinem Grund eine Empfehlung zurücksetzen möchten, können Sie Folgendes tun.

1. Wählen Sie eine erfolgreich angewendete Empfehlung im Bereich **Optimierungsverlauf** aus.
2. Klicken Sie auf dem Blatt **Details zur Empfehlung** auf **Zurücksetzen**.

![Empfohlene Indizes](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a>Überwachen der Auswirkung von Indexempfehlungen auf die Leistung
Nachdem Empfehlungen erfolgreich implementiert wurden (zurzeit nur Indexvorgänge und Empfehlungen zum Parametrisieren von Abfragen), können Sie auf dem Blatt „Details zur Empfehlung“ auf **Details abfragen** klicken, um über [Query Performance Insight](sql-database-query-performance.md) die Statistik zur Abfrageleistung zu öffnen und die Auswirkung Ihrer häufigsten Abfragen auf die Leistung anzuzeigen.

![Überwachen der Auswirkung auf die Leistung](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a>Zusammenfassung
Azure SQL-Datenbank bietet Empfehlungen zur Leistungsverbesserung für die SQL-Datenbank. Durch das Bereitstellen von T-SQL-Skripts sowie individueller und vollständig automatisierter Optionen, erhalten Sie wertvolle Unterstützung bei der Optimierung Ihrer Datenbank und damit Ihrer Abfrageleistung.

## <a name="next-steps"></a>Nächste Schritte
Überwachen Sie Ihre Empfehlungen, und wenden Sie sie weiterhin an, um die Leistung zu optimieren. Datenbankworkloads sind dynamisch und ändern sich ständig. Azure SQL-Datenbank setzt die Überwachung fort und bietet Empfehlungen, durch die die Leistung Ihrer Datenbank erhöht werden kann. 

* Weitere Informationen zur automatischen Optimierung von Azure SQL-Datenbank finden Sie im Artikel zur [Automatischen Optimierung](sql-database-automatic-tuning.md).
* Einen Überblick der Empfehlungen zur Leistung von Azure SQL-Datenbank finden Sie im Artikel zu [Empfehlungen zur Leistung](sql-database-advisor.md).
* Unter [Query Performance Insight](sql-database-query-performance.md) erfahren Sie, wie Sie die Auswirkungen Ihrer wichtigsten Abfragen auf die Leistung untersuchen können.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx)
* [CREATE INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
* [Rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-what-is.md)


