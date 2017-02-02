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
ms.sourcegitcommit: 5c8b3ef69cd2bc663d6ee744c2351da9c22adb94
ms.openlocfilehash: 24b9e5c2f423f7ef8bb29ab18359318f93c1d88c


---

# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a>Laden von Daten in SQL Data Warehouse mit Data Factory

Mit Azure Data Factory können Sie Daten aus allen [unterstützten Quelldatenspeichern](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats) in Azure SQL Data Warehouse laden. Beispielsweise können Sie Daten aus einer Azure SQL-Datenbank oder einer Oracle-Datenbank mithilfe von Data Factory in ein SQL Data Warehouse laden. Das Tutorial in diesem Artikel zeigt, wie Daten aus einer lokalen SQL Server-Datenbank in ein SQL Data Warehouse geladen werden.

**Geschätzte Zeit**: Dieses Tutorial dauert ca. 10 bis 15 Minuten, sobald Sie die Voraussetzungen eingerichtet haben.

## <a name="prerequisites"></a>Voraussetzungen

- Eine SQL Server-Datenbank, deren Tabellen die Daten enthalten, die in das SQL Data Warehouse kopiert werden sollen  

- Sie benötigen ein Azure Storage-Konto. Sie können entweder ein [kostenloses Azure-Konto erstellen](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder [Visual Studio-Abonnementvorteile aktivieren](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

- Sie benötigen ein online SQL Data Warehouse. Sollten Sie noch nicht über ein Data Warehouse verfügen, machen Sie sich mit der [Erstellung eines Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md) vertraut. Zur Verbesserung der Leistung sollten sich das Speicherkonto und das Data Warehouse in der gleichen Azure-Region befinden.

- Bereiten Sie Ihr Data Warehouse auf den Empfang von eingehendem Datenverkehr, indem Sie ein oder mehrere Tabellenschemas erstellen. Sie können das [SQL Data Warehouse-Dienstprogramm für die Migration](sql-data-warehouse-migrate-migration-utility.md) verwenden, um ein Skript für die Schemas zu erstellen. 

## <a name="configure-a-new-data-factory"></a>Konfigurieren einer neuen Data Factory
1. Melden Sie sich beim [Azure-Portal][]an.
2. Suchen Sie Ihr Data Warehouse, und klicken Sie darauf, um es zu öffnen. 
3. Klicken Sie auf dem Blatt **Eigenschaften** auf **Daten laden > Azure Data Factory**.

    ![Starten des Assistenten zum Laden von Daten](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)
4. Wenn in Ihrem Abonnement keine Data Factory enthalten ist, wird auf einer separaten Registerkarte des Browsers das Dialogfeld **Neue Data Factory** angezeigt. Geben Sie die erforderlichen Informationen ein, und klicken Sie auf **Erstellen**. Nach der Erstellung der Data Factory wird das Dialogfeld **Neue Data Factory** geschlossen, und das Dialogfeld **Data Factory auswählen** angezeigt.

    Wenn im Ihrem Azure-Abonnement bereits mindestens eine Data Factory verfügbar ist, wird das Dialogfeld **Data Factory auswählen** angezeigt. In diesem Dialogfeld können Sie entweder eine vorhandene Data Factory auswählen oder auf **Neue Data Factory erstellen** klicken, um eine neue zu erstellen. 
5. Im Dialogfeld **Data Factory auswählen** ist die Option **Daten laden** standardmäßig aktiviert. Klicken Sie auf **Weiter**, um mit der Erstellung einer Datenladeaufgabe zu beginnen. 

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

4. Das letzte Feld ist für den Namen des Gateways vorgesehen. Wenn der Quelldatenspeicher lokal vorliegt oder sich auf einem virtuellen Azure IaaS-Computer befindet, ist das Gateway erforderlich. Wenn Sie eine vorhandene Data Factory verwenden, die bereits über ein Gateway verfügt, können Sie das Gateway wiederverwenden, indem Sie es aus der Dropdownliste auswählen. Klicken Sie auf den Link **Gateway erstellen**, um ein Gateway zur Datenverwaltung zu erstellen.  

    > [!NOTE]
    > Für ein Gateway besteht eine 1:1-Beziehung mit einer Data Factory. Es kann nicht in einer anderen Data Factory verwendet werden, die Verwendung durch mehrere Datenladeaufgaben in derselben Data Factory ist jedoch möglich. Ein Gateway kann beim Ausführen von Datenladeaufgaben zum Herstellen von Verbindungen mit mehreren Datenspeichern verwendet werden.
    > 
    > Ausführliche Informationen zum Gateway finden Sie im Artikel [Datenverwaltungsgateway](../data-factory/data-factory-data-management-gateway.md). 

5. Ein Dialogfeld **Gateway erstellen** wird angezeigt. Geben Sie **GatewayForDWLoading** als Namen ein, und klicken Sie auf **Erstellen**.

6. Ein Dialogfeld **Gateway konfigurieren** wird angezeigt.  
    ![Express-Setup starten](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

7. Klicken Sie auf **Launch express setup on this computer** (Express-Setup auf diesem Computer starten), um das Datenverwaltungsgateway auf Ihrem aktuellen Computer (mit dem Sie auf das Portal zugreifen) herunterzuladen, zu installieren und zu registrieren. Wenn der Computer keine Verbindung mit dem Datenspeicher herstellen kann, können Sie das Gateway auf einem Computer, der eine Verbindung mit dem Datenspeicher herstellen kann, [herunterladen und installieren](https://www.microsoft.com/download/details.aspx?id=39717) und dann mithilfe des Schlüssels registrieren. Der Status wird in einem Popupfenster angezeigt.

    > [!NOTE]
    > Das Express-Setup funktioniert nativ in Microsoft Edge und im Internet Explorer. Wenn Sie Google Chrome verwenden, installieren Sie zunächst die ClickOnce-Erweiterung über den Chrome-Webstore. 

8. Warten Sie auf den Abschluss des Gateway-Setups. Sobald das Gateway erfolgreich registriert wurde und online ist, wird das Popupfenster geschlossen, und das neue Gateway wird im Gateway-Feld angezeigt. Klicken Sie auf **Weiter**.

9. Im nächsten Schritt wählen Sie die Tabellen aus, aus denen die Daten kopiert werden. Sie können die Tabellen mithilfe von Schlüsselwörtern filtern. Sie können außerdem eine Vorschau der Daten und des Tabellenschemas im unteren Bereich anzeigen. Nachdem Sie Ihre Auswahl beendet haben, klicken Sie auf **Weiter**.

    ![Auswählen von Tabellen](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-the-destination-your-sql-data-warehouse"></a>Konfigurieren des Ziels, Ihres SQL Data Warehouse

1. Klicken Sie auf **Ziel**. Die Verbindungsinformationen für Ihr SQL Data Warehouse werden automatisch ausgefüllt.
2. Geben Sie das Kennwort für den Benutzernamen ein, und klicken Sie auf **Weiter**.

    ![Konfigurieren des Ziels](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

3. Eine intelligente Tabellenzuordnung wird angezeigt, die auf der Grundlage von ähnlichen Namen die Quell- und Zieltabellen zuordnet. Fügen
4.  Sie die Zuordnung für jede Tabelle hinzu und der Rest wird automatisch aus den Beispielen zugeordnet. 
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

3. Erweitern Sie im **Ressourcen-Explorer** die Knotenpipelines, und klicken Sie auf **DWLoadData-fromSQL
4. Die Server**datenladepipeline, die Sie in diesem Tutorial erstellt haben.

    ![Anzeigen der Pipeline](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

5. Klicken Sie auf die Pipeline, um den detaillierten Status einer jeden Tabelle anzuzeigen, die einer Aktivität zugeordnet ist.

    ![Anzeigen der Tabellenaktivität](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

6. Klicken Sie auf eine weitere Aktivität, und die Datenladedetails werden im rechten Bereich angezeigt, einschließlich der Datengröße, Datenzeilen, Datendurchsatz usw.

    ![Anzeigen der Details der Tabellenaktivität](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

7. Um diese Überwachungsansicht später anzuzeigen, wechseln Sie zu Ihrem SQL Data Warehouse, klicken Sie auf **Daten laden > Azure Data Factory**, wählen Sie Ihre Factory aus, und wählen Sie **Vorhandene Ladevorgänge überwachen** aus.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Migration Ihrer Datenbank zu SQL Data Warehouse finden Sie unter [Migrationsübersicht](sql-data-warehouse-overview-migrate.md).

Weitere Informationen zu Azure Data Factory und den zugehörigen Funktionen für die Datenverschiebung finden Sie in den folgenden Artikeln: 

- [Einführung in den Azure Data Factory-Dienst](../data-factory/data-factory-introduction.md)
- [Verschieben von Daten mit der Kopieraktivität](../data-factory/data-factory-data-movement-activities.md)
- [Verschieben von Daten in und aus Azure SQL Data Warehouse mithilfe von Azure Data Factory](../data-factory/data-factory-azure-sql-data-warehouse-connector.md)

Informationen zum Untersuchen Ihrer Daten in SQL Data Warehouse finden Sie in den folgenden Artikeln: 

- [Herstellen einer Verbindung mit SQL Data Warehouse über Visual Studio und SSDT](sql-data-warehouse-query-visual-studio.md) 
- [Visuelle Daten mit Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)

<!-- Azure references -->
[Azure-Portal]: https://portal.azure.com 


<!--HONumber=Dec16_HO2-->


