---
title: "Überwachung in Microsoft Azure | Microsoft-Dokumentation"
description: "Optionen bei der Überwachung von Elemente in Microsoft Azure. Azure Monitor, Application Insights Log Analytics"
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: robb
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: d4a94a92585420cf92018084437422fd0c66fa2d
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="overview-of-monitoring-in-microsoft-azure"></a>Überblick über die Überwachung in Microsoft Azure
Dieser Artikel bietet einen Überblick über Tools, die zur Überwachung von Microsoft Azure zur Verfügung stehen. Folgendes wird behandelt: 
- Überwachen von Anwendungen, die in Microsoft Azure ausgeführt werden 
- Tools/Dienste, die außerhalb von Azure ausgeführt werden und mit denen Objekte in Azure überwacht werden können 

Der Artikel erläutert die verschiedenen verfügbaren Produkte und Dienste und wie sie zusammen funktionieren. Er unterstützt Sie bei der Festlegung der Tools, die für Sie in den jeweiligen Fällen am besten geeignet sind.  

## <a name="why-use-monitoring-and-diagnostics"></a>Gründe für die Verwendung von Überwachung und Diagnose

Leistungsprobleme in Ihrer Cloud-App können Ihr Unternehmen beeinträchtigen. Durch mehrere miteinander verbundene Komponenten und häufige Versionswechsel können Leistungseinbußen jederzeit vorkommen. Bei der Entwicklung einer App werden von Ihren Benutzern in der Regel Probleme entdeckt, die Sie beim Testen nicht gefunden haben. Sie sollten sofort über diese Probleme Bescheid wissen und über Tools zu deren Diagnose und Beseitigung verfügen. Microsoft Azure umfasst eine Reihe von Tools zum Identifizieren dieser Probleme.

## <a name="how-do-i-monitor-my-azure-cloud-apps"></a>Wie überwache ich meine Azure-Cloud-Apps?

Es gibt eine Reihe von Tools zum Überwachen von Azure-Anwendungen und -Diensten. Einige ihrer Funktionen überschneiden sich. Dies hat teils historische Gründe und liegt teils auch an der verschwommenen Grenze zwischen der Entwicklung und dem Betrieb einer Anwendung. 

Hier sind die wichtigsten Tools aufgeführt:

-    **Azure Monitor** ist ein grundlegendes Tool zum Überwachen von Diensten, die in Azure ausgeführt werden. Dieses Tool bietet Ihnen Daten auf Infrastrukturebene über den Durchsatz eines Diensts und über dessen Umgebung. Wenn Sie Ihre Apps vollständig in Azure verwalten und entscheiden, ob Sie Ihre Ressourcen zentral hoch- oder herunterskalieren, bietet Azure Monitor Ihnen einen Ausgangspunkt.

-    **Application Insights** kann für die Entwicklung und als Überwachungslösung für die Produktion verwendet werden. Dieses Tool installiert ein Paket in Ihrer App und bietet Ihnen damit eine interne Ansicht der Geschehnisse. Seine Daten umfassen die Antwortzeiten von Abhängigkeiten, Ausnahmeablaufverfolgungen, Debuggingmomentaufnahmen und Ausführungsprofilen. Das Tool stellt Ihnen leistungsstarke intelligente Tools zum Analysieren all dieser Telemetriedaten bereit, um Sie beim Debuggen einer App zu unterstützen und zu veranschaulichen, wie sie von den Benutzern genutzt wird. Sie können feststellen, ob eine Spitze in den Antwortzeiten auf ein Problem in einer App oder auf ein externes Ressourcenproblem zurückzuführen ist. Wenn Sie Visual Studio verwenden und die App fehlerhaft ist, können Sie direkt zu den problematischen Codezeilen gelangen und das Problem beheben.  

