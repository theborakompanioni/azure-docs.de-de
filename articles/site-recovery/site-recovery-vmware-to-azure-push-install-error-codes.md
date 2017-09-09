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
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: 1e2452ccc6d3f1859a39e1ee09cdde32f53767ba
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="troubleshooting-mobility-service-push-install-issues"></a>Beheben von Problemen bei Pushinstallationen von Mobility Service

In diesem Artikel werden häufig auftretende Probleme beim Installieren von Mobility Service auf dem Quellserver zum Aktivieren des Schutzes beschrieben.

## <a name="error-code-95107-protection-could-not-be-enabled"></a>(Fehlercode 95107:) Der Schutz konnte nicht aktiviert werden.
**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95107 </br>***Meldung:*** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer mit dem Fehlercode ***EP0858***. <br> Entweder sind die Anmeldeinformationen für das Installieren von Mobility Service falsch, oder das Benutzerkonto verfügt nicht über ausreichende Berechtigungen. | Die auf dem Quellcomputer angegebenen Benutzeranmeldeinformationen zum Installieren von Mobility Service sind falsch. | Stellen Sie sicher, dass die für den Quellcomputer auf dem Konfigurationsserver angegebenen Anmeldeinformationen richtig sind. <br> Hinzufügen/Bearbeiten von Benutzeranmeldeinformationen: Navigieren Sie zu Konfigurationsserver > Symbol „Cspsconfigtool“ > „Konto verwalten“. </br> Stellen Sie darüber hinaus sicher, dass die unten aufgeführten Voraussetzungen erfüllt sind, um die Pushinstallation erfolgreich abzuschließen.

## <a name="error-code-95015-protection-could-not-be-enabled"></a>(Fehlercode 95015:) Der Schutz konnte nicht aktiviert werden.
**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95105 </br>***Meldung:*** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer mit dem Fehlercode ***EP0856***. <br> Entweder ist die Datei und Druckerfreigabe auf dem Quellcomputer nicht zulässig, oder es bestehen Netzwerkverbindungsprobleme zwischen dem Prozessserver und dem Quellcomputer.| Die Datei- und Druckfreigabe ist nicht aktiviert. | Lassen Sie die Datei und Druckerfreigabe auf dem Quellcomputer in der Windows-Firewall zu. Navigieren Sie auf dem Quellcomputer zu den Windows-Firewalleinstellungen, und wählen Sie unter „Eine App oder ein Feature durch die Windows-Firewall zulassen“ die Datei- und Druckerfreigabe für alle Profile aus. </br> Stellen Sie darüber hinaus sicher, dass die unten aufgeführten Voraussetzungen erfüllt sind, um die Pushinstallation erfolgreich abzuschließen.

## <a name="error-code-95117-protection-could-not-be-enabled"></a>(Fehlercode: 95117) Der Schutz konnte nicht aktiviert werden.
**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95117 </br>***Meldung:*** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer mit dem Fehlercode ***EP0865***. <br> Entweder wird der Quellcomputer nicht ausgeführt, oder es bestehen Netzwerkverbindungsprobleme zwischen dem Prozessserver und dem Quellcomputer. | Netzwerkkonnektivität zwischen dem Prozessserver und dem Quellserver | Überprüfen Sie die Konnektivität zwischen dem Prozessserver und dem Quellserver. </br> Stellen Sie darüber hinaus sicher, dass die unten aufgeführten Voraussetzungen erfüllt sind, um die Pushinstallation erfolgreich abzuschließen.

## <a name="error-code-95103-protection-could-not-be-enabled"></a>(Fehlercode: 95103) Der Schutz konnte nicht aktiviert werden.
**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95103 </br>***Meldung:*** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer mit dem Fehlercode ***EP0854***. <br> Entweder ist die Windows-Verwaltungsinstrumentation (WMI) auf dem Quellcomputer nicht zulässig, oder es bestehen Netzwerkverbindungsprobleme zwischen dem Prozessserver und dem Quellcomputer.| Windows-Verwaltungsinstrumentation (WMI) in der Windows-Firewall blockiert | Erlauben Sie die Windows-Verwaltungsinstrumentation (WMI) in der Windows-Firewall. Wählen Sie in den Windows-Firewalleinstellungen unter „Eine App oder ein Feature durch die Windows-Firewall zulassen“ WMI für alle Profile aus. </br> Stellen Sie darüber hinaus sicher, dass die unten aufgeführten Voraussetzungen erfüllt sind, um die Pushinstallation erfolgreich abzuschließen.

