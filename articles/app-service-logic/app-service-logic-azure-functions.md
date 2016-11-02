<properties
   pageTitle="Verwenden von Azure Functions mit Logik-Apps | Microsoft Azure"
   description="Erfahren Sie, wie Sie Azure Functions mit Logik-Apps verwenden."
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="jehollan"/>

# Verwenden von Azure Functions mit Logik-Apps

Sie können benutzerdefinierte Codeausschnitte von C# oder node.js ausführen, indem Sie Azure Functions aus einer Logik-App ausführen. [Azure Functions](../azure-functions/functions-overview.md) ermöglicht Computing ohne Server in Microsoft Azure. Dies ist für die Durchführung der folgenden Aufgaben hilfreich:

* Formatieren des Werts einer Aktion (z.B. Konvertierung von DateTime in eine Datumszeichenfolge)
* Durchführen von Berechnungen innerhalb eines Workflows
* Erweitern der Funktionalität von Logik-Apps um Funktionen, die in C# oder node.js unterstützt werden

## Erstellen einer Funktion für Logik-Apps

Es wird empfohlen, eine neue Funktion im Azure Functions-Portal zu erstellen, indem Sie die Vorlage für **generische Knotenwebhooks** oder **generische C#-Webhooks** verwenden. Hierbei wird eine Vorlage automatisch aufgefüllt, die `application/json` von einer Logik-App akzeptiert. Funktionen, für die diese Vorlagen verwendet werden, werden automatisch ermittelt und im Logik-Apps-Designer unter **Azure Functions in my region** (Azure Functions in meiner Region) aufgeführt.

Webhookfunktionen akzeptieren eine Anforderung und übergeben sie über eine `data`-Variable an die Methode. Sie können auf die Eigenschaften Ihrer Nutzlast mit der Punktnotation zugreifen, z.B. `data.foo`. Beispielsweise sieht eine einfache JavaScript-Funktion, die einen DateTime-Wert in eine Datumszeichenfolge konvertiert, wie im folgenden Beispiel aus:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## Aufrufen von Azure Functions aus einer Logik-App

Wenn Sie im Designer auf das Menü **Aktionen** klicken, können Sie **Azure Functions in my region** (Azure Functions in meiner Region) auswählen. Die Container in Ihrem Abonnement werden aufgeführt, und Sie können die Funktion auswählen, die Sie aufrufen möchten.

Nachdem Sie die Funktion ausgewählt haben, werden Sie aufgefordert, ein Eingabenutzlastobjekt anzugeben. Dies ist die Nachricht, die von der Logik-App an die Funktion gesendet wird. Es muss ein JSON-Objekt sein. Wenn Sie beispielsweise das Datum der letzten Änderung von einem Salesforce-Trigger übergeben möchten, kann die Funktionsnutzlast unter Umständen wie folgt aussehen:

![Datum der letzten Änderung][1]

## Auslösen von Logik-Apps aus einer Funktion

Es ist auch möglich, eine Logik-App aus einer Funktion heraus auszulösen. Erstellen Sie dazu einfach eine Logik-App mit einem manuellen Trigger. Weitere Informationen finden Sie unter [Logik-Apps als aufrufbare Endpunkte](app-service-logic-http-endpoint.md). Erstellen Sie dann in Ihrer Funktion einen HTTP POST-Aufruf, um die URL mit der Nutzlast, die Sie an die Logik-App senden möchten, manuell auszulösen.

### Erstellen von Funktionen im Designer

Sie können auch eine node.js-Webhookfunktion direkt im Designer erstellen. Wählen Sie zunächst **Azure Functions in my region** (Azure Functions in meiner Region) und dann einen Container für Ihre Funktion aus. Wenn noch kein Container vorhanden ist, müssen Sie ihn im [Azure Functions-Portal](https://functions.azure.com/signin) erstellen. Klicken Sie anschließend auf **Neu erstellen**.

Wenn Sie eine Vorlage auf Grundlage der zu berechnenden Daten generieren möchten, geben Sie das Kontextobjekt an, das Sie an eine Funktion übergeben möchten. Dies muss ein JSON-Objekt sein. Wenn Sie den Dateiinhalt beispielsweise aus einer FTP-Aktion übergeben, sieht die Kontextnutzlast wie folgt aus:

![Kontextnutzlast][2]

>[AZURE.NOTE] Da dieses Objekt nicht als Zeichenfolge umgewandelt wurde, wird der Inhalt der JSON-Nutzlast direkt hinzugefügt. Dies führt aber zu einem Fehler, wenn es sich nicht um ein JSON-Token handelt (also eine Zeichenfolge oder ein JSON-Objekt/-Array). Fügen Sie zum Umwandeln als Zeichenfolge einfach Anführungszeichen hinzu, wie dies in der ersten Abbildung in diesem Artikel gezeigt ist.

Der Designer generiert dann eine Funktionsvorlage, die Sie inline erstellen können. Variablen werden basierend auf dem Kontext, in dem Sie die Funktion übergeben möchten, vorab erstellt.




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png

<!---HONumber=AcomDC_0907_2016-->