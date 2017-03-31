---
title: Festlegen von Kennwortablaufrichtlinien in Azure Active Directory | Microsoft Docs
description: "Hier erfahren Sie, wie Sie Ablaufrichtlinien überprüfen und den Ablauf einzelner oder mehrerer Azure Active Directory-Kennwörter ändern."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 6887250c-15d4-4b59-a161-f0380c0f0acb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: ab55d08c78f6c333819dc31936b24ae6797870bb
ms.lasthandoff: 03/28/2017


---
# <a name="set-password-expiration-policies-in-azure-active-directory"></a>Festlegen von Kennwortablaufrichtlinien in Azure Active Directory
> [!IMPORTANT]
> **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen zum [Ändern und Zurücksetzen Ihres eigenen Kennworts](active-directory-passwords-update-your-own-password.md#reset-your-password)weiter.
>
>

Als globaler Administrator für einen Microsoft-Clouddienst können Sie mit dem Microsoft Azure Active Directory-Modul für Windows PowerShell festlegen, dass Benutzerkennwörter nicht ablaufen. Sie können auch Windows PowerShell-Cmdlets verwenden, um die Konfiguration zu entfernen, die angibt, dass Kennwörter nicht ablaufen, oder um anzuzeigen, welche Benutzerkennwörter nicht ablaufen. Dieser Artikel bietet Hilfe für Clouddienste wie Microsoft Intune und Office 365, die auf Microsoft Azure Active Directory als Identitäts- und Verzeichnisdienste zurückgreifen.

> [!NOTE]
> Nur Kennwörter für Benutzerkonten, die nicht über die Verzeichnissynchronisierung synchronisiert werden, kann so konfiguriert werden, dass sie nicht ablaufen. Weitere Informationen über die Verzeichnissynchronisierung finden Sie in der Liste der Themen unter [Fahrplan zur Verzeichnissynchronisierung](https://msdn.microsoft.com/library/azure/hh967642.aspx).
>
>

Damit Sie Windows PowerShell-Cmdlets verwenden können, müssen Sie sie zunächst installieren.

## <a name="what-do-you-want-to-do"></a>Was möchten Sie tun?
* [Überprüfen der Ablaufrichtlinie für ein Kennwort](#how-to-check-expiration-policy-for-a-password)
* [Festlegen, dass ein Kennwort abläuft](#set-a-password-to-expire)
* [Festlegen, dass ein Kennwort nicht abläuft](#set-a-password-to-never-expire)

## <a name="how-to-check-expiration-policy-for-a-password"></a>Überprüfen der Ablaufrichtlinie für ein Kennwort
1. Stellen Sie mit den Anmeldeinformationen des Unternehmensadministrators eine Verbindung mit Windows PowerShell her.
2. Führen Sie einen der folgenden Schritte aus:

   * Um festzustellen, ob für das Kennwort eines bestimmten Benutzers festgelegt ist, dass es nie abläuft, führen Sie das folgende Cmdlet mit dem Benutzerprinzipalnamen (UPN, z. B. aprilr@contoso.onmicrosoft.com) oder der Benutzer-ID des zu überprüfenden Benutzers aus: `Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`
   * Um die Einstellung "Kennwort läuft nie ab" für alle Benutzer anzuzeigen, führen Sie das folgende Cmdlet aus: `Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## <a name="set-a-password-to-expire"></a>Festlegen, dass ein Kennwort abläuft
1. Stellen Sie mit den Anmeldeinformationen des Unternehmensadministrators eine Verbindung mit Windows PowerShell her.
2. Führen Sie einen der folgenden Schritte aus:

   * Um für das Kennwort eines Benutzers festzulegen, dass es abläuft, führen Sie das folgende Cmdlet mit dem Benutzerprinzipalnamen (UPN) oder der Benutzer-ID des Benutzers aus: `Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`
   * Um für die Kennwörter aller Benutzer in der Organisation festzulegen, dass sie ablaufen, verwenden Sie das folgende Cmdlet: `Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

## <a name="set-a-password-to-never-expire"></a>Festlegen, dass ein Kennwort nicht abläuft
1. Stellen Sie mit den Anmeldeinformationen des Unternehmensadministrators eine Verbindung mit Windows PowerShell her.
2. Führen Sie einen der folgenden Schritte aus:

   * Um für das Kennwort eines Benutzers festzulegen, dass es nie abläuft, führen Sie das folgende Cmdlet mit dem Benutzerprinzipalnamen (UPN) oder der Benutzer-ID des Benutzers aus: `Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`
   * Um für die Kennwörter aller Benutzer in einer Organisation festzulegen, dass sie nie ablaufen, führen Sie das folgende Cmdlet aus: `Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Nächste Schritte
* **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen zum [Ändern und Zurücksetzen Ihres eigenen Kennworts](active-directory-passwords-update-your-own-password.md#reset-your-password).

