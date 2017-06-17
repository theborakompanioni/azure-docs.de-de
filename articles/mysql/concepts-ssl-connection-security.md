---
title: "SSL-Konnektivität für Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "Informationen zum Konfigurieren von Azure-Datenbank für MySQL und zugehörigen Anwendungen für die richtige Verwendung von SSL-Verbindungen"
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 6dc147c9dd3983c2c599108162fa285d87ffc7a3
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>SSL-Konnektivität in Azure-Datenbank für MySQL
Azure-Datenbank für MySQL unterstützt die Verbindung Ihres Datenbankservers mit Clientanwendungen, die Secure Sockets Layer (SSL) verwenden. Das Erzwingen von SSL-Verbindungen zwischen dem Datenbankserver und Clientanwendungen trägt zum Schutz vor Man-in-the-Middle-Angriffen bei, indem der Datenstrom zwischen dem Server und der Anwendung verschlüsselt wird.

## <a name="default-settings"></a>Standardeinstellungen
Der Datenbankdienst sollte standardmäßig so konfiguriert sein, dass beim Herstellen einer Verbindung mit MySQL SSL-Verbindungen erforderlich sind.  Es wird empfohlen, die SSL-Option wenn möglich nicht zu deaktivieren. 

Bei der Bereitstellung eines neuen Azure-Datenbank für MySQL-Servers über das Azure-Portal und die CLI wird die Erzwingung von SSL-Verbindungen standardmäßig aktiviert. 

Entsprechend enthalten Verbindungszeichenfolgen, die im Azure-Portal unter dem Server in den Verbindungszeichenfolgen-Einstellungen vorab definiert sind, die erforderlichen Parameter für gängige Sprachen für die Verbindung mit dem Datenbankserver mithilfe von SSL. Der SSL-Parameter variiert je nach Connector, z.B. „ssl=true“ oder „sslmode=require“ oder „sslmode=required“ und weitere Variationen.

Informationen zum Aktivieren oder Deaktivieren von SSL-Verbindungen bei der Anwendungsentwicklung finden Sie unter [Konfigurieren von SSL](howto-configure-ssl.md).

## <a name="next-steps"></a>Nächste Schritte
[Datenverbindungsbibliotheken für Azure-Datenbank für MySQL](concepts-connection-libraries.md)

