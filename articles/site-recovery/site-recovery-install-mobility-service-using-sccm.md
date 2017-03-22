---
title: "Automatisieren der Mobility Service-Installation für Azure Site Recovery mit Softwarebereitstellungstools | Microsoft-Dokumentation"
description: Dieser Artikel hilft Ihnen beim Automatisieren der Mobility Service-Installation mit Softwarebereitstellungstools wie System Center Configuration Manager.
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/10/2017
ms.author: anoopkv
translationtype: Human Translation
ms.sourcegitcommit: 1e6ae31b3ef2d9baf578b199233e61936aa3528e
ms.openlocfilehash: 310f2a2fe793601d22952bf516a812bf4867bbec
ms.lasthandoff: 03/03/2017

---
# <a name="automate-mobility-service-installation-using-software-deployment-tools"></a>Automatisieren der Mobility Service-Installation mit Softwarebereitstellungstools

Dieser Artikel zeigt Ihnen an einem Beispiel, wie System Center Configuration Manager (SCCM) verwendet wird, um Azure Site Recovery Mobility Service in Ihrem Datencenter bereitzustellen. Das Verwenden eines Softwarebereitstellungstools wie SCCM hat folgende Vorteile für Sie:
* Planen der Bereitstellung neuer Installationen und Upgrades während eines geplanten Wartungsfensters für Softwareupdates
* Gleichzeitiges Bereitstellen auf Hunderten von Servern


> [!NOTE]
> In diesem Artikel wird anhand von System Center Configuration Manager 2012 R2 die Bereitstellungsaktivität veranschaulicht. Sie können die Mobility Service-Installation auch mit [Azure Automation und der Konfiguration des gewünschten Zustands](site-recovery-automate-mobility-service-install.md) automatisieren.

