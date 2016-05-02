<properties
	pageTitle="Azure Active Directory-Domänendienste (Vorschau): Administratorhandbuch | Microsoft Azure"
	description="Verwalten von DNS in verwalteten Domänen mithilfe der Azure Active Directory-Domänendienste"
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
	ms.date="04/08/2016"
	ms.author="maheshu"/>

# Verwalten von DNS in einer durch Azure AD-Domänendienste verwalteten Domäne
Azure Active Directory-Domänendienste umfassen einen DNS-Server (Domain Name Service), der die DNS-Auflösung für die verwaltete Domäne durchführt. Gelegentlich kann es erforderlich sein, DNS in der verwalteten Domäne zu konfigurieren, um Einträge für Computer, die der Domäne nicht beigetreten sind, für virtuelle IP-Adressen für Load Balancer oder für die externe DNS-Weiterleitung zu erhalten. Aus diesem Grund werden Benutzern, die zur Administratorengruppe für Azure AD-Domänencontroller gehören, DNS-Administratorrechte in der verwalteten Domäne gewährt.

## Einrichten eines der Domäne beigetretenen virtuellen Computers zur Remoteverwaltung von DNS für die verwaltete Domäne
Durch die Azure AD-Domänendienste verwaltete Domänen können mithilfe bekannter Active Directory-Verwaltungstools remote verwaltet werden, beispielsweise über das Active Directory-Verwaltungscenter (Active Directory Administrative Center, ADAC) oder AD PowerShell. Auf die gleiche Weise kann DNS für die verwaltete Domäne remote mithilfe der DNS-Server-Verwaltungstools verwaltet werden. Mandantenadministratoren besitzen keine Berechtigungen, um über Remotedesktop eine Verbindung mit Domänencontrollern in der verwalteten Domäne herzustellen. Aus diesem Grund können Mitglieder der Administratorengruppe für Azure AD-Domänencontroller DNS für verwaltete Domänen remote über die DNS-Servertools eines Windows Server- oder Windows-Clientcomputers verwalten, der der verwalteten Domäne beigetreten ist. DNS-Servertools können im Rahmen der optionalen Remoteserver-Verwaltungstools (Remote Server Administration Tools, RSAT) auf Windows Server- und Windows-Clientcomputern installiert werden, die der verwalteten Domäne beigetreten sind.

Der erste Schritt besteht darin, einen virtuellen Windows Server-Computer einzurichten, der der verwalteten Domäne beigetreten ist. Anleitungen hierzu finden Sie im Artikel [Hinzufügen eines virtuellen Windows Server-Computers zu einer verwalteten Domäne](active-directory-ds-admin-guide-join-windows-vm.md). Beachten Sie, dass bei dieser Anleitung ein virtueller Windows Server-Computer zur Administration der durch Azure AD-Domänendienste verwalteten Domäne verwendet wird. Sie können zu diesem Zweck auch einen virtuellen Windows-Clientcomputer (beispielsweise unter Windows 10) verwenden. In diesem Fall installieren Sie das optionale Feature der Remoteserver-Verwaltungstools auf dem virtuellen Computer.


