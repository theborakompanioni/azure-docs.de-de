---
title: "Einrichten der Quelle und des Ziels für die Hyper-V-Replikation in Azure (ohne System Center VMM) mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel werden die Schritte zum Einrichten der Quell- und Zieleinstellungen für die Replikation von virtuellen Hyper-V-Computern im Azure-Speicher mit Azure Site Recovery zusammengefasst."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d2010d85-77fd-4dea-84f3-1c960ed4c63f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: b38eb3a011d46f2239891ea1d1bcac2a4059a866
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---

# <a name="step-8-set-up-the-source-and-target-for-hyper-v-replication-to-azure"></a>Schritt 8: Einrichten der Quelle und des Ziel der Hyper-V-Replikation in Azure

In diesem Artikel wird beschrieben, wie Sie die Quell- und Zieleinstellungen beim Replizieren lokaler virtueller Hyper-V-Computer (ohne System Center VMM) in Azure mit dem [Azure Site Recovery](site-recovery-overview.md)-Dienst im Azure-Portal konfigurieren.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Richten Sie die Hyper-V-Site ein, installieren Sie den Azure Site Recovery-Anbieter und den Azure Recovery Services-Agent auf Hyper-V-Hosts, und registrieren Sie die Site im Tresor.

1. Klicken Sie unter **Bereiten Sie die Infrastruktur vor** auf **Quelle**. Klicken Sie auf **+Hyper-V-Standort**, um einen neuen Hyper-V-Standort als Container für Ihre Hyper-V-Hosts oder -Cluster hinzuzufügen.

    ![Quelle einrichten](./media/hyper-v-site-walkthrough-source-target/set-source1.png)
2. Geben Sie auf dem Blatt **Hyper-V-Site erstellen** einen Namen für die Site an. Klicken Sie dann auf **OK**. Wählen Sie jetzt die erstellte Site aus, und klicken Sie auf **+Hyper-V Server**, um der Site einen Server hinzuzufügen.

    ![Quelle einrichten](./media/hyper-v-site-walkthrough-source-target/set-source2.png)

3. Vergewissern Sie sich, dass unter **Server hinzufügen** > **Servertyp** der Typ **Hyper-V Server** angezeigt wird.

    - Stellen Sie sicher, dass der Hyper-V-Server, den Sie hinzufügen möchten, die [Voraussetzungen](#on-premises-prerequisites) erfüllt und auf die angegebenen URLs zugreifen kann.
    - Laden Sie die Installationsdatei für den Azure Site Recovery-Anbieter herunter. Führen Sie diese Datei aus, um sowohl den Anbieter als auch den Recovery Services-Agent auf jedem Hyper-V-Host zu installieren.

    ![Quelle einrichten](./media/hyper-v-site-walkthrough-source-target/set-source3.png)


## <a name="install-the-provider-and-agent"></a>Installieren Sie den Anbieter und den Agent.

1. Führen Sie die Setupdatei für den Anbieter auf jedem Host aus, den Sie zum Hyper-V-Standort hinzugefügt haben. Wenn Sie auf einem Hyper-V-Cluster installieren, führen Sie das Setup auf jedem Clusterknoten aus. Durch das Installieren und Registrieren jedes Hyper-V-Clusterknotens wird sichergestellt, dass virtuelle Computer geschützt sind, selbst wenn sie über Knoten hinweg migrieren.
2. Unter **Microsoft Update** können Sie Updates aktivieren, damit Updates für den Anbieter gemäß Ihrer Microsoft Update-Richtlinie installiert werden.
3. Ändern Sie unter **Installation** den Speicherort für die Anbieterinstallation, oder akzeptieren Sie den Standardspeicherort, und klicken Sie auf **Installieren**.
4. Klicken in **Tresoreinstellungen** auf **Durchsuchen**, um die Tresorschlüsseldatei auszuwählen, die Sie heruntergeladen haben. Geben Sie das Azure Site Recovery-Abonnement, den Tresornamen und den Hyper-V-Standort an, zu dem der Hyper-V-Server gehört.

    ![Serverregistrierung](./media/hyper-v-site-walkthrough-source-target/provider3.png)

5. Geben Sie unter **Proxyeinstellungen** an, wie der auf Hyper-V-Hosts ausgeführte Anbieter eine Internetverbindung mit Azure Site Recovery herstellen soll.

    * Wenn der Anbieter eine direkte Verbindung herstellen soll, wählen Sie **Direkt mit Azure Site Recovery verbinden (ohne Proxyserver)**.
    * Wenn für den vorhandenen Proxy eine Authentifizierung erforderlich ist, oder wenn Sie einen benutzerdefinierten Proxy für die Anbieterverbindung verwenden möchten, wählen Sie **Unter Verwendung eines Proxyservers mit Azure Site Recovery verbinden** aus.
    * Wenn Sie einen Proxy verwenden:
        - Geben Sie Adresse, Port und Anmeldeinformationen ein.
        - Stellen Sie sicher, dass die in den [Voraussetzungen](#prerequisites) beschriebenen URLs vom Proxy zugelassen werden.

    ![Internet](./media/hyper-v-site-walkthrough-source-target/provider7.png)

6. Klicken Sie nach Abschluss der Installation auf **Registrieren**, um den Server im Tresor zu registrieren.

    ![Installationsspeicherort](./media/hyper-v-site-walkthrough-source-target/provider2.png)

7. Nach der Registrierung ruft Azure Site Recovery Metadaten vom Hyper-V-Server ab, und der Server wird in **Site Recovery-Infrastruktur** > **Hyper-V-Hosts** angezeigt.


## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Geben Sie das Azure-Speicherkonto für die Replikation und das Azure-Netzwerk an, mit dem Azure-VMs nach dem Failover eine Verbindung herstellen.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**.
2. Wählen Sie das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die Azure-VMs erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die VMs verwendet werden soll (klassisch oder Resource Manager).

3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

    - Wenn Sie kein Speicherkonto haben, klicken Sie auf **+Speicher**, um ein Konto auf Resource Manager-Basis inline zu erstellen. 
    - Wenn Sie nicht über ein Azure-Netzwerk verfügen, klicken Sie auf **+Netzwerk**, um ein Netzwerk auf Resource Manager-Basis inline zu erstellen.






## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 9: Einrichten einer Replikationsrichtlinie](hyper-v-site-walkthrough-replication.md).

