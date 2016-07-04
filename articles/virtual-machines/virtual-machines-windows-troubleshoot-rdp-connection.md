<properties
	pageTitle="Problembehandlung bei der Remotedesktopverbindung mit einem virtuellen Azure-Computer | Microsoft Azure"
	description="Problembehandlung bei Remotedesktop.Verbindungsfehlern auf einem virtuellen Windows-Computer Sie erhalten schnelle Schritte zur Lösung, Hilfe zur jeweiligen Fehlermeldung und Informationen zur ausführlichen Problembehandlung für Netzwerke."
	keywords="Remotedesktop-Fehler,Remotedesktop-Verbindungsfehler,Verbindung mit virtuellem Computer nicht möglich,Remotedesktop-Problembehandlung"
	services="virtual-machines-windows"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="support-article"
	ms.date="06/14/2016"
	ms.author="iainfou"/>

# Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer

Eine Verbindung über das Remotedesktopprotokoll (RDP) mit Ihrem Windows-basierten virtuellen Azure-Computer (VM, Virtual Machine) kann aus verschiedenen Gründen fehlschlagen. Das Problem kann mit dem Remotedesktopdienst auf dem virtuellen Computer, der Netzwerkverbindungoder dem Remotedesktopclient auf Ihrem Hostcomputer zusammenhängen. Dieser Artikel führt Sie durch einige der am häufigsten verwendeten Methoden, um die RDP-Verbindungsprobleme zu beheben. Wenn Ihr Problem hier nicht aufgeführt ist oder Sie trotzdem keine RDP-Verbindung mit Ihrem virtuellen Computer herstellen können, können Sie eine [ausführlichere Darstellung der Konzepte und Schritte für die RDP-Problembehandlung](virtual-machines-windows-detailed-troubleshoot-rdp.md) zu Rate ziehen.

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie Azure-Experten im [MSDN Azure-Forum oder im Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) Fragen stellen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten** aus.

<a id="quickfixrdp"></a>

## Problembehandlung bei virtuellen Computern, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden

Versuchen Sie nach jedem Problembehandlungsschritt, die Verbindung mit dem virtuellen Computer erneut herzustellen.

> [AZURE.TIP] Wenn die Schaltfläche „Verbinden“ im Portal ausgeblendet ist und keine [Express Route](../expressroute/expressroute-introduction.md)- oder [Standort-zu-Standort-VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)-Verbindung mit Azure besteht, müssen Sie eine öffentliche IP-Adresse erstellen und dem virtuellen Computer zuweisen, damit Sie RDP nutzen können. Lesen Sie mehr über [öffentliche IP-Adressen in Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).

