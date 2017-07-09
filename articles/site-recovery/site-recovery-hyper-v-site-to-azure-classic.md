---
title: Replizieren von Hyper-V-VMs in Azure im klassischen Azure-Portal | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie virtuelle Hyper-V-Computer in Azure repliziert werden, wenn die Computer nicht in VMM-Clouds verwaltet werden.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 3f4c4483-e3dd-495a-bd02-c16e9e28c88d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/21/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 305f02bb120a1096c46de94d103a5700dabdc8ba
ms.contentlocale: de-de
ms.lasthandoff: 04/03/2017


---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-without-vmm-with-azure-site-recovery"></a>Replizieren zwischen lokalen virtuellen Hyper-V-Maschinen und Azure (ohne VMM) mit Azure Site Recovery
> [!div class="op_single_selector"]
> * [Azure-Portal](site-recovery-hyper-v-site-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
> * [Klassisches Portal](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

In diesem Artikel erfahren Sie, wie Sie lokale virtuelle Hyper-V-Computer mit dem [Azure Site Recovery](site-recovery-overview.md)-Dienst über das Azure-Portal in Azure replizieren. In diesem Szenario werden Hyper-V-Server nicht in VMM-Clouds verwaltet.

Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery im Azure-Portal

Azure verfügt über zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen: Azure Resource Manager- und klassische Bereitstellung. Azure verfügt zudem über zwei Portale: das klassische Azure-Portal und das Azure-Portal.

In diesem Artikel wird die Bereitstellung über das klassische Portal beschrieben. Das klassische Portal kann verwendet werden, um bestehende Tresore zu verwalten. Sie können keine neuen Tresore mithilfe des klassischen Azure-Portals erstellen.

## <a name="site-recovery-in-your-business"></a>Site Recovery in Ihrem Unternehmen

Organisationen benötigen eine BCDR-Strategie, die bestimmt, wie Apps und Daten bei geplanten und ungeplanten Ausfällen verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können. Site Recovery bietet folgende Vorteile:

* Offsite-Schutz für Unternehmens-Apps, die auf virtuellen Hyper-V-Computern ausgeführt werden
* Zentraler Ort zum Einrichten, Verwalten und Überwachen von Replikation, Failover und Wiederherstellung
* Unkompliziertes Failover zu Azure und Failback (Wiederherstellung) von Azure zu Hyper-V-Hostservern an Ihrem lokalen Standort
* Wiederherstellungspläne mit mehreren virtuellen Computern, sodass für mehrstufige Anwendungsworkloads ein gemeinsames Failover ausgeführt wird

## <a name="azure-prerequisites"></a>Voraussetzungen für Azure
* Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung.
* Sie benötigen ein Azure Storage-Konto, um replizierte Daten zu speichern. Für das Konto muss Georeplikation aktiviert sein. Es muss sich in der gleichen Region wie der Azure Site Recovery-Tresor befinden und dem gleichen Abonnement zugeordnet sein. [Weitere Informationen zu Azure Storage](../storage/storage-introduction.md) Beachten Sie, dass das Verschieben von Speicherkonten, die mit dem [neuen Azure-Portal](../storage/storage-create-storage-account.md) erstellt wurden, über Ressourcengruppen hinweg nicht unterstützt wird.
* Sie benötigen ein virtuelles Azure-Netzwerk, damit für virtuelle Azure-Maschinen eine Netzwerkverbindung besteht, wenn Sie ein Failover von Ihrem primären Standort durchführen.

## <a name="hyper-v-prerequisites"></a>Hyper-V-Voraussetzungen
* Am lokalen Quellstandort benötigen Sie einen oder mehrere Server mit **Windows Server 2012 R2** mit installierter Hyper-V-Rolle oder **Microsoft Hyper-V Server 2012 R2**. Für diesen Server sollten folgende Anforderungen erfüllt sein:
* Er enthält eine oder mehrere virtuelle Maschinen.
* Er muss entweder direkt oder über einen Proxy mit dem Internet verbunden sein.
* Es müssen die unter KB [2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977") beschriebenen Fixes ausgeführt werden.

## <a name="virtual-machine-prerequisites"></a>Voraussetzungen für virtuelle Computer
Die zu schützenden virtuellen Computer müssen die [Anforderungen für virtuelle Azure-Computer](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements)erfüllen.

## <a name="provider-and-agent-prerequisites"></a>Voraussetzungen für Anbieter und Agent
Im Rahmen der Azure Site Recovery-Bereitstellung werden auf jedem Hyper-V-Server der Azure Site Recovery-Anbieter und der Azure Recovery Services-Agent installiert. Beachten Sie Folgendes:

* Es wird empfohlen, immer die aktuellen Versionen des Anbieters und des Agents auszuführen Diese sind im Site Recovery-Portal verfügbar.
* Alle Hyper-V-Server in einem Tresor sollten über die gleichen Versionen des Anbieters und Agents verfügen.
* Der Anbieter, der auf dem Server ausgeführt wird, stellt die Verbindung mit Site Recovery über das Internet her. Sie können dies ohne Proxy, mit den derzeit auf dem Hyper-V-Server konfigurierten Proxyeinstellungen oder mit benutzerdefinierten Proxyeinstellungen durchführen, die Sie während der Installation des Anbieters konfigurieren. Sie müssen sicherstellen, dass der Proxyserver, den Sie verwenden möchten, Zugriff auf die URLs für die Verbindungsherstellung mit Azure hat:

  * *.accesscontrol.windows.net
  * *.backup.windowsazure.com
  * *.hypervrecoverymanager.windowsazure.com
  * *.store.core.windows.net      
  * *.blob.core.windows.net
  - https://www.msftncsi.com/ncsi.txt
  - time.windows.com
  - time.nist.gov
* Lassen Sie außerdem die unter [IP-Bereiche des Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) angegebenen IP-Adressen sowie das HTTPS-Protokoll (443) zu. Sie müssen die IP-Adressbereiche der zu verwendenden Azure-Region sowie die IP-Adressbereiche der westlichen USA erlauben.

Diese Grafik zeigt die verschiedenen Kommunikationskanäle und Ports, die Site Recovery für die Orchestrierung und Replikation verwendet.

![B2A-Topology](./media/site-recovery-hyper-v-site-to-azure-classic/b2a-topology.png)

## <a name="step-1-create-a-vault"></a>Schritt 1: Erstellen eines Tresors
1. Melden Sie sich im [Verwaltungsportal](https://portal.azure.com)an.
2. Erweitern Sie **Data Services** > **Recovery Services**, und klicken Sie auf **Site Recovery-Tresor**.
3. Klicken Sie auf **Neu erstellen** > **Schnellerfassung**.
4. Geben Sie unter **Name**einen Anzeigenamen ein, über den der Tresor identifiziert wird.
5. Wählen Sie unter **Region**die geografische Region für den Tresor aus. Eine Liste mit den unterstützten Regionen finden Sie in den [Preisdetails zu Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/)unter „Geografische Verfügbarkeit“.
6. Klicken Sie auf **Tresor erstellen**.

    ![Neuer Tresor](./media/site-recovery-hyper-v-site-to-azure-classic/vault.png)

Überprüfen Sie auf der Statusleiste, ob der Tresor erfolgreich erstellt wurde. Der Tresor wird auf der Hauptseite von Recovery Services als **aktiv** aufgelistet.

## <a name="step-2-create-a-hyper-v-site"></a>Schritt 2: Erstellen eines Hyper-V-Standorts
1. Klicken Sie auf der Seite Recovery Services auf den Tresor, um die Seite "Schnellstart" zu öffnen. Schnellstart kann auch jederzeit über das Symbol geöffnet werden.

    ![Schnellstart](./media/site-recovery-hyper-v-site-to-azure-classic/quick-start-icon.png)
2. Wählen Sie in der Dropdownliste den Eintrag **Zwischen einem lokalen Hyper-V-Standort und Azure**aus.

    ![Hyper-V-Standortszenario](./media/site-recovery-hyper-v-site-to-azure-classic/select-scenario.png)
3. Klicken Sie unter **Hyper-V-Standort erstellen** auf **Hyper-V-Standort erstellen**. Geben Sie einen Standortnamen ein, und speichern Sie ihn.

    ![Hyper-V-Standort](./media/site-recovery-hyper-v-site-to-azure-classic/create-site.png)

## <a name="step-3-install-the-provider-and-agent"></a>Schritt 3: Installieren des Anbieters und des Agents
Installieren Sie den Anbieter und den Agent auf allen Hyper-V-Servern, die zu schützende VMs enthalten.

Wenn Sie die Installation für einen Hyper-V-Cluster vornehmen, führen Sie die Schritte 5 bis 11 auf jedem Knoten des Failoverclusters durch. Wenn alle Knoten registriert sind und der Schutz aktiviert wurde, sind die virtuellen Computer sogar bei der knotenübergreifenden Migration innerhalb des Clusters geschützt.

1. Klicken Sie unter **Hyper-V-Server vorbereiten** auf **Registrierungsschlüssel herunterladen**.
2. Klicken Sie auf der Seite **Registrierungsschlüssel herunterladen** neben dem Standort auf **Herunterladen**. Laden Sie den Schlüssel an einen sicheren Speicherort herunter, auf den der Hyper-V-Server einfach zugreifen kann. Der Schlüssel ist nach der Erstellung 5 Tage lang gültig.

    ![Registrierungsschlüssel](./media/site-recovery-hyper-v-site-to-azure-classic/download-key.png)
3. Klicken Sie auf **Anbieter herunterladen** , um die neueste Version herunterzuladen.
4. Führen Sie die Datei auf allen Hyper-V-Servern aus, die Sie im Tresor registrieren möchten. Die Datei installiert zwei Komponenten:
   * **Azure Site Recovery-Anbieter**: Diese Komponente ist zuständig für die Kommunikation und Orchestrierung zwischen Hyper-V-Server und Azure Site Recovery-Portal.
   * **Azure Recovery Services-Agent**: Diese Komponente ist zuständig für den Datentransport zwischen virtuellen Computern auf dem Hyper-V-Quellserver und Azure-Speicher.
5. Unter **Microsoft Update** können Sie Updates abonnieren. Mit dieser Einstellung werden Anbieter- und Agent-Updates gemäß Ihrer Microsoft Update-Richtlinie installiert.

    ![Microsoft Updates](./media/site-recovery-hyper-v-site-to-azure-classic/provider1.png)
6. Geben Sie unter **Installation** an, wo der Anbieter und der Agent auf dem Hyper-V-Server installiert werden sollen.

    ![Installationspfad](./media/site-recovery-hyper-v-site-to-azure-classic/provider2.png)
7. Fahren Sie nach der Installation mit der Einrichtung fort, um den Server im Tresor zu registrieren.
8. Klicken Sie auf der Seite **Tresoreinstellungen** auf **Durchsuchen**, um die Schlüsseldatei auszuwählen. Geben Sie das Azure Site Recovery-Abonnement, den Tresornamen und den Hyper-V-Standort an, zu dem der Hyper-V-Server gehört.

    ![Serverregistrierung](./media/site-recovery-hyper-v-site-to-azure-classic/provider8.PNG)
9. Geben Sie auf der Seite **Internetverbindung** an, wie die Verbindung zwischen Anbieter und Azure Site Recovery hergestellt werden soll. Wählen Sie **Proxyeinstellungen des Systems verwenden** aus, um die Standard-Internetverbindungseinstellungen auf dem Server zu verwenden. Wenn Sie keinen Wert angeben, werden die Standardeinstellungen verwendet.

   ![Interneteinstellungen](./media/site-recovery-hyper-v-site-to-azure-classic/provider7.PNG)
10. Die Registrierung des Servers im Tresor beginnt.

    ![Serverregistrierung](./media/site-recovery-hyper-v-site-to-azure-classic/provider15.PNG)
11. Nach der Registrierung ruft Azure Site Recovery Metadaten vom Hyper-V-Server ab, und der Server wird im Tresor auf der Registerkarte **Hyper-V-Standorte** der Seite **Server** angezeigt.

### <a name="install-the-provider-from-the-command-line"></a>Installieren des Anbieters über die Befehlszeile
Alternativ können Sie den Azure Site Recovery-Anbieter auch über die Befehlszeile installieren. Verwenden Sie dieses Verfahren, wenn Sie den Anbieter auf einem Computer mit Windows Server Core 2012 R2 installieren möchten. Führen Sie über die Befehlszeile Folgendes aus:

1. Laden Sie die Installationsdatei und den Registrierungsschlüssel des Anbieters in einen Ordner herunter. Beispiel: C:\ASR.
2. Führen Sie eine Eingabeaufforderung als Administrator aus, und geben Sie Folgendes ein:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Führen Sie anschließend den folgenden Befehl aus, um den Anbieter zu installieren:

        C:\ASR> setupdr.exe /i
4. Führen Sie Folgendes aus, um die Registrierung abzuschließen:

        CD C:\Program Files\Microsoft Azure Site Recovery Provider
        C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>         

Parameter:

* **/Credentials**: Geben Sie den Speicherort des heruntergeladenen Registrierungsschlüssels an.
* **/FriendlyName**: Geben Sie einen Namen zur Identifizierung des Hyper-V-Hostservers an. Dieser Name wird im Portal angezeigt.
* **/EncryptionEnabled**: Optional. Geben Sie an, ob Sie virtuelle Replikatmaschinen in Azure verschlüsseln möchten (Verschlüsselung im ruhenden Zustand).
* **/proxyAddress**; **/proxyport**; **/proxyUsername**; **/proxyPassword**: Optional. Geben Sie Proxyparameter ein, wenn Sie einen benutzerdefinierten Proxyserver verwenden möchten oder falls für den vorhandenen Proxy eine Authentifizierung erforderlich ist.

## <a name="step-4-create-an-azure-storage-account"></a>Schritt 4: Erstellen eines Azure-Speicherkontos
* Wählen Sie unter **Ressourcen vorbereiten** die Option **Speicherkonto erstellen** aus, um ein Azure-Speicherkonto zu erstellen, sofern noch nicht vorhanden. Für das Konto muss Georeplikation aktiviert sein. Es muss sich in der gleichen Region befinden wie der Azure Site Recovery-Tresor und dem gleichen Abonnement zugeordnet sein.

    ![Speicherkonto erstellen](./media/site-recovery-hyper-v-site-to-azure-classic/create-resources.png)

> [!NOTE]
> 1. Eine Verschiebung von mit dem [neuen Azure-Portal](../storage/storage-create-storage-account.md) erstellten Speicherkonten über Ressourcengruppen hinweg wird nicht unterstützt.
> 2. [Die Migration von Speicherkonten](../azure-resource-manager/resource-group-move-resources.md) zwischen Ressourcengruppen im gleichen Abonnement oder zwischen verschiedenen Abonnements wird für Speicherkonten nicht unterstützt, die für die Site Recovery-Bereitstellung verwendet werden.
>

## <a name="step-5-create-and-configure-protection-groups"></a>Schritt 5: Erstellen und Konfigurieren von Schutzgruppen
Schutzgruppen sind logische Gruppen virtueller Computer, für die die gleichen Schutzeinstellungen gelten sollen. Sie weisen einer Schutzgruppe Schutzeinstellungen zu, die für alle virtuellen Computer gelten, die Sie der Gruppe hinzufügen.

1. Klicken Sie unter **Schutzgruppen erstellen und konfigurieren** auf **Schutzgruppe erstellen**. Falls nicht alle Voraussetzungen erfüllt sind, erscheint eine entsprechende Meldung. Klicken Sie auf **Details anzeigen**, um weitere Informationen zu erhalten.
2. Fügen Sie auf der Registerkarte **Schutzgruppen** eine neue Schutzgruppe hinzu. Geben Sie einen Namen, den Hyper-V-Quellstandort, das Ziel in **Azure**, den Namen Ihres Azure Site Recovery-Abonnements und das Azure-Speicherkonto an.

    ![Schutzgruppe](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group.png)
3. Legen Sie unter **Replikationseinstellungen** die **Kopierhäufigkeit** fest, um anzugeben, wie oft geänderte Daten zwischen Quelle und Ziel synchronisiert werden sollen. Verfügbare Optionen sind 30 Sekunden, fünf Minuten und 15 Minuten.
4. Geben Sie unter **Wiederherstellungspunkte beibehalten für** an, für wie viele Stunden der Wiederherstellungsverlauf gespeichert werden soll.
5. Unter **Häufigkeit von anwendungskonsistenten Momentaufnahmen** können Sie angeben, ob Momentaufnahmen erstellt werden sollen, bei denen mithilfe des Volumeschattenkopie-Diensts (Volume Shadow Copy Service, VSS) sichergestellt wird, dass sich die Anwendungen bei der Erstellung der Momentaufnahme in einem konsistenten Zustand befinden. Standardmäßig werden solche Momentaufnahmen nicht erstellt. Stellen Sie sicher, dass der Wert geringer ist als die Anzahl der konfigurierten zusätzlichen Wiederherstellungspunkte. Dies wird nur für virtuelle Computer mit einem Windows-Betriebssystem unterstützt.
6. Geben Sie im Feld für die **Startzeit der ersten Replikation** an, wann die erste Replikation der virtuellen Computer in der Schutzgruppe an Azure gesendet werden soll.

    ![Schutzgruppe](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group2.png)

## <a name="step-6-enable-virtual-machine-protection"></a>Schritt 6: Aktivieren des Schutzes für virtuelle Computer
Fügen Sie einer Schutzgruppe virtuelle Computer hinzu, um sie zu schützen.

> [!NOTE]
> Das Schützen von VMs, auf denen Linux mit einer statischen IP-Adresse ausgeführt wird, wird nicht unterstützt.
>
>

1. Klicken Sie auf der Registerkarte **Computer** der Schutzgruppe auf **Virtuelle Computer zum Aktivieren von Schutz zu Schutzgruppen hinzufügen**.
2. Wählen Sie auf der Seite **Schutz für virtuelle Computer aktivieren** die virtuellen Computer aus, die Sie schützen möchten.

    ![Schutz für virtuelle Computer aktivieren](./media/site-recovery-hyper-v-site-to-azure-classic/add-vm.png)

    Der Auftrag Schutz aktivieren wird ausgeführt. Der Fortschritt kann auf der Registerkarte **Aufträge** nachverfolgt werden. Nachdem der Auftrag zum Abschließen des Schutzes ausgeführt wurde, ist der virtuelle Computer bereit für das Failover.
3. Nach der Einrichtung des Schutzes haben Sie folgende Möglichkeiten:

   * Anzeigen virtueller Computer unter **Geschützte Elemente** > **Schutzgruppen** > *Name der Schutzgruppe* > **Virtuelle Computer**. Auf der Registerkarte **Eigenschaften** können Sie Detailinformationen zu den Computern anzeigen.
   * Konfigurieren der Failovereigenschaften für einen virtuellen Computer unter **Geschützte Elemente** > **Schutzgruppen** > *Name der Schutzgruppe* > **Virtuelle Computer** *Name des virtuellen Computers* > **Konfigurieren**. Sie können Folgendes konfigurieren:

     * **Name**: Der Name des virtuellen Computers in Azure.
     * **Größe**: Die Zielgröße des virtuellen Computers, für den ein Failover durchgeführt wird.

       ![Konfigurieren der Eigenschaften virtueller Computer](./media/site-recovery-hyper-v-site-to-azure-classic/vm-properties.png)
   * Konfigurieren weiterer Einstellungen für den virtuellen Computer unter *Geschützte Elemente** > **Schutzgruppen** > *Name der Schutzgruppe* > **Virtuelle Computer** *Name des virtuellen Computers* > **Konfigurieren**. Hierzu zählen:

     * **Netzwerkadapter**: Die Anzahl der Netzwerkadapter hängt von der Größe ab, die Sie für den virtuellen Zielcomputer angeben. Überprüfen Sie in den [Spezifikationen für virtuelle Computer](../virtual-machines/linux/sizes.md) , wie viele Netzwerkschnittstellenkarten von virtuellen Maschinen einer bestimmten Größe unterstützt werden.

       Wenn Sie die Größe für einen virtuellen Computer ändern und die Einstellungen speichern, wird die Anzahl der Netzwerkadapter beim nächsten Öffnen der Seite **Konfigurieren** geändert. Die Anzahl der Netzwerkadapter der virtuellen Zielcomputer entspricht mindestens der Anzahl der Netzwerkadapter auf virtuellen Quellcomputern und der maximalen Anzahl der Netzwerkadapter, die nach der unterstützten Größe des virtuellen Computers ausgewählt werden. Hierzu eine kurze Erläuterung:

       * Wenn die Anzahl der Netzwerkadapter des Quellcomputers maximal der Anzahl der Netzwerkadapter entspricht, die für die Größe des Zielcomputers zulässig ist, hat der Zielcomputer die gleiche Anzahl von Netzwerkadaptern wie der Quellcomputer.
       * Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputer die maximal zulässige Anzahl für die Größe des Zielcomputers übersteigt, wird die Anzahl verwendet, die maximal für die Größe des Zielcomputers zulässig ist.
       * Ein Beispiel: Wenn ein Quellcomputer zwei Netzwerkkarten besitzt und der Zielcomputer aufgrund seiner Größe vier Netzwerkkarten unterstützt, erhält der Zielcomputer zwei Netzwerkkarten. Wenn der Quellcomputer dagegen zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe nur einen Adapter unterstützt, erhält der Zielcomputer nur einen Adapter.

     * **Azure-Netzwerk**: Geben Sie das Failover-Zielnetzwerk für den virtuellen Computer an. Wenn der virtuelle Computer mehrere Netzwerkadapter besitzt, müssen alle Adapter mit dem gleichen Azure-Netzwerk verbunden werden.
     * **Subnetz** : Wählen Sie für jeden Netzwerkadapter des virtuellen Computers das Subnetz im Azure-Netzwerk aus, mit dem der Computer nach einem Failover eine Verbindung herstellen soll.
     * **Ziel-IP-Adresse**: Wenn der Netzwerkadapter der virtuellen Quellmaschine für die Verwendung einer statischen IP-Adresse konfiguriert ist, können Sie die IP-Adresse für die virtuelle Zielmaschine angeben, um sicherzustellen, dass die Maschine nach dem Failover die gleiche IP-Adresse besitzt.  Wenn Sie keine IP-Adresse angeben, wird beim Failover eine der verfügbaren Adressen zugewiesen. Wenn Sie eine Adresse angeben, die bereits verwendet wird, ist das Failover nicht erfolgreich.

     > [!NOTE]
     > Für Netzwerke, die für die Site Recovery-Bereitstellung verwendet werden, wird die [Migration von Netzwerken](../azure-resource-manager/resource-group-move-resources.md) zwischen Ressourcengruppen im selben Abonnement oder zwischen verschiedenen Abonnements nicht unterstützt.
     >

     ![Konfigurieren der Eigenschaften virtueller Computer](./media/site-recovery-hyper-v-site-to-azure-classic/multiple-nic.png)




## <a name="step-7-create-a-recovery-plan"></a>Schritt 7: Erstellen eines Wiederherstellungsplans
Zum Testen der Bereitstellung können Sie ein Test-Failover für einen einzelnen virtuellen Computer durchführen. Alternativ können Sie einen Wiederherstellungsplan erstellen, der mehrere virtuelle Computer enthält. Lesen Sie die [weiteren Informationen](site-recovery-create-recovery-plans.md) zur Erstellung eines Wiederherstellungsplans.

## <a name="step-8-test-the-deployment"></a>Schritt 8: Testen der Bereitstellung
Es gibt zwei Möglichkeiten, ein Testfailover in Azure auszuführen.

* **Testfailover ohne Azure-Netzwerk**: Bei dieser Art von Testfailover wird überprüft, ob die virtuelle Maschine in Azure richtig hochgefahren wird. Der virtuelle Computer wird nach einem Failover mit keinem Azure-Netzwerk verbunden.
* **Testfailover mit Azure-Netzwerk**: Bei dieser Art von Failover wird überprüft, ob die gesamte Replikationsumgebung wie erwartet hochgefahren wird und die betreffenden virtuellen Maschinen mit dem angegebenen Azure-Zielnetzwerk verbunden werden. Beachten Sie Folgendes: Beim Testfailover wird das Subnetz der virtuellen Testmaschine basierend auf dem Subnetz der virtuellen Replikatmaschine festgelegt. Dies unterscheidet sich von der regulären Replikation, bei der das Subnetz des virtuellen Replikatcomputers auf dem Subnetz des virtuellen Quellcomputers basiert.

Wenn Sie ein Testfailover durchführen möchten, ohne ein Azure-Netzwerk anzugeben, müssen Sie keine Vorbereitungen treffen.

Für ein Testfailover mit einem Azure-Zielnetzwerk müssen Sie ein neues Azure-Netzwerk erstellen, das von Ihrem Azure-Produktionsnetzwerk isoliert ist (Standardverhalten bei der Erstellung eines neuen Netzwerks in Azure). Unter [Ausführen eines Testfailovers](site-recovery-failover.md) finden Sie weitere Details.

Um Ihre Replikation und Netzwerkbereitstellung vollständig zu testen, müssen Sie die Infrastruktur einrichten, damit die replizierte virtuelle Maschine wie erwartet funktioniert. Eine Möglichkeit ist: Richten Sie eine virtuelle Maschine als Domänencontroller mit DNS ein, und replizieren Sie sie in Azure. Verwenden Sie Site Recovery für die Erstellung im Testnetzwerk, indem Sie ein Testfailover ausführen.  Weitere Informationen zu den Überlegungen zum Testfailover für Active Directory finden Sie [hier](site-recovery-active-directory.md#test-failover-considerations).

Führen Sie das Testfailover wie folgt aus:

> [!NOTE]
> Um beim Durchführen eines Failovers in Azure die bestmögliche Leistung zu erzielen, sollten Sie sicherstellen, dass Sie den Azure-Agent auf dem geschützten Computer installiert haben. Dies fördert das schnellere Starten und ist hilfreich für die Diagnose bei Problemen. Den Linux-Agent finden Sie [hier](https://github.com/Azure/WALinuxAgent), den Windows-Agent [hier](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

1. Wählen Sie auf der Registerkarte **Wiederherstellungspläne** den gewünschten Wiederherstellungsplan aus, und klicken Sie auf **Testfailover**.
2. Wählen Sie auf der Seite **Testfailover bestätigen** den Eintrag **Kein** oder ein bestimmtes Azure-Netzwerk aus.  Beachten Sie, dass ein Testfailover bei Auswahl von **Kein** zwar prüft, ob der virtuelle Computer ordnungsgemäß zu Azure repliziert wurde, die Konfiguration des Replikationsnetzwerks wird jedoch nicht geprüft.

    ![Testfailover](./media/site-recovery-hyper-v-site-to-azure-classic/test-nonetwork.png)
3. Auf der Registerkarte **Aufträge** können Sie den Fortschritt des Failovers verfolgen. Außerdem wird Ihnen das Testreplikat des virtuellen Computers im Azure-Portal angezeigt. Wenn Sie den Zugriff auf virtuelle Computer aus Ihrem lokalen Netzwerk eingerichtet haben, können Sie eine Remotedesktopverbindung mit dem virtuellen Computer herstellen.
4. Klicken Sie auf **Test abschließen**, wenn das Failover die Phase **Tests abschließen** erreicht, um das Testfailover abzuschließen. Auf der Registerkarte **Auftrag** können Sie den Failoverfortschritt und -status detailliert nachverfolgen und etwaige erforderliche Aufgaben ausführen.
5. Nach dem Failover können Sie das Testreplikat des virtuellen Computers im Azure-Portal sehen. Wenn Sie den Zugriff auf virtuelle Computer aus Ihrem lokalen Netzwerk eingerichtet haben, können Sie eine Remotedesktopverbindung mit dem virtuellen Computer herstellen.

   1. Prüfen Sie, ob die virtuellen Computer erfolgreich starten.
   2. Wenn Sie nach dem Failover eine Verbindung mit dem virtuellen Computer in Azure über Remote Desktop herstellen möchten, aktivieren Sie die Remote Desktop-Verbindung auf dem virtuellen Computer, bevor Sie das Test-Failover ausführen. Außerdem müssen Sie dem virtuellen Computer einen RDP-Endpunkt hinzufügen. Hierzu können Sie ein [Azure Automation-Runbook](site-recovery-runbook-automation.md) verwenden.
   3. Falls Sie nach dem Failover über eine öffentliche IP-Adresse eine Remotedesktopverbindung mit dem virtuellen Computer in Azure herstellen, achten Sie darauf, dass keine Domänenrichtlinien vorhanden sind, die dies verhindern.
6. Gehen Sie nach dem Test wie folgt vor:

   * Klicken Sie auf **Das Testfailover ist abgeschlossen**. Bereinigen Sie die Testumgebung, um die virtuellen Testcomputer automatisch auszuschalten und zu löschen.
   * Klicken Sie auf **Notizen** , um alle Beobachtungen im Zusammenhang mit dem Test-Failover aufzuzeichnen und zu speichern.
7. Wenn das Failover die Phase **Test abschließen** erreicht, schließen Sie die Überprüfung wie folgt ab:
   1. Zeigen Sie den virtuellen Replikatcomputer im Azure-Portal an. Prüfen Sie, ob der virtuelle Computer erfolgreich startet.
   2. Wenn Sie den Zugriff auf virtuelle Computer aus Ihrem lokalen Netzwerk eingerichtet haben, können Sie eine Remotedesktopverbindung mit dem virtuellen Computer herstellen.
   3. Klicken Sie auf **Test abschließen** , um den Test abzuschließen.
   4. Klicken Sie auf **Notizen** , um alle Beobachtungen im Zusammenhang mit dem Test-Failover aufzuzeichnen und zu speichern.
   5. Klicken Sie auf **Das Testfailover ist abgeschlossen**. Bereinigen Sie die Testumgebung, um den virtuellen Testcomputer automatisch auszuschalten und ihn zu löschen.

## <a name="next-steps"></a>Nächste Schritte
Wenn die Bereitstellung eingerichtet ist und ausgeführt wird, informieren Sie sich über [Failover](site-recovery-failover.md) .

