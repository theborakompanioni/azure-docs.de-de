---
title: "Hinzufügen einer mehrinstanzenfähigen Anwendung zum Azure AD-Anwendungskatalog | Microsoft-Dokumentation"
description: "Erläutert, wie Sie Ihre benutzerdefiniert entwickelte mehrinstanzenfähige Anwendung im Azure AD-Anwendungskatalog auflisten können."
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
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 9f3caff0e998d670ce82b847525a63f2a85572d3
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

# <a name="how-to-add-a-multi-tenant-application-to-the-azure-ad-application-gallery"></a>Hinzufügen einer mehrinstanzenfähigen Anwendung zum Azure AD-Anwendungskatalog

## <a name="what-is-the-azure-ad-application-gallery"></a>Was ist der Azure AD-Anwendungskatalog?

Der Azure AD-Anwendungskatalog stellt eine hervorragende Möglichkeit dar, um Ihre Anwendung Millionen von Kunden von Azure Active Directory zu präsentieren, um den Einfluss und die Reichweite der Anwendung auf dem Markt zu vergrößern. In den folgenden Schritte wird erläutert, wie Sie Ihre Anwendung im Azure AD-Anwendungskatalog auflisten.

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Wenn Ihre Anwendung SAML oder OpenIDConnect unterstützt
Wenn Sie über eine mehrinstanzenfähige Anwendung verfügen, die Sie zum Azure AD-Anwendungskatalog hinzufügen möchten, müssen Sie zunächst sicherstellen, dass Ihre Anwendung eine der folgenden Technologien zum einmaligen Anmelden unterstützt:

1. **OpenID Connect** : Direkte Integration in Azure AD unter Verwendung von OpenID Connect für die Authentifizierung und der Azure AD-Zustimmungs-API für die Konfiguration. Wenn Sie gerade erst mit der Integration beginnen und Ihre Anwendung SAML nicht unterstützt, ist dies der empfohlene Modus.
2. **SAML** : In Ihrer Anwendung können bereits Identitätsanbieter von Drittanbietern mithilfe des SAML-Protokolls konfiguriert werden.

Wenn Ihre Anwendung einen dieser Modi zum einmaligen Anmelden unterstützt und Sie Ihre mehrinstanzenfähige Anwendung zum Azure AD-Anwendungskatalog hinzufügen möchten, führen Sie die Schritte im folgenden Dokument aus. Senden Sie zum Schnelleinstieg eine E-Mail an **waadpartners@microsoft.com**.

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Wenn Ihre Anwendung keine SAML- oder OpenIDConnect-Unterstützung bietet
Auch wenn Ihre Anwendung keinen dieser Modi unterstützt, können wir sie mittels einmaligem Anmelden per Kennwort in unseren Katalog integrieren. Wenn Sie diese Option nutzen möchten, können Sie eine E-Mail an **waadpartners@microsoft.com** senden.

## <a name="next-steps"></a>Nächste Schritte
[Auflisten Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)

