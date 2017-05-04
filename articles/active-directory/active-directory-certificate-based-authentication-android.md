---
title: Zertifikatbasierte Authentifizierung mit Azure Active Directory unter Android | Microsoft-Dokumentation
description: "In diesem Thema lernen Sie unterstützte Szenarien sowie die Voraussetzungen für die Konfiguration der zertifikatbasierten Authentifizierung in Lösungen mit Android-Geräten kennen."
services: active-directory
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: c6ad7640-8172-4541-9255-770f39ecce0e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 0e2bbb2a94986ca93035aa553db0f9b54af635d5
ms.lasthandoff: 04/27/2017


---
# <a name="azure-active-directory-certificate-based-authentication-on-android"></a>Zertifikatbasierte Authentifizierung mit Azure Active Directory unter Android


Bei Verwendung der zertifikatbasierten Authentifizierung können Sie von Azure Active Directory mit einem Clientzertifikat auf einem Windows-, Android- oder iOS-Gerät authentifiziert werden, wenn Sie Ihr Exchange Online-Konto mit folgenden Anwendungen oder Clients verbinden: 

* Mobile Office-Anwendungen wie Microsoft Outlook und Microsoft Word   
* Exchange ActiveSync-Clients (EAS) 

Wenn Sie dieses Feature konfigurieren, ist es bei bestimmten E-Mail- und Microsoft Office-Anwendungen auf Ihrem mobilen Gerät nicht länger erforderlich, einen Benutzernamen und ein Kennwort einzugeben. 

Dieses Thema behandelt die Voraussetzungen und die unterstützten Szenarien für die Konfiguration der zertifikatbasierten Authentifizierung auf Android-Geräten für Mandantenbenutzer mit Office 365 Enterprise-, Business-, Education- und US Government-Plänen. 

Dieses Feature ist als Vorversion in Office 365 US Government Defense- und Federal-Plänen verfügbar.


## <a name="office-mobile-applications-support"></a>Unterstützung mobiler Office-Anwendungen
| Apps | Support |
| --- | --- |
| Word/Excel/PowerPoint |![Prüfen][1] |
| OneNote |![Prüfen][1] |
| OneDrive |![Prüfen][1] |
| Outlook |![Prüfen][1] |
| Yammer |![Prüfen][1] |
| Skype for Business |![Prüfen][1] |
| Azure Information Protection-App |![Prüfen][1] |


### <a name="implementation-requirements"></a>Implementierungsanforderungen

Die Betriebssystemversion des Geräts muss Android 5.0 (Lollipop) oder eine höhere Version sein. 

Ein Verbundserver muss konfiguriert werden.  

Damit Azure Active Directory ein Clientzertifikat sperren kann, muss das AD FS-Token die folgenden Ansprüche enthalten:  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  (Die Seriennummer des Clientzertifikat) 
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  (Die Zeichenfolge für den Aussteller des Clientzertifikats) 

Wenn diese Ansprüche im AD FS-Token (oder in einem anderen SAML-Token) enthalten sind, fügt Azure Active Directory die Ansprüche dem Aktualisierungstoken hinzu. Wenn das Aktualisierungstoken überprüft werden muss, werden diese Informationen zum Überprüfen der Sperrung verwendet. 

Als bewährte Methode wird empfohlen, die AD FS-Fehlerseiten mit Anleitungen zum Abrufen eines Benutzerzertifikats zu aktualisieren.  
Weitere Einzelheiten finden Sie unter [Anpassen der AD FS-Anmeldeseiten](https://technet.microsoft.com/library/dn280950.aspx).  

Einige Office-Apps (mit aktivierter moderner Authentifizierung) senden „*prompt=login*“ in der Anforderung an Azure AD. Azure AD übersetzt dies in der Anforderung standardmäßig in „*wauth=usernamepassworduri*“ (fordert AD FS zum Durchführen der U/P-Authentifizierung auf) und „*wfresh = 0*“ (fordert AD FS auf, den SSO-Status zu ignorieren und eine erneute Authentifizierung durchzuführen). Wenn Sie eine zertifikatbasierte Authentifizierung für diese Apps aktivieren möchten, müssen Sie das Azure AD-Standardverhalten ändern. Legen Sie dazu einfach „*PromptLoginBehavior*“ in den Einstellungen der Verbunddomäne auf „*deaktiviert*“ fest. Für diese Aufgabe können Sie das Cmdlet [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) verwenden:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`



## <a name="exchange-activesync-clients-support"></a>Unterstützung von Exchange ActiveSync-Clients
Bestimmte Exchange ActiveSync-Anwendungen auf Android 5.0 (Lollipop) oder einer höheren Version werden unterstützt. Wenden Sie sich an Ihren Anwendungsentwickler, um herauszufinden, ob Ihre E-Mail-Anwendung dieses Feature unterstützt. 


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die zertifikatbasierte Authentifizierung in Ihrer Umgebung konfigurieren möchten, finden Sie unter [Erste Schritte mit der zertifikatbasierten Authentifizierung unter Android](active-directory-certificate-based-authentication-get-started.md) entsprechende Anweisungen.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png

