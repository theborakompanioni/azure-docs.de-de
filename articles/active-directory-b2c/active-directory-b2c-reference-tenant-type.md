---
title: 'Azure Active Directory B2C: Vergleich zwischen B2C-Produktionsmandanten und -Vorschaumandanten | Microsoft Docs'
description: "Das Thema enthält eine Beschreibung der Typen von Azure Active Directory B2C-Mandanten."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 8a0644da-b825-4edc-8ce9-541c3c976afb
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
translationtype: Human Translation
ms.sourcegitcommit: 4b13c040a15bef2f04d2cd2126e2270d061898bd
ms.openlocfilehash: a37992cd2bfe346fd171bde15b6180c56527289b


---
# <a name="azure-active-directory-b2c-production-scale-vs-preview-b2c-tenants"></a>Azure Active Directory B2C: Vergleich zwischen B2C-Produktionsmandanten und -Vorschaumandanten
Wenn Sie das Schreiben einer Produktions-App in Azure Active Directory (Azure AD) B2C planen, müssen Sie sicher sein, dass Sie über den richtigen Mandantentyp für die Liveschaltung verfügen. Führen Sie im Azure-Portal die Schritte unter [Navigieren zum Blatt „B2C-Funktionen“](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) aus, und sehen Sie unter **Mandantentyp**nach, um dies zu ermitteln.

## <a name="summary"></a>Zusammenfassung
Azure AD B2C unterstützt Produktions-Apps NUR für B2C-Mandanten für die **Produktion** in Nordamerika.

| Mandantentyp | Länder/Regionen | Allgemein verfügbar? |
| --- | --- | --- |
| **Produktionsmandant** |Länder/Regionen in Nordamerika |Ja |
| **Produktionsmandant** |Alle Länder/Regionen mit Ausnahme von Nordamerika |Nein |
| **Vorschaumandant** |Alle Länder/Regionen |Nein |

> [!NOTE]
> Azure AD B2C-Mandanten (für Endkunden) sind derzeit in einigen Ländern oder Regionen nicht verfügbar, in denen Azure AD-Mandanten (für Mitarbeiter) verfügbar sind. Die folgenden Abschnitte enthalten hierzu weitere Informationen.
> 
> 

## <a name="production-scale-b2c-tenant-in-north-america"></a>B2C-Produktionsmandant in Nordamerika
Wenn Sie die [Erstellung Ihres B2C-Mandanten](active-directory-b2c-get-started.md) in Nordamerika durchgeführt haben (also in einem der folgenden Länder/Regionen: USA, Kanada, Costa Rica, Dominikanische Republik, El Salvador, Guatemala, Mexiko, Panama, Puerto Rico und Trinidad und Tobago) UND für den **Mandantentyp** in der B2C-Verwaltungsoberfläche **Produktion** festgelegt ist, können Sie den Mandanten für Produktions-Apps verwenden.

> [!NOTE]
> Mit Produktionsmandanten können mehrere Hundertmillionen Endkundenidentitäten pro Mandant skaliert werden.
> 
> 

![Screenshot eines Produktionsmandanten](./media/active-directory-b2c-reference-tenant-type/production-scale-b2c-tenant.png)

## <a name="preview-b2c-tenant-in-any-countryregion"></a>Vorschauversion des B2C-Mandanten in allen Ländern/Regionen
Wenn Sie während des Vorschauzeitraums von Azure AD B2C einen B2C-Mandanten erstellt haben, ist die Wahrscheinlichkeit hoch, dass der **Mandantentyp** auf **Vorschaumandant** festgelegt ist. In diesem Fall können Sie Ihren Mandanten NUR für Entwicklungs- und Testzwecke verwenden, und NICHT für Produktions-Apps.

> [!IMPORTANT]
> Es ist kein Migrationspfad von einem B2C-Vorschaumandanten zu einem B2C-Produktionsmandanten vorhanden. Beachten Sie, dass beim Löschen eines B2C-Vorschaumandanten und erneuten Erstellen eines B2C-Produktionsmandanten mit demselben Domänennamen bekannte Probleme auftreten. Sie müssen einen B2C-Produktionsmandanten mit einem anderen Domänennamen erstellen.
> 
> 

![Screenshot eines Vorschaumandanten](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)

## <a name="production-scale-b2c-tenant-outside-of-north-america"></a>B2C-Produktionsmandant außerhalb von Nordamerika
Azure AD B2C ist derzeit außerhalb von Nordamerika NICHT allgemein verfügbar. Sie haben aber die Möglichkeit, Produktionsmandanten zu Entwicklungs- und Testzwecken in einem der folgenden Länder oder Regionen zu erstellen und zu verwenden: Algerien, Ägypten, Aserbaidschan, Bahrain, Belarus, Belgien, Bulgarien, Dänemark, Deutschland, Estland, Finnland, Frankreich, Griechenland, Irland, Island, Israel, Italien, Jordanien, Kasachstan, Katar, Kenia, Kroatien, Kuwait, Lettland, Libanon, Liechtenstein, Litauen, Luxemburg, Malta, Marokko, Mazedonien (ehem. jugoslawische Republik), Montenegro, Niederlande, Nigeria, Norwegen, Oman, Österreich, Pakistan, Polen, Portugal, Rumänien, Russland, Saudi-Arabien, Schweden, Schweiz, Serbien, Slowakei, Slowenien, Spanien, Südafrika, Tschechische Republik, Tunesien, Türkei, Ukraine, Ungarn, Vereinigte Arabische Emirate, Vereinigtes Königreich und Zypern.

Sobald Azure AD B2C die allgemeine Verfügbarkeit in den oben genannten Ländern oder Regionen ankündigt, können Sie diese Produktionsmandanten weiterhin verwenden und Ihre Produktions-Apps ohne Datenverlust freischalten.

## <a name="availability-of-b2c-tenants"></a>Verfügbarkeit von B2C-Mandanten
In den folgenden Ländern oder Regionen sind B2C-Mandanten derzeit nicht verfügbar: Afghanistan, Argentinien, Australien, Brasilien, Chile, Ecuador, Hongkong, Indien, Indonesien, Irak, Japan, Kolumbien, Malaysia, Neuseeland, Paraguay, Peru, Philippinen, Singapur, Sri Lanka, Südkorea, Taiwan, Thailand, Uruguay und Venezuela. Diese Länder sollen in Zukunft eingebunden werden.




<!--HONumber=Dec16_HO4-->


