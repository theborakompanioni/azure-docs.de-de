---
title: "Azure Active Directory B2C: Konfiguration für QQ | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Registrierung und Anmeldung für Kunden mit QQ-Konten in Ihren mit Azure Active Directory B2C gesicherten Anwendungen bereitstellen."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 18c2cf94-8004-4de1-81c2-e45be65ce12d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 4448b945be947847293beb710929adc77f19c945
ms.lasthandoff: 03/28/2017


---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-qq-accounts"></a>Azure Active Directory B2C: Bereitstellen von Registrierung und Anmeldung für Kunden mit QQ-Konten

> [!NOTE]
> Dieses Feature befindet sich noch in der Vorschauphase.
> 

## <a name="create-a-qq-application"></a>Erstellen einer QQ-Anwendung

Um QQ als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine QQ-Anwendung erstellen und die entsprechenden Parameter bereitstellen. Sie benötigen dazu ein QQ-Konto. Wenn Sie kein Konto besitzen, können Sie dieses unter folgendem Link erstellen: [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).

### <a name="register-for-the-qq-developer-program"></a>Registrieren beim QQ-Entwicklerprogramm

1. Navigieren Sie zum [QQ-Entwicklerportal](http://open.qq.com) und melden Sie sich mit den Anmeldeinformationen Ihres QQ-Kontos an.
2. Rufen Sie nach der Anmeldung die Seite [http://open.qq.com/reg](http://open.qq.com/reg) auf, um sich als Entwickler zu registrieren.
3. Wählen Sie im Menü **个人** (Einzelentwickler).
4. Geben Sie die erforderlichen Informationen in das Formular ein und klicken Sie auf **下一步** (Nächster Schritt).
5. Schließen Sie den E-Mail-Überprüfungsprozess ab.

> [!NOTE]
> Es dauert einige Tage, bis Ihre Registrierung als Entwickler genehmigt wird. 

### <a name="register-a-qq-application"></a>Registrieren einer QQ-Anwendung

1. Rufen Sie die Seite [https://connect.qq.com/index.html](https://connect.qq.com/index.html) auf.
2. Klicken Sie auf **应用管理** (App-Verwaltung).
3. Klicken Sie auf **创建应用** (App erstellen).
4. Geben Sie die erforderlichen App-Informationen ein.
5. Klicken Sie auf **创建应用** (App erstellen).
6. Geben Sie die erforderlichen Informationen ein.
7. Geben Sie in das Feld **授权回调域** (Rückruf-URL) `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` ein. Wenn Ihr `tenant_name` beispielsweise „contoso.onmicrosoft.com“ lautet, setzen Sie die URL auf `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
8. Klicken Sie auf **创建应用** (App erstellen).
9. Klicken Sie auf der Bestätigungsseite auf **应用管理** (App-Verwaltung), um zur Seite für die App-Verwaltung zurückzukehren.
10. Klicken Sie neben der App, die Sie gerade erstellt haben, auf **查看** (Anzeigen).
11. Klicken Sie auf **修改** (Bearbeiten).
12. Kopieren Sie im oberen Bereich der Seite die **APP-ID** und den **APP-SCHLÜSSEL**.

## <a name="configure-qq-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von QQ als Identitätsanbieter in Ihrem Mandanten
1. Führen Sie die folgenden Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) .
2. Klicken Sie auf dem B2C-Featureblatt auf **Identitätsanbieter**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z.B. „QQ“ ein.
5. Klicken Sie auf **Identitätsanbietertyp**, wählen Sie **QQ** und klicken Sie auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**.
7. Geben Sie den **App-Schlüssel** ein, den Sie zuvor als **Client-ID** kopiert haben.
8. Geben Sie das **App-Geheimnis** ein, das Sie zuvor als **Clientgeheimnis** kopiert haben.
9. Klicken Sie auf **OK** und dann auf **Erstellen**, um die QQ-Konfiguration zu speichern.


