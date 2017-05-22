---
title: 'Azure Active Directory B2C: Referenz: Anpassen der UI einer User Journey mit benutzerdefinierten Richtlinien | Microsoft-Dokumentation'
description: Thema zu benutzerdefinierten Azure Active Directory B2C-Richtlinien
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 68f40aa638a687398512278a0b77d1ba392859cf
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---

# <a name="customize-the-ui-of-a-user-journey-with-custom-policies"></a>Anpassen der UI einer User Journey mit benutzerdefinierten Richtlinien

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

> [!NOTE]
> Dieser Artikel enthält eine erweiterte Beschreibung der Funktionsweise der UI-Anpassung und der Aktivierung mit benutzerdefinierten B2C-Richtlinien per Identity Experience Framework.


Ein reibungslose Benutzeroberfläche ist für jede B2C-Lösung (Business to Consumer) von entscheidender Bedeutung. Reibungslose Benutzeroberfläche bedeutet in diesem Zusammenhang, dass sich eine „User Journey“ durch unseren Dienst – ob per Gerät oder Browser – sich nicht von der User Journey des verwendeten Diensts des Kunden unterscheidet.

## <a name="understand-the-cors-way-for-ui-customization"></a>Verinnerlichen des CORS-Ansatzes für die UI-Anpassung

Mit Azure AD B2C können Sie das Aussehen und Verhalten der Benutzeroberfläche auf den unterschiedlichen Seiten anpassen, die ggf. bereitgestellt und mit Azure AD B2C über Ihre benutzerdefinierten Richtlinien angezeigt werden können.

