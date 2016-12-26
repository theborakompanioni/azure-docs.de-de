---

title: "Laden von Daten in Azure SQL Data Warehouse – Data Factory | Microsoft Docs"
description: In diesem Tutorial werden Daten mithilfe von Azure Data Factory in Azure SQL Data Warehouse geladen und eine SQL Server-Datenbank als Datenquelle verwendet.
services: sql-data-warehouse
documentationcenter: NA
author: linda33wj
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse;azure-data-factory
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2016
ms.author: jingwang;kevin;barbkess
translationtype: Human Translation
ms.sourcegitcommit: b46ac604c10a2cb014991f3707fc3fc3b300f772
ms.openlocfilehash: 5b045ed236771919ea3f644a6b2351d54c75549b


---

# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a>Laden von Daten in SQL Data Warehouse mit Data Factory

In diesem Tutorial werden Daten mithilfe von Azure Data Factory in Azure SQL Data Warehouse geladen und eine SQL Server-Datenbank als Datenquelle verwendet. Nach diesem Tutorial befinden sich Ihre Daten in SQL Data Warehouse.

**Geschätzte Zeit**: Dieses Tutorial dauert ca. 10 bis 15 Minuten, sobald Sie die Voraussetzungen eingerichtet haben.

## <a name="prerequisites"></a>Voraussetzungen

- Für dieses Tutorial werden grundlegende Kenntnisse der Verwendung von Transact-SQL zum Erstellen von Tabellen und Schemas vorausgesetzt.  

