---
title: Azure Disk Encryption FAQ| Microsoft-Dokumentation
description: "Dieser Artikel bietet Antworten zu FAQ über Microsoft Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer."
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
ms.sourcegitcommit: a9cfd6052b58fe7a800f1b58113aec47a74095e3
ms.openlocfilehash: 0d15bf42c156ea7a72c54d690f4016877913efe4
ms.contentlocale: de-de
ms.lasthandoff: 08/12/2017

---
# <a name="azure-disk-encryption-frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) zu Azure Disk Encryption

Diese FAQ beantworten Fragen zu Azure Disk Encryption für Windows- und Linux-IaaS-VMs. Weitere Informationen zu diesem Dienst finden Sie unter [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="general-questions"></a>Allgemeine Fragen
**F.** In welcher Region ist Azure Disk Encryption allgemein verfügbar?

**A:** Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer ist in allen öffentlichen Azure-Regionen allgemein verfügbar.

**F:** Welche Benutzeroberflächen sind für Azure Disk Encryption verfügbar?

**A:** Azure Disk Encryption mit allgemeiner Verfügbarkeit unterstützt Azure Resource Manager-Vorlagen, Azure PowerShell und die Azure-Befehlszeilenschnittstelle. Dies bietet Ihnen ein hohes Maß an Flexibilität, da Ihnen drei verschiedene Optionen zum Aktivieren der Datenträgerverschlüsselung für Ihre IaaS-VMs zur Verfügung stehen. Weitere Informationen zur Benutzeroberfläche und eine Schritt-für-Schritt-Anleitung sind in den Bereitstellungsszenarios und Erfahrungen von Azure Disk Encryption verfügbar.

**F:** Was kostet Azure Disk Encryption?

**A:** Für die Verschlüsselung von VM-Datenträgern mit Azure Disk Encryption fallen keine Gebühren an.

**F:** Mit welchen VM-Tarifen kann ich Azure Disk Encryption verwenden?

**A:** Azure Disk Encryption steht nur für virtuelle Computer im Standard-Tarif wie [A, D, DS, G, GS, F](https://azure.microsoft.com/pricing/details/virtual-machines/) sowie für virtuelle IaaS-Computer (einschließlich virtuelle Computer mit Storage Premium) zur Verfügung. Es ist nicht für VMs im Basic-Tarif verfügbar.

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

**A:** Informationen zu den ersten Schritten finden Sie im [Whitepaper zu Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

**F:** Kann ich sowohl Start- als auch Datenvolumes mit Azure Disk Encryption verschlüsseln?

**A:** Ja. Sie können Start- und Datenvolumes für virtuelle Windows- und Linux-IaaS-Computer verschlüsseln. Für Windows-VMs können Sie die Daten ohne vorherige Verschlüsselung des Betriebssystemvolumes nicht verschlüsseln. Für Linux-VMs können Sie das Datenvolume ohne vorherige Verschlüsselung des Betriebssystemvolumes verschlüsseln. Nachdem Sie das Betriebssystemvolume für Liux verschlüsselt haben, wird das Deaktivieren der Verschlüsselung auf einem Betriebssystemvolume für Linux-IaaS-VMs nicht unterstützt.

**F:** Ermöglicht Azure Disk Encryption die Verwendung von „Bring Your Own Key“ (BYOK)?

**A:** Ja. Sie können Ihre eigenen Schlüssel für die Verschlüsselung anderer Schlüssel (Key Encryption Keys, KEKs) angeben. Diese Schlüssel werden in Azure Key Vault, dem Schlüsselspeicher für Azure Disk Encryption, aufbewahrt. Weitere Informationen zu den einzelnen KEK-Szenarios finden Sie in den Bereitstellungsszenarios und Erfahrungen von Azure Disk Encryption.

**F:** Kann ich einen von Azure erstellten KEK verwenden?

**A:** Ja. Sie können Azure Key Vault verwenden, um einen KEK für Azure Disk Encryption zu erstellen. Diese Schlüssel werden in Azure Key Vault, dem Schlüsselspeicher für Azure Disk Encryption, aufbewahrt. Weitere Informationen zu den einzelnen KEK-Szenarios finden Sie in den Bereitstellungsszenarios und Erfahrungen von Azure Disk Encryption.

**F:** Kann ich den lokalen Schlüsselverwaltungsdienst/HSM verwenden, um die Verschlüsselungsschlüssel zu schützen?

**A:** Sie können den lokalen Schlüsselverwaltungsdienst/HSM nicht verwenden, um die Verschlüsselungsschlüssel mit Azure Disk Encryption zu schützen. Sie können nur den Azure Key Vault-Dienst verwenden, um die Verschlüsselungsschlüssel zu schützen. Weitere Informationen zu den einzelnen KEK-Szenarios finden Sie in den Bereitstellungsszenarios und Erfahrungen von Azure Disk Encryption.

**F:** Was sind die Voraussetzungen für die Konfiguration von Azure Disk Encryption?

**A:** Das für Azure Disk Encryption erforderliche PowerShell-Skript zum Erstellen einer Azure AD-Anwendung, zum Erstellen eines neuen Schlüsseltresors oder zum Einrichten eines vorhandenen Schlüsseltresors für den Zugriff auf die Datenträgerverschlüsselung, um Geheimnisse und Schlüssel zu schützen.  Weitere Informationen zu den einzelnen KEK-Supportszenarios finden Sie in den Voraussetzungen, Bereitstellungsszenarios und Erfahrungen von Azure Disk Encryption.

**F:** Wo erhalte ich weitere Informationen zum Konfigurieren von Azure Disk Encryption mithilfe von PowerShell?

**A:** Wir bieten einige hervorragende Artikel zu grundlegenden Aufgaben für Azure Disk Encryption sowie zu komplexeren Szenarien. Lesen Sie für die grundlegenden Aufgaben [Explore Azure Disk Encryption with Azure PowerShell – Part 1 (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 1)](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Lesen Sie für die erweiterten Szenarios [Explore Azure Disk Encryption with Azure PowerShell – Part 2 (Erkunden von Azure Disk Encryption mit Azure PowerShell – Teil 2)](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/).

**F:** Welche Version von Azure PowerShell wird von Azure Disk Encryption unterstützt?

**A:** Verwenden Sie die neueste Version des Azure PowerShell SDKs, um Azure Disk Encryption zu konfigurieren. Laden Sie die neueste Version von [Azure PowerShell](https://github.com/Azure/azure-powershell/releases) herunter. Azure Disk Encryption wird NICHT von Azure SDK Version 1.1.0 unterstützt.

> [!NOTE]
> Die Vorschauerweiterung von Azure Disk Encryption auf Linux ist veraltet. Einzelheiten finden Sie in der Dokumentation [hier](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/).

**F:** Kann ich Azure Disk Encryption auf mein benutzerdefiniertes Linux-Image anwenden?

**A:** Sie können Azure Disk Encryption nicht auf Ihr benutzerdefiniertes Linux-Image anwenden. Wir unterstützen nur die Katalog-Linux-Images für die oben genannten unterstützten Distributionen. Wir unterstützen benutzerdefinierte Linux-Images derzeit nicht.

**F:** Kann ich eine virtuellen Linux Red Hat-Computer mit Yum-Update aktualisieren?

**A:** Ja. Sie können gemäß [dieser Anleitung](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/) ein Update und/oder einen Patch für einen virtuellen Linux Red Hat-Computer anwenden.

**F:** Wo kann ich Fragen stellen oder Feedback abgeben?

**A:** Sie können Fragen im [Forum zu Azure Disk Encryption](https://social.msdn.microsoft.com/Forums/home?forum=AzureDiskEncryption) stellen. Dort können Sie auch Feedback abgeben.

## <a name="see-also"></a>Weitere Informationen
In diesem Artikel haben Sie mehr über die häufigsten Fragen im Zusammenhang mit Azure Disk Encryption erfahren. Für weitere Informationen zu diesem Dienst und seinen Funktionen können Sie Folgendes lesen:

- [Anwenden der Datenträgerverschlüsselung in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Verschlüsseln eines virtuellen Azure-Computers](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Azure-Datenverschlüsselung ruhender Daten](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)

