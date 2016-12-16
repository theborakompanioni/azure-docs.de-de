---
title: "Andere Ports als 1433 für SQL-Datenbank | Microsoft Docs"
description: Bei Clientverbindungen von ADO.NET mit Azure SQL-Datenbank V12 wird der Proxy manchmal umgangen und direkt mit der Datenbank interagiert. Andere Ports als 1433 werden wichtig.
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
ms.assetid: 3f17106a-92fd-4aa4-b6a9-1daa29421f64
ms.service: sql-database
ms.custom: development
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fa3f1215c809fe2b45293c56cdca4496fa352afb


---
# <a name="ports-beyond-1433-for-adonet-45-and-sql-database-v12"></a>Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12
Dieses Thema beschreibt die Änderungen, die Azure SQL-Datenbank V12 im Hinblick auf das Verbindungsverhalten von Clients mit ADO.NET 4.5 oder einer höheren Version aufweist.

## <a name="v11-of-sql-database-port-1433"></a>SQL-Datenbank V11: Port 1433
Wenn Ihr Clientprogramm ADO.NET 4.5 zum Herstellen einer Verbindung und Abfragen mit SQL-Datenbank V11 verwendet, sieht der interne Ablauf wie folgt aus:

1. ADO.NET versucht, eine Verbindung mit der SQL-Datenbank herzustellen.
2. ADO.NET verwendet Port 1433 zum Aufrufen eines Middlewaremoduls, und die Middleware stellt eine Verbindung mit der SQL-Datenbank her.
3. Die SQL-Datenbank sendet die Antwort zurück an die Middleware, welche die Antwort an ADO.NET an Port 1433 weiterleitet.

**Terminologie:** Bei dem zuvor beschriebenen Ablauf ist davon die Rede, dass ADO.NET mithilfe der *Proxyroute*mit der SQL-Datenbank interagiert. Wäre keine Middleware einbezogen, wäre von einer Verwendung der *direkten Route* die Rede.

## <a name="v12-of-sql-database-outside-vs-inside"></a>SQL-Datenbank V12: "Außerhalb" im Vergleich zu "Innerhalb"
Bei Verbindungen mit V12 muss gefragt werden, ob das Clientprogramm *außerhalb* oder *innerhalb* der Azure-Cloudgrenzen ausgeführt wird. In den Unterabschnitten werden zwei häufige Szenarien erläutert.

#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Außerhalb:* Client wird auf dem Desktopcomputer ausgeführt.
Port 1433 ist der einzige Port, der auf dem Desktopcomputer geöffnet sein muss, auf dem die Clientanwendung der SQL-Datenbank gehostet ist.

#### <a name="inside-client-runs-on-azure"></a>*Innerhalb:* Client wird in Azure ausgeführt.
Wenn der Client innerhalb der Azure-Cloudgrenzen ausgeführt wird, verwendet er die sogenannte *direkte Route* für die Interaktion mit dem Azure SQL-Datenbankserver. Nachdem eine Verbindung hergestellt wurde, ist bei weiteren Interaktionen zwischen dem Client und der Datenbank kein Middleware-Proxy eingebunden.

Der Ablauf ist wie folgt:

1. ADO.NET 4.5 (oder höher) initiiert eine kurze Interaktion mit der Azure-Cloud und empfängt eine dynamisch ermittelte Portnummer.
   
   * Die dynamisch ermittelte Portnummer liegt im Bereich von 11000 bis 11999 oder 14000 bis 14999.
2. ADO.NET stellt dann eine direkte Verbindung mit dem SQL-Datenbankserver ohne Einbindung von Middleware her.
3. Abfragen werden direkt an die Datenbank gesendet, und Ergebnisse werden direkt an den Client zurückgegeben.

Stellen Sie sicher, dass der Portbereich von 11000 bis 11999 und 14000 bis 14999 auf Ihrem Azure-Clientcomputer für ADO.NET 4.5-Clientinteraktionen mit SQL-Datenbank V12 verfügbar ist.

* Insbesondere dürfen Ports in diesem Bereich keine anderen ausgehenden Blockierungen aufweisen.
* Auf Ihrer Azure-VM steuert die **Windows-Firewall mit erweiterter Sicherheit** die Porteinstellungen.
  
  * Sie können die [Benutzeroberfläche der Firewall](http://msdn.microsoft.com/library/cc646023.aspx) verwenden, um eine Regel hinzuzufügen, für die Sie das **TCP**-Protokoll sowie einen Portbereich mit einer Syntax wie **11000–11999** angeben.

## <a name="version-clarifications"></a>Erläuterungen zu Versionen
In diesem Abschnitt werden die Moniker erläutert, die auf Produktversionen verweisen. Außerdem sind einige Kombinationen von Versionen zwischen Produkten aufgeführt.

#### <a name="adonet"></a>ADO.NET
* ADO.NET 4.0 unterstützt das Protokoll TDS 7.3, aber nicht 7.4.
* ADO.NET 4.5 und höher unterstützt das Protokoll TDS 7.4.

#### <a name="sql-database-v11-and-v12"></a>SQL-Datenbank V11 und V12
In diesem Thema werden die Unterschiede bei der Clientverbindung zwischen SQL-Datenbank V11 und V12 hervorgehoben.

*Hinweis*: Die Transact-SQL-Anweisung `SELECT @@version;` gibt einen Wert zurück, der mit einer Zahl wie z.B. „11.“ oder „12.“ beginnt. Diese Zahlen stimmen mit den Versionsnamen V11 und V12 für die SQL-Datenbank überein.

## <a name="related-links"></a>Verwandte Links
* ADO.NET 4.6 wurde am 20. Juli 2015 veröffentlicht. Eine Ankündigung im Blog des .NET-Teams finden Sie [hier](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).
* ADO.NET 4.5 wurde am 15. August 2012 veröffentlicht. Eine Ankündigung im Blog des .NET-Teams finden Sie [hier](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
  
  * Ein Blogbeitrag zu ADO.NET 4.5.1 steht [hier](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx)bereit.
* [Liste der TDS-Protokollversionen](http://www.freetds.org/userguide/tdshistory.htm)
* [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md)
* [Firewall für die Azure SQL-Datenbank](sql-database-firewall-configure.md)
* [Konfigurieren der Firewalleinstellungen für Azure SQL-Datenbank](sql-database-configure-firewall-settings.md)




<!--HONumber=Nov16_HO3-->


