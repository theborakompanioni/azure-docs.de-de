---
title: "Benutzerdefinierte Domänen im Azure AD-Anwendungsproxy | Microsoft-Dokumentation"
description: "Verwalten Sie benutzerdefinierte Domänen im Azure AD-Anwendungsproxy, sodass die URL für die App immer die gleiche ist – unabhängig davon, wo Ihre Benutzer darauf zugreifen."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 22aa82e5cbce5b00f733f72209318c901079b665
ms.openlocfilehash: d0abd789d47b34f3dad206b9862c289d6683a83b
ms.contentlocale: de-de
ms.lasthandoff: 07/24/2017

---

# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Arbeiten mit benutzerdefinierten Domänen im Azure AD-Anwendungsproxy

Wenn Sie eine Anwendung über den Azure Active Directory-Anwendungsproxy veröffentlichen, erstellen Sie eine externe URL, die Ihre Benutzer bei der Remotearbeit verwenden können. Mit dieser URL wird die Standarddomäne *yourtenant-msappproxy.net* aufgerufen. Konfigurieren Sie eine benutzerdefinierte Domäne für Ihre Anwendung, wenn Sie Ihren eigenen Domänennamen verwenden möchten. 

Wir empfehlen Ihnen, nach Möglichkeit immer benutzerdefinierte Domänen für Ihre Anwendungen einzurichten. Einige Vorteile von benutzerdefinierten Domänen sind:

- Ihre Benutzer können unabhängig davon, ob sie innerhalb oder außerhalb Ihres Netzwerks arbeiten, mit derselben URL auf die Anwendung zugreifen.
- Wenn alle Anwendungen über die gleichen internen und externen URLs verfügen, funktionieren Links in einer Anwendung, die auf eine andere verweisen, auch außerhalb des Unternehmensnetzwerks. 
- Sie steuern Ihr Branding und erstellen die gewünschten URLs. 


## <a name="configure-a-custom-domain"></a>Konfigurieren einer benutzerdefinierten Domäne

Stellen Sie vor dem Konfigurieren einer benutzerdefinierten Domäne sicher, dass Sie die folgenden Anforderungen erfüllt haben: 
- Sie haben [Azure Active Directory eine überprüfte Domäne hinzugefügt](active-directory-domains-add-azure-portal.md).
- Es ist ein benutzerdefiniertes Zertifikat für die Domäne in Form einer PFX-Datei vorhanden. 
- Es ist eine lokale App vorhanden, [die über den Anwendungsproxy veröffentlicht wurde](application-proxy-publish-azure-portal.md).

Wenn diese drei Anforderungen erfüllt sind, können Sie die folgenden Schritte ausführen, um Ihre benutzerdefinierte Domäne einzurichten:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Navigieren Sie zu **Azure Active Directory** > **Unternehmensanwendungen** > **Alle Anwendungen**, und wählen Sie die App aus, die Sie verwalten möchten.
3. Wählen Sie **Anwendungsproxy**. 
4. Verwenden Sie im Feld für die „Externe URL“ die Dropdownliste, um Ihre benutzerdefinierte Domäne auszuwählen. Wenn Ihre Domäne in der Liste nicht angezeigt wird, wurde sie noch nicht überprüft. 
5. Das Feld **Zertifikat**, das deaktiviert war, wird aktiviert. Wählen Sie dieses Feld aus. 

   ![Klicken zum Hochladen eines Zertifikats](./media/active-directory-application-proxy-custom-domains/certificate.png)

   Wenn dieses Feld deaktiviert bleibt, bedeutet dies wahrscheinlich, dass für die Domäne bereits ein Zertifikat hochgeladen wurde. 

6. Laden Sie das PFX-Zertifikat hoch, und geben Sie das Kennwort für das Zertifikat ein. 
7. Wählen Sie **Speichern**, um Ihre Änderungen zu speichern. 
8. Fügen Sie einen DNS-Eintrag hinzu, mit dem die neue externe URL an die Domäne „msappproxy.net“ geleitet wird. 

>[!TIP] 
>Sie müssen nur ein Zertifikat pro benutzerdefinierter Domäne hochladen. Nachdem Sie ein Zertifikat hochgeladen haben, können Sie die benutzerdefinierte Domäne beim Veröffentlichen einer neuen App auswählen. Mit Ausnahme des DNS-Eintrags sind keine weiteren Konfigurationsschritte erforderlich. 

## <a name="manage-certificates"></a>Verwalten von Zertifikaten

### <a name="certificate-format"></a>Zertifikatformat
Es gibt keine Einschränkung für die Zertifikatsignaturmethoden. ECC, SAN und andere allgemeine Zertifikattypen werden unterstützt. Sie können auch Platzhalterzertifikate verwenden. Stellen Sie beim Verwenden eines Platzhalterzertifikats sicher, dass der Platzhalter mit der gewünschten externen URL übereinstimmt. Auch selbstsignierte Zertifikate werden akzeptiert. Wenn Sie eine private Zertifizierungsstelle verwenden, sollte der Verteilungspunkt für die Zertifikatssperrliste (CDP) für das Zertifikat öffentlich sein.

### <a name="changing-the-domain"></a>Ändern der Domäne
Alle überprüften Domänen werden in der Dropdownliste „Externe URL“ Ihrer Anwendung angezeigt. Sie können einfach dieses Feld für die Anwendung aktualisieren, um die Domäne zu ändern. Wenn Sie eine Domäne auswählen, der kein Zertifikat zugeordnet ist, können Sie die Schritte 5 bis 7 ausführen, um das Zertifikat hinzuzufügen. [Fügen Sie die gewünschte Domäne als überprüfte Domäne hinzu](active-directory-domains-add-azure-portal.md), falls sie nicht in der Liste enthalten ist. Stellen Sie anschließend sicher, dass Sie den DNS-Eintrag aktualisieren, damit die Umleitung von der neuen externen URL durchgeführt wird. 

### <a name="certificate-management"></a>Zertifikatverwaltung
Sie können dasselbe Zertifikat für mehrere Anwendungen verwenden, sofern für die Anwendungen nicht ein gemeinsamer externer Host genutzt wird. 

Wenn ein Zertifikat abläuft, erhalten Sie eine Warnung mit der Aufforderung, über das Portal ein anderes Zertifikat hochzuladen. Wenn das Zertifikat widerrufen wird, wird für Ihre Benutzer beim Zugreifen auf die Anwendung ggf. eine Sicherheitswarnung angezeigt. Wir führen für Zertifikate keine Widerrufsprüfungen durch.  Navigieren Sie zum Aktualisieren des Zertifikats für eine bestimmte Anwendung zu dieser Anwendung, und führen Sie die Schritte 5 bis 7 für die Konfiguration der benutzerdefinierten Domänen von veröffentlichten Anwendungen aus, um ein neues Zertifikat hochzuladen. Wenn das alte Zertifikat von anderen Anwendungen nicht verwendet wird, wird es automatisch gelöscht. 

Derzeit wird die gesamte Zertifikatverwaltung über einzelne Anwendungsseiten durchgeführt, sodass Sie Zertifikate im Kontext der relevanten Anwendungen verwalten müssen. 

## <a name="next-steps"></a>Nächste Schritte
* [Aktivieren des einmaligen Anmeldens](active-directory-application-proxy-sso-using-kcd.md) bei Ihren veröffentlichten Apps mithilfe der Azure AD-Authentifizierung.
* [Aktivieren des bedingten Zugriffs](active-directory-application-proxy-conditional-access.md) auf Ihre veröffentlichten Apps.
* [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](active-directory-domains-add-azure-portal.md)



