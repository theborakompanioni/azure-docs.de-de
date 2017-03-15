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
ms.date: 02/23/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 847a8bdcf880b56f587f6759058825fd1965d29e
ms.openlocfilehash: 43ab735b91bf3f3f1e9631067827f2c456dd7b72
ms.lasthandoff: 03/01/2017


---
# <a name="managing-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Verwalten von Benutzereinstellungen mit Azure Multi-Factor Authentication (MFA) in der Cloud
Als Administrator können Sie die folgenden Einstellungen für Benutzer und Geräte verwalten:

* Ausgewählte Benutzer müssen Kontaktmethoden erneut bereitstellen.
* Löschen vorhandener App-Kennwörter eines Benutzers
* Wiederherstellen der MFA auf allen ausgesetzten Geräten eines Benutzers

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
Eines der konfigurierbaren Features von Azure Multi-Factor Authentication besteht darin, Ihren Benutzern die Option zum Markieren von Geräten als vertrauenswürdig bereitzustellen. Weitere Informationen finden Sie unter [Konfigurieren der Einstellungen von Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md#remember-multi-factor-authentication-for-devices-that-users-trust).

Benutzer können die Überprüfung in zwei Schritten für eine konfigurierbare Anzahl von Tagen auf ihren regulären Geräten deaktivieren. Wenn ein Konto kompromittiert wird oder ein vertrauenswürdiges Gerät verloren geht, müssen Sie in der Lage sein, den Status „Vertrauenswürdig“ aufzuheben und die Überprüfung in zwei Schritten wieder anzufordern.

Die Einstellung **Mehrstufige Authentifizierung für alle gespeicherten Geräte wiederherstellen** bewirkt, dass der Benutzer bei der nächsten Anmeldung die Überprüfung in zwei Schritten durchführen muss, unabhängig davon, ob das Gerät als vertrauenswürdig markiert wurde. 

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

