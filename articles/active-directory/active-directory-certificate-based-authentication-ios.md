---
title: Zertifikatbasierte Authentifizierung mit Azure Active Directory unter iOS | Microsoft-Dokumentation
description: "In diesem Thema lernen Sie unterstützte Szenarien sowie die Voraussetzungen für die Konfiguration der zertifikatbasierten Authentifizierung in Lösungen mit iOS-Geräten kennen."
services: active-directory
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: 26a6fc54-0153-44fb-b970-9b432c99e9f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 7c28fda22a08ea40b15cf69351e1b0aff6bd0a95
ms.openlocfilehash: 28c2ef4878305d5cdb37ab8c0802644b1f236337
ms.lasthandoff: 03/07/2017


---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a>Zertifikatbasierte Authentifizierung mit Azure Active Directory unter iOS

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

## <a name="requirements"></a>Anforderungen 

Die Betriebssystemversion des Geräts muss iOS 9 oder eine höhere Version sein. 

Ein Verbundserver muss konfiguriert werden.  

Für Office-Anwendungen unter iOS ist Microsoft Authenticator erforderlich.  

Damit Azure Active Directory ein Clientzertifikat sperren kann, muss das AD FS-Token die folgenden Ansprüche enthalten:  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  (Die Seriennummer des Clientzertifikat) 
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  (Die Zeichenfolge für den Aussteller des Clientzertifikats) 

Wenn diese Ansprüche im AD FS-Token (oder in einem anderen SAML-Token) enthalten sind, fügt Azure Active Directory die Ansprüche dem Aktualisierungstoken hinzu. Wenn das Aktualisierungstoken überprüft werden muss, werden diese Informationen zum Überprüfen der Sperrung verwendet. 

Als bewährte Methode sollten Sie die AD FS-Fehlerseiten mit folgenden Informationen aktualisieren:

* Die Voraussetzungen für die Installation von Microsoft Authenticator unter iOS
* Anleitungen zum Abrufen eines Benutzerzertifikats 

Weitere Einzelheiten finden Sie unter [Anpassen der AD FS-Anmeldeseiten](https://technet.microsoft.com/library/dn280950.aspx).

Einige Office-Apps (mit aktivierter moderner Authentifizierung) senden „*prompt=login*“ in der Anforderung an Azure AD. Azure AD übersetzt dies in der Anforderung standardmäßig in „*wauth=usernamepassworduri*“ (fordert AD FS zum Durchführen der U/P-Authentifizierung auf) und „*wfresh =&0;*“ (fordert AD FS auf, den SSO-Status zu ignorieren und eine erneute Authentifizierung durchzuführen). Wenn Sie eine zertifikatbasierte Authentifizierung für diese Apps aktivieren möchten, müssen Sie das Azure AD-Standardverhalten ändern. Legen Sie dazu einfach „*PromptLoginBehavior*“ in den Einstellungen der Verbunddomäne auf „*deaktiviert*“ fest. Für diese Aufgabe können Sie das Cmdlet [MSOLDomainFederationSettings](https://docs.microsoft.com/en-us/powershell/msonline/v1/set-msoldomainfederationsettings) verwenden:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`
  

## <a name="exchange-activesync-clients-support"></a>Unterstützung von Exchange ActiveSync-Clients
Unter iOS 9 oder höher wird der native iOS-E-Mail-Client unterstützt. Wenden Sie sich bei allen anderen Exchange ActiveSync-Anwendungen an den Anwendungsentwickler, um zu erfragen, ob dieses Feature unterstützt wird.  


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die zertifikatbasierte Authentifizierung in Ihrer Umgebung konfigurieren möchten, finden Sie unter [Erste Schritte mit der zertifikatbasierten Authentifizierung unter Android](active-directory-certificate-based-authentication-get-started.md) entsprechende Anweisungen.


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png

