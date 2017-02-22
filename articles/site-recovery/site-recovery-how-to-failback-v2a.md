---
title: Hot Failback von Azure auf lokale Instanz | Microsoft-Dokumentation
description: "Nach einem Failover von virtuellen Computern auf Azure können Sie die virtuellen Computer mittels Failback wieder in die lokale Umgebung übertragen. Schritte für ein Failback"
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
ms.sourcegitcommit: 1a6dfb60f5db8b842af27b4c88bc3e5927331622
ms.openlocfilehash: 20446aadbb7bc3d13c5a38de5c4040f5898d4f6b


---
# <a name="failback-from-azure-to-on-premises"></a>Failback von Azure auf lokale Instanz
In diesem Artikel wird beschrieben, wie Sie für virtuelle Azure-Computer ein Failback auf den lokalen Standort durchführen. Befolgen Sie die Anweisungen in diesem Artikel, wenn Sie bereit sind, Ihre virtuellen VMware-Computer oder Ihre physischen Windows-/Linux-Server nach einem Failover vom lokalen Standort auf Azure (beschrieben in diesem [Tutorial](site-recovery-vmware-to-azure-classic.md)) wieder per Failback auf den lokalen Standort zurückzuführen.

## <a name="overview"></a>Übersicht
Das Failback funktioniert wie folgt:

* Nach einem Failover zu Azure erfolgt die Rückführung zum lokalen Standort per Failback in mehreren Phasen:
  * **Phase 1**: Sie schützen die virtuellen Azure-Computer erneut, damit diese zurück auf die virtuellen VMware-Computer am lokalen Standort repliziert werden. Dafür müssen Sie zudem folgende Schritte ausführen: 
        1. Ein lokales Masterziel einrichten
        2. Einen Prozessserver einrichten
        3. Und dann den erneuten Schutz initiieren
  * **Phase 2**: Nach der Replikation Ihrer virtuellen Azure-Computer auf den lokalen Standort führen Sie ein Failover aus, um das Failback von Azure durchzuführen.
  * **Phase 3**: Nachdem Ihre Daten per Failback zurückgeführt wurden, schützen Sie die lokalen virtuellen Computer, auf die Sie das Failback durchgeführt haben, erneut, damit diese mit der Replikation nach Azure beginnen.

### <a name="failback-to-the-original-or-alternate-location"></a>Failback zum ursprünglichen oder zu einem anderen Standort

    
Wenn Sie ein Failover für einen virtuellen VMware-Computer durchgeführt haben, können Sie das Failback zum gleichen virtuellen Quellcomputer ausführen, sofern er lokal noch vorhanden ist. In diesem Szenario werden nur die geänderten Daten (Deltaänderungen) repliziert. Dieses Szenario wird als Wiederherstellung am ursprünglichen Speicherort bezeichnet.

* Wenn Sie ein Failback zum ursprünglichen virtuellen Computer durchführen, sind folgende Bedingungen erforderlich:
  * Wenn der virtuelle Computer von einem vCenter-Server verwaltet wird, muss der ESX-Host des Masterziels Zugriff auf den Datenspeicher des virtuellen Computers haben.
  * Wenn sich der virtuelle Computer auf einem ESX-Host befindet, aber nicht von vCenter verwaltet wird, muss sich die Festplatte des virtuellen Computers in einem Datenspeicher befinden, auf den der Host des Masterziels Zugriff hat.
  * Wenn sich Ihr virtueller Computer auf einem ESX-Host befindet und nicht vCenter verwendet, müssen Sie für den ESX-Host des Masterziels eine Ermittlung ausführen, bevor Sie den erneuten Schutz ausführen. Dasselbe gilt auch bei einem Failback für physische Server.

  
Wenn der lokale virtuelle Computer nicht vorhanden ist, bevor Sie den virtuellen Computer erneut schützen, wird der Vorgang als Wiederherstellung an einem alternativen Speicherort bezeichnet. Hier wird mit dem Workflow zum erneuten Schützen der lokale virtuelle Computer neu erstellt.

* Wenn Sie ein Failback an einem alternativen Speicherort des virtuellen Computers ausführen, erfolgt die Wiederherstellung auf demselben ESX-Host, auf dem der Masterzielserver bereitgestellt ist. Der zum Erstellen des Datenträgers verwendete Datenspeicher ist derselbe Datenspeicher, der beim erneuten Schützen des virtuellen Computers ausgewählt wurde.
* Sie können ein Failback nur auf einen VMFS-Datenspeicher ausführen. Bei vSAN oder RDM funktionieren das erneute Schützen und das Failback nicht.
* Das erneute Schützen umfasst eine große anfängliche Datenübertragung gefolgt von Deltaänderungen. Da der virtuelle Computer lokal nicht vorhanden ist, müssen die gesamten Daten zurück repliziert werden. Dieser erneute Schutz dauert außerdem länger als die Wiederherstellung am ursprünglichen Speicherort.

Für einen physischen Computer kann beim Failover auf Azure nur ein Failback als virtueller VMware-Computer (auch als P2A2V bezeichnet) ausgeführt werden. Dieser Flow fällt unter Wiederherstellung an einem alternativen Speicherort.

* Für einen geschützten Windows Server 2008 R2 SP1-Computer, für den ein Failover zu Azure ausgeführt wurde, kann kein Failback ausgeführt werden.
* Stellen Sie sicher, dass Sie mindestens einen Masterzielserver zusammen mit den erforderlichen ESX/ESXi-Hosts ermitteln, auf die Sie ein Failback ausführen müssen.


## <a name="pre-requisites"></a>Voraussetzungen

