---
title: "Schützen der Workload eines VMware-Servers mit Azure Backup Server | Microsoft-Dokumentation"
description: "Sichern Sie mit Azure Backup Server einen VMware-Server in Azure oder auf einem Datenträger. In diesem Artikel erfahren Sie, wie Sie Ihre VMware-Workload schützen."
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
ms.date: 04/20/2017
ms.author: markgal;
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: 47728711930703121626c3ed0b654a0f74603ca4
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017


---
# <a name="back-up-a-vmware-server-to-azure"></a>Sichern eines VMware-Servers in Azure

Dieser Artikel erläutert, wie Sie Azure Backup Server zum Schutz von VMware-Server-Workloads konfigurieren. In diesem Artikel wird davon ausgegangen, dass Sie Azure Backup Server bereits installiert haben. Falls Sie Azure Backup Server nicht installiert haben, lesen Sie [Vorbereiten der Sicherung von Workloads per Azure Backup Server](backup-azure-microsoft-azure-backup.md).

Azure Backup Server kann die VMware vCenter Server-Versionen 6.0 und 5.5 sichern bzw. schützen.


## <a name="create-a-secure-connection-to-the-vcenter-server"></a>Erstellen einer sichere Verbindung mit dem vCenter-Server

Azure Backup Server kommuniziert mit jedem vCenter-Server standardmäßig über einen HTTPS-Kanal. Zum Aktivieren der sicheren Kommunikation empfehlen wir, das Zertifikat der VMware-Zertifizierungsstelle (Certificate Authority, CA) in Azure Backup Server zu installieren. Falls Sie keine sichere Kommunikation benötigen und die HTTPS-Anforderung deaktivieren möchten, lesen Sie [Deaktivieren des sicheren Kommunikationsprotokolls](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol). Importieren Sie zum Erstellen einer sicheren Verbindung zwischen Azure Backup Server und dem vCenter-Server das vertrauenswürdige Zertifikat in Azure Backup Server.

Üblicherweise verwenden Sie einen Browser auf dem Azure Backup Server-Computer, um über den vSphere-Webclient eine Verbindung mit dem vCenter-Server herzustellen. Wenn Sie über den Browser von Azure Backup Server zum ersten Mal eine Verbindung mit dem vCenter-Server herstellen, ist die Verbindung nicht sicher. Die folgende Abbildung zeigt die unsichere Verbindung.

![Beispiel einer unsicheren Verbindung mit dem VMware-Server](./media/backup-azure-backup-server-vmware/unsecure-url.png)

Um dieses Problem zu beheben und eine sichere Verbindung zu erstellen, laden Sie die vertrauenswürdigen Zertifikate der Stammzertifizierungsstelle herunter.

