<properties
   pageTitle="Azure Backup – Offlinesicherung oder anfängliches Seeding mithilfe des Azure Import/Export-Diensts | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit Azure Backup mithilfe des Azure Import/Export-Diensts Daten aus dem Netzwerk senden können. Dieser Artikel beschreibt das Offlineseeding der ersten Sicherungsdaten über den Azure Import/Export-Dienst."
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

# Workflow zur Offlinesicherung in Azure Backup
In Azure Backup sind mehrere effiziente Funktionen integriert, um bei den ersten vollständigen Datensicherungen in Azure Netzwerk- und Speicherkosten zu sparen. Bei den ersten vollständigen Sicherungen werden meist große Datenmengen übertragen, sodass eine höhere Netzwerkbandbreite als bei den nachfolgenden Sicherungen erforderlich ist, bei denen nur die Deltamengen bzw. Inkremente übertragen werden. Von Azure Backup werden nicht nur die ersten Sicherungen komprimiert, sondern per Offlineseeding wird ein Mechanismus zum Hochladen der komprimierten ersten Sicherungsdaten in Azure über Datenträger (Festplatten) im Offlinezustand angeboten.

Der Offlineseeding-Prozess von Azure Backup ist eng auf den [Import/Export-Dienst von Azure](../storage/storage-import-export-service.md) abgestimmt, bei dem Sie Daten mithilfe von Datenträgern schnell in Azure übertragen können. Wenn Sie bei der ersten Sicherung mehrere Terabyte an Daten über ein Netzwerk mit hoher Latenz und niedriger Bandbreite übertragen müssen, können Sie die erste Sicherungskopie mithilfe des Offlineseeding-Workflows auf mehreren Festplatten an ein Azure-Rechenzentrum senden. Dieser Artikel bietet eine Übersicht über die für diesen Workflow erforderlichen Schritte.

## Übersicht

Mit der Offlineseeding-Funktion und Azure Import/Export von Azure Backup können die Daten einfach und unkompliziert offline über Datenträger in Azure hochgeladen werden. Statt die erste vollständige Kopie über das Netzwerk zu übertragen, werden die Sicherungsdaten in einen *Stagingspeicherort* geschrieben. Nachdem der erste Kopiervorgang an den *Stagingspeicherort* durchgeführt wurde, werden diese Daten mit dem *Azure Import/Export-Tool* auf eine oder mehrere SATA-Festplatten geschrieben. Dies hängt von der Größe der ersten Sicherung ab. Diese Festplatten werden später an das nächstgelegene Azure-Rechenzentrum gesendet.

