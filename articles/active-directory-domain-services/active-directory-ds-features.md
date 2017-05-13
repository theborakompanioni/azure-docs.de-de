---
title: 'Vorschau der Azure Active Directory Domain Services: Features | Microsoft Docs'
description: Features der Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.translationtype: Human Translation
ms.sourcegitcommit: a3ca1527eee068e952f81f6629d7160803b3f45a
ms.openlocfilehash: 8005be7ded6ea005af086aeaf594963a5f2d4ac2
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Features
Die folgenden Features sind in Domänen verfügbar, die in Azure AD Domain Services verwaltet werden.

* **Einfache Bereitstellung:** Sie können die Azure AD Domain Services für Ihren Azure AD-Mandanten mit nur wenigen Mausklicks aktivieren. Unabhängig davon, ob es sich bei Ihrem Azure AD-Mandanten um einen Cloudmandanten handelt oder ob er mit Ihrem lokalen Verzeichnis synchronisiert wird, kann Ihre verwaltete Domäne schnell bereitgestellt werden.
* **Unterstützung für den Domänenbeitritt:** In einem virtuellen Azure-Netzwerk, in dem Ihre verwaltete Domäne verfügbar ist, können Sie Computer ganz einfach in eine Domäne aufnehmen. Das Beitreten zur Domäne auf Windows-Client- und Server-Betriebssystemen funktioniert nahtlos mit Domänen, die von den Azure Active Directory Domain Services bereitgestellt werden. Sie können bei diesen Domänen auch automatisierte Domänenbeitrittstools verwenden.
* **Eine Domäneninstanz pro Azure AD-Verzeichnis:** Sie können eine einzige Active Directory-Domäne für jedes Azure AD-Verzeichnis erstellen.
* **Domänen mit benutzerdefinierten Namen erstellen:** Erstellen Sie Domänen mit benutzerdefinierten Namen (z.B. contoso100.com) mithilfe der Azure Active Directory Domain Services. Sie können entweder überprüfte oder nicht überprüfte Domänennamen verwenden. Optional können Sie eine Domäne mit dem integrierten Domänensuffix erstellen (d.h. „*.onmicrosoft.com“), die von Ihrem Azure AD-Verzeichnis bereitgestellt wird.
* **In Azure AD integriert:** Sie müssen die Replikation in Azure Active Directory Domain Services nicht konfigurieren oder verwalten. Benutzerkonten, Gruppenmitgliedschaften und Benutzeranmeldeinformationen (Kennwörter) aus dem Azure AD-Verzeichnis stehen in den Azure Active Directory Domain Services automatisch zur Verfügung. Neue Benutzer, Gruppen oder Änderungen von Attributen auf Ihrem Azure AD-Mandanten oder in Ihrem lokalen Verzeichnis werden automatisch mit den Azure Active Directory Domain Services synchronisiert.
* **NTLM- und Kerberos-Authentifizierung:** Durch die Unterstützung für die NTLM- und Kerberos-Authentifizierung können Sie Anwendungen bereitstellen, die auf der integrierten Windows-Authentifizierung beruhen.
* **Verwenden der Anmeldeinformationen/Kennwörter Ihres Unternehmens:** Kennwörter für Benutzer auf Ihrem Azure AD-Mandanten können für die Azure Active Directory Domain Services verwendet werden. Benutzer können ihre Unternehmensanmeldeinformationen verwenden, um Computer in die Domäne aufzunehmen, sich interaktiv anzumelden oder Remotedesktop zu nutzen und die Authentifizierung gegenüber der verwalteten Domäne durchzuführen.
* **LDAP-Bindungs- und LDAP-Leseunterstützung:** Sie können auf LDAP-Bindungen beruhende Anwendungen verwenden, um Benutzer in Domänen zu authentifizieren, die von den Azure Active Directory Domain Services bereitgestellt werden. Darüber hinaus können Anwendungen, die Benutzer-/Computerattribute anhand von LDAP-Lesevorgängen aus dem Verzeichnis abfragen, auch mit den Azure Active Directory Domain Services arbeiten.
* **Sicheres LDAP (LDAPS):** Sie können den Zugriff auf das Verzeichnis über sicheres LDAP (LDAPS) aktivieren. Sicherer LDAP-Zugriff ist innerhalb des virtuellen Netzwerks standardmäßig verfügbar. Allerdings können Sie optional auch sicheren LDAP-Zugriff über das Internet aktivieren.
* **Gruppenrichtlinie:** Sie können ein einzelnes integriertes GPO sowohl für die Benutzer- als auch für die Computercontainer verwenden, um Compliance mit den erforderlichen Sicherheitsrichtlinien für Benutzerkonten und in die Domäne eingebundene Computer durchzusetzen. Sie können auch eigene benutzerdefinierte Gruppenrichtlinienobjekte erstellen und benutzerdefinierten Organisationseinheiten zuweisen, um die [Gruppenrichtlinie zu verwalten](active-directory-ds-admin-guide-administer-group-policy.md).
* **DNS verwalten:** Mitglieder der Gruppe der AAD-DC-Administratoren können DNS für Ihre verwaltete Domäne mit vertrauten DNS-Verwaltungstools wie dem MMC-Snap-In verwalten.
* **Benutzerdefinierte Organisationseinheiten (OUs) erstellen:** Mitglieder der Gruppe der AAD-DC-Administratoren können benutzerdefinierte Organisationseinheiten in der verwalteten Domäne erstellen. Diesen Benutzern werden vollständige Administratorrechte für benutzerdefinierte Organisationseinheiten erteilt, sodass sie Dienstkonten, Computer, Gruppen usw. in diesen benutzerdefinierten Organisationseinheiten hinzufügen/entfernen können.
* **Verfügbar in mehreren Azure-Regionen:** Informationen zu den Azure-Regionen, in denen Azure AD Domain Services verfügbar sind, finden Sie unter [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services/) .
* **Hohe Verfügbarkeit:** Azure AD Domain Services bieten eine hohe Verfügbarkeit für Ihre Domäne. Durch dieses Feature wird eine höhere Dienstverfügbarkeit und Stabilität bei Fehlern gewährleistet. Die integrierte Integritätsüberwachung bietet eine automatische Wiederherstellung nach Fehlern durch das Einrichten neuer Instanzen zum Ersetzen fehlerhafter Instanzen und zum Bereitstellen eines fortlaufenden Diensts für Ihre Domäne.
* **Verwendung vertrauter Verwaltungstools:** Sie können vertraute Windows Server Active Directory-Verwaltungstools wie das Active Directory-Verwaltungscenter oder Active Directory-PowerShell zur Verwaltung von Domänen einsetzen, um Domänen zu verwalten.

