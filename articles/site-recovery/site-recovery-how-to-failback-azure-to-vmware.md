---
title: "Durchführen eines Failbacks von Azure zu Hyper-V | Microsoft-Dokumentation"
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
ms.date: 02/13/2017
ms.author: ruturajd
translationtype: Human Translation
ms.sourcegitcommit: bedb59414ed328fe2fcca2b09f3360a98c98ff60
ms.openlocfilehash: b4bdd27db343bfb4a0e47c432f726ff454f7f55a
ms.lasthandoff: 02/23/2017


---
# <a name="failback-from-azure-to-on-premises"></a>Failback von Azure zu lokal
In diesem Artikel wird beschrieben, wie Sie für virtuelle Azure-Computer ein Failback auf den lokalen Standort durchführen. Befolgen Sie die Anweisungen in diesem Artikel, wenn Sie bereit sind, Ihre virtuellen VMware-Computer oder Ihre physischen Windows-/Linux-Server nach einem Failover vom lokalen Standort auf Azure (beschrieben in diesem [Tutorial](site-recovery-vmware-to-azure-classic.md)) wieder per Failback auf den lokalen Standort zurückzuführen.

## <a name="overview-of-failback"></a>Übersicht über das Failback
Das Failback funktioniert wie folgt: Nach einem Failover zu Azure erfolgt die Rückführung zum lokalen Standort per Failback in mehreren Phasen:

1. [Schützen Sie die virtuellen Azure-Computer erneut](site-recovery-how-to-reprotect.md), damit diese zurück auf die virtuellen VMware-Computer am lokalen Standort repliziert werden. Dafür müssen Sie zudem folgende Schritte ausführen: 
    1. Einrichten eines lokalen Masterziels – Windows MT für Windows-VMs und [Linux-MT](site-recovery-how-to-install-linux-master-target.md) für Linux-VMs
    2. Einrichten eines [Prozessservers](site-recovery-vmware-setup-azure-ps-resource-manager.md)
    3. Initiieren des [erneuten Schützens](site-recovery-how-to-reprotect.md)
5. Initiieren Sie nach der Replikation Ihrer virtuellen Azure-Computer am lokalen Standort ein Failover von Azure zu lokal.
  
Nachdem Ihre Daten per Failback zurückgeführt wurden, schützen Sie die lokalen virtuellen Computer, auf die Sie das Failback durchgeführt haben, erneut, damit diese mit der Replikation nach Azure beginnen.

Sie können sich auch dieses Video ansehen, um einen kurzen Überblick zu erhalten.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]

### <a name="failback-to-the-original-or-alternate-location"></a>Failback zum ursprünglichen oder zu einem anderen Standort
    
Wenn Sie ein Failover für einen virtuellen VMware-Computer durchgeführt haben, können Sie das Failback zum gleichen virtuellen Quellcomputer ausführen, sofern er lokal noch vorhanden ist. In diesem Szenario werden nur die geänderten Daten (Deltaänderungen) repliziert. Dieses Szenario wird als Wiederherstellung am ursprünglichen Speicherort bezeichnet. Wenn der lokale virtuelle Computer nicht vorhanden ist, handelt es sich um eine Wiederherstellung an einem alternativen Speicherort.

#### <a name="original-location-recovery"></a>Wiederherstellung am ursprünglichen Speicherort

Wenn Sie ein Failback zum ursprünglichen virtuellen Computer durchführen, sind folgende Bedingungen erforderlich:
* Wenn der virtuelle Computer von einem vCenter-Server verwaltet wird, muss der ESX-Host des Masterziels Zugriff auf den Datenspeicher des virtuellen Computers haben.
* Wenn sich der virtuelle Computer auf einem ESX-Host befindet, aber nicht von vCenter verwaltet wird, muss sich die Festplatte des virtuellen Computers in einem Datenspeicher befinden, auf den der Host des Masterziels Zugriff hat.
* Wenn sich Ihr virtueller Computer auf einem ESX-Host befindet und nicht vCenter verwendet, müssen Sie für den ESX-Host des Masterziels eine Ermittlung ausführen, bevor Sie den erneuten Schutz ausführen. Dasselbe gilt auch bei einem Failback für physische Server.
* Sie können ein Failback in ein vSAN oder auf RDM-basierte Datenträger durchführen, wenn die Datenträger bereits vorhanden sind und über eine Verbindung mit dem lokalen virtuellen Computer verfügen.

