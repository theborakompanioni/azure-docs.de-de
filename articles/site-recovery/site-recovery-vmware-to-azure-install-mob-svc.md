---
title: Installieren von Mobility Service (VMware/physisch in Azure) | Microsoft-Dokumentation
description: "Dieser Artikel beschreibt, wie der Mobility Service-Agent auf Ihren lokalen Computern installiert wird, um sie zu schützen."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 2/20/2017
ms.author: anoopkv
translationtype: Human Translation
ms.sourcegitcommit: 1e6ae31b3ef2d9baf578b199233e61936aa3528e
ms.openlocfilehash: 7e82ac74a8aef4e3cc8aff4dea3c572dcb9d9c40
ms.lasthandoff: 03/03/2017

---

# <a name="installing-mobility-service-vmwarephysical-to-azure"></a>Installieren von Mobility Service (VMware/physisch in Azure)
Mobility Service muss auf jedem Computer (VMware-VM oder physischer Server) bereitgestellt werden, den Sie in Azure replizieren möchten. Er erfasst die Datenschreibvorgänge auf dem Computer und sendet sie an den Prozessserver. Mobility Service kann mit den folgenden Methoden auf Servern bereitgestellt werden, die Schutz erfordern


1. [Installieren von Mobility Service mit Softwarebereitstellungstools wie System Center Configuration Manager](site-recovery-install-mobility-service-using-sccm.md)
2. [Installieren von Mobility Service mit Azure Automation und der Konfiguration des gewünschten Zustands (Desired State Configuration, DSC)](site-recovery-automate-mobility-service-install.md)
3. [Manuelles Installieren von Mobility Service mithilfe der grafischen Benutzeroberfläche (Graphical User Interface, GUI)](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-using-the-graphical-user-interface)
4. [Manuelles Installieren von Mobility Service über die Befehlszeile](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-using-command-line)
5. [Installieren von Mobility Service mithilfe der Pushinstallation in Azure Site Recovery](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-using-push-install-from-azure-site-recovery)


>[!IMPORTANT]
> Ab Version 9.7.0.0 wird mit dem Mobility Service-Installationsprogramm auf virtuellen Windows-Computern auch der jeweils neueste verfügbare [Azure-VM-Agent](../virtual-machines/virtual-machines-windows-extensions-features.md#azure-vm-agent) installiert. Dadurch wird sichergestellt, dass bei einem Failover für den Computer in Azure diese für die Verwendung von VM-Erweiterungen erforderliche Voraussetzung erfüllt wird.

## <a name="prerequisites"></a>Voraussetzungen
Führen Sie diese erforderlichen Schritte vor dem Beginn der Mobility Service-Installation manuell auf Ihren Servern aus.
1. Melden Sie sich bei Ihrem Konfigurationsserver an, und öffnen Sie eine Eingabeaufforderung mit Administratorberechtigungen.
2. Wechseln Sie in den Ordner „bin“, und erstellen Sie eine Passphrasedatei.

  ```
  cd %ProgramData%\ASR\home\svsystems\bin
  genpassphrase.exe -v > MobSvc.passphrase
  ```
3. Speichern Sie diese Datei an einem sicheren Ort, da wir sie während der Mobility Service-Installation verwenden müssen.
4. Die Mobility Service-Installationsprogramme für die unterstützten Betriebssysteme finden Sie in folgendem Verzeichnis:     

  `%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository`

#### <a name="mobility-service-installer-to-operating-system-mapping"></a>Zuordnung von Mobility Service-Installationsprogrammen zu Betriebssystemen

| Vorlagenname des Installationsprogramms| Betriebssystem |
|---|--|
|Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2008 R2 (64 Bit) SP1</br> Windows Server 2012 (64 Bit) </br> Windows Server 2012 R2 (64 Bit) |
|Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz| RHEL 6.4, 6.5, 6.6, 6.7, 6.8 (nur 64 Bit) </br> CentOS 6.4, 6.5, 6.6, 6.7. 6.8 (nur&64; Bit) |
|Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 (nur 64 Bit)|
|Microsoft-ASR_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5 (nur 64 Bit)|


## <a name="install-mobility-service-manually-using-the-graphical-user-interface"></a>Manuelles Installieren von Mobility Service mithilfe der grafischen Benutzeroberfläche

>[!NOTE]
> Die Installation über die grafische Benutzeroberfläche wird nur für Microsoft Windows-Betriebssysteme unterstützt.

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-using-command-line"></a>Manuelles Installieren von Mobility Service über die Befehlszeile
### <a name="command-line-based-install-on-windows-computers"></a>Installation über die Befehlszeile auf Windows-Computern
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-based-install-on-linux-computers"></a>Installation über die Befehlszeile auf Linux-Computern
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-using-push-install-from-azure-site-recovery"></a>Installieren von Mobility Service mithilfe der Pushinstallation in Azure Site Recovery
Um die Pushinstallation von Mobility Service mithilfe von Azure Site Recovery auszuführen, müssen Sie sicherstellen, dass auf allen Zielcomputern die folgenden Voraussetzungen erfüllt sind.

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
Nach der Installation von Mobility Service können Sie die Schaltfläche **+Replizieren** im Azure-Portal verwenden, um den Schutz für diese virtuellen Computer zu aktivieren.

## <a name="uninstall-mobility-service-on-windows-servers"></a>Deinstallieren von Mobility Service auf Windows-Servern
Es gibt zwei Möglichkeiten, wie Sie Mobility Service auf einem Windows-Server deinstallieren können

### <a name="uninstall-using-graphical-user-interface"></a>Deinstallieren mithilfe der grafischen Benutzeroberfläche
1. Öffnen Sie „Systemsteuerung“ > „Programme“
2. Wählen Sie **Microsoft Azure Site Recovery Mobility Service/Masterzielserver** aus, und klicken Sie auf „Deinstallieren“.

### <a name="uninstall-using-command-line"></a>Deinstallieren mithilfe der Befehlszeile
1. Öffnen Sie eine Administratoreingabeaufforderung
2. Führen Sie den folgenden Befehl zum Deinstallieren von Mobility Service aus.

```
MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
```

## <a name="uninstall-mobility-service-on-linux-computers"></a>Deinstallieren von Mobility Service auf Linux-Computern
1. Melden Sie sich als **Root-Benutzer** auf dem Linux-Server an
2. Navigieren Sie in einem **Terminal** zu /user/local/ASR
3. Führen Sie den folgenden Befehl zum Deinstallieren von Mobility Service aus

```
uninstall.sh -Y
```

