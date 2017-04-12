---
title: "Zurücksetzen des Kennworts oder der Remotedesktopkonfiguration auf einem virtuellen Windows-Computer in Azure | Microsoft Docs"
description: "Erfahren Sie, wie Sie das Kennwort für ein Konto oder Remotedesktopdienste auf einem virtuellen Windows-Computer, der mit dem klassischen Bereitstellungsmodell erstellt wurde, mit dem Azure-Portal oder Azure PowerShell zurücksetzen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 43e5cf1ab3bc3121d7e3915ea0785998e0ee2fc6
ms.lasthandoff: 03/31/2017


---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-the-classic-deployment-model"></a>Zurücksetzen des Remotedesktopdiensts oder dessen Anmeldekennworts in einer Windows-VM, die mit dem klassischen Bereitstellungsmodell erstellt wurde
> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und klassische Bereitstellungen](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Sie können diese Schritte auch für virtuelle Computer durchführen, die mit dem [Resource Manager-Bereitstellungsmodell](../reset-rdp.md) erstellt wurden.

Wenn Sie keine Verbindung mit einem virtuellen Windows-Computer herstellen können, können Sie das lokale Administratorkennwort oder die Konfiguration des Remotedesktopdiensts zurücksetzen. Das Kennwort kann entweder über das Azure-Portal oder über die VM-Zugriffserweiterung in Azure PowerShell zurückgesetzt werden.

## <a name="ways-to-reset-configuration-or-credentials"></a>Methoden zum Zurücksetzen der Konfiguration oder der Anmeldeinformationen
Sie können Remotedesktopdienste und -Anmeldeinformationen auf verschiedene Weise zurücksetzen, je nach Ihren Anforderungen:

- [Zurücksetzen mithilfe des Azure-Portals](#azure-portal)
- [Zurücksetzen mithilfe von Azure PowerShell](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure-Portal
Sie können den Remotedesktopdienst im [Azure-Portal](https://portal.azure.com) zurücksetzen. Klicken Sie zum Erweitern des Portalmenüs oben links auf die drei Balken und dann auf **Virtuelle Computer (Klassisch)**:

![Navigieren Sie zu Ihrem virtuellen Azure-Computer](./media/reset-rdp/Portal-Select-Classic-VM.png)

Wählen Sie Ihren virtuellen Windows-Computer aus, und klicken Sie dann auf **Remote zurücksetzen**. Das folgende Dialofeld zum Zurücksetzen der Remotedesktopkonfiguration wird angezeigt:

![Seite „Zurücksetzen der RDP-Konfiguration“](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

Sie können auch den Benutzernamen und das Kennwort des lokalen Administratorkontos zurücksetzen. Klicken Sie auf Ihren virtuellen Computer auf **Support und Problembehandlung** > **Zurücksetzen des Kennworts**. Das Blatt zum Zurücksetzen des Kennworts wird angezeigt:

![Seite „Zurücksetzen des Kennworts“](./media/reset-rdp/Portal-PW-Reset-Windows.png)

Klicken Sie auf **Speichern**, nachdem Sie den neuen Benutzernamen und das neue Kennwort eingegeben haben.

## <a name="vmaccess-extension-and-powershell"></a>VMAccess-Erweiterung und PowerShell
Stellen Sie sicher, dass der VM-Agent auf dem virtuellen Computer installiert ist. Die Erweiterung „VMAccess“ muss nicht installiert sein, bevor Sie ihn benutzen können, solange der VM-Agent verfügbar ist. Stellen Sie mit folgendem Befehl sicher, dass der VM-Agent bereits installiert ist. (Ersetzen Sie myCloudService und myVM durch den Namen Ihres Clouddiensts bzw. Ihrer VM. Die entsprechenden Namen können Sie ermitteln, indem Sie `Get-AzureVM` ohne Parameter ausführen.)

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

Der Befehl **write-host** zeigt **True** an, wenn der VM-Agent installiert ist. Wenn **False**angezeigt wird, nutzen Sie die Anweisungen und den Link zum Download im Azure-Blogbeitrag VM [Agent and Extensions – Part 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) (in englischer Sprache).

Wenn Sie den virtuellen Computer über das Portal erstellt haben, überprüfen Sie, ob `$vm.GetInstance().ProvisionGuestAgent` den Wert **True**zurückgibt. Wenn dies nicht der Fall ist, können Sie es mit diesem Befehl festlegen:

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

Dieser Befehl verhindert beim Ausführen des Befehls **Set-AzureVMExtension** in den nächsten Schritten den folgenden Fehler: „Provision Guest Agent muss für das VM-Objekt aktiviert sein, bevor Sie die IaaS VMAccess-Erweiterung festlegen.“

### <a name="reset-the-local-administrator-account-password"></a>**Zurücksetzen des Kennworts eines lokalen Administratorkontos**
Erstellen Sie Anmeldeinformationen mit dem Namen des aktuellen lokalen Administratorkontos und einem neuen Kennwort, und führen Sie dann `Set-AzureVMAccessExtension` wie folgt aus.

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

Wenn Sie einen anderen Namen als das aktuelle Konto eingeben, benennt die VMAccess-Erweiterung das lokale Administratorkonto um, weist das Kennwort diesem Konto zu und gibt einen Befehl zum Abmelden von Remotedesktop aus. Wenn das lokale Administratorkonto deaktiviert ist, wird es durch die VMAccess-Erweiterung aktiviert.

Diese Befehle setzen auch die Konfiguration des Remotedesktopdiensts zurück.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Zurücksetzen der Konfiguration des Remotedesktopdiensts**
Führen Sie den folgenden Befehl zum Zurücksetzen der Konfiguration des Remotedesktopdiensts aus:

```powershell
Set-AzureVMAccessExtension –vm $vm | Update-AzureVM
```

Die VMAccess-Erweiterung führt zwei Befehle auf dem virtuellen Computer aus:

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

Dieser Befehl aktiviert die integrierte Windows-Firewallgruppe, die den eingehenden Remotedesktop-Datenverkehr über TCP-Port 3389 zulässt.

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

Dieser Befehl legt den Registrierungswert "fDenyTSConnections" auf 0 fest und aktiviert damit Remotedesktopverbindungen.

## <a name="next-steps"></a>Nächste Schritte
Falls die Erweiterung für den Zugriff auf virtuelle Computer nicht reagiert und Sie das Kennwort nicht zurücksetzen können, können Sie [das lokale Windows-Kennwort offline zurücksetzen](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bei dieser Methode handelt es sich um einen fortgeschritteneren Prozess, bei dem Sie die virtuelle Festplatte des problematischen virtuellen Computers mit einem anderen virtuellen Computer verbinden müssen. Führen Sie zunächst die in diesem Artikel dokumentierten Schritte aus. Die Methode zur Offlinezurücksetzung des Kennworts sollte nur als letzter Ausweg verwendet werden.

[Azure-VM-Erweiterungen und Features](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Herstellen einer Verbindung mit einem virtuellen Azure-Computer über RDP oder SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)


