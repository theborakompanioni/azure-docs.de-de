---
title: "Web-Baselinebewertung in der Sicherheits- und Überwachungslösung von Operations Management Suite | Microsoft-Dokumentation"
description: "In diesem Dokument erfahren Sie, wie Sie die Web-Baselinebewertung in der Sicherheits- und Überwachungslösung von OMS verwenden, um zu Compliance- und Sicherheitszwecken eine Baselinebewertung aller überwachten Webserver durchzuführen."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/28/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: 8be49b182df675fe3235d148b87379e1dff3a384
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Web-Baselinebewertung in der Sicherheits- und Überwachungslösung von Operations Management Suite
In diesem Dokument erfahren Sie, wie Sie die Web-Baselinebewertungsfunktionen der OMS-Sicherheits- und -Überwachungslösung verwenden, um auf den sicheren Zustand Ihrer überwachten Ressourcen zuzugreifen.

## <a name="what-is-web-baseline-assessment"></a>Was ist eine Web-Baselinebewertung?
Derzeit ermöglicht die OMS-Sicherheitslösung eine Bewertung der Sicherheitsbaseline für Betriebssysteme. Dabei werden die Betriebssystemeinstellungen Ihrer Server alle 24 Stunden gescannt, und potenziell anfällige Einstellungen werden angezeigt. Weitere Informationen hierzu finden Sie unter [Baselinebewertung in der Sicherheits- und Überwachungslösung von Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/oms-security-baseline).

Das Ziel der Web-Baselinebewertung besteht darin, potenziell anfällige Webservereinstellungen zu finden. Die drei Hauptquellen für die Web-Baselinekonfigurationen sind die .NET-, ASP.NET- und IIS-Konfigurationen.  Wie bei der Baselinebewertung für Betriebssysteme auch, werden bei der OMS-Sicherheitslösung alle 24 Stunden Ihre Webserver gescannt, und anschließend wird der entsprechende Sicherheitszustand angezeigt.  In Internetinformationsdienste (IIS) können Konfigurationen stark angepasst werden, sodass verschiedene Standort- und Anwendungsebenen außer Kraft gesetzt werden können. Die Scanfunktion überprüft nicht nur die Einstellungen auf der Standardstammebene, sondern auch auf allen Anwendungs- und Standortebenen. Dadurch können Sie potenziell anfällige Einstellungen ermitteln und schnell beheben sowie unsere Empfehlungen für diese Einstellungen implementieren.


## <a name="web-security-baseline-assessment"></a>Bewertung der Websicherheitsbaseline

Für diese Vorschau können Sie auf das Feature über die OMS-Suchoption sowie über das Sicherheits- und Überwachungs-Dashboard von OMS zugreifen. Führen Sie die folgenden Schritte aus, um die entsprechende Abfrage durchzuführen:

1. Klicken Sie im Hauptdashboard von **Microsoft Operations Management Suite** auf die Kachel **Security and Audit** (Sicherheit und Überwachung).
2. Auf dem Dashboard **Sicherheit und Überwachung** werden Web-Baseline und Betriebssystem-Baseline nebeneinander angezeigt.
   
    ![Bewertung der Websicherheitsbaseline der Sicherheits- und Überwachungslösung von OMS](./media/oms-security-web-baseline/oms-security-web-baseline-fig5.png)

3. Der linke Bereich zeigt die Anzahl von Webservern relativ zur Basislinie, den durchschnittlichen Prozentsatz erfolgreicher Regeln auf allen ausgewerteten Servern und die Liste mit den ausgewerteten Servern an.
4. Im rechten Bereich werden die einzelnen nicht erfolgreichen Regeln nach *Schweregrad* und *Regeltyp* aufgeführt. Wenn Sie im rechten Bereich auf eine der Regeln klicken, erhalten Sie weitere Informationen zu der Regel. Das Bild weiter unten zeigt ein entsprechendes Beispiel. Die ausgewertete Regel ist unter *Regeleinstellung* angegeben. Das Feld *AzId* enthält einen eindeutigen Bezeichner für jede Regel, die von Microsoft zum Nachverfolgen von Baselineregeln verwendet wird. Darüber hinaus werden das *erwartete Ergebnis* (von Microsoft empfohlener Wert) sowie weitere Details zu den Sicherheitsauswirkungen der Regel angezeigt.
    
    ![Abfrage](./media/oms-security-web-baseline/oms-security-web-baseline-fig6.png)

5. Sie können eigene Abfragen zur Überprüfung der Ergebnisse erstellen. 

Die erste Abfrage, die Sie verwenden können, ist die **Web Baseline Assessment Summary** (Zusammenfassung der Web-Baselinebewertung). Geben Sie im Feld **Begin search here** (Suche hier beginnen) die folgende Abfrage ein: *Type=SecurityBaselineSummary BaselineType=Web*. Beispiel für die Ausgabe:

![Abfrageergebnis](./media/oms-security-web-baseline/oms-security-web-baseline-fig7.png)

>[!NOTE] 
>Bei dieser Abfrage steht jeder Datensatz für die Zusammenfassung der Bewertung auf einem bestimmten Server.

Wenn Sie sich in der **Protokollsuche** befinden, können Sie verschiedene Abfragen eingeben, um weitere Informationen zur Web-Baselinebewertung zu erhalten. Zusätzlich zur obigen Abfrage können Sie in dieser Vorschau auch die folgenden Abfragen verwenden:

**Web Baseline Rule Assessment** (Bewertung einer Web-Baselineregel): Jeder Datensatz steht für die Bewertung einer einzelnen Web-Baselineregel auf einem Server. Er enthält alle Daten für eine nicht erfolgreiche Regel, den Standortpfad (*SitePath*), auf dessen Grundlage die Regel ausgewertet wurde, sowie *das erwartete* und *das tatsächliche Ergebnis*.

Abfrage: *Type=SecurityBaseline BaselineType=Web AnalyzeResult=Failed*

![Abfrageergebnis 2](./media/oms-security-web-baseline/oms-security-web-baseline-fig8.png)

**Show all results for a specific server** (Alle Ergebnisse für einen bestimmten Server anzeigen): Mit dieser Abfrage zeigen Sie die Ergebnisse für einen bestimmten Server an. Abfrage: *Type=SecurityBaseline BaselineType=Web Computer=BaselineTestVM1*

![Abfrageergebnis 3](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

Sie können diese Datensätze bzw. Abfragen verwenden, um Ihre eigenen Dashboards, Berichte oder Warnungen zu erstellen. Hier sehen Sie ein UI-Beispielsteuerelement, das Sie Ihrem Dashboard hinzufügen können. Informationen dazu, wie Sie Ihre Daten mit dem Ansicht-Designer von OMS visualisieren, finden Sie [hier](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/). Die Bildschirmansicht unten enthält ein Beispiel dafür, wie die Kachel nach dieser Anpassung aussieht.

![Dashboard](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

## <a name="see-also"></a>Weitere Informationen
In diesem Dokument wurde die Web-Baselinebewertung der Sicherheits- und Überwachungslösung von OMS beschrieben. Weitere Informationen zur Sicherheitslösung von OMS finden Sie in den folgenden Artikeln:

* [Operations Management Suite (OMS) – Übersicht](operations-management-suite-overview.md)
* [Überwachen von und Reagieren auf Sicherheitswarnungen in der Sicherheits- und Überwachungslösung von Operations Management Suite](oms-security-responding-alerts.md)
* [Überwachen von Ressourcen in der Sicherheits- und Überwachungslösung von Operations Management Suite](oms-security-monitoring-resources.md)


