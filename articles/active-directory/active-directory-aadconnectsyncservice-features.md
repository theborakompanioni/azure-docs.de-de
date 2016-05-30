<properties
	pageTitle="Features und Konfiguration des Azure AD Connect-Synchronisierungsdiensts | Microsoft Azure"
	description="Beschreibt die dienstseitigen Features des Azure AD Connect-Synchronisierungsdiensts."
	services="active-directory"
	documentationCenter=""
	authors="andkjell"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/10/2016"
	ms.author="andkjell;markvi"/>

# Features des Azure AD Connect-Synchronisierungsdiensts

Das Synchronisierungsfeature von Azure AD Connect besteht aus zwei Komponenten:

- Der lokalen Komponente **Azure AD Connect-Synchronisierung**, die auch als **Synchronisierungsmodul** bezeichnet wird.
- Dem Service in Azure AD, der auch als **Azure AD Connect-Synchronisierungsdienst** bezeichnet wird.

In diesem Thema wird erläutert, wie die folgenden Features des **Azure AD Connect-Synchronisierungsdiensts** funktionieren und wie Sie diese mit Windows PowerShell konfigurieren können.

Diese Einstellungen werden durch das [Azure Active Directory-Modul für Windows PowerShell](http://aka.ms/aadposh) konfiguriert, und Sie müssen dieses separat von Azure AD Connect herunterladen und installieren, um die Einstellungen konfigurieren zu können. Die dokumentierten Cmdlets wurden in der [Version von März 2016 (Build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1) eingeführt. Wenn Sie nicht über die in diesem Thema dokumentierten Cmdlets verfügen oder diese nicht zu den gleichen Ergebnissen führen, stellen Sie sicher, dass Sie die neueste Version ausführen.

Führen Sie zum Anzeigen der Konfiguration in Ihrem Azure AD-Verzeichnis `Get-MsolDirSyncFeatures` aus. ![Get-MsolDirSyncFeatures – Ergebnis](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Viele dieser Einstellungen können nur von Azure AD Connect geändert werden.

Folgende Einstellungen können durch `Set-MsolDirSyncFeature` konfiguriert werden:

| DirSyncFeature | Kommentar |
| --- | --- |
| [DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) | Ermöglicht es, ein Attribut unter Quarantäne zu stellen, falls es sich um ein Duplikat eines anderen Objekts handelt, damit nicht der Export des gesamten Objekts mit einem Fehler abgebrochen wird. |
| [EnableSoftMatchOnUpn](#userprincipalname-soft-match) | Ermöglicht Objekten den Beitritt zu userPrincipalName zusätzlich zur primären SMTP-Adresse. |
| [SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) | Ermöglicht dem Synchronisierungsmodul die Aktualisierung des userPrincipalName-Attributs für verwaltete/lizenzierte Benutzer (keine Verbundbenutzer). |

Nachdem Sie ein Feature aktiviert haben, kann es nicht wieder deaktiviert werden.

Die folgenden Eigenschaften werden von Azure AD Connect konfiguriert und können von `Set-MsolDirSyncFeature` nicht geändert werden:

| DirSyncFeature | Kommentar |
| --- | --- |
| DeviceWriteback | [Azure AD Connect: Aktivieren des Geräterückschreibens](active-directory-aadconnect-feature-device-writeback.md) |
| DirectoryExtensions | [Azure AD Connect Sync: Verzeichniserweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md) |
| PasswordSync | [Implementieren der Kennwortsynchronisierung mit der Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) |
| UnifiedGroupWriteback | [Vorschau: Gruppenrückschreiben](active-directory-aadconnect-feature-preview.md#group-writeback) |
| UserWriteback | Derzeit nicht unterstützt. |

## Resilienz bei doppelten Attributen
Damit die Bereitstellung von Objekten mit doppelten UPN-/proxyAddress-Werten nicht mit einem Fehler abgebrochen wird, wird das doppelte Attribut unter Quarantäne gestellt und bei Bedarf ein temporärer Wert zugewiesen. Wenn der Konflikt gelöst ist, wird der temporäre UPN-Wert automatisch auf den richtigen Wert gesetzt. Dieses Verhalten kann für UPN und proxyAddress separat aktiviert werden. Weitere Informationen finden Sie unter [Identitätssynchronisierung und Resilienz bei doppelten Attributen](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)

## UserPrincipalName-„Soft Match“
Wenn dieses Feature aktiviert ist, wird ein „Soft Match“ für den UPN sowie für die [primäre SMTP-Adresse](https://support.microsoft.com/kb/2641663) angewendet, die immer aktiviert ist. Ein „Soft Match“ wird verwendet, um vorhandene Cloudbenutzer in Azure AD mit lokalen Benutzern abzugleichen.

Die Aktivierung dieses Features ist besonders hilfreich, wenn Sie lokale AD-Konten mit vorhandenen in der Cloud erstellten Konten abgleichen müssen und nicht Exchange Online verwenden. In diesem Szenario gibt es im Allgemeinen keinen Grund dafür, das SMTP-Attribut in der Cloud festzulegen.

Dieses Feature ist standardmäßig für neu erstellte Azure AD-Verzeichnisse aktiviert. Sie können ermitteln, ob es aktiviert ist, indem Sie Folgendes ausführen:
```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Wenn dieses Feature für Ihr Azure AD-Verzeichnis nicht aktiviert ist, können Sie es aktivieren, indem Sie Folgendes ausführen:
```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## Synchronisieren von userPrincipalName-Updates
Früher wurden Updates des userPrincipalName-Attributs mithilfe des Synchronisierungsdiensts vom lokalen Standort blockiert, sofern nicht die beiden folgenden Bedingungen erfüllt waren:

- Der Benutzer ist verwaltet (kein Verbundbenutzer).
- Dem Benutzer wurde keine Lizenz zugewiesen.

Weitere Informationen finden Sie unter [Benutzernamen in Office 365, Azure oder Intune entsprechen nicht dem lokalen UPN oder der alternativen Anmelde-ID.](https://support.microsoft.com/kb/2523192)

Durch Aktivierung dieses Features kann das Synchronisierungsmodul den userPrincipalName aktualisieren, wenn dieser lokal geändert wird und Sie die Kennwortsynchronisierung verwenden. Wenn Sie einen Verbund verwenden, funktioniert das Feature nicht.

Dieses Feature ist standardmäßig für neu erstellte Azure AD-Verzeichnisse aktiviert. Sie können ermitteln, ob es aktiviert ist, indem Sie Folgendes ausführen:
```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Wenn dieses Feature für Ihr Azure AD-Verzeichnis nicht aktiviert ist, können Sie es aktivieren, indem Sie Folgendes ausführen:
```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Nach Aktivierung dieses Features bleiben vorhandene userPrincipalName-Werte unverändert. Nach der nächsten lokalen Änderung des userPrincipalName-Attributs wird der UPN im Rahmen der normalen Deltasynchronisierung der Benutzer aktualisiert.

## Zukünftige Änderungen
Diese Einstellungen werden für alle Azure AD-Verzeichnisse in Zukunft aktiviert.

## Weitere Informationen

- [Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-whatis.md)

- [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

<!---HONumber=AcomDC_0518_2016-->