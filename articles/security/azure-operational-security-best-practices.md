---
title: "Bewährte Methoden für Azure Operational Security | Microsoft-Dokumentation"
description: "Dieser Artikel enthält bewährte Methoden für Azure Operational Security."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: ed3c4bf2dcdda3e04a18682da568ae73f41bb37d
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---

# <a name="azure-operational-security-best-practices"></a>Bewährte Methoden für Azure Operational Security
Azure Operational Security bezieht sich auf die Dienste, Steuerelemente und Features, die für Benutzer zum Schützen ihrer Daten, Anwendungen und anderen Ressourcen in Microsoft Azure zur Verfügung stehen. Azure Operational Security basiert auf einem Framework, das die über verschiedene für Microsoft einzigartige Funktionen erworbenen Kenntnisse einbezieht, einschließlich Microsoft Security Development Lifecycle (SDL), dem Microsoft Security Response Center-Programm und den umfassenden Informationen zur Bedrohungslage hinsichtlich der Sicherheit im Internet.

In diesem Artikel werden die bewährten Methoden für die Azure-Datenbanksicherheit beschrieben. Diese bewährten Methoden sind aus unseren Erfahrungen mit der Azure-Datenbanksicherheit und den Erfahrungen von Kunden wie Ihnen abgeleitet.

Für jede bewährte Methode wird Folgendes beschrieben:
-   Wobei es bei der bewährten Methode geht
-   Warum Sie die bewährte Methode nutzen sollten
-   Was die Folge sein könnte, wenn Sie die bewährte Methode nicht aktivieren
- Wie Sie erfahren können, wie Sie die empfohlenen Vorgehensweisen aktivieren

Dieser Artikel zu den bewährten Methoden für Azure Operational Security basiert auf einer Konsensmeinung und den Möglichkeiten und Funktionssätzen der Azure-Plattform, wie sie zum Zeitpunkt der Erstellung dieses Artikels vorhanden waren. Meinungen und Technologien ändern sich im Laufe der Zeit. Dieser Artikel wird daher regelmäßig aktualisiert, um diese Änderungen widerzuspiegeln.

Bewährte Methoden für Azure Operational Security, die in diesem Artikel beschrieben werden:

-   Überwachen, Verwalten und Schützen der Cloudinfrastruktur
-   Verwalten von Identitäten und Implementieren des einmaligen Anmeldens (SSO)
-   Verfolgen von Anforderungen, Analysieren von Verwendungstrends und Diagnostizieren von Problemen
-   Überwachen von Diensten mit einer zentralen Überwachungslösung
-   Verhindern, Erkennen und Reagieren auf Bedrohungen
-   Szenariobasierte lückenlose Netzwerküberwachung
-   Sichern der Bereitstellung mithilfe von bewährten DevOps-Tools

## <a name="monitor-manage-and-protect-cloud-infrastructure"></a>Überwachen, Verwalten und Schützen der Cloudinfrastruktur
Die IT-Abteilung (IT Operations) ist für die Verwaltung der Infrastruktur, Anwendungen und Daten von Rechenzentren verantwortlich, z.B. die Stabilität und Sicherheit dieser Systeme. Für die Gewinnung von Erkenntnissen zur Sicherheit in immer komplexeren IT-Umgebungen müssen Organisationen aber häufig Daten aus mehreren Sicherheits- und Verwaltungssystemen zusammentragen.

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Ihnen das Verwalten und Schützen Ihrer Infrastruktur lokal und in der Cloud erleichtert.

Mit der [Sicherheits- und Überwachungslösung von OMS](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) kann die IT-Abteilung aktiv alle Ressourcen überwachen. Dies kann zur Minimierung der Auswirkungen von Sicherheitsincidents beitragen. Die Sicherheits- und Überwachungslösung von OMS verfügt über Sicherheitsdomänen, die zum Überwachen von Ressourcen verwendet werden können.

