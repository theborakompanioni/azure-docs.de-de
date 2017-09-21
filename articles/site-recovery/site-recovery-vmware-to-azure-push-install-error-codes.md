---
title: "Azure Site Recovery – Problembehandlung für „VMware zu Azure“ | Microsoft-Dokumentation"
description: Beheben von Fehlern beim Replizieren von virtuellen Azure-Computern
services: site-recovery
documentationcenter: 
author: asgang
manager: srinathv
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/24/2017
ms.author: asgang
ms.translationtype: HT
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: d4d6d273001642ebb8c960333a1c01977e863dbc
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Beheben von Problemen bei Pushinstallationen von Mobility Service

In diesem Artikel werden häufige Probleme beschrieben, die beim Installieren von Azure Site Recovery Mobility Service auf dem Quellserver zum Aktivieren des Schutzes auftreten können.

## <a name="error-code-95107-protection-could-not-be-enabled"></a>(Fehlercode 95107) Der Schutz konnte nicht aktiviert werden.
**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95107 </br>**Meldung:** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer mit dem Fehlercode **EP0858**. <br> Entweder sind die Anmeldeinformationen für das Installieren von Mobility Service falsch, oder das Benutzerkonto verfügt nicht über ausreichende Berechtigungen. | Die auf dem Quellcomputer angegebenen Benutzeranmeldeinformationen zum Installieren von Mobility Service sind falsch. | Stellen Sie sicher, dass die für den Quellcomputer auf dem Konfigurationsserver angegebenen Anmeldeinformationen richtig sind. <br> Navigieren Sie zum Hinzufügen oder Bearbeiten von Benutzeranmeldeinformationen zum Konfigurationsserver, und wählen Sie **Cspsconfigtool** > **Konto verwalten** aus. </br> Überprüfen Sie außerdem, ob die unten aufgeführten Voraussetzungen erfüllt sind, um die Pushinstallation erfolgreich abzuschließen.

## <a name="error-code-95015-protection-could-not-be-enabled"></a>(Fehlercode 95015) Der Schutz konnte nicht aktiviert werden.
**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95105 </br>**Meldung:** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer mit dem Fehlercode **EP0856**. <br> Entweder ist die **Datei- und Druckerfreigabe** auf dem Quellcomputer nicht zulässig, oder es bestehen Netzwerkverbindungsprobleme zwischen dem Prozessserver und dem Quellcomputer.| Die **Datei- und Druckerfreigabe** ist nicht aktiviert. | Lassen Sie die **Datei- und Druckerfreigabe** auf dem Quellcomputer in der Windows-Firewall zu. Wählen Sie auf dem Quellcomputer unter **Windows-Firewall** > **Eine App oder ein Feature durch die Windows-Firewall zulassen** die Option **Datei- und Druckerfreigabe für alle Profile** aus. </br> Überprüfen Sie außerdem, ob die unten aufgeführten Voraussetzungen erfüllt sind, um die Pushinstallation erfolgreich abzuschließen.

## <a name="error-code-95117-protection-could-not-be-enabled"></a>(Fehlercode 95117) Der Schutz konnte nicht aktiviert werden.
**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95117 </br>**Meldung:** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer mit dem Fehlercode **EP0865**. <br> Entweder wird der Quellcomputer nicht ausgeführt, oder es bestehen Netzwerkverbindungsprobleme zwischen dem Prozessserver und dem Quellcomputer. | Netzwerkverbindungsprobleme zwischen dem Prozessserver und dem Quellserver. | Überprüfen Sie die Verbindung zwischen dem Prozessserver und dem Quellserver. </br> Überprüfen Sie außerdem, ob die unten aufgeführten Voraussetzungen erfüllt sind, um die Pushinstallation erfolgreich abzuschließen.

## <a name="error-code-95103-protection-could-not-be-enabled"></a>(Fehlercode 95103) Der Schutz konnte nicht aktiviert werden.
**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95103 </br>**Meldung:** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer mit dem Fehlercode **EP0854**. <br> Entweder ist die Windows-Verwaltungsinstrumentation (WMI) auf dem Quellcomputer nicht zulässig, oder es bestehen Netzwerkverbindungsprobleme zwischen dem Prozessserver und dem Quellcomputer.| WMI ist in der Windows-Firewall blockiert. | Lassen Sie WMI in der Windows-Firewall zu. Wählen Sie unter **Windows-Firewall** > **Eine App oder ein Feature durch die Windows-Firewall zulassen** die Option **WMI für alle Profile** aus. </br> Überprüfen Sie außerdem, ob die unten aufgeführten Voraussetzungen erfüllt sind, um die Pushinstallation erfolgreich abzuschließen.

