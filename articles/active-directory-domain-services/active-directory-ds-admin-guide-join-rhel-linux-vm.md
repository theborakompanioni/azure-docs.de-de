<properties
	pageTitle="Azure Active Directory-Domänendienste (Vorschau): Administratorhandbuch | Microsoft Azure"
	description="Einbinden eines virtuellen Red Hat Enterprise Linux-Computers in Azure AD-Domänendienste"
	services="active-directory-ds"
	documentationCenter=""
	authors="mahesh-unnikrishnan"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory-ds"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/31/2016"
	ms.author="maheshu"/>

# Einbinden eines virtuellen Red Hat Enterprise Linux 7-Computers in eine verwaltete Domäne
Dieser Artikel zeigt, wie ein virtueller Red Hat Enterprise Linux 7-Computer (RHEL) einer durch Azure AD-Domänendienste verwalteten Domäne beitritt.

## Bereitstellen eines virtuellen Red Hat Enterprise Linux-Computers
Führen Sie die folgenden Schritte aus, um einen virtuellen RHEL 7-Computer über das Azure-Portal bereitzustellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

    ![Dashboard des Azure-Portals](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)

2. Klicken Sie im linken Bereich auf **Neu**, und geben Sie **Red Hat** in die Suchleiste ein, wie im folgenden Screenshot gezeigt. In den Suchergebnissen werden Einträge für Red Hat Enterprise Linux angezeigt. Klicken Sie auf **Red Hat Enterprise Linux 7.2**.

    ![RHEL in den Ergebnissen auswählen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)

3. In den Suchergebnissen im Bereich **Alles** sollte das Image von Red Hat Enterprise Linux 7.2 angezeigt werden. Klicken Sie auf **Red Hat Enterprise Linux 7.2**, um weitere Informationen zum Image des virtuellen Computers anzuzeigen.

    ![RHEL in den Ergebnissen auswählen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)

4. Im Bereich **Red Hat Enterprise Linux 7.2** sollten weitere Informationen zum Image des virtuellen Computers angezeigt werden. Wählen Sie in der Dropdownliste **Bereitstellungsmodell auswählen** den Eintrag **Klassisch** aus. Klicken Sie dann auf die Schaltfläche **Erstellen**.

    ![Imagedetails anzeigen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)

5. Geben Sie im Bereich **Virtuellen Computer erstellen** den **Hostnamen** für den neuen virtuellen Computer ein. Geben Sie auch den Benutzernamen eines lokalen Administrators in das Feld **Benutzername** sowie ein **Kennwort** ein. Sie können auch einen SSH-Schlüssel zur Authentifizierung des lokalen Administratorbenutzers verwenden. Wählen Sie auch einen **Tarif** für den virtuellen Computer aus.

    ![Virtuellen Computer erstellen – grundlegende Informationen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)

6. Klicken Sie auf **Optionale Konfiguration**. Daraufhin sollte der Bereich **Optionale Konfiguration** geöffnet werden. Klicken Sie im Bereich **Optionale Konfiguration** auf **OK**.

    ![Virtuellen Computer erstellen – virtuelles Netzwerk konfigurieren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-configure-vnet.png)

7. Daraufhin wird der Bereich **Netzwerk** angezeigt. Klicken Sie im Bereich **Netzwerk** auf **Virtuelles Netzwerk**, um das virtuelle Netzwerk auszuwählen, in dem der virtuelle Linux-Computer bereitgestellt werden soll. Daraufhin wird der Bereich **Virtuelles Netzwerk** geöffnet. Wählen Sie im Bereich **Virtuelles Netzwerk** die Option **Vorhandenes virtuelles Netzwerk verwenden** aus. Wählen Sie dann das virtuelle Netzwerk aus, in dem die Azure AD-Domänendienste verfügbar sind. In diesem Beispiel wählen wir das virtuelle Netzwerk „MyPreviewVNet“.

    ![Virtuellen Computer erstellen – virtuelles Netzwerk auswählen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)

