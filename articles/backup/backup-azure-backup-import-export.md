<properties
   pageTitle="Azure Backup – Offlinesicherung oder anfängliches Seeding mithilfe des Azure Import/Export-Diensts | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit Azure Backup mithilfe des Azure Import/Export-Diensts Daten aus dem Netzwerk senden können. Dieser Artikel erläutert das Offlineseeding der ersten Sicherungsdaten mit dem Azure Import/Export-Dienst."
   services="backup"
   documentationCenter=""
   authors="saurabhsensharma"
   manager="shivamg"
   editor=""/>
<tags
   ms.service="backup"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="storage-backup-recovery"
   ms.date="08/16/2016"
   ms.author="jimpark;saurabhsensharma;nkolli;trinadhk"/>


# <a name="offline-backup-workflow-in-azure-backup"></a>Workflow zur Offlinesicherung in Azure Backup
Azure Backup verfügt über mehrere integrierte effizienzsteigernde Funktionen, die die Netzwerk- und Speicherkosten bei den ersten vollständigen Datensicherungen in Azure reduzieren. Bei den ersten vollständigen Sicherungen werden meist große Datenmengen übertragen, sodass eine höhere Netzwerkbandbreite als bei den nachfolgenden Sicherungen erforderlich ist, bei denen nur die Deltamengen bzw. Inkremente übertragen werden. Azure Backup komprimiert die Erstsicherungen. Durch den Prozess des Offlineseedings kann Azure Backup Datenträger verwenden, um die komprimierten Daten der Erstsicherungen offline in Azure hochzuladen.  

Der Offlineseeding-Prozess von Azure Backup ist eng in den [Import/Export-Dienst von Azure](../storage/storage-import-export-service.md) integriert, der Ihnen die Übertragung von Daten mithilfe von Datenträgern in Azure ermöglicht. Wenn Sie bei der Erstsicherung mehrere Terabyte an Daten über ein Netzwerk mit hoher Latenz und niedriger Bandbreite übertragen müssen, können Sie die erste Sicherungskopie mithilfe des Offlineseeding-Workflows auf einer oder mehreren Festplatten an ein Azure-Datencenter senden. Dieser Artikel bietet eine Übersicht über die für diesen Workflow erforderlichen Schritte.

## <a name="overview"></a>Übersicht

Mit der Offlineseeding-Funktion von Azure Backup und Azure Import/Export können die Daten einfach mithilfe von Datenträgern offline in Azure hochgeladen werden. Statt die erste vollständige Kopie über das Netzwerk zu übertragen, werden die Sicherungsdaten in einen *Stagingspeicherort*geschrieben. Nachdem die Daten mit dem Azure Import/Export-Tool an den Stagingspeicherort kopiert wurden, werden sie je nach Datenmenge auf eine oder mehrere SATA-Festplatten geschrieben. Diese Festplatten werden später an das nächstgelegene Azure-Datencenter gesendet.

