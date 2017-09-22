---
title: Technische Funktionen der Azure-Sicherheit | Microsoft-Dokumentation
description: "Enthält Informationen zu cloudbasierten Computingdiensten mit einer großen Auswahl an Computeinstanzen und -diensten, die automatisch zentral hoch- und herunterskaliert werden können, um die Anforderungen Ihrer Anwendung bzw. Ihres Unternehmens zu erfüllen."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/26/2017
ms.author: TomSh
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: a32693a274c2615a515838306d72dbe64a5d1763
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="azure-security-technical-capabilities"></a>Technische Funktionen der Azure-Sicherheit

Microsoft hat eine Reihe von Whitepapers, Sicherheitsübersichten, bewährten Methoden und Prüflisten entwickelt, um aktuelle und zukünftige Azure-Kunden dabei zu unterstützen, die verschiedenen sicherheitsbezogenen Funktionen in und um die Azure Platform herum zu verstehen und zu verwenden. Die Themen variieren in Bezug auf Umfang und Reichweite und werden regelmäßig aktualisiert. Dieses Dokument ist Teil dieser Serie (im Abschnitt unten zusammengefasst). Weitere Informationen zur Reihe zur Azure-Sicherheit finden Sie unter (URL).

## <a name="azure-platform"></a>Azure Platform

[Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) ist eine Cloudplattform mit Infrastruktur- und Anwendungsdiensten, integrierten Datendiensten und erweiterten Analysefunktionen sowie Entwicklertools und -diensten, die in den öffentlichen Cloud-Rechenzentren von Microsoft gehostet werden. Kunden verwenden Azure für viele verschiedene Kapazitäten und Szenarien: von einfachen Compute-, Netzwerk- und Speicheraufgaben über Dienste für mobile Apps und Web-Apps bis hin zu vollständigen Cloudszenarien wie dem Internet der Dinge. Azure kann mit Open Source-Technologie genutzt und als Hybrid Cloud bereitgestellt oder im Datencenter eines Kunden gehostet werden. In Azure wird Cloudtechnologie in Form von Bausteinen bereitgestellt, damit Unternehmen Kosten sparen, Innovationen schnell umsetzen und Systeme proaktiv verwalten können. Wenn Sie IT-Ressourcen erstellen oder zu einem Cloudanbieter migrieren, verlassen Sie sich darauf, dass die jeweilige Organisation Ihre Anwendungen und Daten mit den Diensten und Steuerungsmöglichkeiten schützen kann, die zum Verwalten der Sicherheit Ihrer cloudbasierten Ressourcen bereitgestellt werden.

Microsoft Azure ist der einzige Cloud Computing-Anbieter, der eine sichere, einheitliche Anwendungsplattform und Infrastructure-as-a-Service bereitstellt, damit Teams mit unterschiedlichen Cloudkenntnissen die jeweiligen Ebenen der Projektkomplexität bewältigen können. Hierfür sind integrierte Datendienste und Analysefunktionen vorhanden, mit denen umfassende Erkenntnisse aus Daten gewonnen werden können – auf Microsoft-Plattformen und anderen Plattformen und für offene Frameworks und Tools. Sie können die Cloud in lokale Datencenter integrieren und Azure-Clouddienste in lokalen Datencentern bereitstellen. Im Rahmen der vertrauenswürdigen Cloud von Microsoft (Microsoft Trusted Cloud) nutzen Kunden Azure, weil sie branchenführende Sicherheit, Zuverlässigkeit, Konformität, Datenschutz und ein großes Netzwerk mit Ansprechpartnern, Partnerunternehmen und Prozessen als Unterstützung für Organisationen in der Cloud erhalten.

Mit Microsoft Azure haben Sie folgende Möglichkeiten:

- Beschleunigen der Innovation mit der Cloud

- Nutzen von Erkenntnissen für Unternehmensentscheidungen und -Apps

- Freies Erstellen und Bereitstellen an jedem Ort

- Schützen Ihres Unternehmens

## <a name="scope"></a>Umfang

Der Schwerpunkt dieses Whitepapers liegt auf Sicherheitsfeatures und -funktionen, die die Kernkomponenten von Microsoft Azure unterstützen, z.B. [Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-introduction), [Microsoft Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/), [VM-Modell von Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/    ) und Tools und Infrastruktur für die allgemeine Verwaltung. In diesem Whitepaper werden die technischen Fähigkeiten von Microsoft Azure behandelt, die Ihnen als Kunde zur Verfügung stehen, um für die Sicherheit und den Schutz Ihrer Daten zu sorgen.

Es ist wichtig, dass Kunden, die eine Umstellung auf die Cloud durchführen, sich der Bedeutung dieses Modells der gemeinsamen Verantwortung bewusst sind. Angebote von Cloudanbietern sind mit großen Vorteilen in Bezug auf die Sicherheit und Konformität verbunden, aber dies bedeutet nicht, dass der Kunde nicht mehr für den Schutz seiner Benutzer, Anwendungen und Dienstangebote verantwortlich ist.

Bei IaaS-Lösungen ist der Kunde ganz oder teilweise dafür verantwortlich, das Betriebssystem, die Netzwerkkonfiguration, Anwendungen, Identitäten, Clients und Daten zu schützen und zu verwalten.  PaaS-Lösungen bauen auf IaaS-Bereitstellungen auf, und der Kunde ist auch hier weiterhin ganz oder teilweise für den Schutz und die Verwaltung von Anwendungen, Identitäten, Clients und Daten verantwortlich. Bei SaaS-Lösungen liegt die Verantwortung trotzdem immer noch beim Kunden. Er muss sicherstellen, dass Daten richtig klassifiziert sind, und ist auch mit dafür verantwortlich, seine Benutzer und Endpunktgeräte zu verwalten.

In diesem Dokument werden die verwandten Komponenten der Microsoft Azure-Plattform, z.B. Azure Websites, Azure Active Directory, HDInsight, Media Services und andere Dienste, die oberhalb der Kernkomponenten angeordnet sind, nicht ausführlich behandelt. Auch wenn einige wenige allgemeine Informationen vorhanden sind, wird vorausgesetzt, dass der Leser mit den Grundbegriffen von Azure vertraut ist, die in anderen Referenzdokumenten von Microsoft und unter den in diesem Whitepaper angegebenen Links beschrieben werden.


## <a name="available-security-technical-capabilities-to-fulfil-user-customer-responsibility---big-picture"></a>Verfügbare technische Sicherheitsfunktionen für die Übernahme der Verantwortung für den Benutzer (Kunden) – Gesamtübersicht

In Microsoft Azure werden Dienste bereitgestellt, mit denen Kunden ihre Sicherheits-, Datenschutz- und Konformitätsanforderungen erfüllen können. Die folgende Abbildung ist eine Hilfe zur Erläuterung der verschiedenen Azure-Dienste, die Benutzer zum Erstellen einer sicheren und konformen Anwendungsinfrastruktur basierend auf Branchenstandards verwenden können.

![Verfügbare technische Sicherheitsfunktionen – Gesamtübersicht](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig1.png)

## <a name="manage-and-control-identity-and-user-access-protect"></a>Verwalten und Steuern der Identität und des Benutzerzugriffs (Schutz)

Azure unterstützt Sie beim Schützen von geschäftlichen und persönlichen Informationen, indem Sie in die Lage versetzt werden, Benutzeridentitäten und Anmeldeinformationen zu verwalten und den Zugriff zu steuern.

### <a name="azure-active-directory"></a>Azure Active Directory

Lösungen zur Identitäts- und Zugriffsverwaltung von Microsoft unterstützen IT-Profis dabei, den Zugriff auf Anwendungen und Ressourcen über das Unternehmensrechenzentrum und in der Cloud zu schützen, wobei zusätzliche Ebenen der Überprüfungen aktiviert werden, z.B. mehrstufige Authentifizierung und Richtlinien für bedingten Zugriff. Die Überwachung verdächtiger Aktivitäten über erweiterte Sicherheitsberichtserstellung, Überwachung und Warnung trägt dazu bei, potenzielle Sicherheitsprobleme zu verringern. [Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/active-directory-editions) ermöglicht einmaliges Anmelden bei Tausenden von Cloudanwendungen (SaaS) und Zugriff auf Webanwendungen, die Sie lokal ausführen.

