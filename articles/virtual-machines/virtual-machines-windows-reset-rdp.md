<properties
    pageTitle="Zurücksetzen des Kennworts oder der Remotedesktopkonfiguration auf einem virtuellen Windows-Computer | Microsoft Azure"
    description="Erfahren Sie, wie Sie das Kennwort für ein Konto oder Remotedesktopdienste auf einem virtuellen Windows-Computer mit dem Azure-Portal oder Azure PowerShell zurücksetzen."
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
    ms.date="09/01/2016"
    ms.author="iainfou"/>


# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Zurücksetzen des Remotedesktopdiensts oder seines Anmeldekennworts in einer Windows-VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Wenn Sie keine Verbindung mit einem virtuellen Windows-Computer herstellen können, können Sie das lokale Administratorkennwort oder die Konfiguration des Remotedesktopdiensts zurücksetzen. Das Kennwort kann entweder über das Azure-Portal oder über die VM-Zugriffserweiterung in Azure PowerShell zurückgesetzt werden. Wenn Sie PowerShell verwenden, stellen Sie sicher, dass das neueste PowerShell-Modul auf Ihrem Arbeitscomputer installiert ist, und Sie bei Ihrem Azure-Abonnement angemeldet sind. Ausführliche Anweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

> [AZURE.TIP] Die installierte PowerShell-Version können Sie mithilfe von `Import-Module Azure, AzureRM; Get-Module Azure, AzureRM | Format-Table Name, Version` ermitteln.

## <a name="windows-vms-in-resource-manager-deployment-model"></a>Windows-VMs im Resource Manager-Bereitstellungsmodell

### <a name="azure-portal"></a>Azure-Portal
Wählen Sie Ihren virtuellen Computer aus, indem Sie auf **Durchsuchen** > **Virtuelle Computer** > *Ihr virtueller Windows-Computer* > **Alle Einstellungen** > **Kennwort zurücksetzen** klicken. Das Blatt zum Zurücksetzen des Kennworts wird angezeigt:

![Seite „Zurücksetzen des Kennworts“](./media/virtual-machines-windows-reset-rdp/Portal-RM-PW-Reset-Windows.png)

Geben Sie den Benutzernamen und ein neues Kennwort ein, und klicken Sie anschließend auf **Speichern**. Versuchen Sie erneut, eine Verbindung mit Ihrem virtuellen Computer herzustellen.

### <a name="vmaccess-extension-and-powershell"></a>VMAccess-Erweiterung und PowerShell

Stellen Sie sicher, dass Azure PowerShell 1.0 oder höher installiert ist und Sie sich bei Ihrem Konto mithilfe des Cmdlets `Login-AzureRmAccount` angemeldet haben.

#### <a name="**reset-the-local-administrator-account-password**"></a>**Zurücksetzen des Kennworts eines lokalen Administratorkontos**

Sie können das Administratorkennwort oder den Benutzernamen mithilfe des PowerShell-Befehls [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) zurücksetzen.

Erstellen Sie Ihre lokalen Administratorkonto-Anmeldeinformationen mithilfe des folgenden Befehls:

    $cred=Get-Credential

Wenn Sie einen anderen Namen als das aktuelle Konto eingeben, benennt der folgende VMAccess-Erweiterungsbefehl das lokale Administratorkonto um, weist das Kennwort diesem Konto zu und gibt ein Ereignis zum Abmelden von Remotedesktop aus. Wenn das lokale Administratorkonto deaktiviert ist, wird es durch die VMAccess-Erweiterung aktiviert.

Verwenden Sie die VMAccess-Erweiterung, um die neuen Anmeldeinformationen wie folgt festlegen:

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" -Name "myVMAccess" `
        -Location WestUS -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"


Ersetzen Sie `myRG`, `myVM`, `myVMAccess` und den Speicherort durch relevante Werte für Ihre Umgebung.


#### <a name="**reset-the-remote-desktop-service-configuration**"></a>**Zurücksetzen der Konfiguration des Remotedesktopdiensts**

