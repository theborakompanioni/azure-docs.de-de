---
title: 'Azure Disk Encryption: Problembehandlung | Microsoft-Dokumentation'
description: "Dieser Artikel enthält Tipps zur Problembehandlung für Microsoft Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer."
services: security
documentationcenter: na
author: deventiwari
manager: avibm
editor: yuridio
ms.assetid: ce0e23bd-07eb-43af-a56c-aa1a73bdb747
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: devtiw
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: f2b9aad02a1ae3d5117ffd59b448eabe65a936fc
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Leitfaden zur Azure Disk Encryption-Problembehandlung

Dieser Leitfaden ist für IT-Experten, Informationssicherheitsanalysten und Cloudadministratoren gedacht, in deren Organisation Azure Disk Encryption verwendet wird und die Probleme mit der Datenträgerverschlüsselung lösen müssen.

## <a name="troubleshooting-linux-os-disk-encryption"></a>Problembehandlung für die Verschlüsselung von Datenträgern mit Linux-Betriebssystem

Bei der Verschlüsselung von Datenträgern mit Linux-Betriebssystem muss die Bereitstellung des Betriebssystemlaufwerks aufgehoben werden, bevor die vollständige Datenträgerverschlüsselung erfolgt. Wenn das Aufheben der Bereitstellung nicht möglich ist, tritt unter Umständen eine Fehlermeldung der Art „Fehler beim Aufheben der Bereitstellung…“ auf.

Die Wahrscheinlichkeit dieses Fehlers ist am höchsten, wenn versucht wird, die Verschlüsselung des Betriebssystemdatenträgers in einer VM-Zielumgebung durchzuführen, die gegenüber dem unterstützten vorgefertigten Katalogimage modifiziert oder geändert wurde. Im Folgenden sind Beispiele für Abweichungen vom unterstützten Image angegeben, durch die die Fähigkeit der Erweiterung zum Aufheben der Bereitstellung des Betriebssystemlaufwerks beeinträchtigt werden kann:
- Angepasste Images, die nicht mehr mit einem unterstützten Dateisystem oder Partitionierungsschema übereinstimmen.
- Wenn große Anwendungen, z.B. SAP, MongoDB oder Apache Cassandra, installiert sind und vor der Verschlüsselung im Betriebssystem ausgeführt werden. Die Erweiterung kann diese Anwendungen nicht ordnungsgemäß herunterfahren. Wenn die Anwendungen geöffnete Dateihandles zum Betriebssystemlaufwerk behalten, kann die Einbindung des Laufwerks nicht aufgehoben werden, wodurch ein Fehler verursacht wird.
- Wenn benutzerdefinierte Skripts in einem engen zeitlichen Abstand zur Aktivierung der Verschlüsselung ausgeführt werden oder während des Verschlüsselungsprozesses andere Änderungen an der VM vorgenommen werden. Dieser Konflikt kann auftreten, wenn eine Azure Resource Manager-Vorlage mehrere Erweiterungen für die gleichzeitige Ausführung definiert oder wenn eine benutzerdefinierte Skripterweiterung oder andere Aktion parallel zur Datenträgerverschlüsselung erfolgt. Durch eine Serialisierung und Isolierung dieser Schritte lässt sich das Problem ggf. beheben.
- Security Enhanced Linux (SELinux) wurde vor der Aktivierung der Verschlüsselung nicht deaktiviert, weshalb der Schritt zur Aufhebung der Bereitstellung fehlschlägt. SELinux kann wieder aktiviert werden, nachdem die Verschlüsselung abgeschlossen ist.
- Der Betriebssystemdatenträger verwendet ein LVM-Schema (Logical Volume Manager). Wenngleich eingeschränkte Unterstützung für LVM-Datenträger geboten wird, gilt dies nicht für LVM-Betriebssystemdatenträger.
- Die Mindestanforderungen für den Arbeitsspeicher sind nicht erfüllt (für die Verschlüsselung des Betriebssystemdatenträgers werden 7 GB empfohlen).
- Datenlaufwerke sind rekursiv im Verzeichnis „/mnt/“ oder gegenseitig bereitgestellt worden (z.B. „/mnt/data1“, „/mnt/data2“, „/data3 + /data3/data4“).
- Andere Azure Disk Encryption-[Voraussetzungen](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) für Linux sind nicht erfüllt.