#### <a name="alternate-location-recovery"></a>Wiederherstellung an einem alternativen Speicherort
Wenn der lokale virtuelle Computer nicht vorhanden ist, bevor Sie den virtuellen Computer erneut schützen, wird der Vorgang als Wiederherstellung an einem alternativen Speicherort bezeichnet. Hier wird mit dem Workflow zum erneuten Schützen der lokale virtuelle Computer neu erstellt. Außerdem wird ein vollständiger Download der Daten durchgeführt.

* Wenn Sie ein Failback an einem alternativen Speicherort des virtuellen Computers ausführen, erfolgt die Wiederherstellung auf demselben ESX-Host, auf dem der Masterzielserver bereitgestellt ist. Der zum Erstellen des Datenträgers verwendete Datenspeicher ist derselbe Datenspeicher, der beim erneuten Schützen des virtuellen Computers ausgewählt wurde.
* Sie können ein Failback nur auf einen VMFS-Datenspeicher ausführen. Bei vSAN oder RDM funktionieren das erneute Schützen und das Failback nicht.
* Das erneute Schützen umfasst eine große anfängliche Datenübertragung gefolgt von Deltaänderungen. Da der virtuelle Computer lokal nicht vorhanden ist, müssen die gesamten Daten zurück repliziert werden. Dieser erneute Schutz dauert außerdem länger als die Wiederherstellung am ursprünglichen Speicherort.
* Sie können kein Failback auf ein vSAN oder RDM-basierte Datenträger durchführen. In einem VMFS-Datenspeicher können nur neue VMDKs erstellt werden.

Für einen physischen Computer kann beim Failover auf Azure nur ein Failback als virtueller VMware-Computer (auch als P2A2V bezeichnet) ausgeführt werden. Dieser Flow fällt unter Wiederherstellung an einem alternativen Speicherort.

* Für einen geschützten Windows Server 2008 R2 SP1-Computer, für den ein Failover zu Azure ausgeführt wurde, kann kein Failback ausgeführt werden.
* Stellen Sie sicher, dass Sie mindestens einen Masterzielserver zusammen mit den erforderlichen ESX/ESXi-Hosts ermitteln, auf die Sie ein Failback ausführen müssen.

## <a name="have-you-completed-reprotection"></a>Haben Sie das erneute Schützen abgeschlossen?
Führen Sie vor dem Fortfahren alle Schritte für das erneute Schützen aus, damit sich die virtuellen Computer im replizierten Zustand befinden und Sie ein Failover zurück zum lokalen Standort initiieren können.
[Erneutes Schützen von Azure zu lokal](site-recovery-how-to-reprotect.md)

## <a name="pre-requisites"></a>Voraussetzungen
 
* Der Konfigurationsserver ist lokal erforderlich, wenn Sie ein Failback durchführen. Während des Failbacks muss der virtuelle Computer in der Konfigurationsserverdatenbank vorhanden sein, andernfalls ist das Failback nicht erfolgreich. Daher stellen Sie sicher, dass Sie die regelmäßigen geplanten Sicherungen des Servers ausführen. Im Notfall müssen Sie ihn mit der gleichen IP-Adresse wiederherstellen, damit das Failback funktioniert.
* Masterzielserver sollten vor dem Auslösen des Failbacks keine Momentaufnahmen enthalten.

## <a name="steps-to-failback"></a>Schritte zum Failback

Vor dem Initiieren des Failbacks **muss sichergestellt sein, dass der erneute Schutz für die virtuellen Computer abgeschlossen ist**. Die virtuellen Computer müssen geschützt sein und eine intakte Integrität aufweisen. Weitere Informationen zum erneuten Schutz der virtuellen Computer finden Sie unter [Erneuter Schutz](site-recovery-how-to-reprotect.md).

1. Wählen Sie auf der Seite mit den replizierten Elementen den virtuellen Computer aus, und klicken Sie mit der rechten Maustaste auf **Ungeplantes Failover**.
2. Überprüfen Sie unter **Failover bestätigen** die Failoverrichtung (von Azure), und wählen Sie den Wiederherstellungspunkt aus, der für das Failover verwendet werden soll (den neuesten oder den neuesten anwendungskonsistenten Wiederherstellungspunkt). Der anwendungskonsistente Punkt würde vor dem aktuellsten Zeitpunkt liegen und einige Datenverluste verursachen.
3. Während des Failovers fährt Site Recovery die virtuellen Azure-Computer herunter. Vergewissern Sie sich, dass das Failback wie erwartet abgeschlossen wurde, und überprüfen Sie anschließend, ob die virtuellen Azure-Computer heruntergefahren wurden.

