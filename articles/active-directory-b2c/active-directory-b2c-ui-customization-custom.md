---
title: "Azure Active Directory B2C: Anpassung der Benutzeroberfläche durch benutzerdefinierte Richtlinien | Microsoft-Dokumentation"
description: "Ein Thema zur Anpassung der Benutzeroberfläche (UI) bei der Verwendung von benutzerdefinierten Richtlinien in Azure AD B2C"
services: active-directory-b2c
documentationcenter: 
author: saeeda
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
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 215f9baeda7cd0bcf3fd66893919575647849e7d
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017

---
# <a name="azure-active-directory-b2c-ui-customization-in-a-custom-policy"></a>Azure Active Directory B2C: Anpassung der Benutzeroberfläche durch eine benutzerdefinierte Richtlinie

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Nach Durchführung dieses Artikels haben Sie eine benutzerdefinierte Registrierungs- oder Anmelderichtlinie mit Ihrer Marke und Darstellung erstellt.  Azure AD B2C ermöglicht eine nahezu vollständige Steuerung der HTML- und CSS-Inhalte, die dem Endbenutzer angezeigt werden.  Bei Verwendung einer benutzerdefinierten Richtlinie wird die Anpassung der Benutzeroberfläche in XML statt über Steuerelemente im Azure-Portal konfiguriert.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie fortfahren, müssen Sie den Abschnitt [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) lesen.  Sie sollten eine funktionierende benutzerdefinierte Richtlinie für die Registrierung und Anmeldung bei lokalen Konten besitzen.

### <a name="confirming-your-b2c-tenant"></a>Bestätigen Ihres B2C-Mandanten

