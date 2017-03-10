---
title: "Hinzufügen eines benutzerdefinierten Domänennamens zur Azure Active Directory-Vorschau | Microsoft Docs"
description: "Erfahren Sie, wie Sie Azure Active Directory die Domänennamen Ihres Unternehmens hinzufügen und den Domänennamen überprüfen."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d97e57c6-578a-4929-8fb8-42e858a711c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: f1f3d6fa1ba29b0d43d29c7d9cccfc430fdd6f1e
ms.openlocfilehash: 5153926f34961a397ef9632ca1f8ac868ba1986e
ms.lasthandoff: 02/22/2017


---
# <a name="add-a-custom-domain-name-to-azure-active-directory-preview"></a>Hinzufügen eines benutzerdefinierten Domänennamens zur Azure Active Directory-Vorschau
> [!div class="op_single_selector"]
> * [Azure-Portal](active-directory-domains-add-azure-portal.md)
> * [klassischen Azure-Portal](active-directory-add-domain.md)
> 
> 

Sie verfügen über mindestens einen Domänennamen, den Ihre Organisation zu geschäftlichen Zwecken verwendet, und Ihre Benutzer melden sich unter Verwendung Ihres geschäftlichen Domänennamens bei Ihrem Unternehmensnetzwerk an. Mit der Azure Active Directory-Vorschau (Azure AD) können Sie den Namen Ihrer Unternehmensdomäne nun auch Azure AD hinzufügen. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md) Dadurch können Sie in dem Verzeichnis Benutzernamen zuweisen, die Ihren Benutzern vertraut sind (etwa „alice@contoso.com“). Der Prozess ist ganz einfach:

1. Fügen Sie Ihrem Verzeichnis den benutzerdefinierten Domänennamen hinzu.
2. Fügen Sie bei der Domänennamen-Registrierungsstelle einen DNS-Eintrag für den Domänennamen hinzu.
3. Überprüfen Sie den benutzerdefinierten Domänennamen in Azure AD.

## <a name="how-do-i-add-a-domain-name"></a>Wie füge ich einen Domänennamen hinzu?
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
   
   ![Öffnen der Benutzerverwaltung](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Wählen Sie auf dem Blatt ***Verzeichnisname*** die Option **Domänennamen** aus.
4. Wählen Sie auf dem Blatt ***Verzeichnisname* – Domänennamen** den Befehl **Hinzufügen** aus.
   
   ![Auswählen des Befehls „Hinzufügen“](./media/active-directory-domains-add-azure-portal/add-command.png)
5. Geben Sie auf dem Blatt **Domänenname** den Namen Ihrer benutzerdefinierten Domäne in das Feld ein, z.B. „contoso.com“, und wählen Sie dann **Domäne hinzufügen** aus. Achten Sie darauf, die Erweiterung „.com“, „.net“ oder eine andere Erweiterung der obersten Ebene zu verwenden.
6. Rufen Sie auf dem Blatt ***Domänenname*** (dem Blatt mit Ihrem neuen Domänennamen im Titel) die DNS-Eintragsinformationen ab, anhand derer Azure AD überprüft, ob Ihre Organisation der Besitzer des benutzerdefinierten Domänennamens ist.
   
   ![Abrufen von DNS-Eintragsinformationen](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

Nach dem Hinzufügen des Domänennamens muss Azure AD überprüfen, ob der Domänenname Ihrer Organisation gehört. Dazu muss allerdings zuerst der DNS-Zonendatei ein DNS-Eintrag für den Domänennamen hinzugefügt werden. Dieser Schritt wird auf der Website für die Registrierung von Domänennamen ausgeführt.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Hinzufügen des DNS-Eintrags bei der Domänennamen-Registrierungsstelle für die Domäne
Der nächste Schritt der Verwendung des benutzerdefinierten Domänennamens mit Azure AD ist die Aktualisierung der DNS-Zonendatei für die Domäne. So kann Azure AD überprüfen, ob sich der benutzerdefinierte Domänenname im Besitz Ihrer Organisation befindet.

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
4. Wählen Sie auf dem Blatt ***Domänenname*** (dem Blatt mit Ihrem neuen Domänennamen im Titel) die Option **Überprüfen** aus, um die Überprüfung durchzuführen.

Nun können Sie [Benutzernamen zuweisen, die Ihren benutzerdefinierten Domänennamen enthalten](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Problembehandlung
Versuchen Sie Folgendes, wenn Sie einen benutzerdefinierten Domänennamen nicht überprüfen können. Wir beginnen mit den häufigsten Ursachen und fahren mit den weniger häufigen fort.

1. **Warten Sie eine Stunde**. DNS-Einträge müssen weitergegeben (propagiert) werden, bevor Azure AD die Domäne überprüfen kann. Dies kann eine Stunde oder auch länger dauern.
2. **Stellen Sie sicher, dass der DNS-Eintrag eingegeben wurde und richtig ist.** Führen Sie diesen Schritt auf der Website der Domänennamen-Registrierungsstelle für die Domäne aus. Azure AD kann den Domänennamen nicht überprüfen, wenn der DNS-Eintrag nicht in der DNS-Zonendatei enthalten ist oder wenn sich keine genaue Übereinstimmung mit dem DNS-Eintrag ergibt, der von Azure AD bereitgestellt wird. Wenn Sie keinen Zugriff für die Aktualisierung von DNS-Einträgen für die Domäne bei der Domänennamen-Registrierungsstelle haben, können Sie den DNS-Eintrag für eine Person bzw. ein Team in Ihrer Organisation mit dem entsprechenden Zugriff bereitstellen und um das Hinzufügen des DNS-Eintrags bitten.
3. **Löschen Sie den Domänennamen aus einem anderen Verzeichnis in Azure AD**. Ein Domänenname kann nur in einem einzelnen Verzeichnis überprüft werden. Falls ein Domänenname bereits in einem anderen Verzeichnis überprüft wurde, muss er darin zuerst gelöscht werden, bevor er im neuen Verzeichnis überprüft werden kann. Informationen zum Löschen von Domänennamen finden Sie unter [Verwalten von benutzerdefinierten Domänennamen](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Hinzufügen weiterer benutzerdefinierter Domänennamen
Wenn Sie in Ihrer Organisation mehrere benutzerdefinierte Domänennamen verwenden, z.B. „contoso.com“ und „contosobank.com“, können Sie maximal 900 Domänennamen hinzufügen. Verwenden Sie die gleichen Schritte in diesem Artikel, um die einzelnen Domänennamen hinzuzufügen.

## <a name="next-steps"></a>Nächste Schritte
[Verwalten von benutzerdefinierten Domänennamen](active-directory-domains-manage-azure-portal.md)


