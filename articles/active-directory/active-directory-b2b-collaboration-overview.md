---
title: Azure Active Directory B2B-Zusammenarbeit | Microsoft Docs
description: "Die Azure Active Directory B2B-Zusammenarbeit ermöglicht es Geschäftspartnern auf Ihre Unternehmensanwendungen zuzugreifen, wobei jeder ihrer Benutzer durch ein einzelnes Azure AD-Konto dargestellt wird."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 9f6a044c-0b36-4a1d-8080-33e28696c42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 7c56dee52201c95c2a0677355e2b4edd2f113259


---
# <a name="azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B-Zusammenarbeit
Mit der Azure Active Directory (Azure AD) B2B-Zusammenarbeit können Sie den Zugriff auf Unternehmensanwendungen mit von Partnern verwalteten Identitäten aktivieren. Sie können unternehmensübergreifende Beziehungen erstellen, indem Sie Benutzer von Partnerunternehmen zum Zugriff auf Ihre Ressourcen einladen und autorisieren. Die Komplexität wird verringert, weil jedes Unternehmen einmal als Verbund in Azure Active Directory eingebunden wird. Jeder Benutzer wird dann durch ein einzelnes Azure AD-Konto repräsentiert. Die Sicherheit wird erhöht, wenn Ihre Geschäftspartner ihre Konten in Azure AD verwalten, da der Zugriff aufgehoben wird, wenn Partnerbenutzer ihre Unternehmen verlassen, und der unbeabsichtigte Zugriff über die Mitgliedschaft in internen Verzeichnissen wird verhindert. Für Geschäftspartner, die Azure AD noch nicht besitzen, hat die B2B-Zusammenarbeit einen optimierten Registrierungsprozess, um Ihren Geschäftspartnern Azure AD-Konten bereitzustellen.

* Ihre Geschäftspartner verwenden ihre eigenen Anmeldeinformationen. Das erspart Ihnen das Verwalten eines externen Partnerverzeichnisses und das Entfernen des Zugriffs, wenn Benutzer das Partnerunternehmen verlassen.
* Sie verwalten den Zugriff auf Ihre Apps unabhängig vom Kontolebenszyklus Ihrer Geschäftspartner. Dies bedeutet beispielsweise, dass Sie den Zugriff widerrufen können, ohne die IT-Abteilung Ihres Geschäftspartners um Hilfe zu bitten.

## <a name="capabilities"></a>Funktionen
Die B2B-Zusammenarbeit vereinfacht die Verwaltung und verbessert die Sicherheit des Partnerzugriffs auf Unternehmensressourcen, einschließlich SaaS-Apps wie Office 365, Salesforce, Azure-Dienste und jede mobile, Cloud- und lokale Anwendung, die Ansprüche unterstützt. Die B2B-Zusammenarbeit ermöglicht Partnern das Verwalten ihrer eigenen Konten, und Unternehmen können Sicherheitsrichtlinien auf den Partnerzugriff anwenden.

Die Azure Active Directory B2B-Zusammenarbeit ist leicht zu konfigurieren und vereinfacht über einen mit E-Mails überprüften Prozess die Registrierung für Partner aller Größen, selbst wenn sie nicht über ein eigenes Azure Active Directory verfügen. Die Verwaltung ist auch einfach, da keine externen Verzeichnisse oder Verbundkonfigurationen pro Partner erforderlich sind.

## <a name="b2b-collaboration-process"></a>Prozess der B2B-Zusammenarbeit
1. Mit der Azure AD B2B-Zusammenarbeit kann ein Unternehmensadministrator eine Reihe externer Benutzern durch Hochladen einer Datei mit durch Kommas getrennten Werten (CSV) mit nicht mehr als 2000 Zeilen in das Portal für die B2B-Zusammenarbeit einladen und autorisieren.

   ![Dialogfeld zum Hochladen der CSV-Datei](./media/active-directory-b2b-collaboration-overview/upload-csv.png)
2. Das Portal sendet E-Mail-Einladungen an diese externen Benutzer.
3. Der eingeladene Benutzer meldet sich entweder mit einem vorhandenen Arbeitskonto bei Microsoft (verwaltet in Azure AD) an oder erhält ein neues Arbeitskonto in Azure AD.
4. Nach der Anmeldung wird der Benutzer zur App umgeleitet, die für ihn freigegeben wurde.

Einladungen an E-Mail-Adressen von Privatbenutzern (wie Gmail oder [*comcast.net*](http://comcast.net/)) werden derzeit nicht unterstützt.

Weitere Informationen zur Funktionsweise der B2B-Zusammenarbeit erhalten Sie in [diesem Video](http://aka.ms/aadshowb2b).

## <a name="next-steps"></a>Nächste Schritte
Weitere Artikel zur Azure AD B2B-Zusammenarbeit.

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [So funktioniert's](active-directory-b2b-how-it-works.md)
* [Ausführliche exemplarische Vorgehensweise](active-directory-b2b-detailed-walkthrough.md)
* [Referenz zum CSV-Dateiformat](active-directory-b2b-references-csv-file-format.md)
* [Tokenformat für externe Benutzer](active-directory-b2b-references-external-user-token-format.md)
* [Objektattributänderungen für externe Benutzer](active-directory-b2b-references-external-user-object-attribute-changes.md)
* [Aktuelle Einschränkungen der Vorschau](active-directory-b2b-current-preview-limitations.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)



<!--HONumber=Dec16_HO4-->


