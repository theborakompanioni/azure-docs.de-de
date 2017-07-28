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
ms.sourcegitcommit: 9edcaee4d051c3dc05bfe23eecc9c22818cf967c
ms.openlocfilehash: 1cc36d1fd40121fed23ab6a84429a303690c2726
ms.contentlocale: de-de
ms.lasthandoff: 06/08/2017

---
# <a name="azure-active-directory-b2c-get-started-with-custom-policies"></a>Azure Active Directory B2C: Erste Schritte mit benutzerdefinierten Richtlinien

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Nach der Durchführung der Schritte in diesem Artikel unterstützt Ihre benutzerdefinierte Richtlinie die Registrierung oder Anmeldung mit einem „lokalen Konto“ über eine E-Mail-Adresse und ein Kennwort. Zudem bereiten Sie Ihre Umgebung auf das Hinzufügen von Identitätsanbietern (z.B. Facebook oder Azure Active Directory) vor. Es wird empfohlen, diese Schritte auszuführen, bevor Sie von anderen Verwendungsmöglichkeiten des Frameworks für die Identitätsfunktion von Azure Active Directory (Azure AD) B2C lesen.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass Sie über einen Azure AD B2C-Mandanten verfügen, bevor Sie fortfahren. Dies ist ein Container für all Ihre Benutzer, Apps, Richtlinien etc. Wenn Sie noch keinen Mandanten besitzen, müssen Sie einen [Azure AD B2C-Mandanten erstellen](active-directory-b2c-get-started.md). Wir raten allen Entwicklern dringend, die exemplarischen Vorgehensweisen für die integrierten Azure AD B2C-Richtlinien durchzuarbeiten und vor dem Fortfahren die Anwendungen mit integrierten Richtlinien zu konfigurieren. Ihre Anwendungen funktionieren mit beiden Arten von Richtlinien. Es ist nur eine kleine Änderung des Richtliniennamens zum Aufrufen der benutzerdefinierten Richtlinie erforderlich.

Sie benötigen für den Zugriff auf die Bearbeitung einer benutzerdefinierten Richtlinie ein gültiges Azure-Abonnement, das mit Ihrem Mandanten verknüpft ist.

## <a name="add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies"></a>Hinzufügen von Signatur- und Verschlüsselungsschlüsseln im B2C-Mandanten zur Verwendung durch benutzerdefinierte Richtlinien

1. Öffnen Sie in den Einstellungen für den Azure AD B2C-Mandanten das Blatt **Framework für die Identitätsfunktion**.
2. Wählen Sie **Policy Keys** (Richtlinienschlüssel), um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen.
3. Erstellen Sie B2C_1A_TokenSigningKeyContainer, sofern nicht vorhanden:<br>
    a. Wählen Sie **Hinzufügen**. <br>
    b. Wählen Sie **Generieren** aus.<br>
    c. Verwenden Sie für **Name** den Wert `TokenSigningKeyContainer`. <br> 
    Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.<br>
    d. Verwenden Sie für **Schlüsseltyp** die Option **RSA**.<br>
    e. Verwenden Sie für **Datumsangaben** die Standardwerte. <br>
    f. Verwenden Sie für **Schlüsselverwendung**, die Option **Signatur**.<br>
    g. Klicken Sie auf **Erstellen**.<br>
4. Erstellen Sie B2C_1A_TokenEncryptionKeyContainer, sofern nicht vorhanden:<br>
 a. Wählen Sie **Hinzufügen**.<br>
 b. Wählen Sie **Generieren** aus.<br>
 c. Verwenden Sie für **Name** den Wert `TokenEncryptionKeyContainer`. <br>
   Das Präfix `B2C_1A`_ wird möglicherweise automatisch hinzugefügt.<br>
 d. Verwenden Sie für **Schlüsseltyp** die Option **RSA**.<br>
 e. Verwenden Sie für **Datumsangaben** die Standardwerte.<br>
 f. Verwenden Sie für **Schlüsselverwendung** die Option **Verschlüsselung**.<br>
 g. Klicken Sie auf **Erstellen**.<br>
