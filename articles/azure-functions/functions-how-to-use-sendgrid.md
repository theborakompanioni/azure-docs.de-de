---
title: Verwenden von SendGrid in Azure Functions | Microsoft-Dokumentation
description: Zeigt die Verwendung von SendGrid in Azure Functions.
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/31/2017
ms.author: rachelap
translationtype: Human Translation
ms.sourcegitcommit: b95fcfa1ed4ea4cf1051f4920b9c05bb40a19cab
ms.openlocfilehash: 05c9f4e4a4351219da68af8b702c25f21d7d4d02
ms.lasthandoff: 02/22/2017


---
# <a name="how-to-use-sendgrid-in-azure-functions"></a>Verwenden von SendGrid in Azure Functions

## <a name="sendgrid-overview"></a>Übersicht zu SendGrid

Azure Functions unterstützt SendGrid-Ausgabebindungen, mit denen Sie in Ihren Funktionen das Senden von E-Mail-Nachrichten mit wenigen Codezeilen und einem SendGrid-Konto aktivieren können.

Um die SendGrid-API in einer Azure-Funktion verwenden zu können, benötigen Sie ein [SendGrid-Konto](http://SendGrid.com). Darüber hinaus müssen Sie über einen SendGrid-API-Schlüssel verfügen. Melden Sie sich mit Ihrem SendGrid-Konto an, klicken Sie auf **Einstellungen** und dann auf **API-Schlüssel**, um einen API-Schlüssel zu generieren. Halten Sie diesen Schlüssel bereit, da Sie ihn in einem zukünftigen Schritt verwenden müssen.

Sie können nun eine Azure Function-App erstellen.

## <a name="create-an-azure-function-app"></a>Erstellen einer Azure Function-App 

Azure Function-Apps sind Container für eine oder mehrere Azure-Funktionen. Azure-Funktionen sind genau das, was der Name sagt – Funktionen. Jede Azure-Funktion ist an einen Trigger gebunden, also ein Ereignis, das die Ausführung der Funktion auslöst.
Jede Funktion kann eine beliebige Anzahl von Eingabe- oder Ausgabebindungen enthalten. Bindungen sind Dienste, die Sie in einer Funktion verwenden können. SendGrid ist eine Ausgabebindung, die Sie zum Senden von E-Mails verwenden können. 

1. Melden Sie sich beim Azure-Portal an und [erstellen Sie eine Azure Function-App](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function), oder öffnen Sie eine vorhandene Function-App. 
2. Erstellen Sie eine Azure-Funktion. Wählen Sie der Einfachheit halber einen manuellen Trigger und C#. 

 ![Erstellen einer Azure-Funktion](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a>Konfigurieren von SendGrid für die Verwendung in einer Azure Function-App

Sie müssen Ihren SendGrid-API-Schlüssel für die Verwendung in einer Funktion als App-Einstellung speichern. Das Feld ApiKey ist nicht der tatsächliche SendGrid-API-Schlüssel, jedoch eine von Ihnen definierte App-Einstellung, die Ihren tatsächlichen API-Schlüssel darstellt. Wenn Sie Ihren Schlüssel auf diese Weise speichern, dient dies der Sicherheit, da er so getrennt ist von beliebigem Code oder Dateien, die der Quellcodeverwaltung unterzogen werden könnten.

- Erstellen Sie einen **AppSettings**-Schlüssel in den **Anwendungseinstellungen** Ihrer Funktionen-App.

 ![Erstellen einer Azure-Funktion](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a>Konfigurieren von SendGrid-Ausgabebindungen

SendGrid ist als Azure-Funktionsausgabebindung verfügbar. So erstellen Sie eine SendGrid-Ausgabebindung:

1. Wechseln Sie im Azure-Portal zu der Registerkarte **Integrieren** der Funktion.
2. Klicken Sie auf **Neue Ausgabe**, um eine SendGrid-Ausgabebindung zu erstellen.
3. Füllen Sie die Eigenschaften **API-Schlüssel** und **Nachrichtenparametername** aus. Wenn Sie möchten, können Sie jetzt die anderen Eigenschaften eingeben, oder sie stattdessen codieren. Diese Einstellungen können als Standardwerte verwendet werden.

 ![Konfigurieren von SendGrid-Ausgabebindungen](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

Wenn Sie einer Funktion eine Bindung hinzufügen, wird eine Datei namens **function.json** im Ordner Ihrer Funktion erstellt. Diese Datei enthält die gleichen Informationen, die Sie in der Registerkarte **Integrieren** der Azure-Funktion sehen, jedoch im JSON-Format. Durch Festlegen der Felder **ApiKey**, **Nachrichtentext** und **Absenderadresse** erstellen Sie die folgenden Einträge in der Datei **function.json**: 

```json
{
  "bindings": [    
    {
      "type": "sendGrid",
      "name": "message",
      "apiKey": "SendGridKey",
      "direction": "out",
      "from": "azure@contoso.com"
    }
  ],
  "disabled": false
}
```

Falls gewünscht, ändern Sie diese Datei direkt selbst.

Jetzt haben Sie Funktionen-App und Funktion erstellt und konfiguriert und können den Code zum Senden einer E-Mail schreiben.

## <a name="write-code-that-creates-and-sends-email"></a>Schreiben von Code, der eine E-Mail erstellt und sendet

Die SendGrid-API enthält alle Befehle, die Sie zum Erstellen und Senden einer E-Mail benötigen.  

- Ersetzen Sie den Code in der Funktion durch den folgenden Code:

```cs
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static void Run(TraceWriter log, string input, out Mail message)
{
    message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    // change to email of recipient
    personalization.AddTo(new Email("MoreEmailPlease@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

Beachten Sie, dass die erste Zeile die ```#r```-Richtlinie enthält, die auf die SendGrid-Assembly verweist. Danach können Sie eine ```using```-Anweisung verwenden, um einfacher auf die Objekte in diesem Namespace zuzugreifen. Erstellen Sie im Code Instanzen von ```Mail```-, ```Personalization```- und ```Content```-Objekten aus der SendGrid-API, die die E-Mail erstellen. Wenn Sie die Nachricht zurückgeben, wird sie von SendGrid übermittelt. 

Die Signatur der Funktion enthält auch einen zusätzlichen out-Parameter des Typs ```Mail``` mit dem Namen ```message```. Sowohl Eingabe- als auch Ausgabebindungen werden als Funktionsparameter im Code ausgedrückt. 

2. Testen Sie den Code, indem Sie auf **Test** klicken, eine Nachricht in das Feld **Anforderungstext** eingeben und dann auf die Schaltfläche **Ausführen** klicken.

 ![Testen Ihres Codes](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. Überprüfen Sie, ob SendGrid die E-Mail gesendet hat. Sie sollte an die Adresse im Code aus Schritt 1 gesendet werden und die Nachricht aus dem **Anforderungstext** enthalten.

## <a name="next-steps"></a>Nächste Schritte
Dieser Artikel hat veranschaulicht, wie mit dem SendGrid-Dienst E-Mail erstellt und gesendet wird. Weitere Informationen zur Verwendung von Azure Functions in Apps finden Sie unter den folgenden Themen: 

- [Bewährte Methoden für Azure Functions](functions-best-practices.md) enthält eine Auflistung bewährter Methoden zur Verwendung beim Erstellen von Azure Functions.

- [Azure Functions: Entwicklerhandbuch](functions-reference.md) ist eine Programmiererreferenz zum Codieren von Funktionen sowie Definieren von Triggern und Bindungen.

- [Testen von Azure Functions](functions-test-a-function.md) beschreibt verschiedene Tools und Techniken zum Testen Ihrer Funktionen.
