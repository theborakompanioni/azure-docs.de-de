---
title: "Installieren des Mobilitätsdiensts für die Replikation von VMware in Azure | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie der Mobilitätsdienst-Agent für die Replikation von VMware in Azure mit dem Azure Site Recovery-Dienst installiert wird."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 3189fbcd-6b5b-4ffb-b5a9-e2080c37f9d9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: bc520bd2ea54208889861a7a3b275e3008a05d53
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017

---

# <a name="step-10-install-the-mobility-service"></a>Schritt 10: Installieren des Mobilitätsdiensts


In diesem Artikel wird beschrieben, wie Sie die Quell- und Zieleinstellungen beim Replizieren von lokalen VMware-VMs in Azure mit dem [Azure Site Recovery](site-recovery-overview.md)-Dienst im Azure-Portal konfigurieren können.

Der Mobilitätsdienst erfasst Datenschreibvorgänge auf einem Computer und leitet sie an den Prozessserver weiter. Er sollte auf jedem Computer installiert sein, den Sie in Azure replizieren möchten.

Sie können den Mobilitätsdienst manuell mit der Pushinstallation vom Prozessserver von Site Recovery installieren, wenn die Replikation aktiviert ist. Alternativ können Sie das Tool System Center Configuration Manager verwenden. Wenn Sie die Pushinstallation verwenden, wird der Dienst auf dem virtuellen Computer installiert, wenn die Replikation aktiviert ist.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="install-manually"></a>Manuelle Installation

1. Überprüfen Sie die [Voraussetzungen](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) für die manuelle Installation.
2. Folgen Sie [diesen Anweisungen](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) für die manuelle Installation mithilfe des Portals.
3. Wenn Sie über die Befehlszeile installieren möchten, folgen Sie [diesen Anweisungen](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-the-process-server"></a>Installieren vom Prozessserver aus

Wenn Sie die Installation des Mobilitätsdiensts vom Prozessserver aus per Push durchführen möchten, wenn die Replikation für eine VM aktiviert ist, benötigen Sie ein Konto, mit dem der Prozessserver auf die VM zugreifen kann. Das Konto wird nur für die Pushinstallation verwendet.

1. Sie sollten ein [Konto erstellt](vmware-walkthrough-prepare-vmware.md) haben, das für die Pushinstallation verwendet werden kann. Anschließend geben Sie das Konto an, das Sie beim Konfigurieren der Quelleinstellungen während der Site Recovery-Bereitstellung verwenden können.
2. Befolgen Sie dann [diese Anweisungen](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery), wenn Sie den Mobilitätsdienst mithilfe von Push auf VMs unter Windows oder Linux übertragen möchten.

## <a name="other-methods"></a>Andere Methoden

Erfahren Sie mehr über die [Installation des Mobilitätsdiensts mithilfe von Configuration Manager](site-recovery-install-mobility-service-using-sccm.md) oder über [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).

## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 11: Aktivieren der Replikation](vmware-walkthrough-enable-replication.md).

