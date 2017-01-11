---
title: "Azure Active Directory B2C: Hilfsprogramm für die Anpassung der Seiten-UI | Microsoft Docs"
description: "Ein Hilfsprogramm, mit dem das Anpassungsfeature für die Seiten-UI in Azure Active Directory B2C veranschaulicht werden kann"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: ae935d52-3520-4a94-b66e-b35bb40e7514
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: swkrish
translationtype: Human Translation
ms.sourcegitcommit: f9c1e41c423cc0d638d5c1d273d58b2fe713fa1c
ms.openlocfilehash: 2aeb1f20255264ca97b8aff6bd506b5fa1832e28


---
# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: Ein Hilfsprogramm, mit dem das Anpassungsfeature für die Seitenbenutzeroberfläche (UI) veranschaulicht werden kann
Dies ist ein Begleitartikel zum [Hauptartikel zur Anpassung der Benutzeroberfläche](active-directory-b2c-reference-ui-customization.md) in Azure Active Directory (Azure AD) B2C. In den folgenden Schritten wird beschrieben, wie Sie das Feature zum Anpassen der Seiten-UI verwenden. Zu diesem Zweck haben wir HTML- und CSS-Beispielinhalte bereitgestellt.

## <a name="get-an-azure-ad-b2c-tenant"></a>Erhalten eines Azure AD-B2C-Mandanten
Bevor Sie Anpassungen vornehmen können, müssen Sie sich einen [Azure AD B2C-Mandanten beschaffen](active-directory-b2c-get-started.md) , falls Sie diesen Schritt noch nicht durchgeführt haben.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Erstellen einer Registrierungs- oder Anmelderichtlinie
Die von uns bereitgestellten Beispielinhalte können zum Anpassen von zwei Seiten in einer [Registrierungs- oder Anmelderichtlinie](active-directory-b2c-reference-policies.md) verwendet werden: der [vereinheitlichten Anmeldeseite](active-directory-b2c-reference-ui-customization.md) und der [Seite mit selbst bestätigten Attributen](active-directory-b2c-reference-ui-customization.md). Fügen Sie beim [Erstellen der Registrierungs- oder Anmelderichtlinie](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy)das lokale Konto (E-Mail-Adresse), Facebook, Google und Microsoft als **Identitätsanbieter**hinzu. Dies sind die einzigen Identitätsanbieter (IdPs), die für die HTML-Beispielinhalte akzeptiert werden.  Wenn Sie möchten, können Sie auch eine Teilmenge dieser Identitätsanbieter hinzufügen.

## <a name="register-an-application"></a>Registrieren einer Anwendung
Sie müssen das [Registrieren einer Anwendung](active-directory-b2c-app-registration.md) in Ihrem B2C-Mandanten durchführen, der zum Ausführen Ihrer Richtlinie verwendet werden kann. Nach dem Registrieren der Anwendung stehen Ihnen einige Optionen zum eigentlichen Ausführen der Registrierungsrichtlinie zur Verfügung:

