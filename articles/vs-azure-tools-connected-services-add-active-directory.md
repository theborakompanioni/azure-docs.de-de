---
title: "Hinzufügen von Azure Active Directory mithilfe von verbundenen Diensten in Visual Studio | Microsoft Docs"
description: "Fügen Sie Azure Active Directory mithilfe des Dialogfelds &quot;Verbundene Dienste hinzufügen&quot; in Visual Studio hinzu"
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: bf3274f69cae8ec463fd158178394c7ba662ae75


---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Hinzufügen von Azure Active Directory mithilfe von verbundenen Diensten in Visual Studio
## <a name="overview"></a>Übersicht
Mithilfe von Azure Active Directory (Azure AD) können Sie einmaliges Anmelden (Single Sign-On, SSO) für ASP.NET MVC-Webanwendungen oder AD-Authentifizierung in Web-API-Diensten unterstützen. Mit der Azure AD-Authentifizierung können Benutzer ihre Konten in Azure AD verwenden, um eine Verbindung mit Webanwendungen herzustellen. Die Vorteile der Azure AD-Authentifizierung mit Web-API sind u. a. eine verbesserte Sicherheit, wenn eine API über eine Webanwendung verfügbar gemacht wird. Mit Azure AD benötigen Sie kein separates Authentifizierungssystem mit eigenem Konto und Benutzerverwaltung.

## <a name="supported-project-types"></a>Unterstützte Projekttypen
Mit dem Dialogfeld "Verbundene Dienste" können Sie in den folgenden Projekttypen eine Verbindung mit Azure AD herstellen.

* ASP.NET MVC-Projekte
* ASP.NET-Web-API-Projekte

### <a name="connect-to-azure-ad-using-the-connected-services-dialog"></a>Herstellen einer Verbindung mit Azure AD mithilfe des Dialogfelds "Verbundene Dienste"
1. Stellen Sie sicher, dass Sie über ein Azure-Konto verfügen. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](http://go.microsoft.com/fwlink/?LinkId=518146)registrieren.
2. Öffnen Sie in Visual Studio das Kontextmenü für den Knoten **Verweise** in Ihrem Projekt, und wählen Sie **Verbundene Dienste hinzufügen** aus.
3. Wählen Sie **Azure AD-Authentifizierung** und dann **Konfigurieren** aus.
   
    ![Wählen Sie "Azure AD-Authentifizierung hinzufügen" aus](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)
4. Aktivieren Sie auf der ersten Seite von **Azure AD-Authentifizierung konfigurieren** das Kontrollkästchen **Einmaliges Anmelden mit Azure AD konfigurieren**.
   
    Wenn Ihr Projekt mit einer anderen Authentifizierungskonfiguration konfiguriert ist, warnt Sie der Assistent, dass die vorherige Konfiguration deaktiviert wird, wenn Sie fortfahren.
   
    ![Konfigurieren Sie Azure AD im Assistenten](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)
5. Wählen Sie auf der zweiten Seite eine Domäne aus der Dropdownliste **Domäne** aus. Die Liste der Domänen enthält alle Domänen, auf die mit den im Dialogfeld "Kontoeinstellungen" aufgeführten Konten zugegriffen werden kann. Alternativ können Sie einen Domänennamen eingeben, wenn Sie den gewünschten nicht finden, beispielsweise "mydomain.onmicrosoft.com". Sie können die Option zum Erstellen einer neuen Azure AD-App auswählen oder die Einstellungen einer vorhandenen Azure AD-App verwenden. 
   
   ![Konfigurieren Sie Azure AD im Assistenten](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)
6. Stellen Sie auf der dritten Seite des Assistenten sicher, dass **Verzeichnisdaten lesen** aktiviert ist. Der Assistent füllt das Feld **Geheimer Clientschlüssel**aus. 
   
    ![Konfigurieren Sie Azure AD im Assistenten](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)
7. Klicken Sie auf die Schaltfläche **Fertig stellen** . Das Dialogfeld fügt den erforderlichen Konfigurationscode und Verweise hinzu, um das Projekt für die Azure AD-Authentifizierung zu aktivieren. Die AD-Domäne wird im [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)angezeigt.
8. Überprüfen Sie die Seite "Erste Schritte", die in Ihrem Browser angezeigt wird, um mögliche nächste Schritte zu ermitteln, und die Seite "Was ist passiert", um zu sehen, wie Ihr Projekt geändert wurde. Wenn Sie überprüfen möchten, ob alles funktioniert hat, öffnen Sie die geänderten Konfigurationsdateien, und stellen Sie sicher, dass die Einstellungen, die unter "Was ist passiert" genannt werden, vorhanden sind. Beispiel: Der wichtigsten web.config-Datei in einem ASP.NET MVC-Projekt werden die folgenden Einstellungen hinzugefügt:
   
        <appSettings> 
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:AADInstance" value="https://login.windows.net/" />
            <add key="ida:Domain" value="Your selected domain" />
            <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
            <add key="ida:PostLogoutRedirectUri" value="The default redirect URI from the project" />
        </appSettings>

## <a name="how-your-project-is-modified"></a>Änderungen am Projekt
Beim Ausführen des Assistenten fügt Visual Studio Ihrem Projekt Azure AD und entsprechende Verweise hinzu. Zudem werden Konfigurationsdateien und Codedateien im Projekt geändert, um Unterstützung für Azure AD hinzuzufügen. Die genauen Änderungen, die von Visual Studio vorgenommen werden, hängen vom Projekttyp ab. Ausführliche Informationen dazu, wie ASP.NET MVC-Projekte geändert werden, finden Sie unter [Was ist passiert – MVC-Projekte](http://go.microsoft.com/fwlink/p/?LinkID=513809). Informationen zu Web-API-Projekten finden Sie unter [Was ist passiert – Web-API-Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513810).

## <a name="next-steps"></a>Nächste Schritte
Stellen Sie Fragen, und holen Sie sich Hilfe.

* [MSDN-Forum: Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)
* [Azure AD-Dokumentation](https://azure.microsoft.com/documentation/services/active-directory/)
* [Blogbeitrag: Intro to Azure AD (in englischer Sprache)](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)




<!--HONumber=Nov16_HO3-->


