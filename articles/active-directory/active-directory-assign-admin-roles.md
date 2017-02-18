---
title: Zuweisen von Administratorrollen in Azure Active Directory | Microsoft Docs
description: "Erläutert, welche Administratorrollen in Azure Active Directory verfügbar sind und wie sie zugewiesen werden."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7fc27e8e-b55f-4194-9b8f-2e95705fb731
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 2d428e0e3aaf8fd4a2138648411da644ccd308f6
ms.openlocfilehash: 3c19c2035b8dc9717dced5164b0214ab8019afd1


---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Zuweisen von Administratorrollen in Azure Active Directory
Mithilfe von Azure Active Directory (Azure AD) können Sie verschiedene Administratoren bestimmen, um unterschiedliche Funktionen zu erfüllen. Diese Administratoren haben Zugriff auf verschiedene Funktionen im Azure-Portal oder im klassischen Azure-Portal und können abhängig von ihrer Rolle unter anderem Benutzer erstellen oder bearbeiten, anderen administrative Rollen zuweisen, Benutzerkennwörter zurücksetzen, Benutzerlizenzen verwalten und Domänen verwalten. Ein Benutzer mit einer Administratorrolle besitzt die gleichen Berechtigungen für alle Clouddienste, die Ihre Organisation abonniert hat, unabhängig davon, ob Sie die Rolle im Office 365-Portal, im klassischen Azure-Portal oder mithilfe des Azure AD-Moduls für Windows PowerShell zuweisen.

Die folgenden Administratorrollen sind verfügbar:

