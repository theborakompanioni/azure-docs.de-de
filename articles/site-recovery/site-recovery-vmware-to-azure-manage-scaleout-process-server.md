---
title: " Verwalten eines Prozessservers für horizontales Hochskalieren in Azure Site Recovery | Microsoft-Dokumentations"
description: "Dieser Artikel beschreibt das Einrichten und Verwalten eines Prozessservers für horizontales Hochskalieren in Azure Site Recovery."
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
ms.date: 2/14/2017
ms.author: anoopkv
translationtype: Human Translation
ms.sourcegitcommit: cabbce12a07720c37375092962ee1f89c32269ef
ms.openlocfilehash: 03bb87bdbf3dce07a282323f484d0aefae8bad62
ms.lasthandoff: 02/22/2017

---

# <a name="manage-a-scale-out-process-server"></a>Verwalten eines Prozessservers für horizontales Hochskalieren

Der Prozessserver für horizontales Hochskalieren fungiert als Koordinator für die Datenübertragung zwischen den Site Recovery-Diensten und Ihrer lokalen Infrastruktur. In diesem Artikel wird beschrieben, wie Sie einen Prozessserver für horizontales Hochskalieren einrichten, konfigurieren und verwalten können.

## <a name="prerequisites"></a>Voraussetzungen
Im Folgenden wird die empfohlene Hardware-, Software- und Netzwerkkonfiguration zum Einrichten eines Prozessservers für horizontales Hochskalieren aufgeführt.

