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
ms.date: 12/15/2016
ms.author: ruturajd
translationtype: Human Translation
ms.sourcegitcommit: e650439b4bfd468c4bd7b236b80ce7361de1c6ac
ms.openlocfilehash: 972a3e88d15be656fd8cd3d51b7e507c7cbb49d5


---
# <a name="reprotect-from-azure-to-on-premises"></a>Erneutes Schützen von Azure zu lokal

## <a name="overview"></a>Übersicht
In diesem Artikel erfahren Sie, wie Sie den Schutz virtueller Azure-Computer von Azure am lokalen Standort wiederherstellen. Befolgen Sie die Anweisungen in diesem Artikel, wenn Sie bereit sind, Ihre virtuellen VMware-Computer oder Ihre physischen Windows-/Linux-Server nach einem Failover vom lokalen Standort auf Azure (beschrieben in diesem [Tutorial](site-recovery-vmware-to-azure-classic.md)) wieder per Failback auf den lokalen Standort zurückzuführen.

Wenn der Schutz wiederhergestellt wurde und die geschützten virtuellen Computer repliziert werden, können Sie ein Failback für die virtuellen Computer initiieren, um sie wieder in die lokale Umgebung zu übertragen.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

## <a name="pre-requisites"></a>Voraussetzungen
Im Anschluss finden Sie einige Schritte und Aspekte, die bei der Vorbereitung des erneuten Schützens ausgeführt bzw. berücksichtigt werden müssen.

### <a name="reprotect-needs-a-s2s-vpn-or-an-express-route-to-replicate-data-back-to-on-premises"></a>Für erneutes Schützen wird eine S2S-VPN- oder eine ExpressRoute-Verbindung benötigt, um Daten wieder an die lokale Umgebung zu replizieren.
Wenn die Replikation von lokal nach Azure über das Internet oder über eine ExpressRoute-Verbindung mit öffentlichem Peering erfolgen kann, muss für das erneute Schützen und das Failback ein S2S-VPN zum Replizieren von Daten eingerichtet werden. Das Netzwerk muss so bereitgestellt werden, dass die virtuellen Computer in Azure, für die ein Failover durchgeführt wurde, den lokalen Konfigurationsserver erreichen können. Sie können im Azure-Netzwerk des virtuellen Computers, für den ein Failover durchgeführt wurde, auch einen Prozessserver bereitstellen. Dieser muss dann ebenfalls mit dem lokalen Konfigurationsserver kommunizieren können.

### <a name="process-server-is-needed-to-replicate-the-data-from-source-vms-to-on-premises"></a>Zum Replizieren der Daten von virtuellen Quellcomputern an die lokale Umgebung wird ein Prozessserver benötigt.
<!-- Read more about a process server here.!todo -->

Die virtuellen Azure-Computer, die Sie erneut schützen möchten, senden die Replikationsdaten an einen Prozessserver. Ihr Netzwerk muss so eingerichtet sein, dass der Prozessserver über den virtuellen Azure-Computer erreichbar ist.

Sie können einen Prozessserver in Azure bereitstellen oder den vorhandenen Prozessserver verwenden, den Sie auch während des Failovers verwendet haben. Berücksichtigen Sie unbedingt die Latenz, die beim Senden der Daten von virtuellen Azure-Computern an den Prozessserver auftritt. 

* Wenn Sie über eine ExpressRoute-Verbindung verfügen, können Sie die Daten unter Verwendung eines lokalen Prozessservers senden. In diesem Fall ist die Latenz zwischen virtuellem Computer und Prozessserver gering.
    
    ![Architekturdiagramm für ExpressRoute](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)



* Wenn Sie dagegen nur über ein S2S-VPN verfügen, empfiehlt es sich, den Prozessserver in Azure bereitzustellen.

    ![Architekturdiagramm für VPN](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)


Beachten Sie, dass die Replikation nur über ein S2S-VPN oder über das private Peering Ihres ExpressRoute-Netzwerks erfolgt. Vergewissern Sie sich, dass für diesen Netzwerkkanal genügend Bandbreite zur Verfügung steht.

### <a name="ports-to-be-opened-on-different-machines-so-that-all-the-components-can-communicate"></a>Ports, die auf verschiedenen Computern geöffnet sein müssen, damit alle Komponenten kommunizieren können

