---
title: "Einrichten der Quelle und des Ziels für die Hyper-V-Replikation in Azure (mit System Center VMM) mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel werden die Schritte zum Einrichten der Quell- und Zieleinstellungen für die Replikation von virtuellen Hyper-V-Computern in VMM-Clouds in Azure Storage mit Azure Site Recovery zusammengefasst."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5edb6d87-25a5-40fe-b6f1-ddf7b55a6b31
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/25/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: c3c30d047498e30164903c5884080a4db08613e3
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---

# <a name="step-8-set-up-the-source-and-target-for-hyper-v-with-vmm-replication-to-azure"></a>Schritt 8: Einrichten der Quelle und des Ziels der Hyper-V-Replikation (mit VMM) in Azure

Nach dem [Erstellen eines Tresors](vmm-to-azure-walkthrough-create-vault.md) und der Angabe, was repliziert werden soll, konfigurieren Sie anhand der Informationen in diesem Artikel die Quell- und Zieleinstellungen beim Replizieren lokaler virtueller Hyper-V-Computer in System Center Virtual Machine Manager (VMM) in Azure mit dem [Azure Site Recovery](site-recovery-overview.md)-Dienst im Azure-Portal.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

S1. Klicken Sie auf **Infrastruktur vorbereiten** > **Quelle**.

    ![Set up source](./media/vmm-to-azure-walkthrough-source-target/set-source1.png)

2. Klicken Sie unter **Quelle vorbereiten** auf **+ VMM**, um einen VMM-Server hinzuzufügen.

    ![Quelle einrichten](./media/vmm-to-azure-walkthrough-source-target/set-source2.png)

