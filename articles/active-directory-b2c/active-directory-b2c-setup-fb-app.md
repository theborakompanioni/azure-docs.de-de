---
title: "Azure Active Directory B2C: Konfiguration für Facebook | Microsoft Docs"
description: "Bereitstellen von Registrierung und Anmeldung für Kunden mit Facebook-Konten in mit Azure Active Directory B2C gesicherten Anwendungen."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: b875f235-a1d2-4abb-b9f0-b89beac38a32
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 329e22f307a1db5b8ab957caaa7222e452e0b008


---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory B2C: Bereitstellen von Registrierung und Anmeldung für Kunden mit Facebook-Konten
## <a name="create-a-facebook-application"></a>Erstellen einer Facebook-Anwendung
Um Facebook als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Facebook-Anwendung erstellen und mit den entsprechenden Parametern bereitstellen. Sie benötigen dazu ein Facebook-Konto. Wenn Sie über kein Konto verfügen, können Sie unter [https://www.facebook.com/](https://www.facebook.com/)eines erstellen.

1. Navigieren Sie zur [Facebook-Entwickler-Website](https://developers.facebook.com/), und melden Sie sich mit den Anmeldeinformationen für Ihr Facebook-Konto an.
2. Wenn Sie dies noch nicht getan haben, müssen Sie sich als Facebook-Entwickler registrieren. Dazu klicken Sie in der oberen rechten Ecke der Seite auf **Register**, akzeptieren die Facebook-Richtlinien und führen die Registrierungsschritte aus.
3. Klicken Sie auf **My Apps** und dann auf **Create a New App**. Wählen Sie als Plattform **Website** aus, und klicken Sie dann auf **Skip and Create App ID**.
   
    ![Facebook – Hinzufügen einer neuen App](./media/active-directory-b2c-setup-fb-app/fb-add-new-app.png)
   
    ![Facebook – Hinzufügen einer neuen App – Website](./media/active-directory-b2c-setup-fb-app/fb-add-new-app-website.png)
   
    ![Facebook – Erstellen einer App-ID](./media/active-directory-b2c-setup-fb-app/fb-new-app-skip.png)
4. Geben Sie im Formular einen Wert für **Display Name** sowie einen gültigen Wert für **Contact Email** an, wählen Sie einen Wert für **Category** aus, und klicken Sie auf **Create App ID**. Hierzu müssen Sie die Richtlinien für die Facebook-Plattform akzeptieren und eine Online-Sicherheitsüberprüfung durchführen.
   
    ![Facebook – Erstellen einer neuen App-ID](./media/active-directory-b2c-setup-fb-app/fb-create-app-id.png)
5. Klicken Sie im linken Navigationsbereich auf **Settings**.
6. Klicken Sie auf **+Add Platform**, und wählen Sie dann **Website** aus.
   
    ![Facebook – Einstellungen](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook – Einstellungen – Website](./media/active-directory-b2c-setup-fb-app/fb-website.png)
7. Geben Sie [https://login.microsoftonline.com/](https://login.microsoftonline.com/) im Feld **Site URL** ein, und klicken Sie dann auf **Save Changes**.
   
    ![Facebook – Website-URL](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)
8. Kopieren Sie den Wert unter **App ID**. Klicken Sie auf **Show**, und kopieren Sie den Wert unter **App Secret**. Sie benötigen beide Angaben, um Facebook als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. **App Secret** handelt es sich um eine wichtige Sicherheitsinformation für die Anmeldung.
   
    ![Facebook – App-ID und Anwendungsgeheimnis](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
9. Klicken Sie im linken Navigationsbereich auf **+ Add Product** und dann neben **Facebook Login** auf **Get Started**.
   
    ![Facebook – Facebook-Anmeldung](./media/active-directory-b2c-setup-fb-app/fb-login.png)
10. Geben Sie im Feld **Valid OAuth redirect URIs** im Abschnitt **Client OAuth Settings** den Wert `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. „contosob2c.onmicrosoft.com“). Klicken Sie unten auf der Seite auf **Save Changes**.
    
    ![Facebook – OAuth-Umleitungs-URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
11. Damit Ihre Facebook-Anwendung von Azure AD B2C verwendet werden kann, müssen Sie sie öffentlich verfügbar machen. Klicken Sie hierzu links in der Navigationsleiste auf **App Review**, stellen Sie den Schalter oben auf der Seite auf **Yes**, und klicken Sie auf **Confirm**.
    
    ![Facebook – Veröffentlichen einer App](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Facebook als Identitätsanbieter in Ihrem Mandanten
1. Führen Sie die folgenden Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf dem B2C-Featureblatt auf **Identitätsanbieter**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z.B. „FB“ ein.
5. Klicken Sie auf **Typ des Identitätsanbieters**, wählen Sie **Facebook** aus, und klicken Sie auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**, und geben Sie die App-ID und das Anwendungsgeheimnis (der Facebook-Anwendung, die Sie zuvor erstellt haben) in den Feldern **Client-ID** bzw. **Clientgeheimnis** ein.
7. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Facebook-Konfiguration zu speichern.




<!--HONumber=Nov16_HO3-->


