---
title: "Einbinden eines neuen Windows 10-Geräts in Azure AD auf der Windows-Willkommensseite | Microsoft-Dokumentation"
description: "In diesem Thema wird erklärt, wie Benutzer die Azure AD-Einbindung auf der Windows-Willkommensseite einrichten können."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/27/2017
ms.author: markvi
ms.reviewer: jairoc
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: b07386eeb0d3e05a0b4545be39030066fbdafd3d
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="join-a-new-windows-10-device-with-azure-ad-during-a-first-run"></a>Einbinden eines neuen Windows 10-Geräts in Azure AD auf der Windows-Willkommensseite

Mit der Geräteverwaltung in Azure Active Directory (Azure AD) können Sie sicherstellen, dass Benutzer auf Ihre Ressourcen über Geräte zugreifen, die Ihren Standards für Sicherheit und Konformität entsprechen. Weitere Informationen finden Sie unter [Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md).

Mit Windows 10 können Sie auf der Windows-Willkommensseite ein neues Gerät in Azure AD einbinden.  
Dadurch können Sie eingeschweißte Geräte an Ihre Mitarbeiter oder Schüler/Studenten verteilen.

Wenn Windows 10 Professional oder Windows 10 Enterprise auf einem Gerät installiert ist, wird standardmäßig der Setupprozess für firmeneigene Geräte übernommen.

Auf der *Windows-Willkommensseite* wird das Verknüpfen einer lokalen Active Directory-Domäne (AD) nicht unterstützt. Wenn Sie einen Computer mit einer AD-Domäne verknüpfen möchten, klicken Sie während des Setups auf den Link **Windows mit einem lokalen Konto einrichten**. Sie können dann die Domäne über die Einstellungen Ihres Computers verknüpfen.
 


## <a name="before-you-begin"></a>Voraussetzungen

Um ein Windows 10-Gerät einzubinden, muss der Geräteregistrierungsdienst so konfiguriert sein, dass Sie Geräte registrieren können. Sie benötigen Berechtigungen zum Einbinden von Geräten in den Azure AD-Mandanten. Darüber hinaus müssen weniger Geräte registriert sein als die konfigurierte maximale Anzahl. Weitere Informationen finden Sie unter [Configure device settings](device-management-azure-portal.md#configure-device-settings) (Konfigurieren von Geräteeinstellungen).

## <a name="joining-a-device"></a>Einbinden eines Geräts

**So binden Sie ein Windows 10-Gerät auf der Windows-Willkommensseite in Azure AD ein:**


1. Wenn Sie das neue Gerät einschalten und den Setupprozess starten, sollte die Meldung **Vorbereitung** angezeigt werden. Befolgen Sie die Anweisungen, um Ihr Gerät einzurichten.

2. Passen Sie zuerst die Region und die Sprache an. Akzeptieren Sie anschließend die Microsoft-Software-Lizenzbedingungen.
 
    ![Für Ihre Region anpassen](./media/device-management-azuread-joined-devices-frx/01.png)

3. Wählen Sie das Netzwerk aus, das Sie für die Verbindung mit dem Internet verwenden möchten.

4. Klicken Sie auf **Dieses Gerät gehört meiner Organisation**. 

    ![Bildschirm „Wem gehört dieser PC?“](./media/device-management-azuread-joined-devices-frx/02.png)

5. Geben Sie die Anmeldeinformationen ein, die von Ihrer Organisation bereitgestellt wurden, und klicken Sie auf **Anmelden**.

    ![Anmeldebildschirm](./media/device-management-azuread-joined-devices-frx/03.png)

6. Ihr Gerät ermittelt einen übereinstimmenden Mandanten in Azure AD. Wenn Sie in einer Verbunddomäne sind, werden Sie an Ihren lokalen Sicherheitstokendienst-Server (STS) umgeleitet, beispielsweise Active Directory-Verbunddienste (AD FS).

7. Wenn Sie ein Benutzer in einer nicht verbundenen Domäne sind, geben Sie Ihre Anmeldeinformationen direkt auf der von Azure AD gehosteten Seite ein. 

8. Sie werden zu einer mehrstufigen Authentifizierung aufgefordert. 
 
9. Azure AD prüft, ob eine Anmeldung zur mobilen Geräteverwaltung erforderlich ist.

10. Windows registriert das Gerät im Verzeichnis der Organisation in Azure AD und in der Mobilgeräteverwaltung (falls zutreffend).

11. Wenn Sie…
    - ein verwalteter Benutzer sind, leitet Windows Sie über den automatischen Anmeldeprozess zum Desktop weiter.

    - ein Verbundbenutzer sind, werden Sie zum Windows-Anmeldebildschirm weitergeleitet und müssen Ihre Anmeldeinformationen eingeben.

## <a name="verification"></a>Überprüfung

Ob ein Gerät in Ihre Azure AD-Instanz eingebunden wurde, können Sie im Dialogfenster **Auf Arbeits- oder Schulkonto zugreifen** Ihres Windows-Geräts überprüfen. Im Dialogfenster sollte angegeben sein, dass Sie mit dem Azure AD-Verzeichnis verbunden sind.

![Auf Arbeits- oder Schulkonto zugreifen](./media/device-management-azuread-joined-devices-frx/13.png)


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md).

- Ausführliche Informationen zum Verwalten von Geräten im Azure AD-Portal finden Sie unter [Verwalten von Geräten mithilfe des Azure-Portals – Vorschau](device-management-azure-portal.md).

