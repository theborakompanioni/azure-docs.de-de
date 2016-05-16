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
	ms.date="05/02/2016"
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


Über das Azure-Portal können Sie einen Pool für elastische Datenbanken und die zugehörigen Datenbanken überwachen, verwalten und konfigurieren. Die SQL-Datenbank verfügt über eine integrierte Logik, die die bisherige Nutzung aller Datenbanken (eigenständige Datenbanken und Datenbanken in Pools) auf einem Server analysiert und einen Pool für Datenbanken empfiehlt, wenn dies die kostengünstigere Lösung ist.

Über das Portal können Sie die Pool- und Datenbankeinstellungen ändern, eine Vorschau der Änderungen anzeigen und dann alle Änderungen gleichzeitig übernehmen. Sie können eine Vorschau der Änderungen anzeigen, z.B. wenn Datenbanken hinzugefügt oder entfernt werden. Die möglichen Auswirkungen auf die Leistung und die Preise werden ebenfalls angezeigt.

Um diese Anleitung durchzuarbeiten, benötigen Sie einige Datenbanken und einen Pool. Wenn Sie bereits über Datenbanken verfügen, finden Sie weiterführende Informationen unter [Erstellen eines Pools](sql-database-elastic-pool-create-portal.md). Wenn Sie keine Datenbank definiert haben, finden Sie entsprechende Informationen im [SQL-Datenbank-Tutorial](sql-database-get-started.md).

## Auswählen eines zu verwendenden Pools

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Durchsuchen**.
2. Klicken Sie auf **Elastische SQL-Pools**.
3. Klicken Sie in der Liste auf den Pool, den Sie verwenden möchten.

## Verschieben einer Datenbank in einen elastischen Pool

Sie können Datenbanken in einem vorhandenen Pool hinzufügen oder entfernen. Datenbanken können sich in verschiedenen Pools befinden. Sie können jedoch nur Datenbanken hinzufügen, die sich auf demselben logischen Server befinden.

1. Klicken Sie auf dem Blatt für den Pool unter **Elastische Datenbanken** auf **Pool konfigurieren**.

    ![Klicken auf „Pool konfigurieren“][1]

2. Klicken Sie auf dem Blatt **Pool konfigurieren** auf **Zu Pool hinzufügen**.

	![Klicken auf „Zu Pool hinzufügen“](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)

	
