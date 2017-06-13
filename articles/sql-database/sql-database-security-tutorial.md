---
title: "Schützen der Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die Methoden und Funktionen zum Schützen Ihrer Azure SQL-Datenbank."
services: sql-database
documentationcenter: 
author: DRediske
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/07/2017
ms.author: daredis
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 34815f5b716a38f957392d8955f924eeb6fe621e
ms.contentlocale: de-de
ms.lasthandoff: 05/09/2017


---
# <a name="secure-your-azure-sql-database"></a>Schützen der Azure SQL-Datenbank

SQL-Datenbank schützt Ihre Daten, indem der Zugriff auf Ihre Datenbank beschränkt wird. Hierfür wird Folgendes verwendet: Firewallregeln, Authentifizierungsmechanismen, bei denen Benutzer ihre Identität nachweisen müssen, und Datenautorisierung über rollenbasierte Mitgliedschaften und Berechtigungen sowie mithilfe von Sicherheit auf Zeilenebene und dynamischer Datenmaskierung.

Sie können den Schutz Ihrer Datenbank vor schädlichen Benutzern oder nicht autorisiertem Zugriff mit wenigen einfachen Schritten verbessern. In diesem Tutorial lernen Sie Folgendes: 

> [!div class="checklist"]
> * Einrichten von Firewallregeln für den Server oder die Datenbank
> * Herstellen einer Verbindung mit Ihrer Datenbank mithilfe einer sicheren Verbindungszeichenfolge
> * Verwalten des Benutzerzugriffs
> * Schützen von Daten durch Verschlüsselung
> * Aktivieren der SQL-Datenbanküberwachung
> * Aktivieren der Bedrohungserkennung der SQL-Datenbank

Stellen Sie zur Durchführung dieses Tutorials sicher, dass Sie Excel und die neueste Version von [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) installiert haben.


## <a name="set-up-firewall-rules-for-your-database"></a>Einrichten von Firewallregeln für die Datenbank

SQL-Datenbanken werden in Azure durch eine Firewall geschützt. Standardmäßig werden alle Verbindungen zum Server und den Datenbanken im Server abgelehnt, es sei denn, es handelt sich um Verbindungen von anderen Azure-Diensten. Die sicherste Konfiguration besteht darin, „Zugriff auf Azure-Dienste erlauben“ auf „AUS“ festzulegen. Wenn Sie eine Verbindung zwischen der Datenbank und einer Azure-VM oder einem Clouddienst herstellen müssen, sollten Sie eine [reservierte IP](../virtual-network/virtual-networks-reserved-public-ip.md) erstellen und nur den Zugriff auf die reservierte IP-Adresse durch die Firewall zulassen. 

