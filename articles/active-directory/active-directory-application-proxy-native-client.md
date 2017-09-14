---
title: "Veröffentlichen nativer Client-Apps – Azure AD | Microsoft-Dokumentation"
description: "Erläutert, wie Sie es systemeigenen Client-Apps ermöglichen, mit einem Azure AD-Anwendungsproxyconnector zu kommunizieren und so einen sicheren Remotezugriff auf lokale Anwendungen bereitzustellen."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f0cae145-e346-4126-948f-3f699747b96e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 3eb68cba15e89c455d7d33be1ec0bf596df5f3b7
ms.openlocfilehash: e1f992aede3af99fa7c2ffa661bccbcac9f52ba9
ms.contentlocale: de-de
ms.lasthandoff: 09/01/2017

---

# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Aktivieren von nativen Client-Apps für die Interaktion mit Proxyanwendungen

Zusätzlich zu Webanwendungen kann der Azure Active Directory-Anwendungsproxy auch zum Veröffentlichen nativer Client-Apps verwendet werden, die mit der Azure AD Authentication Library (ADAL) konfiguriert werden. Native Client-Apps unterscheiden sich von Web-Apps dadurch, dass sie auf einem Gerät installiert werden, während auf Web-Apps über einen Browser zugegriffen wird. 

Der Anwendungsproxy unterstützt native Client-Apps durch das Akzeptieren der von Azure AD ausgegebenen Token, die im Header gesendet werden. Der Anwendungsproxydienst führt die Authentifizierung für den Benutzer durch. Diese Lösung verwendet keine Anwendungstoken für die Authentifizierung. 

![Beziehung zwischen Endbenutzern, Azure Active Directory und veröffentlichten Anwendung](./media/active-directory-application-proxy-native-client/richclientflow.png)

Verwenden Sie zum Veröffentlichen nativer Anwendungen die Active Directory Authentication Library (ADAL), die die Authentifizierung übernimmt und viele Clientumgebungen unterstützt. Der Anwendungsproxy gehört zum Szenario für eine [native Anwendung zu Web-API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api). 

Dieser Artikel begleitet Sie durch die vier Schritte zum Veröffentlichen einer nativen Anwendung mit dem Anwendungsproxy und der Active Directory Authentication Library. 

## <a name="step-1-publish-your-application"></a>Schritt 1: Veröffentlichen der Anwendung
Veröffentlichen Sie Ihre Proxyanwendung wie jede andere Anwendung, und weisen Sie Benutzern den Zugriff auf Ihre Anwendung zu. Weitere Informationen finden Sie unter [Veröffentlichen von Anwendungen mit dem Anwendungsproxy](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Schritt 2: Konfigurieren der Anwendung
Konfigurieren Sie Ihre systemeigene Anwendung wie folgt:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Navigieren Sie zu **Azure Active Directory** > **App-Registrierungen**.
3. Wählen Sie **Registrierung einer neuen Anwendung** aus.
4. Geben Sie einen Namen für Ihre Anwendung an. Wählen Sie als Anwendungstyp **Nativ**, und geben Sie den Umleitungs-URI für Ihre Anwendung an. 

   ![Erstellen einer neuen App-Registrierung](./media/active-directory-application-proxy-native-client/create.png)
5. Klicken Sie auf **Erstellen**.

Ausführlichere Informationen zum Erstellen einer neuen App-Registrierung finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](.//develop/active-directory-integrating-applications.md).


## <a name="step-3-grant-access-to-other-applications"></a>Schritt 3: Gewähren des Zugriffs auf andere Anwendungen
Machen Sie die systemeigene Anwendung für andere Anwendungen in Ihrem Verzeichnis verfügbar:

1. Wählen Sie in **App-Registrierungen** die neue native Anwendung aus, die Sie gerade erstellt haben.
2. Wählen Sie **Erforderliche Berechtigungen** aus.
3. Wählen Sie **Hinzufügen**.
4. Öffnen Sie den ersten Schritt, **API auswählen**.
5. Suchen Sie über die Suchleiste die Anwendungsproxy-App, die Sie im ersten Abschnitt veröffentlicht haben. Wählen Sie diese App, und klicken Sie auf **Auswählen**. 

   ![Suchen der Proxy-App](./media/active-directory-application-proxy-native-client/select_api.png)
6. Öffnen Sie den zweiten Schritt, **Berechtigungen auswählen**.
7. Aktivieren Sie das Kontrollkästchen, um Ihrer nativen Anwendung Zugriff auf Ihre Proxyanwendung zu gewähren, und klicken Sie dann auf **Auswählen**.

   ![Gewähren des Zugriffs auf die Proxy-App](./media/active-directory-application-proxy-native-client/select_perms.png)
8. Wählen Sie **Fertig**aus.


## <a name="step-4-edit-the-active-directory-authentication-library"></a>Schritt 4: Bearbeiten der Active Directory-Authentifizierungsbibliothek
Bearbeiten Sie den Code für native Anwendungen im Authentifizierungskontext der Active Directory-Authentifizierungsbibliothek (ADAL, Active Directory Authentication Library), sodass er folgenden Text enthält:

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = authContext.AcquireToken("< External Url of Proxy App >",
        "<App ID of the Native app>",
        new Uri("<Redirect Uri of the Native App>"),
        PromptBehavior.Never);

//Use the Access Token to access the Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

Die Variablen im Beispielcode müssen wie folgt ersetzt werden:

* Die **Mandanten-ID** finden Sie im Azure-Portal. Navigieren Sie zu **Azure Active Directory** > **Eigenschaften**, und kopieren Sie die Verzeichnis-ID. 
* **Externe URL** ist die Front-End-URL, die Sie in der Proxyanwendung eingegeben haben. Um diesen Wert zu finden, navigieren Sie zum Abschnitt **Anwendungsproxy** der Proxy-App.
* Die **App-ID** der nativen App finden Sie auf der Seite **Eigenschaften** der nativen Anwendung.
* Den **Umleitungs-URI der nativen App** finden Sie auf der Seite **Umleitungs-URI** der nativen Anwendung.

Nachdem die ADAL mit diesen Parametern bearbeitet wurde, sollten sich Ihre Benutzer bei nativen Client-Apps authentifizieren können, auch wenn sie sich außerhalb des Unternehmensnetzwerks befinden. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über den Fluss bei nativen Anwendungen finden Sie unter [Systemeigene Anwendung zu Web-API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api).

Informationen zum Einrichten von einmaligem Anmelden für den Anwendungsproxy finden Sie [hier](application-proxy-sso-overview.md).

