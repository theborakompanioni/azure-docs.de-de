---
title: "Autorisieren von Entwicklerkonten mithilfe von Azure Active Directory B2C – Azure API Management | Microsoft-Dokumentation"
description: Erfahren Sie, wie Benutzer mithilfe von Azure Active Directory B2C in API Management autorisiert werden.
services: api-management
documentationcenter: API Management
author: miaojiang
manager: erikre
editor: 
ms.assetid: 33a69a83-94f2-4e4e-9cef-f2a5af3c9732
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
translationtype: Human Translation
ms.sourcegitcommit: c772f242f36e401c3b9f4b9be2366139c8a871f8
ms.openlocfilehash: 6a8505e52c2f422375063d6a780a6a205d768462
ms.lasthandoff: 02/24/2017

---
# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-b2c-in-azure-api-management"></a>Autorisieren von Entwicklerkonten mithilfe von Azure Active Directory B2C in Azure API Management
## <a name="overview"></a>Übersicht
Bei Azure Active Directory B2C handelt es sich um eine Lösung zur Cloudidentitätsverwaltung für kundenorientierte Web- und Mobilanwendungen. Sie können damit den Zugriff auf Ihr Entwicklerportal verwalten. In diesem Handbuch wird die in Ihrem API Management-Dienst für die Integration von Azure Active Directory B2C erforderliche Konfiguration gezeigt. Informationen zum Aktivieren des Zugriffs auf das Entwicklerportal mithilfe des klassischen Azure Active Directory finden Sie unter [Autorisieren von Entwicklerkonten mithilfe von Active Directory].

> [!NOTE]
> Zum Ausführen der hier genannten Schritte müssen Sie über einen Azure Active Directory B2C-Mandanten verfügen, in dem eine Anwendung erstellt wird, und Richtlinien zur Registrierung und Anmeldung bereit haben. Weitere Informationen finden Sie unter [Azure Active Directory B2C – Übersicht].
> 
> 

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory-b2c"></a>Autorisieren von Entwicklerkonten mithilfe von Azure Active Directory B2C

Klicken Sie zunächst im Azure-Portal für Ihren API Management-Dienst auf **Entwicklerportal**. Daraufhin gelangen Sie zum API Management-Herausgeberportal.

![Herausgeberportal][api-management-management-console]

> [!NOTE]
> Falls Sie noch keine API Management-Dienstinstanz erstellt haben, finden Sie weitere Informationen im Abschnitt [Erstellen einer API Management-Dienstinstanz][Create an API Management service instance] im Tutorial [Erste Schritte mit Azure API Management][Get started with Azure API Management].
> 
> 

Klicken Sie auf im **API Management**-Menü auf **Sicherheit**. Wählen Sie auf der Registerkarte **Identitäten** die Option **Azure Active Directory B2C**.

![Externe Identitäten][api-management-howto-aad-b2c-security-tab]

Notieren Sie sich den Wert für **Umleitungs-URL**, und wechseln Sie im Azure-Portal zu Azure Active Directory B2C.

![Externe Identitäten][api-management-howto-aad-b2c-security-tab-reply-url]

Klicken Sie auf die Schaltfläche **Anwendungen**.

![Neue Anwendung registrieren][api-management-howto-aad-b2c-portal-menu]

Klicken Sie auf die Schaltfläche **Hinzufügen**, um eine neue Azure Active Directory B2C-Anwendung zu erstellen.

![Neue Anwendung registrieren][api-management-howto-aad-b2c-add-button]

Geben Sie auf dem Blatt **Neue Anwendung** einen Namen für die Anwendung ein. Wählen Sie für **Web-App/Web-API** die Antwort **Ja**, und wählen Sie für **Impliziten Fluss** zulassen ebenfalls **Ja**. Kopieren Sie dann die **Umleitungs-URL** aus dem Abschnitt **Azure Active Directory B2C** der Registerkarte **Identitäten** im Herausgeberportal, und fügen Sie sie in das Textfeld **Antwort-URL** ein. 

![Neue Anwendung registrieren][api-management-howto-aad-b2c-app-details]

Klicken Sie auf die Schaltfläche **Erstellen** . Sobald die Anwendung erstellt wurde, wird sie auf dem Blatt **Anwendungen** angezeigt. Klicken Sie auf den Anwendungsnamen, um die Details anzuzeigen. 

![Neue Anwendung registrieren][api-management-howto-aad-b2c-app-created]

Kopieren Sie die **Anwendungs-ID** vom Blatt **Eigenschaften** in die Zwischenablage.

![App-ID][api-management-howto-aad-b2c-app-id]

Wechseln Sie zurück zum Herausgeberportal, und fügen Sie die ID in das Textfeld **Client-ID** ein.

![App-ID][api-management-howto-aad-b2c-client-id]

Wechseln Sie zurück zum Azure-Portal, und klicken Sie auf die Schaltfläche **Schlüssel** und anschließend auf **Schlüssel generieren**. Klicken Sie auf **Speichern** , um die Konfiguration zu speichern und den **App-Schlüssel** anzuzeigen. Kopieren Sie den Schlüssel in die Zwischenablage. 

![App-Schlüssel][api-management-howto-aad-b2c-app-key]

