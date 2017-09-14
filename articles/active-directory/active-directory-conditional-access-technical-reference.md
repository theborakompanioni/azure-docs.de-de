---
title: Technische Referenz zum bedingten Azure Active Directory-Zugriff | Microsoft-Dokumentation
description: "Mit der bedingten Zugriffssteuerung überprüft Azure Active Directory die besonderen Bedingungen, die Sie beim Authentifizieren des Benutzers und vor dem Gewähren des Zugriffs auf die Anwendung auswählen. Nachdem diese Bedingungen erfüllt sind, wird der Benutzer authentifiziert und erhält Zugriff auf die Anwendung."
services: active-directory.
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/01/2017
ms.author: markvi
ms.reviewer: spunukol
ms.translationtype: HT
ms.sourcegitcommit: a16daa1f320516a771f32cf30fca6f823076aa96
ms.openlocfilehash: f3d8bdbfc29ca1008006837512c0e6ae8cb8f6fe
ms.contentlocale: de-de
ms.lasthandoff: 09/02/2017

---
# <a name="azure-active-directory-conditional-access-technical-reference"></a>Technische Referenz zum bedingten Azure Active Directory-Zugriff

Mit dem [bedingten Zugriff von Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) können Sie präzise steuern, wie autorisierte Benutzer auf Ihre Ressourcen zugreifen können.  
In diesem Thema erhalten Sie Supportinformationen für die folgenden Elemente einer Richtlinie für bedingten Zugriff: 

- Zuweisungen von Cloud-Apps

- Geräteplattformbedingung 

- Client-Apps-Bedingung

- Genehmigte Client-App als Voraussetzung 



## <a name="cloud-apps-assignments"></a>Zuweisungen von Cloud-Apps

