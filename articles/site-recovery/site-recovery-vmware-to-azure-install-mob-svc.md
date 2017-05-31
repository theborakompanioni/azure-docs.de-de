---
title: Installieren von Mobility Service (VMware oder physisch in Azure) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Mobility Service-Agent zum Schutz Ihrer lokalen Computer installieren.
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
ms.date: 5/11/2017
ms.author: anoopkv
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: 6e91a8b21fe7bfe2c6944cfa1c10d835a75ddd13
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017

---

# <a name="install-mobility-service-vmware-or-physical-to-azure"></a>Installieren von Mobility Service (VMware oder physisch in Azure)
Azure Site Recovery Mobility Service erfasst Datenschreibvorgänge auf einem Computer und leitet sie dann an den Prozessserver weiter. Stellen Sie Mobility Service auf jedem Computer (VMware-VM oder physischer Server) bereit, den Sie in Azure replizieren möchten. Sie können die Mobility Service auf den Servern bereitstellen, die Sie mithilfe der folgenden Methoden schützen möchten:


* [Installieren von Mobility Service mit Softwarebereitstellungstools wie System Center Configuration Manager](site-recovery-install-mobility-service-using-sccm.md)
* [Installieren von Mobility Service mit Azure Automation-Konfiguration für den gewünschten Zustand (Desired State Configuration, DSC)](site-recovery-automate-mobility-service-install.md)
* [Manuelles Installieren von Mobility Service mithilfe der grafischen Benutzeroberfläche (Graphical User Interface, GUI)](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui)
* [Manuelles Installieren von Mobility Service über eine Eingabeaufforderung](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt)
* [Installieren von Mobility Service mithilfe der Pushinstallation in Azure Site Recovery](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> Ab Version 9.7.0.0 wird mit dem Mobility Service-Installationsprogramm auf virtuellen Windows-Computern (VMs) auch der jeweils neueste verfügbare [Azure-VM-Agent](../virtual-machines/windows/extensions-features.md#azure-vm-agent) installiert. Wenn ein Computer ein Failover zu Azure ausführt, erfüllt der Computer die Agent-Installationsvoraussetzungen für die Verwendung beliebiger VM-Erweiterungen.

## <a name="prerequisites"></a>Voraussetzungen
Führen Sie diese erforderlichen Schritte aus, bevor Sie Mobility Service manuell auf Ihrem Server installieren:
1. Melden Sie sich bei Ihrem Konfigurationsserver an, und öffnen Sie ein Eingabeaufforderungsfenster als Administrator.
2. Wechseln Sie in das Verzeichnis „bin“, und erstellen Sie dann eine Passphrasedatei:

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Speichern Sie die Passphrase an einem sicheren Speicherort. Sie verwenden die Datei während der Installation von Mobility Service.
4. Die Mobility Service-Installationsprogramme für alle unterstützten Betriebssysteme befinden sich im Ordner „%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository“.

### <a name="mobility-service-installer-to-operating-system-mapping"></a>Zuordnung von Mobility Service-Installationsprogrammen zu Betriebssystemen

| Vorlagenname des Installationsprogramms| Betriebssystem |
|---|--|
|Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2008 R2 SP1 (64 Bit) </br> Windows Server 2012 (64 Bit) </br> Windows Server 2012 R2 (64 Bit) |
|Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz| Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8 (nur 64 Bit) </br> CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (nur 64 Bit) |
|Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (nur 64 Bit) </br> CentOS 7.0, 7.1, 7.2 (nur 64 Bit)</br> CentOs 7.3 (nur Migration) |
|Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 (nur 64 Bit)|
|Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 (nur 64 Bit)|
|Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5 (nur 64 Bit)|
|Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04 (nur 64 Bit)|


## <a name="install-mobility-service-manually-by-using-the-gui"></a>Manuelles Installieren von Mobility Service über die grafische Benutzeroberfläche (GUI)

>[!IMPORTANT]
> Die GUI-basierte Installation funktioniert nur mit Windows-Betriebssystemen.
> Wenn Sie einen Konfigurationsserver zum Replizieren von Azure-IaaS-VMs von einem Azure-Abonnement/einer Azure-Region zu einem bzw. einer anderen nutzen, verwenden Sie die befehlszeilenbasierte Installationsmethode.

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a>Manuelles Installieren von Mobility Service über eine Eingabeaufforderung

### <a name="command-line-installation-on-a-windows-computer"></a>Befehlszeileninstallation auf einem Windows-Computer
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a>Befehlszeileninstallation auf einem Linux-Computer
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a>Installieren von Mobility Service mithilfe der Pushinstallation in Azure Site Recovery
Um eine Pushinstallation von Mobility Service mithilfe von Site Recovery auszuführen, müssen alle Zielcomputer die folgenden Voraussetzungen erfüllen.

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
Nachdem Mobility Service installiert wurde, klicken Sie im Azure-Portal auf die Schaltfläche **Replizieren**, um den Schutz dieser virtuellen Computer zu starten.

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a>Deinstallieren von Mobility Service auf einem Windows Server-Computer
Verwenden Sie eine der folgenden Methoden, um Mobility Service auf einem Windows Server-Computer zu deinstallieren.

### <a name="uninstall-by-using-the-gui"></a>Deinstallieren mithilfe der grafischen Benutzeroberfläche (GUI)
1. Wählen Sie in der Systemsteuerung **Programme** aus.
2. Wählen Sie **Microsoft Azure Site Recovery Mobility Service/Masterzielserver** und dann **Deinstallieren** aus.

### <a name="uninstall-at-a-command-prompt"></a>Deinstallieren über die Eingabeaufforderung
1. Öffnen Sie ein Eingabeaufforderungsfenster als ein Administrator.
2. Führen Sie zum Deinstallieren von Mobility Service den folgenden Befehl aus:

```
MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a>Deinstallieren von Mobility Service auf einem Linux-Computer
1. Melden Sie sich auf dem Linux-Server als ein **Root**-Benutzer an.
2. Navigieren Sie in einem Terminal zu „/user/local/ASR“.
3. Führen Sie zum Deinstallieren von Mobility Service den folgenden Befehl aus:

```
uninstall.sh -Y
```

