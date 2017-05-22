---
title: "Herstellen einer Verbindung zwischen Anwendungen und Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "In diesem Dokument sind alle derzeit unterstützten Verbindungszeichenfolgen für Anwendungen aufgeführt, um eine Verbindung mit Azure-Datenbank für MySQL herzustellen."
services: mysql
author: wuta
ms.author: wuta
editor: jasonh
manager: jhubbard
ms.assetid: 
ms.service: mysql-database
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 93b74f146cd0a75437e85dbb1c941b29534ffab2
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="how-to-connect-applications-to-azure-database-for-mysql"></a>Herstellen einer Verbindung zwischen Anwendungen und Azure-Datenbank für MySQL
In diesem Dokument sind alle von Azure-Datenbank für MySQL unterstützten Typen von Verbindungszeichenfolgen sowie Vorlagen und Beispiele aufgeführt. Ihre Verbindungszeichenfolge enthält möglicherweise andere Parameter und Einstellungen.

- Informationen zum Abrufen des Zertifikats finden Sie im Dokument zum [Konfigurieren von SSL](./howto-configure-ssl.md).
- {your_host} = <servername>.mysql.database.azure.com

## <a name="adonet"></a>ADO.NET
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={your_username};Pwd={your_password};[SslMode=Required;]
```

Beispiel: Vorausgesetzt der Servername lautet **wpdemo**, der Datenbankname **wpdb**, der Benutzername **WPAdmin** und das Kennwort **orcas!2**.

In dem Fall muss die Verbindungszeichenfolge wie folgt lauten:

```ado.net
Server= "wpdemo.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@wpdemo"; Pwd="orcas!2"; SslMode=Required;
```

## <a name="jdbc"></a>JDBC
```jdbc
String url ="jdbc:mysql://%s:%s/%s[?verifyServerCertificate=true&useSSL=true&requireSSL=true]",{your_host},{your_port},{your_database}"; myDbConn = DriverManager.getConnection(url, {your_username}, {your_password}";
```

## <a name="nodejs"></a>Node.JS
```node.js
var conn = mysql.createConnection({host: {your_host}, user: {your_username}, password: {your_password}, database: {your_database}, Port: {your_port}[, ssl:{ca:fs.readFileSync({ca-cert filename})}}]);
```

## <a name="odbc"></a>ODBC
```odbc
DRIVER={MySQL ODBC 5.3 UNICODE Driver};Server={your_host};Port={your_port};Database={your_database};Uid={your_username};Pwd={your_password}; [sslca={ca-cert filename}; sslverify=1; Option=3;]
```

## <a name="php"></a>PHP
```php
$con=mysqli_init(); [mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL);] mysqli_real_connect($con, {your_host}, {your_username}, {your_password}, {your_database}, {your_port});
```

## <a name="python"></a>Python
```python
cnx = mysql.connector.connect(user={your_username}, password={your_password}, host={your_host}, port={your_port}, database={your_database}[, ssl_ca={ca-cert filename}, ssl_verify_cert=true])
```

## <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(username: {your_username}, password: {your_password}, database: {your_database}, host: {your_host}, port: {your_port}[, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA'])
```

## <a name="get-connection-string-from-portal"></a>Abrufen der Verbindungszeichenfolge im Portal
Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Azure-Datenbank für MySQL, und klicken Sie auf **Verbindungszeichenfolgen**, um die Zeichenfolgenliste für Ihre Instanz abzurufen: ![Verbindungszeichenfolgen im Portal](./media/howto-connection-strings/connection-strings-on-portal.png)

Die Zeichenfolge enthält Details wie Treiber, Server und andere Parameter für die Datenbankverbindung. Kopieren Sie die gewünschte Verbindungszeichenfolge, und passen Sie sie mit Ihren eigenen Parametern wie Datenbankname, Kennwort usw. an. Anschließend können Sie diese Zeichenfolge zum Herstellen einer Verbindung mit dem Server verwenden.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu Verbindungsbibliotheken finden Sie unter [Connection libraries for Azure Database for MySQL](./concepts-connection-libraries.md) (Verbindungsbibliotheken für Azure-Datenbank für MySQL).

