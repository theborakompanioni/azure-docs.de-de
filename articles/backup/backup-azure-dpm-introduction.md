<properties
	pageTitle="Einführung in Azure DPM Backup | Microsoft Azure"
	description="Eine Einführung in die Sicherung von DPM-Servern mithilfe des Azure Backup-Diensts"
	services="backup"
	documentationCenter=""
	authors="Nkolli1"
	manager="shreeshd"
	editor=""
	keywords="System Center Data Protection Manager, Data Protection Manager, DPM-Sicherung"/>

<tags
	ms.service="backup"
	ms.workload="storage-backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/10/2016"
	ms.author="trinadhk;giridham;jimpark;markgal"/>

# Vorbereiten der Sicherung von Workloads in Azure mit DPM

> [AZURE.SELECTOR]
- [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Azure Backup Server (klassisch)](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM (klassisch)](backup-azure-dpm-introduction-classic.md)

Dieser Artikel enthält eine Einführung zur Verwendung von Microsoft Azure Backup zum Schutz von DPM-Servern und -Workloads (System Center Data Protection Manager). In diesem Artikel lernen Sie Folgendes:

- Funktionsweise der Azure DPM-Serversicherung
- Voraussetzungen für eine reibungslose Sicherung
- Typische Fehler und ihre Behandlung
- Unterstützte Szenarien

> [AZURE.NOTE] Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../resource-manager-deployment-model.md). Dieser Artikel enthält die Informationen und Verfahren für die Wiederherstellung virtueller Computer, die über das Resource Manager-Bereitstellungsmodell bereitgestellt werden.

System Center DPM sichert Datei- und Anwendungsdaten. Daten, die in DPM gesichert werden, können auf Band, auf Festplatte gespeichert oder in Azure mit Microsoft Azure Backup gesichert werden. DPM interagiert mit Azure Backup wie folgt:

- **DPM als physischer Server oder auf einem lokalen virtuellen Computer bereitgestellt** – Wenn DPM als physischer Server oder als lokaler Hyper-V-Computer bereitgestellt wird, können Sie Daten zusätzlich zur Festplatten- und Bandsicherung im Recovery Services-Tresor sichern.
- **DPM als virtueller Azure-Computer bereitgestellt** – Ab System Center 2012 R2 mit Update 3 kann DPM als virtueller Azure-Computer bereitgestellt werden. Wenn DPM als virtueller Azure-Computer bereitgestellt wird, können Sie Daten auf Azure-Datenträgern sichern, die an den virtuellen Azure DPM-Computer angeschlossen sind, oder Sie können die Datenspeicherung durch Sichern in einem Recovery Services-Tresor auslagern.

## Warum sollten Sie DPM-Server sichern?

Vorteile der Verwendung von Azure Backup für die Sicherung von DPM-Servern:

- Für die lokale DPM-Bereitstellung können Sie Azure als Alternative zur langfristigen Bereitstellung auf Band verwenden.
- Bei DPM-Bereitstellungen in Azure ermöglicht Azure Backup das Auslagern des Speichers vom Azure-Datenträger, sodass Sie zentral hochskalieren können, indem Sie ältere Daten im Recovery Services-Tresor und neue Daten auf dem Datenträger speichern.

## Funktionsweise der DPM-Serversicherung
Um datenträgerbasierten Datenschutz bereitzustellen, erstellt der DPM-Server ein Replikat bzw. eine Kopie der Daten, die sich auf geschützten Servern befindet, und verwaltet sie. Die Replikate werden im Speicherpool gespeichert, der aus einer Gruppe von Datenträgern auf dem DPM-Server oder einem benutzerdefinierten Volume besteht. Ob Sie Datei- oder Anwendungsdaten schützen möchten, der Schutz beginnt stets mit der Erstellung des Replikats der Datenquelle. Das Replikat wird in regelmäßigen Abständen gemäß den Einstellungen, die Sie konfigurieren, synchronisiert, d.h. aktualisiert. Wenn Sie kurzfristigen datenträgerbasierten Schutz und langfristigen Schutz in der Cloud verwenden, kann DPM Daten aus dem Replikatvolume im Recovery Services-Tresor sichern, damit keine Auswirkungen auf den geschützten Computer auftreten.