Das [Update „August 2016“ von Azure Backup (und höher)](http://go.microsoft.com/fwlink/?LinkID=229525&clcid=0x409) wird mit einem *Azure-Tool für die Datenträgervorbereitung* mit dem Namen **AzureOfflineBackupDiskPrep** gesendet. Es leistet Folgendes:

   - Hilfe beim einfachen Vorbereiten der Festplatten für Azure Import mit dem Azure Import/Export-Tool
   - Automatische Erstellung eines Azure-Importauftrags für den Azure Import/Export-Dienst im [klassischen Azure-Portal](https://manage.windowsazure.com) gegenüber der manuellen Erstellung mit älteren Versionen von Azure Backup

Nachdem der Upload der Sicherungsdaten in Azure abgeschlossen ist, kopiert Azure Backup die Sicherungsdaten in den Sicherungstresor, und die inkrementellen Sicherungen werden geplant.

  > [AZURE.NOTE] Stellen Sie beim Verwenden des Azure-Tools für die Datenträgervorbereitung sicher, dass Sie das Update „August 2016“ von Azure Backup (oder höher) installiert haben, und führen Sie alle Schritte des Workflows damit aus. Wenn Sie eine ältere Version von Azure Backup verwenden, können Sie die SATA-Festplatte mit dem *Azure Import/Export-Tool* vorbereiten. Dies wird in den Abschnitten weiter unten in diesem Artikel beschrieben.

## Voraussetzungen

1. Machen Sie sich unbedingt mit dem Azure Import/Export-Workflow vertraut, der [hier](../storage/storage-import-export-service.md) beschrieben wird.
2. Stellen Sie vor dem Initiieren des Workflows sicher, dass ein Azure-Sicherungstresor erstellt wurde, Tresoranmeldedaten heruntergeladen wurden, der Azure Backup-Agent entweder auf dem Windows-Server/Windows-Client oder dem SCDPM-Server (System Center Data Protection Manager) installiert wurde und der Computer beim Azure Backup-Tresor registriert ist.
3. Laden Sie [hier](https://manage.windowsazure.com/publishsettings) die Azure-Einstellungen zum Veröffentlichen von Dateien auf den Computer herunter, von dem die Daten gesichert werden sollen.
4. Bereiten Sie einen *Stagingspeicherort* vor. Dabei kann es sich um eine Netzwerkfreigabe oder ein zusätzliches Laufwerk auf dem Computer handeln. Der Stagingspeicherort ist ein Übergangsspeicher, der während dieses Workflows temporär verwendet wird. Stellen Sie sicher, dass der Stagingspeicherort genügend Speicherplatz für Ihre erste Kopie bietet. Wenn Sie beispielsweise versuchen, einen 500-GB-Dateiserver zu sichern, sollten Sie dafür sorgen, dass der Stagingbereich mindestens 500 GB umfasst (auch wenn aufgrund der Komprimierung weniger Speicherplatz genutzt wird).
5. Mindestens eine interne 3,5-Zoll-Festplatte (SATA II/III). Für den Azure Import/Export-Dienst werden nur interne 3,5-Zoll-Festplatten (SATA II/III) unterstützt. Festplatten mit einer Größe von mehr als 10 TB werden nicht unterstützt. Diese Festplatten können entweder intern über einen kompatiblen SATA-Stecker oder extern unter Verwendung eines SATA II/III-USB-Adapters angeschlossen werden. Informationen zu den neuesten unterstützten Festplatten und SATA-USB-Adaptern finden Sie in der [Dokumentation zu Azure Import/Export](../storage/storage-import-export-service.md).
6. Aktivieren Sie BitLocker auf dem Computer, an den der SATA-Laufwerkswriter angeschlossen ist.
7. Laden Sie das Azure Import/Export-Tool von [hier](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) auf den Computer herunter, mit dem der SATA-Laufwerkswriter verbunden ist. Dieser Schritt ist nicht erforderlich, wenn Sie das Update „August 2016“ von Azure Backup (oder höher) heruntergeladen und installiert haben.

## Workflow
Die in diesem Abschnitt bereitgestellten Informationen dienen zum Abschließen des Workflows zur **Offlinesicherung**, damit Ihre Daten an ein Azure-Datencenter versandt und in den Azure-Speicher hochgeladen werden können. Wenn Sie Fragen zum Importdienst oder zu einem anderen Aspekt des Prozesses haben, finden Sie weitere Informationen in der oben referenzierten [Dokumentation mit der Übersicht über den Importdienst](../storage/storage-import-export-service.md).

### Initiieren der Offlinesicherung

1. Bei der zeitlichen Planung einer Sicherung wird Ihnen der folgende Bildschirm angezeigt (für Windows-Server, Windows-Client oder SCDPM).

    ![ImportScreen](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)
    
    Der entsprechende SCDPM-Bildschirm sieht wie folgt aus: <br/> ![DPM-Importbildschirm](./media/backup-azure-backup-import-export/dpmoffline.png)

    Die Beschreibung der Eingaben lautet wie folgt:

    - **Stagingspeicherort**: Bezieht sich auf den temporären Speicherort, an den die erste Sicherungskopie geschrieben wird. Hierbei kann es sich um eine Netzwerkfreigabe oder den lokalen Computer handeln. Wenn der *Kopiercomputer* und der *Quellcomputer* nicht identisch sind, wird empfohlen, den vollständigen Netzwerkpfad des Stagingspeicherorts anzugeben.
    - **Name des Azure-Importauftrags**: Bezieht sich auf den eindeutigen Namen, der vom Azure Import-Dienst und Azure Backup zum Nachverfolgen der Datenübertragung auf Festplatten in Azure verwendet wird.
    - **Azure-Veröffentlichungseinstellungen**: Eine XML-Datei, die Informationen zu Ihrem Abonnementprofil enthält. Außerdem enthält sie sichere Anmeldeinformationen, die Ihrem Abonnement zugeordnet sind. Die Datei kann [hier](https://manage.windowsazure.com/publishsettings) heruntergeladen werden. Geben Sie den lokalen Pfad zu der Datei mit den Veröffentlichungseinstellungen an.
    - **Azure-Abonnement-ID**: Die Azure-Abonnement-ID, mit der Sie den Azure-Importauftrag initiieren möchten. Verwenden Sie bei mehreren Azure-Abonnements die ID, der Sie den Importauftrag zuordnen möchten.
    - **Azure-Speicherkonto**: Das Speicherkonto vom Typ „klassisch“ im bereitgestellten Azure-Abonnement, das dem Azure-Importauftrag zugeordnet wird.
    - **Azure-Speichercontainer**: Der Name des Zielspeicherblobs im Azure-Speicherkonto, in das die Daten dieses Auftrags importiert werden.
    
    > [AZURE.NOTE] Wenn Sie Ihren Server über das [neue Azure-Portal](https://ms.portal.azure.com) für die Sicherungen bei einem Recovery Services-Tresor registriert haben und kein CSP-Abonnement (Cloud Service Provider, Clouddienstanbieter) verwenden, können Sie über das neue Azure-Portal trotzdem ein Speicherkonto vom Typ „klassisch“ erstellen und für den Workflow zur Offlinesicherung einsetzen.
    
    Speichern Sie diese Informationen separat, da sie in den folgenden Schritten erneut eingegeben werden müssen. Nur der *Stagingspeicherort* ist erforderlich, wenn das *Azure-Tool für die Datenträgervorbereitung* zum Vorbereiten der Datenträger verwendet wird.
    

2. Schließen Sie den Workflow ab, und wählen Sie in der Azure Backup-Verwaltungskonsole die Option **Jetzt sichern**, um die Kopie der Offlinesicherung zu initiieren. Die erste Sicherung wird bei diesem Schritt in den Stagingbereich geschrieben.

    ![Jetzt sichern](./media/backup-azure-backup-import-export/backupnow.png)
    
    Der entsprechende Workflow in SCDPM wird aktiviert, indem Sie mit der rechten Maustaste auf die **Schutzgruppe** klicken und die Option **Wiederherstellungspunkt erstellen** wählen. Nach diesem Schritt wird die Option **Onlineschutz** gewählt.

    ![DPM – Jetzt sichern](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    Nachdem der Vorgang abgeschlossen ist, kann der Stagingspeicherort für die Datenträgervorbereitung verwendet werden.

    ![Ausgabe](./media/backup-azure-backup-import-export/opbackupnow.png)

###Vorbereiten der SATA-Festplatte und Erstellen eines Azure-Importauftrags mit dem *Azure-Tool für die Datenträgervorbereitung*
Das *Azure-Tool für die Datenträgervorbereitung* ist im Installationsverzeichnis des Microsoft Azure Recovery Services-Agents (Update „August 2016“ und höher) unter dem folgenden Pfad verfügbar:

   *\\Microsoft* *Azure* *Recovery* *Services* *Agent\\Utils*

1. Navigieren Sie zum oben angegebenen Verzeichnis, und kopieren Sie das Verzeichnis **AzureOfflineBackupDiskPrep** auf einen *Kopiercomputer*, auf dem die vorzubereitenden Datenträger bereitgestellt werden. Überprüfen Sie in Bezug auf den *Kopiercomputer* Folgendes:

      - Der für den Offlineseeding-Workflow bereitgestellte *Stagingspeicherort* ist vom *Kopiercomputer* aus über denselben Netzwerkpfad zugänglich, der während des Workflows **Initiieren der Offlinesicherung** angegeben wurde.
      
      - BitLocker ist auf dem Computer aktiviert.
      
      - Der Computer hat Zugriff auf das Azure-Portal.
      
      Falls erforderlich, kann der *Kopiercomputer* mit dem *Quellcomputer* identisch sein.

2. Öffnen Sie auf dem Kopiercomputer ein Eingabeaufforderungsfenster mit erhöhten Rechten und dem Verzeichnis des Azure-Tools für die Datenträgervorbereitung als aktuellem Verzeichnis. Führen Sie dann den folgenden Befehl aus:

      *.\\AzureOfflineBackupDiskPrep.exe* s:<*Pfad zum Stagingspeicherort*> [p:<*Pfad zu PublishSettingsFile*>]

| Parameter | Beschreibung
|-------------|-------------|
|s:<*Pfad zum Stagingspeicherort*> | Obligatorische Eingabe. Wird zum Angeben des Pfads zum Stagingspeicherort verwendet, der während der Initiierung der Offlinesicherung eingegeben wurde. |
|p:<*Pfad zu PublishSettingsFile*> | Optionale Eingabe. Wird zum Angeben des Pfads zur Datei mit den Veröffentlichungseinstellungen verwendet, der während der Initiierung der Offlinesicherung eingegeben wurde. |

   > [AZURE.NOTE] Die Datei mit den Veröffentlichungseinstellungen ist eine obligatorische Eingabe, wenn der *Kopiercomputer* und der *Quellcomputer* nicht identisch sind.

Beim Ausführen des Befehls fordert das Tool die Auswahl des Azure-Importauftrags an. Dies richtet sich danach, welche Datenträger vorbereitet werden müssen. Falls dem angegebenen Stagingspeicherort nur ein einzelner Importauftrag zugeordnet ist, wird ein Bildschirm wie in der folgenden Abbildung angezeigt.

   ![Azure-Tool für die Datenträgervorbereitung – Eingabe](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>
3. Geben Sie den **Laufwerkbuchstaben** für den bereitgestellten Datenträger, den Sie für die Übertragung an Azure vorbereiten möchten, ohne nachgestellten Doppelpunkt an. Bestätigen Sie danach die Formatierung des Laufwerks, wenn Sie dazu aufgefordert werden.

Das Tool beginnt dann damit, den Datenträger mit den Sicherungsdaten vorzubereiten. Unter Umständen müssen Sie weitere Datenträger anfügen, wenn Sie vom Tool dazu aufgefordert werden, falls auf dem bereitgestellten Datenträger nicht genügend Platz für die Sicherungsdaten ist. <br/>

Nach der erfolgreichen Ausführung des Tools werden die von Ihnen bereitstellten Datenträger für das Senden an Azure vorbereitet, und ein Importauftrag mit dem Namen, den Sie während des Workflows **Initiieren der Offlinesicherung** angegeben haben, wird im klassischen Azure-Portal erstellt. Außerdem zeigt das Tool die Lieferadresse des Azure-Datencenters an, an die die Datenträger gesendet werden müssen, sowie den Link zum Auffinden des Importauftrags im klassischen Azure-Portal.

   ![Azure-Tool für die Datenträgervorbereitung – Abgeschlossen](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>
4. Senden Sie die Datenträger an die vom Tool angegebene Adresse, und bewahren Sie die Nachverfolgungsnummer zur späteren Nutzung auf.<br/>
5. Beim Klicken auf den Link, der vom Tool angezeigt wird, gelangen Sie zu dem **Azure-Speicherkonto**, das während des Workflows zur Offlinesicherung angegeben wurde. Hier sehen Sie den neu erstellten Importauftrag auf der Registerkarte **IMPORT/EXPORT** des Speicherkontos.

   ![Importauftrag wurde erstellt](./media/backup-azure-backup-import-export/ImportJobCreated.png)<br/>
6. Klicken Sie unten auf der Seite auf **VERSANDINFORMATIONEN**, um die **Kontaktdetails** zu aktualisieren. Microsoft verwendet diese Informationen für den Rückversand an Sie, nachdem der Importauftrag ausgeführt wurde.

   ![Kontaktinformationen](./media/backup-azure-backup-import-export/contactInfoAddition.PNG)<br/>
7. Geben Sie die **Versanddetails** auf dem nächsten Bildschirm wie unten gezeigt ein. Geben Sie die Details zum Spediteur und die Nachverfolgungsnummer für die Datenträger an, die Sie an das Azure-Datencenter geschickt haben.
   
   ![Versandinformationen](./media/backup-azure-backup-import-export/shippingInfoAddition.PNG)<br/>

### Abschließen des Workflows
Nach Abschluss des Importauftrags sind die Daten der ersten Sicherung in Ihrem Speicherkonto verfügbar. Der Microsoft Azure Recovery Services-Agent kopiert den Inhalt der Daten aus diesem Konto in den Sicherungstresor oder den Recovery Services-Tresor (je nach Verfügbarkeit). Zum nächsten geplanten Sicherungszeitpunkt führt der Azure Backup-Agent die inkrementelle Sicherung für die erste Sicherungskopie durch.


> [AZURE.NOTE] Die folgenden Abschnitte gelten für Benutzer von früheren Versionen von Azure Backup, die keinen Zugriff auf das *Azure-Tool für die Datenträgervorbereitung* haben.

### Vorbereiten der SATA-Festplatte

1. Laden Sie das [Microsoft Azure Import/Export-Tool](http://go.microsoft.com/fwlink/?linkid=301900&clcid=0x409) auf den *Kopiercomputer* herunter. Stellen Sie sicher, dass der Bereitstellungsspeicherort (im vorherigen Schritt) von dem Computer aus zugänglich ist, auf dem der nächste Satz von Befehlen ausgeführt werden soll. Falls erforderlich, kann der Kopiercomputer mit dem Quellcomputer identisch sein.

2. Entpacken Sie die Datei *WAImportExport.zip*. Führen Sie das *WAImportExport*-Tool aus. Es formatiert die SATA-Festplatte, schreibt die Sicherungsdaten auf die SATA-Festplatte und verschlüsselt sie. Bevor Sie den folgenden Befehl ausführen, sollten Sie sicherstellen, dass BitLocker auf dem Computer aktiviert ist. <br/>

    *.\\WAImportExport.exe PrepImport /j:<*JournalFile*>.jrn /id: <*SessionId*> /sk:<*StorageAccountKey*> /BlobType:**PageBlob** /t:<*TargetDriveLetter*> /format /encrypt /srcdir:<*Stagingspeicherort*> /dstdir: <*DestinationBlobVirtualDirectory*>/*
    
    > [AZURE.NOTE] Wenn Sie das Update „August 2016“ von Azure Backup (oder höher) installiert haben, müssen Sie sicherstellen, dass der eingegebene *Stagingspeicherort* der Anzeige auf dem Bildschirm **Jetzt sichern** entspricht und *AIB*- und *Basisblob*-Dateien enthält.

| Parameter | Beschreibung
|-------------|-------------|
| /j: <*JournalFile*>| Der Pfad zur Journaldatei. Jedes Laufwerk muss über genau eine Journaldatei verfügen. Die Journaldatei darf sich nicht auf dem Ziellaufwerk befinden. Die Dateierweiterung der Journaldatei lautet ".jrn" und wird bei der Ausführung dieses Befehls erstellt.|
|/ ID: <*SessionId*> | Die Sitzungs-ID identifiziert eine *Kopiersitzung*. Sie wird verwendet, um eine exakte Wiederherstellung einer unterbrochenen Kopiersitzung sicherzustellen. Die in einer Kopiersitzung kopierten Dateien werden in einem Verzeichnis gespeichert, das nach der Sitzungs-ID auf dem Ziellaufwerk benannt wird.|
| /SK: <*StorageAccountKey*> | Der Kontoschlüssel für das Speicherkonto, in das die Daten importiert werden. Dieser Schlüssel muss identisch mit dem Schlüssel sein, der während der Erstellung der Sicherungsrichtlinie/Schutzgruppe eingegeben wurde.|
| /BlobType | Geben Sie **PageBlob** an. Dieser Workflow wird nur erfolgreich ausgeführt, wenn die Option „PageBlob“ angegeben ist. Dies ist nicht die Standardoption und sollte in diesem Befehl angegeben werden. |
|/ t: <*TargetDriveLetter*> | Der Laufwerkbuchstabe der Zielfestplatte für die aktuelle Kopiersitzung ohne nachgestellten Doppelpunkt.|
|/format | Geben Sie diesen Parameter an, wenn das Laufwerk formatiert werden muss. Andernfalls können Sie ihn auslassen. Bevor das Tool das Laufwerk formatiert, werden Sie über die Konsole zur Bestätigung aufgefordert. Um die Bestätigung zu unterdrücken, geben Sie den /silentmode-Parameter an.|
|/encrypt | Dieser Parameter wird angegeben, wenn das Laufwerk noch nicht mit BitLocker verschlüsselt wurde und vom Tool verschlüsselt werden muss. Wenn das Laufwerk bereits mit BitLocker verschlüsselt wurde, lassen Sie diesen Parameter aus, und geben Sie den /bk-Parameter an, in dem Sie den vorhandenen BitLocker-Schlüssel bereitstellen. Wenn Sie den /format-Parameter angeben, müssen Sie auch den /encrypt-Parameter festlegen. |
|/srcdir: <*SourceDirectory*> | Das Quellverzeichnis, das Dateien enthält, die auf das Ziellaufwerk kopiert werden. Stellen Sie sicher, dass der hier angegebene Verzeichnisname ein vollständiger Pfad (kein relativer Pfad) ist.|
|/dstdir: <*DestinationBlobVirtualDirectory*> | Der Pfad zum virtuellen Zielverzeichnis in Ihrem Microsoft Azure-Speicherkonto. Achten Sie darauf, gültige Containernamen zu verwenden, wenn Sie virtuelle Zielverzeichnisse oder Blobs angeben. Containernamen müssen kleingeschrieben werden. Dieser Containername sollte derselbe sein wie der während der Erstellung der Sicherungsrichtlinie/Schutzgruppe eingegebene Name.|

  > [AZURE.NOTE] Eine Journaldatei wird im Ordner "WAImportExport" erstellt. Darin werden die gesamten Informationen des Workflows erfasst. Sie benötigen diese Datei beim Erstellen eines Importauftrags im Azure-Portal.

  ![PowerShell-Ausgabe](./media/backup-azure-backup-import-export/psoutput.png)

### Erstellen eines Importauftrags im Azure-Portal
1. Navigieren Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) zu Ihrem Speicherkonto, und klicken Sie auf **Import/Export** und dann im Aufgabenbereich auf **Importauftrag erstellen**.

    ![Portal](./media/backup-azure-backup-import-export/azureportal.png)

2. Geben Sie im ersten Schritt des Assistenten an, dass Sie Ihr Laufwerk vorbereitet haben und dass Sie die Laufwerkprotokolldatei zur Hand haben. Geben Sie im zweiten Schritt des Assistenten die Kontaktinformationen des Ansprechpartners für diesen Importauftrag an.
3. Laden Sie im dritten Schritt die Laufwerkjournaldateien hoch, die Sie im vorherigen Abschnitt erhalten haben.
4. Geben Sie in Schritt 4 einen aussagekräftigen Namen für den Importauftrag ein, der während der Erstellung der Sicherungsrichtlinie/Schutzgruppe eingegeben wurde. Der eingegebene Name darf nur Kleinbuchstaben, Ziffern, Trennstriche und Unterstriche enthalten, muss mit einem Buchstaben beginnen und darf keine Leerzeichen enthalten. Mithilfe des ausgewählten Namens können Sie Ihre Aufträge während und nach der Bearbeitung nachverfolgen.
5. Wählen Sie anschließend Ihr Rechenzentrum aus der Liste aus. Unter Rechenzentrumsregion wird das Rechenzentrum und die Adresse angegeben, an die Sie Ihr Paket schicken müssen.

    ![DC](./media/backup-azure-backup-import-export/dc.png)

6. Wählen Sie in Schritt 5 Ihren Rücktransporteur aus und geben Sie Ihr Kundenkonto beim Transportunternehmen an. Microsoft verwendet diese Kontodaten, um Ihre Laufwerke nach Abschluss des Importauftrags an Sie zurückzuschicken.

7. Versenden Sie den Datenträger, und geben Sie die Nachverfolgungsnummer ein, um den Status der Lieferung zu verfolgen. Sobald der Datenträger im Datencenter eintrifft, wird er in das Speicherkonto kopiert, und der Status wird aktualisiert.

    ![Status "Abgeschlossen"](./media/backup-azure-backup-import-export/complete.png)

### Abschließen des Workflows
Nachdem die Daten der ersten Sicherung im Speicherkonto verfügbar sind, kopiert der Microsoft Azure Recovery Services-Agent den Inhalt der Daten aus diesem Konto in den Sicherungstresor oder den Recovery Services-Tresor (je nach Verfügbarkeit). Zum nächsten geplanten Sicherungszeitpunkt führt der Azure Backup-Agent die inkrementelle Sicherung über die erste Sicherungskopie durch.

## Nächste Schritte
- Wenn Sie Fragen zum Azure Import/Export-Workflow haben, helfen Ihnen die Informationen in diesem [Artikel](../storage/storage-import-export-service.md) weiter.
- Bei Fragen zum Workflow finden Sie weitere Informationen im Abschnitt zur Offlinesicherung in den [häufig gestellten Fragen](backup-azure-backup-faq.md) zu Azure Backup.

<!---HONumber=AcomDC_0824_2016-->