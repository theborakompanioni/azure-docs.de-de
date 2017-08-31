---
title: Partnerintegration in Azure Security Center | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Partner in Azure Security Center integriert werden, um die allgemeine Sicherheit Ihrer Azure-Ressourcen zu verbessern.
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
ms.date: 08/18/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: 44beafeff5cbe58ac8ca37632879f6ffc2b67e53
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---
# <a name="partner-integration-in-azure-security-center"></a>Partnerintegration in Azure Security Center

In diesem Artikel erfahren Sie, wie Partner in Azure Security Center integriert werden, um die allgemeine Sicherheit Ihrer Azure-Ressourcen zu verbessern. Security Center bietet eine integrierte Umgebung in Azure und nutzt den Azure Marketplace für die Partnerzertifizierung und für Abrechnungen.

> [!NOTE] 
> Ab Juni 2017 verwendet Security Center den Microsoft Monitoring Agent zum Erfassen und Speichern von Daten. Weitere Informationen finden Sie unter [Azure Security Center – Plattformmigration](security-center-platform-migration.md). Die Informationen in diesem Artikel stellen Security Center-Funktionen nach dem Umstieg auf den Microsoft Monitoring Agent vor.
>

## <a name="why-deploy-partner-solutions-from-security-center"></a>Gründe für die Bereitstellung von Partnerlösungen über Security Center

Für die Nutzung der Partnerintegration in Security Center sprechen vier Gründe:

- **Einfache Bereitstellung:** Das Bereitstellen einer Partnerlösung ist viel einfacher, wenn Sie die Security Center-Empfehlung befolgen. Der Bereitstellungsprozess kann durch Verwendung einer Standardeinrichtung und einer Standardnetzwerktopologie vollständig automatisiert werden. Alternativ können Kunden auch eine halb automatisierte Option wählen, um eine größere Flexibilität und mehr Anpassungsmöglichkeiten zu erhalten.
- **Integrierte Erkennungen:** Sicherheitsereignisse von Partnerlösungen werden automatisch gesammelt, aggregiert und im Rahmen der Security Center-Warnungen und -Vorfälle angezeigt. Diese Ereignisse werden mit Erkennungen anderer Quellen zusammengeführt, um erweiterte Bedrohungserkennungsfunktionen bereitzustellen.
- **Einheitliche Integritätsüberwachung und -verwaltung:** Mithilfe integrierter Integritätsereignisse können Kunden alle Partnerlösungen auf einen Blick überwachen. Es ist eine grundlegende Verwaltung verfügbar – mit einfachem Zugriff auf die erweiterte Konfiguration über die Partnerlösung.
- **SIEM-Export:** Mithilfe der Microsoft Azure-Protokollintegration (Vorschau) können Kunden alle Security Center- und Partnerwarnungen im CEF-Format an lokale SIEM-Systeme (Security Information and Event Management) exportieren.


## <a name="partners-that-integrate-with-security-center"></a>Partner mit Security Center-Integration

In Security Center sind derzeit folgende Lösungen integriert:

- Endpoint Protection ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec und [Microsoft Antimalware für Azure Cloud Services und Virtual Machines](https://docs.microsoft.com/azure/security/azure-security-antimalware)) 
- Web Application Firewall ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) und [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/)) 
- Firewall der nächsten Generation ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) und [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html)) 
- Sicherheitsrisikobewertung ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

Die Anzahl von Partnern innerhalb dieser Kategorien wird nach und nach erhöht, und es kommen neue Kategorien hinzu. 

## <a name="deploy-a-partner-solution"></a>Bereitstellen einer Partnerlösung

Auf der Grundlage der Konfiguration der Azure-Umgebung und der von Ihnen definierten Sicherheitsrichtlinie empfiehlt Security Center unter Umständen die Bereitstellung einer Partnerlösung. Die Security Center-Empfehlung führt Sie durch den Auswahl- und Installationsprozess für eine Partnerlösung. Die Abläufe bei der Bereitstellung können sich je nach verwendetem Lösungstyp und Partner unterscheiden. Weitere Informationen finden Sie in den folgenden Artikeln:

- [Endpoint Protection installieren](security-center-install-endpoint-protection.md)
- [Web Application Firewall hinzufügen](security-center-add-web-application-firewall.md)
- [Firewall der nächsten Generation hinzufügen](security-center-add-next-generation-firewall.md)
- [Sicherheitsrisikobewertung nicht installiert](security-center-vulnerability-assessment-recommendations.md)

## <a name="manage-partner-solutions"></a>Verwalten von Partnerlösungen

Nach der Bereitstellung können Sie Informationen zur Integrität der Lösung anzeigen und grundlegende Verwaltungsaufgaben ausführen. Wählen Sie hierzu auf dem Blatt **Security Center** die Option **Partnerlösungen** aus. Weitere Informationen zum Verwalten von Partnerlösungen in Security Center finden Sie unter [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md).

![Partnerintegration](./media/security-center-partner-integration/security-center-partner-integration-fig1-new2.png)

> [!NOTE]
> Die Unterstützung von Symantec Endpoint Protection ist auf die Ermittlung beschränkt. Es sind keine Integritätswarnungen verfügbar.
>

## <a name="see-also"></a>Weitere Informationen

In diesem Artikel haben Sie erfahren, wie Sie eine Partnerlösung in Azure Security Center integrieren. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md)
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Verstehen der Sicherheitswarnungen in Azure Security Center](security-center-alerts-type.md)
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

