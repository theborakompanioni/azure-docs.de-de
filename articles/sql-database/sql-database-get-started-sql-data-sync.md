---
title: Erste Schritte mit Azure SQL-Datensynchronisierung (Vorschauversion) | Microsoft-Dokumentation
description: "Dieses Tutorial unterstützt Sie bei den ersten Schritten mit Azure SQL-Datensynchronisierung (Vorschau)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: a295a768-7ff2-4a86-a253-0090281c8efa
ms.service: sql-database
ms.custom: load & move data
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: douglasl
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: af4d41f8fa04902c766cd85d7e90f61dff8133e7
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---
# <a name="getting-started-with-azure-sql-data-sync-preview"></a>Erste Schritte mit Azure SQL-Datensynchronisierung (Vorschauversion)
In diesem Tutorial erfahren Sie, wie sie Azure SQL-Datensynchronisierung einrichten können, indem Sie eine hybride Synchronisierungsgruppe erstellen, die Azure SQL-Datenbank und SQL Server-Instanzen enthält. Die neue Synchronisierungsgruppe ist vollständig konfiguriert und synchronisiert mit dem von Ihnen festgelegten Zeitplan.

In diesem Tutorial wird davon ausgegangen, dass Sie über eine gewisse Erfahrung mit SQL-Datenbank und SQL Server verfügen. 

Einen Überblick über die SQL-Datensynchronisierung finden Sie unter [Daten synchronisieren](sql-database-sync-data.md).

> [!NOTE]
> Die gesamte technische Dokumentation zur SQL-Datensynchronisierung von Azure, die früher im MSDN zu finden war, ist als PDF-Datei verfügbar. Sie können sie [hier](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)herunterladen.

## <a name="step-1---create-sync-group"></a>Schritt 1: Erstellen einer Synchronisierungsgruppe

### <a name="locate-the-data-sync-settings"></a>Suchen Sie die Einstellungen zur Datensynchronisierung

1.  Navigieren Sie in Ihrem Browser zum Azure-Portal.

