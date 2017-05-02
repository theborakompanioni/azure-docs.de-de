---
title: "Schützen eines VMware-Servers mit Azure Backup Server | Microsoft-Dokumentation"
description: "Sichern Sie mit Azure Backup Server einen VMware-Server in Azure oder auf einem Datenträger. Verwenden Sie die Informationen in diesem Artikel, um Ihre VMware-Workload zu schützen."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
ms.assetid: 6b131caf-de85-4eba-b8e6-d8a04545cd9d
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/28/2017
ms.author: markgal;
translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: 779841483e72b42725354d85a0f593aee7db8278
ms.lasthandoff: 04/11/2017


---
# <a name="back-up-vmware-server-to-azure"></a>Sichern eines VMware-Servers in Azure

Dieser Artikel beschreibt, wie Sie einen VMware-Server mit Azure Backup Server verbinden, damit Sie die Inhalte des VMware-Servers in der Cloud sichern können. In diesem Artikel wird davon ausgegangen, dass Sie Azure Backup Server bereits installiert haben.

## <a name="create-secure-connection-to-vmware-server"></a>Herstellen einer sicheren Verbindung mit dem VMware-Server

Um einen VMware-Server zu schützen, muss Azure Backup Server eine sichere Verbindung mit dem VMware-Server herstellen können. Installieren Sie zum Herstellen einer sicheren Verbindung ein gültiges Zertifikat auf dem VMware-Server und in Azure Backup Server.

Wenn Sie eine Verbindung mit dem VMware-Server herstellen und die URL nicht sicher ist, müssen Sie das Zertifikat exportieren, damit die Verbindung mit der Website sicher ist.
![Beispiel für eine nicht gesicherte Verbindung mit dem VMware-Server](./media/backup-azure-backup-server-vmware/unsecure-url.png)

1. Klicken Sie auf „https“ (durchgestrichen), und klicken Sie dann im Popupmenü auf den Link „Details“.

  Abhängig von Ihrem Browser müssen Sie möglicherweise auf **Einstellungen** > **More Tools** (Mehr Tools)  > **Entwicklertools** klicken und die Registerkarte „Sicherheit“ wählen.

  ![Beispiel für die Fehlermeldung bei einer nicht gesicherten Verbindung](./media/backup-azure-backup-server-vmware/security-tab.png)

2. Klicken Sie auf der Registerkarte „Sicherheit“ in den Detailinformationen auf **Zertifikat anzeigen**.

  ![Beispiel für die Fehlermeldung bei einer nicht gesicherten Verbindung](./media/backup-azure-backup-server-vmware/security-tab-view-certificate.png)

  Das Dialogfeld „Zertifikat“ wird geöffnet.

3. Klicken Sie im Dialogfeld „Zertifikat“ auf die Registerkarte „Zertifizierungspfad“.  

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/certificate-certification-path.png)

  Das hervorgehobene Zertifikat ist nicht vertrauenswürdig, da in diesem Fall der Aussteller nicht gefunden wurde. Es gibt auch noch andere Gründe, warum das Zertifikat nicht vertrauenswürdig ist.

4. Klicken Sie zum Exportieren des Zertifikats auf den lokalen Computer auf die Registerkarte „Details“, und klicken Sie dann auf „In Datei kopieren“.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/certificate-details-tab.png)

  Der Zertifikatexport-Assistent wird geöffnet.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/certificate-wizard1.png)

  Klicken Sie auf **Weiter**, um die einzelnen Schritte des Assistenten auszuführen.

5. Geben Sie auf dem Bildschirm „Format der zu exportierenden Datei“ das für das Zertifikat bevorzugte Format an. Falls Sie kein Format bevorzugen, übernehmen Sie das Standarddateiformat für das Zertifikat, und klicken Sie auf **Weiter**.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/certificate-wizard1b.png)

6. Geben Sie auf dem Bildschirm „Zu exportierende Datei“ einen Namen für das Zertifikat an, und klicken Sie auf „Durchsuchen“, um den Speicherort für das Zertifikat auf dem lokalen Computer auszuwählen. Speichern Sie das Zertifikat an einem geeigneten Ort.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/certificate-wizard2.png)

