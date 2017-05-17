---
title: "Erstellen eines serverlosen Dashboards für soziale Medien in Azure | Microsoft-Dokumentation"
description: "Erstellen eines serverlosen Dashboards für soziale Medien in Azure"
services: functions, logic-apps, cognitive-services
keywords: "Workflow, Cloud-Apps, Cloud-Dienste, Geschäftsprozesse, Systemintegration, Enterprise Application Integration, EAI"
documentationcenter: 
author: rick-anderson
manager: wpickett
editor: 
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: riande
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 0d3eb2af197e9923d8e4a86bf1a0033f61e3c568
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="building-a-serverless-social-media-dashboard-in-azure"></a>Erstellen eines serverlosen Dashboards für soziale Medien in Azure

[Azure Functions](functions-overview.md) ist in [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md) integriert, damit Sie komplexe Orchestrierungen mit anderen Azure-Diensten und Diensten von Drittanbietern erstellen können. In diesem Thema wird veranschaulicht, wie eine Logik-App über einen Feed in sozialen Medien ausgelöst und der Text mit [Azure Cognitive Services](../cognitive-services/Welcome.md) analysiert wird.

In diesem Artikel wird beschrieben, wie Sie eine Logik-App mit folgenden Aufgaben im Azure-Portal erstellen:

> [!div class="checklist"]
> * Prüfen auf neue Tweets mit einem Schlüsselwort oder Hashtag, das Sie angeben.
> * Verwenden des Connectors zum **Erkennen von Stimmungen** zum Schätzen der Stimmung von Tweets (von schlecht bis gut).
> * Verwenden einer Azure-Funktion zum Unterteilen der Stimmung von Tweets in drei Kategorien (RED, YELLOW oder GREEN für schlecht, neutral und gut).
> * Verwenden einer Bedingung zum Überprüfen, ob die Stimmung RED (schlecht) ist.
> * Senden einer E-Mail, wenn die Bedingung RED ist.

Die folgende Abbildung zeigt einen Teil der Logik-App im Designer:

![Abbildung der ersten zwei Schritte der App im Logik-App-Designer](media/functions-twitter-email/designer1.png)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* Ein Twitter-Konto.

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App
 
[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal2.md)]

### <a name="create-a-categorize-function"></a>Erstellen einer Kategorisierungsfunktion

Wenn die Bereitstellung der Funktionen-App abgeschlossen ist, öffnen Sie die neue Funktionen-App. In diesem Abschnitt erstellen Sie eine Funktion zum Unterteilen der Stimmung von Tweets in drei Kategorien (RED, YELLOW oder GREEN für schlecht, neutral und gut).

![Blatt „Funktionen-App“, Funktionen +](media/functions-twitter-email/add_fun.png)

Behalten Sie den Standardwert für **Webhook + API** und **CSharp** bei, und wählen Sie dann **Diese Funktion erstellen** aus.

![Blatt „Funktionen-App“, Funktionen +](media/functions-twitter-email/add_fun2.png)

Sie haben eine Webhook/API-Funktion erstellt (auch bekannt als HTTP-Trigger), die bei Bedarf von der App aufgerufen werden kann, die Sie erstellen. Wenn Sie eine Funktion erstellen möchten, die nach einem Zeitplan ausgeführt wird, erstellen Sie eine Timerfunktion.

Ersetzen Sie den Inhalt der Datei *run.csx* durch den folgenden Code:

```c#
using System.Net;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    string category = "GREEN";

    // Get request body.
    double score = await req.Content.ReadAsAsync<double>();

    if (score < .3)
    {
        category = "RED";
    }
    else if (score < .6)
    {
        category = "YELLOW";
    }

    return req.CreateResponse(HttpStatusCode.OK, category);
}
```

Speichern Sie die Änderungen.

### <a name="test-the-function"></a>Testen der Funktion

Wählen Sie **Testen** (rechts neben dem Codefeld) aus.  Geben Sie 0.2 in das Textfeld **Anforderungstext** ein, und wählen Sie dann **Ausführen** aus. Die Ausgabe zeigt „RED“ an, und der HTTP-Statuscode ist „200 OK“.

 ![test ](media/functions-twitter-email/test.png)

## <a name="cognitive-services"></a>Cognitive Services

Erstellen Sie ein Cognitive Services-Konto. Ein Cognitive Services-Konto ist erforderlich, um die Stimmung von Tweets zu erkennen, die wir überwachen.

Navigieren Sie zu **Neu > Intelligence und Analyse > Cognitive Services**. Legen Sie alle Pflichtfelder fest:

![Blatt zum Erstellen eines Cognitive-Kontos](media/functions-twitter-email/cog_svcs_account.png)

| Feld               | Beispielwert | Kommentar |
| ----------------- | ------------ | ------------- |
| Kontoname | MyCognitiveServicesAccnt | Geben Sie einen eindeutigen Namen ein. |
| API-Typ | Textanalyse-API | Wählen Sie „Textanalyse“ aus. |
| Ort | USA (West) | Derzeit ist nur **USA, Westen** verfügbar |
| Tarif | F0 | Wenn die Aufrufe nicht ausreichen, legen Sie einen höheren Tarif fest.|
| Ressourcengruppe | rg1 | Verwenden Sie die Ressourcengruppe, die Sie zuvor angegeben haben.|

