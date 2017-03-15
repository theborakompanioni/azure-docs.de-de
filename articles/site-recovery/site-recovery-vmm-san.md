---
title: Replizieren virtueller Hyper-V-Computer in VMM mit SAN mithilfe von Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie virtuelle Hyper-V-Maschinen zwischen zwei Standorten mit Azure Site Recovery per SAN-Replikation replizieren.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: eb480459-04d0-4c57-96c6-9b0829e96d65
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/19/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 67b4861ac564565b2a36932ae15141a1e1f56035
ms.openlocfilehash: 29084a52aca22b31a2fa42660ec2cdf050aec7ca
ms.lasthandoff: 02/23/2017


---
# <a name="replicate-hyper-v-vms-in-vmm-clouds-to-a-secondary-site-with-azure-site-recovery-by-using-san"></a>Replizieren von virtuellen Hyper-V-Computern in VMM-Clouds an einen sekundären Standort mit Azure Site Recovery per SAN


Verwenden Sie diesen Artikel, wenn Sie [Azure Site Recovery](site-recovery-overview.md) bereitstellen möchten, um die Replikation virtueller (in System Center Virtual Machine Manager-Clouds verwalteter) Hyper-V-Computer an einen sekundären VMM-Standort mit Azure Site Recovery im klassischen Portal zu verwalten. Dieses Szenario ist nicht im neuen Azure-Portal verfügbar.



Am Ende dieses Artikels können Sie einen Kommentar eingeben. Antworten auf technische Fragen erhalten Sie im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="why-replicate-with-san-and-site-recovery"></a>Argumente für die Replikation mit SAN und Site Recovery

* SAN bietet eine professionelle, skalierbare Replikationslösung, mit der ein primärer Standort, der über Hyper-V mit SAN verfügt, LUNs an einen sekundären Standort mit SAN replizieren kann. Der Speicher wird von VMM verwaltet, Replikation und Failover werden mithilfe von Site Recovery orchestriert.
* Site Recovery bietet in Zusammenarbeit mit mehreren [SAN-Speicherpartnern](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) eine übergreifende Replikation für Fibre Channel und iSCSI-Speicher.  
* Nutzen Sie Ihre vorhandene SAN-Infrastruktur, um unternehmenskritische, in Hyper-V-Clustern bereitgestellte Apps zu schützen. Virtuelle Computer können als Gruppe repliziert werden, um ein konsistentes Failover für n-schichtige Apps zu ermöglichen.
* Die SAN-Replikation gewährleistet Replikationskonsistenz auf verschiedenen Ebenen einer Anwendung – mit synchronisierter Replikation zur Erzielung niedriger RTO- und RPO-Werte sowie mit nicht synchronisierter Replikation zur Erzielung einer hohen Flexibilität (abhängig von den Funktionen Ihres Speicherarrays).  
* Sie können SAN-Speicher in der VMM-Fabric verwalten und vorhandenen Speicher mithilfe von SMI-S in VMM ermitteln.  

Beachten Sie Folgendes:
- Die Site-to-Site-Replikation mit SAN ist im Azure-Portal nicht verfügbar. Sie steht nur im klassischen Portal zur Verfügung. Im klassischen Portal können keine neuen Tresore erstellt werden. Vorhandene Tresore können verwaltet werden.
- Die Replikation von SAN an Azure wird nicht unterstützt.
- Sie können weder freigegebene VHDX-Dateien noch logische Einheiten (Logical Units, LUNs) replizieren, die über iSCSI oder Fibre Channel direkt mit virtuellen Computern verbunden sind. Gastcluster können repliziert werden.


## <a name="architecture"></a>Architektur

![SAN-Architektur](./media/site-recovery-vmm-san/architecture.png)

- **Azure:** Richten Sie im Azure-Portal einen Site Recovery-Tresor ein.
- **SAN-Speicher:** SAN-Speicher wird in der VMM-Fabric verwaltet. Sie fügen den Speicheranbieter hinzu, erstellen Speicherklassifizierungen und richten Speicherpools ein.
- **VMM und Hyper-V:** Wir empfehlen die Verwendung eines VMM-Servers an jedem Standort. Richten Sie private VMM-Clouds ein, und platzieren Sie darin Hyper-V-Cluster. Im Zuge der Bereitstellung wird der Azure Site Recovery-Anbieter auf den einzelnen VMM-Servern installiert, und die Server werden im Tresor registriert. Der Anbieter kommuniziert mit dem Site Recovery-Dienst, um Replikation, Failover und Failback zu verwalten.
- **Replikation:** Nachdem Sie den Speicher in VMM eingerichtet und die Replikation in Site Recovery konfiguriert haben, wird die Replikation zwischen dem primären und sekundären SAN-Speicher ausgeführt. An Site Recovery werden keine Replikationsdaten gesendet.
- **Failover:** Aktivieren Sie das Failover über das Site Recovery-Portal. Bei einem Failover gehen keine Daten verloren, da die Replikation synchron erfolgt.
- **Failback:** Für das Failback ermöglichen Sie die umgekehrte Replikation, um Deltaänderungen vom sekundären Standort an den primären Standort zu übertragen. Nach Abschluss der umgekehrten Replikation führen Sie ein geplantes Failover vom sekundären an den primären Standort durch. Dieses geplante Failover stoppt die virtuellen Replikatcomputer am sekundären Standort und startet sie am primären Standort.


