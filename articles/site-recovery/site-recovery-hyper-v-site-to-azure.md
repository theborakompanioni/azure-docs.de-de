---
title: Replizieren virtueller Hyper-V-Computer in Azure | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Azure Site Recovery bereitstellen, um Replikation, Failover und Wiederherstellung von lokalen Hyper-V-VMs, die nicht von VMM verwaltet werden, auf Azure mit dem Azure-Portal zu orchestrieren.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/23/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 75653b84d6ccbefe7d5230449bea81f498e10a98
ms.openlocfilehash: aac1d2016043cd64d94ec0d10921d6e208db1d7f

---

# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure-using-azure-site-recovery-with-the-azure-portal"></a>Replizieren von virtuellen Hyper-V-Computern (ohne VMM) nach Azure per Azure Site Recovery über das Azure-Portal
> [!div class="op_single_selector"]
> * [Azure-Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Azure – klassisch](site-recovery-hyper-v-site-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

Willkommen beim Azure Site Recovery-Dienst!

Der Azure Site Recovery-Dienst unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR). Site Recovery dient zum Orchestrieren der Replikation von lokalen physischen Servern und virtuellen Computern in die Cloud (Azure) oder in ein sekundäres Datencenter. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Weitere Informationen finden Sie unter [Was ist Site Recovery?](site-recovery-overview.md)

In diesem Artikel erfahren Sie, wie Sie lokale virtuelle Hyper-V-Computer mithilfe von Azure Site Recovery über das Azure-Portal zu Azure replizieren oder migrieren. In diesem Szenario werden Hyper-V-Server nicht in VMM-Clouds verwaltet. Stellen Sie die Replikation bereit, um ein Failover für VMs zu Azure auszuführen, wenn Ihr primärer Standort nicht verfügbar ist, und ein Failback von Azure zu lokalen Standorten durchzuführen, wenn der primäre Standort zum normalen Betrieb zurückkehrt. Führen Sie die Schritte in diesem Artikel aus, um VMs zu Azure zu migrieren (ohne Failback). Anschließend können Sie nach einem erfolgreichen Testfailover ein geplantes Failover zum Abschließen der Migration ausführen.


Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="quick-reference"></a>Kurzübersicht

Für eine vollständige Bereitstellung empfiehlt es sich dringend, alle Schritte in diesem Artikel auszuführen. Sollten Sie wenig Zeit haben, finden Sie hier eine kurze Zusammenfassung.

 **Bereich** | **Details**
 --- | ---
 **Bereitstellungsszenario** | Replizieren virtueller Hyper-V-Computer (nicht in VMM-Clouds) zu Azure über das Azure-Portal
 **Lokale Anforderungen** | Mindestens ein Hyper-V-Server, auf dem mindestens Windows Server 2012 R2 mit den neuesten Updates und der Hyper-V-Rolle aktiviert oder Microsoft Hyper-V Server 2012 R2 mit den neuesten Updates ausgeführt wird<br/><br/> Hyper-V-Hosts benötigen Internetzugriff und müssen entweder direkt oder über einen Proxy auf bestimmte URLs zugreifen können. [Details](#on-premises-prerequisites)
 **Lokale Einschränkungen** | HTTPS-basierte Proxys werden nicht unterstützt.
 **Anbieter/Agent** | Der Azure Site Recovery-Anbieter und der Recovery Services-Agent werden während der Bereitstellung auf Hyper-V-Hosts installiert.
 **Anforderungen für Azure** | Azure-Konto<br/><br/> Recovery Services-Tresor<br/><br/> LRS- oder GRS-Speicherkonto in der Tresorregion<br/><br/> Standardspeicherkonto<br/><br/> Virtuelles Azure-Netzwerk in der Tresorregion. [Details](#azure-prerequisites)
 **Azure-Einschränkungen** | Bei Verwendung von GRS benötigen Sie ein weiteres LRS-Konto für die Protokollierung.<br/><br/> Beachten Sie, dass im Azure-Portal erstellte Speicherkonten nicht über Ressourcengruppen in einem oder mehreren Abonnements hinweg verschoben werden können. <br/><br/> Storage Premium wird nicht unterstützt.<br/><br/> Für Site Recovery verwendete Azure-Netzwerke können nicht über Ressourcengruppen in einem oder mehreren Abonnements hinweg verschoben werden.
 **VM-Replikation** | VMs müssen die [Anforderungen für Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllen.<br/><br/>
 **Replikationseinschränkungen** | Hyper-V-VMs, die Linux mit einer statischen IP-Adresse ausführen, können nicht repliziert werden.<br/><br/> Sie können bestimmte Datenträger von der Replikation ausschließen, allerdings nicht den Betriebssystem-Datenträger.
 **Bereitstellungsschritte** | **1)** Erstellen eines Recovery Services-Tresors -> **2)** Erstellen eines Hyper-V-Standorts, der alle Hyper-V-Hosts umfasst -> **3)** Einrichten des Hyper-V-Hosts -> **4**) Vorbereiten von Azure (Abonnement, Speicher, Netzwerk) -> **5)** Konfigurieren der Replikationseinstellungen -> **6)** Aktivieren der Replikation -> **7)** Testen der Replikation und des Failover. **8)** Führen Sie ein geplantes Failover aus, wenn Sie eine Migration durchführen.

## <a name="azure-deployment-models"></a>Azure-Bereitstellungsmodelle

