---
title: "Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe des Azure-Portals | Microsoft-Dokumentation"
description: "Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe des Azure-Portals"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 33198e5a6e11df2db3a17fc96a0b3cd4b1a284e8
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017

---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-the-azure-portal"></a>Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe des Azure-Portals
Mithilfe von Firewallregeln auf Serverebene können Administratoren über eine bestimmte IP-Adresse oder über einen IP-Adressbereich auf einen Server für Azure-Datenbank für MySQL zugreifen. 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Erstellen einer Firewallregel auf Serverebene im Azure-Portal

1. Klicken Sie auf dem Blatt des MySQL-Servers unter der Überschrift „Einstellungen“ auf **Verbindungssicherheit**, um das Blatt „Verbindungssicherheit“ für Azure-Datenbank für MySQL zu öffnen.

   ![Azure-Portal – Klicken auf „Verbindungssicherheit“](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klicken Sie auf der Symbolleiste auf **Meine IP-Adresse hinzufügen**, um eine Regel mit der IP-Adresse Ihres Computers (wie vom Azure-System erkannt) zu erstellen.

   ![Azure-Portal – Klicken auf „Meine IP-Adresse hinzufügen“](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Überprüfen Sie Ihre IP-Adresse, bevor Sie die Konfiguration speichern. In einigen Situationen weicht die vom Azure-Portal erkannte IP-Adresse von der IP-Adresse ab, die für den Zugriff auf das Internet und die Azure-Server verwendet wird. Aus diesem Grund müssen Sie die Start-IP und die End-IP ändern, damit die Regel wie erwartet funktioniert.

   Verwenden Sie eine Suchmaschine oder ein anderes Onlinetool, um Ihre eigene IP-Adresse zu überprüfen. (Suchen Sie beispielsweise „Wie lautet meine IP-Adresse?“.)

   ![Bing-Ergebnisse für „Wie lautet meine IP-Adresse?“](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Fügen Sie weitere Adressräume hinzu. In den Regeln für die Firewall für Azure-Datenbank für MySQL können Sie eine einzelne IP-Adresse oder einen Adressbereich angeben. Wenn Sie die Regel auf eine einzelne IP-Adresse beschränken möchten, geben Sie dieselbe Adresse in das Feld für Start-IP und End-IP ein. Mit dem Öffnen der Firewall wird es Administratoren und Benutzern ermöglicht, auf dem MySQL-Server auf alle Datenbanken zuzugreifen, für die sie über gültige Anmeldeinformationen verfügen.

   ![Azure-Portal – Firewallregeln ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. Klicken Sie auf der Symbolleiste auf **Speichern**, um diese Firewallregel auf Serverebene zu speichern. Warten Sie auf die Bestätigung, dass das Update für die Firewallregeln erfolgreich war.

   ![Azure-Portal – Klicken auf „Speichern“](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Verwalten vorhandener Firewallregeln auf Serverebene über das Azure-Portal
Wiederholen Sie die Schritte zum Verwalten der Firewallregeln.
* Um den aktuellen Computer hinzuzufügen, klicken Sie auf **+ Meine IP-Adresse hinzufügen**.
* Um zusätzliche IP-Adressen hinzuzufügen, geben Sie **REGELNAME**, **START-IP** und **END-IP** ein.
* Um eine vorhandene Regel zu ändern, klicken Sie auf eines der Felder in der Regel, und ändern Sie den betreffenden Wert.
* Um eine vorhandene Regel zu löschen, klicken Sie auf die Auslassungspunkte [...] und dann auf **Löschen**.
* Klicken Sie zum Speichern der Änderungen auf **Speichern** .

## <a name="next-steps"></a>Nächste Schritte
- Wenn Sie Unterstützung beim Herstellen einer Verbindung mit einem Server für Azure-Datenbank für MySQL benötigen, lesen Sie die Informationen unter [Connection libraries for Azure Database for MySQL](./concepts-connection-libraries.md) (Verbindungsbibliotheken für Azure-Datenbank für MySQL).

