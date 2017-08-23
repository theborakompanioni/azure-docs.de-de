---
title: "Schnellstart: Erstellen einer Azure-Datenbank für MySQL-Server – Azure-Portal | Microsoft-Dokumentation"
description: "Dieser Artikel führt Sie durch die Verwendung des Azure-Portals, um in etwa fünf Minuten schnell eine Azure-Beispieldatenbank für MySQL-Server zu erstellen."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 08/15/2017
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 829c7e73cbf22d866bbd6fd54edc7a954ad7174c
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Erstellen einer Azure-Datenbank für MySQL-Server mithilfe des Azure-Portals
Azure-Datenbank für MySQL ist ein verwalteter Dienst, mit dem Sie hochverfügbare MySQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. In dieser Schnellstartanleitung wird veranschaulicht, wie Sie in etwa fünf Minuten über das Azure-Portal einen Server für Azure-Datenbank für MySQL erstellen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure
Öffnen Sie Ihren Webbrowser, und navigieren Sie zum [Microsoft Azure-Portal](https://portal.azure.com/). Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden. Die Standardansicht ist Ihr Dienstdashboard.

## <a name="create-azure-database-for-mysql-server"></a>Erstellen einer Azure-Datenbank für MySQL-Server
Ein Azure-Datenbank für MySQL-Server wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](./concepts-compute-unit-and-storage.md) erstellt. Der Server wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) erstellt.

Führen Sie die folgenden Schritte aus, um einen Server für Azure-Datenbank für MySQL zu erstellen:

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu** (+).

2. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Datenbanken** die Option **Azure-Datenbank für MySQL** aus. Sie können auch **MySQL** in das Suchfeld auf der Seite „Neu“ eingeben, um den Dienst zu suchen.
![Azure-Portal – Neu – Datenbank – MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Geben Sie die folgenden Informationen in das Formular für den neuen Server ein, wie in der obigen Abbildung dargestellt:

    **Einstellung** | **Empfohlener Wert** | **Feldbeschreibung** 
    ---|---|---
    Servername | myserver4demo | Wählen Sie einen eindeutigen Namen, der Ihren Server für Azure-Datenbank für MySQL identifiziert. Der Domänenname *mysql.database.azure.com* wird an den Servernamen angefügt, den Sie für Anwendungen angeben, damit diese eine Verbindung damit herstellen können. Der Servername darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten und muss zwischen drei und 63 Zeichen lang sein.
    Abonnement | Ihr Abonnement | Das Azure-Abonnement, das Sie für Ihren Server verwenden möchten. Falls Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, über das die Ressource abgerechnet wird.
    Ressourcengruppe | myresourcegroup | Sie können entweder einen neuen Ressourcengruppennamen erstellen oder einen bereits vorhandenen Namen aus Ihrem Abonnement verwenden.
    Serveradministratoranmeldung | myadmin | Erstellen Sie Ihr eigenes Anmeldekonto für die Verbindungsherstellung mit dem Server. Der Administratoranmeldename darf nicht „azure_superuse“, „admin“, „administrator“, „root“, „guest“ oder „public“ lauten.
    Kennwort | *Beliebig* | Erstellen Sie ein neues Kennwort für das Serveradministratorkonto. Es muss zwischen 8 und 128 Zeichen lang sein. Ihr Kennwort muss Zeichen aus drei der folgenden Kategorien enthalten: englische Großbuchstaben, englische Kleinbuchstaben, Zahlen (0-9) und nicht alphanumerische Zeichen (!, $, #, % usw.).
    Kennwort bestätigen | *Beliebig*| Bestätigen Sie das Kennwort des Administratorkontos.
    Ort | *Die Region, die Ihren Benutzern am nächsten liegt*| Wählen Sie den Speicherort aus, der Ihren Benutzern oder anderen Azure-Anwendungen am nächsten liegt.
    Version | *Wählen Sie die neueste Version aus.*| Wählen Sie die neueste Version aus (es sei denn, Sie haben besondere Anforderungen).
    Preisstufe | **Basic**, **50 Compute-Einheiten** **50 GB** | Klicken Sie auf **Tarif**, um die Dienstebene und die Leistungsstufe für die neue Datenbank anzugeben. Wählen Sie auf der oberen Registerkarte den Tarif **Basic** aus. Klicken Sie auf das linke Ende des Schiebereglers **Compute-Einheiten**, um für diese Schnellstartanleitung den kleinstmöglichen Wert festzulegen. Klicken Sie auf **OK**, um die Tarifauswahl zu speichern. Der folgende Screenshot zeigt dies.
    An Dashboard anheften | Prüfen | Aktivieren Sie die Option **An Dashboard anheften**, um Ihren Server komfortabel auf der Titelseite des Dashboards im Azure-Portal nachverfolgen zu können.

    > [!IMPORTANT]
    > Der hier angegebene Benutzername und das Kennwort für den Serveradministrator sind erforderlich, um später in diesem Schnellstart die Anmeldung am Server und an den dazugehörigen Datenbanken durchzuführen. Behalten Sie diese Angaben im Kopf, oder notieren Sie sie zur späteren Verwendung.
    > 

    ![Azure-Portal – Erstellen von MySQL durch erforderliche Formulareingaben](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

4.  Klicken Sie auf **Erstellen**, um den Server bereitzustellen. Die Bereitstellung kann bis zu 20 Minuten dauern.
   
5.  Klicken Sie auf der Symbolleiste auf **Benachrichtigungen** (Glockensymbol), um den Bereitstellungsprozess zu überwachen.

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurieren einer Firewallregel auf Serverebene

Der Dienst „Azure-Datenbank für MySQL“ erstellt eine Firewall auf der Serverebene. Diese Firewall verhindert, dass externe Anwendungen und Tools eine Verbindung mit dem Server und Datenbanken auf dem Server herstellen – sofern keine Firewallregel erstellt wird, um die Firewall für bestimmte IP-Adressen zu öffnen. 

1.  Navigieren Sie nach Abschluss der Bereitstellung zu Ihrem Server. Bei Bedarf können Sie danach suchen. Klicken Sie beispielsweise im linken Menü auf **Alle Ressourcen**, und geben Sie den Servernamen (beispielsweise *myserver4demo*) ein, um nach dem neu erstellten Server zu suchen. Klicken Sie in der Suchergebnisliste auf den Namen Ihres Servers. Die Seite **Übersicht** für Ihren Server wird geöffnet und enthält Optionen für die weitere Konfiguration.

2. Wählen Sie auf der Serverseite die Option **Verbindungssicherheit** aus.

3.  Klicken Sie unter der Überschrift **Firewallregeln** in der Spalte **Regelname** auf das leere Textfeld, um mit der Erstellung der Firewallregel zu beginnen. 

    Im Rahmen dieser Schnellstartanleitung lassen wir für den Server alle IP-Adressen zu, indem wir das Textfeld in jeder Spalte mit folgenden Werten ausfüllen:

    Regelname | Start-IP | End-IP 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. Klicken Sie auf der oberen Symbolleiste der Seite **Verbindungssicherheit** auf **Speichern**. Warten Sie einen Augenblick, bis die Benachrichtigung angezeigt wird, dass die Aktualisierung der Verbindungssicherheit abgeschlossen wurde, und setzen Sie anschließend den Vorgang fort.

    > [!NOTE]
    > Die Kommunikation für Verbindungen mit Azure-Datenbank für MySQL erfolgt über Port 3306. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 3306 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Server herstellen, wenn Ihre IT-Abteilung Port 3306 öffnet.
    > 

## <a name="get-the-connection-information"></a>Abrufen der Verbindungsinformationen
Für die Verbindungsherstellung mit Ihrem Datenbankserver benötigen Sie den vollständigen Servernamen und die Administratoranmeldeinformationen. Diese Werte haben Sie möglicherweise zuvor im Rahmen des Schnellstartartikels notiert. Falls nicht, finden Sie den Servernamen und die Anmeldeinformationen auf der Seite **Übersicht** des Servers oder auf der Seite **Eigenschaften** im Azure-Portal.

1. Öffnen Sie die Seite **Übersicht** für Ihren Server. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**. 
    Zeigen Sie mit dem Cursor auf das jeweilige Feld. Daraufhin erscheint rechts neben dem Text ein Kopiersymbol. Klicken Sie auf das Kopiersymbol, um die Werte zu kopieren.

    In diesem Beispiel lautet der Servername *myserver4demo.mysql.database.azure.com* und die Serveradministratoranmeldung *myadmin@myserver4demo*.

## <a name="connect-to-mysql-using-mysql-command-line-tool"></a>Herstellen einer Verbindung mit MySQL mithilfe des mysql-Befehlszeilentools
Es gibt verschiedene Anwendungen, mit denen Sie eine Verbindung mit Ihrem Server für Azure-Datenbank für MySQL herstellen können. Als Erstes verwenden wir das [mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html)-Befehlszeilentool, um das Herstellen einer Verbindung mit dem Server zu veranschaulichen.  Sie können wie hier beschrieben einen Webbrowser und die Azure Cloud Shell verwenden, ohne zusätzliche Software zu installieren. Wenn das mysql-Hilfsprogramm lokal auf Ihrem eigenen Computer installiert ist, können Sie auch von dort eine Verbindung herstellen.

1. Starten Sie die Azure Cloud Shell über das Terminalsymbol (>_) oben rechts auf der Webseite des Azure-Portals.

2. Die Azure Cloud Shell wird in Ihrem Browser geöffnet, sodass Sie Bash-Shellbefehle eingeben können.

    ![Eingabeaufforderung – Beispiel für mysql-Befehlszeile](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

3. Stellen Sie an der Cloud Shell-Eingabeaufforderung eine Verbindung mit Ihrem Server für Azure-Datenbank für MySQL her, indem Sie die mysql-Befehlszeile an der grünen Aufforderung eingeben.

    Das folgende Format wird verwendet, um mit dem mysql-Hilfsprogramm eine Verbindung mit dem Server für Azure-Datenbank für MySQL herzustellen:
    ```bash
    mysql --host <yourserver> --user <server admin login> --password
    ```

    Mithilfe des folgenden Befehls wird beispielsweise eine Verbindung mit unserem Beispielserver hergestellt:
    ```azurecli-interactive
    mysql --host myserver4demo.mysql.database.azure.com --user myadmin@myserver4demo --password
    ```

    mysql-Parameter |Empfohlener Wert|Beschreibung
    ---|---|---
    --host | *Servername* | Geben Sie den Servernamen an, den Sie zuvor beim Erstellen von Azure-Datenbank für MySQL verwendet haben. Unser Beispielserver heißt „myserver4demo.mysql.database.azure.com“. Verwenden Sie den vollqualifizierten Domänennamen (\*.mysql.database.azure.com), wie im Beispiel gezeigt. Sollten Sie sich nicht an Ihren Servernamen erinnern, ermitteln Sie die Verbindungsinformationen gemäß den Schritten aus dem vorherigen Abschnitt. 
    --user | *Anmeldename des Serveradministrators* |Geben Sie den Anmeldename des Serveradministrators ein, den Sie zuvor beim Erstellen von Azure-Datenbank für MySQL verwendet haben. Sollten Sie sich nicht an Benutzernamen erinnern, ermitteln Sie die Verbindungsinformationen gemäß den Schritten aus dem vorherigen Abschnitt.  Zu verwendendes Format: *username@servername*.
    --password | *Warten Sie bis zur Aufforderung.* | Nach der Eingabe des Befehls werden Sie zur Eingabe des Kennworts aufgefordert. Geben Sie dann das gleiche Kennwort ein, das Sie beim Erstellen des Servers angegeben haben.  Hinweis: Die für das Kennwort eingegebenen Zeichen werden bei der Eingabe nicht in der Bash-Eingabeaufforderung angezeigt. Drücken Sie nach der Eingabe aller Zeichen die EINGABETASTE, um die Authentifizierung durchzuführen und die Verbindung herzustellen.

   Nachdem die Verbindung hergestellt wurde, wird vom mysql-Hilfsprogramm eine `mysql>`-Eingabeaufforderung für die Befehlseingabe angezeigt. 

    mysql-Beispielausgabe:
    ```bash
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 65505
    Server version: 5.6.26.0 MySQL Community Server (GPL)
    
    Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql>
    ```
    > [!TIP]
    > Wenn die Firewall nicht zum Zulassen der IP-Adresse von Azure Cloud Shell konfiguriert ist, tritt der folgende Fehler auf:
    >
    > FEHLER 2003 (28000): Der Client mit der IP-Adresse 123.456.789.0 hat keine Zugriffsberechtigung für den Server.
    >
    > Vergewissern Sie sich zur Behebung des Fehlers, dass die Serverkonfiguration gemäß den Schritten im Abschnitt *Konfigurieren einer Firewallregel auf Serverebene* des Artikels durchgeführt wurde.

4. Überprüfen Sie den Serverstatus, um sicherzustellen, dass die Verbindung funktionsfähig ist. Geben Sie an der Eingabeaufforderung „mysql>“ den Befehl `status` ein, nachdem eine Verbindung hergestellt wurde.
    ```sql
    status
    ```

   > [!TIP]
   > Zusätzliche Befehle finden Sie unter [MySQL 5.7-Referenzhandbuch – Kapitel 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

5.  Erstellen Sie eine leere Datenbank, indem Sie an der mysql>-Eingabeaufforderung den folgenden Befehl eingeben:
    ```sql
    CREATE DATABASE quickstartdb;
    ```
    Die Ausführung des Befehls kann einen Moment dauern. 

    Sie können in einer Instanz von Azure-Datenbank für MySQL-Server eine oder mehrere Datenbanken erstellen. Sie können eine Einzeldatenbank pro Server erstellen, die alle Ressourcen nutzt, oder Sie können mehrere Datenbanken erstellen, um die Ressourcen gemeinsam zu verwenden. Es gibt keine Beschränkung der Anzahl der Datenbanken, die erstellt werden können, aber mehrere Datenbanken verwenden dieselben Serverressourcen. 

6. Listen Sie die Datenbanken auf, indem Sie an der mysql>-Eingabeaufforderung den folgenden Befehl eingeben:

    ```sql
    SHOW DATABASES;
    ```

7.  Geben Sie `\q` ein, und drücken Sie die EINGABETASTE, um das mysql-Tool zu beenden. Anschließend können Sie Azure Cloud Shell schließen.

Sie haben nun eine Verbindung mit Azure-Datenbank für MySQL hergestellt und eine leere Benutzerdatenbank erstellt. Fahren Sie mit dem nächsten Abschnitt fort, um eine ähnliche Übung zum Herstellen einer Verbindung mit dem gleichen Server mithilfe eines anderen gängigen Tools (MySQL Workbench) zu wiederholen.

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Herstellen einer Verbindung zum Server mit dem MySQL Workbench-GUI-Tool
So stellen Sie eine Verbindung mit dem Azure MySQL-Server mithilfe des GUI-Tools „MySQL Workbench“ her:

1.  Starten Sie die Anwendung MySQL Workbench auf dem Clientcomputer. Sie können MySQL Workbench [hier](https://dev.mysql.com/downloads/workbench/) herunterladen und installieren.

2.  Geben Sie im Dialogfeld **Setup New Connection** (Neue Verbindung einrichten) auf der Registerkarte **Parameter** folgende Informationen ein:

    ![Einrichten einer neuen Verbindung](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

    | **Einstellung** | **Empfohlener Wert** | **Feldbeschreibung** |
    |---|---|---|
    |   Verbindungsname | Beispielverbindung | Geben Sie eine Bezeichnung für diese Verbindung an. |
    | Verbindungsmethode | Standard (TCP/IP) | Standard (TCP/IP) ist ausreichend. |
    | Hostname | *Servername* | Geben Sie den Servernamen an, den Sie zuvor beim Erstellen von Azure-Datenbank für MySQL verwendet haben. Unser Beispielserver heißt „myserver4demo.mysql.database.azure.com“. Verwenden Sie den vollqualifizierten Domänennamen (\*.mysql.database.azure.com), wie im Beispiel gezeigt. Sollten Sie sich nicht an Ihren Servernamen erinnern, ermitteln Sie die Verbindungsinformationen gemäß den Schritten aus dem vorherigen Abschnitt.  |
    | Port | 3306 | Verwenden Sie immer den Port 3306, wenn Sie eine Verbindung mit Azure-Datenbank für MySQL herstellen. |
    | Benutzername |  *Anmeldename des Serveradministrators* | Geben Sie den Anmeldename des Serveradministrators ein, den Sie zuvor beim Erstellen von Azure-Datenbank für MySQL verwendet haben. Unser Beispielbenutzername lautet myadmin@myserver4demo. Sollten Sie sich nicht an Benutzernamen erinnern, ermitteln Sie die Verbindungsinformationen gemäß den Schritten aus dem vorherigen Abschnitt. Zu verwendendes Format: *username@servername*.
    | Kennwort | Ihr Kennwort | Klicken Sie auf die Schaltfläche „In Vault speichern“, um das Kennwort zu speichern. |

    Klicken Sie auf **Verbindung testen**, um zu testen, ob alle Parameter richtig konfiguriert wurden. Klicken Sie zum Speichern der Verbindung auf „OK“. 

    > [!NOTE]
    > SSL wird standardmäßig auf dem Server erzwungen, sodass für eine erfolgreiche Verbindung eine zusätzliche Konfiguration erforderlich ist. Weitere Informationen finden Sie unter [Konfigurieren von SSL-Konnektivität in der Anwendung für eine sichere Verbindung mit Azure-Datenbank für MySQL](./howto-configure-ssl.md).  Wenn Sie SSL für diese Schnellstartanleitung deaktivieren möchten, besuchen Sie das Azure-Portal, und klicken Sie auf die Seite „Verbindungssicherheit“, um die Umschaltfläche „SSL-Verbindung erzwingen“ zu deaktivieren.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Bereinigen Sie die in der Schnellstartanleitung erstellten Ressourcen. Löschen Sie dazu entweder die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md), die alle Ressourcen in der Ressourcengruppe enthält, oder löschen Sie die eine Serverressource, wenn die anderen Ressourcen intakt bleiben sollen.

> [!TIP]
> Andere Schnellstartanleitungen in dieser Sammlung bauen auf dieser Schnellstartanleitung auf. Wenn Sie planen, mit den nachfolgenden Schnellstarts fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.
>

So löschen Sie die gesamte Ressourcengruppe, einschließlich des neu erstellten Servers:
1.  Navigieren Sie im Azure-Portal zu Ihrer Ressourcengruppe. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen** und dann auf den Namen Ihrer Ressourcengruppe (in unserem Beispiel: **myresourcegroup**).
2.  Klicken Sie auf der Seite der Ressourcengruppe auf **Löschen**. Geben Sie dann den Namen der Ressourcengruppe (in unserem Beispiel: **myresourcegroup**) in das Textfeld ein, um das Löschen zu bestätigen. Klicken Sie anschließend auf **Löschen**.

So löschen Sie den neu erstellten Server:
1.  Navigieren Sie im Azure-Portal zu Ihrem Server, falls er nicht geöffnet ist. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie nach dem von Ihnen erstellten Server.
2.  Klicken Sie auf der Seite **Übersicht** im oberen Bereich auf die Schaltfläche **Löschen**.
![Azure-Datenbank für MySQL – Server löschen](./media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png)
3.  Bestätigen Sie, dass Sie den Server wirklich löschen möchten, und zeigen Sie die darunter liegenden betroffenen Datenbanken an. Geben Sie den Servernamen (in unserem Beispiel: **myserver4demo**) in das Textfeld ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Entwerfen Sie Ihre erste Azure-Datenbank für MySQL-Datenbank](./tutorial-design-database-using-portal.md)


