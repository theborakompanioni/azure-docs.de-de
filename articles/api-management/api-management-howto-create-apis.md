---
title: Erstellen von APIs in Azure API Management
description: Erfahren Sie, wie Sie APIs in Azure API Management erstellen und konfigurieren.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 14c20da4-f29f-4b28-bec7-3d4c50b734da
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apipm
translationtype: Human Translation
ms.sourcegitcommit: 30ec6f45da114b6c7bc081f8a2df46f037de61fd
ms.openlocfilehash: 496aa7754b35877900da020283c8b7efe0e04cef


---
# <a name="how-to-create-apis-in-azure-api-management"></a>Erstellen von APIs in Azure API Management
Eine API in API Management besteht aus einem Satz von Operationen, die von Clientanwendungen aufgerufen werden können. Neue APIs werden im Herausgeberportal erstellt, anschließend werden die gewünschten Operationen hinzugefügt. Nachdem Sie die Operationen hinzugefügt haben, wird die API zu einem Produkt hinzugefügt und kann veröffentlicht werden. Nach der Veröffentlichung der API können Entwickler diese abonnieren und verwenden.

Diese Anleitung beschreibt die ersten Schritte im Prozess: das Erstellen und Konfigurieren einer neuen API in API Management. Weitere Informationen zum Hinzufügen von Operationen und zum Veröffentlichen von Produkten finden Sie unter [Hinzufügen von Operationen zu einer API][How to add operations to an API] und [Erstellen und Veröffentlichen von Produkten][How to create and publish a product].

## <a name="create-new-api"> </a>Erstellen einer neuen API
APIs werden im Herausgeberportal erstellt und konfiguriert. Um auf das Herausgeberportal zuzugreifen, klicken Sie im Azure-Portal für Ihren API Management-Dienst auf **Herausgeberportal**.

![Herausgeberportal][api-management-management-console]

> Falls Sie noch keine API Management-Dienstinstanz erstellt haben, finden Sie weitere Informationen im Abschnitt [Erstellen einer API Management-Dienstinstanz][Create an API Management service instance] im Tutorial [Erste Schritte mit Azure API Management][Get started with Azure API Management].
> 
> 

Klicken Sie im Menü **API Management** auf der linken Seite auf **APIs**, und klicken Sie anschließend auf **API hinzufügen**.

![API erstellen][api-management-create-api]

Konfigurieren Sie die neue API im Fenster **Neue API hinzufügen** .

![Neue API hinzufügen][api-management-add-new-api]

Die folgenden Felder werden zum Konfigurieren der neuen API verwendet.

* **Name der Web-API** enthält einen eindeutigen und beschreibenden Namen für die API. Dieser Name wird in den Entwickler- und Herausgeberportalen angezeigt.
* **Webdienst-URL** verweist auf den HTTP-Dienst, der die API implementiert. API Management leitet Anfragen an diese Adresse weiter.
* **URL-Suffix für Web-API** wird an die Basis-URL für den API Management-Dienst angehängt. Alle in einer API Management-Dienstinstanz gehosteten APIs teilen sich dieselbe Basis-URL. API Management unterscheidet APIs durch deren Suffix. Daher muss jede API eines bestimmten Herausgebers ein eindeutiges Suffix haben.
* **URL-Schema für Web-API** wird festgelegt, welche Protokolle für den Zugriff auf die API verwendet werden können. Standardmäßig wird HTTPS angegeben.
* Um diese neue API optional einem Produkt hinzuzufügen, klicken Sie in das Dropdown-Listenfeld **Produkte (optional)** und wählen ein Produkt aus. Dieser Schritt kann mehrfach wiederholt werden, um die API mehreren Produkten hinzuzufügen.

Wenn Sie die gewünschten Werte konfiguriert haben, klicken Sie auf **Speichern**. Sobald die neue API erstellt wurde, wird die Zusammenfassungsseite für die API im Herausgeberportal angezeigt.

![API-Zusammenfassung][api-management-api-summary]

## <a name="configure-api-settings"> </a>Konfigurieren der API-Einstellungen
Auf der Registerkarte **Einstellungen** können Sie die Konfiguration einer API anzeigen und bearbeiten. **Name der Web-API**, **Webdienst-URL** und **URL-Suffix für die Web-API** werden bei der Erstellung der API konfiguriert und können hier geändert werden. **Beschreibung** liefert eine optionale Beschreibung, und mit **URL-Schema für Web-API** wird festgelegt, welche Protokolle für den Zugriff auf die API verwendet werden können.

![API-Einstellungen][api-management-api-settings]

Um die Gatewayauthentifizierung für die Back-End-Dienst-Implementierung der API zu konfigurieren, wählen Sie die Registerkarte **Sicherheit** . Über die Dropdownliste **Mit Anmeldeinformationen** können Sie die **HTTP-Standardauthentifizierung** oder die **Clientzertifikatauthentifizierung** konfigurieren. Zum Verwenden der HTTP-Standardauthentifizierung geben Sie einfach die gewünschten Anmeldeinformationen ein. Informationen zum Verwenden der Clientzertifikatauthentifizierung finden Sie unter [Sichern von Back-End-Diensten über die Clientzertifikatauthentifizierung in Azure API Management][How to secure back-end services using client certificate authentication in Azure API Management].

Auf der Registerkarte **Sicherheit** können Sie außerdem die **Benutzerautorisierung** mithilfe von OAuth 2.0 konfigurieren. Weitere Informationen finden Sie unter [Autorisieren von Entwicklerkonten mithilfe von OAuth 2.0 in Azure API Management][How to authorize developer accounts using OAuth 2.0 in Azure API Management].

![Einstellungen für die Standardauthentifizierung][api-management-api-settings-credentials]

Klicken Sie auf **Speichern** , um die Änderungen an Ihren API-Einstellungen zu speichern.

## <a name="next-steps"> </a>Nächste Schritte
Nachdem Sie die API erstellt und die Einstellungen konfiguriert haben, können Sie Operationen zur API hinzufügen, die API zu einem Produkt hinzufügen und anschließend veröffentlichen, um die API für Entwickler zugänglich zu machen. Weitere Informationen finden Sie in den folgenden Artikeln.

* [Hinzufügen von Operationen zu einer API][How to add operations to an API]
* [Erstellen und Veröffentlichen von Produkten][How to create and publish a product]

[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How to secure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How to authorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md



<!--HONumber=Dec16_HO3-->