## Voraussetzungen
Bereiten Sie Azure Backup wie folgt zum Sichern von DPM-Daten vor:

1. **Erstellen eines Recovery Services-Tresors** – Erstellen Sie einen Tresor im Azure-Portal.
2. **Herunterladen von Tresoranmeldeinformationen** – Laden Sie die Anmeldeinformationen herunter, die Sie verwenden, um den DPM-Server beim Recovery Services-Tresor zu registrieren.
3. **Installieren des Azure Backup-Agents und Registrieren des Servers** – Installieren Sie von Azure Backup aus den Agent auf jedem DPM-Server, und registrieren Sie den DPM-Server beim Recovery Services-Tresor.

### 1\. Erstellen eines Recovery Services-Tresors
So erstellen Sie einen Recovery Services-Tresor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Klicken Sie im Hub-Menü auf **Durchsuchen**, und geben Sie in der Liste mit den Ressourcen **Recovery Services** ein. Wenn Sie mit der Eingabe beginnen, wird die Liste anhand Ihrer Eingaben gefiltert. Klicken Sie auf **Recovery Services-Tresor**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-dpm-introduction/open-recovery-services-vault.png)

    Die Liste mit den Recovery Services-Tresoren wird angezeigt.

3. Klicken Sie im Menü **Recovery Services-Tresore** auf **Hinzufügen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-azure-dpm-introduction/rs-vault-menu.png)

    Das Blatt „Recovery Services-Tresor“ wird geöffnet, und Sie werden aufgefordert, **Name**, **Abonnement**, **Ressourcengruppe** und **Standort** anzugeben.

    ![Erstellen eines Recovery Services-Tresors – Schritt 5](./media/backup-azure-dpm-introduction/rs-vault-attributes.png)

4. Geben Sie unter **Name** einen Anzeigenamen für den Tresor ein. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der zwischen 2 und 50 Zeichen enthält. Er muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.

5. Klicken Sie auf **Abonnement**, um die Liste mit den verfügbaren Abonnements anzuzeigen. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Organisationskonto mehreren Azure-Abonnements zugeordnet ist.

6. Klicken Sie auf **Ressourcengruppe**, um die Liste mit den verfügbaren Ressourcengruppen anzuzeigen, oder klicken Sie auf **Neu**, um eine neue Ressourcengruppe zu erstellen. Umfassende Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../resource-group-overview.md).

7. Klicken Sie auf **Standort**, um die geografische Region für den Tresor auszuwählen.

8. Klicken Sie auf **Erstellen**. Es kann einige Zeit dauern, bis der Recovery Services-Tresor erstellt wurde. Verfolgen Sie die Benachrichtigungen oben rechts im Portal. Nachdem der Tresor erstellt wurde, wird er im Portal geöffnet.

### Festlegen der Speicherreplikation

