---
title: "Verwaltung von Benutzern und Geräten durch Administratoren – Azure MFA | Microsoft-Dokumentation"
description: "Hier wird das Ändern von Benutzereinstellungen beschrieben, z. B. wie Benutzer dazu gebracht werden, den Nachweis-Prozess noch einmal durchzuführen."
documentationcenter: 
services: multi-factor-authentication
author: kgremban
manager: femila
ms.assetid: aac3b922-7cc1-428c-9044-273579aa7b5a
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 4c2be7c35f678430d0ad83a3374ef25f68fd2509
ms.openlocfilehash: e9b8504d4a59cf0fae69a4e975d6f834028066d5
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Verwalten von Benutzereinstellungen mit Azure Multi-Factor Authentication (MFA) in der Cloud
Als Administrator können Sie die folgenden Einstellungen für Benutzer und Geräte verwalten:

* Benutzer auffordern, Kontaktmethoden erneut bereitzustellen
* App-Kennwörter löschen
* MFA auf allen vertrauenswürdigen Geräten erfordern 

## <a name="require-users-to-provide-contact-methods-again"></a>Benutzer auffordern, Kontaktmethoden erneut bereitzustellen
Durch diese Einstellung wird der Benutzer gezwungen, den Registrierungsprozess erneut durchzuführen. Nicht-Browser-Apps funktionieren weiterhin, wenn der Benutzer über App-Kennwörter dafür verfügt.  Sie können die App-Kennwörter eines Benutzers löschen, indem Sie zusätzlich **Löschen aller vorhandener App-Kennwörter, die von ausgewählten Benutzern generiert wurden**auswählen.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Benutzer dazu bringen, Kontaktmethoden erneut bereitzustellen
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie auf der linken Seite **Azure Active Directory** > **Benutzer und Gruppen** > **Alle Benutzer** aus.
3. Wählen Sie **Multi-Factor Authentication** aus. Die Seite „Multi-Factor Authentication“ wird geöffnet. 
4. Aktivieren Sie das Kontrollkästchen neben den Benutzern, die Sie verwalten möchten. Auf der rechten Seite wird eine Liste mit Optionen für schnelle Schritte angezeigt. 
5. Wählen Sie **Benutzereinstellungen verwalten** aus.
6. Aktivieren Sie das Kontrollkästchen **Bereitstellen der Kontaktmethoden bei ausgewählten Benutzern erneut anfordern**.
   ![Kontaktmethoden bereitstellen](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
7. Klicken Sie auf **Speichern**.
8. Klicken Sie auf **Schließen**.

## <a name="delete-users-existing-app-passwords"></a>Löschen vorhandener App-Kennwörter eines Benutzers
Diese Einstellung löscht alle App-Kennwörter, die ein Benutzer erstellt hat. Nicht-Browser-Apps, die diesen App-Kennwörtern zugeordnet waren, sind bis zur Erstellung eines neuen App-Kennworts nicht mehr funktionsfähig.

### <a name="how-to-delete-users-existing-app-passwords"></a>Löschen vorhandener App-Kennwörter eines Benutzers
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie auf der linken Seite **Azure Active Directory** > **Benutzer und Gruppen** > **Alle Benutzer** aus.
3. Wählen Sie **Multi-Factor Authentication** aus. Die Seite „Multi-Factor Authentication“ wird geöffnet. 
6. Aktivieren Sie das Kontrollkästchen neben den Benutzern, die Sie verwalten möchten. Auf der rechten Seite wird eine Liste mit Optionen für schnelle Schritte angezeigt. 
7. Wählen Sie **Benutzereinstellungen verwalten** aus.
8. Aktivieren Sie das Kontrollkästchen **Alle vorhandenen App-Kennwörter löschen, die von den ausgewählten Benutzern erstellt wurden**.
   ![App-Kennwörter löschen](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
9. Klicken Sie auf **Speichern**.
10. Klicken Sie auf **Schließen**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Wiederherstellen der MFA auf allen gespeicherten Geräten für einen Benutzer
Eines der konfigurierbaren Features von Azure Multi-Factor Authentication besteht darin, Ihren Benutzern die Option zum Markieren von Geräten als vertrauenswürdig bereitzustellen. Weitere Informationen finden Sie unter [Konfigurieren der Einstellungen von Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md#remember-multi-factor-authentication-for-devices-that-users-trust).

Benutzer können die Überprüfung in zwei Schritten für eine konfigurierbare Anzahl von Tagen auf ihren regulären Geräten deaktivieren. Wenn ein Konto kompromittiert wird oder ein vertrauenswürdiges Gerät verloren geht, müssen Sie in der Lage sein, den Status „Vertrauenswürdig“ aufzuheben und die Überprüfung in zwei Schritten wieder anzufordern.

Die Einstellung **Mehrstufige Authentifizierung für alle gespeicherten Geräte wiederherstellen** bewirkt, dass der Benutzer bei der nächsten Anmeldung die Überprüfung in zwei Schritten durchführen muss, unabhängig davon, ob das Gerät als vertrauenswürdig markiert wurde. 

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>MFA auf allen ausgesetzten Geräten für einen Benutzer wiederherstellen
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie auf der linken Seite **Azure Active Directory** > **Benutzer und Gruppen** > **Alle Benutzer** aus.
3. Wählen Sie **Multi-Factor Authentication** aus. Die Seite „Multi-Factor Authentication“ wird geöffnet. 
6. Aktivieren Sie das Kontrollkästchen neben den Benutzern, die Sie verwalten möchten. Auf der rechten Seite wird eine Liste mit Optionen für schnelle Schritte angezeigt. 
7. Wählen Sie **Benutzereinstellungen verwalten** aus.
8. Aktivieren Sie das Kontrollkästchen **Mehrstufige Authentifizierung für alle gespeicherten Geräte wiederherstellen**
   ![App-Kennwörter löschen](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png).
9. Klicken Sie auf **Speichern**.
10. Klicken Sie auf **Schließen**.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Konfigurieren der Einstellungen von Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md).

- Wenn Ihre Benutzer Hilfe benötigen, verweisen Sie sie auf das [Benutzerhandbuch für die Überprüfung in zwei Schritten](./end-user/multi-factor-authentication-end-user.md).

