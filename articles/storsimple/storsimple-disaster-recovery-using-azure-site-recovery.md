<properties 
   pageTitle="Automatisieren von DR für Dateifreigaben auf StorSimple mithilfe von Azure Site Recovery | Microsoft Azure"
   description="Beschreibt die Schritte und bewährte Methoden zum Erstellen einer Lösung zur Notfallwiederherstellung für auf StorSimple-Speicher gehostete Dateifreigaben."
   services="storsimple"
   documentationCenter="NA"
   authors="vidarmsft"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/16/2016"
   ms.author="vidarmsft" />

# Automatische Lösung zur Notfallwiederherstellung mithilfe von Azure Site Recovery für auf StorSimple gehostete Dateifreigaben

## Übersicht

Microsoft Azure StorSimple ist eine Hybridcloud-Speicherlösung, die sich mit der Komplexität von unstrukturierten Daten befasst, die bei Datenfreigabe häufig auftreten. StorSimple verwendet den Cloudspeicher als eine Erweiterung der lokalen Lösung und verteilt die Daten automatisch auf die Ebenen des lokalen Speichers und des Cloudspeichers. Durch den integrierten Datenschutz mit lokalen Momentaufnahmen und Cloudmomentaufnahmen wird keine ausgedehnte Speicherinfrastruktur mehr benötigt.

[Azure Site Recovery](../site-recovery/site-recovery-overview.md) ist ein auf Azure basierter Dienst, der Funktionen für die Notfallwiederherstellung (Disaster Recovery; DR) bietet, indem die Replikation, das Failover und die Wiederherstellung virtueller Computer koordiniert wird. Azure Site Recovery unterstützt eine Reihe von Replikationstechnologien, um für virtuelle Computer und Anwendungen eine durchgängig die Replikation, den Schutz und ein reibungsloses Failover auf private/öffentliche Clouds bzw. auf gehostete Clouds zu bieten.

Mithilfe von Azure Site Recovery, der Replikation von virtuellen Computern und StorSimple-Funktionen für Cloudmomentaufnahmen können Sie die gesamte Dateiserverumgebung schützen. Im Falle einer Unterbrechung können Sie durch einen Mausklick Ihre Dateifreigaben in nur wenigen Minuten auf Azure online stellen.

In diesem Dokument wird ausführlich erläutert, wie Sie eine Lösung für die Notfallwiederherstellung für Ihre Dateifreigabe auf dem StorSimple-Speicher erstellen und ein geplantes Failover, ein ungeplantes Failover und ein Testfailover mithilfe eines mit einem Klick aktivierbaren Wiederherstellungsplans ausführen. Im Wesentlichen wird gezeigt, wie Sie den Wiederherstellungsplan in Ihrem Azure Site Recovery-Tresor ändern können, um StorSimple-Failover während Notfallszenarien zu aktivieren. Darüber hinaus werden unterstützte Konfigurationen und Voraussetzungen beschrieben. In diesem Dokument wird davon ausgegangen, dass Sie mit den Grundlagen der Azure Site Recovery- und StorSimple-Architekturen vertraut sind.

## Unterstütze Azure Site Recovery-Bereitstellungsoptionen

Kunden können Dateiserver als physische Server oder virtuelle Computer (VMs) bereitstellen, die auf Hyper-V oder VMware laufen. Anschließend können Sie einen Teil des StoreSimple-Speichers abzweigen, um einen Bereich für den Dateiaustausch zu schaffen. Azure Site Recovery kann jeweils physische und virtuelle Bereitstellungen entweder auf einem sekundären Standort oder auf Azure schützen. Dieses Dokument behandelt die Details einer DR-Lösung mit Azure als Wiederherstellungsstandort für eine Dateiserver-VM, die auf Hyper-V-VM und mit Dateifreigaben auf dem StorSimple-Speicher gehostet wird. Andere Szenarien, in denen sich die Dateiserver-VM auf einer VMware-VM oder einem physischen Computer befindet, können auf ähnliche Weise implementiert werden.

## Voraussetzungen

Für das Implementieren einer Lösung zur Notfallwiederherstellung, die Azure Site Recovery für auf dem StorSimple-Speicher gehostete Dateifreigaben verwendet, gelten die folgenden Voraussetzungen:

-   Eine lokale Windows Server 2012 R2-Dateiserver-VM, die auf Hyper-V oder VMware oder einem physischen Computer gehostet wird

-   Ein lokales StorSimple-Speichergerät, das bei Azure StorSimple Manager registriert ist