Zu diesem Zweck wird von Azure AD B2C Code im Browser des Kunden ausgeführt und der moderne Standardansatz [Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS)](http://www.w3.org/TR/cors/) verwendet, um für benutzerdefinierten Inhalt von einer bestimmten URL, die Sie in einer benutzerdefinierten Richtlinie angeben, auf Ihre HTML5/CSS-Vorlagen zu verweisen. CORS ist ein Mechanismus, mit dem eingeschränkte Ressourcen, z.B. Schriftarten, auf einer Webseite von einer anderen Domäne aus angefordert werden können, die sich außerhalb der Ursprungsdomäne der Ressource befindet.

Im Vergleich zum herkömmlichen Ansatz, bei dem sich Vorlagenseiten im Besitz der Lösung befinden, nur eingeschränkte Textelemente und Bilder bereitgestellt werden und eine eingeschränkte Steuerung des Layouts und Aussehens möglich war und sich Schwierigkeiten in Bezug auf die Bereitstellung einer reibungslosen Oberfläche ergeben haben, werden beim CORS-Ansatz HTML5 und CSS unterstützt. Dies eröffnet Ihnen folgende Möglichkeiten:

- Sie hosten den Inhalt, und die Lösung fügt dann die Steuerelemente der Lösung per clientseitigem Skript ein.
- Sie haben die vollständige Kontrolle über jedes Pixel des Layouts und somit des Erscheinungsbilds.

Sie können beliebig viele Inhaltsseiten bereitstellen, indem Sie entsprechende HTML5/CSS-Dateien erstellen.

> [!NOTE]
> Aus Sicherheitsgründen ist die Nutzung von JavaScript für die Anpassung derzeit blockiert. Zum Aufheben der Blockierung für JavaScript ist die Verwendung eines benutzerdefinierten Domänennamens für Ihren Azure AD B2C-Mandanten erforderlich.

Geben Sie in Ihren HTML5/CSS-Vorlagen jeweils ein *anchor*-Element an, das dem erforderlichen `<div id=”api”>`-Element im HTML-Code oder auf der Inhaltsseite (unten dargestellt)Inhaltsseite entspricht. Für Azure AD B2C müssen alle Inhaltsseiten über dieses spezifische div-Element verfügen.

```
<!DOCTYPE html>
<html>
  <head>
    <title>Your page content’s tile!</title>
  </head>
  <body>
    <div id="api"></div>
  </body>
</html>
```

Azure AD B2C-bezogener Inhalt für die Seite wird in dieses div-Element eingefügt, während der Rest der Seite von Ihnen gesteuert werden kann. Mit dem JavaScript-Code von Azure AD B2C wird Ihr Inhalt eingefügt, und unser HTML-Code wird in dieses spezifische div-Element eingefügt. Azure AD B2C fügt nach Bedarf die folgenden Steuerelemente ein: Steuerelement für die Kontoauswahl, Steuerelemente für die Anmeldung, Multi-Factor-Steuerelemente (derzeit Smartphone-basiert) und Steuerelemente für die Attributsammlung. Mit Azure AD B2C wird sichergestellt, dass alle Steuerelemente HTML5-konform und zugänglich sind, der Stil aller Steuerelemente vollständig angepasst werden kann und keine ältere Steuerelementversion verwendet wird.

Schließlich wird der zusammengeführte Inhalt Ihrem Kunden als dynamisches Dokument angezeigt.

Führen Sie folgende Schritte aus, um dafür zu sorgen, dass dies wie gewünscht funktioniert:

- Sicherstellen, dass Ihr Inhalt HTML5-konform und zugänglich ist
- Sicherstellen, dass Ihr Inhaltsserver für CORS aktiviert ist
- Bereitstellen von Inhalt per HTTPS
- Verwenden von absoluten URLs, z.B. „https://ihredomäne/content“, für alle Links und den gesamten CSS-Inhalt.

> [!TIP]
> Sie können die Website „http://test-cors.org/“ verwenden, um zu bestätigen, dass die von Ihnen gehostete Website für Ihren Inhalt über eine CORS-Aktivierung verfügt, und um CORS-Anforderungen zu testen. Dank dieser Website können Sie entweder einfach die CORS-Anforderung an einen Remoteserver senden (um zu testen, ob CORS unterstützt wird), oder die CORS-Anforderung an einen Testserver senden (um bestimmte Features von CORS auszuprobieren).

> [!TIP]
> Auch die Website „http://enable-cors.org/“ ist eine sehr nützliche Ressource für CORS.

Aufgrund dieses CORS-basierten Ansatzes verfügen die Endbenutzer dann in Bezug auf Ihre Anwendung und die Seiten, die von Azure AD B2C bereitgestellt werden, über einheitliche Oberflächen.

## <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto.

Als Voraussetzung hierfür müssen Sie ein Speicherkonto erstellen. Sie benötigen ein Azure-Abonnement, um ein Azure-Blobspeicherkonto zu erstellen. Sie können sich auf der [Azure-Website](https://azure.microsoft.com/en-us/pricing/free-trial/)für eine kostenlose Testversion registrieren.

1. Öffnen Sie eine Browsersitzung, und navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Melden Sie sich mit Ihren Administratoranmeldeinformationen an.
3. Klicken Sie auf **Neu** > **Daten und Speicher** > **Speicherkonto**.  Das Blatt **Speicherkonto erstellen** wird geöffnet.
4. Geben Sie unter **Name** einen Namen für das Speicherkonto ein, z.B. *contoso369b2c*. Dieser Wert wird später als *storageAccountName* bezeichnet.
5. Treffen Sie die gewünschte Auswahl für die Bereiche Tarif, Ressourcengruppe und Abonnement. Stellen Sie sicher, dass die Option **An Startmenü anheften** aktiviert ist. Klicken Sie auf **Erstellen**.
6. Wechseln Sie zurück zum Startmenü, und klicken Sie auf das Speicherkonto, das Sie gerade erstellt haben.
7. Klicken Sie im Abschnitt **Dienste** auf **Blobs**. Das Blatt **Blob-Dienst** wird geöffnet.
8. Klicken Sie auf **+ Container**.
9. Geben Sie unter **Name** einen Namen für den Container ein, z.B. *b2c*. Auf diesen Wert wird später als *containerName* verwiesen.
9. Wählen Sie **Blob** als **Zugriffstyp** aus. Klicken Sie auf **Erstellen**.
10. Der Container, den Sie erstellt haben, wird in der Liste auf dem Blatt **Blob-Dienst** angezeigt.
11. Schließen Sie das Blatt **Blobs**.
12.    Klicken Sie auf dem Blatt **Speicherkonto** auf das **Schlüssel**-Symbol. Das Blatt **Zugriffsschlüssel** wird geöffnet.  
13.    Notieren Sie sich den Wert von **key1**. Auf diesen Wert wird später als *key1* verwiesen.

## <a name="downloading-the-helper-tool"></a>Herunterladen des Hilfstools

1.    Laden Sie das Hilfstool von [GitHub](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) herunter.
2.    Speichern Sie die Datei *B2C-AzureBlobStorage-Client-master.zip* auf Ihrem lokalen Computer.
3.    Extrahieren Sie den Inhalt der Datei „B2C-AzureBlobStorage-Client-master.zip“ auf Ihren lokalen Datenträger, z.B. im Ordner **UI-Customization-Pack**. Darunter wird der Ordner *B2C-AzureBlobStorage-Client-master* erstellt.
4.    Öffnen Sie diesen Ordner, und extrahieren Sie den Inhalt der Archivdatei *B2CAzureStorageClient.zip* in den Ordner.

## <a name="upload-the-ui-customization-pack-sample-files"></a>Hochladen der UI-Customization-Pack-Beispieldateien

1.    Navigieren Sie mit Windows-Explorer zum Ordner *B2C-AzureBlobStorage-Client-master* unter dem Ordner *UI-Customization-Pack*, der im vorherigen Abschnitt erstellt wurde.
2.    Führen Sie die Datei *B2CAzureStorageClient.exe* aus. Mit diesem Programm werden alle Dateien im von Ihnen angegebenen Verzeichnis einfach in Ihr Speicherkonto hochgeladen, und der CORS-Zugriff für diese Dateien wird aktiviert.
3.    Geben Sie bei Aufforderung Folgendes an: a.    Den Namen des Speicherkontos (*storageAccountName*), z.B. *contoso369b2c*.
    b.    Den primären Zugriffsschlüssel Ihres Azure-Blobspeichers (*key1*), z.B. *contoso369b2c*.
    c.    Den Namen Ihres Blobspeichercontainers (*containerName*), z.B. *b2c*.
    d.    Den Pfad der *Starter-Pack*-Beispieldateien, z.B. *..\B2CTemplates\wingtiptoys*.

Wenn Sie die obigen Schritte ausgeführt haben, wird in den HTML5- und CSS-Dateien des *UI-Customization-Pack* für das fiktive Unternehmen **wingtiptoys** jetzt auf Ihr Speicherkonto verwiesen.  Sie können sich vergewissern, dass der Inhalt richtig hochgeladen wurde, indem Sie im Azure-Portal das entsprechende Containerblatt öffnen. Alternativ dazu können Sie das richtige Hochladen des Inhalts auch überprüfen, indem Sie über einen Browser auf die Seite zugreifen. Weitere Informationen finden Sie unter [Azure Active Directory B2C: Ein Hilfsprogramm, mit dem das Anpassungsfeature für die Seitenbenutzeroberfläche (UI) veranschaulicht werden kann](active-directory-b2c-reference-ui-customization-helper-tool.md).

## <a name="ensure-the-storage-account-has-cors-enabled"></a>Sicherstellen der CORS-Aktivierung für das Speicherkonto

CORS (Cross-Origin Resource Sharing) muss auf Ihrem Endpunkt aktiviert sein, damit Ihr Inhalt von Azure AD B2C Premium geladen werden kann. Der Grund ist, dass Ihr Inhalt in einer anderen Domäne als der Domäne gehostet wird, aus der Azure AD B2C Premium die Seite bereitstellt.

Führen Sie die folgenden Schritte aus, um sicherzustellen, dass für den Speicher, in dem Sie Ihren Inhalt hosten, CORS aktiviert ist:

1. Öffnen Sie eine Browsersitzung, und navigieren Sie zur Seite *unified.html*, indem Sie die vollständige URL des Standorts in Ihrem Speicherkonto verwenden: `https://<storageAccountName>.blob.core.windows.net/<containerName>/unified.html`. Beispiel: „https://contoso369b2c.blob.core.windows.net/b2c/unified.html“.
2. Navigieren Sie zu „http://test-cors.org“. Mit dieser Website können Sie überprüfen, ob für die verwendete Seite CORS aktiviert ist.  
<!--
![test-cors.org](../../media/active-directory-b2c-customize-ui-of-a-user-journey/test-cors.png)
-->

3. Geben Sie unter **Remote-URL** die vollständige URL für Ihren „unified.html“-Inhalt an, und klicken Sie auf **Anforderung senden**.
4. Vergewissern Sie sich, dass die Ausgabe im Abschnitt **Ergebnisse** den Text *XHR status: 200* enthält. Hiermit wird angegeben, dass CORS aktiviert ist.
<!--
![CORS enabled](../../media/active-directory-b2c-customize-ui-of-a-user-journey/cors-enabled.png)
-->
Das Speicherkonto sollte jetzt einen Blobcontainer mit dem Namen *b2c* in unserer Darstellung mit den folgenden wingtiptoys-Vorlagen aus dem *Starter-Pack* enthalten.

<!--
![Correctly configured storage account](../../articles/active-directory-b2c/media/active-directory-b2c-reference-customize-ui-custom/storage-account-final.png)
-->

In der folgenden Tabelle ist der Zweck der obigen HTML5-Seiten beschrieben.

| HTML5-Vorlage | Beschreibung |
|----------------|-------------|
| *phonefactor.html* | Diese Seite kann als Vorlage für eine Multi-Factor Authentication-Seite verwendet werden. |
| *resetpassword.html* | Diese Seite kann als Vorlage für eine Seite für ein vergessenes Kennwort verwendet werden. |
| *selfasserted.html* | Diese Seite kann als Vorlage für eine Seite zum Anmelden an einem sozialen Netzwerk, eine Seite zum Registrieren für ein lokales Konto oder eine Seite zum Anmelden an einem lokalen Konto verwendet werden. |
| *unified.html* | Diese Seite kann als Vorlage für eine einheitliche Registrierungs- oder Anmeldeseite verwendet werden. |
| *updateprofile.html* | Diese Seite kann als Vorlage für eine Seite für die Profilaktualisierung verwendet werden. |

## <a name="add-a-link-to-your-html5css-templates-to-your-user-journey"></a>Hinzufügen eines Links zu Ihren HTML5/CSS-Vorlagen für die User Journey

Sie können für Ihre User Journey einen Link hinzufügen, der zu Ihren HTML5/CSS-Vorlagen führt, indem Sie eine benutzerdefinierte Richtlinie direkt bearbeiten.

Die benutzerdefinierten HTML5/CSS-Vorlagen für Ihre User Journey müssen in einer Liste mit Inhaltsdefinitionen angegeben werden, die in diesen User Journeys verwendet werden können. Zu diesem Zweck muss ein optionales *<ContentDefinitions>*-XML-Element im Abschnitt *<BuildingBlocks>* der XML-Datei mit Ihrer benutzerdefinierten Richtlinie deklariert werden.

In der folgenden Tabelle werden die Gruppe mit den IDs der Inhaltsdefinitionen, die von der Azure AD B2C Identity Experience Engine erkannt werden, und der Typ der entsprechenden Seiten beschrieben.

| ID für Inhaltsdefinition | Beschreibung |
|-----------------------|-------------|
| *api.error* | **Fehlerseite**: Diese Seite wird angezeigt, wenn eine Ausnahme oder ein Fehler auftreten. |
| *api.idpselections* | **Seite zur Auswahl des Identitätsanbieters**: Diese Seite enthält eine Liste mit den Identitätsanbietern, unter denen der Benutzer bei der Anmeldung wählen kann. Dabei handelt es sich um Unternehmensidentitätsanbieter, Identitätsanbieter in Form von sozialen Netzwerken wie Facebook und Google+ oder lokale Konten (basierend auf E-Mail-Adresse oder Benutzername). |
| *api.idpselections.signup* | **Seite zur Auswahl des Identitätsanbieters für die Registrierung**: Diese Seite enthält eine Liste mit den Identitätsanbietern, aus denen der Benutzer bei der Registrierung auswählen kann. Dabei handelt es sich um Unternehmensidentitätsanbieter, Identitätsanbieter in Form von sozialen Netzwerken wie Facebook und Google+ oder lokale Konten (basierend auf E-Mail-Adresse oder Benutzername). |
| *api.localaccountpasswordreset* | **Seite „Kennwort vergessen“**: Diese Seite enthält ein Formular, das vom Benutzer ausgefüllt werden muss, um die Kennwortzurücksetzung zu initiieren.  |
| *api.localaccountsignin* | **Seite für Anmeldung mit lokalem Konto**: Diese Seite enthält ein Anmeldeformular. Es muss vom Benutzer bei der Anmeldung für ein lokales Konto ausgefüllt werden, das auf einer E-Mail-Adresse oder einem Benutzernamen basiert. Das Formular kann ein Texteingabefeld und ein Kennworteingabefeld enthalten. |
| *api.localaccountsignup* | **Seite für Registrierung mit lokalem Konto**: Diese Seite enthält ein Registrierungsformular, das der Benutzer bei der Registrierung für ein lokales Konto ausfüllen muss, das auf einer E-Mail-Adresse oder einem Benutzernamen basiert.  Das Formular kann verschiedene Eingabesteuerelemente enthalten, z. B. Texteingabefelder, Kennworteingabefelder, Optionsfelder, Dropdownfelder mit einer Auswahlmöglichkeit und Kontrollkästchen mit mehreren Optionen. |
| *api.phonefactor* | **Seite „Multi-Factor Authentication“**: Auf dieser Seite können Benutzer während der Registrierung oder Anmeldung ihre Telefonnummern überprüfen (per SMS oder Sprachnachricht). |
| *api.selfasserted* | **Seite zur Registrierung über Konto für soziales Netzwerk**: Diese Seite enthält ein Registrierungsformular, das der Benutzer ausfüllen muss, wenn die Registrierung mit einem vorhandenen Konto bei einem sozialen Netzwerk wie Facebook oder Google+ als Identitätsanbieter erfolgt. Mit Ausnahme der Felder zum Eingeben des Kennworts ähnelt diese Seite der obigen „Seite zur Registrierung über Konto für soziales Netzwerk“. |
| *api.selfasserted.profileupdate* | **Seite für Profilaktualisierung**: Diese Seite enthält ein Formular, das vom Benutzer zum Aktualisieren des Profils verwendet werden kann. Mit Ausnahme der Felder zum Eingeben des Kennworts ähnelt diese Seite der obigen „Seite zur Registrierung über Konto für soziales Netzwerk“. |
| *api.signuporsignin* | **Einheitliche Seite für Registrierung oder Anmeldung**:  Auf dieser Seite wird sowohl die Registrierung als auch die Anmeldung von Benutzern verarbeitet, die dafür Unternehmensidentitätsanbieter, Identitätsanbieter sozialer Netzwerke, z.B. Facebook oder Google+, oder lokale Konten verwenden können.

## <a name="next-steps"></a>Nächste Schritte
[Reference: Understand how custom policies work with the Identity Experience Framework in B2C](active-directory-b2c-reference-custom-policies-understanding-contents.md) (Referenz: Funktionsweise von benutzerdefinierten Richtlinien mit Identity Experience Framework in B2C)

