---
title: Verwenden von Power BI Embedded mit REST | Microsoft Docs
description: 'Verwenden von Power BI Embedded mit REST  '
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 8bcef780-cca0-4f30-9a9b-9daa1a7ae865
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 02/06/2017
ms.author: asaxton
translationtype: Human Translation
ms.sourcegitcommit: 89e16687f858708cdfd1432114c39bd9109dc6ac
ms.openlocfilehash: 31624b9d15772a4f08cf013ac713b3aa636acfca
ms.lasthandoff: 02/07/2017


---
# <a name="how-to-use-power-bi-embedded-with-rest"></a>Verwendung von Power BI Embedded mit REST

## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Power BI Embedded: Gegenstand und Funktionsbeschreibung

Einen Überblick über Power BI Embedded finden Sie auf der offiziellen Website von [Power BI Embedded](https://azure.microsoft.com/services/power-bi-embedded/). Verschaffen wir uns zunächst einen Eindruck, bevor wir uns näher mit der Verwendung mit REST beschäftigen.

Es ist wirklich ziemlich einfach. Sie können die dynamischen Datenvisualisierungen von [Power BI](https://powerbi.microsoft.com) in Ihrer eigenen Anwendung verwenden.

Die meisten benutzerdefinierten Anwendungen müssen Daten für Kunden bereitstellen, nicht unbedingt für Benutzer in der eigenen Organisation. Wenn Sie beispielsweise Dienste sowohl für Unternehmen A als auch für Unternehmen B bereitstellen, sollten Benutzer im Unternehmen A nur die Daten für ihr eigenes Unternehmen A sehen. Für die Bereitstellung ist also Mehrinstanzenfähigkeit erforderlich.

Darüber hinaus bietet die benutzerdefinierte Anwendung möglicherweise eigene Authentifizierungsmethoden wie Formularauthentifizierung, Standardauthentifizierung usw. In diesem Fall muss die eingebettete Lösung sicher mit diesen bestehenden Authentifizierungsmethoden zusammenarbeiten. Zudem müssen Benutzer in der Lage sein, diese ISV-Anwendungen zu verwenden, ohne zusätzlich ein Power BI-Abonnement erwerben oder lizenzieren zu müssen.

 **Power BI Embedded** wurde speziell für diese Art von Szenarien entwickelt. Nachdem wir diese kurze Einführung abgeschlossen haben, können wir nun näher ins Detail gehen.

Sie können das SDK für .NET \(C#) oder Node.js verwenden, um Ihre Anwendung mit Power BI Embedded zu erstellen. In diesem Artikel erläutern wir allerdings den HTTP-Ablauf \(einschließlich AuthN) von Power BI ohne SDKs. Wenn Sie diesen Ablauf kennen, verstehen Sie die wesentlichen Grundzüge von Power BI Embedded und können Ihre Anwendung **mit einer beliebigen Programmiersprache** entwickeln.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Erstellen der Power BI-Arbeitsbereichssammlung und Abrufen des Zugriffsschlüssels \(Bereitstellung)

Power BI Embedded ist einer der Azure-Dienste. Nutzungsgebühren fallen nur für den ISV an, der das Azure-Portal verwendet \(die Abrechnung erfolgt pro Sitzungsstunde des Benutzers). Für den Benutzer, der den Bericht anzeigt, werden keine Gebühren erhoben, ein Azure-Abonnement ist ebenfalls nicht erforderlich.
Bevor wir mit der Anwendungsentwicklung beginnen, müssen wir über das Azure-Portal die **Power BI-Arbeitsbereichssammlung** erstellen.

Jeder Arbeitsbereich von Power BI Embedded ist der Arbeitsbereich für einen Kunden (Mandanten), und wir können jeder Arbeitsbereichssammlung zahlreiche Arbeitsbereiche hinzufügen. In jeder Arbeitsbereichssammlung wird der gleiche Zugriffsschlüssel verwendet. Die Arbeitsbereichssammlung ist die Sicherheitsgrenze für Power BI Embedded.

![](media/power-bi-embedded-iframe/create-workspace.png)

Kopieren Sie den Zugriffsschlüssel aus dem Azure-Portal, wenn Sie die Erstellung der Arbeitsbereichssammlung abgeschlossen haben.

![](media/power-bi-embedded-iframe/copy-access-key.png)

> [!NOTE]
> Sie können die Arbeitsbereichssammlung auch bereitstellen und den Zugriffsschlüssel über die REST-API beziehen. Weitere Informationen finden Sie unter [Power BI Resource Provider APIs](https://msdn.microsoft.com/library/azure/mt712306.aspx)(Power BI-Ressourcenanbieter-APIs).

## <a name="create-pbix-file-with-power-bi-desktop"></a>Erstellen einer PBIX-Datei mit Power BI Desktop

Im nächsten Schritt müssen wir die Datenverbindung und die Berichte erstellen, die eingebettet werden sollen.
Für diese Aufgabe gibt es keine Programmierung und keinen Code. Wir verwenden ausschließlich Power BI Desktop.
In diesem Artikel werden keine Details zur Verwendung von Power BI Desktop erläutert. Falls Sie hierbei Unterstützung benötigen, finden Sie weitere Informationen unter [Erste Schritte mit Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). In unserem Beispiel verwenden wir das [Analysebeispiel für den Einzelhandel](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media/power-bi-embedded-iframe/power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Erstellen eines Power BI-Arbeitsbereichs

Nachdem die Bereitstellung vollständig abgeschlossen ist, beginnen wir jetzt damit, mithilfe von REST-APIs einen Kundenarbeitsbereich in der Arbeitsbereichssammlung zu erstellen. Die folgende HTTP POST-Anforderung (REST) erstellt einen neuen Arbeitsbereich in unserer bestehenden Arbeitsbereichssammlung. Dies ist die [POST-Arbeitsbereich-API](https://msdn.microsoft.com/library/azure/mt711503.aspx). In unserem Beispiel lautet der Name der Arbeitsbereichssammlung **mypbiapp**. Wir legen den Zugriffsschlüssel, den wir zuvor kopiert haben, als **AppKey**fest. So ist die Authentifizierung ganz einfach!

**HTTP-Anforderung**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-Antwort**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

Die zurückgegebene **WorkspaceId** wird für die folgenden API-Aufrufe verwendet. Dieser Wert muss von unserer Anwendung beibehalten werden.

## <a name="import-pbix-file-into-the-workspace"></a>Importieren einer PBIX-Datei in den Arbeitsbereich

Jeder Bericht in einem Arbeitsbereich entspricht einer einzelnen Power BI Desktop-Datei mit einem Dataset \(einschließlich Datenquelleneinstellungen). Wir können unsere PBIX-Datei wie in unten stehendem Code gezeigt in den Arbeitsbereich importieren. Wir können die Binärdaten der PBIX-Datei wie dargestellt mit mehrteiligem MIME in HTTP hochladen.

Das URI-Fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** ist die workspaceId, und der Abfrageparameter **datasetDisplayName** ist der zu erstellende Datasetname. Das erstellte Dataset enthält alle datenbezogenen Artefakte in der PBIX-Datei, wie beispielsweise importierte Daten, den Zeiger zur Datenquelle usw.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

Dieser Importvorgang kann eine Weile dauern. Sobald der Vorgang abgeschlossen ist, kann die Anwendung den Aufgabenstatus anhand der Import-ID abfragen. In unserem Beispiel lautet die Import-ID **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

Der folgende Code fragt den Status anhand dieser Import-ID ab:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Wenn die Aufgabe nicht abgeschlossen ist, lautet die HTTP-Antwort möglicherweise wie folgt:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Wenn die Aufgabe abgeschlossen ist, lautet die HTTP-Antwort eher folgendermaßen:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Datenquellenkonnektivität \(und Mehrinstanzenfähigkeit von Daten)

Die meisten Artefakte in der PBIX-Datei werden in unseren Arbeitsbereich importiert, nicht jedoch die Anmeldeinformationen für Datenquellen. Im **DirectQuery-Modus**kann der eingebettete Bericht daher nicht ordnungsgemäß angezeigt werden. Allerdings können wir den Bericht im **Importmodus**unter Verwendung der vorhandenen importierten Daten anzeigen. In diesem Fall müssen wir die folgenden Schritte durchführen, um die Anmeldeinformationen über REST-Aufrufe festzulegen.

Zunächst müssen wir die Gatewaydatenquelle ermitteln. Wir wissen, dass die **ID** des Datasets die zuvor zurückgegebene ID ist.

**HTTP-Anforderung**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-Antwort**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

Anhand der zurückgegebenen Gateway-ID und der Datenquellen-ID \(siehe vorherige Werte für **gatewayId** und **id** im Rückgabeergebnis) können wir die Anmeldeinformationen dieser Datenquelle wie folgt ändern:

**HTTP-Anforderung**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**HTTP-Antwort**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

Im Produktionsmodus können wir die REST-API verwenden, um für jeden Arbeitsbereich eine andere Verbindungszeichenfolge festzulegen. \(Wir können also die Datenbank für die einzelnen Kunden unterteilen.)

Der folgende Code ändert die Verbindungszeichenfolge der Datenquelle über REST.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Alternativ können wir die Sicherheit auf Zeilenebene in Power BI Embedded verwenden und die Daten für jeden einzelnen Benutzer in getrennten Berichten separieren. Dementsprechend können wir jeden Kundenbericht mit derselben PBIX-Datei \(Benutzeroberfläche usw.) und unterschiedlichen Datenquellen bereitstellen.

> [!NOTE]
> Wenn Sie anstelle des **DirectQuery-Modus** den **Importmodus** verwenden, besteht keine Möglichkeit, die Modelle über die API zu aktualisieren. Zudem bietet Power BI Embedded noch keine Unterstützung für lokale Datenquellen über Power BI Gateway. Besuchen Sie regelmäßig den [Power BI-Blog](https://powerbi.microsoft.com/blog/) , um sich über Neuigkeiten und zukünftige Releases zu informieren.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Authentifizierung und Einbetten von Berichten in unsere Webseite

Bei der vorherigen REST-API können wir den Zugriffsschlüssel **AppKey** selbst als Autorisierungsheader verwenden. Da diese Aufrufe seitens des Back-End-Servers verarbeitet werden können, ist dies eine sichere Vorgehensweise.

Wenn wir allerdings den Bericht in unsere Webseite einbetten, wird diese Art von Sicherheitsinformationen über JavaScript \(Front-End) verarbeitet. In diesem Fall muss der Wert des Autorisierungsheaders gesichert werden. Falls unser Zugriffsschlüssel von einem böswilligen Benutzer oder einer Schadsoftware entdeckt wird, kann der Benutzer bzw. die Software mit diesem Schlüssel sämtliche Vorgänge aufrufen.

Beim Einbetten des Berichts in unsere Webseite müssen wir anstelle des **AppKey**-Zugriffsschlüssels das berechnete Token verwenden. Unsere Anwendung muss das OAuth Json Web Token \(JWT) erstellen, das aus den Ansprüchen und der berechneten digitalen Signatur besteht. Wie im Folgenden dargestellt, besteht dieses OAuth JWT aus codierten Zeichenfolgentoken, die durch Punkte getrennt sind.

![](media/power-bi-embedded-iframe/oauth-jwt.png)

Zunächst müssen wir den Eingabewert vorbereiten, der später signiert wird. Dieser Wert ist die Base64-URL-codierte (rfc4648) Zeichenfolge der folgenden JSON. Die Trennung erfolgt durch einen Punkt \(.). Später erläutern wir, wie Sie die Berichts-ID abrufen.

> [!NOTE]
> Um die Sicherheit auf Zeilenebene (Row Level Security, RLS) in Power BI Embedded zu verwenden, müssen wir zudem **Benutzername** und **Rollen** in den Ansprüchen festlegen.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Als Nächstes müssen wir die Base64-codierte HMAC-Zeichenfolge \(die Signatur) mit dem SHA256-Algorithmus erstellen. Bei diesem signierten Eingabewert handelt es sich um die vorherige Zeichenfolge.

Zuletzt müssen wir den Eingabewert und die Signaturzeichenfolge durch einen Punkt \(.) verbinden. Die vollständige Zeichenfolge ist das App-Token für die Einbettung des Berichts. Selbst wenn das App-Token von einem böswilligen Benutzer entdeckt wird, kann dieser nicht auf den ursprünglichen Zugriffsschlüssel zugreifen. Dieses App-Token läuft innerhalb kürzester Zeit ab.

Im Folgenden finden Sie ein PHP-Beispiel für die erläuterten Schritte:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Einbetten des Berichts in die Webseite

Damit wir unseren Bericht einbetten können, müssen wir mithilfe der folgenden REST-API die eingebettete URL und die Berichts- **ID** aufrufen.

**HTTP-Anforderung**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-Antwort**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

Wir können den Bericht mithilfe des vorherigen App-Tokens in unsere Web-App einbetten.
Wenn Sie den nächsten Beispielcode betrachten, fällt auf, dass der Anfang mit dem vorherigen Beispiel identisch ist. Im unteren Teil zeigt dieses Beispiel die **embedUrl** \(siehe vorheriges Ergebnis) im IFrame und stellt das App-Token im IFrame bereit.

> [!NOTE]
> Sie müssen den Wert der Berichts-ID in den Wert einer Ihrer eigenen Berichts-IDs ändern. Aufgrund eines Fehlers in unserem Content Management-System wird das IFrame-Tag im Codebeispiel wörtlich gelesen. Entfernen Sie den gekappten Text aus dem Tag, wenn Sie diesen Beispielcode kopieren und einfügen.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

Das ist unser Ergebnis:

![](media/power-bi-embedded-iframe/view-report.png)

Aktuell zeigt Power BI Embedded den Bericht nur im IFrame an. Besuchen Sie regelmäßig den [Power BI-Blog](https://powerbi.microsoft.com/blog/). Zukünftige Verbesserungen beinhalten möglicherweise neue clientseitige APIs, mit denen Sie Informationen an den IFrame senden und aus diesem abrufen können. Alles sehr spannend!

## <a name="see-also"></a>Siehe auch
* [Authentifizieren und Autorisieren in Power BI Embedded](power-bi-embedded-app-token-flow.md)

Weitere Fragen? [Power BI-Community ausprobieren](http://community.powerbi.com/)


