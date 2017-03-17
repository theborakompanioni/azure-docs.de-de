---
title: "Erneutes Schützen von Azure zu lokal | Microsoft-Dokumentation"
description: "Nach einem Failover von virtuellen Computern auf Azure können Sie die virtuellen Computer mittels Failback wieder in die lokale Umgebung übertragen. Hier erfahren Sie, wie Sie vor einem Failback den Schutz wiederherstellen."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 02/13/2017
ms.author: ruturajd
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: f2da9c4b02f0bb8a93347c05be358516f39e2df0
ms.lasthandoff: 03/09/2017


---
# <a name="reprotect-from-azure-to-on-premises"></a>Erneutes Schützen von Azure zu lokal

## <a name="overview"></a>Übersicht
In diesem Artikel erfahren Sie, wie Sie den Schutz virtueller Azure-Computer von Azure am lokalen Standort wiederherstellen. Befolgen Sie die Anweisungen in diesem Artikel, wenn Sie bereit sind, Ihre virtuellen VMware-Computer oder Ihre physischen Windows-/Linux-Server nach einem Failover vom lokalen Standort auf Azure (beschrieben in diesem [Tutorial](site-recovery-failover.md)) wieder per Failback auf den lokalen Standort zurückzuführen.

Wenn der Schutz wiederhergestellt wurde und die geschützten virtuellen Computer repliziert werden, können Sie ein Failback für die virtuellen Computer initiieren, um sie wieder in die lokale Umgebung zu übertragen.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

Sie können sich auch dieses Video ansehen, um einen kurzen Überblick zu erhalten.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]

Lesen Sie sich die hier angegebenen Informationen zum gesamten Failbackprozess durch.

## <a name="pre-requisites"></a>Voraussetzungen
Im Anschluss finden Sie einige Schritte und Aspekte, die bei der Vorbereitung des erneuten Schützens ausgeführt bzw. berücksichtigt werden müssen.