### <a name="copy-the-cognitive-services-key"></a>Kopieren des Cognitive Services-Schlüssels

Wählen Sie **Schlüssel** aus. Sie benötigen einen Schlüssel in einem späteren Schritt.

 ![Schlüssel](media/functions-twitter-email/keys.png)

## <a name="create-a-logic-app"></a>Erstellen einer Logik-App

Klicken Sie im Azure-Portal auf **Neu > Enterprise Integration > Logik-App**.

![Schritt für die neue Logik-App, vorangehender Schritt](media/functions-twitter-email/new_logicApp.png)

Geben Sie auf dem Blatt **Logik-App erstellen** einen Wert in jedes Feld ein, und wählen Sie **Erstellen** aus.

![Schritt zum Erstellen der Logik-App, vorangehender Schritt](media/functions-twitter-email/new_logicApp2.png)

Sobald die Logik-App erstellt wurde, wird sie im Designer geöffnet. Wählen Sie die Vorlage **Leere Logik-App** aus.

![Leere Logik-App](media/functions-twitter-email/blank.png)

## <a name="add-a-trigger-to-twitter"></a>Hinzufügen eines Triggers für Twitter

Im **Logik-App-Designer** werden viele Dienste und Trigger angezeigt, mit denen Sie eine Verbindung herstellen können.

Wählen Sie den Dienst **Twitter** aus.

![Twitter-Connector](media/functions-twitter-email/twitter_connector.png)

Wählen Sie den Trigger **Wenn ein neuer Tweet gepostet wird** aus.

![Trigger „Wenn ein neuer Tweet gepostet wird“](media/functions-twitter-email/tw_trig.png)

Melden Sie sich bei Ihrem Twitter-Konto an.

![Melden Sie sich bei Ihrem Twitter-Konto an](media/functions-twitter-email/signin_twit.png)

Geben Sie Ihr Kennwort ein, und wählen Sie **Authorize app** (App autorisieren) aus.

![Authentifizierung bei Twitter in einem neuem Fenster](media/functions-twitter-email/auth_twit.png)

