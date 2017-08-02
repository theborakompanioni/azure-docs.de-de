---
title: "Erneutes Schützen von Azure zu einem lokalen Standort | Microsoft-Dokumentation"
description: "Nach einem Failover von virtuellen Computern auf Azure können Sie die virtuellen Computer mittels Failback wieder in die lokale Umgebung übertragen. Erfahren Sie, wie Sie vor einem Failback den Schutz erneut vornehmen."
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
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.translationtype: HT
ms.sourcegitcommit: 49bc337dac9d3372da188afc3fa7dff8e907c905
ms.openlocfilehash: 9687b8342723239d1ab07bcaf59176f4a0911215
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="reprotect-from-azure-to-an-on-premises-site"></a>Erneutes Schützen von Azure zu einem lokalen Standort



## <a name="overview"></a>Übersicht
In diesem Artikel wird beschrieben, wie Sie virtuelle Azure-Computer aus Azure an einem lokalen Standort erneut schützen. Befolgen Sie die Anweisungen in diesem Artikel, wenn alles bereit ist, um Ihre virtuellen VMware-Computer oder Ihre physischen Windows-/Linux-Server nach einem Failover vom lokalen Standort zu Azure per Failback wieder an den lokalen Standort zurückzuführen (wie unter [Replizieren von virtuellen VMware-Computern und physischen Servern in Azure mithilfe von Azure Site Recovery](site-recovery-failover.md) beschrieben).

> [!WARNING]
> Sie können kein Failback ausführen, nachdem Sie die [Migration abgeschlossen](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), den virtuellen Computer in eine andere Ressourcengruppe verschoben oder den virtuellen Azure-Computer gelöscht haben.


Wenn der Schutz wiederhergestellt wurde und die geschützten virtuellen Computer repliziert werden, können Sie ein Failback für die virtuellen Computer initiieren, um sie an den lokalen Standort zu übertragen.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

Sehen Sie sich das folgende Video zum Failover von Azure zu einem lokalen Standort an, um eine kurze Übersicht zu erhalten.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="prerequisites"></a>Voraussetzungen
Unternehmen oder erwägen Sie die folgenden erforderlichen Aktionen, wenn Sie den erneuten Schutz virtueller Computer vorbereiten:

* Wenn die virtuellen Computer, für die Sie das Failback durchführen möchten, von einem vCenter-Server verwaltet werden, stellen Sie sicher, dass auf den vCenter-Servern die [erforderlichen Berechtigungen](site-recovery-vmware-to-azure-classic.md) für die Ermittlung von virtuellen Computern vorhanden sind.

  > [!WARNING]
  > Wenn auf dem lokalen Masterziel oder lokalen virtuellen Computer Momentaufnahmen vorhanden sind, tritt beim Aktivieren des erneuten Schutzes ein Fehler auf. Sie können vor dem Fortfahren mit dem erneuten Schützen die Momentaufnahmen aus dem Hauptziel löschen. Die Momentaufnahmen auf dem virtuellen Computer werden während des Auftrags zum erneuten Schützen automatisch zusammengeführt.

* Erstellen Sie vor dem Ausführen des Failbacks zwei zusätzliche Komponenten:

  * **Prozessserver**: Der [Prozessserver](site-recovery-vmware-setup-azure-ps-resource-manager.md) empfängt Daten vom geschützten virtuellen Computer in Azure und sendet Daten an den lokalen Standort. Zwischen dem Prozessserver und dem geschützten virtuellen Computer ist ein Netzwerk mit geringer Wartezeit erforderlich. Sie können also einen lokalen Prozessserver einsetzen, wenn Sie eine Azure ExpressRoute-Verbindung verwenden, oder einen Azure-basierten Prozessserver und ein VPN.
  
  * **Masterzielserver**: Der Masterzielserver empfängt Failbackdaten. Auf dem von Ihnen erstellten lokalen Verwaltungsserver ist standardmäßig ein Masterzielserver installiert. Je nach Datenverkehrsvolumen beim Failback müssen Sie jedoch u.U. einen separaten Masterzielserver für das Failback erstellen.
    * [Für einen virtuellen Linux-Computer wird ein Linux-Masterzielserver benötigt](site-recovery-how-to-install-linux-master-target.md).
    * Für einen virtuellen Windows-Computer wird ein Windows-Masterzielserver benötigt. Sie können den lokalen Prozessserver und die Masterzielcomputer wiederverwenden.

    Für das Masterziel gelten andere Voraussetzungen, die unter [Allgemeine Überprüfungen nach der Installation des Masterziels](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server) aufgeführt sind.

