---
title: Verwalten von Benutzerkonten in Azure API Management | Microsoft Docs
description: Erfahren Sie, wie Sie Benutzer in Azure API Management erstellen und einladen.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 078abfa5-1e4f-4c9d-b9c7-a172bd19c1a2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: a7ff82a47b4e972db96929acb47fcce760b244b3
ms.openlocfilehash: eb2e260e71d9ebc3000b440d0413c7d557e15258


---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Verwalten von Benutzerkonten in Azure API Management
In API Management sind Entwickler die Benutzer der APIs, die Sie mit API Management verfügbar machen. Diese Anleitung beschreibt die Erstellung und Einladung von Entwicklern zur Nutzung Ihrer API und der Produkte, die Sie über die API Management-Instanz bereitgestellt haben. Informationen zum programmgesteuerten Verwalten von Benutzerkonten finden Sie in der Dokumentation zur [Benutzerentität](https://msdn.microsoft.com/library/azure/dn776330.aspx) in der [Referenz zu API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx).

## <a name="create-developer"> </a>Erstellen eines neuen Entwicklers
Klicken Sie im Azure-Portal für Ihren API Management-Dienst auf **Herausgeberportal**, um einen neuen Entwickler zu erstellen. Daraufhin gelangen Sie zum API Management-Herausgeberportal. Falls Sie noch keine API Management-Dienstinstanz erstellt haben, finden Sie weitere Informationen im Abschnitt [Erstellen einer API Management-Dienstinstanz][Create an API Management service instance] im Tutorial [Erste Schritte mit Azure API Management][Get started with Azure API Management].

![Herausgeberportal][api-management-management-console]

Klicken Sie im Menü **API Management** auf der linken Seite auf **Benutzer**, und klicken Sie anschließend auf **Benutzer hinzufügen**.

![Entwickler erstellen][api-management-create-developer]

Geben Sie **E-Mail**, **Kennwort** und **Name** für den neuen Entwickler ein, und klicken Sie auf **Speichern**.

![Entwickler erstellen][api-management-add-new-user]

Neu erstellte Entwicklerkonten sind standardmäßig **Aktiv** und werden der Gruppe **Entwickler** zugewiesen.

![Neuer Entwickler][api-management-new-developer]

Entwicklerkonten mit dem Status **Aktiv** können sämtliche APIs aufrufen, die sie abonniert haben. Informationen zum Zuordnen des neu erstellten Entwicklers zu weiteren Gruppen finden Sie unter [Zuordnen von Entwicklern zu Gruppen][How to associate groups with developers].

## <a name="invite-developer"> </a>Einladen eines Entwicklers
Klicken Sie im Menü **API Management** auf der linken Seite auf **Benutzer**, und klicken Sie anschließend auf **Benutzer einladen**, um einen Entwickler einzuladen.

![Entwickler einladen][api-management-invite-developer]

Geben Sie Name und E-Mail-Adresse des Entwicklers ein, und klicken Sie auf **Einladen**.

![Entwickler einladen][api-management-invite-developer-window]

Eine Bestätigungsmeldung wird angezeigt. Der neu eingeladene Entwickler wird jedoch erst in der Liste angezeigt, wenn die Einladung angenommen wurde. 

![Bestätigung der Einladung][api-management-invite-developer-confirmation]

Wenn Sie einen Entwickler einladen, wird eine E-Mail an diesen Entwickler geschickt. Diese E-Mail wird anhand einer Vorlage erstellt und ist konfigurierbar. Weitere Informationen finden Sie unter [Konfigurieren von E-Mail-Vorlagen][Configure email templates].

Nachdem die Einladung angenommen wurde, ist das Konto aktiv.

## <a name="block-developer"> </a> Deaktivieren oder erneutes Aktivieren eines Entwicklerkontos
Neu erstellte oder eingeladene Entwicklerkonten sind standardmäßig **Aktiv**. Klicken Sie auf **Blockieren**, um ein Entwicklerkonto zu deaktivieren. Klicken Sie auf **Aktivieren**, um ein blockiertes Entwicklerkonto erneut zu aktivieren. Blockierte Entwicklerkonten haben keinen Zugriff auf das Entwicklerportal und können keine APIs aufrufen. Klicken Sie zum Löschen eines Benutzerkontos auf **Löschen**.

![Entwickler blockieren][api-management-new-developer]

## <a name="reset-a-user-password"></a>Zurücksetzen des Benutzerkennworts
Klicken Sie zum Zurücksetzen des Kennworts für ein Benutzerkonto auf den Namen des Kontos.

![Kennwort zurücksetzen][api-management-view-developer]

Klicken Sie auf **Kennwort zurücksetzen** , um einen Link an den Benutzer zu senden, mit dem das Kennwort zurückgesetzt werden kann.

![Zurücksetzen des Kennworts][api-management-reset-password]

Informationen zur programmgesteuerten Verwendung von Benutzerkonten finden Sie in der Dokumentation zur [Benutzerentität](https://msdn.microsoft.com/library/azure/dn776330.aspx) in der [Referenz zu API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx). Um das Kennwort eines Benutzerkontos auf einen bestimmten Wert zurückzusetzen, können Sie den Vorgang [Aktualisieren eines Benutzers](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) verwenden und das gewünschte Kennwort angeben.

## <a name="pending-verification"></a>Ausstehende Bestätigung
![Ausstehende Bestätigung][api-management-pending-verification]

## <a name="next-steps"> </a>Nächste Schritte
Nachdem Sie das Entwicklerkonto erstellt haben, können Sie das Konto zu Rollen zuweisen und Produkte und APIs abonnieren. Weitere Informationen finden Sie unter [Erstellen und Verwenden von Gruppen][How to create and use groups].

[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png


[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates



<!--HONumber=Dec16_HO3-->