- Sie benötigen ein Azure Storage-Konto. Sie können entweder ein [kostenloses Azure-Konto erstellen](/pricing/free-trial/?WT.mc_id=A261C142F) oder [Visual Studio-Abonnementvorteile aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

- Sie benötigen ein online SQL Data Warehouse. Sollten Sie noch nicht über ein Data Warehouse verfügen, machen Sie sich mit der [Erstellung eines Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md) vertraut. Zur Verbesserung der Leistung sollten sich das Speicherkonto und das Data Warehouse in der gleichen Azure-Region befinden.

- Bereiten Sie Ihr Data Warehouse auf den Empfang von eingehendem Datenverkehr, indem Sie ein oder mehrere Tabellenschemas erstellen. Sie können das [SQL Data Warehouse-Dienstprogramm für die Migration](sql-data-warehouse-migrate-migration-utility.md) verwenden, um ein Skript für die Schemas zu erstellen. 

## <a name="configure-a-new-data-factory"></a>Konfigurieren einer neuen Data Factory
1. Melden Sie sich beim [Azure-Portal][]an.
2. Suchen Sie Ihr Data Warehouse, und klicken Sie darauf, um es zu öffnen. 
3. Klicken Sie auf dem Blatt **Eigenschaften** auf **Daten laden > Azure Data Factory**.

    ![Starten des Assistenten zum Laden von Daten](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)

4. Das Dialogfeld **Neue Data Factory** wird angezeigt. Geben Sie die angeforderten Informationen ein, oder wählen Sie eine vorhandene Data Factory aus. Klicken Sie auf **Erstellen**.

5. Im Dialogfeld **Data Factory auswählen** ist die Option **Daten laden** standardmäßig aktiviert. Klicken Sie auf **Weiter**, um die Erstellung der Data Factory abzuschließen. 

## <a name="configure-the-data-factory-properties"></a>Konfigurieren der Data Factory-Eigenschaften
Nun, da Sie eine Data Factory erstellt haben, konfigurieren Sie im nächsten Schritt einen Datenladezeitplan. 

1. Wählen Sie **Eigenschaften** aus, und geben Sie die angeforderten Informationen an.
2. Geben Sie **DWLoadData-fromSQLServer** als **Taskname** ein.
3. Klicken Sie auf **Weiter**.

    ![Konfigurieren des Ladezeitplans](media/sql-data-warehouse-load-with-data-factory/configure-load-schedule.png)

## <a name="configure-the-data-factory-source-and-gateway"></a>Konfigurieren der Data Factory-Datenquelle und des entsprechenden Gateways
Jetzt können Sie Data Factory über die lokale SQL Server-Datenbank informieren, aus der Sie Daten laden möchten.

1. Klicken Sie auf **Quelle**.
2. Wählen Sie **SQL Server** aus dem unterstützten Quelldatenspeicherkatalog aus, und klicken Sie auf **Weiter**.

    ![Auswählen der SQL Server-Quelle](media/sql-data-warehouse-load-with-data-factory/choose-sql-server-source.png)

3. Das Dialogfeld **Specify the on-premises SQL Server database** (Angeben der lokalen SQL Server-Datenbank) wird angezeigt. Füllen Sie die erforderlichen Felder wie folgt aus:

    - **Verbindungsname**: Geben Sie einen neuen Namen für Ihre Verbindung an.
    - **Servername**: Name des lokalen SQL-Servers
    - **Datenbankname**: SQL Server-Datenbank
    - **Verschlüsselung von Anmeldeinformationen**: Keine 
    - **Authentifizierungstyp**: Wählen Sie den Authentifizierungstyp aus, den Sie verwenden.
    - **Benutzername** und **Kennwort**: Geben Sie den Benutzernamen und das Kennwort für einen Benutzer ein, der über die Berechtigung zum Kopieren der Daten verfügt.

4. Das letzte Feld fragt nach dem Namen des Gateways. Klicken Sie auf den Link **Gateway erstellen**, um ein Gateway zur Datenverwaltung zu erstellen. Das Gateway ist ein Client-Agent, den Sie in Ihrer lokalen Umgebung installieren müssen, um Daten zwischen lokalen und Clouddatenspeichern kopieren zu können. 

5. Ein Dialogfeld **Gateway erstellen** wird angezeigt. Geben Sie **GatewayForDWLoading** als Namen ein, und klicken Sie auf **Erstellen**.

6. Ein Dialogfeld **Gateway konfigurieren** wird angezeigt.  
    ![Express-Setup starten](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

7. Klicken Sie auf **Launch express setup on this computer** (Express-Setup auf diesem Computer starten) zum Herunterladen, Installieren und Registrieren des Gateways auf Ihrem aktuellen Computer. Der Status wird in einem Popupfenster angezeigt.

    Das Express-Setup funktioniert nativ in Microsoft Edge und im Internet Explorer. Wenn Sie Google Chrome verwenden, installieren Sie zunächst die ClickOnce-Erweiterung über den Chrome-Webstore. Alternativ können Sie das Gateway manuell herunterladen und installieren und den Schlüssel zur Registrierung nutzen.

8. Warten Sie auf den Abschluss des Gateway-Setups. Sobald das Gateway erfolgreich registriert wurde und online ist, wird das Popupfenster geschlossen, und das neue Gateway wird im Gateway-Feld angezeigt. Klicken Sie auf **Weiter**.

9. Im nächsten Schritt wählen Sie die Tabellen aus, aus denen die Daten kopiert werden. Sie können die Tabellen mithilfe von Schlüsselwörtern filtern. Sie können außerdem eine Vorschau der Daten und des Tabellenschemas im unteren Bereich anzeigen. Nachdem Sie Ihre Auswahl beendet haben, klicken Sie auf **Weiter**.

    ![Auswählen von Tabellen](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-the-destination-your-sql-data-warehouse"></a>Konfigurieren des Ziels, Ihres SQL Data Warehouse

1. Klicken Sie auf **Ziel**. Die Verbindungsinformationen für Ihr SQL Data Warehouse werden automatisch ausgefüllt.
2. Geben Sie das Kennwort für den Benutzernamen ein,  und klicken Sie auf **Weiter**.

    ![Konfigurieren des Ziels](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

3. Eine intelligente Tabellenzuordnung wird angezeigt, die auf der Grundlage von ähnlichen Namen die Quell- und Zieltabellen zuordnet. Fügen
4.   Sie die Zuordnung für eine beliebige Tabelle hinzu, und die restlichen Zuordnungen werden automatisch anhand der Beispiele hergeleitet und durchgeführt. 
5. Überprüfen Sie die Zuordnung, und klicken Sie auf **Weiter**.

    ![Zuordnen von Tabellen](media/sql-data-warehouse-load-with-data-factory/table-mapping.png)

6. Überprüfen Sie die Schemazuordnung, und suchen Sie nach Fehlermeldungen. Die intelligente Zuordnung basiert auf Spaltennamen. Wenn es eine nicht unterstützte Datentypkonvertierung zwischen der Quell- und Zielspalte gibt, wird eine Fehlermeldung zusammen mit der entsprechenden Tabelle angezeigt.

    ![Zuordnen von Schemas](media/sql-data-warehouse-load-with-data-factory/schema-mapping.png)

7. Klicken Sie auf **Weiter**.

## <a name="configure-the-performance-settings"></a>Konfigurieren der Leistungseinstellungen
Konfigurieren Sie ein Azure Storage-Konto in den Leistungskonfigurationen zum Bereitstellen der Daten, bevor sie in SQL Data Warehouse geladen werden.

1. Klicken Sie auf **Leistung**. 
2. Wählen Sie ein vorhandenes Azure Storage-Konto aus, und klicken Sie auf **Weiter**.

    ![Konfigurieren von Stagingblobs](media/sql-data-warehouse-load-with-data-factory/configure-staging-blob.png)

## <a name="review-summary-information-and-deploy-the-pipeline"></a>Überprüfen der Zusammenfassungsinformationen und Bereitstellen der Pipelines

1. Klicken Sie auf **Zusammenfassung**, und überprüfen Sie die Informationen.
2. Klicken Sie für die Bereitstellung der Pipeline auf **Fertigstellen**.

    ![Bereitstellen der Data Factory](media/sql-data-warehouse-load-with-data-factory/deploy-data-factory.png)

## <a name="monitor-data-loading-progress"></a>Überwachen des Fortschritts des Ladevorgangs

Nachdem die Bereitstellung abgeschlossen ist, wird die Option **Bereitstellung** im linken Menü angezeigt. 

1. Klicken Sie auf **Bereitstellung**.
2. Klicken Sie zum Überwachen des Datenladestatus auf den Link **Click here to monitor copy pipeline** (Klicken Sie hier, um die Kopierpipeline zu überwachen).

    ![Überwachen der Pipeline](media/sql-data-warehouse-load-with-data-factory/monitor-pipeline.png)

3. Erweitern Sie im **Ressourcen-Explorer** die Knotenpipelines, und klicken Sie auf die Datenladepipeline **DWLoadData-fromSQL
4.  Server**, die Sie in diesem Tutroial erstellt haben.

    ![Anzeigen der Pipeline](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

5. Klicken Sie auf die Pipeline, um den detaillierten Status einer jeden Tabelle anzuzeigen, die einer Aktivität zugeordnet ist.

    ![Anzeigen der Tabellenaktivität](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

6. Klicken Sie auf eine weitere Aktivität, und die Datenladedetails werden im rechten Bereich angezeigt, einschließlich der Datengröße, Datenzeilen, Datendurchsatz usw.

    ![Anzeigen der Details der Tabellenaktivität](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

7. Um diese Überwachungsansicht später anzuzeigen, wechseln Sie zu Ihrem SQL Data Warehouse, klicken Sie auf **Daten laden > Azure Data Factory**, wählen Sie Ihre Factory aus, und wählen Sie **Vorhandene Ladevorgänge überwachen** aus.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Migration Ihrer Datenbank zu SQL Data Warehouse finden Sie unter [Migrationsübersicht](sql-data-warehouse-overview-migrate.md).

Weitere Informationen zu den Kopierfunktionen von Azure Data Factory finden Sie unter [Einführung in Azure Data Factory](../data-factory/data-factory-introduction.md) und [Verschieben von Daten mithilfe von Copy Activity](../data-factory/data-factory-data-movement-activities.md).

Weitere Informationen zum Auswerten Ihrer Daten in SQL Data Warehouse finden Sie unter [Herstellen einer Verbindung mit SQL Data Warehouse mit Visual Studio und SSDT](sql-data-warehouse-query-visual-studio.md) und [Visuellen Daten mit Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).

<!-- Azure references -->
[Azure-Portal]: https://portal.azure.com 


<!--HONumber=Nov16_HO4-->