Sie können den Remotezugriff auf Ihren virtuellen Computer zurücksetzen, indem Sie entweder [Set-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) oder [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) wie folgt verwenden. (Ersetzen Sie `myRG`, `myVM`, `myVMAccess` und den Speicherort durch Ihre eigenen Werte.)

    Set-AzureRmVMExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -ExtensionType "VMAccessAgent" -Location WestUS `
        -Publisher "Microsoft.Compute" -typeHandlerVersion "2.0"

Alternative:<br>

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0


> [AZURE.TIP] Beide Befehle fügen einen neu benannten VM-Zugriffs-Agent auf dem virtuellen Computer hinzu. Zu jedem Zeitpunkt kann eine VM nur einen einzelnen VM-Zugriffs-Agent haben. Entfernen Sie zum Festlegen der Eigenschaften des VM-Zugriffs-Agents den zuvor festgelegten Zugriffs-Agent mit `Remove-AzureRmVMAccessExtension` oder `Remove-AzureRmVMExtension`. Ab Azure PowerShell Version 1.2.2 können Sie diesen Schritt vermeiden, wenn Sie `Set-AzureRmVMExtension` mit einer Option vom Typ `-ForceRerun` verwenden. Verwenden Sie bei Verwendung von `-ForceRerun`unbedingt den gleichen Namen für den VM-Zugriffs-Agent, den Sie mit dem vorherigen Befehl festgelegt haben.

Wenn Sie weiterhin nicht remote auf den virtuellen Computer zugreifen können, finden Sie weitere Schritte unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="windows-vms-in-the-classic-deployment-model"></a>Virtuelle Windows-Computer im klassischen Bereitstellungsmodell

### <a name="azure-portal"></a>Azure-Portal

Für mit dem klassischen Bereitstellungsmodell erstellte virtuelle Computer können Sie das [Azure-Portal](https://portal.azure.com) zum Zurücksetzen des Remotedesktopdiensts verwenden. Klicken Sie auf **Durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Windows-Computer* > **Remotezugriff zurücksetzen...**. Die folgende Seite wird angezeigt.

![Seite „Zurücksetzen der RDP-Konfiguration“](./media/virtual-machines-windows-reset-rdp/Portal-RDP-Reset-Windows.png)

Sie können auch versuchen, den Namen und das Kennwort des lokalen Administratorkontos zurückzusetzen. Klicken Sie auf **Durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Windows-Computer* > **Alle Einstellungen** > **Kennwort zurücksetzen**. Die folgende Seite wird angezeigt.

![Seite „Zurücksetzen des Kennworts“](./media/virtual-machines-windows-reset-rdp/Portal-PW-Reset-Windows.png)

Klicken Sie auf **Speichern**, nachdem Sie den neuen Benutzernamen und das neue Kennwort eingegeben haben.

### <a name="vmaccess-extension-and-powershell"></a>VMAccess-Erweiterung und PowerShell

Stellen Sie sicher, dass der VM-Agent auf dem virtuellen Computer installiert ist. Die Erweiterung „VMAccess“ muss nicht installiert sein, bevor Sie ihn benutzen können, solange der VM-Agent verfügbar ist. Stellen Sie mit folgendem Befehl sicher, dass der VM-Agent bereits installiert ist. (Ersetzen Sie myCloudService und myVM durch den Namen Ihres Clouddiensts bzw. Ihrer VM. Die entsprechenden Namen können Sie ermitteln, indem Sie `Get-AzureVM` ohne Parameter ausführen.)

    $vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
    write-host $vm.VM.ProvisionGuestAgent

Der Befehl **write-host** zeigt **True** an, wenn der VM-Agent installiert ist. Wenn **False**angezeigt wird, nutzen Sie die Anweisungen und den Link zum Download im Azure-Blogbeitrag VM [Agent and Extensions – Part 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) (in englischer Sprache).

Wenn Sie den virtuellen Computer über das Portal erstellt haben, überprüfen Sie, ob `$vm.GetInstance().ProvisionGuestAgent` den Wert **True**zurückgibt. Wenn dies nicht der Fall ist, können Sie es mit diesem Befehl festlegen:

    $vm.GetInstance().ProvisionGuestAgent = $true

Dieser Befehl verhindert beim Ausführen des Befehls **Set-AzureVMExtension** in den nächsten Schritten den folgenden Fehler: „Provision Guest Agent muss für das VM-Objekt aktiviert sein, bevor Sie die IaaS VMAccess-Erweiterung festlegen.“

#### <a name="**reset-the-local-administrator-account-password**"></a>**Zurücksetzen des Kennworts eines lokalen Administratorkontos**

Erstellen Sie Anmeldeinformationen mit dem Namen des aktuellen lokalen Administratorkontos und einem neuen Kennwort, und führen Sie dann `Set-AzureVMAccessExtension` wie folgt aus.

    $cred=Get-Credential
    Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password  | Update-AzureVM

Wenn Sie einen anderen Namen als das aktuelle Konto eingeben, benennt die VMAccess-Erweiterung das lokale Administratorkonto um, weist das Kennwort diesem Konto zu und gibt einen Befehl zum Abmelden von Remotedesktop aus. Wenn das lokale Administratorkonto deaktiviert ist, wird es durch die VMAccess-Erweiterung aktiviert.

Diese Befehle setzen auch die Konfiguration des Remotedesktopdiensts zurück.

#### <a name="**reset-the-remote-desktop-service-configuration**"></a>**Zurücksetzen der Konfiguration des Remotedesktopdiensts**

Führen Sie den folgenden Befehl zum Zurücksetzen der Konfiguration des Remotedesktopdiensts aus:

    Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

Die VMAccess-Erweiterung führt zwei Befehle auf dem virtuellen Computer aus:

- `netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes`

Dieser Befehl aktiviert die integrierte Windows-Firewallgruppe, die den eingehenden Remotedesktop-Datenverkehr über TCP-Port 3389 zulässt.

- `Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0`

Dieser Befehl legt den Registrierungswert "fDenyTSConnections" auf 0 fest und aktiviert damit Remotedesktopverbindungen.


## <a name="next-steps"></a>Nächste Schritte

Falls die Erweiterung für den Zugriff auf virtuelle Computer nicht reagiert und Sie das Kennwort nicht zurücksetzen können, können Sie [das lokale Windows-Kennwort offline zurücksetzen](virtual-machines-windows-reset-local-password-without-agent.md). Bei dieser Methode handelt es sich um einen fortgeschritteneren Prozess, bei dem Sie die virtuelle Festplatte des problematischen virtuellen Computers mit einem anderen virtuellen Computer verbinden müssen. Führen Sie zunächst die in diesem Artikel dokumentierten Schritte aus. Die Methode zur Offlinezurücksetzung des Kennworts sollte nur als letzter Ausweg verwendet werden.

[Azure-VM-Erweiterungen und Features](virtual-machines-windows-extensions-features.md)

[Herstellen einer Verbindung mit einem virtuellen Azure-Computer über RDP oder SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md)



<!--HONumber=Oct16_HO2-->


