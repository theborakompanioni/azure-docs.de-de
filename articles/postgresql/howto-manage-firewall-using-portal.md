---
title: "Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals | Microsoft-Dokumentation"
description: "Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: b93827ec35916759c26e5e58186acaff1f160a35
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-the-azure-portal"></a>Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals
Mithilfe von Firewallregeln auf Serverebene können Administratoren über eine bestimmte IP-Adresse oder über einen IP-Adressbereich auf einen Server für Azure-Datenbank für PostgreSQL zugreifen. 

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Einen Server zum [Erstellen einer Azure-Datenbank für PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Erstellen einer Firewallregel auf Serverebene im Azure-Portal
1. Klicken Sie auf dem Blatt des PostgreSQL-Servers unter der Überschrift „Einstellungen“ auf **Verbindungssicherheit**, um das Blatt „Verbindungssicherheit“ für die Azure-Datenbank für PostgreSQL zu öffnen.

  ![Azure-Portal – Klicken auf „Verbindungssicherheit“](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klicken Sie auf der Symbolleiste auf **Meine IP-Adresse hinzufügen**. Dadurch wird automatisch eine Regel mit der IP-Adresse Ihres Computers (wie vom Azure-System erkannt) erstellt.

  ![Azure-Portal – Klicken auf „Meine IP-Adresse hinzufügen“](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Überprüfen Sie Ihre IP-Adresse, bevor Sie die Konfiguration speichern. In einigen Situationen weicht die vom Azure-Portal erkannte IP-Adresse von der IP-Adresse ab, die für den Zugriff auf das Internet und die Azure-Server verwendet wird. Aus diesem Grund müssen Sie die Start-IP und die End-IP ändern, damit die Regel wie erwartet funktioniert.
Verwenden Sie eine Suchmaschine oder ein anderes Onlinetool, um Ihre eigene IP-Adresse zu überprüfen. (Suchen Sie in Bing beispielsweise „Wie lautet meine IP?“.)

  ![Bing-Suche für „Wie lautet meine IP?“](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Fügen Sie weitere Adressräume hinzu. In den Regeln für die Firewall für Azure-Datenbank für PostgreSQL können Sie eine einzelne IP-Adresse oder einen Adressbereich angeben. Wenn Sie die Regel auf eine einzelne IP-Adresse beschränken möchten, geben Sie dieselbe Adresse in das Feld für Start-IP und End-IP ein. Mit dem Öffnen der Firewall wird es Administratoren und Benutzern ermöglicht, sich auf dem PostgreSQL-Server bei allen Datenbanken anzumelden, für die sie über gültige Anmeldeinformationen verfügen.

  ![Azure-Portal – Firewallregeln ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Klicken Sie auf der Symbolleiste auf **Speichern**, um diese Firewallregel auf Serverebene zu speichern. Warten Sie auf die Bestätigung, dass das Update für die Firewallregeln erfolgreich war.

  ![Azure-Portal – Klicken auf „Speichern“](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)


## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Verwalten vorhandener Firewallregeln auf Serverebene über das Azure-Portal
Wiederholen Sie die Schritte zum Verwalten der Firewallregeln.
* Um den aktuellen Computer hinzuzufügen, klicken Sie auf die Schaltfläche **+ Meine IP-Adresse hinzufügen**. Klicken Sie zum Speichern der Änderungen auf **Speichern** .
* Um zusätzliche IP-Adressen hinzuzufügen, geben Sie den Regelnamen, die Start-IP-Adresse und die End-IP-Adresse ein. Klicken Sie zum Speichern der Änderungen auf **Speichern** .
* Um eine vorhandene Regel zu ändern, klicken Sie auf eines der Felder in der Regel, und ändern Sie den betreffenden Wert. Klicken Sie zum Speichern der Änderungen auf **Speichern** .
* Um eine vorhandene Regel zu löschen, klicken Sie auf die Auslassungspunkte [...] und dann auf „Löschen“. Klicken Sie zum Speichern der Änderungen auf **Speichern** .

## <a name="next-steps"></a>Nächste Schritte
- Sie können auch ein Skript zum [Erstellen und Verwalten von Firewallregeln für die Azure-Datenbank für PostgreSQL mithilfe der Azure CLI](howto-manage-firewall-using-cli.md) schreiben.
- Wenn Sie Unterstützung beim Herstellen einer Verbindung mit einem Server für Azure-Datenbank für MySQL benötigen, lesen Sie die Informationen unter [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md) (Verbindungsbibliotheken für Azure-Datenbank für PostgreSQL).

