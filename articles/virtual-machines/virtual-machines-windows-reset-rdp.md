<properties
	pageTitle="Zurücksetzen des Kennworts oder von Remotedesktop auf einen virtuellen Windows-Computer | Microsoft Azure"
	description="Setzen Sie das Administratorkennwort oder die Remotedesktopdienste auf einem mit dem Ressourcen-Manager-Bereitstellungsmodell erstellten virtuellen Windows-Computer zurück."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/10/2016"
	ms.author="iainfou"/>

# Zurücksetzen des Remotedesktopdiensts oder seines Anmeldekennworts in einer Windows-VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Wenn Sie ein Kennwort vergessen haben oder ein Problem mit der Konfiguration des Remotedesktopdiensts vorliegt, und Sie daher keine Verbindung mit einem virtuellen Windows-Computer herstellen können, können Sie das lokale Administratorkennwort oder die Konfiguration des Remotedesktopdiensts zurücksetzen. Das Kennwort kann entweder über das Azure-Portal oder über die VM-Zugriffserweiterung in Azure PowerShell zurückgesetzt werden. Wenn Sie PowerShell verwenden, stellen Sie sicher, dass das neueste PowerShell-Modul auf Ihrem Arbeitscomputer installiert ist, und Sie bei Ihrem Azure-Abonnement angemeldet sind. Ausführliche Anweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

> [AZURE.TIP] Die installierte PowerShell-Version können Sie mithilfe von `Import-Module Azure, AzureRM; Get-Module Azure, AzureRM | Format-Table Name, Version` ermitteln.

## Windows-VMs im Resource Manager-Bereitstellungsmodell

### Azure-Portal
Wählen Sie Ihren virtuellen Computer aus, indem Sie auf **Alle durchsuchen** > **Virtuelle Computer** > *<Ihr virtueller Windows-Computer>* klicken, und klicken Sie anschließend auf **Alle Einstellungen** > **Kennwort zurücksetzen** . Das Blatt zum Zurücksetzen des Kennworts wird angezeigt:

![Seite „Zurücksetzen des Kennworts“](./media/virtual-machines-windows-reset-rdp/Portal-RM-PW-Reset-Windows.png)

Geben Sie den Benutzernamen und ein neues Kennwort ein, und klicken Sie anschließend auf **Speichern**. Versuchen Sie erneut, eine Verbindung mit Ihrem virtuellen Computer herzustellen.

### VMAccess-Erweiterung und PowerShell

Stellen Sie sicher, dass Azure PowerShell 1.0 oder höher installiert ist und Sie sich bei Ihrem Konto mithilfe des Cmdlets `Login-AzureRmAccount` angemeldet haben.

#### **Zurücksetzen des Kennworts eines lokalen Administratorkontos**

Sie können das Administratorkennwort oder den Benutzernamen mithilfe des PowerShell-Befehls [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) zurücksetzen.

Erstellen Sie Ihre lokalen Administratorkonto-Anmeldeinformationen mithilfe des folgenden Befehls:

	$cred=Get-Credential

Wenn Sie einen anderen Namen als das aktuelle Konto eingeben, benennt der folgende VMAccess-Erweiterungsbefehl das lokale Administratorkonto um, weist das Kennwort diesem Konto zu und gibt einen Befehl zum Abmelden von Remotedesktop aus. Wenn das lokale Administratorkonto deaktiviert ist, wird es durch die VMAccess-Erweiterung aktiviert.

Verwenden Sie die VMAccess-Erweiterung, um die neuen Anmeldeinformationen wie folgt festlegen:

	Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" -Name "myVMAccess" `
		-Location WestUS -UserName $cred.GetNetworkCredential().Username `
		-Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"


Ersetzen Sie `myRG`, `myVM`, `myVMAccess` und den Speicherort durch relevante Werte für Ihre Umgebung.


#### **Zurücksetzen der Konfiguration des Remotedesktopdiensts**

Sie können den Remotezugriff auf Ihren virtuellen Computer zurücksetzen, indem Sie entweder [Set-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) oder [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) wie folgt verwenden. (Ersetzen Sie `myRG`, `myVM`, `myVMAccess` und den Speicherort durch Ihre eigenen Werte.)

	Set-AzureRmVMExtension -ResourceGroupName "myRG" -VMName "myVM" `
		-Name "myVMAccess" -ExtensionType "VMAccessAgent" -Location WestUS `
		-Publisher "Microsoft.Compute" -typeHandlerVersion "2.0"

Alternative:<br>

	Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" `
		-Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0


> [AZURE.TIP] Beide Befehle fügen einen neu benannten VM-Zugriffs-Agent auf dem virtuellen Computer hinzu. Zu jedem Zeitpunkt kann eine VM nur einen einzelnen VM-Zugriffs-Agent haben. Entfernen Sie zum Festlegen der Eigenschaften des VM-Zugriffs-Agents den zuvor festgelegten Zugriffs-Agent mit `Remove-AzureRmVMAccessExtension` oder `Remove-AzureRmVMExtension`. Ab Azure PowerShell Version 1.2.2 können Sie diesen Schritt vermeiden, wenn Sie `Set-AzureRmVMExtension` mit einer Option vom Typ `-ForceRerun` verwenden. Verwenden Sie bei Verwendung von `-ForceRerun` unbedingt den gleichen Namen für den VM-Zugriffs-Agent, den Sie mit dem vorherigen Befehl festgelegt haben.

