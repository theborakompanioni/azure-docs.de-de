---
title: Neustarten von Linux-VMs in Azure zu Wartungszwecken | Microsoft-Dokumentation
description: Neustarten virtueller Linux-Computer in Azure zu Wartungszwecken.
services: virtual-machines-linux
documentationcenter: 
author: zivr
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: eb4b92d8-be0f-44f6-a6c3-f8f7efab09fe
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: zivr
ms.translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: c02b2878462afb107c81317c9ea0a317cb9ce838
ms.contentlocale: de-de
ms.lasthandoff: 04/18/2017


---

# <a name="vm-restarting-maintenance"></a>Neustarten von VMs zu Wartungszwecken

Es gibt Fälle, in denen Ihre VMs aufgrund geplanter Wartungsarbeiten an der zugrunde liegenden Infrastruktur neu gestartet werden. Da dies mit Auswirkungen auf die Verfügbarkeit Ihrer in Azure gehosteten VMs verbunden ist, gilt nun Folgendes:

-   Eine Benachrichtigung wird mindestens 30 Tage vor der Beeinträchtigung gesendet.

-   Das Wartungsfenster für jede VM ist klar ersichtlich.

-   Die exakte Uhrzeit der Wartung mit Auswirkungen auf Ihre VMs kann flexibel festgelegt werden.

Wartungsarbeiten in Microsoft Azure werden schrittweise geplant. Die anfänglichen Schritte haben einen kleineren Umfang, um das Risiko bei der Einführung neuer Korrekturen und Funktionen zu verringern. Spätere Schritte können sich auf mehrere Regionen (jedoch niemals auf dasselbe Regionspaar) beziehen. Eine VM wird in einen einzelnen Wartungsschritt einbezogen. Wenn ein Schritt abgebrochen wird, werden verbleibende VMs in einen anderen, künftigen Schritt einbezogen.

Der geplante Wartungsschritt erfolgt in zwei Phasen, und zwar in einem vorbereitenden und in einem geplanten Wartungsfenster.

Das **vorbereitende Wartungsfenster** ermöglicht Ihnen das flexible Einleiten der Wartung auf Ihrem VMs. Auf diese Weise können Sie den Zeitpunkt der Beeinträchtigung Ihrer VMs, die Sequenz des Updates und die Zeit zwischen den einzelnen zu wartenden VMs bestimmen. Sie können jede VM auf einen geplanten Wartungstermin abfragen und das Ergebnis der letzten ausgelösten Wartungsanforderung überprüfen.

Das **geplante Wartungsfenster** bestimmt den Zeitpunkt, für den Azure die Wartung Ihrer VMs geplant hat. In diesem Zeitfenster, das auf das vorbereitende Zeitfenster folgt, können Sie zwar den Wartungszeitraum abfragen, aber die Wartung nicht mehr orchestrieren.

## <a name="availability-considerations-during-planned-maintenance"></a>Überlegungen zur Verfügbarkeit während einer geplanten Wartung 

### <a name="paired-regions"></a>Regionspaare

