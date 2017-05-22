---
title: "Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für PostgreSQL mithilfe der Azure CLI | Microsoft-Dokumentation"
description: "Hier wird beschrieben, wie Sie mithilfe der Azure CLI Firewallregeln für Azure-Datenbank für PostgreSQL erstellen und verwalten."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: postgresql-database
ms.tgt_pltfrm: portal
ms.devlang: azurecli
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 5cb9a3e1fe6e5477e399fd7148fc5366f76cea38
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Erstellen und Verwalten von Firewallregeln für die Azure-Datenbank für PostgreSQL mithilfe der Azure CLI
Mithilfe von Firewallregeln auf Serverebene können Administratoren den Zugriff auf einen Server für Azure-Datenbank für PostgreSQL über eine bestimmte IP-Adresse oder über einen IP-Adressbereich verwalten. Dank praktischer Azure CLI-Befehle können Sie zum Verwalten Ihres Servers Firewallregeln erstellen, aktualisieren, löschen, auflisten und anzeigen. Eine Übersicht über Firewalls für Azure-Datenbank für PostgreSQL finden Sie unter [Azure Database for PostgreSQL Server firewall rules](concepts-firewall-rules.md) (Firewallregeln des Servers für Azure-Datenbank für PostgreSQL).

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- [Einen Server und eine Datenbank für Azure-Datenbank für PostgreSQL](quickstart-create-server-database-azure-cli.md)
- Das Befehlszeilenprogramm [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) muss installiert sein.

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>Konfigurieren von Firewallregeln für Azure-Datenbank für PostgreSQL
Der Befehl **az postgres server firewall-rule** wird in der Azure CLI zum Erstellen, Löschen, Auflisten, Anzeigen und Aktualisieren von Firewallregeln verwendet. Dieses 

## <a name="login-to-azure-and-list-servers"></a>Anmelden bei Azure und Auflisten von Servern
Stellen Sie eine sichere Verbindung zwischen der Azure CLI und Ihrem Azure-Konto her. Verwenden Sie hierfür den Befehl **az login**.
1. Führen Sie den folgenden Befehl an der Befehlszeile aus:
```azurecli
az login
```
Dieser Befehl gibt einen Code aus, der im nächsten Schritt verwendet wird.

2. Öffnen Sie die Seite [https://aka.ms/devicelogin](https://aka.ms/devicelogin) in einem Webbrowser, und geben Sie den Code ein.

3. Melden Sie sich an der Eingabeaufforderung mit Ihren Azure-Anmeldeinformationen an.

4. Nachdem Ihre Anmeldung autorisiert wurde, wird eine Liste der Abonnements auf der Konsole ausgegeben.
Kopieren Sie die ID des gewünschten Abonnements, um das aktuelle Abonnement festzulegen, das künftig verwendet werden soll.
```azurecli
az account set --subscription {your subscription id}
```
5. Listen Sie die Server für Azure-Datenbanken für PostgreSQL für Ihr Abonnement und die Ressourcengruppen auf, wenn Sie sich in Bezug auf die Namen unsicher sind.

```azurecli
az postgres server list --resource-group myresourcegroup
```
Notieren Sie das Namenattribut in der Auflistung. Mit diesem Attribut wird angegeben, welcher PostgreSQL-Server verwendet werden soll. Bestätigen Sie ggf. die Details für diesen Server, um mithilfe des Namenattributs die Richtigkeit des Namens zu überprüfen:

```azurecli
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="list-firewall-rules"></a>Auflisten von Firewallregeln 
Listen Sie mithilfe des Servernamens und des Ressourcengruppennamens die vorhandenen Firewallregeln für den Server auf. Beachten Sie, dass das Attribut des Servernamens im Switch **--server** und nicht im Switch **--name** angegeben ist.
```azurecli
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401
```
Die Ausgabe listet die Regeln (sofern vorhanden) standardmäßig im JSON-Format auf. Sie können mit dem Switch **--output table** ein besser lesbares Tabellenformat als die Ausgabe erzeugen.
```azurecli
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401 --output table
```
## <a name="create-firewall-rule"></a>Erstellen einer Firewallregel
Erstellen Sie mithilfe des Namens des Servers für Azure-Datenbank für PostgreSQL und mithilfe des Ressourcengruppennamens eine neue Firewallregel auf dem Server. Geben Sie einen Namen für die Regel sowie Start-IP und End-IP ein, um einen Bereich von IP-Adressen anzugeben, für die der Zugriff gewährt wird.
```azurecli
az postgres server firewall-rule create --resource-group myresourcegroup  --server mypgserver-20170401 --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
Damit nur einer einzelnen IP-Adresse Zugriff gewährt wird, geben Sie wie in diesem Beispiel dieselbe Adresse als Start-IP und End-IP an.
```azurecli
az postgres server firewall-rule create --resource-group myresourcegroup  
--server mypgserver-20170401 --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen erstellten Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

## <a name="update-firewall-rule"></a>Erstellen oder Aktualisieren einer Firewallregel 
Aktualisieren Sie mithilfe des Namens des Servers für Azure-Datenbank für PostgreSQL und mithilfe des Ressourcengruppennamens eine vorhandene Firewallregel auf dem Server. Geben Sie den Namen der vorhandenen Firewallregel als Eingabe und die zu aktualisierenden Attribute für Start-IP und End-IP ein.
```azurecli
az postgres server firewall-rule update --resource-group myresourcegroup --server mypgserver-20170401 --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Bei erfolgreicher Ausführung führt die Befehlsausgabe die Details der von Ihnen aktualisierten Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.
> [!NOTE]
> Falls die Firewallregel nicht vorhanden ist, wird sie vom Updatebefehl erstellt.

## <a name="show-firewall-rule-details"></a>Anzeigen der Details einer Firewallregel
Zeigen Sie mithilfe des Namens des Servers für Azure-Datenbank für PostgreSQL und mithilfe des Ressourcengruppennamens die Details der vorhandenen Firewallregel auf dem Server an. Geben Sie den Namen der vorhandenen Firewallregel als Eingabe an.
```azurecli
az postgres server firewall-rule show --resource-group myresourcegroup --server mypgserver-20170401 --name "Firewall Rule 1"
```
Bei erfolgreicher Ausführung führt die Befehlsausgabe die Details der von Ihnen angegebenen Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

## <a name="delete-firewall-rule"></a>Löschen einer Firewallregel
Entfernen Sie mithilfe des Namens des Server für Azure-Datenbank für PostgreSQL und mithilfe des Ressourcengruppennamens eine vorhandene Firewallregel vom Server. Geben Sie den Namen der vorhandenen Firewallregel an.
```azurecli
az postgres server firewall-rule delete --resource-group myresourcegroup --server mypgserver-20170401 --name "Firewall Rule 1"
```
Bei erfolgreicher Ausführung wird keine Ausgabe angezeigt. Bei einem Fehler wird eine Fehlermeldung zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte
- Sie können auch einen Webbrowser zum [Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals](howto-manage-firewall-using-portal.md) verwenden.
- Weitere Informationen zu den [Firewallregeln des Servers für Azure-Datenbank für PostgreSQL](concepts-firewall-rules.md)
- Wenn Sie Unterstützung beim Herstellen einer Verbindung mit einem Server für Azure-Datenbank für MySQL benötigen, lesen Sie die Informationen unter [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md) (Verbindungsbibliotheken für Azure-Datenbank für PostgreSQL).

