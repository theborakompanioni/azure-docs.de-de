---
title: Authentifizieren und Autorisieren mit Power BI Embedded
description: Authentifizieren und Autorisieren mit Power BI Embedded
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 1c1369ea-7dfd-4b6e-978b-8f78908fd6f6
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 93027f0f5467e0b21c47bbcbc84c67cdd50b26b8
ms.lasthandoff: 03/14/2017


---
# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Authentifizieren und Autorisieren mit Power BI Embedded

Der Power BI Embedded-Dienst nutzt anstelle der expliziten Endbenutzerauthentifizierung **Schlüssel** und **App-Token** zur Authentifizierung und Autorisierung. Bei diesem Modell verwaltet Ihre Anwendung die Authentifizierung und Autorisierung für Ihre Endbenutzer. Bei Bedarf erstellt und sendet Ihre App die App-Token, die unseren Dienst anweisen, den angeforderten Bericht zu rendern. Bei diesem Konzept muss Ihre App nicht Azure Active Directory für die Benutzerauthentifizierung und -autorisierung verwenden, auch wenn dies natürlich möglich ist.

## <a name="two-ways-to-authenticate"></a>Zwei Authentifizierungsmöglichkeiten

**Schlüssel**: Schlüssel können für alle Power BI Embedded-REST-API-Aufrufe verwendet werden. Sie finden die Schlüssel im **Azure-Portal**, indem Sie auf **Alle Einstellungen** und dann auf **Zugriffsschlüssel** klicken. Behandeln Sie Ihren Schlüssel immer wie ein Kennwort. Die Schlüssel verfügen über Berechtigungen, die es ihnen ermöglichen, beliebige REST-API-Aufrufe für eine bestimmte Arbeitsbereichssammlung durchzuführen.

Wenn Sie einen Schlüssel für einen REST-Aufruf verwenden möchten, fügen Sie den folgenden Autorisierungsheader hinzu:            

    Authorization: AppKey {your key}

**App-Token** : App-Token werden für alle Einbettungsanforderungen verwendet. Da Sie für die clientseitige Ausführung vorgesehen sind, sind sie auf einen einzelnen Bericht beschränkt, und es empfiehlt sich, eine Ablaufzeit festzulegen.

App-Token sind JWT (JSON Web Token), die mit einem Ihrer Schlüssel signiert sind.

Ihr App-Token kann folgende Ansprüche enthalten:

| Anspruch | Beschreibung |
| --- | --- |
| **ver** |Die Version des App-Tokens. Die aktuelle Version ist&0;.2.0. |
| **aud** |Der vorgesehene Empfänger des Tokens. Bei Power BI Embedded: https://analysis.windows.net/powerbi/api. |
| **iss** |Eine Zeichenfolge, die die Anwendung angibt, die das Token ausgestellt hat. |
| **type** |Die Art des App-Tokens, das erstellt wird. Aktuell wird nur der Typ **embed**unterstützt. |
| **wcn** |Der Name der Arbeitsbereichssammlung, für die das Token ausgegeben wird. |
| **wid** |Die ID der Arbeitsbereichssammlung, für die das Token ausgegeben wird. |
| **rid** |Die Berichts-ID der Arbeitsbereichssammlung, für die das Token ausgegeben wird. |
| **username** (optional) |In Kombination mit RLS ermöglicht diese Zeichenfolge die Identifizierung des Benutzers, wenn RLS-Regeln angewendet werden. |
| **roles** (optional) |Mit den in dieser Zeichenfolge enthaltenen Rollen wird ausgewählt, wann die Regeln der Sicherheit auf Zeilenebene (Row Level Security, RLS) angewendet werden sollen. Mehrere Rollen sollten als Zeichenfolgenarray übergeben werden. |
| **scp** (optional) |Eine Zeichenfolge, die die Berechtigungsbereiche enthält. Mehrere Rollen sollten als Zeichenfolgenarray übergeben werden. |
| **exp** (optional) |Gibt die Ablaufzeit für das Token an. Dieser Wert muss als Unix-Zeitstempel übergeben werden. |
| **nbf** (optional) |Gibt an, ab wann das Token gültig wird. Dieser Wert muss als Unix-Zeitstempel übergeben werden. |

