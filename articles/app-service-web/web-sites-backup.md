<properties 
	pageTitle="Sichern einer App in Azure" 
	description="Erfahren Sie, wie Sie Sicherungen Ihrer Apps in Azure App Service erstellen." 
	services="app-service" 
	documentationCenter="" 
	authors="cephalin" 
	manager="wpickett" 
	editor="jimbe"/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/06/2016" 
	ms.author="cephalin"/>

# Sichern einer App in Azure


Das Feature zum Sichern und Wiederherstellen von [Azure App Service](../app-service/app-service-value-prop-what-is.md) ermöglicht die einfache Erstellung manueller oder automatischer App-Sicherungen. Sie können die App in einem früheren Zustand wiederherstellen oder eine neue App basierend auf einer App-Sicherung erstellen.

Informationen zum Wiederherstellen einer App aus einer Sicherung finden Sie unter [Wiederherstellen einer App in Azure](web-sites-restore.md).

<a name="whatsbackedup"></a>
## Was wird gesichert? 
App Service kann die folgenden Informationen sichern:

* App-Konfiguration
* Dateiinhalte
* Azure SQL- und Azure MySQL (ClearDB)-Datenbanken, die mit Ihrer App verbunden sind (Sie können auswählen, welche in die Sicherung einbezogen werden sollen.)

Diese Informationen werden in dem von Ihnen angegebenen Azure-Speicherkonto und -Container gesichert.

> [AZURE.NOTE] Jede Sicherung ist eine vollständige Offlinekopie Ihrer App. Es gibt keine inkrementellen Aktualisierungen.

<a name="requirements"></a>
## Anforderungen und Einschränkungen

