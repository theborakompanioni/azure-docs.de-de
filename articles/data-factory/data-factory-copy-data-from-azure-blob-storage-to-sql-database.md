<properties
	pageTitle="Kopieren von Daten aus Blob Storage in SQL-Datenbank | Microsoft Azure"
	description="In diesem Tutorial erfahren Sie, wie Sie die Kopieraktivität in einer Azure Data Factory-Pipeline verwenden, um Daten aus Blob Storage in SQL-Datenbank zu kopieren."
	Keywords="Blob-SQL, Blob Storage, Datenkopie"
	services="data-factory"
	documentationCenter=""
	authors="spelluru"
	manager="jhubbard"
	editor="monicar"/>

<tags
	ms.service="data-factory"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article" 
	ms.date="08/01/2016"
	ms.author="spelluru"/>

# Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory 
> [AZURE.SELECTOR]
- [Übersicht und Voraussetzungen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Kopier-Assistent](data-factory-copy-data-wizard-tutorial.md)

In diesem Tutorial erstellen Sie eine Data Factory mit einer Pipeline, um Daten aus Blob Storage in SQL-Datenbank zu kopieren.

Die Kopieraktivität dient zum Verschieben von Daten in Azure Data Factory. Sie basiert auf einem global verfügbaren Dienst, mit dem Daten zwischen verschiedenen Datenspeichern sicher, zuverlässig und skalierbar kopiert werden können. Ausführliche Informationen zur Kopieraktivität finden Sie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md).

> [AZURE.NOTE] Eine ausführliche Übersicht über den Data Factory-Dienst finden Sie im Artikel [Einführung in Azure Data Factory][data-factory-introduction].

##Voraussetzungen für das Tutorial
Bevor Sie mit diesem Lernprogramm beginnen können, benötigen Sie Folgendes:

- **Azure-Abonnement**. Wenn Sie über kein Abonnement verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Im Artikel [Kostenlose Testversion][azure-free-trial] finden Sie Details.
- **Azure Storage-Konto**. In diesem Tutorial verwenden Sie den Blobspeicher als **Quelldatenspeicher**. Wenn Sie kein Azure Storage-Konto haben, finden Sie im Artikel [Erstellen eines Speicherkontos][data-factory-create-storage] Schritte zum Erstellen eines Azure Storage-Kontos.
- **Azure SQL-Datenbank**. In diesem Tutorial verwenden Sie eine Azure SQL-Datenbank als **Zieldatenspeicher**. Wenn Sie keine Azure SQL-Datenbank haben, die Sie in diesem Tutorial verwenden können, finden Sie unter [Erstellen und Konfigurieren einer Azure SQL-Datenbank][data-factory-create-sql-database] entsprechende Anweisungen.
- **SQL Server 2012/2014 oder Visual Studio 2013**. Sie verwenden SQL Server Management Studio oder Visual Studio zum Erstellen einer Beispieldatenbank und zum Anzeigen der Ergebnisdaten in der Datenbank.

## Erfassen von Blob-Speicherkontoname und -schlüssel 
Sie benötigen in diesem Tutorial den Kontonamen und -schlüssel Ihres Azure-Speicherkontos. Notieren Sie sich den **Kontonamen** und den **Kontoschlüssel** Ihres Azure-Speicherkontos.

1. Melden Sie sich beim [Azure-Portal][azure-portal] an.
2. Klicken Sie auf der linken Seite auf den Hub **DURCHSUCHEN**, und wählen Sie **Speicherkonten** aus.
3. Wählen Sie auf dem Blatt **Speicherkonten** das **Azure-Speicherkonto** aus, das Sie in diesem Tutorial verwenden möchten.
4. Wählen Sie den Link **Zugriffsschlüssel** unter **EINSTELLUNGEN** aus.
5.  Klicken Sie auf die (Bild-)Schaltfläche **Kopieren** neben dem Textfeld **Speicherkontoname**, und speichern Sie den Eintrag z.B. in einer Textdatei.
6. Wiederholen Sie den vorherigen Schritt zum Kopieren oder Notieren von **key1**.
7. Schließen Sie alle Blätter, indem Sie auf **X** klicken.

## Erfassen der Namen von SQL Server, Datenbank und Benutzer
Sie benötigen in diesem Tutorial die Namen von Azure SQL-Server, -Datenbank und -Benutzer. Notieren Sie sich den Namen des **Servers**, der **Datenbank** und des **Benutzers** Ihrer Azure SQL-Datenbank.

