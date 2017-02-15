---
title: "Warnungsverwaltung in Microsoft-Überwachungsprodukten | Microsoft Docs"
description: "Mit einer Warnung wird auf ein Problem hingewiesen, für das die Aufmerksamkeit eines Administrators erforderlich ist.  In diesem Artikel werden die Unterschiede der Erstellung und Verwaltung von Warnungen in System Center Operations Manager (SCOM) und Log Analytics beschrieben. Außerdem enthält der Artikel die bewährten Methoden zur Nutzung der beiden Produkte für eine Alert Management-Hybridstrategie."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6572c3f8-78ca-4fa9-8fe1-d0b488590788
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/09/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: f2b0655bdddc162c7c8ef7d478b2ce1fd1993f15


---
# <a name="managing-alerts-with-microsoft-monitoring"></a>Verwalten von Warnungen per Microsoft-Überwachung
Mit einer Warnung wird auf ein Problem hingewiesen, für das die Aufmerksamkeit eines Administrators erforderlich ist.  Es gibt deutliche Unterschiede zwischen System Center Operations Manager (SCOM) und Log Analytics in Operations Management Suite (OMS), was die Erstellung von Warnungen, deren Verwaltung und Analyse sowie die Art der Benachrichtigung über die Erkennung eines schwerwiegenden Problems betrifft.

## <a name="alerts-in-operations-manager"></a>Warnungen in Operations Manager
Warnungen in SCOM werden mit individuellen Regeln oder Überwachungseinheiten generiert, um ein bestimmtes Problem anzuzeigen.  Eine Überwachungseinheit kann eine Warnung generieren, wenn ein Fehlerzustand eintritt. Mit einer Regel kann eine Warnung generiert werden, um auf ein schwerwiegendes Problem hinzuweisen, das sich nicht direkt auf die Integrität eines verwalteten Objekts bezieht.  Management Packs enthalten viele verschiedene Workflows, mit denen Warnungen für die verwaltete Anwendung oder den Dienst erstellt werden.  Ein Teil des Prozesses zur Konfiguration eines neuen Management Packs ist die Optimierung. Damit soll sichergestellt werden, dass Sie nicht übermäßig viele Warnungen für Probleme erhalten, die von Ihnen nicht als schwerwiegend angesehen werden.

