---
title: SendGrid-Bindungen in Azure Functions | Microsoft Docs
description: "Referenz für SendGrid-Bindungen in Azure Functions"
services: functions
documentationcenter: na
author: rachelappel
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/26/2017
ms.author: rachelap
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: bcdbb6aee49d230a4cb0ba08d836facacb64de7f
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="azure-functions-sendgrid-bindings"></a>SendGrid-Bindungen in Azure Functions

Dieser Artikel erläutert die Konfiguration und Verwendung von SendGrid-Bindungen in Azure Functions. Mit SendGrid können Sie über Azure Functions angepasste E-Mails programmgesteuert senden.

Dieser Artikel umfasst Referenzinformationen für Azure Functions-Entwickler. Falls Sie mit Azure Functions noch nicht vertraut sind, beginnen Sie mit den folgenden Ressourcen:

[Erstellen Sie Ihre erste Azure-Funktion](functions-create-first-azure-function.md). 
Entwicklerreferenz zu [C#](functions-reference-csharp.md), [F#](functions-reference-fsharp.md) oder [Node](functions-reference-node.md).

## <a name="functionjson-for-sendgrid-bindings"></a>function.json für SendGrid-Bindungen

Azure Functions umfasst eine Ausgabebindung für SendGrid. Mithilfe der SendGrid-Ausgabebindung können Sie E-Mails programmgesteuert erstellen und senden. 

Die SendGrid-Bindung unterstützt die folgenden Eigenschaften:

|Eigenschaft  |Beschreibung  |
|---------|---------|
|**name**| Erforderlich – der Variablenname, der im Funktionscode für die Anforderung oder den Anforderungstext verwendet wird. Dieser Wert ist ```$return```, wenn es nur einen Rückgabewert gibt. |
|**type**| Erforderlich – muss auf `sendGrid` festgelegt sein.|
|**direction**| Erforderlich – muss auf `out` festgelegt sein.|
|**apiKey**| Erforderlich – muss auf den Namen des API-Schlüssels festgelegt sein, der in den App-Einstellungen der Funktionen-App gespeichert ist. |
|**to**| E-Mail-Adresse des Empfängers |
|**from**| E-Mail-Adresse des Absenders |
|**subject**| Betreff der E-Mail |
|**text**| Inhalt der E-Mail |

Beispiel für **function.json**:

```json 
{
    "bindings": [
        {
            "name": "$return",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

> [!NOTE]
> Azure Functions speichert die Verbindungsinformationen und API-Schlüssel als App-Einstellungen, sodass sie nicht ins Repository der Quellcodeverwaltung eingecheckt werden. Mit dieser Aktion werden vertrauliche Daten geschützt.
>
>

## <a name="c-example-of-the-sendgrid-output-binding"></a>C#-Beispiel für die SendGrid-Ausgabebindung

```csharp
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static Mail Run(TraceWriter log, string input, out Mail message)
{
     message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    personalization.AddTo(new Email("recipient@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

## <a name="node-example-of-the-sendgrid-output-binding"></a>Node-Beispiel für die SendGrid-Ausgabebindung

```javascript
module.exports = function (context, input) {    
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: "sender@contoso.com",        
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};

```

## <a name="next-steps"></a>Nächste Schritte
Informationen zu anderen Bindungen und Triggern für Azure Functions finden Sie unter 
- [Entwicklerreferenz zu Triggern und Bindungen in Azure Functions](functions-triggers-bindings.md)

- [Bewährte Methoden für Azure Functions](functions-best-practices.md) enthält eine Auflistung bewährter Methoden zur Verwendung beim Erstellen von Azure Functions.

- [Azure Functions: Entwicklerhandbuch](functions-reference.md) ist eine Programmiererreferenz zum Codieren von Funktionen sowie Definieren von Triggern und Bindungen.

