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
ms.date: 04/20/2017
ms.author: markgal;
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: faa3c9d5de759e4d8c0589153ea93e83a580f983
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="back-up-vmware-server-to-azure"></a>Sichern eines VMware-Servers in Azure

Dieser Artikel erläutert, wie Sie zum Schutz der VMware-Server-Workload eine Azure Backup Server-Instanz konfigurieren. In diesem Artikel wird davon ausgegangen, dass Sie Azure Backup Server bereits installiert haben. Falls Sie Azure Backup Server nicht installiert haben, lesen Sie den Artikel [Vorbereiten der Sicherung von Workloads per Azure Backup Server](backup-azure-microsoft-azure-backup.md).

Azure Backup Server kann die VMware vCenter-Serverversionen 6.0 und 5.5 sichern bzw. schützen.


## <a name="create-a-secure-connection-to-the-vcenter-server"></a>Erstellen einer sichere Verbindung mit dem vCenter-Server

Azure Backup Server kommuniziert mit vCenter-Servern standardmäßig über den HTTPS-Kanal. Zum Aktivieren der sicheren Kommunikation empfiehlt es sich, in der Azure Backup Server-Instanz das Zertifikat der VMware-Zertifizierungsstelle (Certificate Authority, CA) zu installieren. Falls Sie keine sichere Kommunikation benötigen und die HTTPS-Anforderung deaktivieren möchten, lesen Sie den Abschnitt [Deaktivieren des sicheren Kommunikationsprotokolls](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol). Importieren Sie zum Erstellen einer sicheren Verbindung zwischen Azure Backup Server und dem vCenter-Server das vertrauenswürdige Zertifikat in die Azure Backup Server-Instanz.

Üblicherweise verwenden Sie einen Browser auf dem Azure Backup Server-Computer, um über den vSphere-Webclient eine Verbindung mit dem vCenter-Server herzustellen. Wenn Sie über den Browser des Azure Backup Server-Computers zum ersten Mal eine Verbindung mit dem vCenter-Server herstellen, ist die Verbindung nicht sicher. Die folgende Abbildung zeigt die unsichere Verbindung.

![Beispiel für eine unsichere Verbindung mit dem VMware-Server](./media/backup-azure-backup-server-vmware/unsecure-url.png)

Um dieses Problem zu beheben und eine sichere Verbindung zu erstellen, laden Sie die vertrauenswürdigen Zertifikate der Stammzertifizierungsstelle herunter.

