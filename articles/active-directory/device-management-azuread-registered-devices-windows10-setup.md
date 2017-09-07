---
title: "Konfigurieren von bei Azure Active Directory registrierten Geräten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie bei Azure Active Directory registrierte Geräte konfigurieren."
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
ms.date: 08/27/2017
ms.author: markvi
ms.reviewer: jairoc
ms.translationtype: HT
ms.sourcegitcommit: 48dfc0fa4c9ad28c4c64c96ae2fc8a16cd63865c
ms.openlocfilehash: d73062a258f05a630692670363cabf0080aba1ab
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="how-to-configure-azure-active-directory-registered-windows-10-devices"></a>Konfigurieren von bei Azure Active Directory registrierten Windows 10-Geräten

Mit der Geräteverwaltung in Azure Active Directory (Azure AD) können Sie sicherstellen, dass Benutzer auf Ihre Ressourcen über Geräte zugreifen, die Ihren Standards für Sicherheit und Konformität entsprechen. Weitere Informationen finden Sie unter [Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md).

Wenn Sie **Bring Your Own Device (BYOD)** ermöglichen möchten, konfigurieren Sie dazu bei Azure AD registrierte Geräte. In Azure AD können Sie bei Azure AD registrierte Geräte für Windows 10, iOS, Android und macOS konfigurieren. Die entsprechenden Schritte für Windows 10-Geräte werden in diesem Thema beschrieben. 


## <a name="before-you-begin"></a>Voraussetzungen

Um ein Windows 10-Gerät zu registrieren, muss der Geräteregistrierungsdienst so konfiguriert sein, dass Sie Geräte registrieren können. Sie benötigen Berechtigungen zum Registrieren von Geräten beim Azure AD-Mandanten. Darüber hinaus müssen weniger Geräte registriert sein als die konfigurierte maximale Anzahl. Weitere Informationen finden Sie unter [Configure device settings](device-management-azure-portal.md#configure-device-settings) (Konfigurieren von Geräteeinstellungen).

## <a name="what-you-should-know"></a>Wichtige Informationen

Berücksichtigen Sie beim Registrieren eines Geräts Folgendes:

- Windows registriert das Gerät im Organisationsverzeichnis in Azure AD.

- Unter Umständen werden Sie zur mehrstufigen Authentifizierung aufgefordert. Diese Aufforderung kann vom IT-Administrator konfiguriert werden.

- Azure AD prüft, ob eine Registrierung des Geräts in der Mobilgeräteverwaltung erforderlich ist, und registriert es ggf.

- Wenn Sie ein verwalteter Benutzer sind, führt Windows Sie über die automatische Anmeldung auf den Desktop.

- Wenn Sie ein Partnerbenutzer sind, werden Sie zu einem Windows-Anmeldebildschirm geführt und müssen Ihre Anmeldeinformationen eingeben.


## <a name="registering-a-device"></a>Registrieren eines Geräts

Dieser Abschnitt enthält die Schritte zum Registrieren Ihres Windows 10-Geräts bei Azure AD. Wenn Sie Ihr Gerät in Azure AD registriert haben, wird im Dialogfeld **Auf Arbeits- oder Schulkonto zugreifen** der Eintrag **Geschäfts-, Schul- oder Unikonto** angezeigt.

![Registrieren](./media/device-management-azuread-registered-devices-windows10-setup/08.png)


**So registrieren Sie Ihr Windows 10-Gerät:**

1. Klicken Sie im Menü **Start** auf **Einstellungen**.

    ![Einstellungen](./media/device-management-azuread-registered-devices-windows10-setup/01.png)

2. Klicken Sie auf **Konten**.

    ![Konten](./media/device-management-azuread-registered-devices-windows10-setup/02.png)


3. Klicken Sie auf **Auf Arbeits- oder Schulkonto zugreifen**.

    ![Auf Arbeits- oder Schulkonto zugreifen](./media/device-management-azuread-registered-devices-windows10-setup/03.png)

4. Klicken Sie im Dialogfenster **Auf Arbeits- oder Schulkonto zugreifen** auf **Verbinden**.

    ![Verbinden](./media/device-management-azuread-registered-devices-windows10-setup/04.png)


5. Geben Sie im Dialogfenster **Geschäfts-, Schul- oder Unikonto einrichten** den Kontonamen (z.B. someone@example.com) ein, und klicken Sie auf **Weiter**.

    ![Verbinden](./media/device-management-azuread-registered-devices-windows10-setup/06.png)


6. Geben Sie im Dialogfenster **Kennwort eingeben** Ihr Kennwort ein, und klicken Sie dann auf **Weiter**.

    ![Verbinden](./media/device-management-azuread-registered-devices-windows10-setup/05.png)


7. Klicken Sie im Dialogfenster **Alles erledigt** auf **Fertig**.

    ![Verbinden](./media/device-management-azuread-registered-devices-windows10-setup/07.png)

## <a name="verification"></a>Überprüfung

Ob ein Gerät in eine Azure AD-Instanz eingebunden wurde, können Sie im Dialogfenster **Auf Arbeits- oder Schulkonto zugreifen** Ihres Geräts überprüfen.

![Registrieren](./media/device-management-azuread-registered-devices-windows10-setup/08.png)

Alternativ können Sie die Geräteeinstellungen auch im Azure AD-Portal überprüfen.

![Registrieren](./media/device-management-azuread-registered-devices-windows10-setup/09.png)





## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md).

- Ausführliche Informationen zum Verwalten von Geräten im Azure AD-Portal finden Sie unter [Verwalten von Geräten mithilfe des Azure-Portals – Vorschau](device-management-azure-portal.md).