1. Geben Sie im Browser von Azure Backup Server die URL des vSphere-Webclients ein. Die Anmeldeseite des vSphere-Webclients erscheint.

    ![vSphere-Webclient](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

    Im unteren Bereich der Informationen für Administratoren und Entwickler befindet sich der Link zum**** Herunterladen der vertrauenswürdigen Zertifikate der Stammzertifizierungsstelle.

    ![Link zum Herunterladen der vertrauenswürdigen Zertifikate der Stammzertifizierungsstelle](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  Sollte die Anmeldeseite für den vSphere-Webclient nicht angezeigt werden, überprüfen Sie die Proxyeinstellungen Ihres Browsers.

2. Klicken Sie auf **Download trusted root CA certificates** (Vertrauenswürdige Zertifikate der Stammzertifizierungsstelle herunterladen).

    Der vCenter-Server lädt eine Datei auf Ihren lokalen Computer herunter. Die Datei heißt **download**. Abhängig von Ihrem Browser werden Sie gefragt, ob Sie die Datei öffnen oder speichern möchten.

    ![Downloadmeldung beim Herunterladen von Zertifikaten](./media/backup-azure-backup-server-vmware/download-certs.png)

3. Speichern Sie die Datei an einen Speicherort in Azure Backup Server. Fügen Sie beim Speichern der Datei die Dateinamenerweiterung „.zip“ hinzu.

    Bei der Datei handelt es sich um eine ZIP-Datei mit Informationen zu den Zertifikaten. Die Erweiterung „zip.“ ermöglicht die Nutzung von Extraktionstools.

4. Klicken Sie mit der rechten Maustaste auf **download.zip**, und wählen Sie **Alle extrahieren** aus, um den Inhalt zu extrahieren.

    Der Inhalt der ZIP-Datei wird in einen Ordner namens **certs** extrahiert. Der Ordner „certs“ enthält zwei Arten von Dateien. Die Stammzertifikatdatei hat eine Erweiterung, die mit einer nummerierten Sequenz wie „.0“ und „.1“ beginnt.
    
    Die Erweiterung der CRL-Datei beginnt mit „.r0“ oder „.r1“. Die CRL-Datei ist einem Zertifikat zugeordnet.

    ![Lokal extrahierte Datei „download“ ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. Klicken Sie im Ordner **certs** mit der rechten Maustaste auf die Stammzertifikatdatei, und klicken Sie anschließend auf **Umbenennen**.

    ![Umbenennen des Stammzertifikats ](./media/backup-azure-backup-server-vmware/rename-cert.png)

    Ändern Sie die Erweiterung des Stammzertifikats in „.crt“. Wenn Sie gefragt werden, ob Sie die Erweiterung ändern möchten, klicken Sie auf **Ja** oder **OK**. Andernfalls ändern Sie die vorgesehene Funktion der Datei. Das Symbol der Datei ändert sich in das Symbol eines Stammzertifikats.

6. Klicken Sie mit der rechten Maustaste auf das Stammzertifikat, und wählen Sie im Kontextmenü die Option **Zertifikat installieren** aus.

    Das Dialogfeld **Zertifikatimport-Assistent** wird geöffnet.

7. Wählen Sie im Dialogfeld **Zertifikatimport-Assistent** als Ziel für das Zertifikat **Lokaler Computer** aus, und klicken Sie auf **Weiter**, um den Vorgang fortzusetzen.

    ![Zieloptionen für die Zertifikatspeicherung ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

    Wenn Sie gefragt werden, ob Sie Änderungen am Computer zulassen möchten, klicken Sie für alle Änderungen auf **Ja** oder **OK**.

8. Wählen Sie auf der Seite **Zertifikatspeicher** die Option **Alle Zertifikate in folgendem Speicher speichern** aus, und klicken Sie auf **Durchsuchen**, um den Zertifikatspeicher auszuwählen.

    ![Ablegen von Zertifikaten an einem bestimmten Speicherort](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

    Das Dialogfeld **Zertifikatspeicher auswählen** wird geöffnet.

    ![Ordnerhierarchie im Zertifikatspeicher](./media/backup-azure-backup-server-vmware/cert-store.png)

9. Wählen Sie als Zielordner für die Zertifikate **Vertrauenswürdige Stammzertifizierungsstellen** aus, und klicken Sie auf **OK**.

    ![Zielordner für Zertifikate](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

    Der Ordner **Vertrauenswürdige Stammzertifizierungsstellen** wird als Zertifikatspeicher bestätigt. Klicken Sie auf **Weiter**.

    ![Speicherordner von Zertifikaten](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. Vergewissern Sie sich auf der Seite **Fertigstellen des Assistenten**, das sich das Zertifikat im gewünschten Ordner befindet, und klicken Sie auf **Fertig stellen**, um den Assistenten abzuschließen.

    ![Prüfen, ob sich das Zertifikat im richtigen Ordner befindet](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

    Ein Dialogfeld wird angezeigt, in dem der erfolgreiche Import des Zertifikats bestätigt wird.

11. Melden Sie sich am vCenter-Server an, um sich zu vergewissern, dass Ihre Verbindung sicher ist.

  Wenn der Zertifikatimport nicht erfolgreich war und Sie keine sichere Verbindung herstellen können, lesen Sie in der VMware vSphere-Dokumentation den Abschnitt [Obtaining Server Certificates](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html) (Beziehen von Serverzertifikaten).

  Wenn in Ihrer Organisation sichere Grenzen eingerichtet wurden und das HTTPS-Protokoll nicht aktiviert werden soll, gehen Sie wie folgt vor, um die sichere Kommunikation zu deaktivieren.

### <a name="disable-secure-communication-protocol"></a>Deaktivieren des sicheren Kommunikationsprotokolls

Falls Ihre Organisation das HTTPS-Protokoll nicht benötigt, führen Sie die folgenden Schritte aus, um HTTPS zu deaktivieren. Erstellen Sie zum Deaktivieren des Standardverhaltens einen Registrierungsschlüssel, der das Standardverhalten ignoriert.

1. Kopieren Sie den folgenden Text, und fügen Sie ihn in eine TXT-Datei ein.

  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
  "IgnoreCertificateValidation"=dword:00000001
  ```

2. Speichern Sie die Datei auf Ihrem Azure Backup Server-Computer. Wählen Sie als Dateiname „DisableSecureAuthentication.reg“.

3. Doppelklicken Sie auf die Datei, um den Registrierungseintrag zu aktivieren.


## <a name="create-a-role-and-user-account-on-the-vcenter-server"></a>Erstellen einer Rolle und eines Benutzerkontos auf dem vCenter-Server

Auf dem vCenter-Server ist eine Rolle eine vordefinierte Gruppe von Berechtigungen. Ein vCenter Server-Administrator erstellt die Rollen. Zum Zuweisen von Berechtigungen ordnet der Administrator Benutzerkonten einer Rolle zu. Erstellen Sie zum Einrichten der erforderlichen Benutzeranmeldeinformationen für die Sicherung des vCenter Server-Computers eine Rolle mit bestimmten Berechtigungen, und ordnen Sie das Benutzerkonto der Rolle zu.

Azure Backup Server verwendet für die Authentifizierung beim vCenter-Server einen Benutzernamen und ein Kennwort. Diese Anmeldeinformationen werden von Azure Backup Server zur Authentifizierung bei allen Sicherungsvorgängen verwendet.

So fügen Sie eine vCenter Server-Rolle und Berechtigungen für einen Sicherungsadministrator hinzu

1. Melden Sie sich beim vCenter-Server an, und klicken Sie dann im vCenter Server-Bereich **Navigator** auf **Verwaltung**.

    ![Option „Verwaltung“ im vCenter Server-Bereich „Navigator“](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. Wählen Sie im Abschnitt **Verwaltung** die Option **Rollen** aus, und klicken Sie im Bereich **Rollen** auf das Symbol zum Hinzufügen von Rollen (+-Symbol).

    ![Hinzufügen einer Rolle](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

    Das Dialogfeld **Rolle erstellen** wird angezeigt.

    ![Rolle erstellen](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. Geben Sie im Dialogfeld **Rolle erstellen** im Feld **Rollenname** den Namen *BackupAdminRole* ein. Sie können als Rollennamen einen beliebigen Namen auswählen, es empfiehlt sich jedoch, einen aussagekräftigen Namen zu verwenden.

4. Wählen Sie die Berechtigungen für die entsprechende Version von vCenter aus, und klicken Sie anschließend auf **OK**. Die folgende Tabelle gibt Aufschluss über die erforderlichen Berechtigungen für vCenter 6.0 und vCenter 5.5.

  Klicken Sie beim Auswählen der Berechtigungen auf das Symbol neben der übergeordneten Bezeichnung, um die übergeordnete Berechtigung zu erweitern und die untergeordneten Berechtigungen anzuzeigen. Um die Berechtigung „VirtualMachine“ auszuwählen, müssen Sie mehrere Ebenen der Hierarchie über- und untergeordneter Berechtigungen durchlaufen. Sie müssen nicht alle untergeordneten Berechtigungen einer übergeordneten Berechtigung auswählen.

  ![Hierarchie über- und untergeordneter Berechtigungen](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  Nachdem Sie auf **OK** geklickt haben, wird die neue Rolle in der Liste im Bereich „Rollen“ angezeigt.

|Berechtigungen für vCenter 6.0| Berechtigungen für vCenter 5.5|
|--------------------------|---------------------------|
|Datastore.AllocateSpace   | Datastore.AllocateSpace|
|Global.ManageCustomFields | Global.ManageCustomerFields|
|Global.SetCustomFields    |   |
|Host.Local.CreateVM       | Network.Assign |
|Network.Assign            |  |
|Resource.AssignVMToPool   |  |
|VirtualMachine.Config.AddNewDisk  | VirtualMachine.Config.AddNewDisk   |
|VirtualMachine.Config.AdvanceConfig| VirtualMachine.Config.AdvancedConfig|
|VirtualMachine.Config.ChangeTracking| VirtualMachine.Config.ChangeTracking |
|VirtualMachine.Config.HostUSBDevice||
|VirtualMachine.Config.QueryUnownedFiles|    |
|VirtualMachine.Config.SwapPlacement| VirtualMachine.Config.SwapPlacement |
|VirtualMachine.Interact.PowerOff| VirtualMachine.Interact.PowerOff |
|VirtualMachine.Inventory.Create| VirtualMachine.Inventory.Create |
|VirtualMachine.Provisioning.DiskRandomAccess| |
|VirtualMachine.Provisioning.DiskRandomRead|VirtualMachine.Provisioning.DiskRandomRead |
|VirtualMachine.State.CreateSnapshot| VirtualMachine.State.CreateSnapshot|
|VirtualMachine.State.RemoveSnapshot|VirtualMachine.State.RemoveSnapshot |
</br>



## <a name="create-a-vcenter-server-user-account-and-permissions"></a>Erstellen eines vCenter Server-Benutzerkontos und von Berechtigungen

Nachdem die Rolle mit Berechtigungen eingerichtet wurde, erstellen Sie ein Benutzerkonto. Das Benutzerkonto verfügt über einen Namen und ein Kennwort, die die für die Authentifizierung verwendeten Anmeldeinformationen darstellen.

1. Klicken Sie zum Erstellen eines Benutzerkontos im vCenter Server-Bereich **Navigator** auf **Benutzer und Gruppen**.

    ![Option „Benutzer und Gruppen“](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    Der vCenter-Bereich **Benutzer und Gruppen** wird angezeigt.

    ![Der vCenter-Bereich „Benutzer und Gruppen“](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. Klicken Sie im vCenter-Bereich **Benutzer und Gruppen** auf der Registerkarte **Benutzer** auf das Symbol zum Hinzufügen von Benutzern (+-Symbol).

    Das Dialogfeld **Neuer Benutzer** wird angezeigt.

3. Fügen Sie im Dialogfeld **Neuer Benutzer** die Informationen des Benutzers hinzu, und klicken Sie dann auf **OK**. In diesem Verfahren lautet der Benutzername „BackupAdmin“.

    ![Dialogfeld „Neuer Benutzer“](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

    Das neue Benutzerkonto wird in der Liste angezeigt.

4. Um das Benutzerkonto der Rolle zuzuordnen, klicken Sie im Bereich **Navigator** auf **Globale Berechtigungen**. Klicken Sie im Bereich **Globale Berechtigungen** auf der Registerkarte **Verwalten** auf das Symbol zum Hinzufügen (+-Symbol).

    ![Bereich „Globale Berechtigungen“](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

    Das Dialogfeld **Globale Berechtigungen – Berechtigung hinzufügen** wird geöffnet.

5. Klicken Sie im Dialogfeld **Globale Berechtigungen – Berechtigung hinzufügen** auf **Hinzufügen**, um den Benutzer oder die Gruppe auszuwählen.

    ![Auswählen eines Benutzers oder einer Gruppe](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

    Das Dialogfeld **Benutzer/Gruppen auswählen** wird geöffnet.

6. Wählen Sie im Dialogfeld **Benutzer/Gruppen auswählen** die Option **BackupAdmin** aus, und klicken Sie anschließend auf **Hinzufügen**.

    In **Benutzer** wird das Format *Domäne\Benutzername* für das Benutzerkonto verwendet. Wenn Sie eine andere Domäne verwenden möchten, wählen Sie sie in der Liste **Domäne** aus.

    ![Benutzer „BackupAdmin“ hinzufügen](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

    Klicken Sie auf **OK**, um die ausgewählten Benutzer dem Dialogfeld **Berechtigung hinzufügen** hinzuzufügen.

7. Sie haben den Benutzer bestimmt. Weisen Sie ihn nun der Rolle hinzu. Wählen Sie im Dropdownmenü des Bereichs **Zugewiesene Rolle** den Eintrag **BackupAdminRole** aus, und klicken Sie anschließend auf **OK**.

    ![Zuweisen des Benutzers zur Rolle](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  Im Bereich **Globale Berechtigungen** werden auf der Registerkarte **Verwalten** das neue Benutzerkonto und die zugeordnete Rolle in der Liste angezeigt.


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a>Einrichten von vCenter Server-Anmeldeinformationen für Azure Backup Server

Installieren Sie [Update 1 für Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server), bevor Sie den VMware-Server zu Azure Backup Server hinzufügen.

1. Doppelklicken Sie zum Öffnen von Azure Backup Server auf das Symbol auf dem Azure Backup Server-Desktop.

    ![Azure Backup Server-Symbol](./media/backup-azure-backup-server-vmware/mabs-icon.png)

    Sollte das Symbol auf dem Desktop nicht angezeigt werden, öffnen Sie Azure Backup Server über die Liste der installierten Apps. Der Name der Azure Backup Server-App lautet „Microsoft Azure Backup“.

2. Klicken Sie in der Azure Backup Server-Konsole auf **Verwaltung**, dann auf **Produktionsserver** und anschließend auf dem Menüband auf **VMware verwalten**.

    ![Azure Backup Server-Konsole](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

    Das Dialogfeld **Anmeldeinformationen verwalten** wird angezeigt.

    ![Azure Backup Server-Dialogfeld „Anmeldeinformationen verwalten“](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. Klicken Sie im Dialogfeld **Anmeldeinformationen verwalten** auf **Hinzufügen**, um das Dialogfeld **Anmeldeinformationen hinzufügen** zu öffnen.

4. Geben Sie im Dialogfeld **Anmeldeinformationen hinzufügen** einen Namen und eine Beschreibung der neuen Anmeldeinformationen ein. Geben Sie dann den Benutzernamen und das Kennwort an. Der Anmeldename *Contoso vCenter-Anmeldeinformationen* wird verwendet, um die Anmeldeinformationen im nächsten Schritt zu bestimmen. Verwenden Sie den Benutzernamen samt Kennwort, den Sie auch für den vCenter-Server verwenden. Falls sich der vCenter-Server und Azure Backup Server nicht in der gleichen Domäne befinden, geben Sie in **Benutzername** die Domäne an.

    ![Azure Backup Server-Dialogfeld „Anmeldeinformationen hinzufügen“](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

    Klicken Sie auf **Hinzufügen**, um die neuen Anmeldeinformationen zu Azure Backup Server hinzuzufügen. Die neuen Anmeldeinformationen werden in der Liste des Dialogfelds **Anmeldeinformationen verwalten** angezeigt.
    ![Azure Backup Server-Dialogfeld „Anmeldeinformationen verwalten“](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. Klicken Sie rechts oben auf **X**, um das Dialogfeld **Anmeldeinformationen verwalten** zu schließen.


## <a name="add-the-vcenter-server-to-azure-backup-server"></a>Hinzufügen des vCenter-Servers zu Azure Backup Server

Mit dem Assistenten zum Hinzufügen von Produktionsservern wird der vCenter-Server zu Azure Backup Server hinzugefügt.

Um den Assistenten zum Hinzufügen von Produktionsservern zu öffnen, führen Sie die folgenden Schritte aus:

1. Klicken Sie in der Azure Backup Server-Konsole auf **Verwaltung**, dann auf **Produktionsserver** und anschließend auf **Hinzufügen**.

    ![Öffnen des Assistenten zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

    Das Dialogfeld **Assistent zum Hinzufügen von Produktionsservern** wird geöffnet.

    ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. Wählen Sie auf der Seite **Produktionsservertyp auswählen** die Option **VMware-Server** aus, und klicken Sie auf **Weiter**.

3. Geben Sie unter **Servername/IP-Adresse** den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) oder die IP-Adresse des VMware-Servers ein. Wenn alle ESXi-Server von der gleichen vCenter-Instanz verwaltet werden, können Sie den vCenter-Namen verwenden.

    ![Angeben des FQDN oder der IP-Adresse des VMware-Servers](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. Geben Sie in **SSL-Port** den Port ein, der für die Kommunikation mit dem VMware-Server verwendet wird. Verwenden Sie den Standardport 443, es sei denn, Sie wissen, dass ein anderer Port erforderlich ist.

5. Wählen Sie im Dialogfeld **Anmeldeinformationen angeben** die zuvor erstellten Anmeldeinformationen aus.

    ![Angeben von Anmeldeinformationen](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. Klicken Sie auf **Hinzufügen**, um den VMware-Server der Liste **Hinzugefügte VMware-Server** hinzuzufügen, und klicken Sie anschließend auf **Weiter**, um zur nächsten Seite des Assistenten zu gelangen.

    ![Hinzufügen des VMware-Servers und von Anmeldeinformationen](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. Klicken Sie auf der Seite **Zusammenfassung** auf **Hinzufügen**, um den angegebenen VMware-Server zu Azure Backup Server hinzuzufügen.

    ![Hinzufügen eines VMware-Servers zu Azure Backup Server](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  Da die VMware-Serversicherung ohne Agent erfolgt, wird der neue Server sofort hinzugefügt. Die Ergebnisse werden auf der Seite **Fertig stellen** angezeigt.

  ![Seite „Fertig stellen“](./media/backup-azure-backup-server-vmware/summary-screen.png)

  Um Azure Backup Server mehrere vCenter Server-Instanzen hinzuzufügen, wiederholen Sie die vorherigen Schritten in diesem Abschnitt.

Nachdem Sie den vCenter-Server zu Azure Backup Server hinzugefügt haben, erstellen Sie als Nächstes eine Schutzgruppe. Die Schutzgruppe enthält die Details für die kurz- und die langfristige Aufbewahrung. Darüber hinaus legen Sie in der Schutzgruppe die Sicherungsrichtlinie fest und wenden sie an. Die Sicherungsrichtlinie steuert, wann Sicherungen erstellt werden und was gesichert wird.


## <a name="configure-a-protection-group"></a>Konfigurieren einer Schutzgruppe

Falls Sie System Center Data Protection Manager oder Azure Backup Server bisher noch nicht verwendet haben, lesen Sie [Planen von Datenträgersicherungen](https://technet.microsoft.com/library/hh758026.aspx), um Ihre Hardwareumgebung vorzubereiten. Nachdem Sie sich vergewissert haben, dass Sie über genügend Speicher verfügen, fügen Sie mithilfe des Assistenten zum Erstellen einer neuen Schutzgruppe die VMware-VMs hinzu.

1. Klicken Sie in der Azure Backup Server-Verwaltungskonsole auf **Schutz** und dann im Menüband des Tools auf **Neu**, um den Assistenten zum Erstellen einer neuen Schutzgruppe zu öffnen.

    ![Öffnen des Assistenten zum Erstellen einer neuen Schutzgruppe](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

    Das Dialogfeld **Assistent zum Erstellen einer neuen Schutzgruppe** wird geöffnet.

    ![Dialogfeld „Assistent zum Erstellen einer neuen Schutzgruppe“](./media/backup-azure-backup-server-vmware/protection-wizard.png)

    Klicken Sie auf **Weiter**, um mit der Seite **Schutzgruppentyp auswählen** fortzufahren.

2. Klicken Sie auf der Seite **Schutzgruppentyp auswählen** auf **Server** und dann auf **Weiter**. Die Seite **Gruppenmitglieder auswählen** wird angezeigt.

3. Auf der Seite **Gruppenmitglieder auswählen** werden die verfügbaren und die ausgewählten Mitglieder angezeigt. Wählen Sie die zu schützenden Mitglieder aus, und klicken Sie auf **Weiter**.

    ![Gruppenmitglieder auswählen](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

    Beachten Sie beim Auswählen eines Mitglieds Folgendes: Wenn Sie einen Ordner auswählen, der weitere Ordner oder VMs enthält, werden diese Ordner oder VMs ebenfalls ausgewählt. Die Aufnahme der Ordner und virtuellen Computer im übergeordneten Ordner wird als „Schutz auf Ordnerebene“ bezeichnet. Deaktivieren Sie das Kontrollkästchen, um einen Ordner oder eine VM zu entfernen.

    Wenn ein virtueller Computer oder ein Ordner mit einem virtuellen Computer bereits in Azure gesichert wird, können Sie diesen virtuellen Computer nicht erneut auswählen. Das bedeutet, dass eine VM nicht erneut gesichert werden kann, wenn sie in Azure gesichert wird. Dadurch wird die Erstellung doppelter Wiederherstellungspunkte für eine VM verhindert. Um anzuzeigen, welche Azure Backup Server-Instanz bereits ein Mitglied sichert, zeigen Sie auf das Mitglied, um den Namen des Schutzservers anzuzeigen.

4. Geben Sie auf der Seite **Methode für die Datensicherheit auswählen** einen Namen für die Schutzgruppe ein. Die Optionen für kurzfristigen Schutz (auf Datenträger) und Onlineschutz sind ausgewählt. Wenn Sie den (Azure-basierten) Onlineschutz verwenden möchten, müssen Sie den kurzfristigen datenträgerbasierten Schutz verwenden. Klicken Sie auf **Weiter**, um zur Dauer für den kurzfristigen Schutz zu wechseln.

    ![Datenschutzmethode auswählen](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. Geben Sie auf der Seite **Kurzfristige Ziele angeben** für **Beibehaltungsdauer** an, wie viele Tage die *auf Datenträgern gespeicherten* Wiederherstellungspunkte beibehalten werden sollen. Klicken Sie zum Ändern der Uhrzeit und des Datums für die Erstellung von Wiederherstellungspunkten auf **Ändern**. Bei kurzfristigen Wiederherstellungspunkten handelt es sich um vollständige Sicherungen. Es sind keine inkrementellen Sicherungen. Wenn Sie mit den kurzfristigen Zielen zufrieden sind, klicken Sie auf **Weiter**.

    ![Kurzfristige Ziele angeben](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. Überprüfen Sie auf der Seite **Datenträgerzuordnung überprüfen** den Speicherplatz für die VMs, und passen Sie ihn ggf. an. Die empfohlenen Datenträgerzuordnungen basieren auf der auf der Seite **Kurzfristige Ziele angeben** angegebenen Beibehaltungsdauer, auf dem Typ der Workload und dem Umfang der geschützten Daten (in Schritt 3 ermittelt).  

  - **Datengröße:** Umfang der Daten in der Schutzgruppe.
  - **Speicherplatz:** Der für die Schutzgruppe empfohlene Speicherplatz auf dem Datenträger. Wenn Sie diese Einstellung ändern möchten, muss der zugewiesene Gesamtspeicherplatz geringfügig größer sein als das voraussichtliche Wachstum der einzelnen Datenquellen.
  - **Daten zusammenstellen**: Wenn Sie das Zusammenstellen aktivieren, können mehrere Datenquellen im Schutzumfang einem einzelnen Volume für Replikate und Wiederherstellungspunkte zugeordnet werden. Die Zusammenstellung wird nicht für alle Workloads unterstützt.
  - **Automatisch anwachsen:** Wenn Sie diese Einstellung aktivieren, versucht System Center Data Protection Manager die Datenträgergröße um 25 % zu erhöhen, wenn Daten in der Schutzgruppe die ursprüngliche Zuordnung überschreiten.
  - **Speicherpooldetails:** Zeigt den aktuellen Status des Speicherpools an, einschließlich Gesamtgröße und verbleibende Größe des Datenträgers.

    ![Überprüfen der Datenträgerzuordnung](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

    Wenn Sie mit der Speicherzuordnung zufrieden sind, klicken Sie auf **Weiter**.

7. Geben Sie auf der Seite **Replikaterstellungsmethode auswählen** an, wie die erste Kopie (bzw. das erste Replikat) der geschützten Daten in Azure Backup Server erstellt werden soll.

    Die Standardeinstellungen lauten **Automatisch über das Netzwerk** und **Jetzt**. Bei Verwendung der Standardeinstellungen wird die Angabe einer Nebenzeit empfohlen. Wählen Sie **Später**, und geben Sie Datum und Uhrzeit an.

    Ziehen Sie bei großen Datenmengen oder nicht optimalen Netzwerkbedingungen die Offlinereplikation der Daten mit Wechselmedien in Betracht.

    Nachdem Sie Ihre Auswahl vorgenommen haben, klicken Sie auf **Weiter**.

    ![Replikaterstellungsmethode auswählen](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. Legen Sie auf der Seite **Konsistenzprüfungsoptionen** fest, wie und wann Konsistenzprüfungen automatisiert werden sollen. Sie können Konsistenzprüfungen bei inkonsistenten Replikatdaten oder gemäß einem festgelegten Zeitplan ausführen.

    Wenn Sie keine automatische Konsistenzprüfung konfigurieren möchten, können Sie eine manuelle Überprüfung ausführen. Klicken Sie im Bereich „Schutz“ der Azure Backup Server-Konsole mit der rechten Maustaste auf die Schutzgruppe, und wählen Sie **Konsistenzprüfung ausführen** aus.

    Klicken Sie auf **Weiter**, um zur nächsten Seite zu wechseln.

9. Wählen Sie auf der Seite **Online zu schützende Daten angeben** eine oder mehrere zu schützende Datenquellen aus. Sie können die Mitglieder einzeln auswählen oder auf **Select All** (Alles markieren) klicken, um alle Mitglieder auszuwählen. Klicken Sie nach dem Auswählen der Mitglieder auf **Weiter**.

    ![Onlineschutzdaten angeben](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. Geben Sie auf der Seite **Onlinesicherungszeitplan angeben** den Zeitplan zum Erstellen von Wiederherstellungsendpunkten aus der Datenträgersicherung an. Nach der Erstellung des Wiederherstellungspunkts wird er in den Recovery Services-Tresor in Azure übertragen. Wenn Sie mit dem Onlinesicherungszeitplan zufrieden sind, klicken Sie auf **Weiter**.

    ![Zeitplan für Onlinesicherung angeben](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. Geben Sie auf dem Bildschirm **Onlineaufbewahrungsrichtlinie angeben** an, wie lange die Sicherungsdaten in Azure aufbewahrt werden sollen. Nachdem die Richtlinie definiert wurde, klicken Sie auf **Weiter**.

    ![Online-Aufbewahrungsrichtlinie angeben](./media/backup-azure-backup-server-vmware/retention-policy.png)

    Für die Datenaufbewahrung in Azure gibt es kein zeitliches Limit. Beim Speichern von Wiederherstellungspunktdaten in Azure gilt nur folgende Einschränkung: Pro geschützter Instanz können maximal 9.999 Wiederherstellungspunkte erstellt werden. In diesem Beispiel ist die geschützte Instanz der VMware-Server.

12. Überprüfen Sie auf der Seite **Zusammenfassung** die Details der Mitglieder der Schutzgruppe und die Einstellungen, und klicken Sie dann auf **Gruppe erstellen**.

    ![Zusammenfassung mit den Schutzgruppenmitgliedern und Einstellungen](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie Azure Backup Server für den Schutz von VMware-Workloads verwenden, interessiert Sie vielleicht auch die Verwendung von Azure Backup Server für den Schutz von [Microsoft Exchange Server](./backup-azure-exchange-mabs.md), einer [Microsoft SharePoint-Farm](./backup-azure-backup-sharepoint-mabs.md) oder einer [SQL Server-Datenbank](./backup-azure-sql-mabs.md).

Informationen zu Problemen beim Registrieren des Agents, Konfigurieren der Schutzgruppe oder bei Sicherungsaufträgen finden Sie unter [Behandeln von Problemen mit Azure Backup Server](./backup-azure-mabs-troubleshoot.md).

