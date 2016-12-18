---
title: "Anwenden der Datenträgerverschlüsselung in Azure Security Center | Microsoft Docs"
description: "In diesem Dokument wird gezeigt, wie Sie die Azure Security Center-Empfehlung **Datenträgerverschlüsselung anwenden** implementieren."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 6cc7824a-8d6b-4a5f-ab40-e3bbaebc4a91
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9676501e6a5c897d4a49d40c030c843e14c8560c


---
# <a name="apply-disk-encryption-in-azure-security-center"></a>Anwenden der Datenträgerverschlüsselung in Azure Security Center
Azure Security Center empfiehlt die Anwendung der Datenträgerverschlüsselung, wenn Sie über Windows- oder Linux-VM-Datenträger verfügen, die nicht per Azure Disk Encryption verschlüsselt sind. Mit der Datenträgerverschlüsselung können Sie Ihre Windows- und Linux-IaaS-VM-Datenträger verschlüsseln.  Die Verschlüsselung wird sowohl für die Betriebssystem- als auch für die Datenvolumes auf Ihrer VM empfohlen.

Bei der Datenträgerverschlüsselung werden das [BitLocker](https://technet.microsoft.com/library/cc732774.aspx)-Feature (Branchenstandard) von Windows und das [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt)-Feature von Linux verwendet, um das Betriebssystem und die Daten zu verschlüsseln. Dies trägt zum Schutz Ihrer Daten und zur Erfüllung der Sicherheits- und Complianceanforderungen Ihres Unternehmens bei. Die Datenträgerverschlüsselung ist in [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) integriert, damit Sie die Schlüssel für die Datenträgerverschlüsselung und die Geheimnisse unter Ihrem Key Vault-Abonnement steuern und verwalten können. Gleichzeitig können Sie sicherstellen, dass alle Daten auf den Datenträgern von virtuellen Computern im Ruhezustand in Ihrem [Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/) verschlüsselt sind.

> [!NOTE]
> Azure Disk Encryption wird für die folgenden Windows Server-Betriebssysteme unterstützt: Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2. Eine Datenträgerverschlüsselung wird für die folgenden Linux-Betriebssysteme unterstützt: Ubuntu, CentOS, SUSE und SUSE Linux Enterprise Server (SLES).
> 
> 

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung
1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Datenträgerverschlüsselung anwenden** aus.
2. Auf dem Blatt **Datenträgerverschlüsselung anwenden** wird eine Liste mit den VMs angezeigt, für die die Datenträgerverschlüsselung empfohlen wird.
3. Führen Sie die Schritte der Anleitung aus, um die Verschlüsselung auf diese VMs anzuwenden.

![][1]

Zum Verschlüsseln virtueller Azure-Computer, für die von Security Center festgestellt wurde, dass sie verschlüsselt werden müssen, empfehlen wir die folgenden Schritte:

* Installieren und konfigurieren Sie Azure PowerShell. So können Sie die PowerShell-Befehle ausführen, die zum Einrichten der für die Verschlüsselung virtueller Azure-Computer erforderlichen Voraussetzungen notwendig sind.
* Rufen Sie das für Azure Disk Encryption erforderliche PowerShell-Skript ab, und führen Sie es aus.
* Verschlüsseln Sie Ihre virtuellen Computer.

[Verschlüsseln eines virtuellen Azure-Computers](security-center-disk-encryption.md) werden Sie durch diese Schritte geführt.  In diesem Thema wird davon ausgegangen, dass Sie Windows 10 als Clientcomputer verwenden, von dem aus Sie die Datenträgerverschlüsselung konfigurieren.

Ihnen stehen verschiedene Methoden zur Verfügung, um die Voraussetzungen einzurichten und die Verschlüsselung für virtuelle Azure-Computer zu konfigurieren. Wenn Sie bereits über Kenntnisse in Azure PowerShell oder Azure-CLI verfügen, ziehen Sie möglicherweise andere Methoden vor. Informationen über diese anderen Ansätze erfahren Sie unter [Azure-Datenträgerverschlüsselung für virtuelle Windows- und Linux-IaaS-Computer](../security/azure-security-disk-encryption.md).

## <a name="see-also"></a>Weitere Informationen
In diesem Dokument wurde gezeigt, wie Sie die Security Center-Empfehlung „Datenträgerverschlüsselung anwenden“ implementieren. Weitere Informationen zur Datenträgerverschlüsselung finden Sie unter folgenden Themen:

* [Encryption and key management with Azure Key Vault](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (Verschlüsselung und Schlüsselverwaltung mit dem Azure-Schlüsseltresor, Video, 36 Min. 39 Sek.): Erfahren Sie, wie Sie die Verwaltung der Datenträgerverschlüsselung für IaaS-VMs und Azure Key Vault nutzen, um Ihre Daten besser zu schützen.
* [Verschlüsseln eines virtuellen Azure-Computers](security-center-disk-encryption.md) (Dokument): Erfahren Sie, wie virtuelle Azure-Computer verschlüsselt werden.
* [Azure-Datenträgerverschlüsselung](../security/azure-security-disk-encryption.md) (Dokument): Enthält Informationen zum Aktivieren der Datenträgerverschlüsselung für Windows- und Linux-VMs.

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) : Hier erfahren Sie, wie Sie Sicherheitsrichtlinien konfigurieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) : Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und auf diese reagieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md) : Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png



<!--HONumber=Nov16_HO3-->


