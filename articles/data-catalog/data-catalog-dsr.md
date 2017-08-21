---
title: "Von Azure Data Catalog unterstützte Datenquellen | Microsoft-Dokumentation"
description: "In diesem Artikel sind die derzeit unterstützten Datenquellen aufgeführt."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: jstevens
editor: 
tags: 
ms.assetid: fd4345ca-2ed8-4c5e-9c4b-f954be2fc9f9
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 1ef3903bc4c401988c0f8b27dc9f9f0f3ec99466
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017

---

# <a name="supported-data-sources-in-azure-data-catalog"></a>Von Azure Data Catalog unterstützte Datenquellen

Sie können Metadaten auf verschiedene Weisen veröffentlichen: Sie können eine öffentliche API nutzen, ein ClickOnce-Registrierungstool verwenden oder die Informationen direkt in das Azure Data Catalog-Webportal eingeben. In der folgenden Tabelle sind alle derzeit vom Katalog unterstützten Datenquellen zusammengefasst. Außerdem werden die jeweiligen Veröffentlichungsmöglichkeiten genannt. Ferner sind die externen Datentools aufgelistet, in denen die jeweiligen Datenquelle direkt im Portal geöffnet werden kann. Die zweite Tabelle enthält weitere technische Angaben zu den Verbindungseigenschaften der einzelnen Datenquellen.


## <a name="list-of-supported-data-sources"></a>Liste der unterstützten Datenquellen

<table>
    <tr>
       <td><b>Datenquellenobjekt</b></td>
       <td><b>API</b></td>
       <td><b>Manuelle Eingabe</b></td>
       <td><b>Registrierungstool</b></td>
       <td><b>Tool für „Öffnen in“</b></td>
       <td><b>Hinweise</b></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store-Verzeichnis</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store-Datei</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure-Blobspeicher</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Storage-Verzeichnis</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Storage-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>
    <tr>
      <td>HDFS-Verzeichnis</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>HDFS-Datei</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Hive-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Struktur anzeigen</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>MySQL-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>MySQL-Sicht</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Oracle Database-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Oracle Database-Sicht</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Anderes (generisches Objekt)</td>
      <td>✓ </td>
      <td>✓ </td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure SQL Data Warehouse-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Excel, Power BI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Data Warehouse-Sicht</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Excel, Power BI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Dimension</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-KPI</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Measure</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services-Bericht</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Browser</font></td>
      <td><font size=2>Nur Server im nativen Modus. SharePoint-Modus wird nicht unterstützt.</font></td>
    </tr>
    <tr>
      <td>SQL Server-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Excel, Power BI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server-Sicht</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Excel, Power BI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Teradata-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Teradata-Sicht</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SAP HANA-Sicht</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>DB2-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>DB2-Sicht</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Datei im Dateisystem</td>
      <td>✓ </td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>FTP-Verzeichnis</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>FTP-Datei</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>HTTP-Bericht</td>
      <td>✓ </td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>HTTP-Endpunkt</td>
      <td>✓ </td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>HTTP-Datei</td>
      <td>✓ </td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>OData-Entitätenmenge</td>
      <td>✓ </td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>OData-Funktion</td>
      <td>✓ </td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>PostgreSQL-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>PostgreSQL-Sicht</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SAP HANA-Sicht</td>
      <td>✓ </td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td> Salesforce-Objekt</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SharePoint-Liste </td>
      <td>✓ </td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Cosmos DB-Sammlung</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Generische ODBC-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Generische ODBC-Sicht</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Cassandra-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2>Als generisches ODBC-Objekt veröffentlichen</font></td>
    </tr>
    <tr>
      <td>Cassandra-Sicht</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2>Als generisches ODBC-Objekt veröffentlichen</font></td>
    </tr>
    <tr>
      <td>Sybase-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Sybase-Sicht</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>MongoDB-Tabelle</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2>Als generisches ODBC-Objekt veröffentlichen</font></td>
    </tr>
    <tr>
      <td>MongoDB-Sicht</td>
      <td>✓ </td>
      <td>✓ </td>
      <td>✓ </td>
      <td><font size=2></font></td>
      <td><font size=2>Als generisches ODBC-Objekt veröffentlichen</font></td>
    </tr>
</table>

