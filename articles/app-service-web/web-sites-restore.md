---
title: Wiederherstellen einer App in Azure
description: Erfahren Sie, wie Sie Ihre App aus einer Sicherung wiederherstellen.
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 4444dbf7-363c-47e2-b24a-dbd45cb08491
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin;aelnably
ms.translationtype: Human Translation
ms.sourcegitcommit: 74f34bdbf5707510c682814716aa0b95c19a5503
ms.openlocfilehash: 61396e8a3521c25eaf49c66a4948e520377d342a
ms.contentlocale: de-de
ms.lasthandoff: 06/09/2017


---
# <a name="restore-an-app-in-azure"></a>Wiederherstellen einer App in Azure
In diesem Artikel erfahren Sie, wie Sie eine App in [Azure App Service](../app-service/app-service-value-prop-what-is.md) wiederherstellen, die zuvor gesichert wurde (siehe [Sichern einer App in Azure](web-sites-backup.md)). Sie können Ihre App mit den zugehörigen verknüpften Datenbanken bei Bedarf in einem vorherigen Zustand wiederherstellen oder basierend auf der Sicherung der ursprünglichen App eine neue App erstellen. Azure App Service unterstützt die folgenden Datenbanken für Sicherung und Wiederherstellung:
- [SQL-Datenbank](https://azure.microsoft.com/en-us/services/sql-database/)
- [Azure-Datenbank für MySQL (Vorschau)](https://azure.microsoft.com/en-us/services/mysql)
- [Azure-Datenbank für PostgreSQL (Vorschau)](https://azure.microsoft.com/en-us/services/postgres)
- [ClearDB MySQL](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
- [MySQL In-App](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

Das Wiederherstellen aus Sicherungen ist für Apps verfügbar, die in den Tarifen **Standard** und **Premium** ausgeführt werden. Informationen zum zentralen Hochskalieren der App finden Sie unter [Zentrales Hochskalieren einer App in Azure](web-sites-scale.md). Im Tarif **Premium** ist eine größere Anzahl täglicher Sicherungen zulässig ist als im Tarif **Standard**.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Wiederherstellen einer App aus einer vorhandenen Sicherung
1. Klicken Sie im Azure-Portal auf dem Blatt **Einstellungen** Ihrer App auf **Sicherungen**, um das Blatt **Sicherungen** anzuzeigen. Klicken Sie auf **Wiederherstellen**.
   
    !["Jetzt wiederherstellen" auswählen][ChooseRestoreNow]
2. Wählen Sie auf dem Blatt **Wiederherstellen** zuerst die Sicherungsquelle aus.
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    Die Option **App-Sicherung** zeigt alle vorhandenen Sicherungen der aktuellen App, aus denen Sie eine auswählen können.
    Die Option **Speicher** ermöglicht Ihnen in einem in Ihrem Abonnement vorhandenen Azure Storage-Konto und Container das Auswählen einer Sicherungsdatei im ZIP-Format.
    Wenn Sie versuchen, eine Sicherung einer anderen Anwendung wiederherzustellen, verwenden Sie die Option **Speicher** .
3. Geben Sie anschließend das Ziel für die App-Wiederherstellung unter **Wiederherstellungsziel**an.
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > Wenn Sie **Überschreiben** wählen, werden alle Daten in Ihrer vorhandenen App gelöscht und überschrieben. Bevor Sie auf **OK**klicken, stellen Sie sicher, dass alles genau Ihren Vorstellungen entspricht.
   > 
   > 
   
    Sie können **Vorhandene App** auswählen, um die App-Sicherung in einer anderen App in derselben Ressourcengruppe wiederherzustellen. Bevor Sie diese Option verwenden, sollten Sie bereits eine andere App in der Ressourcengruppe erstellt haben, deren Datenbankkonfiguration derjenigen entspricht, die in der App-Sicherung definiert ist. Sie können auch eine **neue** App erstellen, auf der Ihr Inhalt wiederhergestellt wird.

4. Klicken Sie auf **OK**.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Herunterladen oder Löschen einer Sicherung aus einem Speicherkonto
1. Wählen Sie auf dem Hauptblatt **Durchsuchen** des Azure-Portals die Option **Speicherkonten** aus. Eine Liste Ihrer vorhandenen Speicherkonten wird angezeigt.
2. Wählen Sie das Speicherkonto aus, das die herunterzuladende oder zu löschende Sicherung enthält. Das Blatt des Speicherkontos wird angezeigt.
3. Wählen Sie auf dem Blatt des Speicherkontos den gewünschten Container aus.
   
    ![Container anzeigen][ViewContainers]
4. Wählen Sie die Sicherungsdatei aus, die Sie herunterladen oder löschen möchten.
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. Klicken Sie je nachdem, was Sie tun möchten, auf **Herunterladen** oder **Löschen**.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>Überwachen eines Wiederherstellungsvorgangs
Um Details über den Erfolg oder Misserfolg des Wiederherstellungsvorgangs für die App anzuzeigen, wechseln Sie im Azure-Portal zum Blatt **Aktivitätsprotokoll**.  
 

Scrollen Sie nach unten zum gewünschten Wiederherstellungsvorgang, und klicken Sie darauf, um ihn auszuwählen.

Auf dem Blatt „Details“ werden die verfügbaren Informationen im Zusammenhang mit dem Wiederherstellungsvorgang angezeigt.

## <a name="next-steps"></a>Nächste Schritte
Sie können App Service-Apps mithilfe der REST-API sichern und wiederherstellen (siehe [Verwenden von REST zum Sichern und Wiederherstellen von App Service-Apps](websites-csm-backup.md)).


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png

