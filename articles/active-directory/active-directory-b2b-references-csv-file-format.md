---
title: CSV-Dateiformat bei der Vorschau der Azure Active Directory B2B-Zusammenarbeit | Microsoft Docs
description: "Azure Active Directory B2B ermöglicht Geschäftspartnern den gezielten Zugriff auf Ihre Unternehmensanwendungen und unterstützt so Ihre unternehmensübergreifenden Beziehungen."
services: active-directory
documentationcenter: 
author: viv-liu
manager: cliffdi
editor: 
tags: 
ms.assetid: bd2c5364-4164-407d-ac25-34088c175c4a
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/09/2016
ms.author: viviali
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: f972fa0f8322ee33e1d198738f35d800564b4ddf


---
# <a name="azure-ad-b2b-collaboration-preview-csv-file-format"></a>Vorschau der Azure AD B2B-Zusammenarbeit: CSV-Dateiformat
Die Vorschauversion der Azure AD B2B-Zusammenarbeit erfordert eine CSV-Datei mit Angaben zum Partnerbenutzer, die über das Azure AD-Portal hochgeladen werden müssen. Die CSV-Datei muss die nachfolgenden erforderlichen Bezeichnungen enthalten und kann bei Bedarf optionale Felder umfassen. Ändern Sie die CSV-Beispieldatei (siehe unten), ohne die Schreibweise der Bezeichnungen in der ersten Zeile zu ändern.

> [!NOTE]
> Die erste Zeile mit Bezeichnungen (z.B. „E-Mail“, „DisplayName“ usw.) ist erforderlich, damit die CSV-Datei erfolgreich analysiert werden kann. Die Schreibweise muss mit den unten angegebenen Feldern übereinstimmen. Diese Bezeichnungen geben die Inhalte darunter an. Die Bezeichnungen der nicht benötigten optionalen Felder können aus der CSV-Datei entfernt werden. Die gesamte Spalte kann leer gelassen werden.
> 
> 

## <a name="required-fields-br"></a>Erforderliche Felder:  <br/>
**Email**: E-Mail-Adresse des eingeladenen Benutzers. <br/>
**DisplayName**: Anzeigename für den eingeladenen Benutzer (i.d.R. Vor- und Nachname).<br/>

## <a name="optional-fields-br"></a>Optionale Felder:  <br/>
**InvitationText:** Anpassen des Texts der Einladungs-E-Mail nach dem App-Branding und vor dem Einlöselink.<br/>
**InvitedToApplications:**: AppIDs für Unternehmensanwendungen, um Benutzer zuzuweisen. AppIDs können aus PowerShell abgerufen werden. Der entsprechende Befehl lautet: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InvitedToGroups:** ObjectIDs für Gruppen, um Benutzer hinzuzufügen. ObjectIDs können aus PowerShell abgerufen werden. Der entsprechende Befehl lautet: `Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteRedirectUR:** Die URL, an die ein eingeladener Benutzer nach der Annahme der Einladung weitergeleitet wird. Dies sollte eine unternehmensspezifische URL sein (z.B. [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/)). Wenn dieses optionale Feld nicht angegeben ist, wird der eingeladene Benutzer zum App-Zugriffsbereich weitergeleitet, in dem er zu Ihren ausgewählten Unternehmens-Apps navigieren kann. Die URL für den App-Zugriffsbereich hat das Format `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`.<br/>
**CcEmailAddress**: E-Mail-Adresse, um die per E-Mail zugesendete Einladung zu kopieren. Wenn das CcEmailAddress-Feld verwendet wird, kann diese Einladung nicht für E-Mail-verifizierte Benutzer oder zum Erstellen eines viralen Mandanten verwendet werden.<br/>
**Language**: Die Sprache, die für die Einladungs-E-Mail und den Einlösevorgang verwendet wird. Wenn keine Angabe erfolgt, wird standardmäßig Englisch verwendet. Die anderen 10 unterstützten Sprachcodes sind:<br/>

1. de: Deutsch<br/>
2. es: Spanisch<br/>
3. fr: Französisch<br/>
4. it: Italienisch<br/>
5. ja: Japanisch<br/>
6. ko: Koreanisch<br/>
7. pt-BR: Portugiesisch (Brasilien)<br/>
8. ru: Russisch<br/>
9. zh-HANS: Chinesisch (vereinfacht)<br/>
10. zh-HANT: Chinesisch (traditionell)<br/>

## <a name="sample-csv-file"></a>Beispiel-CSV-Datei
Dies ist ein Beispiel für eine CSV-Datei, das Sie anpassen können.

> [!NOTE]
> Kopieren Sie diese Beispieldatei, fügen Sie sie in Editor ein, und speichern Sie sie mit der Dateierweiterung „.csv“. Bearbeiten Sie sie dann in Excel. Sie wird in eine Tabelle mit Bezeichnungen in der ersten Zeile gegliedert.
> 
> Fügen Sie in Excel weitere optionale Felder hinzu, indem Sie die Bezeichnung angeben und die Spalte darunter füllen.
> 
> 

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## <a name="related-articles"></a>Verwandte Artikel
Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [So funktioniert's](active-directory-b2b-how-it-works.md)
* [Ausführliche exemplarische Vorgehensweise](active-directory-b2b-detailed-walkthrough.md)
* [Tokenformat für externe Benutzer](active-directory-b2b-references-external-user-token-format.md)
* [Objektattributänderungen für externe Benutzer](active-directory-b2b-references-external-user-object-attribute-changes.md)
* [Aktuelle Einschränkungen der Vorschau](active-directory-b2b-current-preview-limitations.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)




<!--HONumber=Jan17_HO1-->


