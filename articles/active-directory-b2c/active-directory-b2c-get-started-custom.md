---
title: 'Azure Active Directory B2C: Erste Schritte mit benutzerdefinierten Richtlinien | Microsoft-Dokumentation'
description: Ein Thema zum Einstieg in benutzerdefinierte Azure Active Directory B2C-Richtlinien
services: active-directory-b2c
documentationcenter: 
author: gsacavdm
manager: krassk
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: gsacavdm
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 93fc922433f0ba60ee438db8d4dcb14527c208fc
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017

---
# <a name="azure-active-directory-b2c-getting-started-with-custom-policies"></a>Azure Active Directory B2C: Erste Schritte mit benutzerdefinierten Richtlinien

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Nach der Durchführung der Schritte in diesem Artikel unterstützt Ihre benutzerdefinierte Richtlinie die Registrierung oder Anmeldung mit einem lokalen Konto mithilfe einer E-Mail-Adresse und eines Kennworts. Zudem bereiten Sie Ihre Umgebung auf das Hinzufügen zusätzlicher Identitätsanbieter (z.B. Facebook oder Azure AD) vor.  Bevor alle anderen Einsatzmöglichkeiten der Azure AD B2C-Identity Experience Engine und viele grundlegende Konzepte vorgestellt werden, müssen die nachfolgenden Schritte durchgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie fortfahren, stellen Sie sicher, dass Sie über einen Azure AD B2C-Mandanten verfügen. Ein Azure AD B2C-Mandant ist ein Container für all Ihre Benutzer, Apps, Richtlinien etc. Wenn Sie keinen Mandanten besitzen, müssen Sie einen solchen [erstellen](active-directory-b2c-get-started.md).

### <a name="confirming-your-b2c-tenant"></a>Bestätigen Ihres B2C-Mandanten