## <a name="check-push-installation-logs-for-errors"></a>Überprüfen der Pushinstallationsprotokolle auf Fehler

Navigieren Sie auf dem Konfigurations-/Prozessserver unter „<Microsoft Azure Site Recovery Install Location>\home\svsystems\pushinstallsvc\“ zur Datei „PushinstallService“, um die Ursache des Problems zu verstehen. Führen Sie zum Beheben des Problems die unten stehenden Schritte aus.</br>

![Pushinstallationsprotokolle](./media/site-recovery-protection-common-errors/pushinstalllogs.png)

## <a name="push-installation-prerequisites-for-windows"></a>Voraussetzungen für die Pushinstallation unter Windows
### <a name="ensure-that-file-and-printer-sharing-is-enabled"></a>Sicherstellen, dass die **Datei- und Druckerfreigabe** aktiviert ist
Lassen Sie die **Datei- und Druckerfreigabe** und die **Windows-Verwaltungsinstrumentation** auf dem Quellcomputer in der Windows-Firewall zu. </br>
#### <a name="if-the-source-machine-is-domain-joined-br"></a>Wenn der Quellcomputer in die Domäne eingebunden ist </br>
Konfigurieren Sie die Firewalleinstellungen mithilfe der Gruppenrichtlinien-Verwaltungskonsole.

1. Melden Sie sich als Administrator auf dem Computer in der Azure Active Directory-Domäne an. Öffnen Sie die Gruppenrichtlinien-Verwaltungskonsole (GPMC.MSC, Ausführung über **Start** > **Ausführen**).</br>

