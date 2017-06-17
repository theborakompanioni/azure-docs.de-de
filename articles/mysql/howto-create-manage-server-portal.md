---
title: "Erstellen und Verwalten eines Servers mit Azure-Datenbank für MySQL im Azure-Portal | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie schnell einen neuen Server mit Azure-Datenbank für MySQL erstellen und mithilfe des Azure-Portals verwalten."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: cbf1bcd90c53a7221773c7dc7731b37cf762e953
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Erstellen und Verwalten eines Servers mit Azure-Datenbank für MySQL im Azure-Portal
In diesem Artikel wird beschrieben, wie Sie schnell einen neuen Server mit Azure-Datenbank für MySQL erstellen und mithilfe des Azure-Portals verwalten. Die Serververwaltung umfasst das Anzeigen von Serverdetails und Datenbanken, die Kennwortzurücksetzung und das Löschen des Servers.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal
Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

## <a name="create-an-azure-database-for-mysql-server"></a>Erstellen eines Servers für Azure-Datenbank für MySQL
Führen Sie die folgenden Schritte aus, um einen Server mit Azure-Datenbank für MySQL mit dem Namen „mysqlserver4demo“ zu erstellen:

1: Klicken Sie im Azure-Portal oben links auf die Schaltfläche **Neu**.

2: Wählen Sie auf der Seite „Neu“ die Option **Datenbanken** und dann auf der Seite „Datenbanken“ die Option **Azure-Datenbank für MySQL** aus.

> Ein Server mit Azure-Datenbank für MySQL wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](./concepts-compute-unit-and-storage.md) erstellt. Die Datenbank wird in einer Azure-Ressourcengruppe und auf einem Server mit Azure-Datenbank für MySQL erstellt.

![create-new-server](./media/howto-create-manage-server-portal/create-new-server.png)

3: Füllen Sie das Azure-Datenbank für MySQL-Formular mit den folgenden Informationen aus:

| **Formularfeld** | **Feldbeschreibung** |
|----------------|-----------------------|
| *Servername* | azure-mysql (Servername ist global eindeutig) |
| *Abonnement* | MySQLaaS (aus der Dropdownliste auswählen) |
| *Ressourcengruppe* | myresource (neue Ressourcengruppe erstellen oder eine bereits vorhandene verwenden) |
| *Serveradministratoranmeldung* | myadmin (Administratorkontoname einrichten) |
| *Kennwort* | Kennwort des Administratorkontos einrichten |
| *Kennwort bestätigen* | Kennwort des Administratorkontos bestätigen |
| *Standort* | Europa, Norden (zwischen „Europa, Norden“ und „USA, Westen“ wählen) |
| *Version* | 5.6 (Serverversion für Azure-Datenbank für MySQL wählen) |

4: Klicken Sie auf **Tarif**, um die Dienstebene und die Leistungsstufe für den neue Server anzugeben. Für die Compute-Einheit kann zwischen 50 und 100 im Basic-Tarif und zwischen 100 und 200 im Standard-Tarif konfiguriert werden, und Speicher kann basierend auf der enthaltenen Menge hinzugefügt werden. Für diese Anleitung wählen wir 50 für die Compute-Einheit und 50 GB aus. Klicken Sie zum Speichern der Auswahl auf **OK** .
![create-server-pricing-tier](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5: Klicken Sie auf **Erstellen**, um den Server bereitzustellen. Die Bereitstellung dauert einige Minuten.

> Aktivieren Sie die Option **An Dashboard anheften**, um ein leichtes Nachverfolgen Ihrer Bereitstellungen zu ermöglichen.
> [!NOTE]
> Für den Speicher werden zwar bis zu 1000 GB im Basic-Tarif und 10000 GB im Standard-Tarif unterstützt, für die öffentliche Vorschau ist die maximale Speichergröße jedoch weiterhin vorübergehend auf 1000 GB beschränkt. 
</Include>

## <a name="update-an-azure-database-for-mysql-server"></a>Aktualisieren eines Servers mit Azure-Datenbank für MySQL
Nach der Bereitstellung des neuen Servers hat der Benutzer zwei Optionen zum Bearbeiten eines vorhandenen Servers: Zurücksetzen des Administratorkennworts oder zentrales Hoch/Herunterskalieren des Servers durch Ändern der Compute-Einheiten.

### <a name="change-the-administrator-user-password"></a>Ändern des Administratorbenutzerkennworts
1: Klicken Sie auf dem Serverblatt **Übersicht** auf **Kennwort zurücksetzen**, um eine Kennworteingabe und ein Bestätigungsfenster auszufüllen.

2: Geben Sie ein neues Kennwort ein, und bestätigen Sie es wie unten gezeigt im Fenster: ![Kennwort zurücksetzen](./media/howto-create-manage-server-portal/reset-password.png).

3: Klicken Sie auf **OK**, um das neue Kennwort zu speichern.

### <a name="scale-updown-by-changing-compute-units"></a>Zentrales Hoch/Herunterskalieren des durch Ändern der Compute-Einheiten

1: Klicken Sie auf dem Serverblatt unter **Einstellungen** auf **Tarif**, um das Blatt „Tarif“ für den Server mit Azure-Datenbank für MySQL zu öffnen.

2: Führen Sie Schritt 4 in **Erstellen eines Servers für Azure-Datenbank für MySQL** aus, um Compute-Einheiten im gleichen Tarif zu ändern.

## <a name="delete-an-azure-database-for-mysql-server"></a>Löschen eines Servers mit Azure-Datenbank für MySQL

1: Klicken Sie auf dem Serverblatt **Übersicht** auf die Befehlsschaltfläche **Löschen**, um das Blatt mit der Löschbestätigung zu öffnen.

2: Geben Sie den richtigen Servernamen im Eingabefeld des Blatts zur doppelten Bestätigung ein.

3: Klicken Sie erneut auf die Schaltfläche **Löschen**, um den Löschvorgang zu bestätigen, und warten Sie auf die Popupmeldung auf der Benachrichtigungsleiste, dass das Löschen erfolgreich war.

## <a name="list-the-azure-database-for-mysql-databases"></a>Auflisten der Datenbanken von Azure-Datenbank für MySQL
Scrollen Sie auf dem Serverblatt **Übersicht** nach unten, bis die Datenbankkachel im unteren Bereich angezeigt wird. Alle Datenbanken werden in der Tabelle aufgeführt. Klicken Sie auf die Befehlsschaltfläche **Löschen**, um das Blatt mit der Löschbestätigung zu öffnen.

![show-databases](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Anzeigen von Details zu einem Server mit Azure-Datenbank für MySQL
Wenn Sie auf dem Serverblatt unter **Einstellungen** auf **Eigenschaften** klicken, wird das Blatt **Eigenschaften** geöffnet. Anschließend können Sie alle ausführlichen Informationen zum Server anzeigen.

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Erstellen eines Servers mit Azure-Datenbank für MySQL im Azure-Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)

