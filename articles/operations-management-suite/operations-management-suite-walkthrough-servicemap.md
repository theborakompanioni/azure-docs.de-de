---
title: "Service Map-Lösung – Demo mit individuellem Arbeitstempo | Microsoft-Dokumentation"
description: "Service Map ist eine Lösung der Operations Management Suite (OMS), die Anwendungskomponenten auf Windows- und Linux-Systemen automatisch ermittelt und die Kommunikation zwischen Diensten abbildet.  Dies ist eine Demo mit individuellem Arbeitstempo, bei der schrittweise beschrieben wird, wie Sie Service Map zum Identifizieren und Diagnostizieren eines simulierten Problems in einer Webanwendung verwenden."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 7f469fb309f92b86dbf289d3a0462ba9042af48a
ms.openlocfilehash: c3548d24c74f8ad865b22d6af3490d0b5cc77a84
ms.lasthandoff: 04/13/2017


---

# <a name="operations-management-suite-oms-self-paced-demo---service-map"></a>OMS-Demo (Operations Management Suite) mit individuellem Arbeitstempo – Service Map
Dies ist eine Demo mit individuellem Arbeitstempo, in der die Nutzung der [Service Map-Lösung](operations-management-suite-service-map.md) in der Operations Management Suite (OMS) schrittweise beschrieben wird, um ein simuliertes Problem in einer Webanwendung zu identifizieren und zu diagnostizieren.  Service Map ermittelt automatisch Anwendungskomponenten auf Windows- und Linux-Systemen und stellt die Kommunikation zwischen Diensten dar.  Außerdem werden Daten zusammengefasst, die von anderen OMS-Dienten gesammelt werden, um Sie beim Analysieren der Leistung und Identifizieren von Problemen zu unterstützen.  Außerdem nutzen Sie [Protokollsuchen in Log Analytics](../log-analytics/log-analytics-log-searches.md), um ein Drilldown für erfasste Daten durchzuführen und das zugrunde liegende Problem zu identifizieren.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Sie haben soeben eine Benachrichtigung erhalten, dass für die ACME Customer Portal-Anwendung Leistungsprobleme bestehen.  Sie verfügen nur über die Information, dass diese Probleme heute um 4:00 Uhr morgens (PST) begonnen haben.  Sie wissen nicht genau, von welchen Komponenten das Portal abhängig ist (mit Ausnahme einer Gruppe von Webservern).  

## <a name="components-and-features-used"></a>Verwendete Komponenten und Features
- [Service Map-Lösung](operations-management-suite-service-map.md)
- [Log Analytics-Protokollsuchen](../log-analytics/log-analytics-log-searches.md)


## <a name="walk-through"></a>Exemplarische Vorgehensweise

