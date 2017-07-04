---
title: "Schnellstart: Erstellen einer Azure-Datenbank für MySQL-Server – Azure-Portal | Microsoft-Dokumentation"
description: "Dieser Artikel führt Sie durch die Verwendung des Azure-Portals, um in etwa fünf Minuten schnell eine Azure-Beispieldatenbank für MySQL-Server zu erstellen."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: hero-article
ms.date: 06/14/2017
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 3e0aa1ecccdb55b75dd332818e09b21431716fdc
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Erstellen einer Azure-Datenbank für MySQL-Server mithilfe des Azure-Portals
Dieser Artikel führt Sie durch die Verwendung des Azure-Portals, um in etwa fünf Minuten eine Azure-Datenbank für MySQL-Server zu erstellen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure
Öffnen Sie Ihren Webbrowser, und navigieren Sie zum [Microsoft Azure-Portal](https://portal.azure.com/). Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden. Die Standardansicht ist Ihr Dienstdashboard.

## <a name="create-azure-database-for-mysql-server"></a>Erstellen einer Azure-Datenbank für MySQL-Server
1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Datenbanken** die Option **Azure-Datenbank für MySQL** aus. Sie können auch **MySQL** in das Suchfeld auf der Seite „Neu“ eingeben, um den Dienst zu suchen.
![Azure-Portal – Neu – Datenbank – MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Geben Sie die folgenden Informationen in das Formular für den neuen Server ein, wie in der obigen Abbildung dargestellt:

| **Einstellung** | **Empfohlener Wert** | **Feldbeschreibung** |
|---|---|---|
| *Servername* | myserver4demo  | Der Servername muss global eindeutig sein. |
| *Abonnement* | mysubscription | Wählen Sie in der Dropdownliste Ihr Abonnement aus. |
| *Ressourcengruppe* | myresourcegroup | Erstellen Sie eine Ressourcengruppe, oder verwenden Sie eine bereits vorhandene. |
| *Serveradministratoranmeldung* | myadmin | Geben Sie den Namen eines Kontos an, das Administrator des MySQL-Moduls sein soll. |
| *Kennwort* |  | Legen Sie ein sicheres Kennwort für das Administratorkonto fest. |
| *Kennwort bestätigen* |  | Bestätigen Sie das Kennwort des Administratorkontos. |
| *Standort* |  | Wählen Sie eine verfügbare Region aus. |
| *Version* | 5.7 | Wählen Sie die neueste Version aus. |
| *Tarif* | Basic, 50 Compute-Einheiten, 50 Speicher (GB)  | Wählen Sie **Tarif**, **Compute-Einheiten**, **Speicher (GB)** aus, und klicken Sie dann auf **OK**. |
| *An Dashboard anheften* | Prüfen | Es wird empfohlen, dieses Kontrollkästchen zu aktivieren, damit Sie den Server später einfacher finden. |

   Klicken Sie auf **Tarif**, um den Tarif und die Leistungsebene für die neue Datenbank anzugeben. Wählen Sie für diesen Schnellstart den Tarif Basic, 50 Compute-Einheiten und 50 GB enthaltenen Speicher aus. Klicken Sie anschließend auf **OK**, um den Tarif zu speichern.
   
   ![Azure-Portal – Erstellen von MySQL durch erforderliche Formulareingaben](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

   Klicken Sie dann auf **Erstellen**. In ein oder zwei Minuten wird eine neue Azure-Datenbank für MySQL-Server in der Cloud ausgeführt. Sie können auf der Symbolleiste auf die Schaltfläche **Benachrichtigungen** (Glockensymbol) klicken, um den Bereitstellungsprozess zu überwachen.

## <a name="configure-the-firewall"></a>Konfigurieren der Firewall
Vor dem erstmaligen Herstellen einer Verbindung mit Azure-Datenbank für MySQL müssen Sie die Firewall konfigurieren und die öffentliche Netzwerk-IP-Adresse (oder den Adressbereich) des Clients der Whitelist hinzufügen.

1. Klicken Sie nach Abschluss der Bereitstellung im linken Menü auf **Alle Ressourcen**, und geben Sie den Namen **myserver4demo** ein, um nach dem neu erstellten Server zu suchen. Klicken Sie auf den im Suchergebnis aufgelisteten Servernamen. Die Seite „Übersicht“ für Ihren Server mit Optionen für die weitere Konfiguration wird geöffnet.

2. Wählen Sie auf dem Serverblatt **Verbindungssicherheit** aus.

3. Klicken Sie auf **Meine IP-Adresse hinzufügen**, um die IP-Adresse des lokalen Computers hinzuzufügen, oder konfigurieren Sie einen Bereich von IP-Adressen. Denken Sie nach dem Erstellen der Regeln daran, auf **Speichern** zu klicken.
  ![Azure-Portal – Firewallregel hinzufügen und speichern](./media/quickstart-create-mysql-server-database-using-azure-portal/5_firewall-settings.png)

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie den vollqualifizierten Domänennamen für Ihren Azure MySQL-Server im Azure-Portal ab. Sie verwenden den vollqualifizierten Domänennamen, um mit dem Befehlszeilentool **mysql.exe** eine Verbindung mit Ihrem Server herzustellen.

1.  Klicken Sie im [Azure-Portal](https://portal.azure.com/) im linken Menü auf **Alle Ressourcen** und dann auf Ihre Azure-Datenbank für MySQL-Server.

2.  Klicken Sie auf **Eigenschaften**. Notieren Sie sich **SERVERNAME** und **SERVERADMINISTRATORANMELDUNG**.
In diesem Beispiel lautet der Servername *myserver4demo.mysql.database.azure.com* und die Serveradministratoranmeldung *myadmin@myserver4demo*.

## <a name="connect-to-the-server-using-mysqlexe-command-line-tool"></a>Herstellen einer Verbindung zum Server mit dem Befehlszeilentool „mysql.exe“
Verwenden Sie das [Befehlszeilentool mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) zum Herstellen einer Verbindung mit Ihrer Azure-Datenbank für MySQL-Server. Sie können das Befehlszeilentool mysql im Browser mit der Azure Cloud Shell ausführen, oder Sie können es von Ihrem Computer über die lokal installierten MySQL-Tools starten. Um die Azure Cloud Shell zu starten, klicken Sie in einem Codeblock in diesem Artikel auf die Schaltfläche `Try It`, oder besuchen Sie das [Azure-Portal](https://portal.azure.com), und klicken Sie auf das Symbol `>_` rechts oben auf der Symbolleiste. 

1. Geben Sie den Befehl für das Herstellen einer Verbindung ein:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. Überprüfen Sie den Serverstatus, um sicherzustellen, dass die Verbindung funktionsfähig ist. Geben Sie an der Eingabeaufforderung „mysql>“ den Befehl `status` ein, nachdem eine Verbindung hergestellt wurde.
```sql
status
```

   ![Eingabeaufforderung – Beispiel für mysql-Befehlszeile](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

   > [!TIP]
   > Zusätzliche Befehle finden Sie unter [MySQL 5.7-Referenzhandbuch – Kapitel 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

3. Erstellen Sie eine leere Datenbank, indem Sie den Befehl `CREATE DATABASE` an der Eingabeaufforderung „mysql>“ eingeben.

   ```sql
   CREATE DATABASE quickstartdb;
   ```

   Sie können in einer Instanz von Azure-Datenbank für MySQL-Server eine oder mehrere Datenbanken verwenden. Sie können eine Einzeldatenbank pro Server erstellen, die alle Ressourcen nutzt, oder Sie können mehrere Datenbanken erstellen, um die Ressourcen gemeinsam zu verwenden. Es gibt keine Beschränkung der Anzahl der Datenbanken, die erstellt werden können, aber mehrere Datenbanken verwenden dieselben Serverressourcen.  

4. Sie listen die Datenbanken durch Eingabe des Befehls `SHOW DATABASES` an der Eingabeaufforderung „mysql>“ auf.

   ```sql
   SHOW DATABASES;
   ```

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Herstellen einer Verbindung zum Server mit dem MySQL Workbench-GUI-Tool
1.  Starten Sie die Anwendung MySQL Workbench auf dem Clientcomputer. Sie können MySQL Workbench [hier](https://dev.mysql.com/downloads/workbench/) herunterladen und installieren.

2.  Geben Sie im Dialogfeld **Setup New Connection** (Neue Verbindung einrichten) auf der Registerkarte **Parameter** folgende Informationen ein:

   ![Einrichten einer neuen Verbindung](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

| **Einstellung** | **Empfohlener Wert** | **Feldbeschreibung** |
|---|---|---|
|   *Verbindungsname* | Beispielverbindung| Geben Sie eine Bezeichnung für diese Verbindung an. |
| *Verbindungsmethode* | Standard (TCP/IP) | Standard (TCP/IP) ist ausreichend. |
| *Hostname* | myserver4demo.mysql.database.azure.com | Verwenden Sie den vollqualifizierten Servernamen für Ihren Server. |
| *Port* | 3306 | Verwenden Sie den Standardport 3306. |
| *Benutzername* | myadmin@myserver4demo  | Verwenden Sie die Administratoranmeldung für den Server, die Sie sich zuvor notiert haben, mit einem @-Zeichen und dem Namen des Servers. |
| *Kennwort* | Ihr Kennwort | Klicken Sie auf die Schaltfläche „In Vault speichern“, um das Kennwort zu speichern. |

Klicken Sie auf **Verbindung testen**, um zu testen, ob alle Parameter richtig konfiguriert wurden. Klicken Sie zum Speichern der Verbindung auf „OK“. 

> [!NOTE]
> SSL wird standardmäßig auf dem Server erzwungen, sodass für eine erfolgreiche Verbindung eine zusätzliche Konfiguration erforderlich ist. Siehe [Konfigurieren von SSL-Verbindungen in der Anwendung für eine sichere Verbindung mit Azure-Datenbank für MySQL](./howto-configure-ssl.md).  Wenn Sie SSL für diesen Schnellstart deaktivieren möchten, besuchen Sie das Azure-Portal, und klicken Sie auf die Seite „Verbindungssicherheit“, um die Umschaltfläche „SSL-Verbindung erzwingen“ zu deaktivieren.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Bereinigen Sie alle im Schnellstart erstellten Ressourcen, indem Sie die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) löschen.

> [!TIP]
> Andere Schnellstarts dieser Sammlung bauen auf diesem Schnellstart auf. Wenn Sie planen, mit den nachfolgenden Schnellstarts fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.

1.  Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen** und dann auf **myresourcegroup**.
2.  Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld **myresourcegroup** ein, und klicken Sie anschließend auf „Löschen“.

Wenn Sie den neu erstellten Server löschen möchten:
1.  Klicken Sie im Azure-Portal im linken Menü auf „PostgreSQL-Server“, und suchen Sie nach dem soeben erstellten Server.
2.  Klicken Sie auf der Seite „Übersicht“ im oberen Bereich ![Azure-Datenbank für MySQL – Server löschen](./media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png) auf die Schaltfläche „Löschen“.
3.  Bestätigen Sie, dass Sie den Server wirklich löschen möchten, und zeigen Sie die darunter liegenden betroffenen Datenbanken an. Geben Sie im Textfeld **myserver4demo** ein, und klicken Sie dann auf „Löschen“.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Entwerfen Sie Ihre erste Azure-Datenbank für MySQL-Datenbank](./tutorial-design-database-using-portal.md)


