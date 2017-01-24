---
title: Debuggen des SAML-basierten einmaligen Anmeldens bei Anwendungen in Azure Active Directory | Microsoft Docs
description: 'Hier erfahren Sie, wie Sie das SAML-basierte einmalige Anmelden bei Anwendungen in Azure Active Directory debuggen. '
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/09/2016
ms.author: asmalser
translationtype: Human Translation
ms.sourcegitcommit: c579135f798ea0c2a5461fdd7c88244d2d6d78c6
ms.openlocfilehash: 245c2de65ca771cdc0e6371010246725c407ae11


---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Debuggen des SAML-basierten einmaligen Anmeldens bei Anwendungen in Azure Active Directory
Beim Debuggen einer SAML-basierten Anwendungsintegration ist es oft hilfreich, ein Tool wie [Fiddler](http://www.telerik.com/fiddler) zu verwenden, um sich die SAML-Anforderung, die SAML-Antwort und das eigentliche SAML-Token anzusehen, das für die Anwendung ausgegeben wird. Anhand des SAML-Tokens können Sie sicherstellen, dass alle erforderlichen Attribute, der Benutzername des SAML-Subjects und der URI des Ausstellers erwartungsgemäß übermittelt werden.

![][1]

Die Antwort von Azure AD, die das SAML-Token enthält, erfolgt in der Regel im Anschluss an eine HTTP 302-Umleitung von „https://login.windows.net“ und wird an die konfigurierte **Antwort-URL** der Anwendung gesendet. 

Sie können das SAML-Token anzeigen, indem Sie diese Zeile auswählen und anschließend im rechten Bereich zur Registerkarte **Inspectors > WebForms** (Inspectors > WebForms) navigieren. Klicken Sie dort mit der rechten Maustaste auf den Wert **SAMLResponse**, und wählen Sie **Send to TextWizard** (An „TextWizard“ senden). Wählen Sie dann im Menü **Transform** (Transformieren) die Option **From Base64** (Aus Base64) aus, um das Token zu decodieren und seinen Inhalt anzuzeigen.

**Hinweis**: Fiddler fordert Sie unter Umständen auf, die Entschlüsselung von HTTPS-Datenverkehr zu konfigurieren, um den Inhalt dieser HTTP-Anforderung anzeigen zu können.

## <a name="related-articles"></a>Verwandte Artikel
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](../active-directory-apps-index.md)
* [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure Active Directory-Anwendungskatalog enthalten sind](../active-directory-saas-custom-apps.md)
* [Gewusst wie: Anpassen ausgestellter Ansprüche im SAML-Token für bereits integrierte Apps](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png



<!--HONumber=Jan17_HO3-->


