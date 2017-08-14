---
title: "Registrieren für Azure Active Directory Premium"
description: "Hier erfahren Sie, wie Sie sich für die Azure Active Directory Premium-Edition registrieren."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: 391764e5-c4eb-4ae6-b8a9-3d09f0de04a6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: curtand
ms.custom: it-pro;
ms.translationtype: HT
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: fe9f4fa6f776a0c2edb572d0526c7520d1c1c7a0
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---
# <a name="quickstart-sign-up-for-azure-active-directory-premium"></a>Schnellstartanleitung: Registrieren für Azure Active Directory Premium
Um Azure Active Directory (Azure AD) Premium verwenden zu können, müssen Sie zunächst Lizenzen erwerben und Ihrem Azure-Abonnement zuordnen. Wenn Sie ein neues Azure-Abonnement erstellen, müssen Sie außerdem Ihren Lizenzplan und den Azure AD-Dienstzugriff aktivieren. Dies wird in den folgenden Abschnitten beschrieben. 

## <a name="sign-up-for-active-directory-premium"></a>Registrieren für Active Directory Premium
Es gibt mehrere Möglichkeiten, sich für Active Directory Premium zu registrieren: 

### <a name="azure-or-office-365"></a>Azure oder Office 365 
Abonnenten von Azure oder Office 365 können Azure Active Directory Premium online erwerben. 

Ausführliche Schritte finden Sie unter [How to Purchase Azure Active Directory Premium - Existing Customers](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/How-to-Purchase-Azure-Active-Directory-Premium-Existing-Customer) (Erwerben von Azure Active Directory Premium – Bestandskunden) oder [How to Purchase Azure Active Directory Premium - New Customers](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/How-to-Purchase-Azure-Active-Directory-Premium-New-Customers) (Erwerben von Azure Active Directory Premium – Neukunden).  

