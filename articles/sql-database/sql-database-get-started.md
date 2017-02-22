---
title: 'Schnellstart: Ihre erste Azure SQL-Datenbank | Microsoft-Dokumentation'
description: "Erfahren Sie, wie Sie einen logischen SQL-Datenbankserver, eine Firewallregel auf Serverebene und Datenbanken mit dem Azure-Portal erstellen. Außerdem lernen Sie, wie Sie SQL Server Management Studio mit Azure SQL-Datenbank verwenden."
keywords: Tutorial zu SQL-Datenbank, Erstellen einer SQL-Datenbank
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: single databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/04/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 6453cca9f876e6c363fbed463263c0f9684a3e70
ms.openlocfilehash: b838974de06ecbc751254064e2310df51c450086


---
# <a name="quick-start-tutorial-your-first-azure-sql-database"></a>Schnellstarttutorial: Ihre erste Azure SQL-Datenbank

In diesem Schnellstarttutorial wird Folgendes behandelt:

* [Erstellen eines neuen logischen Servers](sql-database-get-started.md#create-a-new-logical-sql-server) 
* [Anzeigen der Eigenschaften eines logischen Servers](sql-database-get-started.md#view-the-logical-server-properties) 
* [Erstellen einer Firewallregel auf Serverebene](sql-database-get-started.md#create-a-server-level-firewall-rule) 
* [Herstellen einer Verbindung für den Server mit SSMS](sql-database-get-started.md#connect-to-the-server-with-ssms) 
* [Erstellen einer Datenbank mit Beispieldaten](sql-database-get-started.md#create-a-database-with-sample-data) 
* [Anzeigen der Datenbankeigenschaften](sql-database-get-started.md#view-the-database-properties) 
* [Abfragen der Datenbank im Azure-Portal](sql-database-get-started.md#query-the-database-in-the-azure-portal) 
* [Herstellen einer Verbindung mit der Datenbank und Abfragen dieser Datenbank mit SSMS](sql-database-get-started.md#connect-and-query-the-database-with-ssms) 
* [Erstellen einer neuen, leeren Datenbank mit SSMS](sql-database-get-started.md#create-a-blank-database-with-ssms) 
* [Behandeln von Konnektivitätsproblemen](sql-database-get-started.md#troubleshoot-connectivity) 
* [Löschen einer Datenbank](sql-database-get-started.md#delete-a-single-database) 


In diesem Schnellstarttutorial erstellen Sie eine Beispieldatenbank und eine leere Datenbank, die in einer Azure-Ressourcengruppe ausgeführt werden und einem logischen Server zugeordnet sind. Sie erstellen außerdem zwei Firewallregeln auf Serverebene, die so konfiguriert sind, dass sich der Prinzipal auf Serverebene am Server über zwei angegebene IP-Adressen anmelden kann. Zum Schluss lernen Sie, wie eine Datenbank im Azure-Portal abgefragt wird und wie mithilfe von SQL Server Management Studio eine Verbindung hergestellt und Abfragen durchgeführt werden. 

**Geschätzte Zeit**: Dieses Tutorial ist in ca. 30 Minuten abgeschlossen (sofern die Voraussetzungen bereits erfüllt sind).

> [!TIP]
> Die gleichen Aufgaben können Sie mit [C#](sql-database-get-started-csharp.md) oder [PowerShell](sql-database-get-started-powershell.md) ausführen.
>

## <a name="prerequisites"></a>Voraussetzungen

* Sie benötigen ein Azure-Konto. Sie können entweder ein [kostenloses Azure-Konto erstellen](/pricing/free-trial/?WT.mc_id=A261C142F) oder [Visual Studio-Abonnementvorteile aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Es muss möglich sein, dass Sie mit einem Konto, das über die Rolle „Besitzer“ oder „Mitwirkender“ für das Abonnement verfügt, eine Verbindung mit dem Azure-Portal herstellen. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Erste Schritte mit der Zugriffsverwaltung im Azure-Portal](../active-directory/role-based-access-control-what-is.md).

> [!NOTE]
> Dieses Schnellstarttutorial dient Ihnen als Hilfe zu diesen Themen: [Logische Azure SQL-Datenbankserver](sql-database-server-overview.md), [Übersicht zu Azure SQL-Datenbank](sql-database-overview.md) und [Übersicht über Firewallregeln für Azure SQL-Datenbank](sql-database-firewall-configure.md).
>  


### <a name="sign-in-to-the-azure-portal-with-your-azure-account"></a>Anmelden beim Azure-Portal mit Ihrem Azure-Konto
Führen Sie die folgenden Schritte aus, um unter Verwendung Ihres [Azure-Kontos](https://account.windowsazure.com/Home/Index)eine Verbindung mit dem Azure-Portal herzustellen.

1. Öffnen Sie einen Browser Ihrer Wahl, und stellen Sie eine Verbindung mit dem [Azure-Portal](https://portal.azure.com/)her.
2. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com/)an.
3. Geben Sie auf der Anmeldeseite **** die Anmeldeinformationen für Ihr Abonnement ein.
   
   ![Anmelden](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="create-a-new-logical-sql-server"></a>Erstellen eines neuen logischen SQL-Servers

Führen Sie die Schritte in diesem Verfahren aus, um mit dem Azure-Portal in der Region Ihrer Wahl einen neuen logischen Server zu erstellen.

1. Klicken Sie auf **Neu**, geben Sie **sql server** ein, und drücken Sie dann die **EINGABETASTE**.

    ![Logischer SQL-Server](./media/sql-database-get-started/logical-sql-server.png)
2. Klicken Sie auf **SQL Server (logischer Server)**.
   
    ![Erstellen eines logischen SQL-Servers](./media/sql-database-get-started/create-logical-sql-server.png)
3. Klicken Sie auf **Erstellen**, um das neue Blatt „SQL Server (logischer Server) zu öffnen.

    ![Neuer logischer SQL-Server](./media/sql-database-get-started/new-logical-sql-server.png)
3. Geben Sie im Textfeld „Servername“ einen gültigen Namen für den neuen logischen Server ein. Wenn der angegebene Name gültig ist, wird ein grünes Häkchen angezeigt.
    
    ![Neuer Servername](./media/sql-database-get-started/new-server-name.png)

    > [!IMPORTANT]
    > Der vollqualifizierte Name für den neuen Server weist folgendes Format auf „<Ihr_Servername>.database.windows.net“.
    >
    
4. Geben Sie im Textfeld „Serveradministratoranmeldung“ einen Benutzernamen für die SQL-Authentifizierungsanmeldung für diesen Server ein. Diese Anmeldung wird als Serverprinzipalanmeldung bezeichnet. Wenn der angegebene Name gültig ist, wird ein grünes Häkchen angezeigt.
    
    ![SQL-Administratoranmeldung](./media/sql-database-get-started/sql-admin-login.png)
5. Geben Sie in den Textfeldern **Kennwort** und **Kennwort bestätigen** ein Kennwort für das Konto für die Serverprinzipalanmeldung an. Wenn das angegebene Kennwort gültig ist, wird ein grünes Häkchen angezeigt.
    
    ![SQL-Administratorkennwort](./media/sql-database-get-started/sql-admin-password.png)
6. Wählen Sie ein Abonnement aus, für das Sie über die Berechtigung zum Erstellen von Objekten verfügen.

    ![Abonnement](./media/sql-database-get-started/subscription.png)
7. Wählen Sie im Textfeld „Ressourcengruppe“ die Option **Neu erstellen**, und geben Sie dann im Textfeld „Ressourcengruppe“ einen gültigen Namen für die neue Ressourcengruppe ein. (Sie können auch eine vorhandene Ressourcengruppe verwenden, wenn Sie bereits eine erstellt haben.) Wenn der angegebene Name gültig ist, wird ein grünes Häkchen angezeigt.

    ![Neue Ressourcengruppe](./media/sql-database-get-started/new-resource-group.png)

8. Wählen Sie im Textfeld **Standort** ein geeignetes Rechenzentrum für Ihren Standort aus, z.B. „Australien, Osten“.
    
    ![Serverstandort](./media/sql-database-get-started/server-location.png)
    
    > [!TIP]
    > Das Kontrollkästchen **Azure-Diensten Zugriff auf den Server erlauben** kann auf diesem Blatt nicht geändert werden. Sie können diese Einstellung auf dem Blatt für die Serverfirewall ändern. Weitere Informationen finden Sie unter [Erste Schritte mit der Sicherheit](sql-database-control-access-sql-authentication-get-started.md).
    >
    
9. Klicken Sie auf **Erstellen**.

    ![Schaltfläche erstellen](./media/sql-database-get-started/create.png)

## <a name="view-the-logical-server-properties"></a>Anzeigen der Eigenschaften eines logischen Servers

Führen Sie die in diesem Verfahren beschriebenen Schritte aus, um mit dem Azure-Portal die Servereigenschaften anzuzeigen. Sie benötigen den vollqualifizierten Servernamen, um in einem nachfolgenden Verfahren eine Verbindung mit diesem Server herzustellen. 

1. Klicken Sie im Azure-Portal auf **Weitere Dienste**.

    ![Weitere Dienste](./media/sql-database-get-started/more-services.png)
2. Geben Sie im Textfeld „Filter“ den Text **SQL** ein, und klicken Sie auf das Sternchen für die SQL-Server, um SQL-Server als Favoriten in Azure anzugeben. 

    ![Festlegen von Favoriten](./media/sql-database-get-started/favorite.png)
3. Klicken Sie auf dem Standardblatt auf **Computer mit SQL Server**, um die Liste mit den SQL-Servern in Ihrem Azure-Abonnement zu öffnen. 

    ![Neuer SQL-Server](./media/sql-database-get-started/new-sql-server.png)

4. Klicken Sie auf den neuen SQL-Server, um die Eigenschaften im Azure-Portal anzuzeigen. In den nachfolgenden Tutorials werden die Optionen beschrieben, die auf diesem Blatt verfügbar sind.

    ![Blatt „SQL Server“](./media/sql-database-get-started/sql-server-blade.png)
5. Klicken Sie unter „Einstellungen“ auf **Eigenschaften**, um verschiedene Eigenschaften für den logischen SQL-Server anzuzeigen.

    ![SQL Server-Eigenschaften](./media/sql-database-get-started/sql-server-properties.png)
6. Kopieren Sie den vollqualifizierten Servernamen in die Zwischenablage, damit Sie ihn später in diesem Tutorial verwenden können.

    ![Vollständiger SQL Server-Name](./media/sql-database-get-started/sql-server-full-name.png)

## <a name="create-a-server-level-firewall-rule"></a>Erstellen einer Firewallregel auf Serverebene

Führen Sie die in diesem Verfahren beschriebenen Schritte aus, um mit dem Azure-Portal eine neue Firewallregel auf Serverebene zu erstellen, damit Sie im nächsten Verfahren Ihren Server mit SQL Server Management Studio verbinden können.

1. Klicken Sie auf dem Blatt „SQL Server“ unter „Einstellungen“ auf **Firewall**, um das Blatt „Firewall“ für den SQL-Server zu öffnen.

    ![SQL Server-Firewall](./media/sql-database-get-started/sql-server-firewall.png)

2. Klicken Sie auf der Symbolleiste auf **Client-IP-Adresse hinzufügen**.

    ![Hinzufügen der Client-IP-Adresse](./media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > Sie können die SQL-Datenbank-Firewall auf dem Server für eine einzelne IP-Adresse oder einen gesamten Adressbereich öffnen. Mit dem Öffnen der Firewall wird es SQL-Administratoren und Benutzern ermöglicht, sich auf dem Server an allen Datenbanken anzumelden, für die sie über gültige Anmeldeinformationen verfügen.
    >

4. Klicken Sie in der Symbolleiste auf **Speichern**, um diese Firewallregel auf Serverebene zu speichern, und klicken Sie dann auf **OK**.

    ![Hinzufügen der Client-IP-Adresse](./media/sql-database-get-started/save-firewall-rule.png)

## <a name="connect-to-the-server-with-ssms"></a>Herstellen einer Verbindung mit dem Server mit SSMS

Führen Sie die in diesem Verfahren beschriebenen Schritte aus, um mit SQL Server Management Studio eine Verbindung mit dem logischen SQL-Server herzustellen.

1. Laden Sie die aktuelle Version von SSMS unter [Herunterladen von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx) herunter, und installieren Sie die Anwendung, falls noch nicht geschehen. Damit Sie immer auf dem neuesten Stand bleiben, werden Sie von der aktuellen Version von SSMS informiert, wenn eine neue Version als Download verfügbar ist.

2. Geben Sie nach der Installation im Windows-Suchfeld den Text **Microsoft SQL Server Management Studio** ein, und drücken Sie die **EINGABETASTE**, um SSMS zu öffnen:

    ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)
3. Geben Sie im Dialogfeld „Mit Server verbinden“ die erforderlichen Informationen ein, um mithilfe der SQL Server-Authentifizierung eine Verbindung mit Ihrer SQL Server-Instanz herzustellen.

    ![Verbindung mit dem Server herstellen](./media/sql-database-get-started/connect-to-server.png)
4. Klicken Sie auf **Verbinden**.

    ![Mit dem Server verbunden](./media/sql-database-get-started/connected-to-server.png)
5. Erweitern Sie im Objekt-Explorer die Optionen **Datenbanken**, **Systemdatenbanken** und **Master**, um die Objekte in der Masterdatenbank anzuzeigen.

    ![Masterdatenbank](./media/sql-database-get-started/master-database.png)
6. Klicken Sie mit der rechten Maustaste auf **Master** und dann auf **Neue Abfrage**.

    ![Abfragen der Masterdatenbank](./media/sql-database-get-started/query-master-database.png)

8. Geben Sie im Abfragefenster die folgende Abfrage ein:

   ```select * from sys.objects```

9.  Klicken Sie auf der Symbolleiste auf **Ausführen**, um eine Liste mit allen Systemobjekten in der Masterdatenbank zurückzugeben.

    ![Abfragen von Systemobjekten in der Masterdatenbank](./media/sql-database-get-started/query-master-database-system-objects.png)

    > [!NOTE]
    > Informationen zur SQL-Sicherheit finden Sie unter [Erste Schritte mit SQL-Sicherheit](sql-database-control-access-sql-authentication-get-started.md).
    >

## <a name="create-a-database-with-sample-data"></a>Erstellen einer Datenbank mit Beispieldaten

Führen Sie die in diesem Verfahren beschriebenen Schritte aus, um mit dem Azure-Portal eine Datenbank mit Beispieldaten zu erstellen. Diese erstellte Datenbank wird an den zuvor erstellten logischen Server angefügt. Wenn die Dienstebene „Basic“ in der Region, in der Sie Ihren Server erstellt haben, nicht verfügbar ist, löschen Sie Ihren Server, und erstellen Sie ihn in einer anderen Region neu. Informationen zu den Schritten zum Löschen finden Sie im vorherigen Verfahren in diesem Tutorial.

1. Klicken Sie im Azure-Portal auf dem Standardblatt auf **SQL-Datenbanken**.

    ![SQL-Datenbanken](./media/sql-database-get-started/new-sql-database.png)
2. Klicken Sie auf dem Blatt „SQL-Datenbanken“ auf **Hinzufügen**.

    ![Hinzufügen der SQL-Datenbank](./media/sql-database-get-started/add-sql-database.png)
3. Überprüfen Sie auf dem Blatt „SQL-Datenbank“ die Daten, die für Sie eingefügt wurden.

    ![Blatt „SQL-Datenbank“](./media/sql-database-get-started/sql-database-blade.png)
4. Geben Sie einen gültigen Datenbanknamen an.

    ![SQL-Datenbankname](./media/sql-database-get-started/sql-database-name.png)
5. Klicken Sie unter „Quelle auswählen“ auf **Beispiel** und dann unter „Beispiel auswählen“ auf **AdventureWorksLT [V12]**.
   
    ![AdventureWorks LT](./media/sql-database-get-started/adventureworkslt.png)
6. Geben Sie unter „Server“ den Benutzernamen und das Kennwort für die Administratoranmeldung ein.

    ![Serveranmeldeinformationen](./media/sql-database-get-started/server-credentials.png)

    > [!NOTE]
    > Beim Hinzufügen einer Datenbank zu einem Server kann die Datenbank als Einzeldatenbank (Standardeinstellung) oder einem elastischen Pool hinzugefügt werden. Weitere Informationen zu elastischen Pools finden Sie unter [Elastische Pools](sql-database-elastic-pool.md).
    >

7. Ändern Sie unter „Tarif“ den Tarif in **Basic**. (Sie können die Tarifstufe später wieder erhöhen, aber zu Lernzwecken ist es ratsam, hier den einfachsten Tarif zu wählen.)

    ![Tarif](./media/sql-database-get-started/pricing-tier.png)
8. Klicken Sie auf **Erstellen**.

    ![Schaltfläche erstellen](./media/sql-database-get-started/create.png)

## <a name="view-the-database-properties"></a>Anzeigen der Datenbankeigenschaften

Führen Sie die in diesem Verfahren beschriebenen Schritte aus, um mit dem Azure-Portal eine Datenbankabfrage durchzuführen.

1. Klicken Sie auf dem Blatt „SQL-Datenbanken“ auf Ihre neue Datenbank, um die dazugehörigen Eigenschaften im Azure-Portal anzuzeigen. In den nachfolgenden Tutorials werden die Optionen beschrieben, die auf diesem Blatt verfügbar sind. 

    ![Blatt für neue Beispieldatenbank](./media/sql-database-get-started/new-sample-db-blade.png)
2. Klicken Sie auf **Eigenschaften**, um weitere Informationen zur Datenbank anzuzeigen.

    ![Eigenschaften der neuen Beispieldatenbank](./media/sql-database-get-started/new-sample-db-properties.png)

3. Klicken Sie auf **Datenbankverbindungszeichenfolgen anzeigen**.

    ![Datenbankverbindungszeichenfolgen für neue Beispieldatenbank](./media/sql-database-get-started/new-sample-db-connection-strings.png)
4. Klicken Sie auf **Übersicht** und dann im Bereich „Zusammenfassung“ auf Ihren Servernamen.
    
    ![Bereich „Zusammenfassung“ für neue Beispieldatenbank](./media/sql-database-get-started/new-sample-db-essentials-pane.png)
5. Im Bereich „Zusammenfassung“ Ihres Servers wird die neu hinzugefügte Datenbank angezeigt.

    ![Neue Beispieldatenbank im Bereich „Zusammenfassung“ des Servers](./media/sql-database-get-started/new-sample-db-server-essentials-pane.png)

## <a name="query-the-database-in-the-azure-portal"></a>Abfragen der Datenbank im Azure-Portal

Führen Sie die in diesem Verfahren beschriebenen Schritte aus, um im dem Azure-Portal eine Datenbankabfrage mithilfe des Abfrage-Editors durchzuführen. Die Abfrage zeigt die Objekte in der Datenbank an.

1. Klicken Sie auf dem Blatt „SQL-Datenbanken“ auf **Tools**.

    ![Tools](./media/sql-database-get-started/tools.png)
2. Klicken Sie auf dem Blatt „Tools“ auf **Abfrage-Editor (Vorschau)**.

    ![Abfrage-Editor](./media/sql-database-get-started/query-editor.png)
3. Aktivieren Sie das Kontrollkästchen, um zu bestätigen, dass der Abfrage-Editor eine Vorschaufunktion ist, und klicken Sie anschließend auf **OK**.
4. Klicken Sie auf dem Blatt **Abfrage-Editor** auf **Anmelden**.

    ![Blatt „Abfrage-Editor“](./media/sql-database-get-started/query-editor-blade.png)
5. Prüfen Sie den Autorisierungstyp und die Anmeldung, und geben Sie anschließend das Kennwort für die Anmeldung ein. 

    ![Abfrage-Editor – Anmeldung](./media/sql-database-get-started/query-editor-login.png)
6. Klicken Sie auf **OK**, um sich anzumelden.
7. Wenn Sie einen Anmeldefehler erhalten, der angibt, dass Ihr Client über keine Berechtigungen zum Anmelden verfügt, da für die IP-Adresse Ihres Clients keine Firewallregel vorhanden ist, kopieren Sie die IP-Adresse Ihres Clients in das Fehlerfenster, und erstellen Sie auf dem Blatt „SQL Server“ für diese Datenbank eine Firewallregel auf Serverebene.

    ![Abfrage-Editor – Fehler](./media/sql-database-get-started/query-editor-error.png)
8. Wiederholen Sie den vorherigen Schritt (Schritt 6), um sich bei Ihrer Datenbank anzumelden.
9. Geben Sie nach der Authentifizierung im Abfragefenster die folgende Abfrage ein:

   ```select * from sys.objects```

    ![Abfrage-Editor-Abfrage](./media/sql-database-get-started/query-editor-query.png)
10.  Klicken Sie auf **Run**(Ausführen).
11. Die Abfrageergebnisse werden im Bereich **Ergebnisse** angezeigt.

    ![Abfrage-Editor – Ergebnisse](./media/sql-database-get-started/query-editor-results.png)

## <a name="connect-and-query-the-database-with-ssms"></a>Herstellen einer Verbindung mit der Datenbank und Abfragen dieser Datenbank mit SSMS

Führen Sie die in diesem Verfahren beschriebenen Schritte aus, um mit SQL Server Management Studio eine Verbindung mit der Datenbank herzustellen und anschließend die Beispieldaten abzufragen, damit die Objekte in der Datenbank angezeigt werden.

1. Wechseln Sie zu SQL Server Management Studio, und klicken Sie im Objekt-Explorer auf **Datenbanken** und dann in der Symbolleiste auf **Aktualisieren**, um die Beispieldatenbank anzuzeigen.

    ![Neue Beispieldatenbank mit SSMS](./media/sql-database-get-started/new-sample-db-ssms.png)
2. Erweitern Sie im Objekt-Explorer die neue Datenbank, um die dazugehörigen Objekte anzuzeigen.

    ![Objekte der neuen Beispieldatenbank mit SSMS](./media/sql-database-get-started/new-sample-db-objects-ssms.png)
3. Klicken Sie mit der rechten Maustaste auf die Beispieldatenbank, und klicken Sie dann auf **Neue Abfrage**.

    ![Abfrage für neue Beispieldatenbank mit SSMS](./media/sql-database-get-started/new-sample-db-query-ssms.png)
4. Geben Sie im Abfragefenster die folgende Abfrage ein:

   ```select * from sys.objects```
   
9.  Klicken Sie auf der Symbolleiste auf **Ausführen**, um eine Liste mit allen Systemobjekten in der Beispieldatenbank zurückzugeben.

    ![Abfragen von Systemobjekten in der neuen Beispieldatenbank mit SSMS](./media/sql-database-get-started/new-sample-db-query-objects-ssms.png)

## <a name="create-a-blank-database-with-ssms"></a>Erstellen einer neuen, leeren Datenbank mit SSMS

Führen Sie die in diesem Verfahren beschriebenen Schritte aus, um mit SQL Server Management Studio eine neue Datenbank auf dem logischen SQL-Server zu erstellen.

1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf **Datenbanken**, und klicken Sie dann auf **Neue Datenbank**.

    ![Neue leere Datenbank mit SSMS](./media/sql-database-get-started/new-blank-database-ssms.png)

    > [!NOTE]
    > Sie können auch von SSMS ein Datenbankskript für Sie erstellen lassen, um eine neue Datenbank mit Transact-SQL zu erstellen.
    >

2. Geben Sie im Dialogfeld „Neue Datenbank“ einen Datenbanknamen in das Textfeld „Datenbankname“ ein. 

    ![Name für neue leere Datenbank mit SSMS](./media/sql-database-get-started/new-blank-database-name-ssms.png)

3. Klicken Sie im Dialogfeld „Neue Datenbank“ auf **Optionen**, und ändern Sie dann die Edition in **Basic**.

    ![Optionen der neuen leeren Datenbank mit SSMS](./media/sql-database-get-started/new-blank-database-options-ssms.png)

    > [!TIP]
    > Sehen Sie sich die anderen Optionen des Dialogfelds an, die Sie für eine Azure SQL-Datenbank ändern können. Weitere Informationen zu diesen Optionen finden Sie unter [Erstellen einer Datenbank](https://msdn.microsoft.com/library/dn268335.aspx).
    >

4. Klicken Sie auf **OK**, um die leere Datenbank zu erstellen.
5. Aktualisieren Sie nach Abschluss des Vorgangs den Datenbankknoten im Objekt-Explorer, um die neu erstellte leere Datenbank anzuzeigen. 

    ![Neue leere Datenbank im Objekt-Explorer](./media/sql-database-get-started/new-blank-database-object-explorer.png)

## <a name="troubleshoot-connectivity"></a>Behandeln von Konnektivitätsproblemen

> [!IMPORTANT]
> Bei Problemen mit der Verbindung finden Sie unter [Verbindungsprobleme](sql-database-troubleshoot-common-connection-issues.md) weitere Informationen.
> 

## <a name="delete-a-single-database"></a>Löschen einer Einzeldatenbank

Führen Sie die Schritte in diesem Verfahren aus, um mit dem Azure-Portal eine Einzeldatenbank zu löschen.

1. Klicken Sie im Azure-Portal auf dem Blatt für Ihre SQL-Datenbank auf **Löschen**.

    ![Datenbank löschen](./media/sql-database-get-started/delete-database.png)
2. Klicken Sie auf **Ja**, um zu bestätigen, dass Sie diese Datenbank dauerhaft löschen möchten.

    ![Datenbank löschen – Ja](./media/sql-database-get-started/delete-database-yes.png)

> [!TIP]
> Während der Beibehaltungsdauer für Ihre Datenbank können Sie diese von den automatischen Sicherungen, die vom Dienst initiiert werden, wiederherstellen. Sie haben die Möglichkeit, Datenbanken der Basic Edition innerhalb von sieben Tagen wiederherzustellen. Vermeiden Sie es aber, einen Server zu löschen. Wenn Sie dies tun, ist es nicht möglich, den Server oder die gelöschten Datenbanken wiederherzustellen. Weitere Informationen zu Datenbanksicherungen finden Sie unter [Weitere Informationen zu Sicherungen für SQL-Datenbank](sql-database-automated-backups.md). Informationen zum Wiederherstellen einer Datenbank aus den Sicherungen finden Sie unter [Wiederherstellen einer Datenbank](sql-database-recovery-using-backups.md). Unter [Wiederherstellen einer gelöschten Azure SQL-Datenbank im Azure-Portal](sql-database-restore-deleted-database-portal.md) finden Sie Anleitungen zum Wiederherstellen einer gelöschten Datenbank.
>


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie dieses Tutorial nun abgeschlossen haben, können Sie weitere Tutorials durcharbeiten, die auf den hier erworbenen Kenntnissen aufbauen. 

- Informationen zu den ersten Schritten im SQL Server-Authentifizierungstutorial finden Sie unter [SQL-Authentifizierung und -Autorisierung](sql-database-control-access-sql-authentication-get-started.md).
- Informationen zu den ersten Schritten im Azure Active Directory-Authentifizierungstutorial finden Sie unter [Azure AD-Authentifizierung und Autorisierung](sql-database-control-access-aad-authentication-get-started.md).
* Informationen zum Abfragen der Beispieldatenbank über das Azure-Portal finden Sie unter [Public preview: Interactive query experience for SQL databases](https://azure.microsoft.com/en-us/updates/azure-sql-database-public-preview-t-sql-editor/) (Öffentliche Vorschau: Interaktives Abfragen von SQL-Datenbanken).
* Wenn Sie mit Excel vertraut sind, informieren Sie sich über das [Herstellen von Verbindungen mit SQL-Datenbank in Azure mit Excel](sql-database-connect-excel.md).
* Wenn Sie mit dem Programmieren beginnen möchten, wählen Sie unter [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md)Ihre Programmiersprache aus.
* Wenn Sie lokale SQL Server-Datenbanken in Azure verschieben möchten, helfen Ihnen die Informationen unter [Migrieren von Datenbanken zu SQL-Datenbank](sql-database-cloud-migrate.md) weiter.
* Wenn Sie Daten aus einer CSV-Datei mit dem BCP-Befehlszeilentool in eine neue Tabelle laden möchten, finden Sie unter [Laden von Daten aus einer CSV-Datei in SQL-Datenbank mit BPC](sql-database-load-from-csv-with-bcp.md) weitere Informationen.
* Wenn Sie mit dem Erstellen von Tabellen und anderen Objekten beginnen möchten, helfen Ihnen die Informationen unter „Erstellen einer Tabelle“ im Thema [Erstellen einer Tabelle](https://msdn.microsoft.com/library/ms365315.aspx) weiter.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- Eine technische Übersicht finden Sie unter [Was ist SQL-Datenbank?](sql-database-technical-overview.md).
- Preisinformationen finden Sie unter [Preise für Azure SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/).




<!--HONumber=Feb17_HO2-->


