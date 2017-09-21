---
title: "Einrichten von in Azure Active Directory eingebundenen Geräten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie in Azure Active Directory eingebundene Geräte einrichten."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2017
ms.author: markvi
ms.reviewer: jairoc
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: f548833cca27debb67cb155be0791299470f28dd
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="set-up-azure-active-directory-joined-devices"></a>Einrichten von in Azure Active Directory eingebundenen Geräten

Mit der Geräteverwaltung in Azure Active Directory (Azure AD) können Sie sicherstellen, dass Benutzer auf Ihre Ressourcen über Geräte zugreifen, die Ihren Standards für Sicherheit und Konformität entsprechen. Weitere Informationen finden Sie unter [Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md).

Sie können Windows 10-Geräte, die im Besitz Ihres Unternehmens sind, als in [Azure AD eingebundene Geräte](device-management-introduction.md#azure-ad-joined-devices) der Kontrolle von Azure AD unterstellen. Diese Geräte dürfen nicht bereits [in ein lokales AD eingebunden](device-management-introduction.md#hybrid-azure-ad-joined-devices) sein.

Dieses Thema enthält Anweisungen zum Registrieren von Windows 10-Geräten bei Azure AD. 

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, sollten Sie überprüfen, ob Folgendes zutrifft:

- Sie verfügen über Berechtigungen zum Einbinden von Geräten in Azure AD.

    ![Verbunden](./media/device-management-azuread-joined-devices-setup/21.png)

- Die maximale Anzahl von Geräten pro Benutzer wurde noch nicht überschritten. 

    ![Verbunden](./media/device-management-azuread-joined-devices-setup/22.png)


Weitere Informationen finden Sie unter [Configure device settings](device-management-azure-portal.md#configure-device-settings) (Konfigurieren von Geräteeinstellungen).



## <a name="what-you-should-know"></a>Wichtige Informationen


- Windows registriert das Gerät im Organisationsverzeichnis in Azure AD.

- Unter Umständen werden Sie zur mehrstufigen Authentifizierung aufgefordert. Diese kann vom IT-Administrator eingerichtet werden.

- Azure AD überprüft, ob ein Gerät bei der mobilen Geräteverwaltung registriert werden muss. Das Gerät wird ggf. von Azure AD registriert.

- Windows leitet verwaltete Benutzer über die automatische Anmeldung zum Desktop weiter.

- Verbundbenutzer werden zu einer Windows-Anmeldeseite weitergeleitet, um Anmeldeinformationen einzugeben.


## <a name="joining-a-device"></a>Einbinden eines Geräts

Dieser Abschnitt enthält die Schritte zum Einbinden Ihres Windows 10-Geräts in Azure AD. Wenn Sie Ihr Gerät in Azure AD eingebunden haben, wird im Dialogfeld **Auf Arbeits- oder Schulkonto zugreifen** der Eintrag **Verbunden mit \<Ihre Azure AD-Instanz\>** angezeigt.

![Verbunden](./media/device-management-azuread-joined-devices-setup/13.png)


**So binden Sie Ihr Windows 10-Gerät ein:**

1. Klicken Sie im Menü **Start** auf **Einstellungen**.

    ![Einstellungen](./media/device-management-azuread-joined-devices-setup/01.png)

2. Klicken Sie auf **Konten**.

    ![Konten](./media/device-management-azuread-joined-devices-setup/02.png)


3. Klicken Sie auf **Auf Arbeits- oder Schulkonto zugreifen**.

    ![Auf Arbeits- oder Schulkonto zugreifen](./media/device-management-azuread-joined-devices-setup/03.png)

4. Klicken Sie im Dialogfenster **Auf Arbeits- oder Schulkonto zugreifen** auf **Verbinden**.

    ![Verbinden](./media/device-management-azuread-joined-devices-setup/04.png)


5. Klicken Sie im Dialogfenster **Geschäfts-, Schul- oder Unikonto einrichten** auf **Dieses Gerät in Azure Active Directory einbinden**.

    ![Verbinden](./media/device-management-azuread-joined-devices-setup/08.png)


6. Geben Sie im Dialogfenster **Melden Sie sich an** Ihren Kontonamen (z.B. someone@example.com) ein, und klicken Sie dann auf **Weiter**.

    ![Melden Sie sich an](./media/device-management-azuread-joined-devices-setup/10.png)


6. Geben Sie im Dialogfenster **Kennwort eingeben** Ihr Kennwort ein, und klicken Sie dann auf **Anmelden**.

    ![Kennwort eingeben](./media/device-management-azuread-joined-devices-setup/05.png)


7. Geben Sie im Dialogfenster **Stellen Sie sicher, dass dies Ihr Unternehmen ist.** auf **Beitreten**.

    ![Stellen Sie sicher, dass dies Ihr Unternehmen ist.](./media/device-management-azuread-joined-devices-setup/11.png)


8. Klicken Sie im Dialogfenster **Alles erledigt** auf **Fertig**.

    ![Alles erledigt](./media/device-management-azuread-joined-devices-setup/12.png)

## <a name="verification"></a>Überprüfung

Ob ein Gerät in eine Azure AD-Instanz eingebunden wurde, können Sie im Dialogfenster **Auf Arbeits- oder Schulkonto zugreifen** Ihres Geräts überprüfen.

![Verbunden](./media/device-management-azuread-joined-devices-setup/13.png)

Alternativ können Sie den folgenden Befehl ausführen: `dsregcmd /status`.  
Auf einem erfolgreich eingebundenen Gerät wird **Yes** für **AzureAdJoined** angezeigt.

![Verbunden](./media/device-management-azuread-joined-devices-setup/14.png)

Sie können die Geräteeinstellungen auch im Azure AD-Portal überprüfen.

![Verbunden](./media/device-management-azuread-joined-devices-setup/15.png)

Weitere Informationen finden Sie unter [Locate devices](device-management-azure-portal.md#locate-devices) (Geräte ermitteln).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter: 

- [Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md)

- [Verwalten von Geräten mithilfe des Azure-Portals – Vorschau](device-management-azure-portal.md)





