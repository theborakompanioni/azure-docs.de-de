---
title: Azure Disk Encryption-Problembehandlung | Microsoft-Dokumentation
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
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 31eeaa3df41065b65d6202f00c01ad2f706e230a
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Leitfaden zur Azure Disk Encryption-Problembehandlung

Dieser Leitfaden ist für IT-Experten, Informationssicherheitsanalysten und Cloudadministratoren gedacht, in deren Organisation Azure Disk Encryption verwendet wird und die Probleme mit der Datenträgerverschlüsselung lösen müssen.

## <a name="troubleshooting-linux-os-disk-encryption"></a>Problembehandlung für die Verschlüsselung von Datenträgern mit Linux-Betriebssystem

Bei der Verschlüsselung von Datenträgern mit Linux-Betriebssystem muss die Bereitstellung des Betriebssystemlaufwerks aufgehoben werden, bevor der vollständige Prozess der Datenträgerverschlüsselung durchgeführt wird.   Wenn dies nicht möglich ist, tritt unter Umständen eine Fehlermeldung der Art „Fehler beim Aufheben der Bereitstellung…“ auf.

Die Wahrscheinlichkeit hierfür ist am höchsten, wenn versucht wird, die Verschlüsselung des Betriebssystemdatenträgers in einer VM-Zielumgebung durchzuführen, die gegenüber dem unterstützten vorgefertigten Katalogimage modifiziert oder geändert wurde.  Hier sind Beispiele für Abweichungen vom unterstützten Image angegeben, durch die die Fähigkeit der Erweiterung zum Aufheben der Betriebssystemlaufwerk-Bereitstellung beeinträchtigt werden kann:
- Angepasste Images, die nicht mehr mit einem unterstützten Dateisystem bzw. Partitionierungsschema übereinstimmen.
- Wenn große Anwendungen, z.B. SAP, MongoDB oder Apache Cassandra, installiert sind und vor der Verschlüsselung im Betriebssystem ausgeführt werden.  Die Erweiterung kann diese Komponenten nicht richtig herunterfahren. Wenn dann offene Dateihandles zum Betriebssystemlaufwerk vorhanden sind, kann die Bereitstellung des Laufwerks nicht aufgehoben werden, und es tritt ein Fehler auf.
- Wenn benutzerdefinierte Skripts in einem engen zeitlichen Abstand zur Aktivierung der Verschlüsselung ausgeführt werden oder wenn während des Verschlüsselungsprozesses andere Änderungen an der VM vorgenommen werden.   Dies kann vorkommen, wenn eine Resource Manager-Vorlage mehrere Erweiterungen für die gleichzeitige Ausführung definiert oder wenn eine benutzerdefinierte Skripterweiterung oder andere Aktion parallel zur Datenträgerverschlüsselung ausgeführt wird.   Durch eine Serialisierung und Isolierung dieser Schritte lässt sich das Problem unter Umständen beheben.
- Wenn SELinux vor der Aktivierung der Verschlüsselung nicht deaktiviert wurde, schlägt der Schritt für die Aufhebung der Bereitstellung fehl.  SELinux kann wieder aktiviert werden, nachdem die Verschlüsselung abgeschlossen ist.
- Wenn der Betriebssystemdatenträger ein LVM-Schema verwendet (eingeschränkte Unterstützung für LVM-Datenträger ist zwar vorhanden, aber nicht für den LVM-Betriebssystemdatenträger).
- Wenn die Mindestanforderungen für den Arbeitsspeicher nicht erfüllt sind (für die Verschlüsselung des Betriebssystemdatenträgers werden 7 GB empfohlen).
- Wenn Datenträger für Daten rekursiv im Verzeichnis „/mnt/“ oder gegenseitig bereitgestellt wurden (z.B. „/mnt/data1“, „/mnt/data2“, „/data3 + /data3/data4“ usw.).
- Wenn andere Azure Disk Encryption-[Voraussetzungen](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) für Linux nicht erfüllt sind.

## <a name="unable-to-encrypt"></a>Verschlüsselung nicht möglich

In einigen Fällen hängt die Verschlüsselung des Linux-Datenträgers scheinbar bei „OS disk encryption started“, und SSH ist deaktiviert. Dieser Prozess kann zwischen 3 und 16 Stunden dauern und benötigt ggf. mehr Zeit.  Bei der Sequenz zur Verschlüsselung des Linux-Betriebssystemdatenträgers wird die Bereitstellung des Betriebssystemlaufwerks vorübergehend aufgehoben, und es wird Block für Block eine Verschlüsselung des gesamten Betriebssystemdatenträgers durchgeführt, bevor er im verschlüsselten Zustand wieder bereitgestellt wird.   Im Gegensatz zu Azure Disk Encryption unter Windows ist bei der Linux-Datenträgerverschlüsselung keine gleichzeitige Nutzung der VM während des Verschlüsselungsvorgangs möglich.  Die Leistungsmerkmale der VM, z.B. die Größe des Datenträgers und die Unterstützung des Speicherkontos durch Standard- oder Premium-Speicher (SSD), können eine starke Auswirkung darauf haben, wie viel Zeit für die Durchführung der Verschlüsselung benötigt wird.

