<properties
   pageTitle="Verwenden von Azure Functions mit Logik-Apps | Microsoft Azure"
   description="Erfahren Sie, wie Sie Azure Functions mit Logik-Apps verwenden."
   services="app-service\logic,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/14/2016"
   ms.author="jehollan"/>
   
# Verwenden von Azure Functions mit Logik-Apps

Sie können benutzerdefinierte Ausschnitte in C# oder Node.js ausführen, indem Sie Azure Functions in einer Logik-App verwenden. [Azure Functions](../azure-functions/functions-overview.md) ist ein Angebot, das serverlose Berechnungen in Microsoft Azure ermöglicht. Dies ist in Logik-Apps für viele der folgenden Szenarios hilfreich:

* Formatieren eines Werts aus einer Aktion (z.B. Konvertieren eines DateTime-Werts in eine Datumszeichenfolge)
* Durchführen von Berechnungen innerhalb eines Workflows
* Erweitern der Funktionalität von Logik-Apps mit Funktionen, die in C# oder Node.js unterstützt werden

## Erstellen einer Azure-Funktion für Logik-Apps

Es wird empfohlen, dass Sie eine neue Azure-Funktion im Functions-Portal mithilfe der Vorlagen für generische Knotenwebhooks oder generische C#-Webhooks erstellen. Dabei wird eine Vorlage automatisch aufgefüllt, wenn sie `application/json` von einer Logik-App akzeptiert. Funktionen, die diese Vorlagen verwenden, werden automatisch erkannt und im Logik-Apps-Designer unter „Azure Functions in my region“ (Azure Functions in meiner Region) aufgeführt.

Webhookfunktionen akzeptieren eine Anforderung und übergeben sie über eine `data`-Variable an die Methode. Sie können auf die Eigenschaften Ihrer Nutzlast mit der Punktnotation wie z.B. `data.foo` zugreifen. Beispielsweise sieht eine einfache Javascript-Funktion, die einen DateTime-Wert in eine Datumszeichenfolge konvertiert, wie folgt aus:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## Aufrufen von Azure Functions aus einer Logik-App

Wenn Sie im Designer auf das Dropdownmenü für Aktionen klicken, können Sie „Azure Functions in my Region“ (Azure Functions in meiner Region) auswählen Damit listen Sie die Container in Ihrem Abonnement auf. Gleichzeitig haben Sie die Möglichkeit, die Funktion auszuwählen, die Sie aufrufen möchten. Nach der Auswahl werden Sie aufgefordert, ein Eingabenutzlastobjekt anzugeben. Dies ist die Nachricht, die die Logik-App an die Funktion sendet. Sie muss ein JSON-Objekt sein. Wenn Sie z.B. das Datum der letzten Änderung von einem Salesforce-Trigger übergeben möchten, könnte die Funktionsnutzlast wie folgt aussehen:

![][1]

## Auslösen von Logik-Apps aus einer Azure-Funktion

Es ist auch möglich, eine Logik-App in einer Funktion heraus auszulösen. Erstellen Sie dazu einfach eine Logik-App mit einem manuellen Trigger (Details finden Sie [hier](app-service-logic-http-endpoint.md)). Erstellen Sie dann in Ihrer Azure-Funktion einen HTTP POST-Aufruf, um die URL mit der Nutzlast, die Sie an die Logik-App senden möchten, manuell auszulösen.

### Erstellen von Funktionen im Designer

Sie können auch eine node.js-Webhookfunktion direkt im Designer erstellen. Wählen Sie zunächst „Azure Functions in my Region“ (Azure Functions in meiner Region) und dann einen Container für Ihre Funktion aus. Wenn Sie noch über keinen Container verfügen, müssen ihn im [Azure Functions-Portal](https://functions.azure.com/signin) erstellen. Sie können dann „Neu erstellen“ auswählen. Wenn Sie eine Vorlage auf Grundlage der zu berechnenden Daten generieren möchten, geben Sie das Kontextobjekt an, das Sie an eine Funktion übergeben möchten. Dies muss ein JSON-Objekt sein. Wenn Sie z.B. den Dateiinhalt einer FTP-Aktion übergeben, würde der Nutzlastkontext wie folgt aussehen:

![][2]

>[AZURE.NOTE] Da dieses Objekt nicht durch Hinzufügen von Anführungszeichen in eine Zeichenfolge umgewandelt wurde, wird der Inhalt direkt der JSON-Nutzlast hinzugefügt. Dies führt zu einem Fehler, wenn es sich nicht um ein JSON-Token (d.h. eine Zeichenfolge oder ein JSON-Objekt/-Array) handelt. Für die Umwandlung in eine Zeichenfolge fügen Sie einfach Anführungszeichen hinzu, wie im Salesforce-Beispiel oben.

Der Designer erstellt dann eine Funktionsvorlage, die Sie inline erstellen können. Variablen werden basierend auf dem Kontext, in dem Sie die Funktion übergeben möchten, vorab erstellt.

<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png

<!---HONumber=AcomDC_0525_2016-->