Ein App-Token kann beispielsweise wie folgt aussehen:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXIiOiIwLjIuMCIsInR5cGUiOiJlbWJlZCIsIndjbiI6Ikd1eUluQUN1YmUiLCJ3aWQiOiJkNGZlMWViMS0yNzEwLTRhNDctODQ3Yy0xNzZhOTU0NWRhZDgiLCJyaWQiOiIyNWMwZDQwYi1kZTY1LTQxZDItOTMyYy0wZjE2ODc2ZTNiOWQiLCJzY3AiOiJSZXBvcnQuUmVhZCIsImlzcyI6IlBvd2VyQklTREsiLCJhdWQiOiJodHRwczovL2FuYWx5c2lzLndpbmRvd3MubmV0L3Bvd2VyYmkvYXBpIiwiZXhwIjoxNDg4NTAyNDM2LCJuYmYiOjE0ODg0OTg4MzZ9.v1znUaXMrD1AdMz6YjywhJQGY7MWjdCR3SmUSwWwIiI
```

Decodiert sieht es dann etwa wie folgt aus:

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

Innerhalb der SDKs stehen Methoden zur Verfügung, die die Erstellung von App-Token vereinfachen. Sehen Sie sich beispielsweise für .NET die Klasse [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) und die Methoden [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) an.

Informationen zum .NET SDK finden Sie unter [Scopes](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes) (Bereiche).

## <a name="scopes"></a>Bereiche

Bei der Verwendung von Einbettungstoken möchten Sie vielleicht die Nutzung der Ressourcen beschränken, auf die Sie Zugriff gewähren. Aus diesem Grund können Sie ein Token mit bereichsbezogenen Berechtigungen generieren.

Im Folgenden sehen Sie die verfügbaren Bereiche für Power BI Embedded.

|Umfang|Beschreibung|
|---|---|
|Dataset.Read|Stellt die Berechtigung zum Lesen des angegebenen Datasets bereit.|
|Dataset.Write|Stellt die Berechtigung zum Schreiben in das angegebene Dataset bereit.|
|Dataset.ReadWrite|Stellt die Berechtigung zum Lesen des angegebenen Datasets sowie zum Schreiben bereit.|
|Report.Read|Stellt die Berechtigung zum Anzeigen des angegebenen Berichts bereit.|
|Report.ReadWrite|Stellt die Berechtigung zum Anzeigen und Bearbeiten des angegebenen Berichts bereit.|
|Workspace.Report.Create|Stellt die Berechtigung zum Erstellen eines neuen Berichts innerhalb des angegebenen Arbeitsbereichs bereit.|
|Workspace.Report.Copy|Stellt die Berechtigung zum Klonen eines vorhandenen Berichts innerhalb des angegebenen Arbeitsbereichs bereit.|

Sie können mehrere Bereiche angeben, indem Sie zwischen den Bereichen ein Leerzeichen einfügen (s.u.).

```
string scopes = "Dataset.Read Workspace.Report.Create";
```

**Erforderliche Ansprüche – Bereiche**

scp: {scopesClaim} scopesClaim kann entweder eine Zeichenfolge oder ein Array von Zeichenfolgen sein, worin die zulässigen Berechtigungen für die Arbeitsbereichsressourcen (Bericht, Dataset usw.) erfasst werden.

Ein decodiertes Token mit definierten Bereichen sieht etwa wie folgt aus.

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "scp": "Report.Read",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

### <a name="operations-and-scopes"></a>Vorgänge und Bereiche

|Vorgang|Zielressource|Tokenberechtigungen|
|---|---|---|
|Erstellen Sie (In-Memory) einen neuen Bericht basierend auf einem Dataset.|Datensatz|Dataset.Read|
|Erstellen Sie (im Speicher) einen neuen Bericht basierend auf einem Dataset, und speichern Sie den Bericht.|Datensatz|*Dataset.Read<br>* Workspace.Report.Create|
|Zeigen Sie einen vorhandenen Bericht an (im Speicher), und untersuchen/bearbeiten Sie ihn. Report.Read impliziert Dataset.Read. Dies umfasst keine Berechtigungen zum Speichern von Änderungen.|Bericht|Report.Read|
|Bearbeiten und speichern Sie einen vorhandenen Bericht.|Bericht|Report.ReadWrite|
|Speichern Sie eine Kopie eines Berichts (Speichern unter).|Bericht|*Report.Read<br>* Workspace.Report.Copy|

## <a name="heres-how-the-flow-works"></a>Informationen zum Ablauf
1. Kopieren der API-Schlüssel zu Ihrer Anwendung Sie finden die Schlüssel im **Azure-Portal**.
   
    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
2. Das Token setzt einen Anspruch durch und hat eine Ablaufzeit.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-2.png)
3. Das Token wird mit einer API-Tastenkombination angemeldet.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-3.png)
4. Der Benutzer fordert die Ansicht des Berichts an.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-4.png)
5. Das Token wird mit einer API-Tastenkombination validiert.
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-5.png)
6. Power BI Embedded sendet einen Bericht an den Benutzer.
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-6.png)

Nachdem **Power BI Embedded** einen Bericht an den Benutzer geschickt hat, kann dieser den Bericht in Ihrer benutzerdefinierten App anzeigen. Wenn Sie also etwa das [Beispiel „Analyzing Sales Data PBIX“](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix)importiert haben, sieht die Beispiel-Web-App wie folgt aus:

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="see-also"></a>Weitere Informationen

[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Erste Schritte mit dem Beispiel zu Microsoft Power BI Embedded](power-bi-embedded-get-started-sample.md)  
[Häufige Microsoft Power BI Embedded-Szenarios](power-bi-embedded-scenarios.md)  
[Erste Schritte mit Microsoft Power BI Embedded](power-bi-embedded-get-started.md)  
[Git-Repository für PowerBI-CSharp](https://github.com/Microsoft/PowerBI-CSharp)  
Weitere Fragen? [Power BI-Community ausprobieren](http://community.powerbi.com/)


