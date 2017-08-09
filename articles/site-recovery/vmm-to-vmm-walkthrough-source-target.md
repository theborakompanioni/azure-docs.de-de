---
title: "Einrichten der Quelle und des Ziels für die Hyper-V-Replikation an einem sekundären Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Beschreibt, wie die Quelle und das Ziel beim Replizieren virtueller Hyper-V-Computer an einem sekundären VMM-Standort mit Azure Site Recovery eingerichtet werden."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: fa7809f1-7633-425f-b25d-d10d004e8d0b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 07135e9b5e619971a59cc22ec68a0a4e8bcaabe1
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="step-6-set-up-the-replication-source-and-target"></a>Schritt 6: Einrichten von Replikationsquelle und -ziel


Nach dem Erstellen eines Recovery Services-Tresors für die Hyper-V-Replikation an einem sekundären VMM-Standort mit [Azure Site Recovery](site-recovery-overview.md) richten Sie anhand der Informationen in diesem Artikel die Quell- und Zielstandorte für die Replikation ein. 

Kommentare können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) hinterlassen.




## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Installieren Sie den Azure Site Recovery-Anbieter auf den VMM-Servern, und ermitteln und registrieren Sie die Server im Tresor.

1. Klicken Sie auf **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Quelle**.

    ![Quelle einrichten](./media/vmm-to-vmm-walkthrough-source-target/goals-source.png)
2. Klicken Sie unter **Quelle vorbereiten** auf **+ VMM**, um einen VMM-Server hinzuzufügen.

    ![Quelle einrichten](./media/vmm-to-vmm-walkthrough-source-target/set-source1.png)
3. Vergewissern Sie sich unter **Server hinzufügen**, dass unter **Servertyp** der Eintrag **System Center-VMM-Server** angezeigt wird und dass der VMM-Server die [Voraussetzungen](#prerequisites) erfüllt.
4. Laden Sie die Installationsdatei für den Azure Site Recovery-Anbieter herunter.
5. Laden Sie den Registrierungsschlüssel herunter. Sie benötigen diese Angaben beim Ausführen des Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

    ![Quelle einrichten](./media/vmm-to-vmm-walkthrough-source-target/set-source3.png)
6. Installieren Sie den Azure Site Recovery-Anbieter auf dem VMM-Server. Sie müssen nicht explizit etwas auf Hyper-V-Hostservern installieren.


## <a name="install-the-azure-site-recovery-provider"></a>Installieren des Azure Site Recovery-Anbieters

1. Führen Sie die Anbietersetupdatei auf jedem VMM-Server aus. Falls VMM in einem Cluster bereitgestellt wird, gehen Sie bei der erstmaligen Installation wie folgt vor:
    -  Installieren Sie den Anbieter auf einem aktiven Knoten, und schließen Sie die Installation ab, um den VMM-Server im Tresor zu registrieren.
    - Installieren Sie den Anbieter anschließend auf den anderen Knoten. Auf allen Clusterknoten sollte die gleiche Version des Anbieters ausgeführt werden.
2. Das Setup führt einige Voraussetzungsüberprüfungen durch und fordert die Berechtigung zum Beenden des VMM-Diensts an. Der VMM-Dienst wird nach Abschluss des Setups automatisch neu gestartet. Bei der Installation in einem VMM-Cluster werden Sie aufgefordert, die Clusterrolle zu beenden.
3. Unter **Microsoft Update** können Sie Updates aktivieren, damit Updates für den Anbieter gemäß Ihrer Microsoft Update-Richtlinie installiert werden.
4. Ändern Sie unter **Installation** den Speicherort für die Installation, oder akzeptieren Sie den Standardspeicherort, und klicken Sie auf **Installieren**.

    ![Installationsspeicherort](./media/vmm-to-vmm-walkthrough-source-target/provider-location.png)
5. Klicken Sie nach Abschluss der Installation auf **Registrieren**, um den Server im Tresor zu registrieren.

    ![Installationsspeicherort](./media/vmm-to-vmm-walkthrough-source-target/provider-register.png)
6. Prüfen Sie unter **Tresorname**den Namen des Tresors, in dem der Server registriert wird. Klicken Sie auf *Weiter*.

    ![Serverregistrierung](./media/vmm-to-vmm-walkthrough-source-target/vaultcred.png)
7. Geben Sie unter **Internetverbindung** an, wie der auf dem VMM-Server ausgeführte Anbieter eine Verbindung mit Azure herstellen soll.

    ![Interneteinstellungen](./media/vmm-to-vmm-walkthrough-source-target/proxydetails.png)

   - Der Anbieter kann entweder eine direkte oder eine proxybasierte Internetverbindung herstellen.
   - Geben Sie ggf. Proxyeinstellungen an.
   - Bei Verwendung eines Proxys wird automatisch ein ausführendes VMM-Konto (DRAProxyAccount) mit den angegebenen Proxyanmeldeinformationen erstellt. Konfigurieren Sie den Proxyserver so, dass dieses Konto erfolgreich authentifiziert werden kann. Die Einstellungen für ausführende Konten können über die VMM-Konsole unter **Einstellungen** > **Sicherheit** > **Ausführende Konten** geändert werden. Starten Sie den VMM-Dienst neu, um die Änderungen zu übernehmen.
8. Wählen Sie unter **Registrierungsschlüssel**den Schlüssel aus, den Sie von Azure Site Recovery heruntergeladen und auf den VMM-Server kopiert haben.
9. Die Verschlüsselungseinstellung wird nur verwendet, wenn Sie Hyper-V-VMs in VMM-Clouds in Azure replizieren. Bei der Replikation an einem sekundären Standort wird sie nicht verwendet.
10. Geben Sie unter **Servername**einen Anzeigenamen ein, um den VMM-Server im Tresor zu identifizieren. Geben Sie in einer Clusterkonfiguration den Rollennamen des VMM-Clusters an.
11. Wählen Sie unter **Cloudmetadaten synchronisieren** aus, ob Sie Metadaten für alle Clouds auf dem VMM-Server mit dem Tresor synchronisieren möchten. Diese Aktion muss für jeden VMM-Server nur einmal ausgeführt werden. Wenn Sie nicht alle Clouds synchronisieren möchten, können Sie diese Einstellung deaktiviert lassen und in den Cloudeigenschaften in der VMM-Konsole jede Cloud einzeln synchronisieren.
12. Klicken Sie auf **Weiter** , um den Prozess abzuschließen. Nach der Registrierung werden die Metadaten vom VMM-Server von Azure Site Recovery abgerufen. Der Server wird im Tresor auf der Registerkarte **VMM-Server** der Seite **Server** angezeigt.

    ![Server](./media/vmm-to-vmm-walkthrough-source-target/provider13.png)
13. Sobald der Server in der Site Recovery-Konsole verfügbar ist, wählen Sie unter **Quelle** > **Quelle vorbereiten** den VMM-Server sowie die Cloud aus, in der sich der Hyper-V-Host befindet. Klicken Sie dann auf **OK**.

Der Anbieter kann auch über die Befehlszeile installiert werden:

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie VMM-Zielserver und Cloud aus:

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie den gewünschten VMM-Zielserver aus.
2. Clouds auf dem Server, die mit Site Recovery synchronisiert werden, werden angezeigt. Wählen Sie die Zielcloud aus.

   ![Ziel](./media/vmm-to-vmm-walkthrough-source-target/target-vmm.png)



## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zu [Schritt 7: Konfigurieren der Netzwerkzuordnung](vmm-to-vmm-walkthrough-network-mapping.md).

