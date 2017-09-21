---
title: "Einrichten von bei Azure Active Directory registrierten Windows 10-Geräten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie bei Azure Active Directory registrierte Windows 10-Geräte einrichten."
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
ms.date: 09/14/2017
ms.author: markvi
ms.reviewer: jairoc
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: e2554450bdb426cff50f302132158f39f5630ee3
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="set-up-azure-active-directory-registered-windows-10-devices"></a>Einrichten von bei Azure Active Directory registrierten Windows 10-Geräten

Mit der Geräteverwaltung in Azure Active Directory (Azure AD) können Sie sicherstellen, dass Benutzer auf Ihre Ressourcen über Geräte zugreifen, die Ihren Standards für Sicherheit und Konformität entsprechen. Weitere Informationen finden Sie unter [Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md).

Mit bei [Azure AD registrierten Geräten](device-management-introduction.md#azure-ad-registered-devices) ermöglichen Sie das **Bring Your Own Device (BYOD)**-Szenario, in dem die Benutzer mit Geräten in ihrem privaten Besitz auf Ressourcen Ihrer Organisation zugreifen können.  

In Azure AD können Sie bei Azure AD registrierte Geräte mit folgenden Betriebssystemen einrichten:

- Windows 10
- iOS
- Android
- macOS  

Dieses Thema enthält Anweisungen zum Registrieren von Windows 10-Geräten bei Azure AD. 


## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, sollten Sie überprüfen, ob Folgendes zutrifft:

- Sie verfügen über die Berechtigungen zum Registrieren von Geräten. 

    ![Registrieren](./media/device-management-azuread-registered-devices-windows10-setup/21.png)

- Die maximale Anzahl von Geräten pro Benutzer wurde noch nicht überschritten. 

    ![Registrieren](./media/device-management-azuread-registered-devices-windows10-setup/22.png)

Weitere Informationen finden Sie unter [Konfigurieren der Geräteeinstellungen](device-management-azure-portal.md#configure-device-settings).

## <a name="what-you-should-know"></a>Wichtige Informationen

Wenn Sie ein Gerät registrieren, beachten Sie Folgendes:

- Windows registriert das Gerät im Organisationsverzeichnis in Azure AD.

- Unter Umständen werden Sie zur mehrstufigen Authentifizierung aufgefordert. Diese kann vom IT-Administrator eingerichtet werden.

- Azure AD überprüft, ob ein Gerät bei der mobilen Geräteverwaltung registriert werden muss. Das Gerät wird ggf. von Azure AD registriert.

- Windows leitet verwaltete Benutzer über die automatische Anmeldung zum Desktop weiter.

- Verbundbenutzer werden zu einer Windows-Anmeldeseite weitergeleitet, um Anmeldeinformationen einzugeben.


## <a name="register-a-device"></a>Registrieren eines Geräts

Führen Sie die folgenden Schritte aus, um Ihr Windows 10-Gerät bei Azure AD zu registrieren. Wenn Sie Ihr Gerät bei Azure AD registriert haben, wird auf der Seite **Auf Arbeits- oder Schulkonto zugreifen** der Eintrag **Geschäfts-, Schul- oder Unikonto** angezeigt.

![Registrieren](./media/device-management-azuread-registered-devices-windows10-setup/08.png)


So registrieren Sie Ihr Windows 10-Gerät:

1. Klicken Sie im Menü **Start** auf **Einstellungen**.

    ![„Einstellungen“ auswählen](./media/device-management-azuread-registered-devices-windows10-setup/01.png)

2. Klicken Sie auf **Konten**.

    ![„Konten“ auswählen](./media/device-management-azuread-registered-devices-windows10-setup/02.png)


3. Klicken Sie auf **Auf Arbeits- oder Schulkonto zugreifen**.

    ![„Auf Arbeits- oder Schulkonto zugreifen“ auswählen](./media/device-management-azuread-registered-devices-windows10-setup/03.png)

4. Klicken Sie auf der Seite **Auf Arbeits- oder Schulkonto zugreifen** auf **Verbinden**.

    ![Seite „Auf Arbeits- oder Schulkonto zugreifen“](./media/device-management-azuread-registered-devices-windows10-setup/04.png)


5. Geben Sie auf der Seite **Geschäfts-, Schul- oder Unikonto einrichten** den Namen Ihres Kontos (z.B. someone@example.com) ein, und klicken Sie auf **Weiter**.

    ![Seite „Geschäfts-, Schul- oder Unikonto einrichten“](./media/device-management-azuread-registered-devices-windows10-setup/06.png)


6. Geben Sie auf der Seite **Kennwort eingeben** Ihr Kennwort ein, und klicken Sie dann auf **Weiter**.

    ![Kennwort eingeben](./media/device-management-azuread-registered-devices-windows10-setup/05.png)


7. Klicken Sie auf der Seite **Alles erledigt** auf **Fertig**.

    ![Seite „Alles erledigt“](./media/device-management-azuread-registered-devices-windows10-setup/07.png)

## <a name="verification"></a>Überprüfung

Ob ein Gerät in Azure AD eingebunden wurde, können Sie auf der Seite **Auf Arbeits- oder Schulkonto zugreifen** Ihres Geräts überprüfen.

![Status „Geschäfts- oder Schulkonto“](./media/device-management-azuread-registered-devices-windows10-setup/08.png)

Alternativ können Sie die Geräteeinstellungen im Azure AD-Portal überprüfen.

![Bei Azure AD registrierte Geräte](./media/device-management-azuread-registered-devices-windows10-setup/09.png)


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter: 

- [Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md)

- [Verwalten von Geräten mithilfe des Azure-Portals – Vorschau](device-management-azure-portal.md)





