---
title: Replizieren von virtuellen Hyper-V-Maschinen in VMM-Clouds zu Azure | Microsoft Docs
description: Dieser Artikel beschreibt, wie virtuelle Hyper-V-Maschinen auf Hyper-V-Hosts in System Center VMM-Clouds zu Azure repliziert werden.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 9d526a1f-0d8e-46ec-83eb-7ea762271db5
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/01/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 27df1166a23e3ed89fdc86f861353c80a4a467ad
ms.openlocfilehash: 28c41f08bf8eaf7e6679040bb8fbab2e134d08fb


---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure"></a>Replizieren von virtuellen Hyper-V-Maschinen in VMM-Clouds zu Azure
> [!div class="op_single_selector"]
> * [Azure-Portal](site-recovery-vmm-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Klassisches Portal](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – klassisch](site-recovery-deploy-with-powershell.md)
>
>

Der Dienst Azure Site Recovery unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung, indem Replikation, Failover und Wiederherstellung virtueller Computer und physischer Server aufeinander abgestimmt werden. Computer können in Azure oder in einem sekundären lokalen Rechenzentrum repliziert werden. Eine kurze Übersicht über das Gesamtthema finden Sie unter [Was ist Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Übersicht
In diesem Artikel wird beschrieben, wie Sie Site Recovery bereitstellen, um virtuelle Hyper-V-Maschinen auf Hyper-V-Hostservern, die sich in privaten VMM-Clouds befinden, zu Azure replizieren.

Dieser Artikel enthält Informationen zu den Voraussetzungen für dieses Szenario und zeigt, wie Sie einen Site Recovery-Tresor einrichten, den Azure Site Recovery-Anbieter auf dem Quell-VMM-Server installieren, den Server im Tresor registrieren, Azure-Speicherkonten hinzufügen, den Azure Recovery Services-Agent auf Hyper-V-Hostservern installieren, Schutzeinstellungen für VMM-Clouds konfigurieren, die auf alle geschützten virtuellen Maschinen angewandt werden, und den Schutz für diese virtuellen Maschinen aktivieren. Zum Schluss testen Sie das Failover, um sicherzustellen, dass alles wie erwartet funktioniert.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

## <a name="architecture"></a>Architektur
![Architektur](./media/site-recovery-vmm-to-azure-classic/topology.png)

* Der Azure Site Recovery-Anbieter ist während der Bereitstellung von Site Recovery auf dem VMM-Server installiert, der wiederum im Site Recovery-Tresor registriert ist. Der Anbieter kommuniziert mit Site Recovery in Bezug auf die Replikationsorchestrierung.
* Der Azure Recovery Services-Agent wird während der Bereitstellung von Site Recovery auf Hyper-V-Host-Servern installiert. Er verarbeitet die Datenreplikation zum Azure-Speicher.

## <a name="azure-prerequisites"></a>Voraussetzungen für Azure
In Azure benötigen Sie Folgendes:

| **Voraussetzung** | **Details** |
| --- | --- |
| **Azure-Konto** |Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung. [hier](https://azure.microsoft.com/pricing/details/site-recovery/) . |
| **Azure-Speicher** |Sie benötigen ein Azure-Speicherkonto, um replizierte Daten zu speichern. Replizierte Daten werden im Azure-Speicher gespeichert, und virtuelle Azure-Computer werden bei einem Failover hochgefahren. <br/><br/>Sie benötigen ein [standardmäßiges georedundantes Speicherkonto](../storage/storage-redundancy.md#geo-redundant-storage). Das Konto muss sich in der gleichen Region wie der Site Recovery-Dienst befinden und dem gleichen Abonnement zugeordnet sein. Beachten Sie, dass eine Replikation in Storage Premium-Konten derzeit nicht unterstützt wird und nicht verwendet werden sollte.<br/><br/>[hier](../storage/storage-introduction.md) . |
| **Azure-Netzwerk** |Sie benötigen ein virtuelles Azure-Netzwerk, mit dem die virtuellen Azure-Computer eine Verbindung herstellen, wenn ein Failover stattfindet. Das virtuelle Azure-Netzwerk muss sich in der gleichen Region wie der Site Recovery-Tresor befinden. |

## <a name="on-premises-prerequisites"></a>Lokale Voraussetzungen
Lokal benötigen Sie Folgendes:

| **Voraussetzung** | **Details** |
| --- | --- |
| **VMM** |Sie benötigen mindestens einen VMM-Server, der als physischer oder virtueller eigenständiger Server oder als virtueller Cluster bereitgestellt wird. <br/><br/>Auf dem VMM-Server sollte System Center 2012 R2 mit den neuesten kumulativen Updates ausgeführt werden.<br/><br/>Sie benötigen mindestens eine Cloud, die auf dem VMM-Server konfiguriert ist.<br/><br/>Die Quellcloud, die Sie schützen möchten, muss mindestens eine VMM-Hostgruppe enthalten.<br/><br/>Weitere Informationen zum Einrichten von VMM-Clouds finden Sie unter [Walkthrough: Creating private clouds with System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) (Exemplarische Vorgehensweise: Erstellen privater Clouds mit System Center 2012 SP1 VMM) im Blog von Keith Mayer. |
| **Hyper-V** |Sie benötigen mindestens einen Hyper-V-Hostserver oder Cluster in der VMM-Cloud. Der Hostserver benötigt mindestens eine VM. <br/><br/>Auf dem Hyper-V-Hostserver muss mindestens **Windows Server 2012 R2** mit der Hyper-V-Rolle oder **Microsoft Hyper-V Server 2012 R2** ausgeführt werden, und die neuesten Updates müssen installiert sein.<br/><br/>Alle Hyper-V-Server, die zu schützende VMs enthalten, müssen sich in einer VMM-Cloud befinden.<br/><br/>Wenn Sie Hyper-V in einem Cluster ausführen, wird der Clusterbroker nicht automatisch erstellt, wenn Sie einen Cluster mit statischen IP-Adressen verwenden. Sie müssen den Clusterbroker manuell konfigurieren. [hier](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) . |
| **Geschützte Computer** |Die zu schützenden virtuellen Computer müssen den [Anforderungen für Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)entsprechen. |

## <a name="network-mapping-prerequisites"></a>Voraussetzungen für die Netzwerkzuordnung
Wenn Sie virtuelle Computer in Azure schützen, wird eine Netzwerkzuordnung zwischen VM-Netzwerken auf dem Quell-VMM-Server und den Azure-Zielnetzwerken erstellt, um Folgendes zu ermöglichen:

* Alle Computer, die ein Failover in demselben Netzwerk durchführen, können sich unabhängig vom jeweiligen Wiederherstellungsplan miteinander verbinden.
* Wenn ein Netzwerkgateway im Azure-Zielnetzwerk eingerichtet ist, können die virtuellen Computer eine Verbindung zu anderen lokalen virtuellen Computern herstellen.
* Wenn Sie keine Netzwerkzuordnung konfigurieren, können nur virtuelle Computer, für die ein Failover in demselben Wiederherstellungsplan erfolgt, nach einem Failover auf Azure eine Verbindung miteinander herstellen.

Wenn Sie eine Netzwerkzuordnung bereitstellen möchten, benötigen Sie Folgendes:

* Die virtuellen Computer, die Sie auf dem VMM-Quellserver schützen möchten, sollten mit einem VM-Netzwerk verbunden sein. Dieses Netzwerk sollte mit einem logischen Netzwerk verbunden sein, das der Cloud zugeordnet ist.
* Ein Azure-Netzwerk, mit dem replizierte virtuelle Computer nach einem Failover eine Verbindung herstellen können. Dieses Netzwerk wählen Sie zum Zeitpunkt des Failovers aus. Das Netzwerk sollte sich in derselben Region wie Ihr Azure Site Recovery-Abonnement befinden.

Bereiten Sie die Netzwerkzuordnung wie folgt vor:

1. [Machen Sie sich mit den Anforderungen für die Netzwerkzuordnung vertraut](site-recovery-network-mapping.md) .
2. Bereiten Sie VM-Netzwerke in VMM vor:

   * [Richten Sie logische Netzwerke ein](https://technet.microsoft.com/library/jj721568.aspx).
   * [Richten Sie VM-Netzwerke ein](https://technet.microsoft.com/library/jj721575.aspx).

## <a name="step-1-create-a-site-recovery-vault"></a>Schritt 1: Erstellen eines Site Recovery-Tresors
1. Melden Sie sich über den VMM-Server, den Sie registrieren möchten, beim [Verwaltungsportal](https://portal.azure.com) an.
2. Klicken Sie auf **Data Services** > **Recovery Services** > **Site Recovery-Tresor**.
3. Klicken Sie auf **Neu erstellen** > **Schnellerfassung**.
4. Geben Sie unter **Name**einen Anzeigenamen ein, über den der Tresor identifiziert wird.
5. Wählen Sie unter **Region**die geografische Region für den Tresor aus. Eine Liste mit den unterstützten Regionen finden Sie in den [Preisdetails zu Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/)unter „Geografische Verfügbarkeit“.
6. Klicken Sie auf **Tresor erstellen**.

    ![Neuer Tresor](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Überprüfen Sie auf der Statusleiste, ob der Tresor erfolgreich erstellt wurde. Der Tresor wird auf der Hauptseite von Recovery Services als **aktiv** aufgelistet.

## <a name="step-2-generate-a-vault-registration-key"></a>Schritt 2: Generieren eines Tresorregistrierungsschlüssels
Generieren Sie einen Registrierungsschlüssel im Tresor. Nachdem Sie den Azure Site Recovery-Anbieter heruntergeladen und auf dem VMM-Server installiert haben, verwenden Sie diesen Schlüssel, um den VMM-Server im Tresor zu registrieren.

1. Klicken Sie auf der Seite **Recovery Services** auf den Tresor, um die Seite „Schnellstart“ zu öffnen. Schnellstart kann auch jederzeit über das Symbol geöffnet werden.

    ![Schnellstart-Symbol](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)
2. Wählen Sie in der Dropdownliste den Eintrag **Zwischen einem lokalen VMM-Standort und Microsoft Azure**aus.
3. Klicken Sie unter **VMM-Server vorbereiten** auf **Registrierungsschlüssel erstellen**. Die Schlüsseldatei wird automatisch generiert und ist nach ihrer Erstellung für 5 Tage gültig. Wenn Sie nicht vom VMM-Server aus auf das Azure-Portal zugreifen, müssen Sie diese Datei auf den Server kopieren.

    ![Registrierungsschlüssel](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Schritt 3: Installieren des Azure Site Recovery-Anbieters
1. Klicken Sie unter **Schnellstart** > **VMM-Server vorbereiten** auf **Microsoft Azure Site Recovery-Anbieter für die Installation auf VMM-Servern herunterladen**, um die neueste Version der Anbieter-Installationsdatei herunterzuladen.
2. Führen Sie diese Datei auf dem VMM-Quellserver aus.

   > [!NOTE]
   > Wenn VMM in einem Cluster bereitgestellt wird und Sie den Anbieter erstmals installieren, führen Sie die Installation auf einem aktiven Knoten durch und schließen Sie sie ab, um den VMM-Server im Tresor zu registrieren. Installieren Sie den Anbieter anschließend auf den anderen Knoten. Beachten Sie, dass Sie ein Upgrade des Anbieters auf allen Knoten durchführen müssen, da auf allen Knoten dieselbe Anbieterversion ausgeführt werden sollte.
   >
   >
3. Das Installationsprogramm führt eine Voraussetzungsprüfung durch und fordert die Berechtigung zum Beenden des VMM-Diensts an, um das Setup des Anbieters zu starten. Der VMM-Dienst wird nach Abschluss des Setups automatisch neu gestartet. Bei der Installation auf einem VMM-Cluster werden Sie aufgefordert, die Clusterrolle zu beenden.
4. Unter **Microsoft Update** können Sie Updates abonnieren. Mit dieser Einstellung werden Anbieterupdates gemäß Ihrer Microsoft Update-Richtlinie installiert.

    ![Microsoft Updates](./media/site-recovery-vmm-to-azure-classic/updates.png)
5. Der Installationspfad für den Anbieter ist auf **<SystemDrive>\Programme\Microsoft System Center 2012 R2\Virtual Machine Manager\bin** festgelegt. Klicken Sie auf **Installieren**.

   ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)
6. Klicken Sie nach der Installation des Anbieters auf **Registrieren** , um den Server im Tresor zu registrieren.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)
7. Prüfen Sie unter **Tresorname**den Namen des Tresors, in dem der Server registriert wird. Klicken Sie auf *Weiter*.

    ![Serverregistrierung](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)
8. Geben Sie auf der Seite **Internetverbindung** an, wie sich der Anbieter auf dem VMM-Server mit dem Internet verbinden soll. Wählen Sie **Mit vorhandenen Proxyeinstellungen verbinden** aus, um die Standard-Internetverbindungseinstellungen auf dem Server zu verwenden.

    ![Interneteinstellungen](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

   * Wenn Sie einen benutzerdefinierten Proxy verwenden möchten, sollten Sie diesen vor der Installation des Anbieters einrichten. Wenn Sie benutzerdefinierte Proxyeinstellungen konfigurieren, wird ein Test ausgeführt, um die Proxyverbindung zu überprüfen.
   * Wenn Sie einen benutzerdefinierten Proxy verwenden oder Ihr Standardproxy eine Authentifizierung verlangt, müssen Sie die Proxydetails einschließlich der Proxyadresse und des Ports eingeben.
   * Der VMM-Server und die Hyper-V-Hosts sollten auf die folgenden URLs Zugriff haben:
     * *.hypervrecoverymanager.windowsazure.com
     * *.accesscontrol.windows.net
     * *.backup.windowsazure.com
     * *.blob.core.windows.net
     * *.store.core.windows.net
   * Lassen Sie die unter [IP-Bereiche des Azure-Rechenzentrums](https://www.microsoft.com/download/confirmation.aspx?id=41653) angegebenen IP-Adressen sowie das HTTPS-Protokoll (443) zu. Fügen Sie die IP-Adressbereiche der zu verwendenden Azure-Region sowie die IP-Adressbereiche der westlichen USA einer Positivliste hinzu.
   * Wenn Sie einen benutzerdefinierten Proxy verwenden, wird ein ausführendes VMM-Konto (DRAProxyAccount) automatisch mit den angegebenen Proxyanmeldeinformationen erstellt. Konfigurieren Sie den Proxyserver so, dass dieses Konto erfolgreich authentifiziert werden kann. In der VMM-Konsole können die Einstellungen des ausführenden VMM-Kontos geändert werden. Zu diesem Zweck öffnen Sie den Arbeitsbereich **Einstellungen**, erweitern Sie **Sicherheit**, klicken Sie auf **Ausführende Konten**, und ändern Sie das Kennwort für DRAProxyAccount. Sie müssen den VMM-Dienst neu starten, damit diese Einstellung wirksam wird.
9. Wählen Sie unter **Registrierungsschlüssel**den Schlüssel aus, den Sie von Azure Site Recovery heruntergeladen und auf den VMM-Server kopiert haben.
10. Die Verschlüsselungseinstellung wird nur verwendet, wenn Sie Hyper-V-VMs in VMM-Clouds in Azure replizieren. Bei der Replikation an einem sekundären Standort wird sie nicht verwendet.
11. Geben Sie unter **Servername**einen Anzeigenamen ein, um den VMM-Server im Tresor zu identifizieren. Geben Sie in einer Clusterkonfiguration den Rollennamen des VMM-Clusters an.
12. Wählen Sie unter **Cloudmetadaten synchronisieren** aus, ob Sie Metadaten für alle Clouds auf dem VMM-Server mit dem Tresor synchronisieren möchten. Diese Aktion muss für jeden VMM-Server nur einmal ausgeführt werden. Wenn Sie nicht alle Clouds synchronisieren möchten, können Sie diese Einstellung deaktiviert lassen und in den Cloudeigenschaften in der VMM-Konsole jede Cloud einzeln synchronisieren.
13. Klicken Sie auf **Weiter** , um den Prozess abzuschließen. Nach der Registrierung werden die Metadaten vom VMM-Server von Azure Site Recovery abgerufen. Der Server wird im Tresor auf der Registerkarte **VMM-Server** der Seite **Server** angezeigt.

    ![Lastpage](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Nach der Registrierung werden die Metadaten vom VMM-Server von Azure Site Recovery abgerufen. Der Server wird im Tresor auf der Registerkarte **VMM-Server** der Seite **Server** angezeigt.

### <a name="command-line-installation"></a>Installation über die Befehlszeile
Der Azure Site Recovery-Anbieter kann auch über die folgende Befehlszeile installiert werden. Mit dieser Methode kann der Anbieter in Server Core für Windows Server 2012 R2 installiert werden.

1. Laden Sie die Installationsdatei und den Registrierungsschlüssel des Anbieters in einen Ordner herunter. Beispiel: C:\ASR.
2. Beenden Sie den System Center Virtual Machine Manager-Dienst.
3. Extrahieren Sie über eine Eingabeaufforderung mit erhöhten Rechten mithilfe der folgenden Befehle das Installationsprogramm des Anbieters:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Installieren Sie den Anbieter wie folgt:

        C:\ASR> setupdr.exe /i
5. Registrieren Sie den Anbieter wie folgt:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Die Parameter lauten wie folgt:

* **/Credentials** : erforderlicher Parameter zum Angeben des Speicherorts der Registrierungsschlüsseldatei.  
* **/FriendlyName** : erforderlicher Parameter für den Namen des Hyper-V-Hostservers, der im Azure Site Recovery-Portal angezeigt wird.
* **/EncryptionEnabled** : optionaler Parameter, um anzugeben, ob Sie Ihre virtuellen Computer in Azure verschlüsseln möchten (Verschlüsselung im ruhenden Zustand). Der Dateiname muss die Erweiterung **.pfx** haben.
* **/proxyAddress** : optionaler Parameter, der die Adresse des Proxyservers angibt.
* **/proxyport** : optionaler Parameter, der den Port des Proxyservers angibt.
* **/proxyUsername** : optionaler Parameter, der den Proxybenutzernamen angibt.
* **/proxyPassword** : optionaler Parameter, der das Proxykennwort angibt.  

## <a name="step-4-create-an-azure-storage-account"></a>Schritt 4: Erstellen eines Azure-Speicherkontos
1. Falls Sie noch kein Azure-Speicherkonto haben, klicken Sie zum Erstellen eines Kontos auf **Azure Storage-Konto hinzufügen** .
2. Erstellen Sie ein Konto mit aktivierter geografischer Replikation. Es muss sich in der gleichen Region wie der Azure Site Recovery-Dienst befinden und dem gleichen Abonnement zugeordnet sein.

    ![Speicherkonto](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [!NOTE]
> [Die Migration von Speicherkonten](../azure-resource-manager/resource-group-move-resources.md) zwischen Ressourcengruppen im gleichen Abonnement oder zwischen verschiedenen Abonnements wird für Speicherkonten nicht unterstützt, die für die Site Recovery-Bereitstellung verwendet werden.
>
>

## <a name="step-5-install-the-azure-recovery-services-agent"></a>Schritt 5: Installieren des Azure Recovery Services-Agent
Installieren Sie den Azure Recovery Services-Agent auf jedem Hyper-V-Hostserver in der VMM-Cloud.

1. Klicken Sie auf **Schnellstart** > **Azure Site Recovery Services-Agent für die Installation auf Hosts herunterladen**, um die neueste Version der Agent-Installationsdatei zu erhalten.

    ![Recovery Services-Agent installieren](./media/site-recovery-vmm-to-azure-classic/install-agent.png)
2. Führen Sie die Installationsdatei auf jedem Hyper-V-Hostserver aus.
3. Klicken Sie auf der Seite **Voraussetzungsüberprüfung** auf **Weiter**. Alle fehlenden Komponenten, die Voraussetzung sind, werden automatisch installiert.

    ![Voraussetzungen Recovery Services-Agent](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)
4. Geben Sie auf der Seite **Installationseinstellungen** an, wo Sie den Agent installieren möchten, und wählen Sie den Cachespeicherort aus, an dem die Sicherungsmetadaten installiert werden sollen. Klicken Sie dann auf **Weiter**.
5. Klicken Sie nach Abschluss der Installation auf **Schließen** , um den Assistenten abzuschließen.

    ![MARS-Agent registrieren](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Installation über die Befehlszeile
Sie können den Microsoft Azure Recovery Services-Agent auch mit dem folgenden Befehl über die Befehlszeile installieren:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Schritt 6: Konfigurieren der Cloudschutzeinstellungen
Nachdem Sie den VMM-Server registriert haben, können Sie Schutzeinstellungen für die Clouds konfigurieren. Sie haben bei der Installation des Anbieters die Option **Cloud-Daten mit Tresor synchronisieren** aktiviert. Deshalb werden alle Clouds auf dem VMM-Server im Tresor auf der Registerkarte <b>Geschützte Elemente</b> angezeigt.

![Veröffentlichte Cloud](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Klicken Sie auf der Seite „Schnellstart“ auf **Schutz für VMM-Clouds einrichten**.
2. Klicken Sie auf der Registerkarte **Geschützte Elemente** auf die Cloud, die Sie konfigurieren möchten, und wechseln Sie zur Registerkarte **Konfiguration**.
3. Geben Sie unter **Ziel** select **Azure**
4. Wählen Sie unter **Speicherkonto** das Azure-Speicherkonto aus, das Sie für die Replikation verwenden.
5. Legen Sie für **Gespeicherte Daten verschlüsseln** die Option **Aus** fest. Diese Einstellung gibt an, dass die Daten beim Replizieren zwischen dem lokalen Standort und Azure verschlüsselt werden sollen.
6. Lassen Sie unter **Kopierhäufigkeit** die Standardeinstellung stehen. Dieser Wert gibt an, wie oft die Daten zwischen dem Quell- und dem Zielspeicherort synchronisiert werden sollen.
7. Lassen Sie unter **Wiederherstellungspunkte beibehalten für**die Standardeinstellung stehen. Wenn die Standardeinstellung 0 (Null) aktiviert ist, wird nur der letzte Wiederherstellungspunkt für einen primären virtuellen Computer auf einem Replikathostserver gespeichert.
8. Lassen Sie unter **Häufigkeit von anwendungskonsistenten Momentaufnahmen**die Standardeinstellung stehen. Dieser Wert gibt an, wie oft Momentaufnahmen erstellt werden. Momentaufnahmen verwenden VSS (Volume Shadow Copy Service, Volumenschattenkopie-Dienst), um sicherzustellen, dass Anwendungen zum Zeitpunkt der Momentaufnahme konsistent sind.  Wenn Sie einen Wert festlegen möchten, stellen Sie sicher, dass er geringer als die Anzahl der konfigurierten zusätzlichen Wiederherstellungspunkte ist.
9. Geben Sie unter **Startzeit für Replikation**an, wann die erste Datenreplikation an Azure starten sollte. Die Zeitzone auf dem Hyper-V-Hostserver wird verwendet. Es wird empfohlen, die erste Replikation außerhalb der Spitzenzeiten zu planen.

    ![Cloud-Replikationseinstellungen](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

Nachdem Sie die Einstellungen gespeichert haben, wird ein Auftrag erstellt, der auf der Registerkarte **Aufträge** überwacht werden kann. Alle Hyper-V-Hostserver in der VMM-Quellcloud werden für die Replikation konfiguriert.

Nach dem Speichern können die Cloudeinstellungen auf der Registerkarte **Konfigurieren** bearbeitet werden. Um den Zielspeicherort bzw. das Zielspeicherkonto zu ändern, müssen Sie die Cloudkonfiguration entfernen und dann die Cloud neu konfigurieren. Beachten Sie: Wenn Sie das Speicherkonto ändern, wird die Änderung nur für virtuelle Computer angewandt, für die der Schutz nach Änderung des Speicherkontos aktiviert wird. Vorhandene virtuelle Computer werden nicht an das neue Speicherkonto migriert.

## <a name="step-7-configure-network-mapping"></a>Schritt 7: Konfigurieren der Netzwerkzuordnung
Bevor Sie mit der Netzwerkzuordnung beginnen, stellen Sie sicher, dass virtuelle Computer auf dem VMM-Quellserver mit einem VM-Netzwerk verbunden sind. Erstellen Sie außerdem ein oder mehrere virtuelle Azure-Netzwerke. Beachten Sie, dass einem einzelnen Azure-Netzwerk mehrere VM-Netzwerke zugeordnet werden können.

1. Klicken Sie auf der Seite „Schnellstart“ auf **Netzwerke zuordnen**.
2. Wählen Sie auf der Registerkarte **Netzwerke** unter **Quellspeicherort** den VMM-Quellserver aus. Wählen Sie unter **Zielspeicherort** die Option "Azure" aus.
3. Unter **Quelle** wird eine Liste der VM-Netzwerke angezeigt, die mit dem VMM-Server verbunden sind. Unter **Ziel** werden die dem Abonnement zugeordneten Azure-Netzwerke angezeigt.
4. Wählen Sie das VM-Quellnetzwerk aus, und klicken Sie auf **Zuordnen**.
5. Wählen Sie auf der Seite **Zielnetzwerk auswählen** das Azure-Zielnetzwerk aus, das Sie verwenden möchten.
6. Aktivieren Sie das Kontrollkästchen, um den Zuordnungsprozess abzuschließen.

    ![Cloud-Replikationseinstellungen](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

Nachdem Sie die Einstellungen gespeichert haben, wird ein Auftrag zum Nachverfolgen des Zuordnungsvorgangs gestartet, der auf der Registerkarte "Aufträge" überwacht werden kann. Vorhandene virtuelle Replikatcomputer, die dem VM-Quellnetzwerk entsprechen, werden mit den Azure-Zielnetzwerken verbunden. Neue virtuelle Computer, die mit dem VM-Quellnetzwerk verbunden sind, werden nach der Replikation mit dem zugeordneten Azure-Netzwerk verbunden. Wenn Sie eine vorhandene Zuordnung mit einem neuen Netzwerk ändern, werden virtuelle Replikatcomputer anhand der neuen Einstellungen verbunden.

Wenn das Zielnetzwerk mehrere Subnetze enthält und eines dieser Subnetze denselben Namen besitzt wie das Subnetz des virtuellen Quellcomputers, dann wird der virtuelle Replikatcomputer nach einem Failover mit diesem Zielsubnetz verbunden. Gibt es kein Zielsubnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.

> [!NOTE]
> Für Netzwerke, die für die Site Recovery-Bereitstellung verwendet werden, wird die [Migration von Netzwerken](../azure-resource-manager/resource-group-move-resources.md) zwischen Ressourcengruppen im selben Abonnement oder zwischen verschiedenen Abonnements nicht unterstützt.
>
>

## <a name="step-8-enable-protection-for-virtual-machines"></a>Schritt 8: Aktivieren des Schutzes für virtuelle Computer
Nach der korrekten Konfiguration von Servern, Clouds und Netzwerken können Sie den Schutz für die virtuellen Computer in der Cloud aktivieren. Beachten Sie Folgendes:

* Die virtuellen Computer müssen die [Azure-Anforderungen](site-recovery-best-practices.md#azure-virtual-machine-requirements)erfüllen.
* Um den Schutz zu aktivieren, müssen die Eigenschaften "Betriebssystem" und "Betriebssystem-Datenträger" für den virtuellen Computer gesetzt sein. Sie können diese Eigenschaften setzen, wenn Sie den virtuellen Computer in VMM mithilfe einer Vorlage für virtuelle Computer erstellen. Außerdem können Sie diese Eigenschaften für vorhandene virtuelle Computer auf den Registerkarten **Allgemein** und **Hardwarekonfiguration** in den Eigenschaften der virtuellen Computer festlegen. Falls diese Eigenschaften in VMM nicht angezeigt werden, sollten Sie sie dennoch im Azure Site Recovery-Portal konfigurieren können.

    ![Erstellen eines virtuellen Computers](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![Eigenschaften des virtuellen Computers ändern](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)

1. Klicken Sie zum Aktivieren des Schutzes auf der Registerkarte **Virtuelle Computer** in der Cloud, in der sich der virtuelle Computer befindet, auf **Schutz aktivieren** > **Virtuelle Computer hinzufügen**.
2. Wählen Sie aus der Liste der virtuellen Computer in der Cloud denjenigen aus, den Sie schützen möchten.

    ![Schutz für virtuelle Computer aktivieren](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    Auf der Registerkarte **Aufträge** können Sie den Fortschritt der Aktion **Schutz aktivieren** und der ersten Replikation verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der virtuelle Computer bereit zum Failover. Nach dem Aktivieren des Schutzes und der Replikation der virtuellen Computer können Sie die virtuellen Computer in Azure sehen.

    ![Schutzauftrag für virtuellen Computer](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

1. Überprüfen Sie den virtuellen Computer und nehmen Sie ggf. Änderungen vor.

    ![Überprüfen des virtuellen Computers](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)
2. Auf der Registerkarte **Konfigurieren** unter den Eigenschaften des virtuellen Computers können die unten angegebenen Netzwerkeigenschaften geändert werden.

* **Anzahl von Netzwerkadaptern auf dem virtuellen Zielcomputer:** Die Anzahl von Netzwerkadaptern hängt von der Größe ab, die Sie für den virtuellen Zielcomputer angeben. Überprüfen Sie in den [Spezifikationen für virtuelle Computer](../virtual-machines/virtual-machines-linux-sizes.md#size-tables) , wie viele Adapter von virtuellen Computern einer bestimmten Größe unterstützt werden. Wenn Sie die Größe für einen virtuellen Computer ändern und die Einstellungen speichern, wird die Anzahl der Netzwerkadapter beim nächsten Öffnen der Seite **Konfigurieren** geändert. Die Anzahl der Netzwerkadapter der virtuellen Zielmaschinen entspricht der Mindestzahl an Netzwerkadaptern auf virtuellen Quellmaschinen und der Höchstzahl an Netzwerkadaptern, die von der ausgewählten Größe der virtuellen Maschine unterstützt werden:

  * Wenn die Anzahl der Netzwerkkarten des Quellcomputers maximal der Anzahl der Netzwerkkarten entspricht, die für die Größe des Zielcomputers zulässig ist, hat der Zielcomputer die gleiche Anzahl von Netzwerkkarten wie der Quellcomputer.
  * Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputer die maximal zulässige Anzahl für die Größe des Zielcomputers übersteigt, wird die Anzahl verwendet, die maximal für die Größe des Zielcomputers zulässig ist.
  * Wenn beispielsweise ein Quellcomputer zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe vier Netzwerkadapter unterstützt, erhält der Zielcomputer zwei Netzwerkadapter. Wenn der Quellcomputer dagegen zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe nur einen Adapter unterstützt, erhält der Zielcomputer nur einen Adapter.     
* **Netzwerk des virtuellen Zielcomputers:** Das Netzwerk, mit dem der virtuelle Computer eine Verbindung herstellt, wird durch die Netzwerkzuordnung des virtuellen Quellcomputers bestimmt. Wenn die virtuelle Quellmaschine über mehrere Netzwerkadapter verfügt und Quellnetzwerke verschiedenen Netzwerken im Ziel zugeordnet sind, müssen Sie zwischen den Zielnetzwerken wählen.
* **Subnetz der einzelnen Netzwerkadapter:** Sie können für jeden Netzwerkadapter das Subnetz auswählen, mit dem der virtuelle Computer, auf dem ein Failover ausgeführt wird, eine Verbindung herstellt.
* **Ziel-IP-Adresse:** Wenn der Netzwerkadapter des virtuellen Quellcomputers für die Verwendung statischer IP-Adressen konfiguriert ist, können Sie die IP-Adresse für den virtuellen Zielcomputer angeben. Verwenden Sie dieses Feature, um die IP-Adresse einer virtuellen Quellmaschine nach einem Failover beizubehalten. Wenn keine IP-Adresse angegeben wird, erhält der Netzwerkadapter beim Failover eine beliebige verfügbare IP-Adresse. Wenn die Ziel-IP-Adresse angegeben ist, sie aber bereits von einer anderen virtuellen Maschine in Azure verwendet wird, tritt beim Failover ein Fehler auf.  

    ![Ändern der Netzwerkeigenschaften](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

> [!NOTE]
> Virtuelle Linux-Maschinen mit statischen IP-Adressen werden nicht unterstützt.
>
>

## <a name="test-the-deployment"></a>Testen der Bereitstellung
Um Ihre Bereitstellung zu testen, können Sie ein Testfailover für eine einzelne virtuellen Maschine durchführen oder einen Wiederherstellungsplan erstellen, der mehrere virtuelle Maschinen umfasst, und ein Testfailover für diesen Plan durchführen.  

Das Testfailover simuliert Ihre Failover- und Wiederherstellungsmechanismen in einem isolierten Netzwerk. Beachten Sie Folgendes:

* Wenn Sie nach dem Failover eine Verbindung mit dem virtuellen Computer in Azure über Remote Desktop herstellen möchten, aktivieren Sie die Remote Desktop-Verbindung auf dem virtuellen Computer, bevor Sie das Testfailover ausführen.
* Nach dem Failover verwenden Sie eine öffentliche IP-Adresse, um eine Verbindung zum virtuellen Computer in Azure über Remote Desktop herzustellen. Wenn Sie dies möchten, stellen Sie sicher, dass keine Domänenrichtlinien vorhanden sind, die das Verbinden zu einem virtuellen Computer über eine öffentliche Adresse verhindern.

> [!NOTE]
> Um beim Durchführen eines Failovers in Azure die bestmögliche Leistung zu erzielen, sollten Sie sicherstellen, dass Sie den Azure-Agent auf dem geschützten Computer installiert haben. Dies fördert das schnellere Starten und ist hilfreich für die Diagnose bei Problemen. Den Linux-Agent finden Sie [hier](https://github.com/Azure/WALinuxAgent), den Windows-Agent [hier](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

### <a name="create-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans
1. Fügen Sie auf der Registerkarte **Wiederherstellungspläne** einen neuen Plan hinzu. Geben Sie einen Namen an. Geben Sie **VMM** unter **Quelltyp** und den VMM-Server unter **Quelle** an. Das „Ziel“ ist Azure.

    ![Wiederherstellungsplan erstellen](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)
2. Wählen Sie auf der Seite **Virtuelle Computer auswählen** die virtuellen Computer aus, die dem Wiederherstellungsplan hinzugefügt werden sollen. Diese virtuellen Computer werden der Standardgruppe des Wiederherstellungsplans (Gruppe 1) hinzugefügt. Maximal 100 virtuelle Computer wurden in einem einzelnen Wiederherstellungsplan getestet.

* Wenn Sie die Eigenschaften der virtuellen Computer überprüfen möchten, bevor Sie sie dem Plan hinzufügen, klicken Sie auf den virtuellen Computer auf der Eigenschaftenseite der Cloud, in der er sich befindet. Sie können auch die Eigenschaften des virtuellen Computers in der VMM-Konsole konfigurieren.
* Für alle angezeigten virtuellen Computer wurde die Schutzfunktion aktiviert. Die Liste enthält sowohl virtuelle Computer, die für den Schutz aktiviert wurden und deren erste Replikation abgeschlossen ist, als auch solche, die für den Schutz aktiviert wurden, deren erste Replikation jedoch noch aussteht. Nur virtuelle Computer, deren erste Replikation abgeschlossen ist, können im Rahmen eines Wiederherstellungsplans ein Failover durchführen.

    ![Wiederherstellungsplan erstellen](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

Nachdem ein Wiederherstellungsplan erstellt wurde, wird er auf der Registerkarte **Wiederherstellungspläne** angezeigt. Sie können dem Wiederherstellungsplan auch [Azure Automation-Runbooks](site-recovery-runbook-automation.md) hinzufügen, um Aktionen beim Failover zu automatisieren.

### <a name="run-a-test-failover"></a>Durchführen eines Test-Failovers
Es gibt zwei Möglichkeiten, ein Testfailover in Azure auszuführen.

* **Testfailover ohne Azure-Netzwerk**: Bei dieser Art von Testfailover wird überprüft, ob die virtuelle Maschine in Azure richtig hochgefahren wird. Der virtuelle Computer wird nach einem Failover mit keinem Azure-Netzwerk verbunden.
* **Testfailover mit einem Azure-Netzwerk**: Bei dieser Art von Failover wird überprüft, ob die gesamte Replikationsumgebung wie erwartet hochgefahren wird und die betreffenden virtuellen Computer mit dem angegebenen Azure-Zielnetzwerk verbunden werden. Bei der Subnetzverarbeitung wird für das Testfailover das Subnetz des virtuellen Testcomputers basierend auf dem Subnetz des virtuellen Replikatcomputers festgelegt. Dies unterscheidet sich von der regulären Replikation, bei der das Subnetz des virtuellen Replikatcomputers auf dem Subnetz des virtuellen Quellcomputers basiert.

Wenn Sie ein Testfailover für einen virtuellen Computer, der für den Schutz in Azure aktiviert ist, ohne Angabe eines Azure-Zielnetzwerks durchführen möchten, müssen Sie nichts vorbereiten. Für ein Testfailover mit einem Azure-Zielnetzwerk müssen Sie ein neues Azure-Netzwerk erstellen, das von Ihrem Azure-Produktionsnetzwerk isoliert ist (Standardverhalten bei der Erstellung eines neuen Netzwerks in Azure). Unter [Ausführen eines Testfailovers](site-recovery-failover.md#run-a-test-failover) finden Sie weitere Details.

Sie müssen außerdem die Infrastruktur so einrichten, dass die replizierte virtuelle Maschine erwartungsgemäß funktioniert. So kann beispielsweise ein virtueller Computer mit Domänencontroller und DNS mithilfe von Azure Site Recovery zu Azure repliziert und mittels Testfailover im Testnetzwerk erstellt werden. Unter [Überlegungen zum Testfailover für Active Directory](site-recovery-active-directory.md#test-failover-considerations) finden Sie weitere Details.

Gehen Sie folgendermaßen vor, um ein Testfailover durchzuführen:

1. Wählen Sie auf der Registerkarte **Wiederherstellungspläne** den gewünschten Wiederherstellungsplan aus, und klicken Sie auf **Testfailover**.
2. Wählen Sie auf der Seite **Testfailover bestätigen** den Eintrag **Kein** oder ein bestimmtes Azure-Netzwerk aus.  Beachten Sie, dass ein Testfailover bei Auswahl von "Kein" prüft, ob der virtuelle Computer ordnungsgemäß nach Azure repliziert wurde. Es wird jedoch nicht die Konfiguration des Replikationsnetzwerks geprüft.

    ![Kein Netzwerk](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)
3. Wenn für die Cloud Datenverschlüsselung aktiviert ist, wählen Sie unter **Verschlüsselungsschlüssel** das Zertifikat aus, das während der Installation des Anbieters auf dem VMM-Server ausgestellt wurde, als Sie die Option zum Aktivieren der Datenverschlüsselung für eine Cloud aktiviert haben.
4. Auf der Registerkarte **Aufträge** können Sie den Fortschritt des Failovers verfolgen. Außerdem wird Ihnen das Testreplikat des virtuellen Computers im Azure-Portal angezeigt. Wenn Sie den Zugriff auf virtuelle Computer aus Ihrem lokalen Netzwerk eingerichtet haben, können Sie eine Remotedesktopverbindung mit dem virtuellen Computer herstellen.
5. Klicken Sie auf **Test abschließen**, wenn das Failover die Phase **Tests abschließen** erreicht, um das Testfailover abzuschließen. Auf der Registerkarte **Auftrag** können Sie den Failoverfortschritt und -status detailliert nachverfolgen und etwaige erforderliche Aufgaben ausführen.
6. Nach dem Failover können Sie das Testreplikat des virtuellen Computers im Azure-Portal sehen. Wenn Sie den Zugriff auf virtuelle Computer aus Ihrem lokalen Netzwerk eingerichtet haben, können Sie eine Remotedesktopverbindung mit dem virtuellen Computer herstellen. Gehen Sie wie folgt vor:

   1. Prüfen Sie, ob die virtuellen Computer erfolgreich starten.
   2. Wenn Sie nach dem Failover eine Verbindung mit dem virtuellen Computer in Azure über Remote Desktop herstellen möchten, aktivieren Sie die Remote Desktop-Verbindung auf dem virtuellen Computer, bevor Sie das Test-Failover ausführen. Außerdem müssen Sie dem virtuellen Computer einen RDP-Endpunkt hinzufügen. Hierzu können Sie ein Azure Automation-Runbook [](site-recovery-runbook-automation.md) verwenden.
   3. Falls Sie nach dem Failover über eine öffentliche IP-Adresse eine Remotedesktopverbindung mit dem virtuellen Computer in Azure herstellen, achten Sie darauf, dass keine Domänenrichtlinien vorhanden sind, die dies verhindern.
7. Gehen Sie nach dem Test wie folgt vor:

   * Klicken Sie auf **Das Testfailover ist abgeschlossen**. Bereinigen Sie die Testumgebung, um die virtuellen Testcomputer automatisch auszuschalten und zu löschen.
   * Klicken Sie auf **Notizen** , um alle Beobachtungen im Zusammenhang mit dem Test-Failover aufzuzeichnen und zu speichern.


## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich über das [Einrichten von Wiederherstellungsplänen](site-recovery-create-recovery-plans.md) und über [Failover](site-recovery-failover.md).



<!--HONumber=Dec16_HO4-->


