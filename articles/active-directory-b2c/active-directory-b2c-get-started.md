---
title: 'Azure Active Directory B2C: Erstellen eines Azure Active Directory B2C-Mandanten | Microsoft Docs'
description: Thema zum Erstellen eines Azure Active Directory B2C-Mandanten
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 01/26/2017
ms.author: swkrish
translationtype: Human Translation
ms.sourcegitcommit: 351149296a6d7dfa801b295ec21fc04215c7b051
ms.openlocfilehash: 77b8e707b5743ce2d0b3eb52f75828c9ab0337d9


---
# <a name="azure-active-directory-b2c-create-an-azure-ad-b2c-tenant"></a>Azure Active Directory B2C: Erstellen eines Azure AD B2C-Mandanten
Zum Einstieg in Microsoft Azure Active Directory (Azure AD) B2C führen Sie die drei Schritte aus, die in diesem Artikel beschrieben werden.

## <a name="step-1-sign-up-for-an-azure-subscription"></a>Schritt 1: Registrieren für ein Azure-Abonnement
Wenn Sie bereits über ein Azure-Abonnement verfügen, überspringen Sie diesen Schritt. Andernfalls registrieren Sie sich für ein [Azure-Abonnement](../active-directory/sign-up-organization.md) , um Zugriff auf Azure AD B2C zu erhalten.

## <a name="step-2-create-an-azure-ad-b2c-tenant"></a>Schritt 2: Erstellen eines Azure AD B2C-Mandanten
Führen Sie die folgenden Schritte aus, um einen neuen Azure AD B2C-Mandanten zu erstellen. Derzeit können B2C-Features in Ihren vorhandenen Mandanten noch nicht aktiviert werden.

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/) als Abonnementadministrator an. Dies ist dasselbe Geschäfts-, Schul- oder Unikonto bzw. dasselbe Microsoft-Konto, mit dem Sie sich bei Azure registriert haben.
2. Klicken Sie auf **Neu** > **App Services** > **Active Directory** > **Directory** > **Benutzerdefiniert erstellen**.
   
    ![Screenshot der ersten Schritte zum Erstellen eines Mandanten](./media/active-directory-b2c-get-started/new-directory.png)
3. Wählen Sie **Name**, **Domänenname** und **Land oder Region** für den Mandanten aus.
4. Aktivieren Sie die Option **Hierbei handelt es sich um ein B2C-Verzeichnis**.
5. Klicken Sie auf das Häkchen, um die Aktion abzuschließen.
   
    ![Screenshot des Kontrollkästchens zum Erstellen eines B2C-Verzeichnisses](./media/active-directory-b2c-get-started/create-b2c-directory.png)
6. Der Mandant wird jetzt erstellt und in der Active Directory-Erweiterung aufgeführt. Außerdem werden Sie als globaler Administrator des Mandanten festgelegt. Sie können bei Bedarf weitere globale Administratoren hinzufügen.
   
   > [!IMPORTANT]
   > Wenn Sie einen B2C-Mandanten für eine Produktions-App verwenden möchten, lesen Sie den Artikel zum [Vergleich zwischen Produktionsmandanten und B2C-Vorschaumandanten](active-directory-b2c-reference-tenant-type.md). Beachten Sie, dass beim Löschen eines vorhandenen B2C-Mandanten und erneuten Erstellen mit demselben Domänennamen bekannte Probleme auftreten. Sie müssen einen B2C-Mandanten mit einem anderen Domänennamen erstellen.
   > 
   > 

## <a name="step-3-navigate-to-the-b2c-features-blade-on-the-azure-portal"></a>Schritt 3: Navigieren zum Blatt „B2C-Funktionen“ im Azure-Portal
1. Navigieren Sie auf der Navigationsleiste links zur Active Directory-Erweiterung.
2. Suchen Sie Ihren Mandanten auf der Registerkarte **Verzeichnis** , und klicken Sie darauf.
3. Klicken Sie auf die Registerkarte **Configure** .
4. Klicken Sie im Abschnitt **B2C-Verwaltung** auf den Link **B2C-Einstellungen verwalten**.
   
    ![Screenshot der Verzeichniskonfiguration für B2C](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)