Wechseln Sie zurück zum Herausgeberportal, und fügen Sie den Schlüssel in das Textfeld **Geheimer Clientschlüssel** ein.

![App-Schlüssel][api-management-howto-aad-b2c-client-secret]

Geben Sie im Feld **Zulässiger Mandant** den Domänennamen des Azure Active Directory B2C-Mandanten an.

![Zulässiger Mandant][api-management-howto-aad-b2c-allowed-tenant]

Geben Sie die **Registrierungs**- und die **Anmelderichtlinie** an. Optional können Sie auch die Richtlinien für die **Profilbearbeitung** und die **Kennwortrücksetzung** angeben. 

![Richtlinien][api-management-howto-aad-b2c-policies]

> Weitere Informationen zu Richtlinien finden Sie unter [Azure Active Directory B2C: Erweiterbares Richtlinienframework].
>
>

Nachdem die gewünschte Konfiguration angegeben wurde, klicken Sie auf **Speichern**.

Nach dem Speichern der Änderungen können Entwickler mithilfe von AAD B2C neue Konten und Anmeldungen für das Entwicklerportal erstellen. 

## <a name="how-to-sign-up-for-a-developer-account-using-azure-active-directory-b2c"></a>Registrieren für ein Entwicklerkonto mithilfe von Azure Active Directory B2C

Zur Registrierung für ein Entwicklerkonto mithilfe von Azure Active Directory B2C öffnen Sie ein neues Browserfenster und wechseln zum Entwicklerportal. Klicken Sie auf die Schaltfläche **Registrieren**.

![Entwicklerportal][api-management-howto-aad-b2c-dev-portal]

Wählen Sie auf der Schaltfläche die Registrierung mit **Azure Active Directory B2C**. 

![Entwicklerportal][api-management-howto-aad-b2c-dev-portal-b2c-button]

Sie werden zur Registrierungsrichtlinie umgeleitet, die im vorherigen Abschnitt konfiguriert wurde, und können sich entweder mit Ihrer E-Mail-Adresse oder Ihren vorhandenen Konten für soziale Netzwerke registrieren. 

> [!NOTE]
> Wenn Azure Active Directory B2C als einzige Option im Herausgeberportal auf der Registerkarte **Identitäten** aktiviert ist, werden Sie direkt zur Registrierungsrichtlinie umgeleitet.
>
>

![Entwicklerportal][api-management-howto-aad-b2c-dev-portal-b2c-options]

Wenn die Registrierung abgeschlossen ist, werden Sie zum Entwicklerportal zurückgeleitet. Sie sind jetzt beim Entwicklerportal für Ihre API Management-Dienstinstanz angemeldet.

![Registrierung abgeschlossen][api-management-registration-complete]

## <a name="next-step"></a>Nächster Schritt

*  [Azure Active Directory B2C – Übersicht]
*  [Azure Active Directory B2C: Erweiterbares Richtlinienframework]
*  [Verwenden eines Microsoft-Kontos als Identitätsanbieter in Azure Active Directory B2C]
*  [Verwenden eines Google-Kontos als Identitätsanbieter in Azure Active Directory B2C]
*  [Verwenden eines LinkedIn-Kontos als Identitätsanbieter in Azure Active Directory B2C]
*  [Verwenden eines Facebook-Kontos als Identitätsanbieter in Azure Active Directory B2C]




[api-management-howto-aad-b2c-security-tab]: ./media/api-management-howto-aad-b2c/api-management-b2c-security-tab.PNG
[api-management-howto-aad-b2c-security-tab-reply-url]: ./media/api-management-howto-aad-b2c/api-management-b2c-security-tab-reply-url.PNG
[api-management-howto-aad-b2c-portal-menu]: ./media/api-management-howto-aad-b2c/api-management-b2c-portal-menu.PNG
[api-management-howto-aad-b2c-add-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-add-button.PNG
[api-management-howto-aad-b2c-app-details]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-details.PNG
[api-management-howto-aad-b2c-app-created]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-created.PNG
[api-management-howto-aad-b2c-app-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-id.PNG
[api-management-howto-aad-b2c-client-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-id.PNG
[api-management-howto-aad-b2c-app-key]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key.PNG
[api-management-howto-aad-b2c-app-key-saved]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key-saved.PNG
[api-management-howto-aad-b2c-client-secret]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-secret.PNG
[api-management-howto-aad-b2c-allowed-tenant]: ./media/api-management-howto-aad-b2c/api-management-b2c-allowed-tenant.PNG
[api-management-howto-aad-b2c-policies]: ./media/api-management-howto-aad-b2c/api-management-b2c-policies.PNG
[api-management-howto-aad-b2c-dev-portal]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-button.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-options]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-options.PNG
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.PNG
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-b2c-security-tab.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph
[Azure Active Directory B2C – Übersicht]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview
[Autorisieren von Entwicklerkonten mithilfe von Active Directory]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad
[Azure Active Directory B2C: Erweiterbares Richtlinienframework]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies
[Verwenden eines Microsoft-Kontos als Identitätsanbieter in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app
[Verwenden eines Google-Kontos als Identitätsanbieter in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app
[Verwenden eines Facebook-Kontos als Identitätsanbieter in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app
[Verwenden eines LinkedIn-Kontos als Identitätsanbieter in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account


