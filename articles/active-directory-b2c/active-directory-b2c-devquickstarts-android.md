---
title: 'Azure Active Directory B2C: Anfordern eines Tokens mit einer Android-Anwendung | Microsoft-Dokumentation'
description: "In diesem Artikel erfahren Sie, wie Sie eine Android-App erstellen, die Appauth mit Azure Active Directory B2C zum Verwalten von Benutzeridentitäten und Authentifizieren von Benutzern verwendet."
services: active-directory-b2c
documentationcenter: android
author: parakhj
manager: krassk
editor: 
ms.assetid: d00947c3-dcaa-4cb3-8c2e-d94e0746d8b2
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 03/06/2017
ms.author: parakhj
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: fdf9f494f30f11a00831b6c56a3d6ac40582c0ad
ms.lasthandoff: 03/14/2017


---
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a>Azure AD B2C: Anmelden mit einer Android-Anwendung

Die Microsoft Identity-Plattform nutzt offene Standards, z.B. OAuth2 und OpenID Connect. Dadurch können Entwickler eine beliebige Bibliothek verwenden, um sie in unsere Dienste zu integrieren. Um Entwickler bei der Nutzung unserer Plattform mit anderen Bibliotheken zu unterstützen, haben wir einige exemplarische Vorgehensweisen wie diese erstellt. Darin wird veranschaulicht, wie Sie Drittanbieterbibliotheken konfigurieren, um eine Verbindung mit der Microsoft Identity-Plattform herzustellen. Die meisten Bibliotheken, die die [RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749)-Spezifikation implementieren, können eine Verbindung mit der Microsoft Identity-Plattform herstellen.

> [!WARNING]
> Microsoft stellt keine Fehlerbehebungen für diese Drittanbieterbibliotheken bereit und hat diese Bibliotheken keinerlei Prüfungen unterzogen. In diesem Beispiel wird eine Drittanbieterbibliothek namens AppAuth verwendet, deren Kompatibilität in grundlegenden Szenarien mit Azure AD B2C getestet wurde. Probleme und Funktionsanforderungen müssen direkt an das Open Source-Projekt der Bibliothek weitergeleitet werden. Weitere Informationen hierzu finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).  
>
>

Wenn OAuth2 oder OpenID Connect neu für Sie ist, ergibt diese Beispielkonfiguration für Sie vielleicht noch nicht viel Sinn. Wir empfehlen Ihnen, sich eine kurze [Übersicht über das Protokoll](active-directory-b2c-reference-protocols.md)anzusehen.