8. Klicken Sie im Bereich **Optionale Konfiguration** auf die Schaltfläche **OK**.

    ![Virtuellen Computer erstellen – ausgewähltes virtuelles Netzwerk](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)

9. Sie können den virtuellen Computer nun erstellen. Klicken Sie im Bereich **Virtuellen Computer erstellen** auf die Schaltfläche **Erstellen**.

    ![Virtuellen Computer erstellen – bereit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm.png)

10. Die Bereitstellung des neuen, auf dem RHEL 7.2-Image basierenden virtuellen Computers sollte beginnen.

  ![Virtuellen Computer erstellen – Bereitstellung gestartet](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)

11. Nach einigen Minuten sollte der virtuelle Computer erfolgreich bereitgestellt und einsatzbereit sein.

  ![Virtuellen Computer erstellen – bereitgestellt](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)



## Herstellen einer Remoteverbindung mit dem neu bereitgestellten virtuellen Linux-Computer
Der virtuelle RHEL 7.2-Computer wurde in Azure bereitgestellt. Die nächste Aufgabe besteht darin, eine Remoteverbindung mit dem virtuellen Computer herzustellen.

**Herstellen einer Verbindung mit dem virtuellen RHEL 7.2-Computer:** Folgen Sie den Anweisungen im Artikel [Erstellen von SSH-Schlüsseln unter Linux und Mac für Linux-VMs in Azure](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

Bei den weiteren Schritten wird vorausgesetzt, dass Sie PuTTY als SSH-Client verwenden, um eine Verbindung mit dem virtuellen RHEL-Computer herzustellen. Weitere Informationen finden Sie auf der [PuTTY-Downloadseite](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Öffnen Sie das PuTTY-Programm.

2. Geben Sie den **Hostnamen** für den neu erstellten virtuellen RHEL-Computer ein. In diesem Beispiel weist der virtuelle Computer den Hostnamen „contoso-rhel.cloudapp.net“ auf. Wenn Sie nicht wissen, wie der Hostname Ihres virtuellen Computers lautet, finden Sie in auf dem VM-Dashboard im Azure-Portal.

    ![PuTTY-Verbindung](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)

3. Melden Sie sich mit den Anmeldeinformationen des lokalen Administrators, die Sie beim Erstellen des virtuellen Computers angegeben haben, beim virtuellen Computer an. In diesem Beispiel wird das lokale Administratorkonto „mahesh“ verwendet.

    ![PuTTY-Anmeldung](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)


## Installieren der erforderlichen Pakete auf dem virtuellen Linux-Computer
Wenn die Verbindung mit dem virtuellen Computer hergestellt wurde, besteht die nächste Aufgabe darin, die erforderlichen Pakete für den Domänenbeitritt des virtuellen Computers zu installieren. Führen Sie die folgenden Schritte aus:

1. **Installieren von realmd**: Das realmd-Paket wird für den Domänenbeitritt verwendet. Geben Sie in Ihrem PuTTY-Terminal folgenden Befehl ein.

    sudo yum install realmd

    ![realmd installieren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Nach einigen Minuten sollte das realmd-Paket auf dem virtuellen Computer installiert werden.

    ![realmd installiert](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)

3. **Installieren von sssd:** Das realmd-Paket benötigt sssd, um Vorgänge für den Domänenbeitritt auszuführen. Geben Sie in Ihrem PuTTY-Terminal folgenden Befehl ein.

    sudo yum install sssd

	![sssd installieren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Nach einigen Minuten sollte das sssd-Paket auf dem virtuellen Computer installiert werden.

    ![realmd installiert](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)

4. **Installieren von Kerberos**: Geben Sie in Ihrem PuTTY-Terminal folgenden Befehl ein.

    sudo yum install krb5-workstation krb5-libs

	![Kerberos installieren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

	Nach einigen Minuten sollte das realmd-Paket auf dem virtuellen Computer installiert werden.

	![Kerberos installiert](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)


## Einbinden der virtuellen Linux-Computers in die verwaltete Domäne
Nachdem die erforderlichen Pakete auf dem virtuellen Linux-Computer installiert sind, besteht die nächste Aufgabe darin, den virtuellen Computer in die verwaltete Domäne einzubinden.

1. Ermitteln Sie die durch Azure AD-Domänendienste verwaltete Domäne. Geben Sie in Ihrem PuTTY-Terminal folgenden Befehl ein.

    sudo realm discover CONTOSO100.COM

	![realm discover](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

	Wenn Ihre verwaltete Domäne durch **realm discover** nicht gefunden wird, stellen Sie (z. B. per Ping) sicher, dass der virtuelle Computer die Domäne erreichen kann. Stellen Sie auch sicher, dass der virtuelle Computer wirklich in dem virtuellen Netzwerk bereitgestellt wurde, in dem die verwaltete Domäne verfügbar ist.

2. Initialisieren Sie Kerberos. Geben Sie in Ihrem PuTTY-Terminal folgenden Befehl ein. Stellen Sie sicher, dass Sie einen Benutzer angeben, der zur Administratorengruppe für Azure AD-Domänencontroller gehört. Nur diese Benutzer können Computer in die verwaltete Domäne einbinden.

    kinit bob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Stellen Sie sicher, dass der Domänenname in Großbuchstaben angegeben wird, andernfalls schlägt kinit fehl.

3. Binden Sie den Computer in die Domäne ein. Geben Sie in Ihrem PuTTY-Terminal folgenden Befehl ein. Stellen Sie sicher, dass Sie den gleichen Benutzer angeben wie im Schritt oben („kinit“).

    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'

	![realm join](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Wenn der Computer erfolgreich in die verwaltete Domäne eingebunden wurde, sollte eine Meldung angezeigt werden, dass der Computer erfolgreich im Bereich registriert wurde.


## Überprüfen des Domänenbeitritts
Sie können schnell überprüfen, ob der Computer erfolgreich der verwalteten Domäne beigetreten ist. Stellen Sie über SSH und ein Domänenbenutzerkonto eine Verbindung mit dem neu in die Domäne eingebundenen virtuellen RHEL-Computer her, und überprüfen Sie ob das Benutzerkonto ordnungsgemäß aufgelöst wird.

1. Geben Sie in Ihrem PuTTY-Terminal den folgenden Befehl ein, um über SSH eine Verbindung mit dem neu in die Domäne eingebundenen virtuellen RHEL-Computer herzustellen. Verwenden Sie ein Domänenkonto, das zu der verwalteten Domäne gehört (in diesem Fall z. B. „bob@CONTOSO100.COM“).

    ssh -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net

2. Geben Sie in Ihrem PuTTY-Terminal den folgenden Befehl ein, um zu ermitteln, ob das Basisverzeichnis ordnungsgemäß initialisiert wurde.

	pwd

3. Geben Sie in Ihrem PuTTY-Terminal den folgenden Befehl ein, um zu ermitteln, ob die Gruppenmitgliedschaften ordnungsgemäß aufgelöst wurden.

    id

Ein Beispiel für die Ausgabe dieser Befehle sehen Sie unten.

![Überprüfen des Domänenbeitritts](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)


## Problembehandlung beim Domänenbeitritt
Informationen finden Sie im Artikel [Problembehandlung beim Domänenbeitritt](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join).


## Verwandte Inhalte
- [Azure AD-Domänendienste – Leitfaden zu den ersten Schritten](./active-directory-ds-getting-started.md)

- [Einbinden eines virtuellen Windows Server-Computers in eine verwaltete Domäne der Azure AD-Domänendienste](active-directory-ds-admin-guide-join-windows-vm.md)

- [Anmelden bei einem mit Linux betriebenen virtuellen Computer](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)

- [Installing Kerberos (Installieren von Kerberos)](https://access.redhat.com/documentation/de-DE/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)

- [Red Hat Enterprise Linux 7 - Windows Integration Guide (Red Hat Enterprise Linux 7 – Windows-Integrationsleitfaden)](https://access.redhat.com/documentation/de-DE/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)

<!---HONumber=AcomDC_0907_2016-->