---
title: Vor der Replikation virtueller Azure-Computer in eine andere Region | Microsoft-Dokumentation
description: In diesem Artikel werden die Schritte zusammengefasst, die vor der Replikation virtueller Azure-Computer zwischen Azure-Regionen mithilfe des Azure Site Recovery-Diensts erforderlich sind.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: d38fc766d5226be7161433555da9622e006c80e9
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---

# <a name="step-2-before-you-start"></a>Schritt 2: Vorbereitung

Nachdem Sie die [Architektur](azure-to-azure-walkthrough-architecture.md) für die Replikation virtueller Azure-Computer (Virtual Machines, VMs) zwischen Azure-Regionen mit [Azure Site Recovery](site-recovery-overview.md) überprüft haben, können Sie mit diesem Artikel feststellen, ob die Voraussetzungen erfüllt sind.

- Am Ende des Artikels sollten Sie eine klare Vorstellung davon haben, was für die funktionierende Bereitstellung erforderlich ist, und die vorbereitenden Schritte erfüllt haben.
- Kommentare können Sie am Ende dieses Artikels senden oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) Fragen stellen.

>[!NOTE]
>
> Die Azure-VM-Replikation ist derzeit als Vorschau verfügbar.



## <a name="support-recommendations"></a>Supportempfehlungen

Siehe hierzu die nachstehende Tabelle. Eine vollständige Liste der Supportanforderungen erhalten Sie in der [Supportmatrix](site-recovery-support-matrix-azure-to-azure.md).

**Komponente** | **Anforderung**
--- | ---
**Recovery Services-Tresor** | Es wird empfohlen, einen Recovery Services-Tresor in der Azure-Zielregion zu erstellen, die für die Notfallwiederherstellung verwendet werden soll. Wenn Sie zum Beispiel Quell-VMs aus USA, Osten in USA, Mitte replizieren wollen, sollten Sie den Tresor in USA, Mitte erstellen.
**Azure-Abonnement** | Ihr Azure-Abonnement sollte zum Erstellen von VMs am Zielstandort aktiviert sein, den Sie als Region für die Notfallwiederherstellung verwenden möchten. Wenden Sie sich an den Support, um das erforderliche Kontingent zu aktivieren.
**Kapazität der Zielregion** | In der Azure-Zielregion sollte das Abonnement über ausreichend Kapazität für VMs, Speicherkonten und Netzwerkkomponenten verfügen.
**Speicher** | Verwenden Sie den [Leitfaden für Speicher](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) für Ihre Azure-Quell-VMs, um Leistungsprobleme zu vermeiden.<br/><br/> Speicherkonten müssen sich in derselben Region wie der Tresor befinden.<br/><br/> Die Replikation in Premium-Konten in den Regionen „Indien, Mitte“ und „Indien, Süden“ ist nicht möglich.<br/><br/> Wenn Sie die Replikation mit den Standardeinstellungen bereitstellen, erstellt Site Recovery die erforderlichen Speicherkonten auf Basis der Quellkonfiguration. Befolgen Sie beim Anpassen der Einstellungen die [Skalierbarkeitsziele für Festplatten virtueller Computer](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Netzwerk** | Sie müssen ausgehende Verbindungen von Azure-VMs für bestimmte URLs/IP-Adressbereiche zulassen.<br/><br/> Netzwerkkonten müssen sich in derselben Region wie der Tresor befinden.
**Azure-VM** | Stellen Sie sicher, dass alle aktuellen Stammzertifikate auf dem virtuellen Azure-Computer unter Windows/Linux vorhanden sind. Wenn dies nicht der Fall ist, können Sie den virtuellen Computer aufgrund von Sicherheitseinschränkungen nicht in Site Recovery registrieren.
**Azure-Benutzerkonto** | Ihrem Azure-Benutzerkonto müssen bestimmte [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) zum Aktivieren der Replikation eines virtuellen Azure-Computers zugewiesen sein.


## <a name="set-permissions-on-the-account"></a>Festlegen von Berechtigungen für das Konto

1. Lesen Sie mehr über die [Berechtigungen](site-recovery-role-based-linked-access-control.md), die für die Replikation erforderlich sind.
2. Befolgen Sie diese [Anweisungen](../active-directory/role-based-access-control-configure.md#add-access), um Berechtigungen hinzuzufügen.


## <a name="verify-target-resources"></a>Überprüfen der Zielressourcen

1. Aktivieren Sie Ihr Azure-Abonnement für die Erstellung von VMs in der Zielregion, die Sie für die Notfallwiederherstellung verwenden wollen. Wenden Sie sich an den Support, um das erforderliche Kontingent zu aktivieren.
2. Stellen Sie sicher, dass Ihr Abonnement über ausreichend Ressourcen verfügt, um VMs mit Größen zu ermöglichen, die den Quell-VMs entsprechen. In der Standardeinstellung wählt Site Recovery beim Einrichten der Replikation die gleiche oder eine möglichst ähnliche Größe für die Ziel-VM aus. Erfahren Sie mehr über das [Behandeln von Problemen](site-recovery-azure-to-azure-troubleshoot-errors.md#azure-resource-quota-issues-error-code-150097) mit Zielressourcen.

## <a name="verify-azure-vm-certificates"></a>Überprüfen der Azure-VM-Zertifikate

1. Stellen Sie sicher, dass alle neuesten Stammzertifikate auf den Windows- oder Linux-VMs vorhanden sind, die Sie replizieren wollen. Wenn die neuesten Stammzertifikate nicht vorhanden sind, kann die VM aufgrund von Sicherheitseinschränkungen nicht bei Site Recovery registriert werden.
2. Führen Sie für Windows-VMs folgende Schritte aus:

    - Installieren Sie alle aktuellen Windows-Updates auf der VM, damit alle vertrauenswürdigen Stammzertifikate auf dem Computer vorhanden sind.
    - Wenn Sie sich in einer nicht verbundenen Umgebung befinden, können Sie den standardmäßigen Windows Update-Prozess/Updateprozess für Zertifikate in Ihrer Organisation befolgen, um die neuesten Stammzertifikate und die aktualisierte Zertifikatssperrliste (Certificate Revocation List, CRL) abzurufen.
3. Befolgen Sie bei Linux-VMs die Anleitung Ihres Linux-Distributors, um die aktuellen vertrauenswürdigen Stammzertifikate und die aktuelle Zertifikatssperrliste auf der VM abzurufen. Erfahren Sie mehr über das [Behandeln von Problemen](site-recovery-azure-to-azure-troubleshoot-errors.md#trusted-root-certificates-error-code-151066) mit vertrauenswürdigen Stammzertifikaten.


## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zu [Schritt 3: Planen des Netzwerks](azure-to-azure-walkthrough-network.md), um eine ausgehende Verbindung herzustellen.

