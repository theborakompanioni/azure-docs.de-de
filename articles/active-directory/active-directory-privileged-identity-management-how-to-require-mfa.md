---
title: Erzwingen der Multi-Factor Authentication | Microsoft Docs
description: Erfahren Sie, wie Sie mit der Erweiterung Azure Active Directory Privileged Identity Management Multi-Factor Authentication (MFA) als erforderlich festlegen.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 1e3dc4ad-3a6a-4a52-8417-3ca4f84ae05c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/01/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d36fb97f22802b040d2d527ccfd7a48327fe0cb4


---
# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>Erfordern von MFA in Azure AD Privileged Identity Management
Es wird empfohlen, von allen Administratoren Multi-Factor Authentication (MFA) zu fordern. Auf diese Weise sinkt das Risiko eines Angriffs aufgrund eines offengelegten Kennworts.

Sie können festlegen, dass Benutzer beim Anmelden eine mehrstufige Authentifizierung durchlaufen müssen. Im Blogbeitrag [MFA for Office 365 and MFA for Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) (MFA für Office 365 und MFA für Azure) werden die in Office- und Azure-Abonnements enthaltenen Funktionen mit den Funktionen der Microsoft Azure Multi-Factor Authentication verglichen.

Sie können auch festlegen, dass Benutzer eine mehrstufige Authentifizierung durchlaufen müssen, wenn sie eine Rolle in Azure AD PIM aktivieren. Auf diese Weise wird ein Benutzer, der bei der Anmeldung keine mehrstufige Authentifizierung durchlaufen hat, von PIM aufgefordert, dies zu tun.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Erfordern von MFA in Azure AD Privileged Identity Management
Wenn Sie Identitäten in PIM als Administrator für privilegierte Rollen verwalten, werden möglicherweise Warnungen angezeigt, die MFA für privilegierte Konten empfehlen. Klicken Sie im PIM-Dashboard auf die Sicherheitswarnung. Anschließend wird ein neues Blatt mit einer Liste der Administratorkonten geöffnet, für die MFA erforderlich sein sollte.  Sie können MFA als erforderlich festlegen, indem Sie mehrere Rollen auswählen und dann auf die Schaltfläche **Korrigieren** klicken. Sie können aber auch auf die Auslassungspunkte neben einzelnen Rollen und dann auf die Schaltfläche **Korrigieren** klicken.

> [!IMPORTANT]
> Zurzeit funktioniert Azure MFA nur für Geschäfts- oder Schulkonten, aber nicht für Microsoft-Konten (normalerweise ein persönliches Konto, das zum Anmelden bei Microsoft-Diensten wie Skype, Xbox, Outlook.com usw. verwendet wird). Aus diesem Grund kann kein Benutzer mit einem Microsoft-Konto ein berechtigter Administrator sein, da zum Aktivieren der Rollen MFA verwendet werden muss. Wenn Benutzer weiterhin Workloads mit einem Microsoft-Konto verwalten müssen, stufen Sie sie vorerst zu permanenten Administratoren hoch.
> 
> 

Darüber hinaus können Sie die MFA-Anforderung für eine bestimmte Rolle ändern, indem Sie auf dem PIM-Dashboard im Abschnitt „Rollen“ auf die Rolle klicken. Klicken Sie dann auf dem Blatt der Rolle auf **Einstellungen**, und wählen Sie unter „Multi-Factor Authentication“ die Option **Aktivieren** aus.

## <a name="how-azure-ad-pim-validates-mfa"></a>Überprüfen der MFA durch Azure AD PIM
Wenn ein Benutzer eine Rolle aktiviert, stehen zwei Optionen zum Überprüfen von MFA zur Verfügung.

Die einfachste Methode besteht darin, sich bei Benutzern, die eine privilegierte Rolle aktivieren, auf Azure MFA zu verlassen. Überprüfen Sie dazu ggf. zunächst, ob diese Benutzer lizenziert sind und sich für Azure MFA registriert haben. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure Multi-Factor Authentication in der Cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Es ist zwar nicht erforderlich, wird jedoch empfohlen, Azure AD so zu konfigurieren, dass MFA für diese Benutzer bei ihrer Anmeldung erzwungen wird. Der Grund dafür ist, dass die MFA-Überprüfungen von Azure AD PIM selbst durchgeführt werden.

Wenn sich Benutzer lokal authentifizieren, können Sie alternativ festlegen, dass der Identitätsanbieter für MFA verantwortlich ist. Wenn Sie beispielsweise AD-Verbunddienste so konfiguriert haben, dass vor dem Zugriff auf Azure AD eine smartcardbasierte Authentifizierung erforderlich ist, lesen Sie die Anweisungen zum Konfigurieren von AD FS für das Senden von Ansprüchen an Azure AD unter [Sichern von Cloudressourcen mit Azure Multi-Factor Authentication und AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) . Wenn ein Benutzer versucht, eine Rolle zu aktivieren, akzeptiert Azure AD PIM, dass MFA für den Benutzer bereits überprüft wurde, sobald die Anwendung die entsprechenden Ansprüche empfängt.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]




<!--HONumber=Jan17_HO1-->