5. Erstellen Sie B2C_1A_FacebookSecret. <br>
Wenn Sie bereits über ein Facebook-Anwendungsgeheimnis verfügen, können Sie es Ihrem Mandanten als Richtlinienschlüssel hinzufügen. Andernfalls müssen Sie den Schlüssel mit einem Platzhalterwert erstellen, damit Ihre Richtlinien die Überprüfung bestehen.<br>
 a. Wählen Sie **Hinzufügen**.<br>
 b. Verwenden Sie für **Optionen** die Option **Manuell**.<br>
 c. Verwenden Sie für **Name** den Wert `FacebookSecret`. <br>
 Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.<br>
 d. Geben Sie in das Feld **Geheimer Schlüssel** Ihr FacebookSecret aus developers.facebook.com oder `0` als Platzhalter ein. *Dies ist nicht die ID Ihrer Facebook-App.* <br>
 e. Verwenden Sie für **Schlüsselverwendung**, die Option **Signatur**. <br>
 f. Wählen Sie **Erstellen** aus, und bestätigen Sie die Erstellung.

## <a name="register-identity-experience-framework-applications"></a>Registrieren von Identity Experience Framework-Anwendungen

Azure AD B2C erfordert, dass Sie zwei zusätzliche Anwendungen registrieren, die vom Modul zur Registrierung und Anmeldung von Benutzern verwendet werden.

>[!NOTE]
>Sie müssen zwei Anwendungen erstellen, welche die Anmeldung mit lokalen Konten ermöglichen: IdentityExperienceFramework (eine Web-App) und ProxyIdentityExperienceFramework (eine native App) mit delegierter Berechtigung von der IdentityExperienceFramework-App. Lokale Konten sind nur in Ihrem Mandanten vorhanden. Ihre Benutzer melden sich mit einer eindeutigen Kombination aus E-Mail-Adresse und Kennwort an, um auf Ihre bei Mandanten registrierten Anwendungen zuzugreifen.

### <a name="create-the-identityexperienceframework-application"></a>Erstellen der IdentityExperienceFramework-Anwendung

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) in den [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md).
2. Öffnen Sie das Blatt **Azure Active Directory** (nicht das Blatt **Azure AD B2C**). Möglicherweise müssen Sie **Weitere Dienste** auswählen, um das Blatt zu finden.
3. Wählen Sie **App-Registrierungen** aus.
4. Wählen Sie **Registrierung einer neuen Anwendung** aus.
   * Verwenden Sie für **Name** den Wert `IdentityExperienceFramework`.
   * Verwenden Sie für **Anwendungstyp** die Option **Web-App/API**.
   * Verwenden Sie `https://login.microsoftonline.com/yourtenant.onmicrosoft.com` für **Anmelde-URL**, wobei `yourtenant` für den Domänennamen Ihres Azure AD B2C-Mandanten steht.
5. Klicken Sie auf **Erstellen**.
6. Wählen Sie die neu erstellte Anwendung **IdentityExperienceFramework** aus, sobald die Erstellung abgeschlossen ist.<br>
   a. Wählen Sie **Eigenschaften** aus.<br>
   b. Kopieren Sie die Anwendungs-ID und speichern Sie sie für eine spätere Verwendung.

### <a name="create-the-proxyidentityexperienceframework-application"></a>Erstellen der Anwendung ProxyIdentityExperienceFramework

1. Wählen Sie **App-Registrierungen** aus.
1. Wählen Sie **Registrierung einer neuen Anwendung** aus.
   * Verwenden Sie für **Name** den Wert `ProxyIdentityExperienceFramework`.
   * Verwenden Sie für **Anwendungstyp** die Option **Nativ**.
   * Verwenden Sie `https://login.microsoftonline.com/yourtenant.onmicrosoft.com` für den **Umleitungs-URI**, wobei `yourtenant` für Ihren Azure AD B2C-Mandanten steht.
1. Klicken Sie auf **Erstellen**.
1. Wählen Sie die Anwendung **ProxyIdentityExperienceFramework** aus, sobald sie erstellt wurde.<br>
   a. Wählen Sie **Eigenschaften** aus. <br>
   b. Kopieren Sie die Anwendungs-ID und speichern Sie sie für eine spätere Verwendung.
1. Wählen Sie **Erforderliche Berechtigungen** aus.
1. Wählen Sie **Hinzufügen**.
1. Wählen Sie **API auswählen** aus.
1. Suchen Sie nach dem Namen IdentityExperienceFramework. Wählen Sie **IdentityExperienceFramework** in den Ergebnissen aus, und klicken Sie anschließend auf **Auswählen**.
1. Aktivieren Sie das Kontrollkästchen neben **Auf IdentityExperienceFramework zugreifen**, und klicken Sie anschließend auf **Auswählen**.
1. Wählen Sie **Fertig**aus.
1. Wählen Sie **Berechtigungen erteilen** aus, und bestätigen Sie dies anschließend durch Auswahl von **Ja**.