Da sich benutzerdefinierte Richtlinien noch in der privaten Vorschau befinden, vergewissern Sie sich, dass Ihr Azure AD B2C-Mandant für den Upload benutzerdefinierter Richtlinien aktiviert ist:

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) [in den Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und öffnen Sie das Blatt „Azure AD B2C“.
1. Klicken Sie auf **Alle Richtlinien**.
1. Stellen Sie sicher, dass **Richtlinie hochladen** verfügbar ist.  Wenn die Schaltfläche deaktiviert ist, senden Sie eine E-Mail an AADB2CPreview@microsoft.com.

## <a name="set-up-keys-for-your-custom-policy"></a>Einrichten von Schlüsseln für Ihre benutzerdefinierte Richtlinie

Der erste Schritt zum Verwenden benutzerdefinierter Richtlinien ist die Einrichtung von Schlüsseln.

[!INCLUDE [active-directory-b2c-setup-keys-custom.md](../../includes/active-directory-b2c-setup-keys-custom.md)]

## <a name="download-starter-pack-and-modify-policies"></a>Herunterladen des Starter Pack und Ändern von Richtlinien

Benutzerdefinierte Richtlinien sind ein Satz von XML-Dateien, die in Ihren Azure AD B2C-Mandanten hochgeladen werden müssen. Wir stellen ein Starter Pack bereit, das Sie für den Einstieg verwenden können. Das Starter Pack enthält Folgendes:

* Die [Basisdatei](active-directory-b2c-overview-custom.md#policy-files) der Richtlinie. Es sind einige Änderungen an der Basisdatei erforderlich.
* Die [Erweiterungsdatei](active-directory-b2c-overview-custom.md#policy-files) der Richtlinie.  In dieser Datei werden die meisten Konfigurationsänderungen vorgenommen.

Lassen Sie uns anfangen:

1. Laden Sie „Azure AD B2C Starter Pack für benutzerdefinierte Richtlinien“ von GitHub herunter.  [Laden Sie das ZIP-Paket herunter](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) oder führen Sie es aus.

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```

1. Öffnen Sie den Ordner `SocialIDPAndLocalAccounts`.  Die Basisdatei (`TrustFrameworkBase.xml`) in diesem Ordner enthält Inhalte, die sowohl für lokale als auch Social Media-/Unternehmenskonten erforderlich sind. Die Inhalte sozialer Netzwerke beeinträchtigen nicht die Schritte zum Einrichten von lokalen Konten.
1. Öffnen Sie `TrustFrameworkBase.xml`.  Wenn Sie einen XML-Editor benötigen, probieren Sie [Visual Studio Code](https://code.visualstudio.com/download) aus, einen einfachen plattformübergreifenden Editor.
1. Aktualisieren Sie im Stammelement `TrustFrameworkPolicy` die Attribute `TenantId` und `PublicPolicyUri`, und ersetzen Sie dabei `{tenantName}` durch Ihren Azure AD B2C-Mandanten:

    ```xml
    <TrustFrameworkPolicy
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
    PolicySchemaVersion="0.3.0.0"
    TenantId="{tenantName}.onmicrosoft.com"
    PolicyId="B2C_1A_TrustFrameworkBase"
    PublicPolicyUri="http://{tenantName}.onmicrosoft.com">
    ```

1. Speichern Sie die Datei.
1. Öffnen Sie `TrustFrameworkExtensions.xml` und nehmen Sie dieselben Änderungen vor, indem Sie `{tenantName}` durch Ihren Azure AD B2C-Mandanten ersetzen. Speichern Sie die Datei.
1. Öffnen Sie `SignUpOrSignIn.xml` und nehmen Sie dieselben Änderungen vor, indem Sie `{tenantName}` durch Ihren Azure AD B2C-Mandanten ersetzen. Speichern Sie die Datei.

>[!NOTE]
>Wenn Ihr XML-Editor Validierungen unterstützt, sollten Sie die Dateien anhand der XML-Schemadatei `TrustFrameworkPolicy_0.3.0.0.xsd` im Stammordner des Starter Pack validieren. Durch die XML-Schemavalidierung werden vor dem Upload Fehler festgestellt.

## <a name="register-policy-engine-applications"></a>Registrieren von Richtlinienmodulanwendungen

Azure AD B2C erfordert, dass Sie zwei zusätzliche Anwendungen registrieren, die vom Modul zur Registrierung und Anmeldung von Benutzern verwendet werden.

>[!NOTE]
>Nachfolgend erstellen wir zwei Anwendungen, die die Anmeldung mit lokalen Konten ermöglichen: „PolicyEngine“ (eine Web-App) und „PolicyEngineProxy“ (eine systemeigene App) mit von PolicyEngine delegierter Berechtigung. Dieser Abschnitt ist nur für Azure AD B2C-Mandanten erforderlich, bei denen die Verwendung von lokalen Konten erwartet wird.

### <a name="create-the-policy-engine-application"></a>Erstellen der Richtlinienmodulanwendung

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) [in den Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md).
1. Öffnen Sie das Blatt `Azure Active Directory` (nicht das Blatt „Azure AD B2C“). Sie müssen möglicherweise auf **> Weitere Dienste** klicken, um danach zu suchen.
1. Wählen Sie **App-Registrierungen** aus.
1. Klicken Sie auf **+ Registrierung einer neuen Anwendung**.
   * Name: `PolicyEngine`
   * Anwendungstyp: `Web app/API`
   * Anmelde-URL: `https://login.microsoftonline.com/{tenantName}.onmicrosoft.com`, wobei `{tenantName}` für Ihren Azure AD B2C-Mandanten steht.
1. Klicken Sie auf **Erstellen**.
1. Wählen Sie nach der Erstellung die neu erstellte Anwendung `PolicyEngine` aus, kopieren Sie die Anwendungs-ID, und speichern Sie sie zur späteren Verwendung.

### <a name="create-the-policy-engine-proxy-application"></a>Erstellen der Richtlinienmodul-Proxyanwendung

1. Wählen Sie **App-Registrierungen** aus.
1. Klicken Sie auf **+ Registrierung einer neuen Anwendung**.
   * Name: `PolicyEngineProxy`
   * Anwendungstyp: `Native`
   * Anmelde-URL: `https://login.microsoftonline.com/{tenantName}.onmicrosoft.com`, wobei `{tenantName}` für Ihren Azure AD B2C-Mandanten steht.
1. Klicken Sie auf **Erstellen**.
1. Wählen Sie nach der Erstellung die Anwendung `PolicyEngineProxy` aus, kopieren Sie die Anwendungs-ID, und speichern Sie sie zur späteren Verwendung.
1. Wählen Sie zunächst **Erforderliche Berechtigungen**, dann **+ Hinzufügen** und schließlich **API auswählen** aus.
1. Suchen Sie nach dem Namen `PolicyEngine`, wählen Sie „PolicyEngine“ in den Ergebnissen aus, und klicken Sie dann auf **Auswählen**.
1. Aktivieren Sie das Kontrollkästchen neben `Access PolicyEngine`, und klicken Sie dann auf **Auswählen**.
1. Klicken Sie auf **Done**.

### <a name="add-the-application-ids-to-your-custom-policy"></a>Hinzufügen der Anwendungs-IDs zur benutzerdefinierten Richtlinie

Um eine benutzerdefinierte Richtlinie mit aktivierten lokalen Konten zu erstellen, müssen Sie die Anwendungs-IDs zur Erweiterungsdatei (`TrustFrameworkExtensions.xml`) hinzufügen.

1. Suchen Sie in der Erweiterungsdatei (`TrustFrameworkExtensions.xml`) nach dem Element `<TechnicalProfile Id="login-NonInteractive">`.
1. Ersetzen Sie beide Instanzen von `{Policy Engine Proxy Application ID}` durch die Anwendungs-ID der [Richtlinienmodul-Proxyanwendung, die Sie erstellt haben](#create-the-policy-engine-proxy-application).
1. Ersetzen Sie beide Instanzen von `{Policy Engine Application ID}` durch die Anwendungs-ID der [Richtlinienmodulanwendung, die Sie erstellt haben](#create-the-policy-engine-application).
1. Speichern Sie Ihre Erweiterungsdatei.

## <a name="upload-the-policies-to-your-tenant"></a>Hochladen der Richtlinien in Ihren Mandanten

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) [in den Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und öffnen Sie das Blatt „Azure AD B2C“.
1. Klicken Sie auf **Alle Richtlinien**.
1. Wählen Sie **Richtlinie hochladen** aus.

    >[!WARNING]
    >Die benutzerdefinierten Richtliniendateien müssen in der folgenden Reihenfolge hochgeladen werden:

1. Laden Sie `TrustFrameworkBase.xml` hoch.
1. Laden Sie `TrustFrameworkExtensions.xml` hoch.
1. Laden Sie `SignUpOrSignin.xml` hoch.

Wenn eine Datei hochgeladen ist, wird dem Namen `B2C_1A_` vorangestellt.  Integrierte Richtlinien beginnen stattdessen mit `B2C_1_`.

## <a name="test-the-custom-policy-using-run-now"></a>Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“

1. Öffnen Sie das Blatt **Azure AD B2C**, und navigieren Sie zu **Alle Richtlinien**.
1. Wählen Sie die benutzerdefinierte Richtlinie aus, die hochgeladen werden soll, und klicken Sie auf die Schaltfläche **Jetzt ausführen**.
1. Sie sollten sich mit einer E-Mail-Adresse registrieren können.

## <a name="next-steps"></a>Nächste Schritte

Die Basisdatei, die wir in diesem Leitfaden für erste Schritte verwendet haben, enthält bereits einige Inhalte, die Sie zum Hinzufügen anderer Identitätsanbieter benötigen. Um die Anmeldung mithilfe von Azure AD-Konten einzurichten, fahren Sie mit [diesem Thema](active-directory-b2c-setup-aad-custom.md) fort.

## <a name="reference"></a>Referenz

* Ein **technisches Profil (TP)** ist ein Element, das den Endpunktnamen, die zugehörigen Metadaten, das Protokoll und Details zum Austausch von Ansprüchen definiert, die die Identity Experience Engine durchführen soll.  Das TechnicalProfile, das von der Identity Experience Engine zur Anmeldung mit einem lokalem Konto verwendet wird, ist „Local Account SignIn“.

