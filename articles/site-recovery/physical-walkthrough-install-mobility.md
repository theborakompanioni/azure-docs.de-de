---
title: "Installieren des Mobilitätsdiensts für die Replikation von physischen Servern nach Azure | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie der Mobilitätsdienst-Agent auf physischen Servern für die Replikation nach Azure mit dem Azure Site Recovery-Dienst installiert wird."
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
ms.openlocfilehash: d73267d7a64221a3138af19e9a2d5dd15809b927
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017

---

# <a name="step-9-install-the-mobility-service"></a>Schritt 9: Installieren des Mobilitätsdiensts


In diesem Artikel wird beschrieben, wie Sie die Mobilitätsdienstkomponente bei der Replikation von lokalen Windows/Linux-Servern nach Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts im Azure-Portal installieren.

Der Mobilitätsdienst erfasst Datenschreibvorgänge auf einem Computer und leitet sie an den Prozessserver weiter. Er sollte auf jedem Server installiert sein, den Sie in Azure replizieren möchten.

Sie können den Mobilitätsdienst manuell mit der Pushinstallation vom Prozessserver von Site Recovery installieren, wenn die Replikation aktiviert ist. Alternativ können Sie ein Tool wie System Center Configuration Manager verwenden. Wenn Sie die Pushinstallation verwenden, wird der Dienst auf dem Server installiert, wenn Sie die Replikation aktivieren.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="install-manually"></a>Manuelle Installation

1. Überprüfen Sie die [Voraussetzungen](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) für die manuelle Installation.
2. Folgen Sie [diesen Anweisungen](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) für die manuelle Installation mithilfe des Portals.
3. Wenn Sie über die Befehlszeile installieren möchten, folgen Sie [diesen Anweisungen](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-the-process-server"></a>Installation vom Prozessserver aus

Wenn Sie die Installation des Mobilitätsdiensts vom Prozessserver aus per Push durchführen möchten, wenn die Replikation für einen Computer aktiviert ist, benötigen Sie ein Konto, mit dem der Prozessserver auf den Computer zugreifen kann. Das Konto wird nur für die Pushinstallation verwendet.

1. Wenn Sie noch kein Konto erstellt haben, befolgen Sie dazu folgende Richtlinien:

    - Sie können ein Domänenkonto oder ein lokales Konto verwenden
    - Wenn Sie kein Domänenkonto verwenden, müssen Sie für Windows die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren. Zu diesem Zweck fügen Sie in der Registrierung unter **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** den DWORD-Eintrag **LocalAccountTokenFilterPolicy** mit dem Wert 1 hinzu.
    - Geben Sie Folgendes ein, wenn Sie den Registrierungseintrag für Windows über eine CLI hinzufügen möchten:

        ```
        REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.
        ```

    - Bei Linux muss auf dem Linux-Quellserver das root-Konto verwendet werden.

2. Befolgen Sie dann [diese Anweisungen](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery), wenn Sie den Mobilitätsdienst mithilfe von Push auf VMs unter Windows oder Linux übertragen möchten.

## <a name="other-installation-methods"></a>Andere Installationsmethoden

- [Erfahren Sie mehr über](site-recovery-install-mobility-service-using-sccm.md) die Installation des Mobilitätsdiensts mithilfe von Configuration Manager
- [Erfahren Sie mehr über](site-recovery-automate-mobility-service-install.md) die Installation mit Azure Automation DSC.


## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 10: Aktivieren der Replikation](physical-walkthrough-enable-replication.md)

