---
title: "Advisor-Empfehlungen für hohe Verfügbarkeit | Microsoft Docs"
description: "Mithilfe von Azure Advisor können Sie eine hohe Verfügbarkeit Ihrer Azure-Bereitstellungen sicherstellen."
services: advisor
documentationcenter: NA
author: KumudD
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
ms.translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 23c159471a6e5a7ad9cb545840e8afd3ac72ecba
ms.contentlocale: de-de
ms.lasthandoff: 03/31/2017

---

# <a name="advisor-high-availability-recommendations"></a>Advisor-Empfehlungen für hohe Verfügbarkeit

Der Azure-Ratgeber hilft Ihnen, die ununterbrochene Verfügbarkeit Ihrer unternehmenskritischen Anwendungen zu gewährleisten und zu verbessern. Empfehlungen für hohe Verfügbarkeit von Advisor erhalten Sie auf dem Advisor-Dashboard auf der Registerkarte **Hohe Verfügbarkeit**.

![Schaltfläche „Hohe Verfügbarkeit“ im Ratgeberdashboard](./media/advisor-high-availability-recommendations/advisor-high-availability-tab.png)


## <a name="ensure-virtual-machine-fault-tolerance"></a>Sicherstellen von Fehlertoleranz für virtuelle Computer

Advisor identifiziert virtuelle Computer, die nicht zu einer Verfügbarkeitsgruppe gehören und empfiehlt, diese in eine Verfügbarkeitsgruppe zu verschieben. Um Redundanz für Ihre Anwendung zu gewährleisten, empfehlen wir die Gruppierung von zwei oder mehr virtuellen Computern in einer Verfügbarkeitsgruppe. Durch diese Konfiguration wird sichergestellt, dass während eines geplanten oder ungeplanten Wartungsereignisses mindestens ein virtueller Computer verfügbar ist und die Azure-SLA für virtuelle Computer eingehalten wird. Sie können entweder eine Verfügbarkeitsgruppe für den virtuellen Computer erstellen oder diesen einer vorhandenen Verfügbarkeitsgruppe hinzufügen.

> [!NOTE]
> Wenn Sie eine Verfügbarkeitsgruppe erstellen möchten, müssen Sie ihr mindestens einen weiteren virtuellen Computer hinzufügen. Wir empfehlen das Gruppieren von mindestens zwei virtuellen Computern in einer Verfügbarkeitsgruppe, um sicherzustellen, dass mindestens ein Computer bei einem Ausfall verfügbar ist.

![Ratgeberempfehlung: Verwenden Sie Verfügbarkeitsgruppen, um Redundanz für virtuelle Computer zu gewährleisten.](./media/advisor-high-availability-recommendations/advisor-high-availability-create-availability-set.png)

## <a name="ensure-availability-set-fault-tolerance"></a>Sicherstellen von Fehlertoleranz für Verfügbarkeitsgruppen 

Der Ratgeber ermittelt Verfügbarkeitsgruppen mit nur einem virtuellen Computer und empfiehlt, dieser weitere hinzuzufügen. Um Redundanz für Ihre Anwendung zu gewährleisten, empfehlen wir die Gruppierung von zwei oder mehr virtuellen Computern in einer Verfügbarkeitsgruppe. Durch diese Konfiguration wird sichergestellt, dass während eines geplanten oder ungeplanten Wartungsereignisses mindestens ein virtueller Computer verfügbar ist und die Azure-SLA für virtuelle Computer eingehalten wird. Sie können entweder einen virtuellen Computer erstellen oder der Verfügbarkeitsgruppe einen vorhandenen virtuellen Computer hinzufügen.  

![Ratgeberempfehlung: Fügen Sie dieser Verfügbarkeitsgruppe mindestens einen virtuellen Computer hinzu.](./media/advisor-high-availability-recommendations/advisor-high-availability-add-vm-to-availability-set.png)


## <a name="ensure-application-gateway-fault-tolerance"></a>Sicherstellen von Fehlertoleranz für Anwendungsgateways
Um die Geschäftskontinuität unternehmenswichtiger, durch Anwendungsgateways unterstützter Anwendungen zu gewährleisten, identifiziert der Ratgeber Anwendungsgatewayinstanzen, die nicht für die Fehlertoleranz konfiguriert sind, und schlägt Ihnen Wartungsaktionen vor. Der Ratgeber identifiziert mittelgroße oder große Einzelinstanz-Anwendungsgateways und empfiehlt, mindestens eine weitere Instanz hinzufügen. Identifiziert außerdem kleine Einzel- und Mehrinstanz-Anwendungsgateways und empfiehlt die Migration zu mittelgroßen oder großen SKUs. Der Ratgeber empfiehlt diese Aktionen, um sicherzustellen, dass Ihre Anwendungsgatewayinstanzen so konfiguriert sind, dass sie die aktuellen SLA-Anforderungen für diese Ressourcen erfüllen.

