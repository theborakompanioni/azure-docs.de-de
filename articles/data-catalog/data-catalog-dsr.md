<properties
   pageTitle="Von Azure Data Catalog unterstützte Datenquellen | Microsoft Azure"
   description="Spezifikation für die derzeit unterstützten Datenquellen."
   services="data-catalog"
   documentationCenter=""
   authors="trhabe"
   manager="jstrauss"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="trhabe"/>

# Von Azure Data Catalog unterstützte Datenquellen

Benutzer von Azure Data Catalog können Metadaten auf verschiedenem Wege veröffentlichen: Sie können eine öffentliche API nutzen, sie können ein ClickOnce-Registrierungstool verwenden, und sie können die Informationen direkt in das Data Catalog-Webportal eingeben. Die folgende Tabelle fasst alle derzeit vom Katalog unterstützten Quellen zusammen und benennt jeweils die Veröffentlichungsmöglichkeiten. Außerdem werden die externen Datentools aufgelistet, in denen die jeweiligen Quelle direkt aus dem Portal heraus geöffnet werden kann – wir nennen das die „Öffnen in“-Erfahrung im Portal. Die zweite Tabelle im Artikel enthält weitere technische Spezifikationen für die Verbindungseigenschaften der Datenquellen.


## Liste der unterstützten Datenquellen

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
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure Data Lake-Speicherdatei</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure Storage-Blob</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power&#160;BI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure-Speicherverzeichnis</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power&#160;BI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure Storage-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        ✓{✓1✓>✓&lt;✓1✓}✓{✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓>✓{✓1✓>✓&lt;✓1✓}✓&lt;✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓}✓{✓1✓>✓&lt;✓1✓}✓{✓{✓1✓>✓&lt;✓1✓}✓{✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓>✓{✓1✓>✓&lt;✓1✓}✓&lt;✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓}✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓{✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓>✓{✓1✓>✓&lt;✓1✓}✓&lt;✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓}✓{✓1✓>✓&lt;✓1✓}✓>✓{✓1✓>✓&lt;✓1✓}✓{✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓>✓{✓1✓>✓&lt;✓1✓}✓&lt;✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓}✓{✓1✓>✓&lt;✓1✓}✓&lt;✓{✓1✓>✓&lt;✓1✓}✓{✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓>✓{✓1✓>✓&lt;✓1✓}✓&lt;✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓}✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓{✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓>✓{✓1✓>✓&lt;✓1✓}✓&lt;✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓}✓{✓1✓>✓&lt;✓1✓}✓}✓{✓1✓>✓&lt;✓1✓}✓{✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓>✓{✓1✓>✓&lt;✓1✓}✓&lt;✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓}✓{✓1✓>✓&lt;✓1✓}✓
      </td>
    </tr>

    <tr>
      <td>HDFS-Verzeichnis</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HDFS-Datei</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Hive-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Struktur anzeigen</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td>Excel{Excel1Excel>Excel&lt;Excel1Excel}Excel{Excel{Excel1Excel>Excel&lt;Excel1Excel}Excel1Excel{Excel1Excel>Excel&lt;Excel1Excel}Excel>Excel{Excel1Excel>Excel&lt;Excel1Excel}Excel&lt;Excel{Excel1Excel>Excel&lt;Excel1Excel}Excel1Excel{Excel1Excel>Excel&lt;Excel1Excel}Excel}Excel{Excel1Excel>Excel&lt;Excel1Excel}Excel</td>
    </tr>

    <tr>
      <td>MySQL-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power&#160;BI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL-Ansicht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power&#160;BI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle Database-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power&#160;BI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle Database-Ansicht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power&#160;BI</font></td>
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
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Data Warehouse-Sicht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-Dimension</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power&#160;BI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-KPI</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power&#160;BI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-Measure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power&#160;BI</font></td>
      <td>Excel, Power&#160;BI{Excel, Power&#160;BI1Excel, Power&#160;BI>Excel, Power&#160;BI&lt;Excel, Power&#160;BI1Excel, Power&#160;BI}Excel, Power&#160;BI{Excel, Power&#160;BI{Excel, Power&#160;BI1Excel, Power&#160;BI>Excel, Power&#160;BI&lt;Excel, Power&#160;BI1Excel, Power&#160;BI}Excel, Power&#160;BI1Excel, Power&#160;BI{Excel, Power&#160;BI1Excel, Power&#160;BI>Excel, Power&#160;BI&lt;Excel, Power&#160;BI1Excel, Power&#160;BI}Excel, Power&#160;BI>Excel, Power&#160;BI{Excel, Power&#160;BI1Excel, Power&#160;BI>Excel, Power&#160;BI&lt;Excel, Power&#160;BI1Excel, Power&#160;BI}Excel, Power&#160;BI&lt;Excel, Power&#160;BI{Excel, Power&#160;BI1Excel, Power&#160;BI>Excel, Power&#160;BI&lt;Excel, Power&#160;BI1Excel, Power&#160;BI}Excel, Power&#160;BI1Excel, Power&#160;BI{Excel, Power&#160;BI1Excel, Power&#160;BI>Excel, Power&#160;BI&lt;Excel, Power&#160;BI1Excel, Power&#160;BI}Excel, Power&#160;BI}Excel, Power&#160;BI{Excel, Power&#160;BI1Excel, Power&#160;BI>Excel, Power&#160;BI&lt;Excel, Power&#160;BI1Excel, Power&#160;BI}Excel, Power&#160;BI</td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power&#160;BI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Reporting Services-Bericht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Browser</font></td>
      <td><font size=2>Nur Server im nativen Modus. SharePoint-Modus wird nicht unterstützt.</font></td>
    </tr>

    <tr>
      <td>SQL Server-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server-Ansicht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata-Sicht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP HANA-Sicht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power&#160;BI</font></td>
      <td><font size=2>Nur Berechnungs- und Analyseansichten; Attributansichten werden nicht unterstützt.</font></td>
    </tr>

    <tr>
      <td>DB2-Tabelle</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>DB2-Sicht</td>
      <td>✓</td>
      <td></td>
      <td></td>
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
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP-Datei</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td>✓{✓1✓>✓&lt;✓1✓}✓{✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓>✓{✓1✓>✓&lt;✓1✓}✓&lt;✓{✓1✓>✓&lt;✓1✓}✓1✓{✓1✓>✓&lt;✓1✓}✓}✓{✓1✓>✓&lt;✓1✓}✓</td>
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
      <td></td>
      <td></td>
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