## <a name="check-push-install-logs-for-errors"></a>Überprüfen der Pushinstallationsprotokolle auf Fehler

Navigieren Sie auf dem Konfigurations-/Prozessserver unter „<Microsoft Azure Site Recovery Install Location>\home\svsystems\pushinstallsvc\“ zur Datei „PushinstallService“, um die Ursache des Problems zu verstehen. Verwenden Sie zum Beheben des Problems die unten stehenden Schritte.</br>
![pushiinstalllogs](./media/site-recovery-protection-common-errors/pushinstalllogs.png)

## <a name="push-install-pre-requisites-for-windows"></a>Voraussetzungen für die Pushinstallation unter Windows
### <a name="ensure-file-and-printer-sharing-is-enabled"></a>Stellen Sie sicher, dass die Datei- und Druckerfreigabe aktiviert ist.
Konfigurieren Sie die Windows-Firewall auf dem Quellcomputer so, dass die Datei- und Druckerfreigabe und die Windows-Verwaltungsinstrumentation zulässig sind. </br>
#### <a name="if-source-machine-is-domain-joined-br"></a>Wenn der Quellcomputer einer Domäne beigetreten ist: </br>
Konfigurieren Sie die Firewalleinstellungen mithilfe der Gruppenrichtlinien-Verwaltungskonsole (GPMC).
1. Melden Sie als Administrator auf dem Computer in der Active Directory-Domäne an, und öffnen Sie die Gruppenrichtlinien-Verwaltungskonsole (GPMC.MSC, Ausführung über „Start“ > „Ausführen“).</br>
3. Wenn die Gruppenrichtlinien-Verwaltungskonsole nicht installiert ist, verwenden Sie den Link zum [Installieren der Gruppenrichtlinien-Verwaltungskonsole](https://technet.microsoft.com/library/cc725932.aspx). </br>
4. Doppelklicken Sie in der GPMC-Konsolenstruktur auf Gruppenrichtlinienobjekte in der Gesamtstruktur und der Domäne, und navigieren Sie zur „Standarddomänenrichtlinie“. </br>
![gpmc1](./media/site-recovery-protection-common-errors/gpmc1.png) </br>
</br>
5. Klicken Sie mit der rechten Maustaste auf „Standarddomänenrichtlinie“, und wählen Sie „Bearbeiten“ aus. Ein neues Fenster für den Gruppenrichtlinienverwaltungs-Editor wird geöffnet. </br>
![gpmc2](./media/site-recovery-protection-common-errors/gpmc2.png) </br>
</br>
6. Navigieren Sie im Gruppenrichtlinienverwaltungs-Editor zu „Computerkonfiguration“ > „Richtlinien“ > „Administrative Vorlagen“ > „Netzwerk“ > „Netzwerkverbindungen“ > „Windows-Firewall“. </br>
![gpmc3](./media/site-recovery-protection-common-errors/gpmc3.png) </br>
</br>
7. Aktivieren Sie die folgenden Einstellungen für das Domänenprofil und das Standardprofil. </br>
a) Doppelklicken Sie auf die Ausnahme „Windows-Firewall: Eingehende Ausnahme für Datei- und Druckerfreigabe zulassen“. Wählen Sie „Aktiviert“ aus, und klicken Sie auf „OK“. </br>
b) Doppelklicken Sie auf die Ausnahme „Windows-Firewall: Eingehende Remoteverwaltungsausnahme zulassen“. Wählen Sie „Aktiviert“ aus, und klicken Sie auf „OK“. </br>
![gpmc4](./media/site-recovery-protection-common-errors/gpmc4.png) </br>
</br>

###### <a name="if-source-machine-is-not-domain-joined-and-part-of-workgroup-br"></a>Wenn der Quellcomputer keiner Domäne oder Arbeitsgruppe angehört </br>
Konfigurieren Sie Firewalleinstellungen auf dem Remotecomputer (für die Arbeitsgruppe):
1. Wechseln Sie zum Quellcomputer.</br>
2. Wählen Sie in den Windows-Firewalleinstellungen unter „Eine App oder ein Feature durch die Windows-Firewall zulassen“ Datei- und Druckerfreigabe für alle Profile aus. </br>
3. Wählen Sie in den Windows-Firewalleinstellungen unter „Eine App oder ein Feature durch die Windows-Firewall zulassen“ WMI für alle Profile aus. </br>

