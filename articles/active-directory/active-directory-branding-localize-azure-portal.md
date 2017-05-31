---
title: "Hinzufügen eines sprachspezifischen Unternehmensbrandings zur Anmeldeseite in Azure Active Directory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie sprachspezifische Bilder und Texte zu einer Azure-Anmeldeseite hinzufügen."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a0310d6a-aaa7-4ea0-991d-6d3135b4382a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: e1fe8d855386ceec39edbc985538cdf32d78a13b
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017


---
# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory"></a>Hinzufügen eines sprachspezifischen Unternehmensbrandings zur Anmeldeseite in Azure Active Directory
Um Verwirrung zu vermeiden, möchten viele Unternehmen über alle verwalteten Websites und Dienste hinweg für ein einheitliches Erscheinungsbild sorgen. Dies ist in Azure Active Directory möglich, da Sie die Darstellung der Anmeldeseite mit Ihrem Firmenlogo und Ihren benutzerdefinierten Farbschemas anpassen können. Die Anmeldeseite wird angezeigt, wenn Sie sich an Office 365 oder anderen webbasierten Anwendungen anmelden, die Azure AD als Identitätsanbieter verwenden. Sie interagieren mit dieser Seite, um Ihre Anmeldeinformationen einzugeben.

## <a name="customizing-the-sign-in-page-for-another-language"></a>Anpassen der Anmeldeseite für eine andere Sprache
Sie können Ihrer benutzerdefinierten Anmeldeseite sprachspezifische Elemente hinzufügen, wenn Sie bereits eine benutzerdefinierte Anmeldeseite erstellt haben. Weitere Informationen hierzu finden Sie unter [Hinzufügen eines Unternehmensbrandings zur Anmeldeseite](active-directory-branding-custom-signon-azure-portal.md). Sie können mithilfe eines Standardsatzes anpassbarer Elementen eine Anmeldeseite pro Verzeichnis konfigurieren. Sobald Sie den Standardsatz an Seitenelementen konfiguriert haben, können Sie zusätzliche Versionen für verschiedene Gebietsschemas konfigurieren. Sie können auch verschiedene Elemente miteinander kombinieren. Beispielsweise ist Folgendes möglich:

* Erstellen Sie eine **Standardabbildung für die Anmeldeseite** , die für alle Kulturen verwendet werden kann, und erstellen Sie dann spezifische Versionen für Englisch und Französisch. Wenn Sie Ihre Browser auf eine dieser zwei Sprachen festlegen, wird das sprachspezifische Bild angezeigt, während für alle anderen Sprachen das Standardbild angezeigt wird.
* Konfigurieren Sie verschiedene Logos für Ihre Organisation (z.B. japanische oder hebräische Versionen).

Es wird empfohlen, die Anzahl der Sprachvariationen aus Wartungs- und Leistungsgründen gering zu halten.

**So fügen Sie Ihrem Verzeichnis ein Unternehmensbranding hinzu**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die EINGABETASTE****.

   ![Öffnen der Benutzerverwaltung](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Unternehmensbranding** aus.
4. Wählen Sie auf dem Blatt **Benutzer und Gruppen – Unternehmensbranding** den Befehl **Sprache hinzufügen** aus.

    ![Hinzufügen sprachspezifischer Brandingelemente](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. Ändern Sie die Elemente, die Sie anpassen möchten. Alle Elemente sind optional.
6. Klicken Sie auf **Speichern**.

Es kann bis zu einer Stunde dauern, bis Änderungen übernommen werden, die Sie am Branding der Anmeldeseite vorgenommen haben.

## <a name="next-steps"></a>Nächste Schritte
[Hinzufügen eines Unternehmensbrandings zu Ihrer Anmeldeseite](active-directory-branding-custom-signon-azure-portal.md)

