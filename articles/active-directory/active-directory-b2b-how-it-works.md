---
title: 'Vorschau der Azure AD B2B-Zusammenarbeit: Funktionsweise | Microsoft Docs'
description: "Beschreibt, wie die Azure Active Directory B2B-Zusammenarbeit Geschäftspartnern den gezielten Zugriff auf Ihre Unternehmensanwendungen ermöglicht und so Ihre unternehmensübergreifenden Beziehungen unterstützt."
services: active-directory
documentationcenter: 
author: viv-liu
manager: cliffdi
editor: 
tags: 
ms.assetid: 0c1cdde6-1988-4eff-a876-75c8f845f0ed
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/09/2016
ms.author: viviali
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 4f458a0835951692fa447d19b2ce1dda7cc2c8e2


---
# <a name="azure-ad-b2b-collaboration-preview-how-it-works"></a>Vorschau der Azure AD B2B-Zusammenarbeit: Funktionsweise
Die Azure AD-B2B-Zusammenarbeit basiert auf einem Einladungs- und Einlösemodell. Sie geben die E-Mail-Adressen der Parteien, mit denen Sie zusammenarbeiten möchten, sowie die Anwendungen ein, die sie verwenden sollen. Azure AD sendet den Parteien eine E-Mail-Einladung mit einem Link. Der Partnerbenutzer folgt dem Link und wird aufgefordert, sich über sein Azure AD-Konto anzumelden oder sich für ein neues Azure AD-Konto zu registrieren.

1. Ihr Administrator lädt Partnerbenutzer durch Hochladen einer [strukturierten CSV-Datei](active-directory-b2b-references-csv-file-format.md) über das Azure-Portal ein.
2. Über das Portal werden Einladungs-E-Mails an diese Partnerbenutzer gesendet.
3. Die Partnerbenutzer klicken auf den Link in der E-Mail und werden aufgefordert, sich mit den Anmeldeinformationen ihrer Arbeit anzumelden (wenn sie nicht bereits in Azure AD vorhanden sind) oder sich als Benutzer der Azure AD B2B-Zusammenarbeit zu registrieren.
4. Die Partnerbenutzer werden zu der Anwendung umgeleitet, zu der sie eingeladen worden sind und auf die sie nun Zugriff haben.

## <a name="directory-operations"></a>Verzeichnisvorgänge
Partnerbenutzer sind in Ihrem Azure AD als externe Benutzer vorhanden. Das bedeutet, dass Ihr Administrator genau wie für Benutzer in Ihrem Unternehmen über das Azure-Portal oder mithilfe von Azure PowerShell für diese externen Benutzer Lizenzen bereitstellen, eine Gruppenmitgliedschaft zuweisen und zudem Zugriff auf Unternehmens-Apps gewähren kann.

Obwohl zur Nutzung von Azure AD B2B kein kostenpflichtiges Azure AD-Abonnement (Basic oder Premium) erforderlich ist, profitieren Mandanten mit einem kostenpflichtigen Azure AD-Abonnement (Basic oder Premium) von den folgenden zusätzlichen Vorteilen:

* Administratoren können Apps Gruppen zuweisen, sodass sich die Verwaltung des Zugriffs für eingeladene Benutzer vereinfacht.
* Für das Branding der Einladungs-E-Mails und beim Einlösen wird das Mandantenbranding verwendet, sodass den eingeladenen Partnerbenutzern mehr Kontext zur Verfügung steht.

## <a name="related-articles"></a>Verwandte Artikel
 Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Ausführliche exemplarische Vorgehensweise](active-directory-b2b-detailed-walkthrough.md)
* [Referenz zum CSV-Dateiformat](active-directory-b2b-references-csv-file-format.md)
* [Tokenformat für externe Benutzer](active-directory-b2b-references-external-user-token-format.md)
* [Objektattributänderungen für externe Benutzer](active-directory-b2b-references-external-user-object-attribute-changes.md)
* [Aktuelle Einschränkungen der Vorschau](active-directory-b2b-current-preview-limitations.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)




<!--HONumber=Nov16_HO3-->


