---
title: "Azure Active Directory B2C: Konfiguration für Facebook | Microsoft Docs"
description: "Bereitstellen von Registrierung und Anmeldung für Kunden mit Facebook-Konten in mit Azure Active Directory B2C gesicherten Anwendungen."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: b875f235-a1d2-4abb-b9f0-b89beac38a32
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: 4c45322573bd1e4b1711b56e03c1d297f1cd468e
ms.lasthandoff: 03/29/2017


---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory B2C: Bereitstellen von Registrierung und Anmeldung für Kunden mit Facebook-Konten
## <a name="create-a-facebook-application"></a>Erstellen einer Facebook-Anwendung
Um Facebook als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Facebook-Anwendung erstellen und mit den entsprechenden Parametern bereitstellen. Sie benötigen dazu ein Facebook-Konto. Wenn Sie über kein Konto verfügen, können Sie unter [https://www.facebook.com/](https://www.facebook.com/)eines erstellen.

1. Navigieren Sie zur [Facebook-Entwickler-Website](https://developers.facebook.com/), und melden Sie sich mit den Anmeldeinformationen für Ihr Facebook-Konto an.
2. Wenn Sie dies noch nicht getan haben, müssen Sie sich als Facebook-Entwickler registrieren. Dazu klicken Sie in der oberen rechten Ecke der Seite auf **Register**, akzeptieren die Facebook-Richtlinien und führen die Registrierungsschritte aus.
3. Klicken Sie auf **My Apps** (Meine Apps) und dann auf **Add a New App** (Neue App hinzufügen). 
4. Geben Sie im Formular unter **Display Name** einen Anzeigenamen und unter **Contact Email** (Kontakt-E-Mail) eine gültige Adresse an.
5. Klicken Sie auf **Create App ID** (App-ID erstellen). Hierzu müssen Sie möglicherweise die Richtlinien für die Facebook-Plattform akzeptieren und eine Online-Sicherheitsüberprüfung durchführen.
6. Klicken Sie in der linken Spalte auf **Settings** (Einstellungen), und wählen Sie dann **Basic** (Allgemein) aus, falls noch nicht ausgewählt.
7. Wählen Sie unter **Category** eine Kategorie aus. 
8. Klicken Sie auf **+ Add Platform** (+ Plattform hinzufügen), und wählen Sie **Website** aus.
   
    ![Facebook – Einstellungen](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook – Einstellungen – Website](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. Geben Sie `https://login.microsoftonline.com/` im Feld **Site URL** (Website-URL) ein, und klicken Sie dann auf **Save Changes** (Änderungen speichern).
   
    ![Facebook – Website-URL](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. Kopieren Sie den Wert unter **App ID**. Klicken Sie auf **Show**, und kopieren Sie den Wert unter **App Secret**. Sie benötigen beide Angaben, um Facebook als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. **App Secret** handelt es sich um eine wichtige Sicherheitsinformation für die Anmeldung.
   
    ![Facebook – App-ID und Anwendungsgeheimnis](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. Klicken Sie im linken Navigationsbereich auf **+ Add Product** und dann neben **Facebook Login** auf **Get Started**.
   
    ![Facebook – Facebook-Anmeldung](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. Wählen Sie **Website** aus, und geben Sie dann im Feld **Valid OAuth redirect URIs** (Gültige OAuth-Umleitungs-URIs) im Abschnitt **Client OAuth Settings** (Client-OAuth-Einstellungen) den Wert `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. „contosob2c.onmicrosoft.com“). Klicken Sie unten auf der Seite auf **Save Changes**.
    
    ![Facebook – OAuth-Umleitungs-URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
13. Damit Ihre Facebook-Anwendung von Azure AD B2C verwendet werden kann, müssen Sie sie öffentlich verfügbar machen. Klicken Sie hierzu links in der Navigationsleiste auf **App Review**, stellen Sie den Schalter oben auf der Seite auf **Yes**, und klicken Sie auf **Confirm**.
    
    ![Facebook – Veröffentlichen einer App](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Facebook als Identitätsanbieter in Ihrem Mandanten
1. Führen Sie die folgenden Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf dem B2C-Featureblatt auf **Identitätsanbieter**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z.B. „FB“ ein.
5. Klicken Sie auf **Typ des Identitätsanbieters**, wählen Sie **Facebook** aus, und klicken Sie auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**, und geben Sie die App-ID und das Anwendungsgeheimnis (der Facebook-Anwendung, die Sie zuvor erstellt haben) in den Feldern **Client-ID** bzw. **Clientgeheimnis** ein.
7. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Facebook-Konfiguration zu speichern.


