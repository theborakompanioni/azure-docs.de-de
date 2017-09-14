---
title: "Bedingter Azure Active Directory-Zugriff für Verbindungen über ein virtuelles privates Netzwerk (Vorschauversion) | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie der bedingte Azure Active Directory-Zugriff für Verbindungen über ein virtuelles privates Netzwerk funktioniert. "
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/01/2017
ms.author: markvi
ms.reviewer: jairoc
ms.translationtype: HT
ms.sourcegitcommit: a16daa1f320516a771f32cf30fca6f823076aa96
ms.openlocfilehash: 540d8974ee2c02f80bccf28764b4d0d243e98d85
ms.contentlocale: de-de
ms.lasthandoff: 09/02/2017

---
# <a name="azure-active-directory-conditional-access-for-virtual-private-network-connectivity-preview"></a>Bedingter Azure Active Directory-Zugriff für Verbindungen über ein virtuelles privates Netzwerk (Vorschauversion)

Mit dem [bedingten Zugriff von Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) können Sie präzise steuern, wie autorisierte Benutzer auf Ihre Ressourcen zugreifen können. Der bedingte Azure AD-Zugriff für Verbindungen über ein virtuelles privates Netzwerk (VPN) ermöglicht den Schutz Ihrer VPN-Verbindungen durch den Einsatz von bedingtem Zugriff.


Führen Sie zum Konfigurieren des bedingten Azure AD-Zugriffs für VPN-Verbindungen folgende Schritte aus: 

1.  Konfigurieren Ihres VPN-Servers
2.  Konfigurieren Ihres VPN-Clients 
3.  Konfigurieren der Richtlinie zum bedingten Zugriff
4.  Überprüfung


## <a name="before-you-begin"></a>Voraussetzungen

In diesem Thema wird vorausgesetzt, dass Sie mit folgenden Themen vertraut sind:

- [Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md)
- [VPN und bedingter Zugriff](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Darüber hinaus sollten Sie sich unter [Enhancing remote access in Windows 10 with an automatic VPN profile](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile) (Erweitern des Remotezugriffs unter Windows 10 mit einem automatischen VPN-Profil) darüber informieren, wie Microsoft dieses Feature implementiert hat.   


## <a name="prerequisites"></a>Voraussetzungen

Um den bedingten Azure Active Directory-Zugriff für Verbindungen über ein virtuelles privates Netzwerk konfigurieren zu können, benötigen Sie einen konfigurierten VPN-Server. 



## <a name="step-1---configure-your-vpn-server"></a>Schritt 1: Konfigurieren Ihres VPN-Servers 

In diesem Schritt werden Stammzertifikate für die VPN-Authentifizierung mit Azure AD konfiguriert. Erforderliche Schritte zum Konfigurieren des bedingten Zugriffs für Verbindungen über ein virtuelles privates Netzwerk:

1. Erstellen eines VPN-Zertifikats über das Azure-Portal
2. Herunterladen des VPN-Zertifikats
2. Bereitstellen des Zertifikats auf Ihrem VPN-Server

Bei dem VPN-Zertifikat handelt es sich um den Zertifikataussteller, der von Azure AD zum Signieren von Zertifikaten verwendet wird, die für Windows 10-Clients ausgestellt werden, wenn die Authentifizierung bei Azure AD für VPN-Verbindungen erfolgt. Stellen Sie sich vor, bei dem vom Windows 10-Client angeforderten Token handelt es sich um ein Zertifikat, das dann für die Anwendung (in diesem Fall: der VPN-Server) bereitgestellt wird.

![Bedingter Zugriff](./media/active-directory-conditional-access-vpn-connectivity-windows10/06.png)

Im Azure-Portal können Sie zwei Zertifikate erstellen, um einen reibungslosen Übergang zu gewährleisten, wenn ein Zertifikat in Kürze abläuft. Bei der Zertifikaterstellung können Sie angeben, ob es sich bei dem Zertifikat um das primäre Zertifikat handelt. Das primäre Zertifikat wird während Authentifizierung zum Signieren des Zertifikats für die Verbindung verwendet.


**So erstellen Sie ein VPN-Zertifikat:**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator an.

2. Klicken Sie auf der linken Navigationsleiste auf **Azure Active Directory**. 

    ![VPN-Konnektivität](./media/active-directory-conditional-access-vpn-connectivity-windows10/01.png)

3. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Verwalten** auf **Bedingter Zugriff**.

    ![VPN-Konnektivität](./media/active-directory-conditional-access-azure-portal-get-started/02.png)

4. Klicken Sie auf der Seite **Bedingter Zugriff** im Abschnitt **Verwalten** auf **VPN-Konnektivität (Vorschau)**.

    ![VPN-Konnektivität](./media/active-directory-conditional-access-vpn-connectivity-windows10/03.png)

5. Klicken Sie auf der Seite **VPN-Konnektivität** auf **Neues Zertifikat**.

    ![VPN-Konnektivität](./media/active-directory-conditional-access-vpn-connectivity-windows10/04.png)

6. Führen Sie auf der Seite **Neu** die folgenden Schritte aus:

    ![VPN-Konnektivität](./media/active-directory-conditional-access-vpn-connectivity-windows10/05.png)

    a. Wählen Sie als **Dauer** die Option **1 Jahr** aus.

    b. Legen Sie **Primär** auf **Ja** fest.

    c. Klicken Sie auf **Erstellen**.

7. Klicken Sie auf der Seite „VPN-Konnektivität“ auf **Zertifikat herunterladen**.


Nun können Sie Ihr neu erstelltes Zertifikat auf Ihrem VPN-Server bereitstellen. Auf dem VPN-Server müssen Sie das heruntergeladene Zertifikat als *vertrauenswürdige Stammzertifizierungsstelle für die VPN-Authentifizierung* hinzufügen.

Bei Windows-RRAS-basierten Bereitstellungen müssen Sie das Stammzertifikat auf Ihrem NPS-Server durch Ausführen der folgenden Befehle dem Speicher *Enterprise NTauth* hinzufügen:

1. `certutil -dspublish <CACERT> RootCA`
2. `certutil -dspublish <CACERT> NtAuthCA`



## <a name="step-2---configure-your-vpn-client"></a>Schritt 2: Konfigurieren Ihres VPN-Clients 

In diesem Schritt müssen Sie Ihr VPN-Clientkonnektivitätsprofil gemäß den Angaben unter [VPN und bedingter Zugriff](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access) konfigurieren.


## <a name="step-3---configure-your-conditional-access-policy"></a>Schritt 3: Konfigurieren der Richtlinie zum bedingten Zugriff

In diesem Abschnitt erfahren Sie, wie Sie Ihre Richtlinie zum bedingten Zugriff für VPN-Konnektivität konfigurieren.

**So konfigurieren Sie die Richtlinie zum bedingten Zugriff:** 

1. Klicken Sie auf der Seite **Bedingter Zugriff** auf der Symbolleiste am oberen Rand auf **Hinzufügen**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-vpn-connectivity-windows10/07.png)

