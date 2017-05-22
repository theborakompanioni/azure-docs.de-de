---
title: "Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe der Azure CLI | Microsoft-Dokumentation"
description: "Hier wird beschrieben, wie Sie mithilfe der Azure CLI Firewallregeln für Azure-Datenbank für MySQL erstellen und verwalten."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.tgt_pltfrm: portal
ms.devlang: azurecli
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: d7ff4ae8dbf9610b843c8b48d83b0f3c23ac72d5
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-azure-cli"></a>Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe der Azure CLI
Mithilfe von Firewallregeln auf Serverebene können Administratoren den Zugriff auf einen Server für Azure-Datenbank für MySQL über eine bestimmte IP-Adresse oder über einen IP-Adressbereich verwalten. Dank praktischer Azure CLI-Befehle können Sie zum Verwalten Ihres Servers Firewallregeln erstellen, aktualisieren, löschen, auflisten und anzeigen. Eine Übersicht über Firewalls für Azure-Datenbank für MySQL finden Sie unter [Azure Database for MySQL server firewall rules](./concepts-firewall-rules.md) (Firewallregeln des Servers für Azure-Datenbank für MySQL).

## <a name="prerequisites"></a>Voraussetzungen
* [Installieren der Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)
* Installieren des Azure Python SDK für den PostgreSQL- und den MySQL-Dienst
* Installieren der Azure CLI-Komponente für den PostgreSQL- und den MySQL-Dienst
* Erstellen eines Servers für Azure-Datenbank für MySQL

## <a name="firewall-rule-commands"></a>Befehle für Firewallregeln:
Der Befehl **az mysql server firewall-rule** wird in der Azure CLI zum Erstellen, Löschen, Auflisten, Anzeigen und Aktualisieren von Firewallregeln verwendet.

Befehle:
- **create**: Dient zum Erstellen einer Firewallregel für den Azure MySQL-Server.
- **delete**: Dient zum Löschen einer Firewallregel für den Azure MySQL-Server.
- **list**: Dient zum Auflisten der Firewallregeln für den Azure MySQL-Server.
- **show** : Dient zum Anzeigen der Details einer Firewallregel für den Azure MySQL-Server.
- **update**: Dient zum Aktualisieren der Firewallregel für den Azure MySQL-Server.

## <a name="login-to-azure-and-list-your-azure-database-for-mysql-servers"></a>Anmelden bei Azure und Auflisten der Server für Azure-Datenbank für MySQL
Stellen Sie eine sichere Verbindung zwischen der Azure CLI und Ihrem Azure-Konto her. Verwenden Sie hierfür den Befehl **az login**.

1. Führen Sie den folgenden Befehl an der Befehlszeile aus:
```azurecli
az login
```
Dieser Befehl gibt einen Code aus, der im nächsten Schritt verwendet wird.

2. Öffnen Sie die Seite [https://aka.ms/devicelogin](https://aka.ms/devicelogin) in einem Webbrowser, und geben Sie den Code ein.

3. Melden Sie sich an der Eingabeaufforderung mit Ihren Azure-Anmeldeinformationen an.

4. Nachdem Ihre Anmeldung autorisiert wurde, wird eine Liste der Abonnements auf der Konsole angezeigt.

Kopieren Sie die ID des gewünschten Abonnements, um das aktuelle Abonnement festzulegen, das künftig verwendet werden soll.
```azurecli
az account set --subscription {your subscription id}
```
5. Listen Sie die Server für Azure-Datenbanken für MySQL für Ihr Abonnement und die Ressourcengruppe auf, wenn Sie sich in Bezug auf die Namen unsicher sind.
```azurecli
az mysql server list --resource-group myResourceGroup
```
Notieren Sie das Namenattribut in der Auflistung. Mit diesem Attribut wird angegeben, welcher MySQL-Server verwendet werden soll. Bestätigen Sie ggf. die Details für diesen Server, um mithilfe des Namenattributs die Richtigkeit des Namens zu überprüfen:
```azurecli
az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Auflisten der Firewallregeln auf dem Server für Azure-Datenbank für MySQL 
Listen Sie mithilfe des Servernamens und des Ressourcengruppennamens die vorhandenen Firewallregeln auf dem Server auf. Beachten Sie, dass das Attribut des Servernamens im Switch **--server** und nicht im Switch **--name** angegeben ist.
```azurecli
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
Die Ausgabe listet die Regeln (sofern vorhanden) standardmäßig im JSON-Format auf. Sie können mit dem Switch **--output table** ein besser lesbares Tabellenformat als die Ausgabe erzeugen.
```azurecli
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-firewall-rule-on-azure-database-for-mysql-server"></a>Erstellen einer Firewallregel für Azure-Datenbank für MySQL-Server
Erstellen Sie mithilfe des Azure MySQL-Servernamens und des Ressourcengruppennamens eine neue Firewallregel auf dem Server. Geben Sie einen Namen für die Regel sowie Start-IP und End-IP ein, um einen Bereich von IP-Adressen anzugeben, für die der Zugriff gewährt wird.
```azurecli
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
Damit nur einer einzelnen IP-Adresse Zugriff gewährt wird, geben Sie wie in diesem Beispiel dieselbe Adresse als Start-IP und End-IP an.
```azurecli
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen erstellten Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

## <a name="update-firewall-rule-on-azure-database-for-mysql-server"></a>Aktualisieren einer Firewallregel des Servers für Azure-Datenbank für MySQL 
Aktualisieren Sie mithilfe des Azure MySQL-Servernamens und des Ressourcengruppennamens eine vorhandene Firewallregel auf dem Server. Geben Sie den Namen der vorhandenen Firewallregel als Eingabe und die zu aktualisierenden Attribute für Start-IP und End-IP ein.
```azurecli
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen aktualisierten Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

> [!NOTE]
> Falls die Firewallregel nicht vorhanden ist, wird sie vom Updatebefehl erstellt.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Anzeigen von Firewallregeldetails auf dem Server für Azure-Datenbank für MySQL
Zeigen Sie mithilfe des Azure MySQL-Servernamens und des Ressourcengruppennamens die Details der vorhandenen Firewallregel auf dem Server an. Geben Sie den Namen der vorhandenen Firewallregel als Eingabe an.
```azurecli
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Bei erfolgreicher Ausführung listet die Befehlsausgabe die Details der von Ihnen angegebenen Firewallregel standardmäßig im JSON-Format auf. Falls ein Fehler auftritt, wird in der Ausgabe stattdessen eine Fehlermeldung angezeigt.

## <a name="delete-firewall-rule-on-azure-database-for-mysql-server"></a>Löschen einer Firewallregel auf dem Server für Azure-Datenbank für MySQL
Entfernen Sie mithilfe des Azure MySQL-Servernamens und des Ressourcengruppennamens eine vorhandene Firewallregel vom Server. Geben Sie den Namen der vorhandenen Firewallregel an.
```azurecli
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Bei erfolgreicher Ausführung wird keine Ausgabe angezeigt. Bei einem Fehler wird eine Fehlermeldung zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu den [Firewallregeln des Servers für Azure-Datenbank für MySQL](./concepts-firewall-rules.md)
- [Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./howto-manage-firewall-using-portal.md)
