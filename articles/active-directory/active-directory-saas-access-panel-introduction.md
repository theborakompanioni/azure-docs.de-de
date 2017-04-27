---
title: "Einführung in den Zugriffsbereich | Microsoft Docs"
description: Es wird beschrieben, wie Sie Varianten des Zugriffsbereichs (Webbrowser, Android-App, iPhone- und iPad-App) zum Zugreifen auf SaaS-Apps verwenden.
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
ms.date: 02/27/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: c5a0f3f958d42d514a95477c1e39f02c78307d4d
ms.lasthandoff: 04/06/2017


---
# <a name="what-is-the-access-panel"></a>Was ist der Zugriffsbereich?
Der Zugriffsbereich ist ein webbasiertes Portal. Damit können Benutzer, die in Azure Active Directory (Azure AD) über ein Geschäfts-, Schul- oder Unikonto verfügen, cloudbasierte Anwendungen anzeigen und starten, für die der Azure AD-Administrator ihnen Zugriff gewährt hat. Ein Benutzer, der über Azure AD-Editionen verfügt, kann über den Zugriffsbereich auch die Funktionen für die Self-Service-Gruppenverwaltung nutzen.

Der Zugriffsbereich ist unabhängig vom Azure-Portal und setzt nicht voraus, dass Benutzer über ein Azure-Abonnement verfügen.

![Anpassung des Zugriffsbereichs][1]

Der Zugriffsbereich ermöglicht es Benutzern, einige ihrer Profileinstellungen zu bearbeiten. Folgende Aufgaben können ausgeführt werden:

