---
title: "Azure Active Directory B2C: Übersicht | Microsoft Docs"
description: Entwickeln von kundenorientierten Anwendungen mit Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: c465dbde-f800-4f2e-8814-0ff5f5dae610
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: swkrish
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: 2f64c905d2304bfc94415e871012a783cd2cf328
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Azure Active Directory B2C: Registrieren und Anmelden von Kunden in den Anwendungen
Bei Azure Active Directory B2C handelt es sich um eine umfassende Lösung zur Cloudidentitätsverwaltung für kundenorientierte Web- und Mobilanwendungen. Es ist ein hochverfügbarer globaler Dienst, der für Hunderte von Millionen von Kundenidentitäten skaliert werden kann. Azure Active Directory B2C basiert auf einer sicheren Plattform der Unternehmensklasse und schützt Ihre Anwendungen, Ihr Unternehmen und Ihre Kunden.

In der Vergangenheit mussten Anwendungsentwickler ihren eigenen Code schreiben, wenn Sie Verbraucher in ihren Anwendungen registrieren und anmelden wollten. Und sie hätten lokale Datenbanken oder Systeme zum Speichern von Benutzernamen und Kennwörtern verwendet. Azure Active Directory B2C bietet Entwicklern eine bessere Lösung, um die Verwaltung von Kundenidentitäten in ihre Anwendungen zu integrieren. Dabei kommen eine sichere, standardbasierte Plattform und ein umfassender Satz von erweiterbaren Richtlinien zum Einsatz. Wenn Sie Azure Active Directory B2C verwenden, können sich Ihre Kunden mit vorhandenen Konten in sozialen Netzwerken (Facebook, Google, Amazon, LinkedIn) oder durch Erstellen neuer Anmeldeinformationen (E-Mail-Adresse und Kennwort oder Benutzername und Kennwort, im Folgenden als „lokale Konten“ bezeichnet) bei Ihren Anwendungen registrieren.

## <a name="get-started"></a>Erste Schritte
Zum Erstellen einer Anwendung, die Registrierungen und Anmeldungen von Kunden akzeptiert, müssen Sie die Anwendung zunächst bei einem Azure Active Directory B2C-Mandanten registrieren. Erstellen Sie einen eigenen Mandanten mithilfe der unter [Erstellen eines Azure AD B2C-Mandanten](active-directory-b2c-get-started.md)beschriebenen Schritte.

Sie können Ihre Anwendung für den Azure Active Directory B2C-Dienst programmieren, indem Sie direkt Protokollnachrichten mithilfe von [OAuth 2.0 oder Open ID Connect](active-directory-b2c-reference-protocols.md) senden oder die von uns bereitgestellten Bibliotheken verwenden. Wählen Sie zum Einstieg in der folgenden Tabelle Ihre bevorzugte Plattform aus.

[!INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>Neuigkeiten
Sie sollten diese Seite häufiger aufrufen, da Informationen über zukünftige Änderungen an Azure Active Directory B2C hier aufgeführt werden. Unter @AzureAD veröffentlichen wir außerdem Tweets zu Updates.

* Informieren Sie sich über das [erweiterbare Richtlinienframework](active-directory-b2c-reference-policies.md) und die Typen von Richtlinien, die Sie in Ihren Anwendungen erstellen und verwenden können.
* Legen Sie sich ein Lesezeichen für unseren [Dienst-Blog](https://blogs.msdn.microsoft.com/azureadb2c/) an, damit Sie keine Benachrichtigungen zu kleineren Dienstproblemen, Updates, zum Status oder zu Lösungen verpassen. Verfolgen Sie den Stand außerdem weiter im [Azure-Statusdashboard](https://azure.microsoft.com/status/) .
* Aktuelle [Einschränkungen der Vorschau](active-directory-b2c-limitations.md)
* [Codebeispiel](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c) mit Azure AD B2C und ASP.NET Core

## <a name="how-to-articles"></a>Anleitungen
Informieren Sie sich über die Verwendung bestimmter Azure Active Directory B2C-Features:

* Konfigurieren Sie, dass Konten bei [Facebook](active-directory-b2c-setup-fb-app.md), [Google+](active-directory-b2c-setup-goog-app.md), [Microsoft](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md) und [LinkedIn](active-directory-b2c-setup-li-app.md) in Ihren Anwendungen für Endkunden verwendet werden können.
* [Verwenden Sie benutzerdefinierte Attribute zum Erfassen von Informationen über Ihre Kunden](active-directory-b2c-reference-custom-attr.md).
* [Aktivieren Sie Azure Multi-Factor Authentication in Ihren kundenorientierten Anwendungen](active-directory-b2c-reference-mfa.md).
* [Richten Sie die Self-Service-Kennwortzurücksetzung für Kunden ein](active-directory-b2c-reference-sspr.md).
* [Passen Sie das Aussehen und Verhalten der Seiten für die Registrierung und Anmeldung und anderer Seiten für Kunden an](active-directory-b2c-reference-ui-customization.md) , die von Azure Active Directory B2C bereitgestellt werden.
* [Verwenden Sie die Azure Active Directory Graph-API, um Kunden im Azure Active Directory B2C-Mandanten programmgesteuert zu erstellen, zu lesen, zu aktualisieren und zu löschen.](active-directory-b2c-devquickstarts-graph-dotnet.md)

## <a name="next-steps"></a>Nächste Schritte
Unter den folgenden Links finden Sie ausführliche Informationen zum Dienst:

* Hier finden Sie [Azure Active Directory B2C-Preisinformationen](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
* Sehen Sie sich unsere [Codebeispiele](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory&term=b2c) für Azure Active Directory B2C an. 
* Verwenden Sie das Tag [azure-ad-b2c](http://stackoverflow.com/questions/tagged/azure-ad-b2c), um Hilfe zu Stack Overflow zu erhalten.
* Teilen Sie uns Ihre Meinung über [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) mit.
* Lesen Sie die [Referenz zu Azure AD B2C-Protokollen](active-directory-b2c-reference-protocols.md).
* Lesen Sie die [Referenz zu Azure AD B2C-Token](active-directory-b2c-reference-tokens.md).
* Lesen Sie die [häufig gestellten Fragen zu Azure Active Directory B2C](active-directory-b2c-faqs.md).
* [Senden von Supportanfragen für Azure Active Directory B2C](active-directory-b2c-support.md)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unsere Produkte
Wir empfehlen Ihnen, den Erhalt von Benachrichtigungen zu Sicherheitsvorfällen einzurichten. Rufen Sie dazu [diese Seite](https://technet.microsoft.com/security/dd252948) auf, und abonnieren Sie Sicherheitsempfehlungen.