7. Navigieren Sie nach dem Exportieren des Zertifikats zu seinem Speicherort, klicken Sie mit der rechten Maustaste auf das Zertifikat, und wählen Sie im Menü **Zertifikat installieren** aus.

  Der Zertifikatimport-Assistent wird geöffnet.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/cert-import-wizard1.png)

8. Wählen Sie im Zertifikatimport-Assistenten als Ziel für das Zertifikat **Lokaler Computer** aus, und klicken Sie auf **Weiter**, um den Vorgang fortzusetzen.

9. Wählen Sie auf dem Bildschirm „Zertifikatspeicher“ die Option **Alle Zertifikate in folgendem Speicher speichern** aus, und klicken Sie auf **Durchsuchen**.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

  Das Dialogfeld „Zertifikatspeicher auswählen“ wird geöffnet.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/cert-store.png)

  Wählen Sie den Zielordner für die Zertifikate aus, und klicken Sie auf **OK**. Wenn Sie nicht wissen, welcher Ordner verwendet werden soll, wählen Sie „Vertrauenswürdige Stammzertifizierungsstellen“. Der ausgewählte Zielordner wird im Dialogfeld „Zertifikatspeicher“ angezeigt. Klicken Sie zum Importieren des Zertifikats auf **Weiter**.

10. Überprüfen Sie auf dem Bildschirm zum Fertigstellen des Zertifikatimport-Assistenten, ob sich das Zertifikat im gewünschten Ordner befindet, und klicken Sie auf „Fertig stellen“, um den Assistenten abzuschließen.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

  In einem Dialogfeld wird angezeigt, ob der Import erfolgreich war.

11. Melden Sie sich beim virtuellen VMware-Computer an, um zu überprüfen, ob eine sichere Verbindung mit dem VMware-Server besteht. Azure Backup Server stellt über einen sicheren HTTPS-Kanal eine Verbindung mit dem VMware-Server her. Wenn in Ihrer Organisation sichere Grenzen eingerichtet wurden und das HTTPS-Protokoll nicht aktiviert werden soll, deaktivieren Sie die sichere Kommunikation über die Registrierung. Es wird jedoch empfohlen, Zertifikate auf Azure Backup Server und dem VMware-Server zu installieren, um die sichere Kommunikation zu ermöglichen.


## <a name="create-role-and-user-account-on-vmware-server"></a>Erstellen von Rollen und Benutzerkonten auf einem VMware-Server

Azure Backup Server kommuniziert mit einem VMware-Remoteserver durch die Authentifizierung der Anmeldedaten eines bestimmten VMware-Benutzers. Azure Backup Server authentifiziert die Anmeldedaten des VMware-Benutzers für alle Sicherungsvorgänge. Verwenden Sie den Assistenten zum Hinzufügen von Produktionsservern von Azure Backup Server, um Azure Backup Server die sichere Kommunikation mit dem VMware-Server zu ermöglichen. Die Einrichtung der Beziehung zwischen Azure Backup Server und dem VMware-Server besteht aus drei Phasen: 

- Erstellen einer Benutzerrolle mit zugewiesenen Berechtigungen
- Erstellen eines Benutzerkontos mit Anmeldeinformationen – Benutzername und Kennwort
- Hinzufügen des VMware-Serverbenutzerkontos zu Azure Backup Server

, wenn Sie die Schritte im Assistenten zum Hinzufügen von Produktionsservern ausführen. Das Benutzername- und Kennwortpaar wird als Anmeldeinformationen gespeichert.


### <a name="create-user-role-and-add-privileges"></a>Erstellen von Benutzerrollen und Berechtigungen
Das VMware-Benutzerkonto, das in den Azure Backup Server-Anmeldeinformationen angegeben wird, muss über bestimmte zugeordnete Berechtigungen verfügen. Berechtigungen werden jedoch einer Benutzerrolle zugewiesen, daher erstellen wir zunächst eine Benutzerrolle und fügen anschließend dieser Rolle bestimmte Berechtigungen hinzu. Die der Benutzerrolle zugeordneten Berechtigungen gelten für einen Sicherungsadministrator.