* Wenn die virtuellen Computer, für die Sie das Failback ausführen möchten, von einem vCenter-Server verwaltet werden, müssen Sie sicherstellen, dass auf den vCenter-Servern die erforderlichen Berechtigungen für die Ermittlung von virtuellen Computern (VMs) vorhanden sind. [Weitere Informationen](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
* Wenn auf einem lokalen virtuellen Computer Momentaufnahmen vorhanden sind, tritt beim Ausführen des erneuten Schutzes ein Fehler auf. Sie können die Momentaufnahmen vor dem Fortfahren mit dem erneuten Schutz löschen.
* Vor dem Ausführen des Failbacks müssen Sie zwei zusätzliche Komponenten erstellen:
  * **Erstellen eines Prozessservers**. Der Prozessserver wird zum Empfangen der Daten vom geschützten virtuellen Computer in Azure und zum Senden der lokalen Daten verwendet. Dies erfordert ein Netzwerk mit geringer Latenz zwischen dem Prozessserver und dem geschützten virtuellen Computer. Daher kann sich der Prozessserver lokal (bei Verwendung einer ExpressRoute-Verbindung) oder in Azure befinden, wenn Sie ein VPN verwenden.
  * **Erstellen eines Masterzielservers:** Der Masterzielserver empfängt Failbackdaten. Auf dem von Ihnen lokal erstellten Verwaltungsserver ist standardmäßig ein Masterzielserver installiert. Je nach Datenverkehrsvolumen beim Failback müssen Sie jedoch u. U. einen separaten Masterzielserver für das Failback erstellen.
        * [Ein virtueller Linux-Computer benötigt einen Linux-Masterzielserver](site-recovery-how-to-install-linux-master-target.md).
        * Ein virtueller Windows-Computer benötigt ein Windows-Masterziel. Sie können den lokalen PS+MT-Computer verwenden.
* Der Konfigurationsserver ist lokal erforderlich, wenn Sie ein Failback durchführen. Während des Failbacks muss der virtuelle Computer in der Konfigurationsserverdatenbank vorhanden sein, andernfalls ist das Failback nicht erfolgreich. Daher stellen Sie sicher, dass Sie die regelmäßigen geplanten Sicherungen des Servers ausführen. Im Notfall müssen Sie ihn mit der gleichen IP-Adresse wiederherstellen, damit das Failback funktioniert.
* Stellen Sie sicher, dass Sie die Einstellung „disk.enableUUID=true“ in den Konfigurationsparametern der Masterziel-VM in VMware festlegen. Wenn diese Zeile nicht vorhanden ist, fügen Sie sie hinzu. Dies ist erforderlich, um eine konsistente UUID der VMDK-Datei bereitzustellen, damit sie ordnungsgemäß bereitgestellt wird.
* **Für den Masterzielserver kann kein speicherbezogener vMotion-Vorgang ausgeführt werden**. Dadurch kann ein Fehler beim Failback auftreten. Der virtuelle Computer wird nicht gestartet, da die Datenträger nicht für ihn verfügbar gemacht werden.
* Sie benötigen ein neues Laufwerk, das zum Masterzielserver hinzugefügt wurde. Dieses Laufwerk wird als Aufbewahrungslaufwerk bezeichnet. Fügen Sie einen neuen Datenträger hinzu, und formatieren Sie das Laufwerk.
* Für das Masterziel gelten andere Voraussetzungen, die unter [Allgemeine Überprüfungen nach der Installation des Masterziels](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-master-target) aufgeführt sind.


### <a name="why-do-i-need-a-s2s-vpn-or-an-expressroute-to-replicate-data-back-to-on-premises"></a>Warum wird eine S2S-VPN- oder ExpressRoute-Verbindung benötigt, um Daten wieder in der lokalen Umgebung zu replizieren?
Wenn die Replikation von lokal nach Azure über das Internet oder über eine ExpressRoute-Verbindung mit öffentlichem Peering erfolgen kann, muss für das erneute Schützen und das Failback ein S2S-VPN zum Replizieren von Daten eingerichtet werden. **Das Netzwerk muss so bereitgestellt werden, dass die virtuellen Computer in Azure, für die ein Failover durchgeführt wurde, den lokalen Konfigurationsserver erreichen können (per Ping)**. Sie können im Azure-Netzwerk des virtuellen Computers, für den ein Failover durchgeführt wurde, auch einen Prozessserver bereitstellen. Dieser muss dann ebenfalls mit dem lokalen Konfigurationsserver kommunizieren können.

### <a name="when-should-i-install-a-process-server-in-azure"></a>Wann sollte in Azure ein Prozessserver installiert werden?


Die virtuellen Azure-Computer, die Sie erneut schützen möchten, senden die Replikationsdaten an einen Prozessserver. Ihr Netzwerk muss so eingerichtet sein, dass der Prozessserver über den virtuellen Azure-Computer erreichbar ist.

Sie können einen Prozessserver in Azure bereitstellen oder den vorhandenen Prozessserver verwenden, den Sie auch während des Failovers verwendet haben. Berücksichtigen Sie unbedingt die Latenz, die beim Senden der Daten von virtuellen Azure-Computern an den Prozessserver auftritt.

* Wenn Sie über eine ExpressRoute-Verbindung verfügen, können Sie die Daten unter Verwendung eines lokalen Prozessservers senden. In diesem Fall ist die Latenz zwischen virtuellem Computer und Prozessserver gering.

    ![Architekturdiagramm für ExpressRoute](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)



* Wenn Sie dagegen nur über ein S2S-VPN verfügen, empfiehlt es sich, den Prozessserver in Azure bereitzustellen.

    ![Architekturdiagramm für VPN](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)


Beachten Sie, dass die Replikation nur über ein S2S-VPN oder über das private Peering Ihres ExpressRoute-Netzwerks erfolgt. Vergewissern Sie sich, dass für diesen Netzwerkkanal genügend Bandbreite zur Verfügung steht.

Weitere Informationen zur Installation eines Azure-Prozessservers finden Sie in [diesem Artikel](site-recovery-vmware-setup-azure-ps-resource-manager.md).

### <a name="what-are-the-different-ports-to-be-open-on-different-components-so-that-reprotect-can-work"></a>Welche einzelnen Ports müssen auf den verschiedenen Komponenten geöffnet sein, damit das erneute Schützen funktioniert?

![Failover-Failback alle Ports](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

### <a name="which-master-target-server-to-use-for-reprotect"></a>Welcher Masterzielserver sollte für das erneute Schützen verwendet werden?
Ein Masterzielserver muss lokal vorhanden sein, um die Daten vom Prozessserver empfangen und in die VMDK-Datei des lokalen virtuellen Computers schreiben zu können. Wenn Sie virtuelle Windows-Computer schützen, benötigen Sie einen Windows-Masterzielserver und können den lokalen Prozessserver und das Masterziel wiederverwenden <!-- !todo component -->. Bei virtuellen Linux-Computern muss ein zusätzliches lokales Linux-Masterziel eingerichtet werden.


Unter den folgenden Links finden Sie jeweils eine Anleitung zum Installieren eines Masterzielservers:

* [Installieren eines Windows-Masterzielservers](site-recovery-vmware-to-azure.md#run-site-recovery-unified-setup)
* [Installieren eines Linux-Masterzielservers](site-recovery-how-to-install-linux-master-target.md)


#### <a name="common-things-to-check-after-completing-installation-of-master-target"></a>Allgemeine Überprüfungen nach der Installation des Masterziels

* Wenn der virtuelle Computer lokal auf dem vCenter-Server vorhanden ist, benötigt der Masterzielserver Zugriff auf die VMDK-Datei des lokalen virtuellen Computers. Dies ist erforderlich, um die replizierten Daten auf die Datenträger des virtuellen Computers schreiben zu können. Hierzu muss sichergestellt sein, dass **der Datenspeicher des lokalen virtuellen Computers mit Lese-/Schreibzugriff in den Masterzielhost eingebunden ist**.

* Ist der virtuelle Computer nicht lokal auf dem vCenter-Server vorhanden, muss im Zuge des erneuten Schützens ein neuer virtueller Computer erstellt werden. Dieser virtuelle Computer wird auf dem ESX-Host erstellt, auf dem Sie das Masterziel erstellen. Wählen Sie den ESX-Host daher mit Bedacht, damit der virtuelle Failback-Computer auf dem gewünschten Host erstellt wird.

* **Für den Masterzielserver kann kein speicherbezogener vMotion-Vorgang ausgeführt werden**. Dadurch kann ein Fehler beim Failback auftreten. Der virtuelle Computer wird nicht gestartet, da die Datenträger nicht für ihn verfügbar gemacht werden.

* Dem vorhandenen Windows-Masterzielserver muss ein neues Laufwerk hinzugefügt werden. Dieses Laufwerk wird als Aufbewahrungslaufwerk bezeichnet. Fügen Sie einen neuen Datenträger hinzu, und formatieren Sie das Laufwerk. Das Aufbewahrungslaufwerk wird verwendet, um die Zeitpunkte festzuhalten, wenn von der VM zurück auf den lokalen Standort repliziert wird. Einige der Kriterien eines Aufbewahrungslaufwerks sind nachstehend beschrieben. Ohne diese wird das Laufwerk nicht für den Masterzielserver aufgelistet.

   * Das Volume darf nicht für andere Zwecke (Ziel für die Replikation usw.) verwendet werden.

   * Das Volume darf sich nicht im Sperrmodus befinden.

   * Bei dem Volume darf es sich nicht um ein Cachevolume handeln. (Die Masterzielserver-Installation darf auf diesem Volume nicht vorhanden sein. Das Volume für die benutzerdefinierte Prozessserver- und Masterzielserver-Installation kann für das Aufbewahrungsvolume nicht ausgewählt werden. Das hier installierte Prozessserver- und Masterzielserver-Volume ist ein Cachevolume des Masterzielservers.)

   * Der Dateisystemtyp für das Volume darf nicht FAT oder FAT32 lauten.

   * Die Volumekapazität muss größer als&0; sein.

   * Das Standardaufbewahrungsvolume für Windows ist das R-Volume.

   * Das Standardaufbewahrungsvolume für Linux ist „/mnt/retention“.

* Ein virtueller Linux-Computer, für den ein Failover ausgeführt wurde, benötigt einen Linux-Masterzielserver. Ein virtueller Windows-Computer, für den ein Failover ausgeführt wurde, benötigt einen Windows-Masterzielserver.

* Installieren Sie VMware-Tools auf dem Masterzielserver. Ohne die VMware-Tools werden die Datenspeicher auf dem ESXi-Host des Masterziels nicht erkannt.

* Legen Sie den Parameter „disk.EnableUUID“ für den virtuellen Masterzielcomputer über die vCenter-Eigenschaften auf „True“ fest, um ihn zu aktivieren. <!-- !todo Needs link. -->

* An das Masterziel sollte mindestens ein VMFS-Datenspeicher angefügt sein. Wenn keiner vorhanden ist, ist die Datenspeichereingabe auf der Seite für das erneute Schützen leer, und Sie können den Vorgang nicht fortsetzen.

* Auf den Datenträgern des Masterzielservers können keine Momentaufnahmen enthalten sein. Wenn Momentaufnahmen vorhanden sind, schlägt das erneute Schützen bzw. das Failback fehl.

* Das Masterziel kann nicht über einen Paravirtual-SCSI-Controller verfügen. Es kann nur ein LSI Logic-Controller verwendet werden. Ohne LSI Logic-Controller schlägt das erneute Schützen fehl.

<!--
### Failback policy
To replicate back to on-premises, you will need a failback policy. This policy get automatically created when you create a forward direction policy. Note that

1. This policy gets auto associated with the configuration server during creation.
2. This policy is not editable.
3. The set values of the policy are (RPO Threshold = 15 Mins, Recovery Point Retention = 24 Hours, App Consistency Snapshot Frequency = 60 Mins)
   ![](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

-->

## <a name="steps-to-reprotect"></a>Schritte zum erneuten Schützen

Stellen Sie vor dem erneuten Schützen sicher, dass Sie den [Prozessserver](site-recovery-vmware-setup-azure-ps-resource-manager.md) in Azure und das lokale Windows- bzw. [Linux-Masterziel](site-recovery-how-to-install-linux-master-target.md) installiert haben.

> [!NOTE]
> Nachdem eine VM in Azure gestartet wurde, dauert es eine Weile, bis der Agent auf dem Konfigurationsserver registriert wird (bis zu 15 Minuten). Während dieses Zeitraums schlägt das erneute Schützen fehl, und in der Fehlermeldung wird angegeben, dass der Agent nicht installiert ist. Warten Sie einige Minuten, und versuchen Sie dann noch einmal, das erneute Schützen durchzuführen.



1. Wählen Sie im Tresor unter den replizierten Elementen den virtuellen Computer aus, für den ein Failover ausgeführt wurde, und klicken Sie mit der rechten Maustaste, um **Erneut schützen** auszuwählen. Sie können auch auf den Computer klicken und „reprotect“ (Erneut schützen) aus den Befehlsschaltflächen auswählen.
2. Auf dem Blatt können Sie sehen, dass die Schutzrichtung „Azure auf lokal“ bereits ausgewählt ist.
3. Wählen Sie unter **Masterzielserver** und **Prozessserver** den lokalen Masterzielserver und den Prozessserver aus.
4. Wählen Sie den **Datenspeicher** aus, in dem die Datenträger lokal wiederhergestellt werden sollen. Diese Option wird verwendet, wenn der lokale virtuelle Computer gelöscht wird und neue Datenträger erstellt werden müssen. Diese Option wird ignoriert, wenn die Datenträger bereits vorhanden sind. Sie müssen aber trotzdem einen Wert angeben.
5. Wählen Sie das Aufbewahrungslaufwerk.
6. Die Failbackrichtlinie wird automatisch ausgewählt.
7. Nachdem Sie auf **OK** geklickt haben, um den erneuten Schutz zu starten, beginnt ein Auftrag mit der Replikation des virtuellen Computers von Azure zum lokalen Standort. Sie können den Fortschritt auf der Registerkarte **Aufträge** nachverfolgen.

Wenn die Wiederherstellung an einem anderen Ort erfolgen soll (falls der lokale virtuelle Computer gelöscht wurde), wählen Sie das für den Masterzielserver konfigurierte Aufbewahrungslaufwerk und den entsprechenden Datenspeicher aus. Beim Ausführen des Failbacks auf den lokalen Standort verwenden die virtuellen VMware-Computer im Failback-Schutzplan den gleichen Datenspeicher wie der Masterzielserver, und in vCenter wird ein neuer virtueller Computer erstellt.
Wenn Sie den virtuellen Azure-Computer unter einem vorhandenen lokalen virtuellen Computer wiederherstellen möchten, müssen die Datenspeicher des lokalen virtuellen Computers mit Lese-/Schreibzugriff auf dem ESXi-Host des Masterzielservers eingebunden sein.
    ![](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

Sie können den erneuten Schutz auch auf einer Wiederherstellungsplanebene ausführen. Wenn Sie über eine Replikationsgruppe verfügen, können Sie diese nur mithilfe eines Wiederherstellungsplans erneut schützen. Beim erneuten Schützen über einen Wiederherstellungsplan müssen Sie die obigen Werte für jeden geschützten Computer angeben.

> [!NOTE]
> Eine Replikationsgruppe muss mithilfe desselben Masterzielservers erneut geschützt werden. Replikationsgruppen können nicht gleichzeitig erneut geschützt werden, wenn Sie dafür verschiedene Masterzielserver verwenden.


Nach erfolgreicher Wiederherstellung des Schutzes befindet sich der virtuelle Computer wieder in einem geschützten Zustand.

## <a name="next-steps"></a>Nächste Schritte

Sobald sich der virtuelle Computer in einem geschützten Zustand befindet, können Sie ein Failback initiieren. Durch das Failback wird der virtuelle Computer in Azure heruntergefahren und lokal gestartet. Dadurch ergibt sich eine kurze Ausfallzeit für die Anwendung. Führen Sie das Failback daher zu einer Zeit aus, zu der dies kein Problem darstellt.

[Schritte zum Initiieren eines Failbacks des virtuellen Computers](site-recovery-how-to-failback-azure-to-vmware.md#steps-to-failback)

## <a name="common-issues"></a>Häufige Probleme

* Wenn Ihre virtuellen Computer mit einer Vorlage erstellt wurden, sollten Sie vor dem Schützen sicherstellen, dass jede VM über eine eindeutige UUID für die Datenträger verfügt. Wenn für die UUID der lokalen VM ein Konflikt mit der ID des Masterziels besteht (weil beide mit der gleichen Vorlage erstellt wurden), schlägt das erneute Schützen fehl. Sie müssen ein anderes Masterziel bereitstellen, das nicht mit der gleichen Vorlage erstellt wurde.
* Wenn Sie die vCenter-Ermittlung schreibgeschützter Benutzer ausführen und virtuelle Computer schützen, funktioniert das Failover. Während des erneuten Schützens schlägt dies fehl, da die Datenspeicher nicht ermittelt werden können. Das Symptom dafür ist, dass Sie die Datenspeicher beim während des erneuten Schützens nicht sehen. Zum Beheben dieses Problems können Sie die vCenter-Anmeldeinformationen mit einem entsprechenden Konto aktualisieren, das über die erforderlichen Berechtigungen verfügt, und den Auftrag wiederholen. Weitere Informationen finden Sie unter [Replizieren von virtuellen VMware-Computern und physischen Servern in Azure mithilfe von Azure Site Recovery](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
* Wenn Sie ein Failback für einen virtuellen Linux-Computer durchführen und ihn lokal ausführen, sehen Sie, dass das Netzwerk-Manager-Paket auf dem Computer deinstalliert wurde. Der Grund für die Deinstallation ist, dass beim Wiederherstellen des virtuellen Computers in Azure das Netzwerk-Manager-Paket entfernt wird.
* Wenn ein virtueller Linux-Computer mit einer statischen IP-Adresse konfiguriert ist und ein Failover zu Azure ausgeführt wird, wird die IP-Adresse über DHCP abgerufen. Nach dem Failover zurück zum lokalen Standort verwendet der virtuelle Computer weiterhin DHCP zum Abrufen der IP-Adresse. Melden Sie sich ggf. manuell am Computer an, und setzen Sie die IP-Adresse auf die statische Adresse zurück. Eine Windows-VM kann ihre statische IP wiederbeschaffen.
* Wenn Sie die kostenlose Edition ESXi 5.5 oder die kostenlose Edition vSphere 6 Hypervisor verwenden, wäre das Failover erfolgreich, das Failback jedoch nicht. Sie müssen auf die Evaluierungslizenz eines der Programme aktualisieren, um das Failback zu aktivieren.
* Wenn der Konfigurationsserver auf dem Prozessserver nicht erreichbar ist, können Sie Konnektivität mit dem Konfigurationsserver mit dem Telnet-Befehl an den Konfigurationsservercomputer an Port 443 überprüfen. Sie können auch versuchen, den Konfigurationsserver auf dem Prozessservercomputer zu pingen. Ein Prozessserver sollte auch einen Takt aufweisen, wenn er mit dem Konfigurationsserver verbunden ist.
* Wenn Sie versuchen, ein Failback auf einen alternativen vCenter-Computer durchzuführen, stellen Sie sicher, dass sowohl Ihr neuer vCenter-Computer als auch der Masterzielserver erkannt werden. Als typisches Symptom sind die Datenspeicher im Dialogfeld **Erneut schützen** nicht zugänglich oder sichtbar.
* Für einen als physischen lokalen Computer geschützten WS&2008; R2 SP1-Computer kann kein Failback von Azure auf lokale Standorte ausgeführt werden.