### <a name="enterprise-mobility--security"></a>Enterprise Mobility + Security
Enterprise Mobility + Security (EMS) ist eine kostengünstige Lösung für Unternehmen, die die folgenden Dienste unter einem einzelnen Lizenzplan verwenden möchten: Azure Active Directory Premium, Azure Information Protection und Microsoft Intune. Weitere Informationen zu EMS finden Sie auf der [Enterprise Mobility + Security-Website](https://www.microsoft.com/cloud-platform/enterprise-mobility-security). Weitere Informationen zu den erhältlichen EMS-Lizenztypen finden Sie auf der [Seite mit Preisoptionen für Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-pricing).  

Für Ihre ersten Schritte mit Azure AD unter EMS-Lizenzen können Sie eine der folgenden Lizenzoptionen verwenden:

- Testen Sie EMS mit einem kostenlosen [Enterprise Mobility + Security E5-Testabonnement](https://signup.microsoft.com/Signup?OfferId=87dd2714-d452-48a0-a809-d2f58c4f68b7&ali=1).
- Erwerben Sie [Enterprise Mobility + Security E5-Lizenzen](https://signup.microsoft.com/Signup?OfferId=e6de2192-536a-4dc3-afdc-9e2602b6c790&ali=1).
- Erwerben Sie [Enterprise Mobility + Security E3-Lizenzen](https://signup.microsoft.com/Signup?OfferId=4BBA281F-95E8-4136-8B0F-037D6062F54C&ali=1).

### <a name="microsoft-volume-licensing"></a>Microsoft-Volumenlizenzierung
Azure Active Directory Premium ist per [Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx) (ab 250 Lizenzen) oder über das [Open Volume License](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx)-Programm (fünf bis 250 Lizenzen) erhältlich.

Weitere Informationen zu Kaufoptionen im Rahmen der Volumenlizenzierung finden Sie [hier](https://www.microsoft.com/licensing/how-to-buy/how-to-buy.aspx).

> [!NOTE]
> Die Azure Active Directory Premium und Basic Editions stehen für Kunden in China zur Verfügung, die mit der weltweit verfügbaren Instanz von Azure Active Directory arbeiten. Allerdings werden die Azure Active Directory-Editionen Premium und Basic derzeit durch den in China von 21Vianet betriebenen Microsoft Azure-Dienst nicht unterstützt. Wenn Sie weitere Informationen benötigen, kontaktieren Sie uns im [Azure Active Directory-Forum](https://feedback.azure.com/forums/169401-azure-active-directory/).
> 
> 

Wenn Sie bereits Azure AD-Lizenzen für das Azure-Abonnement erworben und aktiviert haben, das Sie in den vorherigen Schritten verwendet haben, werden die Lizenzen automatisch im gleichen Verzeichnis aktiviert. In diesem Fall müssen Sie die restlichen Schritte in diesem Artikel nicht ausführen.

## <a name="activate-your-license-plan"></a>Aktivieren Ihres Lizenzplans
Ist das der erste Azure AD-Lizenzplan, den Sie von Microsoft erworben haben? Falls ja, erhalten Sie nach Abschluss Ihres Kaufs eine Bestätigungs-E-Mail. Sie benötigen diese E-Mail zum Aktivieren Ihres ersten Lizenzplans.

**Führen Sie einen der folgenden Schritte aus, um den Lizenzplan zu aktivieren:**

1. Klicken Sie zum Starten der Aktivierung entweder auf **Anmelden** oder **Registrieren**.
   
    ![Anmelden][1]

    - Wenn Sie bereits über einen Mandanten verfügen, klicken Sie auf **Anmelden** , um sich bei Ihrem vorhandenen Administratorkonto anzumelden. Melden Sie sich mit den Anmeldeinformationen des globalen Administrators für das Verzeichnis an, in dem die Lizenzen aktiviert werden sollen.

    - Wenn Sie einen neuen Azure AD-Mandanten für die Verwendung mit Ihrem Lizenzplan erstellen möchten, klicken Sie auf **Registrieren**, um das Dialogfeld **Kontoprofil erstellen** zu öffnen.

        ![Kontoprofil erstellen][2]

Nach Abschluss des Vorgangs wird das folgende Dialogfeld angezeigt, um zu bestätigen, dass die Aktivierung des Lizenzplans für Ihren Mandanten durchgeführt wurde:

![Bestätigung][3]

## <a name="activate-your-azure-active-directory-access"></a>Aktivieren des Azure Active Directory-Zugriffs
Wenn Sie neue Azure AD Premium-Lizenzen zu einem vorhandenen Abonnement hinzufügen, sollte Ihr Azure AD-Zugriff bereits aktiviert sein. Andernfalls müssen Sie den Azure AD-Zugriff aktivieren, nachdem Sie die **Begrüßungs-E-Mail** erhalten haben.  

Nach der Bereitstellung der erworbenen Lizenzen in Ihrem Verzeichnis erhalten Sie eine **Begrüßungs-E-Mail**. Mit der E-Mail wird bestätigt, dass Sie mit der Verwaltung der Lizenzen und Features von Azure Active Directory Premium oder Enterprise Mobility + Security beginnen können. 

> [!TIP]
> Sie können erst auf Azure AD für Ihren neuen Mandanten zugreifen, wenn Sie den Azure AD-Verzeichniszugriff über die Begrüßungs-E-Mail aktiviert haben, die automatisch nach Abschluss der Lizenzbereitstellung versendet wird. 

**Führen Sie die folgenden Schritte aus, um den Azure AD-Zugriff zu aktivieren:**

1. Klicken Sie in der **Willkommens-E-Mail** auf **Anmelden**. 
   
    ![Begrüßungs-E-Mail][4]
2. Nach erfolgreicher Anmeldung ist noch eine zweite Authentifizierung über ein mobiles Gerät erforderlich:
   
    ![Mobile Verifizierung][5]

Die Aktivierung dauert nur ein wenige Minuten. Danach haben Sie Zugriff auf Ihr Azure AD. 

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie gelernt, wie Sie sich für Azure AD Premium registrieren und Ihren Azure Active Directory-Zugriff aktivieren. 

Falls Sie bereits über ein Azure-Abonnement verfügen, können Sie über den folgenden Link eine Testversion verwenden oder Azure AD Premium-Lizenzen über das Azure-Portal erwerben.

> [!div class="nextstepaction"]
> [Azure AD Premium-Lizenzen aktivieren](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/TryBuyProductBlade) 

<!--Image references-->
[1]: ./media/active-directory-get-started-premium/MOLSEmail.png
[2]: ./media/active-directory-get-started-premium/MOLSAccountProfile.png
[3]: ./media/active-directory-get-started-premium/MOLSThankYou.png
[4]: ./media/active-directory-get-started-premium/AADEmail.png
[5]: ./media/active-directory-get-started-premium/SignUppage.png
[6]: ./media/active-directory-get-started-premium/Subscriptionspage.png
[7]: ./media/active-directory-get-started-premium/Premiuminportal.png
[8]: ./media/active-directory-get-started-premium/Premiuminportal_large.png
[9]: ./media/active-directory-get-started-premium/Signuppage_oops.png
[10]: ./media/active-directory-get-started-premium/contosolicenseplan.png
[11]: ./media/active-directory-get-started-premium/Assignlicensespicker.png
[12]: ./media/active-directory-get-started-premium/Usagelocation.png