-    **Log Analytics** ist für Personen bestimmt, die die Leistung optimieren und die Wartung für Anwendungen planen müssen, die in der Produktionsumgebung ausgeführt werden. Dieses Tool basiert auf Azure. Es erfasst und aggregiert Daten aus vielen verschiedenen Quellen, wenn auch mit einer Verzögerung von 10 bis 15 Minuten. Es bietet eine ganzheitliche IT-Verwaltungslösung für Azure-Infrastrukturen, lokale Infrastrukturen und cloudbasierte Drittanbieterinfrastrukturen (z.B. Amazon Web Services). Es stellt umfassende Tools zur quellenübergreifenden Datenanalyse bereit, erlaubt komplexe Abfragen in allen Protokollen und kann beim Eintreten angegebener Bedingungen proaktiv Warnungen ausgeben.  Sie können sogar benutzerdefinierte Daten im zugehörigen zentralen Repository sammeln, um sie abfragen und visualisieren zu können. 

-    **System Center Operations Manager (SCOM)** dient zur Verwaltung und Überwachung großer Cloudinstallationen. Möglicherweise kennen Sie dieses Tool bereits als Verwaltungstool für lokale Windows Server- und Hyper-V-basierte Clouds. Es kann jedoch auch in Azure-Apps integriert werden und diese verwalten. Unter anderem kann es Application Insights in vorhandenen Live-Apps installieren.  Wenn eine App ausfällt, werden Sie innerhalb von Sekunden darauf hingewiesen. Beachten Sie, dass Log Analytics keinen Ersatz für SCOM darstellt. Die beiden Tools lassen sich gut kombinieren.  


## <a name="accessing-monitoring-in-the-azure-portal"></a>Zugreifen auf die Überwachung im Azure-Portal
Alle Azure-Überwachungsdienste sind jetzt in einem zentralen Bereich der Benutzeroberfläche verfügbar. Weitere Informationen zum Zugriff auf diesen Bereich finden Sie unter [Erste Schritte mit Azure Monitor](monitoring-get-started.md). 

Auf Überwachungsfunktionen für bestimmte Ressourcen können Sie auch zugreifen, indem Sie diese Ressourcen markieren und einen Drilldown in die Überwachungsoptionen durchführen. 

## <a name="examples-of-when-to-use-which-tool"></a>Beispiele für die Verwendung der einzelnen Tools 

In den folgenden Abschnitten werden einige grundlegende Szenarien beschreiben, und es wird ausgeführt, welche Tools zusammen verwendet werden sollten. 

### <a name="scenario-1--fix-errors-in-an-azure-application-under-development"></a>Szenario 1: Beheben von Fehlern in einer Azure-Anwendung in der Entwicklungsphase   

**Die beste Option besteht darin, Application Insights, Azure Monitor und Visual Studio gemeinsam zu verwenden.**

Azure bietet jetzt die volle Leistung des Visual Studio-Debuggers in der Cloud. Konfigurieren Sie Azure Monitor für das Senden von Telemetriedaten an Application Insights. Ermöglichen Sie in Visual Studio das Einschließen des Application Insights-SDK in Ihrer Anwendung. In Application Insights können Sie die Anwendungszuordnung verwenden, um visuell zu ermitteln, welche Teile der ausgeführten Anwendung fehlerhaft sind. Für die Teile, die nicht fehlerfrei sind, stehen bereits Fehler und Ausnahmen für die Untersuchung zur Verfügung. Sie können die verschiedenen Analysefunktionen in Application Insights für eine detailliertere Analyse verwenden. Wenn Sie sich über den Fehler nicht sicher sind, können Sie den Code mit dem Visual Studio-Debugger schrittweise durchlaufen und ein Problem weiter eingrenzen. 

Weitere Informationen finden Sie unter [Überwachen von Web-Apps](../application-insights/app-insights-azure-web-apps.md). Im Inhaltsverzeichnis auf der linken Seite finden Sie Anleitungen zu verschiedenen Arten von Apps und Sprachen.  

### <a name="scenario-2--debug-an-azure-net-web-application-for-errors-that-only-show-in-production"></a>Szenario 2: Debuggen einer Azure .NET-Webanwendung auf Fehler, die nur in der Produktion auftreten 

> [!NOTE]
> Diese Features befinden sich in der Vorschauphase. 

