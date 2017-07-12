---
title: "Azure Active Directory B2C: Anpassen einer Benutzeroberfläche mit benutzerdefinierten Richtlinien | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie eine Benutzeroberfläche (UI) anpassen, während Sie benutzerdefinierte Richtlinien in Azure AD B2C verwenden."
services: active-directory-b2c
documentationcenter: 
author: SaeedAkhter-MSFT
manager: krassk
editor: gsacavdm
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: saeeda
ms.translationtype: Human Translation
ms.sourcegitcommit: 1e6f2b9de47d1ce84c4043f5f6e73d462e0c1271
ms.openlocfilehash: cede72e742bbea7b1d51d42609743ea574af75ed
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017

---
<a id="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy" class="xliff"></a>

# Azure Active Directory B2C: Konfigurieren der Benutzeroberflächenanpassung in einer benutzerdefinierten Richtlinie

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Nach dem Durcharbeiten dieses Artikels haben Sie eine benutzerdefinierte Registrierungs- oder Anmelderichtlinie mit Ihrer Marke und in Ihrem Design erstellt. Mit Azure Active Directory B2C (Azure AD B2C) erlangen Sie nahezu vollständige Kontrolle über den HTML- und CSS-Inhalt, der Benutzern angezeigt wird. Bei Verwendung einer benutzerdefinierten Richtlinie konfigurieren Sie die Anpassung der Benutzeroberfläche in XML, anstatt über Steuerelemente im Azure-Portal. 

<a id="prerequisites" class="xliff"></a>

## Voraussetzungen

Arbeiten Sie zunächst [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) durch. Sie sollten eine funktionierende benutzerdefinierte Richtlinie für die Registrierung und Anmeldung bei lokalen Konten besitzen.

<a id="page-ui-customization" class="xliff"></a>

## Seite für die Benutzeroberflächenanpassung

Mit der Funktion zum Anpassen der Benutzeroberfläche können Sie das Aussehen und Verhalten benutzerdefinierter Richtlinien anpassen. Außerdem können Sie die Konsistenz von Marken und visuellen Elementen zwischen Ihrer Anwendung und Azure AD B2C verwalten.

So funktioniert es: Azure AD B2C führt den Code im Browser Ihres Kunden aus und verwendet einen modernen Ansatz namens [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/) (Ressourcenfreigabe zwischen verschiedenen Ursprüngen). Zuerst legen Sie eine URL in der benutzerdefinierten Richtlinie mit benutzerdefiniertem HTML-Inhalt fest. Azure AD B2C führt die UI-Elemente mit dem HTML-Inhalt, der über Ihre URL geladen wird, zusammen und zeigt anschließend die Seite für den Kunden an.

<a id="create-your-html5-content" class="xliff"></a>

## Erstellen des HTML5-Inhalts

Erstellen Sie den HTML-Inhalt mit dem Markennamen Ihres Produkts im Titel.

1. Kopieren Sie den folgenden HTML-Codeausschnitt. Es handelt sich um wohlgeformten HTML5-Code mit dem leeren Element *\<div id="api"\>\</div\>*, das in *\<body\>*-Tags enthalten ist. Dieses Element gibt an, wo Azure AD B2C-Inhalt eingefügt werden soll.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

   >[!NOTE]
   >Aus Sicherheitsgründen ist die Nutzung von JavaScript für die Anpassung derzeit blockiert.

2. Fügen Sie den kopierten Codeausschnitt in einen Text-Editor ein, und speichern Sie die Datei dann unter dem Namen *customize-ui.html*.

<a id="create-an-azure-blob-storage-account" class="xliff"></a>

## Erstellen eines Azure Blob Storage-Kontos

