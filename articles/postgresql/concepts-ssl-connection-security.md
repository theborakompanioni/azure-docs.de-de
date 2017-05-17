---
title: "Konfigurieren von SSL-Konnektivität in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Anweisungen und Informationen zum Konfigurieren von Azure-Datenbank für PostgreSQL und zugehörigen Anwendungen für die richtige Verwendung von SSL-Verbindungen"
services: postgresql
author: JasonMAnderson
ms.author: janders
editor: jasonh
manager: jhubbard
ms.assetid: 
ms.service: postgresql-database
ms.custom: connection security
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 079e4234c7969b267e7e3a3a518cae570da77dfe
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a>Konfigurieren von SSL-Konnektivität in Azure-Datenbank für PostgreSQL
Azure-Datenbank für PostgreSQL stellt Verbindungen zwischen Clientanwendungen und dem PostgreSQL-Dienst hauptsächlich mittels Secure Sockets Layer (SSL) her. Das Erzwingen von SSL-Verbindungen zwischen dem Datenbankserver und Clientanwendungen trägt zum Schutz vor Man-in-the-Middle-Angriffen bei, indem der Datenstrom zwischen dem Server und der Anwendung verschlüsselt wird.

Standardmäßig ist der PostgreSQL-Datenbankdienst so konfiguriert, dass SSL-Verbindungen erforderlich sind. Wenn Ihre Clientanwendung keine SSL-Konnektivität unterstützt, können Sie optional die Erzwingung von SSL zum Herstellen einer Verbindung mit dem Datenbankdienst deaktivieren. 

## <a name="enforcing-ssl-connections"></a>Erzwingen von SSL-Verbindungen
Bei allen Azure-Datenbank für PostgreSQL-Servern, die über das Azure-Portal und die CLI bereitgestellt werden, ist die Erzwingung von SSL-Verbindungen standardmäßig aktiviert. 

Entsprechend enthalten Verbindungszeichenfolgen, die im Azure-Portal unter dem Server in den Verbindungszeichenfolgen-Einstellungen vorab definiert sind, die erforderlichen Parameter für gängige Sprachen für die Verbindung mit dem Datenbankserver mithilfe von SSL. Der SSL-Parameter variiert je nach Connector, z.B. „ssl=true“ oder „sslmode=require“ oder „sslmode=required“ und weitere Variationen.

## <a name="configure-enforcement-of-ssl"></a>Konfigurieren der Erzwingung von SSL
Sie können die Erzwingung von SSL-Verbindungen optional deaktivieren. Microsoft Azure empfiehlt, die Einstellung **SSL-Verbindung erzwingen** immer zu aktivieren, um die Sicherheit zu erhöhen.

#### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals
Rufen Sie Ihren Azure-Datenbank für PostgreSQL-Server auf, und klicken Sie auf **Verbindungssicherheit**. Verwenden Sie die Umschaltfläche, um die Einstellung **SSL-Verbindung erzwingen** zu aktivieren bzw. zu deaktivieren. Klicken Sie anschließend auf **Speichern**. 

![Verbindungssicherheit – Erzwingung von SSL deaktivieren](./media/concepts-ssl-connection-security/1-disable-ssl.png)

Sie können die Einstellung auf der Seite **Übersicht** bestätigen, indem Sie den Indikator für den **SSL-Erzwingungsstatus** anzeigen.

#### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Sie können den Parameter **ssl-enforcement** in der Azure CLI mit den Werten `Enabled` bzw. `Disabled` aktivieren oder deaktivieren.