## <a name="prerequisites"></a>Voraussetzungen
1. Ein Softwarebereitstellungstool wie System Center Configuration Manager(SCCM), das bereits in Ihrer Umgebung bereitgestellt wurde.
  * Erstellen Sie zwei [Gerätesammlungen](https://technet.microsoft.com/library/gg682169.aspx): eine für alle **Windows-Server** und eine für alle **Linux-Server**, die mithilfe von Azure Site Recovery geschützt werden sollen.
3. Ein Konfigurationsserver, der bereits bei Azure Site Recovery registriert ist.
4. Eine sichere Netzwerkdateifreigabe (SMB-Freigabe), auf die der SCCM-Server zugreifen kann.

## <a name="deploy-mobility-service-on-computers-running-microsoft-windows-operating-systems"></a>Bereitstellen von Mobility Service auf Computern unter Microsoft Windows-Betriebssystemen
> [!NOTE]
> In diesem Artikel wird Folgendes vorausgesetzt:
> 1. Die IP-Adresse des Konfigurationsservers ist 192.168.3.121
> 2. Die sichere Netzwerkdateifreigabe ist \\\ContosoSecureFS\MobilityServiceInstallers

### <a name="step-1-prepare-for-deployment"></a>Schritt 1: Vorbereiten der Bereitstellung
1. Erstellen Sie einen Ordner auf der Netzwerkfreigabe, und nennen Sie ihn **MobSvcWindows**.
2. Melden Sie sich bei Ihrem Konfigurationsserver an, und öffnen Sie eine Administratoreingabeaufforderung.
3. Führen Sie die folgenden Befehle aus, um eine Passphrasedatei zu generieren.

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
6. Kopieren Sie die Datei „MobSvc.passphrase“ in den Ordner „MobSvcWindows“ auf der Netzwerkfreigabe.
5. Wechseln Sie als Nächstes zum Installerrepository auf dem Konfigurationsserver, indem Sie den folgenden Befehl ausführen.

  `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`
6. Kopieren Sie **Microsoft ASR\_UA\_*Version*\_Windows\_GA\_*Datum*\_Release.exe** in den Ordner **MobSvcWindows** auf der Netzwerkfreigabe.
7. Kopieren Sie den unten dargestellten Code, und speichern Sie ihn als **install.bat** im Ordner **MobSvcWindows**.
> [!NOTE]
> Denken Sie daran, den Platzhalter [CSIP] im folgenden Skript durch die tatsächlichen Werte der IP-Adresse des Konfigurationsserver zu ersetzen.

```
Time /t >> C:\Temp\logfile.log
REM ==================================================
REM ==== Clean up the folders ========================
RMDIR /S /q %temp%\MobSvc
MKDIR %Temp%\MobSvc
REM ==================================================
REM ==== Copy new files ==============================
COPY M*.* %Temp%\MobSvc
CD %Temp%\MobSvc
REN Micro*.exe MobSvcInstaller.exe
REM ==================================================
REM ==== Extract the installer =======================
MobSvcInstaller.exe /q /x:%Temp%\MobSvc\Extracted
REM ==== Wait 10s for extraction to complete =========
TIMEOUT /t 10
REM =================================================
REM ==== Extract the installer ======================
CD %Temp%\MobSvc\Extracted
REM ==================================================
REM ==== Check if Mob Svc is already installed =======
REM ==== If not installed run install command ========
REM ==== Else run upgrade command =====================
REM ==== {275197FC-14FD-4560-A5EB-38217F80CBD1} is ====
REM ==== guid for Mob Svc Installer ====================
whoami >> C:\temp\logfile.log
REM SET PRODKEY=HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
REM REG QUERY %PRODKEY%\{275197FC-14FD-4560-A5EB-38217F80CBD1} >> C:\Temp\logfile.log 2>&1
REM REG QUERY %PRODKEY%\{275197FC-14FD-4560-A5EB-38217F80CBD1}
REM IF NOT %ERRORLEVEL% EQU 0 (GOTO :INSTALL) ELSE GOTO :UPDATE
NET START | FIND "InMage Scout Application Service"
IF  %ERRORLEVEL% EQU 1 (GOTO :INSTALL) ELSE GOTO :UPDATE
:INSTALL
    echo "Install" >> c:\Temp\logfile.log
     UnifiedAgent.exe /Role "Agent" /CSEndpoint "10.10.20.168" /PassphraseFilePath %Temp%\MobSvc\MobSvc.passphrase
GOTO :ENDSCRIPT
:UPDATE
    echo "Update" >> C:\Temp\logfile.log
    UnifiedAgent.exe /upgrade
:ENDSCRIPT

```

### <a name="step-2-create-a-package"></a>Schritt 2: Erstellen eines Pakets

1. Melden Sie sich bei Ihrer System Center Configuration Manager-Konsole an.
2. Navigieren Sie zu **Softwarebibliothek** > **Anwendungsverwaltung** > **Pakete**.
3. Klicken Sie mit der rechten Maustaste auf **Pakete**, und wählen Sie **Paket erstellen** aus.
4. Geben Sie Werte für Name, Beschreibung, Hersteller, Sprache, Version an.
5. Aktivieren Sie das Kontrollkästchen **Dieses Paket enthält Quelldateien**.
6. Klicken Sie auf die Schaltfläche **Durchsuchen**, und wählen Sie die Netzwerkfreigabe aus, in der der Installer gespeichert ist (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows)

  ![create-sccm-package](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package.png)

7. Wählen Sie auf der Seite **Typ des zu erstellenden Programms auswählen** die Option **Standardprogramm** aus, und klicken Sie auf **Weiter**

  ![create-sccm-package](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)
8. Geben Sie auf der Seite **Informationen zu diesem Standardprogramm angeben** die folgenden Eingaben an, und klicken Sie auf **Weiter**. (Bei den anderen Eingaben können Sie die Standardwerte übernehmen.)

  ![sccm-package-properties](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties.png)   
| **Parametername** | **Wert** |
|--|--|
| Name | Microsoft Azure Mobility Service installieren (Windows) |
| Befehlszeile | install.bat |
| Programm kann ausgeführt werden | Unabhängig von Benutzeranmeldung |
9. Wählen Sie auf der nächsten Seite die Zielbetriebssysteme aus. Mobility Service kann nur unter Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 installiert werden.

  ![sccm-package-properties-page2](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2.png)   
10. Klicken Sie zweimal auf „Weiter“, um den Assistenten abzuschließen.

> [!NOTE]
> Das Skript unterstützt Neuinstallationen von Mobility Service-Agents und Upgrades/Updates bereits installierter Agents.

### <a name="step-3-deploy-the-package"></a>Schritt 3: Bereitstellen des Pakets
1. Klicken Sie in der SCCM-Konsole mit der rechten Maustaste auf das Paket, und wählen Sie **Inhalt verteilen** aus.
  ![distribute-sccm-package](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Wählen Sie die ** [Verteilungspunkte](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints) ** aus, auf die die Pakete kopiert werden sollen.
3. Nachdem Sie den Assistenten abgeschlossen haben, beginnt die Replikation des Pakets auf die angegebenen Verteilungspunkte.
4. Klicken Sie nach Abschluss der Paketverteilung mit der rechten Maustaste auf das Paket, und wählen Sie **Bereitstellen** aus.
  ![deploy-sccm-package](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Wählen Sie die Gerätesammlung für Windows-Server, die Sie im Abschnitt „Voraussetzungen“ erstellt haben, als die Zielsammlung für die Bereitstellung aus.

  ![sccm-select-target-collection](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection.png)
6. Wählen Sie auf der Seite **Das Inhaltsziel angeben** Ihre **Verteilungspunkte** aus.
7. Stellen Sie auf der Seite **Geben Sie Einstellungen an, um die Bereitstellung dieser Software zu steuern** sicher, dass der gewünschte Zweck ausgewählt ist.

  ![sccm-deploy-select-purpose](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)
8. Geben Sie unter **Zeitplan für diese Bereitstellung angeben** einen Zeitplan an. Weitere Informationen finden Sie unter [Planen von Paketen](https://technet.microsoft.com/library/gg682178.aspx).
9. Konfigurieren Sie die Eigenschaften auf der Seite **Verteilungspunkte** nach den Anforderungen Ihres Datencenters, und schließen Sie den Assistenten ab.

> [!TIP]
> Um unnötige Neustarts zu vermeiden, planen Sie die Installation des Pakets während Ihrer monatlichen Wartungsfensters oder der Zeitfenster für Softwareupdates.

Sie können den Bereitstellungsstatus mithilfe der SCCM-Konsole überwachen, indem Sie zu **Überwachung** > **Bereitstellungen** > *[Paketname]* navigieren.
  ![monitor-sccm](./media/site-recovery-install-mobility-service-using-sccm/report.PNG)

## <a name="deploy-mobility-service-on-computers-running-linux-operating-systems"></a>Bereitstellen von Mobility Service auf Computern unter Linux-Betriebssystemen
> [!NOTE]
> In diesem Artikel wird Folgendes vorausgesetzt:
> 1. Die IP-Adresse des Konfigurationsservers ist 192.168.3.121
> 2. Die sichere Netzwerkdateifreigabe ist \\\ContosoSecureFS\MobilityServiceInstallers

### <a name="step-1-prepare-for-deployment"></a>Schritt 1: Vorbereiten der Bereitstellung
1. Erstellen Sie einen Ordner auf der Netzwerkfreigabe, und nennen Sie ihn **MobSvcLinux**.
2. Melden Sie sich bei Ihrem Konfigurationsserver an, und öffnen Sie eine Administratoreingabeaufforderung.
3. Führen Sie die folgenden Befehle aus, um eine Passphrasedatei zu generieren.

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
6. Kopieren Sie die Datei „MobSvc.passphrase“ in den Ordner „MobSvcWindows“ auf der Netzwerkfreigabe.
5. Wechseln Sie als Nächstes zum Installerrepository auf dem Konfigurationsserver, indem Sie den folgenden Befehl ausführen.

  `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`
6. Kopieren Sie die folgenden Dateien in den Ordner **MobSvcLinux** auf der Netzwerkfreigabe.
  * Microsoft-ASR\_UA\_*Version*\_OEL-64\_GA\_*Datum*\_Release.tar.gz
  * Microsoft-ASR\_UA\_*Version*\_RHEL6-64\_GA\_*Datum*\_Release.tar.gz
  * Microsoft-ASR\_UA\_*Version*\_RHEL7-64\_GA\_*Datum*\_Release.tar.gz
  * Microsoft-ASR\_UA\_*Version*\_SLES11-SP3-64\_GA\_*Datum*\_Release.tar.gz

7. Kopieren Sie den unten dargestellten Code, und speichern Sie ihn als **install_linux.sh** im Ordner **MobSvcLinux**.
> [!NOTE]
> Denken Sie daran, den Platzhalter [CSIP] im folgenden Skript durch die tatsächlichen Werte der IP-Adresse des Konfigurationsserver zu ersetzen.

```
#!/bin/sh

rm -rf /tmp/MobSvc

mkdir -p /tmp/MobSvc

if [ -f /etc/oracle-release ] && [ -f /etc/redhat-release ]; then
    if grep -q 'Oracle Linux Server release 6.*' /etc/oracle-release; then
        if uname -a | grep -q x86_64; then
            OS="OL6-64"
        cp *OL6*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/redhat-release ]; then
    if grep -q 'Red Hat Enterprise Linux Server release 6.* (Santiago)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 6.* (Final)' /etc/redhat-release || \
        grep -q 'CentOS release 6.* (Final)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL6-64"
            cp *RHEL6*.tar.gz /tmp/MobSvc
        fi
    elif grep -q 'Red Hat Enterprise Linux Server release 7.* (Maipo)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 7.* (Core)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL7-64"
            cp *RHEL7*.tar.gz /tmp/MobSvc
    fi
    fi
elif [ -f /etc/SuSE-release ] && grep -q 'VERSION = 11' /etc/SuSE-release; then
    if grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 3' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP3-64"
        echo $OS >> /tmp/MobSvc/sccm.log
        cp *SLES11*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/lsb-release ] ; then
    if grep -q 'DISTRIB_RELEASE=14.04' /etc/lsb-release ; then
       if uname -a | grep -q x86_64; then
           OS="UBUNTU-14.04-64"
       cp *UBUNTU*.tar.gz /tmp/MobSvc
       fi
    fi
else
    exit 1
fi
if [ "${OS}" ==  "" ]; then
    exit 1
fi
cp MobSvc.passphrase /tmp/MobSvc
cd /tmp/MobSvc

tar -zxvf *.tar.gz


if [ -e /usr/local/.vx_version ];
then
    ./install -A u
    echo "Errorcode:$?"
    Error=$?

else
    ./install -t both -a host -R Agent -d /usr/local/ASR -i [CS IP] -p 443 -s y -c https -P MobSvc.passphrase >> /tmp/MobSvc/sccm.log 2>&1 && echo "Install Progress"
    Error=$?
fi
cd /tmp
rm -rf /tm/MobSvc
exit ${Error}
```

### <a name="step-2-create-a-package"></a>Schritt 2: Erstellen eines Pakets

1. Melden Sie sich bei Ihrer System Center Configuration Manager-Konsole an.
2. Navigieren Sie zu **Softwarebibliothek** > **Anwendungsverwaltung** > **Pakete**.
3. Klicken Sie mit der rechten Maustaste auf **Pakete**, und wählen Sie **Paket erstellen** aus.
4. Geben Sie Werte für Name, Beschreibung, Hersteller, Sprache, Version an.
5. Aktivieren Sie das Kontrollkästchen **Dieses Paket enthält Quelldateien**.
6. Klicken Sie auf die Schaltfläche **Durchsuchen**, und wählen Sie die Netzwerkfreigabe aus, in der der Installer gespeichert ist (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux)

  ![create-sccm-package](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package-linux.png)

7. Wählen Sie auf der Seite **Typ des zu erstellenden Programms auswählen** die Option **Standardprogramm** aus, und klicken Sie auf **Weiter**

  ![create-sccm-package](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)
8. Geben Sie auf der Seite **Informationen zu diesem Standardprogramm angeben** die folgenden Eingaben an, und klicken Sie auf **Weiter**. (Bei den anderen Eingaben können Sie die Standardwerte übernehmen.)

  ![sccm-package-properties](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-linux.png)   
| **Parametername** | **Wert** |
|--|--|
| Name | Microsoft Azure Mobility Service installieren (Linux) |
| Befehlszeile | ./install_linux.sh |
| Programm kann ausgeführt werden | Unabhängig von Benutzeranmeldung |

9. Wählen Sie auf der nächsten Seite **Dieses Programm kann auf jeder Plattform ausgeführt werden** aus.
  ![sccm-package-properties-page2](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)   

10. Klicken Sie zweimal auf **Weiter**, um den Assistenten abzuschließen.
> [!NOTE]
> Das Skript unterstützt Neuinstallationen von Mobility Service-Agents und Upgrades/Updates bereits installierter Agents.

### <a name="step-3-deploy-the-package"></a>Schritt 3: Bereitstellen des Pakets
1. Klicken Sie in der SCCM-Konsole mit der rechten Maustaste auf das Paket, und wählen Sie **Inhalt verteilen** aus.
  ![distribute-sccm-package](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Wählen Sie die ** [Verteilungspunkte](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints) ** aus, auf die die Pakete kopiert werden sollen.
3. Nachdem Sie den Assistenten abgeschlossen haben, beginnt die Replikation des Pakets auf die angegebenen Verteilungspunkte.
4. Klicken Sie nach Abschluss der Paketverteilung mit der rechten Maustaste auf das Paket, und wählen Sie **Bereitstellen** aus.
  ![deploy-sccm-package](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Wählen Sie die Gerätesammlung für Linux-Server, die Sie im Abschnitt „Voraussetzungen“ erstellt haben, als die Zielsammlung für die Bereitstellung aus.

  ![sccm-select-target-collection](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection-linux.png)
6. Wählen Sie auf der Seite **Das Inhaltsziel angeben** Ihre **Verteilungspunkte** aus.
7. Stellen Sie auf der Seite **Geben Sie Einstellungen an, um die Bereitstellung dieser Software zu steuern** sicher, dass der gewünschte Zweck ausgewählt ist.

  ![sccm-deploy-select-purpose](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)
8. Geben Sie unter **Zeitplan für diese Bereitstellung angeben** einen Zeitplan an. Weitere Informationen finden Sie unter [Planen von Paketen](https://technet.microsoft.com/library/gg682178.aspx).
9. Konfigurieren Sie die Eigenschaften auf der Seite **Verteilungspunkte** nach den Anforderungen Ihres Datencenters, und schließen Sie den Assistenten ab.

Mobility Service wird auf den Linux-Servern der Gerätesammlung entsprechend dem konfigurierten Zeitplan installiert.

## <a name="other-methods-to-install-mobility-services"></a>Andere Methoden zum Installieren von Mobility Services
Informieren Sie sich über andere Möglichkeiten zum Installieren von Mobility Services.
* [Manuelle Installation mithilfe der GUI](http://aka.ms/mobsvcmanualinstall)
* [Manuelle Installation über die Befehlszeile](http://aka.ms/mobsvcmanualinstallcli)
* [Pushinstallation mithilfe eines Konfigurationsservers](http://aka.ms/pushinstall)
* [Automatisierte Installation mithilfe von Azure Automation und Konfiguration des gewünschten Zustands](http://aka.ms/mobsvcdscinstall)

## <a name="uninstall-mobility-service"></a>Mobility Service deinstallieren
Genau wie bei der Installation können Sie SCCM-Pakete erstellen, um Mobility Service zu deinstallieren. Verwenden Sie das unten aufgeführte Skript, um Mobility Service zu deinstallieren.

```
Time /t >> C:\logfile.log
REM ==================================================
REM ==== Check if Mob Svc is already installed =======
REM ==== If not installed no operation required ========
REM ==== Else run uninstall command =====================
REM ==== {275197FC-14FD-4560-A5EB-38217F80CBD1} is ====
REM ==== guid for Mob Svc Installer ====================
whoami >> C:\logfile.log
NET START | FIND "InMage Scout Application Service"
IF  %ERRORLEVEL% EQU 1 (GOTO :INSTALL) ELSE GOTO :UNINSTALL
:NOOPERATION
                echo "No Operation Required." >> c:\logfile.log
                GOTO :ENDSCRIPT
:UNINSTALL
                echo "Uninstall" >> C:\logfile.log
                MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
:ENDSCRIPT

```

## <a name="next-steps"></a>Nächste Schritte
Sie können jetzt [Schutz](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) für Ihre virtuellen Computer aktivieren.