3. Wählen Sie auf dem Blatt **Datenbanken hinzufügen** die Datenbanken aus, die dem Pool hinzugefügt werden sollen. Klicken Sie dann auf **Auswählen**.

	![Auswählen hinzuzufügender Datenbanken](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    Auf dem Blatt **Pool konfigurieren** wird nun die soeben hinzugefügte Datenbank angezeigt. Der Status der Datenbank ist auf **Ausstehend** festgelegt.

    ![Ausstehende Hinzufügungen zum Pool](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. Klicken Sie auf dem Blatt „Pool konfigurieren“ auf **Speichern**.

    ![Klicken Sie auf Speichern.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## Verschieben einer Datenbank aus einem elastischen Pool

1. Wählen Sie auf dem Blatt **Pool konfigurieren** die zu entfernenden Datenbanken aus.

    ![Auflisten von Datenbanken](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Klicken Sie auf **Aus Pool entfernen**.

    ![Auflisten von Datenbanken](./media/sql-database-elastic-pool-manage-portal/remove-from-pool.png)

	Die ausgewählten Datenbanken werden in der Benutzeroberfläche als zum Entfernen ausgewählt angezeigt.


## Überwachen der Ressourcenverwendung eines Pools


1. Wählen Sie einen zu verwendenden Pool aus.
2. Unter **Überwachung des elastischen Pools** werden in einem Diagramm und in Live-Kacheln wichtige Auslastungsdaten zu Ihrem Pool angezeigt.

![Überwachen eines elastischen Pools](./media/sql-database-elastic-pool-manage-portal/monitor-elastic-pool.png)

**So ändern Sie das Diagramm und die Anzeige**

- Klicken Sie auf **Bearbeiten**.

	![Klicken auf „Bearbeiten“](./media/sql-database-elastic-pool-manage-portal/edit-resource-utlization.png)

- Wählen Sie auf dem Blatt **Diagramm bearbeiten** einen neuen Zeitbereich („letzte Stunde“, „heute“ oder „letzte Woche“) aus, oder klicken Sie auf **benutzerdefiniert**, um einen anderen Zeitraum festzulegen. Wählen Sie den Diagrammtyp („Balken“ oder „Linie“) und dann die zu überwachenden Ressourcen aus.

	![Klicken auf „Bearbeiten“](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

- Klicken Sie dann auf **OK**.


## Eine Warnung zu einer Poolressource hinzufügen

Sie können Regeln zu Ressourcen hinzufügen, die E-Mails an Personen verschicken, oder Warnzeichenfolgen zu einem URL-Endpunkt, wenn die Ressource einen Verwendungsschwellenwert erreicht, den Sie eingerichtet haben.

**So fügen Sie eine Warnung zu einer Ressource hinzu:**

1. Klicken Sie auf das Diagramm **Ressourcenverwendung**, um das Blatt **Metrik** zu öffnen. Klicken Sie auf **Warnung hinzufügen**, und füllen Sie dann die Informationen auf dem Blatt **Warnungsregel hinzufügen** aus (**Ressource** wird automatisch auf den Pool festgelegt, mit dem Sie arbeiten).
2. Geben Sie einen **Namen** und eine **Beschreibung** ein, die die Warnung Ihnen und anderen Empfängern gegenüber identifiziert.
3. Wählen Sie in der Liste eine **Metrik** aus, für die eine Warnung ausgegeben werden soll.

    Das Diagramm zeigt dynamisch die Ressourcenverwendung für diese Metrik an, um Ihnen die Festlegung eines Schwellenwerts zu erleichtern.

4. Wählen Sie eine **Bedingung** (größer als, kleiner als usw.) und einen **Schwellenwert** aus.
5. Klicken Sie auf **OK**.

## Ändern Sie die Leistungseinstellungen eines Pools

Bei der Überwachung der Ressourcenverwendung eines Pools stellen Sie möglicherweise fest, dass der Pool weitere eDTUs benötigt oder dass für einzelne Datenbanken im Pool unterschiedliche eDTU-Einstellungen erforderlich sind. Sie können das Setup des Pools jederzeit ändern, um die beste Leistungsverteilung und Kosteneffizienz zu erzielen. Weitere Informationen finden Sie unter [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md)

**So ändern Sie die eDTUs pro Pool und die eDTUs pro Datenbank**

1. Öffnen Sie das Blatt **Pool konfigurieren**.

    Ändern Sie mithilfe der Schieberegler unter **Einstellungen des elastischen Datenbankpools** die Pooleinstellungen.

    ![Ressourcenverwendung eines elastischen Pools](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Wenn sich die Einstellung ändert, werden in der Anzeige die geschätzten monatlichen Kosten der Änderung angezeigt.

    ![Einen Pool aktualisieren und neue monatliche Kosten](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## Anzeigen der Vorschau von Datenbankaktion

Sie können eine Vorschau der hinzugefügten und entfernten Datenbanken anzeigen, bevor Sie die Aktion auf dem Blatt **Pool konfigurieren** übernehmen:

![Vorschau-Datenbank hinzufügen und entfernen](./media/sql-database-elastic-pool-manage-portal/pools-tab.png).


## Erstellen und Verwalten von elastischen Aufträgen

Elastische Aufträge ermöglichen die Ausführung von Transact-SQL-Skripts für eine beliebige Anzahl von Datenbanken im Pool. Vor der Verwendung von Aufträgen installieren Sie die Komponenten für elastische Aufträge, und geben Sie Ihre Anmeldeinformationen an. Weitere Informationen finden Sie unter [Übersicht über elastische Datenbankaufträge](sql-database-elastic-jobs-overview.md).

Unter [Übersicht über Features für elastische Datenbanken](sql-database-elastic-scale-introduction.md) finden Sie Informationen zur Verwendung elastischer Datenbanktools für die horizontale Skalierung, zum Verschieben von Daten, für die Abfrage oder zum Erstellen von Transaktionen.


## Zusätzliche Ressourcen

- [Elastische SQL-Datenbankpools](sql-database-elastic-pool.md)
- [Erstellen eines Pools für elastische Datenbanken mit dem Portal](sql-database-elastic-pool-create-csharp.md)
- [Erstellen eines Pools für elastische Datenbanken mit PowerShell](sql-database-elastic-pool-create-powershell.md)
- [Erstellen eines Pools für elastische Datenbanken mit C#](sql-database-elastic-pool-create-csharp.md)
- [Überlegungen zum Preis und zur Leistung eines Pools für elastische Datenbanken](sql-database-elastic-pool-guidance.md)


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png

<!---HONumber=AcomDC_0504_2016-->