Nicht alle Szenarien und Funktionen von Azure Active Directory werden von der B2C-Plattform unterstützt.  Informationen zum Treffen der Entscheidung, ob die Verwendung der B2C-Plattform sinnvoll ist, finden Sie im Artikel zu den [B2C-Einschränkungen](active-directory-b2c-limitations.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Erstellen eines Azure AD B2C-Verzeichnisses

Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis oder einen Mandanten erstellen. Ein Verzeichnis ist ein Container für alle Ihre Benutzer, Apps, Gruppen usw. Erstellen Sie, sofern noch nicht geschehen, [ein B2C-Verzeichnis](active-directory-b2c-get-started.md) , bevor Sie fortfahren.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie in Ihrem B2C-Verzeichnis eine App erstellen. Dadurch werden Azure AD die Informationen bereitgestellt, die für die sichere Kommunikation mit Ihrer App erforderlich sind. Befolgen Sie zum Erstellen einer Mobile App [diese Anweisungen](active-directory-b2c-app-registration.md). Führen Sie folgende Schritte aus:

* Fügen Sie einen **nativen Client** in die Anwendung ein.
* Kopieren Sie die **Anwendungs-ID** , die Ihrer App zugewiesen ist. Sie benötigen sie später.
* Richten Sie eine **Umleitungs-URI** mit einem benutzerdefinierten Schema ein (z.B. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect) Diese benötigen sie später ebenfalls.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen der Richtlinien

In Azure AD B2C wird jede Benutzererfahrung durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Diese App enthält eine kombinierte Identitätsoberfläche für die Anmeldung und Registrierung. Sie müssen diese Richtlinie wie im [Artikel mit Richtlinienreferenzen](active-directory-b2c-reference-policies.md#create-a-sign-up-policy) beschrieben erstellen. Achten Sie beim Erstellen der Richtlinie auf Folgendes:

* Wählen Sie den **Anzeigenamen** und die Registrierungsattribute in der Richtlinie aus.
* Wählen Sie den **Anzeigenamen** und die **Objekt-ID** als Anwendungsansprüche in jeder Richtlinie aus. Sie können auch andere Ansprüche auswählen.
* Notieren Sie sich die **Namen** der einzelnen Richtlinien nach ihrer Erstellung. Sie müssen das Präfix `b2c_1_`aufweisen.  Sie benötigen den Richtliniennamen später noch.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die Richtlinien erstellt haben, können Sie Ihre App erstellen.

## <a name="download-the-sample-code"></a>Herunterladen des Beispielcodes

Wir haben ein funktionsfähiges Beispiel bereitgestellt, das AppAuth mit Azure AD B2C [auf GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c) verwendet. Sie können den Code herunterladen und ausführen. Folgen Sie den Anweisungen in der Datei [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md), um bei den ersten Schritten mit Ihrer eigenen App Ihre eigene Azure AD B2C-Konfiguration zu verwenden.

Das Beispiel ist eine geänderte Version des durch [AppAuth](https://openid.github.io/AppAuth-Android/) bereitgestellten Beispiels. Auf der Seite von AppAuth finden Sie weitere Informationen zu AppAuth und dessen Funktionen.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Ändern Ihrer Anwendung zur Verwendung von Azure AD B2C mit AppAuth

> [!NOTE]
> AppAuth unterstützt Android API 16 (Jellybean) und höher. Wir empfehlen API 23 und höher.
>

### <a name="configuration"></a>Konfiguration

Sie können die Kommunikation mit Azure AD B2C konfigurieren, indem Sie die Ermittlungs-URI oder die Autorisierungsendpunkt- und die Token-Endpunkt-URIs angeben. In beiden Fällen benötigen Sie die folgenden Informationen:

* Mandanten-ID (z.B. contoso.onmicrosoft.com)
* Richtlinienname (z.B. B2C\_1\_SignUpIn)

Falls Sie die Autorisierungs- und Token-Endpunkt-URIs automatisch ermitteln möchten, müssen Sie Informationen aus der Ermittlungs-URI abrufen. Sie können die Ermittlungs-URI generieren, indem Sie Tenant\_ID (Mandanten-ID) und Policy\_Name (Richtlinienname) in der folgenden URL ersetzen:

```java
String mDiscoveryURI = "https://login.microsoftonline.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

Sie können dann die Autorisierungs- und Token-Endpunkt-URIs abrufen und ein AuthorizationServiceConfiguration-Objekt erstellen, indem Sie Folgendes ausführen:

```java
final Uri issuerUri = Uri.parse(mDiscoveryURI);
AuthorizationServiceConfiguration config;

AuthorizationServiceConfiguration.fetchFromIssuer(
    issuerUri,
    new RetrieveConfigurationCallback() {
      @Override public void onFetchConfigurationCompleted(
          @Nullable AuthorizationServiceConfiguration serviceConfiguration,
          @Nullable AuthorizationException ex) {
        if (ex != null) {
            Log.w(TAG, "Failed to retrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed to authorization...
        }
      }
  });
```

Anstatt die Autorisierungs- und Token-Endpunkt-URIs per Ermittlung abzurufen, können Sie diese auch explizit durch Ersetzen von Tenant\_ID (Mandanten-ID) und Policy\_Name (Richtlinienname) in der folgenden URL festlegen:

```java
String mAuthEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

Führen Sie den folgenden Code aus, um Ihr AuthorizationServiceConfiguration-Objekt zu erstellen:

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform the auth request...
```

### <a name="authorizing"></a>Autorisieren

Nach dem Konfigurieren oder Abrufen einer Autorisierungsdienstkonfiguration kann eine Autorisierungsanforderung erstellt werden. Zum Erstellen dieser Anforderung benötigen Sie die folgenden Informationen:

* CLIENT-ID (z.B. 00000000-0000-0000-0000-000000000000)
* Umleitungs-URI mit einem benutzerdefinierten Schema (z.B. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)

Sie sollten beide Elemente beim [Registrieren Ihrer App](#create-an-application) gespeichert haben.

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

Informationen zum restlichen Prozess finden Sie im [AppAuth Handbuch](https://openid.github.io/AppAuth-Android/). Sehen Sie sich [unser Beispiel](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c) für die ersten Schritte mit einer funktionsfähigen App an. Folgen Sie den Anweisungen in der Datei [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) aus, um Ihre eigene Azure AD B2C-Konfiguration einzugeben.

Wir sind stets offen für Feedback und Vorschläge. Wenn Sie Probleme mit diesem Thema oder Vorschläge zur Verbesserung dieses Inhalts haben, würden wir uns über Ihr Feedback unten auf der Seite freuen. Anforderungen neuer Features geben Sie bitte unter [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) ein.