</table>

Wenn Sie Unterstützung für zusätzliche Quellen benötigen, senden Sie einen Featurewunsch über das [Azure Data Catalog-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


<br> <br>
## Datenquellen-Referenzspezifikation
> [AZURE.NOTE] Die Spalte „DSL-Struktur“ in der folgenden Tabelle enthält nur die Verbindungseigenschaften für den Eigenschaftenbehälter „Adresse“, den Azure Data Catalog verwendet (d.h. Eigenschaftenbehälter „Adresse“ kann andere Verbindungseigenschaften der Datenquelle enthalten, die Azure Data Catalog weiterhin behält, jedoch nicht verwendet.)
<table>
    <tr>
       <td><b>Quellentyp</b></td>
       <td><b>Assettyp</b></td>
       <td><b>Objekttype(n)</b></td>
       <td><b>DSL-Struktur<b></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Container</td>
      <td>Data Lake</td>
      <td>
        <font size=2> protocol: webhdfs
            <br>authentication: {basic, oauth}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Tabelle</td>
      <td>Verzeichnis, Datei</td>
      <td>
        <font size=2> protocol: webhdfs
            <br>authentication: {basic, oauth}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2> protocol: azure-blobs
            <br>authentication: {azure-access-key}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; domain
            <br>&#160;&#160;&#160;&#160;&#160; account
            <br>&#160;&#160;&#160;&#160;&#160; container </font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Tabelle</td>
      <td>Blob, Verzeichnis</td>
      <td>
        <font size=2> protocol: azure-blobs
            <br>authentication: {azure-access-key}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; domain
            <br>&#160;&#160;&#160;&#160;&#160; account
            <br>&#160;&#160;&#160;&#160;&#160; container
            <br>&#160;&#160;&#160;&#160;&#160; name </font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2> protocol: azure-tables
            <br>authentication: {azure-access-key}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; domain
            <br>&#160;&#160;&#160;&#160;&#160; account </font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Tabelle</td>
      <td>Tabelle</td>
      <td>
        <font size=2> protocol: azure-tables
            <br>authentication: {azure-access-key}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; domain
            <br>&#160;&#160;&#160;&#160;&#160; account
            <br>&#160;&#160;&#160;&#160;&#160; name </font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Container</td>
      <td>Virtueller Cluster</td>
      <td>
        <font size=2> protocol: cosmos
            <br>authentication: {basic, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Tabelle</td>
      <td>Stream, Streamgruppe, Ansicht</td>
      <td>
        <font size=2> protocol: cosmos
            <br>authentication: {basic, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Container</td>
      <td>Website</td>
      <td>
        <font size=2> protocol: http
            <br>authentication: {none, basic, windows, oauth}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Bericht</td>
      <td>Bericht, Dashboard</td>
      <td>
        <font size=2> protocol: http
            <br>authentication: {none, basic, windows, oauth}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: db2
            <br>authentication: {basic, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database </font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: db2
            <br>authentication: {basic, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; Objekt
            <br>&#160;&#160;&#160;&#160;&#160; schema </font>
      </td>
    </tr>
    <tr>
      <td>Dateisystem</td>
      <td>Tabelle</td>
      <td>File</td>
      <td>
        <font size=2> protocol: file
            <br>authentication: {none, basic, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; path </font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Tabelle</td>
      <td>Verzeichnis, Datei</td>
      <td>
        <font size=2> protocol: ftp
            <br>authentication: {none, basic, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Container</td>
      <td>Cluster</td>
      <td>
        <font size=2> protocol: webhdfs
            <br>authentication: {basic, oauth}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Tabelle</td>
      <td>Verzeichnis, Datei</td>
      <td>
        <font size=2> protocol: webhdfs
            <br>authentication: {basic, oauth}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: hive
            <br>authentication: {hdinsight, basic, username, none}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>connectionProperties:
            <br>&#160;&#160;&#160;&#160;&#160; serverProtocol: {hive2} </font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: hive
            <br>authentication: {hdinsight, basic, username, none}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; Objekt
            <br>connectionProperties:
            <br>&#160;&#160;&#160;&#160;&#160; serverProtocol: {hive2} </font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Container</td>
      <td>Website</td>
      <td>
        <font size=2> protocol: http
            <br>authentication: {none, basic, windows, oauth}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Bericht</td>
      <td>Bericht, Dashboard</td>
      <td>
        <font size=2> protocol: http
            <br>authentication: {none, basic, windows, oauth}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Tabelle</td>
      <td>Endpunkt, Datei</td>
      <td>
        <font size=2> protocol: http
            <br>authentication: {none, basic, windows, oauth}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: mysql
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database </font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: mysql
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; object </font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Container</td>
      <td>Entitätencontainer</td>
      <td>
        <font size=2> protocol: odata
            <br>authentication: {none, basic, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Tabelle</td>
      <td>Entitätenmenge, Funktion</td>
      <td>
        <font size=2> protocol: odata
            <br>authentication: {none, basic, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url
            <br>&#160;&#160;&#160;&#160;&#160; resource </font>
      </td>
    </tr>
    <tr>
      <td>Oracle-Datenbank</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: oracle
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database </font>
      </td>
    </tr>
    <tr>
      <td>Oracle-Datenbank</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: oracle
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; schema
            <br>&#160;&#160;&#160;&#160;&#160; object </font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: postgresql
            <br>authentication: {basic, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database </font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: postgresql
            <br>authentication: {basic, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; schema
            <br>&#160;&#160;&#160;&#160;&#160; object </font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Container</td>
      <td>Website</td>
      <td>
        <font size=2> protocol: http
            <br>authentication: {none, basic, windows, oauth}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Bericht</td>
      <td>Bericht, Dashboard</td>
      <td>
        <font size=2> protocol: http
            <br>authentication: {none, basic, windows, oauth}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Tabelle</td>
      <td>Datenmashup</td>
      <td>
        <font size=2> Protokoll: Power Query
            <br>Authentifizierung: {oauth}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>Salesforce</td>
      <td>Tabelle</td>
      <td>Objekt</td>
      <td>
        <font size=2> protocol: salesforce-com
            <br>authentication: {basic, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; loginServer
            <br>&#160;&#160;&#160;&#160;&#160; class
            <br>&#160;&#160;&#160;&#160;&#160; itemName </font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2> protocol: sap-hana-sql
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server </font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Tabelle</td>
      <td>Sicht</td>
      <td>
        <font size=2> protocol: sap-hana-sql
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; schema
            <br>&#160;&#160;&#160;&#160;&#160; object </font>
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>Tabelle</td>
      <td>Auflisten</td>
      <td>
        <font size=2> protocol: sharepoint-list
            <br>authentication: {basic, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; url </font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Befehl</td>
      <td>Stored Procedure (Gespeicherte Prozedur)</td>
      <td>
        <font size=2> protocol: tds
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; schema
            <br>&#160;&#160;&#160;&#160;&#160; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>TableValuedFunction</td>
      <td>Tabellenwertfunktion</td>
      <td>
        <font size=2> protocol: tds
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; schema
            <br>&#160;&#160;&#160;&#160;&#160; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: tds
          <br>authentication: {protocol, windows}
          <br>address:
          <br>&#160;&#160;&#160;&#160;&#160; server
          <br>&#160;&#160;&#160;&#160;&#160; database </font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: tds
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; schema
            <br>&#160;&#160;&#160;&#160;&#160; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Befehl</td>
      <td>Stored Procedure (Gespeicherte Prozedur)</td>
      <td>
        <font size=2> protocol: tds
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; schema
            <br>&#160;&#160;&#160;&#160;&#160; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Tabellenwertfunktion</td>
      <td>
        <font size=2> protocol: tds
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; schema
            <br>&#160;&#160;&#160;&#160;&#160; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: tds
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: tds
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; schema
            <br>&#160;&#160;&#160;&#160;&#160; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services – mehrdimensional</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size=2> protocol: analysis-services
            <br>authentication: {windows, basic, anonymous, none}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; model </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services – mehrdimensional</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2> protocol: analysis-services
            <br>authentication: {windows, basic, anonymous, none}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; model
            <br>&#160;&#160;&#160;&#160;&#160; Objekt
            <br>&#160;&#160;&#160;&#160;&#160; objectType: {KPI} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services – mehrdimensional</td>
      <td>"Measure"</td>
      <td>"Measure"</td>
      <td>
        <font size=2> protocol: analysis-services
            <br>authentication: {windows, basic, anonymous, none}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; model
            <br>&#160;&#160;&#160;&#160;&#160; Objekt
            <br>&#160;&#160;&#160;&#160;&#160; objectType: {Measure} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services – mehrdimensional</td>
      <td>Tabelle</td>
      <td>Dimension</td>
      <td>
        <font size=2> protocol: analysis-services
            <br>authentication: {windows, basic, anonymous, none}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; model
            <br>&#160;&#160;&#160;&#160;&#160; objekt
            <br>&#160;&#160;&#160;&#160;&#160; objectType: {Dimension} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size=2> protocol: analysis-services
            <br>authentication: {windows, basic, anonymous, none}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; model </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2> protocol: analysis-services
            <br>authentication: {windows, basic, anonymous, none}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; model
            <br>&#160;&#160;&#160;&#160;&#160; Objekt
            <br>&#160;&#160;&#160;&#160;&#160; objectType: {KPI} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>"Measure"</td>
      <td>"Measure"</td>
      <td>
        <font size=2> protocol: analysis-services
            <br>authentication: {windows, basic, anonymous, none}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; model
            <br>&#160;&#160;&#160;&#160;&#160; Objekt
            <br>&#160;&#160;&#160;&#160;&#160; objectType: {Measure} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>Tabelle</td>
      <td>Tabelle</td>
      <td>
        <font size=2> protocol: analysis-services
            <br>authentication: {windows, basic, anonymous, none}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; model
            <br>&#160;&#160;&#160;&#160;&#160; Objekt
            <br>&#160;&#160;&#160;&#160;&#160; objectType: {Table} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2> protocol: reporting-services
            <br>authentication: {windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; version: {ReportingService2010} </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Bericht</td>
      <td>Bericht</td>
      <td>
        <font size=2> protocol: reporting-services
            <br>authentication: {windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; path
            <br>&#160;&#160;&#160;&#160;&#160; version: {ReportingService2010} </font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2> protocol: teradata
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database </font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Table</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2> protocol: teradata
            <br>authentication: {protocol, windows}
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; server
            <br>&#160;&#160;&#160;&#160;&#160; database
            <br>&#160;&#160;&#160;&#160;&#160; object </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size="2"> protocol: mssql-mds
          <br>authentication: {windows}
          <br>address:
          <br>&#160;&#160;&#160;&#160;&#160; url
          <br>&#160;&#160;&#160;&#160;&#160; model
          <br>&#160;&#160;&#160;&#160;&#160; version </font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Tabelle</td>
      <td>Entität</td>
      <td>
        <font size="2"> protocol: mssql-mds
          <br>authentication: {windows}
          <br>address:
          <br>&#160;&#160;&#160;&#160;&#160; url
          <br>&#160;&#160;&#160;&#160;&#160; model
          <br>&#160;&#160;&#160;&#160;&#160; Version
          <br>&#160;&#160;&#160;&#160;&#160; entity </font>
      </td>
    </tr>
    <tr>
      <td>Andere (keine der oben genannten)</td>
      <td>*</td>
      <td>*</td>
      <td>
        <font size=2> protocol: generic-asset
            <br>address:
            <br>&#160;&#160;&#160;&#160;&#160; assetId </font>
      </td>
    </tr>
</table>

<!---HONumber=AcomDC_0921_2016-->