1. Melden Sie sich zum Erstellen einer neuen VMware-Benutzerrolle bei Ihrem VMware-Server an, und klicken Sie im Bereich **Navigator** auf **Verwaltung**.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. Wählen Sie zum Erstellen einer neuen Rolle im Abschnitt **Verwaltung** die Option **Rollen**, und klicken Sie im Bereich **Rollen** auf das Symbol zum Hinzufügen von Rollen (das Symbol +).

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

  Das Dialogfeld **Rolle erstellen** wird geöffnet.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. Geben Sie im Dialogfeld **Rolle erstellen** im Feld **Rollenname** einen Namen für die Rolle ein. In diesem Beispiel verwenden wir den Namen *BackupAdminRole*. Sie können als Rollennamen einen beliebigen Namen auswählen, es empfiehlt sich jedoch, einen aussagekräftigen Namen zu verwenden.

4. Wählen Sie die Berechtigungen aus, um sie auf die Benutzerrolle anzuwenden. Wählen Sie die Berechtigungen in der folgenden Liste aus. Klicken Sie beim Auswählen der Berechtigungen auf das Chevron für die übergeordnete Bezeichnung, um die übergeordnete Berechtigung zu erweitern und die untergeordneten Berechtigungen anzuzeigen. Sie müssen nicht alle untergeordneten Berechtigungen einer übergeordneten Berechtigung auswählen.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  - Privilege.Datastore.AllocateSpace.label
  - Privilege.Global.ManageCustomerFields.label
  - privilege.Network.Assign.label
  - Privilege.VirtualMachine.Config.AddNewDisk.label
  - Privilege.VirtualMachine.Config.AdvanceConfig.label
  - Privilege.VirtualMachine.Config.ChangeTracking.label
  - Privilege.VirtualMachine.Config.HostUSBDevice.label
  - Privilege.VirtualMachine.Config.SwapPlacement.label  
  - Privilege.VirtualMachine.Interact.PowerOff.label
  - Privilege.VirtualMachine.Inventory.Create.label
  - Privilege.VirtualMachine.Provisioning.DiskRandomRead.summary
  - Privilege.VirtualMachine.State.CreateSnapshot.label
  - Privilege.VirtualMachine.State.RemoveSnapshot.label
</br>

  Klicken Sie nach dem Auswählen der Berechtigungen auf **OK**. Die neue Rolle wird in der Liste im Bereich „Rollen“ angezeigt.

### <a name="create-a-user-account-and-assign-permissions"></a>Erstellen eines Benutzerkontos und Zuweisen von Berechtigungen

Nachdem Sie die Benutzerrolle mit Berechtigungen festgelegt haben, erstellen Sie ein Benutzerkonto. Beim Erstellen des Benutzerkontos weisen Sie es einer bestimmten Benutzerrolle zu, wodurch das Konto die zugeordneten Berechtigungen erhält. Das Benutzerkonto verfügt über einen Namen und ein Kennwort, die die für die Authentifizierung verwendeten Anmeldeinformationen darstellen.

1. Klicken Sie zum Erstellen eines neuen Benutzerkontos auf dem VMware-Server im Bereich „Navigator“ auf **Benutzer und Gruppen**.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

  Der Bereich „Benutzer und Gruppen“ wird angezeigt.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. Klicken Sie im VMware-Bereich „Benutzer und Gruppen“ auf der Registerkarte „Benutzer“ auf das Symbol zum Hinzufügen von Benutzern (das Symbol +).

  Das Dialogfeld „Neuer Benutzer“ wird geöffnet.

3. Das von Ihnen erstellte Benutzerkonto enthält das Benutzername- und Kennwortpaar, das als Anmeldeinformationen verwendet wird. Füllen Sie im Dialogfeld „Neuer Benutzer“ die Felder aus, und klicken Sie auf **OK**.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

  Das neue Benutzerkonto wird in der Liste angezeigt.

4. Sie haben das Benutzerkonto erstellt. Ordnen Sie es nun der Benutzerrolle (mit den gewünschten Berechtigungen) zu. Klicken Sie im Bereich „Navigator“ auf **Globale Berechtigungen**. Klicken Sie im Bereich „Globale Berechtigungen“ auf die Registerkarte **Verwalten** und anschließend auf das Symbol zum Hinzufügen (das Symbol +).

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

  Das Dialogfeld „Global Permissions Root - Add Permission“ (Stamm: Globale Berechtigungen – Berechtigung hinzufügen) wird geöffnet.