>[!NOTE]
> In diesem Artikel wird Azure Blob Storage zum Hosten unserer Inhalte verwendet. Sie können angeben, dass Ihre Inhalte auf einem Webserver gehostet werden sollen, aber Sie müssen [auf Ihrem Webserver CORS aktivieren](https://enable-cors.org/server.html).

Gehen Sie wie folgt vor, um diesen HTML-Inhalt im Blob-Speicher zu hosten:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Menü **Hub** die Option **Neu** > **Speicher** > **Speicherkonto**.
3. Geben Sie einen eindeutigen **Namen** für Ihr Speicherkonto ein.
4. Als **Bereitstellungsmodell** können Sie **Resource Manager** unverändert lassen.
5. Ändern Sie **Kontoart** in **Blob Storage**.
6. Für **Leistung** können Sie **Standard** übernehmen.
7. Für **Replikation** können Sie **RA-GRS** übernehmen.
8. Für **Zugriffstarif** können Sie **Heiß** übernehmen.
9. Für **Speicherdienstverschlüsselung** können Sie **Deaktiviert** übernehmen.
10. Wählen Sie ein **Abonnement** für Ihr Speicherkonto aus.
11. Erstellen Sie eine **Ressourcengruppe**, oder wählen Sie eine vorhandene Ressourcengruppe aus.
12. Wählen Sie den **geografischen Standort** für das Speicherkonto aus.
13. Klicken Sie auf **Erstellen** , um das Speicherkonto zu erstellen.  
    Nach Abschluss der Bereitstellung wird automatisch das Blatt **Speicherkonto** geöffnet.

<a id="create-a-container" class="xliff"></a>

## Erstellen eines Containers

Gehen Sie wie folgt vor, um im Blob-Speicher einen öffentlichen Container zu erstellen:

1. Klicken Sie auf die Registerkarte **Übersicht**.
2. Klicken Sie auf **Container**.
3. Geben Sie unter **Name** den Text **$root** ein.
4. Setzen Sie **Zugriffstyp** auf **Blob**.
5. Klicken Sie auf **$root**, um den neuen Container zu öffnen.
6. Klicken Sie auf **Hochladen**.
7. Klicken Sie auf das Ordnersymbol neben **Datei auswählen**.
8. Wechseln Sie zur Datei **customize-ui.html**, die Sie zuvor im Abschnitt [Seite für die Benutzeroberflächenanpassung](#the-page-ui-customization-feature) erstellt haben.
9. Klicken Sie auf **Hochladen**.
10. Wählen Sie das Blob „customize-ui.html“ aus, das Sie hochgeladen haben.
11. Klicken Sie neben **URL** auf **Kopieren**.
12. Fügen Sie in einem Browser die kopierte URL ein, und navigieren Sie zur Website. Falls ein Zugriff auf die Website nicht möglich ist, sollten Sie sicherstellen, dass der Zugriffstyp für den Container auf **Blob** festgelegt ist.

<a id="configure-cors" class="xliff"></a>

## Konfigurieren von CORS

Konfigurieren Sie den Blob-Speicher für die Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS), indem Sie wie folgt vorgehen:

>[!NOTE]
>Möchten Sie mit unserem Beispiel-HTML- und CSS-Inhalt die Funktion zum Anpassen der Benutzeroberfläche testen? Wir haben [ein einfaches Hilfeprogramm](active-directory-b2c-reference-ui-customization-helper-tool.md) bereitgestellt, das unsere Beispielinhalte in Ihr Blob-Speicherkonto hochlädt und konfiguriert. Wenn Sie das Programm verwenden, können Sie mit [Ändern von benutzerdefinierten Registrierungs- oder Anmelderichtlinien](#modify-your-sign-up-or-sign-in-custom-policy) fortfahren.

1. Öffnen Sie auf dem Blatt **Speicher** unter **Einstellungen** die Option **CORS**.
2. Klicken Sie auf **Hinzufügen**.
3. Geben Sie unter **Zulässige Ursprünge** ein Sternchen (\*) ein.
4. Wählen Sie in der Dropdownliste **Zulässige Verben** sowohl **GET** als auch **OPTIONS** aus.
5. Geben Sie unter **Zulässige Header** ein Sternchen (\*) ein.
6. Geben Sie unter **Verfügbar gemachte Header** ein Sternchen (\*) ein.
7. Geben Sie für **Maximales Alter (Sekunden)** den Wert **200** ein.
8. Klicken Sie auf **Hinzufügen**.

<a id="test-cors" class="xliff"></a>

## Testen von CORS

Überprüfen Sie, ob alles bereit ist, indem Sie die folgenden Schritte ausführen:

1. Navigieren Sie zur Website [test-cors.org](http://test-cors.org/), und fügen Sie anschließend die URL in das Feld **Remote-URL** ein.
2. Klicken Sie auf **Anforderung senden**.  
    Wenn ein Fehler ausgegeben wird, sollten Sie sich vergewissern, dass Ihre [CORS-Einstellungen](#configure-cors) richtig sind. Außerdem müssen Sie unter Umständen Ihren Browsercache löschen oder eine InPrivate-Browsersitzung öffnen, indem Sie STRG+UMSCHALT+P drücken.

<a id="modify-your-sign-up-or-sign-in-custom-policy" class="xliff"></a>

## Ändern von benutzerdefinierten Registrierungs- oder Anmelderichtlinien

Unter dem *\<TrustFrameworkPolicy\>*-Tag der obersten Ebene sollte sich das *\<BuildingBlocks\>*-Tag befinden. Fügen Sie in den *\<BuildingBlocks\>*-Tags ein *\<ContentDefinitions\>*-Tag hinzu, indem Sie das folgende Beispiel kopieren. Ersetzen Sie *your_storage_account* durch den Namen Ihres Speicherkontos.

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

<a id="upload-your-updated-custom-policy" class="xliff"></a>

## Hochladen der aktualisierten benutzerdefinierten Richtlinie

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) [in den Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und öffnen Sie dann das Blatt **Azure AD B2C**.
2. Klicken Sie auf **Alle Richtlinien**.
3. Klicken Sie auf **Richtlinie hochladen**.
4. Laden Sie `SignUpOrSignin.xml` mit dem zuvor hinzugefügten *\<ContentDefinitions\>*-Tag hoch.

<a id="test-the-custom-policy-by-using-run-now" class="xliff"></a>

## Testen der benutzerdefinierten Richtlinie mit **Jetzt ausführen**

1. Navigieren Sie auf dem Blatt **Azure AD B2C** zu **Alle Richtlinien**.
2. Wählen Sie die benutzerdefinierte Richtlinie aus, die hochgeladen werden soll, und klicken Sie auf die Schaltfläche **Jetzt ausführen**.
3. Sie sollten sich mit einer E-Mail-Adresse registrieren können.

<a id="reference" class="xliff"></a>

## Referenz

Beispielvorlagen für die Benutzeroberflächenanpassung finden Sie hier:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Der Ordner „sample_templates/wingtip“ enthält die folgenden HTML-Dateien:

| HTML5-Vorlage | Beschreibung |
|----------------|-------------|
| *phonefactor.html* | Verwenden Sie diese Datei als Vorlage für eine Seite für die mehrstufige Authentifizierung. |
| *resetpassword.html* | Verwenden Sie diese Datei als Vorlage für eine Seite vom Typ „Kennwort vergessen“. |
| *selfasserted.html* | Verwenden Sie diese Datei als Vorlage für eine Seite zum Anmelden an einem sozialen Netzwerk, eine Seite zum Registrieren für ein lokales Konto oder eine Seite zum Anmelden an einem lokalen Konto. |
| *unified.html* | Verwenden Sie diese Datei als Vorlage für eine einheitliche Registrierungs- oder Anmeldeseite. |
| *updateprofile.html* | Verwenden Sie diese Datei als Vorlage für eine Seite zum Aktualisieren von Profilen. |

Im Abschnitt [Ändern von benutzerdefinierten Registrierungs- oder Anmelderichtlinien](#modify-your-sign-up-or-sign-in-custom-policy) haben Sie die Inhaltsdefinition für `api.idpselections` konfiguriert. Der vollständige Satz mit IDs für die Inhaltsdefinition, die vom Azure AD B2C Identity Experience Framework erkannt werden, und die dazugehörigen Beschreibungen sind in der folgenden Tabelle enthalten:

| ID für Inhaltsdefinition | Beschreibung | 
|-----------------------|-------------|
| *api.error* | **Fehlerseite**: Diese Seite wird angezeigt, wenn eine Ausnahme oder ein Fehler auftreten. |
| *api.idpselections* | **Seite zur Auswahl des Identitätsanbieters**: Diese Seite enthält eine Liste mit den Identitätsanbietern, unter denen der Benutzer bei der Anmeldung wählen kann. Bei diesen Optionen handelt es sich um Unternehmensidentitätsanbieter, Identitätsanbieter in Form von sozialen Netzwerken wie Facebook und Google+ oder lokale Konten. |
| *api.idpselections.signup* | **Seite zur Auswahl des Identitätsanbieters für die Registrierung**: Diese Seite enthält eine Liste mit den Identitätsanbietern, aus denen der Benutzer bei der Registrierung auswählen kann. Bei diesen Optionen handelt es sich um Unternehmensidentitätsanbieter, Identitätsanbieter in Form von sozialen Netzwerken wie Facebook und Google+ oder lokale Konten. |
| *api.localaccountpasswordreset* | **Seite „Kennwort vergessen“**: Diese Seite enthält ein Formular, das vom Benutzer ausgefüllt werden muss, um eine Kennwortzurücksetzung zu initiieren.  |
| *api.localaccountsignin* | **Seite für Anmeldung mit lokalem Konto**: Diese Seite enthält ein Anmeldeformular für die Anmeldung mit einem lokalen Konto, das auf einer E-Mail-Adresse oder einem Benutzernamen basiert. Das Formular kann ein Texteingabefeld und ein Kennworteingabefeld enthalten. |
| *api.localaccountsignup* | **Seite für Registrierung mit lokalem Konto**: Diese Seite enthält ein Registrierungsformular für die Registrierung für ein lokales Konto, das auf einer E-Mail-Adresse oder einem Benutzernamen basiert. Das Formular kann verschiedene Eingabesteuerelemente enthalten, z.B. ein Texteingabefeld, ein Kennworteingabefeld, ein Optionsfeld, Dropdownfelder mit einer Auswahlmöglichkeit und Kontrollkästchen mit mehreren Optionen. |
| *api.phonefactor* | **Seite „Multi-Factor Authentication“**: Auf dieser Seite können Benutzer während der Registrierung oder Anmeldung ihre Telefonnummern verifizieren (per SMS oder Sprachnachricht). |
| *api.selfasserted* | **Seite zur Registrierung über Konto für soziales Netzwerk**: Diese Seite enthält ein Registrierungsformular, das Benutzer ausfüllen müssen, wenn die Registrierung mit einem vorhandenen Konto bei einem sozialen Netzwerk wie Facebook oder Google+ als Identitätsanbieter erfolgt. Diese Seite ähnelt der vorherigen Seite für die Registrierung bei einem Konto für ein soziales Netzwerk, mit Ausnahme der Felder für die Kennworteingabe. |
| *api.selfasserted.profileupdate* | **Seite für Profilaktualisierung**: Diese Seite enthält ein Formular, das von Benutzern zum Aktualisieren des Profils verwendet werden kann. Diese Seite ähnelt der Seite für die Registrierung bei einem Konto für ein soziales Netzwerk, mit Ausnahme der Felder für die Kennworteingabe. |
| *api.signuporsignin* | **Einheitliche Seite für Registrierung oder Anmeldung**: Auf dieser Seite wird sowohl die Registrierung als auch die Anmeldung von Benutzern verarbeitet, die dafür Unternehmensidentitätsanbieter, Identitätsanbieter sozialer Netzwerke, z.B. Facebook oder Google+, oder lokale Konten verwenden können.  |

<a id="next-steps" class="xliff"></a>

## Nächste Schritte

Weitere Informationen dazu, welche Elemente der Benutzeroberfläche angepasst werden können, finden Sie unter [Referenzhandbuch zur Anpassung der Benutzeroberfläche für integrierte Richtlinien](active-directory-b2c-reference-ui-customization.md).