Das [Update vom August 2016 von Azure Backup (und höher)](http://go.microsoft.com/fwlink/?LinkID=229525) enthält das *Azure-Tool für die Datenträgervorbereitung*namens „AzureOfflineBackupDiskPrep“, das folgende Funktionen bietet:

   - Es unterstützt Sie beim Vorbereiten Ihrer Laufwerke für Azure Import mit dem Azure Import/Export-Tool.
   - Es erstellt automatisch einen Azure-Importauftrag für den Azure Import/Export-Dienst im [klassischen Azure-Portal](https://manage.windowsazure.com) (bei älteren Versionen von Azure Backup muss dieser dagegen manuell erstellt werden).

Nachdem die Sicherungsdaten in Azure hochgeladen wurden, kopiert Azure Backup die Sicherungsdaten in den Sicherungstresor, und die inkrementellen Sicherungen werden geplant.

  > [AZURE.NOTE] Stellen Sie beim Verwenden des Azure-Tools für die Datenträgervorbereitung sicher, dass Sie das Update vom August 2016 von Azure Backup (oder höher) installiert haben, und führen Sie alle Schritte des Workflows damit aus. Wenn Sie eine ältere Version von Azure Backup verwenden, können Sie die SATA-Festplatte mit dem Azure Import/Export-Tool vorbereiten. Dies wird in den Abschnitten weiter unten in diesem Artikel beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

- [Machen Sie sich mit dem Azure Import/Export-Workflow vertraut](../storage/storage-import-export-service.md).
- Stellen Sie vor dem Initiieren des Workflows Folgendes sicher:
    - Ein Azure Backup-Tresor wurde erstellt.
    - Tresoranmeldeinformationen wurden heruntergeladen.
    - Der Azure Backup-Agent wurde auf dem Windows-Server/Windows-Client oder System Center Data Protection Manager-Server installiert, und der Computer ist beim Azure Backup-Tresor registriert.
- [Laden Sie die Datei mit den Azure-Veröffentlichungseinstellungen](https://manage.windowsazure.com/publishsettings) auf den Computer herunter, von dem Ihre Daten gesichert werden sollen.
- Bereiten Sie einen Stagingspeicherort vor. Dabei kann es sich um eine Netzwerkfreigabe oder ein zusätzliches Laufwerk auf dem Computer handeln. Der Stagingspeicherort ist ein Übergangsspeicher, der während dieses Workflows temporär verwendet wird. Stellen Sie sicher, dass der Stagingspeicherort genügend Speicherplatz für Ihre erste Kopie bietet. Wenn Sie beispielsweise einen 500-GB-Dateiserver sichern möchten, muss der Stagingbereich mindestens 500 GB groß sein. (Aufgrund der Komprimierung wird weniger Speicherplatz genutzt.)
- Stellen Sie sicher, dass Sie ein unterstütztes Laufwerk verwenden. Der Import/Export-Dienst unterstützt nur SATA II/III-Laufwerke im 3,5-Zoll-Format. Festplatten über 8 TB werden nicht unterstützt. Sie können SATA II/III-Laufwerke über einen SATA II/III-USB-Adapter extern an die meisten Computer anschließen. Schlagen Sie in der Dokumentation zu Azure Import/Export die aktuell vom Dienst unterstützten Laufwerke nach.
- Aktivieren Sie BitLocker auf dem Computer, an den der SATA-Laufwerkswriter angeschlossen ist.
- [Laden Sie das Azure Import/Export-Tool](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) auf den Computer herunter, mit dem der SATA-Laufwerkswriter verbunden ist. Dieser Schritt ist nicht erforderlich, wenn Sie das Update vom August 2016 von Azure Backup (oder höher) heruntergeladen und installiert haben.


## <a name="workflow"></a>Workflow
In diesem Abschnitt wird erläutert, wie Sie den Workflow zur Offlinesicherung durchführen, sodass Ihre Daten an ein Azure-Datencenter gesendet und in Azure Storage hochgeladen werden können. Falls Sie Fragen zum Import-Dienst oder zu einem anderen Aspekt des Prozesses haben, finden Sie in der oben erwähnten Dokumentation eine [Übersicht über den Import-Dienst](../storage/storage-import-export-service.md) .

### <a name="initiate-offline-backup"></a>Initiieren der Offlinesicherung

1. Beim Planen einer Sicherung wird der folgende Bildschirm angezeigt (in Windows Server, auf dem Windows-Client oder in System Center Data Protection Manager).

    ![Importbildschirm](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    Hier sehen Sie den entsprechenden Bildschirm in System Center Data Protection Manager:  <br/>
    ![DPM-Importbildschirm](./media/backup-azure-backup-import-export/dpmoffline.png)

    Die Beschreibung der Eingaben lautet wie folgt:

    - **Stagingspeicherort**: Der temporäre Speicherort, an den die erste Sicherungskopie geschrieben wird. Hierbei kann es sich um eine Netzwerkfreigabe oder einen lokalen Computer handeln. Wenn der Kopiercomputer und der Quellcomputer nicht identisch sind, wird empfohlen, den vollständigen Netzwerkpfad des Stagingspeicherorts anzugeben.
    - **Name des Azure-Importauftrags**: Der eindeutige Name, anhand dessen der Azure Import-Dienst und Azure Backup Datenübertragungen nachverfolgen, die auf Datenträgern an Azure gesendet werden.
    - **Azure-Veröffentlichungseinstellungen**: Eine XML-Datei, die Informationen zu Ihrem Abonnementprofil enthält. Außerdem enthält sie sichere Anmeldeinformationen, die Ihrem Abonnement zugeordnet sind. Sie können die [Datei herunterladen](https://manage.windowsazure.com/publishsettings). Geben Sie den lokalen Pfad zu der Datei mit den Veröffentlichungseinstellungen an.
    - **Azure-Abonnement-ID**: Die Azure-Abonnement-ID für das Abonnement, in dem Sie den Azure-Importauftrag initiieren möchten. Falls Sie über mehrere Azure-Abonnements verfügen, verwenden Sie die ID des Abonnements, das Sie dem Importauftrag zuordnen möchten.
    - **Azure-Speicherkonto**: Das klassische Speicherkonto im angegebenen Azure-Abonnement, das dem Azure-Importauftrag zugeordnet wird.
    - **Azure-Speichercontainer**: Der Name des Zielspeicherblobs im Azure-Speicherkonto, in das die Daten dieses Auftrags importiert werden.

    > [AZURE.NOTE] Wenn Sie Ihren Server über das [Azure-Portal](https://portal.azure.com) bei einem Azure Recovery Services-Tresor für Ihre Sicherungen registriert haben und kein CSP-Abonnement (Cloud Solution Provider = Cloudlösungsanbieter) besitzen, können Sie über das Azure-Portal trotzdem ein klassisches Speicherkonto erstellen und es für den Workflow zur Offlinesicherung verwenden.

    Speichern Sie all diese Informationen. Sie müssen sie in den folgenden Schritten erneut eingeben. Wenn Sie die Datenträger mit dem Azure-Tool für die Datenträgervorbereitung vorbereitet haben, ist nur der *Stagingspeicherort* erforderlich.    


2. Führen Sie den Workflow durch, und wählen Sie anschließend in der Azure Backup-Verwaltungskonsole die Option **Jetzt sichern** aus, um die Offlinesicherungskopie zu initiieren. Die erste Sicherung wird bei diesem Schritt in den Stagingbereich geschrieben.

    ![Jetzt sichern](./media/backup-azure-backup-import-export/backupnow.png)

    Um den entsprechenden Workflow in System Center Data Protection Manager auszuführen, klicken Sie mit der rechten Maustaste auf **Schutzgruppe**, und wählen Sie dann die Option **Wiederherstellungspunkt erstellen**. Wählen Sie anschließend die Option **Onlineschutz** aus.

    ![DPM – Jetzt sichern](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    Nach Abschluss des Vorgangs kann der Stagingspeicherort für die Datenträgervorbereitung verwendet werden.

    ![Sicherungsstatus](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-a-sata-drive-and-create-an-azure-import-job-by-using-the-azure-disk-preparation-tool"></a>Vorbereiten der SATA-Festplatte und Erstellen eines Azure-Importauftrags mit dem Azure-Tool für die Datenträgervorbereitung
Das Azure-Tool für die Datenträgervorbereitung ist im Installationsverzeichnis des Recovery Services-Agents (Update vom August 2016 und höher) unter folgendem Pfad verfügbar:

   *\Microsoft* *Azure* *Recovery* *Services* *Agent\Utils\*

1. Navigieren Sie zu diesem Verzeichnis, und kopieren Sie das Verzeichnis **AzureOfflineBackupDiskPrep** auf einen Kopiercomputer, auf dem die vorzubereitenden Datenträger bereitgestellt werden. Überprüfen Sie für den Kopiercomputer Folgendes:

      - Der Kopiercomputer kann über denselben Netzwerkpfad, der während des Workflows **Initiieren der Offlinesicherung** angegeben wurde, auf den Stagingspeicherort für den Offlineseeding-Workflow zugreifen.

      - BitLocker ist auf dem Computer aktiviert.

      - Der Computer kann auf das Azure-Portal zugreifen.

      Sofern erforderlich, kann der Kopiercomputer mit dem Quellcomputer identisch sein.

2. Öffnen Sie auf dem Kopiercomputer eine Eingabeaufforderung mit erhöhten Rechten und dem Verzeichnis des Azure-Tools für die Datenträgervorbereitung als aktuelles Verzeichnis. Führen Sie dann den folgenden Befehl aus:

      *.\AzureOfflineBackupDiskPrep.exe*   s:<*Pfad zum Stagingspeicherort*>   [p:<*Pfad zu PublishSettingsFile*>]

| Parameter | Beschreibung
|-------------|-------------|
|s:<*Pfad zum Stagingspeicherort*> | Obligatorische Eingabe zum Angeben des Pfads zum Stagingspeicherort, den Sie im Workflow **Initiieren der Offlinesicherung** eingegeben haben. |
|p:<*Pfad zu PublishSettingsFile*> | Optionale Eingabe zum Angeben des Pfads zur Datei **Azure-Veröffentlichungseinstellungen**, den Sie im Workflow **Initiieren der Offlinesicherung** eingegeben haben. |

> [AZURE.NOTE] Der Wert &lt;Pfad zu PublishSettingFile&gt; ist obligatorisch, wenn der Kopiercomputer und der Quellcomputer nicht identisch sind.

Beim Ausführen des Befehls fordert das Tool die Auswahl des Azure-Importauftrags an, der den vorzubereitenden Datenträgern entspricht. Falls dem angegebenen Stagingspeicherort nur ein einzelner Importauftrag zugeordnet ist, wird ein Bildschirm wie in der folgenden Abbildung angezeigt.

   ![Azure-Tool für die Datenträgervorbereitung – Eingabe](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>
3. Geben Sie den Laufwerkbuchstaben ohne nachgestellten Doppelpunkt für den bereitgestellten Datenträger an, den Sie für die Übertragung an Azure vorbereiten möchten. Bestätigen Sie die Formatierung des Laufwerks, wenn Sie dazu aufgefordert werden.

Das Tool beginnt dann, den Datenträger mit den Sicherungsdaten vorzubereiten. Falls auf dem bereitgestellten Datenträger nicht genügend Speicherplatz für die Sicherungsdaten verfügbar ist, müssen Sie unter Umständen weitere Datenträger anfügen, wenn Sie vom Tool dazu aufgefordert werden. <br/>

Nach der erfolgreichen Ausführung des Tools sind ein oder mehrere Datenträger, die Sie angegeben haben, für den Versand an Azure vorbereitet. Außerdem wird im klassischen Azure-Portal ein Importauftrag mit dem Namen erstellt, den Sie im Workflow **Initiieren der Offlinesicherung** eingegeben haben. Zum Schluss zeigt das Tool die Lieferadresse des Azure-Datencenters an, an die die Datenträger gesendet werden müssen, und den Link, unter dem Sie den Importauftrag im klassischen Azure-Portal finden.

   ![Azure-Tool für die Datenträgervorbereitung – Abgeschlossen](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>
4. Senden Sie die Datenträger an die im Tool angegebene Adresse, und bewahren Sie die Nachverfolgungsnummer zur späteren Nutzung auf.<br/>
5. Wenn Sie den im Tool angezeigten Link öffnen, sehen Sie das Azure-Speicherkonto, das Sie im Workflow **Initiieren der Offlinesicherung** angegeben haben. Der neu erstellte Importauftrag wird hier auf der Registerkarte **IMPORT/EXPORT** des Speicherkontos angezeigt.

   ![Erstellter Importauftrag](./media/backup-azure-backup-import-export/ImportJobCreated.png)<br/>
6. Klicken Sie unten auf der Seite auf **VERSANDINFORMATIONEN** , um Ihre Kontaktdetails zu aktualisieren. Microsoft verwendet diese Informationen, um die Datenträger nach der Ausführung des Importauftrags an Sie zurückzusenden.

   ![Kontaktinformationen](./media/backup-azure-backup-import-export/contactInfoAddition.PNG)<br/>
7. Geben Sie die Versanddetails auf dem nächsten Bildschirm wie unten gezeigt ein. Geben Sie den **Spediteur** und die **Nachverfolgungsnummer** für die Datenträger an, die Sie an das Azure-Datencenter geschickt haben.

   ![VERSANDINFORMATIONEN](./media/backup-azure-backup-import-export/shippingInfoAddition.PNG)<br/>

### <a name="complete-the-workflow"></a>Ausführen des Workflows
Nach Abschluss des Importauftrags sind die Daten der Erstsicherung in Ihrem Speicherkonto verfügbar. Der Recovery Services-Agent kopiert den Inhalt der Daten dann aus diesem Konto in den Sicherungstresor oder den Recovery Services-Tresor (je nach Verfügbarkeit). Zum nächsten geplanten Sicherungszeitpunkt führt der Azure Backup-Agent die inkrementelle Sicherung für die erste Sicherungskopie durch.


> [AZURE.NOTE] Die folgenden Abschnitte gelten für Benutzer von früheren Versionen von Azure Backup, die keinen Zugriff auf das Azure-Tool für die Datenträgervorbereitung haben.

### <a name="prepare-a-sata-drive"></a>Vorbereiten einer SATA-Festplatte

1. Laden Sie das [Microsoft Azure Import/Export-Tool](http://go.microsoft.com/fwlink/?linkid=301900&clcid=0x409) auf den Kopiercomputer herunter. Stellen Sie sicher, dass der Stagingspeicherort von dem Computer aus zugänglich ist, auf dem die nächsten Befehle ausgeführt werden sollen. Sofern erforderlich, kann der Kopiercomputer mit dem Quellcomputer identisch sein.

2. Entzippen Sie die Datei „WAImportExport.zip“. Führen Sie das Tool „WAImportExport“ aus. Dieses Tool formatiert die SATA-Festplatte, schreibt die Sicherungsdaten auf die SATA-Festplatte und verschlüsselt sie. Stellen Sie vor dem Ausführen des folgenden Befehls sicher, dass BitLocker auf dem Computer aktiviert ist. <br/>

    *.\WAImportExport.exe PrepImport /j:<*JournalFile*>.jrn /id: <*SessionId*> /sk:<*StorageAccountKey*> /BlobType:**PageBlob** /t:<*TargetDriveLetter*> /format /encrypt /srcdir:<*staging location*> /dstdir: <*DestinationBlobVirtualDirectory*>/*

    > [AZURE.NOTE] Wenn Sie das Update vom August 2016 von Azure Backup (oder höher) installiert haben, müssen Sie sicherstellen, dass der eingegebene Stagingspeicherort dem auf dem Bildschirm **Jetzt sichern** angezeigten Speicherort entspricht und AIB- und Basisblob-Dateien enthält.

| Parameter | Beschreibung
|-------------|-------------|
| /j:<*JournalFile*>| Der Pfad zur Journaldatei. Jedes Laufwerk muss über genau eine Journaldatei verfügen. Die Journaldatei darf sich nicht auf dem Ziellaufwerk befinden. Die Dateierweiterung der Journaldatei lautet ".jrn" und wird bei der Ausführung dieses Befehls erstellt.|
|/id:<*SessionId*> | Die Sitzungs-ID identifiziert eine Kopiersitzung. Sie wird verwendet, um eine exakte Wiederherstellung einer unterbrochenen Kopiersitzung sicherzustellen. Die in einer Kopiersitzung kopierten Dateien werden in einem Verzeichnis gespeichert, das nach der Sitzungs-ID auf dem Ziellaufwerk benannt wird.|
| /sk:<*StorageAccountKey*> | Der Kontoschlüssel für das Speicherkonto, in das die Daten importiert werden. Dieser Schlüssel muss identisch mit dem Schlüssel sein, der während der Erstellung der Sicherungsrichtlinie/Schutzgruppe eingegeben wurde.|
| /BlobType | Der Blobtyp. Dieser Workflow kann nur erfolgreich ausgeführt werden, wenn **PageBlob** angegeben ist. Dies ist nicht die Standardoption und sollte in diesem Befehl angegeben werden. |
|/t:<*TargetDriveLetter*> | Der Laufwerkbuchstabe (ohne nachgestellten Doppelpunkt) der Zielfestplatte für die aktuelle Kopiersitzung.|
|/format | Die Option zum Formatieren des Laufwerks. Geben Sie diesen Parameter an, wenn das Laufwerk formatiert werden muss. Andernfalls können Sie ihn auslassen. Bevor das Tool das Laufwerk formatiert, werden Sie in der Konsole zur Bestätigung aufgefordert. Um die Bestätigung zu unterdrücken, geben Sie den /silentmode-Parameter an.|
|/encrypt | Die Option zum Verschlüsseln des Laufwerks. Geben Sie diesen Parameter an, wenn das Laufwerk noch nicht mit BitLocker verschlüsselt wurde und vom Tool verschlüsselt werden muss. Wenn das Laufwerk bereits mit BitLocker verschlüsselt wurde, lassen Sie diesen Parameter aus, legen Sie den Parameter „/bk“ fest, und geben Sie den vorhandenen BitLocker-Schlüssel an. Wenn Sie den Parameter „/format“ angeben, müssen Sie auch den Parameter „/encrypt“ festlegen. |
|/srcdir:<*SourceDirectory*> | Das Quellverzeichnis, das Dateien enthält, die auf das Ziellaufwerk kopiert werden. Der angegebene Verzeichnisname muss über einen vollständigen Pfad verfügen (keinen relativen Pfad).|
|/dstdir:<*DestinationBlobVirtualDirectory*> | Der Pfad zum virtuellen Zielverzeichnis in Ihrem Azure-Speicherkonto. Achten Sie darauf, gültige Containernamen zu verwenden, wenn Sie virtuelle Zielverzeichnisse oder Blobs angeben. Containernamen müssen kleingeschrieben werden.  Geben Sie den Containernamen an, den Sie während der Erstellung der Sicherungsrichtlinie/Schutzgruppe eingegeben haben.|

  > [AZURE.NOTE] Eine Journaldatei wird im Ordner "WAImportExport" erstellt. Darin werden die gesamten Informationen des Workflows erfasst. Sie benötigen diese Datei, wenn Sie einen Importauftrag im Azure-Portal erstellen.

  ![PowerShell-Ausgabe](./media/backup-azure-backup-import-export/psoutput.png)

### <a name="create-an-import-job-in-the-azure-portal"></a>Erstellen eines Importauftrags im Azure-Portal
1. Navigieren Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) zu Ihrem Speicherkonto, klicken Sie auf **Import/Export** und dann im Aufgabenbereich auf **Importauftrag erstellen**.

    ![Registerkarte „Import/Export“ im Azure-Portal](./media/backup-azure-backup-import-export/azureportal.png)

2. Geben Sie in Schritt 1 des Assistenten an, dass Sie Ihr Laufwerk vorbereitet und die Laufwerkprotokolldatei zur Hand haben.
3. Geben Sie in Schritt 2 des Assistenten die Kontaktinformationen des Ansprechpartners für diesen Importauftrag an.
4. Laden Sie in Schritt 3 die Laufwerkjournaldateien hoch, die Sie im vorherigen Abschnitt erhalten haben.
5. Geben Sie in Schritt 4 einen beschreibenden Namen für den Importauftrag ein, den Sie während der Erstellung der Sicherungsrichtlinie/Schutzgruppe eingegeben haben. Der Name darf nur Kleinbuchstaben, Ziffern, Trennstriche und Unterstriche enthalten, muss mit einem Buchstaben beginnen und darf keine Leerzeichen enthalten. Anhand des ausgewählten Namens können Sie Ihre Aufträge während und nach der Bearbeitung nachverfolgen.
6. Wählen Sie anschließend Ihr Datencenter in der Liste aus. Die Datencenterregion gibt das Datencenter und die Adresse an, an die Sie Ihr Paket schicken müssen.

    ![Auswahl der Datencenterregion](./media/backup-azure-backup-import-export/dc.png)

7. Wählen Sie in Schritt 5 den Spediteur für die Rücksendung in der Liste aus, und geben Sie Ihr Kundenkonto beim Spediteur an. Microsoft verwendet diese Kontodaten, um Ihre Laufwerke nach Abschluss des Importauftrags an Sie zurückzusenden.

8. Versenden Sie den Datenträger, und geben Sie die Nachverfolgungsnummer ein, um den Status der Lieferung zu verfolgen. Sobald der Datenträger im Datencenter eintrifft, wird er in das Speicherkonto kopiert, und der Status wird aktualisiert.

    ![Status „Abgeschlossen“](./media/backup-azure-backup-import-export/complete.png)

### <a name="complete-the-workflow"></a>Ausführen des Workflows
Sobald die Daten der Erstsicherung im Speicherkonto verfügbar sind, kopiert der Microsoft Azure Recovery Services-Agent den Inhalt der Daten aus diesem Konto in den Sicherungstresor oder den Recovery Services-Tresor (je nach Verfügbarkeit). Zum nächsten geplanten Sicherungszeitpunkt führt der Azure Backup-Agent die inkrementelle Sicherung über die erste Sicherungskopie durch.

## <a name="next-steps"></a>Nächste Schritte
- Falls Sie Fragen zum Azure Import/Export-Workflow haben, finden Sie unter [Verwenden des Microsoft Azure Import/Export-Diensts zum Übertragen von Daten in den Blobspeicher](../storage/storage-import-export-service.md)weitere Informationen.
- Bei Fragen zum Workflow finden Sie im Abschnitt zur Offlinesicherung in den [häufig gestellten Fragen](backup-azure-backup-faq.md) zu Azure Backup weitere Informationen.



<!--HONumber=Oct16_HO2-->