![Failover-Failback alle Ports](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

### <a name="master-target-needs-to-be-available-on-premises-to-receive-data-from-process-server"></a>Das Masterziel muss lokal verfügbar sein, um Daten vom Prozessserver empfangen zu können.
Ein Masterzielserver muss lokal vorhanden sein, um die Daten vom Prozessserver empfangen und in die VMDK-Datei des lokalen virtuellen Computers schreiben zu können. Wenn Sie virtuelle Windows-Computer schützen, benötigen Sie einen Windows-Masterzielserver und können den lokalen Prozessserver und das Masterziel wiederverwenden <!-- !todo component -->. Bei virtuellen Linux-Computern muss ein zusätzliches lokales Linux-Masterziel eingerichtet werden.

Unter den folgenden Links finden Sie jeweils eine ausführliche Anleitung zum Installieren eines Masterzielservers:

* [Installieren eines Windows-Masterzielservers](site-recovery-vmware-to-azure.md#run-site-recovery-unified-setup)
* [Installieren eines Linux-Masterzielservers](site-recovery-how-to-install-linux-master-target.md)


#### <a name="common-things-to-check-after-completing-installation-of-master-target"></a>Allgemeine Überprüfungen nach der Installation des Masterziels

* Wenn der virtuelle Computer lokal auf dem vCenter-Server vorhanden ist, benötigt der Masterzielserver Zugriff auf die VMDK-Datei des lokalen virtuellen Computers. Dies ist erforderlich, um die replizierten Daten auf die Datenträger des virtuellen Computers schreiben zu können. Hierzu muss sichergestellt sein, dass **der Datenspeicher des lokalen virtuellen Computers mit Lese-/Schreibzugriff in den Masterzielhost eingebunden ist**.

* Ist der virtuelle Computer nicht lokal auf dem vCenter-Server vorhanden, muss im Zuge des erneuten Schützens ein neuer virtueller Computer erstellt werden. Dieser virtuelle Computer wird auf dem ESX-Host erstellt, auf dem Sie das Masterziel erstellen. Wählen Sie den ESX-Host daher mit Bedacht, damit der virtuelle Failback-Computer auf dem gewünschten Host erstellt wird.
    
* **Für den Masterzielserver kann kein speicherbezogener vMotion-Vorgang ausgeführt werden**. Dadurch kann ein Fehler beim Failback auftreten. Der virtuelle Computer wird nicht gestartet, da die Datenträger nicht für ihn verfügbar gemacht werden.

* Dem vorhandenen Masterzielserver muss ein neues Laufwerk hinzugefügt werden. Dieses Laufwerk wird als Aufbewahrungslaufwerk bezeichnet. Fügen Sie einen neuen Datenträger hinzu, und formatieren Sie das Laufwerk. Das Aufbewahrungslaufwerk wird verwendet, um die Zeitpunkte festzuhalten, wenn von der VM zurück auf den lokalen Standort repliziert wird. Einige der Kriterien eines Aufbewahrungslaufwerks sind nachstehend beschrieben. Ohne diese wird das Laufwerk nicht für den Masterzielserver aufgelistet.
   
   a. Das Volume darf nicht für andere Zwecke (Ziel für die Replikation usw.) verwendet werden.

   b. Das Volume darf sich nicht im Sperrmodus befinden.

   c. Bei dem Volume darf es sich nicht um ein Cachevolume handeln. (Die Masterzielserver-Installation darf auf diesem Volume nicht vorhanden sein. Das Volume für die benutzerdefinierte Prozessserver- und Masterzielserver-Installation kann für das Aufbewahrungsvolume nicht ausgewählt werden. Das hier installierte Prozessserver- und Masterzielserver-Volume ist ein Cachevolume des Masterzielservers.)

   d. Der Dateisystemtyp für das Volume darf nicht FAT oder FAT32 lauten.

   e. Die Volumekapazität muss größer als&0; sein.

   e. Das Standardaufbewahrungsvolume für Windows ist das R-Volume.

   f. Das Standardaufbewahrungsvolume für Linux ist „/mnt/retention“.

* Ein virtueller Linux-Computer, für den ein Failover ausgeführt wurde, benötigt einen Linux-Masterzielserver. Ein virtueller Windows-Computer, für den ein Failover ausgeführt wurde, benötigt einen Windows-Masterzielserver.

* Installieren Sie VMware-Tools auf dem Masterzielserver. Ohne die VMware-Tools werden die Datenspeicher auf dem ESXi-Host des Masterziels nicht erkannt.

* Legen Sie den Parameter „disk.EnableUUID“ für den virtuellen Masterzielcomputer über die vCenter-Eigenschaften auf „True“ fest, um ihn zu aktivieren. <!-- !todo Needs link. -->



### <a name="failback-policy"></a>Failbackrichtlinie
Sie werden eine Failbackrichtlinie benötigen, um zurück auf den lokalen Standort zu replizieren. Diese Richtlinie wird automatisch erstellt, wenn Sie eine vorwärts gerichtete Richtlinie erstellen. Beachten Sie Folgendes:

1. Diese Richtlinie wird während der Erstellung automatisch dem Konfigurationsserver zugeordnet.
2. Diese Richtlinie kann nicht bearbeitet werden.
3. Sie ist auf folgende Werte festgelegt: RPO-Schwellenwert = 15 Min., Aufbewahrungszeitraum des Wiederherstellungspunkts = 24 Stunden, Momentaufnahmehäufigkeit für Anwendungskonsistenz = 60 Min. ![](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)



## <a name="steps-to-reprotect"></a>Schritte zum erneuten Schützen

1. Wählen Sie im Tresor unter den replizierten Elementen den virtuellen Computer aus, für den ein Failover ausgeführt wurde, und klicken Sie mit der rechten Maustaste, um **Erneut schützen** auszuwählen. Sie können auch auf den Computer klicken und „reprotect“ (Erneut schützen) aus den Befehlsschaltflächen auswählen.
2. Auf dem Blatt können Sie sehen, dass die Schutzrichtung „Azure auf lokal“ bereits ausgewählt ist.
3. Wählen Sie unter **Masterzielserver** und **Prozessserver** den lokalen Masterzielserver und den Prozessserver aus.
4. Wählen Sie den **Datenspeicher** aus, in dem die Datenträger lokal wiederhergestellt werden sollen. Diese Option wird verwendet, wenn der lokale virtuelle Computer gelöscht wird und neue Datenträger erstellt werden müssen. Diese Option wird ignoriert, wenn die Datenträger bereits vorhanden sind. Sie müssen jedoch immer noch einen Wert angeben.
5. Wählen Sie das Aufbewahrungslaufwerk. 
6. Die Failbackrichtlinie wird automatisch ausgewählt.
7. Nachdem Sie auf **OK** geklickt haben, um den erneuten Schutz zu starten, beginnt ein Auftrag mit der Replikation des virtuellen Computers von Azure zum lokalen Standort. Sie können den Fortschritt auf der Registerkarte **Aufträge** nachverfolgen.

Wenn die Wiederherstellung an einem anderen Ort erfolgen soll (falls der lokale virtuelle Computer gelöscht wurde), wählen Sie das für den Masterzielserver konfigurierte Aufbewahrungslaufwerk und den entsprechenden Datenspeicher aus. Beim Ausführen des Failbacks auf den lokalen Standort verwenden die virtuellen VMware-Computer im Failback-Schutzplan den gleichen Datenspeicher wie der Masterzielserver, und in vCenter wird ein neuer virtueller Computer erstellt. Wenn Sie den virtuellen Azure-Computer unter einem vorhandenen lokalen virtuellen Computer wiederherstellen möchten, müssen die Datenspeicher des lokalen virtuellen Computers mit Lese-/Schreibzugriff auf dem ESXi-Host des Masterzielservers eingebunden sein.
    ![](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

Sie können den erneuten Schutz auch auf einer Wiederherstellungsplanebene ausführen. Wenn Sie über eine Replikationsgruppe verfügen, können Sie diese nur mithilfe eines Wiederherstellungsplans erneut schützen. Beim erneuten Schützen über einen Wiederherstellungsplan müssen Sie die obigen Werte für jeden geschützten Computer angeben.

> [!NOTE]
> Eine Replikationsgruppe muss mithilfe desselben Masterzielservers erneut geschützt werden. Replikationsgruppen können nicht gleichzeitig erneut geschützt werden, wenn Sie dafür verschiedene Masterzielserver verwenden.
> 
> 

Nach erfolgreicher Wiederherstellung des Schutzes befindet sich der virtuelle Computer wieder in einem geschützten Zustand.

## <a name="next-steps"></a>Nächste Schritte

Sobald sich der virtuelle Computer in einem geschützten Zustand befindet, können Sie ein Failback initiieren. Durch das Failback wird der virtuelle Computer in Azure heruntergefahren und lokal gestartet. Dadurch ergibt sich eine kurze Ausfallzeit für die Anwendung. Führen Sie das Failback daher zu einer Zeit aus, zu der dies kein Problem darstellt.

[Schritte zum Initiieren eines Failbacks des virtuellen Computers](site-recovery-how-to-failback-v2a.md#steps-to-failback)

## <a name="common-issues"></a>Häufige Probleme 



<!--HONumber=Feb17_HO2-->