#### <a name="disable-remote-user-account-control-uac"></a>Deaktivieren der Remote-Benutzerkontensteuerung (UAC)
Deaktivieren Sie die Benutzerkontensteuerung mithilfe eines Registrierungsschlüssels zur Aktivierung von Mobility Service.
1. Klicken Sie auf „Start“ und anschließend auf „Ausführen“, geben Sie „regedit“ ein, und drücken Sie dann die EINGABETASTE.
2. Suchen Sie den folgenden Registrierungsunterschlüssel, und klicken Sie darauf: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System.
3. Wenn der Registrierungseintrag „LocalAccountTokenFilterPolicy“ nicht vorhanden ist, gehen Sie folgendermaßen vor:
4. Klicken Sie im Menü „Bearbeiten“ auf „Neu“ und dann auf „DWORD-Wert“.
5. Geben Sie „LocalAccountTokenFilterPolicy“ ein, und drücken Sie dann die EINGABETASTE.
6. Klicken Sie mit der rechten Maustaste auf „LocalAccountTokenFilterPolicy“, und klicken Sie dann auf „Ändern“.
7. Geben Sie im Datenfeld „Wert“ den Wert „1“ ein, und klicken Sie dann auf „OK“.
8. Beenden Sie den Registrierungs-Editor.


## <a name="push-install-pre-requisites-for-linux"></a>Voraussetzungen für die Pushinstallation unter Linux

1. Erstellen Sie auf dem Linux-Quellserver einen Root-Benutzer. (Verwenden Sie dieses Konto nur für die Pushinstallation und für Updates.)</br>
2. Vergewissern Sie sich, dass die Datei „/etc/hosts“ auf dem Linux-Quellserver Einträge enthält, mit denen die Namen des lokalen Hosts den IP-Adressen zugeordnet werden, die allen Netzwerkkarten zugeordnet sind. </br>
3. Stellen Sie sicher, dass die neuesten Versionen der Pakete openssh, openssh-server und openssl auf dem Linux-Quellserver installiert sind. </br>
Überprüfen Sie, ob der SSH-Port 22 aktiviert ist und ausgeführt wird. </br>
4. Überprüfen Sie, ob auf dem Quellserver bereits veraltete Einträge des Agents vorhanden sind, deinstallieren Sie die älteren Agents, starten Sie den Server neu, und installieren Sie den Agent erneut. </br>

#### <a name="enable-sftp-subsystem-and-password-authentication-in-the-sshdconfig-file"></a>Aktivieren des SFTP-Subsystems und der Kennwortauthentifizierung in der Datei „sshd_config“
1. Melden Sie sich auf dem Quellserver als Root an. </br>
2. Suchen Sie in der Datei „/etc/ssh/sshd_config“</br> nach der Zeile, die mit „PasswordAuthentication“ beginnt. </br>
3. d.   Heben Sie die Auskommentierung der Zeile auf, und ändern Sie den Wert von „no“ in „yes“. </br>
![Linux1](./media/site-recovery-protection-common-errors/linux1.png)
4. Suchen Sie die Zeile, die mit „Subsystem“ beginnt, und heben Sie die Auskommentierung der Zeile auf. </br>
![Linux2](./media/site-recovery-protection-common-errors/linux2.png)
5. Speichern Sie die Änderungen, und starten Sie den sshd-Dienst neu. </br>

## <a name="push-installation-checks-on-configurationprocess-server"></a>Überprüfen der Pushinstallation auf dem Konfigurations-/Prozessserver
#### <a name="validate-credentials-for-discovery-and-installation"></a>Überprüfen der Anmeldeinformationen für Ermittlung und Installation

1. Starten Sie auf dem Konfigurationsserver Cspsconfigtool.</br>
![WMItestConnect5](./media/site-recovery-protection-common-errors/wmitestconnect5.png) </br>

2. Stellen Sie sicher, dass das für den Schutz verwendete Konto auf dem Quellcomputer über Administratorrechte verfügt. </br>

#### <a name="check-connectivity-between-process-server-and-source-server"></a>Überprüfen Sie die Konnektivität zwischen dem Prozessserver und dem Quellserver.
1. Vergewissern Sie sich, dass der Prozessserver über eine Internetverbindung verfügt.
2. Überprüfen Sie die WMI-Verbindung mit „wbemtest.exe“. </br>
Klicken Sie auf dem Prozessserver auf „Start“ > „Ausführen“ > „wbemtest.exe“. Das Fenster für das Testprogramm der Windows-Verwaltungsinstrumentation wird wie dargestellt geöffnet.</br>
   ![WMItestConnect1](./media/site-recovery-protection-common-errors/wmitestconnect1.png) </br>
   </br>
