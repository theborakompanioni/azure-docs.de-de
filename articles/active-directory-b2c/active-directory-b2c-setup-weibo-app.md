---
title: "Azure Active Directory B2C: Konfiguration für Weibo | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Registrierung und Anmeldung für Kunden mit Weibo-Konten in Ihren mit Azure Active Directory B2C gesicherten Anwendungen bereitstellen."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 1860de34-94cb-4ceb-851e-102f930f7230
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: b6fdb9311ca2be3752c4ef74c91fe7abe9a91210
ms.lasthandoff: 04/07/2017


---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-weibo-accounts"></a>Azure Active Directory B2C: Bereitstellen von Registrierung und Anmeldung für Kunden mit Weibo-Konten

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase. Verwenden Sie diesen Identitätsanbieter nicht in Ihrer Produktionsumgebung.
> 

## <a name="create-a-weibo-application"></a>Erstellen einer Weibo-Anwendung

Um Weibo als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Weibo-Anwendung erstellen und die entsprechenden Parameter bereitstellen. Sie benötigen dazu ein Weibo-Konto. Wenn Sie kein Konto besitzen, können Sie dieses unter folgendem Link erstellen: [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).

### <a name="register-for-the-weibo-developer-program"></a>Registrieren beim Weibo-Entwicklerprogramm

1. Navigieren Sie zum [Weibo-Entwicklerportal](http://open.weibo.com/) und melden Sie sich mit den Anmeldeinformationen Ihres Weibo-Kontos an.
2. Klicken Sie nach der Anmeldung oben rechts auf Ihren Anzeigenamen.
3. Wählen Sie in der Dropdownliste **编辑开发者信息** (Entwicklerinformationen bearbeiten).
4. Geben Sie die erforderlichen Informationen in das Formular ein und klicken Sie auf **提交** (Absenden).
5. Schließen Sie den E-Mail-Überprüfungsprozess ab.
6. Rufen Sie die Seite [Identitätsüberprüfung](http://open.weibo.com/developers/identity/edit) auf.
7. Geben Sie die erforderlichen Informationen in das Formular ein und klicken Sie auf **提交** (Absenden).

### <a name="register-a-weibo-application"></a>Registrieren einer Weibo-Anwendung

1. Navigieren Sie zur Seite für die [Registrierung einer neuen Weibo-App](http://open.weibo.com/apps/new).
2. Geben Sie die erforderlichen App-Informationen ein.
3. Klicken Sie auf **创建** (Erstellen).
4. Kopieren Sie die Werte von **App-Schlüssel** und **App-Geheimnis**. Sie benötigen sie später.
5. Laden Sie die erforderlichen Fotos hoch und geben Sie die benötigten Informationen ein.
6. Klicken Sie auf **保存以上信息** (Speichern).
7. Klicken Sie auf **高级信息** (Erweiterte Informationen).
8. Klicken Sie auf **编辑** (Bearbeiten) neben dem Feld für **授权设置** (Umleitungs-URL) von OAuth 2.0.
9. Geben Sie `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` für **授权设置** (Umleitungs-URL) von OAuth 2.0 ein. Wenn Ihr `tenant_name` beispielsweise „contoso.onmicrosoft.com“ lautet, setzen Sie die URL auf `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
10. Klicken Sie auf **提交** (Absenden).  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Weibo als Identitätsanbieter in Ihrem Mandanten
1. Führen Sie die folgenden Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) .
2. Klicken Sie auf dem B2C-Featureblatt auf **Identitätsanbieter**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z.B. „Weibo“ ein.
5. Klicken Sie auf **Identitätsanbietertyp**, wählen Sie **Weibo** und klicken Sie auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**.
7. Geben Sie den **App-Schlüssel** ein, den Sie zuvor als **Client-ID** kopiert haben.
8. Geben Sie das **App-Geheimnis** ein, das Sie zuvor als **Clientgeheimnis** kopiert haben.
9. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Weibo-Konfiguration zu speichern.


