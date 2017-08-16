---
title: "Zertifizieren von AppSource für Azure Active Directory | Microsoft Docs"
description: "Detailinformationen zur Zertifizierung Ihrer AppSource-Anwendung für Azure Active Directory."
services: active-directory
documentationcenter: 
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 21206407-49f8-4c0b-84d1-c25e17cd4183
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/03/2017
ms.author: andret
ms.custom: aaddev
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: d8e2f8fc19ff879e6a7b632f033fd0ed9d77392a
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---

# <a name="how-to-get-appsource-certified-for-azure-active-directory"></a>Zertifizieren von AppSource für Azure Active Directory
[Microsoft AppSource](https://appsource.microsoft.com/) ist ein Ziel für Geschäftsbenutzer zum Entdecken, Ausprobieren und Verwalten branchenspezifischer SaaS-Anwendungen (eigenständiges SaaS und Add-On zu vorhandenen SaaS-Produkten von Microsoft).

Zum Auflisten einer eigenständigen SaaS-Anwendung auf AppSource muss Ihre Anwendung einmaliges Anmelden von Geschäftskonten aus von allen Unternehmen oder Organisationen akzeptieren, die über Azure Active Directory verfügen. Für den Anmeldevorgang muss das [OpenID Connect](./active-directory-protocols-openid-connect-code.md)- oder [OAuth 2.0](./active-directory-protocols-oauth-code.md)-Protokoll verwendet werden. SAML-Integration wird für die AppSource-Zertifizierung nicht akzeptiert.

## <a name="guides-and-code-samples"></a>Anleitungen und Codebeispiele
Wenn Sie weitere Informationen über die Integration Ihrer Anwendung in Azure Active Directory mithilfe der Open ID-Verbindung wünschen, befolgen Sie unsere Anleitungen und Codebeispiele in [Azure Active Directory für Entwickler](./active-directory-developers-guide.md#get-started "Erste Schritte").

## <a name="multi-tenant-applications"></a>Mehrinstanzenfähige Anwendungen

Eine Anwendung, die Anmeldungen von Benutzern aus allen Unternehmen oder Organisationen akzeptiert, die über Azure Active Directory verfügen, ohne dass eine separate Instanz, Konfiguration oder Bereitstellung erforderlich ist, wird als *mehrinstanzenfähige Anwendung* bezeichnet. AppSource empfiehlt, dass Anwendungen Mehrinstanzenfähigkeit implementieren, um die kostenlose Testbenutzeroberfläche *per Einfachklick* zu aktivieren.

So aktivieren Sie die Mehrinstanzenfähigkeit in Ihrer Anwendung:
- Legen Sie in den Registrierungsdaten Ihrer Anwendung im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) die `Multi-Tenanted`-Eigenschaft auf `Yes` fest (standardmäßig werden im Azure-Portal erstellte Anwendungen als *mit nur einem Mandanten* konfiguriert).
- Aktualisieren Sie den Code zum Senden von Anforderungen an den „`common`“-Endpunkt (aktualisieren Sie den Endpunkt von *https://login.microsoftonline.com/{IhrMandant}* in *https://login.microsoftonline.com/common*).
- Für einige Plattformen, wie ASP.NET, müssen Sie den Code auch zum Akzeptieren mehrere Aussteller aktualisieren.

Weitere Informationen zur Mehrinstanzenfähigkeit finden Sie unter: [Anmelden von Azure Active Directory-Benutzern (AD) mit dem mehrinstanzenfähigen Anwendungsmuster](./active-directory-devhowto-multi-tenant-overview.md).

### <a name="single-tenant-applications"></a>Anwendungen mit einem Mandanten
Anwendungen, die nur Anmeldungen von Benutzern einer definierten Azure Active Directory-Instanz akzeptieren, werden *Anwendungen mit einem Mandanten* genannt. Externe Benutzer (einschließlich Geschäfts- oder Schulkonten aus anderen Organisationen oder persönliche Konten) können sich bei Anwendungen mit einem Mandanten anmelden, nachdem jeder Benutzer als *Gastkonto* der Azure Active Directory-Instanz hinzugefügt worden ist, bei der die Anwendung registriert ist. Sie können Benutzer über die [*Azure AD B2B-Zusammenarbeit*](../active-directory-b2b-what-is-azure-ad-b2b.md) als Gastkonten einem Azure Active Directory hinzufügen – und dies kann [programmgesteuert](../active-directory-b2b-code-samples.md) erfolgen. Wenn Sie einen Benutzer einem Azure Active Directory als Gastkonto hinzufügen, wird eine Einladungs-E-Mail an den Benutzer gesendet, der die Einladung durch Klicken auf den Link in der Einladungs-E-Mail annehmen muss. Bei Einladungen, die an weitere Benutzer in einer einladenden Organisation gesendet werden, die auch Mitglied der Partnerorganisation ist, ist das Akzeptieren einer Einladung keine Voraussetzung für die Anmeldung.

Anwendungen mit einem Mandanten können die *Kontaktformular*-Oberfläche aktivieren, aber wenn Sie die kostenlose Testbenutzeroberfläche per Einfachklick, die AppSource empfiehlt, aktivieren möchten, ermöglichen Sie stattdessen die Mehrinstanzenfähigkeit Ihrer Anwendung.


## <a name="appsource-trial-experiences"></a>AppSource – Testbenutzeroberflächen

### <a name="free-trial-customer-led-trial-experience"></a>Kostenlose Testversion (kundengeführte Testbenutzeroberfläche) 
Die *kundengeführte Testversion* ist die Benutzeroberfläche, die AppSource empfiehlt, da sie einen Einfachklickzugriff auf Ihre Anwendung bietet. Die folgende Abbildung zeigt, wie diese Benutzeroberfläche aussieht:<br/><br/>

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%"/><ul><li>Benutzer findet Ihre Anwendung auf der AppSource-Website</li><li>Wählt die Option „Kostenlose Testversion“</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" /><ul><li>AppSource leitet den Benutzer zu einer URL auf Ihrer Website um</li><li>Ihre Website startet automatisch (beim Laden der Seite) den Prozess des <i>einmaligen Anmeldens</i></li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%"/><ul><li>Benutzer wird zur Microsoft-Anmeldeseite umgeleitet</li><li>Benutzer stellt Anmeldeinformationen zum Anmelden bereit</li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%"/><ul><li>Benutzer stimmt Ihrer Anwendung zu</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Anmeldung ist abgeschlossen und Benutzer wird wieder auf Ihre Website umgeleitet</li><li>Benutzer startet die kostenlose Testversion</li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a>Kontaktformular (partnergeführte Testbenutzeroberfläche)
Die *partnergeführte Testbenutzeroberfläche* kann verwendet werden, wenn ein manueller oder langfristiger Vorgang nötig ist, um den Benutzer/das Unternehmen bereitzustellen: Ihre Anwendung muss z.B. virtuelle Computer, Datenbankinstanzen oder Vorgänge bereitstellen, die viel Zeit in Anspruch nehmen. In diesem Fall sendet AppSource Ihnen Kontaktinformationen des Benutzers, nachdem der Benutzer die Schaltfläche *„Testversion anfordern“* ausgewählt und ein Formular ausgefüllt hat. Bei Erhalt dieser Informationen stellen Sie dann die Umgebung bereit und senden die Anweisungen für den Zugriff auf die Testoberfläche an den Benutzer:<br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%"/><ul><li>Benutzer findet Ihre Anwendung auf der AppSource-Website</li><li>Wählt die Option „Kontaktformular“</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%"/><ul><li>Füllt ein Formular mit Kontaktinformationen aus</li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%"/></td>
            <td>Sie erhalten Benutzerinformationen</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%"/></td>
            <td>Umgebung einrichten</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%"/></td>
            <td>Wenden Sie sich mit Testversionsinformationen an den Benutzer</td>
        </tr>
        </table><br/><br/>
        <ul><li>Sie erhalten Informationen über den Benutzer und richten die Testversionsinstanz ein</li><li>Sie senden den Hyperlink, damit der Benutzer auf Ihre Anwendung zugreifen kann</li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%"/><ul><li>Benutzer greift auf Ihre Anwendung zu und schließt den Prozess des einmaligen Anmeldens ab</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%"/><ul><li>Benutzer stimmt Ihrer Anwendung zu</li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Anmeldung ist abgeschlossen und Benutzer wird wieder auf Ihre Website umgeleitet</li><li>Benutzer startet die kostenlose Testversion</li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a>Weitere Informationen
Weitere Informationen zur AppSource-Testversionsbenutzeroberfläche finden Sie in [diesem Video](https://aka.ms/trialexperienceforwebapps). 
 
## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Erstellen von Anwendungen, die Azure Active Directory-Anmeldungen unterstützen, finden Sie unter [Authentifizierungsszenarien für Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios). 

- Informationen zum Auflisten der SaaS-Anwendung in AppSource finden Sie unter [AppSource-Partnerinformationen](https://appsource.microsoft.com/partners).


## <a name="get-support"></a>Erhalten von Support
Für die Azure Active Directory-Integration bieten wir Support mit der Community über [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory). 

Sie sollten Ihre Fragen unbedingt zuerst auf Stack Overflow stellen und die vorhandenen Probleme durchsuchen, um festzustellen, ob bereits vor Ihnen jemand Ihre Frage gestellt hat. Stellen Sie sicher, dass Ihre Fragen und Kommentare mit `[azure-active-directory]` markiert sind.

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#get-started


<!--Image references-->