5. Das Azure-Portal mit dem Blatt „B2C-Funktionen“ wird auf einer neuen Registerkarte oder in einem neuen Fenster des Browsers geöffnet.
   
   > [!IMPORTANT]
   > Sie können im Azure-Portal erst nach etwa zwei bis drei Minuten auf den Mandanten zugreifen. Sollte ein Problem auftreten, warten Sie etwas, und führen Sie die Schritte erneut aus. Wenn dies nicht der Fall ist, wenden Sie sich an den Support.
   > 
   > 
6. Heften Sie dieses Blatt an Ihr Startmenü an, um auf einfache Weise darauf zugreifen zu können. (Das Tool zum Anheften ist oben rechts auf dem Featureblatt rot gekennzeichnet.)
   
    ![Screenshot des Blatts „B2C-Funktionen“](./media/active-directory-b2c-get-started/b2c-features-blade.png)
   
   > [!NOTE]
   > Im [klassischen Azure-Portal](https://manage.windowsazure.com/)können Sie Benutzer und Gruppen, die Konfiguration für die Self-Service-Kennwortzurücksetzung und die Features des Unternehmensbrandings für Ihren Mandanten verwalten.
   > 
   > 

## <a name="step-4-link-your-azure-ad-b2c-tenant-to-your-azure-subscription"></a>Schritt 4: Verknüpfen Ihres Azure AD B2C-Mandanten mit Ihrem Azure-Abonnement
Wenn Sie Ihren B2C-Mandanten für Produktions-Apps verwenden möchten, müssen Sie Ihren Azure AD B2C-Mandanten mit Ihrem Azure-Abonnement verknüpfen, um für Nutzungsgebühren zu bezahlen. Weitere Informationen hierzu finden Sie in [diesem Artikel](active-directory-b2c-how-to-enable-billing.md).

   > [!IMPORTANT]
   > Wenn Sie Ihren Azure AD B2C-Mandanten nicht mit Ihrem Azure-Abonnement verknüpfen, wird Ihnen im Azure-Portal auf dem Blatt mit den B2C-Funktionen eine Warnmeldung angezeigt („Mit diesem B2C-Mandanten ist kein Abonnement verknüpft, oder das Abonnement benötigt Ihre Aufmerksamkeit.“). Es ist wichtig, dass Sie diesen Schritt ausführen, bevor Sie Ihre Apps in die Produktion übertragen.
   > 
   > 

## <a name="easy-access-to-the-b2c-features-blade-on-the-azure-portal"></a>Einfacher Zugriff auf das Blatt „B2C-Funktionen“ im Azure-Portal
Um die Auffindbarkeit zu verbessern, haben wir eine Verknüpfung zum Blatt „B2C-Funktionen“ im Azure-Portal hinzugefügt.

1. Melden Sie sich als globaler Administrator Ihres B2C-Mandanten im Azure-Portal an. Wenn Sie bereits bei einem anderen Mandanten angemeldet sind, wechseln Sie die Mandanten (in der oberen rechten Ecke).
2. Klicken Sie im linken Navigationsbereich auf **Durchsuchen** .
3. Klicken Sie auf **Azure AD B2C** , um auf das Blatt „B2C-Funktionen“ zuzugreifen.
   
    ![Screenshot des Blatts „Navigieren zu B2C-Funktionen“](./media/active-directory-b2c-get-started/b2c-browse.png)

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Registrieren einer Anwendung in Azure AD B2C und zum Erstellen einer Schnellstartanwendung finden Sie unter [Azure Active Directory B2C: Registrieren der Anwendung](active-directory-b2c-app-registration.md).




<!--HONumber=Jan17_HO4-->


