---
title: 'Azure Active Directory: App-Registrierung | Microsoft-Dokumentation'
description: In diesem Artikel wird beschrieben, wie Sie mithilfe des Azure-Portals eine Anwendung bei Azure Active Directory registrieren.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 7dc7b89f-653f-405a-b5f4-2c1288720c15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2016
ms.author: priyamo
translationtype: Human Translation
ms.sourcegitcommit: bf1b8783ccbc3b3e3185eb10c7f772b58352aab7
ms.openlocfilehash: 073940bb607b953af324d784a45958977e1af3a2

---

# <a name="register-your-application-with-your-azure-active-directory-tenant"></a>Registrieren Ihrer Anwendung bei Ihrem Azure Active Directory-Mandanten

Sie können Ihre Anwendung mithilfe des Azure-Portals beim Azure Active Directory-Mandanten (Azure AD) registrieren. Dabei wird eine Anwendungs-ID für die Anwendung erstellt und für den Empfang von Token aktiviert.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie Ihren Azure AD-Mandanten aus, indem Sie in der oberen rechten Ecke der Seite Ihr Konto wählen.
3. Wählen Sie im linken Navigationsbereich **Weitere Dienste**, und klicken Sie auf **App-Registrierungen** und auf **Hinzufügen**.
4. Folgen Sie der Anleitung, und erstellen Sie eine neue Anwendung. Spezifische Beispiele für Webanwendungen oder native Anwendungen finden Sie in unseren [Schnellstarts](active-directory-developers-guide.md).
  * Geben Sie für Webanwendungen die **Anmelde-URL** an. Dies ist die Basis-URL Ihrer App, unter der sich Benutzer anmelden können, z.B. `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Geben Sie für native Anwendungen einen **Umleitungs-URI** an, der von Azure AD zum Zurückgeben von Tokenantworten verwendet wird. Geben Sie einen für Ihre Anwendung spezifischen Wert ein, z.B. `http://MyFirstAADApp`.
5. Nach Abschluss der Registrierung weist Azure AD Ihrer Anwendung eine eindeutige Client-ID zu, die Anwendungs-ID.

## <a name="update-application-settings-from-the-azure-portal"></a>Aktualisieren von Anwendungseinstellungen im Azure-Portal

Mit dem Azure-Portal können Sie einfach die Einstellungen einer vorhandenen Anwendung ändern. Sie können beispielsweise eine Antwort-URL konfigurieren, unter der Azure AD Tokenantworten ausgibt. Konfigurieren Sie ggf. auch Berechtigungen für andere Anwendungen, beispielsweise um Ihrer Anwendung den Zugriff auf die MS Graph-API zu ermöglichen. Alle diese Schritte können Sie über die Seite mit den Anwendungseinstellungen ausführen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie Ihren Azure AD-Mandanten aus, indem Sie in der oberen rechten Ecke der Seite Ihr Konto wählen.
3. Wählen Sie im linken Navigationsbereich **Weitere Dienste**, klicken Sie auf **App-Registrierungen**, und wählen Sie Ihre Anwendung in der Liste aus.
4. Klicken Sie auf **Einstellungen**, um die Seite „Einstellungen“ für die Anwendung zu öffnen.
  * Auf der Seite **Eigenschaften** können Sie die allgemeine Informationen für die Anwendung ändern. Dazu gehören Anwendungsname, Anmelde-URL und Abmelde-URL.
  * Auf der Seite **Antwort-URLs** können Sie eine Antwort-URL hinzufügen, an die Azure AD Tokenantworten sendet.
  * Auf der Seite **Besitzer** können Sie Anwendungsbesitzer hinzufügen.
  * Auf der Seite **Berechtigungen** können Sie Berechtigungen für die App konfigurieren. Beispiel: Klicken Sie zum Zugreifen auf die MS Graph-API auf **Hinzufügen**, und wählen Sie in der API-Auswahl **Microsoft Graph** aus. Wählen Sie anschließend die erforderliche Berechtigung, z.B. **Verzeichnisdaten lesen**.
  * Auf der Seite **Schlüssel** können Sie Anwendungsgeheimnisse hinzufügen. Das Geheimnis wird nur einmal direkt nach der Erstellung angezeigt. Kopieren Sie es daher zur späteren Verwendung.

## <a name="use-the-inline-manifest-editor"></a>Verwenden des Inline-Manifest-Editors

Mithilfe des Inline-Manifest-Editors können Sie bestimmte Anwendungseigenschaften ändern, die nicht direkt im Azure-Portal verfügbar gemacht werden. Sie können ihn beispielsweise zum Ändern des App-ID-URIs der Anwendung oder zum Aktivieren des impliziten OAuth2.0-Flows anstelle des Standardflows für den Autorisierungsgewährungscode verwenden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie Ihren Azure AD-Mandanten aus, indem Sie in der oberen rechten Ecke der Seite Ihr Konto wählen.
3. Wählen Sie im linken Navigationsbereich **Weitere Dienste**, klicken Sie auf **App-Registrierungen**, und wählen Sie Ihre Anwendung in der Liste aus.
4. Klicken Sie auf der Anwendungsseite auf **Manifest**, um den Inline-Manifest-Editor zu öffnen.
5. Sie können das Manifest direkt ändern und anschließend speichern. Alternativ können Sie das Manifest herunterladen, um es in Ihrem bevorzugten Editor zu öffnen, und anschließend das aktualisierte Manifest hochladen.

## <a name="next-steps"></a>Nächste Schritte

1. In den [Schnellstarts](active-directory-developers-guide.md) finden Sie ausführliche Vorgehensweisen für Anwendungen, die die Authentifizierung mithilfe von Azure AD ausführen.
2. Sehen Sie sich die vollständige Liste der Codebeispiele auf [Github](https://github.com/azure-samples) an.



<!--HONumber=Jan17_HO1-->


