---
title: Replizieren von VMware-VMs in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: "Dieser Artikel gibt einen Überblick über die Schritte zum Replizieren von Workloads, die in VMware-VMs ausgeführt werden, in Azure."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: db6f5f95929503e82a529dba26b56af1edb0767f
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="replicate-vmware-vms-to-azure-with-site-recovery"></a>Replizieren von VMware-VMs in Azure mit Site Recovery

In diesem Artikel erfahren Sie, wie Sie lokale virtuelle VMware-Computer mit dem [Azure Site Recovery](site-recovery-overview.md)-Dienst über das Azure-Portal in Azure replizieren können.


![Bereitstellungsprozess](./media/vmware-walkthrough-overview/vmware-to-azure-process.png)

**Abbildung 1: Zusammenfassung des Bereitstellungsprozesses**

## <a name="step-1-review-architecture-and-prerequisites"></a>Schritt 1: Überprüfen der Architektur und der Voraussetzungen

Überprüfen Sie die Szenarioarchitektur, bevor Sie mit der Bereitstellung beginnen, und stellen Sie sicher, dass Sie mit allen Komponenten vertraut sind, die Sie bereitstellen möchten.

Gehen Sie zu [Schritt 1: Überprüfen der Architektur](vmware-walkthrough-architecture.md).


## <a name="step-2-review-prerequisites"></a>Schritt 2: Überprüfen der Voraussetzungen

Stellen Sie sicher, dass Sie über die erforderlichen Komponenten für jede Bereitstellungskomponente verfügen:

- **Azure-Voraussetzungen**: Sie benötigen ein Microsoft Azure-Konto, Azure-Netzwerke und Speicherkonten.
- **Lokale Site Recovery-Komponenten**: Sie benötigen einen Computer mit lokalen Site Recovery-Komponenten.
- **Lokale VMware-Voraussetzungen**: Sie müssen Konten einrichten, damit Site Recovery auf VMware-Server und VMs zugreifen kann.
- **Replizieren virtueller Computer**: VMs, die Sie replizieren möchten, müssen den Azure-Anforderungen entsprechen, und die Komponente des Mobilitätsdiensts muss installiert sein.

Gehen Sie zu [Schritt 2: Überprüfen der Voraussetzungen und Einschränkungen](vmware-walkthrough-prerequisites.md).

## <a name="step-3-plan-capacity"></a>Schritt 3: Planen der Kapazität

Wenn Sie eine vollständige Bereitstellung ausführen, müssen Sie herausfinden, welche Replikationsressourcen benötigt werden. Es gibt eine Reihe von Tools, die Sie dabei unterstützt. Fahren Sie mit Schritt 2 fort. Wenn Sie eine schnelle Einrichtung vornehmen, um die Umgebung zu testen, können Sie diesen Schritt überspringen.

Gehen Sie zu [Schritt 3: Planen der Kapazität](vmware-walkthrough-capacity.md).

## <a name="step-4-plan-networking"></a>Schritt 4: Planen von Netzwerken

Sie müssen einige Netzwerke planen, um sicherzustellen, dass Azure-VMs mit Netzwerken verbunden sind, nachdem es ein Failover gab, und dass sie die richtige IP-Adressen besitzen.

Gehen Sie zu [Schritt 4: Planen von Netzwerken](vmware-walkthrough-network.md).

##  <a name="step-5-prepare-azure-resources"></a>Schritt 5: Vorbereiten der Azure-Ressourcen

Richten Sie Azure-Netzwerke und -Speicher ein, bevor Sie beginnen. Dies können Sie während der Bereitstellung durchführen. Es wird jedoch empfohlen, dass sie es vorher machen.

Gehen Sie zu [Schritt 5: Vorbereiten von Azure](vmware-walkthrough-prepare-azure.md).


## <a name="step-6-prepare-vmware"></a>Schritt 6: Vorbereiten von VMware

Sie müssen Konten einrichten, die Site Recovery verwenden kann, um Folgendes durchführen zu können:

- Zugreifen auf Virtualisierungsserver von VMware, um VMs automatisch zu erkennen
- Zugreifen auf VMs, um den Mobilitätsdienst zu installieren Auf jedem virtuellen Computer, den Sie replizieren möchten, muss der Mobilitätsdienst-Agent installiert sein, bevor Sie die Replikation für ihn aktivieren können.

Gehen Sie zu [Schritt 6: Vorbereiten von VMware](vmware-walkthrough-prepare-vmware.md).

## <a name="step-7-set-up-a-vault"></a>Schritt 7: Einrichten eines Tresors

Sie müssen einen Tresor für Recovery Services einrichten, der die Replikation orchestrieren und verwalten kann. Bei der Einrichtung des Tresors geben Sie an, was Sie replizieren möchten, und wo es repliziert werden soll.

Gehen Sie zu [Schritt 7: Einrichten eines Tresors](vmware-walkthrough-create-vault.md).

## <a name="step-8-configure-source-and-target-settings"></a>Schritt 8: Konfigurieren von Quell-und Zieleinstellungen

Richten Sie die Quelle und das Ziel ein, die für die Replikation verwendet werden. Wenn Sie die Quelleinstellungen vornehmen, wird das einheitliche Setup ausgeführt, um die lokalen Site Recovery-Komponenten zu installieren.

Gehen Sie zu [Schritt 8: Einrichten der Quelle und des Ziels](vmware-walkthrough-source-target.md)

## <a name="step-9-set-up-a-replication-policy"></a>Schritt 9: Aufstellen einer Replikationsrichtlinie

Sie stellen eine Richtlinie auf, um die Replikationseinstellungen für VMware-VMs im Tresor anzugeben.

Gehen Sie zu [Schritt 9: Aufstellen einer Replikationsrichtlinie](vmware-walkthrough-replication.md).

## <a name="step-10-install-the-mobility-service"></a>Schritt 10: Installieren des Mobilitätsdiensts

Der Mobilitätsdienst muss auf jeder VMs installiert sein, den Sie replizieren möchten. Es gibt mehrere Möglichkeiten, um den Dienst mit der Push- oder Pullinstallation einzurichten.

Gehen Sie zu [Schritt 10: Installieren des Mobilitätsdiensts](vmware-walkthrough-install-mobility.md).

## <a name="step-11-enable-replication"></a>Schritt 11: Aktivieren der Replikation

Wenn der Mobilitätsdienst auf einem virtuellen Computer ausgeführt wird, können Sie die Replikation für ihn aktivieren. Nach der Aktivierung erfolgt eine erste Replikation des virtuellen Computers.

Gehen Sie zu [Schritt 11: Aktivieren der Replikation](vmware-walkthrough-enable-replication.md).

## <a name="step-12-run-a-test-failover"></a>Schritt 12: Ausführen eines Testfailovers

Nachdem die erste Replikation abgeschlossen wurde, und die Deltareplikation ausgeführt wird, können Sie ein Testfailover ausführen, um sicherzustellen, dass alles wie erwartet funktioniert.

Gehen Sie zu [Schritt 12: Ausführen eines Testfailovers](vmware-walkthrough-test-failover.md).

