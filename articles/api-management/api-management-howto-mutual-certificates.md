---
title: "Sichern von Back-End-Diensten über eine Clientzertifikatauthentifizierung in Azure API Management"
description: "Erfahren Sie, wie Sie Back-End-Dienste über eine Clientzertifikatauthentifizierung in Azure API Management sichern."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 43453331-39b2-4672-80b8-0a87e4fde3c6
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ad14ed8b36d6d0a2121c32fd9a54de97e8b02342


---
# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Sichern von Back-End-Diensten über eine Clientzertifikatauthentifizierung in Azure API Management
API Management bietet die Möglichkeit, den Zugriff auf den Back-End-Dienst einer API mithilfe von Clientzertifikaten abzusichern. Diese Anleitung beschreibt, wie Zertifikate im API-Herausgeberportal verwaltet werden und eine API zur Verwendung eines Zertifikats für den Zugriff auf ihren Back-End-Dienst konfiguriert wird.

Informationen über die Verwaltung von Zertifikaten mit der API Management REST API finden Sie unter [Azure API Management-REST-API-Zertifikatsentität][Azure API Management-REST-API-Zertifikatsentität].

## <a name="prerequisites"> </a>Voraussetzungen
Diese Anleitung beschreibt, wie Sie Ihre Instanz des API Management-Diensts konfigurieren, um für den Zugriff auf den Back-End-Dienst einer API die Clientzertifikatauthentifizierung zu verwenden. Bevor Sie die Schritte in diesem Thema ausführen, müssen Sie Ihren Back-End-Dienst für die Clientzertifikatauthentifizierung konfiguriert haben und auf das Zertifikat und das Kennwort zum Hochladen in das API Management-Herausgeberportal zugreifen können ([Ausführliche Informationen zum Konfigurieren der Zertifikatauthentifizierung in Azure WebSites finden Sie in diesem Artikel.][Ausführliche Informationen zum Konfigurieren der Zertifikatauthentifizierung in Azure WebSites finden Sie in diesem Artikel.]).

## <a name="step1"> </a>Hochladen eines Clientzertifikats
Klicken Sie zunächst im Azure-Portal für Ihren API Management-Dienst auf **Entwicklerportal**. Daraufhin gelangen Sie zum API Management-Herausgeberportal.

![API-Herausgeberportal][api-management-management-console]

> Falls Sie noch keine API Management-Dienstinstanz erstellt haben, finden Sie weitere Informationen im Abschnitt [Erstellen einer API Management-Dienstinstanz][Erstellen einer API Management-Dienstinstanz] im Tutorial [Erste Schritte mit Azure API Management][Erste Schritte mit Azure API Management].
> 
> 

Klicken Sie auf der linken Seite im Menü **API Management** auf **Sicherheit** und dann auf **Clientzertifikate**.

![Clientzertifikate][api-management-security-client-certificates]

Um ein neues Zertifikat hochzuladen, klicken Sie auf **Hochladen des Zertifikats**.

![Hochladen des Zertifikats][api-management-upload-certificate]

Navigieren Sie zu Ihrem Zertifikat, und geben Sie das Kennwort für das Zertifikat ein.

> Das Zertifikat muss im **.pfx** -Format vorliegen. Selbstsignierte Zertifikate sind zulässig.
> 
> 

![Hochladen des Zertifikats][api-management-upload-certificate-form]

Klicken Sie auf **Hochladen** , um das Zertifikat hochzuladen.

> Zu diesem Zeitpunkt wird das Kennwort des Zertifikats überprüft. Falls es fehlerhaft ist, wird eine Fehlermeldung angezeigt.
> 
> 

![Zertifikat hochgeladen][api-management-certificate-uploaded]

Sobald das Zertifikat hochgeladen wurde, wird es auf der Registerkarte **Clientzertifikate** angezeigt. Falls Sie über mehrere Zertifikate verfügen, notieren Sie sich den Zweck und die letzten vier Zeichen des Fingerabdrucks, anhand derer Sie das Zertifikat auswählen, wenn Sie eine API zum Verwenden von Zertifikaten konfigurieren. Weitere Informationen zu diesem Thema finden Sie unter [Konfigurieren einer API zum Verwenden eines Clientzertifikats für die Gatewayauthentifizierung][Konfigurieren einer API zum Verwenden eines Clientzertifikats für die Gatewayauthentifizierung].

