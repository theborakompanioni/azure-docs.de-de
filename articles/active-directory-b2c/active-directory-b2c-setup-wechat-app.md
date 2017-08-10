---
title: "Azure Active Directory B2C: Konfiguration für WeChat | Microsoft-Dokumentation"
description: "Bereitstellen von Registrierung und Anmeldung für Kunden mit WeChat-Konten in mit Azure Active Directory B2C geschützten Anwendungen."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: d2424c66-ba68-4d82-847e-d137683527b0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: a54aec23d951610118246e9f70cdd27752ef39a6
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-wechat-accounts"></a>Azure Active Directory B2C: Bereitstellen von Registrierung und Anmeldung für Kunden mit WeChat-Konten

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase.
> 

## <a name="create-a-wechat-application"></a>Erstellen einer WeChat-Anwendung

Um WeChat als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine WeChat-Anwendung erstellen und die entsprechenden Parameter bereitstellen. Sie benötigen dazu ein WeChat-Konto. Wenn Sie über kein Konto verfügen, können Sie es durch Registrieren über eine der entsprechenden mobilen Apps oder durch Verwenden Ihrer QQ-Nummer erstellen. Registrieren Sie Ihr Konto anschließend beim WeChat-Entwicklerprogramm. Weitere Informationen finden Sie [hier](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Registrieren einer WeChat-Anwendung

1. Rufen Sie [https://open.weixin.qq.com/](https://open.weixin.qq.com/) auf, und melden Sie sich an.
2. Klicken Sie auf **管理中心** (Management Center).
3. Führen Sie die erforderlichen Schritte zum Registrieren einer neuen Anwendung aus.
4. Geben Sie unter **授权回调域** (Rückruf-URL) `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` ein. Wenn Ihr `tenant_name` beispielsweise „contoso.onmicrosoft.com“ lautet, legen Sie die URL auf `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp` fest.
5. Suchen und kopieren Sie die **APP-ID** und den **ANWENDUNGSSCHLÜSSEL**. Diese werden später benötigt.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von WeChat als Identitätsanbieter in Ihrem Mandanten
1. Führen Sie die folgenden Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) .
2. Klicken Sie auf dem B2C-Featureblatt auf **Identitätsanbieter**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z.B. „WeChat“ ein.
5. Klicken Sie auf **Identitätsanbietertyp**, wählen Sie **WeChat** aus, und klicken Sie auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**.
7. Geben Sie den **App-Schlüssel** ein, den Sie zuvor als **Client-ID** kopiert haben.
8. Geben Sie das **App-Geheimnis** ein, das Sie zuvor als **Clientgeheimnis** kopiert haben.
9. Klicken Sie auf **OK** und dann auf **Erstellen**, um die WeChat-Konfiguration zu speichern.


