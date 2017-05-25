---
title: "Häufig gestellte Fragen zu Log Analytics | Microsoft Docs"
description: "Antworten auf häufig gestellte Fragen zum Azure Log Analytics-Dienst."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: ad536ff7-2c60-4850-a46d-230bc9e1ab45
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ee8f4aafcc35e43c4fcba5a3a72b043dd9fc32c
ms.openlocfilehash: 4695669dc20b4b4b90ccdaf4db06df2cfcba2167
ms.contentlocale: de-de
ms.lasthandoff: 02/21/2017


---
# <a name="log-analytics-faq"></a>Häufig gestellte Fragen zu Log Analytics
Dieser Microsoft-Artikel enthält eine Liste häufig gestellter Fragen zu Log Analytics in Microsoft Operations Management Suite (OMS). Wenn Sie weiteren Fragen zu Log Analytics haben, besuchen Sie das [Diskussionsforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) , und stellen Sie Ihre Fragen. Ein Mitglied der Community hilft Ihnen dabei, Ihre Antworten zu erhalten. Wenn eine Frage häufiger gestellt wird, fügen wir sie diesem Artikel hinzu, damit sie schnell und einfach gefunden werden kann.

## <a name="general"></a>Allgemein
**F: Welche Überprüfungen werden von den AD- und SQL-Bewertungslösungen durchgeführt?**

a. Die folgende Abfrage zeigt eine Beschreibung aller Überprüfungen, die derzeit ausgeführt werden:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

Die Ergebnisse können dann zur weiteren Prüfung in Excel exportiert werden.

**F: Warum sehe ich etwas anderes als *OMS* in der SCOM-Verwaltung?**

A: Je nachdem, welcher Updaterollup von SCOM Sie verwenden, wird Ihnen ggf. ein Knoten für *System Center Advisor*, *Operational Insights* oder *Log Analytics* angezeigt.

Die Aktualisierung der Textzeichenfolgen für *OMS* befindet sich in einem Management Pack, das manuell importiert werden muss. Befolgen Sie die Anweisungen im KB-Artikel zum neuesten Updaterollup von SCOM, und aktualisieren Sie die OMS-Konsole so, dass der Knoten *OMS* die neuesten Updates enthält.

**F: Gibt es eine *lokale* Version von OMS?**

A: Nein. Log Analytics verarbeitet und speichert sehr große Datenmengen. Als Clouddienst kann Log Analytics bei Bedarf hochskaliert werden, sodass Leistungseinbußen in Ihrer Umgebung vermieden werden können.

Die Ausführung als Clouddienst bedeutet auch, dass sich nicht um den laufenden Betrieb der Log Analytics-Infrastruktur kümmern müssen und in den Genuss häufiger Updates und Korrekturen von Features kommen.

## <a name="configuration"></a>Konfiguration
**F: Kann ich den Namen der Tabelle bzw. des Blobcontainers zum Einlesen von Daten aus Azure-Diagnose (WAD) ändern?**  

A:    Nein, dies ist derzeit nicht möglich, aber für eine künftige Version geplant.

**F: Welche IP-Adressen verwenden die OMS-Dienste? Wie stelle ich sicher, dass meine Firewall nur Datenverkehr zu den OMS-Diensten zulässt?**  

