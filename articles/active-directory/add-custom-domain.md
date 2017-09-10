---
title: "Hinzufügen einer benutzerdefinierten Domäne zu Azure AD | Microsoft-Dokumentation"
description: "Erfahren Sie, wie eine benutzerdefinierte Domäne in Azure Active Directory hinzugefügt wird."
services: active-directory
author: jeffgilb
manager: femila
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 3acdc406b5e753d407c7c6fb32fe0ad5ed6dd68c
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="quickstart-add-a-custom-domain-name-to-azure-active-directory"></a>Schnellstart: Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory

Jedes Azure AD-Verzeichnis enthält einen anfänglichen Domänennamen in der Form *domänenname*.onmicrosoft.com. Der anfängliche Domänenname kann nicht geändert oder gelöscht werden, Sie können Azure AD jedoch auch den Namen Ihrer Unternehmensdomäne hinzufügen. Beispielsweise verfügt Ihre Organisation wahrscheinlich über andere Domänennamen, die sie für ihre Geschäftstätigkeit verwendet, sowie über Benutzer, die sich mit dem Domänennamen des Unternehmens anmelden. Indem Sie Azure AD benutzerdefinierte Domänennamen hinzufügen, können Sie Benutzernamen im Verzeichnis zuweisen, die Ihren Benutzern vertraut sind, z.B. „alice@contoso.com“ anstelle von „alice@*<domain name>*.onmicrosoft.com“. Der Prozess ist ganz einfach:

1. Fügen Sie Ihrem Verzeichnis den benutzerdefinierten Domänennamen hinzu.
2. Fügen Sie bei der Domänennamen-Registrierungsstelle einen DNS-Eintrag für den Domänennamen hinzu.
3. Überprüfen Sie den benutzerdefinierten Domänennamen in Azure AD.

