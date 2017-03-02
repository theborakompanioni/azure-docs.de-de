---
title: "Von Azure Data Catalog unterstützte Datenquellen | Microsoft Docs"
description: "Spezifikation für die derzeit unterstützten Datenquellen."
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
ms.date: 02/17/2017
ms.author: maroche
translationtype: Human Translation
ms.sourcegitcommit: e244432f0e7bbc9133610440de34d3316f465ebb
ms.openlocfilehash: a5669da2e2334a94acd7354a084f76dd9c8097e2
ms.lasthandoff: 02/18/2017


---

# <a name="azure-data-catalog-supported-data-sources"></a>Von Azure Data Catalog unterstützte Datenquellen

Sie können Metadaten auf verschiedenem Wege veröffentlichen: Sie können eine öffentliche API nutzen, ein ClickOnce-Registrierungstool verwenden oder die Informationen direkt in das Data Catalog-Webportal eingeben. Die folgende Tabelle fasst alle derzeit vom Katalog unterstützten Quellen zusammen und benennt jeweils die Veröffentlichungsmöglichkeiten.  Außerdem werden die externen Datentools aufgelistet, in denen die jeweiligen Quelle direkt aus dem Portal heraus geöffnet werden kann – wir nennen das die „Öffnen in“-Erfahrung im Portal. Die zweite Tabelle enthält weitere technische Spezifikationen für die Verbindungseigenschaften der einzelnen Datenquellen.


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
      <td>Azure Data Lake-Speicherverzeichnis</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure Data Lake-Speicherdatei</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure Storage-Blob</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure-Speicherverzeichnis</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure Storage-Tabelle</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>

    <tr>
      <td>HDFS-Verzeichnis</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HDFS-Datei</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Hive-Tabelle</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Struktur anzeigen</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL-Tabelle</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL-Ansicht</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle Database-Tabelle</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle Database-Ansicht</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Anderes (generisches Asset)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Data Warehouse-Tabelle</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Data Warehouse-Sicht</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-Dimension</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-KPI</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-Measure</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Reporting Services-Bericht</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Browser</font></td>
      <td><font size=2>Nur Server im nativen Modus. SharePoint-Modus wird nicht unterstützt.</font></td>
    </tr>

    <tr>
      <td>SQL Server-Tabelle</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server-Ansicht</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata-Tabelle</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata-Sicht</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP HANA-Sicht</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>DB2-Tabelle</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>DB2-Sicht</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Dateisystemdatei</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP-Verzeichnis</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP-Datei</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-Bericht</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-Endpunkt</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-Datei</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>OData-Entitätenmenge</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>OData-Funktion</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>PostgreSQL-Tabelle</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>PostgreSQL-Sicht</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP HANA-Sicht</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td> Salesforce-Objekt</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SharePoint-Liste </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
  
    <tr>
      <td>Azure DocumentDB-Sammlung</td>
      <td>✓</td>
      <td>✓ </td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

</table>