* Wenn die virtuellen Computer, für die Sie das Failback ausführen möchten, von einem vCenter-Server verwaltet werden, müssen Sie sicherstellen, dass auf den vCenter-Servern die erforderlichen Berechtigungen für die Ermittlung von virtuellen Computern (VMs) vorhanden sind. [Weitere Informationen](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
* Wenn auf einem lokalen virtuellen Computer Momentaufnahmen vorhanden sind, tritt beim Ausführen des erneuten Schutzes ein Fehler auf. Sie können die Momentaufnahmen vor dem Fortfahren mit dem erneuten Schutz löschen.
* Vor dem Ausführen des Failbacks müssen Sie zwei zusätzliche Komponenten erstellen:
  * **Erstellen eines Prozessservers**. Der Prozessserver wird zum Empfangen der Daten vom geschützten virtuellen Computer in Azure und zum Senden der lokalen Daten verwendet. Dies erfordert ein Netzwerk mit geringer Latenz zwischen dem Prozessserver und dem geschützten virtuellen Computer. Daher kann sich der Prozessserver lokal (bei Verwendung einer ExpressRoute-Verbindung) oder in Azure befinden, wenn Sie ein VPN verwenden.
  * **Erstellen eines Masterzielservers:** Der Masterzielserver empfängt Failbackdaten. Auf dem von Ihnen lokal erstellten Verwaltungsserver ist standardmäßig ein Masterzielserver installiert. Je nach Datenverkehrsvolumen beim Failback müssen Sie jedoch u. U. einen separaten Masterzielserver für das Failback erstellen. 
        * Ein virtueller Linux-Computer benötigt einen Linux-Masterzielserver. 
        * Ein virtueller Windows-Computer benötigt ein Windows-Masterziel.
* Der Konfigurationsserver ist lokal erforderlich, wenn Sie ein Failback durchführen. Während des Failbacks muss der virtuelle Computer in der Konfigurationsserverdatenbank vorhanden sein, andernfalls ist das Failback nicht erfolgreich. Daher stellen Sie sicher, dass Sie die regelmäßigen geplanten Sicherungen des Servers ausführen. Im Notfall müssen Sie ihn mit der gleichen IP-Adresse wiederherstellen, damit das Failback funktioniert.
* Stellen Sie sicher, dass Sie die Einstellung „disk.enableUUID=true“ in den Konfigurationsparametern der Masterziel-VM in VMware festlegen. Wenn diese Zeile nicht vorhanden ist, fügen Sie sie hinzu. Dies ist erforderlich, um eine konsistente UUID der VMDK-Datei bereitzustellen, damit sie ordnungsgemäß bereitgestellt wird.
* **Für den Masterzielserver kann kein speicherbezogener vMotion-Vorgang ausgeführt werden**. Dadurch kann ein Fehler beim Failback auftreten. Der virtuelle Computer wird nicht gestartet, da die Datenträger nicht für ihn verfügbar gemacht werden.
* Sie benötigen ein neues Laufwerk, das zum Masterzielserver hinzugefügt wurde. Dieses Laufwerk wird als Aufbewahrungslaufwerk bezeichnet. Fügen Sie einen neuen Datenträger hinzu, und formatieren Sie das Laufwerk.


## <a name="steps-to-failback"></a>Schritte zum Failback

Vor dem Initiieren des Failbacks **muss sichergestellt sein, dass der erneute Schutz für die virtuellen Computer abgeschlossen ist**. Die virtuellen Computer müssen geschützt sein und eine intakte Integrität aufweisen. Weitere Informationen zum erneuten Schutz der virtuellen Computer finden Sie unter [Erneuter Schutz](site-recovery-how-to-reprotect.md).


1. Wählen Sie auf der Seite mit den replizierten Elementen den virtuellen Computer aus, und klicken Sie mit der rechten Maustaste auf **Ungeplantes Failover**.
2. Überprüfen Sie unter **Failover bestätigen** die Failoverrichtung (von Azure), und wählen Sie den Wiederherstellungspunkt aus, der für das Failover verwendet werden soll (den neuesten oder den neuesten anwendungskonsistenten Wiederherstellungspunkt). Der anwendungskonsistente Punkt würde vor dem aktuellsten Zeitpunkt liegen und einige Datenverluste verursachen.
3. Während des Failovers fährt Site Recovery die virtuellen Azure-Computer herunter. Vergewissern Sie sich, dass das Failback wie erwartet abgeschlossen wurde, und überprüfen Sie anschließend, ob die virtuellen Azure-Computer heruntergefahren wurden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem das Failback abgeschlossen ist, müssen Sie den virtuellen Computer committen, um sicherzustellen, dass die in Azure wiederhergestellten VMs gelöscht wurden.

1. Klicken Sie mit der rechten Maustaste auf das geschützte Element, und klicken Sie dann auf „Commit“. Ein Auftrag wird ausgelöst, der die virtuellen Computer entfernt, für die in Azure ein Failover ausgeführt wurde.

Nach dem Committen befindet sich Ihr virtueller Computer wieder am lokalen Standort, er ist jedoch nicht geschützt. Führen Sie daher folgende Schritte aus, um die Replikation zu Azure erneut zu starten:

1. Wählen Sie im Tresor unter „Einstellung > Replizierte Elemente“ die virtuellen Computer aus, für die das Failback durchgeführt wurde, und klicken Sie auf **Erneut schützen**.
2. Geben Sie den Wert des Prozessservers an, der zum Zurücksenden von Daten an Azure verwendet werden muss.
3. Klicken Sie auf „OK“, um den Auftrag zum erneuten Schützen zu starten.

Nachdem der Auftrag zum erneuten Schützen abgeschlossen ist, wird der virtuelle Computer zurück nach Azure repliziert, und Sie können ein Failover ausführen.

## <a name="common-issues"></a>Häufige Probleme





<!--HONumber=Feb17_HO2-->


