---
title: Was ist der Zugriffsbereich in Azure Active Directory? | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Varianten des Zugriffsbereichs (Webbrowser, Android-App, iPhone- und iPad-App) zum Zugreifen auf SaaS-Apps verwenden.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: bd9066c251188c0f18fe1a9403baa2beaeeb987c
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="what-is-the-access-panel"></a>Was ist der Zugriffsbereich?

Der Zugriffsbereich ist ein webbasiertes Portal. Damit können Benutzer mit einem Geschäfts-, Schul- oder Unikonto in Azure Active Directory (Azure AD) cloudbasierte Anwendungen anzeigen und starten, für die ein Azure AD-Administrator ihnen Zugriff gewährt hat. Sie können über den Zugriffsbereich auch Self-Service-Funktionen für die Gruppen- und App-Verwaltung nutzen.

Der Zugriffsbereich ist unabhängig vom Azure-Portal und setzt nicht voraus, dass Sie über ein Azure-Abonnement verfügen.

![Anpassung des Zugriffsbereichs][1]

Der Zugriffsbereich ermöglicht es Ihnen, einige Ihrer Profileinstellungen zu bearbeiten. Folgende Aufgaben können ausgeführt werden:

- Ändern des Kennworts, das einem Geschäfts-, Schul- oder Unikonto zugeordnet ist.

- Bearbeiten der Einstellungen zum Zurücksetzen des Kennworts.

- Bearbeiten der Kontakt- und Voreinstellungen, die sich auf die Multi-Factor Authentication beziehen (für Konten, für die dies vom Administrator verlangt wird).

- Anzeigen von Kontodetails, z.B.: Benutzer-ID, alternative E-Mail-Adresse, Mobil- und geschäftliche Telefonnummern sowie Geräte.

- Anzeigen und Starten von cloudbasierten Anwendungen, für die der Azure AD-Administrator den Zugriff gewährt hat. Weitere Informationen zum Zugriffsbereich aus Sicht der Benutzer finden Sie unter „Verwenden des Zugriffsbereichs“. 

- Selbstständiges Verwalten von Gruppen. Der Administrator kann Sicherheitsgruppen erstellen und verwalten und die Mitgliedschaft in Sicherheitsgruppen in Azure AD anfordern. Weitere Informationen finden Sie unter [Self-Service-Gruppenverwaltung für Benutzer in Azure AD](active-directory-accessmanagement-self-service-group-management.md) und [Verwalten Ihrer Gruppen](active-directory-manage-groups.md).




## <a name="accessing-the-access-panel"></a>Zugreifen auf den Zugriffsbereich

Sie können auf den Zugriffsbereich zugreifen, indem Sie in einem Webbrowser folgende URL aufrufen: `http://myapps.microsoft.com`

Wenn Sie benutzerdefiniertes Branding für Ihre Anmeldeseite konfiguriert haben, können Sie dieses Branding laden, indem Sie die Domäne Ihrer Organisation am Ende der URL anfügen: `http://myapps.microsoft.com/<your domain>.com`

In diesem Fall können Sie jeden aktiven oder überprüften Domänennamen verwenden, der in Ihrem Azure-Portal konfiguriert wurde.

![Wingtip Toys-Domänenname][2]  

Sie müssen diese URL allen Benutzern mitteilen, die sich bei den in Azure AD integrierten Anwendungen anmelden.

## <a name="authentication"></a>Authentifizierung

Zum Erreichen des Zugriffsbereichs müssen Sie mit einem Geschäfts-, Schul- oder Unikonto in Azure AD authentifiziert sein. Sie können direkt bei Azure AD authentifiziert werden. Wenn Ihre Organisation mithilfe der Active Directory-Verbunddienste (AD FS) oder anderen Technologien einen Verbund konfiguriert hat, können Sie alternativ dazu auch über Windows Server Active Directory authentifiziert werden.

Wenn Sie über ein Abonnement für Azure oder Office 365 verfügen und das Azure-Portal oder eine Office 365-Anwendung verwendet haben, wird Ihnen die Liste mit den Anwendungen angezeigt, ohne dass eine erneute Anmeldung erforderlich ist. Falls Sie nicht authentifiziert sind, werden Sie aufgefordert, sich mit dem Benutzernamen und Kennwort für Ihr Konto in Azure AD anzumelden. Wenn Ihre Organisation einen Verbund konfiguriert hat, reicht die Eingabe des Benutzernamens aus.

Nach der Authentifizierung können Sie mit den Anwendungen interagieren, die Ihr Administrator in das Verzeichnis integriert hat. Informationen zum Integrieren von Anwendungen in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="web-browser-requirements"></a>Webbrowseranforderungen

Die Mindestanforderung für den Zugriffsbereich ist ein Browser mit JavaScript-Unterstützung und CSS-Aktivierung. Damit ein Benutzer sich per kennwortbasiertem einmaligem Anmelden bei Anwendungen anmelden kann, muss die Erweiterung für den Zugriffsbereich im Browser installiert sein. Die Erweiterung wird automatisch heruntergeladen, wenn Sie eine Anwendung auswählen, die für kennwortbasiertes einmaliges Anmelden konfiguriert ist.

