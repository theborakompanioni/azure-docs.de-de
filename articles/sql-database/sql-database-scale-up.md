<properties
	pageTitle="Ändern der Dienstebene und Leistungsstufe einer Azure SQL-Datenbank"
	description="In „Ändern der Dienstebene und Leistungsstufe einer Azure SQL-Datenbank“ wird beschrieben, wie Sie die Dienstebene und Leistungsstufe Ihrer SQL-Datenbank zentral hoch- oder herunterstufen. Ändern des Tarifs einer Azure SQL-Datenbank."
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="03/29/2016"
	ms.author="sstein"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Ändern der Dienstebene und Leistungsstufe (Tarif) einer SQL-Datenbank


> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-scale-up.md)
- [PowerShell](sql-database-scale-up-powershell.md)


Dienstebenen und Leistungsstufen beschreiben die für Ihre SQL-Datenbank verfügbaren Funktionen und Ressourcen und können aktualisiert werden, wenn sich die Anforderungen an Ihre Anwendung ändern. Weitere Informationen finden Sie unter [Tarife](sql-database-service-tiers.md).

Beachten Sie, dass durch das Ändern des Diensttarifs und/oder der Leistungsstufe einer Datenbank ein Replikat der ursprünglichen Datenbank mit der neuen Leistungsstufe erstellt wird. Infolge wird die Verbindung zu diesem Replikat umgestellt. Während dieses Vorgangs gehen keine Daten verloren. Allerdings sind die Verbindungen zur Datenbank inaktiv, während kurz auf das Replikat umgestellt wird. Daher werden möglicherweise einige aktive Transaktionen zurückgesetzt. Dieses Zeitfenster variiert, ist aber durchschnittlich kleiner als vier Sekunden und in mehr als 99 % der Fälle kürzer als 30 Sekunden. Nur in seltenen Fällen, z.B. wenn im Moment der Verbindungstrennung viele Transaktionen stattfinden, ist dieses Fenster länger.

Die Dauer des gesamten zentralen Hochskalierungsvorgangs hängt sowohl von der Größe als auch vom Diensttarif der Datenbank vor und nach der Änderung ab. Beispielsweise sollte eine 250-GB-Datenbank beim Wechsel in, aus oder innerhalb eines Standarddiensttarifs innerhalb von sechs Stunden zentral hochskalieren. Eine Datenbank der gleichen Größe, die ihre Leistungsstufen innerhalb des Premium-Diensttarifs ändert, sollte dies innerhalb von drei Stunden vollziehen.


Bestimmen Sie anhand der Informationen unter [Aktualisieren von Web-/Business-SQL-Datenbanken auf die neuen Dienstebenen und Leistungsstufen der Azure SQL-Datenbank](sql-database-upgrade-server-portal.md) und [Dienstebenen und Leistungsstufen der Azure SQL-Datenbank](sql-database-service-tiers.md) die geeignete Dienstebene und Leistungsstufe für Ihre Azure SQL-Datenbank.

- Für ein Downgrade einer Datenbank sollte die Datenbank kleiner als die in der Zieldienstebene maximal zulässige Größe sein. 
- Beim Aktualisieren einer Datenbank, für die [Georeplikation](sql-database-geo-replication-overview.md) aktiviert ist, müssen Sie vor dem Upgrade der primären Datenbank zunächst die zugehörigen sekundären Datenbanken auf die gewünschte Leistungsstufe aktualisieren.
- Beim Downgraden einer Dienstebene müssen Sie zuerst alle Georeplikationsbeziehungen beenden. 
- Die Angebote des Wiederherstellungsdienstes variieren für die verschiedenen Dienstebenen. Wenn Sie ein Downgrade durchführen, verlieren Sie eventuell die Möglichkeit einer Zeitpunktwiederherstellung, oder der Aufbewahrungszeitraum für Sicherungen verkürzt sich. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen der Azure SQL-Datenbank](sql-database-business-continuity.md).
- Durch das Ändern des Tarifs für Ihre Datenbank ändert sich nicht die maximale Datenbankgröße. Verwenden Sie zum Ändern der maximalen Datenbankgröße [Transact SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) oder [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
- Die neuen Eigenschaften für die Datenbank werden erst angewendet, wenn die Änderungen abgeschlossen sind.



**Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:**

- Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, müssen Sie lediglich oben auf dieser Seite auf den Link **Kostenlose Testversion** klicken. Lesen Sie anschließend den Artikel weiter.
- Eine Azure SQL-Datenbank. Wenn Sie nicht über eine SQL-Datenbank verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).


## Ändern der Dienstebene und Leistungsstufe Ihrer Datenbank


Öffnen Sie das Blatt „SQL-Datenbank“ für die Datenbank, die Sie zentral hoch- oder herunterskalieren möchten:

1.	Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2.	Klicken Sie auf **ALLE DURCHSUCHEN**.
3.	Klicken Sie auf **SQL-Datenbanken**.
2.	Klicken Sie auf die Datenbank, die Sie ändern möchten.
3.	Klicken Sie auf dem Blatt der SQL-Datenbank auf **Alle Einstellungen** und dann auf **Tarif (DTUs skalieren)**:

    ![Tarif][1]


1.  Wählen Sie eine neue Stufe aus, und klicken Sie auf **Auswählen**.

    Das Klicken auf **Auswählen** sendet eine Skalierungsanforderung zum Ändern der Datenbankebene. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Skalierungsvorgang abgeschlossen ist. Klicken Sie auf die Benachrichtigung, um Details und Status des Skalierungsvorgangs anzuzeigen.

    > [AZURE.NOTE] Durch das Ändern des Tarifs für Ihre Datenbank ändert sich nicht die maximale Datenbankgröße. Verwenden Sie zum Ändern der maximalen Datenbankgröße [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) oder [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).

    ![Tarif auswählen][2]

3.	Klicken Sie im linken Menüband auf **Benachrichtigungen**:

    ![Benachrichtigungen][3]

## Überprüfen, ob die Datenbank den ausgewählten Tarif hat

   Nach Abschluss des Skalierungsvorgangs überprüfen und bestätigen Sie, dass die Datenbank den gewünschten Tarif hat:

2.	Klicken Sie auf **ALLE DURCHSUCHEN**.
3.	Klicken Sie auf **SQL-Datenbanken**.
2.	Klicken Sie auf die Datenbank, die Sie aktualisiert haben.
3.	Überprüfen Sie den **Tarif**, und vergewissern Sie sich, dass er richtig festgelegt ist.

    ![neuer Preis][4]


## Nächste Schritte

- Ändern Sie die maximale Datenbankgröße mit [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) oder [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
- [Horizontal hoch- und herunterskalieren](sql-database-elastic-scale-get-started.md)
- [Herstellen einer Verbindung mit einer SQL-Datenbank und Abfragen dieser Datenbank mit SSMS](sql-database-connect-query-ssms.md)
- [Exportieren einer Azure SQL-Datenbank](sql-database-export.md)

## Zusätzliche Ressourcen

- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [SQL-Datenbank-Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-scale-up/pricing-tile.png
[2]: ./media/sql-database-scale-up/choose-tier.png
[3]: ./media/sql-database-scale-up/scale-notification.png
[4]: ./media/sql-database-scale-up/new-tier.png

<!---HONumber=AcomDC_0518_2016-->