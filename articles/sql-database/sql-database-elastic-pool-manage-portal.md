<properties
	pageTitle="Überwachen und Verwalten eines Pools für elastische Datenbanken über das Azure-Portal | Microsoft Azure"
	description="Erfahren Sie, wie Sie mithilfe der im Azure-Portal und in der SQL-Datenbank integrierten Intelligenz einen skalierbaren Pool für elastische Datenbanken verwalten, überwachen und skalieren, um die Leistung der Datenbank und die Kostenverwaltung zu optimieren."
	keywords=""
	services="sql-database"
	documentationCenter=""
	authors="ninarn"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="06/22/2016"
	ms.author="ninarn"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Überwachen und Verwalten eines Pools für elastische Datenbanken über das Azure-Portal

> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Mithilfe des Azure-Portals können Sie einen Pool für elastische Datenbanken sowie die darin enthaltenen Datenbanken überwachen und verwalten. Über das Portal können Sie die Verwendung eines elastischen Pools sowie der darin enthaltenen Datenbanken überwachen. Außerdem können Sie einen Änderungssatz für Ihren elastischen Pool erstellen und alle Änderungen gleichzeitig übermitteln. So können Sie etwa Datenbanken hinzufügen oder entfernen, die Einstellungen des elastischen Pools ändern oder Ihre Datenbankeinstellungen anpassen.

Die Abbildung weiter unten zeigt ein Beispiel für einen elastischen Pool. Die Ansicht enthält Folgendes:

*  Diagramme zur Überwachung der Ressourcenverwendung durch den elastischen Pool und die im Pool enthaltenen Datenbanken.
*  Die Schaltfläche **Pool konfigurieren** zum Vornehmen von Änderungen an dem elastischen Pool.
*  Die Schaltfläche **Datenbank erstellen** zum Erstellen einer neuen Datenbank und Hinzufügen der Datenbank zum aktuellen elastischen Pool.
*  Elastische Aufträge zum Verwalten einer großen Anzahl von Datenbanken durch Ausführen von Transact-SQL-Skripts für alle Datenbanken in einer Liste.

![Poolansicht][2]

Für die Schritte in diesem Artikel benötigen Sie eine SQL Server-Instanz in Azure mit mindestens einer Datenbank und einem elastischen Pool. Informationen zum Erstellen eines elastischen Pools finden Sie unter [Erstellen eines Pools](sql-database-elastic-pool-create-portal.md). Falls Sie über keine Datenbank verfügen, sehen Sie sich das [SQL-Datenbank-Tutorial](sql-database-get-started.md) an.

## Überwachung des elastischen Pools

Navigieren Sie zu einem bestimmten Pool, um dessen Ressourcenverwendung anzuzeigen. In der Standardkonfiguration wird für den Pool die Speicher- und eDTU-Verwendung der letzten Stunde angezeigt. Das Diagramm kann mit anderen Metriken und Zeitfenstern konfiguriert werden.

1. Wählen Sie einen zu verwendenden Pool aus.
2. Unter **Überwachung des elastischen Pools** befindet sich ein Diagramm namens **Ressourcenverwendung**. Klicken Sie auf das Diagramm.

	![Überwachung des elastischen Pools][3]

	Das Blatt **Metrik** wird geöffnet und zeigt eine detaillierte Ansicht der angegebenen Metriken für das angegebene Zeitfenster.

	![Blatt "Metrik"][9]

### So passen Sie die Darstellung des Diagramms an

Sie können das Diagramm und das Metrikblatt bearbeiten, um andere Metriken wie etwa die prozentuale Verwendung von CPU, Daten-E/A und Protokoll-E/A anzuzeigen.

2. Klicken Sie auf dem Metrikblatt auf **Bearbeiten**.

	![Klicken auf „Bearbeiten“][6]

- Wählen Sie auf dem Blatt **Diagramm bearbeiten** einen neuen Zeitbereich („Letzte Stunde“, „Heute“ oder „Letzte Woche“) aus, oder klicken Sie auf **Benutzerdefiniert**, um einen beliebigen Datumsbereich innerhalb der letzten beiden Wochen auszuwählen. Wählen Sie den Diagrammtyp („Balken“ oder „Linie“) und dann die zu überwachenden Ressourcen aus.