Geben Sie Suchtext, Häufigkeit und Intervall ein. Wenn Sie ein beliebtes Hashtag (z.B. #fußball, #soccer oder #football) angeben, können Sie schnell alle Ihnen zugewiesenen Dienstaufrufe in Ihrem Cognitive Services-Konto verwenden. Wenn die Aufrufe nicht ausreichen, können Sie den Tarif erhöhen. 

Suchen Sie alle 15 Minuten nach #Azure:

![#Azure alle 15 Minuten](media/functions-twitter-email/azure_tweet.png)

Speichern Sie die App.

### <a name="add-a-text-analytics-connector"></a>Hinzufügen eines **Textanalyse**-Connectors

Der Textanalyse-Connector erkennt die Stimmung von Tweets.

Wählen Sie **Neuer Schritt** und anschließend **Aktion hinzufügen** aus.

![„Neuer Schritt“ und anschließend „Aktion hinzufügen“](media/functions-twitter-email/new_step.png)

Fügen Sie den **Textanalyse**-Connector hinzu.

![Fenster zum Auswählen einer Aktion](media/functions-twitter-email/choose_action.png)

Wählen Sie die Aktion **Detect Sentiment** (Stimmung erkennen) aus. Die Bewertung der Stimmung ist häufig richtig, aber manchmal wird Text falsch interpretiert.

![Stimmung erkennen](media/functions-twitter-email/detect_sent.png)

### <a name="create-the-detect-sentiment-action"></a>Erstellen einer Aktion zum Erkennen der Stimmung

  * Geben Sie einen Verbindungsnamen wie **MyKey** ein.
  * Kopieren Sie den Schlüssel, den Sie im Schritt [Erstellen eines Cognitive Services-Kontos](#cognitive-services) erstellt haben, und fügen Sie ihn ein.
  * Klicken Sie auf **Erstellen**.
  * Speichern Sie die App.

![Stimmung erkennen](media/functions-twitter-email/ta_detect_sent.png)

Wählen Sie das Symbol **Tweettext** für den **zu analysierenden Text** aus.

![Stimmung erkennen](media/functions-twitter-email/ds_tta.png)

![Stimmung erkennen](media/functions-twitter-email/ds_tta2.png)

Speichern Sie die App.

## <a name="connect-to-the-azure-function"></a>Herstellen einer Verbindung mit der Azure-Funktion

In diesem Abschnitt fügen Sie die zuvor erstellte Funktion hinzu, die die Stimmung von Tweets als RED, YELLOW oder GREEN kategorisiert hat.

* Wählen Sie im Logik-Apps-Designer **Neuer Schritt** und dann **Aktion hinzufügen** aus.
* Wählen Sie **Azure Functions** aus.
* Wählen Sie **Azure-Funktion wählen** aus.

![Azure Functions-Feld mit der Funktion „Azure-Funktion wählen“](media/functions-twitter-email/choose_fun.png)

* Wählen Sie die Azure-Funktion aus, die Sie zuvor erstellt haben.
* Wählen Sie **Bewertung** aus, um den **Anforderungstext** anzugeben.

![Punkte](media/functions-twitter-email/trigger_score.png)

Speichern Sie die App.

## <a name="add-email-notification"></a>Hinzufügen einer E-Mail-Benachrichtigung

In diesem Abschnitt fügen wir eine Bedingungsüberprüfung für Tweets mit negativer Stimmung hinzu (Bedingung RED).

* Wählen Sie **Neuer Schritt** aus.
* Wählen Sie **Bedingung hinzufügen**aus.
* Wählen Sie **Text** im ersten Textfeld **Wert auswählen** aus.
* Geben Sie „RED“ in das zweite Textfeld **Wert auswählen** ein.
* Speichern Sie die App.

![Feld für Bedingungen](media/functions-twitter-email/condition.png)

* Wählen Sie im Feld **Bei ja keine Aktion** die Option **Aktion hinzufügen** aus.
* Geben Sie Outlook oder Gmail in das Feld **Alle Dienste und Aktionen durchsuchen** ein. In diesem Tutorial wird Outlook verwendet. Gmail-Anweisungen finden Sie unter [Hinzufügen einer Gmail-Aktion] (../logic-apps/logic-apps-create-a-logic-app.md#add-an-action-that-responds-to-your-trigger). Hinweis: Besitzer eines privaten [Microsoft-Kontos](https://account.microsoft.com/account) können dieses als Outlook.com-Konto verwenden.

![Feld zum Auswählen einer Aktion](media/functions-twitter-email/outlook.png)

Wählen Sie **Outlook.com E-Mail senden** aus.

![Outlook.com-Feld](media/functions-twitter-email/sendEmail.png)

Melden Sie sich bei Outlook.com an.

![Anmeldefeld](media/functions-twitter-email/signin_outlook.png)

Geben Sie die folgenden Elemente ein:

   * **An**: Die E-Mail-Adresse, an die die Nachricht gesendet werden soll.
   * **Betreff**: Bewertung.
   * **Text**: Der Speicherort und der Tweettext.

![Feld zum Senden einer E-Mail](media/functions-twitter-email/sendEmail2.png)

Speichern Sie die App.
Wählen Sie **Ausführen** aus, um die App zu starten.

### <a name="check-the-status"></a>Überprüfen des Status

Wählen Sie auf dem Logik-App-Blatt **Übersicht** und dann eine Zeile in der Spalte **Alle Ausführungen** aus:

![Blatt „Übersicht“](media/functions-twitter-email/over1.png)

Die folgende Abbildung zeigt die Ausführungsdetails für den Fall an, dass die Bedingung nicht wahr ist und die E-Mail nicht gesendet wurde.

![Blatt „Übersicht“](media/functions-twitter-email/skipped.png)

Wenn Sie die Funktion **E-Mail senden** sofort testen möchten:

* Ändern Sie die **EINGABEN** im ersten Schritt (**Wenn ein neuer Tweet gepostet wird**) in einen beliebten Begriff wie #fußball, #soccer oder #football.

Bei beliebten Begriffen beansprucht die Verarbeitung mehr Ressourcen als bei weniger beliebten Begriffen. Sie sollten den Suchbegriff wieder ändern, nachdem Sie überprüft haben, ob die E-Mail funktioniert.

Die folgende Abbildung zeigt die Ausführungsdetails für den Fall an, dass die Bedingung wahr ist und die E-Mail gesendet wurde.

![Blatt „Übersicht“](media/functions-twitter-email/sent.png)

Sie können beliebige Dienstfelder auswählen, um weitere Informationen zu den Daten anzuzeigen, die für die Ausführung verwendet wurden. Wenn Sie **Wenn ein neuer Tweet gepostet wird** auswählen, werden der Suchtext und alle Ausgaben angezeigt, auch die Ausgaben, die wir nicht verwenden.

## <a name="next-steps"></a>Nächste Schritte

*  [Einführung in Azure Functions](functions-overview.md)
*  [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md)
*  [Hinzufügen von Bedingungen und Ausführen von Workflows](../logic-apps/logic-apps-use-logic-app-features.md)
*  [Logik-App-Vorlagen](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [Erstellen einer Logik-App mithilfe einer Vorlage](../logic-apps/logic-apps-arm-provision.md)

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Im [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) können Sie Fragen stellen, Fragen beantworten und sich über die Aktivitäten anderer Azure Logic Apps-Benutzer informieren.

Zur Optimierung von Azure Logic Apps und Connectors können Sie auf der [Benutzerfeedbackwebsite für Azure Logic Apps](http://aka.ms/logicapps-wish) über Ideen abstimmen oder selbst Ideen einreichen.

