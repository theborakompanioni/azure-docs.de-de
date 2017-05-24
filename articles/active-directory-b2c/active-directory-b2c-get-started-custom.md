---
title: 'Azure Active Directory B2C: Erste Schritte mit benutzerdefinierten Richtlinien | Microsoft-Dokumentation'
description: "Enthält eine Beschreibung der ersten Schritte für benutzerdefinierte Azure Active Directory B2C-Richtlinien."
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: joroja;parahk;gsacavdm
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 42824fe10e635257681f62ab1fec9b47abd4294a
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017

---
# <a name="azure-active-directory-b2c-getting-started-with-custom-policies"></a>Azure Active Directory B2C: Erste Schritte mit benutzerdefinierten Richtlinien

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Nach der Durchführung der Schritte in diesem Artikel unterstützt Ihre benutzerdefinierte Richtlinie die Registrierung oder Anmeldung mit einem lokalen Konto mithilfe einer E-Mail-Adresse und eines Kennworts. Zudem bereiten Sie Ihre Umgebung auf das Hinzufügen zusätzlicher Identitätsanbieter (z.B. Facebook oder Azure AD) vor.  Bevor alle anderen Einsatzmöglichkeiten des Azure AD B2C-Identity Experience Framework und viele grundlegende Konzepte vorgestellt werden, ist es ratsam, die unten angegebenen Schritte auszuführen.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie fortfahren, stellen Sie sicher, dass Sie über einen Azure AD B2C-Mandanten verfügen. Ein Azure AD B2C-Mandant ist ein Container für all Ihre Benutzer, Apps, Richtlinien etc. Wenn Sie keinen Mandanten besitzen, müssen Sie einen solchen [erstellen](active-directory-b2c-get-started.md). Wir raten allen Entwicklern dringend, die exemplarischen Vorgehensweisen für die integrierten Azure AD B2C-Richtlinien durchzuarbeiten und vor dem Fortfahren die Anwendung mit integrierten Richtlinien zu konfigurieren.  Ihre Anwendungen funktionieren mit beiden Arten von Richtlinien. Es ist nur eine kleine Änderung des Richtliniennamens zum Aufrufen der benutzerdefinierten Richtlinie erforderlich.

Für den Zugriff auf die Bearbeitung einer benutzerdefinierten Richtlinie wird ein gültiges Azure-Abonnement benötigt, das mit Ihrem Mandanten verknüpft ist.

## <a name="add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies"></a>Hinzufügen von Signatur- und Verschlüsselungsschlüsseln im B2C-Mandanten zur Verwendung durch benutzerdefinierte Richtlinien

1. Navigieren Sie in den Einstellungen für den Azure AD B2C-Mandanten zum Blatt **Identity Experience Framework**.
2. Wählen Sie **Policy Keys** (Richtlinienschlüssel), um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen.
3. Erstellen Sie das `B2C_1A_TokenSigningKeyContainer`-Element, falls es nicht vorhanden ist:
 * Klicken Sie auf **Hinzufügen**.
 * Optionen > `Generate`
 * Name > `TokenSigningKeyContainer`. Das Präfix „B2C_1A_“ wird unter Umständen automatisch hinzugefügt.
 * Schlüsseltyp > `RSA`
 * Daten – Standardwerte verwenden
 * Schlüsselverwendung > `Signature`
 * Klicken Sie auf **Erstellen**
4. Erstellen Sie das `B2C_1A_TokenEncryptionKeyContainer`-Element, falls es nicht vorhanden ist:
 * Klicken Sie auf **Hinzufügen**.
 * Optionen > `Generate`
 * Name > `TokenEncryptionKeyContainer`. Das Präfix „B2C_1A_“ wird unter Umständen automatisch hinzugefügt.
 * Schlüsseltyp > `RSA`
 * Daten – Standardwerte verwenden
 * Schlüsselverwendung > `Encryption`
 * Klicken Sie auf **Erstellen**
5. Erstellen Sie `B2C_1A_FacebookSecret`. Wenn Sie bereits über ein Facebook-Anwendungsgeheimnis verfügen, können Sie es Ihrem Mandanten als Richtlinienschlüssel hinzufügen.  Andernfalls müssen Sie den Schlüssel mit Platzhalterwert erstellen, damit Ihre Richtlinien die Überprüfung bestehen.
 * Klicken Sie auf **Hinzufügen**.
 * Optionen > `Manual`
 * Name > `FacebookSecret`. Das Präfix „B2C_1A_“ wird unter Umständen automatisch hinzugefügt.
 * Geheimnis > Geben Sie Ihr FacebookSecret aus „developers.facebook.com“ oder „0“ als Platzhalter ein. *Dies ist nicht Ihre Facebook-App-ID.*
 * Schlüsselverwendung > Signatur
 * Klicken Sie auf **Erstellen**, und bestätigen Sie die Erstellung.