2. Geben Sie auf der Seite **Neu** im Textfeld **Name** einen Namen für Ihre Richtlinie ein (beispielsweise **VPN policy**).

    ![Bedingter Zugriff](./media/active-directory-conditional-access-vpn-connectivity-windows10/08.png)

5. Klicken Sie im Abschnitt **Zuweisung** auf **Benutzer und Gruppen**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-vpn-connectivity-windows10/09.png)

6. Führen Sie auf der Seite **Benutzer und Gruppen** die folgenden Schritte aus:

    ![Bedingter Zugriff](./media/active-directory-conditional-access-vpn-connectivity-windows10/10.png)

    a. Wählen Sie **Benutzer und Gruppen auswählen**.

    b. Klicken Sie auf **Auswählen**.

    c. Wählen Sie auf der Seite **Auswählen** Ihren Testbenutzer aus, und klicken Sie dann auf **Auswählen**.

    d. Klicken Sie auf der Seite **Benutzer und Gruppen** auf **Fertig**.

7. Führen Sie auf der Seite **Neu** die folgenden Schritte aus:

    ![Bedingter Zugriff](./media/active-directory-conditional-access-vpn-connectivity-windows10/11.png)

    a. Klicken Sie im Abschnitt **Zuweisungen** auf **Cloud-Apps**.

    b. Klicken Sie auf der Seite **Cloud-Apps** auf **Apps auswählen** und anschließend auf **Auswählen**.

    c. Geben Sie auf der Seite **Auswählen** im Textfeld **Anwendungen** die Zeichenfolge **vpn** ein.

    d. Wählen Sie **VPN-Server** aus.

    e. Klicken Sie auf **Auswählen**.


13. Klicken Sie zum Öffnen der Seite **Gewährung** auf der Seite **Neu** im Abschnitt **Steuerelemente** auf **Gewährung**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. Führen Sie auf der Seite **Gewährung** die folgenden Schritte aus:

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    a. Wählen Sie **Erfordern von Multi-Factor Authentication**.

    b. Klicken Sie auf **Auswählen**.

15. Klicken Sie auf der Seite **Neu** unter **Richtlinie aktivieren** auf **Ein**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. Klicken Sie auf der Seite **Neu** auf **Erstellen**.



## <a name="next-steps"></a>Nächste Schritte

Informationen zur Implementierung dieses Features durch Microsoft finden Sie unter [Enhancing remote access in Windows 10 with an automatic VPN profile](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile) (Erweitern des Remotezugriffs unter Windows 10 mit einem automatischen VPN-Profil).    