* Erstellen Sie eine der Azure AD B2C-Schnellstart-Anwendungen, die unter [Registrieren und Anmelden von Kunden in den Anwendungen](active-directory-b2c-overview.md#get-started)im Abschnitt „Erste Schritte“ aufgeführt sind.
* Verwenden Sie die fertige Anwendung [Azure AD B2C Playground](https://aadb2cplayground.azurewebsites.net) . Wenn Sie die Playground-Anwendung wählen, müssen Sie eine Anwendung mit dem **Umleitungs-URI** `https://aadb2cplayground.azurewebsites.net/` im B2C-Mandanten registrieren.
* Verwenden Sie im **Azure-Portal** die Schaltfläche [Jetzt ausführen](https://portal.azure.com/)für Ihre Richtlinie.

## <a name="customize-your-policy"></a>Anpassen der Richtlinie
Um das Aussehen und Verhalten Ihrer Richtlinie anzupassen, müssen Sie zuerst HTML- und CSS-Dateien anhand der speziellen Konventionen von Azure AD B2C erstellen. Anschließend können Sie Ihre statischen Inhalte an einen öffentlich zugänglichen Speicherort hochladen, damit von Azure AD B2C darauf zugegriffen werden kann. Dies kann ein eigener dedizierter Webserver, Azure Blob Storage, Azure Content Delivery Network oder ein anderer Hostinganbieter für statische Ressourcen sein. Die einzigen Voraussetzungen sind, dass Ihre Inhalte per HTTPS zugänglich sind und dass darauf per CORS zugegriffen werden kann. Nachdem Sie Ihre statischen Inhalte im Web verfügbar gemacht haben, können Sie die Richtlinie so ändern, dass sie auf diesen Speicherort verweist. Anschließend können Sie die Inhalte für Ihre Kunden bereitstellen. Im [Hauptartikel zur Anpassung der Benutzeroberfläche](active-directory-b2c-reference-ui-customization.md) wird ausführlich beschrieben, wie das Azure AD B2C-Feature für die Anpassung funktioniert.

Für dieses Lernprogramm haben wir bereits einige Beispielinhalte erstellt und auf einem Azure-BLOB-Speicher gehostet. Die Beispielinhalte umfassen eine sehr einfache Anpassung des Designs unseres fiktiven Unternehmens „Wingtip Toys“. Gehen Sie folgendermaßen vor, um dies für Ihre eigene Richtlinie auszuprobieren:

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com/) bei Ihrem Mandanten an, und navigieren Sie zum Blatt mit den B2C-Funktionen.
2. Klicken Sie auf **Registrierungs- oder Anmelderichtlinien**, und klicken Sie dann auf Ihre Richtlinie (z.B. „b2c\_1\_sign\_up\_sign\_in“).
3. Klicken Sie auf **Seite für die Benutzeroberflächenanpassung** und anschließend auf **Einheitliche Seite für Registrierung oder Anmeldung**.
4. Stellen Sie die Option **Benutzerdefinierte Seite verwenden** auf **Ja** ein. Geben Sie im Feld **Benutzerdefinierter Seiten-URI** den Wert `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html` ein. Klicken Sie auf **OK**.
5. Klicken Sie auf **Registrierungsseite für lokales Konto**. Stellen Sie die Option **Benutzerdefinierte Vorlage verwenden** auf **Ja** ein. Geben Sie im Feld **Benutzerdefinierter Seiten-URI** den Wert `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html` ein.
6. Wiederholen Sie den gleichen Schritt für die **Social account sign-up page**(Registrierungsseite für Konten sozialer Netzwerke).
   Klicken Sie zweimal auf **OK** , um die Blätter für die UI-Anpassung zu schließen.
7. Klicken Sie auf **Speichern**.

Nun können Sie Ihre angepasste Richtlinie ausprobieren. Sie können eine eigene Anwendung oder die Anwendung „Azure AD B2C Playground“ verwenden. Alternativ dazu können Sie aber auch auf dem Blatt für die Richtlinie einfach auf den Befehl **Jetzt ausführen** klicken. Wählen Sie Ihre Anwendung im Dropdownfeld aus, und wählen Sie den entsprechenden Umleitungs-URI. Klicken Sie auf die Schaltfläche **Jetzt ausführen**. Eine neue Browserregisterkarte wird geöffnet, und Sie können die Benutzeroberfläche mit dem neuen Inhalt für die Registrierung bei Ihrer Anwendung verwenden.

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>Hochladen der Beispielinhalte in den Azure-BLOB-Speicher
Wenn Sie den Azure-BLOB-Speicher zum Hosten Ihrer Beispielinhalte verwenden möchten, können Sie ein eigenes Speicherkonto erstellen und unser B2C-Hilfstool zum Hochladen Ihrer Dateien nutzen.

### <a name="create-a-storage-account"></a>Speicherkonto erstellen
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie auf **+ Neu** > **Daten und Speicher** > **Speicherkonto**. Sie benötigen ein Azure-Abonnement, um ein Azure-Blobspeicherkonto zu erstellen. Sie können sich auf der [Azure-Website](https://azure.microsoft.com/pricing/free-trial/)für eine kostenlose Testversion registrieren.
3. Geben Sie einen **Namen** für das Speicherkonto an (z.B. „contoso“), und treffen Sie eine geeignete Auswahl für **Tarif**, **Ressourcengruppe** und **Abonnement**. Stellen Sie sicher, dass die Option **An Startmenü anheften** aktiviert ist. Klicken Sie auf **Erstellen**.
4. Wechseln Sie zurück zum Startmenü, und klicken Sie auf das Speicherkonto, das Sie gerade erstellt haben.
5. Klicken Sie im Abschnitt **Zusammenfassung** auf **Container** und dann auf **+ Hinzufügen**.
6. Geben Sie einen **Namen** für den Container an (z.B. „b2c“), und wählen Sie als **Zugriffstyp** die Einstellung **Blob** aus. Klicken Sie auf **OK**.
7. Der Container, den Sie erstellt haben, wird in der Liste auf dem Blatt **Blobs** angezeigt. Notieren Sie sich die URL des Containers. Sie sollte ähnlich wie die folgende URL aussehen: `https://contoso.blob.core.windows.net/b2c`. Schließen Sie das Blatt **Blobs**.
8. Klicken Sie auf dem Blatt für das Speicherkonto auf **Schlüssel**, und notieren Sie sich die Werte der Felder **Speicherkontoname** und **Primärer Zugriffsschlüssel**.

> [!NOTE]
> **Primärer Zugriffsschlüssel** ist eine wichtige Anmeldeinformation.
> 
> 

### <a name="download-the-helper-tool-and-sample-files"></a>Herunterladen des Hilfstools und der Beispieldateien
Sie können das [Azure Blob Storage-Hilfsprogramm und die Beispieldateien als ZIP-Datei herunterladen](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) oder von GitHub klonen:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Dieses Repository enthält das Verzeichnis `sample_templates\wingtip` mit HTML- und CSS-Beispieldaten und Bildern. Damit diese Vorlagen auf Ihr eigenes Azure Blob Storage-Konto verweisen, müssen Sie die HTML-Dateien bearbeiten. Öffnen Sie `unified.html` und `selfasserted.html`, und ersetzen Sie alle Instanzen von `https://localhost` durch die URL Ihres eigenen Containers, die Sie in den vorherigen Schritten notiert haben. Sie müssen den absoluten Pfad der HTML-Dateien verwenden, da die HTML-Daten in diesem Fall von Azure AD unter der Domäne `https://login.microsoftonline.com`bereitgestellt werden.

### <a name="upload-the-sample-files"></a>Hochladen der Beispieldateien
Entzippen Sie in demselben Repository die Datei `B2CAzureStorageClient.zip`, und führen Sie die darin enthaltene Datei `B2CAzureStorageClient.exe` aus. Mit diesem Programm werden alle Dateien im von Ihnen angegebenen Verzeichnis einfach in Ihr Speicherkonto hochgeladen, und der CORS-Zugriff für diese Dateien wird aktiviert. Wenn Sie die obigen Schritte befolgt haben, wird in den HTML- und CSS-Dateien jetzt auf Ihr Speicherkonto verwiesen. Beachten Sie, dass der Name Ihres Speicherkontos vor `blob.core.windows.net` steht, z.B. `contoso`. Sie können prüfen, ob der Inhalt ordnungsgemäß hochgeladen wurde, indem Sie versuchen, auf `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` in einem Browser zuzugreifen. Sie können auch [http://test-cors.org/](http://test-cors.org/) verwenden, um sicherzustellen, dass der Inhalt jetzt für CORS aktiviert ist. (Suchen Sie im Ergebnis nach „XHR status: 200“.)

### <a name="customize-your-policy-again"></a>Erneutes Anpassen der Richtlinie
Nachdem Sie die Beispielinhalte in Ihr eigenes Speicherkonto hochgeladen haben, müssen Sie die Registrierungsrichtlinie so ändern, dass darauf verwiesen wird. Wiederholen Sie die Schritte aus dem obigen Abschnitt [Anpassen der Richtlinie](#customize-your-policy) , und verwenden Sie dabei jetzt die URLs Ihres eigenen Speicherkontos. Der Speicherort der Datei `unified.html` würde beispielsweise `<url-of-your-container>/wingtip/unified.html` lauten.

Sie können nun die Schaltfläche **Jetzt ausführen** oder Ihre eigene Anwendung verwenden, um die Richtlinie erneut auszuführen. Das Ergebnis sollte nahezu identisch sein, da Sie in beiden Fällen dieselben HTML- und CSS-Beispieldaten verwendet haben. Die Richtlinien verweisen jetzt aber auf Ihre eigene Instanz von Azure Blob Storage, und Sie können die Dateien wie gewünscht bearbeiten und neu hochladen. Weitere Informationen zum Anpassen der HTML- und CSS-Daten finden Sie im [Hauptartikel zur Anpassung der Benutzeroberfläche](active-directory-b2c-reference-ui-customization.md).




<!--HONumber=Dec16_HO4-->


