---
title: "Gewähren von Berechtigungen für eine benutzerdefiniert entwickelte Anwendung | Microsoft-Dokumentation"
description: "Gewähren von Berechtigungen für Ihre benutzerdefiniert entwickelte Anwendung mit dem Azure AD-Portal oder einem URL-Parameter"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: asteen
translationtype: Human Translation
ms.sourcegitcommit: c300ba45cd530e5a606786aa7b2b254c2ed32fcd
ms.openlocfilehash: 922774c2482737537b64787ae473231ec1fbb68e
ms.lasthandoff: 04/14/2017


---

# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a>Gewähren von Berechtigungen für eine benutzerdefiniert entwickelte Anwendung

Wenn Sie im Vorfeld Berechtigungen für Ihre App gewähren möchten oder ein Fehler auftritt, den Sie keiner App zugeteilt haben, führen Sie die unten stehenden Schritte aus.

## <a name="how-to-perform-admin-consent-for-your-application"></a>Ausführen der Administratorzustimmung für Ihre Anwendung

Dies entspricht dem Erteilen der Zustimmung zur Anwendung für alle Benutzer in Ihrer Organisation.

1. Navigieren Sie als **globaler Administrator** zum Blatt **App-Registrierungen**, und wählen Sie dann die App aus.

2. Wählen Sie **Erforderliche Berechtigungen** und dann die Schaltfläche **Gewähren von Berechtigungen** am oberen Rand des Blatts aus.

Alternativ können Sie eine Anforderung an *login.microsoftonline.com* mit Ihrer App-Konfiguration erstellen und an *&prompt=admin\_consent* anfügen. Nach der Anmeldung als Administrator sollte die App-Zustimmung für alle Benutzer erteilt sein.

## <a name="how-to-force-user-consent-for-your-application"></a>Erzwingen der Benutzerzustimmung für Ihre Anwendung

* Fügen Sie *&prompt=consent* an Autorisierungsanforderungen an, die bei jeder Authentifizierung Zustimmung durch die Endbenutzer erfordern.

## <a name="next-steps"></a>Nächste Schritte

[Genehmigen und Integrieren von Apps in Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

[Genehmigen und Zuweisen von Berechtigungen für konvergierte Azure AD V2.0-Apps](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)

