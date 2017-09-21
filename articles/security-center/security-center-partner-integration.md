---
title: "Partner- und Lösungsintegration in Azure Security Center | Microsoft-Dokumentation"
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
ms.date: 09/12/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 8cc44da0f61362018d2757da58ca4fb3a9a43764
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="partner-and-solutions-integration-in-azure-security-center"></a>Partner- und Lösungsintegration in Azure Security Center

In diesem Artikel erfahren Sie, wie Partner in Azure Security Center integriert werden, um die allgemeine Sicherheit Ihrer Azure-Ressourcen zu verbessern. Security Center bietet eine integrierte Umgebung in Azure und nutzt den Azure Marketplace für die Partnerzertifizierung und für Abrechnungen.

## <a name="why-deploy-partner-solutions-from-security-center"></a>Gründe für die Bereitstellung von Partnerlösungen über Security Center

Für die Nutzung der Partnerintegration in Security Center sprechen vier Gründe:

- **Einfache Bereitstellung:** Das Bereitstellen einer Partnerlösung ist viel einfacher, wenn Sie die Security Center-Empfehlung befolgen. Der Bereitstellungsprozess kann durch Verwendung einer Standardeinrichtung und einer Standardnetzwerktopologie vollständig automatisiert werden. Alternativ können Kunden auch eine halb automatisierte Option wählen, um eine größere Flexibilität und mehr Anpassungsmöglichkeiten zu erhalten.
- **Integrierte Erkennungen:** Sicherheitsereignisse von Partnerlösungen werden automatisch gesammelt, aggregiert und im Rahmen der Security Center-Warnungen und -Vorfälle angezeigt. Diese Ereignisse werden mit Erkennungen anderer Quellen zusammengeführt, um erweiterte Bedrohungserkennungsfunktionen bereitzustellen.
- **Einheitliche Integritätsüberwachung und -verwaltung:** Mithilfe integrierter Integritätsereignisse können Kunden alle Partnerlösungen auf einen Blick überwachen. Es ist eine grundlegende Verwaltung verfügbar – mit einfachem Zugriff auf die erweiterte Konfiguration über die Partnerlösung.
- **SIEM-Export:** Mithilfe der Microsoft Azure-Protokollintegration (Vorschau) können Kunden alle Security Center- und Partnerwarnungen im CEF-Format an lokale SIEM-Systeme (Security Information and Event Management) exportieren.


## <a name="partners-that-integrate-with-security-center"></a>Partner mit Security Center-Integration

Aktuell stehen im Azure Marketplace folgende Lösungen von Partnern für die native Integration in Security Center zur Verfügung:

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

Nach der Bereitstellung können Sie Informationen zur Integrität der Lösung anzeigen und grundlegende Verwaltungsaufgaben ausführen. Wählen Sie hierzu auf dem Dashboard **Security Center** die Option **Partnerlösungen** aus.

