---
title: "Azure Active Directory B2C: Konfiguration für Amazon | Microsoft Docs"
description: "Bereitstellen von Registrierung und Anmeldung für Kunden mit Amazon-Konten in mit Azure Active Directory B2C gesicherten Anwendungen."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 77c099bb-a005-4d75-87f9-f61e3de48725
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: dcc97e1b7f6287bd7692c52bf068950065a26572
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure Active Directory B2C: Bereitstellen von Registrierung und Anmeldung für Kunden mit Amazon-Konten
## <a name="create-an-amazon-application"></a>Erstellen einer Amazon-Anwendung
Um Amazon als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Amazon-Anwendung erstellen und mit den entsprechenden Parametern bereitstellen. Sie benötigen dazu ein Amazon-Konto. Wenn Sie über kein Konto verfügen, können Sie unter [http://www.amazon.com/](http://www.amazon.com/) ein Konto erstellen.

1. Navigieren Sie zum [Amazon Developers Center](https://login.amazon.com/) (Amazon-Entwicklerzentrum), und melden Sie sich mit den Anmeldeinformationen für Ihr Amazon-Konto an.
2. Wenn Sie dies nicht bereits getan haben, klicken Sie auf **Sign Up**, führen Sie die Schritte zur Registrierung von Entwicklern aus, und akzeptieren Sie die Richtlinie.
3. Klicken Sie auf **Register new application**.
   
    ![Registrieren einer neuen Anwendung auf der Amazon-Website](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. Geben Sie Informationen zur Anwendung unter **Name**, **Description** und **Privacy Notice URL** ein, und klicken Sie auf **Save**.
   
    ![Bereitstellen von Anwendungsinformationen zum Registrieren einer neuen Anwendung bei Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. Kopieren Sie im Abschnitt **Web Settings** die Werte für **Client ID** und **Client Secret**. (Hierzu müssen Sie auf die Schaltfläche **Show Secret** klicken.) Sie benötigen beide Angaben, um Amazon als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. Klicken Sie unten im Abschnitt auf **Edit**. Das **Clientgeheimnis** ist eine wichtige Sicherheitsanmeldeinformation.
   
    ![Bereitstellen von Client-ID und Clientgeheimnis für die neue Anwendung bei Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. Geben Sie im Feld `https://login.microsoftonline.com`Allowed JavaScript Origins **den Wert** und im Feld **Allowed Return URLs** den Wert `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. „contoso.onmicrosoft.com“). Klicken Sie auf **Save**. Beim Wert für **{tenant}** wird die Groß-/Kleinschreibung beachtet.
   
    ![Bereitstellen von JavaScript-Ursprüngen und Rückgabe-URLs für die neue Anwendung bei Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Amazon als Identitätsanbieter in Ihrem Mandanten
1. Führen Sie die folgenden Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicken Sie auf dem B2C-Featureblatt auf **Identitätsanbieter**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z.B. „Amzn“ ein.
5. Klicken Sie auf **Typ des Identitätsanbieters**, wählen Sie **Amazon** aus, und klicken Sie auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**, und geben Sie die Client-ID und das Clientgeheimnis der Amazon-Anwendung ein, die Sie zuvor erstellt haben.
7. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Amazon-Konfiguration zu speichern.


