---
title: "Advisor-Empfehlungen für hohe Verfügbarkeit | Microsoft Docs"
description: "Mithilfe von Azure Advisor können Sie eine hohe Verfügbarkeit Ihrer Azure-Bereitstellungen sicherstellen."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: e7fc6cb4ee4752bb7ed90a8f64edcc7c82ec3a88
ms.openlocfilehash: a6ee1d018bf33aa76d089d20f6e71318a3586aa8
ms.lasthandoff: 02/21/2017

---

# <a name="advisor-high-availability-recommendations"></a>Advisor-Empfehlungen für hohe Verfügbarkeit

Advisor hilft Ihnen, die ununterbrochene Verfügbarkeit Ihrer unternehmenskritischen Anwendungen zu gewährleisten und zu verbessern. Empfehlungen für hohe Verfügbarkeit von Advisor erhalten Sie auf dem Advisor-Dashboard auf der Registerkarte **Hohe Verfügbarkeit**.

![](./media/advisor-high-availability-recommendations/advisor-high-availability-tab.png)


## <a name="virtual-machines-without-an-availability-set"></a>Virtuelle Computer ohne Verfügbarkeitsgruppe

Advisor identifiziert virtuelle Computer, die nicht zu einer Verfügbarkeitsgruppe gehören und empfiehlt, diese in eine Verfügbarkeitsgruppe zu verschieben. Um Redundanz für Ihre Anwendung zu gewährleisten, empfehlen wir die Gruppierung von zwei oder mehr virtuellen Computern in einer Verfügbarkeitsgruppe. Durch diese Konfiguration wird sichergestellt, dass während eines geplanten oder ungeplanten Wartungsereignisses mindestens ein virtueller Computer verfügbar ist und die Azure-SLA für virtuelle Computer eingehalten wird. Sie können entweder eine Verfügbarkeitsgruppe für den virtuellen Computer erstellen oder diesen einer vorhandenen Verfügbarkeitsgruppe hinzufügen.

> [!NOTE]
> Wenn Sie eine Verfügbarkeitsgruppe erstellen möchten, müssen Sie dieser Verfügbarkeitsgruppe nach ihrer Erstellung mindestens einen weiteren virtuellen Computer hinzufügen. Wir empfehlen das Gruppieren von mindestens zwei virtuellen Computern in einer Verfügbarkeitsgruppe, um sicherzustellen, dass einer davon bei einem Ausfall verfügbar ist.

![](./media/advisor-high-availability-recommendations/advisor-high-availability-create-availability-set.png)

## <a name="availability-sets-with-a-single-virtual-machine"></a>Verfügbarkeitsgruppen mit einem einzelnen virtuellen Computer 

Advisor ermittelt Verfügbarkeitsgruppen mit nur einem virtuellen Computer und empfiehlt, dieser weitere hinzuzufügen. Um Redundanz für Ihre Anwendung zu gewährleisten, empfehlen wir die Gruppierung von zwei oder mehr virtuellen Computern in einer Verfügbarkeitsgruppe. Durch diese Konfiguration wird sichergestellt, dass während eines geplanten oder ungeplanten Wartungsereignisses mindestens ein virtueller Computer verfügbar ist und die Azure-SLA für virtuelle Computer eingehalten wird. Sie können entweder einen virtuellen Computer erstellen oder einen vorhandenen virtuellen Computer der Verfügbarkeitsgruppe hinzufügen.  


![](./media/advisor-high-availability-recommendations/advisor-high-availability-add-vm-to-availability-set.png)

## <a name="virtual-machines-with-standard-disks"></a>Virtuelle Computer mit Standard-Datenträgern

Advisor ermittelt virtuelle Computer mit Standard-Datenträgern und empfiehlt ein Upgrade auf Premium-Datenträger.  
Azure Storage Premium bietet Datenträgerunterstützung für hohe Leistung mit geringer Latenz für virtuelle Computer mit E/A-intensiven Workloads. Datenträger von virtuellen Computern, die Storage Premium nutzen, speichern Daten auf SSD-Laufwerken (Solid State Drives). Es wird empfohlen, alle Datenträger von virtuellen Computer, die eine hohe IOPS-Leistung erfordern, zu Azure Storage Premium zu migrieren, um die beste Leistung für Ihre Anwendung zu erzielen. Wenn der Datenträger keinen hohen IOPS-Wert erfordert, können Sie Kosten begrenzen, indem Sie ihn mit Standardspeicher betreiben. Bei Wahl von Standardspeicher werden Daten auf VM-Datenträgern auf HDD- (Magnetfestplatten) anstatt auf SSD-Laufwerken gespeichert. Sie können Datenträger virtueller Computer auf Premium-Datenträger migrieren. Premium-Datenträger werden für die meisten VM-SKUs unterstützt. In einigen Fällen müssen Sie, wenn Sie Premium-Datenträger nutzen möchten, auch Ihre VM-SKU aktualisieren.   

![](./media/advisor-high-availability-recommendations/advisor-high-availability-upgrade-to-premium-disks.png) 

## <a name="how-to-access-high-availability-recommendations-in-advisor"></a>Zugreifen auf Advisor-Empfehlungen zu hoher Verfügbarkeit

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**, scrollen Sie im Dienstmenübereich nach unten zu **Überwachung und Verwaltung**, und klicken Sie dann auf **Azure Advisor**. Dadurch wird das Advisor-Dashboard gestartet. 
3. Klicken Sie auf dem Advisor-Dashboard auf die Registerkarte **Hohe Verfügbarkeit**, und wählen Sie das Abonnement aus, für das Sie Empfehlungen erhalten möchten.

> [!NOTE]
> Für den Zugriff auf die Empfehlungen von Ratgeber müssen Sie Ihr Abonnement zunächst bei Ratgeber **registrieren**. Ein Abonnement wird registriert, wenn ein **Abonnementbesitzer** das Ratgeber-Dashboard startet und auf die Schaltfläche **Abrufen von Empfehlungen** klickt. Dies ist eine **einmalige Aufgabe**. Sobald ein Abonnement registriert wurde, können **Besitzer**, **Mitwirkende** oder **Leser** eines Abonnements, einer Ressourcengruppe oder einer bestimmten Ressource auf die Empfehlungen von Ratgeber zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Advisor-Empfehlungen finden Sie in den folgenden Ressourcen:
-  [Einführung in Azure Advisor](advisor-overview.md)
-  [Erste Schritte mit Advisor](advisor-get-started.md)
-  [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)
-  [Advisor-Empfehlungen zur Leistung](advisor-performance-recommendations.md)
-  [Advisor-Empfehlungen zu Kosten](advisor-performance-recommendations.md)