Jeder Azure-Region ist innerhalb des gleichen geografischen Gebiets eine andere Region als Regionspartner zugeordnet. Beim Ausführen der Wartung aktualisiert Azure nur die VM-Instanzen in einer Region des jeweiligen Regionspaars. Wenn z. B. die virtuellen Computer (Virtual Machines, VMs) in der Region „USA, Norden-Mitte“ aktualisiert werden, aktualisiert Azure nicht gleichzeitig die virtuellen Computer in der Region „USA, Süden-Mitte“. Die Aktualisierung für diese Region wird zu einem anderen Zeitpunkt geplant. Dies ermöglicht ein Failover oder einen Lastenausgleich zwischen den Regionen. Andere Regionen wie Nordeuropa können jedoch gleichzeitig mit USA (Ost) gewartet werden.
Erfahren Sie mehr über [Azure-Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

### <a name="single-instance-vms-vs-availability-set-or-vm-scale-set"></a>Einzelinstanz-VMs im Vergleich mit einer Verfügbarkeitsgruppe oder einer VM-Skalierungsgruppe

Wenn Sie mithilfe virtueller Computer eine Workload in Azure bereitstellen, können Sie die VMs in einer Verfügbarkeitsgruppe erstellen, um Ihre Anwendung mit hoher Verfügbarkeit zu versehen. Durch diese Konfiguration wird sichergestellt, dass während eines Ausfalls oder Wartungsereignisses mindestens ein virtueller Computer verfügbar ist.

Innerhalb einer Verfügbarkeitsgruppe werden einzelne VMs auf bis zu 20 Updatedomänen verteilt. Während einer geplanten Wartung ist zum jeweiligen Zeitpunkt immer nur eine Updatedomäne betroffen. Die Reihenfolge der betroffenen Updatedomänen muss während einer geplanten Wartung nicht unbedingt sequenziell sein. Bei Einzelinstanz-VMs (die nicht zu einer Verfügbarkeitsgruppe gehören) kann nicht vorhergesagt oder bestimmt werden, welche und wie viele VMs gemeinsam betroffen sind.

VM-Skalierungsgruppen sind eine Azure-Computeressource, mit der Sie eine Gruppe identischer virtueller Computer als Einzelressource bereitstellen und verwalten können.
Die Skalierungsgruppe bietet in Form von Updatedomänen ähnliche Zusicherungen wie eine Verfügbarkeitsgruppe. 

Weitere Informationen zum Konfigurieren Ihrer virtuellen Computer für hohe Verfügbarkeit finden Sie unter [*Verwalten der Verfügbarkeit virtueller Windows-Computer*](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="scheduled-events"></a>Geplante Ereignisse

Der Azure-Metadatendienst ermöglicht Ihnen das Ermitteln von Informationen zu Ihrem in Azure gehosteten virtuellen Computer. „Geplante Ereignisse“, eine der verfügbar gemachten Kategorien, liefert Informationen zu anstehenden Ereignissen (z.B. Neustart), damit sich Ihre Anwendung darauf vorbereiten und Unterbrechungen begrenzen kann.

Weitere Informationen zu geplanten Ereignissen finden Sie unter [Azure-Metadatendienst: Geplante Ereignisse](../virtual-machines-scheduled-events.md).

## <a name="maintenance-discovery-and-notifications"></a>Bestimmen von Wartungsterminen und Benachrichtigungen

Der Wartungszeitplan wird Kunden auf der Ebene der einzelnen virtuellen Computer angezeigt. Sie können das vorbereitende und geplante Wartungsfenster im Azure-Portal oder über die API, PowerShell oder die CLI abfragen. Darüber hinaus erhalten Sie (per E-Mail) eine Benachrichtigung, wenn Ihre VMs während des Vorgangs beeinträchtigt werden.

Die beiden Phasen der Wartungsvorbereitung und -planung werden mit einer Benachrichtigung eingeleitet. Sie erhalten pro Azure-Abonnement eine Benachrichtigung. Die Benachrichtigung wird standardmäßig an den Administrator und Co-Admin des Abonnements gesendet. Sie können auch die Zielgruppe für die Wartungsbenachrichtigung konfigurieren.

### <a name="view-the-maintenance-window-in-the-portal"></a>Anzeigen des Wartungsfensters im Portal 

Sie können im Azure-Portal nach VMs suchen, für die eine Wartung geplant ist.

1.  Melden Sie sich beim Azure-Portal an.

2.  Klicken Sie auf das Blatt **Virtuelle Computer**, um es zu öffnen.

3.  Klicken Sie auf die Schaltfläche **Spalten**, um die Liste der verfügbaren Spalten zu öffnen.

4.  Wählen Sie die Spalte **Wartungsfenster** aus, und fügen Sie sie hinzu. Für VMs, für die eine Wartung geplant ist, wird das Wartungsfenster angezeigt. Sobald die Wartung für eine VM abgeschlossen ist oder abgebrochen wird, wird das Wartungsfenster nicht mehr angezeigt.

### <a name="query-maintenance-details-using-the-azure-api"></a>Abfragen von Wartungsdetails mithilfe der Azure-API

Verwenden Sie die [API zum Abrufen von VM-Informationen](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-get), und untersuchen Sie die Instanzansicht auf Details zur Wartung einer einzelnen VM. Die Antwort enthält die folgenden Elemente:

  - IsCustomerInitiatedMaintenanceAllowed: Gibt an, ob Sie nun eine präemptive Neubereitstellung auf der VM einleiten können.

  - preMaintenanceWindowStartTime: Die Startzeit des vorbereitenden Wartungsfensters.

  - preMaintenanceWindowEndTime: Die Endzeit des vorbereitenden Wartungsfensters. Nach dieser Zeit können Sie die Wartung auf dieser VM nicht mehr einleiten.
    
  - maintenanceWindowStartTime: Die Startzeit des geplanten Wartungsfensters, wenn Ihre VM betroffen ist.

  - maintenanceWindowEndTime: Die Endzeit des geplanten Wartungsfensters.
  
  - lastOperationResultCode: Das Ergebnis des letzten Wartungs-/Neubereitstellungsvorgangs.
 
  - lastOperationMessage: Meldung mit dem Ergebnis des letzten Wartungs-/Neubereitstellungsvorgangs.


## <a name="pre-emptive-redeploy"></a>Präemptive erneute Bereitstellung

Die präemptive erneute Bereitstellungsaktion ermöglicht die flexible Festlegung des Zeitpunkts der Anwendung der Wartung auf Ihre VMs in Azure. Vor der geplanten Wartung gibt es ein vorbereitendes Wartungsfenster, in dem Sie den exakten Zeitpunkt des Neustarts Ihrer einzelnen VMs bestimmen können. Es folgen Anwendungsfälle, in denen dies nützlich ist:

-   Die Wartung muss mit dem Endkunden abgestimmt werden.

-   Das von Azure angebotene geplante Wartungsfenster ist nicht ausreichend.
    Das Zeitfenster ist ggf. für die aktivste Zeit der Woche terminiert oder zu lang.

-   Bei Anwendungen mit mehreren Instanzen oder Ebenen benötigen Sie ausreichend Zeit zwischen zwei VMs, oder es muss eine bestimmte Sequenz befolgt werden.

Wenn Sie einen virtuellen Computer präemptiv erneut bereitstellen, wird er auf einen bereits aktualisierten Knoten verschoben und dann wieder eingeschaltet. Dabei werden alle Ihre Konfigurationsoptionen und zugehörigen Ressourcen beibehalten. Dabei geht der temporäre Datenträger verloren, und die der virtuellen Netzwerkschnittstelle zugeordneten dynamischen IP-Adressen werden aktualisiert.

Die präemptive erneute Bereitstellung wird einmal pro virtuellem Computer aktiviert. Wenn während des Vorgangs ein Fehler auftritt, wird der Vorgang abgebrochen, der virtuelle Computers nicht beeinträchtigt und vom geplanten Wartungsschritt ausgeschlossen. Sie werden zu einem späteren Zeitpunkt mit einem neuen Zeitplan kontaktiert und erhalten eine neue Gelegenheit, die Beeinträchtigung Ihrer VMs zu planen.

