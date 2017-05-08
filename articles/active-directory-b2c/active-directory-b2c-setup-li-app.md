---
title: 'Azure Active Directory B2C: LinkedIn-Konfiguration | Microsoft Docs'
description: "Bereitstellen von Registrierung und Anmeldung für Kunden mit LinkedIn-Konten in mit Azure Active Directory B2C gesicherten Anwendungen"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: fa51a16b-9ce9-4e27-9eff-0869b4c4f0ef
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
translationtype: Human Translation
ms.sourcegitcommit: e37c48d6c92a8a2cd480458abdff0a3a1ca9338f
ms.openlocfilehash: 3b22dee95d064de8190d7429cc4118884fb81172
ms.lasthandoff: 05/05/2017


---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Bereitstellen von Registrierung und Anmeldung für Kunden mit LinkedIn-Konten
## <a name="create-a-linkedin-application"></a>Erstellen einer LinkedIn-Anwendung
Um LinkedIn als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine LinkedIn-Anwendung erstellen und mit den entsprechenden Parametern bereitstellen. Sie benötigen dazu ein LinkedIn-Konto. Wenn Sie über kein Konto verfügen, können Sie unter [https://www.linkedin.com/](https://www.linkedin.com/) ein Konto erstellen.

1. Navigieren Sie zur [LinkedIn-Entwickler-Website](https://www.developer.linkedin.com/), und melden Sie sich mit den Anmeldeinformationen für Ihr LinkedIn-Konto an.
2. Klicken Sie in der oberen Menüleiste auf **My Apps** und dann auf **Create Application**.
   
    ![LinkedIn – Neue App](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. Geben Sie im Formular **Create a New Application** die relevanten Informationen ein (**Firmenname**, **Name**, **Beschreibung**, **Anwendungslogo-URL**, **Anwendungsverwendung**, **Website-URL**, **E-Mail-Adresse (geschäftlich)** und **Telefon (geschäftlich)**).
4. Akzeptieren Sie die **LinkedIn API Terms of Use**, und klicken Sie auf **Submit**.
   
    ![LinkedIn – Registrieren einer App](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. Kopieren Sie die Werte für **Client ID** und **Client Secret**. (Sie finden sie unter **Authentication Keys**.) Sie benötigen beide Angaben, um LinkedIn als Identitätsanbieter in Ihrem Mandanten zu konfigurieren.
   
   > [!NOTE]
   > Das **Clientgeheimnis** ist eine wichtige Sicherheitsanmeldeinformation.
   > 
   > 
6. Geben Sie im Feld **Authorized Redirect URLs** (unter **OAuth 2.0**) den Wert `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. „contoso.onmicrosoft.com“). Klicken Sie auf **Add** und dann auf **Update**. Beim Wert für **{tenant}** wird die Groß-/Kleinschreibung beachtet.
   
    ![LinkedIn – Einrichten einer App](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von LinkedIn als Identitätsanbieter in Ihrem Mandanten
1. Führen Sie diese Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf dem B2C-Featureblatt auf **Identitätsanbieter**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z.B. „LI“ ein.
5. Klicken Sie auf **Typ des Identitätsanbieters**, wählen Sie **LinkedIn** aus, und klicken Sie auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**, und geben Sie die Client-ID und das Clientgeheimnis der LinkedIn-Anwendung ein, die Sie zuvor erstellt haben.
7. Klicken Sie auf **OK** und dann auf **Erstellen**, um die LinkedIn-Konfiguration zu speichern.


