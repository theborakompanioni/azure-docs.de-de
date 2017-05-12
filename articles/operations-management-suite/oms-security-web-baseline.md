---
title: "Sicherheits- und Überwachungslösung von Operations Management Suite – Web-Baseline | Microsoft-Dokumentation"
description: "In diesem Dokument wird beschrieben, wie Sie die Sicherheits- und Überwachungslösung von OMS verwenden, um eine Web-Baselinebewertung aller überwachten Webserver zu Konformitäts- und Sicherheitszwecken durchzuführen."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ROBOTS: NOINDEX
redirect_url: https://www.microsoft.com/cloud-platform/security-and-compliance
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 0d4707dc0c0ffbf40d0d10a6d12b9709a9655258
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Web-Baselinebewertung in der Sicherheits- und Überwachungslösung von Operations Management Suite
In diesem Dokument erhalten Sie Informationen zur Verwendung der Web-Baselinebewertung der [Sicherheits- und Überwachungslösung von Operations Management Suite (OMS)](operations-management-suite-overview.md), um auf den Zustand Ihrer überwachten Ressourcen zuzugreifen.

## <a name="what-is-web-baseline-assessment"></a>Was ist eine Web-Baselinebewertung?
Derzeit ermöglicht die OMS-Sicherheitslösung eine Bewertung der Sicherheitsbaseline für Betriebssysteme. Dabei werden die Betriebssystemeinstellungen Ihrer Server alle 24 Stunden gescannt, und potenziell anfällige Einstellungen werden angezeigt. Weitere Informationen hierzu finden Sie unter [Baselinebewertung in der Sicherheits- und Überwachungslösung von Operations Management Suite](oms-security-baseline.md).

Das Ziel der Web-Baselinebewertung besteht darin, potenziell anfällige Webservereinstellungen zu finden. Die drei Hauptquellen für die Web-Baselinekonfigurationen sind die .NET-, ASP.NET- und IIS-Konfigurationen.  Wie bei der Baselinebewertung für Betriebssysteme auch, werden bei der OMS-Sicherheitslösung alle 24 Stunden Ihre Webserver gescannt, und anschließend wird der entsprechende Sicherheitszustand angezeigt.  In Internetinformationsdienste (IIS) können Konfigurationen stark angepasst werden, sodass verschiedene Standort- und Anwendungsebenen außer Kraft gesetzt werden können. Die Scanfunktion überprüft nicht nur die Einstellungen auf der Standardstammebene, sondern auch auf allen Anwendungs- und Standortebenen. Auf diese Weise können Sie potenzielle anfällige Einstellungen ermitteln und schnell Maßnahmen ergreifen.


## <a name="web-security-baseline-assessment"></a>Bewertung der Websicherheitsbaseline
In dieser Vorschau wird auf dieses Feature mit der OMS-Suchfunktion zugegriffen. Führen Sie die folgenden Schritte aus, um die entsprechende Abfrage durchzuführen:

1. Klicken Sie im Hauptdashboard der **Microsoft Operations Management Suite** auf die Kachel **Sicherheit und Überwachung**.
2. Klicken Sie im Dashboard **Sicherheit und Überwachung** auf die Schaltfläche **Protokollsuche**.
3. Die erste Abfrage, die Sie verwenden können, ist die **Web Baseline Assessment Summary** (Zusammenfassung der Web-Baselinebewertung). Geben Sie im Feld **Begin search here** (Suche hier beginnen) die folgende Abfrage ein: Type*=SecurityBaselineSummary BaselineType=web*. Die folgende Bildschirmansicht enthält ein Beispiel für die Ausgabe:

![Zusammenfassung der Web-Baselinebewertung](./media/oms-security-web-baseline/oms-security-web-baseline-fig1-new.png)

> [!NOTE]
> Bei dieser Abfrage steht jeder Datensatz für die Zusammenfassung der Bewertung auf einem bestimmten Server.

Wenn Sie sich in der **Protokollsuche** befinden, können Sie verschiedene Abfragen eingeben, um weitere Informationen zur Web-Baselinebewertung zu erhalten. Zusätzlich zur obigen Abfrage können Sie in dieser Vorschau auch die unten angegebenen Abfragen verwenden.

**Web Baseline Rule Assessment** (Bewertung einer Web-Baselineregel): Jeder Datensatz steht für die Bewertung einer einzelnen Web-Baselineregel auf einem Server. Er enthält alle Daten in Bezug auf Regel, Standort, erwartetes Ergebnis und tatsächliches Ergebnis.

**Abfrage**: Type*=SecurityBaseline BaselineType=web*

![Bewertung der Web-Baselineregel](./media/oms-security-web-baseline/oms-security-web-baseline-fig2.png)

**Show all results for a specific server** (Alle Ergebnisse für einen bestimmten Server anzeigen): Mit dieser Abfrage zeigen Sie die Ergebnisse für einen bestimmten Server an.

**Abfrage**: *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*

![Alle Ergebnisse](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

Sie können diese Datensätze bzw. Abfragen auch verwenden, um Ihre eigenen Dashboards, Berichte oder Warnungen zu erstellen. Die Bildschirmansicht unten enthält ein UI-Beispielsteuerelement, das Sie Ihrem Dashboard hinzufügen können. Informationen dazu, wie Sie Ihre Daten mit dem Ansicht-Designer von OMS visualisieren, finden Sie [hier](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/). Die Bildschirmansicht unten enthält ein Beispiel dafür, wie die Kachel nach dieser Anpassung aussieht.

![Beispielbenutzeroberfläche](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

> [!NOTE]
> Falls Sie wissen möchten, welche Einstellungen für die Baselinebewertung überprüft werden, können Sie [diese Excel-Tabelle](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) mit den entsprechenden Einstellungen herunterladen.

## <a name="see-also"></a>Weitere Informationen
In diesem Dokument wurde die Web-Baselinebewertung der Sicherheits- und Überwachungslösung von OMS beschrieben. Weitere Informationen zur Sicherheitslösung von OMS finden Sie in den folgenden Artikeln:

* [Operations Management Suite (OMS) – Übersicht](operations-management-suite-overview.md)
* [Überwachen von und Reagieren auf Sicherheitswarnungen in der Sicherheits- und Überwachungslösung von Operations Management Suite](oms-security-responding-alerts.md)
* [Überwachen von Ressourcen in der Sicherheits- und Überwachungslösung von Operations Management Suite](oms-security-monitoring-resources.md)


