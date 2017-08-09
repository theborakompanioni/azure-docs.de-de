---
title: Portale zum Erstellen und Bearbeiten von Protokollabfragen in Azure Log Analytics | Microsoft-Dokumentation
description: "In diesem Artikel werden die Portale beschrieben, die Sie zum Erstellen und Bearbeiten von Protokollsuchen in Azure Log Analytics verwenden können."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: b415141f152e88e13678c464eddc4f73df2da54e
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Portale zum Erstellen und Bearbeiten von Protokollabfragen in Azure Log Analytics

> [!NOTE]
> Dieser Artikel beschreibt Portale in Azure Log Analytics, in denen die neue Abfragesprache verwendet wird.  Weitere Informationen zur neuen Sprache und zum Upgradeverfahren für den Arbeitsbereich finden Sie unter [Durchführen eines Upgrades für den Azure Log Analytics-Arbeitsbereich auf die neue Protokollsuche](log-analytics-log-search-upgrade.md).  
>
> Wenn für Ihren Arbeitsbereich noch kein Upgrade auf die neue Abfragesprache durchgeführt wurde, finden Sie unter [Suchen von Daten mithilfe der Protokollsuche in Log Analytics](log-analytics-log-searches.md) Informationen zur aktuellen Version des Portals für die Protokollsuche.

Protokollsuchen werden in Log Analytics in zahlreichen Bereichen zum Abrufen von Daten aus dem Arbeitsbereich verwendet.  Um tatsächlich Abfragen zu erstellen und zu bearbeiten und gleichzeitig jedoch interaktiv mit zurückgegebenen Daten zu arbeiten, haben Sie zwei Optionen, die im Folgenden beschrieben werden.  

## <a name="log-search-portal"></a>Portal für die Protokollsuche
Das Portal für die Protokollsuche kann über das Azure-Portal oder das OMS-Portal aufgerufen werden.  Es dient zum Erstellen von grundlegenden Abfragen, die in einer einzelnen Zeile erstellt werden können.  Das Portal für die Protokollsuche kann verwendet werden, ohne ein externes Portal starten zu müssen. Zudem können Sie damit eine Vielzahl von Funktionen mit Protokollsuchen durchführen, einschließlich der Erstellung von Warnungsregeln, der Erstellung von Computergruppen und des Exports der Abfrageergebnisse.  

Das Portal für die Protokollsuche stellt verschiedene Funktionen für die Bearbeitung der Abfrage bereit – dabei sind keine umfassenden Kenntnisse bezüglich der Abfragesprache erforderlich.  Unter [Erstellen von Protokollsuchen in Azure Log Analytics mithilfe des Portals für die Protokollsuche](log-analytics-log-search-log-search-portal.md) finden Sie eine Zusammenfassung dieser neuen Features.


![Portal für die Protokollsuche](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Advanced Analytics-Portal
Das Advanced Analytics-Portal ist ein dediziertes Portal mit erweiterten Funktionen, die nicht im Portal für die Protokollsuche verfügbar sind.  Zu den Funktionen zählen die Möglichkeiten, eine Abfrage in mehreren Zeilen zu bearbeiten, Codes selektiv auszuführen, kontextabhängige IntelliSense-Funktionen auszuführen und intelligente Analysen durchzuführen.  Das Advanced Analytics-Portal ist ideal für die Entwicklung komplexer Abfragen geeignet, die entweder als Protokollsuche gespeichert oder kopiert und in andere Log Analytics-Elemente eingefügt werden.  Das Advanced Analytics-Portal wird über einen Link im Portal für die Protokollsuche gestartet.

![Advanced Analytics-Portal](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


Aufgrund der erweiterten Funktionen wird das Advanced Analytics-Portal in der Regel als primäres Tool für die Erstellung und Bearbeitung von Abfragen verwendet.  Nachdem Sie sich vergewissert haben, dass die Abfrage erwartungsgemäß funktioniert, kopieren Sie sie, und fügen Sie sie an anderer Stelle ein, z.B. im Portal für die Protokollsuche oder im Ansicht-Designer.  Da das Advanced Analytics-Portal jedoch Abfragen mit mehreren Zeilen unterstützt, müssen Sie beim Kopieren einer Abfrage aus diesem Portal Folgendes berücksichtigen.

- Kommentare müssen aus der Abfrage entfernt werden, bevor sie kopiert und an anderer Stelle eingefügt werden.  Sie können eine Zeile durch zwei vorangestellte Schrägstriche (//) kommentieren.  Wenn Sie eine Abfrage mit mehreren Zeilen in eine einzelne Zeile einfügen, werden Zeilenumbrüche entfernt.  Wenn Kommentare eingeschlossen werden, werden alle Zeichen nach dem ersten Kommentar als Bestandteil des Kommentars behandelt.


## <a name="next-steps"></a>Nächste Schritte

- Absolvieren Sie zum Erstellen von Abfragen das Tutorial zur Verwendung des [Portals für die Protokollsuche](log-analytics-log-search-log-search-portal.md) oder des [Advanced Analytics-Portals](https://docs.loganalytics.io/learn/tutorial_getting_started_with_analytics_portal.html).
- Sehen Sie sich ein [Tutorial zum Schreiben von Abfragen](https://docs.loganalytics.io/learn/tutorial_getting_started_with_queries.html) mit der neuen Abfragesprache an.