* Das Feature zum Sichern und Wiederherstellen erfordert einen App Service-Plan im Tarif **Standard** oder höher. Weitere Informationen zum Skalieren des App Service-Plans zur Verwendung eines höheren Tarifs finden Sie unter [Skalieren einer App in Azure](web-sites-scale.md). Beachten Sie, dass im Tarif **Premium** eine größere Anzahl täglicher Sicherungen zulässig ist als im Tarif **Standard**.
* Sie benötigen ein Azure-Speicherkonto und einen Container im selben Abonnement wie die App, die Sie sichern möchten. Weitere Informationen zu Azure-Speicherkonten erhalten Sie unter den [Links](#moreaboutstorage) am Ende dieses Artikels.
* Sicherungen können bis zu 10GB an App- und Datenbankinhalten umfassen. Sie erhalten eine Fehlermeldung, wenn die Sicherungsgröße diesen Grenzwert überschreitet.

<a name="manualbackup"></a>
## Erstellen einer manuellen Sicherung

2. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zum Blatt Ihrer App. Wählen Sie **Einstellungen** und dann **Sicherungen** aus. Das Blatt **Sicherungen** wird angezeigt.
	
	![Seite 'Sicherungen'][ChooseBackupsPage]

	>[AZURE.NOTE] Wenn die folgende Meldung angezeigt wird, klicken Sie darauf, um Ihren App Service-Plan zu aktualisieren, damit Sie mit Sicherungen fortfahren können. Weitere Informationen finden Sie unter [Zentrales Hochskalieren einer App in Azure](web-sites-scale.md). ![Speicherkonto auswählen](./media/web-sites-backup/01UpgradePlan.png)

3. Klicken Sie auf dem Blatt **Sicherungen** auf **Speicher: Nicht konfiguriert**, um ein Speicherkonto zu konfigurieren.

	![Speicherkonto auswählen][ChooseStorageAccount]
	
4. Wählen Sie das Sicherungsziel durch Auswahl eines **Speicherkontos** und **Containers** aus. Das Speicherkonto muss zum selben Abonnement wie die App gehören, die Sie sichern möchten. Bei Bedarf können Sie auf den entsprechenden Blättern ein neues Speicherkonto oder einen neuen Container erstellen. Wenn Sie fertig sind, klicken Sie auf **Auswählen**.
	
	![Speicherkonto auswählen](./media/web-sites-backup/02ChooseStorageAccount1.png)
	
5. Klicken Sie auf dem geöffneten Blatt **Sicherungseinstellungen konfigurieren** auf **Datenbankeinstellungen**. Wählen Sie dann die Datenbanken aus, die in die Sicherungen einbezogen werden sollen (SQL-Datenbank oder MySQL), und klicken Sie auf **OK**.

	![Speicherkonto auswählen](./media/web-sites-backup/03ConfigureDatabase.png)

	> [AZURE.NOTE] 	Damit eine Datenbank in dieser Liste angezeigt wird, muss die zugehörige Verbindungszeichenfolge auf dem Blatt **Anwendungseinstellungen** für Ihre App im Abschnitt **Verbindungszeichenfolgen** angegeben sein.

6. Klicken Sie auf dem Blatt **Sicherungseinstellungen konfigurieren** auf **Speichern**.

7. Klicken Sie in der Befehlsleiste des Blatts **Sicherungen** auf **Jetzt sichern**.
	
	![Schaltfläche "Backup Now"][BackUpNow]
	
	Während des Sicherungsvorgangs wird eine Fortschrittsmeldung angezeigt.

Nachdem Sie ein Speicherkonto und einen Container für Sicherungen konfiguriert haben, können Sie jederzeit eine manuelle Sicherung durchführen.

<a name="automatedbackups"></a>
## Konfigurieren automatischer Sicherungen

1. Klicken Sie im Blatt **Sicherungen** auf **Zeitplan: Nicht konfiguriert**.

	![Speicherkonto auswählen](./media/web-sites-backup/05ScheduleBackup.png)
	
1. Legen Sie auf dem Blatt **Backup Schedule Settings** die Option **Geplante Sicherung** auf **Ein** fest. Konfigurieren Sie dann den Sicherungszeitplan wie gewünscht, und klicken Sie auf **OK**.
	
	![Automatisierte Sicherungen aktivieren][SetAutomatedBackupOn]
	
4. Klicken Sie auf dem geöffneten Blatt **Sicherungseinstellungen konfigurieren** auf **Speichereinstellungen**, wählen Sie dann das Sicherungsziel durch Auswahl eines **Speicherkontos** und **Containers** aus. Das Speicherkonto muss zum selben Abonnement wie die App gehören, die Sie sichern möchten. Bei Bedarf können Sie auf den entsprechenden Blättern ein neues Speicherkonto oder einen neuen Container erstellen. Wenn Sie fertig sind, klicken Sie auf **Auswählen**.
	
	![Speicherkonto auswählen](./media/web-sites-backup/02ChooseStorageAccount1.png)
	
5. Klicken Sie auf dem Blatt **Sicherungseinstellungen konfigurieren** auf **Datenbankeinstellungen**. Wählen Sie dann die Datenbanken aus, die in die Sicherungen einbezogen werden sollen (SQL-Datenbank oder MySQL), und klicken Sie auf **OK**.

	![Speicherkonto auswählen](./media/web-sites-backup/03ConfigureDatabase.png)

	> [AZURE.NOTE] 	Damit eine Datenbank in dieser Liste angezeigt wird, muss die zugehörige Verbindungszeichenfolge auf dem Blatt **Anwendungseinstellungen** für Ihre App im Abschnitt **Verbindungszeichenfolgen** angegeben sein.

6. Klicken Sie auf dem Blatt **Sicherungseinstellungen konfigurieren** auf **Speichern**.

<a name="partialbackups"></a>
## Sichern eines Teils Ihrer App

Mitunter möchten Sie nicht alles in Ihrer App sichern. Hier sind einige Beispiele:

-	Sie [richten wöchentliche Sicherungen](web-sites-backup.md#configure-automated-backups) der App ein, die statische Inhalte enthält, die sich nie ändern, z.B. alte Blogbeiträge oder Bilder.
-	Ihre App verfügt über mehr als 10 GB an Inhalten (der maximale Umfang, der auf einmal gesichert werden kann).
-	Sie möchten die Protokolldateien nicht sichern.

Bei Teilsicherungen können Sie genau auswählen, welche Dateien gesichert werden sollen.

### Ausschließen von Dateien aus der Sicherung

Erstellen Sie zum Ausschließen von Dateien und Ordnern aus der Sicherung eine `_backup.filter`-Datei im Ordner „D:\\home\\site\\wwwroot“ der App, und geben Sie die Liste der Dateien und Ordner an, die Sie dort ausschließen möchten. Eine einfache Zugriffsmöglichkeit ist die [Kudu-Konsole](https://github.com/projectkudu/kudu/wiki/Kudu-console).

Angenommen, Sie verfügen über eine App mit Protokolldateien und statischen Bildern aus früheren Jahren, die sich nicht ändern. Sie haben bereits eine vollständige Sicherung der App erstellt, die die alten Bilder enthält. Jetzt möchten Sie die App täglich sichern, aber Sie möchten nicht für die Speicherung von Protokolldateien oder statischen Bilddateien zahlen, die sich nie ändern.

![Ordner "Logs"][LogsFolder]
![Ordner "Images"][ImagesFolder]
	
Die unten angegebenen Schritte zeigen, wie Sie diese Dateien aus der Sicherung ausschließen.

1. Wechseln Sie zu `http://{yourapp}.scm.azurewebsites.net/DebugConsole`, und identifizieren Sie die Ordner, die Sie von Ihren Sicherungen ausschließen möchten. In diesem Beispiel würden Sie die folgenden Dateien und Ordner ausschließen, die in dieser Benutzeroberfläche dargestellt werden:

		D:\home\site\wwwroot\Logs
		D:\home\LogFiles
		D:\home\site\wwwroot\Images\2013
		D:\home\site\wwwroot\Images\2014
		D:\home\site\wwwroot\Images\brand.png

	[AZURE.NOTE] Die letzte Zeile zeigt, dass Sie einzelne Dateien und Ordner ausschließen können.

2. Erstellen Sie eine Datei namens `_backup.filter`, und fügen Sie die oben aufgeführte Liste in die Datei ein, aber entfernen Sie `D:\home`. Geben Sie ein Verzeichnis oder eine Datei pro Zeile an. Der Inhalt der Datei sollte folgendermaßen aussehen:

    \\site\\wwwroot\\Logs \\LogFiles \\site\\wwwroot\\Images\\2013 \\site\\wwwroot\\Images\\2014 \\site\\wwwroot\\Images\\brand.png

3. Laden Sie diese Datei in das Verzeichnis `D:\home\site\wwwroot` Ihrer Website hoch. Verwenden Sie dazu [ftp](web-sites-deploy.md#ftp) oder eine andere Methode. Wenn Sie möchten, können Sie die Datei direkt in `http://{yourapp}.scm.azurewebsites.net/DebugConsole` erstellen und den Inhalt dort einfügen.

4. Führen Sie die Sicherungen aus, wie Sie es gewohnt sind: [manuell](#create-a-manual-backup) oder [automatisch](#configure-automated-backups).

Alle Dateien und Ordner, die in `_backup.filter` angegeben sind, werden jetzt aus der Sicherung ausgeschlossen. In diesem Beispiel werden die Protokolldateien und die Bilddateien "2013" und "2014" sowie "brand.png" nicht mehr gesichert.

>[AZURE.NOTE] Sie stellen Teilsicherungen Ihrer Website genauso wieder her, wie Sie eine [normale Sicherung wiederherstellen](web-sites-restore.md) würden. Der Wiederherstellungsvorgang wird richtig ausgeführt.
>
>Bei der Wiederherstellung einer vollständigen Sicherung wird der gesamte Inhalt der Website durch den Inhalt der Sicherung ersetzt. Wenn eine Datei auf der Website vorhanden ist, aber nicht in der Sicherung, wird sie gelöscht. Wenn aber eine Teilsicherung wiederhergestellt wird, bleiben alle Inhalte von ausgeschlossenen Verzeichnissen bzw. alle ausgeschlossenen Dateien unverändert.

<a name="aboutbackups"></a>

## Speichern von Sicherungen

Nachdem Sie eine oder mehrere Sicherungen für Ihre App vorgenommen haben, werden die Sicherungen auf dem Blatt **Container** in Ihrem Speicherkonto sowie in Ihrer App angezeigt. Im Speicherkonto besteht jede Sicherung aus einer ZIP-Datei mit den gesicherten Daten und einer XML-Datei, die ein Manifest des ZIP-Dateiinhalts enthält. Sie können diese Dateien extrahieren und durchsuchen, wenn Sie auf die Sicherungen zugreifen möchten, ohne eine App-Wiederherstellung auszuführen.

Die Datenbanksicherung für die App wird im Stammverzeichnis der ZIP-Datei gespeichert. Bei SQL-Datenbanken ist dies eine BACPAC-Datei (ohne Dateierweiterung), die importiert werden kann. Schritte zum Erstellen einer neuen SQL-Datenbank auf Basis des BACPAC-Exports finden Sie im Artikel [Importieren einer BACPAC-Datei zum Erstellen einer neuen Benutzerdatenbank](http://technet.microsoft.com/library/hh710052.aspx).

> [AZURE.WARNING] Wenn Sie die Dateien im Container **websitebackups** ändern, kann die Sicherung ungültig werden und nicht mehr wiederhergestellt werden.

<a name="nextsteps"></a>
## Nächste Schritte
Informationen zum Wiederherstellen einer App aus einer Sicherung finden Sie unter [Wiederherstellen einer App in Azure](web-sites-restore.md). Sie können App Service-Apps auch mithilfe der REST-API sichern und wiederherstellen (siehe [Verwenden von REST zum Sichern und Wiederherstellen von App Service-Apps](websites-csm-backup.md)).

>[AZURE.NOTE] Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751) sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.


<!-- IMAGES -->
[ChooseBackupsPage]: ./media/web-sites-backup/01ChooseBackupsPage.png
[ChooseStorageAccount]: ./media/web-sites-backup/02ChooseStorageAccount.png
[IncludedDatabases]: ./media/web-sites-backup/03IncludedDatabases.png
[BackUpNow]: ./media/web-sites-backup/04BackUpNow.png
[BackupProgress]: ./media/web-sites-backup/05BackupProgress.png
[SetAutomatedBackupOn]: ./media/web-sites-backup/06SetAutomatedBackupOn.png
[Frequency]: ./media/web-sites-backup/07Frequency.png
[StartDate]: ./media/web-sites-backup/08StartDate.png
[StartTime]: ./media/web-sites-backup/09StartTime.png
[SaveIcon]: ./media/web-sites-backup/10SaveIcon.png
[ImagesFolder]: ./media/web-sites-backup/11Images.png
[LogsFolder]: ./media/web-sites-backup/12Logs.png
[GhostUpgradeWarning]: ./media/web-sites-backup/13GhostUpgradeWarning.png
 

<!---HONumber=AcomDC_0810_2016-->