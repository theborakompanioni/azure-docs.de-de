---
title: "Hinzufügen Ihres Unternehmensbranding zur Anmelde- und Zugriffsbereichsseite in Azure Active Directory"
description: "Erfahren Sie, wie Sie Unternehmensbranding der Anmelde- und Zugriffsbereichsseite hinzufügen."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f74621b4-4ef0-4899-8c0e-0c20347a8c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/07/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 6d4fbfe97288fcb76628b45649b8b678152198a9
ms.lasthandoff: 04/07/2017


---
# <a name="add-company-branding-to-sign-in-and-access-panel-pages"></a>Hinzufügen Ihres Unternehmensbranding zur Anmelde- und Zugriffsbereichsseite
Viele Unternehmen möchten für die eigenen Websites und Dienste ein einheitliches Aussehen erzielen. Dies ist mit Azure Active Directory möglich, indem IT-Experten die Anpassung der Darstellung folgender Webseiten mit Firmenlogos und Bildern durchführen:

* **Anmeldeseite**: Diese Seite wird angezeigt, wenn sich Ihre Mitarbeiter und Gäste des Unternehmens an Office 365 oder anderen Anwendungen anmelden, für die Azure AD verwendet wird.
* **Zugriffsbereichsseite**: Der Zugriffsbereich ist ein webbasiertes Portal, mit dem Sie die cloudbasierten Anwendungen anzeigen und starten können, für die Ihr Azure AD-Administrator Ihnen Zugriff gewährt hat. Den Zugriffsbereich finden Sie unter [https://myapps.microsoft.com](https://myapps.microsoft.com).

In diesem Thema wird beschrieben, wie Sie die Anmeldeseite und die Zugriffsbereichsseite anpassen können.

> [!NOTE]
> * Die Funktion für das Unternehmensbranding ist nur verfügbar, wenn Sie Ihre Azure Active Directory-Instanz auf die Premium oder Basic Edition aktualisiert haben oder im Besitz einer Office 365-Lizenz sind. Weitere Informationen finden Sie unter „Azure Active Directory-Editionen“.
> 
> * Die Azure Active Directory Premium und Basic Editions stehen für Kunden in China zur Verfügung, die mit der weltweit verfügbaren Instanz von Azure Active Directory arbeiten. Allerdings werden die Azure Active Directory-Editionen Premium und Basic derzeit durch den in China von 21Vianet betriebenen Microsoft Azure-Dienst nicht unterstützt. Wenn Sie weitere Informationen benötigen, können Sie über das Azure Active Directory-Forum Kontakt mit uns aufnehmen.


## <a name="customizing-the-sign-in-page"></a>Anpassen der Anmeldeseite
Ihre Benutzer interagieren normalerweise mit der Azure AD-Anmeldeseite, wenn sie versuchen, auf Cloudanwendungen und -dienste zuzugreifen, die von Ihrem Unternehmen abonniert wurden.

Wenn Sie Brandingänderungen auf Ihre Anmeldeseite angewendet haben, kann es bis zu einer Stunde dauern, bis die Änderungen für Endbenutzer sichtbar sind.

Unternehmensbranding-Elemente werden auf der Anmeldeseite von Azure AD angezeigt, wenn Benutzer auf eine mandantenspezifische URL zugreifen, z.B. „https://outlook.com/contoso.com“.

Wenn Benutzer auf einen Dienst unter einer generischen URL zugreifen, z.B. „www.office.com“, enthält die Anmeldeseite noch keine Unternehmensbranding-Informationen, weil der Benutzer dem System nicht bekannt ist. Das Unternehmensbranding wird aber angezeigt, nachdem Benutzer ihre Benutzer-ID eingegeben oder eine Benutzerkachel ausgewählt haben.

> [!NOTE]
> * Ihr Domänenname muss im Abschnitt **Active Directory** > **Verzeichnis** > **Domänen** im klassischen Azure-Portal, in dem Sie das Branding konfiguriert haben, als „Aktiv“ angezeigt werden.
> * Das Branding der Anmeldeseite wird nicht auf die Anmeldeseite für persönliche Microsoft-Konten übernommen. Wenn sich Ihre Mitarbeiter oder Gäste des Unternehmens mit einem persönlichen Microsoft-Konto anmelden, wird auf der Anmeldeseite das Branding Ihres Unternehmens nicht angezeigt.
>

In den folgenden Screenshots ist dargestellt, wie Anmeldeseiten angepasst werden.

### <a name="scenario-1-contoso-employee-goes-to-a-generic-app-url-for-example-wwwofficecom"></a>Szenario 1: Contoso-Mitarbeiter greift auf eine generische App-URL (z.B. „www.office.com“) zu

In diesem Beispiel meldet sich ein Contoso-Benutzer mit einer generischen URL an einer mobilen Anwendung oder einer Webanwendung an. In der Abbildung auf der linken Seite wird immer die App dargestellt, und der Interaktionsbereich auf der rechten Seite wird aktualisiert, um bei Bedarf Elemente der Marke Contoso anzuzeigen.

![Anmeldeseite von Office 365 vor und nach der Anpassung][1]

### <a name="scenario-2-contoso-employee-goes-to-contoso-app-thats-restricted-to-internal-users"></a>Szenario 2: Contoso-Mitarbeiter greift auf Contoso-App zu, die nur für interne Benutzer zugänglich ist

In diesem Beispiel meldet sich ein Contoso-Benutzer mit einer unternehmensspezifischen URL an einer internen Anwendung an. In der Abbildung auf der linken Seite wird das Unternehmensbranding (Contoso) angezeigt. Der Interaktionsbereich auf der rechten Seite ist nur für die Anmeldung von Contoso-Mitarbeitern zugänglich.

![Eingeschränkte App-Anmeldeseite][2]

### <a name="scenario-3-contoso-employee-goes-to-a-contoso-app-thats-open-to-external-users"></a>Szenario 3: Contoso-Mitarbeiter greift auf eine Contoso-App zu, die auch für externe Benutzer zugänglich ist

In diesem Beispiel melden sich Benutzer, die nicht zwingend Mitarbeiter von Contoso sein müssen, an einer Branchenanwendung von Contoso an. Die Abbildung auf der linken Seite zeigt – wie oben in Szenario \#2 – den Ressourcenbesitzer (Contoso). Aber der Interaktionsbereich auf der rechten Seite ist nicht nur auf Contoso beschränkt, um zu verdeutlichen, dass sich auch externe Benutzer anmelden können.

![Anmeldeseite für offenen Zugang][3]

### <a name="scenario-4-fabrikam-business-guest-goes-to-contoso-app-thats-open-to-external-users"></a>Szenario 4: Fabrikam-Gast greift auf eine Contoso-App zu, die für externe Benutzer zugänglich ist

In diesem Beispiel meldet sich ein Contoso-Benutzer mit einer unternehmensspezifischen URL an einer internen Anwendung an. In der Abbildung auf der linken Seite wird das Unternehmensbranding (Contoso) angezeigt. Der Interaktionsbereich auf der rechten Seite ist nur für die Anmeldung von Contoso-Mitarbeitern zugänglich.

![Anmeldung als externer Benutzer][4]


## <a name="what-elements-on-the-page-can-i-customize"></a>Welche Elemente auf der Seite können angepasst werden?

Sie können die folgenden Elemente auf der Anmeldeseite anpassen:

![][5]

| Seitenelement | Position auf der Seite |
|:--- | --- |
| Bannerlogo | Rechts oben auf der Seite angezeigt. Ersetzt das App-Logo, nachdem das Unternehmens des Benutzers ermittelt wurde (normalerweise nach dem Eingeben des Benutzernamens). |
| Abbildung im Hintergrund | Wird auf der linken Seite der Anmeldeseite als randloses Bild angezeigt. Ersetzt die Abbildung der App für Mandanten-Anmeldeszenarien (wenn Benutzer auf eine Anwendung zugreifen, die von ihrem eigenen Unternehmen oder einem Unternehmen veröffentlicht wird, dessen Gast sie sind).<br>Bei Verbindungen mit niedriger Bandbreite wird die Abbildung im Hintergrund durch eine Hintergrundfarbe ersetzt. Auf schmalen Bildschirmen, z.B. von Smartphones, wird die Abbildung nicht angezeigt.<br>Die Abbildung im Hintergrund wird entsprechend angepasst, wenn Benutzer die Größe des Browsers ändern. Ordnen Sie wichtige visuelle Elemente beim Entwerfen Ihrer Abbildung in der oberen linken Ecke an, damit sie nicht abgeschnitten werden. | 
| Kontrollkästchen „Angemeldet bleiben“ | Wird unter dem Feld **Kennwort** angezeigt. |
| Text der Anmeldeseite | Textbaustein, der oberhalb der Seitenfußzeile angezeigt wird. Er kann verwendet werden, um hilfreiche Informationen für Benutzer zu vermitteln, z.B. Ihre Helpdesk-Telefonnummer oder einen rechtlichen Hinweis. |

> [!NOTE]
> Alle Elemente sind optional. Wenn Sie beispielsweise ein Bannerlogo angeben, aber keine Abbildung für den Hintergrund, werden auf der Anmeldeseite Ihr Logo und die Abbildung für den Zielstandort (z.B. das Office 365-Bild mit dem kalifornischen Highway) angezeigt.
>

Dank des Kontrollkästchens **Angemeldet bleiben** auf der Anmeldeseite kann der Benutzer angemeldet bleiben, wenn er den Browser schließt und wieder öffnet. Dies hat keine Auswirkung auf die Sitzungslebensdauer.

Ob das Kontrollkästchen angezeigt wird, hängt von der Einstellung **„Angemeldet bleiben“ ausblenden** ab.

![„Angemeldet bleiben“ ausblenden][6]

Legen Sie diese Einstellung zum Ausblenden des Kontrollkästchens auf **Ausgeblendet** fest.

> [!NOTE]
> Einige Features von SharePoint Online und Office 2010 setzen voraus, dass dieses Kontrollkästchen von den Benutzern aktiviert werden kann. Wenn Sie das Kontrollkästchen mithilfe dieser Einstellung ausblenden, werden den Benutzern unter Umständen zusätzliche und unerwartete Anmeldeaufforderungen angezeigt.
>
>

Sie können auch alle Elemente auf dieser Seite lokalisieren. Sobald Sie einen „Standardsatz“ mit Anpassungselementen konfiguriert haben, können Sie zusätzliche Versionen für verschiedene Gebietsschemas konfigurieren. Sie können auch verschiedene Elemente miteinander kombinieren. Dazu zählen z. B.:

* Erstellen Sie eine Standardabbildung, die für alle Kulturen funktioniert, und erstellen Sie dann spezifische Versionen für Englisch und Französisch. Wenn Sie Ihre Browser auf eine dieser Sprachen festlegen, wird das jeweilige Bild angezeigt, während das Standardbild für alle anderen Sprachen angezeigt wird.
* Konfigurieren Sie verschiedene Logos für Ihre Organisation (z.B. japanische oder hebräische Versionen).

## <a name="access-panel-page-customization"></a>Anpassung der Zugriffsbereichsseite
Die Zugriffsbereichsseite ist im Wesentlichen eine Portalseite für den schnellen Zugriff auf die Cloud-Apps, für die Ihnen vom Administrator der Zugriff gewährt wurde. Auf dieser Seite werden Ihre Apps als Anwendungskacheln aufgeführt, auf die Sie klicken können.

Der folgende Screenshot zeigt ein Beispiel für eine Zugriffsbereichsseite nach der Anpassung:

![][8]

## <a name="configure-your-directory-with-company-branding"></a>Konfigurieren Sie Ihr Verzeichnis mit Unternehmensbranding
Sie können einen Standardsatz mit anpassbaren Elementen pro Verzeichnis im klassischen Azure-Portal konfigurieren. Nachdem die Standardeinstellungen gespeichert wurden, kann ein Administrator lokalisierte Versionen jedes Elements für verschiedene Sprachen bzw. Gebietsschemas hinzufügen. Alle anpassbaren Elemente sind optional.

Wenn Sie beispielsweise ein standardmäßiges Bannerlogo konfigurieren, aber keine große Abbildung, wird Ihr Logo auf der Anmeldeseite oben rechts angezeigt. Außerdem wird die Standardabbildung der Website angezeigt.

Stellen Sie sich die folgende Konfiguration vor:

* Standardbannerlogo und Anmeldeseitentext in Englisch
* Sprachspezifischer Anmeldeseitentext für Deutsch

Wenn Ihre bevorzugte Sprache Deutsch ist, erhalten Sie das Standardbannerlogo und den deutschen Text.

Während Sie technisch gesehen einen anderen Satz für jede von Azure AD unterstützte Sprache konfigurieren könnten, empfehlen wir, dass die Anzahl der Varianten aus Wartungs- und Leistungsgründen so klein wie möglich bleibt.
 
**Führen Sie die folgenden Schritte aus, um Ihrem Verzeichnis Unternehmensbranding hinzuzufügen:**

1. Melden Sie sich am [klassischen Azure-Portal](https://manage.windowsazure.com) als Administrator des Verzeichnisses an, das Sie anpassen möchten.
2. Wählen Sie das Verzeichnis, das Sie anpassen möchten.
3. Klicken Sie in der Symbolleiste oben auf **Konfigurieren**.
4. Klicken Sie auf **Branding anpassen**.
5. Ändern Sie die Elemente, die Sie anpassen möchten. Alle Felder sind optional.
6. Klicken Sie auf **Speichern**.

Es kann bis zu einer Stunde dauern, bis Neuerungen angezeigt werden, die Sie am Branding der Anmeldeseite vorgenommen haben.

**Führen Sie die folgenden Schritte aus, um sprachspezifisches Unternehmensbranding hinzuzufügen:**

1. Melden Sie sich am [klassischen Azure-Portal](https://manage.windowsazure.com) als Administrator des Verzeichnisses an, das Sie anpassen möchten.
2. Wählen Sie das Verzeichnis, das Sie anpassen möchten.
fs3. Klicken Sie in der Symbolleiste oben auf **Konfigurieren**.
4. Klicken Sie auf **Branding anpassen**.
5. Klicken Sie auf **Brandingeinstellungen für eine bestimmte Sprache hinzufügen**.
6. Wählen Sie die Sprache, für die Sie das Logo anpassen möchten, und klicken Sie dann auf **Weiter**.
7. Bearbeiten Sie nur die Elemente, für die Sie sprachspezifische Überschreibungen konfigurieren möchten. Alle Felder sind optional. Wenn ein Feld leer bleibt, wird stattdessen der benutzerdefinierte Standardwert angezeigt (oder die Microsoft-Standardeinstellung, wenn kein benutzerdefinierter Standardwert konfiguriert ist).
8. Klicken Sie auf **Speichern**.

**Führen Sie die folgenden Schritte aus, um das Unternehmensbranding für Ihr Verzeichnis zu entfernen:**

1. Melden Sie sich am [klassischen Azure-Portal](https://manage.windowsazure.com) als Administrator des Verzeichnisses an, das Sie anpassen möchten.
2. Wählen Sie das Verzeichnis, das Sie anpassen möchten.
3. Klicken Sie in der Symbolleiste oben auf **Konfigurieren**.
4. Klicken Sie auf **Branding anpassen**.
5. Wählen Sie auf der Seite Branding anpassen **Vorhandene Brandingeinstellungen bearbeiten** und fahren Sie anschließend mit der nächsten Seite fort.
6. Abhängig von den Elementen, die Sie entfernen möchten, führen Sie eine oder mehrere der folgenden Aktionen aus:

    a. Wählen Sie unter **Bannerlogo** die Option **Hochgeladenes Logo entfernen**.

    b. Wählen Sie unter **Kachellogo** die Option **Hochgeladenes Logo entfernen**.

    c. Entfernen Sie den Text aus allen Textfeldern.

    d. Klicken Sie auf **Next**.

    e. Entfernen Sie den Text aus allen Textfeldern.
7. Klicken Sie auf **Speichern** , um die Elemente zu entfernen.
8. Klicken Sie ggf. erneut auf **Branding anpassen**, und wiederholen Sie diese Schritte für das gesamte sprachspezifische Branding, das entfernt werden soll.
    Alle Brandingeinstellungen wurden entfernt, wenn Sie auf **Branding anpassen** klicken und das Formular **Standardmäßiges Branding anpassen** ohne konfigurierte Einstellungen angezeigt wird.


## <a name="customizable-elements"></a>Anpassbare Elemente
Unternehmenslogos werden für die Anmelde- und Zugriffsbereichsseite verwendet, während andere Elemente nur auf der Anmeldeseite angezeigt werden. Die folgende Tabelle enthält die Details für die verschiedenen anpassbaren Elemente.

| Name | Beschreibung | Einschränkungen | Recommendations |
| --- | --- | --- | --- |
| Bannerlogo |Das Bannerlogo wird auf der Anmeldeseite und im Zugriffsbereich angezeigt. |<p>JPG oder PNG</p><p>60 x 280 Pixel</p><p>10 KB</p> |<p>Verwenden Sie das vollständige Logo Ihrer Organisation (einschließlich Piktogramm und Firmenschriftzug)</p><p>Halten Sie es unter 30 Pixel, um Bildlaufleisten auf mobilen Geräten zu vermeiden</p><p>Halten Sie es kleiner als 4 KB</p><p>Verwenden Sie eine transparente PNG-Datei (gehen Sie nicht davon aus, dass die Anmeldeseite immer einen weißen Hintergrund hat)</p> |
| Kachellogo | Wird derzeit nicht verwendet |<p>JPG oder PNG</p><p>120 x 120 Pixel</p><p>10 KB</p> |<p>Halten Sie es einfach (kein kleiner Text), da dieses Abbild um 50 % verkleinert werden kann |
| </p> | | | |
| Bezeichnung für Anmeldebenutzername | Wird derzeit nicht verwendet |<p>Unicode-Text, bis zu 50 Zeichen</p><p>Ausschließlich Nur-Text (keine Links oder HTML-Tags)</p> |<p>Halten Sie es kurz und einfach.</p><p>Fragen Sie die Benutzer, wie Sie sich in der Regel auf das Geschäfts- oder Schulkonto beziehen, das Sie zur Verfügung stellen.</p> |
| Textbaustein für Anmeldeseite |Dieser Textbaustein wird unter dem Anmeldeseitenformular angezeigt und kann verwendet werden, um zusätzliche Anweisungen zu kommunizieren oder mitzuteilen, wo es Hilfe und Support gibt. |<p>Unicode-Text, bis zu 256 Zeichen</p><p>Ausschließlich Nur-Text (keine Links oder HTML-Tags)</p> |Verwenden Sie maximal 250 Zeichen (ungefähr drei Zeilen Text) |
| Abbildung für den Hintergrund der Anmeldeseite | Großes Bild, das im linken Bereich der Anmeldeseite angezeigt wird (für RTL-Sprachen im rechten Bereich), wenn Benutzer auf mandantenspezifische URLs zugreifen. |<p>JPG oder PNG</p><p>1420 x 1200</p><p>500 KB</p> |<p>1420 x 1200 Pixel</p><p>Wichtig: Halten Sie es so klein wie möglich, idealerweise unter 200 KB. Falls dieses Bild zu groß ist, beeinflusst dies die Leistung der Anmeldeseite, wenn das Bild nicht zwischengespeichert wird</p><p>Die Größe dieses Bilds wird fast immer angepasst, um unterschiedliche Seitenverhältnisse von Bildschirmen abzudecken. Ordnen Sie die wichtigen visuellen Elemente in der oberen linken Ecke an.</p> |
| Hintergrundfarbe auf der Anmeldeseite | Bei Verbindungen mit geringer Bandbreite wird anstelle der Abbildung für den Hintergrund diese Volltonfarbe verwendet. | Muss eine RGB-Farbe im hexadezimalen Format sein (Beispiel: \#FFFFFF) | Wir empfehlen, die Grundfarbe des Bannerlogos auszuwählen. |

## <a name="next-steps"></a>Nächste Schritte
* [Erste Schritte mit Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Anzeigen Ihrer Zugriffs- und Nutzungsberichte](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/signin-page_before-customization.png
[2]: ./media/active-directory-add-company-branding/signin-page-restricted-app.png
[3]: ./media/active-directory-add-company-branding/signin-page-open-access.png
[4]: ./media/active-directory-add-company-branding/signin-page-external-guest.png
[5]: ./media/active-directory-add-company-branding/which-elements-can-i-customize.png
[6]: ./media/active-directory-add-company-branding/hide-kmsi.png
[8]: ./media/active-directory-add-company-branding/APBranding.png
[9]: ./media/active-directory-add-company-branding/hidekmsi.png

