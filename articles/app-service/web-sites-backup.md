---
title: Sichern einer App in Azure
description: Erfahren Sie, wie Sie Sicherungen Ihrer Apps in Azure App Service erstellen.
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 6223b6bd-84ec-48df-943f-461d84605694
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: 041847f2f341528c742d127f5d624e60c26e01fe
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="back-up-your-app-in-azure"></a>Sichern einer App in Azure
Das Feature zum Sichern und Wiederherstellen in [Azure App Service](app-service-web-overview.md) ermöglicht Ihnen, App-Sicherungen einfach manuell oder nach einem Zeitplan zu erstellen. Sie können die App mit einer Momentaufnahme eines früheren Zustands wiederherstellen, indem Sie die vorhandene App überschreiben oder als andere App wiederherstellen. 

Informationen zum Wiederherstellen einer App aus einer Sicherung finden Sie unter [Wiederherstellen einer App in Azure](web-sites-restore.md).

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a>Was wird gesichert?
App Service kann die folgenden Informationen in einem Azure-Speicherkonto und einem Container sichern, für deren Verwendung Sie die App konfiguriert haben. 

* App-Konfiguration
* Dateiinhalte
* Datenbank, die mit Ihrer App verbunden ist.

Die folgenden Datenbanklösungen werden von der Sicherungsfunktion unterstützt: 
   - [SQL-Datenbank](https://azure.microsoft.com/en-us/services/sql-database/)
   - [Azure-Datenbank für MySQL (Vorschau)](https://azure.microsoft.com/en-us/services/mysql)
   - [Azure-Datenbank für PostgreSQL (Vorschau)](https://azure.microsoft.com/en-us/services/postgres)
   - [ClearDB MySQL](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
   - [MySQL In-App](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  Jede Sicherung ist eine vollständige Offlinekopie Ihrer App. Es gibt keine inkrementellen Aktualisierungen.
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a>Anforderungen und Einschränkungen
* Das Feature zum Sichern und Wiederherstellen erfordert einen App Service-Plan im Tarif **Standard** oder **Premium**. Weitere Informationen zum Skalieren des App Service-Plans zur Verwendung eines höheren Tarifs finden Sie unter [Skalieren einer App in Azure](web-sites-scale.md).  
  Im Tarif **Premium** ist eine größere Anzahl täglicher Sicherungen zulässig als im Tarif **Standard**.
* Sie benötigen ein Azure-Speicherkonto und einen Container im gleichen Abonnement wie die App, die Sie sichern möchten. Weitere Informationen zu Azure-Speicherkonten erhalten Sie unter den [Links](#moreaboutstorage) am Ende dieses Artikels.
* Sicherungen können bis zu 10GB an App- und Datenbankinhalten umfassen. Wenn die Sicherungsgröße diesen Grenzwert überschreitet, erhalten Sie eine Fehlermeldung.

<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a>Erstellen einer manuellen Sicherung
1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zum Blatt Ihrer App, und wählen Sie **Sicherungen** aus. Das Blatt **Sicherungen** wird angezeigt.
   
    ![Seite 'Sicherungen'][ChooseBackupsPage]
   
   > [!NOTE]
   > Wenn die folgende Meldung angezeigt wird, klicken Sie darauf, um Ihren App Service-Plan zu aktualisieren, damit Sie mit Sicherungen fortfahren können.
   > Weitere Informationen finden Sie unter [Zentrales Hochskalieren einer App in Azure](web-sites-scale.md) .  
   > ![Speicherkonto auswählen](./media/web-sites-backup/01UpgradePlan1.png)
   > 
   > 

2. Klicken Sie auf dem Blatt **Sicherung** auf **Konfigurieren**
![Klicken Sie auf „Konfigurieren“](./media/web-sites-backup/ClickConfigure1.png)
3. Klicken Sie auf dem Blatt **Sicherungskonfiguration** auf **Speicher: nicht konfiguriert**, um ein Speicherkonto zu konfigurieren.
   
    ![Speicherkonto auswählen][ChooseStorageAccount]
4. Wählen Sie das Sicherungsziel durch Auswahl eines **Speicherkontos** und **Containers** aus. Das Speicherkonto muss zum selben Abonnement wie die App gehören, die Sie sichern möchten. Bei Bedarf können Sie auf den entsprechenden Blättern ein neues Speicherkonto oder einen neuen Container erstellen. Wenn Sie fertig sind, klicken Sie auf **Auswählen**.
   
    ![Speicherkonto auswählen](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. Auf dem noch geöffneten Blatt **Sicherungskonfiguration** können Sie die Einstellungen für **Datenbank sichern** konfigurieren. Wählen Sie dann die Datenbanken aus, die in die Sicherungen einbezogen werden sollen (SQL-Datenbank oder MySQL), und klicken Sie auf **OK**.  
   
    ![Speicherkonto auswählen](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > Damit eine Datenbank in dieser Liste angezeigt wird, muss die zugehörige Verbindungszeichenfolge auf dem Blatt **Anwendungseinstellungen** für Ihre App im Abschnitt **Verbindungszeichenfolgen** angegeben sein.
   > 
   > 
6. Klicken Sie auf dem Blatt **Sicherungskonfiguration** auf **Speichern**.    
7. Klicken Sie auf dem Blatt **Sicherungen** auf **Sichern**.
   
    ![Schaltfläche "Backup Now"][BackUpNow]
   
    Während des Sicherungsvorgangs wird eine Fortschrittsmeldung angezeigt.

Sobald Speicherkonto und Container konfiguriert sind, können Sie jederzeit eine manuelle Sicherung initiieren.  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a>Konfigurieren automatischer Sicherungen
1. Legen Sie auf dem Blatt **Sicherungskonfiguration** die Option **Geplante Sicherung** auf **Ein** fest. 
   
    ![Speicherkonto auswählen](./media/web-sites-backup/05ScheduleBackup1.png)
2. Optionen für den Sicherungszeitplan werden angezeigt. Legen Sie die Option **Geplante Sicherung** auf **Ein** fest, konfigurieren Sie dann den Sicherungszeitplan wie gewünscht, und klicken Sie auf **OK**.
   
    ![Automatisierte Sicherungen aktivieren][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a>Konfigurieren von Teilsicherungen
Mitunter möchten Sie nicht alles in Ihrer App sichern. Hier sind einige Beispiele:

* Sie [richten wöchentliche Sicherungen](web-sites-backup.md#configure-automated-backups) der App ein, die statische Inhalte enthält, die sich nie ändern, z.B. alte Blogbeiträge oder Bilder.
* Ihre App verfügt über mehr als 10GB an Inhalten (der maximale Umfang, der auf einmal gesichert werden kann).
* Sie möchten die Protokolldateien nicht sichern.

Bei Teilsicherungen können Sie genau auswählen, welche Dateien gesichert werden sollen.

### <a name="exclude-files-from-your-backup"></a>Ausschließen von Dateien aus der Sicherung
Angenommen, Sie verfügen über eine App mit Protokolldateien und statischen Images, die einmal gesichert wurden und sich nicht ändern werden. In solchen Fällen können Sie diese Ordner und Dateien von der Speicherung in zukünftigen Sicherungen ausschließen. Um Dateien und Ordner von Ihren Sicherungen auszuschließen, erstellen Sie eine `_backup.filter`-Datei im `D:\home\site\wwwroot`-Ordner Ihrer App. Geben Sie die Liste von Dateien und Ordnern an, die Sie in dieser Datei ausschließen möchten. 

Kudu zu verwenden, ist eine einfache Möglichkeit, auf Ihre Dateien zuzugreifen. Klicken Sie auf die Einstellung **Erweiterte Tools -> Los** für Ihre Web-App, um auf Kudu zuzugreifen.

![Verwendung des Portals durch Kudu][kudu-portal]

Geben Sie die Ordner an, die Sie von Ihren Sicherungen ausschließen möchten.  Sie möchten z.B. die hervorgehobenen Ordner und Dateien herausfiltern.

![Ordner "Images"][ImagesFolder]

Erstellen Sie eine Datei namens `_backup.filter`, und fügen Sie die oben aufgeführte Liste in die Datei ein, aber entfernen Sie `D:\home`. Geben Sie ein Verzeichnis oder eine Datei pro Zeile an. Der Inhalt der Datei sollte folgendermaßen aussehen:
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

Laden Sie die `_backup.filter`-Datei in das `D:\home\site\wwwroot\`-Verzeichnis Ihrer Website hoch. Verwenden Sie dazu [ftp](app-service-deploy-ftp.md) oder eine andere Methode. Wenn Sie möchten, können Sie die Datei direkt mit der Kudu-`DebugConsole` erstellen und den Inhalt dort einfügen.

Führen Sie die Sicherungen wie gewohnt aus: [manuell](#create-a-manual-backup) oder [automatisch](#configure-automated-backups). Jetzt sind alle in `_backup.filter` angegebenen Dateien und Ordner von zukünftigen geplanten oder manuell initiierten Sicherungen ausgeschlossen. 

> [!NOTE]
> Sie stellen Teilsicherungen Ihrer Website genauso wieder her, wie Sie eine [normale Sicherung wiederherstellen](web-sites-restore.md)würden. Der Wiederherstellungsvorgang wird richtig ausgeführt.
> 
> Bei der Wiederherstellung einer vollständigen Sicherung wird der gesamte Inhalt der Website durch den Inhalt der Sicherung ersetzt. Wenn eine Datei auf der Website vorhanden ist, aber nicht in der Sicherung, wird sie gelöscht. Wenn aber eine Teilsicherung wiederhergestellt wird, bleiben alle Inhalte von ausgeschlossenen Verzeichnissen bzw. alle ausgeschlossenen Dateien unverändert.
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a>Speichern von Sicherungen
Nachdem Sie eine oder mehrere Sicherungen für Ihre App vorgenommen haben, werden die Sicherungen auf dem Blatt **Container** in Ihrem Speicherkonto sowie in Ihrer App angezeigt. Im Speicherkonto besteht jede Sicherung aus einer `.zip`-Datei mit den gesicherten Daten und einer `.xml`-Datei, die ein Manifest des `.zip`-Dateiinhalts enthält. Sie können diese Dateien extrahieren und durchsuchen, wenn Sie auf die Sicherungen zugreifen möchten, ohne eine App-Wiederherstellung auszuführen.

Die Datenbanksicherung für die App wird im Stammverzeichnis der ZIP-Datei gespeichert. Bei SQL-Datenbanken ist dies eine BACPAC-Datei (ohne Dateierweiterung), die importiert werden kann. Schritte zum Erstellen einer SQL-Datenbank auf Basis des BACPAC-Exports finden Sie im Artikel [Importieren einer BACPAC-Datei zum Erstellen einer neuen Benutzerdatenbank](http://technet.microsoft.com/library/hh710052.aspx).

> [!WARNING]
> Wenn Sie die Dateien im Container **websitebackups** ändern, kann die Sicherung ungültig werden und nicht mehr wiederhergestellt werden.
> 
> 

<a name="nextsteps"></a>

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Wiederherstellen einer App aus einer Sicherung finden Sie unter [Wiederherstellen einer App in Azure](web-sites-restore.md). 


<!-- IMAGES -->
[ChooseBackupsPage]: ./media/web-sites-backup/01ChooseBackupsPage1.png
[ChooseStorageAccount]: ./media/web-sites-backup/02ChooseStorageAccount-1.png
[BackUpNow]: ./media/web-sites-backup/04BackUpNow1.png
[SetAutomatedBackupOn]: ./media/web-sites-backup/06SetAutomatedBackupOn1.png
[SaveIcon]: ./media/web-sites-backup/10SaveIcon.png
[ImagesFolder]: ./media/web-sites-backup/11Images.png
[LogsFolder]: ./media/web-sites-backup/12Logs.png
[GhostUpgradeWarning]: ./media/web-sites-backup/13GhostUpgradeWarning.png
[kudu-portal]:./media/web-sites-backup/kudu-portal.PNG


