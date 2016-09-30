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
	ms.date="08/05/2016"
	ms.author="douglasl"/>

# Ausführen des Assistenten zum Aktivieren einer Datenbank für Stretch

Führen Sie zum Konfigurieren einer Datenbank für Stretch-Datenbank den Assistenten zum Aktivieren einer Datenbank für Stretch aus. In diesem Thema werden die Informationen beschrieben, die Sie eingeben, und die Optionen, die Sie im Assistenten festlegen müssen.

Weitere Informationen zu Stretch-Datenbank finden Sie unter [Stretch-Datenbank](sql-server-stretch-database-overview.md).

 >   [AZURE.NOTE] Bedenken Sie später beim Deaktivieren von Stretch-Datenbank Folgendes: Wenn Sie Stretch-Datenbank für eine Tabelle oder eine Datenbank deaktivieren, wird das Remoteobjekt nicht gelöscht. Wenn Sie die Remotetabelle oder die Remotedatenbank löschen möchten, müssen Sie sie mithilfe des Azure-Verwaltungsportals entfernen. Für die Remoteobjekte fallen weiterhin Azure-Kosten an, bis Sie die Objekte manuell löschen.

## Starten des Assistenten

1.  Wählen Sie in SQL Server Management Studio im Objekt-Explorer die Datenbank aus, für die Stretch aktiviert werden soll.

2.  Klicken Sie mit der rechten Maustaste, wählen Sie **Aufgaben** > **Stretch** und anschließend **Aktivieren** aus, um den Assistenten zu starten.

## <a name="Intro"></a>Einführung
Überprüfen Sie den Zweck des Assistenten und die Voraussetzungen.

Zu diesen wichtigen Voraussetzungen gehört Folgendes:

-   Sie müssen ein Administrator sein, um die Datenbankeinstellungen zu ändern.
-   Sie müssen über ein Microsoft Azure-Abonnement verfügen.
-   Ihre SQL Server-Instanz muss mit dem Azure-Remoteserver kommunizieren können.

![Einführungsseite des Assistenten der Stretch-Datenbank][StretchWizardImage1]

## <a name="Tables"></a>Auswählen von Tabellen
Wählen Sie die Tabellen, die Sie für Stretch aktivieren möchten.

Tabellen mit vielen Zeilen werden in der sortierten Liste oben angezeigt. Bevor der Assistent die Liste mit den Tabellen anzeigt, werden diese in Bezug auf Datentypen analysiert, die derzeit von Stretch-Datenbank nicht unterstützt werden.

![Auswahl der Tabellenseite des Assistenten der Stretch-Datenbank][StretchWizardImage2]