* Ändern des Kennworts, das einem Geschäfts-, Schul- oder Unikonto zugeordnet ist.
* Bearbeiten der Einstellungen zum Zurücksetzen des Kennworts.
* Bearbeiten der Kontakt- und Voreinstellungen, die sich auf die Multi-Factor Authentication beziehen (für Konten, für die dies vom Administrator verlangt wird).
* Anzeigen von Kontodetails, z.B. Benutzer-ID, alternative E-Mail-Adresse und Mobil- und geschäftliche Telefonnummern.
* Anzeigen und Starten von cloudbasierten Anwendungen, für die der Azure AD-Administrator den Zugriff gewährt hat. Informationen zum Zugriffsbereich aus Sicht der Benutzer finden Sie unter [Verwenden des Zugriffsbereichs](https://msdn.microsoft.com/library/azure/dn756411.aspx).
* Selbstständiges Verwalten von Gruppen. Der Administrator kann Sicherheitsgruppen erstellen und verwalten und die Mitgliedschaft in Sicherheitsgruppen in Azure AD anfordern. Weitere Informationen finden Sie unter [Self-Service-Gruppenverwaltung für Benutzer in Azure AD](active-directory-accessmanagement-self-service-group-management.md) und [Verwalten Ihrer Gruppen](active-directory-manage-groups.md).

## <a name="accessing-the-access-panel"></a>Zugreifen auf den Zugriffsbereich
Benutzer greifen auf den Zugriffsbereich zu, indem sie in einem Webbrowser die folgende URL verwenden:  <br>
**http://myapps.microsoft.com**

Wenn Sie benutzerdefiniertes Branding für Ihre Anmeldeseite konfiguriert haben, können Sie dieses Branding standardmäßig laden, indem Sie die Domäne Ihrer Organisation am Ende der URL anfügen  <br>
**http://myapps.microsoft.com/contosobuild.com**

In diesem Fall können Sie jeden aktiven oder überprüften Domänennamen verwenden, der auf der Registerkarte **Domänen** Ihres Verzeichnisses im Azure-Portal konfiguriert wurde. Dies ist im folgenden Screenshot dargestellt:

![Wingtip Toys-Domänenname][2]  

Diese URL muss allen Benutzern mitgeteilt werden, die sich an den in Azure AD integrierten Anwendungen anmelden.

## <a name="authentication"></a>Authentifizierung
Zum Erreichen des Zugriffsbereichs muss ein Benutzer mit einem Geschäfts-, Schul- oder Unikonto in Azure AD authentifiziert werden. Ein Benutzer kann direkt bei Azure AD authentifiziert sein. Wenn eine Organisation mithilfe von Active Directory-Verbunddienste (AD FS) oder anderen Technologien einen Verbund konfiguriert hat, können Benutzer alternativ dazu auch über Windows Server Active Directory authentifiziert werden.

Falls ein Benutzer über ein Abonnement für Azure oder Office 365 verfügt und das Azure-Portal oder eine Office 365-Anwendung verwendet hat, wird ihm die Liste mit den Anwendungen angezeigt, ohne dass eine erneute Anmeldung erforderlich ist. Benutzer, die nicht authentifiziert sind, werden aufgefordert, sich mit dem Benutzernamen und Kennwort für ihr Konto in Azure AD anzumelden. Wenn die Organisation einen Verbund konfiguriert hat, reicht die Eingabe des Benutzernamens aus.

Nach der Authentifizierung können Benutzer mit den Anwendungen interagieren, die der Administrator in das Verzeichnis integriert hat. Informationen zum Integrieren von Anwendungen in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="web-browser-requirements"></a>Webbrowseranforderungen
Die Mindestanforderung für den Zugriffsbereich ist ein Browser mit Unterstützung von JavaScript und CSS-Aktivierung. Damit der Benutzer bei Anwendungen mithilfe von kennwortbasiertem SSO (Single Sign-On, einmaliges Anmelden) angemeldet werden kann, muss die Erweiterung für den Zugriffsbereich im Browser des Benutzers installiert sein. Diese Erweiterung wird automatisch heruntergeladen, wenn ein Benutzer eine Anwendung auswählt, die für kennwortbasiertes SSO konfiguriert ist.

Die Zugriffsbereichserweiterung ist derzeit für die Browser Internet Explorer 8 oder höher, Edge, Chrome und Firefox verfügbar.

## <a name="mobile-app-support"></a>Unterstützung für mobile Apps
Vom Azure Active Directory-Team wird die mobile App „Meine Apps“ veröffentlicht. Wenn diese App von Benutzern installiert wird, können sie sich auf iOS- und Android-Geräten an Anwendungen anmelden, für die kennwortbasiertes SSO genutzt wird.

> [!NOTE]
> Benutzer können sich an Anwendungen anmelden, die den Verbund mit Azure AD unterstützen (z.B. Salesforce, Google Apps, Dropbox, Box, Concur, Workday, Office 365 und mehr als 70 weitere), indem sie nahezu jeden Webbrowser auf beliebigen Geräten verwenden, ohne dass ein Plug-In oder eine mobile App erforderlich ist. Für den restlichen [Zugriffsbereich](https://myapps.microsoft.com/) ist es ebenfalls nicht erforderlich, die mobile App „Meine Apps“ auf einem mobilen Gerät zu verwenden.
>
>

### <a name="my-apps-for-android"></a>"Meine Apps" für Android
„Meine Apps“ für Android wird auf allen Android-Geräten unterstützt, auf denen Android-Version 4.1 oder höher ausgeführt wird. Die App ist im [Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.myapps) erhältlich.

![„Meine Apps“ für Android – Bildschirm][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>"Meine Apps" für iPhone und iPad
„Meine Apps“ für iOS wird auf allen iPhone- oder iPad-Geräten mit iOS-Version 7 oder höher unterstützt. Die App ist im App Store von Apple erhältlich.

![„Meine Apps“ für iOS – Bildschirm][4]    

## <a name="tips-for-testing-the-user-experience"></a>Tipps zum Testen der Benutzerfunktionen
Wenn Sie Azure-Administrator und mit einem Konto im Verzeichnis am Azure-Portal angemeldet sind, werden Sie automatisch mit dem aktuellen Administratorkonto beim Zugriffsbereich angemeldet. In diesem Fall werden Ihnen alle Anwendungen angezeigt, die diesem Konto zugewiesen wurden.

Gehen Sie wie folgt vor, um ein *anderes* Benutzerkonto zu testen:

1. Klicken Sie in der oberen rechten Ecke des Azure-Portals oder des Zugriffsbereichs auf das Benutzermenü, und wählen Sie **Abmelden**. Sie werden von Azure AD abgemeldet.
2. Wechseln Sie zum [Zugriffsbereich](http://myapps.microsoft.com).
3. Geben Sie auf der Anmeldeseite den Benutzernamen und das Kennwort für das Konto in Ihrem Verzeichnis ein, das Sie testen möchten.

## <a name="starting-applications"></a>Starten von Anwendungen
Im Zugriffsbereich können mehrere Arten von Anwendungen angezeigt werden.

### <a name="office-365-applications"></a>Office 365-Anwendungen
Wenn eine Organisation Office 365-Anwendungen verwendet und der Benutzer für diese lizenziert ist, werden die Office 365-Anwendungen im Zugriffsbereich des Benutzers angezeigt.

Wenn ein Benutzer auf eine Anwendungskachel für eine Office 365-Anwendung klickt, wird er an diese Anwendung weitergeleitet und automatisch angemeldet.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Anwendungen von Microsoft und Drittanbietern, für die verbundbasiertes SSO konfiguriert ist
Der Administrator kann im Abschnitt „Active Directory“ des Azure-Portals Anwendungen mit der SSO-Moduseinstellung **Azure AD – einmaliges Anmelden** hinzufügen. Einem Benutzer werden diese Anwendungen nur angezeigt, wenn der Administrator diesem Benutzer explizit Zugriff auf die Anwendungen gewährt hat.

Wenn ein Benutzer auf eine Kachel für eine dieser Anwendungen klickt, wird er an die betreffende Anwendung weitergeleitet und automatisch angemeldet.

### <a name="password-based-sso-without-identity-provisioning"></a>Kennwortbasiertes SSO ohne Identitätsbereitstellung
Der Administrator kann im Abschnitt „Active Directory“ des Azure-Portals Anwendungen mit der SSO-Moduseinstellung **Kennwortbasiertes einmaliges Anmelden** hinzufügen. Alle Benutzer im Verzeichnis sehen alle Anwendungen, die in diesem Modus konfiguriert wurden.

Wenn ein Benutzer zum ersten Mal auf eine Kachel für eine dieser Anwendungen klickt, wird er aufgefordert, das Kennwort-SSO-Plug-In für Internet Explorer oder Chrome zu installieren. Für die Installation ist es unter Umständen erforderlich, dass der Benutzer den Webbrowser neu startet. Wenn der Benutzer in den Zugriffsbereich zurückkehrt und erneut auf die Anwendungskachel klickt, wird er zum Eingeben eines Benutzernamens und Kennworts für die Anwendung aufgefordert. Nachdem der Benutzer den Benutzernamen und das Kennwort eingegeben hat, werden diese Anmeldeinformationen in Azure AD sicher gespeichert und mit dem Konto in Azure AD verknüpft.

Wenn der Benutzer dann das nächste Mal auf die Anwendungskachel klickt, wird er automatisch angemeldet. Der Benutzer muss die Anmeldeinformationen nicht erneut eingeben und auch das Kennwort-SSO-Plug-In nicht erneut installieren.

Wenn sich die Anmeldeinformationen in der Drittanbieter-Anwendung geändert haben, muss der Benutzer seine in Azure AD gespeicherten Anmeldeinformationen aktualisieren. Zum Aktualisieren der Anmeldeinformationen wählt der Benutzer das Symbol auf der Anwendungskachel aus und wählt dann die Option **Anmeldeinformationen aktualisieren**, um den Benutzernamen und das Kennwort für die Anwendung erneut einzugeben.

### <a name="password-based-sso-with-identity-provisioning"></a>Kennwortbasiertes SSO mit Identitätsbereitstellung
Der Administrator kann im Abschnitt „Active Directory“ des Azure-Portals Anwendungen mit der SSO-Moduseinstellung **Kennwortbasiertes einmaliges Anmelden** und zusätzlich die Identitätsbereitstellung hinzufügen.

Wenn ein Benutzer zum ersten Mal auf eine Anwendungskachel für eine dieser Anwendungen klickt, wird er aufgefordert, das Kennwort-SSO-Plug-In für Internet Explorer oder Chrome zu installieren. Für die Installation ist es unter Umständen erforderlich, dass der Benutzer den Webbrowser neu startet. Wenn der Benutzer in den Zugriffsbereich zurückkehrt und erneut auf die Anwendungskachel klickt, wird er automatisch an der Anwendung angemeldet.

Bei einigen Anwendungen kann es erforderlich sein, dass der Benutzer sein Kennwort bei der ersten Anmeldung ändert. Wenn sich die Anmeldeinformationen in der Drittanbieter-Anwendung geändert haben, muss der Benutzer seine in Azure AD gespeicherten Anmeldeinformationen aktualisieren. Zum Aktualisieren der Anmeldeinformationen wählt der Benutzer das Symbol auf der Anwendungskachel aus und wählt dann die Option **Anmeldeinformationen aktualisieren**, um den Benutzernamen und das Kennwort für die Anwendung erneut einzugeben.

### <a name="application-with-existing-sso-solutions"></a>Anwendung mit vorhandenen SSO-Lösungen
Zum Konfigurieren des einmaligen Anmeldens für eine Anwendung bietet das Azure-Portal als dritte Möglichkeit die Option **Vorhandenes einmaliges Anmelden**. Diese Option ermöglicht es dem Administrator, eine Verknüpfung zu einer Anwendung zu erstellen und sie für ausgewählte Benutzer im Zugriffsbereich zu platzieren.

Wenn eine Anwendung beispielsweise für die Authentifizierung von Benutzern per AD FS 2.0 konfiguriert ist, kann der Administrator die Option **Vorhandenes einmaliges Anmelden** verwenden, um im Zugriffsbereich eine entsprechende Verknüpfung dafür einzurichten. Wenn Benutzer die Verknüpfung verwenden, werden sie mit AD FS 2.0 bzw. der jeweiligen vorhandenen SSO-Lösung der Anwendung authentifiziert.

## <a name="related-articles"></a>Verwandte Artikel
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Liste der Tutorials zur Integration von SaaS-Apps](active-directory-saas-tutorial-list.md)
* [Einführung in das einmalige Anmelden und das Verwalten von App-Zugriff mit Azure Active Directory](active-directory-appssoaccess-whatis.md)
* [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen](active-directory-saas-app-provisioning.md)

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/ic767166.png
[2]: ./media/active-directory-saas-access-panel-introduction/ic767167.png
[3]: ./media/active-directory-saas-access-panel-introduction/ic767168.png
[4]: ./media/active-directory-saas-access-panel-introduction/ic767169.png