### <a name="1-connect-to-the-oms-experience-center"></a>1. Herstellen einer Verbindung mit dem OMS Experience Center
In dieser exemplarischen Vorgehensweise wird das [Operations Management Suite Experience Center](https://experience.mms.microsoft.com/) genutzt, über das eine vollständige OMS-Umgebung mit Beispieldaten bereitgestellt wird. Verwenden Sie den obigen Link, geben Sie Ihre Informationen ein, und wählen Sie dann das Szenario **Insight & Analytics** aus.


### <a name="2-start-service-map"></a>2. Starten von Service Map
Starten Sie die Service Map-Lösung, indem Sie auf die Kachel **Service Map** klicken.

![Service Map-Kachel](media/operations-management-suite-walkthrough-servicemap/tile.png)

Die Service Map-Konsole wird angezeigt.  Im linken Bereich wird eine Liste mit den Computern Ihrer Umgebung angezeigt, für die der Service Map-Agent installiert wurde.  In dieser Liste wählen Sie den Computer aus, den Sie anzeigen möchten.

![Computerliste](media/operations-management-suite-walkthrough-servicemap/computer-list.png)


### <a name="3-view-computer"></a>3. Anzeigen des Computers
Wir wissen, dass die Webserver die Namen AcmeWFE001 und AcmeWFE002 haben. Dies ist also ein guter Ort, um zu beginnen.  Klicken Sie auf **AcmeWFE001**.  Die Zuordnung für AcmeWFE001 und alle Abhängigkeiten wird angezeigt.  Sie können anzeigen, welche Prozesse auf dem ausgewählten Computer ausgeführt werden und mit welchen externen Diensten sie kommunizieren.

![Webserver](media/operations-management-suite-walkthrough-servicemap/web-server.png)

Da es um die Leistung unserer Webanwendung geht, klicken wir auf den Prozess **AcmeAppPool (IIS App Pool)**.  Die Details zu diesem Prozess werden angezeigt, und die dazugehörigen Abhängigkeiten werden hervorgehoben.  

![App-Pool](media/operations-management-suite-walkthrough-servicemap/app-pool.png)


### <a name="4-change-time-window"></a>4. Ändern des Zeitfensters

Wir haben gehört, dass das Problem zuerst um 4:00 Uhr nachts aufgetreten ist. Daher sehen wir uns an, was zu dieser Zeit passiert ist. Klicken Sie auf **Zeitbereich**, und ändern Sie die Uhrzeit in „4:00 PST“ (aktuelles Datum beibehalten und Anpassung an Ihre lokale Zeitzone vornehmen) und eine Dauer von 20 Minuten.

![Zeitauswahl](./media/operations-management-suite-walkthrough-servicemap/time-picker.png)


### <a name="5-view-alert"></a>5. Anzeigen einer Warnung

Wir können erkennen, dass für die Abhängigkeit **acmetomcat** eine Warnung angezeigt wird. Dies ist also unser potenzielles Problem.  Klicken Sie auf das Warnsymbol unter **acmetomcat**, um die Details der Warnung anzuzeigen.  Wir können sehen, dass eine kritische CPU-Auslastung vorgelegen hat, und weitere Details dazu anzeigen.  Dies ist wahrscheinlich der Grund für die schlechte Leistung. 

![Warnung](./media/operations-management-suite-walkthrough-servicemap/alert.png)


### <a name="6-view-performance"></a>6. Anzeigen der Leistung

Wir sehen uns **acmetomcat** genauer an.  Klicken Sie rechts oberhalb von **acmetomcat**, und wählen Sie die Option **Serverübersicht laden**, um die Details und Abhängigkeiten für den Computer anzuzeigen. Anschließend können wir uns eingehender über diese Leistungsindikatoren informieren, um unseren Verdacht zu untermauern.  Wählen Sie die Registerkarte **Leistung**, um die [mit Log Analytics erfassten Leistungsindikatoren](../log-analytics/log-analytics-data-sources-performance-counters.md) für den Zeitbereich anzuzeigen.  Wir sehen, dass für den Prozessor und Arbeitsspeicher regelmäßige Spitzenauslastungen auftreten.

![Leistung](./media/operations-management-suite-walkthrough-servicemap/performance.png)


### <a name="7-view-change-tracking"></a>7. Anzeigen der Änderungsnachverfolgung
Wir möchten nun ermitteln, welchen Grund diese hohe Auslastung hat.  Klicken Sie auf die Registerkarte **Zusammenfassung**.  Sie enthält Informationen, die per OMS für den Computer erfasst wurden, z.B. Fehler bei der Verbindungsherstellung, kritische Warnungen und Softwareänderungen.  Abschnitte mit interessanten Informationen, die kürzlich erfasst wurden, sollten bereits erweitert sein. Sie können auch die anderen Abschnitte erweitern, um die darin enthaltenen Informationen anzuzeigen.


Erweitern Sie die Option **Änderungsnachverfolgung**, falls sie nicht bereits geöffnet ist.  Darunter werden Informationen angezeigt, die mit der [Lösung für die Änderungsnachverfolgung](../log-analytics/log-analytics-change-tracking.md) erfasst wurden.  Es sieht so aus, als ob in diesem Zeitfenster eine Softwareänderung vorgenommen wurde.  Klicken Sie auf **Software**, um die Details hierzu anzuzeigen.  Kurz nach 4:00 Uhr wurde dem Computer ein Sicherungsprozess hinzugefügt. Dies scheint der Grund für die übermäßige Ressourcennutzung zu sein.

![Änderungsnachverfolgung](./media/operations-management-suite-walkthrough-servicemap/change-tracking.png)



### <a name="8-view-details-in-log-search"></a>8. Anzeigen von Details in der Protokollsuche
Wir können dies weiter überprüfen, indem wir uns die ausführlichen Leistungsinformationen ansehen, die im Log Analytics-Repository erfasst wurden.  Klicken Sie erneut auf die Registerkarte **Warnungen** und dann auf eine der Warnungen unter **High CPU** (Hohe CPU-Auslastung).  Klicken Sie auf **In Protokollsuche anzeigen**.  Das Fenster „Protokollsuche“ wird geöffnet. In diesem Fenster können Sie [Protokollsuchen](../log-analytics/log-analytics-log-searches.md) für im Repository gespeicherte Daten durchführen.  Von Service Map wurde bereits eine Abfrage eingefügt, mit der wir die gewünschte Warnung abrufen können.  

![Protokollsuche](./media/operations-management-suite-walkthrough-servicemap/log-search.png)


### <a name="9-open-saved-search"></a>9. Öffnen der gespeicherten Suche
Wir möchten nun weitere Details zu der Erfassung von Leistungsdaten erhalten, bei der diese Warnung generiert wurde, und unseren Verdacht bestätigen, dass die Probleme von diesem Sicherungsprozess verursacht werden.  Ändern Sie den Zeitbereich in **6 Stunden**.  Klicken Sie anschließend auf **Favoriten**, und führen Sie einen Bildlauf nach unten zu den gespeicherten Suchen für **Service Map** durch.  Dies sind Abfragen, die wir speziell für diese Analyse erstellt haben.  Klicken Sie auf **Top 5 Processes by CPU for acmetomcat** (Top 5-Prozesse nach CPU für acmetomcat).

![Gespeicherte Suche](./media/operations-management-suite-walkthrough-servicemap/saved-search.png)


Diese Abfrage gibt eine Liste mit den Top 5 der Prozesse zurück, die den Prozessor für **acmetomcat** nutzen.  Sie können sich die Abfrage näher ansehen, um sich über die Abfragesprache zu informieren, die für Protokollsuchen verwendet wird.  Falls Prozesse auf anderen Computern für Sie interessant sind, können Sie die Abfrage entsprechend ändern, um diese Informationen abzurufen.

In diesem Fall ist erkennbar, dass der Sicherungsprozess ständig ca. 60% der CPU des App-Servers verbraucht.  Die Wahrscheinlichkeit ist hoch, dass dieser neue Prozess für unser Leistungsproblem verantwortlich ist.  Die Lösung sollte nun natürlich darin bestehen, diese neue Sicherungssoftware vom Anwendungsserver zu entfernen.  Wir können die Konfiguration für den gewünschten Zustand (Desired State Configuration, DSC) nutzen, die von Azure Automation verwaltet wird, um anhand von Richtlinien sicherzustellen, dass dieser Prozessserver auf diesen kritischen Systemen nicht ausgeführt wird.


## <a name="summary-points"></a>Zusammenfassung
- Mit [Service Map](operations-management-suite-service-map.md) erhalten Sie auch dann eine Übersicht über Ihre gesamte Anwendung, wenn Ihnen nicht alle dazugehörigen Server und Abhängigkeiten bekannt sind.
- Service Map bringt Daten ans Licht, die mit anderen OMS-Lösungen erfasst werden, damit Sie Probleme mit Ihrer Anwendung und der zugrunde liegenden Infrastruktur identifizieren können.
- Mit [Protokollsuchen](../log-analytics/log-analytics-log-searches.md) können Sie einen Drilldown für bestimmte Daten ausführen, die im Log Analytics-Repository erfasst werden.    

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Service Map](operations-management-suite-service-map.md)
- [Bereitstellen und Konfigurieren](operations-management-suite-service-map-configure.md) von Service Map
- Informieren Sie sich über [Log Analytics](../log-analytics/log-analytics-overview.md). Diese Anwendung ist für Service Map erforderlich und dient zum Speichern von Betriebsdaten, die von Agents gespeichert werden.
