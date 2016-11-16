---
title: Was ist Log Analytics? | Microsoft Docs
description: "Log Analytics ist ein Dienst in der Operations Management Suite (OMS), mit dem Sie betriebsbezogene Daten sammeln und analysieren können, die von Ressourcen in Ihren Cloud- und lokalen Umgebungen generiert werden.  Dieser Artikel bietet eine kurze Übersicht über die verschiedenen Komponenten von Log Analytics sowie Links zu weiteren Informationen."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: bd90b460-bacf-4345-ae31-26e155beac0e
ms.service: log-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/06/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 09a2b3ccd2048ab2638dd91d1905483f10d6d339


---
# <a name="what-is-log-analytics"></a>Was ist Log Analytics?
Log Analytics ist ein Dienst in der [Operations Management Suite \(OMS\)](../operations-management-suite/operations-management-suite-overview.md), mit dem Sie Daten sammeln und analysieren können, die von Ressourcen in Ihren Cloud- und lokalen Umgebungen generiert werden. Der Dienst bietet Echtzeiteinblicke mithilfe integrierter Suchfunktionen und benutzerdefinierter Dashboards, sodass Sie Millionen von Datensätzen über all Ihre Workloads und Server hinweg unabhängig vom physischen Standort analysieren können.

## <a name="log-analytics-components"></a>Log Analytics-Komponenten
Im Mittelpunkt von Log Analytics steht das OMS-Repository, das in der Azure-Cloud gehostet wird.  Konfigurieren Sie Datenquellen und fügen Sie Ihrem Abonnement Lösungen hinzu, um Daten aus verbundenen Quellen im Repository zu sammeln.  Datenquellen und Lösungen erzeugen verschiedene Arten von Datensätzen, die jeweils über einen eigenen Eigenschaftensatz verfügen, aber dennoch in Abfragen im Repository gemeinsam analysiert werden können.  So können Sie die gleichen Tools und Methoden für verschiedene Arten von Daten verwenden, die aus verschiedenen Quellen gesammelt werden.

![OMS-Repository](media/log-analytics-overview/overview.png)

Bei den verbundenen Quellen handelt es sich um die Computer und anderen Ressourcen, die die Daten generieren, die von Log Analytics gesammelt werden.  Hierzu können Agents gehören, die auf direkt verbundenen [Windows](log-analytics-windows-agents.md)- oder [Linux](log-analytics-linux-agents.md)-Computern installiert sind, oder Agents in einer [verbundenen System Center Operations Manager-Verwaltungsgruppe](log-analytics-om-agents.md).  Log Analytics kann auch Daten aus [Azure-Speichern](log-analytics-azure-storage.md)sammeln.

[Datenquellen](log-analytics-data-sources.md) sind die verschiedenen Arten von Daten, die aus jeder verbundenen Quelle gesammelt werden.  Hierzu gehören Ereignisse und [Leistungsdaten](log-analytics-data-sources-performance-counters.md) aus [Windows](log-analytics-data-sources-windows-events.md)- und Linux-Agents sowie Quellen wie beispielsweise [IIS-Protokolle](log-analytics-data-sources-iis-logs.md) und [benutzerdefinierte Textprotokolle](log-analytics-data-sources-custom-logs.md).  Sie konfigurieren jede Datenquelle, aus der Sie Daten sammeln möchten, und die Konfiguration wird automatisch an jede verbundene Quelle übermittelt.

## <a name="analyzing-log-analytics-data"></a>Analysieren von Log Analytics-Daten
Der größte Teil der Interaktion mit Log Analytics erfolgt über das OMS-Portal, das in jedem Browser ausgeführt werden kann und Ihnen Zugriff auf Konfigurationseinstellungen und verschiedene Tools bietet, mit denen Sie die gesammelten Daten analysieren und mit den Daten agieren können.  Im Portal können Sie mithilfe von [Protokollsuchvorgängen](log-analytics-log-searches.md) Abfragen erstellen, um die gesammelten Daten zu analysieren. Sie können [Dashboards](log-analytics-dashboards.md) nutzen und Ihre wichtigsten Suchvorgänge mit grafischen Ansichten anpassen. Darüber hinaus bieten [Lösungen](log-analytics-add-solutions.md) zusätzliche Funktionalität und Analysemöglichkeiten.

![OMS-Portal](media/log-analytics-overview/portal.png)

