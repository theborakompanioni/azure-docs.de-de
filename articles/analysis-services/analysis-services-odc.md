---
title: "Erstellen einer ODC-Datei für die Verbindung mit einem Azure Analysis Services-Server | Microsoft Docs"
description: Erfahren Sie, wie Sie eine Office Data-Verbindung mit einem Analysis Services-Server in Azure herstellen und Daten von diesem abrufen.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 194910a3e4cb655b39a64d2540994d90d34a68e4
ms.openlocfilehash: 90a0cfb7c7cd7d3364ff42559dd7fdc7b2951abf
ms.contentlocale: de-de
ms.lasthandoff: 02/16/2017

---
# <a name="create-an-office-data-connection-odc-file"></a>Erstellen einer ODC-Datei (Office Data Connection)

Die Informationen in diesem Artikel beschreiben, wie Sie eine ODC-Datei erstellen, um aus Excel 2016, Versionsnummer 16.0.7369.2117 oder früher, oder aus Excel 2013 eine Verbindung mit einem Azure Analysis Services-Server herzustellen. Ein aktualisierter [MSOLAP.7-Anbieter](analysis-services-data-providers.md) ist ebenfalls erforderlich.


1. Kopieren Sie die ODC-Beispieldatei für die Verbindung unten, und fügen Sie sie in einem Text-Editor ein. 

2. Ändern Sie in **odc:ConnectionString** die folgenden Eigenschaften:

    *   In **Data Source=asazure://*region*.asazure.windows.net/*servername*;**: Ändern Sie *region* in die Region Ihres Analysis Services-Server und *servername* in den Namen Ihres Servers.

    *   In **Initial Catalog=*database*;**: Ändern Sie *database* in den Namen Ihrer Datenbank.

3. In **&lt;odc:CommandText>*Model*&lt;/odc:CommandText>**: Ändern Sie *Model* in den Namen Ihres Modells oder Ihrer Perspektive. 

4. Speichern Sie die Datei mit der Erweiterung **.odc** im Ordner „C:\Benutzer\\*Benutzername*\Dokumente\My Data Sources“.

5. Klicken Sie mit der rechten Maustaste auf die Datei, und klicken Sie dann auf **In Excel öffnen**. Oder klicken Sie in Excel auf dem Menüband **Daten** auf **Vorhandene Verbindungen**, wählen Sie Ihre Datei aus, und klicken Sie dann auf **Öffnen**.



**ODC-Beispielverbindungsdatei**
```
<html xmlns:o="urn:schemas-microsoft-com:office:office"
xmlns="http://www.w3.org/TR/REC-html40">

<head>
<meta http-equiv=Content-Type content="text/x-ms-odc; charset=utf-8">
<meta name=ProgId content=ODC.Cube>
<meta name=SourceType content=OLEDB>
<meta name=Catalog content="Database">
<meta name=Table content=Model>
<title>AzureAnalysisServicesConnection</title>
<xml id=docprops><o:DocumentProperties
  xmlns:o="urn:schemas-microsoft-com:office:office"
  xmlns="http://www.w3.org/TR/REC-html40">
  <o:Name>SampleAzureAnalysisServices</o:Name>
 </o:DocumentProperties>
</xml><xml id=msodc><odc:OfficeDataConnection
  xmlns:odc="urn:schemas-microsoft-com:office:odc"
  xmlns="http://www.w3.org/TR/REC-html40">
  <odc:Connection odc:Type="OLEDB">
   <odc:ConnectionString>Provider=MSOLAP.7;Data Source=asazure://<region>.asazure.windows.net/<servername>;Initial Catalog=<database>;</odc:ConnectionString>
   <odc:CommandType>Cube</odc:CommandType>
   <odc:CommandText>Model</odc:CommandText>
  </odc:Connection>
 </odc:OfficeDataConnection>
</xml>
<style>
<!--
    .ODCDataSource
    {
    behavior: url(dataconn.htc);
    }
-->
</style>
 
</head>

<body onload='init()' scroll=no leftmargin=0 topmargin=0 rightmargin=0 style='border: 0px'>
<table style='border: solid 1px threedface; height: 100%; width: 100%' cellpadding=0 cellspacing=0 width='100%'> 
  <tr> 
    <td id=tdName style='font-family:arial; font-size:medium; padding: 3px; background-color: threedface'> 
      &nbsp; 
    </td> 
     <td id=tdTableDropdown style='padding: 3px; background-color: threedface; vertical-align: top; padding-bottom: 3px'>

      &nbsp; 
    </td> 
  </tr> 
  <tr> 
    <td id=tdDesc colspan='2' style='border-bottom: 1px threedshadow solid; font-family: Arial; font-size: 1pt; padding: 2px; background-color: threedface'>

      &nbsp; 
    </td> 
  </tr> 
  <tr> 
    <td colspan='2' style='height: 100%; padding-bottom: 4px; border-top: 1px threedhighlight solid;'> 
      <div id='pt' style='height: 100%' class='ODCDataSource'></div> 
    </td> 
  </tr> 
</table> 

  
<script language='javascript'> 

function init() { 
  var sName, sDescription; 
  var i, j; 
  
  try { 
    sName = unescape(location.href) 
  
    i = sName.lastIndexOf(".") 
    if (i>=0) { sName = sName.substring(1, i); } 
  
    i = sName.lastIndexOf("/") 
    if (i>=0) { sName = sName.substring(i+1, sName.length); } 

    document.title = sName; 
    document.getElementById("tdName").innerText = sName; 

    sDescription = document.getElementById("docprops").innerHTML; 
  
    i = sDescription.indexOf("escription>") 
    if (i>=0) { j = sDescription.indexOf("escription>", i + 11); } 

    if (i>=0 && j >= 0) { 
      j = sDescription.lastIndexOf("</", j); 

      if (j>=0) { 
          sDescription = sDescription.substring(i+11, j); 
        if (sDescription != "") { 
            document.getElementById("tdDesc").style.fontSize="x-small"; 
          document.getElementById("tdDesc").innerHTML = sDescription; 
          } 
        } 
      } 
    } 
  catch(e) { 

    } 
  } 
</script> 

</body> 
 
</html>

```