Führen Sie die folgenden Schritte aus, um für den Server eine [SQL-Datenbank-Firewallregel auf Serverebene](sql-database-firewall-configure.md) zu erstellen, damit Verbindungen von einer bestimmten IP-Adresse zugelassen werden. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
1. Klicken Sie im linken Menü auf **SQL-Datenbanken** und anschließend auf die Datenbank, für die Sie die Firewallregel auf der Seite **SQL-Datenbanken** konfigurieren möchten. Die Übersichtsseite für Ihre Datenbank wird geöffnet, die den vollqualifizierten Servernamen (z.B. **mynewserver20170313.database.windows.net**) und Optionen für die weitere Konfiguration enthält.

      ![Serverfirewallregel](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. Klicken Sie auf der Symbolleiste wie in der obigen Abbildung dargestellt auf **Serverfirewall festlegen**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet. 

3. Klicken Sie auf der Symbolleiste auf **Client-IP hinzufügen**, um die öffentliche IP-Adresse des mit dem Portal verbundenen Computers hinzuzufügen, oder geben Sie die Firewallregel manuell ein. Klicken Sie anschließend auf **Speichern**.

      ![Festlegen der Serverfirewallregel](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. Klicken Sie auf **OK** und dann auf das **X**, um die Seite mit den **Firewalleinstellungen** zu schließen.

Sie können nun eine Verbindung zu einer beliebigen Datenbank auf dem Server mit der angegebenen IP-Adresse oder dem angegebenen IP-Adressbereich herstellen.

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet.
>

Wenn Sie andere Firewalleinstellungen für verschiedene Datenbanken auf demselben logischen Server benötigen, müssen Sie für jede Datenbank eine Regel auf Datenbankebene erstellen. Firewallregeln auf Datenbankebene können nur mithilfe von Transact-SQL-Anweisungen und erst dann konfiguriert werden, nachdem Sie die erste Firewallregel auf Serverebene konfiguriert haben. Führen Sie folgende Schritte aus, um eine datenbankspezifische Firewallregel zu erstellen.

1. Stellen Sie eine Verbindung mit Ihrer Datenbank her, z.B. mithilfe von [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

2. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Datenbank, der eine Firewallregel hinzugefügt werden soll. Klicken Sie dann auf **Neue Abfrage**. Ein leeres Abfragefenster mit einer Verbindung mit Ihrer Datenbank wird geöffnet.

3. Geben Sie im Abfragefenster die folgende Abfrage ein:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. Klicken Sie auf der Symbolleiste auf **Ausführen**, um die Firewallregel zu erstellen.

## <a name="connect-to-your-database-using-a-secure-connection-string"></a>Herstellen einer Verbindung mit Ihrer Datenbank mithilfe einer sicheren Verbindungszeichenfolge

Um für eine sichere, verschlüsselte Verbindung zwischen dem Client und der SQL-Datenbank zu sorgen, muss die Verbindungszeichenfolge so konfiguriert werden, dass 1) eine verschlüsselte Verbindung angefordert und 2) dem Serverzertifikat nicht vertraut wird. Hierdurch wird eine Verbindung mithilfe von Transport Layer Security (TLS) hergestellt und das Risiko von Man-in-the-Middle-Angriffen verringert. Wie für ADO.NET in diesem Screenshot dargestellt, können Sie korrekt konfigurierte Verbindungszeichenfolgen für Ihre SQL-Datenbank für unterstützte Clienttreiber aus dem Azure-Portal abrufen.

1. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank.

2. Klicken Sie auf der Seite **Überblick** Ihrer Datenbank auf **Datenbank-Verbindungszeichenfolgen anzeigen**.

3. Überprüfen Sie die vollständige **ADO.NET**-Verbindungszeichenfolge.

    ![ADO.NET-Verbindungszeichenfolge](./media/sql-database-security-tutorial/adonet-connection-string.png)


## <a name="user-management"></a>Benutzerverwaltung

Vor der Erstellung von Benutzern müssen Sie zuerst einen der beiden Authentifizierungstypen wählen, die von der Azure SQL-Datenbank unterstützt werden: 

**SQL-Authentifizierung**, bei der Benutzername und Kennwort für Anmeldungen und Benutzer verwendet werden, die nur im Kontext einer bestimmten Datenbank auf einem logischen Server gültig sind. 

**Azure Active Directory-Authentifizierung**, bei der von Azure Active Directory verwaltete Identitäten verwendet werden. 

Wenn Sie [Azure Active Directory](./sql-database-aad-authentication.md) zur Authentifizierung anhand einer SQL-Datenbank verwenden möchten, muss ein ausgefülltes Azure Active Directory vorhanden sein, bevor Sie fortfahren können.

Führen Sie die folgenden Schritte aus, um einen Benutzer mit SQL-Authentifizierung zu erstellen:

1. Stellen Sie eine Verbindung mit Ihrer Datenbank her, z.B. mit [SQL Server Management Studio](./sql-database-connect-query-ssms.md), indem Sie Ihre Anmeldeinformationen für die Serveranmeldung verwenden.

2. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Datenbank, der ein neuer Benutzer hinzugefügt werden soll. Klicken Sie dann auf **Neue Abfrage**. Ein leeres Abfragefenster, das mit der ausgewählten Datenbank verbunden ist, wird geöffnet.

3. Geben Sie im Abfragefenster die folgende Abfrage ein:

    ```sql
    CREATE USER ApplicationUserUse' WITH PASSWORD = 'YourStrongPassword1';
    ```

4. Klicken Sie auf der Symbolleiste auf **Ausführen**, um den Benutzer zu erstellen.

5. Standardmäßig kann der Benutzer eine Verbindung zur Datenbank herstellen, besitzt jedoch keine Berechtigungen zum Lesen oder Schreiben von Daten. Um dem neu erstellten Benutzer diese Berechtigungen zu gewähren, führen Sie die folgenden zwei Befehle in einem neuen Abfragefenster aus.

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUserUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUserUser;
    ```

Es wird empfohlen, diese Nichtadministratorkonten für die Verbindung mit Ihrer Datenbank auf Datenbankebene zu erstellen, es sei denn, Sie müssen Administratoraufgaben wie die Erstellung von neuen Benutzern ausführen. Informationen zur Authentifizierung mit Azure Active Directory finden Sie im [Azure Active Directory-Tutorial](./sql-database-aad-authentication-configure.md).


## <a name="protect-your-data-with-encryption"></a>Schützen von Daten durch Verschlüsselung

Die TDE-Funktion (Transparent Data Encryption) der Azure SQL-Datenbank verschlüsselt automatisch Ihre ruhenden Daten, ohne Änderungen an der Anwendung, die auf die verschlüsselte Datenbank zugreift, vorzunehmen. Um TDE für Ihre Datenbank zu aktivieren, führen Sie folgende Schritte aus:

1. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 

2. Klicken Sie auf **Transparent Data Encryption**, um die Konfigurationsseite für TDE zu öffnen.

    ![Transparent Data Encryption](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. Setzen Sie **Datenverschlüsselung** auf „EIN“, und klicken Sie auf **Speichern**.

Der Verschlüsselungsvorgang wird im Hintergrund gestartet. Sie können den Fortschritt überwachen, indem Sie per [SQL Server Management Studio](./sql-database-connect-query-ssms.md) eine Verbindung mit SQL-Datenbank herstellen und die Spalte „encryption_state“ der Ansicht `sys.dm_database_encryption_keys` abfragen.

## <a name="enable-sql-database-auditing"></a>Aktivieren der SQL-Datenbanküberwachung

Die SQL-Datenbank-Überprüfung in Azure verfolgt Datenbankereignisse und schreibt diese in ein Überwachungsprotokoll in Ihrem Azure-Speicherkonto. Die Überwachung kann Ihnen dabei helfen, die gesetzlichen Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die Hinweise auf potenzielle Sicherheitsverstöße darstellen können. Führen Sie folgende Schritte aus, um eine standardmäßige Überwachungsrichtlinie für Ihre SQL-Datenbank zu erstellen:

1. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank.

2. Wählen Sie auf dem Blatt „Einstellungen“ die Option **Überwachung und Bedrohungserkennung**.

    ![Blatt „Überwachung“](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. Auf dem Blatt für die Konfiguration der Datenbanküberwachung können Sie das Kontrollkästchen **Einstellungen vom Server erben** aktivieren, um anzugeben, dass diese Datenbank gemäß den Einstellungen des zugehörigen Servers überwacht wird. Wenn Sie diese Option aktivieren, wird der Link **Server-Überwachungseinstellungen anzeigen** angezeigt, den Sie verwenden können, um die Überwachungseinstellungen des Servers für diesen Kontext anzuzeigen oder zu ändern.

    ![Erben von Einstellungen](./media/sql-database-security-tutorial/auditing-get-started-server-inherit.png)

4. Wenn Sie lieber einen Überwachungstyp (oder ggf. Überwachungsort) aktivieren möchten, der sich von der Angabe auf Serverebene unterscheidet, können Sie wie folgt vorgehen: **Deaktivieren** Sie die Option **Überwachungseinstellungen von Server erben**, legen Sie die Überwachung auf **EIN** fest, und wählen Sie den Überwachungstyp **Blob** aus.

    > Wenn die Serverblobüberwachung aktiviert ist, existiert die konfigurierte Datenbanküberwachung parallel zur Serverblobüberwachung.

    ![Aktivieren der Überwachung](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

5. Wählen Sie **Speicherdetails** , um das Blatt „Speicherung von Überwachungsprotokollen“ zu öffnen. Wählen Sie das Azure-Speicherkonto aus, in dem die Protokolle gespeichert werden sollen, sowie die Aufbewahrungsdauer, nach der die alten Protokolle gelöscht werden. Klicken Sie anschließend unten auf **OK**. **Tipp:** Verwenden Sie für alle überwachten Datenbanken dasselbe Speicherkonto, um die Berichtvorlagen für die Überwachung optimal einzusetzen.

    ![Navigationsbereich](./media/sql-database-security-tutorial/auditing-get-started-storage-details.png)

6. Wenn Sie die überwachten Ereignisse anpassen möchten, können Sie dies über PowerShell oder die REST-API tun. Weitere Informationen finden Sie im Abschnitt [Automatisierung (PowerShell / REST-API)](#subheading-7).

7. Klicken Sie auf **Speichern**.


## <a name="enable-sql-database-threat-detection"></a>Aktivieren der Bedrohungserkennung der SQL-Datenbank

Die Bedrohungserkennung bietet eine neue Sicherheitsebene und ermöglicht es den Kunden, auf erkannte potenzielle Bedrohungen zu reagieren. Zu diesem Zweck werden Sicherheitshinweise zu anormalen Aktivitäten bereitgestellt. Benutzer können verdächtige Ereignisse mithilfe der SQL-Datenbanküberwachung untersuchen, um zu ermitteln, ob sie auf einen Zugriffsversuch, die Verletzung der Datensicherheit oder den Missbrauch von Daten in der Datenbank zurückzuführen sind. Die Bedrohungserkennung vereinfacht den Umgang mit potenziellen Bedrohungen für die Datenbank, ohne das Fachwissen eines Sicherheitsexperten besitzen oder komplexe Sicherheitsüberwachungssysteme verwalten zu müssen.
So erkennt die Bedrohungserkennung beispielsweise bestimmte anormale Datenbankaktivitäten, die auf eine potenzielle Einschleusung von SQL-Befehlen hindeuten. Die Einschleusung von SQL-Befehlen ist ein verbreitetes Sicherheitsproblem für Webanwendungen im Internet und wird für Angriffe auf datengesteuerte Anwendungen verwendet. Die Angreifer nutzen Sicherheitslücken der Anwendung, um böswillige SQL-Anweisungen in Eingabefelder der Anwendung einzuschleusen und so an die in der Datenbank enthaltenen Daten zu gelangen oder diese zu verändern.

1. Navigieren Sie zum Konfigurationsblatt der SQL-Datenbank, die Sie überwachen möchten. Wählen Sie auf dem Blatt „Einstellungen“ die Option **Überwachung und Bedrohungserkennung**.

    ![Navigationsbereich](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. Aktivieren Sie die **Überwachung auf dem Blatt für die Konfiguration** der **Überwachung und Bedrohungserkennung**. Daraufhin werden die Einstellungen für die Bedrohungserkennung angezeigt.

3. Aktivieren **Sie** die Bedrohungserkennung.

4. Konfigurieren Sie die Liste der E-Mail-Empfänger, die bei Erkennung anormaler Datenbankaktivitäten einen Sicherheitshinweis erhalten sollen.

5. Klicken Sie auf dem Blatt **Überwachung und Bedrohungserkennung** auf **Speichern**, um die neue oder aktualisierte Richtlinie für die Überwachung und Bedrohungserkennung zu speichern.

    ![Navigationsbereich](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    Bei Erkennung anormaler Datenbankaktivitäten erhalten Sie eine E-Mail-Benachrichtigung. Die E-Mail enthält Informationen zum verdächtigen Sicherheitsereignis (wie etwa Art der anomalen Aktivitäten, Datenbankname, Servername und Zeit des Ereignisses). Darüber hinaus enthält sie Angaben zu möglichen Ursachen und empfohlenen Maßnahmen zur Untersuchung und Abwehr der potenziellen Bedrohung für die Datenbank. In den nächsten Schritten erfahren Sie, was Sie tun sollten, falls Sie eine solche E-Mail erhalten sollten:

    ![Bedrohungserkennungs-E-Mail](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. Klicken Sie in der E-Mail auf den Link für das **Azure SQL-Überwachungsprotokoll**, um das Azure-Portal aufzurufen und die relevanten Überwachungsdatensätze für die Zeit des verdächtigen Ereignisses anzuzeigen.

    ![Überwachungsdatensätze](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. Klicken Sie auf die Überwachungsdatensätze, um weitere Details zu den verdächtigen Datenbankaktivitäten (wie etwa SQL-Anweisung, Fehlerursache und Client-IP) anzuzeigen.

    ![Datensatzdetails](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. Klicken Sie auf dem Blatt mit den Überwachungsdatensätzen auf **In Excel öffnen**, um eine vorkonfigurierte Excel-Vorlage zu öffnen, das Überwachungsprotokoll zu importieren und eine ausführlichere Analyse für die Zeit des verdächtigen Ereignisses durchzuführen.

    > [!NOTE]
    > Ab Excel 2010 werden Power Query und die Einstellung **Schnelles Kombinieren** benötigt.

    ![Öffnen von Datensätzen in Excel](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. So konfigurieren Sie die Einstellung **Schnelles Kombinieren**: Klicken Sie auf der Registerkarte **POWER QUERY** des Menübands auf **Optionen**, um das gleichnamige Dialogfeld anzuzeigen. Navigieren Sie zum Datenschutzabschnitt, und wählen Sie die zweite Option zum Ignorieren der Sicherheitsstufen und zur potenziellen Verbesserung der Leistung:

    ![Excel-Einstellung „Schnelles Kombinieren“](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. Wenn Sie SQL-Überwachungsprotokolle laden möchten, vergewissern Sie sich, dass die Parameter auf der Einstellungsregisterkarte ordnungsgemäß festgelegt sind. Klicken Sie dann im Menüband auf „Daten“ und anschließend auf die Schaltfläche „Alle aktualisieren“.

    ![Excel-Parameter](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. Die Ergebnisse werden auf dem Blatt mit den **SQL-Überwachungsprotokollen** angezeigt. Dadurch können Sie die erkannten anomalen Aktivitäten eingehender untersuchen und die Auswirkungen des Sicherheitsereignisses in Ihrer Anwendung begrenzen.


## <a name="next-steps"></a>Nächste Schritte
Sie können den Schutz Ihrer Datenbank vor schädlichen Benutzern oder nicht autorisiertem Zugriff mit wenigen einfachen Schritten verbessern. In diesem Tutorial lernen Sie Folgendes: 

> [!div class="checklist"]
> * Einrichten von Firewallregeln für den Server oder die Datenbank
> * Herstellen einer Verbindung mit Ihrer Datenbank mithilfe einer sicheren Verbindungszeichenfolge
> * Verwalten des Benutzerzugriffs
> * Schützen von Daten durch Verschlüsselung
> * Aktivieren der SQL-Datenbanküberwachung
> * Aktivieren der Bedrohungserkennung der SQL-Datenbank

> [!div class="nextstepaction"]
>[Verbessern der Leistung der SQL-Datenbank](sql-database-performance-tutorial.md)