## <a name="unable-to-encrypt"></a>Verschlüsselung nicht möglich

In einigen Fällen hängt die Verschlüsselung des Linux-Datenträgers scheinbar bei „OS disk encryption started“, und SSH ist deaktiviert. Dieser Prozess kann bei einem normalen Katalogimage 3-16 Stunden bis zum Abschluss dauern. Wenn Datenträger mit mehreren Terabyte Daten hinzugefügt werden, kann der Prozess Tage dauern.

Durch die Verschlüsselungssequenz für Linux-Betriebssystemdatenträger wird die Bereitstellung des Betriebssystemlaufwerks vorübergehend aufgehoben. Es erfolgt anschließend eine blockweise Verschlüsselung des gesamten Betriebssystemdatenträgers, ehe er im verschlüsselten Zustand wieder bereitgestellt wird. Im Gegensatz zu Azure Disk Encryption unter Windows ist bei der Linux-Datenträgerverschlüsselung keine gleichzeitige Nutzung der VM während des Verschlüsselungsvorgangs möglich. Die Leistungsmerkmale der VM können sich signifikant auf den Zeitaufwand auswirken, der bis zur Verschlüsselung anfällt. Zu diesen Merkmalen zählen die Größe des Datenträgers und das Speicherkonto (Standard oder Storage Premium).

Fragen Sie zum Überprüfen des Verschlüsselungsstatus das Feld **ProgressMessage** ab, das vom Cmdlet [Get-AzureRmVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) zurückgegeben wird. Während das Betriebssystemlaufwerk verschlüsselt wird, befindet sich die VM in einem Wartungszustand, und SSH wird deaktiviert, um eine Störung des laufenden Prozesses zu verhindern. Solange die Verschlüsselung läuft, wird die Meldung **EncryptionInProgress** die meiste Zeit zurückgegeben. Mehrere Stunden später werden Sie in der Meldung **VMRestartPending** aufgefordert, die VM neu zu starten. Beispiel:


```
PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : EncryptionInProgress
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started

PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : VMRestartPending
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk successfully encrypted, please reboot the VM
```

Nach Aufforderung zum Neustart der VM und deren Neustart müssen Sie 2-3 Minuten auf den Neustart und die letzten Schritte warten, die auf dem Ziel erfolgen. Die Statusmeldung ändert sich, wenn die Verschlüsselung schließlich abgeschlossen ist. Wenn diese Meldung angezeigt wird, sollte das verschlüsselte Betriebssystemlaufwerk betriebsbereit und die VM wieder verwendbar sein.

In den folgenden Fällen wird empfohlen, die VM in den Snapshot zurückzuspeichern oder unmittelbar vor der Verschlüsselung eine Sicherung zu erstellen.
   - Wenn die zuvor beschriebene Neustartsequenz nicht erfolgt.
   - Wenn die Startinformationen, Statusmeldung oder andere Fehlerindikatoren melden, dass die Betriebssystemverschlüsselung mitten in diesem Prozess fehlgeschlagen ist. Ein Beispiel einer Meldung ist der in dieser Anleitung beschriebene Fehler „Fehler beim Aufheben der Bereitstellung“.