> [!NOTE]
> [Kapazitätsplanung](site-recovery-capacity-planner.md) ist ein wichtiger Schritt, um sicherzustellen, dass Sie den Prozessserver für horizontales Hochskalieren mit einer Konfiguration bereitstellen, die Ihren Lastanforderungen entspricht. Erfahren Sie mehr über [Skalierungsmerkmale für einen Prozessserver für horizontales Hochskalieren](#sizing-requirements-for-a-configuration-server).

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-the-scale-out-process-server-software"></a>Herunterladen der Software für einen Prozessserver für horizontales Hochskalieren
1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu Ihrem Recovery Services-Tresor.
2. Navigieren Sie zu **Site Recovery-Infrastruktur** > **Konfigurationsserver** (unter „Für VMware und physische Computer“).
3. Wählen Sie Ihren Konfigurationsserver, um die Detailsseite des Konfigurationsservers aufzurufen.
4. Klicken Sie auf die Schaltfläche **+ Prozessserver**.
5. Wählen Sie auf der Seite **Prozessserver hinzufügen** die Option **Prozessserver für horizontales Hochskalieren lokal bereitstellen** in der Dropdownliste **Geben Sie an, wo Sie Ihren Prozessserver bereitstellen möchten.** aus.

  ![Seite „Server hinzufügen“](./media/site-recovery-vmware-to-azure-manage-scaleout-process-server/add-process-server.png)
6. Klicken Sie auf den Link **Einheitliches Setup von Microsoft Azure Site Recovery herunterladen**, um die neueste Version der Installation des Prozessservers für horizontales Hochskalieren herunterzuladen.

  > [!WARNING]
  Die Version Ihres Prozessservers für horizontales Hochskalieren sollte gleich der Konfigurationsserverversion sein, die in Ihrer Umgebung ausgeführt wird, oder höher. Eine einfache Möglichkeit, die Versionskompatibilität sicherzustellen, ist die Verwendung der gleichen Installationsdateien, die Sie zuletzt zum Installieren/Aktualisieren Ihres Konfigurationsservers verwendet haben.

## <a name="installing-and-registering-a-scale-out-process-server-from-gui"></a>Installieren und Registrieren eines Prozessservers für horizontales Hochskalieren von der GUI aus
Wenn Sie Ihre Bereitstellung über 200 Quellcomputer oder eine gesamte tägliche Änderungsrate von 2 TB hinaus horizontal hochskalieren müssen, benötigen Sie zusätzliche Prozessserver zur Bewältigung des Datenverkehrsaufkommens.

Überprüfen Sie die [für Prozessserver empfohlenen Größen](#size-recommendations-for-the-process-server), und befolgen Sie anschließend diese Anweisungen, um den Prozessserver einzurichten. Nachdem Sie den Prozessserver eingerichtet haben, migrieren Sie Quellcomputer, damit diese ihn verwenden können.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="installing-and-registering-a-scale-out-process-server-using-command-line"></a>Installieren und Registrieren eines Prozessservers für horizontales Hochskalieren über die Befehlszeile

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

### <a name="sample-usage"></a>Beispielverwendung
```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```

### <a name="scale-out-process-server-installer-command-line-arguments"></a>Befehlszeilenargumente für das Installationsprogramm für den Prozessserver für horizontales Hochskalieren.
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-proxy-settings-configuration-file"></a>Erstellen einer Konfigurationsdatei für Proxyeinstellungen
Der ProxySettingsFilePath-Parameter nimmt eine Datei als Eingabe an. Erstellen Sie die Datei im folgenden Format, und übergeben Sie sie als Eingabe an den Parameter ProxySettingsFilePath.
```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-scale-out-process-server"></a>Ändern der Proxyeinstellungen für den Prozessserver für horizontales Hochskalieren
1. Melden Sie sich bei Ihrem Prozessserver für horizontales Hochskalieren an.
2. Öffnen Sie ein PowerShell-Befehlsfenster mit Administratorrechten.
3. Führen Sie den folgenden Befehl aus
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
4. Wechseln Sie als Nächstes zum Verzeichnis **%PROGRAMDATA%\ASR\Agent**, und führen Sie den folgenden Befehl aus.
  ```
  cmd
  cdpcli.exe --registermt
  
  net stop obengine

  net start obengine

  exit
  ```

## <a name="re-registering-a-scale-out-process-server"></a>Erneutes Registrieren eines Prozessservers für horizontales Hochskalieren
[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

* Öffnen Sie als Nächstes eine Eingabeaufforderung.
* Wechseln Sie zum Verzeichnis **%PROGRAMDATA%\ASR\Agent**, und führen Sie den Befehl aus.

```
cdpcli.exe --registermt

net stop obengine

net start obengine
```

## <a name="upgrading-a-scale-out-process-server"></a>Aktualisieren eines Prozessservers für horizontales Hochskalieren
[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

## <a name="decommissioning-a-scale-out-process-server"></a>Außerbetriebnahme eines Prozessservers für horizontales Hochskalieren
1. Stellen Sie Folgendes sicher:
  - Für den Konfigurationsserver wird der Verbindungsstatus **Verbunden** im Azure-Portal angezeigt.
  - Der Prozessserver kann weiterhin mit dem Konfigurationsserver kommunizieren.
2. Melden Sie sich als Administrator bei dem Prozessserver an.
3. Öffnen Sie „Systemsteuerung > Programme > Programme deinstallieren“.
4. Deinstallieren Sie die Programme in der folgenden Reihenfolge:
  * Microsoft Azure Site Recovery-Konfigurationsserver/Prozessserver
  * Microsoft Azure Site Recovery-Konfigurationsserverabhängigkeiten
  * Microsoft Azure Recovery Services-Agent

Es kann bis zu 15 Minuten dauern, bis die Löschung des Prozessservers im Azure-Portal angezeigt wird.

  > [!NOTE]
  Wenn der Prozessserver nicht mit dem Konfigurationsserver kommunizieren kann (Verbindungsstatus im Portal ist „Getrennt“), müssen Sie ihn mit folgenden Schritten vom Konfigurationsserver löschen.

## <a name="unregistering-a-disconnected-scale-out-process-server-from-a-configuration-server"></a>Aufheben der Registrierung eines getrennten Prozessservers für horizontales Hochskalieren von einem Konfigurationsserver

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="sizing-requirements-for-a-scale-out-process-server"></a>Anforderungen an die Größenanpassung für einen Prozessserver mit horizontaler Skalierung

| **Zusätzlicher Prozessserver** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer** |
| --- | --- | --- | --- |
|4 vCPUs (2 Sockets * 2 Kerne mit 2,5GHz), 8GB Arbeitsspeicher |300 GB |250 GB oder weniger |Bis zu 85 Computer replizieren. |
|8 vCPUs (2 Sockets * 4 Kerne mit 2,5GHz), 12GB Arbeitsspeicher |600 GB |250 GB bis 1 TB |Zwischen 85 und 150 Computer replizieren. |
|12 vCPUs (2 Sockets * 6 Kerne mit 2,5GHz), 24GB Arbeitsspeicher |1 TB |1 TB bis 2 TB |Zwischen 150 und 225 Computer replizieren. |