Wenn Sie Unterstützung für zusätzliche Quellen benötigen, senden Sie einen Featurewunsch über das [Azure Data Catalog-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


<br>
<br>
## <a name="data-source-reference-specification"></a>Datenquellen-Referenzspezifikation
> [!NOTE]
> Die Spalte „DSL-Struktur“ in der folgenden Tabelle listet nur die Verbindungseigenschaften für den Eigenschaftenbehälter „address“ auf, die von Azure Data Catalog verwendet werden. Der Eigenschaftenbehälter „address“ kann also andere Verbindungseigenschaften der Datenquelle enthalten, die Azure Data Catalog beibehält, jedoch nicht verwendet.
<table>
    <tr>
       <td><b>Quellentyp</b></td>
       <td><b>Assettyp</b></td>
       <td><b>Objekttypen</b></td>
       <td><b>DSL-Struktur<b></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Container</td>
      <td>Data Lake</td>
      <td>
        <font size=2> protocol: webhdfs <br>authentication: {basic, oauth} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Tabelle</td>
      <td>Verzeichnis, Datei</td>
      <td>
        <font size=2> protocol: webhdfs <br>authentication: {basic, oauth} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2> protocol: azure-blobs <br>authentication: {azure-access-key} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container </font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Tabelle</td>
      <td>Blob, Verzeichnis</td>
      <td>
        <font size=2> protocol: azure-blobs <br>authentication: {azure-access-key} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2> protocol: azure-tables <br>authentication: {azure-access-key} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account </font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Tabelle</td>
      <td>Tabelle</td>
      <td>
        <font size=2> protocol: azure-tables <br>authentication: {azure-access-key} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name </font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Container</td>
      <td>Virtueller Cluster</td>
      <td>
        <font size=2> protocol: cosmos <br>authentication: {basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Tabelle</td>
      <td>Stream, Streamgruppe, Ansicht</td>
      <td>
        <font size=2> protocol: cosmos <br>authentication: {basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Container</td>
      <td>Website</td>
      <td>
        <font size=2> protocol: http <br>authentication: {none, basic, windows, oauth} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Bericht</td>
      <td>Bericht, Dashboard</td>
      <td>
        <font size=2> protocol: http <br>authentication: {none, basic, windows, oauth} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: db2 <br>authentication: {basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: db2 <br>authentication: {basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema </font>
      </td>
    </tr>
    <tr>
      <td>Dateisystem</td>
      <td>Tabelle</td>
      <td>File</td>
      <td>
        <font size=2> protocol: file <br>authentication: {none, basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path </font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Tabelle</td>
      <td>Verzeichnis, Datei</td>
      <td>
        <font size=2> protocol: ftp <br>authentication: {none, basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Container</td>
      <td>Cluster</td>
      <td>
        <font size=2> protocol: webhdfs <br>authentication: {basic, oauth} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Tabelle</td>
      <td>Verzeichnis, Datei</td>
      <td>
        <font size=2> protocol: webhdfs <br>authentication: {basic, oauth} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: hive <br>authentication: {hdinsight, basic, username, none} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: hive <br>authentication: {hdinsight, basic, username, none} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2} </font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Container</td>
      <td>Website</td>
      <td>
        <font size=2> protocol: http <br>authentication: {none, basic, windows, oauth} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Bericht</td>
      <td>Bericht, Dashboard</td>
      <td>
        <font size=2> protocol: http <br>authentication: {none, basic, windows, oauth} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Tabelle</td>
      <td>Endpunkt, Datei</td>
      <td>
        <font size=2> protocol: http <br>authentication: {none, basic, windows, oauth} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: mysql <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: mysql <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Container</td>
      <td>Entitätencontainer</td>
      <td>
        <font size=2> protocol: odata <br>authentication: {none, basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Tabelle</td>
      <td>Entitätenmenge, Funktion</td>
      <td>
        <font size=2> protocol: odata <br>authentication: {none, basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; resource </font>
      </td>
    </tr>
    <tr>
      <td>Oracle-Datenbank</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: oracle <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>Oracle-Datenbank</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: oracle <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: postgresql <br>authentication: {basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: postgresql <br>authentication: {basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Container</td>
      <td>Website</td>
      <td>
        <font size=2> protocol: http <br>authentication: {none, basic, windows, oauth} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Bericht</td>
      <td>Bericht, Dashboard</td>
      <td>
        <font size=2> protocol: http <br>authentication: {none, basic, windows, oauth} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Tabelle</td>
      <td>Datenmashup</td>
      <td>
        <font size=2> Protokoll: Power Query <br>Authentifizierung: {oauth} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>Salesforce</td>
      <td>Tabelle</td>
      <td>Objekt</td>
      <td>
        <font size=2> protocol: salesforce-com <br>authentication: {basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; class <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; itemName </font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2> protocol: sap-hana-sql <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server </font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Tabelle</td>
      <td>Sicht</td>
      <td>
        <font size=2> protocol: sap-hana-sql <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>Tabelle</td>
      <td>Auflisten</td>
      <td>
        <font size=2> protocol: sharepoint-list <br>authentication: {basic, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url </font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Befehl</td>
      <td>Stored Procedure (Gespeicherte Prozedur)</td>
      <td>
        <font size=2> protocol: tds <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>TableValuedFunction</td>
      <td>Tabellenwertfunktion</td>
      <td>
        <font size=2> protocol: tds <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: tds <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: tds <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Befehl</td>
      <td>Stored Procedure (Gespeicherte Prozedur)</td>
      <td>
        <font size=2> protocol: tds <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Tabellenwertfunktion</td>
      <td>
        <font size=2> protocol: tds <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: tds <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: tds <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services – mehrdimensional</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size=2> protocol: analysis-services <br>authentication: {windows, basic, anonymous, none} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services – mehrdimensional</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2> protocol: analysis-services <br>authentication: {windows, basic, anonymous, none} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services – mehrdimensional</td>
      <td>"Measure"</td>
      <td>"Measure"</td>
      <td>
        <font size=2> protocol: analysis-services <br>authentication: {windows, basic, anonymous, none} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services – mehrdimensional</td>
      <td>Tabelle</td>
      <td>Dimension</td>
      <td>
        <font size=2> protocol: analysis-services <br>authentication: {windows, basic, anonymous, none} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Dimension} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size=2> protocol: analysis-services <br>authentication: {windows, basic, anonymous, none} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2> protocol: analysis-services <br>authentication: {windows, basic, anonymous, none} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>"Measure"</td>
      <td>"Measure"</td>
      <td>
        <font size=2> protocol: analysis-services <br>authentication: {windows, basic, anonymous, none} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>Tabelle</td>
      <td>Tabelle</td>
      <td>
        <font size=2> protocol: analysis-services <br>authentication: {windows, basic, anonymous, none} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Table} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2> protocol: reporting-services <br>authentication: {windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Bericht</td>
      <td>Bericht</td>
      <td>
        <font size=2> protocol: reporting-services <br>authentication: {windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010} </font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: teradata <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Table</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: teradata <br>authentication: {protocol, windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size="2"> protocol: mssql-mds <br>authentication: {windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Tabelle</td>
      <td>Entität</td>
      <td>
        <font size="2"> protocol: mssql-mds <br>authentication: {windows} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; entity </font>
      </td>
    </tr>
    <tr>
      <td>Azure DocumentDB</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: document-db <br>authentication: {azure-access-key} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database </font>
      </td>
    </tr>
    <tr>
      <td>Azure DocumentDB</td>
      <td>Sammlung</td>
      <td>Sammlung</td>
      <td>
        <font size=2> protocol: document-db <br>authentication: {azure-access-key} <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; collection </font>
      </td>
    </tr>
    <tr>
      <td>Andere (keine der oben genannten)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2> protocol: generic-asset <br>address: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; assetId </font>
      </td>
    </tr>
</table>

