---
title: "Erstellen einer Azure-Datenbank für PostgreSQL mithilfe der Azure-CLI | Microsoft-Dokumentation"
description: "Schnellstart-Anleitung zum Erstellen und Verwalten von Azure-Datenbank für PostgreSQL-Server mithilfe der Azure-CLI (Befehlszeilenschnittstelle)."
services: postgresql
author: sanagama
ms.author: sanagama
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: postgresql-database
ms.custom: quick start create
ms.tgt_pltfrm: portal
ms.devlang: azurecli
ms.topic: hero-article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: fa02abd9176fa17df3b2d7f396988b72c28a473c
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="create-an-azure-database-for-postgresql-using-the-azure-cli"></a>Erstellen einer Azure-Datenbank für PostgreSQL mithilfe der Azure-CLI

Azure-Datenbank für PostgreSQL ist ein verwalteter Dienst, mit dem Sie hochverfügbare PostgreSQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. Dieser Schnellstart veranschaulicht das Erstellen einer Azure-Datenbank für PostgreSQL-Server in einer [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) mithilfe der Azure-CLI.

Für dieses Schnellstarttutorial muss die aktuelle Version von [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) installiert sein. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.
```azurecli
az login
```
Wenn Sie über mehrere Abonnements verfügen, wählen Sie das entsprechende Abonnement aus, in dem die Ressource in Rechnung gestellt wird. Wählen Sie eine bestimmte Abonnement-ID unter Ihrem Konto mit dem Befehl [az account set](/cli/azure/account#set) aus.
```azurecli
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myresourcegroup` am Standort `westus` erstellt.
```azurecli
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a>Erstellen einer Azure-Datenbank für PostgreSQL-Server

Erstellen Sie mit dem Befehl **az postgres server create** eine [Azure-Datenbank für PostgreSQL-Server](overview.md). Ein Server enthält eine Gruppe von Datenbanken, die als Gruppe verwaltet werden. 

Im folgenden Beispiel wird der Server `mypgserver-20170401` in der Ressourcengruppe `myresourcegroup` mit dem Serveradministrator-Anmeldenamen `mylogin` erstellt. Der Name eines Servers wird dem DNS-Namen zugeordnet und muss deshalb in Azure global eindeutig sein. Ersetzen Sie `<server_admin_password>` durch einen eigenen Wert.
```azurecli
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> Der hier angegebene Benutzername bzw. das Kennwort für den Serveradministrator sind erforderlich, um später in diesem Schnellstart die Anmeldung am Server und an den dazugehörigen Datenbanken durchzuführen. Behalten Sie diese Angaben im Kopf, oder notieren Sie sie zur späteren Verwendung.

Standardmäßig wird die **postgres**-Datenbank unter dem Server erstellt. Die [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html)-Datenbank ist eine Standarddatenbank für die Verwendung durch Benutzer, Hilfsprogramme und Drittanbieteranwendungen. 


## <a name="configure-a-server-level-firewall-rule"></a>Konfigurieren einer Firewallregel auf Serverebene

Erstellen Sie mit dem Befehl **az postgres server firewall-rule create** eine Azure-PostgreSQL-Firewallregel auf Serverebene. Eine Firewallregel auf Serverebene ermöglicht einer externen Anwendung wie z.B. [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) oder [PgAdmin](https://www.pgadmin.org/), über die Firewall des Azure-PostgreSQL-Diensts eine Verbindung mit Ihrem Server herzustellen. 

Sie können eine Firewallregel festlegen, die einen Bereich von IP-Adressen abdeckt, mit denen Verbindungen aus dem Netzwerk hergestellt werden können. Im folgenden Beispiel wird mit **az postgres server firewall-rule create** die Firewallregel `AllowAllIps` für einen IP-Adressbereich erstellt. Verwenden Sie 0.0.0.0 als IP-Startadresse und 255.255.255.255 als Endadresse, wenn Sie alle IP-Adressen öffnen möchten.
```azurecli
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Der Azure-PostgreSQL-Server kommuniziert über Port 5432. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 5432 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 5432 öffnet.
>

## <a name="get-the-connection-information"></a>Abrufen der Verbindungsinformationen

Zum Herstellen einer Verbindung mit dem Server müssen Sie Hostinformationen und Anmeldeinformationen für den Zugriff angeben.
```azurecli
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

Das Ergebnis liegt im JSON-Format vor. Notieren Sie sich die Werte für **administratorLogin** und **fullyQualifiedDomainName**.
```json
{
  "administratorLogin": "mylogin",
  "fullyQualifiedDomainName": "mypgserver-20170401.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mypgserver-20170401",
  "location": "westus",
  "name": "mypgserver-20170401",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "PGSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 51200,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

## <a name="connect-to-postgresql-database-using-psql"></a>Herstellen einer Verbindung mit einer PostgreSQL-Datenbank mithilfe von psql

Wenn auf Ihrem Clientcomputer PostgreSQL installiert ist, können Sie mit einer lokalen Instanz von [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) eine Verbindung mit einer Azure-PostgreSQL-Serverinstanz herstellen. Wir stellen jetzt mit dem Befehlszeilen-Hilfsprogramm psql eine Verbindung mit der Azure-PostgreSQL-Serverinstanz her.

1. Führen Sie den folgenden psql-Befehl aus, um für eine PostgreSQL-Serverinstanz eine Verbindung mit einer Azure-Datenbank herzustellen:
```bash
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Der folgende Befehl stellt z.B. mithilfe der Zugriffsanmeldeinformationen eine Verbindung mit der Standarddatenbank **postgres** auf Ihrem PostgreSQL-Server **mypgserver-20170401.postgres.database.azure.com** her. Geben Sie das `<server_admin_password>` ein, das Sie bei der Aufforderung zur Kennworteingabe ausgewählt haben.
  
  ```bash
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  Sobald Sie mit dem Server verbunden sind, erstellen Sie eine leere Datenbank an der Eingabeaufforderung.
```bash
CREATE DATABASE mypgsqldb;
```

3.  Führen Sie an der Eingabeaufforderung den folgenden Befehl zum Wechseln der Verbindung zur neu erstellten Datenbank **mypgsqldb** aus:
```bash
\c mypgsqldb
```

## <a name="connect-to-postgresql-database-using-pgadmin"></a>Herstellen einer Verbindung mit einer PostgreSQL-Datenbank mithilfe von pgAdmin

So stellen Sie eine Verbindung mit der Azure-PostgreSQL-Serverinstanz mithilfe des GUI-Tools _pgAdmin_ her
1.    Starten Sie die Anwendung _pgAdmin_ auf dem Clientcomputer. Sie können _pgAdmin_ über http://www.pgadmin.org/ installieren.
2.    Wählen Sie im Menü **Kurzlinks** die Option **Neuen Server hinzufügen** aus.
3.    Geben Sie im Dialogfeld **Erstellen - Server** auf der Registerkarte **Allgemein** einen eindeutigen Anzeigenamen für den Server ein. Beispiel: **Azure PostgreSQL Server**.
 ![pgAdmin-Tool – Erstellen – Server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)
4.    Führen Sie im Dialogfeld **Erstellen -Server** auf der Registerkarte **Verbindung** Folgendes aus:
    - Geben Sie den vollqualifizierten Servernamen (z.B. **mypgserver-20170401.postgres.database.azure.com**) im Feld **Hostname/-adresse** ein. 
    - Geben Sie im Feld **Port** die Portnummer 5432 ein. 
    - Geben Sie den zuvor in diesem Schnellstart erhaltenen **Server-Administratoranmeldenamen (user@mypgserver)** und das beim Erstellen des Servers angegebene Kennwort in die Felder **Benutzername** und **Passwort** ein.
    - Wählen Sie für **SSL-Modus** den Wert **Require** aus. In der Standardeinstellung werden alle Azure PostgreSQL-Server mit aktivierter SSL-Erzwingung erstellt. Informationen zum Deaktivieren der SSL-Erzwingung finden Sie unter [Erzwingen von SSL](./concepts-ssl-connection-security.md).

    ![pgAdmin – Erstellen – Server](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.    Klicken Sie auf **Speichern**.
6.    Erweitern Sie im linken Browserbereich die **Servergruppen**. Wählen Sie Ihren Server **Azure PostgreSQL-Server** aus.
7.  Wählen Sie den **Server** aus, mit dem Sie eine Verbindung hergestellt haben, und wählen Sie darunter die Option **Datenbanken** aus. 
8.    Klicken Sie mit der rechten Maustaste auf **Datenbanken**, um eine Datenbank zu erstellen.
9.    Wählen Sie den Datenbanknamen **mypgsqldb** und für den Besitzer den Serveradministrator-Anmeldenamen **mylogin** aus.
10. Klicken Sie auf **Speichern**, um eine leere Datenbank zu erstellen.
11. Erweitern Sie im **Browser** die Gruppe **Server**. Erweitern Sie den erstellten Server. Sie bemerken darunter die Datenbank **mypgsqldb**.
 ![pgAdmin – Erstellen – Datenbank](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Bereinigen Sie alle im Schnellstart erstellten Ressourcen, indem Sie die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) löschen.

> [!TIP]
> Andere Schnellstarts dieser Sammlung bauen auf diesem Schnellstart auf. Wenn Sie planen, mit den nachfolgenden Schnellstarts fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts in der Azure-CLI zu löschen.

```azurecli
az group delete --name myresourcegroup
```

Wenn Sie lediglich den neu erstellten Server löschen möchten:
```azurecli
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="next-steps"></a>Nächste Schritte
- Migrieren Sie die Datenbank durch [Exportieren und Importieren](./howto-migrate-using-export-and-import.md) oder durch [Sichern und Wiederherstellen](./howto-migrate-using-dump-and-restore.md).
- Eine technische Übersicht dieses Diensts finden Sie unter [Was ist Azure-Datenbank für PostgreSQL?](overview.md)
