---
title: "Bewährte Methoden für die Azure-Identitäts- und Zugriffssicherheit | Microsoft Docs"
description: "In diesem Artikel werden einige bewährte Methoden zur Identitätsverwaltung und Zugriffssteuerung mit integrierten Azure-Funktionen beschrieben."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 3cba38d95535ff5ed3cd62aac5c0aa04a310f48c
ms.openlocfilehash: 50f9073d3c35bd9dcfd826ff44e767fb69558757


---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Azure-Identitätsverwaltung und Sicherheit der Zugriffssteuerung – Bewährte Methoden
Die Identität wird häufig als neue Grenzschicht für die Sicherheit angesehen, indem diese Rolle vom herkömmlichen netzwerkzentrierten Ansatz übernommen wird. Die Weiterentwicklung dieses Dreh- und Angelpunkts für die Bereiche Sicherheit und Investitionen basiert darauf, dass Umkreisnetzwerke immer durchlässiger geworden sind und die Verteidigung des Umkreisnetzwerks nicht mehr so effektiv wie vor der immensen Nutzung von [BYOD](http://aka.ms/byodcg)-Geräten und Cloudanwendungen sein kann.

In diesem Artikel werden bewährte Methoden für die Azure-Identitätsverwaltung und die Sicherheit der Zugriffssteuerung beschrieben. Diese empfohlenen Vorgehensweisen sind aus unseren Erfahrungen mit [Azure AD](../active-directory/active-directory-whatis.md) und den Erfahrungen von Kunden wie Ihnen abgeleitet.

Für jede empfohlene Vorgehensweise erklären wir:

* Wobei es bei der bewährten Methode geht
* Warum Sie die bewährte Methode nutzen sollten
* Was die Folge sein könnte, wenn Sie die bewährte Methode nicht aktivieren
* Mögliche Alternativen zur bewährten Methode
* Wie Sie erfahren können, wie Sie die empfohlenen Vorgehensweisen aktivieren

Dieser Artikel zu den bewährten Methoden für die Azure-Identitätsverwaltung und Zugriffssteuerung basiert auf einer Konsensmeinung und den Fähigkeiten und Funktionssätzen der Azure-Plattform, wie sie zum Erstellungszeitpunkt dieses Artikels existierten. Meinungen und Technologien ändern sich im Laufe der Zeit. Dieser Artikel wird daher regelmäßig aktualisiert, um diese Änderungen widerzuspiegeln.

Der Artikel enthält bewährte Methoden zur Azure-Identitätsverwaltung und Sicherheit der Zugriffssteuerung für die folgenden Bereiche:

* Zentralisieren der Identitätsverwaltung
* Ermöglichen des einmaligen Anmeldens (SSO)
* Bereitstellen der Kennwortverwaltung
* Erzwingen der Multi-Factor Authentication (MFA) für Benutzer
* Verwenden der rollenbasierten Zugriffssteuerung (Role-Based Access Control; RBAC)
* Steuern von Standorten, an denen Ressourcen erstellt werden, mit Resource Manager
* Anleiten von Entwicklern zur Nutzung von Identitätsfunktionen für SaaS-Apps
* Aktives Überwachen auf verdächtige Aktivitäten

## <a name="centralize-your-identity-management"></a>Zentralisieren der Identitätsverwaltung
Ein wichtiger Schritt zum Schützen Ihrer Identität ist die Sicherstellung, dass die IT-Mitarbeiter Konten unabhängig vom Erstellungsort von einem zentralen Standort aus verwalten können. In den meisten IT-Abteilungen von Unternehmen liegt das primäre Kontenverzeichnis lokal vor. Aber die Zahl der Hybrid-Cloudbereitstellungen nimmt zu, und es ist wichtig, dass Sie verstehen, wie Sie lokale Verzeichnisse und Cloudverzeichnisse integrieren und Endbenutzern eine nahtlose Oberfläche bieten können.

Wir empfehlen Ihnen zwei Optionen, um dieses Szenario mit [Hybrididentitäten](../active-directory/active-directory-hybrid-identity-design-considerations-overview.md) zu erreichen:

* Synchronisieren des lokalen Verzeichnisses mit Ihrem Cloudverzeichnis per Azure AD Connect
* Erstellen eines Verbunds Ihrer lokalen Identität mit dem Cloudverzeichnis über [Active Directory-Verbunddienste](https://msdn.microsoft.com/library/bb897402.aspx) (AD FS)

Für Organisationen, die ihre lokale Identität nicht in die Cloudidentität integrieren, fällt beim Verwalten der Konten Mehraufwand an. Dies erhöht die Wahrscheinlichkeit von Fehlern und Sicherheitsverletzungen.

Weitere Informationen zur Azure AD-Synchronisierung finden Sie im Artikel [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](../active-directory/active-directory-aadconnect.md).

## <a name="enable-single-sign-on-sso"></a>Ermöglichen des einmaligen Anmeldens (SSO)
Wenn Sie mehrere Verzeichnisse verwalten müssen, wird dies nicht nur für die IT-Abteilung zu einem Verwaltungsproblem, sondern auch für die Endbenutzer, die sich mehrere Kennwörter merken müssen. Bei Verwendung von [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) geben Sie Ihren Benutzern die Möglichkeit, die gleiche Gruppe von Anmeldeinformationen zum Anmelden und Zugreifen auf die benötigten Ressourcen zu nutzen. Dabei spielt es keine Rolle, ob sich eine Ressource lokal oder in der Cloud befindet.

Verwenden Sie SSO, um Benutzern zu ermöglichen, basierend auf ihrem Organisationskonto in Azure AD auf ihre [SaaS-Anwendungen](../active-directory/active-directory-appssoaccess-whatis.md) zuzugreifen. Dies gilt nicht nur für Microsoft SaaS-Apps, sondern auch für andere Apps, z.B. [Google-Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) und [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). Ihre Anwendung kann so konfiguriert werden, dass Azure AD als [SAML-Identitätsanbieter](../active-directory/fundamentals-identity.md) verwendet wird. Als Sicherheitsmaßnahme stellt Azure AD nur dann ein Token aus, das den Benutzern die Anmeldung bei der Anwendung erlaubt, wenn ihnen zuvor mit Azure AD Zugriff gewährt wurde. Sie können den Zugriff direkt oder über eine Gruppe gewähren, deren Mitglied die Benutzer sind.

> [!NOTE]
> Die Entscheidung zur Nutzung von SSO wirkt sich auch darauf aus, wie Sie das lokale Verzeichnis in Ihr Cloudverzeichnis integrieren. Wenn Sie SSO verwenden möchten, müssen Sie einen Verbund nutzen, da die Verzeichnissynchronisierung nur eine [Oberfläche für die gleiche Anmeldung (Same Sign-On)](../active-directory/active-directory-aadconnect.md) bietet.
>
>

Für Organisationen, die SSO für ihre Benutzer und Anwendungen nicht erzwingen, kommt es häufiger zu Szenarien, bei denen Benutzer mehrere Kennwörter haben. Dies erhöht direkt die Wahrscheinlichkeit dafür, dass Benutzer Kennwörter wiederverwenden oder unsichere Kennwörter nutzen.

Weitere Informationen zu Azure AD SSO finden Sie im Artikel [Verwaltung der Active Directory-Verbunddienste und Anpassung mit Azure AD Connect](../active-directory/active-directory-aadconnect-federation-management.md).

## <a name="deploy-password-management"></a>Bereitstellen der Kennwortverwaltung
Bei Szenarien, in denen Sie mehrere Mandanten verwenden oder Benutzern das [Zurücksetzen ihres Kennworts](../active-directory/active-directory-passwords-update-your-own-password.md) ermöglichen möchten, ist es wichtig, dass Sie zur Verhinderung von Missbrauch geeignete Sicherheitsrichtlinien verwenden. In Azure können Sie die Funktion für die Self-Service-Kennwortzurücksetzung nutzen und die Sicherheitsoptionen anpassen, um Ihre geschäftlichen Anforderungen zu erfüllen.

Es ist besonders wichtig, Feedback von diesen Benutzern zu erhalten und aus den Erfahrungen zu lernen, die diese beim Ausführen der Schritte machen. Stellen Sie basierend auf diesen Erfahrungen einen Plan zur Lösung von potenziellen Problemen auf, die während der Bereitstellung für eine größere Gruppe auftreten können. Wir empfehlen Ihnen auch, den [Bericht zur Registrierung für die Kennwortzurücksetzung](../active-directory/active-directory-passwords-get-insights.md) zu verwenden, um die registrierten Benutzer zu überwachen.

Für Organisationen, die Supportanrufe zum Thema Kennwortänderung vermeiden möchten und Benutzern gleichzeitig das Zurücksetzen der eigenen Kennwörter erlauben, fallen aufgrund von Kennwortproblemen häufiger vermehrte Anrufe beim Service Desk an. In Organisationen mit mehreren Mandanten ist es unerlässlich, dass Sie diese Art von Funktion implementieren und Benutzern die Kennwortzurücksetzung innerhalb der Sicherheitsgrenzen ermöglichen, die in der Sicherheitsrichtlinie festgelegt wurden.

Weitere Informationen zur Kennwortzurücksetzung finden Sie im Artikel [Bereitstellen der Kennwortverwaltung und Schulen der Benutzer zu deren Verwendung](../active-directory/active-directory-passwords-best-practices.md).

## <a name="enforce-multi-factor-authentication-mfa-for-users"></a>Erzwingen der Multi-Factor Authentication (MFA) für Benutzer
Für Organisationen, die Branchenstandards erfüllen müssen, z.B. [PCI DSS Version 3.2](http://blog.pcisecuritystandards.org/preparing-for-pci-dss-32), ist die Multi-Factor Authentication zwingend erforderlich, um Benutzer authentifizieren zu können. Über die Erfüllung von Branchenstandards hinaus kann die Erzwingung von MFA für die Authentifizierung von Benutzern für Organisationen auch eine Hilfe darstellen, wenn es um eine Lösung für Angriffe geht, bei denen Anmeldeinformationen gestohlen werden, z.B. [Pass-the-Hash (PtH)](http://aka.ms/PtHPaper).

Durch die Aktivierung von Azure MFA für Ihre Benutzer fügen Sie eine zweite Sicherheitsebene für Benutzeranmeldungen und Transaktionen hinzu. In diesem Fall greift eine Transaktion möglicherweise auf ein Dokument zu, das sich auf einem Dateiserver oder in Ihrem SharePoint Online-Konto befindet. Azure MFA hilft der IT, die Wahrscheinlichkeit zu verringern, dass kompromittierte Anmeldeinformationen Zugriff auf die Daten einer Organisation erhalten.

Beispiel: Sie erzwingen Azure MFA für Ihre Benutzer und konfigurieren den Vorgang so, dass zur Verifizierung ein Telefonanruf oder eine SMS verwendet wird. Wenn die Anmeldeinformationen eines Benutzers kompromittiert werden, kann der Angreifer dann nicht auf Ressourcen zugreifen, da er keinen Zugriff auf das Telefon des Benutzers hat. Organisationen, die keine zusätzliche Schicht zur Identitätssicherung hinzufügen, sind anfälliger für Angriffe mit dem Ziel des Diebstahls von Anmeldeinformationen, was wiederum zu kompromittierten Daten führen kann.

Eine Alternative für Organisationen, die die gesamte Authentifizierung weiterhin lokal steuern möchten, ist die Verwendung des [Azure Multi-Factor Authentication-Servers](../multi-factor-authentication/multi-factor-authentication-get-started-server.md). Dies wird auch als „MFA lokal“ bezeichnet. Mithilfe dieser Methode können Sie weiterhin die Multi-Factor Authentication erzwingen und dabei den MFA-Server lokal halten.

Weitere Informationen über Azure Multi-Factor Authentication finden Sie unter [Erste Schritte mit Azure Multi-Factor Authentication in der Cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Verwenden der rollenbasierten Zugriffssteuerung (Role-Based Access Control; RBAC)
Das Einschränken des Zugriffs auf der Grundlage der Sicherheitsprinzipien [Need to know](https://en.wikipedia.org/wiki/Need_to_know) und [Least Privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) ist für Organisationen unerlässlich, die Sicherheitsrichtlinien für den Datenzugriff erzwingen möchten. Die rollenbasierte Zugriffssteuerung in Azure (RBAC) kann verwendet werden, um Benutzern, Gruppen und Anwendungen Berechtigungen für einen bestimmten Bereich zu erteilen. Der Bereich einer Rollenzuweisung kann ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein.

Sie können [integrierte RBAC-Rollen](../active-directory/role-based-access-built-in-roles.md) in Azure verwenden, um Benutzern Berechtigungen zuzuweisen. Ziehen Sie die Verwendung von *Speicherkontomitwirkender* für Cloudoperatoren in Betracht, die Speicherkonten verwalten müssen, und nutzen Sie die Rolle *Klassischer Speicherkontomitwirkender*, um klassische Speicherkonten zu verwalten. Überlegen Sie sich, ob Sie Cloudoperatoren, die VMs und Speicherkonten verwalten müssen, zur Rolle *Mitwirkender für virtuelle Computer* hinzufügen.

Organisationen, die keine Datenzugriffssteuerung mithilfe von Funktionen wie RBAC erzwingen, erteilen ihren Benutzern unter Umständen mehr Berechtigungen als erforderlich. Dies kann zur Kompromittierung von Daten führen, indem Benutzern erlaubt wird, auf bestimmte Arten von Daten zuzugreifen (z.B. Daten mit hoher geschäftlicher Bedeutung), auf die sie eigentlich keinen Zugriff haben dürften.

Lesen Sie den Artikel [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen](../active-directory/role-based-access-control-configure.md), um mehr über die Azure RBAC zu erfahren.

## <a name="control-locations-where-resources-are-created-using-resource-manager"></a>Steuern von Standorten, an denen Ressourcen erstellt werden, mit Resource Manager
Es ist sehr wichtig, für Cloudbediener die Durchführung von Aufgaben zu ermöglichen und gleichzeitig die Nichteinhaltung von Konventionen zu verhindern, die zum Verwalten der Ressourcen einer Organisation erforderlich sind. Organisationen, die die Orte steuern möchten, an denen Ressourcen erstellt werden, sollten diese Orte hartcodieren.

Hierzu können Organisationen Sicherheitsrichtlinien erstellen, die über Definitionen verfügen, mit denen die jeweils verweigerten Aktionen oder Ressourcen beschrieben werden. Sie weisen diese Richtliniendefinitionen dem gewünschten Ziel zu, z. B. einem Abonnement, einer Ressourcengruppe oder einer einzelnen Ressource.

> [!NOTE]
> Dies ist nicht das Gleiche wie RBAC. RBAC wird genutzt, um die Benutzer zu authentifizieren, die über die Berechtigung zum Erstellen dieser Ressourcen verfügen.
>
>

Nutzen Sie [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md), um benutzerdefinierte Richtlinien auch für Szenarien zu erstellen, in denen die Organisation Vorgänge nur zulassen möchte, wenn die entsprechende Kostenstelle zugeordnet ist. Andernfalls wird die Anforderung verweigert.

Organisationen, die die Erstellung von Ressourcen nicht steuern, sind anfälliger dafür, dass Benutzer den Dienst durch die Erstellung von mehr Ressourcen als benötigt missbrauchen. Das Absichern der Ressourcenerstellung ist ein wichtiger Schritt zum Schützen eines Szenarios mit mehreren Mandanten.

Weitere Informationen zur Erstellung von Richtlinien mit Azure Resource Manager finden Sie im Artikel [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](../azure-resource-manager/resource-manager-policy.md).

## <a name="guide-developers-to-leverage-identity-capabilities-for-saas-apps"></a>Anleiten von Entwicklern zur Nutzung von Identitätsfunktionen für SaaS-Apps
Die Benutzeridentität wird in vielen Szenarien genutzt, wenn Benutzer auf [SaaS-Apps](https://azure.microsoft.com/marketplace/active-directory/all/) zugreifen, die in ein lokales Verzeichnis oder ein Cloudverzeichnis integriert werden können. Zuallererst empfehlen wir, dass Entwickler zum Entwickeln dieser Apps eine sichere Methodik verwenden, z.B. [Microsoft Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). Azure AD vereinfacht die Authentifizierung für Entwickler mittels Identity as a Service durch Unterstützung branchenüblicher Protokolle wie [OAuth 2.0](http://oauth.net/2/) und [OpenID Connect](http://openid.net/connect/) sowie durch offene Quellbibliotheken für verschiedene Plattformen.

Stellen Sie sicher, dass alle Anwendungen, für die die Authentifizierung an Azure AD outgesourct wird, registriert sind. Dies ist ein erforderlicher Vorgang. Der Grund ist, dass Azure AD die Kommunikation mit der Anwendung koordinieren muss, wenn die Anmeldung (SSO) verarbeitet wird oder Token ausgetauscht werden. Die Sitzung des Benutzers läuft ab, wenn die Gültigkeitsdauer des von Azure AD ausgestellten Tokens abläuft. Werten Sie immer aus, ob dieser Zeitraum für Ihre Anwendung verwendet werden soll oder ob Sie den Zeitraum reduzieren können. Das Reduzieren der Gültigkeitsdauer kann als Sicherheitsmaßnahme fungieren, bei der Benutzer nach einer bestimmten Zeit der Inaktivität zum Abmelden gezwungen werden.

Organisationen, die keine Identitätssteuerung für den Zugriff auf Apps erzwingen und für ihre Entwickler nicht vorgeben, wie Apps sicher in das System für die Identitätsverwaltung integriert werden können, sind häufig anfälliger für den Diebstahl von Anmeldeinformationen, z.B. bei einer unsicheren Authentifizierung und Sitzungsverwaltung. Dies ist unter [Open Web Application Security Project (OWASP) Top 10](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet) beschrieben.

Weitere Informationen zu Authentifizierungsszenarien für SaaS-Apps finden Sie im Artikel [Authentifizierungsszenarien für Azure AD](../active-directory/active-directory-authentication-scenarios.md).

## <a name="actively-monitor-for-suspicious-activities"></a>Aktives Überwachen auf verdächtige Aktivitäten
Laut [Verizon 2016 Data Breach-Bericht](http://www.verizonenterprise.com/verizon-insights-lab/dbir/2016/) sind für die Kompromittierung von Anmeldeinformationen weiterhin steigende Fallzahlen zu verzeichnen, und diese Art von Angriff ist für Cyberkriminelle zu einem der einträglichsten Geschäfte geworden. Aus diesem Grund ist es wichtig, dass Sie ein System zur aktiven Überwachung der Identität verwenden, mit dem verdächtige Aktivitäten schnell erkannt werden und eine Warnung ausgelöst wird, damit dies näher untersucht werden kann. Azure AD bietet zwei wichtige Funktionen, die für Organisationen beim Überwachen ihrer Identitäten hilfreich sind: Azure AD Premium-[Anomalieberichte](../active-directory/active-directory-view-access-usage-reports.md) und Azure AD-[Identitätsschutz](../active-directory/active-directory-identityprotection.md).

Verwenden Sie auf jeden Fall Anomalieberichte, um Versuche der folgenden Art identifizieren zu können: [Anmeldung ohne Nachverfolgung](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md), [Brute-Force](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md)-Angriffe gegen ein bestimmtes Konto, Anmeldung von mehreren Standorten, Anmeldung mit [infizierten Geräten](../active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices.md) und Verwendung verdächtiger IP-Adressen. Bedenken Sie, dass es sich hierbei um Berichte handelt. Anders ausgedrückt: Sie müssen über Prozesse und Verfahren verfügen, mit denen IT-Administratoren diese Berichte täglich oder bei Bedarf ausführen können (normalerweise in einem Szenario mit Reaktionen auf Zwischenfälle).

Im Gegensatz dazu ist Azure AD Identity Protection ein aktives Überwachungssystem, bei dem die aktuellen Risiken im eigenen Dashboard hervorgehoben werden. Außerdem erhalten Sie auch täglich per E-Mail Benachrichtigungen mit einer Zusammenfassung. Wir empfehlen Ihnen, die Risikoebene gemäß Ihren geschäftlichen Anforderungen anzupassen. Die Risikostufe für ein Risikoereignis ist ein Hinweis (Hoch, Mittel oder Niedrig) auf den Schweregrad des Risikoereignisses. Die Risikostufe dient Identity Protection-Benutzern als Hilfe beim Priorisieren der Aktionen, die sie durchführen müssen, um das Risiko für ihre Organisation zu mindern.

Für Organisationen, die ihre Identitätssysteme nicht aktiv überwachen, besteht das Risiko, dass Anmeldeinformationen kompromittiert werden. Ohne das Wissen, dass diese Anmeldeinformationen für verdächtige Aktivitäten genutzt werden, können Organisationen dieser Art von Bedrohung nicht begegnen.
Weitere Informationen zu Azure Identity Protection finden Sie im Artikel [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md).



<!--HONumber=Jan17_HO5-->