Wenn Sie Unterstützung für zusätzliche Quellen benötigen, senden Sie Ihren Wunsch über das [Azure Data Catalog-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


## <a name="data-source-reference-specification"></a>Datenquellenreferenz
> [!NOTE]
> Die Spalte **DSL-Struktur** in der folgenden Tabelle listet nur die Verbindungseigenschaften für den Eigenschaftenbehälter „address“ auf, die von Azure Data Catalog verwendet werden. Der Eigenschaftenbehälter „address“ kann also andere Verbindungseigenschaften der Datenquelle enthalten, die Azure Data Catalog beibehält, jedoch nicht verwendet.

<table>
    <tr>
       <td><b>Quellentyp</b></td>
       <td><b>Datenobjekttyp</b></td>
       <td><b>Objekttypen</b></td>
       <td><b>DSL-Struktur<b></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Container</td>
      <td>Data Lake</td>
      <td>
        <font size=2> Protokoll: webhdfs <br>Authentifizierung: {Standard, OAuth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Table</td>
      <td>Verzeichnis, Datei</td>
      <td>
        <font size=2> Protokoll: webhdfs <br>Authentifizierung: {Standard, OAuth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2> Protokoll: azure-blobs <br>Authentifizierung: {Azure-Zugriffsschlüssel} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container </font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Table</td>
      <td>Blob, Verzeichnis</td>
      <td>
        <font size=2> Protokoll: azure-blobs <br>Authentifizierung: {Azure-Zugriffsschlüssel} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2> Protokoll: azure-tables <br>Authentifizierung: {Azure-Zugriffsschlüssel} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account </font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Tabelle</td>
      <td>Table</td>
      <td>
        <font size=2> Protokoll: azure-tables <br>Authentifizierung: {Azure-Zugriffsschlüssel} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Container</td>
      <td>Virtueller Cluster</td>
      <td>
        <font size=2> Protokoll: cosmos <br>Authentifizierung: {Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Table</td>
      <td>Stream, Streamgruppe, Sicht</td>
      <td>
        <font size=2> Protokoll: cosmos <br>Authentifizierung: {Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Container</td>
      <td>Website</td>
      <td>
        <font size=2> Protokoll: http <br>Authentifizierung: {keine, Standard, Windows, OAuth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Bericht</td>
      <td>Bericht, Dashboard</td>
      <td>
        <font size=2> Protokoll: http <br>Authentifizierung: {keine, Standard, Windows, OAuth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> Protokoll: db2 <br>Authentifizierung: {Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Table</td>
      <td>Tabelle, Sicht</td>
      <td>
        <font size=2> Protokoll: db2 <br>Authentifizierung: {Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema </font>
      </td>
    </tr>
    <tr>
      <td>Dateisystem</td>
      <td>Table</td>
      <td>File</td>
      <td>
        <font size=2> Protokoll: file <br>Authentifizierung: {keine, Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path </font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Table</td>
      <td>Verzeichnis, Datei</td>
      <td>
        <font size=2> Protokoll: ftp <br>Authentifizierung: {keine, Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Container</td>
      <td>Cluster</td>
      <td>
        <font size=2> Protokoll: webhdfs <br>Authentifizierung: {Standard, OAuth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Table</td>
      <td>Verzeichnis, Datei</td>
      <td>
        <font size=2> Protokoll: webhdfs <br>Authentifizierung: {Standard, OAuth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> Protokoll: hive <br>Authentifizierung: {HDInsight, Standard, Benutzername, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Table</td>
      <td>Tabelle, Sicht</td>
      <td>
        <font size=2> Protokoll: hive <br>Authentifizierung: {HDInsight, Standard, Benutzername, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Container</td>
      <td>Website</td>
      <td>
        <font size=2> Protokoll: http <br>Authentifizierung: {keine, Standard, Windows, OAuth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Bericht</td>
      <td>Bericht, Dashboard</td>
      <td>
        <font size=2> Protokoll: http <br>Authentifizierung: {keine, Standard, Windows, OAuth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Table</td>
      <td>Endpunkt, Datei</td>
      <td>
        <font size=2> Protokoll: http <br>Authentifizierung: {keine, Standard, Windows, OAuth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> Protokoll: mysql <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Table</td>
      <td>Tabelle, Sicht</td>
      <td>
        <font size=2> Protokoll: mysql <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Container</td>
      <td>Entitätencontainer</td>
      <td>
        <font size=2> Protokoll: odata <br>Authentifizierung: {keine, Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Table</td>
      <td>Entitätenmenge, Funktion</td>
      <td>
        <font size=2> Protokoll: odata <br>Authentifizierung: {keine, Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; resource </font>
      </td>
    </tr>
    <tr>
      <td>Oracle-Datenbank</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> Protokoll: oracle <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>Oracle-Datenbank</td>
      <td>Table</td>
      <td>Tabelle, Sicht</td>
      <td>
        <font size=2> Protokoll: oracle <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> Protokoll: postgresql <br>Authentifizierung: {Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Table</td>
      <td>Tabelle, Sicht</td>
      <td>
        <font size=2> Protokoll: postgresql <br>Authentifizierung: {Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Container</td>
      <td>Website</td>
      <td>
        <font size=2> Protokoll: http <br>Authentifizierung: {keine, Standard, Windows, OAuth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Bericht</td>
      <td>Bericht, Dashboard</td>
      <td>
        <font size=2> Protokoll: http <br>Authentifizierung: {keine, Standard, Windows, OAuth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Table</td>
      <td>Datenmashup</td>
      <td>
        <font size=2> Protokoll: power-query <br>Authentifizierung: {OAuth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Salesforce</td>
      <td>Tabelle</td>
      <td>Objekt</td>
      <td>
        <font size=2> Protokoll: salesforce-com <br>Authentifizierung: {Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; class <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; itemName </font>
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2> Protokoll: sap-hana-sql <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server </font>
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Table</td>
      <td>Sicht</td>
      <td>
        <font size=2> Protokoll: sap-hana-sql <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>Tabelle</td>
      <td>Auflisten</td>
      <td>
        <font size=2> Protokoll: sharepoint-list <br>Authentifizierung: {Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Befehl</td>
      <td>Gespeicherte Prozedur</td>
      <td>
        <font size=2> Protokoll: tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>TableValuedFunction</td>
      <td>Tabellenwertfunktion</td>
      <td>
        <font size=2> Protokoll: tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> Protokoll: tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Table</td>
      <td>Tabelle, Sicht</td>
      <td>
        <font size=2> Protokoll: tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Befehl</td>
      <td>Gespeicherte Prozedur</td>
      <td>
        <font size=2> Protokoll: tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Tabellenwertfunktion</td>
      <td>
        <font size=2> Protokoll: tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> Protokoll: tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Table</td>
      <td>Tabelle, Sicht</td>
      <td>
        <font size=2> Protokoll: tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services – mehrdimensional</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size=2> Protokoll: analysis-services <br>Authentifizierung: {Windows, Standard, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services – mehrdimensional</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2> Protokoll: analysis-services <br>Authentifizierung: {Windows, Standard, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services – mehrdimensional</td>
      <td>"Measure"</td>
      <td>"Measure"</td>
      <td>
        <font size=2> Protokoll: analysis-services <br>Authentifizierung: {Windows, Standard, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services – mehrdimensional</td>
      <td>Table</td>
      <td>Dimension</td>
      <td>
        <font size=2> Protokoll: analysis-services <br>Authentifizierung: {Windows, Standard, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Dimension} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size=2> Protokoll: analysis-services <br>Authentifizierung: {Windows, Standard, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2> Protokoll: analysis-services <br>Authentifizierung: {Windows, Standard, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>"Measure"</td>
      <td>"Measure"</td>
      <td>
        <font size=2> Protokoll: analysis-services <br>Authentifizierung: {Windows, Standard, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>Table</td>
      <td>Table</td>
      <td>
        <font size=2> Protokoll: analysis-services <br>Authentifizierung: {Windows, Standard, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Table} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2> Protokoll: reporting-services <br>Authentifizierung: {Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Bericht</td>
      <td>Bericht</td>
      <td>
        <font size=2> Protokoll: reporting-services <br>Authentifizierung: {Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> Protokoll: teradata <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Table</td>
      <td>Tabelle, Sicht</td>
      <td>
        <font size=2> Protokoll: teradata <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size="2"> Protokoll: mssql-mds <br>Authentifizierung: {Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Tabelle</td>
      <td>Entität</td>
      <td>
        <font size="2"> Protokoll: mssql-mds <br>Authentifizierung: {Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; entity </font>
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> Protokoll: document-db <br>Authentifizierung: {Azure-Zugriffsschlüssel} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Sammlung</td>
      <td>Sammlung</td>
      <td>
        <font size=2> Protokoll: document-db <br>Authentifizierung: {Azure-Zugriffsschlüssel} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; collection </font>
      </td>
    </tr>
    <tr>
      <td>Generisches ODBC</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> Protokoll: odbc <br>Authentifizierung: {Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>Generisches ODBC</td>
      <td>Table</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> Protokoll: odbc <br>Authentifizierung: {Standard, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema </font>
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> Protokoll: sybase <br>authentication: {basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Table</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> Protokoll: sybase <br>authentication: {basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>Andere (keine der oben genannten)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2> Protokoll: generic-asset <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; assetId </font>
      </td>
    </tr>
</table>

