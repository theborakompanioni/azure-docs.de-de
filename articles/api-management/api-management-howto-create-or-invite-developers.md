---
title: Verwalten von Benutzerkonten in Azure API Management | Microsoft Docs
description: Erfahren Sie, wie Sie Benutzer in Azure API Management erstellen und einladen.
services: api-management
documentationcenter: ''
author: steved0x
manager: erikre
editor: ''

ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2016
ms.author: sdanie

---
# Verwalten von Benutzerkonten in Azure API Management
In API Management sind Entwickler die Benutzer der APIs, die Sie mit API Management verfügbar machen. Diese Anleitung beschreibt die Erstellung und Einladung von Entwicklern zur Nutzung Ihrer API und der Produkte, die Sie über die API Management-Instanz bereitgestellt haben. Informationen zum programmgesteuerten Verwalten von Benutzerkonten finden Sie in der Dokumentation zur [Benutzerentität](https://msdn.microsoft.com/library/azure/dn776330.aspx) in der Referenz zu [REST-API-Management](https://msdn.microsoft.com/library/azure/dn776326.aspx).

## <a name="create-developer"> </a>Erstellen eines neuen Entwicklers
Klicken Sie im klassischen Azure-Portal für Ihren API Management-Dienst auf **Verwalten**, um einen neuen Entwickler zu erstellen. Daraufhin gelangen Sie zum API Management-Herausgeberportal. Falls Sie noch keine API Management-Dienstinstanz erstellt haben, finden Sie weitere Informationen im Abschnitt [Erstellen einer API Management-Dienstinstanz][Erstellen einer API Management-Dienstinstanz] im Lernprogramm [Erste Schritte mit Azure API Management][Erste Schritte mit Azure API Management].

![Herausgeberportal][api-management-management-console]

Klicken Sie auf **Benutzer** im Menü **API Management** auf der linken Seite, und klicken Sie anschließend auf **Benutzer hinzufügen**.

![Entwickler erstellen][api-management-create-developer]

Geben Sie **E-Mail**, **Kennwort** und **Name** für den neuen Entwickler ein, und klicken Sie auf **Speichern**.

![Entwickler erstellen][api-management-add-new-user]

Neu erstellte Entwicklerkonten sind standardmäßig **Aktiv** und werden zur Gruppe **Entwickler** zugewiesen.

![Neuer Entwickler][api-management-new-developer]

Entwicklerkonten mit dem Status **Aktiv** können sämtliche APIs aufrufen, die sie abonniert haben. Informationen zum Zuordnen des neu erstellten Entwicklers zu weiteren Gruppen finden Sie unter [Zuordnen von Entwicklern zu Gruppen][Zuordnen von Entwicklern zu Gruppen].

## <a name="invite-developer"> </a>Einladen eines Entwicklers
Klicken Sie auf **Benutzer** im Menü **API Management** auf der linken Seite, und klicken Sie anschließend auf **Benutzer einladen**, um einen Entwickler einzuladen.

![Entwickler einladen][api-management-invite-developer]

Geben Sie Name und E-Mail-Adresse des Entwicklers ein, und klicken Sie auf **Einladen**.

![Entwickler einladen][api-management-invite-developer-window]

Eine Bestätigungsmeldung wird angezeigt. Der neu eingeladene Entwickler wird jedoch erst in der Liste angezeigt, wenn die Einladung angenommen wurde.

![Bestätigung der Einladung][api-management-invite-developer-confirmation]

Wenn Sie einen Entwickler einladen, wird eine E-Mail an diesen Entwickler geschickt. Diese E-Mail wird anhand einer Vorlage erstellt und ist konfigurierbar. Weitere Informationen finden Sie unter [E-Mail-Vorlagen konfigurieren][E-Mail-Vorlagen konfigurieren].

Nachdem die Einladung angenommen wurde, ist das Konto aktiv.

## <a name="block-developer"> </a>Deaktivieren oder erneutes Aktivieren eines Entwicklerkontos
Neu erstellte oder eingeladene Entwicklerkonten sind standardmäßig **Aktiv**. Klicken Sie auf **Blockieren**, um ein Entwicklerkonto zu deaktivieren. Klicken Sie auf **Aktivieren**, um ein blockiertes Entwicklerkonto erneut zu aktivieren. Blockierte Entwicklerkonten haben keinen Zugriff auf das Entwicklerportal und können keine APIs aufrufen. Klicken Sie zum Löschen eines Benutzerkontos auf **Löschen**.

![Entwickler blockieren][api-management-new-developer]

## Zurücksetzen des Benutzerkennworts
Klicken Sie zum Zurücksetzen des Kennworts für ein Benutzerkonto auf den Namen des Kontos.

![Kennwort zurücksetzen][api-management-view-developer]

Klicken Sie auf **Kennwort zurücksetzen**, um einen Link an den Benutzer zu senden, mit dem das Kennwort zurückgesetzt werden kann.

![Kennwort zurücksetzen][api-management-reset-password]

Informationen zur programmgesteuerten Verwendung von Benutzerkonten finden Sie in der Dokumentation zur [Benutzerentität](https://msdn.microsoft.com/library/azure/dn776330.aspx) in der Referenz zu [REST-API-Management](https://msdn.microsoft.com/library/azure/dn776326.aspx). Um das Kennwort eines Benutzerkontos auf einen bestimmten Wert zurückzusetzen, können Sie den Vorgang [Aktualisieren eines Benutzers](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) verwenden und das gewünschte Kennwort angeben.

## Ausstehende Bestätigung
![Ausstehende Bestätigung][api-management-pending-verification]

## <a name="next-steps"> </a>Nächste Schritte
Nachdem Sie das Entwicklerkonto erstellt haben, können Sie das Konto zu Rollen zuweisen und Produkte und APIs abonnieren. Weitere Informationen finden Sie unter [Erstellen und Verwenden von Gruppen][Erstellen und Verwenden von Gruppen].

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
[]: ./media/api-management-howto-create-or-invite-developers/.png

[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[Erstellen und Verwenden von Gruppen]: api-management-howto-create-groups.md
[Zuordnen von Entwicklern zu Gruppen]: api-management-howto-create-groups.md#associate-group-developer

[Erste Schritte mit Azure API Management]: api-management-get-started.md
[Erstellen einer API Management-Dienstinstanz]: api-management-get-started.md#create-service-instance
[E-Mail-Vorlagen konfigurieren]: api-management-howto-configure-notifications.md#email-templates

<!---HONumber=AcomDC_0810_2016-->
