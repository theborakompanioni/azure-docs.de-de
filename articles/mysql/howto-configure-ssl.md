---
title: "Konfigurieren von SSL-Verbindungen für eine sichere Verbindung mit Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "Anweisungen zum ordnungsgemäßen Konfigurieren der Azure-Datenbank für MySQL und zugehörigen Anwendungen für die richtige Verwendung von SSL-Verbindungen"
services: mysql
author: seanli1988
ms.author: seanli
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: 38e68712699b3e89a10c3d44d8ec313f531fcbdc
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a>Konfigurieren von SSL-Verbindungen in der Anwendung für eine sichere Verbindung mit der Azure-Datenbank für MySQL
Azure-Datenbank für MySQL unterstützt die Verbindung Ihres Servers mit Azure-Datenbank für MySQL mit Clientanwendungen, die Secure Sockets Layer (SSL) verwenden. Das Erzwingen von SSL-Verbindungen zwischen dem Datenbankserver und Clientanwendungen trägt zum Schutz vor Man-in-the-Middle-Angriffen bei, indem der Datenstrom zwischen dem Server und der Anwendung verschlüsselt wird.

## <a name="step-1-obtain-ssl-certificate"></a>Schritt 1: Abrufen eines SSL-Zertifikats
Laden Sie das Zertifikat, das für die SSL-Kommunikation mit Ihrer Azure-Datenbank für MySQL Server erforderlich ist, von [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) herunter, und speichern Sie die Zertifikatdatei auf dem lokalen Laufwerk. (In diesem Tutorial haben wir „c:\ssl“ verwendet).
**Für Microsoft Internet Explorer und Microsoft Edge:** Benennen Sie nach dem Download das Zertifikat in „BaltimoreCyberTrustRoot.crt.pem“ um.

## <a name="step-2-bind-ssl"></a>Schritt 2: Binden von SSL
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a>Herstellen einer Verbindung mit dem Server mithilfe von MySQL Workbench über SSL
Konfigurieren Sie MySQL Workbench für eine sichere Verbindung über SSL. Navigieren Sie in MySQL Workbench zum Einrichten einer neuen Verbindung im Dialogfeld „Setup New Connection“ (Neue Verbindung einrichten) zur Registerkarte **SSL**. Geben Sie in das Feld für die **SSL-CA-Datei** den Speicherort der Datei **BaltimoreCyberTrustRoot.crt.pem** ein.
![Angepasste Kachel speichern](./media/howto-configure-ssl/mysql-workbench-ssl.png)

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>Herstellen einer Verbindung mit dem Server mithilfe der MySQL-CLI über SSL
Führen Sie über die MySQL-Befehlszeilenschnittstelle den folgenden Befehl aus:
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>Schritt 3: Erzwingen von SSL-Verbindungen in Azure 
### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals
Rufen Sie über das Azure-Portal Ihren Server mit Azure Database for MySQL auf, und klicken Sie auf **Verbindungssicherheit**. Verwenden Sie die Umschaltfläche, um die Einstellung **SSL-Verbindung erzwingen** zu aktivieren/deaktivieren, und klicken Sie dann auf **Speichern**. Microsoft empfiehlt, die Einstellung **SSL-Verbindung erzwingen** immer zu aktivieren, um die Sicherheit zu erhöhen.
![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Sie können den **ssl-enforcement**-Parameter in der Azure-Befehlszeilenschnittstelle mit den entsprechenden Werten aktivieren oder deaktivieren.
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-the-ssl-connection"></a>Schritt 4: Überprüfen der SSL-Verbindung
Führen Sie die MySQL-Befehl **status** aus, um sicherzustellen, dass die Verbindung mit dem MySQL-Server über SSL hergestellt wurde:
```dos
mysql> status
```
Vergewissern Sie sich, dass die Verbindung verschlüsselt ist, indem Sie die Ausgabe überprüfen, die wie folgt angezeigt werden sollte: **SSL: verwendete Verschlüsselung AES256-SHA**. 

## <a name="sample-code"></a>Beispielcode
### <a name="php"></a>PHP
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="python"></a>Python
```
try:
conn = mysql.connector.connect(user='myadmin@myserver4demo',password='yourpassword',database='quickstartdb',host='myserver4demo.mysql.database.azure.com',ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
 print(err)
```

## <a name="next-steps"></a>Nächste Schritte
Überprüfen verschiedener Anwendungsverbindungsoptionen gemäß [Datenverbindungsbibliotheken für die Azure-Datenbank für MySQL](concepts-connection-libraries.md)

