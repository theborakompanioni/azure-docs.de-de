---
title: "Konfigurieren von SSL-Verbindungen in der Anwendung für eine sichere Verbindung mit Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "Anweisungen zum ordnungsgemäßen Konfigurieren der Azure-Datenbank für MySQL und zugehörigen Anwendungen für die richtige Verwendung von SSL-Verbindungen"
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonh
manager: jhubbard
ms.assetid: 
ms.service: mysql-database
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: 801806056b745be5663c0a10241795947d1dd036
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017

---

# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a>Konfigurieren von SSL-Verbindungen in der Anwendung für eine sichere Verbindung mit der Azure-Datenbank für MySQL
Azure-Datenbank für MySQL unterstützt die Verbindung Ihres Servers mit Azure-Datenbank für MySQL mit Clientanwendungen, die Secure Sockets Layer (SSL) verwenden. Das Erzwingen von SSL-Verbindungen zwischen dem Datenbankserver und Clientanwendungen trägt zum Schutz vor Man-in-the-Middle-Angriffen bei, indem der Datenstrom zwischen dem Server und der Anwendung verschlüsselt wird.

Der Datenbankdienst sollte standardmäßig so konfiguriert sein, dass beim Herstellen einer Verbindung mit dem Server mit Azure-Datenbank für MySQL SSL-Verbindungen erforderlich sind.  Es wird empfohlen, die SSL-Option wenn möglich nicht zu deaktivieren. 

## <a name="enforcing-ssl-connections"></a>Erzwingen von SSL-Verbindungen
Bei der Bereitstellung eines neuen Servers mit Azure-Datenbank für MySQL über das Azure-Portal oder die CLI wird die Erzwingung von SSL-Verbindungen standardmäßig aktiviert. 

Entsprechend enthalten Verbindungszeichenfolgen, die im Azure-Portal unter dem Server in den Verbindungszeichenfolgen-Einstellungen vorab definiert sind, die erforderlichen Parameter für gängige Sprachen für die Verbindung mit dem Datenbankserver mithilfe von SSL. Der SSL-Parameter variiert je nach Connector, z.B. „ssl=true“ oder „sslmode=require“ oder „sslmode=required“ und weitere Variationen.

## <a name="configure-enforcement-of-ssl"></a>Konfigurieren der Erzwingung von SSL
Sie können die SSL-Erzwingung deaktivieren oder aktivieren. Microsoft Azure empfiehlt, die Einstellung „SSL-Verbindung erzwingen“ immer zu aktivieren, um die Sicherheit zu erhöhen.

### <a name="using-azure-portal"></a>Verwenden des Azure-Portals
Rufen Sie über das Azure-Portal Ihren Server mit Azure-Datenbank für MySQL auf, und klicken Sie auf **Verbindungssicherheit**. Verwenden Sie die Umschaltfläche, um die Einstellung **SSL-Verbindung erzwingen** zu aktivieren/deaktivieren. Klicken Sie anschließend auf **Speichern**. Microsoft empfiehlt, die Einstellung **SSL-Verbindung erzwingen** immer zu aktivieren, um die Sicherheit zu erhöhen. 
![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)

Bestätigen Sie die Einstellung auf der Seite **Übersicht**, indem Sie den Indikator für den **SSL-Erzwingungsstatus** anzeigen.

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Sie können den Parameter **ssl-enforcement** in der Azure CLI mit den entsprechenden Werten aktivieren oder deaktivieren.
```azurecli
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```
## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Sicherstellen, dass die Anwendung oder das Framework SSL-Verbindungen unterstützt
Viele gängige Anwendungen, die MySQL für Datenbankdienste verwenden, z.B. Wordpress, Drupal und Magento, aktivieren SSL nicht standardmäßig während der Installation.  SSL-Verbindungen müssen nach der Installation oder über bestimmte CLI-Befehle die Anwendung aktiviert werden.  Wenn Ihr MySQL-Server SSL-Verbindungen erzwingt und die zugehörige Anwendung nicht ordnungsgemäß konfiguriert wurde, kann die Anwendung möglicherweise keine Verbindung mit dem Datenbankserver herstellen.  Lesen Sie in der Dokumentation Ihrer Anwendung nach, wie SSL-Verbindungen aktiviert werden.