Klicken Sie auf „Verbinden“, und geben Sie die IP-Adresse des Quellservers in das Eingabefeld „Namespace“ ein. (Wenn der Quellcomputer in eine Domäne eingebunden ist, geben Sie den Domänennamen zusammen mit dem Benutzernamen als „Domänenname\Benutzername“ ein. Wenn der Quellcomputer einer Arbeitsgruppe angehört, geben Sie nur den Benutzernamen an). Wählen Sie als „Authentifizierungsebene“ die Option „Paketdatenschutz“ aus. </br>
![WMItestConnect2](./media/site-recovery-protection-common-errors/wmitestconnect2.png) </br>
   </br>
   Klicken Sie auf „Verbinden“. Die WMI-Verbindung sollte nun mit den bereitgestellten Daten erfolgreich hergestellt werden können, und das Fenster für das Testprogramm der Windows-Verwaltungsinstrumentation sollte wie unten dargestellt angezeigt werden: </br>
   ![WMItestConnect3](./media/site-recovery-protection-common-errors/wmitestconnect3.png) </br>
</br>
   Wenn keine WMI-Verbindung hergestellt werden kann, wird ein Popupfenster mit einer Fehlermeldung angezeigt. Der nachfolgende Screenshot zeigt einen fehlerhaften Versuch, bei dem die WMI/Remoteverwaltung in der Windows-Firewall nicht als App zugelassen wurde. </br>
   ![WMItestConnect4](./media/site-recovery-protection-common-errors/wmitestconnect4.png) </br>
</br>

3. Überprüfen Sie den WMI-Status und die Konnektivität.</br>
Auf dem Konfigurations-/Prozessserver: </br>
Klicken Sie auf „Start“ > „Ausführen“ > „wmimgmt.msc“ > „Aktionen“ > „Weitere Aktionen“ > „Mit einem anderen Computer verbinden“ (Quellcomputer). </br>
Geben Sie die Anmeldeinformationen des Kontos ein, das für den Schutz verwendet wird, und überprüfen Sie die Konnektivität. </br>

#### <a name="verify-network-shared-folders-of-source-machine-is-accessible-from-process-server-ps-remotely-using-specified-credentials"></a>Überprüfen Sie, ob mithilfe der angegebenen Anmeldeinformationen vom Prozessserver aus auf die freigegebenen Netzwerkordner auf dem Quellcomputer zugegriffen werden kann.
  1. Melden Sie sich Prozessserver (PS) an, öffnen Sie den Datei-Explorer, geben Sie auf der Adressleiste „\\\Quellcomputer-IP\C$“ ein, und drücken Sie die EINGABETASTE. </br>
  ![Fileshare1](./media/site-recovery-protection-common-errors/fileshare1.png) </br>
  2. Sie werden im Datei-Explorer zur Eingabe von Anmeldeinformationen aufgefordert. Geben Sie den Benutzernamen und das Kennwort ein, und klicken Sie auf „OK“.</br>
   Wenn der Quellcomputer in eine Domäne eingebunden ist, geben Sie den Domänennamen zusammen mit dem Benutzernamen als „Domänenname\Benutzername“ ein.</br>
   Wenn der Quellcomputer einer Arbeitsgruppe angehört, geben Sie nur den Benutzernamen an. </br>
  ![Fileshare2](./media/site-recovery-protection-common-errors/fileshare2.png) </br>
  3. Wenn die Verbindung erfolgreich ist, können Sie die Ordner des Quellcomputers remote vom Prozessserver (PS) aus anzeigen. </br>
  ![Fileshare3](./media/site-recovery-protection-common-errors/fileshare3.png) </br>

> [!NOTE] 
> Wenn die Verbindung nicht erfolgreich ist, überprüfen Sie, ob alle Voraussetzungen erfüllt sind.
>

Wenn Sie die Windows-Verwaltungsinstrumentation nicht öffnen möchten, können Sie Mobility Service auch manuell auf dem Quellcomputer installieren.</br> [Manuelles Installieren des Mobility Service über die grafische Benutzeroberfläche](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) </br>
[Installation mithilfe von Configuration Manager](site-recovery-install-mobility-service-using-sccm.md) </br>

## <a name="next-steps"></a>Nächste Schritte
- [Aktivieren der Replikation für virtuelle VMware-VMs](vmware-walkthrough-enable-replication.md)