5. Klicken Sie im Dialogfeld **Global Permission Root - Add Permission** (Stamm: Globale Berechtigungen – Berechtigung hinzufügen) auf **Hinzufügen**, um den Benutzer oder die Gruppe auszuwählen.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

  Das Dialogfeld „Select Users/Groups“ (Benutzer/Gruppen auswählen) wird geöffnet.

6. Wählen Sie im Dialogfeld **Select Users/Groups** (Benutzer/Gruppen auswählen) das von Ihnen erstellte Benutzerkonto aus, und klicken Sie auf **Hinzufügen**. Das ausgewählte Benutzerkonto wird im Feld „Benutzer“ angezeigt. Der Benutzername wird im Feld „Benutzer“ im Format *Domäne*`\`*Benutzername* angezeigt.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

  Klicken Sie auf **OK**, um die ausgewählten Benutzer dem Dialogfeld „Berechtigung hinzufügen“ hinzuzufügen.

7. Sie haben den Benutzer bestimmt. Weisen Sie ihn nun der Rolle hinzu. Wählen Sie im Bereich „Assigned Role“ (Zugewiesene Rolle) im Dropdownmenü die Rolle aus, und klicken Sie auf **OK**.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  In den globalen Berechtigungen werden auf der Registerkarte „Verwalten“ das neue Benutzerkonto und die zugeordnete Rolle in der Liste angezeigt.


### <a name="add-the-vmware-user-account-credentials-to-azure-backup-server"></a>Hinzufügen der Anmeldeinformationen für das VMware-Benutzerkonto zu Azure Backup Server

Stellen Sie vor dem Hinzufügen des VMware-Servers zu Azure Backup Server sicher, dass Sie [Update 1 für Microsoft Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server) installiert haben.

1. Klicken Sie auf das folgende Symbol (auf dem Serverdesktop), um die Azure Backup Server-Konsole zu öffnen:

  ![Azure Backup Server-Symbol](./media/backup-azure-backup-server-vmware/mabs-icon.png)

  Wenn Sie das Symbol auf dem Desktop nicht finden, können Sie Azure Backup Server über die Liste der installierten Apps öffnen. In der Liste der installierten Apps heißt die Azure Backup Server-App „Microsoft Azure Backup“.

2. Klicken Sie in der Azure Backup Server-Konsole auf **Verwaltung**, dann auf **Produktionsserver** und anschließend auf dem Menüband auf **VMware verwalten**.

  ![MABS-Konsole](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

  Das Dialogfeld „Anmeldeinformationen verwalten“ wird geöffnet.

  ![MABS-Dialogfeld „Anmeldeinformationen verwalten“](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. Klicken Sie im Dialogfeld „Anmeldeinformationen verwalten“ auf **Hinzufügen**, um das Dialogfeld „Anmeldeinformationen hinzufügen“ zu öffnen.

4. Geben Sie im Dialogfeld „Anmeldeinformationen hinzufügen“ einen Namen und eine Beschreibung für die neuen Anmeldeinformationen ein. Benutzername und Kennwort sollten mit den Informationen übereinstimmen, die Sie beim Erstellen des Benutzerkontos auf dem VMware-Server verwendet haben.

  ![MABS-Dialogfeld „Anmeldeinformationen verwalten“](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog.png)

  Klicken Sie auf **Hinzufügen**, um die neuen Anmeldeinformationen zu Azure Backup Server hinzuzufügen. Die neuen Anmeldeinformationen werden in der Liste des Dialogfelds „Anmeldeinformationen verwalten“ angezeigt.
  ![MABS-Dialogfeld „Anmeldeinformationen verwalten“](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. Klicken Sie in der Ecke oben rechts auf **X**, um das Dialogfeld „Anmeldeinformationen verwalten“ zu schließen.


## <a name="add-vmware-server-to-azure-backup-server"></a>Hinzufügen eines VMware-Servers zu Azure Backup Server

So öffnen Sie den Assistenten zum Hinzufügen von Produktionsservern

1. Klicken Sie in der Azure Backup Server-Konsole auf **Verwaltung**, dann auf **Produktionsserver** und anschließend auf **Hinzufügen**.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

  Der Assistent zum Hinzufügen von Produktionsservern wird geöffnet.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. Wählen Sie auf dem Bildschirm „Produktionsservertyp auswählen“ die Option „VMware-Server“ aus, und klicken Sie auf **Weiter**.

3. Geben Sie unter „Servername/IP-Adresse“ den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) oder die IP-Adresse des VMware-Servers ein. Sie können den VMware-Namen eingeben, wenn alle ESXi-Server von der gleichen vCenter-Instanz verwaltet werden.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. Geben Sie im Dialogfeld **SSL-Port** den Port ein, der für die Kommunikation mit dem VMware-Server verwendet wird. Verwenden Sie den Standardport 443, es sei denn, Sie wissen, dass ein anderer Port erforderlich ist.

5. Im Dialogfeld **Anmeldeinformationen angeben** können Sie neue Anmeldeinformationen erstellen oder vorhandene Anmeldeinformationen auswählen. Im vorherigen Abschnitt haben Sie Anmeldeinformationen erstellt. Wählen Sie diese Anmeldeinformationen aus.

  Falls keine Anmeldeinformationen verfügbar sind oder Sie neue Anmeldeinformationen erstellen müssen, klicken Sie auf **Neue Anmeldeinformationen hinzufügen**, erstellen Sie die neuen Anmeldeinformationen, und klicken Sie auf **OK**.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/specify-new-cred.png)

6. Klicken Sie auf **Hinzufügen**, um den VMware-Server der Liste **Hinzugefügte VMware-Server** hinzuzufügen, und klicken Sie auf **Weiter**, um zum nächsten Bildschirm im Assistenten zu wechseln.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. Klicken Sie auf dem Bildschirm **Tasks** auf **Hinzufügen**, um den angegebenen VMware-Server zu Azure Backup Server hinzuzufügen.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  Da die VMware-Serversicherung ohne Agent erfolgt, wird der neue Server in wenigen Sekunden hinzugefügt. Wiederholen Sie die vorhergehenden Schritte in diesem Abschnitt, um mehrere VMware-Server zu Azure Backup Server hinzuzufügen.

Sie haben nun einen VMware-Server zu Azure Backup Server hinzugefügt. Im nächsten Schritt wird eine Schutzgruppe erstellt. Die Schutzgruppe enthält die Details für die kurz- und die langfristige Aufbewahrung. Darüber hinaus legen Sie in der Schutzgruppe die Sicherungsrichtlinie fest und wenden sie an. Die Sicherungsrichtlinie steuert, wann Sicherungen erstellt werden und welche Elemente gesichert werden.


## <a name="configure-a-protection-group-to-back-up-vmware-server"></a>Konfigurieren einer Schutzgruppe zum Sichern des VMware-Servers

Falls Sie System Center Data Protection Manager oder Azure Backup Server bisher noch nicht verwendet haben, lesen Sie das Thema [Planen von Datenträgersicherungen](https://technet.microsoft.com/library/hh758026.aspx), um Ihre Hardwareumgebung vorzubereiten. Wenn Sie sichergestellt haben, dass Sie über ausreichend Speicher verfügen, fügen Sie die spezifischen virtuellen Computer mithilfe des Assistenten zum Erstellen einer neuen Schutzgruppe hinzu.

1. Klicken Sie in der Azure Backup Server-Verwaltungskonsole auf **Schutz** und dann im Menüband des Tools auf **Neu**, um den Assistenten zum Erstellen einer neuen Schutzgruppe zu öffnen.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

  Der Assistent zum Erstellen einer neuen Schutzgruppe wird geöffnet.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/protection-wizard.png)

  Klicken Sie auf **Weiter**, um mit dem Bildschirm **Schutzgruppentyp auswählen** fortzufahren.

2. Wählen Sie auf dem Bildschirm „Schutzgruppentyp auswählen“ die Option **Server** aus, und klicken Sie auf **Weiter**.

3. Auf dem Bildschirm „Gruppenmitglieder auswählen“ werden die verfügbaren Mitglieder und die ausgewählten Mitglieder angezeigt. Wählen Sie die zu schützenden Mitglieder aus, und klicken Sie auf **Weiter**.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/server-add-to-selected-members.png)

  Beachten Sie beim Auswählen eines Mitglieds Folgendes: Wenn Sie einen Ordner auswählen, der weitere Ordner oder virtuelle Computer enthält, werden diese Ordner oder virtuellen Computer ebenfalls ausgewählt. Die Aufnahme der Ordner und virtuellen Computer im übergeordneten Ordner wird als „Schutz auf Ordnerebene“ bezeichnet. Sie können Ordner oder virtuelle Computer ausschließen, indem Sie das Kontrollkästchen deaktivieren.

  Wenn ein virtueller Computer oder ein Ordner mit einem virtuellen Computer bereits in Azure gesichert wird, können Sie diesen virtuellen Computer nicht erneut auswählen. Das bedeutet, dass ein virtueller Computer nicht erneut gesichert werden kann, wenn er in Azure gesichert wird. Dadurch wird die Erstellung doppelter Wiederherstellungspunkte für einen virtuellen Computer verhindert. Um anzuzeigen, welche Azure Backup Server-Instanz bereits ein Mitglied sichert, zeigen Sie mit der Maus auf das Mitglied, um den Namen des Schutzservers anzuzeigen.

4. Geben Sie auf dem Bildschirm „Methode für die Datensicherheit auswählen“ einen Namen für die Schutzgruppe ein. Wählen Sie dann unter **Schutzmethode** aus, wo Ihre Daten gesichert werden sollen. Daten werden zum kurzfristigen Schutz auf einem Datenträger gesichert. Zum langfristigen Schutz werden Daten in der Cloud (Azure) gesichert. Klicken Sie auf **Weiter**, um zur Dauer für den kurzfristigen Schutz zu wechseln.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. Geben Sie auf dem Bildschirm „Kurzfristige Ziele angeben“ für **Beibehaltungsdauer** an, für wie viele Tage die *auf Datenträgern gespeicherten* Wiederherstellungspunkte beibehalten werden sollen. Klicken Sie zum Ändern der Uhrzeit und des Datums für die Erstellung von Wiederherstellungspunkten auf **Ändern**. Bei kurzfristigen Wiederherstellungspunkten handelt es sich um vollständige Sicherungen. Es sind keine inkrementellen Sicherungen. Wenn Sie mit den kurzfristigen Zielen zufrieden sind, klicken Sie auf **Weiter**.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. Überprüfen Sie auf dem Bildschirm „Datenträgerzuordnung überprüfen“ den Speicherplatz für die virtuellen Computer, und passen Sie ihn ggf. an. Die empfohlenen Datenträgerzuordnungen basieren auf der im vorherigen Bildschirm angegebenen Beibehaltungsdauer, auf dem Typ der Workload und dem Umfang der gesicherten Daten (in Schritt 3 ermittelt).  

  - Datengröße: Größe der Daten in der Schutzgruppe
  - Speicherplatz: Der für die Schutzgruppe empfohlene Speicherplatz. Wenn Sie diese Einstellung ändern möchten, muss der zugewiesene Gesamtspeicherplatz geringfügig größer sein als das voraussichtliche Wachstum der einzelnen Datenquellen.
  - Colocate data (Daten zusammenstellen): Wenn Sie das Zusammenstellen aktivieren, können mehrere Datenquellen im Schutz einem einzelnen Volume für Replikate und Wiederherstellungspunkte zugeordnet werden. Die Zusammenstellung wird nicht für alle Workloads unterstützt.
  - Automatisch anwachsen: Wenn Sie diese Einstellung aktivieren, versucht DPM die Datenträgergröße um 25 Prozent zu erhöhen, wenn Daten in der Schutzgruppe die ursprüngliche Zuordnung überschreiten.
  - Speicherpooldetails: Zeigt den aktuellen Status des Speicherpools an, einschließlich Gesamtgröße und verbleibende Größe des Datenträgers.

    ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

  Wenn Sie fertig sind, klicken Sie auf **Weiter**.

7. Geben Sie auf dem Bildschirm „ Replikaterstellungsmethode auswählen“ an, wie die erste Kopie (Replikat) der geschützten Daten in Azure Backup Server erstellt werden soll.

  Die Standardeinstellungen lauten **Automatisch über das Netzwerk** und **Now** (Jetzt). Bei Verwendung der Standardeinstellungen wird die Angabe einer Nebenzeit empfohlen. Wählen Sie **Später**, und geben Sie Datum und Uhrzeit an.

  Ziehen Sie bei großen Datenmengen oder nicht optimalen Netzwerkbedingungen die Offlinereplikation der Daten mit Wechselmedien in Betracht.

  Nachdem Sie Ihre Auswahl vorgenommen haben, klicken Sie auf **Weiter**.

  ![Assistent zum Erstellen einer neuen Schutzgruppe](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. Legen Sie auf dem Bildschirm **Konsistenzprüfungsoptionen** fest, wie und wann Konsistenzprüfungen automatisiert werden sollen. Sie können Konsistenzprüfungen bei inkonsistenten Replikatdaten oder gemäß einem festgelegten Zeitplan ausführen.

  Wenn Sie keine automatischen Konsistenzprüfungen konfigurieren möchten, können Sie jederzeit eine manuelle Überprüfung ausführen, indem Sie im Bereich „Schutz“ der Azure Backup Server-Konsole mit der rechten Maustaste auf die Schutzgruppe klicken und „ Konsistenzprüfung ausführen“ auswählen.

  Klicken Sie auf **Weiter**, um zum nächsten Bildschirm zu wechseln.

9. Wählen Sie auf dem Bildschirm **Online zu schützende Daten angeben** die zu schützenden Datenquellen aus. Sie können die Mitglieder einzeln auswählen oder auf **Select All** (Alles markieren) klicken, um alle Mitglieder auszuwählen. Klicken Sie anschließend auf *Weiter*.

  ![Assistent zum Erstellen einer neuen Schutzgruppe](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. Geben Sie auf dem Bildschirm **Onlinesicherungszeitplan angeben** den Zeitplan zum Erstellen von Wiederherstellungsendpunkten aus der Datenträgersicherung an. Nach der Erstellung des Wiederherstellungspunkts wird er in den Recovery Services-Tresor in Azure übertragen. Wenn Sie mit dem Onlinesicherungszeitplan zufrieden sind, klicken Sie auf **Weiter**.

  ![Assistent zum Erstellen einer neuen Schutzgruppe](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. Geben Sie auf dem Bildschirm „Onlineaufbewahrungsrichtlinie angeben“ an, wie lange die Sicherungsdaten in Azure aufbewahrt werden sollen. Klicken Sie nach dem Festlegen der Richtlinie auf **Weiter**.

  ![Assistent zum Erstellen einer neuen Schutzgruppe](./media/backup-azure-backup-server-vmware/retention-policy.png)

  Für die Datenaufbewahrung in Azure gibt es kein zeitliches Limit. Beim Speichern von Wiederherstellungspunktdaten in Azure gilt nur folgende Einschränkung: Pro geschützter Instanz können maximal 9.999 Wiederherstellungspunkte erstellt werden. In diesem Beispiel ist die geschützte Instanz der VMware-Server.

12. Überprüfen Sie auf dem Bildschirm mit der Zusammenfassung die Details für Ihre Schutzgruppe. Achten Sie auf die Gruppenmitglieder und die Einstellungen. Wenn Sie mit den Einstellungen zufrieden sind, klicken Sie auf **Gruppe erstellen**.

  ![Assistent zum Erstellen einer neuen Schutzgruppe](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie Azure Backup Server für den Schutz von VMware-Workloads verwenden, interessiert Sie vielleicht auch die Verwendung von Azure Backup Server für den Schutz von [Microsoft Exchange Server](./backup-azure-exchange-mabs.md), [SQL Server](./backup-azure-sql-mabs.md) oder einer [Microsoft SharePoint-Farm](./backup-azure-backup-sharepoint-mabs.md).

Informationen zu Problemen beim Registrieren des Agents oder beim Konfigurieren der Schutzgruppe sowie zu Problemen bei Sicherungsaufträgen finden Sie unter [Behandeln von Problemen mit Azure Backup Server](./backup-azure-mabs-troubleshoot.md).