## <a name="download-starter-pack-and-modify-policies"></a>Herunterladen des Starter Pack und Ändern von Richtlinien

Benutzerdefinierte Richtlinien sind ein Satz von XML-Dateien, die in Ihren Azure AD B2C-Mandanten hochgeladen werden müssen. Es sind Starter Packs verfügbar, die Ihnen den schnellen Einstieg ermöglichen. Die einzelnen Starter Packs in der folgenden Liste enthalten die kleinste Anzahl von technischen Profilen sowie die zum Erreichen der beschriebenen Szenarios benötigten User Journeys:
 * LocalAccounts. Ermöglicht ausschließlich die Verwendung von lokalen Konten.
 * SocialAccounts. Ermöglicht ausschließlich die Verwendung von Konten sozialer Netzwerke (oder Verbundkonten).
 * **SocialAndLocalAccounts**. Diese Datei wird für die exemplarische Vorgehensweise verwendet.
 * SocialAndLocalAccountsWithMFA. Hier sind Optionen für soziale und lokale Authentifizierung sowie für Multi-Factor Authentication enthalten.

Die einzelnen Starter Packs enthalten Folgendes:

* Die [Basisdatei](active-directory-b2c-overview-custom.md#policy-files) der Richtlinie. Es sind einige Änderungen an der Basisdatei erforderlich.
* Die [Erweiterungsdatei](active-directory-b2c-overview-custom.md#policy-files) der Richtlinie.  In dieser Datei werden die meisten Konfigurationsänderungen vorgenommen.
* [Dateien der vertrauenden Seite](active-directory-b2c-overview-custom.md#policy-files). Dies sind aufgabenspezifische Dateien, die von Ihrer Anwendung aufgerufen werden.

>[!NOTE]
>Wenn Ihr XML-Editor Validierungen unterstützt, sollten Sie die Dateien anhand der XML-Schemadatei TrustFrameworkPolicy_0.3.0.0.xsd im Stammverzeichnis des Starter Packs überprüfen. Durch die XML-Schemavalidierung werden vor dem Upload Fehler festgestellt.

 Lassen Sie uns anfangen:

1. Laden Sie active-directory-b2c-custom-policy-starterpack von GitHub herunter. [Laden Sie die ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) oder führen Sie sie aus

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```
2. Öffnen Sie den Ordner SocialAndLocalAccounts.  Die Basisdatei (TrustFrameworkBase.xml) in diesem Ordner umfasst Inhalte, die sowohl für lokale als auch für Social Media-/Unternehmenskonten erforderlich sind. Die Inhalte sozialer Netzwerke beeinträchtigen nicht die Schritte zum Einrichten von lokalen Konten.
3. Öffnen Sie TrustFrameworkBase.xml. Wenn Sie einen XML-Editor benötigen, können Sie [Visual Studio Code](https://code.visualstudio.com/download), einen einfachen plattformübergreifenden Editor, ausprobieren.
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
   >`PolicyId` ist der Richtlinienname, der Ihnen im Portal angezeigt wird, und der Name, unter dem von anderen Richtliniendateien auf diese Richtliniendatei verwiesen wird.

5. Speichern Sie die Datei.
6. Öffnen Sie TrustFrameworkExtensions.xml. Nehmen Sie die gleichen zwei Änderungen vor, indem Sie `yourtenant.onmicrosoft.com` durch Ihren Azure AD B2C-Mandanten ersetzen. Führen Sie den gleichen Ersetzungsvorgang auch im `<TenantId>`-Element durch (insgesamt drei Änderungen). Speichern Sie die Datei.
7. Öffnen Sie SignUpOrSignIn.xml. Nehmen Sie die gleichen Änderungen vor, indem Sie `yourtenant.onmicrosoft.com` an drei Stellen durch Ihren Azure AD B2C-Mandanten ersetzen. Speichern Sie die Datei.
8. Öffnen Sie die Dateien zum Zurücksetzen des Kennworts und zur Bearbeitung des Profils. Nehmen Sie die gleichen Änderungen vor, indem Sie `yourtenant.onmicrosoft.com` an drei Stellen durch Ihren Azure AD B2C-Mandanten ersetzen. Speichern Sie die Dateien.

### <a name="add-the-application-ids-to-your-custom-policy"></a>Hinzufügen der Anwendungs-IDs zur benutzerdefinierten Richtlinie
Fügen Sie die Anwendungs-IDs zur Datei mit den Erweiterungen hinzu (`TrustFrameworkExtensions.xml`):

1. Suchen Sie in der Datei mit den Erweiterungen (TrustFrameworkExtensions.xml) nach dem Element `<TechnicalProfile Id="login-NonInteractive">`.
2. Ersetzen Sie beide Instanzen von `IdentityExperienceFrameworkAppId` durch die Anwendungs-ID der Identity Experience Framework-Anwendung, die Sie zuvor erstellt haben. Beispiel:

   ```xml
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```
3. Ersetzen Sie beide Instanzen von `ProxyIdentityExperienceFrameworkAppId` durch die Anwendungs-ID der Anwendung des Frameworks für die Identitätsfunktion, die Sie zuvor erstellt haben.
4. Speichern Sie Ihre Erweiterungsdatei.

## <a name="upload-the-policies-to-your-tenant"></a>Hochladen der Richtlinien in Ihren Mandanten

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) in den [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und öffnen Sie das Blatt **Azure AD B2C**.
2. Wählen Sie **Framework für die Identitätsfunktion** aus.
3. Wählen Sie **Richtlinie hochladen** aus, um Richtliniendateien hochzuladen.

    >[!WARNING]
    >Die benutzerdefinierten Richtliniendateien müssen in der folgenden Reihenfolge hochgeladen werden:

1. Laden Sie TrustFrameworkBase.xml hoch.
2. Laden Sie TrustFrameworkExtensions.xml hoch.
3. Laden Sie SignUpOrSignin.xml hoch.
4. Laden Sie Ihre anderen Richtliniendateien hoch.

Beim Hochladen einer Datei wird dem Namen der Richtliniendatei `B2C_1A_` vorangestellt.

## <a name="test-the-custom-policy-by-using-run-now"></a>Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“

1. Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.

   >[!NOTE]
   >**Jetzt ausführen** macht erforderlich, dass mindestens eine Anwendung im Mandanten vorab registriert wird. Anwendungen müssen im B2C-Mandanten registriert werden, indem in Azure AD B2C oder im Framework für die Identitätsfunktion die Menüoption **Anwendungen** ausgewählt wird, um sowohl integrierte als auch benutzerdefinierte Richtlinien aufzurufen. Es ist nur eine Registrierung pro Anwendung erforderlich.<br><br>
   Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C.  

2. Öffnen Sie B2C_1A_signup_signin – die benutzerdefinierte Richtlinie der vertrauenden Seite, die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.

3. Sie sollten sich mit einer E-Mail-Adresse registrieren können.

4. Melden Sie sich zur Bestätigung der richtigen Konfiguration mit demselben Konto an.

>[!NOTE]
>Eine häufige Ursache für einen Anmeldefehler ist eine falsch konfigurierte IdentityExperienceFramework-App.


## <a name="next-steps"></a>Nächste Schritte

### <a name="add-facebook-as-an-identity-provider"></a>Hinzufügen von Facebook als Identitätsanbieter
So richten Sie Facebook ein:
1. [Konfigurieren einer Facebook-Anwendung unter developers.facebook.com](active-directory-b2c-setup-fb-app.md).
2. [Hinzufügen des geheimen Schlüssels der Facebook-Anwendung zum Azure AD B2C-Mandanten](#add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies).
3. Ersetzen Sie in der Richtliniendatei TrustFrameworkExtensions den Wert `client_id` durch die ID der Facebook-Anwendung:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```
4. Laden Sie die Richtliniendatei „TrustFrameworkExtensions.xml“ in Ihren Mandanten hoch.
5. Führen Sie einen Test durch, indem Sie **Jetzt ausführen** verwenden, oder rufen Sie die Richtlinie direkt über Ihre registrierte Anwendung auf.

### <a name="add-azure-active-directory-as-an-identity-provider"></a>Hinzufügen von Azure Active Directory als Identitätsanbieter
Die in diesem Leitfaden für erste Schritte verwendete Basisdatei enthält bereits einige Inhalte, die Sie zum Hinzufügen anderer Identitätsanbieter benötigen. Informationen zum Einrichten von Anmeldungen finden Sie im Artikel [Azure Active Directory B2C: Anmelden mit Azure AD-Konten](active-directory-b2c-setup-aad-custom.md).

Eine Übersicht über benutzerdefinierte Richtlinien in Azure AD B2C, die das Framework für die Identitätsfunktion verwenden, finden Sie im Artikel [Azure Active Directory B2C: benutzerdefinierte Richtlinien](active-directory-b2c-overview-custom.md). 

