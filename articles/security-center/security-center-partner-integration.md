---
title: Partnerintegration in Azure Security Center | Microsoft Docs
description: "In diesem Dokument wird erklärt, wie Azure Security Center mit Partnern integriert wird, um die allgemeine Sicherheit Ihrer Azure-Ressourcen zu verbessern."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 5001cd47b6ee51967d1286414ccefedd8e7e7813
ms.openlocfilehash: 095b5c9d1a888a4061450234f80c52c5834fbf53
ms.lasthandoff: 12/02/2016


---
# <a name="partner-integration-in-azure-security-center"></a>Partnerintegration in Azure Security Center
In diesem Dokument wird erklärt, wie Azure Security Center mit Partnern integriert wird, um die allgemeine Sicherheit zu verbessern und eine integrierte Erfahrung in Azure bereitzustellen, und gleichzeitig Vorteile aus dem Azure Marketplace für die Zertifizierung von Partnern und die Abrechnung zu ziehen.

## <a name="why-deploy-partners-solutions-from-security-center"></a>Warum werden Partnerlösungen aus Security Center bereitgestellt?

Die vier Hauptgründe, warum die Partnerintegration in Security Center genutzt wird, sind:

- **Einfache Bereitstellung**: Das Bereitstellen einer Partnerlösung ist viel einfacher, wenn Sie die Security Center-Empfehlung befolgen. Der Bereitstellungsprozess kann mithilfe einer Standardkonfiguration und Netzwerktopologie vollständig automatisiert werden, oder Kunden können eine halbautomatische Option auswählen, um mehr Flexibilität und Anpassung der Konfiguration zu erlauben.
- **Integrierte Erkennungen**: Sicherheitsereignisse von Partnerlösungen werden automatisch gesammelt, aggregiert und im Rahmen der Security Center-Warnungen und Vorfälle angezeigt. Diese Ereignisse werden ebenso mit Erkennungen anderer Quellen verbunden, um erweiterte Bedrohungserkennungsfunktionen bereitzustellen.
- **Vereinheitlichte Systemüberwachung und -verwaltung**: Mit integrierten Integritätsereignissen können Kunden alle Partnerlösungen auf einen Blick überwachen. Die grundlegende Verwaltung ist mit einfachen Zugriff auf die erweiterte Konfiguration mithilfe der Partnerlösung verfügbar.
- **Exportieren auf SIEM**: Kunden können nun alle Security Center- und Partnerwarnungen im CEF-Format auf lokale SIEM-Systeme mithilfe der Microsoft Azure-Protokollintegration (Preview) exportieren.


## <a name="what-partners-are-integrated-with-security-center"></a>Welche Partner sind in Security Center integriert?
In Security Center sind derzeit folgende Partner integriert:

- Endpoint Protection (Trend Micro), 
- Web Application Firewall (Barracuda, F5, Imperva und bald Microsoft WAF sowie Fortinet), 
- Firewall-Lösungen der nächsten Generation (Check Point, Barracuda und bald Fortinet sowie Cisco). 
- Lösungen zur Sicherheitsrisikobewertung (Qualys – Preview) 

Im Laufe der Zeit wird Security Center die Anzahl der Partner innerhalb der vorhandenen Kategorien vergrößern sowie neue Kategorien hinzufügen. 

## <a name="how-to-deploy-a-partner-solution"></a>Wie wird eine Partnerlösung bereitgestellt?

Basierend auf der Konfiguration der Azure-Umgebung und der von Ihnen festgelegten Sicherheitsrichtlinie empfiehlt Security Center unter Umständen die Bereitstellung einer Partnerlösung. Die Empfehlung führt Sie durch den Prozess zur Auswahl und Installation einer Partnerlösung. Die allgemeine Bereitstellungserfahrung kann zu diesem Zeitpunkt abhängig vom Lösungs- und Partnertyp variieren. Unter den folgenden Links finden Sie weitere Informationen:

- [Web Application Firewall hinzufügen](security-center-add-web-application-firewall.md)
- [Firewall der nächsten Generation hinzufügen](security-center-add-next-generation-firewall.md)
- [Endpoint Protection installieren](security-center-install-endpoint-protection.md)
- [Sicherheitsrisikobewertung nicht installiert](security-center-vulnerability-assessment-recommendations.md)

## <a name="how-to-manage-partner-solutions"></a>Wie werden Partnerlösungen verwaltet?

Nach der Bereitstellung einer Partnerlösung können Sie Informationen zur Integrität der Lösung anzeigen und über die Kachel für die Partnerlösung auf dem Security Center-Hauptdashboard allgemeine Verwaltungsaufgaben ausführen. Weitere Informationen zum Verwalten von Partnerlösungen in Security Center finden Sie unter [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md).

![Partnerintegration](./media/security-center-partner-integration/security-center-partner-integration-fig1-new.png)


## <a name="see-also"></a>Weitere Informationen
In diesem Dokument haben Sie erfahren, wie Sie eine Partnerlösung in Azure Security Center integrieren. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md)
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Sicherheitswarnungen nach Typ in Azure Security Center](security-center-alerts-type.md)
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.