Wenn Sie eine Richtlinie für bedingten Zugriff konfigurieren, müssen Sie die [Cloud-Apps auswählen, für die Ihre Richtlinie gilt](active-directory-conditional-access-azure-portal.md#who). 

![Kontrolle](./media/active-directory-conditional-access-technical-reference/09.png)


### <a name="microsoft-cloud-apps"></a>Cloud-Apps von Microsoft

Sie können folgenden Cloud-Apps von Microsoft eine Richtlinie für bedingten Zugriff zuweisen:

- Azure RemoteApp

- Dynamics CRM

- Microsoft Office 365 Yammer

- Microsoft Office 365 Exchange Online

- Microsoft Office 365 SharePoint Online (einschließlich OneDrive for Business)

- Microsoft Power BI 

- Visual Studio Team Services

- Microsoft Teams


### <a name="other-apps"></a>Andere Apps 

Außer den Cloud-Apps von Microsoft können Sie den folgenden Typen von Cloud-Apps eine Richtlinie für bedingten Zugriff zuweisen:

- Mit Azure Active Directory (Azure AD) verbundenen Anwendungen

- Vorab integrierten verbundenen Software-as-a-Service-Anwendungen (SaaS)

- Anwendungen, die einmaliges Anmelden mit Kennwort (SSO) verwenden

- Branchenanwendungen

- Anwendungen, die den Azure AD-Anwendungsproxy verwenden. 


## <a name="device-platforms-condition"></a>Geräteplattformbedingung

In einer Richtlinie zum bedingten Zugriff können Sie die Geräteplattformbedingung konfigurieren, um die Richtlinie mit dem Betriebssystem zu verknüpfen, das auf einem Client ausgeführt wird.

![Kontrolle](./media/active-directory-conditional-access-technical-reference/41.png)

Für den bedingten Azure AD-Zugriff werden folgende Geräteplattformen unterstützt:

- Android

- iOS

- Windows Phone

- Windows

- macOS (Vorschauversion)



## <a name="client-apps-condition"></a>Client-Apps-Bedingung 

Wenn Sie eine Richtlinie für bedingten Zugriff konfigurieren, können Sie eine [Client-Apps-Bedingung](active-directory-conditional-access-azure-portal.md#client-apps) festlegen. Mit der Client-Apps-Bedingung können Sie den Zugriff gewähren oder verweigern, wenn diese Typen von Client-Apps einen Zugriffsversuch unternehmen:

- "Browser"
- Mobile Apps und Desktop-Apps

![Kontrolle](./media/active-directory-conditional-access-technical-reference/03.png)

### <a name="supported-browsers"></a>Unterstützte Browser 

Bei Auswahl von *Browser* in Ihrer Richtlinie für bedingten Zugriff zum Gewähren des Zugriffs auf Ressourcen wird Zugriff nur gewährt, wenn der Zugriffsversuch über einen unterstützten Browser erfolgt. Wenn ein Zugriffsversuch mit einem nicht unterstützten Browser erfolgt, wird der Versuch blockiert.

![Unterstützte Browser](./media/active-directory-conditional-access-technical-reference/05.png)

In der Richtlinie für bedingten Zugriff werden die folgenden Browser unterstützt: 


| Betriebssystem                     | Browser                 | Support     |
| :--                    | :--                      | :-:         |
| Windows 10                 | IE, Edge                 | ![Prüfen][1] |
| Windows 10                 | Chrome                   | Vorschau     |
| Windows 8/8.1            | IE, Chrome               | ![Prüfen][1] |
| Windows 7                  | IE, Chrome               | ![Prüfen][1] |
| iOS                    | Safari                   | ![Prüfen][1] |
| Android                | Chrome                   | ![Prüfen][1] |
| Windows Phone          | IE, Edge                 | ![Prüfen][1] |
| Windows Server 2016    | IE, Edge                 | ![Prüfen][1] |
| Windows Server 2016    | Chrome                   | In Kürze verfügbar |
| Windows Server 2012 R2 | IE, Chrome               | ![Prüfen][1] |
| Windows Server 2008 R2 | IE, Chrome               | ![Prüfen][1] |
| Mac OS                 | Safari                   | ![Prüfen][1] |
| Mac OS                 | Chrome                   | In Kürze verfügbar |

> [!NOTE]
> Für die Unterstützung von Chrome müssen Sie Windows 10 Creators Update verwenden und die Erweiterung installieren, die Sie [hier](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji) finden.


### <a name="supported-mobile-apps-and-desktop-clients"></a>Unterstützte mobile Apps und Desktopclients

Bei Auswahl von **Mobile Apps und Desktopclients** in Ihrer Richtlinie für bedingten Zugriff zum Gewähren des Zugriffs auf Ressourcen wird Zugriff nur gewährt, wenn der Zugriffsversuch mit unterstützten mobilen Apps oder Desktopclients durchgeführt wurde. Bei Zugriffsversuchen mithilfe nicht unterstützter mobiler Apps oder Desktopclients wird der Versuch blockiert.

![Kontrolle](./media/active-directory-conditional-access-technical-reference/06.png)

Die folgenden mobilen Apps und Desktopclients unterstützen den bedingten Zugriff für Office 365 und andere mit Azure AD verbundene Dienstanwendungen:


| Client-Apps| Zieldienst| Plattform |
| --- | --- | --- |
| MFA- und Standort-Richtlinien für Apps Gerätebasierte Richtlinien werden nicht unterstützt.| Alle Meine Apps-App-Dienste| Android und iOS|
| Azure RemoteApp| Azure Remote-App-Dienst| Windows 10, Windows 8.1, Windows 7, iOS, Android, Mac OS X|
| Dynamics CRM-App| Dynamics CRM| Windows 10, Windows 8.1, Windows 7, iOS, Android|
| Microsoft Teams-Dienste: steuert alle Dienste, die Microsoft-Teams unterstützen, und alle dazugehörigen Client-Apps – Windows Desktop, iOS, Android, WP und Webclient| Microsoft Teams| Windows 10, Windows 8.1, Windows 7, iOS und Android|
| Mail-/Kalender-/Kontakte-App, Outlook 2016, Outlook 2013 (mit moderner Authentifizierung), Skype for Business (mit moderner Authentifizierung)| Microsoft Office 365 Exchange Online| Windows 10|
| Outlook 2016, Outlook 2013 (mit moderner Authentifizierung), Skype for Business (mit moderner Authentifizierung)| Microsoft Office 365 Exchange Online| Windows 8.1, Windows 7|
| Outlook Mobile-App| Microsoft Office 365 Exchange Online| iOS|
| Outlook 2016 (Office für macOS)| Microsoft Office 365 Exchange Online| Mac OS X|
| Office 2016-Apps, universelle Office-Apps, Office 2013 (mit moderner Authentifizierung), OneDrive-Synchronisierungsclient (siehe [Hinweise](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), zukünftige Unterstützung für Office-Gruppen und SharePoint-Apps ist geplant.| Office 365 SharePoint Online| Windows 10|
| Office 2016-Apps, Office 2013 (mit moderner Authentifizierung), OneDrive-Synchronisierungsclient (siehe [Hinweise](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))| Office 365 SharePoint Online| Windows 8.1, Windows 7|
| Office Mobile-Apps| Office 365 SharePoint Online| iOS, Android|
| Office 2016 für macOS (nur Word, Excel, PowerPoint und OneNote). Unterstützung von OneDrive for Business für die Zukunft geplant| Office 365 SharePoint Online| Mac OS X|
| Office Yammer-App| Office 365 Yammer| Windows 10, iOS und Android|
| PowerBI-App. Die PowerBI-App für Android unterstützt derzeit keinen gerätebasierten bedingten Zugriff.| PowerBI-Dienst| Windows 10, Windows 8.1, Windows 7 und iOS|
| Visual Studio Team Services-App| Visual Studio Team Services| Windows 10, Windows 8.1, Windows 7, iOS, Android|



## <a name="approved-client-app-requirement"></a>Genehmigte Client-App als Voraussetzung 

Beim Konfigurieren einer Richtlinie zum bedingten Zugriff können Sie angeben, dass der Zugriff nur gestattet werden soll, wenn die Verbindung über eine genehmigte Client-App hergestellt wird. 

![Kontrolle](./media/active-directory-conditional-access-technical-reference/21.png)

Genehmigte Client-Apps für diese Einstellung:

- Microsoft Excel

- Microsoft OneDrive

- Microsoft Outlook

- Microsoft OneNote

- Microsoft PowerPoint

- Microsoft SharePoint

- Microsoft Skype for Business

- Microsoft Teams

- Microsoft Visio

- Microsoft Word


**Hinweise:**

- Diese Apps unterstützen die mobile Anwendungsverwaltung (Mobile Application Management, MAM) von Microsoft Intune.

- Für diese Anforderung gilt Folgendes:

    - Sie unterstützt nur iOS und Android als [Geräteplattformbedingung](#device-platforms-condition). 

    - **Browser** wird als [Client-App-Bedingung](#supported-browsers) nicht unterstützt. 
    
    - Sie hat Vorrang vor der [Client-App-Bedingung](#supported-mobile-apps-and-desktop-clients) **Mobile Apps und Desktopclients** (sofern ausgewählt).  


## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über den bedingten Zugriff finden Sie unter [Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access-azure-portal.md).
- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) nach.



<!--Image references-->
[1]: ./media/active-directory-conditional-access-technical-reference/01.png