Sie können das ProgressMessage-Feld, das über den Befehl [Get-AzureRmVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) zurückgegeben wird, abfragen, um den Status zu überprüfen.   Während das Betriebssystemlaufwerk verschlüsselt wird, befindet sich die VM in einem Wartungszustand, und auch SSH ist deaktiviert, um eine Störung des laufenden Prozesses zu verhindern.  Für den größten Teil des Verschlüsselungszeitraums wird „EncryptionInProgress“ gemeldet, und nach einigen Stunden folgt dann die Meldung „VMRestartPending“ als Aufforderung, die VM neu zu starten.  Beispiel:


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

Nachdem Sie zum Neustarten der VM aufgefordert wurden, den Neustart für die VM durchgeführt haben und zwei bis drei Minuten gewartet haben, bis der Neustart und die abschließenden Schritte auf dem Ziel abgeschlossen sind, zeigt die Statusmeldung die Fertigstellung der Verschlüsselung an.   Wenn diese Meldung angezeigt wird, sollte das verschlüsselte Betriebssystemlaufwerk betriebsbereit und die VM wieder verwendbar sein.

In Fällen, in denen diese Sequenz nicht erkennbar war, oder wenn durch Startinformationen, Statusmeldungen oder andere Fehlerindikatoren melden, dass die Betriebssystemverschlüsselung während dieses Prozesses fehlgeschlagen ist (z.B. mit dem in diesem Leitfaden beschriebenen Fehler der Art „Fehler beim Aufheben der Bereitstellung“), wird Folgendes empfohlen: Stellen Sie die VM anhand der Momentaufnahme oder Sicherung wieder her, die direkt vor der Verschlüsselung erstellt wurde.  Vor dem nächsten Versuch ist es ratsam, die Merkmale der VM erneut auszuwerten und sicherzustellen, dass alle Voraussetzungen erfüllt sind.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Azure Disk Encryption-Problembehandlung hinter einer Firewall
Wenn die Konnektivität durch eine Firewall, eine Proxyanforderung oder Einstellungen für Netzwerksicherheitsgruppen (NSGs) eingeschränkt ist, kann die Fähigkeit der Erweiterung zur Durchführung von erforderlichen Aufgaben beeinträchtigt sein.   Dies kann zu Statusmeldungen der Art „Erweiterungsstatus auf der VM nicht verfügbar“ oder dazu führen, dass erwartete Szenarien nicht abgeschlossen werden können.  In den folgenden Abschnitten werden einige häufig auftretende Firewallprobleme beschrieben, die Sie ggf. lösen müssen.

### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen
Für alle angewendeten Einstellungen für Netzwerksicherheitsgruppen muss es ermöglicht werden, dass der Endpunkt die dokumentierten [Voraussetzungen](https://docs.microsoft.com/azure/security/azure-security-disk-encryption#prerequisites) für die Netzwerkkonfiguration in Bezug auf die Datenträgerverschlüsselung erfüllt.

### <a name="azure-keyvault-behind-firewall"></a>Azure Key Vault hinter einer Firewall
Die VM muss auf den Schlüsseltresor zugreifen können. Lesen Sie den Leitfaden zum Zugreifen auf den Schlüsseltresor von einem Ort hinter einer Firewall, die vom [Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall)-Team verwaltet wird.

### <a name="linux-package-management-behind-firewall"></a>Linux-Paketverwaltung hinter einer Firewall
Zur Laufzeit nutzt Azure Disk Encryption für Linux das Paketverwaltungssystem der Zieldistribution, um vor dem Aktivieren der Verschlüsselung erforderliche Komponenten zu installieren.  Falls die VM durch Firewalleinstellungen am Herunterladen und Installieren dieser Komponenten gehindert wird, ist mit nachfolgend auftretenden Fehlern zu rechnen.    Die Schritte für diese Konfiguration können je nach Distribution variieren.  Wenn unter Red Hat ein Proxy erforderlich ist, ist es sehr wichtig, dass die richtige Einrichtung von subscription-manager und yum sichergestellt ist.  Weitere Informationen zu diesem Thema finden Sie in [diesem Artikel des Red Hat-Supports](https://access.redhat.com/solutions/189533).  

## <a name="see-also"></a>Weitere Informationen
In diesem Dokument haben Sie weitere Informationen zu einigen häufigen Problemen in Azure Disk Encryption und deren Behandlung erhalten. Weitere Informationen zu diesem Dienst und den darin verfügbaren Funktionen finden Sie unter:

- [Anwenden der Datenträgerverschlüsselung in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Verschlüsseln eines virtuellen Azure-Computers](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Azure-Datenverschlüsselung ruhender Daten](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