![Ratgeberempfehlung: Stellen Sie mindestens zwei mittelgroße oder große Anwendungsgatewayinstanzen bereit.](./media/advisor-high-availability-recommendations/advisor-high-availability-application-gateway.png)

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks"></a>Verbessern der Leistung und Zuverlässigkeit von Datenträgern virtueller Computer

Der Ratgeber ermittelt virtuelle Computer mit Standard-Datenträgern und empfiehlt ein Upgrade auf Premium-Datenträger.
 
Azure Storage Premium bietet Datenträgerunterstützung für hohe Leistung mit geringer Latenz für virtuelle Computer mit E/A-intensiven Workloads. Datenträger von virtuellen Computern, die Storage Premium-Konten nutzen, speichern Daten auf Solid State Drives (SSD). Für eine optimale Leistung Ihrer Anwendung empfehlen wir, alle Datenträger von virtuellen Computern, die eine hohe IOPS-Leistung erfordern, zu Storage Premium zu migrieren. 

Wenn Ihre Datenträger keinen hohen IOPS-Wert erfordern, können Sie die Kosten begrenzen, indem Sie sie mit Storage Standard betreiben. Bei Storage Standard werden VM-Datenträgerdaten auf Festplattenlaufwerken (HDD) statt auf Solid State Drives (SSD) gespeichert. Sie können Ihre VM-Datenträger zu Premium-Datenträgern migrieren. Premium-Datenträger werden für die meisten VM-SKUs unterstützt. In einigen Fällen müssen Sie, wenn Sie Premium-Datenträger nutzen möchten, auch Ihre VM-SKUs aktualisieren.

![Ratgeberempfehlung: Verbessern Sie die Zuverlässigkeit Ihrer VM-Datenträger, indem Sie ein Upgrade auf Premium-Datenträger durchführen.](./media/advisor-high-availability-recommendations/advisor-high-availability-upgrade-to-premium-disks.png)

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Schützen Ihrer VM-Daten vor zufälligem Löschen
Der Ratgeber identifiziert virtuelle Computer, auf denen die Sicherung nicht aktiviert ist, und empfiehlt das Aktivieren der Sicherung. Durch das Einrichten der Sicherung eines virtuellen Computers wird die Verfügbarkeit Ihrer geschäftskritischen Daten sichergestellt, und Ihre Daten werden vor versehentlichem Löschen oder vor Beschädigung geschützt.

![Ratgeberempfehlung: Konfigurieren Sie die Sicherung virtueller Computer so, dass Ihre geschäftskritischen Daten geschützt werden.](./media/advisor-high-availability-recommendations/advisor-high-availability-virtual-machine-backup.png)

## <a name="access-high-availability-recommendations-in-advisor"></a>Zugreifen auf Ratgeberempfehlungen zu hoher Verfügbarkeit

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Klicken Sie im linken Bereich auf **Weitere Dienste**.

3. Klicken Sie im Dienstmenübereich unter **Überwachung und Verwaltung** auf **Azure-Ratgeber**.  
 Das Ratgeberdashboard wird angezeigt.

4. Klicken Sie im Ratgeberdashboard auf die Registerkarte **Hohe Verfügbarkeit**, und wählen Sie dann das Abonnement aus, für das Sie Empfehlungen erhalten möchten.

> [!NOTE]
> Für den Zugriff auf die Empfehlungen des Ratgebers müssen Sie Ihr Abonnement zunächst beim Ratgeber *registrieren*. Ein Abonnement wird registriert, wenn ein *Abonnementbesitzer* das Ratgeberdashboard aufruft und auf die Schaltfläche **Empfehlungen abrufen** klickt. Dies ist eine *einmalige Aufgabe*. Sobald ein Abonnement registriert wurde, können *Besitzer*, *Mitwirkende* oder *Leser* eines Abonnements, einer Ressourcengruppe oder einer bestimmten Ressource auf die Ratgeberempfehlungen zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Ratgeberempfehlungen finden Sie unter:
* [Einführung in Azure Advisor](advisor-overview.md)
* [Erste Schritte mit Advisor](advisor-get-started.md)
* [Advisor-Empfehlungen zu Kosten](advisor-performance-recommendations.md)
* [Advisor-Empfehlungen zur Leistung](advisor-performance-recommendations.md)
* [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)