### <a name="to-what-recovery-point-can-i-failback-the-vms-to"></a>Auf welchen Wiederherstellungspunkt kann ich für die VMs ein Failback durchführen?

Während des Failbacks haben Sie für das Failback der VM bzw. des Wiederherstellungsplans zwei Möglichkeiten.

Wenn Sie den letzten Verarbeitungspunkt auswählen, wird für alle virtuellen Computer ein Failover zum letzten verfügbaren Zeitpunkt durchgeführt. Falls im Wiederherstellungsplan eine Replikationsgruppe enthalten ist, wird für jede VM der Replikationsgruppe ein Failover zu ihrem unabhängigen letzten Zeitpunkt durchgeführt.

Sie können für eine VM erst dann ein Failback durchführen, wenn sie über mindestens einen Wiederherstellungspunkt verfügt. Ein Failback für einen Wiederherstellungsplan ist erst möglich, wenn alle beteiligten VMs mindestens einen Wiederherstellungspunkt aufweisen.

> [!NOTE]
> Bei einem letzten Wiederherstellungspunkt handelt es sich um einen absturzkonsistenten Wiederherstellungspunkt.

Wenn Sie den anwendungskonsistenten Wiederherstellungspunkt auswählen, wird bei einem Failback für eine einzelne VM die Wiederherstellung des letzten verfügbaren anwendungskonsistenten Wiederherstellungspunkts durchgeführt. Bei einem Wiederherstellungsplan mit einer Replikationsgruppe erfolgt für jede Replikationsgruppe die Wiederherstellung für den gemeinsamen verfügbaren Wiederherstellungspunkt.
Beachten Sie Folgendes: Anwendungskonsistente Wiederherstellungspunkte können zeitlich weiter zurück liegen, und es kann zu einem Verlust von Daten kommen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem das Failback abgeschlossen ist, müssen Sie den virtuellen Computer committen, um sicherzustellen, dass die in Azure wiederhergestellten VMs gelöscht wurden.

### <a name="commit"></a>Commit
Ein Commit ist erforderlich, um den virtuellen Computer, für den das Failover durchgeführt wurde, aus Azure zu entfernen.
Klicken Sie mit der rechten Maustaste auf das geschützte Element, und klicken Sie dann auf „Commit“. Ein Auftrag wird ausgelöst, der die virtuellen Computer entfernt, für die in Azure ein Failover ausgeführt wurde.

### <a name="reprotect-from-on-premises-to-azure"></a>Erneutes Schützen von einem lokalen Standort nach Azure

Nach dem Committen befindet sich Ihr virtueller Computer wieder am lokalen Standort, er ist jedoch nicht geschützt. Führen Sie daher folgende Schritte aus, um die Replikation zu Azure erneut zu starten:

1. Wählen Sie im Tresor unter „Einstellung > Replizierte Elemente“ die virtuellen Computer aus, für die das Failback durchgeführt wurde, und klicken Sie auf **Erneut schützen**.
2. Geben Sie den Wert des Prozessservers an, der zum Zurücksenden von Daten an Azure verwendet werden muss.
3. Klicken Sie auf „OK“, um den Auftrag zum erneuten Schützen zu starten.

> [!NOTE]
> Nachdem eine VM lokal gestartet wurde, dauert es eine Weile, bis der Agent auf dem Konfigurationsserver registriert wird (bis zu 15 Minuten). Während dieses Zeitraums schlägt das erneute Schützen fehl, und in der Fehlermeldung wird angegeben, dass der Agent nicht installiert ist. Warten Sie einige Minuten, und versuchen Sie dann noch einmal, das erneute Schützen durchzuführen.

Nachdem der Auftrag zum erneuten Schützen abgeschlossen ist, wird der virtuelle Computer zurück nach Azure repliziert, und Sie können ein Failover ausführen.

## <a name="common-issues"></a>Häufige Probleme
* Stellen Sie sicher, dass für vCenter eine Verbindung besteht, bevor Sie ein Failback durchführen. Ansonsten schlägt das Trennen der Datenträger und das erneute Anfügen an die VM fehl.


