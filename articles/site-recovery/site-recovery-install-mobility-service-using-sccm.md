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
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: ded45356e6dd22b485adfe7f85ddd0abb21280e5
ms.lasthandoff: 04/03/2017

---
# <a name="automate-mobility-service-installation-by-using-software-deployment-tools"></a>Automatisieren der Mobility Service-Installation mit Softwarebereitstellungstools

Dieser Artikel zeigt Ihnen an einem Beispiel, wie System Center Configuration Manager verwendet wird, um Azure Site Recovery Mobility Service in Ihrem Rechenzentrum bereitzustellen. Das Verwenden eines Softwarebereitstellungstools wie Configuration Manager bietet Ihnen folgende Vorteile:
* Planen der Bereitstellung neuer Installationen und Upgrades während eines geplanten Wartungsfensters für Softwareupdates
* Skalierung der gleichzeitigen Bereitstellung auf Hunderten von Servern


> [!NOTE]
> In diesem Artikel wird anhand von System Center Configuration Manager 2012 R2 die Bereitstellungsaktivität veranschaulicht. Sie können die Mobility Service-Installation auch mit [Azure Automation und der Konfiguration des gewünschten Zustands](site-recovery-automate-mobility-service-install.md) automatisieren.

## <a name="prerequisites"></a>Voraussetzungen
1. Ein Softwarebereitstellungstool wie Configuration Manager, das bereits in Ihrer Umgebung bereitgestellt wurde.
  Erstellen Sie zwei [Gerätesammlungen](https://technet.microsoft.com/library/gg682169.aspx): eine für alle **Windows-Server** und eine für alle **Linux-Server**, die mithilfe von Site Recovery geschützt werden sollen.
3. Ein Konfigurationsserver, der bereits bei Site Recovery registriert ist.
4. Eine sichere Netzwerkdateifreigabe (Server Message Block-Freigabe), auf die der Configuration Manager-Server zugreifen kann.

## <a name="deploy-mobility-service-on-computers-running-windows"></a>Bereitstellen von Mobility Service auf Computern unter Windows
> [!NOTE]
> Dieser Artikel setzt voraus, dass die IP-Adresse des Konfigurationsservers 192.168.3.121 und die sichere Netzwerkdateifreigabe \\\ContosoSecureFS\MobilityServiceInstallers ist.

### <a name="step-1-prepare-for-deployment"></a>Schritt 1: Vorbereiten der Bereitstellung
1. Erstellen Sie einen Ordner auf der Netzwerkfreigabe, und nennen Sie ihn **MobSvcWindows**.
2. Melden Sie sich bei Ihrem Konfigurationsserver an, und öffnen Sie eine Administratoreingabeaufforderung.
3. Führen Sie die folgenden Befehle aus, um eine Passphrasedatei zu generieren:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopieren Sie die Datei **MobSvc.passphrase** in den Ordner **MobSvcWindows** auf der Netzwerkfreigabe.
5. Wechseln Sie zum Installerrepository auf dem Konfigurationsserver, indem Sie den folgenden Befehl ausführen:

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. Kopieren Sie **Microsoft ASR\_UA\_*Version*\_Windows\_GA\_*Datum*\_Release.exe** in den Ordner **MobSvcWindows** auf der Netzwerkfreigabe.
7. Kopieren Sie den folgenden Code, und speichern Sie ihn als **install.bat** im Ordner **MobSvcWindows**.

   > [!NOTE]
   > Ersetzen Sie den Platzhalter [CSIP] in diesem Skript durch die tatsächlichen Werte der IP-Adresse des Konfigurationsserver.

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

1. Melden Sie sich bei der Configuration Manager-Konsole an.
2. Navigieren Sie zu **Softwarebibliothek** > **Anwendungsverwaltung** > **Pakete**.
3. Klicken Sie mit der rechten Maustaste auf **Pakete**, und wählen Sie **Paket erstellen** aus.
4. Geben Sie Werte für Name, Beschreibung, Hersteller, Sprache und Version an.
5. Aktivieren Sie das Kontrollkästchen **Dieses Paket enthält Quelldateien**.
6. Klicken Sie auf die Schaltfläche **Durchsuchen**, und wählen Sie die Netzwerkfreigabe aus, in der der Installer gespeichert ist (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).

  ![Screenshot des Assistenten zum Erstellen von Paketen und Programmen](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package.png)

7. Wählen Sie auf der Seite **Typ des zu erstellenden Programms auswählen** die Option **Standardprogramm** aus, und klicken Sie auf **Weiter**.

  ![Screenshot des Assistenten zum Erstellen von Paketen und Programmen](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. Geben Sie auf der Seite **Informationen zu diesem Standardprogramm angeben** die folgenden Eingaben an, und klicken Sie auf **Weiter**. (Bei den anderen Eingaben können Sie die Standardwerte übernehmen.)
 
  | **Parametername** | **Wert** |
  |--|--|
  | Name | Microsoft Azure Mobility Service installieren (Windows) |
  | Befehlszeile | install.bat |
  | Programm kann ausgeführt werden | Unabhängig von Benutzeranmeldung |

  ![Screenshot des Assistenten zum Erstellen von Paketen und Programmen](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties.png)

9. Wählen Sie auf der nächsten Seite die Zielbetriebssysteme aus. Mobility Service kann nur unter Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2 installiert werden.

  ![Screenshot des Assistenten zum Erstellen von Paketen und Programmen](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2.png) 

10. Klicken Sie zweimal auf **Weiter**, um den Assistenten abzuschließen.


> [!NOTE]
> Das Skript unterstützt Neuinstallationen von Mobility Service-Agents und Updates bereits installierter Agents.

### <a name="step-3-deploy-the-package"></a>Schritt 3: Bereitstellen des Pakets
1. Klicken Sie in der Configuration Manager-Konsole mit der rechten Maustaste auf das Paket, und wählen Sie **Inhalt verteilen** aus.
  ![Screenshot der Configuration Manager-Konsole](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Wählen Sie die **[Verteilungspunkte](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** aus, auf die die Pakete kopiert werden sollen.
3. Schließen Sie den Assistenten ab. Dann beginnt die Replikation des Pakets auf die angegebenen Verteilungspunkte.
4. Klicken Sie nach Abschluss der Paketverteilung mit der rechten Maustaste auf das Paket, und wählen Sie **Bereitstellen** aus.
  ![Screenshot der Configuration Manager-Konsole](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Wählen Sie die Windows Server-Gerätesammlung, die Sie im Abschnitt „Voraussetzungen“ erstellt haben, als Zielsammlung für die Bereitstellung aus.

  ![Screenshot des Assistenten zum Bereitstellen von Software](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection.png)

6. Wählen Sie auf der Seite **Das Inhaltsziel angeben** Ihre **Verteilungspunkte** aus.
7. Stellen Sie auf der Seite **Geben Sie Einstellungen an, um die Bereitstellung dieser Software zu steuern** sicher, dass der gewünschte Zweck **Erforderlich** ist.

  ![Screenshot des Assistenten zum Bereitstellen von Software](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. Geben Sie auf der Seite **Zeitplan für diese Bereitstellung angeben** einen Zeitplan an. Weitere Informationen finden Sie unter [Bereitstellen von Paketen und Programmen in Configuration Manager](https://technet.microsoft.com/library/gg682178.aspx).
9. Konfigurieren Sie die Eigenschaften auf der Seite **Verteilungspunkte** nach den Anforderungen Ihres Rechenzentrums. Schließen Sie dann den Assistenten ab.

> [!TIP]
> Um unnötige Neustarts zu vermeiden, planen Sie die Installation des Pakets während Ihrer monatlichen Wartungsfensters oder der Zeitfenster für Softwareupdates.

Sie können den Bereitstellungsstatus mithilfe der Configuration Manager-Konsole überwachen. Wechseln Sie zu **Überwachung** > **Bereitstellungen** > *[Ihr Paketname]*.

  ![Screenshot der Configuration Manager-Option zum Überwachen von Bereitstellungen](./media/site-recovery-install-mobility-service-using-sccm/report.PNG)

## <a name="deploy-mobility-service-on-computers-running-linux"></a>Bereitstellen von Mobility Service auf Computern unter Linux
> [!NOTE]
> Dieser Artikel setzt voraus, dass die IP-Adresse des Konfigurationsservers 192.168.3.121 und die sichere Netzwerkdateifreigabe \\\ContosoSecureFS\MobilityServiceInstallers ist.

### <a name="step-1-prepare-for-deployment"></a>Schritt 1: Vorbereiten der Bereitstellung
1. Erstellen Sie einen Ordner auf der Netzwerkfreigabe, und nennen Sie ihn **MobSvcLinux**.
2. Melden Sie sich bei Ihrem Konfigurationsserver an, und öffnen Sie eine Administratoreingabeaufforderung.
3. Führen Sie die folgenden Befehle aus, um eine Passphrasedatei zu generieren:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopieren Sie die Datei **MobSvc.passphrase** in den Ordner **MobSvcLinux** auf der Netzwerkfreigabe.
5. Wechseln Sie zum Installerrepository auf dem Konfigurationsserver, indem Sie den folgenden Befehl ausführen:

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. Kopieren Sie die folgenden Dateien in den Ordner **MobSvcLinux** auf der Netzwerkfreigabe:
   * Microsoft-ASR\_UA\_*Version*\_OEL-64\_GA\_*Datum*\_Release.tar.gz
   * Microsoft-ASR\_UA\_*Version*\_RHEL6-64\_GA\_*Datum*\_Release.tar.gz
   * Microsoft-ASR\_UA\_*Version*\_RHEL7-64\_GA\_*Datum*\_Release.tar.gz
   * Microsoft-ASR\_UA\_*Version*\_SLES11-SP3-64\_GA\_*Datum*\_Release.tar.gz

7. Kopieren Sie den folgenden Code, und speichern Sie ihn als **install_linux.sh** im Ordner **MobSvcLinux**.
   > [!NOTE]
   > Ersetzen Sie den Platzhalter [CSIP] in diesem Skript durch die tatsächlichen Werte der IP-Adresse des Konfigurationsserver.

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

1. Melden Sie sich bei der Configuration Manager-Konsole an.
2. Navigieren Sie zu **Softwarebibliothek** > **Anwendungsverwaltung** > **Pakete**.
3. Klicken Sie mit der rechten Maustaste auf **Pakete**, und wählen Sie **Paket erstellen** aus.
4. Geben Sie Werte für Name, Beschreibung, Hersteller, Sprache und Version an.
5. Aktivieren Sie das Kontrollkästchen **Dieses Paket enthält Quelldateien**.
6. Klicken Sie auf die Schaltfläche **Durchsuchen**, und wählen Sie die Netzwerkfreigabe aus, in der der Installer gespeichert ist (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).

  ![Screenshot des Assistenten zum Erstellen von Paketen und Programmen](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package-linux.png)

7. Wählen Sie auf der Seite **Typ des zu erstellenden Programms auswählen** die Option **Standardprogramm** aus, und klicken Sie auf **Weiter**.

  ![Screenshot des Assistenten zum Erstellen von Paketen und Programmen](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. Geben Sie auf der Seite **Informationen zu diesem Standardprogramm angeben** die folgenden Eingaben an, und klicken Sie auf **Weiter**. (Bei den anderen Eingaben können Sie die Standardwerte übernehmen.)

    | **Parametername** | **Wert** |
  |--|--|
  | Name | Microsoft Azure Mobility Service installieren (Linux) |
  | Befehlszeile | ./install_linux.sh |
  | Programm kann ausgeführt werden | Unabhängig von Benutzeranmeldung |

  ![Screenshot des Assistenten zum Erstellen von Paketen und Programmen](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-linux.png)

9. Wählen Sie auf der nächsten Seite **Dieses Programm kann auf jeder Plattform ausgeführt werden.**
  ![Screenshot des Assistenten zum Erstellen von Paketen und Programmen](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)

10. Klicken Sie zweimal auf **Weiter**, um den Assistenten abzuschließen. 
 
> [!NOTE]
> Das Skript unterstützt Neuinstallationen von Mobility Service-Agents und Updates bereits installierter Agents.

### <a name="step-3-deploy-the-package"></a>Schritt 3: Bereitstellen des Pakets
1. Klicken Sie in der Configuration Manager-Konsole mit der rechten Maustaste auf das Paket, und wählen Sie **Inhalt verteilen** aus.
  ![Screenshot der Configuration Manager-Konsole](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)
2. Wählen Sie die **[Verteilungspunkte](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** aus, auf die die Pakete kopiert werden sollen.
3. Schließen Sie den Assistenten ab. Dann beginnt die Replikation des Pakets auf die angegebenen Verteilungspunkte.
4. Klicken Sie nach Abschluss der Paketverteilung mit der rechten Maustaste auf das Paket, und wählen Sie **Bereitstellen** aus.
  ![Screenshot der Configuration Manager-Konsole](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)
5. Wählen Sie die Gerätesammlung für Linux-Server, die Sie im Abschnitt „Voraussetzungen“ erstellt haben, als die Zielsammlung für die Bereitstellung aus.

  ![Screenshot des Assistenten zum Bereitstellen von Software](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection-linux.png)

6. Wählen Sie auf der Seite **Das Inhaltsziel angeben** Ihre **Verteilungspunkte** aus.
7. Stellen Sie auf der Seite **Geben Sie Einstellungen an, um die Bereitstellung dieser Software zu steuern** sicher, dass der gewünschte Zweck **Erforderlich** ist.

  ![Screenshot des Assistenten zum Bereitstellen von Software](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. Geben Sie auf der Seite **Zeitplan für diese Bereitstellung angeben** einen Zeitplan an. Weitere Informationen finden Sie unter [Bereitstellen von Paketen und Programmen in Configuration Manager](https://technet.microsoft.com/library/gg682178.aspx).
9. Konfigurieren Sie die Eigenschaften auf der Seite **Verteilungspunkte** nach den Anforderungen Ihres Rechenzentrums. Schließen Sie dann den Assistenten ab.

Mobility Service wird auf den Linux-Servern der Gerätesammlung entsprechend dem konfigurierten Zeitplan installiert.

## <a name="other-methods-to-install-mobility-service"></a>Andere Methoden zum Installieren von Mobility Service
Hier finden Sie weitere Optionen für die Installation von Mobility Service:
* [Manuelle Installation mithilfe der GUI](http://aka.ms/mobsvcmanualinstall)
* [Manuelle Installation über die Befehlszeile](http://aka.ms/mobsvcmanualinstallcli)
* [Pushinstallation mithilfe eines Konfigurationsservers](http://aka.ms/pushinstall)
* [Automatisierte Installation mithilfe von Azure Automation und Konfiguration des gewünschten Zustands](http://aka.ms/mobsvcdscinstall)

## <a name="uninstall-mobility-service"></a>Mobility Service deinstallieren
Sie können Configuration Manager-Pakete erstellen, um Mobility Service zu deinstallieren. Führen Sie dazu folgendes Skript aus:

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
Sie können jetzt für Ihre virtuellen Computer [Schutz aktivieren](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications).