Da sich benutzerdefinierte Richtlinien noch in der privaten Vorschau befinden, vergewissern Sie sich, dass Ihr Azure AD B2C-Mandant für den Upload benutzerdefinierter Richtlinien aktiviert ist:

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) [in den Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und öffnen Sie das Blatt „Azure AD B2C“.
1. Klicken Sie auf **Alle Richtlinien**.
1. Stellen Sie sicher, dass **Richtlinie hochladen** verfügbar ist.  Wenn die Schaltfläche deaktiviert ist, senden Sie eine E-Mail an AADB2CPreview@microsoft.com.

## <a name="the-page-ui-customization-feature"></a>Das Anpassungsfeature für die Seiten-UI

Mit der Funktion zum Anpassen der Benutzeroberfläche können Sie das Aussehen und Verhalten benutzerdefinierter Richtlinien anpassen.  Verwalten Sie die Konsistenz von Marken und visuellen Elementen zwischen Ihrer Anwendung und Azure AD B2C.

So funktioniert es: Azure AD B2C führt den Code im Consumer-Browser aus und verwendet einen modernen Ansatz namens [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).  Zuerst legen Sie eine URL in der benutzerdefinierten Richtlinie mit benutzerdefiniertem HTML-Inhalt fest.  Azure AD B2C führt die UI-Elemente mit dem HTML-Inhalt, der von der URL geladen wird, zusammen und zeigt die Seite für den Consumer an.

## <a name="creating-your-html5-content"></a>Erstellen von HTML5-Inhalten

Erstellen wir den HTML-Inhalt mit dem Markennamen Ihres Produkts im Titel.

1. Klicken Sie in diesem HTML-Ausschnitt auf **Kopieren**.  Hierbei handelt es sich um einen wohlgeformten HTML5-Ausschnitt mit einem leeren Element namens `<div id="api"></div>`, das sich an einer beliebigen Stelle in `<body>` befindet. Dieses Element markiert die Stelle, an der der Azure AD B2C-Inhalt eingefügt wird.

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

1. Fügen Sie diesen in einen Texteditor ein und speichern Sie die Datei als `customize-ui.html`.

## <a name="create-a-blob-storage-account"></a>Erstellen eines Blob Storage-Kontos

>[!NOTE]
> In diesem Handbuch wird Azure Blob Storage zum Hosting unserer Inhalte verwendet.  Sie können Ihre Inhalte auch auf einem Webserver hosten, müssen jedoch [Cross-Origin Resource Sharing (CORS) auf Ihrem Webserver aktivieren](https://enable-cors.org/server.html).

Hosten wir diesen HTML-Inhalt im Azure Blob Storage.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
1. Wählen Sie im Menü „Hub“ die Option **Neu** -> **Speicher** -> **Speicherkonto**.
1. Geben Sie einen eindeutigen **Namen** für Ihr Speicherkonto ein.
1. Übernehmen Sie für **Bereitstellungsmodell** die Einstellung **Resource Manager**.
1. Ändern Sie **Kontoart** in **Blob Storage**.
1. Übernehmen Sie für **Leistung** die Einstellung **Standard**.
1. Übernehmen Sie für **Replikation** die Einstellung **RA-GRS**.
1. Übernehmen Sie für **Zugriffstarif** die Einstellung **Heiße Daten**.
1. Übernehmen Sie für **Speicherdienstverschlüsselung** die Einstellung **Deaktiviert**.
1. Wählen Sie ein **Abonnement** für Ihr Speicherkonto aus.
1. Erstellen Sie eine **Ressourcengruppe**, oder wählen Sie eine vorhandene Ressourcengruppe aus.
1. Wählen Sie einen Wert für **Geografischer Standort** für das Speicherkonto aus.
1. Klicken Sie auf **Erstellen** , um das Speicherkonto zu erstellen.
1. Warten Sie, bis die Bereitstellung abgeschlossen ist, und das Blatt für das Speicherkonto automatisch geöffnet wird.

## <a name="create-a-container"></a>Erstellen eines Containers

Erstellen wir einen öffentlichen Container im Azure Blob Storage.

1. Wechseln Sie zur Registerkarte **Übersicht** auf der linken Seite.
1. Klicken Sie auf **+ Container**.
1. Geben Sie für **Name** den Wert `$root` ein.
1. Setzen Sie **Zugriffstyp** auf **Blob**.
1. Klicken Sie auf „$root“, um den neuen Container zu öffnen.
1. Klicken Sie auf **Hochladen**.
1. Klicken Sie auf das Ordnersymbol neben `Select a file`.
1. Navigieren Sie zur Datei `customize-ui.html`, die wir in im [vorherigen Abschnitt](#the-page-ui-customization-feature) erstellt haben.
1. Klicken Sie auf **Hochladen**.
1. Wählen Sie das hochgeladene Blob namens `customize-ui.html` aus.
1. Klicken Sie neben **URL** auf die Schaltfläche zum Kopieren.
1. Öffnen Sie einen Browser, und navigieren Sie zu dieser URL.  Falls ein Zugriff darauf nicht möglich sein sollte, stellen Sie sicher, dass der Zugriffstyp für den Container auf „Blob“ gesetzt ist.

## <a name="configure-cors"></a>Konfigurieren von CORS

Als Nächstes konfigurieren wir den Azure Blob Storage für Cross-Origin Resource Sharing (CORS).

>[!NOTE]
>Möchten Sie mit unserem Beispiel-HTML- und CSS-Inhalt die Funktion zum Anpassen der Benutzeroberfläche testen?  Wir haben [ein einfaches Hilfeprogramm](active-directory-b2c-reference-ui-customization-helper-tool.md) bereitgestellt, das unsere Beispielinhalte in Ihr Azure Blob Storage-Konto hochlädt und konfiguriert.  Wenn Sie das Programm verwenden, fahren Sie mit [Ändern von benutzerdefinierten Registrierungs- oder Anmelderichtlinien](#modify-your-sign-up-or-sign-in-custom-policy) fort.

1. Öffnen Sie auf dem Blatt für den Speicher unter „Einstellungen“ **CORS**.
1. Klicken Sie auf **+ Hinzufügen**.
1. Setzen Sie `Allowed origins` auf `*`.
1. Wählen Sie in der Dropdownliste `Allowed verbs` sowohl `GET` als auch `OPTIONS` aus.
1. Setzen Sie `Allowed headers` auf `*`.
1. Setzen Sie `Exposed headers` auf `*`.
1. Setzen Sie `Maximum age (seconds)` auf `200`.
1. Klicken Sie auf **Hinzufügen**.

## <a name="testing-cors"></a>Testen von CORS

Überprüfen wir, ob wir bereit sind.

1. Navigieren Sie zu http://test-cors.org/, und fügen Sie die URL in das Feld `Remote URL` ein.
1. Klicken Sie auf **Anforderung senden**.
1. Wenn ein Fehler ausgegeben wird, vergewissern Sie sich, dass Ihre [CORS-Einstellungen](#configure-cors) richtig sind.  Möglicherweise müssen Sie auch Ihren Browsercache leeren oder es mit einer InPrivate-Browsersitzung über `CTRL-SHIFT-P` versuchen.

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a>Ändern von benutzerdefinierten Registrierungs- oder Anmelderichtlinien

1. Unter dem Tag `<TrustFrameworkPolicy>` auf der obersten Ebene sollten Sie nach dem Tag `<BuildingBlocks>` suchen.  Fügen Sie im Tag `<BuildingBlocks>` einen `ContentDefinitions`-Tag hinzu, indem Sie dieses Beispiel kopieren.  Ersetzen Sie `{your_storage_account}` durch den Namen Ihres Speicherkontos.

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a>Hochladen der aktualisierten benutzerdefinierten Richtlinie

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) [in den Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und öffnen Sie das Blatt „Azure AD B2C“.
1. Klicken Sie auf **Alle Richtlinien**.
1. Klicken Sie auf **Richtlinie hochladen**.
1. Laden Sie `SignUpOrSignin.xml` mit dem hinzugefügten Tag `ContentDefinitions` hoch.

## <a name="test-the-custom-policy-using-run-now"></a>Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“

1. Öffnen Sie das Blatt **Azure AD B2C**, und navigieren Sie zu **Alle Richtlinien**.
1. Wählen Sie die benutzerdefinierte Richtlinie aus, die hochgeladen werden soll, und klicken Sie auf die Schaltfläche **Jetzt ausführen**.
1. Sie sollten sich mit einer E-Mail-Adresse registrieren können.

## <a name="next-steps"></a>Nächste Schritte

Das [Referenzhandbuch zur Anpassung der Benutzeroberfläche für integrierte Richtlinien](active-directory-b2c-reference-ui-customization.md) enthält zusätzliche Informationen darüber, welche UI-Elemente angepasst werden können.  Bei der Anpassung der Benutzeroberfläche besteht kein Unterschied zwischen integrierten und benutzerdefinierten Richtlinien.