```azurecli
az postgres server update --resource-group myresourcegroup --name mypgserver-20170401 --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Sicherstellen, dass die Anwendung oder das Framework SSL-Verbindungen unterstützt
Viele gängige Anwendungsframeworks, die PostgreSQL für Datenbankdienste verwenden, z.B. Drupal und Django, aktivieren SSL nicht standardmäßig während der Installation. SSL-Verbindungen müssen nach der Installation oder über bestimmte anwendungsspezifische CLI-Befehle aktiviert werden. Wenn Ihr PostgreSQL-Server SSL-Verbindungen erzwingt und die zugehörige Anwendung nicht ordnungsgemäß konfiguriert wurde, kann die Anwendung möglicherweise keine Verbindung mit dem Datenbankserver herstellen. Lesen Sie in der Dokumentation Ihrer Anwendung nach, wie SSL-Verbindungen aktiviert werden.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Anwendungen, die eine Zertifikatüberprüfung für SSL-Verbindungen erfordern
In einigen Fällen erfordern Anwendungen eine lokale Zertifikatdatei, die aus der Zertifikatdatei (CER) einer vertrauenswürdigen Zertifizierungsstelle (Certificate Authority, CA) generiert wurde, um eine sichere Verbindung herzustellen. Führen Sie die folgenden Schritte aus, um die CER-Datei abzurufen, das Zertifikat zu decodieren und sie an Ihre Anwendung zu binden.

### <a name="download-the-certificate-file-from-the-certificate-authority-ca"></a>Herunterladen der Zertifikatdatei von der Zertifizierungsstelle (CA) 
Das erforderliche Zertifikat für die Kommunikation mit Ihrem Azure-Datenbank für PostgreSQL-Server über SSL befindet sich [hier](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt). Laden Sie die Zertifikatdatei lokal herunter.

### <a name="download-and-install-openssl-on-your-machine"></a>Herunterladen und Installieren von OpenSSL auf dem Computer 
Zum Decodieren der Zertifikatdatei, die Ihre Anwendung für die sichere Verbindung mit dem Datenbankserver benötigt, müssen Sie OpenSSL auf dem lokalen Computer installieren.

#### <a name="for-linux-os-x-or-unix"></a>Bei Linux, Mac OS X oder Unix
Die OpenSSL-Bibliotheken finden Sie im Quellcode direkt bei der [OpenSSL Software Foundation](http://www.openssl.org). Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren von OpenSSL auf Ihrem Linux-Computer. In diesem Artikel werden Befehle verwendet, die nachweislich auf Ubuntu 12.04 und höher ausgeführt werden.

Öffnen Sie eine Terminalsitzung, und installieren Sie OpenSSL.
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
Extrahieren Sie die Dateien aus dem Downloadpaket.
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
Geben Sie das Verzeichnis ein, in dem die Dateien extrahiert werden sollen. Standardmäßig sollte folgendes Verzeichnis verwendet werden.

```bash
cd openssl-1.1.0e
```
Konfigurieren Sie OpenSSL, indem Sie den folgenden Befehl ausführen. Wenn Sie die Dateien in einem anderen Ordner als „/usr/local/openssl“ ablegen möchten, achten Sie darauf, den Befehl entsprechend zu ändern.

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
OpenSSL ist jetzt ordnungsgemäß konfiguriert, und Sie müssen es kompilieren, um Ihr Zertifikat zu konvertieren. Führen Sie dazu den folgenden Befehl aus:

```bash
make
```
Nach Abschluss der Kompilierung können Sie OpenSSL als ausführbare Datei installieren, indem Sie den folgenden Befehl ausführen:
```bash
make install
```
Um sicherzustellen, dass OpenSSL erfolgreich in Ihrem System installiert wurde, führen Sie den folgenden Befehl aus, und überprüfen Sie, ob Sie dieselbe Ausgabe erhalten.

```bash
/usr/local/openssl/bin/openssl version
```
Bei einer erfolgreichen Installation wird die folgende Meldung angezeigt:
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a>Für Windows
Die Installation von OpenSSL auf einem Windows-PC kann mit folgenden Methoden durchgeführt werden:
1. **(Empfohlen)**  Mithilfe des integrierten Bash für Windows-Funktionalität in Windows 10 und höher wird OpenSSL standardmäßig installiert. Anweisungen zum Aktivieren von Bash für Windows-Funktionalität in Windows 10 finden Sie [hier](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).
2. Durch Herunterladen einer von der Community bereitgestellten Win32/64-Anwendung. Während die OpenSSL Software Foundation keine bestimmten Windows-Installationsprogramme bereitstellt oder empfiehlt, bietet sie [hier](https://wiki.openssl.org/index.php/Binaries) eine Liste der verfügbaren Installationsprogramme.

### <a name="decode-your-certificate-file"></a>Decodieren der Zertifikatdatei
Die heruntergeladene Stamm-CA-Datei weist ein verschlüsseltes Format auf. Verwenden Sie OpenSSL, um die Zertifikatdatei zu decodieren. Führen Sie hierzu folgenden OpenSSL-Befehl aus:

```dos
OpenSSL>x509 -inform DER -in BaltimoreCyberTrustRoot.cer -text -out root.crt
```

### <a name="connecting-to-azure-database-for-postgresql-with-ssl-certificate-authentication"></a>Herstellen einer Verbindung mit Azure-Datenbank für PostgreSQL mit SSL-Zertifikatauthentifizierung
Jetzt haben Sie Ihr Zertifikat erfolgreich decodiert und können über SSL eine sichere Verbindung mit Ihrem Datenbankserver herstellen. Um eine Überprüfung des Serverzertifikats zu ermöglichen, muss das Zertifikat in die Datei „~/.postgresql/root.crt“ im Basisverzeichnis des Benutzers eingefügt werden. (Bei Microsoft Windows heißt die Datei „%APPDATA%\postgresql\root.crt“.) Nachfolgend erhalten Sie Anweisungen zum Herstellen einer Verbindung mit Azure-Datenbank für PostgreSQL.

#### <a name="using-psql-command-line-utility"></a>Verwendung des Befehlszeilenprogramms psql
In den folgenden Beispielen wird veranschaulicht, wie anhand der Befehlszeilenschnittstelle pgsql und des Befehlszeilenprogramms psql mithilfe der erstellten `root.crt`-Datei und der Option `sslmode=verify-ca` erfolgreich eine Verbindung zu Ihrem PostgreSQL-Server hergestellt wird.

Führen Sie über die PostgreSQL-Befehlszeilenschnittstelle den folgenden Befehl aus:
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mypgserver-20170401.postgres.database.azure.com dbname=postgres user=mylogin@mypgserver-20170401"
```
Daraufhin sollten Sie folgende Ausgabe erhalten:
```bash
Password for user mylogin@mypgserver-20170401:
psql (9.6.2)
WARNING: Console code page (437) differs from Windows code page (1252)
     8-bit characters might not work correctly. See psql reference
     page "Notes for Windows users" for details.
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
Type "help" for help.

postgres=>
```

#### <a name="using-pgadmin-gui-tool"></a>Verwendung des GUI-Tools pgAdmin
Um pgAdmin 4 für die Herstellung einer sicheren Verbindung über SSL zu konfigurieren, müssen Sie folgende Einstellungen für `SSL mode = Verify-CA` vornehmen:

![Screenshot von pgAdmin – Verbindung – SSL-Modus erforderlich](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a>Nächste Schritte
Überprüfen verschiedener Anwendungskonnektivitätsoptionen gemäß [Datenverbindungsbibliotheken für Azure-Datenbank für PostgreSQL](concepts-connection-libraries.md)
