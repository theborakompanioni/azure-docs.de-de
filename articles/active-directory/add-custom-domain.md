---
title: "Hinzufügen einer benutzerdefinierten Domäne zu Azure AD | Microsoft-Dokumentation"
description: "Erfahren Sie, wie eine benutzerdefinierte Domäne in Azure Active Directory hinzugefügt wird."
services: active-directory
author: curtand
manager: femila
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2017
ms.author: curtand
ms.reviewer: jsnow
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: 07aaefc0b2f806178651c2192f6abcbae9a31f83
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---
# <a name="quickstart-add-a-custom-domain-name-to-azure-active-directory"></a>Schnellstart: Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory

Jedes Azure AD-Verzeichnis enthält einen anfänglichen Domänennamen in der Form *domänenname*.onmicrosoft.com. Der anfängliche Domänenname kann nicht geändert oder gelöscht werden, Sie können Azure AD jedoch auch den Namen Ihrer Unternehmensdomäne hinzufügen. Beispielsweise verfügt Ihre Organisation wahrscheinlich über andere Domänennamen, die sie für ihre Geschäftstätigkeit verwendet, sowie über Benutzer, die sich mit dem Domänennamen des Unternehmens anmelden. Indem Sie Azure AD benutzerdefinierte Domänennamen hinzufügen, können Sie Benutzernamen im Verzeichnis zuweisen, die Ihren Benutzern vertraut sind, z.B. „alice@contoso.com“ anstelle von „alice@*<domain name>*.onmicrosoft.com“. Der Prozess ist ganz einfach:

1. Fügen Sie Ihrem Verzeichnis den benutzerdefinierten Domänennamen hinzu.
2. Fügen Sie bei der Domänennamen-Registrierungsstelle einen DNS-Eintrag für den Domänennamen hinzu.
3. Überprüfen Sie den benutzerdefinierten Domänennamen in Azure AD.