* Ein Konfigurationsserver ist lokal erforderlich, wenn Sie ein Failback durchführen. Während des Failbacks muss der virtuelle Computer in der Konfigurationsserverdatenbank vorhanden sein. Andernfalls ist das Failback nicht erfolgreich. Stellen Sie sicher, dass Sie die regelmäßigen geplanten Sicherungen des Servers durchführen. Stellen Sie im Notfall den Server mit der gleichen IP-Adresse wieder her, damit das Failback funktioniert.

* Legen Sie die Einstellung `disk.EnableUUID=true` in den Konfigurationsparametern des virtuellen Masterzielcomputers in VMware fest. Wenn diese Zeile nicht vorhanden ist, fügen Sie sie hinzu. Diese Einstellung ist erforderlich, um für den Datenträger des virtuellen Computers (VMDK) eine einheitliche UUID festzulegen, damit er richtig bereitgestellt wird.

* *Es ist nicht möglich, Storage vMotion auf dem Masterzielserver zu verwenden*. Dadurch kann ein Fehler beim Failback auftreten. Der virtuelle Computer kann nicht gestartet werden, weil die Datenträger nicht für ihn verfügbar sind. Um dieses Problem zu verhindern, schließen Sie die Masterzielserver aus der vMotion-Liste aus.

* Sie müssen dem Masterzielserver ein neues Laufwerk hinzufügen: ein Aufbewahrungslaufwerk. Fügen Sie einen neuen Datenträger hinzu, und formatieren Sie das Laufwerk.


### <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

#### <a name="why-do-i-need-a-s2s-vpn-or-an-expressroute-connection-to-replicate-data-back-to-the-on-premises-site"></a>Warum wird eine S2S-VPN- oder ExpressRoute-Verbindung benötigt, um Daten wieder am lokalen Standort zu replizieren?
Die Replikation von lokal nach Azure kann über das Internet oder über eine ExpressRoute-Verbindung mit öffentlichem Peering erfolgen, für das erneute Schützen und das Failback muss jedoch ein S2S-VPN (Site-to-Site) zum Replizieren von Daten vorhanden sein. Stellen Sie das Netzwerk bereit, damit die virtuellen Computer in Azure, für die ein Failover durchgeführt wurde, den lokalen Konfigurationsserver erreichen können (per Ping). Sie können auch einen Prozessserver im Azure-Netzwerk des virtuellen Computers bereitstellen, für den das Failover durchgeführt wurde. Dieser Prozessserver sollte außerdem mit dem lokalen Konfigurationsserver kommunizieren können.

#### <a name="when-should-i-install-a-process-server-in-azure"></a>Wann sollte in Azure ein Prozessserver installiert werden?


Die virtuellen Computer in Azure, die sie erneut schützen möchten, senden Replikationsdaten an einen Prozessserver. Richten Sie das Netzwerk so ein, dass die virtuellen Computer unter Azure den Prozessserver erreichen können.

Sie können einen Prozessserver in Azure bereitstellen oder den vorhandenen Prozessserver verwenden, den Sie auch während des Failovers verwendet haben. Berücksichtigen Sie unbedingt die Wartezeit, die beim Senden der Daten von den virtuellen Computern in Azure an den Prozessserver auftritt.

Wenn Sie eine ExpressRoute-Verbindung eingerichtet haben, können Sie einen lokalen Prozessserver zum Senden von Daten verwenden, da die Wartezeit zwischen dem virtuellen Computer und dem Prozessserver gering ist.

 ![Architekturdiagramm für ExpressRoute](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)



Wenn Sie dagegen nur über ein S2S-VPN verfügen, empfiehlt es sich, den Prozessserver in Azure bereitzustellen.

 ![Architekturdiagramm für VPN](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)


Beachten Sie, dass die Replikation nur über das S2S-VPN oder über das private Peering Ihres ExpressRoute-Netzwerks erfolgt. Vergewissern Sie sich, dass für diesen Netzwerkkanal genügend Bandbreite zur Verfügung steht.

