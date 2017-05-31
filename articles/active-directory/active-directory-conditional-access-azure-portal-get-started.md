---
title: Erste Schritte mit dem bedingten Zugriff in Azure Active Directory | Microsoft-Dokumentation
description: Testen Sie den bedingten Zugriff mit einer Ortsbedingung.
services: active-directory
keywords: "bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: 52b65b24867906d0c574a323904d980e126bcbd2
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017


---
# <a name="get-started-with-conditional-access-in-azure-active-directory"></a>Erste Schritte mit dem bedingten Zugriff in Azure Active Directory

Beim bedingten Zugriff handelt es sich um eine Funktion von Azure Active Directory, mit der Sie Bedingungen festlegen können, unter denen autorisierte Benutzer auf Ihre Apps zugreifen können. 

Dieses Thema enthält Anweisungen zum Testen des bedingten Zugriffs auf Grundlage einer Ortsbedingung in Ihrer Umgebung.  


## <a name="scenario-description"></a>Beschreibung des Szenarios

Eine häufige Anforderung in vielen Organisationen besteht darin, die mehrstufige Authentifizierung für den Zugriff auf Apps nur dann erforderlich zu machen, wenn der Zugriff nicht vom Unternehmensintranet aus erfolgt. Mit Azure Active Directory erreichen Sie dies problemlos, indem Sie eine ortsbasierte Richtlinie für bedingten Zugriff konfigurieren. Dieses Thema enthält ausführliche Anweisungen zum Konfigurieren einer entsprechenden Richtlinie. Die Richtlinie nutzt [vertrauenswürdige IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips), um zwischen Zugriffsversuchen vom Unternehmensintranet und Zugriffsversuchen von anderswo zu unterscheiden.


## <a name="prerequisites"></a>Voraussetzungen

Bei dem in diesem Thema beschriebenen Szenario wird davon ausgegangen, dass Sie mit den Konzepten unter [Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md) vertraut sind.

Zum Testen dieses Szenario ist Folgendes erforderlich:

- Erstellen eines Testbenutzers 

- Zuweisen einer Azure AD Premium-Lizenz zum Testbenutzer

- Konfigurieren einer verwalteten App und Zuweisen eines Testbenutzers zu dieser App

- Konfigurieren vertrauenswürdiger IP-Adressen

Weitere Informationen zu vertrauenswürdigen IP-Adressen finden Sie unter [Vertrauenswürdige IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).


## <a name="policy-configuration-steps"></a>Schritte für die Richtlinienkonfiguration

**Gehen Sie zum Konfigurieren der Richtlinie für bedingten Zugriff wie folgt vor:**

1. Klicken Sie im Azure-Portal auf der linken Navigationsleiste auf **Azure Active Directory**. 

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/01.png)

2. Klicken Sie auf dem Blatt **Azure Active Directory** im Abschnitt **Verwalten** auf **Bedingter Zugriff**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/02.png)
 
3. Klicken Sie zum Öffnen des Blatts **Neu** auf dem Blatt **Bedingter Zugriff** oben auf der Symbolleiste auf **Hinzufügen**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/03.png)

4. Geben Sie auf dem Blatt **Neu** im Textfeld **Name** einen Namen für Ihre Richtlinie ein.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/04.png)

5. Klicken Sie im Abschnitt **Zuweisung** auf **Benutzer und Gruppen**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/05.png)

6. Führen Sie auf dem Blatt **Benutzer und Gruppen** die folgenden Schritte aus:

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/06.png)

    a. Wählen Sie **Benutzer und Gruppen auswählen**.

    b. Klicken Sie auf **Auswählen**.

    c. Wählen Sie auf dem Blatt **Auswahl** Ihren Testbenutzer aus, und klicken Sie dann auf **Auswahl**.

    d. Klicken Sie auf dem Blatt **Benutzer und Gruppen** auf **Fertig**.

7. Klicken Sie zum Öffnen des Blatts **Cloud-Apps** auf dem Blatt **Neu** im Abschnitt **Zuweisung** auf **Cloud-Apps**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. Führen Sie auf dem Blatt **Cloud-Apps** die folgenden Schritte aus:

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/08.png)

    a. Klicken Sie auf **Apps auswählen**.

    b. Klicken Sie auf **Auswählen**.

    c. Wählen Sie auf dem Blatt **Auswahl** Ihre Cloud-App aus, und klicken Sie dann auf **Auswahl**.

    d. Klicken Sie auf dem Blatt **Cloud-Apps** auf **Fertig**.

9. Klicken Sie zum Öffnen des Blatts **Bedingungen** auf dem Blatt **Neu** im Abschnitt **Zuweisung** auf **Bedingungen**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/09.png)

10. Klicken Sie zum Öffnen des Blatts **Standorte** auf dem Blatt **Bedingungen** auf **Standorte**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/10.png)

11. Führen Sie auf dem Blatt **Standorte** die folgenden Schritte aus:

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/11.png)

    a. Klicken Sie unter **Konfigurieren** auf **Ja**.

    b. Klicken Sie unter **Include** (Einschließen) auf **All locations** (Alle Standorte).

    c. Klicken Sie auf **Exclude** (Ausschließen) und anschließend auf **All trusted IPs** (Alle vertrauenswürdigen IPs).

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/12.png)

    d. Klicken Sie auf **Done**.

12. Klicken Sie auf dem Blatt **Bedingungen** auf **Fertig**.

13. Klicken Sie zum Öffnen des Blatts **Gewährung** auf dem Blatt **Neu** im Abschnitt **Steuerelemente** auf **Gewährung**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. Führen Sie auf dem Blatt **Gewährung** die folgenden Schritte aus:

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    a. Wählen Sie **Erfordern von Multi-Factor Authentication**.

    b. Klicken Sie auf **Auswählen**.

15. Klicken Sie auf dem Blatt **Neu** unter **Richtlinie aktivieren** auf **Ein**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. Klicken Sie auf dem Blatt **Neu** auf **Erstellen**.


## <a name="testing-the-policy"></a>Testen der Richtlinie

Greifen Sie zum Testen der Richtlinie auf die App von einem Gerät zu, das: 

1. eine IP-Adresse besitzt, die in den konfigurierten vertrauenswürdigen IPs enthalten ist. 

1. eine IP-Adresse besitzt, die nicht in den konfigurierten vertrauenswürdigen IPs enthalten ist.

Die mehrstufige Authentifizierung sollte nur bei einem Verbindungsversuch erforderlich sein, der von einem Gerät erfolgte, das nicht in Ihren konfigurierten vertrauenswürdigen IPs enthalten ist. 


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum bedingten Zugriff finden Sie unter [Conditional access in Azure Active Directory - preview](active-directory-conditional-access-azure-portal.md) (Bedingter Zugriff in Azure Active Directory – Vorschau).