Azure verfügt über zwei unterschiedliche [Bereitstellungsmodelle](../azure-resource-manager/resource-manager-deployment-model.md) zum Erstellen und Verwenden von Ressourcen: das Azure Resource Manager-Modell und das klassische Modell. Außerdem verfügt Azure über zwei Portale: das [klassische Azure-Portal](https://manage.windowsazure.com/) mit Unterstützung des klassischen Bereitstellungsmodells und das [Azure-Portal](https://ms.portal.azure.com/) mit Unterstützung für beide Bereitstellungsmodelle.

 Dieser Artikel beschreibt die Bereitstellung im Azure-Portal, die eine optimierte Bereitstellungserfahrung ermöglicht. Das klassische Portal kann verwendet werden, um bestehende Tresore zu verwalten. Sie können keine neuen Tresore mithilfe des klassischen Azure-Portals erstellen.

## <a name="site-recovery-in-your-business"></a>Site Recovery in Ihrem Unternehmen

Organisationen benötigen eine BCDR-Strategie, die bestimmt, wie Apps und Daten bei geplanten und ungeplanten Ausfällen verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können. Site Recovery bietet folgende Vorteile:

 - Offsite-Schutz für Unternehmens-Apps, die auf virtuellen Hyper-V-Computern ausgeführt werden
 - Zentraler Ort zum Einrichten, Verwalten und Überwachen von Replikation, Failover und Wiederherstellung
 - Unkompliziertes Failover zu Azure und Failback (Wiederherstellung) von Azure zu Hyper-V-Hostservern an Ihrem lokalen Standort
 - Wiederherstellungspläne mit mehreren virtuellen Computern, sodass für mehrstufige Anwendungsworkloads ein gemeinsames Failover ausgeführt wird





## <a name="scenario-architecture"></a>Szenarioarchitektur
Dies sind die Szenariokomponenten:

* **Hyper-V-Host oder -Cluster**: lokale Hyper-V-Hostserver oder -Cluster. Die Hyper-V-Hosts, die virtuelle Computer ausführen, die Sie schützen möchten, werden während der Bereitstellung von Site Recovery in logischen Hyper-V-Standorten gesammelt.
* **Azure Site Recovery-Anbieter und Recovery Services-Agent**: Während der Bereitstellung installieren Sie den Azure Site Recovery-Anbieter und den Microsoft Azure Recovery Services-Agent auf Hyper-V-Hostservern. Der Anbieter kommuniziert mit Azure Site Recovery über HTTPS 443, um die Orchestrierung zu replizieren. Standardmäßig repliziert der Agent Daten auf dem Hyper-V-Hostserver über HTTPS 443 in den Azure-Speicher.
* **Azure**: Sie benötigen ein Azure-Abonnement, ein Azure-Speicherkonto zum Speichern replizierter Daten und ein virtuelles Azure-Netzwerk, damit Azure-VMs nach dem Failover über eine Netzwerkverbindung verfügen.

![Hyper-V-Standort-Architektur](./media/site-recovery-hyper-v-site-to-azure/architecture.png)

## <a name="azure-prerequisites"></a>Voraussetzungen für Azure


| **Voraussetzung** | **Details** |
| --- | --- |
| **Azure-Konto** | [Microsoft Azure](http://azure.microsoft.com/)-Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung. [hier](https://azure.microsoft.com/pricing/details/site-recovery/) . |
| **Azure-Speicher** | Standard-Speicherkonto. Sie können ein LRS- oder GRS-Speicherkonto verwenden. Wir empfehlen Ihnen die Verwendung von GRS, damit Resilienz für die Daten besteht, wenn es zu einem regionalen Ausfall kommt oder wenn die primäre Region nicht wiederhergestellt werden kann. [Weitere Informationen](../storage/storage-redundancy.md).<br/><br/> Das Konto muss sich in derselben Region wie der Recovery Services-Tresor befinden.<br/><br/> Storage Premium wird nicht unterstützt.<br/><br/> Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden erstellt, wenn ein Failover durchgeführt wird.<br/><br/> [Erfahren Sie mehr](../storage/storage-introduction.md) zum Azure-Speicher. |
| **Azure-Netzwerk** |Sie benötigen ein virtuelles Azure-Netzwerk, mit dem die virtuellen Azure-Computer eine Verbindung herstellen, wenn ein Failover stattfindet. Das virtuelle Azure-Netzwerk muss sich in derselben Region wie der Recovery Services-Tresor befinden. |

## <a name="on-premises-prerequisites"></a>Lokale Voraussetzungen
Lokal benötigen Sie Folgendes.

| **Voraussetzung** | **Details** |
| --- | --- |
| **Hyper-V** |Ein oder mehrere lokale Server, auf denen **Windows Server 2012 R2** mit den aktuellsten Updates und aktivierter Hyper-V-Rolle oder **Microsoft Hyper-V Server 2012 R2** ausgeführt wird.<br/><br/>Der Hyper-V-Server muss mindestens einen virtuellen Computer enthalten.<br/><br/>Hyper-V-Server müssen entweder direkt oder über einen Proxy mit dem Internet verbunden sein.<br/><br/>Auf Hyper-V-Servern sollten die in [KB2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977") erwähnten Fixes installiert sein. |
| **Anbieter und Agent** |Während der Bereitstellung von Azure Site Recovery installieren Sie den Azure Site Recovery-Anbieter. Die Anbieterinstallation installiert den Azure Recovery Services-Agent außerdem auf jedem Hyper-V-Server, der über zu schützende virtuelle Computer verfügt. Alle Hyper-V-Server in einem Site Recovery-Tresor sollten über die gleichen Anbieter- und Agent-Versionen verfügen.<br/><br/>Der Anbieter muss über das Internet eine Verbindung mit Azure Site Recovery herstellen. Datenverkehr kann direkt oder über einen Proxy gesendet werden. Beachten Sie, dass HTTPS-basierte Proxys nicht unterstützt werden. Der Proxyserver sollte den Zugriff auf Folgendes zulassen:  <br/><br/> ``*.accesscontrol.windows.net``<br/><br/> ``*.backup.windowsazure.com``<br/><br/> ``*.hypervrecoverymanager.windowsazure.com`` <br/><br/> ``*store.core.windows.net``<br/><br/> ``*.blob.core.windows.net``<br/><br/> ``https://www.msftncsi.com/ncsi.txt``<br/><br/> ``time.windows.com``<br/><br/> ``time.nist.gov``<br/><br/> Wenn Sie auf dem Server über Firewallregeln verfügen, die auf IP-Adressen basieren, sollten Sie sicherstellen, dass die Regeln die Kommunikation mit Azure zulassen.<br/><br/> Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.<br/><br/> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ zu. |

## <a name="virtual-machine-prerequisites"></a>Voraussetzungen für virtuelle Computer
| **Voraussetzung** | **Details** |
| --- | --- |
| **Geschützte VMs** |Bevor Sie für eine VM das Failover durchführen, müssen Sie sicherstellen, dass der Name, der dem virtuellen Azure-Computer zugewiesen wird, die [Voraussetzungen für Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)erfüllt. Sie können den Namen ändern, nachdem Sie die Replikation für die VM aktiviert haben.<br/><br/> Die individuelle Datenträgerkapazität auf geschützten Computern sollte nicht mehr als 1023 GB betragen. Ein virtueller Computer kann bis zu 64 Datenträger haben (also bis zu 64 TB).<br/><br/> Gastcluster mit freigegebenen Datenträgern werden nicht unterstützt.<br/><br/> Wenn auf dem virtuellen Quellcomputer NIC-Teamvorgänge ausgeführt wurden, wird er nach dem Failover zu Azure in eine einzelne NIC konvertiert.<br/><br/>Das Schützen von VMs, auf denen Linux mit einer statischen IP-Adresse ausgeführt wird, wird nicht unterstützt. |

## <a name="prepare-for-deployment"></a>Vorbereiten der Bereitstellung
Für die Vorbereitung der Bereitstellung benötigen Sie Folgendes:

1. [Richten Sie ein Azure-Netzwerk ein](#set-up-an-azure-network) , in dem sich Azure-VMs befinden, wenn sie nach dem Failover erstellt werden.
2. [Richten Sie ein Azure-Speicherkonto ein](#set-up-an-azure-storage-account) , das für replizierte Daten verwendet werden kann.
3. [Bereiten Sie die Hyper-V-Hosts vor](#prepare-the-hyper-v-hosts), um sicherzustellen, dass diese auf die erforderlichen URLs zugreifen können.

### <a name="set-up-an-azure-network"></a>Richten Sie ein Azure-Netzwerk ein

Richten Sie ein Azure-Netzwerk ein. Sie benötigen dies, damit die nach dem Failover erstellten Azure-VMs mit einem Netzwerk verbunden sind.

* Das Netzwerk sollte in derselben Region wie das Netzwerk angeordnet sein, in dem Sie den Recovery Services-Tresor bereitstellen.
* Richten Sie das Azure-Netzwerk im [Resource Manager-Modus](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) oder im [klassischen Modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ein (je nachdem, welches Ressourcenmodell Sie für virtuelle Azure-Computer nach dem Failover verwenden möchten).
* Wir empfehlen Ihnen, ein Netzwerk einzurichten, bevor Sie beginnen. Falls Sie es nicht tun, müssen Sie diesen Schritt während der Site Recovery-Bereitstellung ausführen.

> [!NOTE]
> Für Netzwerke, die für die Site Recovery-Bereitstellung verwendet werden, wird die [Migration von Netzwerken](../azure-resource-manager/resource-group-move-resources.md) zwischen Ressourcengruppen im selben Abonnement oder zwischen verschiedenen Abonnements nicht unterstützt.
>
>

### <a name="set-up-an-azure-storage-account"></a>Richten Sie ein Azure-Speicherkonto ein

- Für die Daten, die in Azure repliziert werden, benötigen Sie ein standardmäßiges Azure-Speicherkonto.
- Richten Sie ein Konto im [Resource Manager-Modus](../storage/storage-create-storage-account.md) oder im [klassischen Modus](../storage/storage-create-storage-account-classic-portal.md) ein (je nachdem, welches Ressourcenmodell Sie für virtuelle Azure-Computer nach dem Failover verwenden möchten).
- Es wird empfohlen, ein Speicherkonto einzurichten, bevor Sie beginnen. Falls Sie es nicht tun, müssen Sie diesen Schritt während der Site Recovery-Bereitstellung ausführen. Die Konten müssen sich in derselben Region wie der Recovery Services-Tresor befinden.
- Speicherkonten, die von Site Recovery verwendet werden, können nicht über Ressourcengruppen hinweg im gleichen Abonnement oder über verschiedene Abonnements hinweg verschoben werden.


### <a name="prepare-the-hyper-v-hosts"></a>Bereiten Sie die Hyper-V-Hosts vor
* Stellen Sie sicher, dass die Hyper-V-Hosts die [Voraussetzungen](#on-premises-prerequisites)erfüllen.

### <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Neu** > **Verwaltung** > **Backup und Site Recovery (OMS)**. Alternativ können Sie auf **Durchsuchen** > **Recovery Services-Tresore > ****Hinzufügen** klicken.

    ![Neuer Tresor](./media/site-recovery-hyper-v-site-to-azure/new-vault3.png)
3. Geben Sie unter **Name** einen Anzeigenamen an, mit dem der Tresor identifiziert wird. Wenn Sie mehrere Abonnements haben, müssen Sie ein Abonnement auswählen.
4. [Erstellen Sie eine neue Ressourcengruppe](../azure-resource-manager/resource-group-template-deploy-portal.md) , oder wählen Sie eine vorhandene Ressourcengruppe aus, und geben Sie eine Azure-Region an. Computer werden in dieser Region repliziert. Sie finden eine Liste der unterstützten Regionen unter Geografische Verfügbarkeit auf der Seite [Azure Site Recovery – Preisübersicht](https://azure.microsoft.com/pricing/details/site-recovery/)
5. Wenn Sie schnell über das Dashboard auf den Tresor zugreifen möchten, klicken Sie auf **An Dashboard anheften** und anschließend auf **Tresor erstellen**.

    ![Neuer Tresor](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

Der neue Tresor wird unter **Dashboard** > **Alle Ressourcen** und auf dem Hauptblatt **Recovery Services-Tresore** angezeigt.

## <a name="get-started"></a>Erste Schritte
Site Recovery verfügt über eine Benutzeroberfläche für die ersten Schritte, damit Sie die Bereitstellung so schnell wie möglich durchführen können. Im Rahmen der ersten Schritte werden die Voraussetzungen überprüft, und Sie werden durch die Schritte der Site Recovery-Bereitstellung in der richtigen Reihenfolge geführt.

Sie wählen den Typ der Computer aus, die Sie replizieren möchten, und geben an, wo sie repliziert werden sollen. Sie richten lokale Server, Azure-Speicherkonten und Netzwerke ein. Sie erstellen Replikationsrichtlinien und führen die Kapazitätsplanung durch. Nachdem Sie Ihre Infrastruktur eingerichtet haben, aktivieren Sie die Replikation für VMs. Sie können Failover für bestimmte Computer ausführen oder Wiederherstellungspläne erstellen, um das Failover für mehrere Computer durchzuführen.

Beginnen Sie mit den ersten Schritten, indem Sie auswählen, wie Sie Site Recovery bereitstellen möchten. Der Ablauf der ersten Schritte kann je nach Replikationsanforderungen leicht variieren.

## <a name="step-1-choose-your-protection-goals"></a>Schritt 1: Auswählen der Schutzziele
Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.

1. Wählen Sie auf dem Blatt **Recovery Services-Tresore** Ihren Tresor aus, und klicken Sie auf **Einstellungen**.
2. Klicken Sie unter **Einstellungen** > **Erste Schritte** auf **Site Recovery** > **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Schutzziel**.

    ![Ziele wählen](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)
3. Wählen Sie unter **Schutzziel** die Option **To Azure** (Zu Azure) und dann **Yes, with Hyper-V** (Ja, mit Hyper-V) aus. Wählen Sie **Nein** aus, um zu bestätigen, dass Sie VMM nicht verwenden. Klicken Sie dann auf **OK**.

    ![Ziele wählen](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)

## <a name="step-2-set-up-the-source-environment"></a>Schritt 2: Einrichten der Quellumgebung
Richten Sie den Hyper-V-Standort ein, installieren Sie den Azure Site Recovery-Anbieter und den Azure Recovery Services-Agent auf Hyper-V-Hosts, und registrieren Sie die Hosts im Tresor.

1. Klicken Sie auf **Schritt 2: Bereiten Sie die Infrastruktur vor** > **Quelle**. Klicken Sie auf **+Hyper-V-Standort**, um einen neuen Hyper-V-Standort als Container für Ihre Hyper-V-Hosts oder -Cluster hinzuzufügen.

    ![Quelle einrichten](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)
2. Geben Sie auf dem Blatt **Hyper-V-Standort erstellen** einen Namen für den Standort an. Klicken Sie dann auf **OK**. Wählen Sie den Standort aus, den Sie gerade erstellt haben.

    ![Quelle einrichten](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)
3. Klicken Sie auf **+Hyper-V Server**, um dem Standort einen Server hinzuzufügen.
4. Vergewissern Sie sich, dass unter **Server hinzufügen** > **Servertyp** der Typ **Hyper-V Server** angezeigt wird. Stellen Sie sicher, dass der Hyper-V-Server, den Sie hinzufügen möchten, die [Voraussetzungen](#on-premises-prerequisites) erfüllt und auf die angegebenen URLs zugreifen kann.
5. Laden Sie die Installationsdatei für den Azure Site Recovery-Anbieter herunter. Führen Sie diese Datei aus, um sowohl den Anbieter als auch den Recovery Services-Agent auf jedem Hyper-V-Host zu installieren.
6. Laden Sie den Registrierungsschlüssel herunter. Sie benötigen diese Angaben beim Ausführen des Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

    ![Quelle einrichten](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)
7. Führen Sie die Setupdatei für den Anbieter auf jedem Host aus, den Sie zum Hyper-V-Standort hinzugefügt haben. Wenn Sie auf einem Hyper-V-Cluster installieren, führen Sie das Setup auf jedem Clusterknoten aus. Durch das Installieren und Registrieren jedes Hyper-V-Clusterknotens wird sichergestellt, dass virtuelle Computer geschützt bleiben, selbst wenn sie über Knoten hinweg migrieren.

### <a name="install-the-provider-and-agent"></a>Installieren Sie den Anbieter und den Agent.
1. Führen Sie die Setupdatei für den Anbieter aus.
2. Unter **Microsoft Update** können Sie Updates aktivieren, damit Updates für den Anbieter gemäß Ihrer Microsoft Update-Richtlinie installiert werden.
3. Ändern Sie unter **Installation** den Speicherort für die Anbieterinstallation, oder akzeptieren Sie den Standardspeicherort, und klicken Sie auf **Installieren**.
4. Klicken Sie auf der Seite **Tresoreinstellungen** auf **Durchsuchen**, um die Tresorschlüsseldatei auszuwählen, die Sie heruntergeladen haben. Geben Sie das Azure Site Recovery-Abonnement, den Tresornamen und den Hyper-V-Standort an, zu dem der Hyper-V-Server gehört.

    ![Serverregistrierung](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5. Geben Sie unter **Proxyeinstellungen** an, wie der Anbieter, der auf dem Server installiert wird, eine Internetverbindung mit Azure Site Recovery herstellt.

* Wenn der Anbieter eine direkte Verbindung herstellt, wählen Sie **Ohne Proxy direkt verbinden**aus.
* Wenn die Verbindung über einen derzeit auf dem Server eingerichteten Proxy hergestellt werden soll, wählen Sie **Mit vorhandenen Proxyeinstellungen verbinden**aus.
* Wenn für den vorhandenen Proxy eine Authentifizierung erforderlich ist oder Sie für die Anbieterverbindung einen benutzerdefinierten Proxy verwenden möchten, wählen Sie **Mit benutzerdefinierten Proxyeinstellungen verbinden**aus.
* Bei einem benutzerdefinierten Proxy müssen Sie Adresse, Port und Anmeldeinformationen eingeben.
* Stellen Sie sicher, dass die in den [Voraussetzungen](#on-premises-prerequisites) beschriebenen URLs vom Proxy zugelassen werden, falls Sie einen verwenden.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. Klicken Sie nach Abschluss der Installation auf **Registrieren**, um den Server im Tresor zu registrieren.

![Installationsspeicherort](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7. Nach der Registrierung ruft Azure Site Recovery Metadaten vom Hyper-V-Server ab, und der Server wird auf dem Blatt **Einstellungen** > **Site Recovery-Infrastruktur** > **Hyper-V-Hosts** angezeigt.

### <a name="command-line-installation"></a>Installation über die Befehlszeile
Der Azure Site Recovery-Anbieter und der Azure Site Recovery-Agent können auch mithilfe der folgenden Befehlszeile installiert werden. Mit dieser Methode kann der Anbieter in Server Core für Windows Server 2012 R2 installiert werden.

1. Laden Sie die Installationsdatei und den Registrierungsschlüssel des Anbieters in einen Ordner herunter. Beispiel: C:\ASR.
2. Extrahieren Sie über eine Eingabeaufforderung mit erhöhten Rechten das Installationsprogramm des Anbieters, indem Sie diese Befehle ausführen:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Führen Sie diesen Befehl aus, um die Komponenten zu installieren:

            C:\ASR> setupdr.exe /i
4. Führen Sie anschließend diese Befehle aus, um den Server im Tresor zu registrieren:

            CD C:\Program Files\Microsoft Azure Site Recovery Provider\
            C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file>

<br/>
Hierbei gilt:

* **/Credentials** : erforderlicher Parameter zum Angeben des Speicherorts der Registrierungsschlüsseldatei.  
* **/FriendlyName** : erforderlicher Parameter für den Namen des Hyper-V-Hostservers, der im Azure Site Recovery-Portal angezeigt wird.
* **/proxyAddress** : optionaler Parameter, der die Adresse des Proxyservers angibt.
* **/proxyport** : optionaler Parameter, der den Port des Proxyservers angibt.
* **/proxyUsername** : optionaler Parameter, der den Proxybenutzernamen angibt (sofern der Proxy eine Authentifizierung erfordert).
* **/proxyPassword** : optionaler Parameter, der das Kennwort für die Authentifizierung mit dem Proxyserver angibt (sofern der Proxy eine Authentifizierung erfordert).

## <a name="step-3-set-up-the-target-environment"></a>Schritt 3: Einrichten der Zielumgebung

Geben Sie das Azure-Speicherkonto, das für die Replikation verwendet werden soll, und das Azure-Netzwerk an, mit dem Azure-VMs nach dem Failover eine Verbindung herstellen.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel** > und wählen Sie das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die virtuellen Computer erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die virtuellen Computer verwendet werden soll, für die ein Failover durchgeführt wurde (klassisch oder Resource Manager).

3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

    ![Speicher](./media/site-recovery-vmware-to-azure/enable-rep3.png))


4. Falls Sie noch kein Speicherkonto erstellt haben und dies unter Verwendung von Resource Manager nachholen möchten, können Sie auf **+Speicherkonto** klicken und diesen Schritt direkt ausführen. Geben Sie auf dem Blatt **Speicherkonto erstellen** einen Kontonamen, einen Typ, ein Abonnement und einen Standort an. Das Konto sollte sich an demselben Standort wie der Recovery Services-Tresor befinden.

    ![Speicher](./media/site-recovery-hyper-v-site-to-azure/gs-createstorage.png)


Wenn Sie ein Speicherkonto mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das [Azure-Portal](../storage/storage-create-storage-account-classic-portal.md).


Falls Sie noch kein Azure-Netzwerk erstellt haben und dies unter Verwendung von Resource Manager nachholen möchten, können Sie auf **+Netzwerk** klicken und diesen Schritt direkt ausführen. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** einen Netzwerknamen, einen Adressbereich, Subnetzdetails, ein Abonnement und einen Standort an. Das Netzwerk sollte sich an demselben Standort wie der Recovery Services-Tresor befinden.

   ![Netzwerk](./media/site-recovery-hyper-v-site-to-azure/gs-createnetwork.png)

Wenn Sie ein Netzwerk mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das [Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

## <a name="step-4-set-up-replication-settings"></a>Schritt 4: Einrichten der Replikationseinstellungen
1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Erstellen und zuordnen**.

    ![Netzwerk](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)
2. Geben Sie unter **Richtlinie erstellen und zuordnen** einen Richtliniennamen an.
3. Geben Sie unter **Kopierhäufigkeit** an, wie oft Sie Deltadaten nach der ersten Replikation replizieren möchten (alle 30 Sekunden, nach 5 Minuten oder nach 15 Minuten).
4. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts**das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte in Stunden an. Geschützte Computer können innerhalb eines Zeitfensters an einem beliebigen Punkt wiederhergestellt werden.
5. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit** an, wie häufig (1 bis&12; Stunden) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten. Hyper-V verwendet zwei Momentaufnahmen: eine Standard-Momentaufnahme, die eine inkrementelle Momentaufnahme des gesamten virtuellen Computers bereitstellt, und eine anwendungskonsistente Momentaufnahme, die eine Zeitpunkt-Momentaufnahme der Anwendungsdaten innerhalb des virtuellen Computers erfasst. Anwendungskonsistente Momentaufnahmen verwenden den Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), um sicherzustellen, dass Anwendungen sich bei der Erstellung der Momentaufnahme in einem konsistenten Zustand befinden. Beachten Sie, dass die Leistung von Anwendungen auf virtuellen Quellcomputern durch die Aktivierung anwendungskonsistenter Momentaufnahmen beeinträchtigt wird. Stellen Sie sicher, dass der festgelegte Wert kleiner als die konfigurierte Anzahl der zusätzlichen Wiederherstellungspunkte ist.
6. Geben Sie unter **Startzeit der ersten Replikation** an, wann die erste Replikation starten soll. Da die Replikation über Ihre Internetbandbreite durchgeführt wird, ist es ratsam, sie außerhalb der Zeiten mit der höchsten Arbeitsbelastung einzuplanen. Klicken Sie dann auf **OK**.

    ![Replikationsrichtlinie](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Wenn Sie eine neue Richtlinie erstellen, wird sie dem Hyper-V-Standort automatisch zugeordnet. Klicken Sie auf **OK**. Sie können den Hyper-V-Standort (und die darin enthaltenen VMs) über **Einstellungen** > **Replikation** > Richtlinienname > **Hyper-V-Standort zuordnen** mehreren Replikationsrichtlinien zuordnen.

## <a name="step-5-capacity-planning"></a>Schritt 5: Kapazitätsplanung
Nachdem Sie nun Ihre grundlegende Infrastruktur eingerichtet haben, können Sie sich mit der Kapazitätsplanung beschäftigen und ermitteln, ob Sie zusätzliche Ressourcen benötigen.

Site Recovery verfügt über einen Capacity Planner, der Sie dabei unterstützt, die richtigen Ressourcen für Ihre Quellumgebung, die Site Recovery-Komponenten, das Netzwerk und den Speicher zuzuordnen. Sie können den Planer im Schnellmodus für Schätzungen ausführen, die auf einer durchschnittlichen Anzahl von VMs, Datenträgern und Speicher basieren, oder im ausführlichen Modus, in dem Sie Zahlen auf Workloadebene eingeben. Führen Sie Folgendes aus, bevor Sie beginnen:

* Sammeln Sie Informationen zu Ihrer Replikationsumgebung, z.B. VMs, Datenträger pro VM und Speicher pro Datenträger.
* Schätzen Sie die tägliche Änderungsrate für replizierte Daten. Zur Unterstützung können Sie den [Capacity Planner für Hyper-V-Replikat](https://www.microsoft.com/download/details.aspx?id=39057) verwenden.

1. Klicken Sie auf **Herunterladen**, um das Tool herunterzuladen, und führen Sie es anschließend aus. [entsprechenden Artikel](site-recovery-capacity-planner.md) durch.
2. Wählen Sie anschließend unter **Ja** in **Ja**aus.

   ![Kapazitätsplanung](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Aspekte der Netzwerkbandbreite
Sie können den Capacity Planner verwenden, um die Bandbreite zu berechnen, die Sie für die Replikation benötigen (erste Replikation und dann Deltareplikation). Sie haben verschiedene Möglichkeiten, um die verwendete Bandbreite für die Replikation zu steuern:

* **Bandbreite drosseln:** Hyper-V-Datenverkehr, der nach Azure repliziert wird, wird über einen speziellen Hyper-V-Host geleitet. Sie können die Bandbreite auf dem Hostserver drosseln.
* **Bandbreite optimieren**: Sie können beeinflussen, wie viel Bandbreite für die Replikation verwendet wird, indem Sie einige Registrierungsschlüssel nutzen.

#### <a name="throttle-bandwidth"></a>Bandbreite einschränken
1. Öffnen Sie das Microsoft Azure Backup-MMC-Snap-In auf dem Hyper-V-Hostserver. Standardmäßig ist auf dem Desktop oder unter „C:\Programme\Microsoft Azure Recovery Services Agent\bin\wabadmin“ eine Verknüpfung für Microsoft Azure Backup verfügbar.
2. Klicken Sie im Snap-In auf **Eigenschaften ändern**.
3. Wählen Sie auf der Registerkarte **Drosselung** die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren** aus, und legen Sie die Grenzwerte für Geschäftszeiten und arbeitsfreie Zeiten fest. Der gültige Bereich reicht von 512 KBit/s bis 102 MBit/s.

    ![Bandbreite drosseln](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

Sie können auch das Cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) verwenden, um die Drosselung festzulegen. Hier ein Beispiel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** gibt an, dass keine Drosselung erforderlich ist.

#### <a name="influence-network-bandwidth"></a>Netzwerkbandbreite beeinflussen
1. Navigieren Sie in der Registrierung zu **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Um die Bandbreite für den Datenverkehr auf einem replizierenden Datenträger zu beeinflussen, ändern Sie den Wert für **UploadThreadsPerVM**, oder erstellen Sie den Schlüssel, falls dieser nicht vorhanden ist.
   * Um die Bandbreite für den Failbackdatenverkehr von Azure zu beeinflussen, ändern Sie den Wert für **DownloadThreadsPerVM**.
2. Der Standardwert ist 4. In einem absichtlich mit großen Reserven ausgestatteten Netzwerk müssen die Standardwerte dieser Registrierungsschlüssel geändert werden. Der maximale Wert beträgt 32. Überwachen Sie den Datenverkehr, um den Wert zu optimieren.

## <a name="step-6-enable-replication"></a>Schritt 6: Aktivieren der Replikation
Aktivieren Sie die Replikation jetzt wie folgt:

1. Klicken Sie auf **Schritt 2: Replizieren Sie die Anwendung** > **Quelle**. Klicken Sie nach der erstmaligen Aktivierung der Replikation im Tresor auf **+Replizieren**, um die Replikation für weitere Computer zu aktivieren.

    ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)
2. Wählen Sie auf dem Blatt **Quelle** den Hyper-V-Standort aus. Klicken Sie dann auf **OK**.
3. Wählen Sie unter **Ziel** das Tresorabonnement und das Failovermodell aus, das Sie in Azure (klassisches oder Resource Manager-Modell) nach einem Failover verwenden möchten.
4. Wählen Sie das Speicherkonto aus, das Sie verwenden möchten. Wenn Sie ein anderes Speicherkonto als Ihre bereits vorhandenen Speicherkonten verwenden möchten, können Sie ein [Speicherkonto erstellen](#set-up-an-azure-storage-account). Klicken Sie zum Erstellen eines Speicherkontos mit dem Resource Manager-Modell auf **Neu erstellen**. Wenn Sie ein Speicherkonto mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das [Azure-Portal](../storage/storage-create-storage-account-classic-portal.md). Klicken Sie dann auf **OK**.
5. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem Azure-VMs eine Verbindung herstellen, wenn sie nach einem Failover erstellt werden. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen. Wenn Sie ein anderes Netzwerk als Ihre bereits vorhandenen Netzwerke verwenden möchten, können Sie [ein Netzwerk erstellen](#set-up-an-azure-network). Klicken Sie zum Erstellen eines Netzwerks mit dem Resource Manager-Modell auf **Neu erstellen**. Wenn Sie ein Netzwerk mit dem klassischen Modell erstellen möchten, verwenden Sie das [Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Wählen Sie, falls zutreffend, ein Subnetz aus. Klicken Sie dann auf **OK**.
   ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. Geben Sie unter **Virtuelle Computer** > **Virtuelle Computer auswählen** , und wählen Sie die Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication5-for-exclude-disk.png)
7. Geben Sie unter **Eigenschaften** > **Eigenschaften konfigurieren**das Betriebssystem für die ausgewählten VMs und den Betriebssystem-Datenträger aus. Standardmäßig werden alle Datenträger des virtuellen Computers für die Replikation ausgewählt. Sie können jedoch Datenträger von der Replikation ausschließen, um die Replizierung unnötiger Daten in Azure zu vermeiden und so die beanspruchte Bandbreite zu verringern. Beispielsweise möchten Sie vielleicht nicht, dass Datenträger mit temporären Daten oder Daten, die bei jedem Neustart des Computers oder der Anwendung aktualisiert werden (etwa „pagefile.sys“ oder Microsoft SQL Server tempdb), repliziert werden. Durch Aufheben der Auswahl eines Datenträgers können Sie ihn von der Replikation ausschließen. Stellen Sie sicher, dass der Name des virtuellen Azure-Computers (Zielname) den [Anforderungen für virtuelle Azure-Computer](site-recovery-best-practices.md#azure-virtual-machine-requirements) entspricht, und ändern Sie diesen bei Bedarf. Klicken Sie dann auf **OK**. Sie können später weitere Eigenschaften festlegen.

    ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication6-with-exclude-disk.png)

     > [!NOTE]
     >
     > * Nur Basisdatenträger können von der Replikation ausgeschlossen werden. Der Betriebssystem-Datenträger kann nicht ausgeschlossen werden, und es wird davon abgeraten, dynamische Datenträger auszuschließen. ASR kann nicht ermitteln, ob es sich bei einer VHD des virtuellen Gastcomputers um einen Basisdatenträger oder um einen dynamischen Datenträger handelt.  Wenn keiner der abhängigen dynamischen Volumendatenträger ausgeschlossen wird, tritt bei geschützten dynamischen Datenträgern auf einem virtuellen Failovercomputer ein Fehler auf, und auf die Daten des Datenträgers kann nicht zugegriffen werden.
    > * Nach Aktivierung der Replikation können keine Datenträger für die Replikation hinzugefügt oder entfernt werden. Wenn Sie einen Datenträger hinzufügen oder entfernen möchten, müssen Sie den Schutz für den virtuellen Computer deaktivieren und anschließend wieder aktivieren.
    > * Wenn Sie einen Datenträger ausschließen, der für den Betrieb einer Anwendung erforderlich ist, müssen Sie ihn nach dem Failover auf Azure manuell in Azure erstellen, damit die replizierte Anwendung ausgeführt werden kann. Sie können auch Azure
    > * Automation in einen Wiederherstellungsplan integrieren, um den Datenträger während des Failovers des Computers zu erstellen.
    > * Für Datenträger, die Sie manuell in Azure erstellen, wird kein Failback ausgeführt. Wenn Sie also beispielsweise ein Failover für drei Datenträger ausführen und zwei Datenträger direkt auf dem virtuellen Azure-Computer erstellen, wird nur für die drei Datenträger, für die das Failover ausgeführt wurde, ein Failback von Azure zu Hyper-V ausgeführt. Manuell erstellte Datenträger können nicht in das Failback oder in die umgekehrte Replikation von Hyper-V zu Azure einbezogen werden.
    >
    >       

8. Wählen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren** die Replikationsrichtlinie aus, die Sie für die geschützten VMs anwenden möchten. Klicken Sie dann auf **OK**. Sie können die Replikationsrichtlinie unter **Einstellungen** > **Replikationsrichtlinien** > Richtlinienname > **Einstellungen bearbeiten** ändern. Von Ihnen vorgenommene Änderungen werden für Computer, die bereits repliziert werden, und für neue Computer verwendet.

   ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.

### <a name="view-and-manage-vm-properties"></a>Anzeigen und Verwalten von VM-Eigenschaften
Es wird empfohlen, dass Sie die Eigenschaften des Quellcomputers überprüfen.

1. Klicken Sie auf **Einstellungen** > **Geschützte Elemente** > **Replizierte Elemente**, und wählen Sie den Computer aus.

    ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)
2. Unter **Eigenschaften** können Sie die Informationen zur Replikation und zum Failover für den virtuellen Computer anzeigen.

    ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)
3. Unter **Compute und Netzwerk** > **Compute-Eigenschaften** können Sie den Namen und die Zielgröße des virtuellen Azure-Computers angeben. Ändern Sie ggf. den Namen, damit er die Azure-Anforderungen erfüllt. Sie können auch Informationen zum Zielnetzwerk, zum Subnetz sowie zur IP-Adresse anzeigen und ändern, die der Azure-VM zugewiesen wird. Beachten Sie Folgendes:

   * Sie können die Ziel-IP-Adresse festlegen. Wenn Sie keine Adresse angeben, wird für den Computer, für den das Failover durchgeführt wurde, DHCP verwendet. Wenn Sie eine Adresse festlegen, die beim Failover nicht verfügbar ist, tritt beim Failover ein Fehler auf. Dieselbe Ziel-IP-Adresse kann für das Testfailover verwendet werden, wenn die Adresse im Testfailover-Netzwerk verfügbar ist.
   * Die Anzahl der Netzwerkkarten hängt von der Größe ab, die Sie für den virtuellen Zielcomputer angeben. Hierbei gilt Folgendes:

     * Wenn die Anzahl der Netzwerkkarten des Quellcomputers maximal der Anzahl der Netzwerkkarten entspricht, die für die Größe des Zielcomputers zulässig ist, hat der Zielcomputer die gleiche Anzahl von Netzwerkkarten wie der Quellcomputer.
     * Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputer die maximal zulässige Anzahl für die Größe des Zielcomputers übersteigt, wird die Anzahl verwendet, die maximal für die Größe des Zielcomputers zulässig ist.
     * Ein Beispiel: Wenn ein Quellcomputer zwei Netzwerkkarten besitzt und der Zielcomputer aufgrund seiner Größe vier Netzwerkkarten unterstützt, erhält der Zielcomputer zwei Netzwerkkarten. Wenn der Quellcomputer dagegen zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe nur einen Adapter unterstützt, erhält der Zielcomputer nur einen Adapter.     
     * Wenn die VM über mehrere Netzwerkkarten verfügt, werden alle mit dem gleichen Netzwerk verbunden.
     * Wenn der virtuelle Computer über mehrere Netzwerkadapter verfügt, wird der erste, der in der Liste angezeigt wird, zum *Standard*-Netzwerkadapter im virtuellen Azure-Computer.

     ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

4. Unter **Datenträger** werden das Betriebssystem und die Datenträger auf der VM angezeigt, die repliziert wird.



### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover
Gehen Sie wie folgt vor, wenn Sie die Verbindung mit Azure-VMs nach dem Failover per RDP herstellen möchten: 

**Auf dem lokalen Computer vor dem Failover**:

* Aktivieren Sie für den Zugriff über das Internet RDP. Stellen Sie sicher, dass TCP- und UDP-Regeln für **Öffentlich** hinzugefügt werden, und stellen Sie sicher, dass RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für alle Profile zugelassen ist.
* Aktivieren Sie für den Zugriff über eine Standort-zu-Standort-Verbindung RDP auf dem Computer, und stellen Sie sicher, dass RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für die Netzwerke der Typen **Domäne** und **Privat** zugelassen ist.
* Installieren Sie den [Azure-VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) auf dem lokalen Computer.
* Achten Sie darauf, dass die SAN-Richtlinie des Betriebssystems auf OnlineAll festgelegt ist. [Weitere Informationen](https://support.microsoft.com/kb/3031135)
* Deaktivieren Sie den IPSec-Dienst, bevor Sie das Failover durchführen.

**Auf der Azure-VM nach einem Failover**:

* Fügen Sie der Netzwerkschnittstellenkarte, die dem virtuellen Azure-Computer zugeordnet ist, eine öffentliche IP-Adresse hinzu, um RDP zuzulassen.
* Stellen Sie sicher, dass keine Domänenrichtlinien vorhanden sind, die das Verbinden mit einem virtuellen Computer über eine öffentliche Adresse verhindern.
* Versuchen Sie, eine Verbindung herzustellen. Wenn Sie keine Verbindung herstellen können, sollten Sie überprüfen, ob der virtuelle Computer ausgeführt wird. Weitere Tipps zur Problembehandlung finden Sie in [diesem Artikel](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Gehen Sie wie folgt vor, wenn Sie nach dem Failover auf eine Azure-VM mit Linux über einen Secure Shell-Client (ssh) zugreifen möchten:

**Auf dem lokalen Computer vor dem Failover**:

* Stellen Sie sicher, dass der Secure Shell-Dienst auf der Azure-VM so festgelegt ist, dass er beim Systemstart automatisch gestartet wird.
* Überprüfen Sie, ob die Firewallregeln eine SSH-Verbindung damit zulassen.

**Auf der Azure-VM nach einem Failover**:

* In den Netzwerksicherheitsgruppen-Regeln auf der VM nach dem Failover und für das Azure-Subnetz, mit dem die VM verbunden ist, müssen eingehende Verbindungen zum SSH-Port zulässig sein.
* Es sollte ein öffentlicher Endpunkt erstellt werden, um eingehende Verbindungen für den SSH-Port (standardmäßig TCP-Port 22) zuzulassen.
* Wenn auf die VM über eine VPN-Verbindung (Express Route- oder Standort-zu-Standort-VPN-Verbindung) zugegriffen wird, kann der Client verwendet werden, um per SSH eine direkte Verbindung mit der VM herzustellen.

## <a name="step-7-run-a-test-failover"></a>Schritt 7: Ausführen eines Testfailovers
Zum Testen der Bereitstellung können Sie ein Testfailover für einen einzelnen virtuellen Computer durchführen. Alternativ können Sie einen Wiederherstellungsplan erstellen, der mehrere virtuelle Computer enthält.

1. Klicken Sie zum Durchführen eines Failovers für einen einzelnen Computer unter **Einstellungen** > **Replizierte Elemente** auf den virtuellen Computer und dann auf das Symbol **+Testfailover**.

    ![Testfailover](./media/site-recovery-vmware-to-azure/test-failover1.png)
1. Klicken Sie zum Durchführen eines Failovers für einen Wiederherstellungsplan unter **Einstellungen** > **Wiederherstellungspläne** mit der rechten Maustaste auf den Plan, und klicken Sie dann auf **Testfailover**. Eine Anleitung zum Erstellen eines Wiederherstellungsplans finden Sie [hier](site-recovery-create-recovery-plans.md).
1. Wählen Sie unter **Testfailover** das Azure-Netzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden.
1. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt verfolgen, indem Sie auf den virtuellen Computer klicken, um die Eigenschaften zu öffnen. Alternativ können Sie unter „Tresorname“ > **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** auf den Auftrag **Testfailover** klicken.
1. Nach Abschluss des Failovers sollte der Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt werden. Stellen Sie sicher, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.
1. Wenn Sie die [Vorbereitung für Verbindungen nach dem Failover](#prepare-to-connect-to-azure-vms-after-failover) durchgeführt haben, sollten Sie eine Verbindung mit dem virtuellen Azure-Computer herstellen können.
1. Klicken Sie anschließend auf dem Wiederherstellungsplan auf **Cleanup-Test-Failover** (Testfailover bereinigen). Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Test-Failover. Dadurch werden die während des Testfailovers erstellten virtuellen Computer gelöscht. 

Weitere Informationen finden Sie im Dokument [Testfailover in Azure](site-recovery-test-failover-to-azure.md).


## <a name="failover"></a>Failover
Nach Abschluss der ersten Replikation für Ihre Computer könne Sie bei Bedarf Failover auslösen. Site Recovery unterstützt verschiedene Arten von Failovern: Testfailover, geplantes Failover und ungeplantes Failover.
Lesen Sie die [weiteren Informationen](site-recovery-failover.md) zu den unterschiedlichen Typen von Failovern und die ausführlichen Beschreibungen dazu, wann und wie sie durchgeführt werden sollten.

> [!NOTE]
> Falls Sie virtuelle Computer zu Azure migrieren möchten, empfehlen wir Ihnen dringend, ein [geplantes Failover](site-recovery-failover.md#run-a-planned-failover-primary-to-secondary) durchzuführen, um die virtuellen Computer zu Azure zu migrieren. Nachdem die migrierte Anwendung in Azure per Testfailover überprüft wurde, können Sie die Schritte unter [Vollständige Migration](#Complete-migration-of-your-virtual-machines-to-Azure) ausführen, um die Migration Ihrer virtuellen Computer durchzuführen. Sie müssen keinen Commit- oder Löschvorgang durchführen. Mit „Migration abschließen“ wird die Migration abgeschlossen, der Schutz für den virtuellen Computer beendet und die Azure Site Recovery-Gebührenberechnung für den Computer beendet.


### <a name="run-a-planned-failover"></a>Ausführen eines geplanten Failovers
Diese Option sollte gewählt werden, um Compliance-Anforderungen zu erfüllen oder um während einer geplanten Wartung ein Failover von Daten durchzuführen, damit Workloads vor bekannten Ausfällen wie erwarteten Stromausfällen oder Unwettern geschützt sind. Hier erfahren Sie, wie Sie ein „geplantes Failover“ für einen Wiederherstellungsplan durchführen. Alternativ können Sie das Failover über die Registerkarte „Virtuelle Computer“ auch für einen einzelnen virtuellen Computer durchführen. Vergewissern Sie sich zunächst, dass bei allen virtuellen Computern, für die ein Failover durchgeführt werden soll, die erste Replikation abgeschlossen ist.

1. Wählen Sie **Wiederherstellungspläne > <Name des Wiederherstellungsplans>**.
2. Klicken Sie auf dem Blatt „Wiederherstellungsplan“ auf **Geplantes Failover**.
3. Wählen Sie auf der Seite **Geplantes Failover bestätigen **den Quell- und Zielort aus. 
4. Zu Beginn eines geplantes Failovers wird zur Vermeidung von Datenverlusten zunächst der virtuelle Computer heruntergefahren. Der Fortschritt des Failovers wird auf der Registerkarte **Aufträge** angezeigt. Tritt während des Failovers ein Fehler auf (entweder auf einem virtuellen Computer oder in einem Skript aus dem Wiederherstellungsplan), wird das geplante Failover des Wiederherstellungsplans beendet. Sie können das Failover erneut initiieren.
6. Nach der Erstellung der virtuellen Replikatcomputer weist deren Status darauf hin, dass ein Commit aussteht. Klicken Sie auf **Commit**, um ein Commit für das Failover auszuführen.
7. Nach Abschluss der Replikation werden die virtuellen Computer am sekundären Standort gestartet.


### <a name="run-an-unplanned-failover"></a>Durchführen eines ungeplanten Failovers
Wählen Sie diese Option, falls aufgrund eines unerwarteten Vorfalls (beispielsweise ein Stromausfall oder ein Virusangriff) nicht mehr auf einen primären Standort zugegriffen werden kann. Hier erfahren Sie, wie Sie ein ungeplantes Failover für einen Wiederherstellungsplan durchführen. Alternativ können Sie das Failover über die Registerkarte „Virtuelle Computer“ auch für einen einzelnen virtuellen Computer durchführen. Vergewissern Sie sich zunächst, dass bei allen virtuellen Computern, für die ein Failover durchgeführt werden soll, die erste Replikation abgeschlossen ist.

1. Wählen Sie **Wiederherstellungspläne > <Name des Wiederherstellungsplans>**.
2. Klicken Sie auf dem Blatt „Wiederherstellungsplan“ auf **Ungeplantes Failover**.
3. Wählen Sie auf der Seite **Ungeplantes Failover** den Quell- und Zielort aus.
4. Wählen Sie **Virtuelle Computer herunterfahren und die aktuellen Daten synchronisieren** aus, damit Site Recovery die geschützten virtuellen Computer herunterfährt und die Daten synchronisiert, sodass das Failover mit der neuesten Version der Daten erfolgt.
5. Nach dem Failover weist der Status der virtuellen Computer darauf hin, dass ein Commit aussteht.  Klicken Sie auf **Commit**, um ein Commit für das Failover auszuführen.

[Weitere Informationen](site-recovery-failover.md#run-an-unplanned-failover)

## <a name="complete-migration-of-your-virtual-machines-to-azure"></a>Abschließen der Migration für Ihre virtuellen Computer zu Azure
> [!NOTE]
> Die folgenden Schritte gelten nur, wenn Sie virtuelle Computer zu Azure migrieren.
>
>

1. Ein geplantes Failover können Sie durchführen, indem Sie die [hier](site-recovery-failover.md) angegebenen Informationen verwenden.
2. Klicken Sie unter **Einstellungen > Replizierte Elemente** mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Migration abschließen**.

    ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)
3. Klicken Sie auf **OK**, um die Migration abzuschließen. Sie können den Status verfolgen, indem Sie auf die VM klicken, um die Eigenschaften zu öffnen. Alternativ dazu können Sie unter **Einstellungen > Site Recovery-Aufträge** den Auftrag „Migration abschließen“ verwenden.

## <a name="monitor-your-deployment"></a>Überwachen der Bereitstellung
Hier wird beschrieben, wie Sie die Konfigurationseinstellungen, den Status und die Integrität für die Site Recovery-Bereitstellung überwachen können:

1. Klicken Sie auf den Tresornamen, um auf das Dashboard **Zusammenfassung** zuzugreifen. In diesem Dashboard werden Site Recovery-Aufträge, Replikationsstatus, Wiederherstellungspläne, Serverzustand und Ereignisse angezeigt.  Sie können die Zusammenfassung so anpassen, dass die nützlichsten Kacheln und Layouts angezeigt werden, z.B. auch der Status anderer Site Recovery- und Backup-Tresore.

    ![Zusammenfassung](./media/site-recovery-hyper-v-site-to-azure/essentials.png)
2. Auf der Kachel **Integrität** können Sie Standortserver, bei denen Probleme auftreten, sowie die von Site Recovery in den letzten 24 Stunden ausgelösten Ereignisse überwachen.
3. Sie können die Replikation über die Kacheln **Replizierte Elemente**, **Wiederherstellungspläne** und **Site Recovery-Aufträge** verwalten und überwachen. Genauere Details zu Aufträgen können Sie unter **Einstellungen** -> **Aufträge** -> **Site Recovery-Aufträge** anzeigen.



<!--HONumber=Jan17_HO5-->