- Setzen Sie den Remotezugriff mithilfe von PowerShell zurück.
	- Sofern dies noch nicht erfolgt ist, [installieren und konfigurieren Sie die neueste Version von Azure PowerShell](../powershell-install-configure.md).

	- Setzen Sie Ihre RDP-Verbindung mit einem der folgenden PowerShell-Befehle zurück. Ersetzen Sie `myRG`, `myVM`, `myVMAccessExtension` und den Speicherort durch für Ihr Setup relevante Werte.

	```
	Set-AzureRmVMExtension -ResourceGroupName "myRG" -VMName "myVM" `
		-Name "myVMAccessExtension" -ExtensionType "VMAccessAgent" `
		-Publisher "Microsoft.Compute" -typeHandlerVersion "2.0" `
		-Location Westus
	```
	ODER

  	```
	Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" `
		-VMName "myVM" -Name "myVMAccess" -Location Westus
	```

	> [AZURE.NOTE] In den vorhergehenden Beispielen ist `myVMAccessExtension` oder `MyVMAccess` der Name, den Sie für die neue Erweiterung angeben, die während des Vorgangs installiert wird. Häufig ist dies einfach auf den Namen des virtuellen Computers festgelegt. Wenn Sie zuvor VMAccessAgent verwendet haben, können Sie den Namen der vorhandenen Erweiterung mithilfe von `Get-AzureRmVM -ResourceGroupName "myRG" -Name "myVM"` abrufen, um die Eigenschaften des virtuellen Computers zu überprüfen. Suchen Sie dann im Abschnitt „Erweiterungen“ der Ausgabe. Da nur jeweils ein VMAccessAgent auf einem virtuellen Computer vorhanden sein kann, müssen Sie bei Verwendung von `Set-AzureRmVMExtension` auch den Parameter `-ForceReRun` hinzufügen. Damit wird erzwungen, dass der Agent erneut registriert wird.

- Starten Sie den virtuellen Computer neu, um andere Probleme beim Start zu beheben. Wählen Sie **Durchsuchen** > **Virtuelle Computer** > *Ihr virtueller Computer* > **Neu starten** aus.

- [Stellen Sie Ihren virtuellen Computer auf einem neuen Azure-Knoten erneut bereit](virtual-machines-windows-redeploy-to-new-node.md).

	Beachten Sie, dass nach Beenden dieses Vorgangs kurzlebige Datenträgerdaten verloren gehen und dynamische, dem virtuellen Computer zugeordnete IP-Adressen aktualisiert werden.
	
- Überprüfen Sie, ob Ihre [Regeln für Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md) RDP-Datenverkehr zulassen (über TCP-Port 3389).

- Überprüfen Sie das Konsolenprotokoll oder den Screenshot des virtuellen Computers, um Startprobleme zu beheben. Wählen Sie **Durchsuchen** > **Virtuelle Computer** > *Ihr virtueller Windows-Computer* > **Support und Problembehandlung** > **Startdiagnose** aus.

- [Setzen Sie das Kennwort Ihres virtuellen Computers zurück](virtual-machines-windows-reset-rdp.md).

- Wenn weiterhin RDP-Probleme auftreten, können Sie eine [Supportanfrage stellen](https://azure.microsoft.com/support/options/) oder eine [ausführlichere Darstellung der Konzepte und Schritte für die RDP-Problembehandlung](virtual-machines-windows-detailed-troubleshoot-rdp.md) lesen.


## Problembehandlung bei virtuellen Computern, die mit dem klassischen Bereitstellungsmodell erstellt wurden

Versuchen Sie nach jedem Problembehandlungsschritt, die Verbindung mit dem virtuellen Computer erneut herzustellen.

- Setzen Sie den Remotedesktopdienst im [Azure-Portal](https://portal.azure.com) zurück. Wählen Sie **Durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Computer* > **Remotezugriff zurücksetzen** aus.

- Starten Sie den virtuellen Computer neu, um andere Probleme beim Start zu beheben. Wählen Sie **Durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Computer* > **Neu starten** aus.

- [Stellen Sie Ihren virtuellen Computer auf einem neuen Azure-Knoten erneut bereit](virtual-machines-windows-redeploy-to-new-node.md).

	Beachten Sie, dass nach Beenden dieses Vorgangs kurzlebige Datenträgerdaten verloren gehen und dynamische, dem virtuellen Computer zugeordnete IP-Adressen aktualisiert werden.
	
- Überprüfen Sie, ob Ihr [Cloud Services-Endpunkt RDP-Datenverkehr zulässt](../cloud-services/cloud-services-role-enable-remote-desktop.md).

- Überprüfen Sie das Konsolenprotokoll des virtuellen Computers oder den Screenshot, um Startprobleme zu beheben. Wählen Sie **Durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Computer* > **Einstellungen** > **Startdiagnose** aus.

- Überprüfen Sie die Ressourcenintegrität des virtuellen Computers auf etwaige Plattformprobleme. Wählen Sie **Durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Computer* > **Einstellungen** > **Integrität überprüfen** aus.

- [Setzen Sie das Kennwort Ihres virtuellen Computers zurück](virtual-machines-windows-reset-rdp.md).

- Wenn weiterhin RDP-Probleme auftreten, können Sie eine [Supportanfrage stellen](https://azure.microsoft.com/support/options/) oder eine [ausführlichere Darstellung der Konzepte und Schritte für die RDP-Problembehandlung](virtual-machines-windows-detailed-troubleshoot-rdp.md) lesen.


## Beheben von spezifischen Remotedesktop-Verbindungsfehlern

Möglicherweise wird beim Herstellen der RDP-Verbindung mit Ihrem virtuellen Computer eine spezifische Fehlermeldung angezeigt. Relativ häufig werden z.B. folgende Meldungen angezeigt:

- [Die Verbindung mit der Remotesitzung wurde getrennt, da keine Lizenzserver für Remotedesktop vorhanden sind](#rdplicense).

- [Remotedesktop kann den Computer „Name“ nicht finden](#rdpname).

- [Authentifizierungsfehler. Die lokale Sicherheitsautorität (LSA) ist nicht erreichbar](#rdpauth).

- [Fehler bei Windows-Sicherheit: Mit den Anmeldeinformationen konnte keine Verbindung hergestellt werden.](#wincred)

- [Dieser Computer kann keine Verbindung mit dem Remotecomputer herstellen](#rdpconnect).

<a id="rdplicense"></a>
### Die Verbindung mit der Remotesitzung wurde getrennt, da keine Lizenzserver für Remotedesktop vorhanden sind.

Ursache: Die 120-Tage-Kulanzfrist für die Rolle "Remotedesktopserver" ist abgelaufen, und Sie müssen Lizenzen installieren.

Als Problemumgehung speichern Sie eine lokale Kopie der RDP-Datei aus dem Portal, und führen Sie diesen Befehl an einer PowerShell-Eingabeaufforderung aus, um eine Verbindung herzustellen. Damit wird die Lizenz nur für diese Verbindung deaktiviert:

		mstsc <File name>.RDP /admin

Wenn Sie eigentlich nicht mehr als zwei gleichzeitige Remotedesktopverbindungen mit dem virtuellen Computer benötigen, können Sie mit Server-Manager die Rolle „Remotedesktopserver“ entfernen.

Weitere Informationen finden Sie im Blogbeitrag [Azure VM fails with „No Remote Desktop License Servers available“](http://blogs.msdn.com/b/wats/archive/2014/01/21/rdp-to-azure-vm-fails-with-quot-no-remote-desktop-license-servers-available-quot.aspx) (Azure-VM schlägt fehl mit „Keine Lizenzserver für Remotedesktop verfügbar“).

<a id="rdpname"></a>
### Remotedesktop kann den Computer „Name“ nicht finden.

Ursache: Der Remotedesktopclient auf Ihrem Computer kann den Namen des Computers, der in den Einstellungen der RDP-Datei angegeben ist, nicht auflösen.

Lösungsvorschläge:

- Wenn Sie sich im Intranet einer Organisation befinden, sollten Sie sicherstellen, dass Ihr Computer Zugriff auf den Proxyserver hat und diesem HTTPS-Datenverkehr senden kann.

- Wenn Sie eine lokal gespeicherte RDP-Datei verwenden, können Sie versuchen, die vom Portal generierte Datei zu verwenden. Dadurch wird sichergestellt, dass Sie den richtigen DNS-Namen für den virtuellen Computer oder den Clouddienst und den Endpunktport des virtuellen Computers verwenden. Dies ist eine RDP-Beispieldatei, die vom Portal generiert wurde:

		full address:s:tailspin-azdatatier.cloudapp.net:55919
		prompt for credentials:i:1

Der Adressteil dieser RDP-Datei besteht aus:
- dem vollqualifizierten Domänennamen des Clouddiensts, der den virtuellen Computer enthält (in diesem Beispiel „tailspin-azdatatier.cloudapp.net“)

- dem externen TCP-Port des Endpunkts für den Remotedesktop-Datenverkehr (55919)

<a id="rdpauth"></a>
### Authentifizierungsfehler. Die lokale Sicherheitsautorität (LSA) ist nicht erreichbar.

Ursache: Der virtuelle Zielcomputer kann die Sicherheitsautorität im Benutzernamensteil Ihrer Anmeldeinformationen nicht finden.

Wenn Ihr Benutzername das Format *Sicherheitsautorität*\\*Benutzername* (Beispiel: „CORP\\User1“) aufweist, ist *Sicherheitsautorität* entweder der Computername des virtuellen Computers (für die lokale Sicherheitsautorität) oder der Name einer Active Directory-Domäne.

Lösungsvorschläge:

- Wenn das Konto lokal für den virtuellen Computer gilt, sollten Sie sicherstellen, dass der Name richtig geschrieben ist.

- Wenn sich das Konto in einer Active Directory-Domäne befindet, sollten Sie die richtige Schreibweise des Domänennamens überprüfen.

- Falls es ein Active Directory-Domänenkonto ist und der Domänenname richtig geschrieben ist, sollten Sie sicherstellen, dass in der Domäne ein Domänencontroller verfügbar ist. In virtuellen Azure-Netzwerken, die Domänencontroller enthalten, tritt häufig das Problem auf, dass ein Domänencontroller nicht verfügbar ist, weil er nicht gestartet wurde. Um dieses Problem zu umgehen, können Sie anstelle eines Domänenkontos ein lokales Administratorkonto verwenden.

<a id="wincred"></a>
### Fehler bei Windows-Sicherheit: Mit den Anmeldeinformationen konnte keine Verbindung hergestellt werden.

Ursache: Der virtuelle Zielcomputer kann Ihren Kontonamen und das Kennwort nicht überprüfen.

Ein Windows-basierter Computer kann die Anmeldeinformationen eines lokalen Kontos oder eines Domänenkontos überprüfen.

- Verwenden Sie für lokale Konten die Syntax *Computername*\\*Benutzername* (Beispiel: „SQL1\\Admin4798“).
- Verwenden Sie für Domänenkonten die Syntax *Domänenname*\\*Benutzername* (Beispiel: „CONTOSO\\peterodman“).

Bei virtuellen Computern, die Sie in einer neuen Active Directory-Gesamtstruktur zu Domänencontrollern heraufgestuft haben, wird das lokale Administratorkonto, mit dem Sie sich angemeldet haben, in der neuen Gesamtstruktur und Domäne in ein entsprechendes Konto mit demselben Kennwort konvertiert. Das lokale Konto wird gelöscht.

Wenn Sie sich beispielsweise mit dem lokalen Konto „DC1\\DCAdmin“ angemeldet und dann den virtuellen Computer zu einem Domänencontroller in einer neuen Gesamtstruktur für die Domäne „corp.contoso.com“ heraufgestuft haben, wird das lokale Konto „DC1\\DCAdmin“ gelöscht und ein neues Domänenkonto („CORP\\DCAdmin“) mit demselben Kennwort erstellt.

Stellen Sie sicher, dass der Kontoname ein Name ist, der vom virtuellen Computer als gültiges Konto bestätigt werden kann, und dass das Kennwort korrekt ist.

Wenn Sie das Kennwort für das lokale Administratorkonto ändern müssen, finden Sie hierzu weitere Informationen unter [Zurücksetzen eines Kennworts oder des Remotedesktopdiensts für virtuelle Computer in Windows](virtual-machines-windows-reset-rdp.md).

<a id="rdpconnect"></a>
### Dieser Computer kann keine Verbindung mit dem Remotecomputer herstellen.

Ursache: Das Konto zum Herstellen der Verbindung verfügt nicht über Remotedesktop-Anmelderechte.

Jeder Windows-Computer hat eine lokale Gruppe von Remotedesktopbenutzern, in der die Konten und Gruppen enthalten sind, die sich per Remoteverbindung anmelden können. Auch Mitglieder der lokalen Gruppe „Administratoren“ haben Zugriff, obwohl diese Konten nicht in der lokalen Gruppe „Remotedesktopbenutzer“ aufgelistet sind. Bei in einer Domäne eingebundenen Computern enthält die lokale Gruppe „Administratoren“ auch die Domänenadministratoren für die Domäne.

Stellen Sie sicher, dass das Konto, das Sie zum Herstellen der Verbindung verwenden, über Remotedesktop-Anmelderechte verfügt. Verwenden Sie als Problemumgehung ein Domänenkonto oder lokales Administratorkonto, um per Remotedesktop eine Verbindung herzustellen. Verwenden Sie anschließend das MMC-Snap-In (**Systemprogramme > Lokale Benutzer und Gruppen > Gruppen > Remotedesktopbenutzer**), um das gewünschte Konto der lokalen Gruppe mit den Remotedesktopbenutzern hinzuzufügen.

## Problembehandlung bei allgemeinen Remotedesktop-Fehlern

Wenn keiner dieser Fehler auftritt und Sie trotzdem keine Verbindung mit dem virtuellen Computer per Remotedesktop herstellen können, können Sie im [ausführlichen Handbuch zur Problembehandlung für Remotedesktop](virtual-machines-windows-detailed-troubleshoot-rdp.md) nachschlagen.


## Zusätzliche Ressourcen

[Azure IaaS (Windows) und Diagnoseprogramme](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)

[Zurücksetzen eines Kennworts oder des Remotedesktopdiensts für virtuelle Windows-Computer](virtual-machines-windows-reset-rdp.md)

[Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)

[Behandeln von Problemen mit Secure Shell-Verbindungen mit einem Linux-basierten virtuellen Azure-Computer](virtual-machines-linux-troubleshoot-ssh-connection.md)

[Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird](virtual-machines-linux-troubleshoot-app-connection.md)

<!---HONumber=AcomDC_0622_2016-->