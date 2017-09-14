---
title: Azure Disk Encryption FAQ| Microsoft-Dokumentation
description: "Dieser Artikel bietet Antworten auf häufig gestellte Fragen zu Microsoft Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer."
services: security
documentationcenter: na
author: deventiwari
manager: avibm
editor: yuridio
ms.assetid: 7188da52-5540-421d-bf45-d124dee74979
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: devtiw
ms.translationtype: HT
ms.sourcegitcommit: a16daa1f320516a771f32cf30fca6f823076aa96
ms.openlocfilehash: f66eabcbb386d5e7b31268a7b04063ff2cefbaf2
ms.contentlocale: de-de
ms.lasthandoff: 09/02/2017

---
# <a name="azure-disk-encryption-faq"></a>Häufig gestellte Fragen zu Azure Disk Encryption

Dieser Artikel bietet Antworten auf häufig gestellte Fragen zu Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer. Weitere Informationen zu diesem Dienst finden Sie unter [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="general-questions"></a>Allgemeine Fragen
**F:** Wo ist Azure Disk Encryption allgemein verfügbar?

**A:** Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer ist in allen öffentlichen Azure-Regionen allgemein verfügbar.

**F:** Welche Benutzeroberflächen sind für Azure Disk Encryption verfügbar?

**A:** Azure Disk Encryption mit allgemeiner Verfügbarkeit unterstützt Azure Resource Manager-Vorlagen, Azure PowerShell und die Azure CLI. Dies bietet Ihnen ein hohes Maß an Flexibilität. Ihnen stehen drei Optionen zum Aktivieren der Datenträgerverschlüsselung für Ihre IaaS-VMs zur Verfügung. Weitere Informationen zur Benutzeroberfläche und eine Schritt-für-Schritt-Anleitung sind in den Bereitstellungsszenarien und -umgebungen von Azure Disk Encryption verfügbar.

**F:** Was kostet Azure Disk Encryption?

**A:** Für die Verschlüsselung von VM-Datenträgern mit Azure Disk Encryption fallen keine Gebühren an.

**F:** Welche VM-Tarife unterstützt Azure Disk Encryption?

**A:** Azure Disk Encryption ist in VMs mit Standardtarifen einschließlich IaaS-VMs der Serien [A, D, DS, G, GS und F](https://azure.microsoft.com/pricing/details/virtual-machines/) verfügbar. Es ist auch für VMs mit Storage Premium verfügbar. Es steht nicht für VMs im Tarif „Basic“ zur Verfügung.

**F:** Welche Linux-Distributionen werden von Azure Disk Encryption unterstützt?

**A:** Azure Disk Encryption wird für die folgenden Linux-Serverdistributionen und -Versionen unterstützt:

| Linux-Distribution | Version | Für die Verschlüsselung unterstützter Volumetyp|
| --- | --- |--- |
| Ubuntu | 16.04-DAILY-LTS | Betriebssystem- und andere Datenträger |
| Ubuntu | 14.04.5-DAILY-LTS | Betriebssystem- und andere Datenträger |
| RHEL | 7.3 | Betriebssystem- und andere Datenträger |
| RHEL | 7.2 | Betriebssystem- und andere Datenträger |
| RHEL | 6,8 | Betriebssystem- und andere Datenträger |
| RHEL | 6.7 | Datenträger |
| CentOS | 7.3 | Betriebssystem- und andere Datenträger |
| CentOS | 7.2n | Betriebssystem- und andere Datenträger |
| CentOS | 6,8 | Betriebssystem- und andere Datenträger |
| CentOS | 7.1 | Datenträger |
| CentOS | 7.0 | Datenträger |
| CentOS | 6.7 | Datenträger |
| CentOS | 6.6 | Datenträger |
| CentOS | 6,5 | Datenträger |
| openSUSE | 13.2 | Datenträger |
| SLES | 12 SP1 | Datenträger |
| SLES | Priorität: 12-SP1 | Datenträger |
| SLES | HPC 12 | Datenträger |
| SLES | Priorität: 11-SP4 | Datenträger |
| SLES | 11 SP4 | Datenträger |

**F:** Wie sehen die ersten Schritte mit Azure Disk Encryption aus?

**A:** Lesen Sie hierzu das Whitepaper [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

**F:** Kann ich sowohl Start- als auch Datenvolumes mit Azure Disk Encryption verschlüsseln?

**A:** Ja. Sie können Start- und Datenvolumes für virtuelle Windows- und Linux-IaaS-Computer verschlüsseln. Bei Windows-VMs können Sie die Daten ohne vorherige Verschlüsselung des Betriebssystemvolumes nicht verschlüsseln. Bei Linux-VMs können Sie das Datenvolume ohne vorherige Verschlüsselung des Betriebssystemvolumes verschlüsseln. Nachdem Sie das Betriebssystemvolume für Linux verschlüsselt haben, wird das Deaktivieren der Verschlüsselung auf einem Betriebssystemvolume für Linux-IaaS-VMs nicht unterstützt.

**F:** Ermöglicht Azure Disk Encryption die Verwendung von „Bring Your Own Key“ (BYOK, dt. Eigenen Schlüssel verwenden)?

**A:** Ja. Sie können Ihre eigenen Schlüssel für die Verschlüsselung anderer Schlüssel (Key Encryption Keys, KEKs) angeben. Diese Schlüssel werden in Azure Key Vault, dem Schlüsselspeicher für Azure Disk Encryption, aufbewahrt. Weitere Informationen zu den einzelnen KEK-Szenarien finden Sie in den Bereitstellungsszenarien und -umgebungen von Azure Disk Encryption.

**F:** Kann ich einen von Azure erstellten KEK verwenden?

**A:** Ja. Sie können Azure Key Vault verwenden, um einen KEK für Azure Disk Encryption zu erstellen. Diese Schlüssel werden in Azure Key Vault, dem Schlüsselspeicher für Azure Disk Encryption, aufbewahrt. Weitere Informationen zu den einzelnen KEK-Unterstützungsszenarien finden Sie in den Bereitstellungsszenarien und -umgebungen von Azure Disk Encryption.

**F:** Kann ich den lokalen Schlüsselverwaltungsdienst oder HSM verwenden, um die Verschlüsselungsschlüssel zu schützen?

**A:** Sie können den lokalen Schlüsselverwaltungsdienst oder HSM nicht verwenden, um die Verschlüsselungsschlüssel mit Azure Disk Encryption zu schützen. Sie können hierzu nur den Azure Key Vault-Dienst verwenden. Weitere Informationen zu den einzelnen KEK-Unterstützungsszenarien finden Sie in den Bereitstellungsszenarien und -umgebungen von Azure Disk Encryption.

**F:** Was sind die Voraussetzungen für die Konfiguration von Azure Disk Encryption?

**A:** Ein PowerShell-Skript muss ausgeführt werden. Mit diesem Skript können Sie eine Azure Active Directory-Anwendung erstellen, einen neuen Schlüsseltresor einrichten oder einen vorhandenen Schlüsseltresor für den verschlüsselten Datenträgerzugriff einrichten, um die Verschlüsselung zu aktivieren und Geheimnisse und Schlüssel zu schützen. Weitere Informationen zu den einzelnen KEK-Unterstützungsszenarien finden Sie in den Voraussetzungen sowie den Bereitstellungsszenarien und -umgebungen von Azure Disk Encryption.

**F:** Wo erhalte ich weitere Informationen zum Konfigurieren von Azure Disk Encryption mithilfe von PowerShell?

**A:** Wir bieten einige hervorragende Artikel zu grundlegenden Aufgaben für Azure Disk Encryption sowie zu komplexeren Szenarien. Informationen zu den grundlegenden Aufgaben finden Sie unter [Explore Azure Disk Encryption with Azure PowerShell – Part 1 (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 1)](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Informationen zu erweiterten Szenarien finden Sie unter [Explore Azure Disk Encryption with Azure PowerShell – Part 2 (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 2)](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/).

**F:** Welche Version von Azure PowerShell wird von Azure Disk Encryption unterstützt?

**A:** Verwenden Sie die neueste Version des Azure PowerShell SDK, um Azure Disk Encryption zu konfigurieren. Laden Sie die neueste Version von [Azure PowerShell](https://github.com/Azure/azure-powershell/releases) herunter. Azure Disk Encryption wird *nicht* vom Azure SDK Version 1.1.0 unterstützt.

> [!NOTE]
> Die Vorschauerweiterung von Azure Disk Encryption auf Linux ist veraltet. Weitere Informationen finden Sie unter [Deprecating Azure disk encryption preview extension for Linux IaaS VMs](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/) (Die Vorschauerweiterung von Azure Disk Encryption unter Linux ist veraltet).

**F:** Kann ich Azure Disk Encryption auf mein benutzerdefiniertes Linux-Image anwenden?

**A:** Sie können Azure Disk Encryption nicht auf Ihr benutzerdefiniertes Linux-Image anwenden. Wir unterstützen nur die Linux-Images im Katalog für die zuvor genannten unterstützten Distributionen. Wir unterstützen derzeit keine benutzerdefinierten Linux-Images.

**F:** Kann ich eine Linux Red Hat-VM mit dem „yum“-Update aktualisieren?

**A:** Ja, Sie können auf eine Linux Red Hat-VM einen Update- oder Patchvorgang anwenden. Weitere Informationen finden Sie unter [Applying updates to an encrypted Azure IaaS Red Hat VM by using the yum update](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/) (Anwenden von Updates auf eine verschlüsselte Azure IaaS Red Hat-VM mithilfe des yum-Updates).

**F:** Welcher Verschlüsselungsworkflow für Azure-Datenträger wird unter Linux empfohlen?

**A:** Der folgende Workflow wird für die besten Ergebnisse unter Linux empfohlen:
* Beginnen Sie mit dem unveränderten Katalogimage für die gewünschte Distribution und Version des Betriebssystems.
* Sichern Sie alle eingebundenen Laufwerke, die verschlüsselt werden.  Dies ermöglicht die Wiederherstellung bei einem Ausfall, wenn z.B. der virtuelle Computer vor dem Abschluss der Verschlüsselung neu gestartet wird.
* Führen Sie die Verschlüsselung durch (dies kann je nach VM-Merkmalen und der Größe aller angefügten Datenträger mehrere Stunden oder sogar Tage dauern).
* Passen Sie das Image an, und fügen Sie bei Bedarf Software hinzu.

Wenn dieser Workflow nicht möglich ist, stellt die [Storage Service Encryption](https://docs.microsoft.com/en-us/azure/storage/common/storage-service-encryption) (SSE, Speicherdienstverschlüsselung) auf Ebene des Plattform-Speicherkontos möglicherweise eine Alternative zur vollständigen Datenträgerverschlüsselung mit dm-crypt dar.

**F:** Wo kann ich Fragen stellen oder Feedback geben?

**A:** Sie können im [Azure Disk Encryption-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureDiskEncryption) Fragen stellen und Feedback geben.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie Antworten auf die am häufigsten gestellten Fragen im Zusammenhang mit Azure Disk Encryption erhalten. Weitere Informationen zu diesem Dienst und seinen Funktionen finden Sie in den folgenden Artikeln:

- [Anwenden der Datenträgerverschlüsselung in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Verschlüsseln eines virtuellen Azure-Computers](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Datenverschlüsselung ruhender Azure-Daten](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)