Die Zugriffsbereichserweiterung ist derzeit für die Browser Internet Explorer 8 oder höher, Edge, Chrome und Firefox verfügbar.

## <a name="mobile-app-support"></a>Unterstützung für mobile Apps

Das Azure Active Directory-Team veröffentlicht die mobile App „Meine Apps“. Wenn Sie diese App installieren, können Sie sich auf iOS- und Android-Geräten bei Anwendungen anmelden, für die kennwortbasiertes einmaliges Anmelden genutzt wird.

> [!NOTE]
> Sie können sich auf nahezu jedem Gerät in praktisch jedem Webbrowser bei Anwendungen anmelden, die den Verbund mit Azure AD unterstützen (z.B. Salesforce, Google Apps, Dropbox, Box, Concur, Workday, Office 365 und mehr als 70 weitere) – ohne dass ein Plug-In oder eine mobile App erforderlich ist. Auch für alle anderen [Funktionen im Zugriffsbereich](https://myapps.microsoft.com/) muss die mobile App „Meine Apps“ auf einem mobilen Gerät nicht verwendet werden.
>
>

### <a name="my-apps-for-android"></a>„Meine Apps“ für Android

„Meine Apps“ für Android wird auf allen Android-Geräten unterstützt, auf denen Android-Version 4.1 oder höher ausgeführt wird.  
Die App ist im [Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.myapps) erhältlich.

![„Meine Apps“ für Android][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>„Meine Apps“ für iPhone und iPad

„Meine Apps“ für iOS wird auf allen iPhone- oder iPad-Geräten mit iOS-Version 7 oder höher unterstützt.  
Die App ist im [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8) erhältlich.

![„Meine Apps“ für iOS][4]    



## <a name="managed-browser-for-my-apps"></a>Managed Browser für „Meine Apps“

„Meine Apps“ ist auch in den Intune Managed Browser integriert. Der Intune Managed Browser für iOS- und Android-Geräte spielt eine wichtige Rolle, um zu gewährleisten, dass Daten auf mobilen Geräten sicher bleiben. Er bietet eine geschützte Webbrowserumgebung, in der Sie Webseiten mit Unternehmensinformationen sicher anzeigen und darin navigieren können.  
Schnellen Zugriff auf „Meine Apps“ erhalten Sie auf Ihrer Managed Browser-Startseite und über Ihre Favoriten, sodass Sie jede Anwendung mit weniger Klicks erreichen können.

Der Browser ist im [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) und im [Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en) erhältlich.

![Managed Browser für „Meine Apps“][5]    





## <a name="tips-for-testing-the-user-experience"></a>Tipps zum Testen der Benutzerfunktionen

Wenn Sie Azure-Administrator und mit einem Konto im Verzeichnis beim Azure-Portal angemeldet sind, werden Sie automatisch mit dem aktuellen Konto beim Zugriffsbereich angemeldet. In diesem Fall werden Ihnen alle Anwendungen angezeigt, die Ihnen zugewiesen wurden.

**Gehen Sie wie folgt vor, um ein *anderes* Benutzerkonto zu testen:**

1. Klicken Sie in der oberen rechten Ecke des Azure-Portals oder des Zugriffsbereichs auf das Benutzermenü, und wählen Sie **Abmelden**. 
2. Wechseln Sie zum [Zugriffsbereich](http://myapps.microsoft.com).
3. Geben Sie auf der Anmeldeseite den Benutzernamen und das Kennwort für das Konto in Ihrem Verzeichnis ein, das Sie testen möchten.


## <a name="starting-applications"></a>Starten von Anwendungen

Im Zugriffsbereich können verschiedene Arten von Anwendungen angezeigt werden.

### <a name="office-365-applications"></a>Office 365-Anwendungen

Wenn Ihre Organisation Office 365-Anwendungen verwendet und Sie für diese lizenziert sind, werden die Office 365-Anwendungen in Ihrem Zugriffsbereich angezeigt.

Wenn Sie auf eine Anwendungskachel für eine Office 365-Anwendung klickt, werden Sie an diese Anwendung weitergeleitet und automatisch angemeldet.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Anwendungen von Microsoft und Drittanbietern, für die verbundbasiertes SSO konfiguriert ist

Ihr Administrator kann im Abschnitt „Active Directory“ des Azure-Portals Anwendungen mit der SSO-Moduseinstellung **Azure AD – einmaliges Anmelden** hinzufügen. Ihnen werden diese Anwendungen nur angezeigt, wenn Ihr Administrator Ihnen explizit Zugriff auf die Anwendungen gewährt hat.

Wenn Sie auf eine Kachel für eine dieser Anwendungen klicken, werden Sie an die betreffende Anwendung weitergeleitet und automatisch angemeldet.

### <a name="password-based-sso-without-identity-provisioning"></a>Kennwortbasiertes SSO ohne Identitätsbereitstellung

Ihr Administrator kann im Abschnitt „Active Directory“ des Azure-Portals Anwendungen mit der SSO-Moduseinstellung **Kennwortbasiertes einmaliges Anmelden** hinzufügen. Alle Benutzer im Verzeichnis können alle Anwendungen anzeigen, die in diesem Modus konfiguriert wurden.

Wenn Sie zum ersten Mal auf eine Kachel für eine dieser Anwendungen klicken, werden Sie aufgefordert, das Kennwort-SSO-Plug-In für Internet Explorer oder Chrome zu installieren. Für die Installation müssen Sie den Webbrowser möglicherweise neu starten. Wenn Sie in den Zugriffsbereich zurückkehren und erneut auf die Anwendungskachel klicken, werden Sie zum Eingeben eines Benutzernamens und Kennworts für die Anwendung aufgefordert. Nachdem Sie Ihren Benutzernamen und Ihr Kennwort eingegeben haben, werden diese Anmeldeinformationen in Azure AD sicher gespeichert und mit dem Konto verknüpft.

Wenn Sie das nächste Mal auf die Anwendungskachel klicken, werden Sie automatisch bei der Anwendung angemeldet.  
Sie müssen die Anmeldeinformationen nicht erneut eingeben und auch das Kennwort-SSO-Plug-In nicht erneut installieren.

Wenn sich Ihre Anmeldeinformationen in der Drittanbieteranwendung geändert haben, müssen Sie Ihre in Azure AD gespeicherten Anmeldeinformationen aktualisieren. 

**So aktualisieren Sie Anmeldeinformationen:**

1. Wählen Sie das Symbol auf der Anwendungskachel.
2. Wählen Sie **Anmeldeinformationen aktualisieren**, und geben Sie Ihren Benutzernamen und Ihr Kennwort für die Anwendung erneut ein.


### <a name="password-based-sso-with-identity-provisioning"></a>Kennwortbasiertes SSO mit Identitätsbereitstellung

Ihr Administrator kann im Abschnitt **Active Directory** des Azure-Portals Anwendungen mit der SSO-Moduseinstellung **Kennwortbasiertes einmaliges Anmelden** und zusätzlich die Identitätsbereitstellung hinzufügen.

Wenn Sie zum ersten Mal auf eine Kachel für eine dieser Anwendungen klicken, werden Sie aufgefordert, das **Kennwort-SSO-Plug-In für Internet Explorer oder Chrome** zu installieren. Für die Installation müssen Sie den Webbrowser möglicherweise neu starten.  
Wenn Sie zum Zugriffsbereich zurückkehren und erneut auf die Anwendungskachel klicken, werden Sie automatisch bei der Anwendung angemeldet.

Bei einigen Anwendungen kann es erforderlich sein, dass Sie Ihr Kennwort bei der ersten Anmeldung ändern. Wenn sich Ihre Anmeldeinformationen in der Drittanbieteranwendung geändert haben, müssen Sie die in Azure AD gespeicherten Anmeldeinformationen aktualisieren. 

**So aktualisieren Sie Anmeldeinformationen:**

1. Wählen Sie das Symbol auf der Anwendungskachel.
2. Wählen Sie **Anmeldeinformationen aktualisieren**, und geben Sie Ihren Benutzernamen und Ihr Kennwort für die Anwendung erneut ein.


### <a name="application-with-existing-sso-solutions"></a>Anwendung mit vorhandenen SSO-Lösungen

Zum Konfigurieren des einmaligen Anmeldens für eine Anwendung bietet das Azure-Portal als dritte Möglichkeit die Option **Vorhandenes einmaliges Anmelden**. Diese Option ermöglicht es Ihrem Administrator, eine Verknüpfung mit einer Anwendung zu erstellen und sie für ausgewählte Benutzer im Zugriffsbereich zu platzieren.

Wenn eine Anwendung beispielsweise für die Benutzerauthentifizierung per AD FS 2.0 konfiguriert ist, kann Ihr Administrator die Option **Vorhandenes einmaliges Anmelden** verwenden, um im Zugriffsbereich eine entsprechende Verknüpfung dafür zu erstellen. Wenn Sie die Verknüpfung verwenden, werden Sie per AD FS 2.0 oder über die SSO-Lösung authentifiziert, die von der Anwendung bereitgestellt wird.


## <a name="next-steps"></a>Nächste Schritte

- Eine Liste aller Themen in Zusammenhang mit der Anwendungsverwaltung finden Sie im [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md).
 
- Informationen zum Integrieren einer SaaS-App in Azure AD finden Sie in der [Liste der Tutorials zur Integration von SaaS-Apps](active-directory-saas-tutorial-list.md).
 
- Informationen zum Verwalten von Apps in Azure AD finden Sie in der [Einführung in das einmalige Anmelden und das Verwalten von App-Zugriff mit Azure Active Directory](active-directory-appssoaccess-whatis.md).
 
- Weitere Informationen zur Benutzerbereitstellung finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen](active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png

