---
title: Anpassen Ihrer Anmeldeseite in Azure Active Directory | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie der Anmeldeseite ein Unternehmensbranding hinzufügen."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeffgilb
custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: bddf2542eecce8bdeccda6053203bf2c2ba0ffb2
ms.contentlocale: de-de
ms.lasthandoff: 08/15/2017

---
# <a name="quickstart-add-company-branding-to-your-sign-in-page-in-azure-ad"></a>Schnellstart: Hinzufügen eines Unternehmensbrandings zur Anmeldeseite in Azure AD
Um Verwirrung zu vermeiden, möchten viele Unternehmen über alle verwalteten Websites und Dienste hinweg für ein einheitliches Erscheinungsbild sorgen. Dies ist in Azure Active Directory (Azure AD) möglich, da Sie die Darstellung der Anmeldeseite mit Ihrem Firmenlogo und Ihren benutzerdefinierten Farbschemas anpassen können. Die Anmeldeseite wird angezeigt, wenn Sie sich an Office 365 oder anderen webbasierten Anwendungen anmelden, die Azure AD als Identitätsanbieter verwenden. Sie interagieren mit dieser Seite, um Ihre Anmeldeinformationen einzugeben.

> [!NOTE]
> * Die Funktion für das Unternehmensbranding ist nur verfügbar, wenn Sie Ihre Azure AD-Instanz auf die Premium oder Basic Edition aktualisiert haben oder im Besitz einer Office 365-Lizenz sind. Informationen dazu, ob bestimmte Features für Ihren Lizenztype unterstützt werden, finden Sie auf der Seite [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * Die Azure Active Directory Premium und Basic Editions stehen für Kunden in China zur Verfügung, die mit der weltweit verfügbaren Instanz von Azure Active Directory arbeiten. Allerdings werden die Azure Active Directory-Editionen Premium und Basic derzeit durch den in China von 21Vianet betriebenen Microsoft Azure-Dienst nicht unterstützt. Wenn Sie weitere Informationen benötigen, kontaktieren Sie uns im [Azure Active Directory-Forum](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="customizing-the-sign-in-page"></a>Anpassen der Anmeldeseite

<!--You can customize the following elements on the sign-in page: <attach image>-->

Anpassungen des Unternehmensbrandings werden auf der Anmeldeseite von Azure AD angezeigt, wenn Benutzer auf eine mandantenspezifische URL zugreifen, z.B. [*https://outlook.com/contoso.com*](https://outlook.com/contoso.com).

Wenn Benutzer auf eine generische URL zugreifen, z.B. „www.office.com“, enthält die Anmeldeseite noch keine Anpassungen des Unternehmensbrandings, weil der Benutzer dem System nicht bekannt ist. Das Unternehmensbranding wird angezeigt, nachdem Benutzer ihre Benutzer-ID eingegeben oder eine Benutzerkachel ausgewählt haben.

> [!NOTE]
> * Ihr Domänenname muss im Abschnitt **Domänen** des Azure-Portals, in dem Sie das Branding konfiguriert haben, als „Aktiv“ angezeigt werden. Weitere Informationen finden Sie unter [Hinzufügen eines benutzerdefinierten Domänennamens](add-custom-domain.md).
> * Das Branding der Anmeldeseite wird nicht auf die Anmeldeseite für persönliche Microsoft-Konten übernommen. Wenn sich Ihre Mitarbeiter oder Gäste des Unternehmens mit einem persönlichen Microsoft-Konto anmelden, wird auf der Anmeldeseite das Branding Ihres Unternehmens nicht angezeigt.


### <a name="banner-logo"></a>Bannerlogo 

Beschreibung | Einschränkungen | Empfehlungen
------- | ------- | ----------
Das Bannerlogo wird auf der Anmelde- und den Zugriffsbereichsseiten angezeigt.<br>Auf der Anmeldeseite wird es angezeigt, nachdem das Unternehmen des Benutzers ermittelt wurde – normalerweise nach dem Eingeben des Benutzernamens.  | Transparente JPG- oder PNG-Datei<br>Max. Höhe: 36 px<br>Max. Breite: 245 px | Verwenden Sie hier das Logo Ihres Unternehmens.<br>Verwenden Sie ein transparentes Bild. Setzen Sie nicht voraus, dass der Hintergrund weiß ist.<br>Fügen Sie keinen Abstand um Ihr Logo herum in der Abbildung ein, da Ihr Logo anderenfalls unverhältnismäßig klein aussehen wird.

### <a name="username-hint"></a>Hinweis auf den Benutzernamen   
Beschreibung | Einschränkungen | Empfehlungen
------- | ------- | ----------
Dies passt den Hinweistext im Benutzernamensfeld an. | Unicode-Text, bis zu 64 Zeichen<br>Nur-Text | Sie sollten dies nicht festlegen, wenn sich voraussichtlich Gastbenutzer außerhalb Ihrer Organisation bei Ihrer App anmelden.
            
### <a name="sign-in-page-text"></a>Text der Anmeldeseite   
Beschreibung | Einschränkungen | Empfehlungen
------- | ------- | ----------
Dies wird am unteren Rand des Anmeldeformulars angezeigt und kann verwendet werden, um zusätzliche Informationen wie z.B. die Telefonnummer Ihres Helpdesks oder einen rechtlichen Hinweis zu kommunizieren. | Unicode-Text, bis zu 256 Zeichen<br>Ausschließlich Nur-Text (keine Links oder HTML-Tags)   

### <a name="sign-in-page-image"></a>Bild der Anmeldeseite  
Beschreibung | Einschränkungen | Empfehlungen
------- | ------- | ----------
Dies wird im Hintergrund der Anmeldeseite angezeigt, ist in der Mitte des sichtbaren Bereichs verankert und wird so skaliert und zugeschnitten, dass das Browserfenster gefüllt ist.    <br>Auf schmalen Bildschirmen, z.B. von Mobiltelefonen, wird dieses Bild nicht angezeigt.<br>Eine schwarze Maske mit einer Deckkraft von 0,55 wendet unser Code beim Laden der Seite auf dieses Bild an. | JPG oder PNG<br>Bildmaße: 1.920 x 1.080 px<br>Dateigröße: &gt; 300 KB | <br>Verwenden Sie Bilder, in denen die Bildschärfe sich nicht auf einen Bereich konzentriert. Das nicht transparente Anmeldeformular wird über dem Mittelpunkt dieses Bilds angezeigt und kann abhängig von der Größe des Browserfensters einen beliebigen Teil des Bilds abdecken.<br>Halten Sie die Dateigröße so klein wie möglich, um schnelle Ladezeiten sicherzustellen. 

### <a name="background-color"></a>Hintergrundfarbe
Beschreibung | Einschränkungen | Empfehlungen
------- | ------- | ----------
Bei Verbindungen mit geringer Bandbreite wird diese Farbe anstelle des Hintergrundbildes verwendet. | RGB-Farbe im Hexadezimalformat (Beispiel: #FFFFFF) | Sie sollten die primäre Farbe des Bannerlogos oder die Farbe Ihrer Organisation verwenden.

### <a name="show-option-to-remain-signed-in"></a>Anzeigen der Option, angemeldet zu bleiben
Beschreibung | Einschränkungen | Empfehlungen
------- | ------- | ----------
Die Azure AD-Anmeldung gewährt Benutzern die Möglichkeit, angemeldet zu bleiben, wenn sie den Browser schließen und erneut öffnen. Blenden Sie diese Option hiermit aus.<br>Legen Sie diese Einstellung auf „Nein“ fest, um diese Option für Ihre Benutzer auszublenden. | &nbsp; | Dies beeinflusst die Sitzungsdauer nicht.<br>Einige Features von SharePoint Online und Office 2010 setzen voraus, dass Benutzer wählen können, angemeldet zu bleiben. Wenn Sie dies ausblenden, werden den Benutzern unter Umständen zusätzliche und unerwartete Anmeldeaufforderungen angezeigt.

> [!NOTE]
> Alle Elemente sind optional. Wenn Sie beispielsweise ein Bannerlogo ohne Hintergrundbild angeben, werden auf der Anmeldeseite Ihr Logo und das Hintergrundbild für die Zielwebsite (z.B. Office 365) angezeigt.

## <a name="add-company-branding-to-your-directory"></a>Hinzufügen eines Unternehmensbrandings zu Ihrem Verzeichnis

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die **EINGABETASTE** .

   ![Öffnen der Benutzerverwaltung](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Unternehmensbranding** aus.
4. Wählen Sie auf dem Blatt **Benutzer und Gruppen – Unternehmensbranding** den Befehl **Bearbeiten** aus.

    ![Bearbeiten des benutzerdefinierten Brandings](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. Ändern Sie die Elemente, die Sie anpassen möchten. Alle Elemente sind optional.
6. Klicken Sie auf **Speichern**.

Es kann bis zu einer Stunde dauern, bis Änderungen übernommen werden, die Sie am Branding der Anmeldeseite vorgenommen haben.

## <a name="add-language-specific-company-branding-to-your-directory"></a>Hinzufügen eines sprachspezifischen Unternehmensbrandings zu Ihrem Verzeichnis

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) mit dem Konto eines globalen Administrators für das Verzeichnis an.
2. Wählen Sie im Textfeld **Benutzer und Gruppen** aus, und drücken Sie die **EINGABETASTE**.

   ![Öffnen der Benutzerverwaltung](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Unternehmensbranding** aus.
4. Wählen Sie auf dem Blatt **Benutzer und Gruppen – Unternehmensbranding** den Befehl **Sprache hinzufügen** aus.

    ![Hinzufügen sprachspezifischer Brandingelemente](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. Ändern Sie die Elemente, die Sie anpassen möchten. Alle Elemente sind optional.
6. Klicken Sie auf **Speichern**.

Es kann bis zu einer Stunde dauern, bis Änderungen übernommen werden, die Sie am Branding der Anmeldeseite vorgenommen haben.

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie erfahren, wie ein Unternehmensbranding zu Ihrem Azure AD-Verzeichnis hinzugefügt wird. 

Über den folgenden Link können Sie mit dem Azure-Portal Ihr Unternehmensbranding in Azure AD konfigurieren.

> [!div class="nextstepaction"]
> [Konfigurieren des Unternehmensbrandings](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 
