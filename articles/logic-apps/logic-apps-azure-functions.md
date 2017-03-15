---
title: "Hinzufügen von benutzerdefiniertem Code zu Azure Logic Apps mit Azure Functions | Microsoft-Dokumentation"
description: "Erstellen von benutzerdefiniertem Code für Azure Logic Apps mit Azure Functions"
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: 7d21ab1180fcd6df39a5dcc5c095c9521c00f6fd
ms.lasthandoff: 03/10/2017

---

# <a name="add-and-run-custom-code-for-logic-apps-through-azure-functions"></a>Hinzufügen von benutzerdefiniertem Code für Logik-Apps über Azure Functions

Sie können über Azure Functions benutzerdefinierte Funktionen erstellen, um benutzerdefinierte C#- oder Node.js-Codeausschnitte auszuführen. 
[Azure Functions](../azure-functions/functions-overview.md) ermöglicht Computing ohne Server in Microsoft Azure und ist für die folgenden Aufgaben nützlich:

* Erweiterte Formatierung oder Berechnung von Feldern in Logik-Apps
* Berechnungen in einem Workflow
* Erweiterung der Funktionalität von Logik-Apps um Funktionen, die in C# oder Node.js unterstützt werden

## <a name="create-custom-functions-for-your-logic-apps"></a>Erstellen von benutzerdefinierten Funktionen für Ihre Logik-Apps

Es wird empfohlen, eine Funktion im Azure Functions-Portal aus den Vorlagen für **generische Knotenwebhooks** oder **generische C#-Webhooks** zu erstellen. Das Ergebnis ist eine automatisch ausgefüllte Vorlage, die `application/json` von einer Logik-App akzeptiert. Funktionen, die Sie mit diesen Vorlagen erstellen, werden automatisch ermittelt und im Logik-Apps-Designer unter **Azure Functions in my region** (Azure Functions in meiner Region) angezeigt.

Im Azure-Portal sollte für die Funktion im Bereich **Integrieren** für die Vorlage **Modus** auf **Webhook** und **Webhooktyp** auf **Generic JSON** (Generisches JSON-Format) festgelegt sein. 

Webhookfunktionen akzeptieren eine Anforderung und übergeben sie über eine `data`-Variable an die Methode. Sie können auf die Eigenschaften Ihrer Nutzlast mit der Punktnotation zugreifen, z.B. `data.function-name`. Beispielsweise sieht eine einfache JavaScript-Funktion, die einen DateTime-Wert in eine Datumszeichenfolge konvertiert, wie im folgenden Beispiel aus:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-logic-apps"></a>Aufrufen von Azure Functions aus Logik-Apps

Um die Container in Ihrem Abonnement aufzulisten und die Funktion auszuwählen, die Sie aufrufen möchten, klicken Sie im Logik-App-Designer auf das Menü **Aktionen**, und treffen Sie Ihre Auswahl unter **Azure Functions in my Region** (Azure-Funktionen in meiner Region).

Nachdem Sie die Funktion ausgewählt haben, werden Sie aufgefordert, ein Eingabenutzlastobjekt anzugeben. Dieses Objekt ist die Nachricht, die von der Logik-App an die Funktion gesendet wird. Es muss ein JSON-Objekt sein. Wenn Sie beispielsweise das Datum der letzten Änderung**** von einem Salesforce-Trigger übergeben möchten, kann die Funktionsnutzlast unter Umständen wie im folgenden Beispiel aussehen:

![Datum der letzten Änderung][1]

## <a name="trigger-logic-apps-from-a-function"></a>Auslösen von Logik-Apps aus einer Funktion

Sie können eine Logik-App aus einer Funktion heraus auslösen. Weitere Informationen finden Sie unter [Logic-Apps als aufrufbare Endpunkte](logic-apps-http-endpoint.md). Erstellen Sie eine Logik-App, die einen manuellen Trigger aufweist, und generieren Sie in der Funktion einen HTTP POST-Aufruf, um die URL mit der Nutzlast, die Sie an die Logik-App senden möchten, manuell auszulösen.

### <a name="create-a-function-from-logic-app-designer"></a>Erstellen einer Funktion im Logik-App-Designer

Sie können auch eine Node.js-Webhookfunktion direkt im Designer erstellen. Wählen Sie zunächst **Azure Functions in my region** (Azure Functions in meiner Region) und dann einen Container für Ihre Funktion aus. Wenn noch kein Container vorhanden ist, müssen Sie ihn im [Azure Functions-Portal](https://functions.azure.com/signin)erstellen. Klicken Sie anschließend auf **Neu erstellen**.  

Wenn Sie eine Vorlage auf Grundlage der zu berechnenden Daten generieren möchten, geben Sie das Kontextobjekt an, das Sie an eine Funktion übergeben möchten. Dieses Objekt muss ein JSON-Objekt sein. Wenn Sie den Dateiinhalt beispielsweise aus einer FTP-Aktion übergeben, sieht die Kontextnutzlast wie im folgenden Beispiel aus:

![Kontextnutzlast][2]

> [!NOTE]
> Da dieses Objekt nicht als Zeichenfolge umgewandelt wurde, wird der Inhalt der JSON-Nutzlast direkt hinzugefügt. Dies führt aber zu einem Fehler, wenn das Objekt kein JSON-Token ist (also eine Zeichenfolge oder ein JSON-Objekt/-Array). Fügen Sie zum Umwandeln des Objekts als Zeichenfolge einfach Anführungszeichen hinzu, wie dies in der ersten Abbildung in diesem Artikel gezeigt ist.
> 

Der Designer generiert dann eine Funktionsvorlage, die Sie inline erstellen können. Variablen werden basierend auf dem Kontext, in dem Sie die Funktion übergeben möchten, vorab erstellt.

<!--Image references-->
[1]: ./media/logic-apps-azure-functions/callfunction.png
[2]: ./media/logic-apps-azure-functions/createfunction.png