**Die beste Option besteht in der Verwendung von Application Insights und, wenn möglich, von Visual Studio, um eine vollständige Debuggingumgebung zu erhalten.**

Verwenden Sie den Application Insights-Momentaufnahmedebugger zum Debuggen Ihrer App. Tritt bei Produktionskomponenten ein bestimmter Fehlerschwellenwert auf, erfasst das System automatisch Telemetriedaten in Zeitfenstern, die als „Momentaufnahmen“ bezeichnet werden. Die erfasste Datenmenge ist für eine Produktionscloud sicher, da sie klein genug ist, um nicht die Leistung zu beeinträchtigen, aber groß genug, um die Ablaufverfolgung zu ermöglichen.  Das System kann mehrere Momentaufnahmen erfassen. Sie können sich im Azure-Portal einen bestimmten Zeitpunkt ansehen oder in Visual Studio die vollständige Umgebung nutzen. Mit Visual Studio können Entwickler diese Momentaufnahme so durchlaufen, als fände das Debuggen in Echtzeit statt. Lokale Variablen, Parameter, Speicher und Frames sind alle verfügbar. Den Entwicklern muss der Zugriff auf diese Produktionsdaten über eine RBAC-Rolle gewährt werden.  

Weitere Informationen finden Sie unter [Debuggen mit Momentaufnahmen](../application-insights/app-insights-snapshot-debugger.md). 

### <a name="scenario-3--debug-an-azure-application-that-uses-containers-or-microservices"></a>Szenario 3: Debuggen einer Azure-Anwendung, die Container oder Microservices verwendet 

**Identisch mit Szenario 1. Verwenden Sie Application Insights, Azure Monitor und Visual Studio gemeinsam.** Application Insights unterstützt auch das Sammeln von Telemetriedaten aus Prozessen, die innerhalb von Containern ausgeführt werden, und aus Microservices wie Kubernetes, Docker oder Azure Service Fabric. Weitere Informationen [finden Sie in diesem Video zum Debuggen von Containern und Microservices](https://go.microsoft.com/fwlink/?linkid=848184). 


### <a name="scenario-4--fix-performance-issues-in-your-azure-application"></a>Szenario 4: Beheben von Leistungsproblemen in Ihrer Azure-Anwendung

Der [Application Insights-Profiler](../application-insights/app-insights-profiler.md) erleichtert die Problembehandlung für diese Art von Problemen. Sie können Leistungsprobleme für Anwendungen erkennen und beheben, die in App Services (Web-Apps, Logic Apps, Mobile Apps, API-Apps) und anderen Computeressourcen wie Virtual Machines, VM-Skalierungsgruppen (VMSS), Cloud Services und Service Fabric ausgeführt werden. 

> [!NOTE]
> Die Möglichkeit zum Profilieren von Virtual Machines, VM-Skalierungsgruppen (VMSS), Cloud Services und Service Fabric befindet sich in der Vorschau.   

Darüber hinaus werden Sie durch das Tool „Intelligente Erkennung“ proaktiv per E-Mail über bestimmte Arten von Fehlern wie z.B. langsame Seitenladezeiten benachrichtigt.  Sie müssen an diesem Tool keine Konfiguration vornehmen. Weitere Informationen finden Sie unter [Intelligente Erkennung – Leistungsabweichungen](../application-insights/app-insights-proactive-performance-diagnostics.md) und unter [Intelligente Erkennung – Leistungsabweichungen](https://azure.microsoft.com/blog/Enhancments-ApplicationInsights-SmartDetection/preview).



## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen

* [Azure Monitor in einem Video von der Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Erste Schritte mit Azure Monitor](monitoring-get-started.md)
* [Azure-Diagnose](../azure-diagnostics.md) für die Diagnose von Problemen mit Clouddiensten, virtuellen Computern, VM-Skalierungsgruppen oder Service Fabric-Anwendungen.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) : Diagnostizieren Sie Probleme mit Ihrer App Service-Web-App.
* [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) und die [Operations Management Suite](https://www.microsoft.com/oms/)-Lösung zur Produktionsüberwachung
