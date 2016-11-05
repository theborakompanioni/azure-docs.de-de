---
title: 'Azure Active Directory Domain Services: Leitfaden zur Problembehandlung | Microsoft Docs'
description: Leitfaden zur Problembehandlung für die Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand

ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2016
ms.author: maheshu

---
# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD Domain Services – Leitfaden zur Problembehandlung
Dieser Artikel enthält Tipps zur Behandlung von Problemen, die beim Einrichten oder Verwalten der Azure Active Directory Domain Services auftreten können.

### <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Sie können Azure AD Domain Services für Ihr Azure AD-Verzeichnis nicht aktivieren
Wenn Sie versuchen, die Azure AD Domain Services für Ihr Verzeichnis zu aktivieren und hierbei ein Fehler auftritt oder der Status anschließend wieder als „Deaktiviert“ angezeigt wird, führen Sie die folgenden Schritte zur Problembehandlung aus:

* Vergewissern Sie sich, dass im gleichen virtuellen Netzwerk keine Domäne mit dem gleichen Domänennamen besteht. Angenommen, im ausgewählten virtuellen Netzwerk befindet sich eine Domäne namens „contoso.com“. Später versuchen Sie, in diesem virtuellen Netzwerk eine verwaltete Domäne der Azure AD Domain Services mit dem gleichen Domänennamen (also „contoso.com“) zu aktivieren. Beim Aktivieren der Azure AD Domain Services tritt daraufhin ein Fehler auf. Der Grund für diesen Fehler ist ein Namenskonflikt in Bezug auf den Domänennamen in diesem virtuellen Netzwerk. In dem Fall müssen Sie einen anderen Namen verwenden, um die verwaltete Domäne der Azure AD Domain Services einzurichten. Alternativ können Sie auch die Bereitstellung der bestehenden Domäne aufheben und mit der Aktivierung der Azure AD Domain Services fortfahren.
* Prüfen Sie, ob sich in Ihrem Azure AD-Verzeichnis eine Anwendung namens „Azure AD Domain Services Sync“ befindet. In diesem Fall müssen Sie die Anwendung löschen und die Azure AD Domain Services erneut aktivieren. Führen Sie die folgenden Schritte aus, um zu prüfen, ob diese Anwendung vorhanden ist, und sie ggf. zu löschen:
  
  1. Navigieren Sie zum **klassischen Azure-Portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
  2. Wählen Sie den Knoten **Active Directory** im linken Bereich aus.
  3. Wählen Sie den Azure AD-Mandanten (Verzeichnis) aus, für den Sie die Azure AD Domain Services aktivieren möchten.
  4. Navigieren Sie zur Registerkarte **Anwendungen** .
  5. Wählen Sie in der Dropdownliste die Option **Anwendungen im Besitz meines Unternehmens** aus.
  6. Suchen Sie nach einer Anwendung mit dem Namen **Azure AD Domain Services Sync**. Wenn die Anwendung vorhanden ist, löschen Sie sie.
  7. Aktivieren Sie die Azure AD Domain Services erneut, sobald Sie die Anwendung gelöscht haben.

### <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Benutzer können sich nicht bei der verwalteten Domäne der Azure AD Domain Services anmelden
Falls sich mindestens ein Benutzer innerhalb Ihres Azure AD-Mandanten nicht bei der neu erstellten verwalteten Domäne anmelden kann, führen Sie die folgenden Problembehandlungsschritte aus:

* Versuchen Sie, sich unter Verwendung des UPN-Formats (z.B. 'joeuser@contoso.com')) statt mit dem SAMAccountName-Format (CONTOSO\joeuser) anzumelden. Gelegentlich wird der SAMAccountName automatisch für Benutzer generiert, deren UPN-Präfix übermäßig lang ist oder deren Präfix mit dem eines anderen Benutzers in der verwalteten Domäne übereinstimmt. Das UPN-Format ist innerhalb eines Azure AD-Mandanten garantiert eindeutig.

> [!NOTE]
> Es wird empfohlen, das UPN-Format zu verwenden, um sich bei der verwalteten Azure AD Domain Services-Domäne anzumelden.
> 
> 

* Stellen Sie sicher, dass Sie die [Aktivierung der Kennwortsynchronisierung](active-directory-ds-getting-started-password-sync.md) in Übereinstimmung mit den Schritten durchgeführt haben, die im Leitfaden zu den ersten Schritten angegeben sind.
* **Externe Konten:** Stellen Sie sicher, dass das betroffene Benutzerkonto kein externes Konto im Azure AD-Mandanten ist. Beispiele für externe Konten sind Microsoft-Konten (z.B. 'joe@live.com')) oder Benutzerkonten aus einem externen Azure AD-Verzeichnis. Da die Azure AD Domain Services nicht über Anmeldeinformationen für diese Benutzerkonten verfügen, können sich diese Benutzer nicht an der verwalteten Domäne anmelden.
* **Synchronisierte Konten** : Falls die betroffenen Benutzerkonten über ein lokales Verzeichnis synchronisiert werden, prüfen Sie Folgendes:
  
  * Sie haben die [neueste empfohlene Version von Azure AD Connect](active-directory-ds-getting-started-password-sync.md#install-or-update-azure-ad-connect) bereitgestellt bzw. das Update darauf durchgeführt.
  * Sie haben Azure AD Connect so konfiguriert, dass [eine vollständige Synchronisierung ausgeführt wird](active-directory-ds-getting-started-password-sync.md).
  * Je nach Größe Ihres Verzeichnisses kann es einige Zeit dauern, bis die Benutzerkonten und Anmeldeinformationshashes in den Azure AD Domain Services verfügbar sind. Warten Sie lange genug, bevor Sie versuchen, die Authentifizierung erneut durchzuführen (abhängig von der Größe Ihres Verzeichnisses – für große Verzeichnisse zwischen einigen Stunden und ein bis zwei Tagen).
  * Wenn das Problem nach Überprüfung der oben genannten Schritte weiterhin auftritt, starten Sie den Microsoft Azure AD Sync-Dienst neu. Öffnen Sie auf Ihrem Synchronisierungscomputer eine Eingabeaufforderung, und führen Sie die folgenden Befehle aus:
    
    1. net stop 'Microsoft Azure AD Sync'
    2. net start 'Microsoft Azure AD Sync'
* **Nur-Cloudkonten**: Wenn das betroffene Benutzerkonto nur für die Cloud gilt, müssen Sie sicherstellen, dass der Benutzer sein Kennwort geändert hat, nachdem Sie die Azure AD Domain Services aktiviert haben. Dieser Schritt führt dazu, dass die Anmeldeinformationshashes für die Azure AD Domain Services generiert werden.

### <a name="contact-us"></a>So erreichen Sie uns
Wenden Sie sich an das Produktteam der Azure Active Directory Domain Services, um [Feedback zu geben oder Support zu erhalten](active-directory-ds-contact-us.md).

<!--HONumber=Oct16_HO2-->