## <a name="add-the-custom-domain-name-to-your-directory"></a>Fügen Sie Ihrem Verzeichnis den benutzerdefinierten Domänennamen hinzu.
1. Melden Sie sich beim [Azure-Portal](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie links **Domänennamen**.
3. Wählen Sie unter ***Verzeichnisname* – Domänennamen** die Option **Hinzufügen** aus.
   
   ![Auswählen des Befehls „Hinzufügen“](./media/active-directory-domains-add-azure-portal/add-command.png)
5. Geben Sie auf **Domänenname** den Namen Ihrer benutzerdefinierten Domäne in das Feld ein, z.B. „contoso.com“, und wählen Sie dann **Domäne hinzufügen** aus. Achten Sie darauf, die Erweiterung „.com“, „.net“ oder eine andere Erweiterung der obersten Ebene zu verwenden.
6. Unter ***Domänenname*** (d.h. dort, wo Ihr neuer Domänenname der Titel ist) finden Sie die DNS-Eintragsinformationen, die Sie später verwenden müssen, um den benutzerdefinierten Domänennamen in Azure AD zu überprüfen.
   
   ![Abrufen von DNS-Eintragsinformationen](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

> [!TIP]
> Wenn Sie planen, Ihre lokale Windows Server AD mit Azure AD in einen Verbund aufzunehmen, müssen Sie das Kontrollkästchen **Ich möchte diese Domäne für einmaliges Anmelden bei meinem lokalen Active Directory konfigurieren** aktivieren, wenn Sie zum Synchronisieren Ihrer Verzeichnisse das Tool Azure AD Connect ausführen. Sie müssen den Domänennamen, den Sie für den Verbund mit Ihrem lokalen Verzeichnis im Schritt **Azure AD-Domäne** des Assistenten auswählen, außerdem registrieren. In [dieser Anleitung](./connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation) wird veranschaulicht, wie der Schritt im Assistenten aussieht. Falls Sie nicht über das Azure AD Connect-Tool verfügen, können Sie es [hier herunterladen](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-a-dns-entry-for-the-domain-name-at-the-domain-name-registrar"></a>Fügen Sie bei der Domänennamen-Registrierungsstelle einen DNS-Eintrag für den Domänennamen hinzu.
Der nächste Schritt der Verwendung des benutzerdefinierten Domänennamens mit Azure AD ist die Aktualisierung der DNS-Zonendatei für die Domäne. Azure AD kann dann überprüfen, ob sich der benutzerdefinierte Domänenname im Besitz Ihrer Organisation befindet. Sie können [Azure DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) für Ihre Azure-/Office 365-/externen DNS-Einträge in Azure verwenden, oder Sie fügen den DNS-Eintrag bei einer [anderen DNS-Registrierungsstelle](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/) hinzu.

1. Melden Sie sich an der Domänennamen-Registrierungsstelle für die Domäne an. Falls Sie nicht über die Zugriffsberechtigung zur Aktualisierung des DNS-Eintrags verfügen, können Sie eine Person oder ein Team mit entsprechendem Zugriff bitten, Schritt 2 auszuführen und Sie zu benachrichtigen, wenn der Vorgang abgeschlossen wurde.
2. Aktualisieren Sie die DNS-Zonendatei für die Domäne, indem Sie den DNS-Eintrag hinzufügen, der von Azure AD bereitgestellt wird. Der DNS-Eintrag nimmt keine Änderungen in Bezug auf das Verhalten, wie etwa E-Mail-Routing oder Webhosting, vor.

## <a name="verify-the-custom-domain-name-in-azure-ad"></a>Überprüfen Sie den benutzerdefinierten Domänennamen in Azure AD.
Nachdem Sie den DNS-Eintrag hinzugefügt haben, können Sie den Domänennamen mit Azure AD überprüfen. Ein Domänenname kann erst überprüft werden, nachdem die DNS-Einträge weitergegeben wurden. Häufig dauert diese Weitergabe nur wenige Sekunden, aber der Vorgang kann unter Umständen auch eine Stunde oder länger dauern. Versuchen Sie, den Vorgang später erneut durchzuführen, wenn die Überprüfung beim ersten Mal nicht funktioniert.

1. Melden Sie sich bei [Azure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie links **Domänennamen**.
3. Wählen Sie auf ***Verzeichnisname* – Domänennamen** den Befehl **Domänenname hinzufügen** aus. 
  ![Auswählen des Befehls „Hinzufügen“](./media/active-directory-domains-add-azure-portal/add-command.png)
3. Wählen Sie auf ***Verzeichnisname* – Domänennamen** den nicht überprüften Domänennamen, den Sie überprüfen möchten.
4. Wählen Sie unter ***Domänenname*** (d.h., der ausgewählte Domänenname ist der Titel) die Option **Überprüfen** aus, um die Überprüfung durchzuführen.

Nun können Sie [Benutzernamen zuweisen, die Ihren benutzerdefinierten Domänennamen enthalten](active-directory-users-create-azure-portal.md). Sie können mit Ihrem benutzerdefinierten Domänennamen cloudbasierte Benutzerkonten erstellen oder zuvor synchronisierte lokale Benutzerkontoinformationen aktualisieren. Sie können auch synchronisierte Domänensuffixinformationen zu Benutzerkonten mit [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) oder der [Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations) ändern.

> [!TIP]
> Sie können bis zu 900 benutzerdefinierte Domänennamen hinzufügen, es kann jedoch nur einer [als primärer Domänennamen für Ihr Azure AD-Verzeichnis festgelegt werden](active-directory-domains-manage-azure-portal.md#set-the-primary-domain-name-for-your-azure-ad-directory), der standardmäßig beim Erstellen neuer Konten verwendet wird.

## <a name="troubleshooting"></a>Problembehandlung
Probieren Sie die folgenden Schritte zur Problembehandlung aus, wenn Sie einen benutzerdefinierten Domänennamen nicht überprüfen können:

1. **Warten Sie eine Stunde**. DNS-Einträge müssen weitergegeben (propagiert) werden, bevor Azure AD die Domäne überprüfen kann. Dies kann eine Stunde oder auch länger dauern.
2. **Stellen Sie sicher, dass der DNS-Eintrag eingegeben wurde und richtig ist.** Führen Sie diesen Schritt auf der Website der Domänennamen-Registrierungsstelle für die Domäne aus. Azure AD kann den Domänennamen nicht überprüfen, wenn Folgendes zutrifft: 
  * Der DNS-Eintrag ist in der DNS-Zonendatei nicht enthalten.
  * Es gibt keine exakte Übereinstimmung mit dem DNS-Eintrag, den Azure AD zur Verfügung gestellt hat. 
  
  Wenn Sie keinen Zugriff für die Aktualisierung von DNS-Einträgen für die Domäne bei der Domänennamen-Registrierungsstelle haben, können Sie den DNS-Eintrag für eine Person bzw. ein Team in Ihrer Organisation mit dem entsprechenden Zugriff bereitstellen und um das Hinzufügen des DNS-Eintrags bitten.
3. **Löschen Sie den Domänennamen aus einem anderen Verzeichnis in Azure AD**. Ein Domänenname kann nur in einem einzelnen Verzeichnis überprüft werden. Wenn ein Domänenname derzeit in einem anderen Verzeichnis überprüft ist, kann er erst in Ihrem neuen Verzeichnis überprüft werden, nachdem er im anderen gelöscht wurde. Informationen zum Löschen von Domänennamen finden Sie unter [Verwalten von benutzerdefinierten Domänennamen](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Hinzufügen weiterer benutzerdefinierter Domänennamen
Wenn Sie in Ihrer Organisation mehrere benutzerdefinierte Domänennamen verwenden, z.B. „contoso.com“ und „contosobank.com“, können Sie maximal 900 Domänennamen hinzufügen. Die Schritte in diesem Artikel können Ihnen helfen, die einzelnen Domänennamen hinzuzufügen.

### <a name="learn-more"></a>Weitere Informationen
[Konzeptionelle Übersicht über benutzerdefinierte Domänennamen in Azure AD](active-directory-add-domain-concepts.md)

[Verwalten von benutzerdefinierten Domänennamen](active-directory-domains-manage-azure-portal.md)

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie gelernt, wie Sie eine benutzerdefinierte Domäne in Azure AD hinzufügen. 

Über den folgenden Link können Sie mit dem Azure-Portal eine neue benutzerdefinierte Domäne in Azure AD hinzufügen.

> [!div class="nextstepaction"]
> [Hinzufügen einer benutzerdefinierten Domäne](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/QuickStart) 