Bewerten Sie vor dem nächsten Versuch die Merkmale der VM erneut, und stellen Sie sicher, dass alle Voraussetzungen erfüllt sind.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Azure Disk Encryption-Problembehandlung hinter einer Firewall
Wenn die Konnektivität durch eine Firewall, eine Proxyanforderung oder Einstellungen für Netzwerksicherheitsgruppen (NSGs) eingeschränkt ist, kann die Fähigkeit der Erweiterung zur Durchführung von erforderlichen Aufgaben beeinträchtigt sein. Dies kann zu Statusmeldungen der Art „Erweiterungsstatus auf der VM nicht verfügbar“. In erwarteten Szenarien kann die Verschlüsselung nicht abgeschlossen werden. In den folgenden Abschnitten werden einige häufig auftretende Firewallprobleme beschrieben, die Sie ggf. untersuchen müssen.

### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen
Für alle angewendeten Einstellungen von Netzwerksicherheitsgruppen muss es ermöglicht werden, dass der Endpunkt die dokumentierten [Voraussetzungen](https://docs.microsoft.com/azure/security/azure-security-disk-encryption#prerequisites) für die Netzwerkkonfiguration in Bezug auf die Datenträgerverschlüsselung erfüllt.

### <a name="azure-key-vault-behind-a-firewall"></a>Azure Key Vault hinter einer Firewall
Die VM muss auf einen Schlüsseltresor zugreifen können. Lesen Sie den Leitfaden für den Zugriff auf den Schlüsseltresor hinter einer Firewall, die vom [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall)-Team verwaltet wird.

### <a name="linux-package-management-behind-a-firewall"></a>Linux-Paketverwaltung hinter einer Firewall

Zur Laufzeit nutzt Azure Disk Encryption für Linux das Paketverwaltungssystem der Zieldistribution, um vor dem Aktivieren der Verschlüsselung erforderliche Komponenten zu installieren. Falls die VM durch Firewalleinstellungen am Herunterladen und Installieren dieser Komponenten gehindert wird, ist mit nachfolgend auftretenden Fehlern zu rechnen. Die Schritte zum Konfigurieren dieses Paketverwaltungssystems können je nach Distribution variieren. Wenn unter Red Hat ein Proxy erforderlich ist, müssen Sie sicherstellen, dass die richtige Einrichtung von „subscription-manager“ und „yum“ sichergestellt ist. Weitere Informationen finden Sie unter [How to troubleshoot subscription-manager and yum problems](https://access.redhat.com/solutions/189533) (Beheben von Problemen mit subscription-manager und yum).  

## <a name="troubleshooting-windows-server-2016-server-core"></a>Problembehandlung bei Windows Server 2016 Server Core

Unter Windows Server 2016 Server Core ist die Komponente bdehdcfg nicht standardmäßig verfügbar. Diese Komponente ist für Azure Disk Encryption erforderlich. Mit ihr wird das Systemvolume von Betriebssystemvolume getrennt. Diese Teilung erfolgt nur einmal während der Lebensdauer des virtuellen Computers. Diese Binärdateien sind während der späteren Verschlüsselungsvorgänge nicht erforderlich.

Um dieses Problem zu umgehen, kopieren Sie die folgenden vier Dateien von einer Windows Server 2016 Data Center-VM an denselben Speicherort wie Server Core:

   ```
   \windows\system32\bdehdcfg.exe
   \windows\system32\bdehdcfglib.dll
   \windows\system32\en-US\bdehdcfglib.dll.mui
   \windows\system32\en-US\bdehdcfg.exe.mui
   ```

   2. Geben Sie den folgenden Befehl ein:

   ```
   bdehdcfg.exe -target default
   ```

   3. Dieser Befehl erstellt eine Systempartition der Größe 550 MB. Starten Sie das System neu.

   4. Verwenden Sie DiskPart zum Überprüfen der Volumes, und fahren Sie dann fort.  

Beispiel:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```
## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie weitere Informationen zu einigen häufigen Problemen in Azure Disk Encryption und deren Behandlung erhalten. Weitere Informationen zu diesem Dienst und seinen Funktionen finden Sie in den folgenden Artikeln:

- [Anwenden der Datenträgerverschlüsselung in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Verschlüsseln eines virtuellen Azure-Computers](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Datenverschlüsselung ruhender Azure-Daten](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)

