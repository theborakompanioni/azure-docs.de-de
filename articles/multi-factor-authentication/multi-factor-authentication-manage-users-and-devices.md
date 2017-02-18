---
title: "Verwaltung von Benutzern und Geräten durch Administratoren – Azure MFA | Microsoft-Dokumentation"
description: "Hier wird das Ändern von Benutzereinstellungen beschrieben, z. B. wie Benutzer dazu gebracht werden, den Nachweis-Prozess noch einmal durchzuführen."
documentationcenter: 
services: multi-factor-authentication
author: kgremban
manager: femila
editor: yossib
ms.assetid: aac3b922-7cc1-428c-9044-273579aa7b5a
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 27ff53646992308d574dcc2e631cd63b8227f9c8
ms.openlocfilehash: 826fc2b2eaaf180d922c7a9a4c329ec4379c2ae0


---
# <a name="managing-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Verwalten von Benutzereinstellungen mit Azure Multi-Factor Authentication (MFA) in der Cloud
Als Administrator können Sie die folgenden Einstellungen für Benutzer und Geräte verwalten.  

* [Ausgewählte Benutzer müssen Kontaktmethoden erneut bereitstellen.](#require-selected-users-to-provide-contact-methods-again)
* [Löschen vorhandener App-Kennwörter eines Benutzers](#delete-users-existing-app-passwords)
* [Wiederherstellen der MFA auf allen ausgesetzten Geräten eines Benutzers](#restore-mfa-on-all-suspended-devices-for-a-user)

Dies ist hilfreich, wenn ein Computer oder Gerät verloren geht oder gestohlen wird, oder wenn Sie einen Benutzerzugriff entfernen müssen.

## <a name="require-selected-users-to-provide-contact-methods-again"></a>Ausgewählte Benutzer müssen Kontaktmethoden erneut bereitstellen.
Durch diese Einstellung wird der Benutzer gezwungen, die Registrierung bei der Anmeldung erneut durchzuführen. Denken Sie daran, dass Nicht-Browser-Apps weiterhin funktionieren, wenn der Benutzer über App-Kennwörter dafür verfügt.  Sie können die App-Kennwörter eines Benutzers löschen, indem Sie zusätzlich **Löschen aller vorhandener App-Kennwörter, die von ausgewählten Benutzern generiert wurden**auswählen.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Benutzer dazu bringen, Kontaktmethoden erneut bereitzustellen
1. Melden Sie sich beim klassischen Azure-Portal an.
2. Klicken Sie im linken Bereich auf "Active Directory".
3. Klicken Sie unter "Directory" auf das Verzeichnis für den Benutzer, der seine Kontaktmethode erneut bereitstellen soll.
4. Klicken Sie oben auf "Benutzer".
5. Klicken Sie unten auf der Seite auf "Verwalten der Multi-Factor Authentication". Die Seite für die mehrstufige Authentifizierung wird geöffnet.
6. Suchen Sie den Benutzer, den Sie verwalten möchten, und aktivieren Sie das Kontrollkästchen neben seinem Namen. Sie müssen möglicherweise oben die Ansicht ändern.
7. Hierdurch wird rechts der Link **Benutzereinstellungen verwalten** angezeigt. Klicken Sie auf diese Schaltfläche.
8. Aktivieren Sie das Kontrollkästchen **Bereitstellen der Kontaktmethoden bei ausgewählten Benutzern erneut anfordern**.
   ![Kontaktmethoden bereitstellen](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
9. Klicken Sie auf "Speichern".
10. Klicken Sie auf "Schließen".

## <a name="delete-users-existing-app-passwords"></a>Löschen vorhandener App-Kennwörter eines Benutzers
Dies löscht alle App-Kennwörter, die ein Benutzer erstellt hat. Nicht-Browser-Apps, die diesen App-Kennwörtern zugeordnet waren, sind bis zur Erstellung eines neuen App-Kennworts nicht mehr funktionsfähig.

### <a name="how-to-delete-users-existing-app-passwords"></a>Löschen vorhandener App-Kennwörter eines Benutzers
1. Melden Sie sich beim klassischen Azure-Portal an.
2. Klicken Sie im linken Bereich auf "Active Directory".
3. Klicken Sie unter "Verzeichnis" auf das Verzeichnis für den Benutzer, dem Sie die App-Kennwörter löschen möchten.
4. Klicken Sie oben auf "Benutzer".
5. Klicken Sie unten auf der Seite auf "Verwalten der Multi-Factor Authentication". Die Seite für die mehrstufige Authentifizierung wird geöffnet.
6. Suchen Sie den Benutzer, den Sie verwalten möchten, und aktivieren Sie das Kontrollkästchen neben seinem Namen. Sie müssen möglicherweise oben die Ansicht ändern.
7. Hierdurch wird rechts der Link **Benutzereinstellungen verwalten** angezeigt. Klicken Sie auf diese Schaltfläche.
8. Aktivieren Sie das Kontrollkästchen **Alle vorhandenen App-Kennwörter löschen, die von den ausgewählten Benutzern erstellt wurden**.
   ![App-Kennwörter löschen](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
9. Klicken Sie auf "Speichern".
10. Klicken Sie auf "Schließen".

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Wiederherstellen der MFA auf allen gespeicherten Geräten für einen Benutzer
Administratoren haben die Möglichkeit, die Multi-Factor Authentication auf Geräten und in Browsern von Benutzern zurückzusetzen. Dabei wird die gespeicherte MFA auf allen Geräten und in allen Browsern der Benutzer entfernt, und die Benutzer müssen bei ihrer nächsten Anmeldung MFA verwenden.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>MFA auf allen ausgesetzten Geräten für einen Benutzer wiederherstellen
1. Melden Sie sich beim klassischen Azure-Portal an.
2. Klicken Sie im linken Bereich auf "Active Directory".
3. Klicken Sie unter "Verzeichnis" auf das Verzeichnis für den Benutzer, für den die MFA wiederhergestellt werden soll.
4. Klicken Sie oben auf "Benutzer".
5. Klicken Sie unten auf der Seite auf "Verwalten der Multi-Factor Authentication". Die Seite für die mehrstufige Authentifizierung wird geöffnet.
6. Suchen Sie den Benutzer, den Sie verwalten möchten, und aktivieren Sie das Kontrollkästchen neben seinem Namen. Sie müssen möglicherweise oben die Ansicht ändern.
7. Hierdurch wird rechts der Link **Benutzereinstellungen verwalten** angezeigt. Klicken Sie auf diese Schaltfläche.
8. Aktivieren Sie das Kontrollkästchen **Mehrstufige Authentifizierung für alle gespeicherten Geräte wiederherstellen**.
   ![App-Kennwörter löschen](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Klicken Sie auf "Speichern".
10. Klicken Sie auf "Schließen".



<!--HONumber=Jan17_HO4-->


