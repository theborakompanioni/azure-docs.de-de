---
title: "Verwenden des Überwachungsprotokolls | Microsoft Docs"
description: "Erfahren Sie, wie Sie in der Erweiterung Azure Privileged Identity Management das Überwachungsprotokoll verwenden."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 5d13a6dd-1fcb-4e76-82fb-cb2f4f0e4357
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/22/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5ac402f1a5f76c02f22a189eec8cada0cc9b4a0f


---
# <a name="how-to-use-the-audit-log-in-azure-ad-privileged-identity-management"></a>Verwenden des Überwachungsprotokolls in Azure AD Privileged Identity Management
Im Privileged Identity Management (PIM)-Überwachungsprotokoll werden alle Benutzerzuweisungen und Aktivierungen in einem angegebenen Zeitraum angezeigt. Wenn Sie den vollständigen Überwachungsverlauf zur Aktivität in Ihrem Mandanten anzeigen möchten – Administratoren, Endbenutzer und Synchronisierungsaktivität eingeschlossen –, können Sie hierzu die [Azure Active Directory-Zugriffs- und Nutzungsberichte](active-directory-view-access-usage-reports.md)

## <a name="navigate-to-the-audit-log"></a>Navigieren zum Überwachungsprotokoll
Wählen Sie auf dem Dashboard im [Azure-Portal](https://portal.azure.com) die App **Azure AD Privileged Identity Management** aus. Dort können Sie das Überwachungsprotokoll aufrufen, indem Sie im PIM-Dashboard auf **Privilegierte Rollen verwalten** > **Überwachungsverlauf** klicken.

## <a name="the-audit-log-graph"></a>Diagramm des Überwachungsprotokolls
Sie können mithilfe des Überwachungsprotokolls die Gesamtzahl der Aktivierungen, die maximale Anzahl der Aktivierungen pro Tag und die durchschnittliche Anzahl der Aktivierungen pro Tag in einem Liniendiagramm anzeigen.  Sie können auch die Daten nach Rolle filtern, wenn der Überwachungsverlauf mehrere Rollen enthält.

Sortieren Sie das Protokoll mithilfe der Schaltflächen **Zeit**, **Aktion** und **Rolle**.

## <a name="the-audit-log-list"></a>Die Überwachungsprotokollliste
Die Überwachungsprotokollliste enthält die folgenden Spalten:

* **Anforderer** : Gibt den Benutzer an, der die Rollenaktivierung oder -änderung angefordert hat.  Wenn der Wert „Azure System“ lautet, suchen Sie im Azure-Überwachungsprotokoll nach weiteren Informationen.
* **Benutzer** : Gibt den Benutzer an, der eine Rolle aktiviert oder einer Rolle zugewiesen ist.
* **Rolle** : Gibt die Rolle an, die dem Benutzer zugewiesen ist oder die durch den Benutzer aktiviert wurde.
* **Aktion** : Gibt die vom Anforderer ausgeführten Aktionen an. Mögliche Aktionen: Zuweisung, Aufhebung einer Zuweisung, Aktivierung oder Deaktivierung.
* **Zeit** : Dies ist der Zeitpunkt, zu dem die Aktion erfolgt ist.
* **Erläuterung** : Wenn während der Aktivierung Text in das Feld für die Begründung eingegeben wurde, wird er hier angezeigt.
* **Ablauf** : Ist nur für die Aktivierung von Rollen relevant.

## <a name="filter-the-audit-log"></a>Filtern des Überwachungsprotokolls
Sie können die im Überwachungsprotokoll angezeigten Informationen filtern, indem Sie auf die Schaltfläche **Filter** klicken.  Das Blatt **Diagrammparameter aktualisieren** wird angezeigt.

Klicken Sie nach dem Einrichten der Filter auf **Aktualisieren** , um die Daten im Protokoll zu filtern.  Wenn die Daten nicht sofort angezeigt werden, aktualisieren Sie die Seite.

### <a name="change-the-date-range"></a>Ändern des Datumsbereichs
Verwenden Sie die Schaltflächen **Heute**, **Letzte Woche**, **Letzter Monat** oder **Benutzerdefiniert**, um den Zeitbereich des Überwachungsprotokolls zu ändern.

Wenn Sie auf **Benutzerdefiniert** klicken, werden die Datumsfelder **Von** und **Bis** angezeigt, um den Datumsbereich für das Protokoll anzugeben.  Sie können die Datumsangaben im Format MM/TT/JJJJ eingeben oder auf das Symbol **Kalender** klicken und das Datum im Kalender auswählen.

### <a name="change-the-roles-included-in-the-log"></a>Ändern der Rollen im Protokoll
Aktivieren oder deaktivieren Sie das Kontrollkästchen **Rolle** neben jeder Rolle, die im Protokoll enthalten bzw. nicht enthalten sein soll.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]




<!--HONumber=Nov16_HO3-->