1. Geben Sie im Browser auf dem Azure Backup Server-Computer die URL für den vSphere-Webclient ein.

  Die Anmeldeseite des vSphere-Webclients erscheint.

  ![vSphere-Webclient](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

  Im unteren Bereich der Informationen für Administratoren und Entwickler befindet sich der Link zum**** Herunterladen der vertrauenswürdigen Zertifikate der Stammzertifizierungsstelle.

  ![Link zum Herunterladen der vertrauenswürdigen Zertifikate der Stammzertifizierungsstelle](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  Sollte die Anmeldeseite für den vSphere-Webclient nicht angezeigt werden, überprüfen Sie die Proxyeinstellungen Ihres Browsers.

2. Klicken Sie auf **Download trusted root CA certificates** (Vertrauenswürdige Zertifikate der Stammzertifizierungsstelle herunterladen).

  Der vCenter-Server lädt eine Datei auf Ihren lokalen Computer herunter. Die Datei heißt **download**. Abhängig von Ihrem Browser werden Sie gefragt, ob Sie die Datei öffnen oder speichern möchten.

  ![Downloadmeldung beim Herunterladen von Zertifikaten](./media/backup-azure-backup-server-vmware/download-certs.png)

3. Speichern Sie die Datei an einen Speicherort auf dem Azure Backup Server-Computer. Hängen Sie beim Speichern der Datei die Dateinamenerweiterung „.zip“ hinzu.

  Bei der Datei handelt es sich um eine ZIP-Datei mit Informationen zu den Zertifikaten. Das Hinzufügen der Erweiterung „.zip“ ermöglicht die Verwendung von Extrahierungstools.

4. Klicken Sie mit der rechten Maustaste auf **download.zip**, und wählen Sie „Alle extrahieren“ aus, um den Inhalt zu extrahieren.

  Der Inhalt der ZIP-Datei wird in einen Ordner namens **certs** extrahiert. Der Ordner „certs“ enthält zwei Arten von Dateien. Die Erweiterung der Stammzertifikatdatei lautet „.0“, „.1“ oder „.*Zahl*“. Die Erweiterung der CRL-Datei beginnt mit „.r0“, „.r1“ usw. Die CRL-Datei ist einem Zertifikat zugeordnet.

  ![Lokal extrahierte Downloaddatei ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. Klicken Sie im Ordner **certs** mit der rechten Maustaste auf die Stammzertifikatdatei, und klicken Sie anschließend auf **Umbenennen**.

  ![Umbenennen des Stammzertifikats ](./media/backup-azure-backup-server-vmware/rename-cert.png)

  Ändern Sie die Erweiterung des Stammzertifikats in „.crt“. Da die Änderung der Erweiterung eine Änderung der Dateifunktionen zur Folge haben kann, werden Sie möglicherweise gefragt, ob Sie die Erweiterung wirklich ändern möchten. Beantworten Sie diese Frage mit „Ja“ oder „OK“. Das Symbol für die Datei ändert sich in das Symbol für ein Stammzertifikat.

6. Klicken Sie mit der rechten Maustaste auf das Stammzertifikat, und wählen Sie im Kontextmenü die Option **Zertifikat installieren** aus.

  Der Zertifikatimport-Assistent wird geöffnet.

7. Wählen Sie im Zertifikatimport-Assistenten als Ziel für das Zertifikat **Lokaler Computer** aus, und klicken Sie auf **Weiter**, um den Vorgang fortzusetzen.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

  Wenn Sie gefragt werden, ob Sie Änderungen am Computer zulassen möchten, klicken Sie auf „Ja“ oder „OK“ (für alle Änderungen).

8. Wählen Sie auf dem Bildschirm „Zertifikatspeicher“ die Option **Alle Zertifikate in folgendem Speicher speichern** aus, und klicken Sie auf **Durchsuchen**, um den Zertifikatspeicher auszuwählen.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

  Das Dialogfeld „Zertifikatspeicher auswählen“ wird geöffnet.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/cert-store.png)

9. Wählen Sie **Vertrauenswürdige Stammzertifizierungsstellen** als Zielordner für die Zertifikate aus, und klicken Sie auf **OK**.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

  Der ausgewählte Zertifikatspeicher wird im **Zertifikatimport-Assistenten** angezeigt. Klicken Sie auf **Weiter**.


  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. Vergewissern Sie sich auf dem Bildschirm zum Fertigstellen des Zertifikatimport-Assistenten, das sich das Zertifikat im gewünschten Ordner befindet, und klicken Sie auf **Fertig stellen**, um den Assistenten abzuschließen.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

  In einem Dialogfeld wird angezeigt, ob der Import erfolgreich war.

11. Melden Sie sich beim vCenter-Server an, um sich zu vergewissern, dass Ihre Verbindung sicher ist.

  Falls Sie Probleme beim Importieren des Zertifikats haben und keine sichere Verbindung herstellen können, lesen Sie in der VMware vSphere-Dokumentation unter [Obtaining Server Certificates](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html) (Beziehen von Serverzertifikaten) nach.

  Wenn in Ihrer Organisation sichere Grenzen eingerichtet wurden und das HTTPS-Protokoll nicht aktiviert werden soll, gehen Sie wie folgt vor, um die sichere Kommunikation zu deaktivieren.

### <a name="disable-secure-communication-protocol"></a>Deaktivieren des sicheren Kommunikationsprotokolls

Falls Ihre Organisation kein sicheres Kommunikationsprotokoll (HTTPS) benötigt, führen Sie die folgenden Schritte aus, um HTTPS zu deaktivieren. Erstellen Sie zum Deaktivieren des Standardverhaltens einen Registrierungsschlüssel, der das Standardverhalten ignoriert.

1. Kopieren Sie den folgenden Text, und fügen Sie ihn in eine TXT-Datei ein.

  ```
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
"IgnoreCertificateValidation"=dword:00000001
```
2. Speichern Sie die Datei unter dem Namen **DisableSecureAuthentication.reg** auf Ihrem Azure Backup Server-Computer.

3. Doppelklicken Sie auf die Datei, um den Registrierungseintrag zu aktivieren.


## <a name="create-a-role-and-user-account-on-the-vcenter-server"></a>Erstellen einer Rolle und eines Benutzerkontos auf dem vCenter-Server

Auf dem vCenter-Server ist eine Rolle eine vordefinierte Gruppe von Berechtigungen. Ein Serveradministrator auf dem vCenter-Server erstellt die Rollen und verknüpft Benutzerkonten mit Rollen, um Berechtigungen zuzuweisen. Erstellen Sie zum Einrichten der erforderlichen Benutzeranmeldeinformationen für die Sicherung des vCenter-Servers eine Rolle mit bestimmten Berechtigungen, und ordnen Sie das Benutzerkonto der Rolle zu.

Azure Backup Server verwendet für die Authentifizierung beim vCenter-Server einen Benutzernamen und ein Kennwort. Diese Anmeldeinformationen werden von Azure Backup Server zur Authentifizierung bei allen Sicherungsvorgängen verwendet.

So fügen Sie eine vCenter-Server-Rolle und Berechtigungen für einen Sicherungsadministrator hinzu:

1. Melden Sie sich beim vCenter-Server an, und klicken Sie im **Navigator** auf **Verwaltung**.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. Wählen Sie im Abschnitt **Verwaltung** die Option **Rollen** aus, und klicken Sie im Bereich **Rollen** auf das Symbol zum Hinzufügen von Rollen (+-Symbol).

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

  Das Dialogfeld **Rolle erstellen** wird geöffnet.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. Geben Sie im Dialogfeld **Rolle erstellen** im Feld **Rollenname** den Namen *BackupAdminRole* ein. Sie können als Rollennamen einen beliebigen Namen auswählen, es empfiehlt sich jedoch, einen aussagekräftigen Namen zu verwenden.

4. Wählen Sie die Berechtigungen für die entsprechende Version von vCenter aus, und klicken Sie anschließend auf **OK**. Die folgende Tabelle gibt Aufschluss über die erforderlichen Berechtigungen für vCenter 6.0 und vCenter 5.5.

  Klicken Sie beim Auswählen der Berechtigungen auf das Chevron der übergeordneten Bezeichnung, um die übergeordnete Berechtigung zu erweitern und die untergeordneten Berechtigungen anzuzeigen. Zum Auswählen der benötigten VirtualMachine-Berechtigungen müssen Sie etwas tiefer in die Struktur vordringen. Sie müssen nicht alle untergeordneten Berechtigungen einer übergeordneten Berechtigung auswählen.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  Nachdem Sie auf **OK** geklickt haben, erscheint die neue Rolle in der Liste im Bereich „Rollen“.

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



## <a name="create-vcenter-server-user-account-and-permissions"></a>Erstellen von Benutzerkonto und Berechtigungen für den vCenter-Server

Wenn die Rolle mit Berechtigungen vorhanden ist, erstellen Sie ein Benutzerkonto. Das Benutzerkonto verfügt über einen Namen und ein Kennwort, die die für die Authentifizierung verwendeten Anmeldeinformationen darstellen.

1. Klicken Sie zum Erstellen eines Benutzerkontos im Navigator des vCenter-Servers auf **Benutzer und Gruppen**.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

  Der Bereich „Benutzer und Gruppen“ wird angezeigt.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. Klicken Sie im vCenter-Bereich „Benutzer und Gruppen“ auf der Registerkarte **Benutzer** auf das Symbol zum Hinzufügen von Benutzern (+-Symbol).

  Das Dialogfeld „Neuer Benutzer“ wird geöffnet.

3. Füllen Sie im Dialogfeld „Neuer Benutzer“ die Felder aus, und klicken Sie auf **OK**. Geben Sie für dieses Beispiel **BackupAdmin** als Benutzername ein. Als Kennwort sollte ein sicheres Kennwort verwendet werden.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

  Das neue Benutzerkonto wird in der Liste angezeigt.

4. Um das Benutzerkonto der Rolle zuzuordnen, klicken Sie im Navigator auf **Globale Berechtigungen**. Klicken Sie im Bereich „Globale Berechtigungen“ auf der Registerkarte **Verwalten** auf das Symbol zum Hinzufügen (+-Symbol).

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

  Das Dialogfeld „Global Permissions Root - Add Permission“ (Stamm: Globale Berechtigungen – Berechtigung hinzufügen) wird geöffnet.

5. Klicken Sie im Dialogfeld **Global Permission Root - Add Permission** (Stamm: Globale Berechtigungen – Berechtigung hinzufügen) auf **Hinzufügen**, um den Benutzer oder die Gruppe auszuwählen.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

  Das Dialogfeld „Select Users/Groups“ (Benutzer/Gruppen auswählen) wird geöffnet.

6. Wählen Sie im Dialogfeld **Select Users/Groups** (Benutzer/Gruppen auswählen) die Option **BackupAdmin** aus, und klicken Sie anschließend auf **Hinzufügen**.

  Das Benutzerkonto im Feld „Benutzer“ hat das Format *Domäne*`\`*Benutzername*. Wenn Sie eine andere Domäne verwenden möchten, wählen Sie sie in der Domänenliste aus.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

  Klicken Sie auf **OK**, um die ausgewählten Benutzer dem Dialogfeld „Berechtigung hinzufügen“ hinzuzufügen.

7. Sie haben den Benutzer bestimmt. Weisen Sie ihn nun der Rolle hinzu. Wählen Sie im Dropdownmenü des Bereichs „Assigned Role“ (Zugewiesene Rolle) die Option **BackupAdminRole** aus, und klicken Sie anschließend auf **OK**.

  ![Dialogfeld „Zertifikat“ mit Fehler ](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  In den globalen Berechtigungen werden auf der Registerkarte „Verwalten“ das neue Benutzerkonto und die zugeordnete Rolle in der Liste angezeigt.


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a>Einrichten von vCenter-Server-Anmeldeinformationen auf dem Azure Backup Server-Computer

Installieren Sie [Update 1 für Microsoft Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server), bevor Sie den VMware-Server zu Azure Backup Server hinzufügen.

1. Doppelklicken Sie zum Öffnen von Azure Backup Server auf das Symbol auf dem Azure Backup Server-Desktop.

  ![Azure Backup Server-Symbol](./media/backup-azure-backup-server-vmware/mabs-icon.png)

  Sollte das Symbol auf dem Desktop nicht angezeigt werden, öffnen Sie Azure Backup Server über die Liste der installierten Apps. Der Name der Azure Backup Server-App lautet Microsoft Azure Backup.

2. Klicken Sie in der Azure Backup Server-Konsole auf **Verwaltung**, dann auf **Produktionsserver** und anschließend auf dem Menüband auf **VMware verwalten**.

  ![MABS-Konsole](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

  Das Dialogfeld „Anmeldeinformationen verwalten“ wird geöffnet.

  ![MABS-Dialogfeld „Anmeldeinformationen verwalten“](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. Klicken Sie im Dialogfeld „Anmeldeinformationen verwalten“ auf **Hinzufügen**, um das Dialogfeld „Anmeldeinformationen hinzufügen“ zu öffnen.

4. Geben Sie im Dialogfeld „Anmeldeinformationen hinzufügen“ einen Namen und eine Beschreibung für die neuen Anmeldeinformationen ein, und geben Sie anschließend den Benutzernamen und das Kennwort an. Anhand des Namens der Anmeldeinformationen (in diesem Beispiel: *Contoso Vcenter credential*) werden die Anmeldeinformationen im folgenden Verfahren identifiziert. Verwenden Sie den Benutzernamen und das Kennwort, die auch für den vCenter-Server verwendet wurden. Falls Sich vCenter-Server und Azure Backup Server nicht in der gleichen Domäne befinden, geben Sie im Benutzernamen die Domäne an.

  ![MABS-Dialogfeld „Anmeldeinformationen verwalten“](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

  Klicken Sie auf **Hinzufügen**, um die neuen Anmeldeinformationen zu Azure Backup Server hinzuzufügen. Die neuen Anmeldeinformationen werden in der Liste des Dialogfelds „Anmeldeinformationen verwalten“ angezeigt.
  ![MABS-Dialogfeld „Anmeldeinformationen verwalten“](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. Klicken Sie in der rechten oberen Ecke auf **X**, um das Dialogfeld „Anmeldeinformationen verwalten“ zu schließen.


## <a name="add-the-vcenter-server-to-azure-backup-server"></a>Hinzufügen des vCenter-Servers zu Azure Backup Server

So öffnen Sie den Assistenten zum Hinzufügen von Produktionsservern:

1. Klicken Sie in der Azure Backup Server-Konsole auf **Verwaltung**, dann auf **Produktionsserver** und anschließend auf **Hinzufügen**.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

  Der Assistent zum Hinzufügen von Produktionsservern wird geöffnet.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. Wählen Sie auf dem Bildschirm „Produktionsservertyp auswählen“ die Option „VMware-Server“ aus, und klicken Sie auf **Weiter**.

3. Geben Sie unter „Servername/IP-Adresse“ den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) oder die IP-Adresse des VMware-Servers ein. Wenn alle ESXi-Server von der gleichen vCenter-Instanz verwaltet werden, können Sie den vCenter-Namen verwenden.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. Geben Sie im Dialogfeld **SSL-Port** den Port ein, der für die Kommunikation mit dem VMware-Server verwendet wird. Verwenden Sie den Standardport 443, es sei denn, Sie wissen, dass ein anderer Port erforderlich ist.

5. Wählen Sie im Dialogfeld **Anmeldeinformationen angeben** die erstellten Anmeldeinformationen aus.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. Klicken Sie auf **Hinzufügen**, um den VMware-Server der Liste **Hinzugefügte VMware-Server** hinzuzufügen, und klicken Sie anschließend auf **Weiter**, um zum nächsten Bildschirm des Assistenten zu gelangen.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. Klicken Sie auf dem Bildschirm **Tasks** auf **Hinzufügen**, um den angegebenen VMware-Server zu Azure Backup Server hinzuzufügen.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  Da die VMware-Serversicherung ohne Agent erfolgt, wird der neue Server in wenigen Sekunden hinzugefügt. Die Ergebnisse werden auf dem Bildschirm „Fertig stellen angezeigt.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/summary-screen.png)

  Wiederholen Sie ggf. die vorherigen Schritte in diesem Abschnitt, um der Azure Backup Server-Instanz mehrere vCenter-Server hinzuzufügen.

Nachdem Sie den vCenter-Server zu Azure Backup Server hinzugefügt haben, erstellen Sie als Nächstes eine Schutzgruppe. Die Schutzgruppe enthält die Details für die kurz- und die langfristige Aufbewahrung. Darüber hinaus legen Sie in der Schutzgruppe die Sicherungsrichtlinie fest und wenden sie an. Die Sicherungsrichtlinie steuert, wann Sicherungen erstellt werden und welche Elemente gesichert werden.


## <a name="configure-a-protection-group"></a>Konfigurieren einer Schutzgruppe

Falls Sie System Center Data Protection Manager oder Azure Backup Server bisher noch nicht verwendet haben, lesen Sie das Thema [Planen von Datenträgersicherungen](https://technet.microsoft.com/library/hh758026.aspx), um Ihre Hardwareumgebung vorzubereiten. Nachdem Sie sich vergewissert haben, dass Sie über genügend Speicher verfügen, fügen Sie mithilfe des Assistenten zum Erstellen einer neuen Schutzgruppe die virtuellen VMware-Computer hinzu.

1. Klicken Sie in der Azure Backup Server-Verwaltungskonsole auf **Schutz** und dann im Menüband des Tools auf **Neu**, um den Assistenten zum Erstellen einer neuen Schutzgruppe zu öffnen.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

  Der Assistent zum Erstellen einer neuen Schutzgruppe wird geöffnet.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/protection-wizard.png)

  Klicken Sie auf **Weiter**, um mit dem Bildschirm **Schutzgruppentyp auswählen** fortzufahren.

2. Wählen Sie auf dem Bildschirm „Schutzgruppentyp auswählen“ die Option **Server** aus, und klicken Sie auf **Weiter**.

3. Auf dem Bildschirm „Gruppenmitglieder auswählen“ werden die verfügbaren Mitglieder und die ausgewählten Mitglieder angezeigt. Wählen Sie die zu schützenden Mitglieder aus, und klicken Sie auf **Weiter**.

  ![Assistent zum Hinzufügen von Produktionsservern](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

  Beachten Sie beim Auswählen eines Mitglieds Folgendes: Wenn Sie einen Ordner auswählen, der weitere Ordner oder virtuelle Computer enthält, werden diese Ordner oder virtuellen Computer ebenfalls ausgewählt. Die Aufnahme der Ordner und virtuellen Computer im übergeordneten Ordner wird als „Schutz auf Ordnerebene“ bezeichnet. Sie können Ordner oder virtuelle Computer ausschließen, indem Sie das Kontrollkästchen deaktivieren.

  Wenn ein virtueller Computer oder ein Ordner mit einem virtuellen Computer bereits in Azure gesichert wird, können Sie diesen virtuellen Computer nicht erneut auswählen. Das bedeutet, dass ein virtueller Computer nicht erneut gesichert werden kann, wenn er in Azure gesichert wird. Dadurch wird die Erstellung doppelter Wiederherstellungspunkte für einen virtuellen Computer verhindert. Um anzuzeigen, welche Azure Backup Server-Instanz bereits ein Mitglied sichert, zeigen Sie mit der Maus auf das Mitglied, um den Namen des Schutzservers anzuzeigen.

4. Geben Sie auf dem Bildschirm „Methode für die Datensicherheit auswählen“ einen Namen für die Schutzgruppe ein. Die Optionen für kurzfristigen Schutz (auf Datenträger) und Onlineschutz sind ausgewählt. Wenn Sie den (Azure-basierten) Onlineschutz verwenden möchten, müssen Sie den kurzfristigen datenträgerbasierten Schutz verwenden. Klicken Sie auf **Weiter**, um zur Dauer für den kurzfristigen Schutz zu wechseln.

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

  Wenn Sie mit der Speicherzuordnung zufrieden sind, klicken Sie auf **Weiter**.

7. Geben Sie auf dem Bildschirm „ Replikaterstellungsmethode auswählen“ an, wie die erste Kopie (Replikat) der geschützten Daten in Azure Backup Server erstellt werden soll.

  Die Standardeinstellungen lauten **Automatisch über das Netzwerk** und **Now** (Jetzt). Bei Verwendung der Standardeinstellungen wird die Angabe einer Nebenzeit empfohlen. Wählen Sie **Später**, und geben Sie Datum und Uhrzeit an.

  Ziehen Sie bei großen Datenmengen oder nicht optimalen Netzwerkbedingungen die Offlinereplikation der Daten mit Wechselmedien in Betracht.

  Nachdem Sie Ihre Auswahl vorgenommen haben, klicken Sie auf **Weiter**.

  ![Assistent zum Erstellen einer neuen Schutzgruppe](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. Legen Sie auf dem Bildschirm **Konsistenzprüfungsoptionen** fest, wie und wann Konsistenzprüfungen automatisiert werden sollen. Sie können Konsistenzprüfungen bei inkonsistenten Replikatdaten oder gemäß einem festgelegten Zeitplan ausführen.

  Wenn Sie keine automatische Konsistenzprüfung konfigurieren möchten, können Sie eine manuelle Überprüfung ausführen. Klicken Sie im Bereich „Schutz“ der Azure Backup Server-Konsole mit der rechten Maustaste auf die Schutzgruppe, und wählen Sie **Konsistenzprüfung ausführen** aus.

  Klicken Sie auf **Weiter**, um zum nächsten Bildschirm zu wechseln.

9. Wählen Sie auf dem Bildschirm **Online zu schützende Daten angeben** die zu schützenden Datenquellen aus. Sie können die Mitglieder einzeln auswählen oder auf **Select All** (Alles markieren) klicken, um alle Mitglieder auszuwählen. Klicken Sie nach dem Auswählen der Mitglieder auf **Weiter**.

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

