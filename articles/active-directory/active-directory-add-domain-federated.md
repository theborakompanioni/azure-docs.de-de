---
title: "Hinzufügen des benutzerdefinierten Domänennamens und Einrichten der Verbundanmeldung an Azure Active Directory | Microsoft Docs"
description: "Es wird beschrieben, wie Sie Azure Active Directory die Domänennamen Ihres Unternehmens hinzufügen und die Verbundanmeldung zwischen Azure Active Directory und Ihrer lokalen Verbundlösung einrichten."
services: active-directory
documentationcenter: 
author: jeffsta
manager: femila
editor: 
ms.assetid: 27126c7e-e6d6-4ef3-a4fb-f5f0706e749d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/04/2016
ms.author: curtand;jeffsta
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 77d34a6ebce12465a15de158b9160a03db58c48e


---
# <a name="add-your-custom-domain-name-to-azure-active-directory"></a>Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory
Sie können einen benutzerdefinierten Domänennamen konfigurieren, z.B. „contoso.com“, damit Benutzer unter „contoso.com“ eine Verbundoberfläche mit einmaligem Anmelden über Ihr Unternehmensnetzwerk nutzen können. Wenn Sie in Ihrem Unternehmensnetzwerk bereits Active Directory-Verbunddienste (AD FS) oder einen anderen Verbundserver ausführen, können Sie Azure AD mit dem Azure AD Connect-Tool so konfigurieren, dass Ihr benutzerdefinierter Domänenname verwendet wird. Sie können Azure AD Connect auch nutzen, um eine neue AD FS-Umgebung bereitzustellen und diese für die einmalige Verbundanmeldung bei Azure AD zu konfigurieren.

Wenn Sie nicht über AD FS oder einen anderen Verbundserver verfügen und auch keine Bereitstellung planen, hilft Ihnen diese Anleitung weiter: [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-to-your-directory"></a>Hinzufügen eines benutzerdefinierten Domänennamens zu Ihrem Verzeichnis
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/) mit einem Benutzerkonto an, das ein globaler Administrator Ihres Azure AD-Verzeichnisses ist.
2. Öffnen Sie in Active Directory Ihr Verzeichnis, und wählen Sie die Registerkarte **Domänen** aus.****
3. Wählen Sie auf der Befehlsleiste **Hinzufügen**aus, und geben Sie den Namen Ihrer benutzerdefinierten Domäne ein (beispielsweise „contoso.com“). Achten Sie darauf, die Erweiterung „.com“, „.net“ oder eine andere Erweiterung der obersten Ebene zu verwenden.
4. Aktivieren Sie das Kontrollkästchen **Ich habe vor, diese Domäne für die einmalige Anmeldung bei meinem lokalen Active Directory-Verzeichnis zu konfigurieren** .
5. Wählen Sie **Hinzufügen**.

