---
title: Replizieren physischer lokaler Server in Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation
description: "Bietet eine Übersicht über die Schritte zum Replizieren von Workloads auf physischen lokalen Windows-/Linux-Servern in Azure mit dem Azure Site Recovery-Dienst."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 20122f01-929a-4675-b85b-a9b99d2618bc
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 0a09b35e98dc0b2f5283c2a707a3a2b8ac9a39f2
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="replicate-physical-servers-to-azure-with-site-recovery"></a>Replizieren physischer Server in Azure mithilfe von Site Recovery

Dieser Artikel bietet einen Überblick über die erforderlichen Schritte zum Replizieren lokaler Windows/Linux-Server in Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts im Azure-Portal.


## <a name="step-1-review-architecture-and-prerequisites"></a>Schritt 1: Überprüfen der Architektur und der Voraussetzungen

Bevor Sie die Bereitstellung beginnen, überprüfen Sie die Szenarioarchitektur, und stellen Sie sicher, dass Sie mit allen Komponenten vertraut sind, die Sie für die Bereitstellung einrichten müssen.

Gehen Sie zu [Schritt 1: Überprüfen der Architektur](physical-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Schritt 2: Überprüfen der Voraussetzungen

Stellen Sie sicher, dass Sie über die Voraussetzungen für jede Bereitstellungskomponente verfügen:

- **Azure-Voraussetzungen**: Sie benötigen ein Microsoft Azure-Konto, Azure-Netzwerke und Speicherkonten.
- **Lokale Site Recovery-Komponenten**: Sie benötigen einen Computer, der lokale Site Recovery-Komponenten ausführt.
- **Replizierte Computer**: Server, die Sie replizieren möchten, müssen lokale und Azure-Anforderungen entsprechen.

Gehen Sie zu [Schritt 2: Überprüfen der Voraussetzungen und Einschränkungen](physical-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Schritt 3: Planen der Kapazität

Wenn Sie eine vollständige Bereitstellung ausführen, müssen Sie herausfinden, welche Replikationsressourcen benötigt werden. Wenn Sie zum Testen der Umgebung ein schnelles Setup durchführen können Sie diesen Schritt überspringen.

Gehen Sie zu [Schritt 3: Planen der Kapazität](physical-walkthrough-capacity.md).

## <a name="step-4-plan-networking"></a>Schritt 4: Planen von Netzwerken

Sie müssen einige Netzwerke planen, um sicherzustellen, dass Azure-VMs nach dem Auftreten von Failover mit Netzwerken verbunden sind und die richtigen IP-Adressen besitzen.

Gehen Sie zu [Schritt 4: Planen von Netzwerken](physical-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>Schritt 5: Vorbereiten der Azure-Ressourcen

Richten Sie Azure-Netzwerke und -Speicher ein, bevor Sie beginnen. 

Gehen Sie zu [Schritt 5: Vorbereiten von Azure](physical-walkthrough-prepare-azure.md)


## <a name="step-6-set-up-a-vault"></a>Schritt 6: Einrichten eines Tresors

Sie richten einen Recovery Services-Tresor ein, um Replikationen zu orchestrieren und zu verwalten. Bei der Einrichtung des Tresors geben Sie an, was Sie replizieren möchten, und wo es repliziert werden soll.

Gehen Sie zu [Schritt 6: Einrichten eines Tresors](physical-walkthrough-create-vault.md)

## <a name="step-7-configure-source-and-target-settings"></a>Schritt 7: Konfigurieren von Quell-und Zieleinstellungen

Konfigurieren Sie Einstellungen für Quell- und Ziel-(Azure)-Site. Die Quelleinstellungen enthalten das Ausführen des einheitlichen Setups, um die lokalen Site Recovery-Komponenten zu installieren.

Gehen Sie zu [Schritt 7: Quelle und Ziel austauschen](physical-walkthrough-source-target.md)

## <a name="step-8-set-up-a-replication-policy"></a>Schritt 8: Einrichten einer Replikationsrichtlinie

Sie richten eine Richtlinie ein, um festzulegen, wie die physischen Server replizieren sollen.

Gehen Sie zu [Schritt 8: Einrichten einer Replikationsrichtlinie](physical-walkthrough-replication.md)

## <a name="step-9-install-the-mobility-service"></a>Schritt 9: Installieren des Mobilitätsdiensts

Der Mobilitätsdienst muss auf jedem Computer installiert sein, den Sie replizieren möchten. Es gibt mehrere Möglichkeiten, um den Dienst mit der Push- oder Pullinstallation einzurichten.

Gehen Sie zu [Schritt 9: Installieren des Mobilitätsdiensts](physical-walkthrough-install-mobility.md)

## <a name="step-10-enable-replication"></a>Schritt 10: Aktivieren der Replikation

Nachdem der Mobilitätsdienst auf einem Server ausgeführt wird, können Sie für ihn die Replikation aktivieren. Nach der Aktivierung erfolgt eine erste Replikation des virtuellen Computers.

Gehen Sie zu [Schritt 10: Aktivieren der Replikation](physical-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>Schritt 11: Ausführen eines Testfailovers

Nachdem die anfängliche Replikation abgeschlossen wurde und Deltareplikation ausgeführt wird, können Sie ein Testfailover ausführen, um sicherzustellen, dass alles wie erwartet funktioniert.

Gehen Sie zu [Schritt 11: Ausführen eines Testfailovers](physical-walkthrough-test-failover.md)


