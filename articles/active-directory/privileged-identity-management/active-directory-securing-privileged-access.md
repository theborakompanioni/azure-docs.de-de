---
title: Sichern des privilegierten Zugriffs in Azure AD | Microsoft-Dokumentation
description: "In diesem Thema werden Methoden zum Sichern des privilegierten Zugriffs über Azure, Azure Active Directory und Microsoft Online Services hinweg erläutert."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: mwahl
ms.assetid: 235a0ce9-1daf-4433-8f65-9c6afcd64d08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 0e6c3c516b5d4271c5201d0734e22a83876dd3fa


---
# <a name="securing-privileged-access-in-azure-ad"></a>Sichern des privilegierten Zugriffs in Azure AD
Das Sichern des privilegierten Zugriffs ist ein entscheidender erster Schritt, um die geschäftlichen Ressourcen in einem modernen Unternehmen zu schützen. Mit privilegierten Konten werden IT-Systeme verwaltet. Cyberkriminelle versuchen diese Konten anzugreifen, um Zugriff auf die Daten und Systeme eines Unternehmens zu erhalten. Zum Schutz des privilegierten Zugriffs empfiehlt es sich, Konten und Systeme zu isolieren, um sie vor dem Zugriff durch böswillige Benutzer zu schützen.

Über Clouddienste erhalten immer mehr Benutzer privilegierten Zugriff. Hierzu gehören globale Administratoren von Office 365, Azure-Abonnementadministratoren und Benutzer die administrativen Zugriff zu virtuellen Computern oder SaaS-Apps besitzen.

Microsoft empfiehlt, der Roadmap unter [Securing Privileged Access](https://technet.microsoft.com/library/mt631194.aspx)(Sichern des privilegierten Zugriffs) zu folgen.

Für Kunden, die Azure Active Directory für die Verwaltung des Zugriffs auf Azure, Office 365 oder andere Microsoft-Dienste und -Anwendungen verwenden, gelten diese Prinzipien unabhängig davon, ob die Benutzerkonten über Active Directory oder Azure Active Directory verwaltet und authentifiziert werden. In den folgenden Abschnitten finden Sie weitere Informationen zu den Azure AD-Features, die das Sichern des privilegierten Zugriffs unterstützen.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Um die Sicherheit der Administratorauthentifizierung zu erhöhen, sollten Sie Multi-Factor Authentication (MFA) erfordern, bevor Berechtigungen gewährt werden. MFA ist eine Methode zum Überprüfen der Identität, für die mehr als nur ein Benutzername und ein Kennwort erforderlich ist. Sie bietet eine zweite Sicherheitsebene für Benutzeranmeldungen und -transaktionen.

Azure Multi-Factor Authentication hilft beim Schutz des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren. Sie bietet eine leistungsfähige Authentifizierung mittels verschiedener einfacher Überprüfungsoptionen: Telefonanruf, SMS, Benachrichtigung in der mobilen App, Überprüfungscode und Drittanbieter-OATH-Token.

Eine Übersicht über die Funktionsweise von Azure Multi-Factor Authentication finden Sie im folgenden Video.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Windows-Azure-Multi-Factor-Authentication/player]
> 
> 

Weitere Informationen finden Sie unter [MFA for Office 365 and MFA for Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/)(MFA für Office 365 und MFA für Azure).

## <a name="time-bound-privileges"></a>Zeitgebundene Berechtigungen
Einige Organisationen stellen möglicherweise fest, dass zu viele Benutzer Rollen mit hohen Berechtigungen innehaben. Möglicherweise wurden Benutzer für eine bestimmte Aktivität zu einer Rolle hinzugefügt, beispielsweise zur Anmeldung bei einem Dienst, haben diese Berechtigung später aber nicht mehr häufig verwendet.

Um Berechtigungen nicht unnötig lange zu gewähren und den Einblick in die Verwendung der Berechtigungen zu erhöhen, sollten Sie Just-In-Time-Berechtigungen für Benutzer einrichten, die die Nutzung der Berechtigung auf die Dauer der Aufgabenausführung beschränken. Für Azure Active Directory und Microsoft Online Services können Sie [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM)verwenden.

![PIM-Dashboard][2]

## <a name="attack-detection"></a>Angriffserkennung
[Azure Active Directory Identity Protection](../active-directory-identityprotection.md) bietet eine umfassende Übersicht über Risikoereignisse und potenzielle Sicherheitsrisiken, die für die Identitäten Ihrer Organisation bestehen. Basierend auf Risikoereignissen berechnet Identity Protection eine Benutzerrisikostufe für jeden Benutzer, sodass Sie risikobasierte Richtlinien konfigurieren können, um die Identitäten Ihrer Organisation automatisch zu schützen. Mit diesen Richtlinien in Verbindung mit anderen Steuerungselementen für den bedingten Zugriff, die von Azure Active Directory und EMS bereitgestellt werden, können Benutzer blockiert oder Vorschläge unterbreitet werden. Hierzu gehören Kennwortzurücksetzungen und die Durchsetzung der Multi-Factor Authentication.

![Azure AD Identity Protection][3]

## <a name="conditional-access"></a>Bedingter Zugriff
Mit der bedingten Zugriffssteuerung überprüft Azure Active Directory beim Authentifizieren des Benutzers die von Ihnen ausgewählten besonderen Bedingungen, bevor der Zugriff auf eine Anwendung gewährt wird. Nachdem diese Bedingungen erfüllt sind, wird der Benutzer authentifiziert und erhält Zugriff auf die Anwendung.

![Festlegen bedingter Zugriffsregeln mit MFA][4]

## <a name="related-articles"></a>Verwandte Artikel
* Aktivieren von [Azure Multi-Factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
* Aktivieren von [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)
* Aktivieren von [Azure AD Identity Protection](../active-directory-identityprotection.md)
* Aktivieren der [Steuerung des bedingten Zugriffs](../active-directory-conditional-access.md)

Weitere Informationen zum Aufbau einer vollständigen Sicherheitsroadmap finden Sie im Abschnitt „Customer responsibilities and roadmap“ (Zuständigkeiten des Kunden und Kundenroadmap) des Dokuments [Microsoft Cloud Security for Enterprise Architects](http://aka.ms/securecustomer) (Microsoft-Cloudsicherheit für Unternehmensarchitekten). Um weitere Informationen zum Einsatz von Microsoft-Diensten zur Unterstützung bei einem dieser Themen zu erhalten, wenden Sie sich an Ihren Microsoft-Vertriebsbeauftragten, oder besuchen Sie unsere Seite zu [Cybersecurity solutions](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx)(Cybersicherheitslösungen).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png



<!--HONumber=Dec16_HO2-->


