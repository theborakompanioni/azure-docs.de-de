<properties
  pageTitle="Vorbereiten Ihrer Umgebung für die Sicherung von Workloads mithilfe von Azure Backup Server | Microsoft Azure"
  description="Vergewissern Sie sich, dass Ihre Umgebung ordnungsgemäß für die Sicherung von Workloads mithilfe von Azure Backup Server vorbereitet ist."
  services="backup"
  documentationCenter=""
  authors="PVRK"
  manager="shivamg"
  editor=""
  keywords="Azure Backup Server: Sicherungstresor"/>

<tags
  ms.service="backup"
  ms.workload="storage-backup-recovery"
  ms.tgt_pltfrm="na"
  ms.devlang="na"
  ms.topic="article"
  ms.date="07/20/2016"
  ms.author="jimpark;trinadhk;pullabhk;markgal"/>

# Vorbereiten der Sicherung von Workloads per Azure Backup Server

> [AZURE.SELECTOR]
- [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Azure Backup Server (klassisch)](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM (klassisch)](backup-azure-dpm-introduction-classic.md)

In diesem Artikel wird das Vorbereiten Ihrer Umgebung für die Sicherung von Workloads per Azure Backup Server beschrieben. Mit Azure Backup Server können Sie Anwendungsworkloads wie Hyper-V-VMs, Microsoft SQL Server, SharePoint Server, Microsoft Exchange und Windows-Clients über eine zentrale Konsole schützen. Sie können auch IaaS-Workloads (Information as a Server) schützen, z.B. VMs in Azure.

> [AZURE.NOTE] Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../resource-manager-deployment-model.md). Dieser Artikel enthält die Informationen und Verfahren für die Wiederherstellung virtueller Computer, die über das Resource Manager-Bereitstellungsmodell bereitgestellt werden.

Azure Backup Server erbt einen Großteil der Funktionalität für die Workloadsicherung von Data Protection Manager (DPM). Dieser Artikel enthält Links zur DPM-Dokumentation mit einer Beschreibung der gemeinsamen Funktionen. Azure Backup Server verfügt auch über einen Großteil der Funktionen von DPM. Für Azure Backup Server werden keine Sicherungen auf Band durchgeführt, und es erfolgt auch keine Integration in System Center.

## 1\. Auswahl der Installationsplattform

Der erste Schritt zur Inbetriebnahme von Azure Backup Server ist die Einrichtung eines Computers mit Windows Server. Der Server kann sich unter Azure oder am lokalen Standort befinden.

### Verwenden eines Servers in Azure

Beim Auswählen eines Servers zum Ausführen von Azure Backup Server wird empfohlen, mit einem Katalogimage von Windows Server 2012 R2 Datacenter zu beginnen. Der Artikel [Erstellen Ihres ersten virtuellen Windows-Computers im Azure-Portal](..\virtual-machines\virtual-machines-windows-hero-tutorial.md) enthält ein Tutorial zu den ersten Schritten für den empfohlenen virtuellen Computer in Azure. Dies gilt auch, wenn Sie Azure noch nie verwendet haben. Die empfohlenen Mindestanforderungen für den virtuellen Servercomputer (VM) lauten: A2-Standard mit 2 Kernen und 3,5 GB RAM.