Weitere Informationen zu OMS finden Sie im Artikel [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

Mit der [Sicherheits- und Überwachungslösung von Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) werden Daten zu Ihren Ressourcen erfasst und verarbeitet, um Sie beim Verhindern, Erkennen und Reagieren auf Bedrohungen zu unterstützen. Beispiele für Daten sind:

-   Sicherheitsereignisprotokoll
-   Ereignisablaufverfolgung für Windows-Ereignisse (ETW)
-   AppLocker-Überwachungsereignisse
-   Windows-Firewallprotokoll
-   Advanced Threat Analytics-Ereignisse
-   Ergebnisse der Baselinebewertung
-   Ergebnisse der Antischadsoftwarebewertung
-   Ergebnisse der Update-/Patchbewertung
-   Syslog-Datenströme, die auf dem Agent explizit aktiviert sind


## <a name="manage-identity-and-implement-single-sign-on"></a>Verwalten von Identitäten und Implementieren des einmaligen Anmeldens
[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft.

[Azure AD](https://azure.microsoft.com/services/active-directory/) bietet auch einen vollständigen Satz von [Identitätsverwaltungsfunktionen](https://docs.microsoft.com/azure/security/security-identity-management-overview), z.B.: [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), Geräteregistrierung, Self-Service-Kennwortverwaltung, Self-Service-Gruppenverwaltung, Verwaltung privilegierter Konten, rollenbasierte Zugriffssteuerung, Überwachung der Anwendungsnutzung, umfassende Überwachungsfunktionen sowie Sicherheitsüberwachung und -warnungen.

Mit den folgenden Funktionen können Sie cloudbasierte Anwendungen sichern, IT-Prozesse optimieren, Kosten senken und sicherstellen, dass unternehmensweite Kompatibilitätsziele erreicht werden:

-   Identitäts- und Zugriffsverwaltung für die Cloud
-   Vereinfachen des Benutzerzugriffs auf sämtliche Cloud-Apps
-   Schützen von sensiblen Daten und Anwendungen
-   Aktivieren von Self-Service für Ihre Mitarbeiter
-   Integrieren in Azure Active Directory

### <a name="identity-and-access-management-for-the-cloud"></a>Identitäts- und Zugriffsverwaltung für die Cloud
Azure Active Directory (Azure AD) ist eine umfassende [Cloudlösung zur Identitäts- und Zugriffsverwaltung](https://www.microsoft.com/cloud-platform/identity-management), die eine Reihe solider Funktionen für die Benutzer- und Gruppenverwaltung bietet. Damit können Sie den sicheren Zugriff auf lokale und Cloudanwendungen gewährleisten, beispielsweise auf Microsoft-Webdienste wie Office 365 oder zahlreiche SaaS-Anwendungen (Software-as-a-Service) anderer Anbieter.
Weitere Informationen zum Aktivieren des Schutzes von Identitäten in Azure AD finden Sie im Artikel [Aktivieren von Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-enable).

### <a name="simplify-user-access-to-any-cloud-app"></a>Vereinfachen des Benutzerzugriffs auf sämtliche Cloud-Apps
[Aktivieren Sie das einmalige Anmelden](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps), um den Benutzerzugriff auf Tausende von Cloudanwendungen über Windows-, Mac-, Android- und iOS-Geräte zu erleichtern. Benutzer können Anwendungen mithilfe ihrer Unternehmensanmeldeinformationen über einen persönlichen webbasierten Zugriffsbereich oder eine mobile App starten. Mit dem Anwendungsproxy-Modul von Azure AD können Sie nicht nur SaaS-Anwendungen, sondern auch lokale Webanwendungen veröffentlichen, um sehr sicheren Remotezugriff und einmaliges Anmelden zu ermöglichen.

### <a name="protect-sensitive-data-and-applications"></a>Schützen von sensiblen Daten und Anwendungen
Aktivieren Sie [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), um nicht autorisierten Zugriff auf lokale Anwendungen und auf Cloudanwendungen zu verhindern, indem eine zusätzliche Authentifizierungsebene geschaffen wird. Schützen Sie Ihr Unternehmen, und wenden Sie potenzielle Bedrohungen ab – mit Sicherheitsüberwachung, Warnungen und auf Machine Learning basierenden Berichten, die inkonsistente Zugriffsmuster erkennen.

### <a name="enable-self-service-for-your-employees"></a>Aktivieren von Self-Service für Ihre Mitarbeiter
Delegieren Sie wichtige Aufgaben wie das Zurücksetzen von Kennwörtern und das Erstellen und Verwalten von Gruppen an Ihre Mitarbeiter. Aktivieren Sie mit Azure AD [Self-Service-Funktionen für Kennwortänderung](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password) und -zurücksetzung sowie Gruppenverwaltung.

### <a name="integrate-with-azure-active-directory"></a>Integrieren in Azure Active Directory
Erweitern Sie [Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate) und andere lokale Verzeichnisse auf Azure AD, um das einmalige Anmelden für alle cloudbasierten Anwendungen zu ermöglichen. Benutzerattribute können automatisch aus allen Arten von lokalen Verzeichnissen mit Ihrem Cloudverzeichnis synchronisiert werden.

Weitere Informationen zur Integration in Azure Active Directory und deren Aktivierung finden Sie im Artikel [Integrieren Ihrer lokalen Verzeichnisse in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="trace-requests-analyze-usage-trends-and-diagnose-issues"></a>Verfolgen von Anforderungen, Analysieren von Verwendungstrends und Diagnostizieren von Problemen
Die [Azure-Speicheranalyse](https://docs.microsoft.com/azure/storage/storage-analytics) führt die Protokollierung aus und stellt Metrikdaten für ein Speicherkonto bereit. Mit diesen Daten können Sie Anforderungen verfolgen, Verwendungstrends analysieren und Probleme mit dem Speicherkonto diagnostizieren.

Metriken der Speicheranalyse sind standardmäßig für neue Speicherkonten aktiviert. Im Azure-Portal können Sie die Protokollierung aktivieren und Metriken sowie die Protokollierung konfigurieren. Details finden Sie unter [Überwachen eines Speicherkontos im Azure-Portal](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Sie können die Speicheranalyse auch programmgesteuert über die REST-API oder die Clientbibliothek aktivieren. Verwenden Sie den Vorgang „Diensteigenschaften festlegen“, um die Speicheranalyse für jeden Dienst einzeln zu aktivieren.

Eine ausführliche Anleitung zum Verwenden der Speicheranalyse sowie weiterer Tools, um Azure Storage-bezogene Probleme zu identifizieren, zu diagnostizieren und zu beheben, finden Sie unter [Microsoft Azure Storage: Überwachung, Diagnose und Problembehandlung](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Weitere Informationen zur Integration in Azure Active Directory und deren Aktivierung finden Sie im Artikel [Enabling and Configuring Storage Analytics (Aktivieren und Konfigurieren der Speicheranalyse)](https://docs.microsoft.com/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics?redirectedfrom=MSDN).

## <a name="monitoring-services"></a>Überwachen von Diensten
Cloudanwendungen sind komplexe Systeme mit zahlreichen Variablen. Die Überwachung stellt Daten bereit, auf deren Grundlage die ordnungsgemäße Ausführung der Anwendung sichergestellt werden kann. Sie trägt auch zur Vermeidung potenzieller Probleme bei und hilft bei der Behandlung bereits aufgetretener Probleme. Darüber hinaus können Sie auf der Grundlage von Überwachungsdaten umfassende Erkenntnisse über Ihre Anwendung gewinnen. Mithilfe dieser Kenntnisse können Sie die Leistung oder Wartungsfreundlichkeit der Anwendung verbessern oder Aktionen automatisieren, die andernfalls manuell ausgeführt werden müssten.

### <a name="monitor-azure-resources"></a>Überwachen von Azure-Ressourcen
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) ist der Plattformdienst, mit dem Sie Ihre Azure-Ressourcen an einem zentralen Ort verwalten können. Mit Azure Monitor können Sie Metriken und Protokolle aus den Ressourcen in Azure visualisieren, abfragen, weiterleiten und archivieren und ggf. notwendige Maßnahmen ergreifen. Sie können im Portal auf dem Blatt „Monitor“, über [Monitor PowerShell-Cmdlets](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples), in einer [plattformübergreifenden Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-cli-samples) oder in [Azure Monitor-REST-APIs](https://msdn.microsoft.com/library/dn931943.aspx) mit diesen Daten arbeiten.

### <a name="enable-autoscale-with-azure-monitor"></a>Aktivieren der automatischen Skalierung mit Azure Monitor
Die Aktivierung der [automatische Skalierung von Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started) gilt nur für VM-Skalierungsgruppen (VMSS), Clouddienste, App Service-Pläne und App Service-Umgebungen.

### <a name="manage-roles-permissions-and-security"></a>Verwalten von Berechtigungen und Sicherheit für Rollen
Viele Teams müssen den [Zugriff auf Überwachungsdaten](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security) und -einstellungen streng regulieren. Wenn einige Ihrer Teammitglieder beispielsweise ausschließlich an der Überwachung arbeiten (Supporttechniker, DevOps-Techniker) oder wenn Sie einen verwalteten Dienstanbieter verwenden, sollten Sie diesen nur Zugriff auf Überwachungsdaten erteilen und deren Möglichkeit zum Erstellen, Ändern oder Löschen von Ressourcen einschränken.

Hier wird beschrieben, wie Sie schnell eine integrierte RBAC-Rolle zur Überwachung auf einen Benutzer in Azure anwenden oder Ihre eigene benutzerdefinierte Rolle für einen Benutzer erstellen, der eingeschränkte Überwachungsberechtigungen benötigt. Anschließend werden Sicherheitsaspekte für Ihre Azure Monitor-Ressourcen erörtert, und es wird beschrieben, wie Sie den Zugriff auf die darin enthaltenen Daten beschränken können.

## <a name="prevent-detect-and-respond-to-threats"></a>Verhindern, Erkennen und Reagieren auf Bedrohungen
Bei der Bedrohungserkennung von Security Center werden automatisch Sicherheitsinformationen von Azure-Ressourcen, aus dem Netzwerk und von verbundenen Partnerlösungen gesammelt. Diese Informationen, bei denen es sich häufig um korrelierende Informationen aus mehreren Quellen handelt, werden analysiert, um Bedrohungen zu identifizieren. Sicherheitswarnungen werden in Security Center zusammen mit Empfehlungen zur Lösung der Bedrohung priorisiert.

-   [Konfigurieren Sie eine Sicherheitsrichtlinie](https://docs.microsoft.com/azure/security-center/security-center-policies) für Ihr Azure-Abonnement.
-   Verwenden Sie die [Empfehlungen in Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations) als Hilfe beim Schützen Ihrer Azure-Ressourcen.
-   Überprüfen und verwalten Sie Ihre aktuellen [Sicherheitswarnungen](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) unterstützt Sie beim Verhindern, Erkennen und Beheben von Bedrohungen durch größere Transparenz und bessere Kontrolle über die Sicherheit Ihrer Azure-Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden.

Security Center stellt benutzerfreundliche und effektive Funktionen bereit, die in Azure integriert sind und verwendet werden können, um Bedrohungen zu verhindern, zu erkennen und darauf zu reagieren. Wichtige Funktionen sind:

-   Verstehen des Sicherheitsstatus der Cloud
-   Kontrollieren der Cloudsicherheit
-   Problemloses Bereitstellen integrierter Cloudsicherheitslösungen
-   Ermitteln von Bedrohungen und umgehendes Reagieren

### <a name="understand-cloud-security-state"></a>Verstehen des Sicherheitsstatus der Cloud
Mit Azure Security Center erhalten Sie eine zentrale Ansicht des Sicherheitsstatus all Ihrer Azure-Ressourcen. Überprüfen Sie auf einen Blick, ob die erforderlichen Sicherheitskontrollfunktionen implementiert und ordnungsgemäß konfiguriert sind, und ermitteln Sie schnell Ressourcen, die Ihre Aufmerksamkeit erfordern.

### <a name="take-control-of-cloud-security"></a>Kontrollieren der Cloudsicherheit
Definieren Sie basierend auf den Anforderungen Ihres Unternehmens an die Sicherheit in der Cloud [Sicherheitsrichtlinien](https://docs.microsoft.com/azure/security-center/security-center-policies) für Ihre Azure-Abonnements, die auf den Typ der Anwendung oder die Vertraulichkeit der Daten in den verschiedenen Abonnements abgestimmt sind. Nutzen Sie richtlinienbasierte Empfehlungen, um Ressourcenbesitzer durch die Implementierung der erforderlichen Kontrollmechanismen zu leiten. So können Sie sich jederzeit auf die Sicherheit Ihrer Cloudlösungen verlassen.

### <a name="easily-deploy-integrated-cloud-security-solutions"></a>Problemloses Bereitstellen integrierter Cloudsicherheitslösungen
[Aktivieren Sie Sicherheitslösungen](https://docs.microsoft.com/azure/security-center/security-center-partner-integration) von Microsoft und den Microsoft-Partnern, wie z.B. branchenführende Firewalls und Antischadsoftware. Profitieren Sie von der optimierten Bereitstellung, um Ihre Sicherheitslösungen zu implementieren – sogar Netzwerkänderungen werden für Sie konfiguriert. Sicherheitsereignisse aus Partnerlösungen werden automatisch erfasst und analysiert, um Sie ggf. über Bedrohungen zu benachrichtigen.

### <a name="detect-threats-and-respond-fast"></a>Ermitteln von Bedrohungen und umgehendes Reagieren
Erkennen Sie aktuelle und zukünftige Cloudbedrohungen rechtzeitig dank eines integrierten, analysebasierten Ansatzes. Security Center kombiniert [globale Bedrohungsanalysen](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities) und die umfassende Erfahrung von Microsoft mit Einblicken in Ereignisse zur Cloudsicherheit über all Ihre Azure-Bereitstellungen hinweg. So unterstützt Security Center Sie dabei, tatsächliche Bedrohungen frühzeitig zu erkennen und falsch positive Ergebnisse zu reduzieren. Cloudsicherheitswarnungen liefern Einblicke in Angriffsversuche, die zugehörigen Ereignisse und die betroffenen Ressourcen. Außerdem erhalten Sie Empfehlungen für eine schnelle Problembehandlung und Wiederherstellung des Betriebs.

## <a name="end-to-end-scenario-based-network-monitoring"></a>Szenariobasierte lückenlose Netzwerküberwachung
Kunden erstellen ein End-to-End-Netzwerk in Azure und orchestrieren und kombinieren dabei verschiedene Netzwerkressourcen wie z.B. VNET, ExpressRoute, Application Gateway, Load Balancer u.v.m. Überwachung steht für jede der Netzwerkressourcen zur Verfügung.

[Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) ist ein regionaler Dienst, mit dem Sie Bedingungen auf der Ebene von Netzwerkszenarien in Azure überwachen und diagnostizieren können. Die Tools zur Netzwerkdiagnose und -visualisierung von Network Watcher helfen Ihnen dabei, Ihr Netzwerk in Azure zu verstehen, Diagnosen durchzuführen und Einblicke zu gewinnen.

### <a name="automate-remote-network-monitoring-with-packet-capture"></a>Automatisieren der Remotenetzwerküberwachung per Paketerfassung
Überwachen und diagnostizieren Sie mithilfe von Network Watcher Netzwerkprobleme, ohne sich an Ihren virtuellen Computern (VMs) anmelden zu müssen. Lösen Sie mithilfe von Warnungen die [Paketerfassung](https://docs.microsoft.com/azure/network-watcher/network-watcher-alert-triggered-packet-capture) aus, und erhalten Sie Zugriff auf Leistungsinformationen in Echtzeit auf Paketebene. Wenn Sie ein Problem feststellen, können Sie dieses detailliert untersuchen, um es besser diagnostizieren zu können.

### <a name="gain-insight-into-your-network-traffic-using-flow-logs"></a>Gewinnen von Einblicken in den Netzwerkdatenverkehr mithilfe von Datenflussprotokollen
Lernen Sie mithilfe von [Datenflussprotokollen von Netzwerksicherheitsgruppen](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-nsg-flow-logging-overview) die Datenverkehrsmuster in Ihrem Netzwerk kennen. Mit den Daten aus den Datenflussprotokollen können Sie Informationen gewinnen, die Sie zum Erfüllen von Complianceanforderungen und zum Überwachen Ihres Netzwerksicherheitsprofils benötigen.

### <a name="diagnose-vpn-connectivity-issues"></a>Diagnostizieren von VPN-Konnektivitätsproblemen
Network Watcher ermöglicht es Ihnen, die [am häufigsten auftretenden VPN-Gateway- und Verbindungsprobleme zu diagnostizieren](https://docs.microsoft.com/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity). So können Sie die Probleme nicht nur identifizieren, sondern anhand detaillierter Protokolle genauer untersuchen.

Weitere Informationen zum Konfigurieren und Aktivieren von Network Watcher finden Sie im Artikel [Konfigurieren von Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="secure-deployment-using-proven-devops-tools"></a>Sichern der Bereitstellung mithilfe von bewährten DevOps-Tools
Dies sind einige Verfahren aus der Liste der Azure DevOps-Verfahren in diesem Microsoft Cloud-Bereich, die Unternehmen und Teams produktiver und effizienter machen.

-   **Infrastruktur als Code (IaC):** „Infrastruktur als Code“ ist ein Satz von Techniken und Methoden, die für IT-Experten die tägliche Erstellung und Verwaltung einer modularen Infrastruktur vereinfachen. IT-Experten können damit ihre moderne Serverumgebung so erstellen und verwalten, wie Softwareentwickler Anwendungscode erstellen und verwalten. Für Azure ermöglicht [Azure Resource Manager]( https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) die Bereitstellung Ihrer Anwendungen mit einer deklarativen Vorlage. In einer Vorlage können Sie mehrere Dienste zusammen mit ihren Abhängigkeiten bereitstellen. Sie verwenden die gleiche Vorlage, um Ihre Anwendung während jeder Phase des Anwendungslebenszyklus wiederholt bereitzustellen.
-   **Continuous Integration und Continuous Deployment:** Sie können Ihre Teamprojekte in Visual Studio Online so konfigurieren, dass sie [automatisch erstellt und in Azure-Web-Apps oder -Clouddiensten bereitgestellt](https://www.visualstudio.com/docs/build/overview) werden. VSO stellt die Binärdateien nach der Durchführung eines Builds in Azure nach jedem Einchecken von Code automatisch bereit. Der hier beschriebene Prozess der Paketerstellung entspricht dem Befehl „Paket“ in Visual Studio, und die Veröffentlichungsschritte entsprechen dem Befehl „Veröffentlichen“ in Visual Studio.
-   **Release Management:** Visual Studio [Release Management](https://msdn.microsoft.com/library/vs/alm/release/overview) ist eine hervorragende Lösung für die Automatisierung einer mehrstufigen Bereitstellung und die Verwaltung des Releaseprozesses. Erstellen Sie verwaltete Continuous Deployment-Pipelines für schnelle, einfache und häufige Releases. Mit Release Management können wir große Teile des Releaseprozesses automatisieren und vordefinierte Genehmigungsworkflows einrichten. Stellen Sie lokal und in der Cloud bereit, und führen Sie nach Bedarf Erweiterungen und Anpassungen durch.
-   **App-Leistungsüberwachung:** Erkennen und lösen Sie Probleme, und verbessern Sie Ihre Anwendungen kontinuierlich. Diagnostizieren Sie schnell alle Probleme in Ihrer Liveanwendung. Verstehen Sie, wie sie von den Benutzern verwendet wird. Zur Konfiguration werden einfach JS-Code und ein webconfig-Eintrag hinzugefügt, und Sie sehen die Ergebnisse innerhalb von Minuten mit allen Details im Portal. Mit [App Insights](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) können Unternehmen Probleme schneller erkennen und lösen.
-   **Auslastungstests und automatische Skalierung:** Wir können Leistungsprobleme in unserer App finden, um die Bereitstellungsqualität zu verbessern und um sicherzustellen, dass unsere App immer verfügbar ist, damit wir allen Unternehmensbedürfnissen gerecht werden. Stellen Sie sicher, dass Ihre App den Datenverkehr für die nächste Produkteinführung oder Marketingkampagne bewältigen kann. Beginnen Sie mit Visual Studio Online im Handumdrehen mit der Ausführung von cloudbasierten [Auslastungstests](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing).

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Azure Operational Security](https://docs.microsoft.com/azure/security/azure-operational-security).
- Erfahren Sie mehr über [Operations Management Suite – Security & Compliance](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [Erste Schritte mit der Sicherheits- und Überwachungslösung von Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started).