2. Wenn die Gruppenrichtlinien-Verwaltungskonsole nicht installiert ist, lesen Sie unter [Installieren der Gruppenrichtlinien-Verwaltungskonsole](https://technet.microsoft.com/library/cc725932.aspx) nach. </br>

3. Doppelklicken Sie in der Struktur der Gruppenrichtlinien-Verwaltungskonsole in der Gesamtstruktur und der Domäne auf **Gruppenrichtlinienobjekte**. Navigieren Sie zu **Standarddomänenrichtlinie**. </br>

    ![Standarddomänenrichtlinie](./media/site-recovery-protection-common-errors/gpmc1.png) </br>
</br>
4. Klicken Sie mit der rechten Maustaste auf **Standarddomänenrichtlinie** > **Bearbeiten**. Ein neues Fenster für den **Gruppenrichtlinienverwaltungs-Editor** wird geöffnet. </br>

    ![Gruppenrichtlinienverwaltungs-Editor](./media/site-recovery-protection-common-errors/gpmc2.png) </br>
</br>
5. Navigieren Sie im **Gruppenrichtlinienverwaltungs-Editor** zu **Computerkonfiguration** > **Richtlinien** > **Administrative Vorlagen** > **Netzwerk** > **Netzwerkverbindungen** > **Windows-Firewall**. </br>

    ![Windows-Firewall](./media/site-recovery-protection-common-errors/gpmc3.png) </br>
</br>
6. Aktivieren Sie die folgenden Einstellungen für **Domänenprofil** und **Standardprofil**: </br>

    a. Doppelklicken Sie auf **Windows-Firewall: Eingehende Ausnahme für Datei- und Druckerfreigabe zulassen**. Wählen Sie **Aktiviert** und dann **OK** aus. </br>

    b. Doppelklicken Sie auf **Windows-Firewall: Eingehende Remoteverwaltungsausnahme zulassen**. Wählen Sie **Aktiviert** und dann **OK** aus. </br>

    ![Domänenprofil](./media/site-recovery-protection-common-errors/gpmc4.png) </br>
</br>

#### <a name="if-the-source-machine-isnt-domain-joined-and-part-of-a-workgroup-br"></a>Wenn der Quellcomputer keiner Domäne oder Arbeitsgruppe angehört </br>
Konfigurieren Sie die Firewalleinstellungen auf einem Remotecomputer (für eine Arbeitsgruppe).

1. Wechseln Sie zum Quellcomputer.</br>

2. Wählen Sie unter **Windows-Firewall** > **Eine App oder ein Feature durch die Windows-Firewall zulassen** für alle Profile die Option **Datei- und Druckerfreigabe** aus. </br>

3. Wählen Sie unter **Windows-Firewall** > **Eine App oder ein Feature durch die Windows-Firewall zulassen** für alle Profile die Option **WMI** aus. </br>

#### <a name="disable-remote-user-account-control"></a>Deaktivieren der Remote-Benutzerkontensteuerung
Deaktivieren Sie die Benutzerkontensteuerung mithilfe des Registrierungsschlüssels zur Aktivierung von Mobility Service.

1. Wählen Sie **Start** > **Ausführen** aus, geben Sie *regedit* ein, und drücken Sie dann die **EINGABETASTE**.

2. Suchen Sie den folgenden Registrierungsunterschlüssel, und wählen Sie ihn aus: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System.

3. Wenn der Registrierungseintrag „LocalAccountTokenFilterPolicy“ nicht vorhanden ist, gehen Sie folgendermaßen vor:

    a. Wählen Sie im Menü **Bearbeiten** die Option **Neu** und dann **DWORD-Wert** aus.

    b. Geben Sie *LocalAccountTokenFilterPolicy* ein, und drücken Sie dann die **EINGABETASTE**.

    c. Klicken Sie mit der rechten Maustaste auf **LocalAccountTokenFilterPolicy**, und wählen Sie dann **Ändern** aus.

    d. Geben Sie im Datenfeld **Wert** den Wert *1* ein, und wählen Sie dann **OK** aus.

4. Schließen Sie den Registrierungs-Editor.


## <a name="push-installation-prerequisites-for-linux"></a>Voraussetzungen für die Pushinstallation unter Linux

1. Erstellen Sie auf dem Linux-Quellserver einen Root-Benutzer. (Verwenden Sie dieses Konto nur für die Pushinstallation und für Updates.)</br>

2. Vergewissern Sie sich, dass die Datei „/etc/hosts“ auf dem Linux-Quellserver Einträge enthält, mit denen die Namen des lokalen Hosts den IP-Adressen zugeordnet werden, die allen Netzwerkkarten zugeordnet sind. </br>

3. Stellen Sie sicher, dass die neuesten Versionen der Pakete openssh, openssh-server und openssl auf dem Linux-Quellserver installiert sind. Überprüfen Sie, ob der SSH-Port 22 aktiviert ist und ausgeführt wird. </br>

4. Überprüfen Sie, ob auf dem Quellserver bereits veraltete Einträge des Agents vorhanden sind, deinstallieren Sie die älteren Agents, und starten Sie den Server neu. Installieren Sie den Agent erneut. </br>

#### <a name="enable-sftp-subsystem-and-password-authentication-in-the-sshdconfig-file"></a>Aktivieren des SFTP-Subsystems und der Kennwortauthentifizierung in der Datei „sshd_config“
1. Melden Sie sich auf dem Quellserver als Root an. </br>

2. Suchen Sie in der Datei „/etc/ssh/sshd_config“ nach der Zeile, die mit „PasswordAuthentication“ beginnt. </br>

3. Heben Sie die Auskommentierung der Zeile auf, und ändern Sie den Wert von „no“ in „yes“. </br>

    ![PasswordAuthentication](./media/site-recovery-protection-common-errors/linux1.png)

4. Suchen Sie die Zeile, die mit „Subsystem“ beginnt, und heben Sie die Auskommentierung der Zeile auf. </br>

    ![Subsystem](./media/site-recovery-protection-common-errors/linux2.png)

5. Speichern Sie die Änderungen, und starten Sie den sshd-Dienst neu. </br>


## <a name="push-installation-checks-on-the-configurationprocess-server"></a>Überprüfen der Pushinstallation auf dem Konfigurations-/Prozessserver
#### <a name="validate-credentials-for-discovery-and-installation"></a>Überprüfen der Anmeldeinformationen für Ermittlung und Installation

1. Starten Sie Cspsconfigtool auf dem Konfigurationsserver.</br>

    ![WMItestConnect5](./media/site-recovery-protection-common-errors/wmitestconnect5.png) </br>

2. Stellen Sie sicher, dass das für den Schutz verwendete Konto auf dem Quellcomputer über Administratorrechte verfügt. </br>

#### <a name="check-connectivity-between-the-process-server-and-the-source-server"></a>Überprüfen der Verbindung zwischen dem Prozessserver und dem Quellserver
1. Vergewissern Sie sich, dass der Prozessserver über eine Internetverbindung verfügt.

2. Überprüfen Sie die WMI-Verbindung mit „wbemtest.exe“. </br>

    a. Wählen Sie auf dem Prozessserver **Start** > **Ausführen** > **wbemtest.exe** aus. Das Fenster für das **Testprogramm der Windows-Verwaltungsinstrumentation** wird wie unten dargestellt geöffnet:</br>

      ![WMI-Testprogramm](./media/site-recovery-protection-common-errors/wmitestconnect1.png) </br>
   </br>
    b. Wählen Sie **Verbinden** aus, und geben Sie im Feld **Namespace** die IP-Adresse des Quellservers ein.

    c. Geben Sie den **Benutzernamen** und das **Kennwort** ein. (Wenn der Quellcomputer in eine Domäne eingebunden ist, geben Sie den Domänennamen zusammen mit dem Benutzernamen als *Domänenname\Benutzername* an. Wenn der Quellcomputer einer Arbeitsgruppe angehört, geben Sie nur den Benutzernamen an.)

    d. Wählen Sie als **Authentifizierungsebene** die Option **Paketdatenschutz** aus. </br>

      ![Paketdatenschutz](./media/site-recovery-protection-common-errors/wmitestconnect2.png) </br>
   </br>
    e. Wählen Sie **Verbinden**aus. Die WMI-Verbindung wird nun mit den bereitgestellten Daten angezeigt, und das Fenster für das **Testprogramm der Windows-Verwaltungsinstrumentation** wird wie unten dargestellt angezeigt: </br>

      ![Daten im WMI-Testprogramm](./media/site-recovery-protection-common-errors/wmitestconnect3.png) </br>
   </br>
      Wenn keine WMI-Verbindung hergestellt werden kann, wird eine Fehlermeldung angezeigt. Der nachfolgende Screenshot zeigt einen fehlerhaften Versuch, bei dem die **WMI/Remoteverwaltung** in der durch die Windows-Firewall zugelassenen App nicht aktiviert wurde: </br>

    ![Fehlermeldung im WMI-Testprogramm](./media/site-recovery-protection-common-errors/wmitestconnect4.png) </br>
   </br>

3. Überprüfen Sie den WMI-Status und die Konnektivität.</br>

    a. Wählen Sie auf dem Konfigurations-/Prozessserver **Start** > **Ausführen** > **wmimgmt.msc** > **Aktionen** > **Weitere Aktionen** > **Mit einem anderen Computer verbinden** (Quellcomputer) aus. </br>

    b. Geben Sie die Anmeldeinformationen des Kontos ein, das für den Schutz verwendet wird, und überprüfen Sie die Konnektivität. </br>

#### <a name="verify-that-network-shared-folders-of-the-source-machine-are-accessible-from-the-process-server-remotely-by-using-specified-credentials"></a>Überprüfen, ob mithilfe der angegebenen Anmeldeinformationen vom Prozessserver aus per Remotezugriff auf die freigegebenen Netzwerkordner auf dem Quellcomputer zugegriffen werden kann

  1. Melden Sie sich beim Prozessservercomputer an, und öffnen Sie den Datei-Explorer. Geben Sie *\\\source-machine-ip\C$* in der Adressleiste ein. Drücken Sie die **EINGABETASTE**. </br>

      ![Datei-Explorer](./media/site-recovery-protection-common-errors/fileshare1.png) </br>

  2. Sie werden im Datei-Explorer zur Eingabe von Anmeldeinformationen aufgefordert. Geben Sie den Benutzernamen und das Kennwort ein, und wählen Sie **OK** aus.</br>

      * Wenn der Quellcomputer in eine Domäne eingebunden ist, geben Sie den Domänennamen zusammen mit dem Benutzernamen als *Domänenname\Benutzername* ein.</br>

      * Wenn der Quellcomputer einer Arbeitsgruppe angehört, geben Sie nur den Benutzernamen ein. </br>

      ![Dialogfeld für Anmeldeinformationen](./media/site-recovery-protection-common-errors/fileshare2.png) </br>

  3. Wenn die Verbindung erfolgreich hergestellt wird, können Sie die Ordner des Quellcomputers remote vom Prozessserver aus anzeigen. </br>

      ![Ordneransicht](./media/site-recovery-protection-common-errors/fileshare3.png) </br>

> [!NOTE] 
> Wenn keine Verbindung hergestellt werden kann, überprüfen Sie, ob alle Voraussetzungen erfüllt sind.
>

Wenn Sie die **Windows-Verwaltungsinstrumentation** nicht öffnen möchten, können Sie Mobility Service auch manuell auf dem Quellcomputer installieren.</br> 

Weitere Informationen finden Sie unter [Manuelles Installieren von Mobility Service über die grafische Benutzeroberfläche (GUI)](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) und [Installation mithilfe von Configuration Manager](site-recovery-install-mobility-service-using-sccm.md). </br>

## <a name="next-steps"></a>Nächste Schritte
- [Aktivieren der Replikation für virtuelle VMware-VMs](vmware-walkthrough-enable-replication.md)