## <a name="before-you-start"></a>Vorbereitung


**Voraussetzungen** | **Details**
--- | ---
**Azure** |Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung. [hier](https://azure.microsoft.com/pricing/details/site-recovery/) . Erstellen Sie einen Azure Site Recovery-Tresor zum Konfigurieren und Verwalten von Replikation und Failover.
**VMM** | Sie können zwar einen einzelnen VMM-Server verwenden und die Replizierung zwischen verschiedenen Clouds durchführen, es empfiehlt sich jedoch, einen VMM-Server am primären und einen VMM-Server am sekundären Standort zu verwenden. Ein VMM kann als physischer oder als virtueller eigenständiger Server (oder als Cluster) bereitgestellt werden. <br/><br/>Auf dem VMM-Server sollte System Center 2012 R2 oder höher mit den neuesten kumulativen Updates ausgeführt werden.<br/><br/> Für jeden Server muss mindestens eine Cloud auf dem primären, zu schützenden VMM-Server und eine Cloud auf dem sekundären VMM-Server konfiguriert sein, der für Failover verwendet werden soll.<br/><br/> Die Quellcloud muss mindestens eine VMM-Hostgruppe enthalten.<br/><br/> Für alle VMM-Clouds muss das Hyper-V-Kapazitätsprofil eingestellt sein.<br/><br/> Weitere Informationen zum Einrichten von VMM-Clouds finden Sie unter [Bereitstellen einer privaten VMM-Cloud](https://technet.microsoft.com/en-us/system-center-docs/vmm/scenario/cloud-overview).
**Hyper-V** | Primäre und sekundäre VMM-Clouds müssen mindestens einen Hyper-V-Cluster enthalten.<br/><br/> Der Hyper-V-Quellcluster muss mindestens einen virtuellen Computer enthalten.<br/><br/> Die VMM-Hostgruppen am primären und sekundären Standort müssen mindestens einen der Hyper-V-Cluster enthalten.<br/><br/>Auf dem Hyper-V-Hostserver und -Zielserver muss Windows Server 2012 oder höher mit der Hyper-V-Rolle ausgeführt werden, und die neuesten Updates müssen installiert sein.<br/><br/> Wenn Sie Hyper-V in einem Cluster ausführen und einen Cluster mit statischen IP-Adressen verwenden, wird der Clusterbroker nicht automatisch erstellt. Sie müssen ihn manuell konfigurieren. Weitere Informationen finden Sie unter [Preparing host clusters for Hyper-V replica](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) (Vorbereiten von Hostclustern für das Hyper-V-Replikat).
**SAN-Speicher** | Sie können virtuelle Computer in Gastclustern mit iSCSI- oder Channel-Speicher oder mit freigegebenen virtuellen Festplatten (VHDX) replizieren.<br/><br/> Sie benötigen zwei SAN-Arrays: eines am primären und eines am sekundären Standort.<br/><br/> Zwischen den Arrays muss eine Netzwerkinfrastruktur eingerichtet sein. Peering und Replikation müssen konfiguriert sein. Replikationslizenzen müssen in Einklang mit den Anforderungen an Speicherarrays eingerichtet sein.<br/><br/>Richten Sie ein Netzwerk zwischen den Hyper-V-Hostservern und dem Speicherarray ein, damit die Hosts über iSCSI oder Fibre Channel mit den Speicher-LUNs kommunizieren können.<br/><br/> Informationen zu unterstützten Speicherarrays finden Sie [hier](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx).<br/><br/> SMI-S-Anbieter von Speicherarrayherstellern müssen installiert sein, und die SAN-Arrays müssen vom Anbieter verwaltet werden. Richten Sie den Anbieter gemäß den Anweisungen des Herstellers ein.<br/><br/>Stellen Sie sicher, dass sich der SMI-S-Anbieter des Arrays auf einem Server befindet, auf den der VMM-Server über das Netzwerk mit einer IP-Adresse oder einem FQDN zugreifen kann.<br/><br/> Jedes SAN-Array muss über mindestens einen Speicherpool verfügen.<br/><br/> Der primäre VMM-Server muss das primäre Array verwalten, und der sekundäre VMM-Server muss das sekundäre Array verwalten.
**Netzwerkzuordnung** | Richten Sie eine Netzwerkzuordnung ein, damit replizierte virtuelle Computer nach einem Failover optimal auf sekundären Hyper-V-Hostservern platziert werden und eine Verbindung mit den entsprechenden VM-Netzwerken hergestellt wird. Wenn Sie die Netzwerkzuordnung nicht konfigurieren, werden Replikat-VMs nach einem Failover nicht mit einem Netzwerk verbunden.<br/><br/> Stellen Sie sicher, dass VMM-Netzwerke ordnungsgemäß konfiguriert sind, sodass Sie die Netzwerkzuordnung im Rahmen der Site Recovery-Bereitstellung einrichten können. In VMM müssen die virtuellen Computer des Hyper-V-Quellhosts mit einem VMM-VM-Netzwerk verbunden sein. Dieses Netzwerk sollte mit einem logischen Netzwerk verbunden sein, das der Cloud zugeordnet ist.<br/><br/> Für die Zielcloud muss ein entsprechendes VM-Netzwerk vorhanden sein. Dieses muss wiederum mit einem entsprechenden logischen Netzwerk verknüpft sein, das der Zielcloud zugeordnet ist.<br/><br/>verfügbar.

## <a name="step-1-prepare-the-vmm-infrastructure"></a>Schritt 1: Vorbereiten der VMM-Infrastruktur
Die Vorbereitung der VMM-Infrastruktur umfasst folgende Schritte:

1. Überprüfen der VMM-Clouds.
2. Integrieren und Klassifizieren von SAN-Speicher in VMM.
3. Erstellen von LUNs und Zuweisen von Speicher.
4. Erstellen von Replikationsgruppen.
5. Einrichten von VM-Netzwerken.

### <a name="verify-vmm-clouds"></a>Überprüfen der VMM-Clouds

Vergewissern Sie sich, dass Ihre VMM-Clouds ordnungsgemäß eingerichtet sind, bevor Sie mit der Bereitstellung von Site Recovery beginnen.

### <a name="integrate-and-classify-san-storage-in-the-vmm-fabric"></a>Integrieren und Klassifizieren von SAN-Speicher in der VMM-Fabric

1. Wechseln Sie in der VMM-Konsole zu **Fabric** > **Speicher** > **Ressourcen hinzufügen** > **Speichergeräte**.
2. Wählen Sie im Assistenten zum **Hinzufügen von Speichergeräten** den Eintrag **Wählen Sie einen Speicheranbietertyp aus.**, und wählen Sie **Von einem SMI-S-Anbieter erkannte und verwaltete SAN- und NAS-Geräte** aus.

    ![Anbietertyp](./media/site-recovery-vmm-san/provider-type.png)

3. Wählen Sie auf der Seite **Protokoll und Adresse des SMI-S-Speicheranbieters angeben** den Eintrag **SMI-S CIMXML** aus, und geben Sie die Einstellungen zum Herstellen einer Verbindung mit dem Anbieter an.
4. Geben Sie unter **IP-Adresse oder FQDN des Anbieters** und **TCP/IP-Port** die Einstellungen zum Herstellen einer Verbindung mit dem Anbieter an. Für SMI-S CIMXML können Sie nur eine SSL-Verbindung verwenden.

    ![Anbieterverbindung](./media/site-recovery-vmm-san/connect-settings.png)
5. Geben Sie unter **Ausführendes Konto** ein ausführendes VMM-Konto an, das auf den Anbieter zugreifen kann, oder erstellen Sie ein Konto.
6. Auf der Seite **Informationen sammeln** versucht VMM, die Informationen zu den Speichergeräten automatisch zu erfassen. Klicken Sie zum Wiederholen der Erkennung auf **Anbieter durchsuchen**. Bei Erfolg des Erkennungsprozesses werden erkannte Speicherarrays und -pools, Hersteller, Modell und Kapazität auf der Seite aufgeführt.

    ![Speicher ermitteln](./media/site-recovery-vmm-san/discover.png)
7. Wählen Sie unter **Wählen Sie Speicherpools aus, die Sie unter "Verwaltung" platzieren, und weisen Sie dann eine Klassifizierung zu**die Speicherpools aus, die VMM verwalten soll, und weisen Sie ihnen eine Klassifizierung zu. LUN-Informationen werden aus den Speicherpools importiert. Erstellen Sie LUNs basierend auf den zu schützenden Anwendungen, ihren Kapazitätsanforderungen und Ihren Anforderungen an die Elemente, die gemeinsam repliziert werden sollen.

    ![Speicher klassifizieren](./media/site-recovery-vmm-san/classify.png)

### <a name="create-luns-and-allocate-storage"></a>Erstellen von LUNs und Zuweisen von Speicher

Erstellen Sie LUNs basierend auf den zu schützenden Anwendungen, Kapazitätsanforderungen und Ihren Anforderungen an die Elemente, die gemeinsam repliziert werden sollen.

1. Wenn der Speicher in der VMM-Fabric erscheint, können Sie [LUNs bereitstellen](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-storage-host-groups#create-a-lun-in-vmm).

     > [!NOTE]
     > Fügen Sie keine virtuellen Festplatten für den virtuellen Computer hinzu, die für die Replikation an LUNs aktiviert sind. Wenn diese LUNs sich nicht in einer Site Recovery-Replikationsgruppe befinden, werden sie nicht von Site Recovery erkannt.
     >

2. Weisen Sie dem Hyper-V-Hostcluster Speicherkapazität zu, damit VMM Daten auf virtuellen Computern im bereitgestellten Speicher bereitstellen kann:

   * Vor der Zuordnung von Speicher zum Cluster müssen Sie den Speicher der VMM-Hostgruppe zuordnen, in der sich der Cluster befindet. Weitere Informationen finden Sie unter [Zuordnen von logischen Speichereinheiten zu einer Hostgruppe in VMM](https://technet.microsoft.com/library/gg610686.aspx) sowie unter [Zuordnen von Speicherpools zu einer Hostgruppe in VMM](https://technet.microsoft.com/library/gg610635.aspx).
   * Ordnen Sie dem Cluster Speicherkapazität gemäß den Angaben unter [Konfigurieren von Speicher für einen Hyper-V-Hostcluster in VMM](https://technet.microsoft.com/library/gg610692.aspx) zu.

    ![Anbietertyp](./media/site-recovery-vmm-san/provider-type.png)
3. Wählen Sie unter **Geben Sie das Protokoll und die Adresse des SMI-S-Speicheranbieters an.** die Option **SMI-S CIM-XML** aus. Geben Sie die Einstellungen für die Verbindung mit dem Anbieter an. Eine SSL-Verbindung können Sie nur für SMI-S CIMXML verwenden.

    ![Anbieterverbindung](./media/site-recovery-vmm-san/connect-settings.png)
4. Geben Sie unter **Ausführendes Konto** ein ausführendes VMM-Konto an, das auf den Anbieter zugreifen kann, oder erstellen Sie ein Konto.
5. Unter **Informationen sammeln** versucht VMM, die Informationen zu den Speichergeräten automatisch zu erkennen und zu importieren. Klicken Sie zum Wiederholen des Vorgangs auf **Anbieter durchsuchen**. Bei erfolgreicher Erkennung werden Speicherarrays und -pools, Hersteller, Modell und Kapazität auf der Seite aufgeführt.

    ![Speicher ermitteln](./media/site-recovery-vmm-san/discover.png)
7. Wählen Sie unter **Wählen Sie Speicherpools aus, die Sie unter "Verwaltung" platzieren, und weisen Sie dann eine Klassifizierung zu.**die Speicherpools aus, die VMM verwalten soll, und weisen Sie ihnen eine Klassifizierung zu. LUN-Informationen werden aus den Speicherpools importiert.

    ![Speicher klassifizieren](./media/site-recovery-vmm-san/classify.png)


### <a name="create-replication-groups"></a>Erstellen von Replikationsgruppen

Erstellen Sie eine Replikationsgruppe, die alle LUNs enthält, die gemeinsam repliziert werden sollen.

1. Öffnen Sie in der VMM-Konsole die Registerkarte **Replikationsgruppen** der Speicherarray-Eigenschaften, und klicken Sie auf **Neu**.
2. Erstellen Sie die Replikationsgruppe.

    ![SAN-Replikationsgruppe](./media/site-recovery-vmm-san/rep-group.png)

### <a name="set-up-networks"></a>Einrichten der Netzwerke

Wenn Sie eine Netzwerkzuordnung konfigurieren möchten, gehen Sie wie folgt vor:

1. Siehe „Netzwerkzuordnung für Site Recovery“.
2. Bereiten Sie VM-Netzwerke in VMM vor:

   * [Richten Sie logische Netzwerke ein](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-logical-networks).
   * [Richten Sie VM-Netzwerke ein](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-vm-networks).


## <a name="step-2-create-a-vault"></a>Schritt 2: Erstellen eines Tresors

1. Melden Sie sich über den VMM-Server, den Sie im Tresor registrieren möchten, beim [Azure-Portal](https://portal.azure.com) an.
2. Erweitern Sie **Data Services** > **Recovery Services**, und klicken Sie auf **Site Recovery-Tresor**.
3. Klicken Sie auf **Neu erstellen** > **Schnellerfassung**.
4. Geben Sie unter **Name**einen Anzeigenamen ein, über den der Tresor identifiziert wird.
5. Wählen Sie unter **Region**die geografische Region für den Tresor aus. Informationen zu den unterstützten Regionen finden Sie unter [Site Recovery Preise](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klicken Sie auf **Tresor erstellen**.

    ![Neuer Tresor](./media/site-recovery-vmm-san/create-vault.png)

Überprüfen Sie auf der Statusleiste, ob der Tresor erfolgreich erstellt wurde. Der Tresor wird auf der Hauptseite von **Recovery Services** als **aktiv** aufgelistet.

### <a name="register-the-vmm-servers"></a>Registrieren der VMM-Server

1. Öffnen Sie auf der Seite **Recovery Services** die Seite **Schnellstart**. Schnellstart kann auch jederzeit über das Symbol geöffnet werden.

    ![Schnellstart-Symbol](./media/site-recovery-vmm-san/quick-start-icon.png)
2. Wählen Sie im Dropdownfeld den Eintrag **Zwischen lokalen Hyper-V-Standorten mithilfe der Arrayreplikation** aus.

    ![Registrierungsschlüssel](./media/site-recovery-vmm-san/select-san.png)
3. Laden Sie unter **VMM-Server vorbereiten**die aktuelle Version der Installationsdatei für den Azure Site Recovery-Anbieter herunter.
4. Führen Sie diese Datei auf dem VMM-Quellserver aus. Wenn VMM in einem Cluster bereitgestellt wird und Sie den Anbieter erstmals installieren, führen Sie die Anbieterinstallation auf einem aktiven Knoten durch und schließen Sie sie ab, um den VMM-Server im Tresor zu registrieren. Installieren Sie den Anbieter anschließend auf den anderen Knoten. Anbieterupgrades müssen auf allen Knoten durchgeführt werden, damit alle über die gleiche Anbieterversion verfügen.
5. Das Installationsprogramm überprüft die Voraussetzungen und fordert die Berechtigung zum Beenden des VMM-Diensts an, um die Anbietereinrichtung zu starten. Der Dienst wird nach Abschluss der Einrichtung automatisch neu gestartet. In einem VMM-Cluster werden Sie zum Beenden der Clusterrolle aufgefordert.
6. Unter **Microsoft Update** können Sie Updates aktivieren. Anbieterupdates werden daraufhin gemäß Ihrer Microsoft Update-Richtlinie installiert.

    ![Microsoft Updates](./media/site-recovery-vmm-san/ms-update.png)

7. Der Installationspfad für den Anbieter ist standardmäßig auf „<SystemDrive>\Programme\Microsoft System Center 2012 R2\Virtual Machine Manager\bin“ festgelegt. Klicken Sie auf **Installieren**, um den Vorgang zu starten.

    ![Installationsspeicherort](./media/site-recovery-vmm-san/install-location.png)

8. Klicken Sie nach der Installation des Anbieters auf **Registrieren**, um den VMM-Server im Tresor zu registrieren.

    ![Installation abgeschlossen](./media/site-recovery-vmm-san/install-complete.png)

9. Geben Sie auf der Seite **Internetverbindung** an, wie der Anbieter eine Verbindung mit dem Internet herstellen soll. Wählen Sie **Proxyeinstellungen des Systems verwenden** aus, wenn Sie die standardmäßigen Internetverbindungseinstellungen auf dem Server verwenden möchten.

    ![Interneteinstellungen](./media/site-recovery-vmm-san/proxy-details.png)

   * Wenn Sie einen benutzerdefinierten Proxy verwenden möchten, richten Sie diesen vor der Anbieterinstallation ein. Wenn Sie benutzerdefinierte Proxyeinstellungen konfigurieren, wird ein Test ausgeführt, um die Proxyverbindung zu überprüfen.
   * Wenn Sie einen benutzerdefinierten Proxy verwenden oder Ihr Standardproxy eine Authentifizierung erfordert, geben Sie die Proxydetails einschließlich Adresse und Port ein.
   * Die erforderlichen URLs müssen für den VMM-Server zugänglich sein.
   * Wenn Sie einen benutzerdefinierten Proxy verwenden, wird ein ausführendes VMM-Konto (DRAProxyAccount) automatisch mit den angegebenen Proxyanmeldeinformationen erstellt. Konfigurieren Sie den Proxyserver so, dass für dieses Konto eine Authentifizierung möglich ist. Die Einstellungen für ausführende Konten können über die VMM-Konsole unter **Einstellungen** > **Sicherheit** > **Ausführende Konten** > **DRAProxyAccount** geändert werden. Sie müssen den VMM-Dienst neu starten, damit die Änderung wirksam wird.
10. Wählen Sie unter **Registrierungsschlüssel** den Schlüssel aus, den Sie aus dem Portal heruntergeladen und auf den VMM-Server kopiert haben.
11. Prüfen Sie unter **Tresorname**den Namen des Tresors, in dem der Server registriert wird.

    ![Serverregistrierung](./media/site-recovery-vmm-san/vault-creds.png)
12. Die Verschlüsselungseinstellung wird nur für die Replikation von VMM an Azure verwendet. Sie kann daher ignoriert werden.

    ![Serverregistrierung](./media/site-recovery-vmm-san/encrypt.png)
13. Geben Sie unter **Servername**einen Anzeigenamen ein, um den VMM-Server im Tresor zu identifizieren. Geben Sie in einer Clusterkonfiguration den Rollennamen des VMM-Clusters an.
14. Wählen Sie unter **Erste Cloudmetadaten-Synchronisierung** aus, ob Sie Metadaten für alle Clouds auf dem VMM-Server synchronisieren möchten. Diese Aktion muss für jeden VMM-Server nur einmal ausgeführt werden. Wenn Sie nicht alle Clouds synchronisieren möchten, können Sie diese Einstellung deaktiviert lassen und in den Cloudeigenschaften in der VMM-Konsole jede Cloud einzeln synchronisieren.

    ![Serverregistrierung](./media/site-recovery-vmm-san/friendly-name.png)

15. Klicken Sie auf **Weiter** , um den Prozess abzuschließen. Nach der Registrierung werden die Metadaten vom VMM-Server von Azure Site Recovery abgerufen. Der Server wird im Tresor unter **Server** > **VMM-Server** angezeigt.

### <a name="command-line-installation"></a>Installation über die Befehlszeile

Der Azure Site Recovery-Anbieter kann auch über die folgende Befehlszeile installiert werden. Mit dieser Methode kann der Anbieter in Server Core für Windows Server 2012 R2 installiert werden.

1. Laden Sie die Installationsdatei und den Registrierungsschlüssel des Anbieters in einen Ordner herunter. Beispiel: C:\ASR.
2. Beenden Sie den VMM-Dienst.
3. Extrahieren Sie das Anbieterinstallationsprogramm. Führen Sie die folgenden Befehle als Administrator aus:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Installieren Sie den Anbieter:

        C:\ASR> setupdr.exe /i
5. Registrieren Sie den Anbieter:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>         

Parameter:

* **/Credentials**: Erforderlicher Parameter zum Angeben des Speicherorts der Registrierungsschlüsseldatei.  
* **/FriendlyName**: Erforderlicher Parameter für den Namen des Hyper-V-Hostservers, der im Azure Site Recovery-Portal angezeigt wird.
* **/EncryptionEnabled**: Optionaler Parameter. Wird nur bei der Replikation von VMM nach Azure verwendet.
* **/proxyAddress**: Optionaler Parameter, der die Adresse des Proxyservers angibt.
* **/proxyport**: Optionaler Parameter, der den Port des Proxyservers angibt.
* **/proxyUsername**: Optionaler Parameter, der den Proxybenutzernamen angibt (sofern der Proxy eine Authentifizierung erfordert).
* **/proxyPassword**: Optionaler Parameter, der das Kennwort für die Authentifizierung mit dem Proxyserver angibt (sofern der Proxy eine Authentifizierung erfordert).

## <a name="step-3-map-storage-arrays-and-pools"></a>Schritt 3: Zuordnen von Speicherarrays und -pools

Ordnen Sie primäre und sekundäre Arrays zu, um anzugeben, welcher sekundäre Speicherpool Replikationsdaten vom primären Pool empfangen soll. Die Speicherzuordnung muss vor dem Konfigurieren der Replikation durchgeführt werden, da die Zuordnungsinformationen beim Aktivieren des Schutzes für Replikationsgruppen genutzt werden.

Prüfen Sie zunächst, ob im Tresor VMM-Clouds vorhanden sind. Clouds werden erkannt, wenn Sie im Zuge der Anbieterinstallation alle Clouds synchronisieren oder wenn Sie eine bestimmte Cloud in der VMM-Konsole synchronisieren.

1. Klicken Sie auf **Ressourcen** > **Serverspeicher** > **Quell- und Zielarrays zuordnen**.
    ![Serverregistrierung](./media/site-recovery-vmm-san/storage-map.png)

2. Wählen Sie die Speicherarrays für den primären Standort aus, und ordnen Sie sie Speicherarrays für den sekundären Standort zu. Wählen Sie unter **Speicherpools** einen Quell- und Zielspeicherpool für die Zuordnung aus.

    ![Serverregistrierung](./media/site-recovery-vmm-san/storage-map-pool.png)

## <a name="step-4-configure-replication-settings"></a>Schritt 4: Konfigurieren der Replikationseinstellungen

Nachdem Sie die VMM-Server registriert haben, konfigurieren Sie Schutzeinstellungen für die Clouds.

1. Klicken Sie auf der Seite **Schnellstart** auf **Schutz für VMM-Clouds einrichten**.
2. Wählen Sie auf der Registerkarte **Geschützte Elemente** die Cloud und anschließend **Konfiguration** aus.
3. Wählen Sie unter **Ziel** die Option **VMM** aus.
4. Wählen Sie unter **Zielspeicherort** den VMM-Server aus, der die Cloud verwaltet, die Sie für die Wiederherstellung verwenden möchten.
5. Wählen Sie unter **Zielcloud** die gewünschte Zielcloud für VM-Failover aus.
   * Sie sollten eine Zielcloud auszuwählen, die die Voraussetzungen für die Wiederherstellung der virtuellen Computer erfüllt, die Sie schützen möchten.
   * Eine Cloud kann nur zu einem einzigen Cloudpaar gehören – entweder als primäre oder als Zielcloud.
6. Site Recovery prüft, ob Clouds Zugriff auf SAN-Speicher haben und ob die Speicherarrays zugeordnet wurden.
7. Wählen Sie nach erfolgreicher Überprüfung unter **Replikationstyp** die Option **SAN** aus.

Nach dem Speichern der Einstellungen wird ein Auftrag erstellt. Dieser kann auf der Registerkarte **Aufträge** überwacht werden. Die Einstellungen können auf der Registerkarte **Konfigurieren** geändert werden. Wenn Sie den Zielspeicherort oder die Zielcloud ändern möchten, müssen Sie die Cloudkonfiguration entfernen und die Cloud anschließend neu konfigurieren.

## <a name="step-5-enable-network-mapping"></a>Schritt 5: Aktivieren der Netzwerkzuordnung

1. Klicken Sie auf der Seite **Schnellstart** auf **Netzwerke zuordnen**.
2. Wählen Sie erst den VMM-Quellserver und anschließend den VMM-Zielserver aus, dem die Netzwerke zugeordnet werden sollen. Die Liste der Quellnetzwerke und ihre zugeordneten Zielnetzwerke werden angezeigt. Für Netzwerke ohne Zuordnung wird ein leerer Wert angezeigt. Klicken Sie auf das Informationssymbol neben den Namen des Quell- und Zielnetzwerks, um die Subnetze für jedes Netzwerk anzuzeigen.
3. Wählen Sie unter **Netzwerk auf Quelle** ein Netzwerk aus, und klicken Sie anschließend auf **Zuordnen**. Der Dienst erkennt die VM-Netzwerke auf dem Zielserver und zeigt sie an.

    ![SAN-Architektur](./media/site-recovery-vmm-san/network-map1.png)
4. Wählen Sie eines der VM-Netzwerke auf dem VMM-Zielserver aus.

    ![SAN-Architektur](./media/site-recovery-vmm-san/network-map2.png)

5. Wenn Sie ein Zielnetzwerk auswählen, werden die geschützten Clouds angezeigt, die das Quellnetzwerk verwenden. Die verfügbaren Zielnetzwerke werden ebenfalls angezeigt. Es empfiehlt sich, ein Zielnetzwerk auszuwählen, das für alle Clouds verfügbar ist, die Sie für die Replikation verwenden.
6. Aktivieren Sie das Kontrollkästchen, um den Zuordnungsprozess abzuschließen. Ein Auftrag beginnt, der den Status nachverfolgt. Dieser kann auf der Registerkarte **Aufträge** angezeigt werden.

## <a name="step-6-enable-replication-for-replication-groups"></a>Schritt 6: Aktivieren der Replikation für Replikationsgruppen

Bevor Sie den Schutz für virtuelle Computer aktivieren können, müssen Sie die Replikation für Speicherreplikationsgruppen aktivieren.

1. Öffnen Sie im Site Recovery-Portal auf der Seite **Eigenschaften** der primären Cloud die Registerkarte **Virtuelle Computer**, und klicken Sie anschließend auf **Replikationsgruppe hinzufügen**.
2. Wählen Sie eine oder mehrere VMM-Replikationsgruppen aus, die der Cloud zugeordnet sind, überprüfen Sie Quell- und Zielarrays, und geben Sie die Replikationsfrequenz an.

Site Recovery, VMM und die SMI-S-Anbieter stellen die Speicher-LUNs für den Zielstandort bereit und aktivieren die Speicherreplikation. Falls die Replikationsgruppe bereits repliziert wurde, verwendet Site Recovery die vorhandene Replikationsbeziehung erneut und aktualisiert die Informationen.

## <a name="step-7-enable-protection-for-virtual-machines"></a>Schritt 7: Aktivieren des Schutzes für virtuelle Computer


Aktivieren Sie bei der Replikation einer Speichergruppe mithilfe einer der folgenden Methoden den Schutz für virtuelle Computer in der VMM-Konsole:

* **Neuer virtueller Computer**: Wenn Sie einen virtuellen Computer erstellen, aktivieren Sie die Replikation, und ordnen Sie den virtuellen Computer der Replikationsgruppe zu.
  Bei dieser Option nutzt VMM die intelligente Platzierung, um den Speicher virtueller Computer optimal in den LUNs der Replikationsgruppe zu platzieren. Site Recovery orchestriert die Erstellung eines virtuellen Schattencomputers am sekundären Standort und weist die Kapazität so zu, dass nach einem Failover virtuelle Replikatcomputer gestartet werden können.
* **Vorhandener virtueller Computer**: Wenn in VMM bereits ein virtueller Computer bereitgestellt wurde, können Sie die Replikation aktivieren und eine Speichermigration zu einer Replikationsgruppe durchführen. Danach erkennen VMM und Site Recovery den neuen virtuellen Computer und beginnen mit dessen Verwaltung in Site Recovery. Ein virtueller Schattencomputer wird am sekundären Standort erstellt. Diesem wird Kapazität zugewiesen, sodass nach einem Failover der virtuelle Replikatcomputer gestartet werden kann.

![Schutz aktivieren](./media/site-recovery-vmm-san/enable-protect.png)

Für die Replikation aktivierte virtuelle Computer werden in der Site Recovery-Konsole angezeigt. Sie können die Eigenschaften virtueller Computer anzeigen, den Status und Failover-Replikationsgruppen mit mehreren virtuellen Computern nachverfolgen. Bei der SAN-Replikation muss das Failover für alle einer Replikationsgruppe zugeordneten virtuellen Computer gemeinsam erfolgen. Der Grund ist, dass das Failover zuerst auf der Speicherebene erfolgt. Wichtig: Die Replikationsgruppen müssen ordnungsgemäß gruppiert werden, und nur einander zugeordnete virtuelle Computer dürfen gemeinsam platziert werden.

> [!NOTE]
> Nachdem Sie die Replikation für einen virtuellen Computer aktiviert haben, fügen Sie seine VHDs keinen LUNs hinzu, sofern diese sich nicht in einer Site Recovery-Replikationsgruppe befinden. VHDs werden nur dann von Site Recovery erkannt, wenn sie sich in einer Site Recovery-Replikationsgruppe befinden.
>
>

Auf der Registerkarte **Aufträge** können Sie den Fortschritt sowie die erste Replikation nachverfolgen. Nach Ausführung des Auftrags „Schutz abschließen“ ist der virtuelle Computer bereit zum Failover.

![Schutzauftrag für virtuellen Computer](./media/site-recovery-vmm-san/job-props.png)

## <a name="step-8-test-the-deployment"></a>Schritt 8: Testen der Bereitstellung

Testen Sie Ihre Bereitstellung, um sicherzugehen, dass das Failover der virtuellen Computer erwartungsgemäß funktioniert. Erstellen Sie hierzu einen Wiederherstellungsplan, und führen Sie ein Testfailover aus.

1. Klicken Sie auf der Registerkarte **Wiederherstellungspläne** auf **Wiederherstellungsplan erstellen**.
2. Geben Sie einen Namen für den Wiederherstellungsplan an, und wählen Sie VMM-Quell- und Zielserver aus. Der Quellserver muss über virtuelle Computer verfügen, die für Failover und Wiederherstellung aktiviert sind. Wählen Sie **SAN** aus, um nur Clouds anzuzeigen, die für die SAN-Replikation konfiguriert sind.

    ![Wiederherstellungsplan erstellen](./media/site-recovery-vmm-san/r-plan.png)

3. Wählen Sie unter **Virtuellen Computer auswählen** Replikationsgruppen aus. Alle der Gruppe zugeordneten virtuellen Computer werden dem Wiederherstellungsplan hinzugefügt. Diese virtuellen Computer werden der Standardgruppe des Wiederherstellungsplans (Gruppe 1) hinzugefügt. Sie können bei Bedarf weitere Gruppen hinzufügen. Nach der Replikation werden virtuelle Computer gemäß der Reihenfolge der Wiederherstellungsplan-Gruppen gestartet.

    ![Virtuelle Computer auswählen](./media/site-recovery-vmm-san/r-plan-vm.png)
4. Erstellte Wiederherstellungspläne werden auf der Registerkarte **Wiederherstellungspläne** angezeigt. Wählen Sie den Plan aus und dann **Testfailover**.
5. Wählen Sie auf der Seite **Testfailover bestätigen** den Eintrag **Kein** aus. Ist diese Option aktiviert, sind die virtuellen Replikatcomputer nach einem Failover nicht mit einem Netzwerk verbunden. Mit dieser Option wird nur getestet, ob das Failover der virtuellen Computer erwartungsgemäß funktioniert hat. Die Netzwerkumgebung wird dabei nicht getestet. Weitere Informationen zu anderen Netzwerkoptionen finden Sie unter [Failover in Site Recovery](site-recovery-failover.md).

    ![Testnetzwerk auswählen](./media/site-recovery-vmm-san/test-fail1.png)

6. Der virtuelle Testcomputer wird auf dem Host erstellt, auf dem sich auch der virtuelle Replikatcomputer befindet. Er wird nicht der Cloud hinzugefügt, in der sich der virtuelle Replikatcomputer befindet.
2. Nach der Replikation hat der virtuelle Replikatcomputer eine andere IP-Adresse als der primäre virtuelle Computer. Wenn Sie Adressen über DHCP vergeben, werden sie automatisch aktualisiert. Wenn Sie DHCP nicht verwenden und identische Adressen verwenden möchten, müssen Sie mehrere Skripts ausführen.
3. Führen Sie das folgende Skript aus, um die IP-Adresse abzurufen:

       $vm = Get-SCVirtualMachine -Name <VM_NAME>
       $na = $vm[0].VirtualNetworkAdapters>
       $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
       $ip.address  

4. Führen Sie das folgende Skript aus, um DNS zu aktualisieren. Geben Sie die abgerufene IP-Adresse an.

       [string]$Zone,
       [string]$name,
       [string]$IP
       )
       $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
       $newrecord = $record.clone()
       $newrecord.RecordData[0].IPv4Address  =  $IP
       Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie ein Testfailover ausgeführt haben, um die richtige Funktionsweise der Umgebung zu überprüfen, informieren Sie sich in [Failover in Site Recovery](site-recovery-failover.md) über die unterschiedlichen Failoverarten.