## <a name="add-your-custom-domain"></a>Hinzufügen Ihrer benutzerdefinierten Domäne
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
   
   ![Öffnen der Benutzerverwaltung](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Wählen Sie auf dem Blatt ***Verzeichnisname*** die Option **Domänennamen** aus.
4. Wählen Sie auf dem Blatt ***Verzeichnisname* – Domänennamen** den Befehl **Hinzufügen** aus.
   
   ![Auswählen des Befehls „Hinzufügen“](./media/active-directory-domains-add-azure-portal/add-command.png)
5. Geben Sie auf dem Blatt **Domänenname** den Namen Ihrer benutzerdefinierten Domäne in das Feld ein, z.B. „contoso.com“, und wählen Sie dann **Domäne hinzufügen** aus. Achten Sie darauf, die Erweiterung „.com“, „.net“ oder eine andere Erweiterung der obersten Ebene zu verwenden.
6. Rufen Sie auf dem Blatt ***Domänenname*** (mit Ihrem benutzerdefinierten Domänennamen im Titel) die DNS-Eintragsinformationen ab, anhand derer bestätigt werden soll, dass Ihre Organisation der Besitzer des benutzerdefinierten Domänennamens ist.
   
   ![Abrufen von DNS-Eintragsinformationen](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

> [!TIP]
> Wenn Sie planen, Ihre lokale Windows Server AD mit Azure AD in einen Verbund aufzunehmen, müssen Sie das Kontrollkästchen **Ich möchte diese Domäne für einmaliges Anmelden bei meinem lokalen Active Directory konfigurieren** aktivieren, wenn Sie zum Synchronisieren Ihrer Verzeichnisse das Tool Azure AD Connect ausführen. Sie müssen den Domänennamen, den Sie für den Verbund mit Ihrem lokalen Verzeichnis im Schritt **Azure AD-Domäne** des Assistenten auswählen, außerdem registrieren. In [dieser Anleitung](./connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation) wird veranschaulicht, wie der Schritt im Assistenten aussieht. Falls Sie nicht über das Azure AD Connect-Tool verfügen, können Sie es [hier herunterladen](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Hinzufügen des DNS-Eintrags bei der Domänennamen-Registrierungsstelle für die Domäne
Der nächste Schritt der Verwendung des benutzerdefinierten Domänennamens mit Azure AD ist die Aktualisierung der DNS-Zonendatei für die Domäne. Azure AD kann dann überprüfen, ob sich der benutzerdefinierte Domänenname im Besitz Ihrer Organisation befindet. Sie können [Azure DNS im Azure-Portal](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) als einzelnen Verwaltungspunkt für Azure, Office 365 und externe DNS-Einträge in Azure verwenden.

1. Melden Sie sich an der Domänennamen-Registrierungsstelle für die Domäne an. Falls Sie nicht über die Zugriffsberechtigung zur Aktualisierung des DNS-Eintrags verfügen, können Sie eine Person oder ein Team mit entsprechendem Zugriff bitten, Schritt 2 auszuführen und Sie zu benachrichtigen, wenn der Vorgang abgeschlossen wurde.
2. Aktualisieren Sie die DNS-Zonendatei für die Domäne, indem Sie den DNS-Eintrag hinzufügen, der von Azure AD bereitgestellt wird. Mit diesem DNS-Eintrag kann Azure AD prüfen, ob sich die Domäne in Ihrem Besitz befindet. Der DNS-Eintrag nimmt keine Änderungen in Bezug auf das Verhalten, wie etwa E-Mail-Routing oder Webhosting, vor.

Hilfe zum Hinzufügen des DNS-Eintrags finden Sie unter [Anleitung für das Hinzufügen eines DNS-Eintrags bei gängigen DNS-Registrierungsstellen](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Überprüfen des Domänennamens mit Azure AD
Nachdem Sie den DNS-Eintrag hinzugefügt haben, können Sie den Domänennamen mit Azure AD überprüfen.

Ein Domänenname kann erst überprüft werden, nachdem die DNS-Einträge weitergegeben wurden. Häufig dauert diese Weitergabe nur wenige Sekunden, aber der Vorgang kann unter Umständen auch eine Stunde oder länger dauern. Versuchen Sie, den Vorgang später erneut durchzuführen, wenn die Überprüfung beim ersten Mal nicht funktioniert.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Durchsuchen** aus, geben Sie „Benutzerverwaltung“ in das Textfeld ein, und wählen Sie dann **Eingeben** aus.
   
   ![Öffnen der Benutzerverwaltung](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Wählen Sie auf dem Blatt **Benutzerverwaltung – Domänennamen** den nicht überprüften Domänennamen, den Sie überprüfen möchten.
4. Wählen Sie auf dem Blatt ***Domänenname*** (dem Blatt mit Ihrem neuen Domänennamen im Titel) die Option **Überprüfen** aus, um die Überprüfung abzuschließen.

> [!TIP]
> Sie können bis zu 900 benutzerdefinierte Domänennamen hinzufügen, es kann jedoch nur einer [als primärer Domänennamen für Ihr Azure AD-Verzeichnis festgelegt werden](active-directory-domains-manage-azure-portal.md#set-the-primary-domain-name-for-your-azure-ad-directory), der standardmäßig beim Erstellen neuer Konten verwendet wird.

Nun können Sie mit Ihrem benutzerdefinierten Domänennamen cloudbasierte Benutzerkonten erstellen oder zuvor synchronisierte lokale Benutzerkontoinformationen aktualisieren. Sie können auch zuvor synchronisierte Domänensuffixinformationen zu Benutzerkonten mit [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) oder der [Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations) ändern.

## <a name="troubleshooting"></a>Problembehandlung
Probieren Sie die folgenden Schritte zur Problembehandlung aus, wenn Sie einen benutzerdefinierten Domänennamen nicht überprüfen können:

1. **Warten Sie eine Stunde**. DNS-Einträge müssen weitergegeben (propagiert) werden, bevor Azure AD die Domäne überprüfen kann. Dies kann eine Stunde oder auch länger dauern.
2. **Stellen Sie sicher, dass der DNS-Eintrag eingegeben wurde und richtig ist.** Führen Sie diesen Schritt auf der Website der Domänennamen-Registrierungsstelle für die Domäne aus. Azure AD kann den Domänennamen nicht überprüfen, wenn der DNS-Eintrag nicht in der DNS-Zonendatei enthalten ist oder wenn sich keine genaue Übereinstimmung mit dem DNS-Eintrag ergibt, der von Azure AD bereitgestellt wird. Wenn Sie keinen Zugriff für die Aktualisierung von DNS-Einträgen für die Domäne bei der Domänennamen-Registrierungsstelle haben, können Sie den DNS-Eintrag für eine Person bzw. ein Team in Ihrer Organisation mit dem entsprechenden Zugriff bereitstellen und um das Hinzufügen des DNS-Eintrags bitten.
3. **Löschen Sie den Domänennamen aus einem anderen Verzeichnis in Azure AD**. Ein Domänenname kann nur in einem einzelnen Verzeichnis überprüft werden. Falls ein Domänenname bereits in einem anderen Verzeichnis überprüft wurde, muss er darin zuerst gelöscht werden, bevor er im neuen Verzeichnis überprüft werden kann. Informationen zum Löschen von Domänennamen finden Sie unter [Verwalten von benutzerdefinierten Domänennamen](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Hinzufügen weiterer benutzerdefinierter Domänennamen
Wenn Ihre Organisation mehrere benutzerdefinierte Domänennamen verwendet, beispielsweise contoso.com und contosobank.com, können Sie bis zu 900 weitere durch Wiederholen der Schritte in diesem Artikel für die anderen hinzufügen.

### <a name="learn-more"></a>Weitere Informationen
[Konzeptionelle Übersicht über benutzerdefinierte Domänennamen in Azure AD](active-directory-add-domain-concepts.md)

[Verwalten von benutzerdefinierten Domänennamen](active-directory-domains-manage-azure-portal.md)


## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie gelernt, wie Sie eine benutzerdefinierte Domäne in Azure AD hinzufügen. 

Über den folgenden Link können Sie mit dem Azure-Portal eine neue benutzerdefinierte Domäne in Azure AD hinzufügen.

> [!div class="nextstepaction"]
> [Hinzufügen einer benutzerdefinierten Domäne](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/QuickStart) 