> Hinweis: Nur Metriken mit der gleichen Maßeinheit können gleichzeitig im Diagramm angezeigt werden. Wenn Sie also beispielsweise die Option für den eDTU-Prozentsatz auswählen, können nur andere prozentbasierte Metriken ausgewählt werden.

	![Click edit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

- Klicken Sie dann auf **OK**.


## Überwachung der elastischen Datenbank

Einzelne Datenbanken können auch auf potenzielle Probleme überwacht werden.

1. Unter **Überwachung der elastischen Datenbank** befindet sich ein Diagramm mit Metriken für fünf Datenbanken. Standardmäßig zeigt das Diagramm die fünf wichtigsten Datenbanken des Pools (gemessen an der durchschnittlichen eDTU-Verwendung innerhalb der letzten Stunde). Klicken Sie auf das Diagramm.

	![Überwachung des elastischen Pools][4]

2. Das Blatt mit der Ressourcenverwendung der Datenbank wird angezeigt. Hier finden Sie eine detaillierte Ansicht der Datenbankverwendung im Pool. Über das Raster im unteren Teil des Blatts können Sie bis zu fünf beliebige Datenbanken im Pool auswählen und deren Verwendung im Diagramm anzeigen. Außerdem können Sie auf **Diagramm bearbeiten** klicken, um die Metriken und das Zeitfenster für das Diagramm anzupassen.

	![Blatt mit der Ressourcenverwendung der Datenbank][8]

### So passen Sie die Ansicht an

1. Klicken Sie auf dem Blatt mit der Ressourcenverwendung der Datenbank auf **Diagramm bearbeiten**.

	![Klicken auf „Diagramm bearbeiten“](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. Wählen Sie auf dem Blatt **Diagramm bearbeiten** einen neuen Zeitbereich („Letzte Stunde“ oder „Letzte 24 Stunden“) aus, oder klicken Sie auf **Benutzerdefiniert**, um einen anderen Tag innerhalb der letzten beiden Wochen auszuwählen.

	![Klicken auf „Benutzerdefiniert“](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Klicken Sie auf das Dropdownfeld für den Datenbankvergleich, um eine andere Metrik auszuwählen.

	![Bearbeiten des Diagramms](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### So wählen Sie die zu überwachenden Datenbanken aus

In der Datenbankliste auf dem Blatt mit der Ressourcenverwendung der Datenbank können Sie nach bestimmten Datenbanken suchen, indem Sie die einzelnen Listenseiten durchgehen oder den Namen einer Datenbank eingeben. Aktivieren Sie das Kontrollkästchen, um die Datenbank auszuwählen.

![Suchen nach zu überwachenden Datenbanken][7]


## Eine Warnung zu einer Poolressource hinzufügen

Sie können Regeln zu Ressourcen hinzufügen, die E-Mails an Personen verschicken, oder Warnzeichenfolgen zu einem URL-Endpunkt, wenn die Ressource einen Verwendungsschwellenwert erreicht, den Sie eingerichtet haben.

> [AZURE.IMPORTANT]Bei der Überwachung der Ressourcenverwendung für elastische Pools gibt es eine Verzögerung von mindestens 20 Minuten. Das Festlegen von Warnungen von weniger als 30 Minuten für elastische Pools wird derzeit nicht unterstützt. Warnungen für elastische Pools mit einem Zeitraum (Parameter namens „-WindowSize“ in der PowerShell-API) von weniger als 30 Minuten werden möglicherweise nicht ausgelöst. Stellen Sie sicher, dass alle Warnungen, die Sie für elastische Pools definieren, einen Zeitraum (WindowSize) von 30 Minuten oder mehr verwenden.

**So fügen Sie eine Warnung zu einer Ressource hinzu:**

1. Klicken Sie auf das Diagramm **Ressourcenverwendung**, um das Blatt **Metrik** zu öffnen. Klicken Sie auf **Warnung hinzufügen**, und füllen Sie dann die Informationen auf dem Blatt **Warnungsregel hinzufügen** aus (**Ressource** wird automatisch auf den Pool festgelegt, mit dem Sie arbeiten).
2. Geben Sie einen aussagekräftigen Namen und eine aussagekräftige Beschreibung für die Warnung ein.
3. Wählen Sie in der Liste eine **Metrik** aus, für die die Warnung ausgegeben werden soll.

    Das Diagramm zeigt dynamisch die Ressourcenverwendung für diese Metrik an, um Ihnen die Festlegung eines Schwellenwerts zu erleichtern.

4. Wählen Sie eine Bedingung (größer als, kleiner als usw.) und einen Schwellenwert aus.
5. Klicken Sie auf **OK**.



## Verschieben einer Datenbank in einen elastischen Pool

Sie können Datenbanken in einem vorhandenen Pool hinzufügen oder entfernen. Datenbanken können sich in verschiedenen Pools befinden. Sie können jedoch nur Datenbanken hinzufügen, die sich auf demselben logischen Server befinden.

1. Klicken Sie auf dem Blatt für den Pool unter **Elastische Datenbanken** auf **Pool konfigurieren**.

    ![Klicken auf „Pool konfigurieren“][1]

2. Klicken Sie auf dem Blatt **Pool konfigurieren** auf **Zu Pool hinzufügen**.

	![Klicken auf „Zu Pool hinzufügen“](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. Wählen Sie auf dem Blatt **Datenbanken hinzufügen** die Datenbanken aus, die dem Pool hinzugefügt werden sollen. Klicken Sie dann auf **Auswählen**.

	![Auswählen hinzuzufügender Datenbanken](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    Auf dem Blatt **Pool konfigurieren** wird nun die hinzuzufügende Datenbank mit dem Status **Ausstehend** angezeigt.

    ![Ausstehende Hinzufügungen zum Pool](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. Klicken Sie auf dem Blatt **Pool konfigurieren** auf **Speichern**.

    ![Klicken Sie auf Speichern.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## Verschieben einer Datenbank aus einem elastischen Pool

1. Wählen Sie auf dem Blatt **Pool konfigurieren** die zu entfernenden Datenbanken aus.

    ![Auflisten von Datenbanken](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Klicken Sie auf **Aus Pool entfernen**.

    ![Auflisten von Datenbanken](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    Auf dem Blatt **Pool konfigurieren** wird nun die zu entfernende Datenbank mit dem Status **Ausstehend** angezeigt.

    ![Vorschau-Datenbank hinzufügen und entfernen](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. Klicken Sie auf dem Blatt **Pool konfigurieren** auf **Speichern**.

    ![Klicken Sie auf Speichern.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## Ändern Sie die Leistungseinstellungen eines Pools

Im Zuge der Überwachung der Ressourcenverwendung eines Pools stellen Sie unter Umständen fest, dass gewisse Anpassungen erforderlich sind. Möglicherweise muss die Leistung oder Speicherbegrenzung des Pools geändert werden. Gegebenenfalls möchten Sie auch die Datenbankeinstellungen im Pool ändern. Sie können das Setup des Pools jederzeit ändern, um die beste Leistungsverteilung und Kosteneffizienz zu erzielen. Weitere Informationen finden Sie unter [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md).

**So ändern Sie die eDTUs oder Speicherbegrenzungen pro Pool und die eDTUs pro Datenbank:**

1. Öffnen Sie das Blatt **Pool konfigurieren**.

    Ändern Sie mithilfe der Schieberegler unter **Einstellungen des elastischen Datenbankpools** die Pooleinstellungen.

    ![Ressourcenverwendung eines elastischen Pools](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Wenn sich die Einstellung ändert, werden in der Anzeige die geschätzten monatlichen Kosten der Änderung angezeigt.

    ![Einen Pool aktualisieren und neue monatliche Kosten](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)


## Erstellen und Verwalten von elastischen Aufträgen

Elastische Aufträge ermöglichen die Ausführung von Transact-SQL-Skripts für eine beliebige Anzahl von Datenbanken im Pool. Sie können neue Aufträge erstellen oder vorhandene Aufträge über das Portal verwalten.

![Erstellen und Verwalten von elastischen Aufträgen][5]


Vor der Verwendung von Aufträgen installieren Sie die Komponenten für elastische Aufträge, und geben Sie Ihre Anmeldeinformationen an. Weitere Informationen finden Sie unter [Übersicht über elastische Datenbankaufträge](sql-database-elastic-jobs-overview.md).

Unter [Horizontales Hochskalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md) finden Sie Informationen zur Verwendung elastischer Datenbanktools für die horizontale Hochskalierung, zum Verschieben von Daten, für die Abfrage oder zum Erstellen von Transaktionen.



## Zusätzliche Ressourcen

- [Elastische SQL-Datenbankpools](sql-database-elastic-pool.md)
- [Erstellen eines Pools für elastische Datenbanken mit dem Portal](sql-database-elastic-pool-create-csharp.md)
- [Erstellen eines Pools für elastische Datenbanken mit PowerShell](sql-database-elastic-pool-create-powershell.md)
- [Erstellen eines Pools für elastische Datenbanken mit C#](sql-database-elastic-pool-create-csharp.md)
- [Überlegungen zum Preis und zur Leistung eines Pools für elastische Datenbanken](sql-database-elastic-pool-guidance.md)


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png

<!---HONumber=AcomDC_0928_2016-->