Führen Sie das Azure AD Connect-Tool aus, um den DNS-Eintrag abzurufen, der von Azure AD zum Überprüfen der Domäne verwendet wird. Der DNS-Eintrag wird im Assistenten im Schritt **Azure AD-Domäne** angezeigt. In [dieser Anleitung](connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation) wird veranschaulicht, wie der Schritt im Assistenten aussieht. Falls Sie nicht über das Azure AD Connect-Tool verfügen, können Sie es [hier herunterladen](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Hinzufügen des DNS-Eintrags bei der Domänennamen-Registrierungsstelle für die Domäne
Der nächste Schritt der Verwendung des benutzerdefinierten Domänennamens mit Azure AD ist die Aktualisierung der DNS-Zonendatei für die Domäne. So kann Azure AD überprüfen, ob sich der benutzerdefinierte Domänenname im Besitz Ihrer Organisation befindet.

1. Melden Sie sich auf der Website der Domänennamen-Registrierungsstelle für Ihren Domänennamen an. Falls Sie nicht über die Zugriffsberechtigung verfügen, können Sie eine Person oder ein Team in Ihrer Organisation mit entsprechendem Zugriff bitten, Schritt 2 auszuführen und Sie zu benachrichtigen, wenn der Vorgang abgeschlossen wurde.
2. Aktualisieren Sie die DNS-Zonendatei für die Domäne, indem Sie den DNS-Eintrag hinzufügen, der von Azure AD bereitgestellt wird. Mit diesem DNS-Eintrag kann Azure AD prüfen, ob sich die Domäne in Ihrem Besitz befindet. Der DNS-Eintrag nimmt keine Änderungen in Bezug auf das Verhalten, wie etwa E-Mail-Routing oder Webhosting, vor.

Hilfe zu diesem Schritt finden Sie unter [Anleitung für das Hinzufügen eines DNS-Eintrags bei gängigen DNS-Registrierungsstellen](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Überprüfen des Domänennamens mit Azure AD
Nachdem Sie den DNS-Eintrag hinzugefügt haben, können Sie den Domänennamen mit Azure AD überprüfen.

Wählen Sie zum Überprüfen der Domäne im Azure AD Connect-Assistenten im Schritt **Azure AD-Domäne** die Option **Weiter**. Azure AD sucht in der DNS-Zonendatei für die Domäne nach dem DNS-Eintrag. Azure AD überprüft den Domänennamen erst, nachdem die DNS-Einträge weitergegeben wurden. Häufig dauert die Weitergabe nur wenige Sekunden, aber der Vorgang kann unter Umständen auch eine Stunde oder länger dauern. Versuchen Sie, den Vorgang später erneut durchzuführen, wenn die Überprüfung beim ersten Mal nicht funktioniert.

Fahren Sie anschließend mit den restlichen Schritten im Azure AD Connect-Assistenten fort. Benutzer unter Windows Server AD werden mit Azure AD synchronisiert. Synchronisierte Benutzer in der Domäne, die Sie für den Verbund konfiguriert haben, können im Unternehmensnetzwerk die einmalige Verbundanmeldung an Azure AD nutzen.

## <a name="troubleshooting"></a>Problembehandlung
Versuchen Sie Folgendes, wenn Sie einen benutzerdefinierten Domänennamen nicht überprüfen können. Wir beginnen mit den häufigsten Ursachen und fahren mit den weniger häufigen fort.

1. **Warten Sie eine Stunde**. DNS-Einträge müssen weitergegeben (propagiert) werden, bevor Azure AD die Domäne überprüfen kann. Dies kann eine Stunde oder auch länger dauern.
2. **Stellen Sie sicher, dass der DNS-Eintrag eingegeben wurde und richtig ist.** Führen Sie diesen Schritt auf der Website der Domänennamen-Registrierungsstelle für die Domäne aus. Azure AD kann den Domänennamen nicht überprüfen, wenn der DNS-Eintrag nicht in der DNS-Zonendatei enthalten ist oder wenn sich keine genaue Übereinstimmung mit dem DNS-Eintrag ergibt, der von Azure AD bereitgestellt wird. Wenn Sie keinen Zugriff für die Aktualisierung von DNS-Einträgen für die Domäne bei der Domänennamen-Registrierungsstelle haben, können Sie den DNS-Eintrag für eine Person bzw. ein Team in Ihrer Organisation mit dem entsprechenden Zugriff bereitstellen und um das Hinzufügen des DNS-Eintrags bitten.
3. **Löschen Sie den Domänennamen aus einem anderen Verzeichnis in Azure AD**. Ein Domänenname kann nur in einem einzelnen Verzeichnis überprüft werden. Falls ein Domänenname bereits in einem anderen Verzeichnis überprüft wurde, muss er darin zuerst gelöscht werden, bevor er im neuen Verzeichnis überprüft werden kann. Informationen zum Löschen von Domänennamen finden Sie unter [Verwalten von benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Hinzufügen weiterer benutzerdefinierter Domänennamen
Wenn Sie in Ihrer Organisation mehrere benutzerdefinierte Domänennamen verwenden, z.B. „contoso.com“ und „contosobank.com“, können Sie maximal 900 Domänennamen hinzufügen. Verwenden Sie die gleichen Schritte in diesem Artikel, um die einzelnen Domänennamen hinzuzufügen.

## <a name="next-steps"></a>Nächste Schritte
* [Verwalten von benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md)
* [Informationen zu den Konzepten der Domänenverwaltung in Azure AD](active-directory-add-domain-concepts.md)
* [Anzeigen des Brandings Ihres Unternehmens bei der Anmeldung der Benutzer](active-directory-add-company-branding.md)
* [Verwenden von PowerShell zum Verwalten von Domänennamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)




<!--HONumber=Dec16_HO2-->


