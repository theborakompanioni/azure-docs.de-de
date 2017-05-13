---
title: "Anpassen ausgestellter Ansprüche im SAML-Token für bereits in Azure Active Directory integrierte Apps | Microsoft Docs"
description: "Erfahren Sie, wie Sie im SAML-Token für bereits in Azure Active Directory integrierte Apps ausgestellte Ansprüche anpassen."
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: asmalser
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: e89a06de6232afef579c32d51137ddf577917436
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps-in-azure-active-directory"></a>Anpassen ausgestellter Ansprüche im SAML-Token für bereits in Azure Active Directory integrierte Apps
Azure Active Directory unterstützt heute Tausende von bereits integrierten Anwendungen im Azure AD-Anwendungskatalog, darunter über 150, die einmaliges Anmelden mithilfe des SAML 2.0-Protokolls unterstützen. Wenn sich ein Benutzer mithilfe von SAML über Azure AD bei einer Anwendung authentifiziert, sendet Azure AD ein Token an die Anwendung (über eine HTTP 302-Umleitung). Die Anwendung überprüft und verwendet dann das Token, um den Benutzer anzumelden, anstatt den Benutzernamen und das Kennwort anzufordern. Diese SAML-Token enthalten Informationen über den Benutzer, die als „Ansprüche“ bezeichnet werden.

Im technischen Jargon bezeichnet ein Anspruch (Claim) Informationen über den Benutzer, die ein Identitätsanbieter im Benutzertoken übergibt. Bei einem [SAML-Token](http://en.wikipedia.org/wiki/SAML_2.0) befinden sich diese Informationen in der Regel in der SAML-Attributanweisung, und die eindeutige ID des Benutzers wird durch den SAML-Betreff dargestellt.

Standardmäßig stellt Azure AD der Anwendung ein SAML-Token aus, das einen NameIdentifier-Anspruch enthält, dessen Wert dem Benutzernamen in Azure AD entspricht. Über diesen Wert kann der Benutzer eindeutig identifiziert werden. Das SAML-Token enthält auch zusätzliche Ansprüche, die E-Mail-Adresse des Benutzers, Vorname und Nachname enthält.

Zum Anzeigen oder Bearbeiten der im SAML-Token an die Anwendung ausgestellten Ansprüche öffnen Sie den Anwendungsdatensatz im klassischen Azure-Portal, und wählen Sie die Registerkarte **Attribute** unterhalb der Anwendung aus.

![Registerkarte „Attribute“][1]

Es gibt zwei Gründe dafür, dass Sie die im SAML-Token ausgegebenen Ansprüche möglicherweise bearbeiten müssen:
* Die Anwendung wurde so geschrieben, dass sie einen anderen Satz an Anspruchs-URIs oder Anspruchswerten erfordert. 
* Ihre Anwendung wurde auf eine Weise bereitgestellt, die erfordert, dass es sich beim NameIdentifier-Anspruch nicht um den in Azure Active Directory gespeicherten Benutzernamen (also Benutzerprinzipalnamen) handelt. 

Sie können beliebige Standardwerte eines Anspruchs bearbeiten. Wählen Sie dazu das Stiftsymbol aus, das auf der rechten Seite angezeigt wird, wenn sich der Mauszeiger über einer der Zeilen in der Tabelle der SAML-Tokenattribute befindet. Ansprüche (mit Ausnahme von „NameIdentifier“) lassen sich mithilfe des Symbols **X** auch entfernen, und es können neue Ansprüche über die Schaltfläche **Benutzerattribut hinzufügen** erstellt werden.

## <a name="editing-the-nameidentifier-claim"></a>Bearbeiten des NameIdentifier-Anspruchs
Falls die Anwendung mit einem anderen Benutzernamen bereitgestellt wurde, klicken Sie auf das Stiftsymbol neben dem NameIdentifier-Anspruch. Dadurch wird ein Dialogfeld mit mehreren Optionen angezeigt:

![Benutzerattribut bearbeiten][2]

Wählen Sie im Menü **Attributwert** die Option **user.mail** aus, um den NameIdentifier Anspruch auf die E-Mail-Adresse des Benutzers im Verzeichnis festzulegen. Oder wählen Sie **user.onpremisessamaccountname** aus, um den SAM-Kontonamen des Benutzers festzulegen, der vom lokalen Azure AD synchronisiert wurde.

Sie können auch die spezielle ExtractMailPrefix()-Funktion verwenden, um das Domänensuffix von der E-Mail-Adresse oder dem Benutzerprinzipalnamen zu entfernen. Dann wird nur der erste Teil des Benutzernamens übergeben (z.B. „joe_smith“ anstelle von joe_smith@contoso.com).

![Benutzerattribut bearbeiten][3]

## <a name="adding-claims"></a>Hinzufügen von Ansprüchen
Wenn Sie einen Anspruch hinzufügen, können Sie den Attributnamen angeben (der nicht unbedingt wie bei der SAML-Spezifikation einem URI-Muster entsprechen muss). Legen Sie den Wert auf ein beliebiges Benutzerattribut fest, das im Verzeichnis gespeichert ist.

![Benutzerattribut hinzufügen][4]

Sie können beispielsweise die Abteilung des Benutzers in seiner Organisation als Anspruch übermitteln (z.B. Sales). Da Sie einen beliebigen Anspruchswert, der von der Anwendung erwartet wird, eingeben können, wählen Sie in diesem Fall **user.department** als Wert aus.

Wenn für einen bestimmten Benutzer kein Wert für ein ausgewähltes Attribut gespeichert ist, wird dieser Anspruch nicht in dem Token ausgestellt.

**Hinweis:** Die Optionen **user.onpremisesecurityidentifier** und **user.onpremisesamaccountname** werden nur beim Synchronisieren von Benutzerdaten aus der lokalen Active Directory-Instanz mithilfe des Tools [Azure AD Connect](../active-directory-aadconnect.md) unterstützt.

## <a name="next-steps"></a>Nächste Schritte
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](../active-directory-apps-index.md)
* [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure Active Directory-Anwendungskatalog enthalten sind](../active-directory-saas-custom-apps.md)
* [Problembehandlung bei SAML-basiertem einmaligem Anmelden](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ../media/active-directory-saml-claims-customization/claimscustomization1.png
[2]: ../media/active-directory-saml-claims-customization/claimscustomization2.png
[3]: ../media/active-directory-saml-claims-customization/claimscustomization3.png
[4]: ../media/active-directory-saml-claims-customization/claimscustomization4.png