a. Die Endpunkte erhalten IP-Adressen, die sich in den [IP-Adressbereichen von Microsoft Azure-Rechenzentren befinden](http://www.microsoft.com/download/details.aspx?id=41653).

Sobald Dienstbereitstellungen erfolgen, ändern sich die tatsächlichen IP-Adressen des OMS-Diensts. Die DNS-Namen, die Ihre Firewall passieren dürfen, sind unter [Konfigurieren von Proxy- und Firewalleinstellungen in Log Analytics](log-analytics-proxy-firewall.md)dokumentiert.

**F: Ich verwende ExpressRoute für die Verbindung mit Azure. Wird für meinen Log Analytics-Datenverkehr meine ExpressRoute-Verbindung verwendet?**  

a. Die verschiedenen Typen von ExpressRoute-Datenverkehr werden in der [ExpressRoute-Dokumentation](../expressroute/expressroute-faqs.md#supported-services) beschrieben.

Für Datenverkehr zu Log Analytics wird eine ExpressRoute-Verbindung mit öffentlichem Peering verwendet.

**F: Gibt es eine einfache Möglichkeit zum Verschieben eines vorhandenen Log Analytics-Arbeitsbereichs in einen anderen Log Analytics-Arbeitsbereich bzw. ein anderes Azure-Abonnement?**  Wir verfügen in unserem Azure-Abonnement zu Testzwecken über einige OMS-Arbeitsbereiche von Kunden. Da diese nun in die Produktion übertragen werden sollen, möchten wir sie in ihre eigenen Azure-/OMS-Abonnements übertragen.  

a. Mit dem Cmdlet `Move-AzureRmResource` können Sie einen Log Analytics-Arbeitsbereich sowie ein Automation-Konto aus einem Azure-Abonnement in ein anderes verschieben. Weitere Informationen finden Sie unter [Move-AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx).

Diese Änderung kann auch im Azure-Portal erfolgen.

Sie können Daten nicht aus einem Log Analytics-Arbeitsbereich in einen anderen verschieben oder die Region ändern, in der Log Analytics-Daten gespeichert werden.

**F: Wie füge ich OMS zu SCOM hinzu?**

A: Nach einer Aktualisierung auf den neuen Updaterollup und dem Import von Management Packs können Sie SCOM mit Log Analytics verbinden.

Beachten Sie, dass die SCOM-Verbindung mit Log Analytics nur für SCOM 2012 SP1 und höher verfügbar ist.

**F: Wie kann sichergestellt werden, dass ein Agent mit Log Analytics kommunizieren kann?**

A: Um sicherzustellen, dass der Agent mit OMS kommunizieren kann, wechseln Sie zu „Systemsteuerung > Sicherheit und Einstellungen > **Microsoft Monitoring Agent**“.

Suchen Sie unter der Registerkarte **Azure Log Analytics (OMS)** nach einem grünen Häkchen. Ein grünes Häkchen bestätigt, dass der Agent mit dem OMS-Dienst kommunizieren kann.

Ein gelbes Warnsymbol bedeutet, dass der Agent Probleme bei der Kommunikation mit OMS hat. Ein häufiger Grund ist, dass der Microsoft Monitoring Agent-Dienst beendet wurde und neu gestartet werden muss.

**F: Wie beende ich die Kommunikation eines Agents mit Log Analytics?**

A: Entfernen Sie in SCOM den Computer aus der von OMS verwalteten Liste. Dadurch wird die gesamte Kommunikation über SCOM für diesen Agent beendet. Bei Agents, die direkt mit OMS verbunden sind, können Sie die Kommunikation mit OMS beenden, indem Sie zu „Systemsteuerung > Sicherheit und Einstellungen > **Microsoft Monitoring Agent**“ wechseln.
Entfernen Sie unter **Azure Log Analytics (OMS)**alle aufgeführten Arbeitsbereiche.

**F: Warum tritt ein Fehler auf, wenn ich versuche, meinen Arbeitsbereich aus einem Azure-Abonnement in ein anderes zu verschieben?**

A: Wenn Sie eine Lösung hinzufügen, erstellt Azure eine Ressource in dem Azure-Abonnement, zu dem der Arbeitsbereich gehört.

In der Regel sind Personen, die das Abonnement hinzufügen, entweder Administratoren oder Mitwirkende für das *Azure-Abonnement*. Administrator oder Mitwirkender im OMS-Portal ist nicht ausreichend, wenn der Benutzer nicht auch über die gleichen Berechtigungen im Azure-Portal für das Azure-Abonnement verfügt.


## <a name="agent-data"></a>Agent-Daten
**F: Wie viele Daten kann ich über den Agent an Log Analytics senden? Gibt es eine maximale Datenmenge pro Kunde?**  
a. Im Tarif Free gilt eine tägliche Obergrenze von 500 MB pro Arbeitsbereich. Die Tarife Standard und Premium sehen keine Begrenzung der Datenmenge vor, die hochgeladen werden kann. Als Clouddienst ist OMS so ausgelegt, dass ein automatisches Hochskalieren erfolgt, um das vom Kunden eingehende Datenvolumen zu bewältigen, selbst wenn es sich um mehrere Terabytes pro Tag handelt.

Der Log Analytics-Agent ist auf einen kleinen Speicherbedarf ausgelegt und führt verschiedene einfache Datenkomprimierungsaufgaben durch. Einer unserer Kunden hat tatsächlich einen Blog zu den Tests verfasst, die sein Unternehmen mit unserem Agent ausgeführt haben, so beeindruckt war er. Das Datenvolumen variiert basierend auf den Lösungen, die Ihre Kunden betreiben. Ausführliche Informationen zum Datenvolumen und die Aufschlüsselung nach Lösung finden Sie auf der Übersichtsseite zu OMS auf der Kachel **Verwendung** .

Weitere Informationen finden Sie in einem [Kundenblog](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) zum geringen Speicherbedarf des OMS-Agents.

**F: Wie viel Netzwerkbandbreite nutzt der Microsoft-Verwaltungs-Agent (MMA) beim Senden von Daten an Log Analytics?**

a. Die Bandbreite hängt von der gesendeten Datenmenge ab. Daten werden komprimiert, bevor sie über das Netzwerk gesendet werden.

**F: Wie viele Daten werden pro Agent gesendet?**

a. Dies hängt größtenteils von Folgendem ab:

* den Lösungen, die Sie aktiviert haben
* der Anzahl der Protokolle und Leistungsindikatoren, die gesammelt werden
* der Menge der Daten in den Protokollen

Der Tarif Free ist eine gute Möglichkeit, dem Dienst mehrere Server hinzuzufügen und das typische Datenvolumen zu messen. Die Gesamtverwendung wird auf der Seite **Verwendung** gezeigt.
Für Computer, die den WireData-Agent ausführen können, wird mithilfe der folgenden Abfrage gezeigt, wie viele Daten gesendet werden:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```



## <a name="next-steps"></a>Nächste Schritte
* [Erste Schritte mit Log Analytics](log-analytics-get-started.md). Hier erfahren Sie mehr über Log Analytics und wie Sie binnen Minuten loslegen können.

