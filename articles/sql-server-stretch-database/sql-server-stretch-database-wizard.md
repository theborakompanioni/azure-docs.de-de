<properties
	pageTitle="Ausführen des Assistenten zum Aktivieren einer Datenbank für Stretch | Microsoft Azure"
	description="Erfahren Sie, wie eine Datenbank für Stretch-Datenbank konfigurieren, indem Sie den Assistenten zum Aktivieren einer Datenbank für Stretch ausführen."
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
	ms.topic="hero-article"
	ms.date="02/26/2016"
	ms.author="douglasl"/>

# Ausführen des Assistenten zum Aktivieren einer Datenbank für Stretch

Führen Sie zum Konfigurieren einer Datenbank für Stretch-Datenbank den Assistenten zum Aktivieren einer Datenbank für Stretch aus. In diesem Thema werden die Informationen beschrieben, die Sie eingeben, und die Optionen, die Sie im Assistenten festlegen müssen.

Weitere Informationen zu Stretch-Datenbank finden Sie unter [Stretch-Datenbank](sql-server-stretch-database-overview.md).

## Starten des Assistenten

1.  Wählen Sie in SQL Server Management Studio im Objekt-Explorer die Datenbank aus, für die Stretch aktiviert werden soll.

2.  Klicken Sie mit der rechten Maustaste, und wählen Sie **Aufgaben** und dann **Stretch** aus. Wählen Sie dann **Aktivieren**, um den Assistenten zu starten.

## <a name="Intro"></a>Einführung
Überprüfen Sie den Zweck des Assistenten und die Voraussetzungen.

![Einführungsseite des Assistenten der Stretch-Datenbank][StretchWizardImage1]

## <a name="Tables"></a>Auswählen von Tabellen
Wählen Sie die Tabellen, die Sie für Stretch aktivieren möchten.

![Auswahl der Tabellenseite des Assistenten der Stretch-Datenbank][StretchWizardImage2]

|Column|Beschreibung|
|----------|---------------|
|(ohne Titel)|Aktivieren Sie das Kontrollkästchen in dieser Spalte, um die ausgewählte Tabelle für Stretch zu aktivieren.|
|**Name**|Gibt den Namen der Spalte in der Tabelle an.|
|(ohne Titel)|Ein Symbol in dieser Spalte gibt normalerweise an, dass Sie die ausgewählte Tabelle aufgrund eines Hindernisses nicht für Stretch aktivieren können. Möglicherweise verwendet die Tabelle einen nicht unterstützten Datentyp. Zeigen Sie auf das Symbol, um weitere Informationen in einer QuickInfo anzuzeigen. Weitere Informationen finden Sie unter [Oberflächeneinschränkungen und Hindernisse für Stretch-Datenbank](sql-server-stretch-database-limitations.md).|
|**Gestreckt**|Gibt an, ob die Tabelle bereits aktiviert ist.|
|**Migrieren**|In RC3 können Sie eine ganze Tabelle (**Ganze Tabelle**) migrieren oder ein datumsbasiertes Filterprädikat im Assistenten festlegen. Wenn Sie ein anderes Filterprädikat verwenden möchten, um die zu migrierenden Spalten auszuwählen, führen Sie die Anweisung ALTER TABLE aus, um das Filterprädikat nach Beenden des Assistenten festzulegen. Weitere Informationen zum Filterprädikat finden Sie unter [Use a filter predicate to select rows to migrate (Stretch Database)](sql-server-stretch-database-predicate-function.md) (Verwenden eines Filterprädikats, um die zu migrierenden Zeilen auszuwählen (Stretch-Datenbank)). Weitere Informationen zum Anwenden des Prädikats finden Sie unter [Enable Stretch Database for a table](sql-server-stretch-database-enable-table.md) (Aktivieren von Stretch-Datenbank für eine Tabelle) oder [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**beibehalten**|Gibt die Anzahl der Zeilen in der Tabelle an|
|**Größe (KB)**|Gibt die Größe der Tabelle in KB an.|

## <a name="Filter"></a>Geben Sie optional ein datenbasiertes Filterprädikat an.

Wenn Sie zum Auswählen zu migrierender Spalten ein datenbasiertes Filterprädikat angeben möchten, führen Sie auf der Seite **Tabellen auswählen** die folgenden Schritte aus.

1.  Klicken Sie in der Liste **Zu streckende Tabellen auswählen** in der Spalte für die Tabelle auf **Ganze Tabelle**. Das Dialogfeld **Zu streckende Zeilen auswählen** wird geöffnet.

    ![Definieren eines datenbasiertes Filterprädikats][StretchWizardImage2a]

2.  Wählen Sie im Dialogfeld **Zu streckende Zeilen auswählen** die Option **Zeilen auswählen** aus.

3.  Geben Sie im **Namensfeld** einen Namen für das Filterprädikat an.

4.  Wählen Sie für die **WHERE**-Klausel eine Datumsspalte der Tabelle und anschließend einen Operator aus, und geben Sie einen Datumswert an.

5. Klicken Sie zum Prüfen des Prädikats auf **Überprüfen**. Wenn das Prädikat aus der Tabelle Ergebnisse zurückgibt (d.h. wenn zu migrierende Zeilen vorhanden sind, auf die die Bedingung zutrifft), meldet der Test **Erfolg**.

6.  Klicken Sie auf „Fertig“, um zur Seite **Tabellen auswählen** zurückzukehren.

    ![Seite „Tabellen auswählen“ nach dem Definieren des Filterprädikats][StretchWizardImage2b]

## <a name="Configure"></a>Konfigurieren der Azure-Bereitstellung

1.  Melden Sie sich bei Microsoft Azure mit einem Microsoft-Konto an.

    ![Anmelden bei Azure – Assistent der Stretch-Datenbank][StretchWizardImage3]

2.  Wählen Sie das Azure-Abonnement für Stretch-Datenbank aus.

3.  Wählen Sie eine Azure-Region. Wenn Sie einen neuen Server erstellen, wird der Server in dieser Region erstellt.

    Um Latenz zu minimieren, wählen Sie die Azure-Region, in der sich Ihr SQL Server befindet. Weitere Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).