3. Vergewissern Sie sich unter **Server hinzufügen**, dass unter **Servertyp** der Eintrag **System Center-VMM-Server** angezeigt wird und dass der VMM-Server die [Voraussetzungen und URL-Anforderungen](#prerequisites) erfüllt.
4. Laden Sie die Installationsdatei für den Azure Site Recovery-Anbieter herunter.
5. Laden Sie den Registrierungsschlüssel herunter. Sie benötigen diese Angaben beim Ausführen des Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

    ![Quelle einrichten](./media/vmm-to-azure-walkthrough-source-target/set-source3.png)

## <a name="install-the-provider-on-the-vmm-server"></a>Installieren des Anbieters auf dem VMM-Server

1. Führen Sie die Anbietersetupdatei auf dem VMM-Server aus.
2. Unter **Microsoft Update** können Sie Updates aktivieren, damit Updates für den Anbieter gemäß Ihrer Microsoft Update-Richtlinie installiert werden.
3. Ändern Sie unter **Installation** den Speicherort für die Anbieterinstallation, oder akzeptieren Sie den Standardspeicherort, und klicken Sie auf **Installieren**.

    ![Installationsspeicherort](./media/vmm-to-azure-walkthrough-source-target/provider2.png)
4. Klicken Sie nach Abschluss der Installation auf **Registrieren**, um den VMM-Server im Tresor zu registrieren.
5. Klicken Sie auf der Seite **Tresoreinstellungen** auf **Durchsuchen**, um die Tresorschlüsseldatei auszuwählen. Geben Sie das Azure Site Recovery-Abonnement und den Tresornamen an.

    ![Serverregistrierung](./media/vmm-to-azure-walkthrough-source-target/provider10.png)
6. Geben Sie unter **Internetverbindung**an, wie für den Anbieter, der auf dem VMM-Server ausgeführt wird, über das Internet eine Verbindung mit Site Recovery hergestellt wird.

   * Wenn der Anbieter eine direkte Verbindung herstellen soll, wählen Sie **Direkt mit Azure Site Recovery verbinden (ohne Proxyserver)** aus.
   * Wenn für den vorhandenen Proxy eine Authentifizierung erforderlich ist oder wenn Sie einen benutzerdefinierten Proxy verwenden möchten, wählen Sie **Unter Verwendung eines Proxyservers mit Azure Site Recovery verbinden** aus.
   * Geben Sie bei einem benutzerdefinierten Proxy die Adresse, den Port und Anmeldeinformationen ein.
   * Bei Verwendung eines Proxys sollten Sie die unter [Voraussetzungen](#on-premises-prerequisites) beschriebenen URLs bereits zugelassen haben.
   * Wenn Sie einen benutzerdefinierten Proxy verwenden, wird ein ausführendes VMM-Konto (DRAProxyAccount) automatisch mit den angegebenen Proxyanmeldeinformationen erstellt. Konfigurieren Sie den Proxyserver so, dass dieses Konto erfolgreich authentifiziert werden kann. In der VMM-Konsole können die Einstellungen des ausführenden VMM-Kontos geändert werden. Erweitern Sie unter **Einstellungen** die Option **Sicherheit** > **Ausführende Konten**, und ändern Sie das Kennwort für DRAProxyAccount. Sie müssen den VMM-Dienst neu starten, damit diese Einstellung wirksam wird.

     ![Internet](./media/vmm-to-azure-walkthrough-source-target/provider13.png)
7. Akzeptieren oder ändern Sie den Speicherort eines SSL-Zertifikats, das für die Datenverschlüsselung automatisch generiert wird. Dieses Zertifikat wird verwendet, wenn Sie Datenverschlüsselung für eine Cloud im Azure Site Recovery-Portal aktivieren. Bewahren Sie dieses Zertifikat sicher auf. Wenn Sie ein Failover zu Azure ausführen, benötigen Sie es für die Entschlüsselung, falls die Datenverschlüsselung aktiviert ist.
8. Geben Sie unter **Servername**einen Anzeigenamen ein, um den VMM-Server im Tresor zu identifizieren. Geben Sie in einer Clusterkonfiguration den Rollennamen des VMM-Clusters an.
9. Aktivieren Sie die Option **Cloudmetadaten synchronisieren**, wenn Metadaten für alle Clouds auf dem VMM-Server mit dem Tresor synchronisiert werden sollen. Diese Aktion muss für jeden VMM-Server nur einmal ausgeführt werden. Wenn Sie nicht alle Clouds synchronisieren möchten, können Sie diese Einstellung deaktiviert lassen und in den Cloudeigenschaften in der VMM-Konsole jede Cloud einzeln synchronisieren. Klicken Sie auf **Registrieren** , um den Prozess abzuschließen.

    ![Serverregistrierung](./media/vmm-to-azure-walkthrough-source-target/provider16.png)
10. Die Registrierung beginnt. Nach Abschluss der Registrierung wird der Server unter **Site Recovery-Infrastruktur** > **VMM-Server** angezeigt.


## <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Installieren des Azure Recovery Services-Agent auf den Hyper-V-Hosts

1. Nachdem Sie den Anbieter eingerichtet haben, müssen Sie die Installationsdatei für den Azure Recovery Services-Agent herunterladen. Führen Sie das Setup auf jedem Hyper-V-Server in der VMM-Cloud aus.

    ![Hyper-V-Standorte](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent1.png)
2. Klicken Sie in der **Voraussetzungsüberprüfung** auf **Weiter**. Alle fehlenden Komponenten, die Voraussetzung sind, werden automatisch installiert.

    ![Voraussetzungen Recovery Services-Agent](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent2.png)
3. Übernehmen oder ändern Sie in den **Installationseinstellungen** den Installationsspeicherort und den Cachespeicherort. Sie können den Cache auf einem Laufwerk konfigurieren, auf dem mindestens 5 GB Speicherplatz verfügbar sind, aber wir raten Ihnen zu einem Cachelaufwerk mit mindestens 600 GB freiem Speicherplatz. Klicken Sie dann auf **Weiter**.
4. Klicken Sie nach Abschluss der Installation auf **Schließen** , um den Vorgang zu beenden.

    ![MARS-Agent registrieren](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent3.png)

### <a name="command-line-installation"></a>Installation über die Befehlszeile
Sie können den Microsoft Azure Recovery Services-Agent mit dem folgenden Befehl über die Befehlszeile installieren:

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Einrichten des Internetzugriffs auf Site Recovery per Proxy auf Hyper-V-Hosts

Für den Recovery Services-Agent, der auf Hyper-V-Hosts ausgeführt wird, ist für die VM-Replikation Internetzugriff auf Azure erforderlich. Gehen Sie bei der Einrichtung wie folgt vor, wenn Sie über einen Proxy auf das Internet zugreifen:

1. Öffnen Sie das Microsoft Azure Backup-MMC-Snap-In auf dem Hyper-V-Host. Standardmäßig ist auf dem Desktop oder unter „C:\Programme\Microsoft Azure Recovery Services Agent\bin\wabadmin“ eine Verknüpfung für Microsoft Azure Backup verfügbar.
2. Klicken Sie im Snap-In auf **Eigenschaften ändern**.
3. Geben Sie auf der Registerkarte **Proxykonfiguration** Informationen zum Proxyserver an.

    ![MARS-Agent registrieren](./media/vmm-to-azure-walkthrough-source-target/mars-proxy.png)
4. Vergewissern Sie sich, dass der Agent die unter [Voraussetzungen](#on-premises-prerequisites) beschriebenen URLs erreichen kann.

## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung
Geben Sie das Azure-Speicherkonto, das für die Replikation verwendet werden soll, und das Azure-Netzwerk an, mit dem Azure-VMs nach dem Failover eine Verbindung herstellen.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die virtuellen Computer erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die virtuellen Computer verwendet werden soll, für die ein Failover durchgeführt wurde (klassisch oder Resource Manager).

    ![Speicher](./media/vmm-to-azure-walkthrough-source-target/enablerep3.png)

2. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

    ![Speicher](./media/vmm-to-azure-walkthrough-source-target/compatible-storage.png)

3. Falls Sie noch kein Speicherkonto erstellt haben und dies unter Verwendung von Resource Manager nachholen möchten, können Sie auf **+Speicherkonto** klicken und diesen Schritt direkt ausführen.  Geben Sie auf dem Blatt **Speicherkonto erstellen** einen Kontonamen, einen Typ, ein Abonnement und einen Standort an. Das Konto sollte sich an demselben Standort wie der Recovery Services-Tresor befinden.

   ![Speicher](./media/vmm-to-azure-walkthrough-source-target/gs-createstorage.png)


   * Wenn Sie ein Speicherkonto mit dem klassischen Modell erstellen möchten, verwenden Sie das Azure-Portal. [Weitere Informationen](../storage/storage-create-storage-account-classic-portal.md)
   * Wenn Sie ein Storage Premium-Konto für replizierte Daten verwenden, müssen Sie ein weiteres Standardspeicherkonto zum Speichern von Replikationsprotokollen einrichten, mit denen laufende Änderungen lokaler Daten erfasst werden.
5. Falls Sie noch kein Azure-Netzwerk erstellt haben und dies unter Verwendung von Resource Manager nachholen möchten, können Sie auf **+Netzwerk** klicken und diesen Schritt direkt ausführen. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** einen Netzwerknamen, einen Adressbereich, Subnetzdetails, ein Abonnement und einen Standort an. Das Netzwerk sollte sich an demselben Standort wie der Recovery Services-Tresor befinden.

   ![Netzwerk](./media/vmm-to-azure-walkthrough-source-target/gs-createnetwork.png)

   Falls Sie ein Netzwerk mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das Azure-Portal. [Weitere Informationen](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).





## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zu [Schritt 9: Konfigurieren der Netzwerkzuordnung](vmm-to-azure-walkthrough-network-mapping.md).

