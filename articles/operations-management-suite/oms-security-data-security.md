---
title: "Sicherheits- und Überwachungslösung von Operations Management Suite – Datensicherheit | Microsoft Docs"
description: "In diesem Dokument wird beschrieben, wie Daten in der Sicherheits- und Überwachungslösung von Operations Management Suite verwaltet und geschützt werden."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 9cdf7deb-2a30-4672-b89f-71179ee8326a
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 5001cd47b6ee51967d1286414ccefedd8e7e7813
ms.openlocfilehash: 6af6c10cef317453131197db74a9380518afadf0


---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Sicherheits- und Überwachungslösung von Operations Management Suite – Datensicherheit
Mit der [Sicherheits- und Überwachungslösung von Operations Management Suite (OMS)](operations-management-suite-overview.md) werden Daten zu Ihren Ressourcen erfasst und verarbeitet, um Kunden beim Verhindern, Erkennen und Reagieren auf Bedrohungen zu unterstützen. Beispiele für Daten sind:

* Sicherheitsereignisprotokoll
* Ereignisablaufverfolgung für Windows-Ereignisse (ETW)
* AppLocker-Überwachungsereignisse
* Windows-Firewallprotokoll
* Advanced Threat Analytics-Ereignisse
* Ergebnisse der Baselinebewertung
* Ergebnisse der Antischadsoftwarebewertung
* Ergebnisse der Update-/Patchbewertung
* Syslog-Datenströme, die auf dem Agent explizit aktiviert sind

Wir unternehmen große Anstrengungen, um für den Datenschutz und die Sicherheit dieser Daten zu sorgen. Microsoft hält strenge Compliance- und Sicherheitsrichtlinien ein – angefangen bei der Codierung bis hin zum Betreiben von Diensten.
In diesem Artikel wird erläutert, wie Daten mit der Sicherheits- und Überwachungslösung von OMS verwaltet und geschützt werden.

## <a name="data-sources"></a>Datenquellen
Mit der Sicherheits- und Überwachungslösung von OMS werden Daten von Ihren virtuellen Computern und physischen Computern analysiert, auf denen der OMS-Agent installiert ist. Die Sicherheits- und Überwachungslösung von OMS ermöglicht die Erfassung von Konfigurationsinformationen zu Sicherheitsereignissen, z.B. Windows-Ereignisse, Überwachungsprotokolle, IIS-Protokolle und Syslog-Nachrichten. Beispiele für Daten dieser Art: Betriebssystemtyp und -version, ausgeführte Prozesse, Computername, IP-Adressen, angemeldeter Benutzer und Mandanten-ID.  

## <a name="data-protection"></a>Datenschutz
**Trennung von Daten:**Daten werden für jede Komponente des Diensts logisch getrennt verwaltet. Sämtliche Daten werden nach Organisation gekennzeichnet. Dieser Kennzeichnung wird während des gesamten Datenlebenszyklus beibehalten und auf jeder Ebene des Diensts erzwungen. 

**Datenzugriff:** Um Sicherheitsempfehlungen bereitstellen und potenzielle Sicherheitsrisiken untersuchen zu können, greifen Mitarbeiter von Microsoft unter Umständen auf Informationen zu, die von den Diensten erfasst oder analysiert wurden. Wir halten uns an die [Microsoft Online Services-Bedingungen](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) und die [Datenschutzerklärung](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), in denen festgelegt ist, dass Microsoft keine Kundendaten oder daraus abgeleiteten Informationen zu Werbezwecken oder anderen kommerziellen Zwecken verwendet. Um Sicherheitsempfehlungen bereitstellen und potenzielle Sicherheitsrisiken untersuchen zu können, greifen Mitarbeiter von Microsoft unter Umständen auf Informationen zu, die von den Azure-Diensten erfasst oder analysiert wurden (beispielsweise Absturzabbilddateien). Wir verwenden Kundendaten nur, wenn dies für die Bereitstellung Ihrer Azure-Dienste erforderlich ist. Dies gilt auch für Zwecke, die mit der Bereitstellung dieser Dienste kompatibel sind. Sie bleiben im Besitz aller Rechte an Ihren Daten.

**Datennutzung:** Microsoft nutzt mandantenübergreifende Muster und Informationen zu Bedrohungen (Threat Intelligence), um die Funktionen für Prävention und Erkennung zu verbessern. Dies erfolgt in Übereinstimmung mit den in unserer [Datenschutzerklärung](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx) beschriebenen Datenschutzzusagen.

> [!NOTE]
> Der Datenspeicherort wird auf der OMS-Arbeitsbereichsebene während der Erstellung des Arbeitsbereichs konfiguriert. Dieser Vorgang ist Teil des ersten Konfigurationsprozesses für die Sicherheits- und Überwachungslösung von OMS.
> 
> 

## <a name="see-also"></a>Weitere Informationen
In diesem Dokument haben Sie erfahren, wie Daten in OMS verwaltet und geschützt werden. Weitere Informationen zur Sicherheits- und Überwachungslösung von OMS finden Sie unter:

* [Operations Management Suite (OMS) – Übersicht](operations-management-suite-overview.md)
* [Überwachen von und Reagieren auf Sicherheitswarnungen in der Sicherheits- und Überwachungslösung von Operations Management Suite](oms-security-responding-alerts.md)
* [Überwachen von Ressourcen in der Sicherheits- und Überwachungslösung von Operations Management Suite](oms-security-monitoring-resources.md)




<!--HONumber=Dec16_HO1-->


