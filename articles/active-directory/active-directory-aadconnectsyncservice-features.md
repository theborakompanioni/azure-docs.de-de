<properties
    pageTitle="Features und Konfiguration des Azure AD Connect-Synchronisierungsdiensts | Microsoft Azure"
    description="Beschreibt die dienstseitigen Features des Azure AD Connect-Synchronisierungsdiensts."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-service-features"></a>Features des Azure AD Connect-Synchronisierungsdiensts

Das Synchronisierungsfeature von Azure AD Connect besteht aus zwei Komponenten:

- Der lokalen Komponente **Azure AD Connect-Synchronisierung**, die auch als **Synchronisierungsmodul** bezeichnet wird.
- Dem Service in Azure AD, der auch als **Azure AD Connect-Synchronisierungsdienst**

In diesem Thema wird erläutert, wie die folgenden Features des **Azure AD Connect-Synchronisierungsdiensts** funktionieren und wie Sie diese mit Windows PowerShell konfigurieren können.

Diese Einstellungen werden vom [Azure Active Directory-Modul für Windows PowerShell](http://aka.ms/aadposh)konfiguriert. Laden Sie es herunter, und installieren Sie es separat von Azure AD Connect. Die in diesem Thema dokumentierten Cmdlets wurden in der [Version von März 2016 (Build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1)eingeführt. Wenn Sie nicht über die in diesem Thema dokumentierten Cmdlets verfügen oder diese nicht zu den gleichen Ergebnissen führen, stellen Sie sicher, dass Sie die neueste Version ausführen.

Führen Sie zum Anzeigen der Konfiguration in Ihrem Azure AD-Verzeichnis `Get-MsolDirSyncFeatures`aus.  
![Get-MsolDirSyncFeatures – Ergebnis](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Viele dieser Einstellungen können nur von Azure AD Connect geändert werden.

Folgende Einstellungen können durch `Set-MsolDirSyncFeature`konfiguriert werden:

DirSyncFeature | Kommentar
--- | ---
[DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) | Ermöglicht es, ein Attribut unter Quarantäne zu stellen, falls es sich um ein Duplikat eines anderen Objekts handelt, damit nicht der Export des gesamten Objekts mit einem Fehler abgebrochen wird.
[EnableSoftMatchOnUpn](#userprincipalname-soft-match) | Ermöglicht Objekten den Beitritt zu userPrincipalName zusätzlich zur primären SMTP-Adresse.
[SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) | Ermöglicht dem Synchronisierungsmodul die Aktualisierung des userPrincipalName-Attributs für verwaltete/lizenzierte Benutzer (keine Verbundbenutzer).

Nachdem Sie ein Feature aktiviert haben, kann es nicht wieder deaktiviert werden.

>[AZURE.NOTE] Seit dem 24. August 2016 ist das Feature *Resilienz bei doppelten Attributen* für neue Azure AD-Verzeichnisse standardmäßig aktiviert. Dieses Feature wird auch für Verzeichnisse freigegeben und aktiviert, die vor diesem Datum erstellt wurden. Sie erhalten eine E-Mail-Benachrichtigung, wenn dieses Feature für Ihr Verzeichnis aktiviert wird.

Die folgenden Einstellungen werden von Azure AD Connect konfiguriert und können von `Set-MsolDirSyncFeature` nicht geändert werden:

DirSyncFeature | Kommentar
--- | ---
DeviceWriteback | [Azure AD Connect: Aktivieren des Geräterückschreibens](active-directory-aadconnect-feature-device-writeback.md)
DirectoryExtensions | [Azure AD Connect Sync: Verzeichniserweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md)
PasswordSync | [Implementieren der Kennwortsynchronisierung mit der Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md)
UnifiedGroupWriteback | [Vorschau: Gruppenrückschreiben](active-directory-aadconnect-feature-preview.md#group-writeback)
UserWriteback | Derzeit nicht unterstützt.

## <a name="duplicate-attribute-resiliency"></a>Resilienz bei doppelten Attributen
Damit die Bereitstellung von Objekten mit doppelten UPN-/proxyAddresses-Werten nicht mit einem Fehler abgebrochen wird, wird das doppelte Attribut unter Quarantäne gestellt, und ein temporärer Wert wird zugewiesen. Wenn der Konflikt gelöst ist, wird der temporäre UPN-Wert automatisch auf den richtigen Wert gesetzt. Dieses Verhalten kann für UPN und proxyAddress separat aktiviert werden. Weitere Informationen finden Sie unter [Identitätssynchronisierung und Resilienz bei doppelten Attributen](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>UserPrincipalName-„Soft Match“
Wenn dieses Feature aktiviert ist, wird ein „Soft Match“ für den UPN sowie für die [primäre SMTP-Adresse](https://support.microsoft.com/kb/2641663)aktiviert, die immer aktiviert ist. Ein „Soft Match“ wird verwendet, um vorhandene Cloudbenutzer in Azure AD mit lokalen Benutzern abzugleichen.

Dieses Feature ist besonders hilfreich, wenn Sie lokale AD-Konten mit vorhandenen in der Cloud erstellten Konten abgleichen müssen und Exchange Online nicht verwenden. In diesem Szenario gibt es im Allgemeinen keinen Grund dafür, das SMTP-Attribut in der Cloud festzulegen.

Dieses Feature ist standardmäßig für neu erstellte Azure AD-Verzeichnisse aktiviert. Sie können ermitteln, ob das Feature aktiviert ist, indem Sie Folgendes ausführen:  
```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Wenn dieses Feature für Ihr Azure AD-Verzeichnis nicht aktiviert ist, können Sie es aktivieren, indem Sie Folgendes ausführen:  
```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>Synchronisieren von userPrincipalName-Updates
Früher wurden Updates des userPrincipalName-Attributs mithilfe des Synchronisierungsdiensts vom lokalen Standort blockiert, sofern nicht die beiden folgenden Bedingungen erfüllt waren:

- Der Benutzer ist verwaltet (kein Verbundbenutzer).
- Dem Benutzer wurde keine Lizenz zugewiesen.

Weitere Informationen finden Sie unter [Benutzernamen in Office 365, Azure oder Intune stimmen nicht mit lokaler UPN oder alternativen Benutzernamen überein](https://support.microsoft.com/kb/2523192).

Durch Aktivierung dieses Features kann das Synchronisierungsmodul den userPrincipalName aktualisieren, wenn dieser lokal geändert wird und Sie die Kennwortsynchronisierung verwenden. Wenn Sie einen Verbund verwenden, wird das Feature nicht unterstützt.

Dieses Feature ist standardmäßig für neu erstellte Azure AD-Verzeichnisse aktiviert. Sie können ermitteln, ob das Feature aktiviert ist, indem Sie Folgendes ausführen:  
```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Wenn dieses Feature für Ihr Azure AD-Verzeichnis nicht aktiviert ist, können Sie es aktivieren, indem Sie Folgendes ausführen:  
```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Nach Aktivierung dieses Features bleiben vorhandene userPrincipalName-Werte unverändert. Nach der nächsten lokalen Änderung des userPrincipalName-Attributs wird der UPN im Rahmen der normalen Deltasynchronisierung der Benutzer aktualisiert.  

## <a name="see-also"></a>Siehe auch

- [Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-whatis.md)
- [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)



<!--HONumber=Oct16_HO2-->