2.  Machen Sie im Portal Ihre SQL-Datenbank über das Dashboard oder über das Symbol für SQL-Datenbanken im der Symbolleiste ausfindig.

    ![Liste der Azure SQL-Datenbanken](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  Wählen Sie auf dem Blatt **SQL-Datenbanken** die vorhandene SQL-Datenbank, die Sie als die Hub-Datenbank für die Synchronisierung von Daten verwenden möchten. Das Blatt für die SQL-Datenbank wird geöffnet.

4.  Wählen Sie auf dem Blatt „SQL-Datenbank“ der ausgewählten Datenbank **Sync to other databases** (Mit anderen Datenbanken synchronisieren). Das Blatt „Datensynchronisierung“ wird geöffnet.

    ![Option „Sync to other databases“ (Mit anderen Datenbanken synchronisieren)](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Erstellen einer neuen Synchronisierungsgruppe

1.  Wählen Sie auf dem Blatt „Datensynchronisierung“ **Neue Synchronisierungsgruppe** aus. Das Blatt **Neue Synchronisierungsgruppe** wird geöffnet. Schritt 1, **Synchronisierungsgruppe erstellen**, ist markiert. Das Blatt **Datensynchronisierungsgruppe erstellen** wird ebenfalls geöffnet.

2.  Führen Sie auf dem Blatt **Datensynchronisierungsgruppe erstellen** folgende Schritte durch:

    1.  Geben Sie im Feld **Name der Synchronisierungsgruppe** einen Namen für die neue Synchronisierungsgruppe ein.

    2.  Wählen Sie im Abschnitt **Datenbank für Synchronisierungsmetadaten** aus, ob Sie eine neue Datenbank erstellen (empfohlen) oder eine vorhandene verwenden möchten.

        > [!NOTE]
        > Es wird empfohlen, dass Sie eine neue, leere Datenbank als Datenbank für Synchronisierungsmetadaten erstellen. Durch die Datensynchronisierung werden Tabellen in Datenbanken erstellt und eine häufige Workload ausgeführt. Diese Datenbank wird automatisch als Datenbank für Synchronisierungsmetadaten für alle Synchronisierungsgruppen in der ausgewählten Region freigegeben. Sie können die Datenbank für Synchronisierungsmetadaten, ihren Namen und ihren Servicelevel nicht ändern, ohne sie zu löschen.

        Wenn Sie auf **Neue Datenbank** geklickt haben, klicken Sie auf **Neue Datenbank erstellen**. Das Blatt für die **SQL-Datenbank** wird geöffnet. Benennen und konfigurieren Sie die neue Datenbank auf dem Blatt **SQL-Datenbank**. Wählen Sie dann **OK**aus.

        Wenn Sie auf **Use existing database** (Bestehende Datenbank verwenden) geklickt haben, wählen Sie die Datenbank aus der Liste.

    3.  Wählen Sie im Abschnitt **Automatisch synchronisieren** zunächst **Ein** oder **Aus**.

        Wenn Sie **Ein** ausgewählt haben, geben Sie im Abschnitt **Synchronisierungshäufigkeit** eine Zahl ein: Geben Sie Sekunden, Minuten, Stunden und Tage an.

        ![Synchronisierungshäufigkeit festlegen](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  Wählen Sie im Abschnitt **Konfliktlösung** „Hub gewinnt“ oder „Member gewinnt“ aus.

        ![Konfliktlösung bestimmen](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  Klicken Sie auf **OK**, und warten Sie, bis die neue Synchronisierungsgruppe erstellt und bereitgestellt wird.

## <a name="step-2---add-sync-members"></a>Schritt 2: Hinzufügen von Synchronisierungsmitgliedern

Nachdem die neue Synchronisierungsgruppe erstellt und bereitgestellt wurde, ist Schritt 2, **Synchronisierungsmitglieder hinzufügen**, auf dem Blatt **Neue Synchronisierungsgruppe** markiert.

Geben Sie im Abschnitt **Hub-Datenbank** die vorhandenen Anmeldeinformationen für den SQL-Datenbankserver ein, auf dem die Hub-Datenbank gespeichert ist. Geben Sie in diesem Abschnitt *keine* neuen Anmeldeinformationen ein.

![Die Hub-Datenbank wurde in die Synchronisierungsgruppe hinzugefügt.](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

## <a name="add-an-azure-sql-database"></a>Hinzufügen einer Azure SQL-Datenbank

Im Abschnitt **Mitgliedsdatenbank** können Sie optional eine Azure SQL-Datenbank zur Synchronisierungsgruppe hinzufügen, indem Sie auf **Azure-Datenbank hinzufügen** klicken. Das Blatt **Azure-Datenbank konfigurieren** wird geöffnet.

Führen Sie auf dem Blatt **Azure-Datenbank konfigurieren** die folgenden Schritte aus:

1.  Geben Sie im Feld **Name des Synchronisierungsmitglieds** einen Namen für das neue Synchronisierungsmitglied ein. Dieser Name unterscheidet sich vom Namen der Datenbank.

2.  Wählen Sie im Feld **Abonnement** das verknüpfte Azure-Abonnement für die Rechnungsstellung aus.

3.  Wählen Sie im Feld **Azure SQL-Server** den vorhandenen SQL-Datenbankserver aus.

4.  Wählen Sie im Feld **Azure SQL-Server** die vorhandene SQL-Datenbank aus.

5.  Wählen Sie im Feld **Synchronisierungsrichtungen** „bidirektionale Synchronisierung“, „Zum Hub“ oder „Vom Hub“ aus.

    ![Hinzufügen eines neuen Synchronisierungsmitglieds von SQL-Datenbank](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  Geben Sie in den Felder **Benutzername** und **Kennwort** die vorhandenen Anmeldeinformationen für den SQL-Datenbankserver ein, auf dem sich die Mitgliedsdatenbank befindet. Geben Sie in diesem Abschnitt *keine* neuen Anmeldeinformationen ein.

7.  Klicken Sie auf **OK**, und warten Sie, bis die neue Synchronisierungsmitglied erstellt und bereitgestellt wird.

    ![Das neue Synchronisierungsmitglied von SQL-Datenbank wurde hinzugefügt.](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

## <a name="add-an-on-premises-sql-server-database"></a>Hinzufügen einer lokalen SQL Server-Datenbank

Im Abschnitt **Mitgliedsdatenbank** können Sie optional einen lokalen SQL-Server zur Synchronisierungsgruppe hinzufügen, indem Sie auf **Lokale Datenbank hinzufügen** klicken. Das Blatt **Lokale Konfiguration** wird geöffnet.

Führen Sie auf dem Blatt **Lokale Konfiguration** die folgenden Schritte aus:

1.  Klicken Sie auf **Synchronisierungs-Agent-Gateway auswählen**. Das Blatt **Synchronisierungs-Agent auswählen** wird geöffnet.

    ![Wählen Sie das Synchronisierungs-Agent-Gateway aus.](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  Wählen Sie auf dem Blatt **Synchronisierungs-Agent-Gateway auswählen** aus, ob Sie einen vorhandenen Agent verwenden oder einen neuen erstellen möchten.

    Wenn Sie **Vorhandene Agents** ausgewählt haben, wählen Sie den vorhandenen Agent aus der Liste aus.

    Wenn Sie **Neuen Agent erstellen**, führen Sie folgende Schritte aus:

    1.  Laden Sie die Software für den Clientsynchronisierungs-Agent unter dem bereitgestellten Link herunter, und installieren Sie diese auf dem Computer, auf dem sich der SQL-Server befindet.
 
        > [!IMPORTANT]
        > Sie müssen den ausgehenden TCP-Port 1433 in der Firewall öffnen, damit der Client-Agent mit dem Server kommunizieren kann.


    2.  Geben Sie einen Namen für den Agent ein.

    3.  Klicken Sie auf **Schlüssel erstellen und generieren**.

    4.  Kopieren Sie den Schlüssel des Agents in die Zwischenablage.
        
        ![Erstellen einen neuen Synchronisierungs-Agents](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  Klicken Sie auf **OK**, um das Blatt **Synchronisierungs-Agent auswählen** zu schließen.

    6.  Suchen Sie auf den SQL-Servercomputer nach der Anwendung des Clientsynchronisierungs-Agents, und führen Sie diese aus.

        ![Die Anwendung des Clientsynchronisierungs-Agents](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  Klicken Sie in der Synchronisierungs-Agent-Anwendung auf **Submit Agent Key** (Agent-Schlüssel übermitteln). Das Dialogfeld **Sync Metadata Database Configuration** (Konfiguration der Datenbank für Synchronisierungsmetadaten) wird geöffnet.

    8.  Fügen Sie im Dialogfeld **Konfiguration der Datenbank für Synchronisierungsmetadaten** den Agent-Schlüssel ein, den Sie aus dem Azure-Portal kopiert haben. Geben Sie auch die vorhandenen Anmeldeinformationen für den Azure SQL-Datenbankserver ein, auf dem sich die Metadatendatenbank befindet. (Wenn Sie eine neue Metadatendatenbank erstellt haben, befindet sich diese Datenbank auf demselben Server wie die Hub-Datenbank.) Klicken Sie auf **OK**, und warten Sie, bis die Konfiguration abgeschlossen ist.

        ![Eingabe des Agent-Schlüssels und der Anmeldeinformationen des Servers](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   Wenn Sie jetzt eine Fehlermeldung der Firewall erhalten, müssen Sie eine Firewallregel in Azure aufstellen, um eingehenden Datenverkehr vom SQL-Servercomputer zuzulassen. Sie können die Regel manuell im Verwaltungsportal erstellen, aber möglicherweise ist es für Sie einfacher, diese in SSMS (SQL Server Management Studio) zu erstellen. Versuchen Sie in SSMS eine Verbindung mit der Hub-Datenbank in Azure herzustellen. Geben Sie den Namen „\<hub_datenbank_name\>.database.windows.net“ ein. Führen Sie die Schritte im Dialogfeld aus, um die Azure-Firewallregel zu konfigurieren. Kehren Sie dann zur Anwendung des Clientsynchronisierungs-Agents zurück.

    9.  Klicken Sie in der Anwendung des Clientsynchronisierungs-Agents auf **Register** (Registrieren), um eine SQL-Serverdatenbank beim Agent zu registrieren. Das Dialogfeld **SQL Server-Konfiguration** wird geöffnet.

        ![Hinzufügen und Konfigurieren einer SQL Server-Datenbank](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. Wählen Sie im Dialogfeld **SQL Server-Konfiguration** aus, ob die Verbindung über die SQL Server-Authentifizierung oder die Windows-Authentifizierung erfolgen soll. Wenn Sie die SQL Server-Authentifizierung ausgewählt haben, geben Sie die vorhandenen Anmeldeinformationen ein. Geben Sie den SQL Server-Namen und den Namen der Datenbank an, die Sie synchronisieren möchten. Klicken Sie auf **Verbindung testen**, um Ihre Einstellung zu testen. Klicken Sie dann auf **Speichern**. Die registrierte Datenbank wird in der Liste angezeigt.

        ![Die SQL Server-Datenbank ist nun registriert.](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. Sie können jetzt die Anwendung des Clientsynchronisierungs-Agents schließen.

    12. Klicken Sie im Portal auf dem Blatt **Lokale Konfiguration** auf **Datenbank auswählen**. Das Blatt **Datenbank auswählen** wird geöffnet.

    13. Geben Sie auf dem Blatt **Datenbank auswählen** im Feld **Name des Synchronisierungsmitglieds** einen Namen für das neue Synchronisierungsmitglied an. Dieser Name unterscheidet sich vom Namen der Datenbank. Wählen Sie die Datenbank aus der Liste aus. Wählen Sie im Feld **Synchronisierungsrichtungen** „bidirektionale Synchronisierung“, „Zum Hub“ oder „Vom Hub“ aus.

        ![Wählen Sie die lokale Datenbank](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. Klicken Sie auf **OK**, um das Blatt **Datenbank auswählen** zu schließen. Klicken Sie dann **auf OK**, um das Blatt **Lokale Konfiguration** zu schließen, und warten Sie, bis das neue Synchronisierungsmitglied erstellt und bereitgestellt wurde. Klicken Sie abschließend auf **OK**, um das Blatt **Synchronisierungsmitglieder auswählen** zu schließen.

        ![Lokale Datenbank zur Synchronisierungsgruppe hinzugefügt](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  Um eine Verbindung mit der SQL-Datensynchronisierung und dem lokalen Agent herzustellen, fügen Sie Ihren Benutzername der Rolle `DataSync_Executor` hinzu. Die Datensynchronisierung erstellt diese Rolle auf der SQL Server-Instanz.

## <a name="step-3---configure-sync-group"></a>Schritt 3: Konfigurieren der Synchronisierungsgruppe

Nachdem die neuen Mitglieder der Synchronisierungsgruppe erstellt und bereitgestellt wurden, ist Schritt 3, **Synchronisierung konfigurieren**, auf dem Blatt **Neue Synchronisierungsgruppe** markiert.

1.  Wählen Sie auf dem Blatt **Tabellen** eine Datenbank aus der Liste der Synchronisierungsgruppenmitglieder aus, und klicken Sie anschließend auf **Schema aktualisieren**.

2.  Wählen Sie aus der Liste der verfügbaren Tabellen die Tabellen aus, die Sie synchronisieren möchten.

    ![Auswählen zu synchronisierender Tabellen](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  Standardmäßig werden alle Spalten in der Tabelle ausgewählt. Wenn Sie nicht alle Spalten synchronisieren möchten, entfernen Sie das Häkchen im Kontrollkästchen für die Spalten, die Sie nicht synchronisieren möchten. Achten Sie darauf, dass die Primärschlüsselspalte ausgewählt bleibt.

    ![Auswählen zu synchronisierender Felder](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  Klicken Sie abschließend auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte
Herzlichen Glückwunsch. Sie haben nun eine Synchronisierungsgruppe erstellt, die sowohl eins SQL-Datenbank-Instanz als auch eine SQL Server-Datenbank enthält.

Weitere Informationen zu SQL-Datenbank und SQL-Datensynchronisierung finden Sie unter:

-   [Download der gesamten technischen Dokumentation zur SQL-Datensynchronisierung](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)
-   [Download the SQL Data Sync REST API documentation (Herunterladen der Dokumentation zur REST-API von SQL-Datensynchronisierung)](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)
-   [Übersicht über die SQL-Datenbank](sql-database-technical-overview.md)
-   [Datenbank-Lebenszyklusverwaltung](https://msdn.microsoft.com/library/jj907294.aspx)