Informationen zum Installieren eines Azure-basierten Prozessservers finden Sie unter [Verwalten eines in Azure ausgeführten Prozessservers ](site-recovery-vmware-setup-azure-ps-resource-manager.md).

> [!TIP]
> Es empfiehlt sich, beim Failback einen Azure-basierten Prozessserver zu verwenden. Die Replikationsleistung ist höher, wenn der Prozessserver sich näher beim replizierenden virtuellen Computer (dem Computer in Azure, für den das Failover durchgeführt wurde) befindet. Während des Proof of Concept (POC) oder bei Demos können Sie jedoch den lokalen Prozessserver zusammen mit ExpressRoute mit privatem Peering verwenden, um den POC schneller abzuschließen.



#### <a name="what-ports-should-i-open-on-different-components-so-that-reprotection-can-work"></a>Welche Ports müssen auf den verschiedenen Komponenten geöffnet sein, damit das erneute Schützen funktioniert?

![Ports für Failover und Failback](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

#### <a name="which-master-target-server-should-i-use-for-reprotection"></a>Welchen Masterzielserver sollte ich zum erneuten Schützen verwenden?
Ein lokaler Masterzielserver muss vorhanden sein, um Daten vom Prozessserver empfangen und in die VMDK-Datei des lokalen virtuellen Computers schreiben zu können. Wenn Sie virtuelle Windows-Computer schützen möchten, benötigen Sie einen Windows-Masterzielserver. Sie können den lokalen Prozessserver und das Masterziel wiederverwenden<!-- !todo component -->. Für virtuelle Linux-Computer müssen Sie ein zusätzliches lokales Linux-Masterziel einrichten.


Informationen zum Installieren eines Masterzielserver finden Sie unter:

* [Installieren eines Windows-Masterzielservers](site-recovery-vmware-to-azure.md)
* [Installieren eines Linux-Masterzielservers](site-recovery-how-to-install-linux-master-target.md)


#### <a name="what-datastore-types-are-supported-on-the-on-premises-esxi-host-during-failback"></a>Welche Datenspeichertypen werden auf dem lokalen ESXi-Host bei einem Failback unterstützt?

Zurzeit unterstützt Azure Site Recovery nur Failbacks auf einen VMFS-Datenspeicher (Dateisystem für virtuelle Computer). Ein vSAN- oder NFS-Datenspeicher wird nicht unterstützt. Aufgrund dieser Einschränkung ist bei NFS-Datenspeichern das Auswahleingabefeld auf der Anzeige für den erneuten Schutz leer, oder es wird der vSAN-Datenspeicher angezeigt, der Auftrag schlägt dann aber fehl. Wenn Sie ein Failback durchführen möchten, können Sie einen VMFS-Datenspeicher lokal erstellen und diesen für das Failback verwenden. Dieses Failback verursacht den vollständigen VMDK-Download.

### <a name="common-things-to-check-after-completing-installation-of-the-master-target-server"></a>Allgemeine Überprüfungen nach der Installation des Masterzielservers

* Wenn sich der virtuelle Computer lokal auf dem vCenter-Server befindet, muss der Masterzielserver auf das VMDK des lokalen virtuellen Computers zugreifen können. Der Zugriff ist erforderlich, um die replizierten Daten auf die Datenträger des virtuellen Computers schreiben zu können. Stellen Sie sicher, dass der Datenspeicher des lokalen virtuellen Computers auf dem Host des Masterziels mit Lese-/Schreibzugriff bereitgestellt wird.

* Wenn der virtuelle Computer nicht lokal auf dem vCenter-Server vorhanden ist, muss der Site Recovery-Dienst beim erneuten Schützen einen neuen virtuellen Computer erstellen. Dieser virtuelle Computer wird auf dem ESX-Host erstellt, auf dem Sie das Masterziel erstellen. Wählen Sie den ESX-Host mit Bedacht, damit der virtuelle Failbackcomputer auf dem gewünschten Host erstellt wird.

* *Es ist nicht möglich, Storage vMotion für den Masterzielserver zu verwenden*. Dadurch kann ein Fehler beim Failback auftreten. Der virtuelle Computer kann nicht gestartet werden, weil die Datenträger nicht für ihn verfügbar sind.

  > [!WARNING]
  > Wenn für ein Masterziel nach dem erneuten Schützen eine Storage vMotion-Aufgabe durchgeführt wird, werden die geschützten VM-Datenträger, die an das Masterziel angefügt sind, zu dem Ziel der vMotion-Aufgabe migriert. Wenn Sie anschließend versuchen, ein Failback durchzuführen, schlägt die Trennung der Datenträger fehl, da die Datenträger nicht gefunden werden. Es wird dann schwierig, die Datenträger in den Speicherkonten zu finden. Sie müssen sie manuell suchen und an den virtuellen Computer anfügen. Anschließend kann der lokale virtuelle Computer neu gestartet werden.

* Fügen Sie dem vorhandenen Windows-Masterzielserver ein Aufbewahrungslaufwerk hinzu. Fügen Sie einen neuen Datenträger hinzu, und formatieren Sie das Laufwerk. Das Aufbewahrungslaufwerk wird verwendet, um die Zeitpunkte festzuhalten, zu denen der virtuelle Computer zurück an den lokalen Standort repliziert wird. Nachfolgend sind einige Kriterien eines Aufbewahrungslaufwerks aufgeführt. Ohne diese wird das Laufwerk nicht für den Masterzielserver aufgelistet.

   * Das Volume wird nicht für andere Zwecke genutzt, z.B. als Ziel der Replikation.

   * Das Volume ist nicht im Sperrmodus.

   * Das Volume ist kein Cachevolume. Die Masterzielserver-Installation darf auf diesem Volume nicht vorhanden sein. Das Volume für die benutzerdefinierte Prozessserver- und Masterzielserver-Installation kann für das Aufbewahrungsvolume nicht ausgewählt werden. Wenn der Prozessserver und Masterzielserver auf einem Volume installiert sind, ist das Volume ein Cachevolume des Masterziels.

   * Der Dateisystemtyp des Volumes ist nicht FAT oder FAT32.

   * Die Volumekapazität ist ungleich null.

   * Das Standardaufbewahrungsvolume für Windows ist das R-Volume.

   * Das Standardaufbewahrungsvolume für Linux ist „/mnt/retention“.

  > [!IMPORTANT]
  > Sie müssen ein neues Laufwerk hinzufügen, wenn Sie einen vorhandenen Prozessserver-/Konfigurationsservercomputer, eine Skalierungsgruppe oder einen Prozessserver-/Masterzielservercomputer verwenden. Das neue Laufwerk muss die oben genannten Anforderungen erfüllen. Wenn das Aufbewahrungslaufwerk nicht vorhanden ist, wird es in der Auswahl-Dropdownliste im Portal nicht angezeigt. Nach dem Hinzufügen eines Laufwerks zum lokalen Masterziel dauert es bis zu 15 Minuten, bis das Laufwerk in der Auswahl im Portal angezeigt wird. Sie können auch den Konfigurationsserver aktualisieren, wenn das Laufwerk nach 15 Minuten nicht angezeigt wird.

* Für einen virtuellen Linux-Computer, für den ein Failover durchgeführt wurde, ist ein Linux-Masterzielserver erforderlich. Für einen virtuellen Windows-Computer, für den ein Failover durchgeführt wurde, wird ein Windows-Masterzielserver benötigt.

* Installieren Sie VMware-Tools auf dem Masterzielserver. Ohne die VMware-Tools werden die Datenspeicher auf dem ESXi-Host des Masterziels nicht erkannt.

* Aktivieren Sie den Parameter `disk.EnableUUID=true` auf dem virtuellen Masterzielcomputer mithilfe der vCenter-Eigenschaften. <!-- !todo Needs link. -->

* An das Masterziel sollte mindestens ein VMFS-Datenspeicher angefügt sein. Wenn keiner vorhanden ist, ist die Eingabe **Datenspeicher** auf der Seite für das erneute Schützen leer, und Sie können den Vorgang nicht fortsetzen.

* Auf den Datenträgern des Masterzielservers können keine Momentaufnahmen enthalten sein. Wenn Momentaufnahmen vorhanden sind, schlagen das erneute Schützen und das Failback fehl.

* Das Masterziel kann nicht über einen Paravirtual-SCSI-Controller verfügen. Als Controller kann nur ein LSI Logic-Controller verwendet werden. Ohne LSI Logic-Controller schlägt das erneute Schützen fehl.

<!--
### Failback policy
To replicate back to on-premises, you will need a failback policy. This policy get automatically created when you create a forward direction policy. Note that

1. This policy gets auto associated with the configuration server during creation.
2. This policy is not editable.
3. The set values of the policy are (RPO Threshold = 15 Mins, Recovery Point Retention = 24 Hours, App Consistency Snapshot Frequency = 60 Mins)
   ![](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

-->

## <a name="steps-to-reprotect"></a>Schritte zum erneuten Schützen

> [!NOTE]
> Nachdem ein virtueller Computer in Azure gestartet wurde, dauert es eine Weile, bis der Agent auf dem Konfigurationsserver registriert wird (bis zu 15 Minuten). Während dieses Zeitraums schlägt das erneute Schützen fehl, und es wird eine Fehlermeldung mit dem Hinweis angezeigt, dass der Agent nicht installiert ist. Warten Sie einige Minuten, und versuchen Sie dann noch einmal, das erneute Schützen durchzuführen.



1. Klicken Sie unter **Tresor** > **Replizierte Elemente** mit der rechten Maustaste auf den virtuellen Computer, für den ein Failover durchgeführt wurde, und wählen Sie dann **Erneut schützen**. Sie können auch auf den Computer klicken und die Befehlsschaltfläche **Erneut schützen** wählen.
2. Auf dem Blatt können Sie sehen, dass die Schutzrichtung **Azure auf lokal** bereits ausgewählt ist.
3. Wählen Sie unter **Masterzielserver** und **Prozessserver** den lokalen Masterzielserver und den Prozessserver aus.
4. Wählen Sie für **Datenspeicher** den Datenspeicher aus, in dem die Datenträger lokal wiederhergestellt werden sollen. Diese Option wird verwendet, wenn der lokale virtuelle Computer gelöscht wird und Sie neue Datenträger erstellen müssen. Diese Option wird ignoriert, wenn die Datenträger bereits vorhanden sind. Sie müssen aber trotzdem einen Wert angeben.
5. Wählen Sie das Aufbewahrungslaufwerk.
6. Die Failbackrichtlinie wird automatisch ausgewählt.
7. Klicken Sie auf **OK**, um das erneute Schützen zu starten. Ein Auftrag beginnt mit der Replikation des virtuellen Computers von Azure am lokalen Standort. Sie können den Fortschritt auf der Registerkarte **Aufträge** nachverfolgen.

Wenn die Wiederherstellung an einem anderen Ort erfolgen soll (falls der lokale virtuelle Computer gelöscht wurde), wählen Sie das für den Masterzielserver konfigurierte Aufbewahrungslaufwerk und den entsprechenden Datenspeicher aus. Beim Ausführen des Failbacks zum lokalen Standort verwenden die virtuellen VMware-Computer im Failback-Schutzplan den gleichen Datenspeicher wie der Masterzielserver. Anschließend wird in vCenter ein neuer virtueller Computer erstellt.

Wenn Sie den virtuellen Computer in Azure auf einem vorhandenen lokalen virtuellen Computer wiederherstellen möchten, stellen Sie die Datenspeicher des lokalen virtuellen Computers mit Lese-/Schreibzugriff für den ESXi-Host des Masterzielservers bereit.
    ![Dialogfeld „Erneut schützen“](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

Sie können das erneute Schützen auch auf der Ebene eines Wiederherstellungsplans durchführen. Eine Replikationsgruppe kann nur mithilfe eines Wiederherstellungsplans erneut geschützt werden. Sie müssen beim erneuten Schützen mithilfe eines Wiederherstellungsplans für jeden geschützten Computer die Werte angeben.

> [!NOTE]
> Verwenden Sie zum erneuten Schützen einer Replikationsgruppe denselben Masterzielserver. Wenn Sie für das erneute Schützen einer Replikationsgruppe einen anderen Masterzielserver verwenden, kann der Server keinen gemeinsamen Zeitpunkt angeben.

> [!NOTE]
> Der lokale virtuelle Computer wird während des erneuten Schützens deaktiviert. Dies hilft, die Konsistenz der Daten während der Replikation sicherzustellen. Schalten Sie den virtuellen Computer nach Abschluss des erneuten Schützens nicht ein.

Nach dem erfolgreichen erneuten Schützen befindet sich der virtuelle Computer in einem geschützten Zustand.

## <a name="next-steps"></a>Nächste Schritte

Wenn sich der virtuelle Computer in einem geschützten Zustand befindet, können Sie ein [Failback initiieren](site-recovery-how-to-failback-azure-to-vmware.md#steps-to-fail-back). 

Beim Failback wird der virtuelle Computer in Azure heruntergefahren und der lokale virtuelle Computer gestartet. Erwarten Sie eine Ausfallzeit für die Anwendung. Führen Sie das Failback zu einer Zeit aus, zu der dies für Ihre Anwendung kein Problem darstellt.

## <a name="common-problems"></a>Häufiger auftretende Probleme

* Wenn Sie zum Erstellen Ihrer virtuellen Computer eine Vorlage verwendet haben, sollten Sie sicherstellen, dass jeder virtuelle Computer über eine eigene UUID für die Datenträger verfügt. Falls für die UUID des lokalen virtuellen Computers ein Konflikt mit der ID des Masterziels besteht (weil beide mit der gleichen Vorlage erstellt wurden), schlägt das erneute Schützen fehl. Stellen Sie ein anderes Masterziel bereit, das nicht mit der gleichen Vorlage erstellt wurde.

* Wenn Sie die vCenter-Ermittlung schreibgeschützter Benutzer ausführen und virtuelle Computer schützen, ist das Schützen erfolgreich, und das Failover funktioniert. Während des erneuten Schützens schlägt dies fehl, da die Datenspeicher nicht ermittelt werden können. Ein Symptom dafür ist, dass die Datenspeicher während des erneuten Schützens nicht aufgelistet werden. Zum Beheben dieses Problems können Sie die vCenter-Anmeldeinformationen mit einem entsprechenden Konto aktualisieren, das über die erforderlichen Berechtigungen verfügt, und den Auftrag wiederholen. Weitere Informationen finden Sie unter [Replizieren von virtuellen VMware-Computern und physischen Servern in Azure mithilfe von Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).

* Wenn Sie ein Failback für einen virtuellen Linux-Computer durchführen und ihn lokal ausführen, sehen Sie, dass das Netzwerk-Manager-Paket auf dem Computer deinstalliert wurde. Der Grund für die Deinstallation ist, dass beim Wiederherstellen des virtuellen Computers in Azure das Netzwerk-Manager-Paket entfernt wird.

* Wenn ein virtueller Linux-Computer mit einer statischen IP-Adresse konfiguriert ist und ein Failover zu Azure ausgeführt wird, wird die IP-Adresse über DHCP abgerufen. Nach dem Failover zum lokalen Standort verwendet der virtuelle Computer weiterhin DHCP zum Abrufen der IP-Adresse. Melden Sie sich ggf. manuell am Computer an, und setzen Sie die IP-Adresse auf die statische Adresse zurück. Ein virtueller Windows-Computer kann seine statische IP-Adresse wiederbeschaffen.

* Wenn Sie die kostenlose Edition ESXi 5.5 oder die kostenlose Edition vSphere 6 Hypervisor verwenden, ist das Failover erfolgreich, aber das Failback nicht. Sie müssen auf die Evaluierungslizenz eines der Programme aktualisieren, um das Failback zu aktivieren.

* Wenn der Konfigurationsserver auf dem Prozessserver nicht erreichbar ist, können Sie Telnet verwenden, um die Konnektivität mit dem Konfigurationsserver an Port 443 zu überprüfen. Sie können auch versuchen, den Konfigurationsserver auf dem Prozessserver per Ping zu erreichen. Ein Prozessserver sollte außerdem einen Takt aufweisen, wenn er mit dem Konfigurationsserver verbunden ist.

* Wenn Sie versuchen, ein Failback auf einen alternativen vCenter-Computer durchzuführen, stellen Sie sicher, dass Ihr neuer vCenter-Computer und der Masterzielserver erkannt werden. Als typisches Symptom sind die Datenspeicher im Dialogfeld **Erneut schützen** nicht zugänglich oder sichtbar.

* Für einen Server vom Typ Windows Server 2008 R2 SP1, der als physischer lokaler Server geschützt wird, kann kein Failback von Azure zu einem lokalen Standort durchgeführt werden.