Beim Schützen von Workloads mit Azure Backup Server sind viele Feinheiten zu beachten. Dies wird im Artikel [Installieren von DPM als virtuellen Azure-Computer](https://technet.microsoft.com/library/jj852163.aspx) beschrieben. Lesen Sie sich den Artikel ganz durch, bevor Sie den Computer bereitstellen.

### Verwenden eines lokalen Servers

Wenn Sie den Basisserver nicht in Azure ausführen möchten, können Sie den Server auf einer Hyper-V-VM, einer VMware-VM oder einem physischen Host ausführen. Die empfohlenen Mindestanforderungen für die Serverhardware lauten 2 Kerne und 4 GB RAM. In der folgenden Tabelle sind die unterstützten Betriebssysteme aufgeführt:

| Betriebssystem | Plattform | SKU |
| :------------- |-------------| :-----|
|Windows Server 2012 R2 und die neuesten SPs|	64-Bit|	Standard, Datacenter, Foundation|
|Windows Server 2012 und die neuesten SPs|	64-Bit|	Datacenter, Foundation, Standard|
|Windows Storage Server 2012 R2 und die neuesten SPs |64-Bit|	Standard, Workgroup|
|Windows Storage Server 2012 und neuesten SPs |64-Bit |Standard, Workgroup|


Sie können die DPM-Speicherung deduplizieren, indem Sie die Windows Server-Deduplizierung verwenden. Lesen Sie die weiteren Informationen zur Zusammenarbeit von [DPM und Deduplizierung](https://technet.microsoft.com/library/dn891438.aspx) bei Bereitstellung auf Hyper-V-VMs.

> [AZURE.NOTE]  Es ist nicht möglich, Azure Backup Server auf einem Computer zu installieren, der als Domänencontroller ausgeführt wird.

Wenn Sie diesen Server zu einem späteren Zeitpunkt mit einer Domäne verknüpfen möchten, ist es ratsam, den Beitritt zur Domäne vor der Installation von Azure Backup Server durchzuführen. Das Verschieben eines vorhandenen Azure Backup Server-Computers in eine neue Domäne nach der Bereitstellung wird *nicht unterstützt*.

## 2\. Recovery Services-Tresor

Die Software muss unabhängig davon über eine Verbindung mit Azure verfügen, ob Sie Sicherungsdaten an Azure senden oder lokal verwalten. Genauer gesagt: Der Azure Backup Server-Computer muss für einen Recovery Services-Tresor registriert sein.

So erstellen Sie einen Recovery Services-Tresor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Klicken Sie im Hub-Menü auf **Durchsuchen**, und geben Sie in der Liste mit den Ressourcen **Recovery Services** ein. Wenn Sie mit der Eingabe beginnen, wird die Liste anhand Ihrer Eingaben gefiltert. Klicken Sie auf **Recovery Services-Tresor**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png) <br/>

    Die Liste mit den Recovery Services-Tresoren wird angezeigt.

3. Klicken Sie im Menü **Recovery Services-Tresore** auf **Hinzufügen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-azure-microsoft-azure-backup/rs-vault-menu.png)

    Das Blatt „Recovery Services-Tresor“ wird geöffnet, und Sie werden aufgefordert, **Name**, **Abonnement**, **Ressourcengruppe** und **Standort** anzugeben.

    ![Erstellen eines Recovery Services-Tresors – Schritt 5](./media/backup-azure-microsoft-azure-backup/rs-vault-attributes.png)

4. Geben Sie unter **Name** einen Anzeigenamen für den Tresor ein. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der zwischen 2 und 50 Zeichen enthält. Er muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.

5. Klicken Sie auf **Abonnement**, um die Liste mit den verfügbaren Abonnements anzuzeigen. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Organisationskonto mehreren Azure-Abonnements zugeordnet ist.

6. Klicken Sie auf **Ressourcengruppe**, um die Liste mit den verfügbaren Ressourcengruppen anzuzeigen, oder klicken Sie auf **Neu**, um eine neue Ressourcengruppe zu erstellen. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über Azure Resource Manager](../resource-group-overview.md).

7. Klicken Sie auf **Standort**, um die geografische Region für den Tresor auszuwählen.

8. Klicken Sie auf **Erstellen**. Es kann einige Zeit dauern, bis der Recovery Services-Tresor erstellt wurde. Verfolgen Sie die Benachrichtigungen oben rechts im Portal. Nachdem der Tresor erstellt wurde, wird er im Portal geöffnet.

### Festlegen der Speicherreplikation