Log Analytics stellt eine Abfragesyntax bereit, mit der Sie Daten schnell abrufen und im Repository konsolidieren können.  Sie können [Protokollsuchvorgänge](log-analytics-log-searches.md) erstellen und speichern, um Daten direkt im OMS-Portal zu analysieren. Sie können auch Protokollsuchvorgänge automatisch ausführen, um Warnungen zu generieren, wenn die Ergebnisse einer Abfrage auf ein wichtiges Problem hindeuten.

![Protokollsuche](media/log-analytics-overview/log-search.png)

Um schnell eine grafische Übersicht der Integrität Ihrer gesamten Umgebung zu erhalten, können Sie Ihrem [Dashboard](log-analytics-dashboards.md)Visualisierungen der gespeicherten Suchvorgänge hinzufügen.   

![Dashboard](media/log-analytics-overview/dashboard.png)

Um Daten außerhalb von Log Analytics zu analysieren, können Sie die Daten aus dem OMS-Repository in Tools wie z.B. [Power BI](log-analytics-powerbi.md) oder Excel exportieren.  Sie können auch die [Protokollsuch-API](log-analytics-log-search-api.md) verwenden, um benutzerdefinierte Lösungen zu erstellen, die Log Analytics-Daten nutzen, oder um sie in andere Systeme zu integrieren.

## <a name="solutions"></a>Lösungen
Lösungen fügen Log Analytics weitere Funktionalität hinzu.  Sie werden primär in der Cloud ausgeführt und ermöglichen die Analyse von Daten, die im OMS-Repository gesammelt wurden. Sie können auch neue Datensatztypen definieren, die gesammelt und mit Protokollsuchvorgängen oder über eine zusätzliche Benutzeroberfläche analysiert werden können, die von der Lösung im OMS-Dashboard bereitgestellt wird.  

![Lösung für die Änderungsnachverfolgung](media/log-analytics-overview/change-tracking.png)

Es gibt Lösungen für eine Vielzahl von Funktionen, und Sie können ganz einfach nach verfügbaren Lösungen suchen und sie aus dem Lösungskatalog [zum OMS-Workspace hinzufügen](log-analytics-add-solutions.md) .  Viele Lösungen werden automatisch bereitgestellt und können sofort verwendet werden, während andere möglicherweise einige Konfigurationseinstellungen erfordern.

![Lösungskatalog](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-architecture"></a>Log Analytics-Architektur
Die Bereitstellungsanforderungen von Log Analytics sind minimal, da die zentralen Komponenten in der Azure-Cloud gehostet werden.  Dies umfasst auch das Repository sowie die Dienste, mit deren Hilfe Sie gesammelte Daten korrelieren und analysieren können.  Sie können in jedem Browser auf das Portal zugreifen, Clientsoftware wird nicht benötigt.

Sie müssen Agents auf [Windows](log-analytics-windows-agents.md)- und [Linux](log-analytics-linux-agents.md)-Computern installieren. Für Agents, die bereits zu einer [verbundenen SCOM-Verwaltungsgruppe](log-analytics-om-agents.md) gehören, ist kein zusätzlicher Agent erforderlich.  SCOM-Agents kommunizieren weiterhin mit Verwaltungsservern, die ihre Daten an Log Analytics weiterleiten.  Bei einigen Lösungen ist es jedoch erforderlich, dass Agents direkt mit Log Analytics kommunizieren.  Informationen zu den Kommunikationsanforderungen finden Sie in der Dokumentation zu den jeweiligen Lösungen.

Wenn Sie sich [bei Log Analytics registrieren](log-analytics-get-started.md), erstellen Sie einen OMS-Arbeitsbereich.  Sie können sich diesen Arbeitsbereich als einzigartige OMS-Umgebung mit eigenem Datenrepository, eigenen Datenquellen und eigenen Lösungen vorstellen. Sie können mehrere Arbeitsbereiche in Ihrem Abonnement erstellen, um mehrere Umgebungen z.B. zu Produktions- und Testzwecken zu unterstützen.

![Log Analytics-Architektur](media/log-analytics-overview/architecture.png)

## <a name="next-steps"></a>Nächste Schritte
* [Registrieren Sie sich für ein kostenloses Log Analytics-Konto](log-analytics-get-started.md) , um es in Ihrer eigenen Umgebung auszuprobieren.
* Sehen Sie sich die verschiedenen verfügbaren [Datenquellen](log-analytics-data-sources.md) an, aus denen Daten für das OMS-Repository gesammelt werden können.
* [Durchsuchen Sie die verfügbaren Lösungen im Lösungskatalog](log-analytics-add-solutions.md) , um Funktionen zu Log Analytics hinzuzufügen.




<!--HONumber=Nov16_HO2-->