> Wenn Sie beispielsweise bei der Verwendung eines selbstsignierten Zertifikats die Überprüfung der Zertifikatkette deaktivieren möchten, führen Sie die Schritte in [diesem Punkt](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end) der häufig gestellten Fragen aus.
> 
> 

## <a name="step1a"> </a>Löschen eines Clientzertifikats
Um ein Zertifikat zu löschen, klicken Sie neben dem betreffenden Zertifikat auf **Löschen** .

![Löschen eines Zertifikats][api-management-certificate-delete]

Klicken Sie zur Bestätigung auf **Ja, löschen** .

![Bestätigen des Löschens][api-management-confirm-delete]

Falls das Zertifikat von einer API verwendet wird, wird ein Warnbildschirm angezeigt. Um das Zertifikat zu löschen, müssen Sie es zunächst aus allen APIs entfernen, die zu seiner Verwendung konfiguriert sind.

![Bestätigen des Löschens][api-management-confirm-delete-policy]

## <a name="step2"> </a>Konfigurieren einer API zum Verwenden eines Clientzertifikats für die Gatewayauthentifizierung
Klicken Sie auf der linken Seite im Menü **API Management** auf **APIs**, auf die gewünschte API und dann auf die Registerkarte **Sicherheit**.

![API-Sicherheit][api-management-api-security]

Wählen Sie in der Dropdownliste **Mit Anmeldeinformationen** die Option **Clientzertifikate** aus.

![Clientzertifikate][api-management-mutual-certificates]

Wählen Sie in der Dropdownliste **Clientzertifikate** das gewünschte Zertifikat aus. Falls mehrere Zertifikate vorhanden sind, können Sie sich bei der Bestimmung des richtigen Zertifikats am angegebenen Thema oder den letzten vier Zeichen des Fingerabdrucks orientieren, wie im vorherigen Abschnitt beschrieben.

![Zertifikat auswählen][api-management-select-certificate]

Klicken Sie auf **Speichern** , um die Konfigurationsänderungen der API zu speichern.

> Die Änderungen werden unmittelbar übernommen, und Funktionsaufrufe dieser API verwenden das Zertifikat für die Authentifizierung beim Back-End-Server.
> 
> 

![API-Änderungen speichern][api-management-save-api]

> Wenn für den Back-End-Dienst einer API ein Zertifikat zur Gateway-authentifizierung angegeben ist, wird es Teil der Richtlinie dieser API und kann im Richtlinien-Editor angezeigt werden.
> 
> 

![Zertifikatrichtlinie][api-management-certificate-policy]

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich im folgenden Video, welche anderen Möglichkeiten zum Absichern von Back-End-Diensten es gibt, z. B. HTTP-Standardauthentifizierung oder Authentifizierung mit gemeinsamem geheimen Schlüssel.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Last-mile-Security/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[Hinzufügen von Operationen zu einer API]: api-management-howto-add-operations.md
[Hinzufügen und Veröffentlichen eines Produkts]: api-management-howto-add-products.md
[Überwachung und Analyse]: ../api-management-monitoring.md
[Hinzufügen von APIs zu einem Produkt]: api-management-howto-add-products.md#add-apis
[Veröffentlichen eines Produkts]: api-management-howto-add-products.md#publish-product
[Erste Schritte mit Azure API Management]: api-management-get-started.md
[API Management-Richtlinienreferenz]: api-management-policy-reference.md
[Cachingrichtlinien]: api-management-policy-reference.md#caching-policies
[Erstellen einer API Management-Dienstinstanz]: api-management-get-started.md#create-service-instance

[Azure API Management-REST-API-Zertifikatsentität]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Ausführliche Informationen zum Konfigurieren der Zertifikatauthentifizierung in Azure WebSites finden Sie in diesem Artikel.]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Voraussetzungen]: #prerequisites
[Hochladen eines Clientzertifikats]: #step1
[Löschen eines Clientzertifikats]: #step1a
[Konfigurieren einer API zum Verwenden eines Clientzertifikats für die Gatewayauthentifizierung]: #step2
[Testen der Konfiguration durch Aufrufen eines Vorgangs im Entwicklerportal]: #step3
[Nächste Schritte]: #next-steps






<!--HONumber=Nov16_HO3-->


