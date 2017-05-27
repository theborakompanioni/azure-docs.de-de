---
title: "Schnellstart: Erstellen einer Azure-Datenbank für MySQL-Server – Azure-Portal | Microsoft-Dokumentation"
description: "Dieser Artikel führt Sie durch die Verwendung des Azure-Portals, um in etwa fünf Minuten schnell eine Azure-Beispieldatenbank für MySQL-Server zu erstellen."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: portal
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: 85bb0396809297f0efb7323bef081055f17ede62
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Erstellen einer Azure-Datenbank für MySQL-Server mithilfe des Azure-Portals

Dieser Artikel führt Sie durch die Verwendung des Azure-Portals, um in etwa fünf Minuten eine Azure-Beispieldatenbank für MySQL-Server zu erstellen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure
Öffnen Sie Ihren Webbrowser, und navigieren Sie zum [Microsoft Azure-Portal](https://portal.azure.com/). Geben Sie Ihre Anmeldeinformationen ein, um sich im Portal anzumelden. Die Standardansicht ist Ihr Dienstdashboard.

## <a name="create-azure-database-for-mysql-server"></a>Erstellen einer Azure-Datenbank für MySQL-Server

1. Navigieren Sie zu **Datenbanken** > **MySQL**. Wenn Sie eine Azure-Datenbank für MySQL-Server unter der Kategorie **Datenbanken** nicht finden können, klicken Sie auf **Alle anzeigen**, um alle verfügbaren Datenbankdienste anzuzeigen. Sie können auch **MySQL** in das Suchfeld eingeben, um den Dienst schnell zu finden.
![Azure-Portal – Neu – Datenbank – MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

2. Klicken Sie auf das **MySQL**-Symbol und anschließend auf **Erstellen**.
Füllen Sie in unserem Beispiel die Seite „Azure-Datenbank für MySQL“ mit den folgenden Informationen:

| **Formularfeld** | **Feldbeschreibung** |
|----------------|-----------------------|
| *Servername* | mysqlserver4demo (Servername ist global eindeutig) |
| *Abonnement* | MySQLaaS (aus der Dropdownliste auswählen) |
| *Ressourcengruppe* | myresource (Ressourcengruppe erstellen oder eine bereits vorhandene verwenden) |
| *Serveradministratoranmeldung* | myadmin (Administratorkontoname einrichten) |
| *Kennwort* | Kennwort des Administratorkontos einrichten |
| *Kennwort bestätigen* | Kennwort des Administratorkontos bestätigen |
| *Standort* | Europa, Norden (zwischen **Europa, Norden** und **USA, Westen** wählen) |
| *Version* | 5.6 (MySQL-Serverversion auswählen) |
| *Leistung konfigurieren* | Basic (**Leistungsebene**, **Compute-Einheiten**, **Speicher** wählen und dann auf **OK** klicken) |

![Azure-Portal – Erstellen von MySQL durch erforderliche Formulareingaben](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

Nach wenigen Minuten wird Ihre Azure-Datenbank für MySQL-Server bereitgestellt und ausgeführt. Sie können in der Symbolleiste auf die Schaltfläche **Benachrichtigungen** (Glockensymbol) klicken, um den Bereitstellungsprozess zu überwachen.

> [!TIP]
> Sie sollten Ihre Azure-Dienste in der gleichen Region einrichten und den nächstgelegenen Standort wählen. Außerdem können Sie die Option **An Dashboard anheften** aktivieren, um leichtes Nachverfolgen Ihrer Bereitstellungen zu ermöglichen.

## <a name="configure-the-firewall"></a>Konfigurieren der Firewall
Vor dem erstmaligen Herstellen einer Verbindung mit Azure-Datenbank für MySQL von Ihrem Client aus müssen Sie die Firewall konfigurieren und die öffentliche Netzwerk-IP-Adresse (oder den IP-Adressbereich) des Clients der Whitelist hinzufügen.

1. Klicken Sie auf den neu erstellten Server und dann auf **Einstellungen**.
  ![Azure-Portal – MySQL – Schaltfläche „Einstellungen“](./media/quickstart-create-mysql-server-database-using-azure-portal/4_server-settings.png)

2. Klicken Sie im Abschnitt **Allgemein** auf **Firewalleinstellungen**. Klicken Sie auf **Meine IP-Adresse hinzufügen**, um die IP-Adresse des lokalen Computers hinzuzufügen, oder konfigurieren Sie einen Bereich von IP-Adressen. Denken Sie nach dem Erstellen der Regeln daran, auf **Speichern** zu klicken.
  ![Azure-Portal – Firewallregel hinzufügen und speichern](./media/quickstart-create-mysql-server-database-using-azure-portal/5_firewall-settings.png)

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie den vollqualifizierten Domänennamen für Ihren Azure MySQL-Server im Azure-Portal ab. Sie verwenden den vollqualifizierten Domänennamen, um mit dem Befehlszeilentool **mysql.exe** eine Verbindung zu Ihrem Server herzustellen.

1.    Klicken Sie im [Azure-Portal](https://portal.azure.com/) im linken Menü auf **Alle Ressourcen** und dann auf Ihre Azure-Datenbank für MySQL-Server.

2.    Klicken Sie auf **Eigenschaften**. Notieren Sie sich **SERVERNAME** und **SERVERADMINISTRATORANMELDUNG**.
In diesem Beispiel lautet der Servername *mysql4doc.database.windows.net* und die Serveradministratoranmeldung *mysqladmin@mysql4doc*.

## <a name="connect-to-the-server-using-mysqlexe-command-line-tool"></a>Herstellen einer Verbindung zum Server mit dem Befehlszeilentool „mysql.exe“
Sie können mehrere Datenbanken in einem MySQL-Server erstellen. Es gibt keine Beschränkung der Anzahl der Datenbanken, die erstellt werden können, aber mehrere Datenbanken verwenden dieselben Serverressourcen.  Öffnen Sie zum Herstellen einer Verbindung zum Server mit dem Befehlszeilentool **mysql.exe** die **Azure Cloud Shell** im Portal, und geben Sie Folgendes ein:

1. Stellen Sie eine Verbindung zum Server mit dem Befehlszeilentool **mysql** her:
```dos
 mysql -h mysqlserver4demo.database.windows.net -u myadmin@mysqlserver4demo -p
```

2. Zeigen Sie den Serverstatus an:
```dos
 mysql> status
```
  ![Eingabeaufforderung – Beispiel für mysql-Befehlszeile](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

> [!TIP]
> Zusätzliche Befehle finden Sie unter [MySQL 5.6-Referenzhandbuch – Kapitel 4.5.1 (in englischer Sprache)](https://dev.mysql.com/doc/refman/5.6/en/mysql.html).

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Herstellen einer Verbindung zum Server mit dem MySQL Workbench-GUI-Tool
1.    Starten Sie die Anwendung MySQL Workbench auf dem Clientcomputer. Sie können MySQL Workbench [hier](https://dev.mysql.com/downloads/workbench/) herunterladen und installieren.

2.    Geben Sie im Dialogfeld **Setup New Connection** (Neue Verbindung einrichten) auf der Registerkarte **Parameter** folgende Informationen ein:

| **Parameter** | **Beschreibung** |
|----------------|-----------------|
|    *Verbindungsname* | Geben Sie einen beliebigen Namen für diese Verbindung ein |
| *Verbindungsmethode* | Wählen Sie „Standard (TCP/IP)“ aus |
| *Hostname* | mycliserver.database.windows.net (der SERVERNAME, den Sie sich zuvor notiert haben) |
| *Port* | 3306 |
| *Benutzername* | myadmin@mycliserver (die SERVERADMINISTRATORANMELDUNG, die Sie sich zuvor notiert haben) |
| *Kennwort* | Sie können das Kennwort für das Administratorkonto im Tresor speichern |

![Einrichten einer neuen Verbindung](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

3.    Klicken Sie auf **Verbindung testen**, um zu testen, ob alle Parameter richtig konfiguriert wurden.

4.    Nun können Sie auf die Verbindung klicken, die gerade erstellt wurde, um eine Verbindung zum Server herzustellen.

> SSL wird standardmäßig auf dem Server erzwungen, was zusätzliche Konfiguration für eine erfolgreiche Verbindung erfordert. Siehe [Konfigurieren von SSL-Verbindungen in der Anwendung für eine sichere Verbindung mit Azure-Datenbank für MySQL](./howto-configure-ssl.md).  Wenn Sie SSL für diesen Schnellstart deaktivieren möchten, können Sie im Portal unter „Verbindungssicherheit“ das Erzwingen von SSL deaktivieren.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht für einen anderen Schnellstart bzw. ein anderes Tutorial benötigen, können Sie sie wie folgt löschen:

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen** und dann auf **myresource**. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld **myresource** ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Entwerfen Sie Ihre erste Azure-Datenbank für MySQL-Datenbank](./tutorial-design-database-using-portal.md)


