<properties
	pageTitle="Vorbereiten einer Windows-VHD für das Hochladen in Azure | Microsoft Azure"
	description="Empfohlene Vorgehensweisen zum Vorbereiten einer Windows-VHD vor dem Hochladen in Azure."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="genlin"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/18/2016"
	ms.author="glimoli;genli"/>

# Vorbereiten einer Windows-VHD für das Hochladen in Azure
Um einen virtuellen Windows-Computer aus einem lokalen Speicherort in Azure hochzuladen, müssen Sie die virtuelle Festplatte (Virtual Hard Disk, VHD) ordnungsgemäß vorbereiten. Es gibt eine Reihe von Schritten, die Sie ausführen sollten, bevor Sie eine VHD in Azure hochladen. Dieser Artikel zeigt Ihnen, wie Sie eine Windows-VHD für das Hochladen in Microsoft Azure vorbereiten, und erläutert, [wann und wie Sie Sysprep verwenden](#step23).

## Vorbereiten des virtuellen Datenträgers

>[AZURE.NOTE] Das modernere VHDX-Format wird in Azure noch nicht unterstützt. Die VHD muss eine feste Größe aufweisen, keine dynamische. Folgen Sie bei Bedarf den unten stehenden Anweisungen, um eine VHDX oder einen dynamischen Datenträger zu konvertieren. Die maximal zulässige Größe für die virtuelle Festplatte beträgt 1.023 GB.

Stellen Sie sicher, dass die Windows-VHD auf dem lokalen Server ordnungsgemäß funktioniert. Beheben Sie alle Probleme auf dem virtuellen Computer selbst, bevor Sie versuchen, ihn zu konvertieren oder in Azure hochzuladen.

Wenn Sie Ihren virtuellen Datenträger in das für Azure erforderliche Format konvertieren müssen, verwenden Sie eine der in den folgenden Abschnitten beschriebenen Methoden. Sichern Sie den virtuellen Computer vor dem Ausführen von Prozessen zur Konvertierung virtueller Datenträger oder Sysprep.

### Konvertieren mithilfe des Hyper-V-Managers
- Öffnen Sie den Hyper-V-Manager, und wählen Sie auf der linken Seite Ihren lokalen Computer aus. Klicken Sie im Menü darüber auf **Aktion** > **Datenträger bearbeiten**.
	- Navigieren Sie auf dem Bildschirm **Virtuelle Festplatte suchen** zu Ihrem virtuellen Datenträger, und wählen Sie ihn aus.
	- Wählen Sie auf dem nächsten Bildschirm **Konvertieren** aus.
		- Wenn Sie eine VHDX konvertieren müssen, wählen Sie **VHD** aus, und klicken Sie auf **Weiter**.
		- Wenn Sie einen dynamischen Datenträger konvertieren müssen, wählen Sie **Feste Größe** aus, und klicken Sie auf **Weiter**.

	- Navigieren Sie zu **Pfad für die neue VHD-Datei**, und wählen Sie den Pfad aus.
	- Klicken Sie zum Abschluss auf **Fertig stellen**.

### Konvertieren mithilfe von PowerShell
Sie können einen virtuellen Datenträger mithilfe des [PowerShell-Cmdlets „Convert-VHD“](http://technet.microsoft.com/library/hh848454.aspx) konvertieren. Im folgenden Beispiel wird eine VHDX in eine VHD und ein dynamischer Datenträger in einen Datenträger fester Größe konvertiert:

```powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

### Konvertieren des VMware-VMDK-Datenträgerformats
Wenn Sie über ein Windows-VM-Image im [VMDK-Dateiformat](https://en.wikipedia.org/wiki/VMDK) verfügen, konvertieren Sie es mit dem [Microsoft Virtual Machine Converter](https://www.microsoft.com/download/details.aspx?id=42497) in eine virtuelle Festplatte. Weitere Informationen finden Sie im Blog [How to Convert a VMWare VMDK to Hyper-V VHD](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) (Konvertieren von VMWare-VMDK in Hyper-V-VHD).

## Vorbereiten der Windows-Konfiguration für das Hochladen

> [AZURE.NOTE] Führen Sie alle folgenden Befehle mit [Administratorrechten](https://technet.microsoft.com/library/cc947813.aspx) aus.

1. Entfernen Sie alle statischen persistenten Routen aus der Routingtabelle:

	- Um die Routentabelle anzuzeigen, führen Sie `route print` aus.
	- Überprüfen Sie die Abschnitte mit **Persistenzrouten**. Wenn eine persistente Route vorhanden ist, verwenden Sie [route delete](https://technet.microsoft.com/library/cc739598.apx), um die Route zu entfernen.

2. Entfernen Sie den WinHTTP-Proxy:

	```
	netsh winhttp reset proxy
	```

3. Konfigurieren Sie die Datenträger-SAN-Richtlinie als [Onlineall](https://technet.microsoft.com/library/gg252636.aspx):

	```
	diskpart san policy=onlineall
	```

4. Verwenden Sie die koordinierte Weltzeit (UTC, Coordinated Universal Time) für Windows, und legen Sie den Starttyp des Windows-Zeitdiensts (w32time) auf **Automatisch** fest:

	```
	REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
	sc config w32time start= auto
	```


## Konfigurieren von Windows-Diensten
5. Stellen Sie sicher, dass jeder der folgenden Windows-Dienste auf die **Windows-Standardwerte** festgelegt ist. Sie sind mit den in der folgenden Liste aufgeführten Starteinstellungen konfiguriert. Sie können diese Befehle ausführen, um die Starteinstellungen zurückzusetzen:

	```
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


## Remotedesktopkonfiguration
6. Wenn selbstsignierte Zertifikate vorhanden sind, die an den RDP-Listener (Remotedesktopprotokoll) gebunden sind, entfernen Sie diese:

	```
	REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash”
	```

	Weitere Informationen zum Konfigurieren von Zertifikaten für den RDP-Listener finden Sie unter [Listener Certificate Configurations in Windows Server](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/) (Konfigurationen für Listenerzertifikate in Windows Server).

7. Konfigurieren Sie die [KeepAlive](https://technet.microsoft.com/library/cc957549.aspx)-Werte für den RDP-Dienst:

	```
	REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f

	REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f

	REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
	```

8. Konfigurieren Sie den Authentifizierungsmodus für den RDP-Dienst:

	```
	REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f

	REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f

	REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
	```

9. Aktivieren Sie den RDP-Dienst, indem Sie die folgenden Unterschlüssel zur Registrierung hinzufügen:

	```
	REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
	```


## Konfigurieren von Windows-Firewallregeln
10. Lassen Sie WinRM in den drei Firewallprofilen zu (Domäne, privat und öffentlich), und aktivieren Sie den PowerShell-Remotedienst:

	```
	Enable-PSRemoting -force
	```

11. Stellen Sie sicher, dass die folgenden Firewallregeln für Gastbetriebssysteme eingerichtet sind:

	- Eingehend

	```
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

	- Eingehend und ausgehend

	```
	netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes

	netsh advfirewall firewall set rule group="Core Networking" new enable=yes
	```

	- Ausgehend

	```
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


## Zusätzliche Konfigurationsschritte für Windows
12. Führen Sie `winmgmt /verifyrepository` aus, um zu bestätigen, dass das Repository für die Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI) konsistent ist. Wenn das Repository beschädigt ist, informieren Sie sich in [diesem Blogbeitrag](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

13. Stellen Sie sicher, dass die Einstellungen für die Startkonfigurationsdaten (Boot Configuration Data, BCD) den folgenden entsprechen:

	```
	bcdedit /set {bootmgr} integrityservices enable

	bcdedit /set {default} device partition=C:

	bcdedit /set {default} integrityservices enable

	bcdedit /set {default} recoveryenabled Off

	bcdedit /set {default} osdevice partition=C:

	bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
	```

14. Entfernen Sie jegliche zusätzlichen Transport Driver Interface-Filter, wie beispielsweise Software, die TCP-Pakete analysiert.
15. Um sicherzustellen, dass der Datenträger fehlerfrei und konsistent ist, führen Sie den `CHKDSK /f`-Befehl aus.
16.	Deinstallieren Sie jegliche Drittanbietersoftware und -treiber im Zusammenhang mit physischen Komponenten oder einer anderen Virtualisierungstechnologie.
17. Stellen Sie sicher, dass keine Drittanbieteranwendung Port 3389 verwendet. Dieser Port wird für den RDP-Dienst in Azure verwendet. Sie können den `netstat -anob`-Befehl verwenden, um die Ports zu überprüfen, die von den Anwendungen genutzt werden.
18.	Wenn es sich bei der Windows-VHD, die Sie hochladen möchten, um einen Domänencontroller handelt, führen Sie [diese zusätzlichen Schritte](https://support.microsoft.com/kb/2904015) aus, um den Datenträger vorzubereiten.
19.	Starten Sie den virtuellen Computer neu, um sicherzustellen, dass Windows weiterhin fehlerfrei ausgeführt wird und über die RDP-Verbindung erreichbar ist.
20.	Setzen Sie das aktuelle Kennwort des lokalen Administrators zurück, und stellen Sie sicher, dass Sie sich mit diesem Konto über die RDP-Verbindung bei Windows anmelden können. Diese Zugriffsberechtigung wird über das Richtlinienobjekt „Anmelden über Remotedesktopdienste zulassen“ gesteuert. Dieses Objekt befindet sich unter „Computerkonfiguration\\Windows-Einstellungen\\Sicherheitseinstellungen\\Lokale Richtlinien\\Zuweisen von Benutzerrechten“.


## Installieren von Windows-Updates
22. Installieren Sie die neuesten Updates für Windows. Wenn dies nicht möglich ist, stellen Sie sicher, dass die folgenden Updates installiert sind:

	- [KB3137061](https://support.microsoft.com/kb/3137061) Virtuelle Microsoft Azure-Computer können nach einem Netzwerkausfall nicht wiederhergestellt werden, und es treten Probleme mit beschädigten Daten auf

	- [KB3115224](https://support.microsoft.com/kb/3115224) Verbesserte Zuverlässigkeit für virtuelle Computer, die auf einem Windows Server 2012 R2- oder Windows Server 2012-Host ausgeführt werden

	- [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Sicherheitsupdate für Microsoft Windows zum Unterbinden von Rechteerweiterungen: 8. März 2016

	- [KB3063075](https://support.microsoft.com/kb/3063075) Es werden viele Ereignisse mit ID 129 protokolliert, wenn ein virtueller Windows Server 2012 R2-Computer in Microsoft Azure ausgeführt wird

	- [KB3137061](https://support.microsoft.com/kb/3137061) Virtuelle Microsoft Azure-Computer können nach einem Netzwerkausfall nicht wiederhergestellt werden, und es treten Probleme mit beschädigten Daten auf

	- [KB3114025](https://support.microsoft.com/kb/3114025) Geringe Leistung beim Zugriff auf den Azure-Dateispeicher über Windows 8.1 oder Server 2012 R2

	- [KB3033930](https://support.microsoft.com/kb/3033930) Hotfix erhöht die RIO-Puffergrenze von 64K pro Prozess für Azure-Dienste in Windows

	- [KB3004545](https://support.microsoft.com/kb/3004545) Sie können über eine VPN-Verbindung in Windows nicht auf virtuelle Computer zugreifen, die in Azure-Hostingdiensten gehostet werden

	- [KB3082343](https://support.microsoft.com/kb/3082343) Standortübergreifende VPN-Konnektivität geht verloren, wenn Azure-Standort-zu-Standort-VPN-Tunnel Windows Server 2012 R2 RRAS verwenden

	- [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Sicherheitsupdate für Microsoft Windows zum Unterbinden von Rechteerweiterungen: 8. März 2016

	- [KB3146723](https://support.microsoft.com/kb/3146723) MS16-048: Hinweise zum Sicherheitsupdate für CSRSS: 12. April 2016
	- [KB2904100](https://support.microsoft.com/kb/2904100) System friert während E/A-Vorgängen in Windows ein <a id="step23"></a>
23. Wenn Sie ein Image erstellen möchten, mit dem Sie mehrere Computer bereitstellen können, müssen Sie `sysprep` ausführen, um das Image zu generalisieren, bevor Sie die VHD in Azure hochladen. Sie müssen `sysprep` nicht ausführen, um eine spezielle VHD zu verwenden. Weitere Informationen zum Erstellen eines generalisierten Images finden Sie in den folgenden Artikeln:

	- [Erstellen eines VM-Images aus einem vorhandenen virtuellen Azure-Computer mithilfe des Resource Manager-Bereitstellungsmodells](virtual-machines-windows-capture-image.md)
	- [Erstellen eines VM-Images aus einem vorhandenen virtuellen Azure-Computer mithilfe des klassischen Bereitstellungsmodells](virtual-machines-windows-classic-capture-image.md)
	- [Sysprep-Unterstützung für Serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)



## Empfohlene zusätzliche Konfigurationen

Die folgenden Einstellungen wirken sich nicht auf das Hochladen von VHDs aus. Es wird jedoch dringend empfohlen, diese Einstellungen zu konfigurieren.

- Installieren Sie den [Azure-Agent für virtuelle Computer](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Nach der Installation des Agents können Sie VM-Erweiterungen aktivieren. Die VM-Erweiterungen implementieren die meisten der wichtigen Funktionen, die Sie für Ihre virtuellen Computer verwenden möchten, darunter das Zurücksetzen von Kennwörtern, das Konfigurieren von RDP und viele andere.

- Das Abbildprotokoll kann bei der Problembehandlung nach Windows-Abstürzen hilfreich sein. Aktivieren Sie die Sammlung von Abbildprotokollen:

	```
	REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`

	REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f

	REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f

	REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f

	sc config wer start= auto
	```

- Nachdem der virtuelle Computer in Azure erstellt wurde, konfigurieren Sie die Auslagerungsdatei mit systemdefinierter Größe auf Laufwerk D:

	```
	REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
	```


## Nächste Schritte

- [Hochladen eines Windows-VM-Images an Azure für Resource Manager-Bereitstellungen](virtual-machines-windows-upload-image.md)

<!---HONumber=AcomDC_0921_2016-->