## Installieren von DNS-Servertools auf dem virtuellen Computer
Führen Sie die folgenden Schritte aus, um die DNS-Verwaltungstools auf dem virtuellen Computer zu installieren, der der Domäne beigetreten ist. Weitere Informationen zum [Bereitstellen der Remoteserver-Verwaltungstools](https://technet.microsoft.com/library/hh831501.aspx) finden Sie auf TechNet.

1. Wechseln Sie im klassischen Azure-Portal zum Knoten **Virtuelle Computer**. Wählen Sie den virtuellen Computer aus, den Sie gerade erstellt haben, und klicken Sie auf der Befehlsleiste unten im Fenster auf **Verbinden**.

    ![Verbindung mit virtuellem Windows-Computer herstellen](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Sie werden aufgefordert, eine RDP-Datei zu öffnen oder zu speichern, die zum Herstellen der Verbindung mit dem virtuellen Computer verwendet wird. Klicken Sie auf die RDP-Datei, wenn der Download abgeschlossen ist.

3. Verwenden Sie bei der Anmeldeaufforderung die Anmeldeinformationen eines Benutzers, der zur Administratorengruppe für Azure AD-Domänencontroller gehört. In diesem Beispiel: „bob@domainservicespreview.onmicrosoft.com“.

4. Öffnen Sie auf dem Startbildschirm den **Server-Manager**. Klicken Sie im mittleren Bereich des Server-Manager-Fensters auf **Rollen und Features hinzufügen**.

    ![Server-Manager auf dem virtuellen Computer starten](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. Klicken Sie auf der Seite **Vorbereitung** des **Assistenten zum Hinzufügen von Rollen und Funktionen** auf **Weiter**.

    ![Seite „Vorbereitung“](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. Behalten Sie auf der Seite **Installationstyp** die Aktivierung der Option **Rollenbasierte oder featurebasierte Installation** bei, und klicken Sie auf **Weiter**.

	![Seite „Installationstyp“](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. Wählen Sie auf der Seite **Serverauswahl** den aktuellen virtuellen Computer aus dem Serverpool aus, und klicken Sie auf **Weiter**.

	![Seite „Serverauswahl“](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. Klicken Sie auf der Seite **Serverrollen** auf **Weiter**. Überspringen Sie diese Seite, da Sie in diesem Beispiel keine Rollen auf dem Server installieren.

9. Klicken Sie auf der Seite **Features**, um den Knoten **Remoteserver-Verwaltungstools** zu erweitern, und klicken Sie erneut, um den Knoten **Rollenverwaltungstools** zu erweitern. Wählen Sie das Feature **DNS-Servertools** aus der Liste der Rollenverwaltungstools aus, wie unten gezeigt.

	![Seite „Features“](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

10. Klicken Sie auf der Seite **Bestätigung** auf **Installieren**, um die DNS-Servertools auf dem virtuellen Computer zu installieren. Wenn die Installation des Features erfolgreich abgeschlossen wurde, klicken Sie auf **Schließen**, um den **Assistenten zum Hinzufügen von Rollen und Funktionen** zu beenden.

	![Bestätigungsseite](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)


## Starten der DNS-Verwaltungskonsole
Nachdem das Feature der DNS-Servertools nun auf dem virtuellen Computer installiert ist, der der Domäne beigetreten ist, können Sie die DNS-Tools zur Administration von DNS in der verwalteten Domäne verwenden.

1. Klicken Sie auf dem Startbildschirm auf **Verwaltung**. Daraufhin sollte die auf dem virtuellen Computer installierte **DNS**-Konsole angezeigt werden.

	![Verwaltungstools – DNS-Konsole](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)

2. Klicken Sie auf **DNS**, um die DNS-Verwaltungskonsole zu starten.

3. Klicken Sie im Dialogfeld **Verbindung mit DNS-Server herstellen** auf die Option **Folgenden Computer**, und geben Sie den DNS-Domänennamen der verwalteten Domäne ein (z. B. „contoso100.com“).

    ![DNS-Konsole – Verbindung mit Domäne herstellen](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

4. Die DNS-Konsole stellt eine Verbindung mit der verwalteten Domäne her. Es wird eine Seite ähnlich der folgenden angezeigt:

    ![DNS-Konsole – Domäne verwalten](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

5. Sie können die DNS-Konsole jetzt verwenden, um DNS-Einträge für Computer in dem virtuellen Netzwerk hinzuzufügen, in dem Sie die Azure AD-Domänendienste aktiviert haben.

> [AZURE.WARNING] Seien Sie sehr vorsichtig bei der Administration von DNS für die verwaltete Domäne mithilfe der DNS-Verwaltungstools. Stellen Sie sicher, dass Sie **die integrierten DNS-Einträge, die von den Domänendiensten in der Domäne verwendet werden, nicht löschen oder ändern**. Hierzu gehören Domänen-DNS-Einträge, Namenservereinträge sowie weitere für den Domänencontrollerstandort verwendete Einträge. Wenn Sie diese Einträge ändern, werden die Domänendienste im virtuellen Netzwerk unterbrochen.


Weitere Informationen zur Verwaltung von DNS finden Sie im [Technet-Artikel zu DNS-Tools](https://technet.microsoft.com/library/cc753579.aspx).

<!---HONumber=AcomDC_0420_2016-->