## <a name="applications-that-require-a-local-certificate-for-ssl-connectivity"></a>Anwendungen, die ein lokales Zertifikat für SSL-Verbindungen erfordern
In einigen Fällen erfordern Anwendungen eine lokale Zertifikatdatei (PEM), die aus der Zertifikatdatei (CER) einer Zertifizierungsstelle (Certificate Authority, CA) generiert wurde, um eine sichere Verbindung herzustellen.  Führen Sie die folgenden Schritte aus, um die CER-Datei abzurufen, die lokale PEM-Datei zu generieren und sie an Ihre Anwendung zu binden.

### <a name="download-the-certificate-file-from-the-certificate-authority-ca"></a>Herunterladen der Zertifikatdatei von der Zertifizierungsstelle 
Das erforderliche Zertifikat für die Kommunikation mit Ihrem Server mit Azure-Datenbank für MySQL über SSL befindet [hier](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt).  Laden Sie die Zertifikatdatei auf das lokale Laufwerk herunter (in diesem Tutorial verwenden wir **C:\ssl**).

### <a name="download-and-install-openssl-on-your-pc"></a>Herunterladen und Installieren von OpenSSL auf dem PC 
Zum Generieren der lokalen **PEM**-Datei, die Ihre Anwendung für die sichere Verbindung mit dem Datenbankserver benötigt, müssen Sie OpenSSL auf dem lokalen Computer installieren.  

Die folgenden Abschnitte beschreiben die möglichen Ansätze entweder auf einem Linux- oder Windows-PC, je nach bevorzugtem Betriebssystem. Sie müssen nur einen Vorgang ausführen.