Bei der Speicherreplikation haben Sie die Wahl zwischen georedundantem Speicher und lokal redundantem Speicher. Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Behalten Sie den georedundanten Speicher bei, wenn es sich hierbei um Ihre primäre Sicherung handelt. Wählen Sie lokal redundanten Speicher, wenn Sie eine günstigere und weniger langfristige Option wünschen. Weitere Informationen zu den Optionen für [georedundanten](../storage/storage-redundancy.md#geo-redundant-storage) und [lokal redundanten](../storage/storage-redundancy.md#locally-redundant-storage) Speicher finden Sie in der Übersicht [Azure Storage-Replikation](../storage/storage-redundancy.md).

So bearbeiten Sie die Einstellung für die Speicherreplikation:

1. Wählen Sie Ihren Tresor aus, um das Tresordashboard und das Blatt „Einstellungen“ zu öffnen. Sollte das Blatt **Einstellungen** nicht geöffnet werden, klicken Sie im Tresordashboard auf **Alle Einstellungen**.

2. Klicken Sie auf dem Blatt **Einstellungen** auf **Sicherungsinfrastruktur** > **Sicherungskonfiguration**, um das Blatt **Sicherungskonfiguration** zu öffnen. Wählen Sie auf dem Blatt **Sicherungskonfiguration** die Speicherreplikationsoption für Ihren Tresor aus.

    ![Liste der Sicherungstresore](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Wenn Sie die Speicheroption für Ihren Tresor ausgewählt haben, können Sie den virtuellen Computer dem Tresor zuordnen. Ermitteln und registrieren Sie die virtuellen Azure-Computer, um mit der Zuordnung zu beginnen.


### 2\. Herunterladen der Tresoranmeldedaten

Die Datei mit Tresoranmeldeinformationen ist ein Zertifikat, das vom Portal für jeden Sicherungstresor generiert wird. Das Portal lädt anschließend den öffentlichen Schlüssel in den Access Control Service (ACS) hoch. Der private Schlüssel des Zertifikats wird dem Benutzer während des Workflows zur Verfügung gestellt und dient als Eingabe für den Workflow zur Computerregistrierung. Dadurch wird der Computer zum Senden von Sicherungsdaten an einen identifizierten Tresor im Azure Backup-Dienst authentifiziert.

Die Tresoranmeldeinformationen werden nur während des Registrierungsworkflows verwendet. Es ist Aufgabe des Benutzers sicherzustellen, dass die Datei mit den Tresoranmeldeinformationen sicher aufbewahrt wird. Fällt sie in die Hände eines böswilligen Benutzers, kann dieser die Datei mit den Tresoranmeldeinformationen zur Registrierung weiterer Computer beim selben Tresor verwenden. Da die Sicherungsdaten jedoch durch eine Passphrase verschlüsselt sind, die dem Kunden gehört, sind vorhandene Sicherungsdaten nicht gefährdet. Um dieses Risiko auf ein Mindestmaß zu verringern, laufen die Tresoranmeldeinformationen nach 48 Stunden ab. Sie können die Anmeldeinformationen eines Recovery Services-Tresors beliebig oft herunterladen – jedoch nur die neueste Datei mit Tresoranmeldeinformationen ist für den Registrierungsworkflow gültig.

Die Datei mit Tresoranmeldeinformationen wird über einen sicheren Kanal aus dem Azure-Portal heruntergeladen. Der Azure Backup-Dienst kennt nicht den privaten Schlüssel des Zertifikats, und der private Schlüssel wird weder im Portal noch im Dienst aufbewahrt. Gehen Sie folgendermaßen vor, um die Datei mit Tresoranmeldeinformationen auf einen lokalen Computer herunterzuladen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Öffnen Sie den Recovery Services-Tresor, bei dem Sie den DPM-Computer registrieren möchten.

3. Blatt „Einstellungen“ wird standardmäßig geöffnet. Wenn es geschlossen ist, klicken Sie auf dem Tresordashboard auf **Einstellungen**, um das Einstellungenblatt zu öffnen. Klicken Sie im Blatt „Einstellungen“ auf **Eigenschaften**.

	![Tresorblatt öffnen](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)

4. Klicken Sie auf der Seite „Eigenschaften“ unter **Sicherungsanmeldeinformationen** auf **Herunterladen**. Das Portal generiert die Datei mit Tresoranmeldeinformationen, die zum Download zur Verfügung gestellt wird.

    ![Herunterladen](./media/backup-azure-dpm-introduction/vault-credentials.png)

Das Portal generiert Tresoranmeldeinformationen mit einer Kombination aus dem Tresornamen und dem aktuellen Datum. Klicken Sie auf **Speichern**, um die Tresoranmeldeinformationen in den Downloadordner des lokalen Kontos herunterzuladen. Sie können auch im Menü "Speichern" die Option "Speichern unter" auswählen, um einen anderen Speicherort anzugeben. Es dauert bis zu einer Minute, bis die Datei generiert ist.

### Hinweis
- Stellen Sie sicher, dass die Tresoranmeldeinformationen-Datei an einem Ort gespeichert wird, der von Ihrem Computer aus zugänglich ist. Wenn sie in einer Dateifreigabe/einem SMB gespeichert sind, überprüfen Sie die Zugriffsberechtigungen.
- Die Datei mit den Tresoranmeldeinformationen wird nur während des Registrierungsworkflows verwendet.
- Die Datei mit den Tresoranmeldeinformationen läuft nach 48 Stunden ab und kann über das Portal heruntergeladen werden.



### 3\. Installieren des Backup-Agents

Nach dem Erstellen des Azure Backup-Tresors sollte auf jedem Windows-Computer (Windows Server, Windows-Client oder System Center Data Protection Manager-Server oder Azure Backup Server-Computer) ein Agent installiert werden, der das Sichern von Daten und Anwendungen in Azure ermöglicht.

1. Öffnen Sie den Recovery Services-Tresor, bei dem Sie den DPM-Computer registrieren möchten.

2. Blatt „Einstellungen“ wird standardmäßig geöffnet. Wenn es geschlossen ist, klicken Sie auf **Einstellungen**, um das Einstellungenblatt zu öffnen. Klicken Sie im Blatt „Einstellungen“ auf **Eigenschaften**.

	![Tresorblatt öffnen](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)

3. Klicken Sie auf der Seite „Einstellungen“ unter **Azure Backup-Agent** auf **Herunterladen**.

    ![Herunterladen](./media/backup-azure-dpm-introduction/azure-backup-agent.png)

   Sobald der Agent heruntergeladen ist, doppelklicken Sie auf „MARSAgentInstaller.exe“, um die Installation des Azure Backup-Agents zu starten. Wählen Sie den Installationsordner und den Ablageordner aus, der für den Agent erforderlich ist. Der verfügbare Speicherplatz am angegebenen Speicherort muss mindestens 5 % der Sicherungsdaten betragen.

4.	Wenn Sie einen Proxyserver für die Verbindung mit dem Internet verwenden, geben Sie im Bildschirm **Proxykonfiguration** die Details des Proxyservers ein. Wenn Sie einen authentifizierten Proxy verwenden, geben Sie in diesem Bildschirm die Informationen zum Benutzernamen und zum Kennwort ein.

5.	Der Azure Backup-Agent installiert .NET Framework 4.5 und Windows PowerShell (falls noch nicht verfügbar), um die Installation abzuschließen.

6.	Wenn der Agent installiert ist, klicken Sie auf die Schaltfläche **Mit Registrierung fortfahren**, um den Workflow fortzusetzen.

    ![Registrieren](../../includes/media/backup-install-agent/register.png)

7. Suchen Sie im Bildschirm für die Tresoranmeldeinformationen nach der zuvor heruntergeladenen Datei mit den Tresoranmeldeinformationen, und wählen Sie sie aus.

    ![Tresoranmeldeinformationen](../../includes/media/backup-install-agent/vc.png)

    Die Datei mit den Tresoranmeldeinformationen gilt nur für 48 Stunden (nachdem sie aus dem Portal heruntergeladen wurde). Wenn in diesem Bildschirm Fehler auftreten (z. B. „Datei mit Tresoranmeldeinformationen abgelaufen“), melden Sie sich beim Azure-Portal an, und laden Sie die Datei mit den Tresoranmeldeinformationen erneut herunter.

    Stellen Sie sicher, dass die Datei mit den Tresoranmeldeinformationen an einem Speicherort verfügbar ist, der für die Setupanwendung zugänglich ist. Wenn Zugriffsfehler auftreten, kopieren Sie die Datei mit den Tresoranmeldeinformationen in einen temporären Speicherort auf diesem Computer, und wiederholen Sie den Vorgang.

    Wenn ein Fehler wegen ungültiger Tresoranmeldeinformationen angezeigt wird (z. B. „Ungültige Tresoranmeldeinformationen angegeben“), ist die Datei entweder beschädigt oder enthält nicht die aktuellen Anmeldeinformationen, die dem Wiederherstellungsdienst zugeordnet sind. Wiederholen Sie den Vorgang, nachdem Sie eine neue Datei mit Tresoranmeldeinformationen vom Portal heruntergeladen haben. Dieser Fehler tritt i. d. R. auf, wenn der Benutzer im Azure-Portal in schneller Folge auf die Option **Tresoranmeldedaten herunterladen** klickt. In diesem Fall ist nur die zweite Datei mit Tresoranmeldeinformationen gültig.

8. Im Bildschirm **Verschlüsselungseinstellung** können Sie entweder eine Passphrase generieren oder eine Passphrase angeben (mindestens 16 Zeichen). Vergessen Sie nicht, die Passphrase an einem sicheren Speicherort zu speichern.

    ![Verschlüsselung](../../includes/media/backup-install-agent/encryption.png)

    > [AZURE.WARNING] Wenn die Passphrase verloren geht oder vergessen wird, kann Microsoft Ihnen bei der Wiederherstellung der Sicherungsdaten nicht behilflich sein. Der Endbenutzer besitzt die Verschlüsselungspassphrase, und Microsoft kann nicht auf die vom Endbenutzer verwendete Passphrase zugreifen. Speichern Sie die Datei an einem sicheren Speicherort, da sie bei einem Wiederherstellungsvorgang benötigt wird.

9. Nach dem Klicken auf die Schaltfläche **Fertig stellen** wird der Computer erfolgreich beim Tresor registriert, und Sie können mit der Sicherung in Microsoft Azure beginnen.

10. Bei eigenständiger Verwendung von Microsoft Azure Backup können Sie die während des Registrierungsworkflows angegebenen Einstellungen ändern, indem Sie im Azure Backup-MMC-Snap-In auf die Option **Eigenschaften ändern** klicken.

    ![Eigenschaften ändern](../../includes/media/backup-install-agent/change.png)

    Alternativ können Sie bei Verwendung von Data Protection Manager die während des Registrierungsworkflows angegebenen Einstellungen ändern, indem Sie auf der Registerkarte **Verwaltung** unter **Online** auf die Option **Konfigurieren** klicken.

    ![Konfigurieren von Azure Backup](../../includes/media/backup-install-agent/configure.png)

## Anforderungen (und Einschränkungen)

- DPM kann als physischer Server oder als virtueller Hyper-V-Computer, der auf System Center 2012 SP1 oder System Center 2012 R2 installiert ist, ausgeführt werden. DPM kann auch als virtueller Azure-Computer unter System Center 2012 R2 mit mindestens DPM 2012 R2 Update Rollup 3 oder auf einem virtuellen Windows-Computer in VMWare in System Center 2012 R2 mit mindestens Update Rollup 5 ausgeführt werden.
- Wenn Sie DPM mit System Center 2012 SP1 ausführen, sollten Sie Update Rollup 2 für System Center Data Protection Manager SP1 installieren. Dies ist erforderlich, bevor Sie den Azure Backup-Agent installieren können.
- Auf dem DPM-Server muss Azure PowerShell und .Net Framework 4.5 installiert sein.
- DPM kann die meisten Workloads in Azure Backup sichern. Eine vollständige Liste der Unterstützungen finden Sie in den Azure Backup-Elementen weiter unten.
- Mit der Option "Kopieren auf Band" können Daten, die in Azure Backup gespeichert sind, nicht wiederhergestellt werden.
- Sie benötigen ein Azure-Konto mit aktivierter Azure Backup-Funktion. Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Erfahren Sie mehr über [Preisgestaltung von Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
- Für das Verwenden von Azure Backup muss der Azure Backup-Agent auf den Servern installiert werden, die Sie sichern möchten. Jeder Server muss mindestens 5% der zu sichernden Datengröße als freien lokalen Speicher bereitstellen. Beispielsweise erfordert das Sichern von 100GB an Daten mindestens 5GB freien Speicherplatz im Scratchverzeichnis.
- Daten werden im Azure-Tresorspeicher gespeichert. Es gibt keine Beschränkung der Datenmenge, die Sie in einem Azure Backup-Tresor sichern, aber die Größe einer Datenquelle (z. B. ein virtueller Computer oder eine Datenbank) sollte 54400 GB nicht überschreiten.

Diese Dateitypen werden für die Sicherung in Azure unterstützt:

- Verschlüsselt (nur vollständige Sicherungen)
- Komprimiert (inkrementelle Sicherungen werden unterstützt)
- Platzsparend (inkrementelle Sicherungen werden unterstützt)
- Komprimiert und platzsparend (als platzsparend behandelt)

Diese werden nicht unterstützt:

- Server auf Dateisystemen, die nach Groß-/Kleinschreibung unterscheiden, werden nicht unterstützt.
- Feste Links (übersprungen)
- Analysepunkte (übersprungen)
- Verschlüsselt und komprimiert (übersprungen)
- Verschlüsselt und platzsparend (übersprungen)
- Komprimierter Stream
- Platzsparender Stream

>[AZURE.NOTE] Ab System Center 2012 DPM mit SP1 können Sie Workloads, die von DPM geschützt werden, in Azure mithilfe von Microsoft Azure Backup sichern.

<!---HONumber=AcomDC_0803_2016-->