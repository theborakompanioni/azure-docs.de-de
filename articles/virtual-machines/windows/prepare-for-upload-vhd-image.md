---
title: "Vorbereiten einer Windows-VHD für das Hochladen in Azure | Microsoft Docs"
description: Vorbereiten einer Windows-VHD oder -VHDX vor dem Hochladen in Azure
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 1/11/2017
ms.author: glimoli;genli
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 104063500a33dfe55c56467517a5002c562772df
ms.lasthandoff: 03/31/2017


---
# <a name="prepare-a-windows-vhd-or-vhdx-to-upload-to-azure"></a>Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure
Um einen virtuellen Windows-Computer aus einem lokalen Speicherort in Azure hochzuladen, müssen Sie die virtuelle Festplatte (Virtual Hard Disk, VHD oder VHDX) vorbereiten. Azure unterstützt nur virtuelle Computer der 1. Generation , die das VHD-Dateiformat aufweisen und einen Datenträger mit fester Größe haben. Die maximal zulässige Größe für die virtuelle Festplatte beträgt 1.023 GB. Sie können virtuelle Computer der 1. Generation vom VHDX- in das VHD-Dateiformat und von einem dynamisch erweiterbaren Datenträger in einen Datenträger mit fester Größe konvertieren. Aber die Generation eines virtuellen Computers können Sie nicht ändern. Weitere Informationen finden Sie unter [Should I create a generation 1 or 2 virtual machine in Hyper-V?](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v) (Sollte ich einen virtuellen Computer der 1. oder 2. Generation in Hyper-V erstellen?)

## <a name="convert-the-virtual-disk-to-vhd-and-fixed-size-disk"></a>Konvertieren des virtuellen Datenträgers in VHD und einen Datenträger mit fester Größe 
Wenn Sie Ihren virtuellen Datenträger in das für Azure erforderliche Format konvertieren müssen, verwenden Sie eine der in diesem Abschnitt beschriebenen Methoden. Sichern Sie den virtuellen Computer, bevor Sie die Konvertierung des virtuellen Datenträgers ausführen, und stellen Sie sicher, dass die Windows-VHD auf dem lokalen Server ordnungsgemäß funktioniert. Beheben Sie alle Probleme auf dem virtuellen Computer selbst, bevor Sie versuchen, ihn zu konvertieren oder in Azure hochzuladen.

Nachdem Sie den Datenträger konvertiert haben, erstellen Sie einen virtuellen Computer, der den konvertierten Datenträger verwendet. Starten Sie, und melden Sie sich bei dem virtuellen Computer an, um die Vorbereitung des virtuellen Computers für den Upload abzuschließen.

### <a name="convert-disk-using-hyper-v-manager"></a>Konvertieren eines Datenträgers mithilfe des Hyper-V-Managers
1. Öffnen Sie den Hyper-V-Manager, und wählen Sie auf der linken Seite Ihren lokalen Computer aus. Klicken Sie im Menü darüber auf **Aktion** > **Datenträger bearbeiten**.
2. Navigieren Sie auf dem Bildschirm **Virtuelle Festplatte suchen** zu Ihrem virtuellen Datenträger, und wählen Sie ihn aus.
3. Wählen Sie im Bildschirm **Aktion auswählen** die Option **Konvertieren** und **Weiter**.
4. Wenn Sie eine VHDX konvertieren müssen, wählen Sie **VHD** aus, und klicken Sie auf **Weiter**.
5. Wenn Sie einen sich dynamisch erweiternden Datenträger konvertieren müssen, wählen Sie **Feste Größe** aus, und klicken Sie auf **Weiter**.
6. Navigieren Sie zu einem Pfad zum Speichern der neuen VHD-Datei, und wählen Sie ihn aus.
7. Klicken Sie zum Abschluss auf **Fertig stellen** .

### <a name="convert-disk-using-powershell"></a>Konvertieren eines Datenträgers mithilfe von PowerShell
Sie können einen virtuellen Datenträger mithilfe des [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx)-Cmdlets in Windows PowerShell konvertieren. Wählen Sie beim Starten von PowerShell **Als Administrator ausführen**. Im folgenden Beispiel wird das Konvertieren von VHDX zu VHD und von einem dynamisch erweiterbaren Datenträger zu einem Datenträger fester Größe gezeigt:

```powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```
Ersetzen Sie die Werte für „-Path“ durch den Pfad zu der virtuellen Festplatte, die Sie konvertieren möchten, und „-DestinationPath“ durch den neuen Pfad und den Namen für den konvertierten Datenträger.