### <a name="linux-users---download-and-install-openssl-using-a-linux-pc"></a>Linux-Benutzer – Herunterladen und Installieren von OpenSSL auf einem Linux-PC
Die OpenSSL-Bibliotheken finden Sie im Quellcode direkt bei der [OpenSSL Software Foundation](http://www.openssl.org).  Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren von OpenSSL auf Ihrem Linux-Computer.  In diesem Leitfaden werden Befehle für Ubuntu 12.04 und höher verwendet.

Öffnen Sie eine Terminalsitzung, und installieren Sie OpenSSL.
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
```  
Extrahieren Sie die Dateien aus dem Downloadpaket.
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
Geben Sie das Verzeichnis ein, in dem die Dateien extrahiert werden sollen.  Standardmäßig sollte folgendes Verzeichnis verwendet werden.

```bash
cd openssl-1.1.0e
```
Konfigurieren Sie OpenSSL durch Ausführen des folgenden Befehls: Wenn Sie die Dateien in einem anderen Ordner als „/usr/local/openssl“ ablegen möchten, achten Sie darauf, den Befehl entsprechend zu ändern.

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
OpenSSL ist jetzt ordnungsgemäß konfiguriert, und Sie müssen es kompilieren, um Ihr Zertifikat umzuwandeln.  Führen Sie dazu den folgenden Befehl aus:

```bash
make
```
Nach Abschluss der Kompilierung können Sie OpenSSL als ausführbare Datei installieren, indem Sie den folgenden Befehl ausführen:
```bash
make install
```
Zum Sicherstellen, dass OpenSSL erfolgreich in Ihrem System installiert wurde, führen Sie dem folgenden Befehl aus und überprüfen, ob Sie die gleiche Ausgabe erhalten.

```bash
/usr/local/openssl/bin/openssl version
```
Bei einer erfolgreichen Installation wird die folgende Meldung angezeigt:
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

### <a name="windows-pc-users---download-and-install-openssl-using-a-windows-pc"></a>Windows-PC-Benutzer – Herunterladen und Installieren von OpenSSL auf einem Windows-PC
Zum Generieren der lokalen **PEM**-Datei, die Ihre Anwendung für die sichere Verbindung mit dem Datenbankserver benötigt, müssen Sie OpenSSL auf dem lokalen Computer installieren.

Die OpenSSL-Bibliotheken finden Sie im Quellcode direkt bei der [OpenSSL Software Foundation](http://www.openssl.org). Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren von OpenSSL auf Ihrem Linux-Computer.

Die Installation von OpenSSL auf einem Windows-PC kann mit folgenden Methoden durchgeführt werden:

1. **(Empfohlen)**  Mithilfe des integrierten Bash für Windows-Funktionalität in Windows 10 und höher wird OpenSSL standardmäßig installiert.  Anweisungen zum Aktivieren von Bash für Windows-Funktionalität in Windows 10 finden Sie [hier](https://msdn.microsoft.com/commandline/wsl/install_guide).

2. Durch Herunterladen einer von der Community bereitgestellten Win32/64-Anwendung. Während die OpenSSL Software Foundation keine bestimmten Windows-Installationsprogramme bereitstellt oder empfiehlt, bietet sie [hier](https://wiki.openssl.org/index.php/Binaries) eine Liste der verfügbaren Installationsprogramme.

### <a name="convert-your-cer-certificate-to-a-local-pem"></a>Umwandeln Ihres CER-Zertifikats in eine lokale PEM-Datei
Die heruntergeladene Stamm-CA-Datei weist das Format **CER** auf. Sie müssen die Zertifikatdatei mithilfe von OpenSSL in eine **PEM**-Datei umwandeln.  Führen Sie dazu das Befehlszeilentool „openssl.exe“ und anschließend den folgenden Befehl aus: 

```dos
OpenSSL>x509 -inform DER -in BaltimoreCyberTrustRoot.cer -out MyServerCACert.pem
```
Jetzt haben Sie Ihre Zertifikatdatei (MyServerCACert.pem) erfolgreich erstellt und können über SSL eine sichere Verbindung mit Ihrem Datenbankserver herstellen. 

Die folgenden Beispiele zeigen das Herstellen der Verbindung mit dem MySQL-Server sowohl über die MySQL-Befehlszeilenschnittstelle als auch durch die Verwendung von MySQL Workbench mithilfe der Datei **MyServerCACert.pem**.

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>Herstellen einer Verbindung mit dem Server mithilfe der MySQL-CLI über SSL
Führen Sie über die MySQL-Befehlszeilenschnittstelle den folgenden Befehl aus:

```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -uUsername@mysqlserver4demo -pYourPassword --ssl-ca=c:\ssl\MyServerCACert.pem
```
Führen Sie die MySQL-Befehl **status** aus, um sicherzustellen, dass die Verbindung mit dem MySQL-Server über SSL hergestellt wurde:

```dos
mysql> status
--------------
mysql.exe  Ver 14.14 Distrib 5.7.18, for Win64 (x86_64)

Connection id:          65535
Current database:
Current user:           jason@66.235.55.141
SSL:                    Cipher in use is AES256-SHA
Using delimiter:        ;
Server version:         5.6.26.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             janderserver.mysql.sqltest-eg1.mscds.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    cp850
Conn.  characterset:    cp850
TCP port:               3306
Uptime:                 1 day 19 hours 9 min 43 sec

Threads: 4  Questions: 26082  Slow queries: 0  Opens: 112  Flush tables: 1  Open tables: 105  Queries per second avg: 0.167
--------------
```

> [!NOTE]
> Zurzeit besteht ein bekanntes Problem: Wenn Sie die Option „--ssl-mode=VERIFY_IDENTITY“ option in Ihrer mysql.exe-Verbindung mit dem Dienst verwenden, tritt der folgende Verbindungsfehler auf: _FEHLER 2026 (HY000): SSL-Verbindungsfehler: Fehler bei der SSL-Zertifikatüberprüfung_ Führen Sie ein Downgrade zu „--ssl-mode=VERIFY_CA“ oder kleineren [SSL-Modi](https://dev.mysql.com/doc/refman/5.7/en/secure-connection-options.html#option_general_ssl-mode) aus. Wenn Sie „--ssl-mode=VERIFY_IDENTITY“ verwenden müssen, können Sie einen Ping für den Namen Ihres Servers ausführen, um den regionalen Servernamen aufzulösen, wie z. B. „westeurope1-a.control.database.windows.net“, und diesen regionalen Servernamen in der Verbindung verwenden, bis dieses Problem behoben ist. Diese Einschränkung soll in zukünftigen Versionen entfallen. 

### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a>Herstellen einer Verbindung mit dem Server mithilfe von MySQL Workbench über SSL
Zum Konfigurieren von MySQL Workbench für eine sichere Verbindung über SSL müssen Sie im MySQL Workbench-Dialogfeld zum Einrichten einer neuen Verbindung zur Registerkarte **SSL** navigieren und den Speicherort der Datei **MyServerCACert.pem** in das Feld für die **SSL-CA-Datei** eingeben.
![Angepasste Kachel speichern](./media/concepts-ssl-connection-security/mysql-workbench-ssl.png)

## <a name="next-steps"></a>Nächste Schritte
- Überprüfen verschiedener Anwendungsverbindungsoptionen gemäß [Datenverbindungsbibliotheken für die Azure-Datenbank für MySQL](concepts-connection-libraries.md)

