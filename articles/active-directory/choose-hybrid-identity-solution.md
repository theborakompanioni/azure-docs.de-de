---
title: "Auswählen einer Azure-Hybrididentitätslösung | Microsoft-Dokumentation"
description: "Erhalten Sie grundlegende Informationen zu den verfügbaren Hybrididentitätslösungen und Empfehlungen, nach denen Sie für Ihre Organisation eine fundierte Entscheidung zur Identitätsgovernance treffen können."
keywords: 
author: jeffgilb
manager: femila
ms.reviewer: jsnow
ms.author: jeffgilb
ms.date: 7/5/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: 5838e3276765f4f074bca2e3cae81b17edfa7c69
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017

---

# <a name="microsoft-hybrid-identity-solutions"></a>Hybrididentitätslösungen
Hybrid-Identitätslösungen mit [Microsoft Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) ermöglichen es Ihnen, lokale Verzeichnisobjekte mit Azure AD zu synchronisieren, während Sie Ihre Benutzer weiterhin lokal verwalten. Die erste Entscheidung bei der Planung der Synchronisierung Ihres lokalen Windows Server Active Directory mit Azure AD ist, ob Sie synchronisierte Identitäten oder einen Identitätsverbund verwenden möchten. Mit synchronisierten Identitäten (und optional Kennworthashes) können Ihre Benutzer dasselbe Kennwort zum Zugreifen auf lokale und cloudbasierte Organisationsressourcen verwenden. Für die Anforderungen erweiterter Szenarien wie einmaliges Anmelden (Single-Sign-On, SSO) oder lokales MFA müssen Sie Active Directory-Verbunddienste (AD FS) für den Identitätsverbund bereitstellen. 

Es gibt mehrere Optionen für das Konfigurieren von Hybrididentitäten. Dieser Artikel bietet Informationen, mit deren Hilfe Sie die beste Option für Ihre Organisation anhand der Einfachheit der Bereitstellung und Ihrer spezifischen Anforderungen an Identitäts- und Zugriffsverwaltung auswählen können. Wenn Sie abwägen, welches Identitätsmodell den Anforderungen Ihrer Organisation am besten entspricht, müssen Sie auch Zeit, vorhandene Infrastruktur, Komplexität und Kosten berücksichtigen. Diese Faktoren unterscheiden sich für jede Organisation und möglicherweise auch über die Zeit. Wenn sich Ihre Anforderungen ändern, können Sie auch flexibel zu einem anderen Identitätsmodell wechseln.

> [!TIP]
> Alle diese Lösungen werden von [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) bereitgestellt.

## <a name="synchronized-identity"></a>Synchronisierte Identität 
Eine synchronisierte Identität ist die einfachste Möglichkeit, lokale Verzeichnisobjekte (Benutzer und Gruppen) in Azure AD zu synchronisieren. 

![Synchronisierte Hybrididentität](./media/choose-hybrid-identity-solution/synchronized-identity.png)

