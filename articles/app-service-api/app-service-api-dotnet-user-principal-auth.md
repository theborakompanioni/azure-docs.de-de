---
title: "Benutzerauthentifizierung für API-Apps in Azure App Service | Microsoft Docs"
description: "Erfahren Sie, wie Sie eine API-App in Azure App Service schützen, indem Sie den Zugriff nur für authentifizierte Benutzer zulassen."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 3896760d-46ff-4b67-b98a-edd233f24758
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: alkarche
translationtype: Human Translation
ms.sourcegitcommit: 16a2c6f8e704528c38054bba394e3d11098077e0
ms.openlocfilehash: c39e7bca6f339a1797a9344778e6a01ffa8adecc
ms.lasthandoff: 01/24/2017


---
# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Benutzerauthentifizierung für API-Apps in Azure App Service
## <a name="overview"></a>Übersicht
In diesem Artikel wird gezeigt, wie Sie eine Azure API-App schützen, damit sie nur von authentifizierten Benutzern aufgerufen werden kann. In dem Artikel wird davon ausgegangen, dass Sie die [Übersicht über die Authentifizierung in Azure App Service](../app-service/app-service-authentication-overview.md)gelesen haben

Sie lernen Folgendes:

* Konfigurieren eines Authentifizierungsanbieters mit Details für Azure Active Directory (Azure AD)
* Nutzen einer geschützten API-App mit der [Active Directory Authentication Library (ADAL) für JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js)

Der Artikel enthält zwei Abschnitte:

* Der Abschnitt [Konfigurieren der Benutzerauthentifizierung in Azure App Service](#authconfig) enthält eine allgemeine Beschreibung, wie Sie die Benutzerauthentifizierung für eine API-App konfigurieren. Sie gilt für alle Frameworks, die von App Service unterstützt werden, z.B. für .NET, Node.js und Java.
* Beginnend mit dem Abschnitt [Fortsetzen der .NET-Tutorials für API-Apps](#tutorialstart) führt Sie dieser Artikel durch das Konfigurieren einer Beispielanwendung mit einem .NET-Back-End und einem AngularJS-Front-End, wobei Azure Active Directory für die Benutzerauthentifizierung verwendet wird. 

## <a id="authconfig"></a> Konfigurieren der Benutzerauthentifizierung in Azure App Service
Dieser Abschnitt enthält allgemeine Anweisungen, die für alle API-Apps gelten. Die speziellen Schritte für die .NET-Beispielanwendung (Aufgabenliste) sind in [Fortsetzen der .NET-Tutorials für API-Apps](#tutorialstart)enthalten.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zum Blatt **Einstellungen** der zu schützenden API-App, suchen Sie den Abschnitt mit den **Features**, und klicken Sie dann auf **Authentifizierung/Autorisierung**.
   
    ![Authentifizierung/Autorisierung im Azure-Portal](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. Klicken Sie auf dem Blatt **Authentifizierung/Autorisierung** auf **Ein**.
3. Wählen Sie in der Dropdownliste **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** eine der Optionen aus.
   
   * Wenn nur authentifizierte Aufrufe die API-App erreichen sollen, wählen Sie eine der Optionen unter **Anmelden mit...** aus. Mit dieser Option können Sie die API-App schützen, ohne Code zu schreiben, der darin ausgeführt wird.
   * Wenn Sie möchten, dass alle Aufrufe Ihre API-App erreichen, wählen Sie **Anforderung zulassen (keine Aktion)**aus. Sie können diese Option verwenden, um nicht authentifizierte Aufrufer an verschiedene Authentifizierungsanbieter weiterzuleiten. Bei dieser Option müssen Sie Code schreiben, um die Autorisierung durchzuführen.
     
     Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung für API-Apps in Azure App Service](app-service-api-authentication.md#multiple-protection-options).
4. Wählen Sie einen oder mehrere **Authentifizierungsanbieter**aus.
   
    Die Abbildung zeigt die Optionen, bei denen alle Aufrufer von Azure AD authentifiziert werden müssen.
   
    ![Blatt „Authentifizierung/Autorisierung“ im Azure-Portal](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
   
    Bei der Auswahl eines Authentifizierungsanbieters wird im Portal ein Konfigurationsblatt für den Anbieter angezeigt. 
   
    Eine ausführliche Anleitung zum Konfigurieren des Authentifizierungsanbieters auf dem jeweiligen Blatt finden Sie unter [So konfigurieren Sie Ihre App Service-Anwendung zur Verwendung der Azure Active Directory-Anmeldung](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (Der Link führt zu einem Artikel über Azure AD, aber der Artikel selbst enthält Registerkarten mit Links zu Artikeln für die anderen Authentifizierungsanbieter.)
5. Wenn Sie mit dem Blatt für die Konfiguration des Authentifizierungsanbieters fertig sind, klicken Sie auf **OK**.
6. Klicken Sie auf dem Blatt **Authentifizierung/Autorisierung** auf **Speichern**.

Wenn dies abgeschlossen ist, authentifiziert App Service alle API-Aufrufe, bevor sie die API-App erreichen. Die Authentifizierungsdienste funktionieren für alle Sprachen gleich, die von App Service unterstützt werden, z. B. .NET, Node.js und Java. 

Zum Durchführen authentifizierter API-Aufrufe fügt der Aufrufer das OAuth 2.0-Bearertoken des Authentifizierungsanbieters in den Autorisierungsheader von HTTP-Anforderungen ein. Das Token kann mit dem SDK des Authentifizierungsanbieters abgerufen werden.

## <a id="tutorialstart"></a> Fortsetzen der .NET-Tutorials für API-Apps
Wenn Sie die Node.js- oder Java-Tutorials für API-Apps durcharbeiten, können Sie mit dem nächsten Artikel [Dienstprinzipalauthentifizierung für API-Apps](app-service-api-dotnet-service-principal-auth.md)fortfahren. 

Wenn Sie die .NET-Tutorialreihe für API-Apps durcharbeiten und die Beispielanwendung schon bereitgestellt haben, wie im [ersten](app-service-api-dotnet-get-started.md) und [zweiten](app-service-api-cors-consume-javascript.md) Tutorial beschrieben, können Sie mit dem Abschnitt [Einrichten der Authentifizierung in App Service und Azure AD](#azureauth) fortfahren.

Wenn Sie dieses Tutorial durcharbeiten möchten, ohne das erste und zweite Tutorial zu absolvieren, führen Sie die folgenden Schritte aus. Darin werden erste Schritte beschrieben, durch die Sie die Beispielanwendung mit einem automatisierten Prozess bereitstellen können.

> [!NOTE]
> Mit diesen nachfolgenden Schritten gelangen Sie zum gleichen Ausgangspunkt wie nach dem Durcharbeiten der ersten beiden Tutorials, mit einer Ausnahme: In Visual Studio liegen noch keine Informationen dazu vor, in welcher Web-App oder API-App die Projekte bereitgestellt werden sollen. Mit anderen Worten: Sie finden in diesem Tutorial keine genauen Anweisungen, wie Sie die Bereitstellung an den richtigen Zielen durchführen. Wenn Sie sich das Vorgehen für diese Bereitstellungsschritte nicht selbst erschließen möchten, ist es besser, die Tutorialreihe vom [ersten Tutorial](app-service-api-dotnet-get-started.md) an durchzuarbeiten und nicht mit dem automatisierten Bereitstellungsprozess zu beginnen.
> 
> 

1. Stellen Sie sicher, dass alle im [ersten Tutorial](app-service-api-dotnet-get-started.md)aufgeführten Voraussetzungen erfüllt sind. Zusätzlich zu den aufgeführten Voraussetzungen wird für diese Authentifizierungstutorials angenommen, dass Sie bereits mit App Service-Web-Apps und API-Apps in Visual Studio und dem Azure-Portal gearbeitet haben.
2. Klicken Sie in der Infodatei **To Do List Azure API App Sample** auf die Schaltfläche [Deploy to Azure](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) , um die API-Apps und die Web-App bereitzustellen. Notieren Sie die Azure-Ressourcengruppe, die erstellt wird, da Sie diese später verwenden können, um Web-App- und API-App-Namen nachzuschlagen.
3. Laden Sie das Repository [To Do List Azure API App Sample](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) herunter, oder klonen Sie es, um den Code zu erhalten, mit dem Sie lokal in Visual Studio arbeiten werden.

## <a id="azureauth"></a> Einrichten der Authentifizierung in App Service und Azure AD
Nun wird die Anwendung in Azure App Service ausgeführt, ohne dass Benutzer authentifiziert sein müssen. In diesem Abschnitt fügen Sie die Authentifizierung hinzu, indem Sie Folgendes durchführen:

* Konfigurieren Sie App Service so, dass zum Aufrufen der API-App der mittleren Ebene die Azure Active Directory-Authentifizierung (Azure AD) erforderlich ist.
* Erstellen Sie eine Azure AD-Anwendung.
* Konfigurieren Sie die Azure AD-Anwendung so, dass das Bearertoken nach der Anmeldung an das AngularJS-Front-End gesendet wird. 

Wenn beim Befolgen der Anweisungen in diesem Tutorial Probleme auftreten, lesen Sie den Abschnitt [Problembehandlung](#troubleshooting) am Ende des Tutorials. 

### <a name="configure-authentication-for-the-middle-tier-api-app"></a>Konfigurieren der Authentifizierung für die API-App der mittleren Ebene
1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zum Blatt **Einstellungen** der API-App, die Sie für das ToDoListAPI-Projekt erstellt haben, suchen Sie den Abschnitt mit den **Features**, und klicken Sie dann auf **Authentifizierung/Autorisierung**.
   
    ![Authentifizierung/Autorisierung im Azure-Portal](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. Klicken Sie auf dem Blatt **Authentifizierung/Autorisierung** auf **Ein**.
3. Wählen Sie in der Dropdownliste **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** die Option **Mit Azure Active Directory anmelden**.
   
    Mit dieser Option wird sichergestellt, dass die API-App keine nicht authentifizierten Anforderungen erreichen. 
4. Under **Authentication Providers**, click **Azure Active Directory**.
   
    ![Blatt „Authentifizierung/Autorisierung“ im Azure-Portal](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. Klicken Sie auf dem Blatt **Azure Active Directory-Einstellungen** auf **Express**.
   
    ![Option „Express“ für Authentifizierung/Autorisierung im Azure-Portal](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)
   
    Mit der Option **Express** kann App Service automatisch eine Azure AD-Anwendung in Ihrem Azure AD-[Mandanten](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)erstellen. 
   
    Sie müssen keinen Mandanten erstellen, da jedes Azure-Konto automatisch darüber verfügt.
6. Klicken Sie unter **Verwaltungsmodus** auf **Neue AD-App erstellen**, sofern die Option noch nicht aktiviert wurde, und notieren Sie sich den Wert im Textfeld **App erstellen**. Sie werden später im klassischen Azure-Portal nach dieser AAD-Anwendung suchen.
   
    ![Azure AD-Einstellungen im Azure-Portal](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)
   
    Azure erstellt in Ihrem Azure AD-Mandanten automatisch eine Azure AD-Anwendung mit dem angegebenen Namen. Standardmäßig hat die Azure AD-Anwendung den gleichen Namen wie die API-App. Wenn Sie möchten, können Sie auch einen anderen Namen eingeben.
7. Klicken Sie auf **OK**.
8. Klicken Sie auf dem Blatt **Authentifizierung/Autorisierung** auf **Speichern**.
   
    ![Klicken Sie auf Speichern.](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Jetzt kann die API-App nur von Benutzern aufgerufen werden, die im Azure AD-Mandanten enthalten sind.

### <a name="optional-test-the-api-app"></a>Optional: Testen der API-App
1. Navigieren Sie in einem Browser zur URL der API-App: Klicken Sie auf dem Blatt **API-App** im Azure-Portal auf den Link unter **URL**.  
   
    Sie werden auf eine Anmeldeseite umgeleitet, da nicht authentifizierte Anforderungen die API-App nicht erreichen dürfen.
   
    Wenn in Ihrem Browser die Seite mit der Meldung zur erfolgreichen Erstellung angezeigt wird, kann es sein, dass der Browser bereits angemeldet ist. Öffnen Sie in diesem Fall ein InPrivate- oder Incognito-Fenster, und greifen Sie auf die URL der API-App zu.
2. Melden Sie sich mit Anmeldeinformationen für einen Benutzer im Azure AD-Mandanten an.
   
    Nachdem Sie sich angemeldet haben, wird im Browser die Seite mit dem Hinweis zur erfolgreichen Erstellung angezeigt.
3. Schließen Sie den Browser.

### <a name="configure-the-azure-ad-application"></a>Konfigurieren der Azure AD-Anwendung
Wenn Sie die Azure AD-Authentifizierung konfiguriert haben, erstellt App Service eine Azure AD-Anwendung für Sie. Standardmäßig ist die Azure AD-Anwendung so konfiguriert, dass das Bearertoken für die URL der API-App bereitgestellt wird. In diesem Abschnitt konfigurieren Sie die Azure AD-Anwendung, um das Bearertoken für das AngularJS-Front-End bereitzustellen, anstatt direkt für die API-App der mittleren Ebene. Das AngularJS-Front-End sendet das Token an die API-App, wenn es die API-App aufruft.

> [!NOTE]
> Um den Vorgang so einfach wie möglich zu halten, wird in diesem Tutorial eine einzelne Azure AD-Anwendung für das Front-End und die API-App der mittleren Ebene verwendet. Es ist aber auch möglich, zwei Azure AD-Anwendungen zu verwenden. In diesem Fall müssten Sie der Azure AD-Anwendung für das Front-End die Berechtigung zum Aufrufen der Azure AD-Anwendung der mittleren Ebene gewähren. Dieser Ansatz mit mehreren Anwendungen bietet mehr Kontrolle über die Berechtigungen der einzelnen Ebenen.
> 
> 

1. Navigieren Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/)zu **Azure Active Directory**.
   
   Sie müssen das klassische Portal verwenden, da bestimmte Azure Active Directory-Einstellungen, auf die Sie zugreifen müssen, im derzeitigen Azure-Portal noch nicht verfügbar sind.
2. Klicken Sie auf der Registerkarte **Verzeichnis** auf Ihren AAD-Mandanten.
   
   ![Azure AD im klassischen Portal](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)
3. Klicken Sie auf **Anwendungen > Anwendungen im Besitz meines Unternehmens** und dann auf das Häkchen.
   
   Unter Umständen müssen Sie die Seite aktualisieren, um die neue Anwendung anzuzeigen.
4. Klicken Sie in der Liste mit den Anwendungen auf den Namen der Anwendung, die von Azure für Sie erstellt wurde, als Sie die Authentifizierung für Ihre API-App aktiviert haben.
   
   ![Registerkarte „Azure AD-Anwendungen“](./media/app-service-api-dotnet-user-principal-auth/appstab.png)
5. Klicken Sie auf **Konfigurieren**.
   
   ![Registerkarte „Azure AD konfigurieren“](./media/app-service-api-dotnet-user-principal-auth/configure.png)
6. Legen Sie die **Anmelde-URL** auf die URL für Ihre AngularJS-Web-App ohne nachgestellten Schrägstrich fest.
   
   Beispiel: https://todolistangular.azurewebsites.net
   
   ![Anmelde-URL](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)
7. Legen Sie die **Antwort-URL** auf die URL für Ihre Web-App ohne nachgestellten Schrägstrich fest.
   
   Beispiel: https://todolistsangular.azurewebsites.net
8. Klicken Sie auf **Speichern**.
   
   ![Antwort-URL](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)
9. Klicken Sie unten auf der Seite auf **Manifest verwalten > Manifest herunterladen**.
   
   ![Manifest herunterladen](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)
10. Laden Sie die Datei an einen Ort herunter, an dem Sie sie bearbeiten können.
11. Suchen Sie in der heruntergeladenen Manifestdatei nach der `oauth2AllowImplicitFlow`-Eigenschaft. Ändern Sie den Wert dieser Eigenschaft von `false` in `true`, und speichern Sie die Datei anschließend.
    
    Diese Einstellung ist für den Zugriff aus einer einseitigen JavaScript-Anwendung erforderlich. Sie ermöglicht dem Oauth 2.0-Bearertoken die Rückgabe im URL-Fragment.
12. Klicken Sie auf **Manifest verwalten > Manifest hochladen**, und laden Sie die Datei hoch, die Sie im vorherigen Schritt aktualisiert haben.
    
    ![Manifest hochladen](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)
13. Kopieren Sie den Wert für **Client-ID** , und speichern Sie ihn an einem Ort, an dem Sie später darauf zugreifen können.

## <a name="configure-the-todolistangular-project-to-use-authentication"></a>Konfigurieren der Authentifizierung für das Projekt ToDoListAngular
In diesem Abschnitt ändern Sie das AngularJS-Front-End, damit es die Active Directory Authentication Library (ADAL) für JS zum Abrufen eines Bearertokens für den angemeldeten Benutzer aus Azure AD verwendet. Der Code enthält das Token in HTTP-Anforderungen, die an die mittlere Ebene gesendet werden. Dies ist in der folgenden Abbildung dargestellt. 

![Diagramm zur Benutzerauthentifizierung](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

Nehmen Sie die folgenden Änderungen an den Dateien im Projekt ToDoListAngular vor.

1. Öffnen Sie die Datei *Index.cshtml*.
2. Kommentieren Sie die Zeilen aus, in denen auf die Active Directory Authentication Library (ADAL) für JS-Skripts verwiesen wird.
   
        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>
3. Öffnen Sie die Datei *app/scripts/app.js* .
4. Kommentieren Sie den Codeblock aus, der über die Kennzeichnung „without authentication“ verfügt, und heben Sie die Auskommentierung für den Codeblock auf, der über die Kennzeichnung „with authentication“ verfügt.
   
    Durch diese Änderung wird auf den ADAL-JS-Authentifizierungsanbieter verwiesen, und es werden Konfigurationswerte dafür bereitgestellt. In den folgenden Schritten legen Sie die Konfigurationswerte für Ihre API-App und die Azure AD-Anwendung fest.
5. Legen Sie im Code, mit dem die Variable `endpoints` definiert wird, die API-URL auf die URL der API-App fest, die Sie für das ToDoListAPI-Projekt erstellt haben. Legen Sie außerdem die ID der Azure AD-Anwendung auf die Client-ID fest, die Sie aus dem klassischen Azure-Portal kopiert haben.
   
    Der Code sieht jetzt wie im folgenden Beispiel aus.
   
        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };
6. Legen Sie im Aufruf von `adalProvider.init` das `tenant`-Element auf Ihren Mandantennamen und `clientId` auf denselben Wert wie im vorherigen Schritt fest.
   
    Der Code sieht jetzt wie im folgenden Beispiel aus.
   
        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );
   
    Mit diesen Änderungen an `app.js` wird angegeben, dass sich der aufrufende Code und die aufgerufene API in derselben Azure AD-Anwendung befinden.
7. Öffnen Sie die Datei *app/scripts/homeCtrl.js* .
8. Kommentieren Sie den Codeblock aus, der über die Kennzeichnung „without authentication“ verfügt, und heben Sie die Auskommentierung für den Codeblock auf, der über die Kennzeichnung „with authentication“ verfügt.
9. Öffnen Sie die Datei *app/scripts/indexCtrl.js* .
10. Kommentieren Sie den Codeblock aus, der über die Kennzeichnung „without authentication“ verfügt, und heben Sie die Auskommentierung für den Codeblock auf, der über die Kennzeichnung „with authentication“ verfügt.

### <a name="deploy-the-todolistangular-project-to-azure"></a>Bereitstellen des Projekts ToDoListAngular in Azure
1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt „ToDoListAngular“, und klicken Sie dann auf **Veröffentlichen**.
2. Klicken Sie auf **Veröffentlichen**.
   
    Visual Studio stellt das Projekt bereit und öffnet einen Browser mit der Basis-URL der Web-App. Dies führt zur Anzeige einer Seite mit dem Fehler 403, die jedoch immer angezeigt wird, wenn Sie versuchen, die Basis-URL einer Web-API in einem Browser zu öffnen.
   
    Sie müssen trotzdem eine Änderung an der API-App der mittleren Ebene vornehmen, bevor Sie die Anwendung testen können.
3. Schließen Sie den Browser.

## <a name="configure-the-todolistapi-project-to-use-authentication"></a>Konfigurieren der Authentifizierung für das Projekt ToDoListAPI
Derzeit sendet das Projekt ToDoListAPI „*“ als `owner` -Wert an ToDoListDataAPI. In diesem Abschnitt ändern Sie den Code zum Senden der ID des angemeldeten Benutzers.

Nehmen Sie am Projekt ToDoListAPI die folgenden Änderungen vor.

1. Öffnen Sie die Datei *Controllers/ToDoListController.cs*, und heben Sie die Auskommentierung der Zeile in allen Aktionsmethoden auf, mit denen `owner` auf den Azure AD-Anspruchswert `NameIdentifier` festgelegt wird. Beispiel:
   
        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;
   
    **Wichtig**: Heben Sie nicht die Auskommentierung des Codes in der `ToDoListDataAPI`-Methode auf. Sie führen diesen Schritt später im Tutorial zur Dienstprinzipalauthentifizierung durch.

### <a name="deploy-the-todolistapi-project-to-azure"></a>Bereitstellen des Projekts ToDoListAPI in Azure
1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das ToDoListAPI-Projekt, und klicken Sie dann auf **Veröffentlichen**.
2. Klicken Sie auf **Veröffentlichen**.
   
    Visual Studio stellt das Projekt bereit und öffnet ein Browserfenster mit der Basis-URL der API-App.
3. Schließen Sie den Browser.

### <a name="test-the-application"></a>Testen der Anwendung
1. Navigieren Sie zur URL der Web-App, indem Sie **HTTPS verwenden, nicht HTTP**.
2. Klicken Sie auf die Registerkarte **Aufgabenliste** .
   
    Sie werden aufgefordert, sich anzumelden.
3. Melden Sie sich mit den Anmeldeinformationen eines Benutzers an Ihrem AAD-Mandanten an.
4. Die Seite **Aufgabenliste** wird angezeigt.
   
   ![Seite „Aufgabenliste“](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)
   
   Es werden keine Aufgabenelemente angezeigt, da sie bis jetzt alle für den Besitzer „*“ galten. Jetzt fordert die mittlere Ebene Elemente für den angemeldeten Benutzer an, aber es wurden noch keine erstellt.
5. Fügen Sie neue Aufgabenelemente hinzu, um zu überprüfen, ob die Anwendung funktioniert.
6. Navigieren Sie in einem anderen Browserfenster zur Swagger-Benutzeroberflächen-URL für die API-App „ToDoListDataAPI“, und klicken Sie auf **ToDoList > Abrufen**. Geben Sie ein Sternchen für den `owner` -Parameter ein, und klicken Sie dann auf **Testen Sie es**.
   
   Die Antwort zeigt, dass die neuen Aufgabenelemente in der Owner-Eigenschaft über die richtige Azure AD-Benutzer-ID verfügen.
   
   ![Besitzer-ID in der JSON-Antwort](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)

## <a name="building-the-projects-from-scratch"></a>Neues Erstellen der Projekte von Grund auf
Die beiden Web-API-Projekte wurden mithilfe der Projektvorlage **Azure-API-App** erstellt, indem der standardmäßige Values-Controller durch einen ToDoList-Controller ersetzt wurde. 

Weitere Informationen zum Erstellen einer einseitigen AngularJS-Anwendung mit einem Web-API 2-Back-End finden Sie unter [Hands On Lab: Build a Single Page Application (SPA) with ASP.NET Web API and Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs) (Hands On Lab: Erstellen einer Single-Page-Anwendung (SPA) mit ASP.NET-Web-API und „Angular.js“). Informationen zum Hinzufügen von Azure AD-Authentifizierungscode finden Sie unter den folgenden Ressourcen:

* [Schützen von einseitigen AngularJS-Apps mit Azure AD](../active-directory/active-directory-devquickstarts-angular.md).
* [Introducing ADAL JS v1 (Einführung in ADAL JS v1)](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Problembehandlung
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Achten Sie darauf, ToDoListAPI (mittlere Ebene) und ToDoListDataAPI (Datenebene) nicht zu verwechseln. Stellen Sie z. B. sicher, dass Sie die Authentifizierung für die API-App der mittleren Ebene – nicht der Datenebene – hinzufügen. 
* Vergewissern Sie sich, dass der AngularJS-Quellcode auf die API-App-URL der mittleren Ebene (ToDoListAPI, nicht ToDoListDataAPI) sowie auf die richtige Azure AD-Client-ID verweist. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, wie Sie die App Service-Authentifizierung für eine API-App verwenden und die API-App mit der ADAL-JS-Bibliothek aufrufen. Im nächsten Tutorial geht es um das [Schützen des Zugriffs auf Ihre API-App für Dienst-zu-Dienst-Szenarien](app-service-api-dotnet-service-principal-auth.md).