1. Klicken Sie links im **Azure-Portal** auf **DURCHSUCHEN**, und wählen Sie **SQL-Datenbanken** aus.
2. Wählen Sie auf dem Blatt **SQL-Datenbanken** die **Datenbank**, die Sie in diesem Tutorial verwenden möchten. Notieren Sie sich den **Datenbanknamen**.
3. Klicken Sie auf dem Blatt **SQL-DATENBANK** auf die Kachel **EIGENSCHAFTEN**.
4. Notieren Sie sich die Werte für **SERVERNAME** und **SERVER-ADMIN-ANMELDUNG**.
5. Schließen Sie alle Blätter, indem Sie auf **X** klicken.

## Gewähren des Zugriffs auf SQL Server für Azure-Dienste 
Stellen Sie sicher, dass die Einstellung **Zugriff auf Azure-Dienste erlauben** für Ihren Azure SQL-Server **aktiviert** ist, damit der Data Factory-Dienst auf Ihren Azure SQL-Server zugreifen kann. Führen Sie folgende Schritte aus, um diese Einstellung zu überprüfen und zu aktivieren:

1. Klicken Sie links auf den Hub **DURCHSUCHEN** und dann auf **SQL-Server**.
2. Wählen Sie **Ihren Server** aus, und klicken Sie auf dem Blatt **SQL SERVER** auf **EINSTELLUNGEN**.
3. Klicken Sie auf dem Blatt **EINSTELLUNGEN** auf **Firewall**.
4. Klicken Sie auf dem Blatt **Firewalleinstellungen** für **Zugriff auf Azure-Dienste erlauben** auf **EIN**.
5. Schließen Sie alle Blätter, indem Sie auf **X** klicken.

## Vorbereiten von Blob Storage und SQL-Datenbank 
Nun bereiten Sie Ihrer Azure-Blobspeicher und die Azure SQL-Datenbank durch Ausführen der folgenden Schritte auf das Lernprogramm vor:

1. Öffnen Sie den Editor, fügen Sie den folgenden Text ein, und speichern Sie die Datei als **emp.txt** im Ordner **C:\\ADFGetStarted** auf Ihrer Festplatte.

        John, Doe
		Jane, Doe

2. Verwenden Sie Tools wie [Azure Storage-Explorer](https://azurestorageexplorer.codeplex.com/) zum Erstellen des Containers **adftutorial** und zum Hochladen der Datei **emp.txt** in den Container.

    ![Azure-Speicher-Explorer. Kopieren von Daten aus Blob Storage in SQL-Datenbank](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Verwenden Sie das folgende SQL-Skript, um die Tabelle **emp** in der Azure SQL-Datenbank zu erstellen.


        CREATE TABLE dbo.emp
		(
			ID int IDENTITY(1,1) NOT NULL,
			FirstName varchar(50),
			LastName varchar(50),
		)
		GO

		CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

	**Wenn Sie SQL Server 2012/2014 auf Ihrem Computer installiert haben:** Befolgen Sie die Anweisungen unter "Schritt 2: Herstellen einer Verbindung mit SQL-Datenbank" im Artikel [Verwalten einer Azure SQL-Datenbank mit SQL Server Management Studio][sql-management-studio], um eine Verbindung mit Ihrem Azure SQL-Server herzustellen und das SQL-Skript auszuführen. In diesem Artikel wird nicht das [neue Azure-Portal](https://portal.azure.com), sondern das [klassische Azure-Portal](http://manage.windowsazure.com) verwendet, um die Firewall für einen Azure SQL-Server zu konfigurieren.

	Wenn Ihr Client nicht auf den Azure SQL-Server zugreifen darf, müssen Sie die Firewall für Ihren Azure SQL-Server so konfigurieren, dass der Zugriff über Ihren Computer (bzw. dessen IP-Adresse) ermöglicht wird. Schritte zum Konfigurieren der Firewall für Ihren Azure SQL-Server finden Sie in [diesem Artikel](../sql-database/sql-database-configure-firewall-settings.md).

Sie haben die Voraussetzungen erfüllt. Klicken Sie oben auf eine Registerkarte, um das Tutorial mit einer der folgenden Optionen auszuführen:

- Azure-Portal
- Visual Studio
- PowerShell
- REST-API
- .NET API
- Kopier-Assistent

<!--Link references-->
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[sql-management-studio]: http://azure.microsoft.com/documentation/articles/sql-database-manage-azure-ssms/#Step2

[data-factory-introduction]: data-factory-introduction.md
[data-factory-create-storage]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account
[data-factory-create-sql-database]: ../sql-database/sql-database-get-started.md

<!---HONumber=AcomDC_0921_2016-->