---
title: Zugreifen auf Azure AD-App-Proxy-Apps in Teams | Microsoft-Dokumentation
description: "Verwenden Sie den Azure AD-Anwendungsproxy, um über Microsoft Teams auf Ihre lokale Anwendung zuzugreifen."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 818f7756189ed4ceefdac9114a0b89ef9ee8fb7a
ms.openlocfilehash: 24e22d7314de536714a825cd7035d2cec2112278
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---

# <a name="access-your-on-premises-applications-through-microsoft-teams"></a>Zugreifen auf lokale Anwendungen über Microsoft Teams

Der Azure Active Directory-Anwendungsproxy ermöglicht ortsunabhängiges einmaliges Anmelden bei lokalen Anwendungen, und Microsoft Teams optimiert die Zusammenarbeit an einem zentralen Ort. Durch die Integration beider Lösungen können Benutzer in jeder Situation produktiv mit ihren Teamkollegen zusammenarbeiten. 

Benutzer können [mithilfe von Registerkarten](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US) Cloud-Apps zu ihren Teams-Kanälen hinzufügen. Aber was, wenn die von allen verwendete SharePoint-Website oder das von allen verwendete Planungstool lokal gehostet wird? Dieses Problem wird durch den Anwendungsproxy gelöst. Benutzer können Ihren Kanälen Apps hinzufügen, die über den Anwendungsproxy veröffentlicht wurden. Dabei können sie die gleichen externen URLs verwenden wie beim Remotezugriff auf ihre Apps. Und da die Authentifizierung des Anwendungsproxys über Azure Active Directory erfolgt, läuft auch das einmalige Anmelden auf die gleiche Weise ab.


## <a name="install-the-application-proxy-connector-and-publish-your-app"></a>Installieren des Anwendungsproxyconnectors und Veröffentlichen der App

[Konfigurieren Sie den Anwendungsproxy für Ihren Mandanten, und installieren Sie den Connector](active-directory-application-proxy-enable.md) (sofern noch nicht geschehen). [Veröffentlichen Sie Ihre lokale Anwendung](application-proxy-publish-azure-portal.md), um Remotezugriff zu ermöglichen. Wenn Sie die App veröffentlichen, notieren Sie sich die externe URL, da die Endbenutzer diese Information benötigen, wenn sie die App zu Teams hinzufügen.

Falls Sie Ihre Apps bereits veröffentlicht haben und sich nicht mehr an die externen URLs erinnern, können Sie sie im [Azure-Portal](https://portal.azure.com) nachschauen. Melden Sie sich an, navigieren Sie zu **Azure Active Directory** > **Unternehmensanwendungen** > **Alle Anwendungen**, wählen Sie Ihre App aus, und wählen Sie schließlich **Anwendungsproxy** aus.

## <a name="add-your-app-to-teams"></a>Hinzufügen Ihrer App zu Teams

Nachdem Sie die App über den Anwendungsproxy veröffentlicht haben, teilen Sie Ihren Benutzer mit, dass sie sie ihren Teams-Kanälen direkt als Registerkarte hinzufügen können. Dazu müssen die Benutzer die folgenden drei Schritte ausführen:

1. Navigieren Sie zu dem Teams-Kanal, unter dem Sie die App hinzufügen möchten, und wählen Sie **+** aus, um eine Registerkarte hinzuzufügen.

   ![Auswählen des Symbols zum Hinzufügen einer Registerkarte](./media/application-proxy-teams/add-tab.png)

2. Wählen Sie für die Registerkarte die Option **Website** aus.

   ![Hinzufügen einer Website](./media/application-proxy-teams/website.png)

3. Benennen Sie die Registerkarte, und legen Sie die URL auf die externe Anwendungsproxy-URL fest. 

   ![Konfigurieren von Name und URL der Registerkarte](./media/application-proxy-teams/tab-name-url.png)

Nachdem ein Mitglied eines Teams die Registerkarte hinzugefügt hat, wird sie für alle Benutzer des Kanals angezeigt. Alle Benutzer mit Zugriff auf die App können beim Zugreifen auf die App einmaliges Anmelden mit den Anmeldeinformationen nutzen, die sie auch für Microsoft Teams verwenden. Benutzern, die keinen Zugriff auf die App haben, wird die Registerkarte zwar in Teams angezeigt, sie können aber nur darauf zugreifen, wenn Sie ihnen Berechtigungen für die lokale App sowie für die über das Azure-Portal veröffentlichte Version der App erteilen. 

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Veröffentlichen lokaler SharePoint-Websites](application-proxy-enable-remote-access-sharepoint.md) mit dem Anwendungsproxy.
- Konfigurieren Sie Ihre Apps für die Verwendung von [benutzerdefinierten Domänen](active-directory-application-proxy-custom-domains.md) für die externe URL. 