![SCOM-Warnungsansicht](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM bietet eine vollständige Warnungsverwaltung mit Warnungen, deren Status von Administratoren geändert werden kann, während sie an der Behebung des Problems arbeiten.  Nachdem das Problem gelöst wurde, legt der Administrator die Warnung auf „Geschlossen“ fest, und sie wird nicht mehr in Ansichten angezeigt, in denen aktive Warnungen aufgeführt sind.  Warnungen, die über Überwachungseinheiten generiert werden, können automatisch gelöscht werden, wenn die Überwachungseinheit in den fehlerfreien Zustand zurückkehrt.

![Warnungsstatus](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Warnungen in Log Analytics
Eine Warnung wird in Log Analytics aus einer Protokollabfrage erstellt, die in regelmäßigen Abständen automatisch ausgeführt wird.  Sie können aus einer Protokollabfrage eine Warnungsregel erstellen.  Wenn die von der Abfrage zurückgegebenen Ergebnisse mit den Kriterien übereinstimmen, die Sie angegeben haben, wird eine Warnung erstellt.  Dies kann eine spezielle Abfrage sein, bei der eine Warnung erstellt wird, wenn ein bestimmtes Ereignis erkannt wird. Sie können aber auch eine allgemeinere Abfrage verwenden, mit der nach Fehlerereignissen für eine bestimmte Anwendung gesucht wird.

Log Analytics-Warnungen werden als Ereignis in das OMS-Repository geschrieben und können mit einer Protokollabfrage abgerufen werden.  Sie haben nicht wie SCOM-Ereignisse einen Status, sodass Sie angeben können, wann das Problem gelöst wurde.

![OMS-Warnung](media/operations-management-suite-monitoring-alerts/oms-alert.png)

Sofern SCOM als Datenquelle für Log Analytics verwendet wird, werden SCOM-Warnungen in das OMS-Repository geschrieben, wenn sie erstellt und geändert werden.  

![SCOM-Warnung](media/operations-management-suite-monitoring-alerts/scom-alert.png)

Die [Lösung „Alert Management“](http://technet.microsoft.com/library/mt484092.aspx) liefert eine Zusammenfassung aktiver Warnungen und mehrere häufig verwendete Abfragen, um unterschiedliche Gruppen von Warnungen abzurufen.  So erhalten Sie eine effektivere Analyse Ihrer Warnungen als mit einem Bericht in SCOM.  Sie können über die Zusammenfassung auf ausführlichere Daten zugreifen und Ad-Hoc-Abfragen erstellen, um unterschiedliche Gruppen von Warnungen abzurufen.

![Lösung „Alert Management“](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Benachrichtigungen
Bei der Benachrichtigungsfunktion in SCOM erhalten Sie eine E-Mail oder SMS, wenn Warnungen bestimmte Kriterien erfüllen.  Sie können verschiedene Benachrichtigungsabonnements erstellen, mit denen abhängig von verschiedenen Kriterien wie z.B. dem überwachten Objekt, dem Schweregrad der Warnung, der Art des erkannten Problems oder der Tageszeit verschiedene Personen benachrichtigt werden.

Es sind nur wenige Abonnements erforderlich, um eine vollständige Benachrichtigungsstrategie für eine große Zahl von Management Packs zu implementieren.

![SCOM-Warnungen](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Sie können von Log Analytics per E-Mail darüber informiert werden, dass eine Warnung erstellt wurde, indem Sie für jede [Warnungsregel](http://technet.microsoft.com/library/mt614775.aspx)eine Aktion zur Benachrichtigung per E-Mail festlegen.  Es ist nicht wie bei SCOM möglich, mit einer Regel mehrere Warnungen zu abonnieren.  Außerdem müssen Sie Ihre eigenen Warnungsregeln erstellen, da von OMS keine vorkonfigurierten Regeln bereitgestellt werden.

![Log Analytics-Warnungen](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

Sie können SCOM-Warnungen aber nicht vollständig in Log Analytics verwalten, da sie nur in der Betriebskonsole geändert werden können.  Log Analytics ist als Teil des Prozesses zur Verwaltung von Warnungen aber durchaus hilfreich, indem Analysetools bereitgestellt werden, die in SCOM allein nicht enthalten sind.

## <a name="alert-remediation"></a>Beheben von Warnungen
[Behebung](http://technet.microsoft.com/library/mt614775.aspx) bezieht sich auf den Versuch, das von einer Warnung identifizierte Problem automatisch zu beheben.

Mit SCOM können Sie die Diagnose und Wiederherstellungen durchführen, um zu reagieren, wenn eine Überwachungseinheit in einen Fehlerzustand wechselt.  Dies passiert gleichzeitig mit der Erstellung der Warnung durch die Überwachungseinheit.  Die Diagnose und Wiederherstellungen werden in der Regel als Skript implementiert, das auf dem Agent ausgeführt wird.  Bei einer Diagnose wird versucht, weitere Informationen zum erkannten Problem zu sammeln, und bei einer Wiederherstellung wird versucht, das Problem zu beheben.

Mit Log Analytics können Sie als Reaktion auf eine Log Analytics-Warnung ein [Azure Automation-Runbook](https://azure.microsoft.com/documentation/services/automation/) starten oder einen Webhook aufrufen.  Runbooks können eine komplexe Logik enthalten, die in PowerShell implementiert ist.  Das Skript wird in Azure ausgeführt und ermöglicht den Zugriff auf alle Azure-Ressourcen oder externen Ressourcen, die in der Cloud verfügbar sind.  Azure Automation verfügt über die Funktion zum Ausführen von Runbooks auf einem Server in Ihrem lokalen Rechenzentrum, aber dieses Feature ist derzeit nicht verfügbar, wenn das Runbook als Reaktion auf Log Analytics-Warnungen gestartet wird.

Sowohl Wiederherstellungen in SCOM als auch Runbooks in OMS können PowerShell-Skripts aufweisen. Wiederherstellungen sind aber schwieriger zu erstellen und zu verwalten, da sie in einem Management Pack enthalten sein müssen.  Runbooks werden in Azure Automation gespeichert. Die Anwendung verfügt über Features zum Erstellen, Testen und Verwalten von Runbooks.

Wenn Sie SCOM als Datenquelle für Log Analytics verwenden, können Sie eine Log Analytics-Warnung mit einer Protokollabfrage erstellen, um im OMS-Repository gespeicherte SCOM-Warnungen abzurufen.  Dies ermöglicht Ihnen die Ausführung eines Azure Automation-Runbooks als Reaktion auf eine SCOM-Warnung.  Da das Runbook in Azure ausgeführt wird, ist dies keine wettbewerbsfähige Strategie für Wiederherstellungen bei lokalen Problemen.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich näher über [Warnungen in System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).




<!--HONumber=Nov16_HO3-->