## <a name="register-identity-experience-framework-applications"></a>Registrieren von Identity Experience Framework-Anwendungen

Azure AD B2C erfordert, dass Sie zwei zusätzliche Anwendungen registrieren, die vom Modul zur Registrierung und Anmeldung von Benutzern verwendet werden.

>[!NOTE]
>Wir müssen zwei Anwendungen erstellen, die die Anmeldung mit lokalen Konten ermöglichen: IdentityExperienceFramework (eine Web-App) und ProxyIdentityExperienceFramework (eine native App) mit delegierter Berechtigung von der IdentityExperienceFramework-App. Lokale Konten sind nur in Ihrem Mandanten vorhanden. Ihre Endbenutzer melden sich mit einer eindeutigen Kombination aus E-Mail-Adresse und Kennwort an, um auf Ihre bei Mandanten registrierten Anwendungen zuzugreifen.

### <a name="create-the-identityexperienceframework-application"></a>Erstellen der IdentityExperienceFramework-Anwendung

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) [in den Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md).
1. Öffnen Sie das Blatt `Azure Active Directory` (nicht das Blatt „Azure AD B2C“). Sie müssen möglicherweise auf **> Weitere Dienste** klicken, um danach zu suchen.
1. Wählen Sie **App-Registrierungen** aus.
1. Klicken Sie auf **+ Registrierung einer neuen Anwendung**.
   * Name: `IdentityExperienceFramework`
   * Anwendungstyp: `Web app/API`
   * Anmelde-URL: `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`, wobei `yourtenant` für den Domänennamen Ihres Azure AD B2C-Mandanten steht.
1. Klicken Sie auf **Erstellen**.
1. Wählen Sie nach der Erstellung die neu erstellte Anwendung `IdentityExperienceFramework` aus, klicken Sie auf **Eigenschaften**, kopieren Sie die Anwendungs-ID, und speichern Sie sie zur späteren Verwendung.

### <a name="create-the-proxy-identity-experience-framework-application"></a>Erstellen der Identity Experience Framework-Proxyanwendung

1. Wählen Sie **App-Registrierungen** aus.
1. Klicken Sie auf **+ Registrierung einer neuen Anwendung**.
   * Name: `ProxyIdentityExperienceFramework`
   * Anwendungstyp: `Native`
   * Anmelde-URL: `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`, wobei `yourtenant` für Ihren Azure AD B2C-Mandanten steht.
1. Klicken Sie auf **Erstellen**.
1. Wählen Sie nach der Erstellung die Anwendung `ProxyIdentityExperienceFramework` aus, klicken Sie auf **Eigenschaften**, kopieren Sie die Anwendungs-ID, und speichern Sie sie zur späteren Verwendung.
1. Wählen Sie zunächst **Erforderliche Berechtigungen**, dann **+ Hinzufügen** und schließlich **API auswählen** aus.
1. Suchen Sie nach dem Namen `IdentityExperienceFramework`, wählen Sie in den Ergebnissen „IdentityExperienceFramework“ aus, und klicken Sie anschließend auf **Auswählen**.
1. Aktivieren Sie das Kontrollkästchen neben `Access IdentityExperienceFramework`, und klicken Sie dann auf **Auswählen**.
1. Klicken Sie auf **Done**.
1. Klicken Sie auf **Berechtigungen erteilen**, und bestätigen Sie den Vorgang mit **Ja**.

## <a name="download-starter-pack-and-modify-policies"></a>Herunterladen des Starter Pack und Ändern von Richtlinien

Benutzerdefinierte Richtlinien sind ein Satz von XML-Dateien, die in Ihren Azure AD B2C-Mandanten hochgeladen werden müssen. Es sind Starter Packs verfügbar, die Ihnen den schnellen Einstieg ermöglichen. Alle unten angegebenen Starter Packs verfügen über die mindestens erforderlichen technischen Profile und User Journeys, die für die beschriebenen Szenarien benötigt werden:
 * LocalAccounts: Ermöglicht ausschließlich die Nutzung von lokalen Konten.
 * SocialAccounts: Ermöglicht ausschließlich die Nutzung von Konten sozialer Netzwerke (oder Verbundkonten).
 * **SocialAndLocalAccounts**: Wir verwenden diese Option für diese exemplarische Vorgehensweise.
 * SocialAndLocalAccountsWithMFA: Umfasst Optionen für Konten sozialer Netzwerke, lokale Konten und MFA.