4.  Geben Sie an, ob Sie einen vorhandenen Server verwenden oder einen neuen Azure-Server erstellen möchten.

    Wenn Active Directory auf dem SQL Server mit Azure Active Directory verbunden ist, können Sie optional ein Verbunddienstkonto für SQL Server für die Kommunikation mit dem Azure-Remoteserver verwenden. Weitere Informationen zu den Anforderungen für diese Option finden Sie unter [ALTER DATABASE SET-Optionen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

	-   **Erstellen eines neuen Servers**

        1.  Erstellen Sie einen Benutzernamen und ein Kennwort für den Serveradministrator.

        2.  Wählen Sie optional ein Verbunddienstkonto für SQL Server für die Kommunikation mit dem Azure-Remoteserver aus.

		![Erstellen eines neuen Azure-Server – Assistent der Stretch-Datenbank][StretchWizardImage4]

    -   **Vorhandener Server**

        1.  Wählen Sie den Namen des vorhandenen Azure-Server aus, oder geben Sie ihn ein.

        2.  Wählen Sie die Authentifizierungsmethode aus.

            -   Wenn Sie die Option **SQL Server-Authentifizierung** auswählen, erstellen Sie einen neuen Benutzernamen und ein neues Kennwort.

            -   Wählen Sie **Integrierte Active Directory-Authentifizierung** aus, um ein Verbunddienstkonto für SQL Server für die Kommunikation mit dem Azure-Remoteserver zu verwenden.

		![Auswählen eines vorhandenen Azure-Servers – Assistent der Stretch-Datenbank][StretchWizardImage5]

## <a name="Credentials"></a>Sichere Anmeldeinformationen
Geben Sie ein sicheres Kennwort zum Erstellen eines Datenbankhauptschlüssels ein, oder geben Sie das Kennwort ein, wenn bereits ein Datenbankhauptschlüssel vorhanden ist.

Sie brauchen einen Datenbankhauptschlüssel, um die Anmeldeinformationen zu sichern, die Stretch-Datenbank für die Verbindung mit der Remotedatenbank verwendet.

![Seite mit den sicheren Anmeldeinformationen des Assistenten der Stretch-Datenbank][StretchWizardImage6]

Weitere Informationen zum Datenbankhauptschlüssel finden Sie unter [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) und [Erstellen eines Datenbankhauptschlüssels](https://msdn.microsoft.com/library/aa337551.aspx). Weitere Informationen über die Anmeldeinformationen, die der Assistent erstellt, finden Sie unter [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>Auswählen der IP-Adresse
Verwenden Sie die öffentliche IP-Adresse von SQL Server, oder geben Sie einen IP-Adressbereich ein, um eine Firewallregel für Azure zu erstellen, damit SQL Server mit dem Azure-Remoteserver kommunizieren kann.

Die auf dieser Seite angegebenen IP-Adressen informieren den Azure-Server darüber, dass eingehende Daten, Abfragen und Verwaltungsvorgänge, die von SQL Server initiiert werden, die Azure-Firewall durchlaufen können. Der Assistent ändert keine Einstellungen der Firewall auf SQL Server.

![Auswählen der IP-Adressseite des Assistenten der Stretch-Datenbank][StretchWizardImage7]

## <a name="Summary"></a>Zusammenfassung
Überprüfen Sie die Werte, die Sie eingegeben haben, und die im Assistenten ausgewählten Optionen. Wählen Sie dann **Fertig stellen**, um Stretch zu aktivieren.

![Zusammenfassungsseite des Assistenten der Stretch-Datenbank][StretchWizardImage8]

## <a name="Results"></a>Ergebnisse
Überprüfen Sie die Ergebnisse.

Wählen Sie optional **Überwachen** aus, um den Status der Datenmigration im Stretch Database Monitor zu überwachen. Weitere Informationen finden Sie unter [Monitor and troubleshoot data migration (Stretch Database)](sql-server-stretch-database-monitor.md) (Überwachen und Behandeln von Problemen der Datenmigration (Stretch-Datenbank)).

## <a name="KnownIssues"></a>Problembehandlung für den Assistenten
**Der Assistent für Stretch-Datenbank ist fehlgeschlagen.** Wenn Stretch-Datenbank noch nicht auf Serverebene aktiviert ist und Sie den Assistenten ohne Systemadministratorberechtigungen zum Aktivieren ausführen, schlägt der Assistent fehl. Bitten Sie den Systemadministrator, Stretch-Datenbank auf der lokalen Serverinstanz zu aktivieren, und führen Sie den Assistenten dann erneut aus. Weitere Informationen finden Sie unter [Prerequisite: Permission to enable Stretch Database on the server](sql-server-stretch-database-enable-database.md#EnableTSQLServer) (Voraussetzung: Berechtigung zum Aktivieren von Stretch-Datenbank auf dem Server).

## Nächste Schritte
Aktivieren von zusätzlichen Tabellen für Stretch-Datenbank Überwachen der Datenmigration und Verwalten von Stretch-fähigen Datenbanken und Tabellen

-   [Enable Stretch Database for a table](sql-server-stretch-database-enable-table.md) (Aktivieren von Stretch-Datenbank für eine Tabelle) zum Aktivieren zusätzlicher Tabellen

-   [Monitor Stretch Database](sql-server-stretch-database-monitor.md) (Überwachen von Stretch-Datenbank) zum Anzeigen des Status der Datenmigration

-   [Anhalten und Fortsetzen von Stretch-Datenbank](sql-server-stretch-database-pause.md)

-   [Verwalten von Stretch-Datenbank und Behandeln von Problemen ](sql-server-stretch-database-manage.md)

-   [Sichern und Wiederherstellen von Stretch-fähigen Datenbanken](sql-server-stretch-database-backup.md)

## Weitere Informationen

[Aktivieren von Stretch-Datenbank für eine Datenbank](sql-server-stretch-database-enable-database.md)

[Aktivieren von Stretch-Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png

<!---HONumber=AcomDC_0420_2016-->