|Column|Beschreibung|
|----------|---------------|
|(ohne Titel)|Aktivieren Sie das Kontrollkästchen in dieser Spalte, um die ausgewählte Tabelle für Stretch zu aktivieren.|
|**Name**|Gibt den Namen der Spalte in der Tabelle an.|
|(ohne Titel)|Ein Symbol in dieser Spalte kann eine Warnung darstellen, durch die nicht verhindert wird, dass Sie die ausgewählte Tabelle für Stretch aktivieren. Es kann auch für ein Hindernis stehen, durch das die Aktivierung der ausgewählten Tabelle für Stretch verhindert wird. Der Grund kann beispielsweise sein, dass in der Tabelle ein nicht unterstützter Datentyp verwendet wird. Zeigen Sie auf das Symbol, um weitere Informationen in einer QuickInfo anzuzeigen. Weitere Informationen finden Sie unter [Einschränkungen für Stretch-Datenbank](sql-server-stretch-database-limitations.md).|
|**Gestreckt**|Gibt an, ob die Tabelle bereits für Stretch aktiviert ist.|
|**Migrieren**|Sie können eine gesamte Tabelle migrieren (**Ganze Tabelle**) oder einen Filter für eine vorhandene Spalte in der Tabelle angeben. Wenn Sie eine andere Filterfunktion verwenden möchten, um die zu migrierenden Spalten auszuwählen, führen Sie die Anweisung ALTER TABLE aus, um die Filterfunktion nach dem Beenden des Assistenten festzulegen. Weitere Informationen zur Filterfunktion finden Sie unter [Auswählen von Zeilen für die Migration mit einer Filterfunktion](sql-server-stretch-database-predicate-function.md). Weitere Informationen zum Anwenden der Funktion finden Sie unter [Aktivieren von Stretch-Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md) oder [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Zeilen**|Gibt die Anzahl der Zeilen in der Tabelle an.|
|**Größe (KB)**|Gibt die Größe der Tabelle in KB an.|

## <a name="Filter"></a>Optionales Bereitstellen eines Zeilenfilters

Führen Sie auf der Seite **Tabellen auswählen** die folgenden Schritte aus, wenn Sie eine Filterfunktion zum Auswählen der zu migrierenden Spalten angeben möchten.

1.  Klicken Sie in der Liste **Wählen Sie die Tabellen aus, die Sie strecken möchten.** in der Spalte für die Tabelle auf **Ganze Tabelle**. Das Dialogfeld **Wählen Sie die zu streckenden Zeilen aus.** wird geöffnet.

    ![Definieren einer Filterfunktion][StretchWizardImage2a]

2.  Wählen Sie im Dialogfeld **Wählen Sie die zu streckenden Zeilen aus.** die Option **Zeilen auswählen** aus.

3.  Geben Sie im Feld **Name** einen Namen für die Filterfunktion an.

4.  Wählen Sie für die **Where**-Klausel eine Spalte der Tabelle und anschließend einen Operator aus, und geben Sie einen Wert an.

5. Klicken Sie zum Prüfen der Funktion auf **Überprüfen**. Wenn die Funktion Ergebnisse aus der Tabelle zurückgibt (also zu migrierende Zeilen vorhanden sind, auf die die Bedingung zutrifft), wird **Erfolgreich** zurückgegeben.

    >   [AZURE.NOTE] Das Textfeld, in dem die Filterabfrage angezeigt wird, ist schreibgeschützt. Es ist nicht möglich, die Abfrage im Textfeld zu bearbeiten.

6.  Klicken Sie auf „Fertig“, um zur Seite **Tabellen auswählen** zurückzukehren.

Die Filterfunktion wird in SQL Server nur erstellt, wenn Sie den Assistenten abschließen. Bis zu diesem Punkt können Sie zur Seite **Tabellen auswählen** zurückkehren, um die Filterfunktion zu ändern oder umzubenennen.

![Seite „Tabellen auswählen“ nach dem Definieren einer Filterfunktion][StretchWizardImage2b]

Führen Sie einen der folgenden Schritte aus, wenn Sie eine andere Art von Filterfunktion für die Auswahl der zu migrierenden Zeilen verwenden möchten.

-   Beenden Sie den Assistenten, und führen Sie die ALTER TABLE-Anweisung aus, um Stretch für die Tabelle zu aktivieren und eine Filterfunktion anzugeben. Weitere Informationen finden Sie unter [Aktivieren von Stretch-Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md).

-   Führen Sie die ALTER TABLE-Anweisung aus, um eine Filterfunktion anzugeben, nachdem Sie den Assistenten beendet haben. Die erforderlichen Schritte finden Sie unter [Hinzufügen einer Filterfunktion nach dem Ausführen des Assistenten](sql-server-stretch-database-predicate-function.md#addafterwiz).

## <a name="Configure"></a>Konfigurieren der Azure-Bereitstellung

1.  Melden Sie sich bei Microsoft Azure mit einem Microsoft-Konto an.

    ![Anmelden bei Azure – Assistent der Stretch-Datenbank][StretchWizardImage3]

2.  Wählen Sie das vorhandene Azure-Abonnement aus, das für Stretch-Datenbank verwendet werden soll.

3.  Wählen Sie eine Azure-Region.
    -   Wenn Sie einen neuen Server erstellen, wird der Server in dieser Region erstellt.
    -   Falls Sie über Server in der ausgewählten Region verfügen, werden diese vom Assistenten aufgeführt, wenn Sie **Vorhandener Server** auswählen.

    Um Latenz zu minimieren, wählen Sie die Azure-Region, in der sich Ihr SQL Server befindet. Weitere Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).

4.  Geben Sie an, ob Sie einen vorhandenen Server verwenden oder einen neuen Azure-Server erstellen möchten.

    Wenn Active Directory auf dem SQL Server mit Azure Active Directory verbunden ist, können Sie optional ein Verbunddienstkonto für SQL Server für die Kommunikation mit dem Azure-Remoteserver verwenden. Weitere Informationen zu den Anforderungen für diese Option finden Sie unter [ALTER DATABASE SET-Optionen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

	-   **Erstellen eines neuen Servers**

        1.  Erstellen Sie einen Benutzernamen und ein Kennwort für den Serveradministrator.

        2.  Wählen Sie optional ein Verbunddienstkonto für SQL Server für die Kommunikation mit dem Azure-Remoteserver aus.

		![Erstellen eines neuen Azure-Server – Assistent der Stretch-Datenbank][StretchWizardImage4]

    -   **Vorhandener Server**

        1.  Wählen Sie den vorhandenen Azure-Server aus.

        2.  Wählen Sie die Authentifizierungsmethode aus.

            -   Geben Sie bei Verwendung der Option **SQL Server-Authentifizierung** den Administratorbenutzernamen und das dazugehörige Kennwort an.

            -   Wählen Sie **Integrierte Active Directory-Authentifizierung** aus, um für die Kommunikation mit dem Azure-Remoteserver ein Verbunddienstkonto für SQL Server zu verwenden. Diese Option wird nicht angezeigt, wenn der ausgewählte Server nicht in Azure Active Directory integriert ist.

		![Auswählen eines vorhandenen Azure-Servers – Assistent der Stretch-Datenbank][StretchWizardImage5]

## <a name="Credentials"></a>Sichere Anmeldeinformationen
Sie brauchen einen Datenbankhauptschlüssel, um die Anmeldeinformationen zu sichern, die Stretch-Datenbank für die Verbindung mit der Remotedatenbank verwendet.

Falls bereits ein Hauptschlüssel vorhanden ist, müssen Sie das Kennwort dafür eingeben.

![Seite mit den sicheren Anmeldeinformationen des Assistenten der Stretch-Datenbank][StretchWizardImage6b]

Wenn die Datenbank keinen vorhandenen Hauptschlüssel aufweist, müssen Sie ein sicheres Kennwort eingeben, um einen Datenbank-Hauptschlüssel zu erstellen.

![Seite mit den sicheren Anmeldeinformationen des Assistenten der Stretch-Datenbank][StretchWizardImage6]

Weitere Informationen zum Datenbankhauptschlüssel finden Sie unter [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) sowie unter [Erstellen eines Datenbankhauptschlüssels](https://msdn.microsoft.com/library/aa337551.aspx). Weitere Informationen zu den Anmeldeinformationen, die der Assistent erstellt, finden Sie unter [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>Auswählen der IP-Adresse
Verwenden Sie den Subnetz-IP-Adressbereich (empfohlen) oder die öffentliche IP-Adresse Ihrer SQL Server-Instanz, um eine Firewallregel für Azure zu erstellen, damit SQL Server mit dem Azure-Remoteserver kommunizieren kann.

Die auf dieser Seite angegebenen IP-Adressen informieren den Azure-Server darüber, dass eingehende Daten, Abfragen und Verwaltungsvorgänge, die von SQL Server initiiert werden, die Azure-Firewall durchlaufen können. Der Assistent ändert keine Einstellungen der Firewall auf SQL Server.

![Auswählen der IP-Adressseite des Assistenten der Stretch-Datenbank][StretchWizardImage7]

## <a name="Summary"></a>Zusammenfassung
Überprüfen Sie die Werte, die Sie eingegeben haben, und die im Assistenten ausgewählten Optionen sowie die geschätzten Kosten in Azure. Wählen Sie dann **Fertig stellen** aus, um Stretch zu aktivieren.

![Zusammenfassungsseite des Assistenten der Stretch-Datenbank][StretchWizardImage8]

## <a name="Results"></a>Ergebnisse
Überprüfen Sie die Ergebnisse.

Informationen zum Überwachen des Status der Datenmigration finden Sie unter [Überwachen und Behandeln von Problemen der Datenmigration (Stretch-Datenbank)](sql-server-stretch-database-monitor.md).

![Ergebnisseite des Assistenten der Stretch-Datenbank][StretchWizardImage9]

## <a name="KnownIssues"></a>Problembehandlung für den Assistenten
**Der Assistent für Stretch-Datenbank ist fehlgeschlagen.** Wenn Stretch-Datenbank noch nicht auf Serverebene aktiviert ist und Sie den Assistenten ohne Systemadministratorberechtigungen zum Aktivieren ausführen, schlägt der Assistent fehl. Bitten Sie den Systemadministrator, Stretch-Datenbank auf der lokalen Serverinstanz zu aktivieren, und führen Sie den Assistenten dann erneut aus. Weitere Informationen finden Sie unter [Voraussetzung: Berechtigung zum Aktivieren von Stretch-Datenbank auf dem Server](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## Nächste Schritte
Aktivieren von zusätzlichen Tabellen für Stretch-Datenbank Überwachen der Datenmigration und Verwalten von Stretch-fähigen Datenbanken und Tabellen

-   [Aktivieren von Stretch-Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md) zum Aktivieren zusätzlicher Tabellen

-   [Überwachen und Behandeln von Problemen der Datenmigration](sql-server-stretch-database-monitor.md) zum Anzeigen des Status der Datenmigration

-   [Anhalten und Fortsetzen von Stretch-Datenbank](sql-server-stretch-database-pause.md)

-   [Verwalten von Stretch-Datenbank und Behandeln von Problemen ](sql-server-stretch-database-manage.md)

-   [Sichern von Stretch-fähigen Datenbanken](sql-server-stretch-database-backup.md)

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
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png

<!----HONumber=AcomDC_0810_2016--->