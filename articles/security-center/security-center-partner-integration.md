---
title: Partnerintegration in Azure Security Center | Microsoft-Dokumentation
description: "In diesem Dokument wird erklärt, wie Azure Security Center mit Partnern integriert wird, um die allgemeine Sicherheit Ihrer Azure-Ressourcen zu verbessern."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/23/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c69630688e4bcd68ab3b4ee6d9fdb0e0c46d04b
ms.openlocfilehash: 85460ea37d251b53074de9d832610d253700f2c1
ms.contentlocale: de-de
ms.lasthandoff: 06/24/2017


---
<a id="partner-integration-in-azure-security-center" class="xliff"></a>

# Partnerintegration in Azure Security Center
In diesem Dokument wird erklärt, wie Azure Security Center mit Partnern integriert wird. Hierdurch verbessern Sie die Sicherheit insgesamt, vermitteln eine integrierte Nutzungserfahrung in Azure und profitieren in Sachen Partnerzertifizierung und Abrechnung von Azure Marketplace.

>[!NOTE] 
>Ab Anfang Juni 2017 verwendet Security Center den Microsoft Monitoring Agent zum Erfassen und Speichern von Daten. Weitere Informationen finden Sie unter [Azure Security Center – Plattformmigration](security-center-platform-migration.md). Die Informationen in diesem Artikel stellen Security Center-Funktionen nach dem Umstieg auf den Microsoft Monitoring Agent vor.
>

<a id="why-deploy-partners-solutions-from-security-center" class="xliff"></a>

## Warum werden Partnerlösungen aus Security Center bereitgestellt?

Die vier Hauptgründe, warum die Partnerintegration in Security Center genutzt wird, sind:

- **Einfache Bereitstellung**: Das Bereitstellen einer Partnerlösung ist viel einfacher, wenn Sie die Security Center-Empfehlung befolgen. Der Bereitstellungsprozess kann mithilfe einer Standardkonfiguration und Netzwerktopologie vollständig automatisiert werden, oder Kunden können eine halbautomatische Option auswählen, um mehr Flexibilität und Anpassung der Konfiguration zu erlauben.
- **Integrierte Erkennungen**: Sicherheitsereignisse von Partnerlösungen werden automatisch gesammelt, aggregiert und im Rahmen der Security Center-Warnungen und Vorfälle angezeigt. Diese Ereignisse werden ebenso mit Erkennungen anderer Quellen verbunden, um erweiterte Bedrohungserkennungsfunktionen bereitzustellen.
- **Vereinheitlichte Systemüberwachung und -verwaltung**: Mit integrierten Integritätsereignissen können Kunden alle Partnerlösungen auf einen Blick überwachen. Die grundlegende Verwaltung ist mit einfachen Zugriff auf die erweiterte Konfiguration mithilfe der Partnerlösung verfügbar.
- **Exportieren auf SIEM**: Kunden können nun alle Security Center- und Partnerwarnungen im CEF-Format auf lokale SIEM-Systeme mithilfe der Microsoft Azure-Protokollintegration (Vorschauversion) exportieren.


<a id="what-partners-are-integrated-with-security-center" class="xliff"></a>

## Welche Partner sind in Security Center integriert?
In Security Center sind derzeit folgende Lösungen integriert:

- Endpoint Protection ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec, [Microsoft Antimalware für Azure](https://docs.microsoft.com/azure/security/azure-security-antimalware)) 
- Web Application Firewall ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets), [App Gateway WAF](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/)) 
- Firewall der nächsten Generation ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) und [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html)) 
- Sicherheitsrisikobewertung ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

Im Laufe der Zeit wird Security Center die Anzahl der Partner innerhalb der vorhandenen Kategorien vergrößern sowie neue Kategorien hinzufügen. 

<a id="how-to-deploy-a-partner-solution" class="xliff"></a>

## Wie wird eine Partnerlösung bereitgestellt?

Basierend auf der Konfiguration der Azure-Umgebung und der von Ihnen festgelegten Sicherheitsrichtlinie empfiehlt Security Center unter Umständen die Bereitstellung einer Partnerlösung. Die Empfehlung führt Sie durch den Prozess zur Auswahl und Installation einer Partnerlösung. Die allgemeine Bereitstellungserfahrung kann zu diesem Zeitpunkt abhängig vom Lösungs- und Partnertyp variieren. Unter den folgenden Links finden Sie weitere Informationen:

- [Web Application Firewall hinzufügen](security-center-add-web-application-firewall.md)
- [Firewall der nächsten Generation hinzufügen](security-center-add-next-generation-firewall.md)
- [Endpoint Protection installieren](security-center-install-endpoint-protection.md)
- [Sicherheitsrisikobewertung nicht installiert](security-center-vulnerability-assessment-recommendations.md)

<a id="how-to-manage-partner-solutions" class="xliff"></a>

## Wie werden Partnerlösungen verwaltet?

Nach der Bereitstellung einer Partnerlösung können Sie Informationen zur Integrität der Lösung anzeigen und über die Kachel für die Partnerlösung auf dem Security Center-Hauptdashboard allgemeine Verwaltungsaufgaben ausführen. Weitere Informationen zum Verwalten von Partnerlösungen in Security Center finden Sie unter [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md).

![Partnerintegration](./media/security-center-partner-integration/security-center-partner-integration-fig1-1-newUI.png)

> [!NOTE]
> Die Unterstützung von Symantec Endpoint Protection ist auf die Ermittlung beschränkt, und es sind keine Integritätswarnungen verfügbar.
>

<a id="see-also" class="xliff"></a>

## Weitere Informationen
In diesem Dokument haben Sie erfahren, wie Sie eine Partnerlösung in Azure Security Center integrieren. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md)
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Sicherheitswarnungen nach Typ in Azure Security Center](security-center-alerts-type.md)
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.