Wenn Sie weiterhin nicht remote auf den virtuellen Computer zugreifen können, finden Sie weitere Schritte unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md).


## Virtuelle Windows-Computer im klassischen Bereitstellungsmodell

### Azure-Portal

Für mit dem klassischen Bereitstellungsmodell erstellte virtuelle Computer können Sie das [Azure-Portal](https://portal.azure.com) zum Zurücksetzen des Remotedesktopdiensts verwenden. Klicken Sie auf **Alle durchsuchen** > **Virtuelle Computer (klassisch)** > *<Ihr virtueller Windows-Computer>* > **Remotezugriff zurücksetzen...**. Die folgende Seite wird angezeigt.

![Seite „Zurücksetzen der RDP-Konfiguration“](./media/virtual-machines-windows-reset-rdp/Portal-RDP-Reset-Windows.png)

Sie können auch versuchen, den Namen und das Kennwort des lokalen Administratorkontos zurückzusetzen. Klicken Sie auf **Alle durchsuchen** > **Virtuelle Computer (klassisch)** > *<Ihr virtueller Windows-Computer>* > **Alle Einstellungen** > **Kennwort zurücksetzen**. Die folgende Seite wird angezeigt.

![Seite „Zurücksetzen des Kennworts“](./media/virtual-machines-windows-reset-rdp/Portal-PW-Reset-Windows.png)

Klicken Sie auf **Speichern**, nachdem Sie den neuen Benutzernamen und das neue Kennwort eingegeben haben.

### VMAccess-Erweiterung und PowerShell

Stellen Sie sicher, dass der VM-Agent auf dem virtuellen Computer installiert ist. Die Erweiterung „VMAccess“ muss nicht installiert sein, bevor Sie ihn benutzen können, solange der VM-Agent verfügbar ist. Stellen Sie mit folgendem Befehl sicher, dass der VM-Agent bereits installiert ist. (Ersetzen Sie myCloudService und myVM durch den Namen Ihres Clouddiensts bzw. Ihrer VM. Die entsprechenden Werte können Sie ermitteln, indem Sie `Get-AzureVM` ohne Parameter ausführen.)

	$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
	write-host $vm.VM.ProvisionGuestAgent

Der Befehl **write-host** zeigt **True** an, wenn der VM-Agent installiert ist. Wenn **False** angezeigt wird, nutzen Sie die Anweisungen und den Link zum Download im Azure-Blogbeitrag VM [Agent and Extensions – Part 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) (in englischer Sprache).

Wenn Sie den virtuellen Computer über das Portal erstellt haben, überprüfen Sie, ob `$vm.GetInstance().ProvisionGuestAgent` den Wert **True** zurückgibt. Wenn dies nicht der Fall ist, können Sie es mit diesem Befehl festlegen:

	$vm.GetInstance().ProvisionGuestAgent = $true

Dieser Befehl verhindert beim Ausführen des Befehls **Set-AzureVMExtension** in den nächsten Schritten den folgenden Fehler: „Provision Guest Agent muss für das VM-Objekt aktiviert sein, bevor Sie die IaaS VMAccess-Erweiterung festlegen.“

#### **Zurücksetzen des Kennworts eines lokalen Administratorkontos**

Erstellen Sie Anmeldeinformationen mit dem Namen des aktuellen lokalen Administratorkontos und einem neuen Kennwort, und führen Sie dann `Set-AzureVMAccessExtension` wie folgt aus.

	$cred=Get-Credential
	Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
		-Password $cred.GetNetworkCredential().Password  | Update-AzureVM

Wenn Sie einen anderen Namen als das aktuelle Konto eingeben, benennt die VMAccess-Erweiterung das lokale Administratorkonto um, weist das Kennwort diesem Konto zu und gibt einen Befehl zum Abmelden von Remotedesktop aus. Wenn das lokale Administratorkonto deaktiviert ist, wird es durch die VMAccess-Erweiterung aktiviert.

Diese Befehle setzen auch die Konfiguration des Remotedesktopdiensts zurück.

#### **Zurücksetzen der Konfiguration des Remotedesktopdiensts**

Führen Sie den folgenden Befehl zum Zurücksetzen der Konfiguration des Remotedesktopdiensts aus:

	Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

Die VMAccess-Erweiterung führt zwei Befehle auf dem virtuellen Computer aus:

a. `netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes`

Dieser Befehl aktiviert die integrierte Windows-Firewallgruppe, die den eingehenden Remotedesktop-Datenverkehr über TCP-Port 3389 zulässt.

b. `Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0`

Dieser Befehl legt den Registrierungswert "fDenyTSConnections" auf 0 fest und aktiviert damit Remotedesktopverbindungen.


## Zusätzliche Ressourcen

[Azure-VM-Erweiterungen und Features](virtual-machines-windows-extensions-features.md)

[Herstellen einer Verbindung mit einem virtuellen Azure-Computer über RDP oder SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md)

<!---HONumber=AcomDC_0615_2016-->