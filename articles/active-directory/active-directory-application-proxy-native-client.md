---
title: "Veröffentlichen von nativen Client-Apps mit Proxyanwendungen | Microsoft Docs"
description: "Erläutert, wie Sie es systemeigenen Client-Apps ermöglichen, mit einem Azure AD-Anwendungsproxyconnector zu kommunizieren und so einen sicheren Remotezugriff auf lokale Anwendungen bereitzustellen."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: f0cae145-e346-4126-948f-3f699747b96e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 8bf2d7d35f7cf14b4f5430624f23d5b3b9c3c6be


---
# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Aktivieren von systemeigenen Client-Apps für die Interaktion mit Proxyanwendungen
Mit Azure Active Directory-Anwendungsproxy werden häufig Browseranwendungen wie SharePoint, Outlook Web Access und benutzerdefinierte Branchenanwendungen veröffentlicht. Außerdem können damit native Client-Apps veröffentlicht werden, die sich insofern von Web-Apps unterscheiden, als sie auf einem Gerät installiert werden. Dies erfolgt durch die Unterstützung von durch Azure AD ausgegebenen Token, die in standardmäßigen Authorize- HTTP-Headern gesendet werden.

![Beziehung zwischen Endbenutzern, Azure Active Directory und veröffentlichten Anwendung](./media/active-directory-application-proxy-native-client/richclientflow.png)

Zum Veröffentlichen solcher Anwendungen wird empfohlen, die Azure AD-Authentifizierungsbibliothek zu verwenden. Sie übernimmt die aufwendige Authentifizierung und unterstützt viele verschiedene Clientumgebungen. Der Anwendungsproxy gehört zum Szenario für eine [native Anwendung zu Web-API](active-directory-authentication-scenarios.md#native-application-to-web-api). Die Vorgehensweise hierfür ist wie folgt:

## <a name="step-1-publish-your-application"></a>Schritt 1: Veröffentlichen der Anwendung
Veröffentlichen Sie Ihre Proxyanwendung wie jede andere Anwendung, weisen Sie Benutzer zu und geben Sie ihnen Premium- oder Basic-Lizenzen. Weitere Informationen finden Sie unter [Veröffentlichen von Anwendungen mit einem Anwendungsproxy](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Schritt 2: Konfigurieren der Anwendung
Konfigurieren Sie Ihre systemeigene Anwendung wie folgt:

1. Melden Sie sich am klassischen Azure-Portal an.
2. Wählen Sie im linken Menü das Active Directory-Symbol und dann Ihr Verzeichnis aus.
3. Klicken Sie im oberen Menü auf **Anwendungen**. Wenn Ihrem Verzeichnis keine Apps hinzugefügt wurden, wird auf dieser Seite der Link **App hinzufügen** angezeigt. Klicken Sie auf den Link oder alternativ auf die Schaltfläche **Hinzufügen** in der Befehlsleiste.
4. Klicken Sie auf der Seite **Was möchten Sie gerne tun?** auf den Link **Eine von meinem Unternehmen entwickelte Anwendung hinzufügen**.
5. Geben Sie auf der Seite **Geben Sie uns Informationen zu Ihrer Anwendung.** einen Namen für Ihre Anwendung ein, und wählen Sie **Systemeigene Clientanwendung** aus. Klicken Sie auf das Pfeilsymbol, um fortzufahren.
6. Geben Sie auf der Seite **Anwendungsinformationen** den **Umleitungs-URI** für die native Clientanwendung an, und klicken Sie abschließend auf das Häkchen.

Ihre Anwendung wird hinzugefügt, und Sie werden auf die Seite "Schnellstart" für Ihre Anwendung umgeleitet.

## <a name="step-3-grant-access-to-other-applications"></a>Schritt 3: Gewähren des Zugriffs auf andere Anwendungen
Machen Sie die systemeigene Anwendung für andere Anwendungen in Ihrem Verzeichnis verfügbar:

1. Klicken Sie im oberen Menü auf **Anwendungen**, wählen Sie die neue native Anwendung aus, und klicken Sie auf **Konfigurieren**.
2. Scrollen Sie nach unten zum Abschnitt **Berechtigungen für andere Anwendungen** . Klicken Sie auf die Schaltfläche **Anwendung hinzufügen** , und wählen Sie die Proxyanwendung aus, auf die die systemeigene Anwendung Zugriff erhalten soll, und klicken Sie auf das Häkchen in der unteren rechten Ecke. Wählen Sie im Dropdownmenü **Delegierte Berechtigungen** die neue Berechtigung aus.

![Screenshot: Berechtigungen für andere Anwendungen – Anwendung hinzufügen](./media/active-directory-application-proxy-native-client/delegate_native_app.png)

## <a name="step-4-edit-the-active-directory-authentication-library"></a>Schritt 4: Bearbeiten der Active Directory-Authentifizierungsbibliothek
Bearbeiten Sie den Code für systemeigene Anwendungen im Authentifizierungskontext der Active Directory-Authentifizierungsbibliothek (ADAL, Active Directory Authentication Library), sodass er Folgendes enthält:

        // Acquire Access Token from AAD for Proxy Application
        AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<TenantId>");
        AuthenticationResult result = authContext.AcquireToken("< Frontend Url of Proxy App >",
                                                        "< Client Id of the Native app>",
                                                        new Uri("< Redirect Uri of the Native App>"),
                                                        PromptBehavior.Never);

        //Use the Access Token to access the Proxy Application
        HttpClient httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
        HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");

Die Variablen müssen wie folgt ersetzt werden:

* Den Wert für **TenantId** finden Sie in der URL auf der Seite **Konfiguration** der Anwendung in der GUID, direkt nach „/Directory/“.
* Den Wert für **Frontend Url of Proxy App** ist die Front-End-URL, die Sie in der Proxyanwendung eingegeben haben. Sie finden sie auf der Seite **Konfiguration** der Proxy-App.
* Den Wert für **Client Id of the Native app** finden Sie auf der Seite **Konfigurieren** der nativen Anwendung.
* Den Wert für **Redirect Uri of the Native App** finden Sie auf der Seite **Konfigurieren** der nativen Anwendung.

![Screenshot der Konfigurationsseite für die neue systemeigene Anwendung](./media/active-directory-application-proxy-native-client/new_native_app.png)

Weitere Informationen über den Fluss bei nativen Anwendungen finden Sie unter [Systemeigene Anwendung zu Web-API](active-directory-authentication-scenarios.md#native-application-to-web-api).

## <a name="see-also"></a>Weitere Informationen
* [Veröffentlichen von Anwendungen mit Ihrem eigenen Domänennamen](active-directory-application-proxy-custom-domains.md)
* [Aktivieren des bedingten Zugriffs](active-directory-application-proxy-conditional-access.md)
* [Arbeiten mit Anwendungen, die Ansprüche unterstützen](active-directory-application-proxy-claims-aware-apps.md)
* [Aktivieren der einmaligen Anmeldung](active-directory-application-proxy-sso-using-kcd.md)

Aktuelle Neuigkeiten und Updates finden Sie im [Blog zum Anwendungsproxy](http://blogs.technet.com/b/applicationproxyblog/)




<!--HONumber=Nov16_HO3-->