Zu den Sicherheitsvorteilen von Azure Active Directory (AD) zählen folgende Möglichkeiten:

- Erstellen und Verwalten einer einzelnen Identität für jeden Benutzer in Ihrer gesamten hybriden Unternehmensumgebung, wobei Benutzer, Gruppen und Geräte synchronisiert werden

- Bereitstellen des Zugriffs auf Ihre Anwendungen mit einmaligem Anmelden, inklusive Tausender bereits integrierter SaaS-Apps

- Aktivieren der Sicherheit für den Anwendungszugriff durch Erzwingen der regelbasierten Multi-Factor Authentication für lokale Anwendungen und Cloudanwendungen

- Bereitstellen des sicheren Remotezugriffs auf lokale Webanwendungen mit dem Azure AD-Anwendungsproxy

Das [Azure Active Directory-Portal](http://aad.portal.azure.com/) ist über das Azure-Portal zugänglich. Über dieses Dashboard erhalten Sie einen Überblick über den Zustand Ihrer Organisation und können den Zugriff auf Verzeichnisse, Benutzer oder Anwendungen schnell und einfach verwalten.

![Azure Active Directory](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig2.png)

Hier sind die wichtigsten Funktionen der Azure-Identitätsverwaltung aufgeführt:

- Einmaliges Anmelden

- Multi-Factor Authentication

- Sicherheitsüberwachung, Warnungen und Machine Learning-basierte Berichte

- Kundenidentitäts- und Kundenzugriffsverwaltung

- Geräteregistrierung

- Privileged Identity Management

- Schutz der Identität (Identity Protection)

#### <a name="single-sign-on"></a>Einmaliges Anmelden

[Einmaliges Anmelden](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) (Single Sign-On, SSO) bedeutet, dass Sie Zugriff auf sämtliche für Ihre Geschäftsaktivitäten benötigten Anwendungen und Ressourcen erhalten, indem Sie sich nur einmal mit einem einzigen Benutzerkonto anmelden. Nach der Anmeldung können Sie auf alle benötigten Anwendungen zugreifen, ohne sich ein zweites Mal (z.B. durch Eingabe eines Kennworts) authentifizieren zu müssen.

Viele Organisationen nutzen SaaS-Anwendungen (Software-as-a-Service), z.B. Office 365, Box und Salesforce, um die Endbenutzerproduktivität zu steigern. In der Vergangenheit musste das IT-Personal Benutzerkonten in jeder SaaS-Anwendung individuell erstellen und aktualisieren, und Benutzer mussten sich für jede SaaS-Anwendung ein Kennwort merken.

[Azure AD weitet das lokale Active Directory auf die Cloud aus](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) und ermöglicht den Benutzern auf diese Weise, sich mit ihrem primären Organisationskonto nicht nur bei den mit ihrer Domäne verknüpften Geräten und Unternehmensressourcen anzumelden, sondern auch bei sämtlichen Web- und SaaS-Anwendungen, die sie für ihre Arbeit benötigen.

Benutzer müssen sich keine Vielzahl von Benutzernamen und Kennwörtern mehr merken, und der Anwendungszugriff für Benutzer kann basierend auf Organisationsgruppen sowie ihrem Mitarbeiterstatus automatisch bereitgestellt oder deaktiviert werden. [Azure AD stellt Funktionen zum Steuern von Sicherheit und Zugriff bereit](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps), mit denen Sie den Benutzerzugriff auf SaaS-Anwendungen zentral verwalten können.

#### <a name="multi-factor-authentication"></a>Multi-Factor Authentication

[Azure Multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) ist eine Authentifizierungsmethode, für die die Verwendung von mehr als einem Verifizierungsverfahren erforderlich ist und die eine wichtige zweite Sicherheitsebene für Benutzeranmeldungen und Transaktionen darstellt. [MFA hilft beim Schützen](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-how-it-works) des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren. Sie bietet eine leistungsfähige Authentifizierung mit verschiedenen Überprüfungsoptionen – Telefonanruf, SMS oder per Benachrichtigung bzw. Überprüfungscode in einer mobilen App sowie OAuth-Token von Drittanbietern.

#### <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Sicherheitsüberwachung, Warnungen und Machine Learning-basierte Berichte

Sicherheitsüberwachung und Warnungen sowie Berichte auf Machine Learning-Basis, die inkonsistente Zugriffsmuster erkennen und Ihnen helfen können, Ihr Unternehmen zu schützen. Sie können die Zugriffs- und Nutzungsberichte von Azure Active Directory verwenden, um sich einen Einblick in die Integrität und Sicherheit des Verzeichnisses Ihrer Organisation zu verschaffen. Mithilfe dieser Informationen kann ein Verzeichnisadministrator mögliche Sicherheitsrisiken besser bestimmen, um angemessen zu planen, wie diese Risiken eingedämmt werden können.

Im klassischen Azure-Portal oder [Azure Active Directory-Portal](http://aad.portal.azure.com/) sind [Berichte](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) wie folgt kategorisiert:

- Anomalieberichte: Enthalten Anmeldeereignisse, die wir als anomal eingestuft haben. Unser Ziel ist, Sie auf solche Aktivitäten aufmerksam zu machen und es Ihnen zu ermöglichen, eine Entscheidung zu treffen, ob ein Ereignis verdächtig ist.

- Integrierte Anwendungsberichte – Bieten Einblicke, wie Cloudanwendungen in Ihrer Organisation verwendet werden. Azure Active Directory ermöglicht die Integration in Tausende von Cloudanwendungen.

- Fehlerberichte: Enthalten Hinweise auf Fehler, die bei der Bereitstellung von Konten für externe Anwendungen auftreten können.

- Benutzerspezifische Berichte: Enthalten Geräte-/Anmeldeaktivitätsdaten für einen bestimmten Benutzer.

- Aktivitätsprotokolle – Enthalten eine Aufzeichnung aller überwachten Ereignisse in den letzten 24 Stunden, 7 Tagen oder 30 Tagen sowie geänderte Gruppenaktivitäten und Kennwortzurücksetzungs- und Registrierungsaktivitäten

#### <a name="consumer-identity-and-access-management"></a>Kundenidentitäts- und Kundenzugriffsverwaltung

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) ist ein hoch verfügbarer, globaler Identitätsverwaltungsdienst für kundenorientierte Anwendungen, der für Hunderte Millionen von Identitäten skaliert werden kann. Er kann über mobile und Webplattformen integriert werden. Ihre Kunden können sich über eine anpassbare Oberfläche bei all Ihren Anwendungen anmelden und zu diesem Zweck entweder vorhandene Konten aus sozialen Netzwerken nutzen oder neue Anmeldeinformationen festlegen.

In der Vergangenheit mussten Anwendungsentwickler ihren eigenen Code schreiben, wenn sie [Verbraucher in ihren Anwendungen registrieren und anmelden](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview) wollten. Und sie hätten lokale Datenbanken oder Systeme zum Speichern von Benutzernamen und Kennwörtern verwendet. Azure Active Directory B2C bietet Ihrer Organisation eine bessere Lösung, um die Verwaltung von Kundenidentitäten in ihre Anwendungen zu integrieren. Dabei kommen eine sichere standardbasierte Plattform und ein umfassender Satz von erweiterbaren Richtlinien zum Einsatz.

Wenn Sie Azure Active Directory B2C verwenden, können sich Ihre Kunden mit vorhandenen Konten in sozialen Netzwerken (Facebook, Google, Amazon, LinkedIn) oder durch Erstellen neuer Anmeldeinformationen (E-Mail-Adresse und Kennwort oder Benutzername und Kennwort) bei Ihren Anwendungen registrieren.

Geräteregistrierung

Die [Azure AD Device Registration](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-device-registration-overview) ist die Grundlage gerätebasierter Szenarien für den [bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-device-registration-overview). Wenn ein Gerät registriert wird, stellt die Azure Active Directory-Geräteregistrierung eine Identität für das Gerät bereit, die bei der Benutzeranmeldung zum Authentifizieren des Geräts dient. Das authentifizierte Gerät und die Attribute des Geräts können anschließend verwendet werden, um bedingte Zugriffsrichtlinien für Anwendungen zu erzwingen, die in der Cloud und lokal gehostet werden.

In Kombination mit einer Lösung für die [mobile Geräteverwaltung](https://www.microsoft.com/itshowcase/Article/Content/588/Mobile-device-management-at-Microsoft), z.B. Intune, werden die Geräteattribute in Azure Active Directory mit zusätzlichen Informationen über das Gerät aktualisiert. So können Sie Regeln für den bedingten Zugriff erstellen, die erzwingen, dass der Zugriff von Geräten Ihren Standards für Sicherheit und Kompatibilität entspricht.

#### <a name="privileged-identity-management"></a>Privileged Identity Management

Mithilfe von [Azure Active Directory (AD) Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) können Sie Ihre privilegierten Identitäten und deren Zugriff auf Ressourcen in Azure AD und anderen Microsoft Online Services wie Office 365 oder Microsoft Intune verwalten, steuern und überwachen.

Manchmal müssen Benutzer privilegierte Vorgänge in Azure- oder Office 365-Ressourcen oder anderen SaaS-Apps ausführen. Dies bedeutet häufig, dass Organisationen diesen Benutzern in Azure AD dauerhaften privilegierten Zugriff gewähren müssen. Dies stellt ein wachsendes Sicherheitsrisiko für die in der Cloud gehosteten Ressourcen dar, da Organisationen die Aktionen, die diese Benutzer mit ihren Administratorberechtigungen ausführen, nicht ausreichend überwachen können. Darüber hinaus kann die Sicherheit der gesamten Cloud in Gefahr sein, wenn ein Benutzerkonto mit privilegiertem Zugriff kompromittiert wird. Mit Azure AD Privileged Identity Management können Sie dieses Risiko in den Griff bekommen.

Azure AD Privileged Identity Management ermöglicht Ihnen Folgendes:

- Ermitteln, welche Benutzer Azure AD-Administratoren sind

- Aktivieren von bedarfsgesteuertem Just-In-Time-Administratorzugriff auf Microsoft Online Services wie z. B. Office 365 und Intune

- Abrufen von Berichten zum Administratorzugriffsverlauf und zu Änderungen bei Administratorzuweisungen

- Aktivieren von Benachrichtigungen zum Zugriff auf eine privilegierte Rolle

#### <a name="identity-protection"></a>Schutz der Identität (Identity Protection)

[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) ist ein Sicherheitsdienst, der eine umfassende Übersicht über Risikoereignisse und potenzielle Sicherheitsrisiken bietet, die für die Identitäten Ihrer Organisation bestehen. Für Identity Protection werden die vorhandenen Azure Active Directory-Funktionen zur Anomalieerkennung genutzt (über die Berichte zu anomalen Aktivitäten von Azure AD), und es werden neue Risikoereignistypen eingeführt, mit denen Anomalien in Echtzeit erkannt werden können.

## <a name="secured-resource-access-in-azure"></a>Zugriff auf geschützte Ressourcen in Azure

Die Zugriffssteuerung in Azure unterliegt zunächst den Abrechnungsaspekten. Der Besitzer eines Azure-Kontos, auf das über das [Azure-Kontocenter](https://account.windowsazure.com/subscriptions) zugegriffen wird, ist der Kontoadministrator (Account Administrator, AA). Abonnements dienen nicht nur als Container für die Abrechnung, sondern auch als Sicherheitsgrenze: Jedes Abonnement verfügt über einen Dienstadministrator (SA), der Azure-Ressourcen für dieses Abonnement mit dem [klassischen Azure-Portal](https://manage.windowsazure.com/) hinzufügen, entfernen und ändern kann. Der standardmäßige Dienstadministrator eines neuen Abonnements ist der Kontoadministrator. Der Kontoadministrator kann den Dienstadministrator jedoch über das Azure Accounts Center ändern.

![Zugriff auf geschützte Ressourcen in Azure](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig3.png)

Außerdem sind Abonnements einem Verzeichnis zugeordnet. Durch das Verzeichnis wird eine Gruppe von Benutzern definiert. Dies können Benutzer aus der Organisation (Unternehmen, Uni oder Schule) sein, die das Verzeichnis erstellt hat, oder externe Benutzer (also Microsoft-Konten). Auf Abonnements kann über eine Teilmenge dieser Verzeichnisbenutzer zugegriffen werden, die entweder als Dienstadministrator (SA) oder Co-Administrator (CA) zugewiesen wurden. Die einzige Ausnahme besteht darin, dass Microsoft-Konten (früher Windows Live ID) – aus rechtlichen Gründen – als SA oder CA zugewiesen werden können, ohne im Verzeichnis vorhanden zu sein.

Sicherheitsorientierte Unternehmen müssen sich darauf konzentrieren, Mitarbeitern genau die Berechtigungen zuzuweisen, die sie benötigen. Zu viele Berechtigungen können ein Konto zum leichten Angriffsziel machen. Wenn ihre Berechtigungen nicht ausreichen, können Mitarbeiter nicht effizient arbeiten. Die [rollenbasierte Zugriffssteuerung in Azure (Role-Based Access Control, RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) begegnet diesem Problem dadurch, dass eine präzise Zugriffsverwaltung für Azure ermöglicht wird.

![Zugriff auf geschützte Ressourcen ](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig4.png)

Mithilfe von RBAC können Sie Aufgaben in Ihrem Team verteilen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen. Anstatt allen uneingeschränkte Berechtigungen in Ihrem Azure-Abonnement oder Ihren Ressourcen zu gewähren, können Sie die Berechtigungen auf bestimmte Aktionen beschränken. Gestatten Sie z.B. mit RBAC einem Mitarbeiter die Verwaltung virtueller Computer in einem Abonnement, während ein anderer im gleichen Abonnement SQL-Datenbanken verwalten kann.

![Zugriff auf geschützte Ressourcen in Azure (RBAC)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig5.png)

## <a name="azure-data-security-and-encryption-protect"></a>Datensicherheit und Verschlüsselung in Azure (Schutz)

Einer der Schlüssel zum Schutz von Daten in der Cloud ist die Berücksichtigung der möglichen Zustände, in denen Ihre Daten auftreten können. Außerdem sollten Sie die Steuerungsmöglichkeiten beachten, die für diesen Zustand verfügbar sind. Im Rahmen der empfohlenen Vorgehensweisen für Datensicherheit und Verschlüsselung in Azure befassen sich die Empfehlungen mit den folgenden Datenzuständen:

- Ruhende Daten: Dies umfasst alle Informationsspeicherobjekte, Container und Typen, die statisch in physischen Medien existieren, sei es auf Magnetplattenspeichern oder optischen Datenträgern.

- Während der Übertragung: Wenn Daten zwischen Komponenten, Speicherorten oder Programmen übertragen werden, beispielsweise über das Netzwerk, über einen Service Bus (von dem lokalen Computer in die Cloud und umgekehrt, inklusive Hybridverbindungen wie ExpressRoute) oder während eines Eingabe-/Ausgabevorganges, werden Sie als „in Bewegung befindlich“ betrachtet.

### <a name="encryption--rest"></a>Verschlüsselung ruhender Daten

Für die Verschlüsselung ruhender Daten ist Folgendes erforderlich:

Unterstützung von mindestens einem der empfohlenen Verschlüsselungsmodelle zum Verschlüsseln von Daten, die in der folgenden Tabelle beschrieben sind.

| Verschlüsselungsmodelle |  |  |  |
| ----------------  | ----------------- | ----------------- | --------------- |
| Serververschlüsselung | Serververschlüsselung | Serververschlüsselung | Clientverschlüsselung
| Serverseitige Verschlüsselung mit vom Dienst verwalteten Schlüsseln | Serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln in Azure Key Vault | Serverseitige Verschlüsselung mit lokalen vom Kunden verwalteten Schlüsseln |
| • Azure-Ressourcenanbieter führen die Verschlüsselungs- und Entschlüsselungsvorgänge durch <br> • Microsoft verwaltet die Schlüssel <br>• Vollständige Cloud-Funktionen | • Azure-Ressourcenanbieter führen die Verschlüsselungs- und Entschlüsselungsvorgänge durch<br>• Kunde steuert Schlüssel per Azure Key Vault<br>• Vollständige Cloud-Funktionen | • Azure-Ressourcenanbieter führen die Verschlüsselungs- und Entschlüsselungsvorgänge durch <br>• Kunde steuert Schlüssel lokal <br> • Vollständige Cloud-Funktionen| • Azure-Dienste können entschlüsselte Daten nicht erkennen <br>• Kunden bewahren die Schlüssel lokal auf (oder in anderen sicheren Speichern). Schlüssel sind für Azure-Dienste nicht verfügbar. <br>• Reduzierte Cloud-Funktionen|

### <a name="enabling-encryption-at-rest"></a>Aktivieren der Verschlüsselung ruhender Daten

**Identifizieren aller Speicherorte Ihrer gespeicherten Daten**

Das Ziel der Verschlüsselung ruhender Daten ist die Verschlüsselung aller Daten. Hiermit wird verhindert, dass wichtige Daten oder alle persistent gemachten Standorte übergangen werden. Sie können alle Daten auflisten, die von Ihrer Anwendung gespeichert wurden. 

> [!Note] 
> Dies gilt nicht nur für „Anwendungsdaten“ oder „personenbezogene Informationen“, sondern für alle Daten, die sich auf die Anwendung beziehen, z.B. Kontometadaten (Abonnementzuordnungen, Vertragsinformationen, personenbezogene Informationen).

Überlegen Sie, welche Speicher Sie zum Speichern von Daten verwenden. Beispiel:

- Externer Speicher (z.B. SQL Azure, DocumentDB, HDInsights, Data Lake usw.)

- Temporärer Speicher (beliebiger lokaler Cache, der Mandantendaten enthält)

- In-Memory-Cache (kann in die Auslagerungsdatei eingefügt werden)

### <a name="leverage-the-existing-encryption-at-rest-support-in-azure"></a>Nutzen der vorhandenen Unterstützung zur Verschlüsselung ruhender Daten in Azure

Für jeden verwendeten Speicher können Sie die vorhandene Unterstützung zur Verschlüsselung ruhender Daten nutzen.

- Azure Storage: Siehe [Azure Storage Service Encryption für ruhende Daten](https://docs.microsoft.com/azure/storage/storage-service-encryption).

- SQL Azure: Siehe [Always Encrypted (Datenbankmodul)](https://msdn.microsoft.com/library/mt163865.aspx)

- VM- und lokaler Datenträgerspeicher ([Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption))

Verwenden Sie für VM- und lokalen Datenträgerspeicher Azure Disk Encryption, sofern dies unterstützt wird:

IaaS

Für Dienste mit IaaS-VMs (Windows oder Linux) sollte [Azure Disk Encryption](https://microsoft.sharepoint.com/teams/AzureSecurityCompliance/Security/SitePages/Azure%20Disk%20Encryption.aspx) verwendet werden, um Volumes zu verschlüsseln, die Kundendaten enthalten.

PaaS v2

Für Dienste, die unter PaaS v2 mit Service Fabric ausgeführt werden, kann Azure Disk Encryption für VM-Skalierungsgruppen (VMSS) verwendet werden, um die entsprechenden PaaS v2-VMs zu verschlüsseln.

PaaS v1

Für PaaS v1 wird Azure Disk Encryption derzeit nicht unterstützt. Daher müssen Sie die Verschlüsselung auf Anwendungsebene verwenden, um persistent gemachte ruhende Daten zu verschlüsseln.  Dies gilt auch für Anwendungsdaten, temporäre Dateien, Protokolle und Absturzabbilder, aber ist nicht darauf beschränkt.

Für die meisten Dienste sollte versucht werden, die Verschlüsselung eines Anbieters von Speicherressourcen zu nutzen. Für einige Dienste muss eine explizite Verschlüsselung durchgeführt werden. Beispielsweise müssen alle persistent gemachten wichtigen Daten (Zertifikate, Stamm-/Hauptschlüssel) in Key Vault gespeichert werden.

Wenn Sie die serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln unterstützen, muss für den Kunden die Möglichkeit bestehen, uns den Schlüssel zur Verfügung zu stellen. Die unterstützte und empfohlene Vorgehensweise ist hierbei die Integration in Azure Key Vault (AKV). Kunden können ihre Schlüssel dann in Azure Key Vault hinzufügen und verwalten. Ein Kunde kann sich unter [Erste Schritte mit dem Azure-Schlüsseltresor](http://go.microsoft.com/fwlink/?linkid=521402) darüber informieren, wie AKV genutzt wird.

Für die Integration in Azure Key Vault fügen Sie Code hinzu, mit dem bei Bedarf für die Entschlüsselung ein Schlüssel von AKV angefordert wird.

- Informationen zur Integration mit AKV finden Sie unter [Azure Key Vault – Step by Step](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step/) (Azure Key Vault – Schritt für Schritt).

Wenn Sie vom Kunden verwaltete Schlüssel unterstützen, müssen Sie eine Benutzeroberfläche für Kunden bereitstellen, über die der zu verwendende Key Vault (bzw. der Key Vault-URI) angegeben werden kann.

Da die Verschlüsselung ruhender Daten die Verschlüsselung von Host-, Infrastruktur- und Mandantendaten umfasst, kann der Verlust der Schlüssel aufgrund eines Systemausfalls oder einer schädlichen Aktivität bedeuten, dass alle verschlüsselten Daten verloren gehen. Aus diesem Grund ist es von entscheidender Bedeutung, dass Ihre Lösung für die Verschlüsselung ruhender Daten über umfassende Funktionalität für die Notfallwiederherstellung verfügt, die gegen Systemausfälle und schädliche Aktivitäten resistent ist.

Dienste, für die die Verschlüsselung ruhender Daten implementiert wird, sind normalerweise weiterhin dafür anfällig, dass die Verschlüsselungsschlüssel oder die Daten unverschlüsselt auf dem Hostlaufwerk aufbewahrt werden (z.B. in der Auslagerungsdatei des Hostbetriebssystems). Für Dienste muss also sichergestellt werden, dass das verwendete Hostvolume verschlüsselt ist. Zu diesem Zweck hat das Compute-Team die Bereitstellung der Hostverschlüsselung ermöglicht, bei der zum Verschlüsseln des Hostvolumes [BitLocker](https://technet.microsoft.com/library/dn306081.aspx) NKP und Erweiterungen des DCM-Diensts und -Agents verwendet werden.

Die meisten Dienste werden auf virtuellen Azure-Standardcomputern implementiert. Für diese Dienste sollte die [Hostverschlüsselung](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) automatisch abgerufen werden, wenn sie von Compute aktiviert wird. Für Dienste, die in per Compute verwalteten Clustern ausgeführt werden, wird die Hostverschlüsselung automatisch aktiviert, wenn der Rollout von Windows Server 2016 durchgeführt wird.

### <a name="encryption-in-transit"></a>Verschlüsselung während der Übertragung

Der Schutz von Daten während der Übertragung sollte ein wesentlicher Bestandteil Ihrer Datenschutzstrategie sein. Da die Daten zwischen vielen verschiedenen Speicherorten übertragen werden, empfiehlt es sich im Allgemeinen, immer SSL/TLS-Protokolle zu verwenden, um Daten zwischen verschiedenen Speicherorten auszutauschen. In einigen Fällen sollten Sie den gesamten Kommunikationskanal zwischen Ihrer lokalen und Ihrer Cloudinfrastruktur isolieren, indem Sie ein virtuelles privates Netzwerk (VPN) verwenden.

Für Daten, die sich zwischen Ihrer lokalen Infrastruktur und Azure bewegen, sollten Sie passende Sicherheitsmaßnahmen in Betracht ziehen, beispielsweise HTTPS oder VPN.

Verwenden Sie eine [Azure-Site-to-Site-VPN-Verbindung ](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-site-to-site-create) für Organisationen, die den Zugriff von mehreren lokalen Arbeitsstationen auf Azure sichern müssen.

Verwenden Sie eine [Punkt-zu-Standort-VPN-Verbindung](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-point-to-site-create) für Organisationen, die den Zugriff von einer lokalen Arbeitsstation auf Azure sichern müssen.

Größere Datasets können über dedizierte Hochgeschwindigkeits-WAN-Verbindungen wie [ExpressRoute](https://azure.microsoft.com/services/expressroute/) verschoben werden. Falls Sie sich für die Verwendung von ExpressRoute entscheiden, können Sie die Daten auch auf Anwendungsebene verschlüsseln, indem Sie [SSL/TLS](https://support.microsoft.com/kb/257591) oder andere Protokolle für zusätzlichen Schutz verwenden.

Falls Sie mit Azure Storage über das Azure-Portal interagieren, werden alle Transaktionen über HTTPS durchgeführt. [Speicher-REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx) über HTTPS kann auch verwendet werden, um mit [Azure Storage](https://azure.microsoft.com/services/storage/) und [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) zu interagieren.

Organisationen, die die Daten während der Übertragung nicht schützen, sind anfälliger für [Man-in-the-Middle-Angriffe](https://technet.microsoft.com/library/gg195821.aspx), [Abhöraktionen](https://technet.microsoft.com/library/gg195641.aspx) und Session Hijacking. Bei diesen Angriffen kann es sich um den ersten Schritt zur Zugriffsgewinnung auf vertrauliche Daten handeln.

Erfahren Sie mehr über die Azure-VPN-Option im Artikel [Planung und Entwurf für VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

### <a name="enforce-file-level-data-encryption"></a>Erzwingen der Datenverschlüsselung auf Dateiebene

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) verwendet Verschlüsselungs-, Identitäts- und Autorisierungsrichtlinien, um Ihre Dateien und E-Mails zu schützen. Azure RMS funktioniert über mehrere Geräte (Handys, Tablets und PCs), indem Schutz sowohl innerhalb als auch außerhalb Ihrer Organisation geboten wird. Diese Funktion ist möglich, weil Azure RMS eine Schutzebene hinzufügt, die bei den Daten verbleibt, selbst wenn diese die Grenzen Ihrer Organisation verlassen.

Wenn Sie Azure RMS verwenden, um Ihre Dateien zu schützen, verwenden Sie eine Kryptographie, die dem Branchenstandard entspricht und [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html) voll unterstützt. Wenn Sie Azure RMS für den Datenschutz einsetzen, haben Sie die Garantie, dass die Datei geschützt bleibt, auch wenn diese in einen Speicher kopiert wird, der nicht von der IT kontrolliert wird, beispielsweise einem Cloudspeicherdienst. Dasselbe gilt für Dateien, die mittels E-Mail freigegeben werden. Die Datei ist als Anhang der E-Mail geschützt, und es wird eine Anleitung mitgeschickt, die erklärt, wie der geschützt Anhang geöffnet wird.
Bei der Planung der Einführung von Azure RMS empfehlen wir Folgendes:

- Installieren Sie die [RMS-Freigabe-App](https://technet.microsoft.com/library/dn339006.aspx). Diese App integriert sich mit Office-Anwendungen, indem sie ein Office-Add-In installiert, damit die Benutzer die Dateien einfach direkt schützen können.

- Konfigurieren Sie Ihre Anwendungen und Dienste für Azure RMS

- Erstellen Sie [benutzerdefinierte Vorlagen](https://technet.microsoft.com/library/dn642472.aspx), entsprechend Ihrer Geschäftsanforderungen. Beispiel: Eine Vorlage für streng geheime Daten, die für alle E-Mails im Zusammenhang mit streng geheimen Informationen verwendet werden soll.

Organisationen, die in Hinsicht auf die [Datenklassifizierung (in englischer Sprache)](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) und den Dateischutz Schwächen haben, sind möglicherweise anfälliger für Datenlecks. Ohne richtigen Dateischutz ist es Organisationen nicht möglich, Einblicke in das Unternehmen zu erhalten, eine Überwachung in Hinsicht auf Missbrauch durchzuführen und böswilligen Zugriff auf Dateien zu verhindern.

> [!Note]
> Erfahren Sie mehr über Azure RMS, indem Sie den Artikel [Erste Schritte mit Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx) lesen.

## <a name="secure-your-application-protect"></a>Schützen Ihrer Anwendung (Schutz)
Während Azure dafür verantwortlich ist, die Infrastruktur und Plattform zu sichern, auf der Ihre Anwendung ausgeführt wird, ist es Ihre Aufgabe die Anwendung selbst zu sichern. Das heißt, Sie müssen Ihren Anwendungscode und den Inhalt in einer sicheren Weise entwickeln, bereitstellen und verwalten. Ansonsten kann Ihr Anwendungscode oder -inhalt noch immer anfällig für Sicherheitsrisiken sein.

### <a name="web-application-firewall-waf"></a>Web Application Firewall (WAF)
[Web Application Firewall (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) ist ein Feature von [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction), das zentralisierten Schutz Ihrer Webanwendungen vor allgemeinen Exploits und Sicherheitsrisiken bietet.

Der Web Application Firewall liegen Regeln aus den [OWASP-Kernregelsätzen](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) (3.0 oder 2.2.9) zugrunde. Webanwendungen sind zunehmend Ziele böswilliger Angriffe, die allgemein bekannte Sicherheitslücken ausnutzen. Zu diesen Sicherheitslücken (Exploits) gehören üblicherweise Angriffe durch Einschleusung von SQL-Befehlen oder Angriffe durch websiteübergreifende Skripts, um nur einige zu nennen. Das Verhindern solcher Angriffe im Anwendungscode ist oft schwierig und erfordert strenge Wartung, Patching und Überwachung auf verschiedenen Ebenen der Anwendungstopologie. Eine zentrale Web Application Firewall vereinfacht die Sicherheitsverwaltung erheblich und bietet Anwendungsadministratoren einen besseren Schutz vor Bedrohungen und Angriffen. Mit einer WAF-Lösung können Sie ebenfalls schneller auf ein Sicherheitsrisiko reagieren, da eine bekannte Schwachstelle an einem zentralen Ort gepatcht wird, statt jede einzelne Webanwendung separat zu sichern. Vorhandene Anwendungsgateways lassen sich problemlos in ein Anwendungsgateway mit Web Application Firewall konvertieren.

Die Web Application Firewall schützt unter anderem vor folgenden gängigen Sicherheitslücken im Web:

- Schutz vor Einschleusung von SQL-Befehlen

- Schutz vor websiteübergreifenden Skripts

- Schutz vor allgemeinen Webangriffen wie Befehlseinschleusung, HTTP Request Smuggling, HTTP Response Splitting und Remote File Inclusion

- Schutz vor Verletzungen des HTTP-Protokolls

- Schutz vor HTTP-Protokollanomalien, z.B. fehlende user-agent- und accept-Header des Hosts

- Verhindern von Bots, Crawlern und Scannern

- Erkennung häufiger Fehler bei der Anwendungskonfiguration (d. h. Apache, IIS usw.)

> [!Note]
> Eine ausführlichere Liste mit Regeln und Informationen zum entsprechenden Schutz finden Sie in den folgenden [Kernregelsätzen](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview#core-rule-sets):

Azure bietet Ihnen für Ihre App zudem zahlreiche benutzerfreundliche Funktionen zum Schutz von eingehendem und ausgehendem Datenverkehr. Bei Azure werden Kunden auch dabei unterstützt, ihren Anwendungscode zu schützen, indem extern bereitgestellte Funktionen zur Verfügung gestellt werden, mit denen Ihre Webanwendungen nach Sicherheitsrisiken durchsucht werden.

- [Einrichten der Azure Active Directory-Authentifizierung für Ihre App](https://azure.microsoft.com/blog/azure-websites-authentication-authorization/)


- [Sichern des Datenverkehrs zu Ihrer App durch Aktivierung von Transport Layer Security (TLS/SSL) – HTTPS](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl)

    - [Erzwingen des gesamten eingehenden Datenverkehrs über HTTPS-Verbindungen](http://microsoftazurewebsitescheatsheet.info/)

  - [Aktivieren von Strict Transport Security (HSTS)](http://microsoftazurewebsitescheatsheet.info/#enable-http-strict-transport-security-hsts)


- [Einschränken des Zugriffs auf Ihre App durch die IP-Adresse des Clients](http://microsoftazurewebsitescheatsheet.info/#filtering-traffic-by-ip)

- [Einschränken des App-Zugriffs auf der Grundlage des Clientverhaltens (Anforderungshäufigkeit und -parallelität)](http://microsoftazurewebsitescheatsheet.info/#dynamic-ip-restrictions)

- [Überprüfen Ihres Web-App-Codes auf Schwachstellen mithilfe der Tinfoil Security-Überprüfung](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/)

- [Konfigurieren der gegenseitigen TLS-Authentifizierung mit obligatorischer Clientzertifikatverwendung für die Verbindungsherstellung mit Ihrer Web-App](https://docs.microsoft.com/azure/app-service/app-service-web-configure-tls-mutual-auth)

- [Konfigurieren eines Clientzertifikats, das Ihre App zum Herstellen einer sicheren Verbindung mit externen Ressourcen verwendet](https://azure.microsoft.com/blog/using-certificates-in-azure-websites-applications/)

- [Entfernen von Serverstandardheadern, um zu verhindern, dass Tools Fingerabdrücke Ihrer App erfassen](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/)

- [Herstellen einer sicheren Verbindung zwischen Ihrer App und Ressourcen in einem privaten Netzwerk mithilfe eines Punkt-zu-Standort-VPNs](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)

- [Herstellen einer sicheren Verbindung zwischen Ihrer App und Ressourcen in einem privaten Netzwerk mithilfe von Hybridverbindungen](https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections)

Azure App Service nutzt die gleiche Antischadsoftwarelösung, die auch von Azure Cloud Services und Virtual Machines verwendet wird. Weitere Informationen dazu finden Sie in der [Dokumentation zu Antischadsoftware](https://docs.microsoft.com/azure/security/azure-security-antimalware).

## <a name="secure-your-network-protect"></a>Schützen Ihres Netzwerks (Schutz)
Microsoft Azure verfügt über eine robuste Netzwerkinfrastruktur zur Unterstützung der Konnektivitätsanforderungen Ihrer Anwendungen und Dienste. Netzwerkkonnektivität ist zwischen Ressourcen in Azure, zwischen lokalen und in Azure gehosteten Ressourcen und zu und aus dem Internet und Azure möglich.

Mit der [Azure-Netzwerkinfrastruktur](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) können Sie Azure-Ressourcen über [virtuelle Netzwerke (VNets)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) sicher miteinander verbinden. Ein VNet ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. Bei einem VNet handelt es sich um eine logische Isolation vom dedizierten Azure-Cloudnetzwerk für Ihr Abonnement. Sie können VNets mit Ihren lokalen Netzwerken verbinden.

![Schützen Ihres Netzwerks (Schutz)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig6.png)

Wenn Sie eine einfache Zugriffssteuerung auf Netzwerkebene benötigen (basierend auf IP-Adresse und TCP- oder UDP-Protokollen), können Sie [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) verwenden. Eine Netzwerksicherheitsgruppe (NSG) ist eine einfache Firewall, die zustandsbehaftete Pakete filtert und die Zugriffssteuerung auf Grundlage eines [5-Tupels](https://www.techopedia.com/definition/28190/5-tuple)ermöglicht.

Azure-Netzwerke unterstützen die Fähigkeit, das Routingverhalten für den Netzwerkverkehr in Ihrem virtuellen Azure-Netzwerk benutzerdefiniert anzupassen. Sie erreichen dies über die Konfiguration der [benutzerdefinierten Routen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) in Azure.

Mithilfe der [Tunnelerzwingung](https://www.petri.com/azure-forced-tunneling) können Sie sicherstellen, dass Ihre Dienste nicht über die Erlaubnis verfügen, eine Verbindung mit Geräten im Internet zu initiieren.

Azure unterstützt eine dedizierte WAN-Linkkonnektivität mit Ihrem lokalen Netzwerk und ein Azure Virtual Network mit [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). Für den Link zwischen Azure und Ihrem Standort wird eine dedizierte Verbindung verwendet, die nicht über das öffentliche Internet verläuft. Wenn Ihre Azure-Anwendung in mehreren Datencentern ausgeführt wird, können Sie den [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) verwenden, um Anfragen von Benutzern auf intelligente Weise zwischen Instanzen der Anwendung weiterzuleiten. Sie können Datenverkehr auch an Dienste leiten, die nicht in Azure ausgeführt werden, sofern sie über das Internet zugänglich sind.

## <a name="virtual-machine-security-protect"></a>Sicherheit von virtuellen Computern (Schutz)

Mit [Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/) können Sie sehr flexibel eine Vielzahl unterschiedlicher Computinglösungen bereitstellen. Mit Unterstützung für Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP und Azure BizTalk Services können Sie jede Workload und jede Sprache auf fast jedem Betriebssystem bereitstellen.

Mit Azure können Sie zum Schützen der virtuellen Computer vor Dateien mit schädlichem Inhalt, Adware und anderen Bedrohungen [Antischadsoftware](https://docs.microsoft.com/azure/security/azure-security-antimalware) von Anbietern wie Microsoft, Symantec, Trend Micro und Kaspersky verwenden.

Microsoft Antimalware for Azure Cloud Services and Virtual Machines ist eine Echtzeit-Schutzfunktion zum Bestimmen und Entfernen von Viren, Spyware und anderer Schadsoftware. Microsoft Antimalware gibt konfigurierbare Warnungen aus, wenn bekannte schädliche oder unerwünschte Software versucht, sich selbst auf Ihren Azure-Systemen zu installieren oder dort auszuführen.

[Azure Backup](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) ist eine skalierbare Lösung, die Ihre Anwendungsdaten schützt – ganz ohne Investitionskosten und mit sehr niedrigen Betriebskosten. Anwendungsfehler können Ihre Daten beschädigen, und menschliche Fehler können Bugs in Ihren Anwendungen verursachen. Mit Azure Backup sind Ihre virtuellen Windows- und Linux-Computer geschützt.

[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) unterstützt die Orchestrierung von Replikation, Failover und Wiederherstellung von Workloads und Apps, damit diese Komponenten über einen sekundären Standort zur Verfügung stehen, wenn der primäre Standort ausfällt.

## <a name="ensure-compliance-cloud-services-due-diligence-checklist-protect"></a>Sicherstellen der Konformität: Checkliste zur Kaufprüfung von Clouddiensten (Schutz)

Microsoft hat eine [Cloud Services Due Diligence Checklist](https://aka.ms/cloudchecklist.download) (Checkliste zur Kaufprüfung von Clouddiensten) entwickelt, damit Organisationen eine etwaige Umstellung auf die Cloud genau prüfen können. Die Checkliste enthält eine Struktur für Organisationen jeder Größe und jedes Typs – Privatunternehmen und Organisationen des öffentlichen Sektors, z.B. auch Behörden und Non-Profit-Organisationen –, mit deren Hilfe die Ziele und Anforderungen in Bezug auf Leistung, Service, Datenverwaltung und Governance ermittelt werden können. Dies ermöglicht einen Vergleich der Angebote unterschiedlicher Clouddienstanbieter, die letztendlich die Basis für einen Clouddienstvertrag bilden.

Die Checkliste stellt ein Gerüst dar, das Klausel für Klausel an einem neuen internationalen Standard für Clouddienstvereinbarungen (ISO/IEC 19086) ausgerichtet ist. Dieser Standard umfasst einen vereinheitlichten Satz von Aspekten für Organisationen, damit Entscheidungen zur Einführung der Cloud leichter getroffen werden können und eine gemeinsame Basis zum Vergleichen von Clouddienstangeboten vorhanden ist.

Mit der Checkliste wird für eine gründlich geprüfte Umstellung auf die Cloud gesorgt, und Sie erhalten eine strukturierte Anleitung und einen einheitlichen, wiederholbaren Ansatz für die Auswahl eines Clouddienstanbieters.

Die Umstellung auf die Cloud ist somit nicht mehr nur eine Technologieentscheidung. Da die Anforderungen der Checkliste alle Aspekte einer Organisation betreffen, werden alle wichtigen internen Entscheider einbezogen: CIO und CISO sowie die Bereiche Rechtsabteilung, Risikomanagement, Einkauf und Konformität (Compliance). Dies erhöht die Effizienz des Entscheidungsfindungsprozesses, und Entscheidungen werden basierend auf fundierten Argumenten getroffen. So wird die Wahrscheinlichkeit des Auftretens von unvorhergesehenen Hindernissen bei der Umstellung reduziert.

Außerdem bietet die Checkliste Folgendes:

- Bereitstellung von wichtigen Diskussionsthemen für Entscheider zu Beginn des Prozesses zur Cloudeinführung

- Unterstützung von eingehenden Unternehmensdiskussionen zu den Bestimmungen und den eigenen Zielen der Organisation in Bezug auf Datenschutz, personenbezogene Informationen und Datensicherheit

- Hilfestellung für Organisationen beim Identifizieren von potenziellen Problemen, die sich auf ein Cloudprojekt auswirken können

- Bereitstellung eines einheitlichen Satzes mit Fragen mit den gleichen Bedingungen, Definitionen, Metriken und Ergebnissen für jeden Anbieter, um das Vergleichen der Angebote verschiedener Clouddienstanbieter zu vereinfachen

## <a name="azure-infrastructure-and-application-security-validation-detect"></a>Überprüfung der Azure-Infrastruktur- und Anwendungssicherheit (Erkennung)

[Azure Operational Security](https://docs.microsoft.com/azure/security/azure-operational-security) bezieht sich auf die Dienste, Steuerelemente und Features, die für Benutzer zum Schützen ihrer Daten, Anwendungen und anderen Ressourcen in Microsoft Azure zur Verfügung stehen.

![Sicherheitsüberprüfung (Erkennung)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig7.png)

Azure Operational Security basiert auf einem Framework, mit dem die über verschiedene einzigartige Microsoft-Funktionen erworbenen Kenntnisse einbezogen werden, z.B. Microsoft Security Development Lifecycle (SDL), Microsoft Security Response Center-Programm und umfassende Informationen zur Bedrohungslage hinsichtlich der Sicherheit im Internet.

### <a name="microsoft-operations-management-suiteoms"></a>Microsoft Operations Management Suite (OMS)

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) ist die IT-Verwaltungslösung für die Hybrid Cloud. Bei der eigenständigen Verwendung oder zur Erweiterung Ihrer vorhandenen Bereitstellung von System Center bietet Ihnen die OMS die maximale Flexibilität und Kontrolle für die cloudbasierte Verwaltung Ihrer Infrastruktur.

![Microsoft Operations Management Suite (OMS)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig8.png)

Mit der OMS können Sie jede Instanz in einer beliebigen Cloud, einschließlich lokaler Cloud, Azure, AWS, Windows Server, Linux, VMware und Openstack, zu geringeren Kosten als die Lösungen der Wettbewerber verwalten. Die primär für die Cloud konzipierte OMS bietet einen neuen Ansatz für die Verwaltung Ihres Unternehmens, bei dem es sich um die schnellste und kostengünstigste Möglichkeit handelt, um sich neuen geschäftlichen Herausforderungen zu stellen sowie neuen Workloads, Anwendungen und Cloudumgebungen nachzukommen.

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) sammelt Daten von verwalteten Ressourcen in einem zentralen Repository und stellt dadurch Überwachungsdienste für OMS bereit. Bei diesen Daten kann es sich um Ereignisse, Leistungsdaten oder benutzerdefinierte Daten handeln, die über die API bereitgestellt wurden. Die gesammelten Daten können für Warnungen und Analysen genutzt und exportiert werden.

![Log Analytics](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig9.png)

Mithilfe dieser Methode können Sie Daten aus einer Vielzahl von Quellen zusammenfassen, um Daten aus Ihren Azure-Diensten mit Ihrer vorhandenen lokalen Umgebung zu kombinieren. Die Datensammlung wird außerdem klar von der Aktion getrennt, die für diese Daten ausgeführt wird, sodass alle Aktionen für alle Arten von Daten verfügbar sind.

### <a name="azure-security-center"></a>Azure Security Center

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) unterstützt Sie beim Verhindern, Erkennen und Beheben von Bedrohungen durch größere Transparenz und bessere Kontrolle über die Sicherheit Ihrer Azure-Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden.

Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen, um potenzielle Sicherheitsrisiken zu erkennen. Anhand einer Liste mit Empfehlungen werden Sie durch den Prozess des Konfigurierens der erforderlichen Sicherheitsmechanismen geführt.

Beispiele:

- Bereitstellung von Antischadsoftware, um Schadsoftware zu erkennen und zu entfernen

- Konfigurieren von Netzwerksicherheitsgruppen und Regeln, um Datenverkehr zu virtuellen Computern zu steuern

- Bereitstellung von Web Application Firewalls als Schutz vor Angriffen, die auf Ihre Webanwendungen abzielen

- Bereitstellen von fehlenden Systemupdates

- Behandeln von Betriebssystemkonfigurationen, die nicht den empfohlenen Basisregeln entsprechen

Security Center erfasst, analysiert und vereinigt automatisch Protokolldaten von Ihren Azure-Ressourcen, vom Netzwerk und von Partnerlösungen wie Antischadsoftware und Firewalls. Wurden Bedrohungen erkannt, wird eine Sicherheitswarnung erstellt. Beispiele für eine Erkennung sind:

- Gefährdete virtuelle Computer, die mit bekannten bösartigen IP-Adressen kommunizieren

- Erweiterte Schadsoftware, die mithilfe der Windows-Fehlerberichterstattung erkannt wurde

- Brute-Force-Angriffe gegen virtuelle Computer

- Sicherheitswarnungen von integrierter Antischadsoftware und integrierten Firewalls

### <a name="azure-monitor"></a>Azure Monitor

[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) enthält Verweise auf Informationen zu bestimmten Ressourcentypen. Es bietet Visualisierung, Abfrage, Weiterleitung, Warnung, automatische Skalierung und Automatisierung für Daten sowohl aus der Azure-Infrastruktur (Aktivitätsprotokoll) als auch aus jeder einzelnen Azure-Ressource (Diagnoseprotokolle).

Cloudanwendungen sind komplexe Systeme mit zahlreichen Variablen. Die Überwachung stellt Daten bereit, auf deren Grundlage die ordnungsgemäße Ausführung der Anwendung sichergestellt werden kann. Sie trägt auch zur Vermeidung potenzieller Probleme bei und hilft bei der Behandlung bereits aufgetretener Probleme.

![Azure Monitor](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig10.png) Darüber hinaus können Sie auf Grundlage von Überwachungsdaten umfassende Erkenntnisse über Ihre Anwendung gewinnen. Mithilfe dieser Kenntnisse können Sie die Leistung oder Wartungsfreundlichkeit der Anwendung verbessern oder Aktionen automatisieren, die andernfalls manuell ausgeführt werden müssten.

Die Überwachung der Netzwerksicherheit ist ein entscheidender Faktor zur Erkennung von Sicherheitsrisiken im Netzwerk und zur Compliance mit Ihrem Modell für die IT-Sicherheit und bestimmende Governance. Mithilfe der Ansicht für Sicherheitsgruppen können Sie die konfigurierte Netzwerksicherheitsgruppe und Sicherheitsregeln sowie die jeweils geltenden Sicherheitsregeln abrufen. Mit den angewendeten Regeln der Liste können Sie die offenen Ports ermitteln und das Netzwerksicherheitsrisiko bewerten.

### <a name="network-watcher"></a>Network Watcher

[Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) ist ein regionaler Dienst, mit dem Sie Bedingungen auf Netzwerkebene in Azure überwachen und diagnostizieren können. Die Tools zur Netzwerkdiagnose und -visualisierung von Network Watcher helfen Ihnen dabei, Ihr Netzwerk in Azure zu verstehen, Diagnosen durchzuführen und Einblicke zu gewinnen. Dieser Dienst umfasst die Paketerfassung, „Nächster Hop“, die IP-Datenflussüberprüfung, die Sicherheitsgruppenansicht und NSG-Datenflussprotokolle. Die Überwachung auf Szenarioebene bietet im Gegensatz zur Überwachung einzelner Netzwerkressourcen eine End-to-End-Ansicht der Netzwerkressourcen.

### <a name="storage-analytics"></a>Speicheranalyse

Bei der [Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) können Metriken gespeichert werden, zu denen aggregierte Transaktionsstatistiken und Kapazitätsdaten für die an einen Speicherdienst gesendeten Anforderungen zählen. Berichte zu Transaktionen werden sowohl auf API-Vorgangsebene als auch auf Speicherdienstebene erstellt, während Berichte zur Kapazität nur auf Speicherdienstebene erstellt werden. Mit den Metrikdaten können die Speicherdienstnutzung analysiert, Probleme mit Anforderungen für den Speicherdienst diagnostiziert und die Leistung von Anwendungen verbessert werden, die einen Dienst verwenden.

### <a name="application-insights"></a>Application Insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) ist ein erweiterbarer, für Webentwickler konzipierter Dienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen. Verwenden Sie ihn, um Ihre aktiven Webanwendung zu überwachen. Der Dienst erkennt automatisch Leistungsanomalien. Er verfügt über leistungsstarke Analysetools, mit denen Sie Probleme diagnostizieren und nachvollziehen können, wie Ihre App von den Benutzern verwendet wird. Der Dienst unterstützt Sie bei der kontinuierlichen Verbesserung der Leistung und Benutzerfreundlichkeit Ihrer App. Er lässt sich für Apps auf einer Vielzahl von Plattformen einsetzen, wie z.B. .NET, Node.js oder J2EE – lokal gehostet oder in der Cloud. Der Dienst lässt sich in Ihren DevOps-Prozess integrieren und verfügt über Verbindungspunkte mit verschiedenen Entwicklungstools.

Der Dienst überwacht:

- **Anforderungsraten, Antwortzeiten und Fehlerraten**: Finden Sie heraus, welche Seiten zu welchen Tageszeiten am häufigsten verwendet werden und wo Ihre Benutzer sind. Stellen Sie fest, welche Seiten die beste Leistung aufweisen. Wenn die Antwortzeiten und Fehlerraten bei mehr Anforderungen ansteigen, haben Sie möglicherweise ein Problem mit den Ressourcen.

- **Abhängigkeitsraten, Antwortzeiten und Fehlerraten**: Finden Sie heraus, ob Sie von externen Diensten verlangsamt werden.

- **Ausnahmen**: Analysieren Sie die aggregierten Statistiken, oder wählen Sie bestimmte Instanzen aus, und untersuchen Sie die Stapelüberwachung und die zugehörigen Anforderungen. Sowohl die Server- als auch die Browserausnahmen werden gemeldet.

- **Seitenansichten und Ladeleistung**: Von den Browsern der Benutzer gemeldet.

- **AJAX-Aufrufe von Webseiten**: Raten, Antwortzeiten und Fehlerraten.

- **Anzahl von Benutzern und Sitzungen**.

- **Leistungsindikatoren** von Ihren Windows- oder Linux-Servercomputern, z.B. CPU, Arbeitsspeicher und Netzwerkverwendung.

- **Hostdiagnose** von Docker oder Azure.

- **Diagnose-Ablaufverfolgungsprotokolle** aus Ihrer App, sodass Sie Ablaufverfolgungsereignisse mit Anforderungen korrelieren können.

- **Benutzerdefinierte Ereignisse und Metriken**, die Sie selbst im Client- oder Servercode schreiben, um Geschäftsereignisse zu verfolgen, z.B. verkaufte Artikel oder gewonnene Spiele.
Die Infrastruktur für Ihre Anwendung besteht normalerweise aus vielen Komponenten: womöglich ein virtueller Computer, ein Speicherkonto und ein virtuelles Netzwerk oder eine Web-App, eine Datenbank, ein Datenbankserver und Drittanbieterdienste. Sie sehen diese Komponenten nicht als separate Entitäten, sondern als verwandte und voneinander abhängige Teile einer einzelnen Entität. Diese möchten Sie als Gruppe bereitstellen, verwalten und überwachen. Mit dem [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) können Sie als Gruppe mit den Ressourcen in Ihrer Lösung arbeiten.

Sie können alle Ressourcen für Ihre Lösung in einem einzigen koordinierten Vorgang bereitstellen, aktualisieren oder löschen. Sie verwenden eine Vorlage für die Bereitstellung, die für unterschiedliche Umgebungen geeignet sein kann, z.B. Testing, Staging und Produktion. Der Ressourcen-Manager bietet Sicherheits-, Überwachungs- und Kennzeichnungsfunktionen, mit denen Sie Ihre Ressourcen nach der Bereitstellung verwalten können.

**Vorteile der Verwendung des Resource Managers**

Der Ressourcen-Manager bietet mehrere Vorteile:

- Sie können alle Ressourcen für Ihre Lösung als Gruppe bereitstellen, verwalten und überwachen, anstatt diese Ressourcen einzeln zu verarbeiten.

- Sie können die Lösung während des gesamten Entwicklungslebenszyklus wiederholt bereitstellen und sicher sein, dass Ihre Ressourcen einheitlich bereitgestellt werden.

- Sie können zum Verwalten Ihrer Infrastruktur anstelle von Skripts auch deklarative Vorlagen verwenden.

- Sie können die Abhängigkeiten zwischen Ressourcen definieren, sodass diese in der richtigen Reihenfolge bereitgestellt werden.

- Sie können die Zugriffssteuerung auf alle Dienste in der Ressourcengruppe anwenden, da die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) standardmäßig in die Verwaltungsplattform integriert ist.

- Sie können Tags auf Ressourcen anwenden, um alle Ressourcen in Ihrem Abonnement logisch zu organisieren.

- Indem Sie die Kosten für eine Gruppe mit Ressourcen anzeigen, für die das gleiche Tag verwendet wird, erhalten Sie die Abrechnungsinformationen für Ihre Organisation.

> [!Note]
> Der Ressourcen-Manager stellt eine neue Möglichkeit zur Bereitstellung und Verwaltung Ihrer Lösungen bereit. Weitere Informationen zu den Änderungen an dem von Ihnen verwendeten früheren Bereitstellungsmodell finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich weiter über die Sicherheit, indem Sie einige unserer weiter gehenden Sicherheitsthemen lesen:

- [Überwachung und Protokollierung](https://www.microsoft.com/en-us/trustcenter/security/auditingandlogging)

- [Cyberkriminalität](https://www.microsoft.com/en-us/trustcenter/security/cybercrime)

- [Gestaltung und betriebsbedingte Sicherheit](https://www.microsoft.com/en-us/trustcenter/security/designopsecurity)

- [Verschlüsselung](https://www.microsoft.com/en-us/trustcenter/security/encryption)

- [Identitäts- und Zugriffsverwaltung](https://www.microsoft.com/en-us/trustcenter/security/identity)

- [Netzwerksicherheit](https://www.microsoft.com/en-us/trustcenter/security/networksecurity)

- [Verwaltung von Bedrohungen](https://www.microsoft.com/en-us/trustcenter/security/threatmanagement)