### <a name="convert-from-vmware-vmdk-disk-format"></a>Konvertieren des VMware-VMDK-Datenträgerformats
Wenn Sie über ein Windows-VM-Image im [VMDK-Dateiformat](https://en.wikipedia.org/wiki/VMDK) verfügen, konvertieren Sie es mit dem [Microsoft Virtual Machine Converter](https://www.microsoft.com/download/details.aspx?id=42497) in eine virtuelle Festplatte. Weitere Informationen finden Sie im Blog [How to Convert a VMWare VMDK to Hyper-V VHD](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) (Konvertieren von VMWare-VMDK in Hyper-V-VHD).

## <a name="set-windows-configurations-for-azure"></a>Festlegen der Windows-Konfigurationen für Azure

Führen Sie alle folgenden Befehle auf dem virtuellen Computer, den Sie in Azure hochladen möchten, mit [Administratorrechten](https://technet.microsoft.com/library/cc947813.aspx) im Eingabeaufforderungsfenster aus.

1. Entfernen Sie alle statischen persistenten Routen aus der Routingtabelle:
   
   * Führen Sie zum Anzeigen der Routingtabelle `route print` über das Eingabeaufforderungsfenster aus.
   * Überprüfen Sie die Abschnitte mit **Persistenzrouten** . Wenn eine persistente Route vorhanden ist, verwenden Sie [route delete](https://technet.microsoft.com/library/cc739598.apx) , um die Route zu entfernen.
2. Entfernen Sie den WinHTTP-Proxy:
   
    ```CMD
    netsh winhttp reset proxy
    ```
3. Legen Sie für die Datenträger-SAN-Richtlinie [Onlineall](https://technet.microsoft.com/library/gg252636.aspx) fest. 
   
    ```CMD
    diskpart 
    san policy=onlineall
    exit
    ```
    

4. Legen Sie die koordinierte Weltzeit (UTC, Coordinated Universal Time) für Windows fest und als Starttyp des Windows-Zeitdiensts (w32time) **Automatisch**:
   
    ```CMD
    REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
    ```
    ```CMD
    sc config w32time start= auto
    ```

## <a name="set-services-startup-to-windows-default-values"></a>Festlegen des Starts von Diensten auf Windows-Standardwerte
Stellen Sie sicher, dass jeder der folgenden Windows-Dienste auf die **Windows-Standardwerte**festgelegt ist. Um die Starteinstellungen zurückzusetzen, führen Sie die folgenden Befehle aus:
   
```CMD
sc config bfe start= auto
   
sc config dcomlaunch start= auto
   
sc config dhcp start= auto
   
sc config dnscache start= auto
   
sc config IKEEXT start= auto
   
sc config iphlpsvc start= auto
   
sc config PolicyAgent start= demand
   
sc config LSM start= auto
   
sc config netlogon start= demand
   
sc config netman start= demand
   
sc config NcaSvc start= demand
   
sc config netprofm start= demand
   
sc config NlaSvc start= auto
   
sc config nsi start= auto
   
sc config RpcSs start= auto
   
sc config RpcEptMapper start= auto
   
sc config termService start= demand
   
sc config MpsSvc start= auto
   
sc config WinHttpAutoProxySvc start= demand
   
sc config LanmanWorkstation start= auto
   
sc config RemoteRegistry start= auto
```

## <a name="update-remote-desktop-registry-settings"></a>Aktualisieren der Remotedesktop-Registrierungseinstellungen
1. Wenn selbstsignierte Zertifikate vorhanden sind, die an den RDP-Listener (Remotedesktopprotokoll) gebunden sind, entfernen Sie diese:
   
    ```CMD
    REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash”
    ```
   
    Weitere Informationen zum Konfigurieren von Zertifikaten für den RDP-Listener finden Sie unter [Listener Certificate Configurations in Windows Server ](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/)
2. Konfigurieren Sie die [KeepAlive](https://technet.microsoft.com/library/cc957549.aspx) -Werte für den RDP-Dienst:
   
    ```CMD
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f
    ```
    ```CMD
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f
    ```
    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
    ```
3. Konfigurieren Sie den Authentifizierungsmodus für den RDP-Dienst:
   
    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f
   ```
    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f
   ```
    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
    ```
4. Aktivieren Sie den RDP-Dienst, indem Sie die folgenden Unterschlüssel zur Registrierung hinzufügen:
   
    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
    ```

## <a name="configure-windows-firewall-rules"></a>Konfigurieren von Windows-Firewallregeln
1. Führen Sie den folgenden Befehl in PowerShell aus, um WinRM in den drei Firewallprofilen (Domäne, privat und öffentlich) zuzulassen, und aktivieren Sie den PowerShell-Remotedienst:
   
   ```powershell
   Enable-PSRemoting -force
   ```
2. Führen Sie die folgenden Befehle im Eingabeaufforderungsfenster aus, um sicherzustellen, dass die folgenden Firewallregeln für das Gastbetriebssystem erfüllt sind:
   
   * Eingehend
   
   ```CMD
   netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes
   
   netsh advfirewall firewall set rule dir=in name="Network Discovery (LLMNR-UDP-In)" new enable=yes
   
   netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Datagram-In)" new enable=yes
   
   netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Name-In)" new enable=yes
   
   netsh advfirewall firewall set rule dir=in name="Network Discovery (Pub-WSD-In)" new enable=yes
   
   netsh advfirewall firewall set rule dir=in name="Network Discovery (SSDP-In)" new enable=yes
   
   netsh advfirewall firewall set rule dir=in name="Network Discovery (UPnP-In)" new enable=yes
   
   netsh advfirewall firewall set rule dir=in name="Network Discovery (WSD EventsSecure-In)" new enable=yes
   
   netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
   
   netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
   ```
   
   * Eingehend und ausgehend
   
   ```CMD
   netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes
   
   netsh advfirewall firewall set rule group="Core Networking" new enable=yes
   ```
   
   * Ausgehend
   
   ```CMD
   netsh advfirewall firewall set rule dir=out name="Network Discovery (LLMNR-UDP-Out)" new enable=yes
   
   netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Datagram-Out)" new enable=yes
   
   netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Name-Out)" new enable=yes
   
   netsh advfirewall firewall set rule dir=out name="Network Discovery (Pub-WSD-Out)" new enable=yes
   
   netsh advfirewall firewall set rule dir=out name="Network Discovery (SSDP-Out)" new enable=yes
   
   netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnPHost-Out)" new enable=yes
   
   netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnP-Out)" new enable=yes
   
   netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD Events-Out)" new enable=yes
   
   netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD EventsSecure-Out)" new enable=yes
   
   netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD-Out)" new enable=yes
   ```

## <a name="verify-vm-is-healthy-secure-and-accessible-with-rdp"></a>Sicherstellen, dass der virtuelle Computer fehlerfrei, sicher und der Zugriff mit RDP darauf möglich ist 
1. Führen Sie im Eingabeaufforderungsfenster `winmgmt /verifyrepository` aus, um zu bestätigen, dass das Repository für die Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI) konsistent ist. Wenn das Repository beschädigt ist, lesen Sie den Blogbeitrag [WMI: Repository Corruption, or Not?](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not) (WMI: Repository beschädigt oder nicht?)
2. Legen Sie die Startkonfigurationsdaten (Boot Configuration Data, BCD) fest:
   
   ```CMD
   bcdedit /set {bootmgr} integrityservices enable
   
   bcdedit /set {default} device partition=C:
   
   bcdedit /set {default} integrityservices enable
   
   bcdedit /set {default} recoveryenabled Off
   
   bcdedit /set {default} osdevice partition=C:
   
   bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
   ```
3. Entfernen Sie jegliche zusätzlichen Transport Driver Interface-Filter, wie beispielsweise Software, die TCP-Pakete analysiert.
4. Um sicherzustellen, dass der Datenträger fehlerfrei und konsistent ist, führen Sie den `CHKDSK /f`-Befehl im Eingabeaufforderungsfenster aus. Geben Sie „J“ ein, um die Überprüfung zu planen, und starten Sie den virtuellen Computer neu.
5. Deinstallieren Sie jegliche Drittanbietersoftware und -treiber im Zusammenhang mit physischen Komponenten oder einer anderen Virtualisierungstechnologie.
6. Stellen Sie sicher, dass keine Drittanbieteranwendung Port 3389 verwendet. Dieser Port wird für den RDP-Dienst in Azure verwendet. Sie können `netstat -anob` im Eingabeaufforderungsfenster ausführen, um die Ports anzuzeigen, die von den Anwendungen verwendet werden.
7. Wenn es sich bei der Windows-VHD, die Sie hochladen möchten, um einen Domänencontroller handelt, führen Sie [diese zusätzlichen Schritte](https://support.microsoft.com/kb/2904015) aus, um den Datenträger vorzubereiten.
8. Starten Sie den virtuellen Computer neu, um sicherzustellen, dass Windows weiterhin fehlerfrei ausgeführt wird und über die RDP-Verbindung erreichbar ist.
9. Setzen Sie das aktuelle Kennwort des lokalen Administrators zurück, und stellen Sie sicher, dass Sie sich mit diesem Konto über die RDP-Verbindung bei Windows anmelden können. Diese Zugriffsberechtigung wird über das Gruppenrichtlinienobjekt „Anmelden über Remotedesktopdienste zulassen“ gesteuert. Sie können dieses Objekt im Editor für lokale Gruppenrichtlinien unter „Computerkonfiguration\Windows-Einstellungen\Sicherheitseinstellungen\Lokale Richtlinien\Zuweisen von Benutzerrechten“ sehen.

## <a name="install-windows-updates"></a>Installieren von Windows-Updates
Installieren Sie die neuesten Updates für Windows. Wenn dies nicht möglich ist, stellen Sie sicher, dass die folgenden Updates installiert sind:
   
   * [KB3137061](https://support.microsoft.com/kb/3137061) Virtuelle Microsoft Azure-Computer können nach einem Netzwerkausfall nicht wiederhergestellt werden, und es treten Probleme mit beschädigten Daten auf
   * [KB3115224](https://support.microsoft.com/kb/3115224) Verbesserte Zuverlässigkeit für virtuelle Computer, die auf einem Windows Server 2012 R2- oder Windows Server 2012-Host ausgeführt werden
   * [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Sicherheitsupdate für Microsoft Windows zum Unterbinden von Rechteerweiterungen: 8. März 2016
   * [KB3063075](https://support.microsoft.com/kb/3063075) Es werden viele Ereignisse mit ID 129 protokolliert, wenn ein virtueller Windows Server 2012 R2-Computer in Microsoft Azure ausgeführt wird
   * [KB3137061](https://support.microsoft.com/kb/3137061) Virtuelle Microsoft Azure-Computer können nach einem Netzwerkausfall nicht wiederhergestellt werden, und es treten Probleme mit beschädigten Daten auf
   * [KB3114025](https://support.microsoft.com/kb/3114025) Geringe Leistung beim Zugriff auf den Azure-Dateispeicher über Windows 8.1 oder Server 2012 R2
   * [KB3033930](https://support.microsoft.com/kb/3033930) Hotfix erhöht die RIO-Puffergrenze von 64K pro Prozess für Azure-Dienste in Windows
   * [KB3004545](https://support.microsoft.com/kb/3004545) Sie können über eine VPN-Verbindung in Windows nicht auf virtuelle Computer zugreifen, die in Azure-Hostingdiensten gehostet werden
   * [KB3082343](https://support.microsoft.com/kb/3082343) Standortübergreifende VPN-Konnektivität geht verloren, wenn Azure-Standort-zu-Standort-VPN-Tunnel Windows Server 2012 R2 RRAS verwenden
   * [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Sicherheitsupdate für Microsoft Windows zum Unterbinden von Rechteerweiterungen: 8. März 2016
   * [KB3146723](https://support.microsoft.com/kb/3146723) MS16-048: Hinweise zum Sicherheitsupdate für CSRSS: 12. April 2016
   * [KB2904100](https://support.microsoft.com/kb/2904100) System friert während Datenträger-E/A-Vorgängen in Windows ein
     
## Ausführen von Sysprep <a id="step23"></a>    
Wenn Sie ein Image erstellen möchten, mit dem Sie mehrere virtuelle Computer bereitstellen können, müssen Sie [das Image durch Ausführen von Sysprep](generalize-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) verallgemeinern, bevor Sie die VHD in Azure hochladen. Sie müssen Sysprep nicht ausführen, um eine spezielle VHD zu verwenden. Weitere Informationen finden Sie in den folgenden Artikeln:
   
   * [Verallgemeinern eines virtuellen Windows-Computers mit Sysprep](generalize-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
   * [Sysprep-Unterstützung für Serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

## <a name="complete-recommended-configurations"></a>Abschließen empfohlener Konfigurationen
Die folgenden Einstellungen wirken sich nicht auf das Hochladen von VHDs aus. Es wird jedoch dringend empfohlen, diese Einstellungen zu konfigurieren.

* Installieren Sie den [Azure-Agent für virtuelle Computer](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Nach der Installation des Agents können Sie VM-Erweiterungen aktivieren. Die VM-Erweiterungen implementieren die meisten der wichtigen Funktionen, die Sie für Ihre virtuellen Computer verwenden möchten, darunter das Zurücksetzen von Kennwörtern, das Konfigurieren von RDP und viele andere.
* Das Abbildprotokoll kann bei der Problembehandlung nach Windows-Abstürzen hilfreich sein. Aktivieren Sie die Sammlung von Abbildprotokollen:
  
    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`
  
    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f
  
    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f
  
    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f
  
    sc config wer start= auto
    ```
* Nachdem der virtuelle Computer in Azure erstellt wurde, konfigurieren Sie die Auslagerungsdatei mit systemdefinierter Größe auf Laufwerk D:
  
    ```CMD
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
    ```

## <a name="next-steps"></a>Nächste Schritte
* [Hochladen eines Windows-VM-Images an Azure für Resource Manager-Bereitstellungen](upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)


