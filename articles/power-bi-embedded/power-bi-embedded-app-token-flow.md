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
ms.date: 01/06/2017
ms.author: asaxton
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 2b860b5815a0dd35138c685eb90490a8e2c53d5e


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
| **ver** |Die Version des App-Tokens. Die aktuelle Version ist 0.2.0. |
| **aud** |Der vorgesehene Empfänger des Tokens. Bei Power BI Embedded: https://analysis.windows.net/powerbi/api. |
| **iss** |Eine Zeichenfolge, die die Anwendung angibt, die das Token ausgestellt hat. |
| **type** |Die Art des App-Tokens, das erstellt wird. Aktuell wird nur der Typ **embed**unterstützt. |
| **wcn** |Der Name der Arbeitsbereichssammlung, für die das Token ausgegeben wird. |
| **wid** |Die ID der Arbeitsbereichssammlung, für die das Token ausgegeben wird. |
| **rid** |Die Berichts-ID der Arbeitsbereichssammlung, für die das Token ausgegeben wird. |
| **username** (optional) |In Kombination mit RLS ermöglicht diese Zeichenfolge die Identifizierung des Benutzers, wenn RLS-Regeln angewendet werden. |
| **roles** (optional) |Mit den in dieser Zeichenfolge enthaltenen Rollen wird ausgewählt, wann die Regeln der Sicherheit auf Zeilenebene (Row Level Security, RLS) angewendet werden sollen. Mehrere Rollen sollten als Zeichenfolgenarray übergeben werden. |
| **exp** (optional) |Gibt die Ablaufzeit für das Token an. Dieser Wert muss als Unix-Zeitstempel übergeben werden. |
| **nbf** (optional) |Gibt an, ab wann das Token gültig wird. Dieser Wert muss als Unix-Zeitstempel übergeben werden. |

Ein App-Token kann beispielsweise wie folgt aussehen:

![](media/power-bi-embedded-app-token-flow/power-bi-embedded-app-token-flow-sample-coded.png)

Decodiert sieht es dann wie folgt aus:

![](media/power-bi-embedded-app-token-flow/power-bi-embedded-app-token-flow-sample-decoded.png)

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
* [Erste Schritte mit dem Beispiel zu Microsoft Power BI Embedded](power-bi-embedded-get-started-sample.md)
* [Häufige Microsoft Power BI Embedded-Szenarios](power-bi-embedded-scenarios.md)
* [Erste Schritte mit Microsoft Power BI Embedded](power-bi-embedded-get-started.md)




<!--HONumber=Nov16_HO3-->