Eine synchronisierte Identität ist zwar die schnellste und einfachste Methode, Ihre Benutzer müssen jedoch weiterhin ein separates Kennwort für cloudbasierte Ressourcen verwalten. Um dies zu vermeiden, können Sie auch (optional) [einen Hash von Benutzerkennwörtern](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization#what-is-password-synchronization) mit Ihrem Azure AD-Verzeichnis synchronisieren. Durch das Synchronisieren von Kennworthashes können Benutzer sich bei cloudbasierten Unternehmensressourcen mit demselben Benutzernamen und Kennwort anmelden, die sie lokal verwenden. Azure AD Connect überprüft in regelmäßigen Abständen Ihr lokales Verzeichnis auf Änderungen und hält Ihr Azure AD-Verzeichnis synchronisiert. Wenn ein Benutzerattribut oder Kennwort im lokalen Active Directory geändert wird, erfolgt auch eine automatische Aktualisierung in Azure AD. 

Für die meisten Organisationen, die lediglich die Benutzeranmeldung für Office 365, SaaS-Anwendungen und andere Azure AD-basierte Ressourcen aktivieren müssen, empfiehlt sich die Standardoption für die Kennwortsynchronisierung. Wenn dies für Sie nicht funktioniert, müssen Sie sich zwischen Pass-Through-Authentifizierung und AD FS entscheiden.

> [!TIP]
> Benutzerkennwörter werden in einer lokalen Instanz von Windows Server Active Directory in Form eines Hashwerts gespeichert, der das eigentliche Benutzerkennwort darstellt. Ein Hashwert ist das Ergebnis einer unidirektionalen mathematischen Funktion (des Hashalgorithmus). Es ist nicht möglich, das Ergebnis einer unidirektionalen Funktion in die Nur-Text-Version eines Kennworts umzukehren. Sie können einen Kennworthash nicht zur Anmeldung in Ihrem lokalen Netzwerk verwenden. Wenn Sie sich dafür entscheiden, Kennwörter zu synchronisieren, extrahiert Azure AD Connect Kennworthashes aus der lokalen Active Directory-Instanz und führt eine zusätzliche Sicherheitsverarbeitung für den Kennworthash durch, bevor er mit Azure AD synchronisiert wird. Die Kennwortsynchronisierung kann auch mit der Kennwortrückschreibung kombiniert werden, um es Benutzern zu ermöglichen, ihre Kennwörter in Azure AD selbst zurückzusetzen. Darüber hinaus können Sie einmaliges Anmelden (SSO) für Benutzer auf in die Domäne eingebundenen Computern aktivieren, die mit dem Unternehmensnetzwerk verbunden sind. Beim einmaligen Anmelden müssen aktivierte Benutzer nur einen Benutzernamen eingeben, um sicher auf Cloudressourcen zuzugreifen. 

## <a name="pass-through-authentication"></a>Passthrough-Authentifizierung
Die [Azure AD-Pass-Through-Authentifizierung](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) bietet eine einfache Kennwortüberprüfungslösung für auf Azure AD basierende Dienste mit Ihrer lokalen Active Directory-Instanz. Wenn die Sicherheits- und Compliancerichtlinien für Ihre Organisation das Senden von Benutzerkennwörtern selbst in Hashform nicht zulassen und Sie nur Desktop-SSO für in die Domäne eingebundene Geräte unterstützen müssen, sollten Sie die Pass-Through-Authentifizierung in Erwägung ziehen. Die Pass-Through-Authentifizierung erfordert keine Bereitstellung in der DMZ, sodass die Bereitstellungsinfrastruktur im Vergleich zu AD FS vereinfacht ist. Wenn Benutzer sich mithilfe von Azure AD anmelden, überprüft diese Authentifizierungsmethode die Benutzerkennwörter direkt anhand Ihrer lokalen Active Directory-Instanz.

![Passthrough-Authentifizierung](./media/choose-hybrid-identity-solution/pass-through-authentication.png)

Mit der Pass-Through-Authentifizierung wird keine komplexe Netzwerkinfrastruktur benötigt, und Sie müssen keine lokalen Kennwörter in der Cloud speichern. In Kombination mit einmaligem Anmelden bietet die Pass-Through-Authentifizierung eine wahrhaft integrierte Lösung für die Anmeldung bei Azure AD oder anderen Clouddiensten.

Die Pass-Through-Authentifizierung kann mit Azure AD Connect konfiguriert werden. Dabei wird über einen einfachen lokalen Agent auf Anforderungen nach Kennwortüberprüfung gelauscht. Der Agent lässt sich problemlos auf mehreren Computern bereitstellen, um für hohe Verfügbarkeit und Lastenausgleich zu sorgen. Da die gesamte Kommunikation ausgehend ist, muss der Connector nicht in einer DMZ installiert werden. Folgende Servercomputeranforderungen gelten für den Connector:

- Windows Server 2012 R2 oder höher
- Mitglied einer Domäne in der Gesamtstruktur, über die Benutzer überprüft werden

> [!NOTE]
> Die Azure AD-Pass-Through-Authentifizierung befindet sich derzeit in der Vorschau und wird für browserbasierte Clients und Office-Clients unterstützt, die moderne Authentifizierungsmechanismen unterstützen. Im Fall von Clients, die eine solche Unterstützung nicht bieten, z.B. ältere Office-Clients oder Exchange Active Sync (einschließlich nativer E-Mail-Clients auf mobilen Geräten), wird empfohlen, die entsprechende äquivalente Methode zur modernen Authentifizierung zu verwenden. Mit moderner Authentifizierung wird nicht nur die Pass-Through-Authentifizierung ermöglicht, sondern auch die Anwendung von Richtlinien für bedingten Zugriff, beispielsweise durch mehrstufige Authentifizierung. 

Die Pass-Through-Authentifizierung wird zurzeit nicht bei Verwendung von Windows 10-Geräten in Azure AD unterstützt. Sie können jedoch die Kennworthash-Synchronisierung als automatisches Fallback zur Unterstützung der genannten Windows 10- und Legacyclients verwenden. Während der Vorschau ist die Kennworthashsynchronisierung standardmäßig aktiviert, wenn die Pass-Through-Authentifizierung als Anmeldeoption in Azure AD Connect ausgewählt wird.


## <a name="federated-identity-ad-fs"></a>Identitätsverbund (AD FS)
Zur besseren Steuerung des Zugriffs auf Office 365 und andere Clouddienste durch Benutzer können Sie die Verzeichnissynchronisierung mit einmaligem Anmelden (SSO) über [Active Directory-Verbunddienste (AD FS)](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server-2016) einrichten. Durch den Identitätsverbund der Anmeldevorgänge Ihrer Benutzer mit AD FS wird die Authentifizierung an einen lokalen Server delegiert, der die Anmeldeinformationen des Benutzers überprüft. In diesem Modell werden lokale Active Directory-Anmeldeinformationen niemals an Azure AD übergeben.

![Identitätsverbund](./media/choose-hybrid-identity-solution/federated-identity.png)

Diese auch als Verbundidentität bezeichnete Anmeldemethode gewährleistet, dass die Benutzerauthentifizierung ausschließlich lokal gesteuert wird, und sie ermöglicht es Administratoren, striktere Ebenen der Zugriffssteuerung zu implementieren. Ein Identitätsverbund mit AD FS ist die komplizierteste Option, da hierbei zusätzliche Server in Ihrer lokalen Umgebung bereitgestellt werden müssen. Beim Identitätsverbund müssen Sie außerdem rund um die Uhr Support für die Active Directory- und AD FS-Infrastruktur anbieten. Dieses hohe Maß an Support ist erforderlich, da sich Benutzer bei Ausfällen Ihres lokalen Internetzugriffs, Ihrer Domänencontroller oder Ihrer AD FS-Server nicht bei Clouddiensten anmelden können.

> [!TIP]
> Wenn Sie die Option „Verbund mit Active Directory Federation Services (AD FS)“ verwenden, können Sie außerdem optional die Kennwortsynchronisierung als Zusatzmaßnahme für den Fall aktivieren, dass Ihre AD FS-Infrastruktur ausfällt.


## <a name="common-scenarios-and-recommendations"></a>Gängige Szenarien und Empfehlungen
Im Folgenden sind einige verbreitete Hybrididentitäts- und Zugriffsverwaltungsszenarien mit Empfehlungen zur Auswahl der jeweils geeigneten Hybrididentitätsoptionen aufgeführt.

|Ziel|PWS und SSO<sup>1</sup>| PTA und SSO<sup>2</sup> | AD FS<sup>3</sup>|
|-----|-----|-----|-----|
|Automatisches Synchronisieren neuer Benutzer-, Kontakt- und Gruppenkonten, die in meiner lokalen Active Directory-Instanz erstellt werden, mit der Cloud|![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png) |![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Einrichten meines Mandanten für Office 365-Hybridszenarien|![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png) |![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Ermöglichen der Anmeldung und des Zugriffs auf Clouddienste für meine Benutzer mit ihrem lokalen Kennwort|![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png) |![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Implementieren des einmaligen Anmeldens mit Anmeldeinformationen des Unternehmens|![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png) |![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Sicherstellen, dass keine Kennworthashes in der Cloud gespeichert werden| |![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png)|![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Aktivieren der lokalen Multi-Factor Authentication-Lösungen| | |![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Unterstützung von Smartcard-Authentifizierung für meine Benutzer<sup>4</sup>| | |![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Anzeigen von Benachrichtigungen zum Kennwortablauf im Office-Portal und auf dem Windows 10-Desktop| | |![Empfohlen](./media/choose-hybrid-identity-solution/ic195031.png)|

> <sup>1</sup> Kennwortsynchronisierung mit einmaliger Anmeldung 

> <sup>2</sup> Pass-Through-Authentifizierung und einmaliges Anmelden 

> <sup>3</sup> Einmalige Verbundanmeldung mit AD FS

> <sup>4</sup> AD FS kann in Ihre Unternehmens-PKI integriert werden, damit die Anmeldung mithilfe von Zertifikaten möglich ist. Bei diesen Zertifikaten kann es sich um Softzertifikate handeln, die über vertrauenswürdige Bereitstellungskanäle bereitgestellt werden, etwa MDM-, GPO- oder Smartcard-Zertifikate (einschließlich PIV/CAC-Karten) oder Hello for Business (cert-trust). Weitere Informationen zur Unterstützung der Smartcard-Authentifizierung finden Sie in [diesem Blog](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/).


## <a name="next-steps"></a>Nächste Schritte
[Erlangen weiterer Kenntnisse in einer Azure Proof of Concept-Umgebung](https://aka.ms/aad-poc)

[Installieren von Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771)

[Überwachen der Hybrididentitätssynchronisierung](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)