Bei der Speicherreplikation haben Sie die Wahl zwischen georedundantem Speicher und lokal redundantem Speicher. Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Behalten Sie den georedundanten Speicher bei, wenn es sich hierbei um Ihre primäre Sicherung handelt. Wählen Sie lokal redundanten Speicher, wenn Sie eine günstigere und weniger langfristige Option wünschen. Weitere Informationen zu den Optionen für [georedundanten](../storage/storage-redundancy.md#geo-redundant-storage) und [lokal redundanten](../storage/storage-redundancy.md#locally-redundant-storage) Speicher finden Sie in der Übersicht [Azure Storage-Replikation](../storage/storage-redundancy.md).

So bearbeiten Sie die Einstellung für die Speicherreplikation:

1. Wählen Sie Ihren Tresor aus, um das Tresordashboard und das Blatt „Einstellungen“ zu öffnen. Sollte das Blatt **Einstellungen** nicht geöffnet werden, klicken Sie im Tresordashboard auf **Alle Einstellungen**.

2. Klicken Sie auf dem Blatt **Einstellungen** auf **Sicherungsinfrastruktur** > **Sicherungskonfiguration**, um das Blatt **Sicherungskonfiguration** zu öffnen. Wählen Sie auf dem Blatt **Sicherungskonfiguration** die Speicherreplikationsoption für Ihren Tresor aus.

    ![Liste der Sicherungstresore](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Wenn Sie die Speicheroption für Ihren Tresor ausgewählt haben, können Sie den virtuellen Computer dem Tresor zuordnen. Ermitteln und registrieren Sie die virtuellen Azure-Computer, um mit der Zuordnung zu beginnen.

## 3\. Softwarepaket

### Herunterladen des Softwarepakets
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Falls Sie bereits über einen geöffneten Recovery Services-Tresor verfügen, können Sie mit Schritt 3 fortfahren. Wenn Sie keinen Recovery Services-Tresor geöffnet haben und sich im Azure-Portal befinden, klicken Sie im Hub-Menü auf **Durchsuchen**.

    - Geben Sie in der Liste mit den Ressourcen **Recovery Services** ein.
    - Wenn Sie mit der Eingabe beginnen, wird die Liste anhand Ihrer Eingaben gefiltert. Klicken Sie auf **Recovery Services-Tresore**, wenn der Eintrag angezeigt wird.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png)

    Die Liste mit den Recovery Services-Tresoren wird angezeigt.

    - Wählen Sie in der Liste mit den Recovery Services-Tresoren einen Tresor aus.

    Das ausgewählte Tresor-Dashboard wird geöffnet.

    ![Tresorblatt öffnen](./media/backup-azure-microsoft-azure-backup/vault-dashboard.png)

3. Blatt „Einstellungen“ wird standardmäßig geöffnet. Wenn es geschlossen ist, klicken Sie auf **Einstellungen**, um das Blatt mit den Einstellungen zu öffnen.

    ![Tresorblatt öffnen](./media/backup-azure-microsoft-azure-backup/vault-setting.png)

4. Klicken Sie in **Erste Schritte** auf **Sicherung**, um den Assistenten für erste Schritte zu öffnen.

    ![Erste Schritte zur Sicherung](./media/backup-azure-microsoft-azure-backup/getting-started-backup.png)

5. Wenn der Assistent für erste Schritte geöffnet wird, ist der Bildschirm „Sicherungsziele“ automatisch ausgewählt. ![Backup-goals-default-opened](./media/backup-azure-microsoft-azure-backup/getting-started.png)

    Wählen Sie im Abschnitt **Sicherungsziele** die Option *lokal* für *Wo wird Ihre Workload ausgeführt?*.

    ![lokal und Workloads als Ziele](./media/backup-azure-microsoft-azure-backup/backup-goals-azure-backup-server.png)

6. Wählen Sie die Workloads, die Sie mithilfe von Azure Backup Server schützen möchten, in *Welche Workloads möchten Sie schützen?*, und klicken Sie auf **OK**.

    > [AZURE.NOTE] Wenn Sie nur Dateien und Ordner schützen möchten, sollten Sie den Azure Backup-Agent verwenden. Wenn Sie mehr Workloads als nur Dateien und Ordner schützen möchten, oder beabsichtigen, in Zukunft die Schutzanforderungen auszudehnen, wählen Sie alle diese Workloads.

    Dies ändert den Assistenten für erste Schritte für die Vorbereitung der Infrastruktur zum Schutz lokaler Workloads in Azure.

    ![Ändern des Erste-Schritte-Assistenten](./media/backup-azure-microsoft-azure-backup/getting-started-prep-infra.png)

7. Klicken Sie auf dem Blatt **Infrastruktur vorbereiten**, das geöffnet wird, auf die **Download**links für die Installation von Azure Backup Server und das Herunterladen der Tresoranmeldedaten. Sie verwenden die Tresoranmeldedaten bei der Registrierung von Azure Backup Server beim Recovery Services-Tresor. Mit den Links gelangen Sie zum Download Center, in dem Sie das Softwarepaket herunterladen können.

    ![Vorbereiten der Infrastruktur für Azure Backup Server](./media/backup-azure-microsoft-azure-backup/azure-backup-server-prep-infra.png)

8. Wählen Sie alle Dateien aus, und klicken Sie auf **Weiter**. Laden Sie alle Dateien herunter, die von der Microsoft Azure Backup-Downloadseite eingehen, und speichern Sie alle Dateien in dem gleichen Ordner.

    ![Download Center 1](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    Da die Downloadgröße aller Dateien zusammen mehr als 3 G beträgt, dauert es über einen Downloadlink mit 10 MBit/s bis zu 60 Minuten, bis der Download abgeschlossen ist.


### Extrahieren des Softwarepakets

Nachdem Sie alle Dateien heruntergeladen haben, klicken Sie auf **MicrosoftAzureBackupInstaller.exe**. Der **Setup-Assistent von Microsoft Azure Backup** wird gestartet, um die Setupdateien an einem Speicherort Ihrer Wahl zu extrahieren. Fahren Sie mit dem Assistenten fort, und klicken Sie auf die Schaltfläche **Extrahieren**, um den Extrahierungsprozess zu starten.

> [AZURE.WARNING] Zum Extrahieren der Setupdateien sind mindestens 4 GB freier Speicherplatz erforderlich.


![Setup-Assistent von Microsoft Azure Backup](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Aktivieren Sie nach Abschluss der Extrahierung das Kontrollkästchen, um die gerade extrahierte Datei *setup.exe* zu starten und mit der Installation von Microsoft Azure Backup Server zu beginnen. Klicken Sie dann auf die Schaltfläche **Fertig stellen**.

### Installieren des Softwarepakets

1. Klicken Sie auf **Microsoft Azure Backup**, um den Setup-Assistenten zu starten.

    ![Setup-Assistent von Microsoft Azure Backup](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)

2. Klicken Sie auf der Willkommensseite auf die Schaltfläche **Weiter**. Sie gelangen zum Abschnitt *Voraussetzungsüberprüfungen*. Klicken Sie auf diesem Bildschirm auf die Schaltfläche **Überprüfen**, um zu ermitteln, ob die Hardware- und Softwarevoraussetzungen für Azure Backup Server erfüllt sind. Wenn alle Voraussetzungen erfolgreich erfüllt wurden, sehen Sie eine Meldung, die darauf hinweist, dass der Computer die Anforderungen erfüllt. Klicken Sie auf die Schaltfläche **Weiter**.

    ![Azure Backup Server – Willkommen und Voraussetzungsüberprüfung](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)

3. Für Microsoft Azure Backup Server ist SQL Server Standard erforderlich, und im Azure Backup Server-Installationspaket sind die passenden SQL Server-Binärdaten enthalten. Beim Starten einer neuen Azure Backup Server-Installation sollten Sie die Option **Neue Instanz von SQL Server bei diesem Setup installieren** auswählen und auf die Schaltfläche **Prüfen und installieren** klicken. Nachdem die erforderlichen Komponenten erfolgreich installiert wurden, klicken Sie auf **Weiter**.

    ![Azure Backup Server – SQL-Überprüfung](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Tritt ein Fehler mit der Empfehlung auf, den Computer neu zu starten, tun Sie dies, und klicken Sie auf **Erneut prüfen**.

    > [AZURE.NOTE] Azure Backup Server funktioniert nicht mit einer Remoteinstanz von SQL Server. Die von Azure Backup Server verwendete Instanz muss lokal vorliegen.

4. Geben Sie einen Speicherort für die Installation der Microsoft Azure Backup-Serverdateien an, und klicken Sie auf **Weiter**.

    ![Voraussetzungen 2 für Microsoft Azure Backup](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    Das Scratchverzeichnis ist eine Anforderung für die Sicherung in Azure. Stellen Sie sicher, dass das Scratchverzeichnis eine Größe von mindestens 5 % der Daten aufweist, die in der Cloud gesichert werden. Für den Datenträgerschutz müssen separate Datenträger nach Abschluss der Installation konfiguriert werden. Weitere Informationen zu Speicherpools finden Sie unter [Konfigurieren von Speicherpools und Datenträgerspeicher](https://technet.microsoft.com/library/hh758075.aspx).

5. Geben Sie ein sicheres Kennwort für eingeschränkte lokale Benutzerkonten an, und klicken Sie auf **Weiter**.

    ![Voraussetzungen 2 für Microsoft Azure Backup](./media/backup-azure-microsoft-azure-backup/security-screen.png)

6. Bestimmen Sie, ob Sie mit *Microsoft Update* nach Updates suchen möchten, und klicken Sie auf **Weiter**.

    >[AZURE.NOTE] Es wird empfohlen, Windows Update an Microsoft Update umzuleiten, da Microsoft Update Sicherheit und wichtige Updates für Windows und andere Produkte wie Microsoft Azure Backup Server bietet.

    ![Voraussetzungen 2 für Microsoft Azure Backup](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)

7. Überprüfen Sie die *Zusammenfassung der Einstellungen*, und klicken Sie auf **Installieren**.

    ![Voraussetzungen 2 für Microsoft Azure Backup](./media/backup-azure-microsoft-azure-backup/summary-screen.png)

8. Die Installation erfolgt in mehreren Phasen. In der ersten Phase wird der Microsoft Azure Recovery Services-Agent auf dem Server installiert. Der Assistent überprüft auch das Vorhandensein der Internetverbindung. Wenn eine Internetverbindung verfügbar ist, können Sie mit der Installation fortfahren, andernfalls müssen Sie Proxydetails angeben, um die Verbindung mit dem Internet herzustellen.

    Der nächste Schritt ist die Konfiguration des Microsoft Azure Recovery Services-Agents. Bei der Konfiguration müssen Sie Ihre Tresoranmeldeinformationen zum Registrieren des Computers beim Recovery Services-Tresor angeben. Sie geben auch eine Passphrase zum Verschlüsseln bzw. Entschlüsseln von Daten an, die zwischen Azure und Ihrem lokalen Standort ausgetauscht werden. Sie können eine automatische Passphrase generieren oder eine eigene Passphrase mit mindestens 16 Zeichen angeben. Fahren Sie mit dem Assistenten fort, bis der Agent konfiguriert wurde.

    ![Voraussetzungen 2 für Azure Backup Server](./media/backup-azure-microsoft-azure-backup/mars/04.png)

9. Nach Abschluss der Registrierung von Microsoft Azure Backup Server fährt der übergreifende Setup-Assistent mit der Installation und Konfiguration von SQL Server und der Azure Backup Server-Komponenten fort. Nachdem die Installation der SQL Server-Komponenten abgeschlossen ist, werden die Azure Backup Server-Komponenten installiert.

    ![Azure Backup Server](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)


Wenn der Installationsschritt abgeschlossen ist, wurden auch die Desktopsymbole des Produkts bereits erstellt. Doppelklicken Sie einfach auf das Symbol, um das Produkt zu starten.

### Hinzufügen von Backup Storage

Die erste Sicherungskopie wird in einem Speicherbereich vorgehalten, der dem Azure Backup Server-Computer zugeordnet ist. Weitere Informationen zum Hinzufügen von Datenträgern finden Sie unter [Konfigurieren von Speicherpools und Datenträgerspeicher](https://technet.microsoft.com/library/hh758075.aspx).

> [AZURE.NOTE] Sie müssen auch dann Backup Storage hinzufügen, wenn Sie Daten an Azure senden möchten. Bei der aktuellen Architektur von Azure Backup Server enthält der Azure Backup-Tresor die *zweite* Kopie der Daten, und der lokale Speicher enthält die erste (obligatorische) Sicherungskopie.

## 4\. Netzwerkverbindung

Azure Backup Server muss mit dem Azure Backup-Dienst verbunden sein, um erfolgreich ausgeführt werden zu können. Verwenden Sie zum Überprüfen, ob der Computer über eine Verbindung mit Azure verfügt, das Cmdlet ```Get-DPMCloudConnection``` in der Azure Backup Server-PowerShell-Konsole. Wenn die Ausgabe des Cmdlets „TRUE“ lautet, besteht eine Verbindung, andernfalls nicht.

Gleichzeitig muss das Azure-Abonnement einen fehlerfreien Zustand aufweisen. Um den Status Ihres Abonnements zu ermitteln und es zu verwalten, melden Sie sich beim [Abonnementportal](https://account.windowsazure.com/Subscriptions) an.

Nachdem Sie den Status der Azure-Verbindung und des Azure-Abonnements kennen, können Sie anhand der Tabelle unten ermitteln, welche Auswirkungen mit einer Sicherungs-/Wiederherstellungsfunktion verbunden sind.

| Verbindungszustand | Azure-Abonnement | Sicherung auf Azure| Sicherung auf einen Datenträger | Wiederherstellung von Azure | Wiederherstellung von einem Datenträger |
| -------- | ------- | --------------------- | ------------------- | --------------------------- | ----------------------- |
| Verbunden | Aktiv | Zulässig | Zulässig | Zulässig | Zulässig |
| Verbunden | Abgelaufen | Beendet | Beendet | Zulässig | Zulässig |
| Verbunden | Bereitstellung aufgehoben | Beendet | Beendet | Beendet und Azure-Wiederherstellungspunkte gelöscht | Beendet |
| Verbindung vor mehr als 15 Tagen verloren | Aktiv | Beendet | Beendet | Zulässig | Zulässig |
| Verbindung vor mehr als 15 Tagen verloren | Abgelaufen | Beendet | Beendet | Zulässig | Zulässig |
| Verbindung vor mehr als 15 Tagen verloren | Bereitstellung aufgehoben | Beendet | Beendet | Beendet und Azure-Wiederherstellungspunkte gelöscht | Beendet |

### Wiederherstellung nach Verbindungsverlust
Wenn Sie über eine Firewall oder einen Proxy verfügen, die bzw. der den Zugriff auf Azure verhindert, müssen Sie im Profil der Firewall bzw. des Proxys die folgenden Domänenadressen auf die Positivliste setzen:

- www.msftncsi.com
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- *.windows.net

Nach dem Wiederherstellen der Verbindung mit Azure für den Azure Backup Server-Computer wird anhand des Azure-Abonnementstatus ermittelt, welche Vorgänge durchgeführt werden können. Die obige Tabelle enthält Details zu den Vorgängen, die nach dem „Verbinden“ des Computers zulässig sind.

### Behandeln von Abonnementstatus

Es ist möglich, den Status eines Azure-Abonnements von *Abgelaufen* oder *Bereitstellung aufgehoben* in *Aktiv* zu ändern. Dies ist aber mit Auswirkungen auf das Produktverhalten verbunden, solange der Status nicht *Aktiv* lautet:

- Ein Abonnement mit dem Status *Bereitstellung aufgehoben* verliert für den Zeitraum der Aufhebung die Funktionalität. Beim Festlegen auf *Aktiv* wird die Produktfunktionalität für Sicherung/Wiederherstellung wieder aktiviert. Die Sicherungsdaten auf der lokalen Festplatte können auch abgerufen werden, sofern sie mit einer ausreichend langen Beibehaltungsdauer versehen sind. Die Sicherungsdaten in Azure gehen aber unwiederbringlich verloren, wenn das Abonnement in den Status *Bereitstellung aufgehoben* versetzt wird.
- Für ein Abonnement mit dem Status *Abgelaufen* geht die Funktionalität nur so lange verloren, bis es wieder in den Status *Aktiv* versetzt wird. Alle Sicherungen für den Zeitraum, in dem sich das Abonnement im Status *Abgelaufen* befindet, werden nicht ausgeführt.


## Problembehandlung

Wenn auf dem Microsoft Azure Backup-Server während der Installationsphase (oder bei der Sicherung oder der Wiederherstellung) Fehler auftreten, verwenden Sie dieses [Dokument mit Fehlercodes](https://support.microsoft.com/kb/3041338), um weitere Informationen zu erhalten. Sie können auch [Azure Backup – Häufig gestellte Fragen](backup-azure-backup-faq.md) nutzen.


## Nächste Schritte

Ausführliche Informationen zum [Vorbereiten der Umgebung für DPM](https://technet.microsoft.com/library/hh758176.aspx) finden Sie auf der Microsoft TechNet-Website. Sie finden dort auch Informationen zu den unterstützten Konfigurationen, unter denen Azure Backup Server bereitgestellt und verwendet werden kann.

In den folgenden Artikeln finden Sie zusätzliche Informationen zum Workloadschutz mit einem Microsoft Azure Backup-Server.

- [SQL Server-Sicherung](backup-azure-backup-sql.md)
- [SharePoint Server-Sicherung](backup-azure-backup-sharepoint.md)
- [Sicherung eines anderen Servers](backup-azure-alternate-dpm-server.md)

<!---HONumber=AcomDC_0803_2016-->