![Integration von Partnerlösungen](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

Welche Inhalte nach dem Öffnen von „Sicherheitslösungen“ angezeigt werden, hängt unter Umständen von Ihrer Infrastruktur ab. Wie auf dem oberen Beispielbild zu sehen, ist die Seite in drei Abschnitte unterteilt:

- **Verbundene Lösungen**: Enthält mit Security Center verbundene Lösungen.
- **Discovered solutions** (Ermittelte Lösungen): Enthält Lösungen, die nicht mit Security Center verbunden sind. Für diese Lösungen können Sie eine Verbindung herstellen. Daraufhin werden sie unter „Verbundene Lösungen“ angezeigt.  Falls von Security Center keine Lösungen ohne Verbindung erkannt werden, wird dieser Abschnitt ausgeblendet.
- **Add data sources** (Datenquellen hinzufügen): Enthält Azure-Datenquellen und Azure-fremde Datenquellen, die Sie Security Center hinzufügen können.

### <a name="connected-solutions"></a>Verbundene Lösungen

Der Abschnitt **Verbunde Lösungen** enthält alle Sicherheitslösungen, die derzeit mit Security Center verbunden sind. 

![Verbundene Lösungen](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

Die hier angezeigten Informationen können je nach Lösung variieren. Auf den Kacheln können unter anderem folgende Informationen angezeigt werden:

- Unternehmenssymbol für den Partner.  Falls das Unternehmenssymbol nicht zur Verfügung steht, wird mindestens das erste Zeichen des Partnernamens angezeigt.
- Der Lösungstyp.
- Der Computername.
- Der Integritätsstatus.  Sollte kein Integritätsindikator gesendet werden, zeigt Security Center das Datum und die Uhrzeit des letzten empfangenen Ereignisses an, um anzugeben, ob die Appliance Berichte übermittelt. Wenn Security Center den Integritätsindikator einer bestimmten Lösung nicht erhält, wird die Kachel der Lösung in diesem Abschnitt nicht angezeigt.

> [!NOTE]
> Security Center zeigt das Datum und die Uhrzeit des letzten empfangenen Ereignisses an, um anzugeben, ob die Appliance Berichte übermittelt. Lösungen, die keinen Integritätsindikator senden, werden als verbunden angezeigt, wenn innerhalb der letzten 14 Tage eine Warnung oder ein Ereignis gesendet wurde.
>  

Einige dieser Lösungen sind möglicherweise vollständig in Azure integriert, bei anderen handelt es sich unter Umständen um lokale Lösungen. Da Security Center [Common Event Format (CEF)](https://docs.microsoft.com/azure/operations-management-suite/oms-security-connect-products#what-is-cef) unterstützt, kann eine Verbindung mit Lösungen hergestellt werden, die CEF verwenden (also etwa mit einer Firewall, die CEF unterstützt). Nach dem Hinzufügen der entsprechenden Lösung zu Security Center sendet die Firewall Protokolle im CEF-Format an Security Center, und Security Center gibt diese dann an [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) weiter. Die Firewall ist eine Azure-fremde Ressource und sendet Ereignisse, aber keinen Integritätsindikator.  Die einzige Integritätsinformation, die Security Center vorliegt, ist der letzte Zeitpunkt, zu dem die Appliance ein Ereignis gesendet hat.  Für alle Azure-fremden Ressourcen zeigt Security Center im Integritätsbereich der Kachel das Datum und die Uhrzeit des letzten Ereignisempfangs an, was darauf hindeutet, dass die Azure-fremde Ressource noch Berichte übermittelt.

### <a name="discovered-solutions"></a>Ermittelte Lösungen

Der Abschnitt **Discovered solutions** (Ermittelte Lösungen) enthält alle Lösungen, die über Azure hinzugefügt wurden, und Security Center schlägt vor, eine Verbindung mit ihnen herzustellen.

![Ermittelte Lösungen](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Security Center kann in integrierte Azure-Lösungen wie etwa [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) integriert werden. Wenn Sie über eine Lizenz für Azure AD Identity Protection verfügen, die Lösung aber nicht mit Security Center verbunden ist, wird Azure AD Identity Protection unter **Discovered solutions** (Ermittelte Lösungen) aufgeführt. Wenn Sie die Lösung in Security Center integrieren möchten, klicken Sie auf der Kachel **Azure AD Identity Protection** auf **VERBINDEN**. Daraufhin wird die folgende Seite angezeigt:

![Azure AD Identity Protection](./media/security-center-partner-integration/security-center-partner-integration-fig6.png)

Um die Verbindungsherstellung mit Azure AD Identity Protection abzuschließen, müssen Sie einen Arbeitsbereich auswählen, in dem die Daten gespeichert werden. Alle Daten aus Azure AD Identity Protection stammen aus der Arbeitsbereichsregion, die in diesem Schritt ausgewählt wurde.  Nachdem Sie mithilfe der Arbeitsbereichsauswahl den Arbeitsbereich ausgewählt haben, fließen die Daten dorthin.

Um die Verbindung mit Security Center herstellen zu können, müssen Sie globaler Administrator oder Sicherheitsadministrator sein.  Wenn Sie nicht über die erforderlichen Berechtigungen verfügen, ist die Schaltfläche **Verbinden** deaktiviert, und es wird eine Meldung mit einer entsprechenden Erklärung angezeigt.

Azure AD Identity Protection-Warnungen durchlaufen die Erkennungs-Pipe von Security Center. Dadurch können Sie Warnungen aus Security Center sowie aus Azure Active Directory Identity Protection erhalten. Security Center führt alle relevant erscheinenden Warnungen zusammen, um einen [Sicherheitsvorfall](https://docs.microsoft.com/azure/security-center/security-center-incident) zu erstellen. Die Beschreibung des Sicherheitsvorfalls enthält weitere Informationen zu verdächtigen Aktivitäten.

### <a name="add-data-sources"></a>Hinzufügen von Datenquellen

Sie können Azure-Computer und Azure-fremde Computer für die Security Center-Integration hinzufügen.  Die Möglichkeit zum Hinzufügen Azure-fremder Computer bedeutet, dass Sie lokale Computer oder eine Appliance mit CEF-Unterstützung hinzufügen können. 

![Datenquellen](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="see-also"></a>Weitere Informationen

In diesem Artikel haben Sie erfahren, wie Sie eine Partnerlösung in Azure Security Center integrieren. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md)
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Verstehen der Sicherheitswarnungen in Azure Security Center](security-center-alerts-type.md)
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

