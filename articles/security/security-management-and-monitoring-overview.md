---
title: "Azure-Sicherheitsverwaltung und -Überwachung – Übersicht | Microsoft Docs"
description: " Azure verfügt über Sicherheitsmechanismen, die als Hilfe bei der Verwaltung und Überwachung von Azure-Clouddiensten und virtuellen Azure-Computern dienen.  Dieser Artikel enthält eine Übersicht über diese Features und Dienste. "
services: security
documentationcenter: na
author: TerryLanfear
manager: StevenPo
editor: TomSh
ms.assetid: 5cf2827b-6cd3-434d-9100-d7411f7ed424
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: terrylan
ms.translationtype: HT
ms.sourcegitcommit: a9cfd6052b58fe7a800f1b58113aec47a74095e3
ms.openlocfilehash: 6787877deabafd0b7308e190cb45b4036049b05b
ms.contentlocale: de-de
ms.lasthandoff: 08/12/2017

---
# <a name="azure-security-management-and-monitoring-overview"></a>Azure-Sicherheitsverwaltung und -Überwachung – Übersicht
Azure verfügt über Sicherheitsmechanismen, die als Hilfe bei der Verwaltung und Überwachung von Azure-Clouddiensten und virtuellen Azure-Computern dienen. Dieser Artikel enthält eine Übersicht über diese Features und Dienste. Es sind jeweils Links zu Artikeln mit weiteren Informationen angegeben.

Die Sicherheit Ihrer Microsoft-Clouddienste basiert auf einer Partnerschaft zwischen Ihnen und Microsoft, und beide Parteien sind gemeinsam dafür verantwortlich. Diese gemeinsame Verantwortung bedeutet, dass Microsoft für Microsoft Azure und die physische Sicherheit seiner Rechenzentren verantwortlich ist (durch den Einsatz von Sicherheitsmaßnahmen wie Türen mit elektronischer Zugangskontrolle, Zäunen und Wachpersonal). Außerdem bietet Azure eine umfassende Cloudsicherheit auf Softwareebene, die auch die Anforderungen der anspruchsvollsten Kunden an Sicherheit, Datenschutz und Compliance erfüllt.

Sie sind der Eigentümer Ihrer Daten und Identitäten und für deren Schutz verantwortlich. Außerdem müssen Sie für die Sicherheit Ihrer lokalen Ressourcen und der Cloudkomponenten sorgen, die Ihrer Kontrolle unterliegen. Microsoft stellt Sicherheitskontrollen und -funktionen bereit, um Sie beim Schützen Ihrer Daten und Anwendungen zu unterstützen. Der Umfang Ihrer Zuständigkeit für die Sicherheit basiert auf dem Typ des Clouddiensts.

Im folgenden Diagramm sind die Informationen zur Verteilung der Zuständigkeiten für Microsoft und den Kunden zusammengefasst.

![Gemeinsame Verantwortung][1]

Ausführlichere Informationen zur Sicherheitsverwaltung finden Sie unter [Sicherheitsverwaltung in Azure](azure-security-management.md).

Hier sind die wichtigsten Features aufgeführt, die in diesem Artikel behandelt werden:

* Rollenbasierte Access Control
* Antimalware
* Multi-Factor Authentication
* ExpressRoute
* Gateways des virtuellen Netzwerks
* Privileged Identity Management
* Schutz der Identität (Identity Protection)
* Security Center

## <a name="role-based-access-control"></a>Rollenbasierte Access Control
Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) ermöglicht eine präzise Verwaltung des Zugriffs für Azure-Ressourcen. Mit RBAC können Sie den Benutzern beispielsweise nur die Zugriffsrechte gewähren, die diese zum Ausführen ihrer Aufgaben benötigen.  Außerdem können Sie mit RBAC sicherstellen, dass Personen, die das Unternehmen verlassen, keinen Zugriff mehr auf die Ressourcen in der Cloud haben.

Weitere Informationen:

* [Active Directory Team Blog zum Thema RBAC](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
* [Rollenbasierte Access Control in Azure](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Antimalware
Mit Azure können Sie zum Schützen Ihrer virtuellen Computer vor Dateien mit schädlichem Inhalt, Adware und anderen Bedrohungen Antischadsoftware von großen Anbietern wie Microsoft, Symantec, Trend Micro, McAfee und Kaspersky verwenden.

Bei Microsoft Antimalware haben Sie die Möglichkeit, einen Antischadsoftware-Agent für PaaS-Rollen und virtuelle Computer zu installieren. Dieses Feature basiert auf System Center Endpoint Protection und ermöglicht die Nutzung von bewährter lokaler Sicherheitstechnologie in der Cloud.

Darüber hinaus bieten wir die tiefe Integration für die Produkte [Deep Security](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ und [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ von Trend in die Azure-Plattform an. DeepSecurity ist eine Antivirenlösung, und SecureCloud ist eine Verschlüsselungslösung. DeepSecurity wird über ein Erweiterungsmodell auf VMs bereitgestellt. Indem Sie die Benutzeroberfläche des Portals und PowerShell verwenden, können Sie DeepSecurity auf neu erstellten, hochgefahrenen VMs oder auf vorhandenen VMs einsetzen, die bereits bereitgestellt wurden.

Symantec End Point Protection (SEP) wird unter Azure ebenfalls unterstützt. Dank der Portalintegration können Kunden angeben, dass sie SEP auf einer VM nutzen möchten. SEP kann über das Azure-Portal auf einer brandneuen VM oder mit PowerShell auf einer vorhandenen VM installiert werden.

Weitere Informationen:

* [Deploying Antimalware Solutions on Azure Virtual Machines (Bereitstellen von Antischadsoftware-Lösungen auf virtuellen Azure-Computern, in englischer Sprache)](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Microsoft Antimalware für Azure Cloud Services und Virtual Machines](azure-security-antimalware.md)
* [Installieren und Konfigurieren von Trend Micro Deep Security als Dienst auf einem virtuellen Windows-Computer](../virtual-machines/windows/classic/install-trend.md)
* [Installieren und Konfigurieren von Symantec Endpoint Protection auf einem virtuellen Windows-Computer](../virtual-machines/windows/classic/install-symantec.md)
* [New Antimalware Options for Protecting Azure Virtual Machines – McAfee Endpoint Protection](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Azure Multi-Factor Authentication (MFA) ist eine Authentifizierungsmethode, für die die Verwendung von mehr als einer Verifizierungsmethode erforderlich ist und die eine wichtige zweite Sicherheitsebene für Benutzeranmeldungen und Transaktionen darstellt. MFA hilft beim Schützen des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren. Sie bietet eine leistungsfähige Authentifizierung mit verschiedenen Überprüfungsoptionen – Telefonanruf, SMS oder Benachrichtigung bzw. Überprüfungscode in einer mobilen App sowie OATH-Token von Drittanbietern.

Weitere Informationen:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Was ist Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* [Funktionsweise von Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute
Mit Microsoft Azure ExpressRoute können Sie Ihre lokalen Netzwerke über eine dedizierte private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, in die Microsoft Cloud erweitern. Mit ExpressRoute können Sie Verbindungen mit Microsoft-Clouddiensten herstellen, z. B. Microsoft Azure, Office 365 und CRM Online. Die Konnektivität kann über ein Any-to-Any-Netzwerk (IP VPN), ein Point-to-Point-Ethernet-Netzwerk oder eine virtuelle Querverbindung über einen Konnektivitätsanbieter in einer Co-Location-Einrichtung bereitgestellt werden. ExpressRoute-Verbindungen verlaufen nicht über das öffentliche Internet. Auf diese Weise können ExpressRoute-Verbindungen eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit bei geringerer Latenz als herkömmliche Verbindungen über das Internet bieten.

Weitere Informationen:

* [ExpressRoute - Technische Übersicht](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Gateways des virtuellen Netzwerks
VPN Gateways, die auch als Azure Virtual Network Gateways bezeichnet werden, werden zum Senden von Netzwerkdatenverkehr zwischen virtuellen Netzwerken und lokalen Standorten verwendet. Außerdem dienen sie zum Senden von Datenverkehr zwischen mehreren virtuellen Netzwerken in Azure (VNET-zu-VNET).  VPN Gateways bieten sichere standortübergreifende Konnektivität zwischen Azure und Ihrer Infrastruktur.

Weitere Informationen:

* [Informationen zu VPN-Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [Übersicht über die Netzwerksicherheit in Azure](security-network-overview.md)

## <a name="privileged-identity-management"></a>Privileged Identity Management
Es kann vorkommen, dass Benutzer privilegierte Vorgänge in Azure-Ressourcen oder anderen SaaS-Anwendungen ausführen müssen. Dies bedeutet häufig, dass Organisationen diesen Benutzern in Azure Active Directory (Azure AD) dauerhaften privilegierten Zugriff gewähren müssen. Dies stellt ein zunehmendes Sicherheitsrisiko für die in der Cloud gehosteten Ressourcen dar, da Organisationen die Aktionen, die diese Benutzer mit dem privilegierten Zugriff ausführen, nicht ausreichend überwachen können.
Darüber hinaus kann die Sicherheit der gesamten Cloud in Gefahr sein, wenn ein Benutzerkonto mit privilegiertem Zugriff kompromittiert wird. Mit Azure AD Privileged Identity Management können Sie dieses Risiko minimieren, indem Sie Berechtigungen nicht unnötig lange gewähren und sich einen besseren Einblick in deren Nutzung verschaffen.  

Mit Privileged Identity Management wird das Konzept eines „temporären Administrators“ für eine Rolle bzw. Just-in-Time-Administratorzugriff eingeführt. Dies ist ein Benutzer, der für die zugewiesene Rolle einen Aktivierungsprozess durchführen muss. Der Aktivierungsprozess ändert die Zuweisung des Benutzers zu einer Rolle in Azure AD für einen bestimmten Zeitraum, z.B. acht Stunden, von „Inaktiv“ zu „Aktiv“.

Weitere Informationen:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Erste Schritte mit Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Schutz der Identität (Identity Protection)
Azure Active Directory (AD) Identity Protection ermöglicht die einheitliche Überwachung von verdächtigen Anmeldeaktivitäten und potenziellen Sicherheitsrisiken, um Ihr Unternehmen zu schützen. Identity Protection erkennt verdächtige Aktivitäten für Benutzer und privilegierte Identitäten (Administratoren) basierend auf Signalen wie beispielsweise Brute-Force-Angriffen, kompromittierten Anmeldeinformationen oder Anmeldeversuchen von unbekannten Standorten oder infizierten Geräten.

Indem Benachrichtigungen gesendet und Abhilfemaßnahmen empfohlen werden, trägt Identity Protection zur Minimierung von Risiken in Echtzeit bei. Der Schweregrad in Bezug auf das Benutzerrisiko wird berechnet, und Sie können risikobasierte Richtlinien konfigurieren, um den Anwendungszugriff automatisch vor neuen Bedrohungen zu schützen.

Weitere Informationen:

* [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md)
* [Channel 9: Azure AD and Identity Show: Identity Protection Preview (Channel 9: Azure AD- und Identity-Show: Vorschau von Identity Protection)](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Security Center
Azure Security Center unterstützt Sie beim Verhindern, Erkennen und Beheben von Bedrohungen und sorgt für eine größere Transparenz und bessere Kontrolle in Bezug auf die Sicherheit Ihrer Azure-Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden.

Mit Security Center können Sie die Sicherheit Ihrer Azure-Ressourcen wie folgt optimieren und überwachen:

* Sie können Richtlinien für die Ressourcen Ihres Azure-Abonnements gemäß den Sicherheitsanforderungen Ihres Unternehmens sowie gemäß den Anwendungstypen oder der Vertraulichkeit der Daten in jedem Abonnement definieren.
* Sie können den Status Ihrer virtuellen Azure-Computer, -Netzwerke und -Anwendungen überwachen.
* Es wird eine Liste mit priorisierten Sicherheitswarnungen bereitgestellt, z.B. Warnungen integrierter Partnerlösungen, sowie mit den Informationen, die Sie zum schnellen Untersuchen von Problemen benötigen. Außerdem sind Empfehlungen zum Reagieren auf einen Angriff vorhanden.

Weitere Informationen:

* [Einführung in Azure Security Center](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png

