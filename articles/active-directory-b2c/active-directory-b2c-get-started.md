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
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 02bb7286f6cd42f86e6cc920742a86d57ed93b60
ms.lasthandoff: 03/09/2017


---
# <a name="azure-active-directory-b2c-create-an-azure-ad-b2c-tenant"></a>Azure Active Directory B2C: Erstellen eines Azure AD B2C-Mandanten
Zum Einstieg in Microsoft Azure Active Directory (Azure AD) B2C führen Sie die vier Schritte aus, die in diesem Artikel beschrieben werden.

## <a name="step-1-sign-up-for-an-azure-subscription"></a>Schritt 1: Registrieren für ein Azure-Abonnement
Wenn Sie bereits über ein Azure-Abonnement verfügen, überspringen Sie diesen Schritt. Andernfalls registrieren Sie sich für ein [Azure-Abonnement](../active-directory/sign-up-organization.md) , um Zugriff auf Azure AD B2C zu erhalten.

## <a name="step-2-create-an-azure-ad-b2c-tenant"></a>Schritt 2: Erstellen eines Azure AD B2C-Mandanten
Führen Sie die folgenden Schritte aus, um einen neuen Azure AD B2C-Mandanten zu erstellen. Derzeit können B2C-Features in Ihren vorhandenen Mandanten noch nicht aktiviert werden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als Abonnementadministrator an. Dies ist dasselbe Geschäfts-, Schul- oder Unikonto bzw. dasselbe Microsoft-Konto, mit dem Sie sich bei Azure registriert haben.
2. Klicken Sie auf **Neu** (bzw. bei reduzierter Ansicht auf die Schaltfläche „+“), und geben Sie im Feld **Marketplace durchsuchen** „Azure Active Directory B2C“ ein.
   
    ![Screenshot der Suche nach der Azure AD B2C-Option](./media/active-directory-b2c-get-started/find-azure-ad-b2c.png)
3. Klicken Sie im Ergebnis auf **Azure Active Directory B2C**.

    ![Screenshot des Ergebnisses für Azure Active Directory B2C](./media/active-directory-b2c-get-started/find-azure-ad-b2c-result.png)
4. Eine Seite mit den Details zu B2C wird angezeigt.  Klicken Sie unten auf **Erstellen**, um mit der Konfiguration Ihres neuen Azure Active Directory B2C-Mandanten zu beginnen.
5. Klicken Sie auf die Option zum **Erstellen eines neuen Azure AD B2C-Mandanten**.
6. Wählen Sie den **Organisations- und Domänennamen sowie Land oder Region** für den Mandanten aus.

    ![Screenshot des Formulars zum Erstellen eines neuen Mandanten](./media/active-directory-b2c-get-started/create-new-b2c-tenant.png)
7. Klicken Sie auf **Erstellen**, um den Mandanten zu erstellen.  Dies kann einige Minuten dauern. Sie werden benachrichtigt, wenn der Vorgang abgeschlossen ist.

8. Der Mandant wird jetzt erstellt und in der Active Directory-Erweiterung aufgeführt. Außerdem werden Sie als globaler Administrator des Mandanten festgelegt. Sie können bei Bedarf weitere globale Administratoren hinzufügen.
   
   > [!IMPORTANT]
   > Wenn Sie einen B2C-Mandanten für eine Produktions-App verwenden möchten, lesen Sie den Artikel zum [Vergleich zwischen Produktionsmandanten und B2C-Vorschaumandanten](active-directory-b2c-reference-tenant-type.md). Beachten Sie, dass beim Löschen eines vorhandenen B2C-Mandanten und erneuten Erstellen mit demselben Domänennamen bekannte Probleme auftreten. Sie müssen einen B2C-Mandanten mit einem anderen Domänennamen erstellen.
   > 
   > 

## <a name="step-3-navigate-to-the-b2c-features-blade-on-the-azure-portal"></a>Schritt 3: Navigieren zum Blatt „B2C-Funktionen“ im Azure-Portal
1. Erweitern Sie auf der linken Seite unterhalb der Navigationsleiste die Option **Weitere Dienste**.
2. Suchen Sie nach **Azure AD B2C**, und klicken Sie auf das Ergebnis (Sie können für den künftigen Zugriff einen Favoriten dafür anlegen).

    ![Screenshot der Suche nach Azure AD B2C im Navigationsbereich](./media/active-directory-b2c-get-started/navigate-to-azure-ad-b2c.png)

3. Das Azure-Portal mit dem Blatt „B2C-Funktionen“ wird auf einer neuen Registerkarte oder in einem neuen Fenster des Browsers geöffnet.
   
4. Heften Sie dieses Blatt an Ihr Startmenü an, um auf einfache Weise darauf zugreifen zu können. (Das Tool zum Anheften befindet sich oben rechts auf dem Featureblatt.)
   
    ![Screenshot des B2C-Featureblatts und der Schaltfläche zum Anheften](./media/active-directory-b2c-get-started/b2c-pin-tenant.png)

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