Jedes Starter Pack enthält Folgendes:

* Die [Basisdatei](active-directory-b2c-overview-custom.md#policy-files) der Richtlinie. Es sind einige Änderungen an der Basisdatei erforderlich.
* Die [Erweiterungsdatei](active-directory-b2c-overview-custom.md#policy-files) der Richtlinie.  In dieser Datei werden die meisten Konfigurationsänderungen vorgenommen.
* [Dateien der vertrauenden Seite:](active-directory-b2c-overview-custom.md#policy-files) Dies sind aufgabenspezifische Dateien, die von Ihrer Anwendung aufgerufen werden.

>[!NOTE]
>Wenn Ihr XML-Editor Validierungen unterstützt, sollten Sie die Dateien anhand der XML-Schemadatei `TrustFrameworkPolicy_0.3.0.0.xsd` im Stammordner des Starter Pack validieren. Durch die XML-Schemavalidierung werden vor dem Upload Fehler festgestellt.

Lassen Sie uns anfangen:

1. Laden Sie „active-directory-b2c-custom-policy-starterpack“ von GitHub herunter.  [Laden Sie das ZIP-Paket herunter](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) oder führen Sie es aus.

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```
2. Öffnen Sie den Ordner `SocialAndLocalAccounts`.  Die Basisdatei (`TrustFrameworkBase.xml`) in diesem Ordner enthält Inhalte, die sowohl für lokale als auch Social Media-/Unternehmenskonten erforderlich sind. Die Inhalte sozialer Netzwerke beeinträchtigen nicht die Schritte zum Einrichten von lokalen Konten.
3. Öffnen Sie `TrustFrameworkBase.xml`.  Wenn Sie einen XML-Editor benötigen, können Sie [Visual Studio Code](https://code.visualstudio.com/download), einen einfachen plattformübergreifenden Editor, ausprobieren.
4. Aktualisieren Sie im Stammelement `TrustFrameworkPolicy` die Attribute `TenantId` und `PublicPolicyUri`, und ersetzen Sie dabei `yourtenant.onmicrosoft.com` durch den Domänennamen Ihres Azure AD B2C-Mandanten:

    ```xml
    <TrustFrameworkPolicy
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
    PolicySchemaVersion="0.3.0.0"
    TenantId="yourtenant.onmicrosoft.com"
    PolicyId="B2C_1A_TrustFrameworkBase"
    PublicPolicyUri="http://yourtenant.onmicrosoft.com">
    ```
>[!NOTE]
>`PolicyId` ist der Richtlinienname, der im Portal angezeigt wird, und der Name, unter dem von anderen Richtliniendateien auf diese Richtliniendatei verwiesen wird.

5. Speichern Sie die Datei.
6. Öffnen Sie `TrustFrameworkExtensions.xml`, und nehmen Sie die gleichen beiden Änderungen vor, indem Sie `yourtenant.onmicrosoft.com` durch Ihren Azure AD B2C-Mandanten ersetzen. Führen Sie den gleichen Ersetzungsvorgang auch im `<TenantId>`-Element durch (insgesamt drei Änderungen).  Speichern Sie die Datei.
7. Öffnen Sie `SignUpOrSignIn.xml`, und nehmen Sie die gleichen Änderungen vor, indem Sie `yourtenant.onmicrosoft.com` an drei Stellen durch Ihren Azure AD B2C-Mandanten ersetzen. Speichern Sie die Datei.
8. Öffnen Sie die Kennwortzurücksetzungs- und Profilbearbeitungsdateien, und nehmen Sie die gleichen Änderungen vor, indem Sie `yourtenant.onmicrosoft.com` in jeder Datei an drei Stellen durch Ihren Azure AD B2C-Mandanten ersetzen. Speichern Sie die Dateien.

### <a name="add-the-application-ids-to-your-custom-policy"></a>Hinzufügen der Anwendungs-IDs zur benutzerdefinierten Richtlinie
Fügen Sie die Anwendungs-IDs der Datei mit den Erweiterungen hinzu (`TrustFrameworkExtensions.xml`).

1. Suchen Sie in der Erweiterungsdatei (`TrustFrameworkExtensions.xml`) nach dem Element `<TechnicalProfile Id="login-NonInteractive">`.
2. Ersetzen Sie beide Instanzen von `IdentityExperienceFrameworkAppId` durch die Anwendungs-ID der Identity Experience Framework-Anwendung, die Sie zuvor erstellt haben. Beispiel:

```xml
<Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
```

3. Ersetzen Sie beide Instanzen von `ProxyIdentityExperienceFrameworkAppId` durch die Anwendungs-ID der Identity Experience Framework-Proxyanwendung, die Sie zuvor erstellt haben.
4. Speichern Sie Ihre Erweiterungsdatei.

## <a name="upload-the-policies-to-your-tenant"></a>Hochladen der Richtlinien in Ihren Mandanten

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) [in den Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und öffnen Sie das Blatt „Azure AD B2C“.
2. Klicken Sie auf **Identity Experience Framework**.
3. Wählen Sie die Option **Richtlinie hochladen**, um Richtliniendateien hochzuladen.

    >[!WARNING]
    >Die benutzerdefinierten Richtliniendateien müssen in der folgenden Reihenfolge hochgeladen werden:

1. Laden Sie `TrustFrameworkBase.xml` hoch.
2. Laden Sie `TrustFrameworkExtensions.xml` hoch.
3. Laden Sie `SignUpOrSignin.xml` hoch.
4. Laden Sie Ihre anderen Richtliniendateien hoch.

Beim Hochladen einer Datei wird dem Namen der Richtliniendatei `B2C_1A_` vorangestellt.

## <a name="test-the-custom-policy-using-run-now"></a>Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“

1. Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Identity Experience Framework**.

>[!NOTE]
>**Jetzt ausführen** Hierfür ist es erforderlich, dass mindestens eine Anwendung für den Mandanten vorab registriert wird. Anwendungen müssen im B2C-Mandanten registriert werden, indem in B2C oder in Identity Experience Framework die Menüoption **Anwendungen** gewählt wird, um sowohl integrierte als auch benutzerdefinierte Richtlinien aufzurufen. Es ist nur eine Registrierung pro Anwendung erforderlich. 

Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C.  

2. Wählen Sie die benutzerdefinierte Richtlinie der vertrauenden Seite aus, die Sie hochgeladen haben (`B2C_1A_signup_signin`), und klicken Sie auf die Schaltfläche **Jetzt ausführen**.


3. Sie sollten sich mit einer E-Mail-Adresse registrieren können.
4. Anmelden mit demselben Konto zur Bestätigung der richtigen Konfiguration

>[!NOTE]
>Eine häufige Ursache für einen Anmeldefehler ist eine falsch konfigurierte IdentityExperienceFramework-App.


## <a name="next-steps"></a>Nächste Schritte

### <a name="add-facebook-as-an-identity-provider"></a>Hinzufügen von Facebook als Identitätsanbieter
So richten Sie Facebook ein:
1. [Konfigurieren einer Facebook-Anwendung unter „developers.facebook.com“](active-directory-b2c-setup-fb-app.md)
2. [Hinzufügen des Facebook-Anwendungsgeheimnisses zum Azure AD B2C-Mandanten](#add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies)
3. Fügen Sie die ID der Facebook-Anwendung in der TrustFrameworkExtensions-Richtliniendatei unter `Item Key="client_id"` ein:

```xml
<TechnicalProfile Id="Facebook-OAUTH">
  <Metadata>
  <!--Replace the value of client_id in this technical profile with your the Facebook App ID"-->
    <Item Key="client_id">00000000000000</Item>
```
4. Laden Sie die Richtliniendatei „TrustFrameworkExtensions.xml“ in Ihren Mandanten hoch.
5. Führen Sie einen Test durch, indem Sie **Jetzt ausführen** verwenden, oder rufen Sie die Richtlinie direkt über Ihre registrierte Anwendung auf.

### <a name="add-azure-active-directory-as-an-identity-provider"></a>Hinzufügen von Azure Active Directory als Identitätsanbieter
Die Basisdatei, die wir in diesem Leitfaden für erste Schritte verwendet haben, enthält bereits einige Inhalte, die Sie zum Hinzufügen anderer Identitätsanbieter benötigen. Um die Anmeldung mithilfe von Azure AD-Konten einzurichten, fahren Sie mit [diesem Thema](active-directory-b2c-setup-aad-custom.md) fort.


## <a name="reference"></a>Referenz

[Übersicht](active-directory-b2c-overview-custom.md) über benutzerdefinierte Richtlinien in Azure AD B2C per Identity Experience Framework