* **[Rechnungsadministrator:](#billing-administrator)** Erledigt Einkäufe, verwaltet Abonnements, verwaltet Supporttickets und überwacht die Dienstintegrität.
* **[Globaler Administrator/Unternehmensadministrator:](#global-administrator)** Hat Zugriff auf alle administrativen Funktionen. Die Person, die die Anmeldung für das Azure-Konto vornimmt, wird ein globaler Administrator. Nur globale Administratoren können weitere Administratorrollen zuweisen. In Ihrem Unternehmen können mehrere globale Administratoren vorhanden sein.

  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und in Azure AD PowerShell wird diese Rolle als „Unternehmensadministrator“ identifiziert. Im [Azure-Portal](https://portal.azure.com)ist dies der „globale Administrator“.
  >
  >
* **Complianceadministrator:** Benutzer mit dieser Rolle verfügen über Verwaltungsberechtigungen innerhalb von [Office 365 Security & Compliance Center](https://support.office.com/en-us/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1?ui=en-US&rs=en-US&ad=US&fromAR=1) und [Exchange Admin Center](https://technet.microsoft.com/en-us/library/jj657489(v=exchg.150).aspx) sowie über Lesezugriff auf Berichte im Office 365 Admin Center. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-US&ad=US).
* **CRM-Dienstadministrator:** Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft CRM Online, wenn der Dienst vorhanden ist. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-US&ad=US).
* **Geräteadministratoren:** Benutzer mit dieser Rolle werden zu Administratoren für alle Windows 10-Geräte, die mit Azure Active Directory verknüpft werden.
* **Verzeichnis lesen**: Dies ist eine Legacyrolle, die Anwendungen ohne Unterstützung für das [Consent Framework](active-directory-integrating-applications.md)zugewiesen wird. Diese Rolle sollte keinem Benutzer zugewiesen werden.
* **Konten für die Verzeichnissynchronisierung**: Verwenden Sie diese Rolle nicht. Diese Rolle wird automatisch dem Azure AD Connect-Dienst zugewiesen und ist weder für eine andere Verwendung vorgesehen, noch wird eine andere Verwendung unterstützt.
* **Verzeichnis schreiben**: Dies ist eine Legacyrolle, die Anwendungen ohne Unterstützung für das [Consent Framework](active-directory-integrating-applications.md)zugewiesen wird. Diese Rolle sollte keinem Benutzer zugewiesen werden.
* **Exchange-Dienstadministrator**: Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft Exchange Online, wenn der Dienst vorhanden ist. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-US&ad=US).
* **Intune-Dienstadministrator**: Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft Intune Online, wenn der Dienst vorhanden ist. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-US&ad=US).
* **Skype for Business-Dienstadministrator**: Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft Skype for Business, wenn der Dienst vorhanden ist. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-US&ad=US). Diese Rolle wurde bisher als **Lync-Dienstadministrator** bezeichnet.
* **Gäste Einladender:** Benutzer in dieser Rolle können Gasteinladungen verwalten. Es sind keine anderen Berechtigungen eingeschlossen.
* **Postfachadministrator:** Diese Rolle wird nur im Rahmen der Exchange Online-E-Mail-Unterstützung für RIM Blackberry-Geräte verwendet. Verwenden Sie diese Rolle nicht, wenn Ihre Organisation keine Exchange Online-E-Mails auf RIM Blackberry-Geräten verwendet.
* **Partner Tier 1 Support** (Partnerunterstützung auf Ebene 1): Nicht verwenden. Diese Rolle wurde als veraltet markiert und wird aus Azure AD entfernt. Diese Rolle ist für einige wenige Wiederverkaufspartner von Microsoft und nicht zur allgemeinen Verwendung vorgesehen.
* **Partner Tier 2 Support** (Partnerunterstützung auf Ebene 2): Nicht verwenden. Diese Rolle wurde als veraltet markiert und wird aus Azure AD entfernt. Diese Rolle ist für einige wenige Wiederverkaufspartner von Microsoft und nicht zur allgemeinen Verwendung vorgesehen.
* **[Kennwortadministrator/Helpdeskadministrator:](#password-administrator)** Setzt Kennwörter zurück, verwaltet Dienstanforderungen und überwacht die Integrität des Diensts. Kennwortadministratoren können Kennwörter nur für Benutzer und andere Kennwortadministratoren zurücksetzen.

  > [!NOTE]
  > In der Microsoft Graph-API, der Azure AD Graph-API und Azure AD PowerShell wird diese Rolle als „Helpdeskadministrator“ identifiziert.
  >
  >
* **SharePoint-Dienstadministrator**: Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft SharePoint Online, wenn der Dienst vorhanden ist. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-US&ad=US).
* **[Dienstadministrator:](#service-administrator)** Verwaltet Dienstanforderungen und überwacht die Integrität des Dienstes.

  > [!NOTE]
  > Um einem Benutzer die Dienstadministratorrolle zuzuweisen, muss der globale Administrator zunächst dem Benutzer Administratorberechtigungen im Dienst (z. B. Exchange Online) zuweisen, und anschließend die Dienstadministratorrolle im klassischen Azure-Portal.
  >
  >
* **[Benutzerkontenadministrator:](#user-administrator)** Setzt Kennwörter zurück, überwacht die Dienstintegrität und verwaltet Benutzerkonten, Benutzergruppen und Dienstanforderungen. Für die Berechtigungen eines Benutzerverwaltungsadministrators gelten einige Einschränkungen. Sie können z. B. keinen globalen Administrator löschen oder andere Administratoren erstellen. Sie können außerdem keine Kennwörter für Abrechnungs-, globale und Dienstadministratoren zurücksetzen.
* **[Sicherheit lesen:](#security-reader)** Schreibgeschützter Zugriff auf eine Reihe von Sicherheitsfeatures von Identity Protection Center, Privileged Identity Management, Monitor Office 365 Service Health und Office 365 Security & Compliance Center.
* **[Sicherheitsadministrator:](#security-administrator)** Alle Leseberechtigungen der Rolle **Sicherheit lesen** sowie eine Reihe zusätzlicher administrativer Berechtigungen für die gleichen Dienste: Identity Protection Center, Privileged Identity Management, Monitor Office 365 Service Health und Office 365 Security & Compliance Center.

## <a name="administrator-permissions"></a>Administratorberechtigungen
### <a name="billing-administrator"></a>Rechnungsadministrator
| Möglich | Nicht möglich |
| --- | --- |
| <p>Anzeigen von Unternehmens- und Benutzerinformationen</p><p>Verwalten von Office-Supporttickets</p><p>Durchführen von Abrechnungs- und Kaufvorgängen für Office-Produkte</p> |<p>Zurücksetzen von Benutzerkennwörtern</p><p>Erstellen und Verwalten von Benutzeransichten</p><p>Erstellen, Bearbeiten und Löschen von Benutzern und Gruppen und Verwalten von Benutzerlizenzen</p><p>Verwalten von Domänen</p><p>Verwalten von Unternehmensinformationen</p><p>Delegieren von Administratorrollen an andere Benutzer</p><p>Verwenden der Verzeichnissynchronisierung</p><p>Anzeigen von Berichten</p> |

### <a name="global-administrator"></a>Globaler Administrator
| Möglich | Nicht möglich |
| --- | --- |
| <p>Anzeigen von Unternehmens- und Benutzerinformationen</p><p>Verwalten von Office-Supporttickets</p><p>Durchführen von Abrechnungs- und Kaufvorgängen für Office-Produkte</p> <p>Zurücksetzen von Benutzerkennwörtern</p><p>Erstellen und Verwalten von Benutzeransichten</p><p>Erstellen, Bearbeiten und Löschen von Benutzern und Gruppen und Verwalten von Benutzerlizenzen</p><p>Verwalten von Domänen</p><p>Verwalten von Unternehmensinformationen</p><p>Delegieren von Administratorrollen an andere Benutzer</p><p>Verwenden der Verzeichnissynchronisierung</p><p>Aktivieren und Deaktivieren der Multi-Factor Authentication</p><p>Anzeigen von Berichten</p> |N/V |

### <a name="password-administrator"></a>Kennwortadministrator
| Möglich | Nicht möglich |
| --- | --- |
| <p>Anzeigen von Unternehmens- und Benutzerinformationen</p><p>Verwalten von Office-Supporttickets</p><p>Zurücksetzen von Benutzerkennwörtern</p> |<p>Durchführen von Abrechnungs- und Kaufvorgängen für Office-Produkte</p><p>Erstellen und Verwalten von Benutzeransichten</p><p>Erstellen, Bearbeiten und Löschen von Benutzern und Gruppen und Verwalten von Benutzerlizenzen</p><p>Verwalten von Domänen</p><p>Verwalten von Unternehmensinformationen</p><p>Delegieren von Administratorrollen an andere Benutzer</p><p>Verwenden der Verzeichnissynchronisierung</p><p>Anzeigen von Berichten</p> |

### <a name="service-administrator"></a>Dienstadministrator
| Möglich | Nicht möglich |
| --- | --- |
| <p>Anzeigen von Unternehmens- und Benutzerinformationen</p><p>Verwalten von Office-Supporttickets</p> |<p>Zurücksetzen von Benutzerkennwörtern</p><p>Durchführen von Abrechnungs- und Kaufvorgängen für Office-Produkte</p><p>Erstellen und Verwalten von Benutzeransichten</p><p>Erstellen, Bearbeiten und Löschen von Benutzern und Gruppen und Verwalten von Benutzerlizenzen</p><p>Verwalten von Domänen</p><p>Verwalten von Unternehmensinformationen</p><p>Delegieren von Administratorrollen an andere Benutzer</p><p>Verwenden der Verzeichnissynchronisierung</p><p>Anzeigen von Berichten</p> |

### <a name="user-administrator"></a>Benutzeradministrator
| Möglich | Nicht möglich |
| --- | --- |
| <p>Anzeigen von Unternehmens- und Benutzerinformationen</p><p>Verwalten von Office-Supporttickets</p><p>Zurücksetzen von Benutzerkennwörtern (mit Einschränkungen). Er kann keine Kennwörter für Abrechnungs-, globale und Dienstadministratoren zurücksetzen.</p><p>Erstellen und Verwalten von Benutzeransichten</p><p>Erstellen, Bearbeiten und Löschen von Benutzern und Gruppen, Verwalten von Benutzerlizenzen (mit Einschränkungen). Er oder Sie kann keinen globalen Administrator löschen oder andere Administratoren erstellen.</p> |<p>Durchführen von Abrechnungs- und Kaufvorgängen für Office-Produkte</p><p>Verwalten von Domänen</p><p>Verwalten von Unternehmensinformationen</p><p>Delegieren von Administratorrollen an andere Benutzer</p><p>Verwenden der Verzeichnissynchronisierung</p><p>Aktivieren und Deaktivieren der Multi-Factor Authentication</p><p>Anzeigen von Berichten</p> |

### <a name="security-reader"></a>Sicherheit lesen
| Geben Sie in | Möglich |
| --- | --- |
| Identity Protection Center |Lesen von allen Sicherheitsberichten und Einstellungsinformationen für die Sicherheitsfunktionen<ul><li>Antispam<li>Verschlüsselung<li>Verhindern von Datenverlusten<li>Antischadsoftware<li>Erweiterter Schutz vor Bedrohungen<li>Antiphishing<li>Nachrichtenflussregeln |
| Privileged Identity Management |<p>Verfügt über schreibgeschützten Zugriff auf alle eingeblendeten Informationen in Azure AD PIM: Richtlinien und Berichte für Azure AD-Rollenzuweisungen, Sicherheitsüberprüfungen und in Zukunft Lesezugriff auf Richtliniendaten und Berichte für Szenarios zusätzlich zur Azure AD-Rollenzuweisung.<p>**Kann sich nicht** für Azure AD PIM registrieren oder Änderungen durchführen. Im PIM-Portal oder über PowerShell können Personen mit dieser Rolle zusätzliche Rollen (z.B. globaler Administrator oder Administrator für privilegierte Rollen) aktivieren, wenn der Benutzer für sie geeignet ist. |
| <p>Überwachen der Office 365-Dienstintegrität</p><p>Office 365 Security & Compliance Center</p> |<ul><li>Lesen und Verwalten von Warnungen<li>Lesen von Sicherheitsrichtlinien<li>Lesen von Bedrohungsanalysen, Cloud App Discovery und Quarantäne in Suchen und Untersuchungen<li>Lesen aller Berichte |

### <a name="security-administrator"></a>Sicherheitsadministrator
| Geben Sie in | Möglich |
| --- | --- |
| Identity Protection Center |<ul><li>Alle Berechtigungen der Rolle „Sicherheit lesen“<li>Darüber hinaus die Möglichkeit, alle IPC-Vorgänge außer des Zurücksetzens von Kennwörtern auszuführen. |
| Privileged Identity Management |<ul><li>Alle Berechtigungen der Rolle „Sicherheit lesen“<li>**nicht** verwalten. |
| <p>Überwachen der Office 365-Dienstintegrität</p><p>Office 365 Security & Compliance Center |<ul><li>Alle Berechtigungen der Rolle „Sicherheit lesen“<li>Kann alle Einstellungen im Feature „Advanced Threat Protection“ (Schutz vor Malware und Viren, schadhafte URL-Konfiguration, URL-Ablaufverfolgung usw.) konfigurieren. |

## <a name="details-about-the-global-administrator-role"></a>Details zur globalen Administratorrolle
Der globale Administrator hat Zugriff auf alle administrativen Funktionen. Standardmäßig wird der Person, die sich für ein Azure-Abonnement registriert, die globale Administratorrolle für das Verzeichnis zugewiesen. Nur globale Administratoren können weitere Administratorrollen zuweisen.

## <a name="assign-or-remove-administrator-roles"></a>Zuweisen oder Entfernen von Administratorrollen
1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)auf **Active Directory**, und klicken Sie dann auf den Namen des Verzeichnisses Ihrer Organisation.
2. Auf der Seite **Benutzer** klicken Sie auf den Anzeigenamen des Benutzers, den Sie bearbeiten möchten.
3. Wählen Sie in der Liste **Organisationsrolle** die Administratorrolle aus, die diesem Benutzer zugewiesen werden soll, oder wählen Sie **Benutzer** aus, wenn Sie eine vorhandene Administratorrolle entfernen möchten.
4. Im Feld **Alternative E-Mail-Adresse** geben Sie eine E-Mail-Adresse ein. Diese E-Mail-Adresse wird für wichtige Benachrichtigungen, einschließlich des automatischen Zurücksetzens des Kennworts verwendet, so dass der Benutzer Zugriff auf das E-Mail-Konto benötigt, und zwar unabhängig davon, ob er auf Azure zugreifen kann oder nicht.
5. Wählen Sie **Zulassen** oder **Blockieren** aus, um anzugeben, ob der Benutzer sich anmelden kann und Zugriff auf die Dienste erhält.
6. Geben Sie einen Speicherort aus der Dropdown-Liste **Nutzungsspeicherort** an.
7. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Ändern von Administratoren für ein Azure-Abonnement finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen](../billing/billing-add-change-azure-subscription-administrator.md)
* Informationen dazu, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zum Zugriff auf Ressourcen in Azure](active-directory-understanding-resource-access.md)
* Weitere Informationen zur Beziehung zwischen Azure Active Directory und Ihrem Azure-Abonnement finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Verwalten von Benutzern](active-directory-create-users.md)
* [Verwalten von Kennwörtern](active-directory-manage-passwords.md)
* [Gruppen verwalten](active-directory-manage-groups.md)



<!--HONumber=Feb17_HO2-->