-   Die StorSimple Cloud Appliance, die in Azure StorSimple Manager erstellt wird (kann im heruntergefahrenen Zustand beibehalten werden)

-   Dateifreigaben, die auf den vom StorSimple-Speichergerät konfigurierten Volumes gehostet werden.

-   [Ein Azure Site Recovery Services-Tresor](../site-recovery/site-recovery-vmm-to-vmm.md), der in einem Microsoft Azure-Abonnement erstellt wurde.

Falls Azure Ihr Wiederherstellungsstandort ist, führen Sie zusätzlich das [Bereitschaftsbewertungstool für virtuelle Computer](http://azure.microsoft.com/downloads/vm-readiness-assessment/) auf VMs aus, um sicherzustellen, dass diese mit Azure VMs und Azure Site Recovery Services kompatibel sind.

Stellen Sie sicher, dass Sie Ihre StorSimple Cloud Appliance, Ihr Automation-Konto und Ihr Speicherkonto/ Ihre Speicherkonten in der gleichen Region erstellt haben, um Latenzprobleme zu vermeiden (die möglicherweise zu höheren Kosten führen).

## Aktivieren von DR für StorSimple-Dateifreigaben  

Jede Komponente der lokalen Umgebung muss geschützt werden, um die vollständige Replikation und Wiederherstellung zu aktivieren. In diesem Abschnitt werden folgende Themen beschrieben:

-   Einrichten der Replikation von Active Directory und DNS (optional)

-   Verwenden von Azure Site Recovery zum Aktivieren des Schutzes der Dateiserver-VM

-   Aktivieren des Schutzes von StorSimple-Volumes

-   Konfigurieren des Netzwerks

### Einrichten der Replikation von Active Directory und DNS (optional)

Wenn Sie die Computer, auf denen Active Directory und DNS ausgeführt wird, so schützen möchten, dass diese auf dem DR-Standort verfügbar sind, müssen Sie sie explizit schützen (sodass auf die Dateiserver nach dem Failover mit der entsprechenden Authentifizierung zugegriffen werden kann). Basierend auf der Komplexität der lokalen Umgebung des Kunden gibt es zwei empfohlene Optionen.

#### Option 1:

Falls der Kunde über eine kleine Anzahl von Anwendungen und über einen einzelnen Domänencontroller für die gesamte lokale Umgebung verfügt, und ein Failover für den gesamten Standorts ausführt, empfehlen wir die Verwendung der Azure Site Recovery-Replikation zum Replizieren des Computers mit dem Domänencontroller an einen sekundären Standort (gilt sowohl für die Standort-zu-Standort- als auch für die Standort-zu-Azure-Replikation).

#### Option 2:

Wenn der Kunde über eine große Anzahl von Anwendungen verfügt und eine Active Directory-Gesamtstruktur ausführt, und ein Failover für einige Anwendungen gleichzeitig ausführt, empfehlen wir die Einrichtung eines zusätzlichen Domänencontrollers am DR-Standort (entweder ein sekundärer Standort oder in Azure).

Weitere Anleitungen zur Bereitstellung eines Domänencontrollers am DR-Standort finden Sie unter [Schützen von Active Directory und DNS mit Azure Site Recovery](../site-recovery/site-recovery-active-directory.md). Für den Rest dieses Dokuments wird davon ausgegangen, dass ein Domänencontroller am DR-Standort verfügbar ist.

### Verwenden von Azure Site Recovery zum Aktivieren des Schutzes der Dateiserver-VM

Dieser Schritt erfordert, dass Sie die lokale Dateiserverumgebung vorbereiten, einen Azure Site Recovery-Tresor erstellen und vorbereiten und den Dateischutz des virtuellen Computers aktivieren.

#### So bereiten Sie die lokale Dateiserverumgebung vor

1.  Legen Sie die **Benutzerkontensteuerung** auf **Nie benachrichtigen** fest. Dies ist erforderlich, damit Sie Azure-Automatisierungsskripts zum Verbinden der iSCSI-Ziele nach dem Failover durch Azure Site Recovery verwenden können.

    1.  Drücken Sie die Windows-Taste „+Q“, und suchen Sie nach **UAC**.

    2.  Wählen Sie **Einstellungen der Benutzerkontensteuerung ändern** aus.

    3.  Ziehen Sie den Balken nach unten zu **Nie benachrichtigen**.

    4.  Klicken Sie auf **OK** und wählen Sie dann **Ja** aus, wenn Sie dazu aufgefordert werden.

		![](./media/storsimple-dr-using-asr/image1.png)

1.  Installieren Sie den VM-Agent auf jedem der Dateiserver-VMs. Dies ist erforderlich, damit Sie Azure-Automatisierungsskripte auf VMs ausführen können, für die ein Failover ausgeführt wurde.

    1.  Geben Sie beim [Herunterladen des Agent](http://aka.ms/vmagentwin) `C:\\Users\<username>\\Downloads` als Ziel an.

    2.  Öffnen Sie Windows PowerShell im Administratormodus (als Administrator ausführen), und geben Sie anschließend den folgenden Befehl ein, um zum Downloadpfad zu navigieren:

		`cd C:\\Users\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

		> [AZURE.NOTE] Der Dateiname kann sich je nach Version ändern.

1.  Klicken Sie auf **Weiter**.

2.  Akzeptieren Sie die **Nutzungsbedingungen**, und klicken Sie anschließend auf **Weiter**.

3.  Klicken Sie auf **Fertig stellen**.


1.  Erstellen Sie Dateifreigaben mithilfe von Volumes, die aus dem StorSimple-Speicher stammen. Weitere Informationen finden Sie unter [Verwalten von Volumes mithilfe des StorSimple Manager-Diensts](storsimple-manage-volumes.md).

    1.  Klicken Sie auf Ihren lokalen VMs die Windows-Taste „+Q“, und suchen Sie nach **iSCSI**.

    2.  Wählen Sie den **iSCSI-Initiator** aus.

    3.  Wählen Sie die Registerkarte **Konfiguration** aus, und kopieren Sie den Initiatornamen.

    4.  Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/) an.

    5.  Wählen Sie die Registerkarte **StorSimple** aus, und wählen Sie anschließend den StorSimple Manager-Dienst aus, der das physische Gerät enthält.

    6.  Erstellen Sie einen oder mehrere Volumecontainer, und erstellen Sie Volume(s). (Diese Volumes dienen der Dateifreigabe(n) auf den Dateiserver-VMs). Kopieren Sie den Initiatornamen, und vergeben Sie einen passenden Namen für die Access Control-Datensätze, wenn Sie die Volumes erstellen.

    7.  Wählen Sie die Registerkarte **Konfigurieren** aus, und notieren Sie sich die IP-Adresse des Geräts.

    8.  Wechseln Sie auf Ihren lokalen VMs erneut zum **iSCSI-Initiator**, und geben Sie die IP-Adresse im Abschnitt „Schnellverbindung“ ein. Klicken Sie auf **Schnellverbindung** (das Gerät sollte jetzt verbunden werden).

    9.  Öffnen Sie das Azure-Verwaltungsportal, und wählen Sie die Registerkarte **Volumes und Geräte** aus. Klicken Sie auf **Automatische Konfiguration**. Das Volume, das Sie gerade erstellt haben, sollte angezeigt werden.

    10. Wählen Sie im Portal die Registerkarte**Geräte** aus, und wählen Sie anschließend **Create a New Virtual Device** (Erstellen eines neuen virtuellen Geräts) aus. (Dieses virtuelle Gerät wird verwendet, wenn ein Failover auftritt). Dieses neue virtuelle Gerät kann im Offlinestatus beibehalten werden, um zusätzliche Kosten zu vermeiden. Wechseln Sie zum Abschnitt **Virtual Machines** im Portal, und beenden Sie das virtuelle Gerät, um es offline zu schalten.

    11. Wechseln Sie zurück zu den lokalen VMs, und öffnen Sie Datenträgerverwaltung (drücken Sie die Windows-Taste „+ X“, und wählen Sie **Datenträgerverwaltung**).

    12. Sie sehen einige zusätzliche Datenträger (abhängig von der Anzahl von Volumes, die Sie erstellt haben). Klicken Sie mit der rechten Maustaste auf den ersten Datenträger, wählen Sie **Datenträger initialisieren** aus und anschließend **OK**. Klicken Sie mit der rechten Maustaste auf den Abschnitt **Nicht zugeordnet**, wählen Sie **Neues einfaches Volumen** aus, weisen Sie ihm einen Laufwerkbuchstaben zu, und beenden Sie den Assistenten.

    13. Wiederholen Sie Schritt 1 für alle Datenträger. Sie sehen jetzt alle Datenträger auf **diesem PC** im Windows-Explorer.

    14. Verwenden Sie die Rolle „Datei- und Speicherdienste“, um Dateifreigaben auf diesen Volumes zu erstellen.

#### So erstellen Sie einen Azure Site Recovery-Tresor und bereiten diesen vor

Sehen Sie sich die [Azure Site Recovery-Dokumentation](../site-recovery/site-recovery-hyper-v-site-to-azure.md) an, um die ersten Schritte mit Azure Site Recovery auszuführen, bevor Sie die Dateiserver-VM schützen.

#### So aktivieren Sie den Schutz

1.  Trennen Sie das iSCSI-Ziel/die iSCSI-Ziele von den lokalen VMs, die Sie durch Azure Site Recovery schützen möchten:

    1.  Drücken Sie die Windows-Taste „+Q“, und suchen Sie nach **iSCSI**.

    2.  Wählen Sie **iSCSI-Initiator einrichten** aus.

    3.  Trennen Sie das StorSimple-Gerät,für das Sie zuvor eine Verbindung hergestellt haben. Alternativ können Sie während der Aktivierung des Schutzes den Dateiserver für ein paar Minuten abschalten.

	> [AZURE.NOTE] Dies bewirkt, dass die Dateifreigaben vorübergehend nicht verfügbar sind.

1.  [Aktivieren des Schutzes für virtueller Computer](../site-recovery/site-recovery-hyper-v-site-to-azure.md##step-6-enable-replication) der Dateiserver-VM aus dem Azure Site Recovery-Portal.

2.  Wenn die erste Synchronisierung beginnt, können Sie die Verbindung mit dem Ziel wieder herstellen. Wechseln Sie zum iSCSI-Initiator, wählen Sie das StorSimple-Gerät, und klicken Sie auf **Verbinden**.

3.  Wenn die Synchronisierung abgeschlossen ist, und der Status der VM **geschützt** ist, wählen Sie die VM und die Registerkarte **Konfigurieren** aus, und aktualisieren Sie entsprechend das Netzwerk der VM (die VM(s), für die ein Failover ausgeführt wurde(n), werden Teil dieses Netzwerks). Falls das Netzwerk nicht angezeigt wird, bedeutet dies, dass die Synchronisierung immer noch ausgeführt wird.

### Aktivieren des Schutzes von StorSimple-Volumes

Wenn Sie die Option **Standardsicherung für dieses Volume aktivieren** für die StorSimple-Volumes nicht ausgewählt haben, gehen Sie im StorSimple Manager-Dienst unter **Sicherungsrichtlinien**, und erstellen Sie eine passende Sicherungsrichtlinie für alle Volumes. Es wird empfohlen die Häufigkeit der Sicherungen auf die Recovery Point Objective (RPO) festzulegen, die Sie für die Anwendung haben möchten.

### Konfigurieren des Netzwerks

Konfigurieren Sie Netzwerkeinstellungen für die Dateiserver-VM in Azure Site Recovery, sodass die VM-Netzwerke nach dem Failover an dem richtigen DR-Netzwerk angefügt sind.

Sie können die VM in der **VMM Cloud** oder **Schutzgruppe** auswählen, um die Netzwerkeinstellungen wie in der folgenden Abbildung gezeigt zu konfigurieren.

![](./media/storsimple-dr-using-asr/image2.png)

## Erstellen eines Wiederherstellungsplans

Sie können einen Wiederherstellungsplan in ASR erstellen, um den Failoverprozess der Dateifreigaben zu automatisieren. Wenn eine Störung auftritt, können Sie die Dateifreigaben in wenigen Minuten mit nur einem einzigen Klick anzeigen. Sie benötigen ein Azure Automation-Konto, um diese Automatisierung zu aktivieren.

#### So erstellen Sie das Konto

1.  Wechseln Sie zum Azure-Portal, und wechseln Sie zum Abschnitt **Automation**.

1.  Erstellen Sie ein neues Automation-Konto. Bewahren Sie es im gleichen geografischen Raum oder in der gleichen Region auf, in dem/der die StorSimple Cloud Appliance und die Speicherkonten erstellt wurden.

2.  Klicken Sie auf **Neu** &gt; **App Services** &gt; **Automation** &gt; **Runbook** &gt; **Aus Katalog**, um alle erforderlichen Runbooks in das Automation-Konto zu importieren.

	![](./media/storsimple-dr-using-asr/image3.png)

1.  Fügen Sie die folgenden Runbooks aus dem Bereich **Notfallwiederherstellung** im Katalog hinzu:

	-   Failover für StorSimple-Volumecontainer

	-   Bereinigen von StorSimple-Volumes nach dem Testfailover (TFO)

	-   Bereitstellen von Volumes auf einem StorSimple-Gerät nach einem Failover

	-   Starten von StorSimple Virtual Appliance

	-   Deinstallieren der benutzerdefinierten Skripterweiterung in Azure-VM

		![](./media/storsimple-dr-using-asr/image4.png)


1.  Veröffentlichen Sie alle Skripts, indem Sie das Runbook im Automation-Konto auswählen und zur Registerkarte **Autor** wechseln. Nach diesem Schritt wird die Registerkarte **Runbook** wie folgt angezeigt:

	 ![](./media/storsimple-dr-using-asr/image5.png)

1.  Wechseln Sie im Automation-Konto zur Registerkarte **Ressourcen**, klicken Sie auf **Einstellung hinzufügen** &gt; **Anmeldeinformationen hinzufügen**, fügen Sie Ihre Azure-Anmeldeinformationen hinzu und geben Sie der Ressource den Namen AzureCredential.

	Verwenden Sie die Windows PowerShell-Anmeldeinformationen. Das sollten Anmeldeinformationen sein, die einen Organisations-ID-Benutzernamen und ein Organisations-ID-Kennwort mit Zugriff auf dieses Azure-Abonnement enthalten und bei denen die Multi-Factor Authentication deaktiviert ist. Dies ist erforderlich, damit während des Failovers im Namen des Benutzers authentifiziert werden kann und um die Dateiservervolumes auf dem DR-Standort anzuzeigen.

1.  Wählen Sie im Automation-Konto die Registerkarte **Ressourcen** aus, und klicken Sie anschließend auf **Einstellung hinzufügen** &gt; **Variable hinzufügen** und fügen Sie die folgenden Variablen hinzu. Sie können diese Ressourcen verschlüsseln. Diese Variablen sind spezifisch für den Wiederherstellungsplan. Wenn der Name des Wiederherstellungsplans (den Sie im nächsten Schritt erstellen) TestPlan lautet, sollten Ihre Variablen TestPlan-StorSimRegKey, TestPlan-AzureSubscriptionName usw. heißen.

	-   *RecoveryPlanName***-StorSimRegKey**: der Registrierungsschlüssel für den StorSimple Manager-Dienst

	-   *RecoveryPlanName***-AzureSubscriptionName**: der Name des Azure-Abonnements

	-   *RecoveryPlanName***-ResourceName **: der Name der StorSimple-Ressource, die über das StorSimple-Gerät verfügt.

	-   *RecoveryPlanName***-DeviceName **: das Gerät, für das ein Failover ausgeführt werden muss.

	-   *RecoveryPlanName***-TargetDeviceName **: die StorSimple Cloud Appliance, auf die das Failover für Container ausgeführt werden soll.

	-   *RecoveryPlanName***-VolumeContainers **: eine durch Trennzeichen getrennte Zeichenfolge von Volumecontainer, die sich auf dem Gerät befindet, und für die ein Failover ausgeführt werden muss, z.B. „volcon1“, „volcon2“, „volcon3“.

	-   RecoveryPlanName**-TargetDeviceDnsName**: der Dienstname des Zielgeräts (dieser befindet sich im Abschnitt **Virtual Machine**: der Dienstname ist identisch mit dem DNS-Namen).

	-   *RecoveryPlanName***-StorageAccountName **: der Name des Speicherkontos, in dem das Skript (das auf der VM, für die ein Failover ausgeführt wurde, ausgeführt werden muss) gespeichert wird. Dies kann ein beliebiges Speicherkonto sein, das über genügend Speicherplatz verfügt, um das Skript zeitweise zu speichern.

	-   *RecoveryPlanName***-StorageAccountKey **: der Zugriffsschlüssel für das oben genannten Speicherkonto.

	-   *RecoveryPlanName***-ScriptContainer **: der Name des Containers, in dem das Skript in der Cloud gespeichert wird. Falls der Container noch nicht vorhanden ist, wird er erstellt.

	-   *RecoveryPlanName***-VMGUIDS **: beim Schützen einer VM weist Azure Site Recovery allen VMs eine eindeutige ID zu, die die Details zur VM enthält, für die ein Failover ausgeführt wurde. Wählen Sie zum Abrufen von VMGUID die Registerkarte **Recovery Services** aus, und klicken Sie anschließend auf **geschütztes Element** &gt; **Schutzgruppen** &gt; **Computer** &gt; **Eigenschaften**. Wenn Sie mehrere VMs haben, fügen Sie die GUIDs als durch Trennzeichen getrennte Zeichenfolge hinzu.

	-   *RecoveryPlanName*** - AutomationAccountName **: der Name des Automation-Kontos, in dem Sie die Runbooks und die Ressourcen hinzugefügt haben.

	Wenn der Name des Wiederherstellungsplans beispielsweise fileServerpredayRP lautet, sollte die Registerkarte **Ressourcen** wie folgt aussehen, nachdem Sie alle Ressourcen hinzugefügt haben.

	![](./media/storsimple-dr-using-asr/image6.png)


1.  Klicken Sie auf den Abschnitt **Recovery Services**, und wählen Sie den Azure Site Recovery-Tresor aus, den Sie zuvor erstellt haben.

2.  Wählen Sie die Registerkarte **Wiederherstellungspläne** aus, und erstellen Sie einen neuen Wiederherstellungsplan wie folgt:

	a. Geben Sie einen Namen an, und wählen Sie die entsprechende **Schutzgruppe** aus.

	b. Wählen Sie die VMs aus der Schutzgruppe aus, die im Wiederherstellungsplan enthalten sein sollen.

	c. Nachdem der Wiederherstellungsplan erstellt wurde, wählen Sie ihn aus, um die Anpassungsansicht des Wiederherstellungsplans zu öffnen.

	d. Wählen Sie **alle Gruppen heruntergefahren** aus, klicken Sie auf **Skript**, und wählen Sie **Add a primary side script before all Group shutdown** (Hinzufügen eines primärseitigen Skripts vor dem Herunterfahren aller Gruppen).

	e. Wählen Sie das Automation-Konto (in dem Sie die Runbooks hinzugefügt haben) aus, und wählen Sie dann das Runbook **Fail over-StorSimple-Volume-Containers** aus.

	f. Klicken Sie auf **Group 1: Start** (Gruppe 1: Start), wählen Sie **Virtual Machines** (Virtuelle Computer) aus, und fügen Sie die VMs hinzu, die im Wiederherstellungsplan geschützt werden sollen.

    g. Klicken Sie auf **Group 1: Start** (Gruppe 1: Start), wählen Sie **Script** (Skript), und fügen Sie alle folgenden Skripts nach **Group 1: Post-steps** (Gruppe 1: nachfolgende Schritte) hinzu.

	- Das Runbook „Start-StorSimple-Virtual-Appliance“
	- Das Runbook „Fail over-StorSimple-volume-containers“
	- Das Runbook „Mount-volumes-after-failover“
	- Das Runbook „Uninstall-custom-script-extension“

1.  Fügen Sie eine manuelle Aktion nach dem oben genannten 4 Skripts im gleichen Abschnitt **Group 1: Post-steps** (Gruppe 1: nachfolgende Schritte). Diese Aktion ist der Punkt, an dem Sie überprüfen können, dass alles ordnungsgemäß funktioniert. Diese Aktion muss nur als Teil des Testfailovers hinzugefügt werden (wählen Sie also nur das **Testfailover**-Kontrollkästchen aus).

2.  Fügen Sie nach der manuellen Aktion das Bereinigungsskript mithilfe der gleichen Prozedur hinzu, die Sie für die anderen Runbooks verwendet haben. Speichern des Wiederherstellungsplans

	> [AZURE.NOTE] Wenn Sie ein Testfailover ausführen, sollten Sie beim Schritt der manuellen Aktion alles überprüfen, da die StorSimple-Volumes, die auf dem Zielgerät geklont wurden, als Teil der Bereinigung nach dem Abschließen der manuellen Aktion gelöscht werden.

	![](./media/storsimple-dr-using-asr/image7.png)

## Ausführen eines Testfailovers

Finden Sie im Begleithandbuch [Active Directory-DR-Lösung](../site-recovery/site-recovery-active-directory.md) Überlegungen in Bezug auf Active Directory während des Testfailovers. Die lokale Einrichtung wird überhaupt nicht gestört, wenn das Testfailover auftritt. Die StorSimple-Volumes, die der lokalen VM angefügt wurden, werden auf die StorSimple Cloud Appliance auf Azure geklont. Eine VM für Testzwecke wird in Azure geladen und die geklonten Volumes der VM angefügt.

#### So führen Sie das Testfailover aus

1.  Wählen Sie im klassischen Azure-Portal Ihren Site Recovery-Tresor aus.

1.  Klicken Sie auf den Wiederherstellungsplan, der für die Dateiserver-VM erstellt wurde.

2.  Klicken Sie auf **Testfailover**.

3.  Wählen Sie das virtuelle Netzwerk aus, um den Testfailoverprozess zu starten.

	![](./media/storsimple-dr-using-asr/image8.png)

1.  Wenn die sekundären Umgebung aktiv ist, können Sie Ihre Überprüfungen durchführen.

2.  Nachdem die Überprüfungen abgeschlossen sind, klicken Sie auf **Überprüfungen abgeschlossen**. Die Testfailover-Umgebung wird bereinigt, und der TFO-Vorgang wird abgeschlossen.

## Ausführen eines ungeplanten Failovers

Während eines ungeplanten Failovers wird ein Failover für die StorSimple-Volumes auf das virtuelle Gerät ausgeführt. Eine Replikat-VM wird in Azure aktiviert, und die Volumes werden der VM angefügt.

#### So führen Sie ein ungeplantes Failover durch

1.  Wählen Sie im klassischen Azure-Portal Ihren Site Recovery-Tresor aus.

1.  Klicken Sie auf den Wiederherstellungsplan, der für die Dateiserver-VM erstellt wurde.

2.  Klicken Sie auf **Failover**, und wählen Sie anschließend **ungeplantes Failover**.

	![](./media/storsimple-dr-using-asr/image9.png)

1.  Wählen Sie das Zielnetzwerk aus, und klicken Sie anschließende auf das Häkchensymbol ✓, um den Failoverprozess zu starten.

## Ausführen eines geplanten Failovers

Bei einem geplanten Failover wird die lokale Dateiserver-VM ordnungsgemäß heruntergefahren, und eine Cloudsicherungs-Momentaufnahme der Volumes auf dem StorSimple-Gerät wird erzeugt. Failover für die StorSimple-Volumes werden auf das virtuelle Gerät ausgeführt. Eine Replikat-VM wird in Azure aktiviert, und die Volumes werden der VM angefügt.

#### So führen Sie ein geplantes Failover aus

1.  Wählen Sie im klassischen Azure-Portal Ihren Site Recovery-Tresor aus.

1.  Klicken Sie auf den Wiederherstellungsplan, der für die Dateiserver-VM erstellt wurde.

2.  Klicken Sie auf **Failover**, und wählen Sie anschließend **geplantes Failover** aus.

3.  Wählen Sie das Zielnetzwerk aus, und klicken Sie anschließende auf das Häkchensymbol ✓, um den Failoverprozess zu starten.

## Ein Failback durchführen

Während eines Failbacks wird für die StorSimple-Volumecontainer ein Failover auf die physischen Geräte durchgeführt, nachdem die Sicherung erstellt wurde.

#### So führen Sie ein Failback durch

1.  Wählen Sie im klassischen Azure-Portal Ihren Site Recovery-Tresor aus.

1.  Klicken Sie auf den Wiederherstellungsplan, der für die Dateiserver-VM erstellt wurde.

2.  Klicken Sie auf **Failover**, und wählen Sie **geplantes Failover** oder **ungeplantes Failover** aus.

3.  Klicken Sie auf **Richtung ändern**.

4.  Wählen Sie die entsprechenden Datensynchronisierung und die Optionen für die Erstellung von VM aus

5.  Klicken Sie auf das Häkchensymbol ✓, um den Failbackprozess zu starten.

	![](./media/storsimple-dr-using-asr/image10.png)

## Bewährte Methoden

### Kapazitätsplanung und Readiness Assessment


#### Hyper-V-Standort

Verwenden Sie das [Benutzertool Capacity Planner](http://www.microsoft.com/download/details.aspx?id=39057), um den Server, Arbeitsspeicher und die Netzwerkinfrastruktur für die Hyper-V-Replikatumgebung zu erstellen.

#### Azure

Sie können das Tool [Azure Virtual Machine Readiness Assessment](http://azure.microsoft.com/downloads/vm-readiness-assessment/) auf VMs ausführen, um sicherzustellen, dass diese mit Azure VMs und Azure Site Recovery Services kompatibel sind. Das Readiness Assessment-Tool überprüft die VM-Konfigurationen und gibt eine Warnung aus, wenn die Konfigurationen mit Azure inkompatibel sind. Es gibt z.B. eine Warnung aus, wenn ein Laufwerk C: größer als 127 GB ist.


Die Kapazitätsplanung besteht aus mindestens zwei wichtigen Prozessen:

-   Zuordnung von lokalen Hyper-V-VMs zu Azure-VM-Größen (z.B. A6, A7, A8 und A9)

-   Bestimmung der benötigten Internet-Bandbreite

## Einschränkungen

- Derzeit kann nur für ein StorSimple-Gerät ein Failover ausgeführt werden (auf eine einzelne StorSimple Cloud Appliance). Das Szenario eines Dateiservers, der mehrere StorSimple-Geräte umfasst, wird noch nicht unterstützt.

- Wenn Sie eine Fehlermeldung erhalten, während Sie den Schutz für eine VM aktivieren, stellen Sie sicher, dass Sie die Verbindung mit den iSCSI-Zielen getrennt haben.

- Für alle Volumecontainer, die aufgrund von Volumecontainer übergreifenden Sicherheitsregeln gruppiert wurden, wird ein Failover ausgeführt.

- Für alle Volumes in Volumecontainern, die Sie ausgewählt haben, wird ein Failover ausgeführt.

- Für Volumes, die zusammen mehr als 64 TB ergeben, kann kein Failover ausgeführt werden, da die maximale Kapazität einer einzelnen StorSimple Cloud Appliance 64 TB beträgt.

- Bereinigen Sie nicht die VMs, wenn das geplante/ungeplante Failover fehlschlägt, und die VMs in Azure erstellt werden. Führen Sie stattdessen ein Failback aus. Wenn Sie die virtuellen Computer löschen, können die lokalen VMs nicht wieder aktiviert werden.

- Wenn Sie nach einem Failover die Volumes nicht sehen können, wechseln Sie zu den VMs, öffnen Sie die Datenträgerverwaltung, lesen Sie die Datenträger erneut ein, und stellen Sie sie online.

- In einigen Fällen können die Laufwerkbuchstaben am DR-Standort von den lokalen abweichen. In diesem Fall müssen Sie das Problem manuell beheben, nachdem das Failover abgeschlossen ist.

- Die mehrstufige Authentifizierung sollte für die Azure-Anmeldeinformationen deaktiviert sein, die im Automation-Konto als Ressource eingegeben werden. Wenn diese Authentifizierung nicht deaktiviert ist, können Skripts nicht automatisch ausgeführt werden, und beim Wiederherstellungsplan tritt ein Fehler auf.

- Failover-Auftragstimeout: für das StorSimple-Skript tritt ein Timeout auf, wenn das Failover von Volumecontainern mehr Zeit benötigt, als das Azure Site Recovery-Limit pro Skript (derzeit 120 Minuten).

- Sicherungsauftragstimeout: für das StorSimple-Skript tritt ein Timeout auf, wenn die Sicherung von Volumecontainern mehr Zeit benötigt, als das Azure Site Recovery-Limit pro Skript (derzeit 120 Minuten).
 
	> [AZURE.IMPORTANT] Führen Sie die Sicherung manuell vom Azure-Portal aus, und führen Sie anschließend den Wiederherstellungsplan erneut aus.

- Klonauftragstimeout: für das StorSimple-Skript tritt ein Timout auf, wenn das Klonen von Volumes mehr Zeit benötigt, als das Azure Site Recovery-Limit pro Skript (derzeit 120 Minuten).

- Fehler bei der Zeitsynchronisierung: für die StorSimple Skripts tritt ein Fehler auf, der besagt, dass die Sicherungen nicht erfolgreich ausgeführt wurden, auch wenn die Sicherung im Portal erfolgreich war. Eine mögliche Ursache dafür ist, dass die Zeit der StorSimple Appliance nicht mehr mit der aktuellen Zeit in der Zeitzone synchronisiert ist.
 
	> [AZURE.IMPORTANT] Synchronisieren Sie die Zeit der Appliance mit der aktuellen Zeit in der Zeitzone.

- Appliance Failover-Fehler: das StorSimple-Skript kann fehlschlagen, wenn ein Failover auf eine Appliance besteht, wenn der Wiederherstellungplan ausgeführt wird.
	
	> [AZURE.IMPORTANT] Wiederholen Sie den Wiederherstellungsplan nach Abschluss des Appliancefailovers.

## Zusammenfassung

Sie können mithilfe von Azure Site Recovery einen vollständig automatisierten Plan für die Notfallwiederherstellung für eine Dateiserver-VM erstellen, die über Dateifreigaben verfügt, die auf StorSimple gehostet werden. Bei einer Störung können Sie das Failover von überall aus in Sekundenschnelle einleiten und die Anwendung binnen weniger Minuten wieder in Betrieb nehmen.

<!---HONumber=AcomDC_0518_2016-->