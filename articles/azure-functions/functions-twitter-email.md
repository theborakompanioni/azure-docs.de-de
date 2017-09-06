---
title: Erstellen einer Funktion, die in Azure Logic Apps integriert ist | Microsoft-Dokumentation
description: Erstellen Sie eine Funktion, die Azure Logic Apps und Azure Cognitive Services integriert, um Stimmungen in Tweets zu kategorisieren und Benachrichtigungen zu senden, wenn die Stimmungslage schlecht ist.
services: functions, logic-apps, cognitive-services
keywords: "Workflow, Cloud-Apps, Cloud-Dienste, Geschäftsprozesse, Systemintegration, Enterprise Application Integration, EAI"
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: glenga
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 4a5dc668e21c5328b308c8f5852aaa922232374d
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---

# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a>Erstellen einer Funktion, die in Azure Logic Apps integriert ist

Azure Functions ist in Azure Logic Apps im Logik-App-Designer integriert. Dank dieser Integration können Sie die Rechenleistung von Functions in Orchestrierungen mit anderen Azure- und Drittanbieterdiensten nutzen. 

In diesem Tutorial wird gezeigt, wie Sie Functions mit Logic Apps und Azure Cognitive Services verwenden, um die Stimmung von Twitter-Postings zu analysieren. Mit einer Funktion mit HTTP-Auslösung werden Tweets basierend auf dem Stimmungswert als Grün, Gelb oder Rot (Green, Yellow, Red) kategorisiert. Wenn eine negative Stimmung erkannt wird, wird eine E-Mail gesendet. 

![Abbildung: Die ersten beiden Schritte der App im Logik-App-Designer](media/functions-twitter-email/designer1.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Cognitive Services-Kontos
> * Erstellen einer Funktion, mit der die Stimmung von Tweets kategorisiert wird
> * Erstellen einer Logik-App, mit der eine Verbindung mit Twitter hergestellt wird
> * Hinzufügen der Stimmungserkennung zur Logik-App 
> * Verbinden der Logik-App mit der Funktion
> * Senden einer E-Mail basierend auf der Antwort von der Funktion

## <a name="prerequisites"></a>Voraussetzungen

+ Ein aktives [Twitter](https://twitter.com/)-Konto 
+ Ein [Outlook.com](https://outlook.com/)-Konto (zum Senden von Benachrichtigungen)
+ In diesem Thema werden als Ausgangspunkt die Ressourcen verwendet, die unter [Erstellen Ihrer ersten Funktion im Azure-Portal](functions-create-first-azure-function.md) erstellt wurden.  
Führen Sie nun diese Schritte zum Erstellen Ihrer Funktionen-App durch, sofern dies noch nicht geschehen ist.

## <a name="create-a-cognitive-services-account"></a>Erstellen Sie ein Cognitive Services-Konto.

Ein Cognitive Services-Konto ist erforderlich, um die Stimmung von Tweets zu erkennen, die überwacht werden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

2. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

3. Klicken Sie auf **Daten + Analysen** > **Cognitive Services**. Verwenden Sie anschließend die Einstellungen gemäß den Angaben in der Tabelle, akzeptieren Sie die Bedingungen, und aktivieren Sie die Option **An Dashboard anheften**.

    ![Blatt zum Erstellen eines Cognitive-Kontos](media/functions-twitter-email/cog_svcs_account.png)

    | Einstellung      |  Empfohlener Wert   | Beschreibung                                        |
    | --- | --- | --- |
    | **Name** | MyCognitiveServicesAccnt | Wählen Sie einen eindeutigen Kontonamen. |
    | **API-Typ** | Textanalyse-API | API zum Analysieren von Text  |
    | **Location** | USA (West) | Derzeit ist nur **USA, Westen** für Textanalysen verfügbar. |
    | **Tarif** | F0 | Beginnen Sie mit dem niedrigsten Tarif. Wenn die Aufrufe nicht ausreichen, können Sie einen höheren Tarif festlegen.|
    | **Ressourcengruppe** | myResourceGroup | Verwenden Sie für alle Dienste in diesem Tutorial dieselbe Ressourcengruppe.|

4. Klicken Sie auf **Erstellen**, um Ihr Konto zu erstellen. Klicken Sie nach der Erstellung des Kontos auf Ihr neues Cognitive Services-Konto, das an das Dashboard angeheftet ist. 

5. Klicken Sie im Konto auf **Schlüssel**, kopieren Sie den Wert von **Schlüssel 1**, und speichern Sie ihn. Sie verwenden diesen Schlüssel, um die Logik-App mit Ihrem Cognitive Services-Konto zu verbinden. 
 
    ![Schlüssel](media/functions-twitter-email/keys.png)

## <a name="create-the-function"></a>Erstellen der Funktion

„Functions“ ist eine gute Möglichkeit, um Verarbeitungsaufgaben in einen Logic Apps-Workflow zu verlagern. In diesem Tutorial wird eine Funktion mit HTTP-Auslösung verwendet, um Tweet-Stimmungswerte aus Cognitive Services zu verarbeiten und einen Kategoriewert zurückzugeben.  

1. Erweitern Sie Ihre Funktionen-App, klicken Sie neben **Functions** auf die Schaltfläche **+**, und klicken Sie auf die Vorlage **HTTPTrigger**. Geben Sie `CategorizeSentiment` für die Funktion **Name** ein, und klicken Sie auf **Erstellen**.

    ![Blatt „Funktionen-App“, Funktionen +](media/functions-twitter-email/add_fun.png)

2. Ersetzen Sie den Inhalt der Datei „run.csx“ durch den folgenden Code, und klicken Sie auf **Speichern**:

    ```c#
    using System.Net;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // The sentiment category defaults to 'GREEN'. 
        string category = "GREEN";
    
        // Get the sentiment score from the request body.
        double score = await req.Content.ReadAsAsync<double>();
        log.Info(string.Format("The sentiment score received is '{0}'.",
                    score.ToString()));
    
        // Set the category based on the sentiment score.
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
    Dieser Funktionscode gibt eine Farbkategorie zurück, die auf dem mit der Anforderung empfangenen Stimmungswert basiert. 

3. Klicken Sie zum Testen der Funktion ganz rechts auf **Testen**, um die Registerkarte „Testen“ zu erweitern. Geben Sie den Wert `0.2` für **Anforderungstext** ein, und klicken Sie dann auf **Ausführen**. Im Text der Antwort wird der Wert **RED** (ROT) zurückgegeben. 

    ![Testen der Funktion im Azure-Portal](./media/functions-twitter-email/test.png)

Sie verfügen nun über eine Funktion zum Kategorisieren von Stimmungswerten. Als Nächstes erstellen Sie eine Logik-App, mit der Ihre Funktion in Ihre Twitter- und Cognitive Services-Konten integriert wird. 

## <a name="create-a-logic-app"></a>Erstellen einer Logik-App   

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Klicken Sie auf **Enterprise Integration** > **Logik-App**. Verwenden Sie anschließend die Einstellungen gemäß der Angabe in der Tabelle, aktivieren Sie die Option **An Dashboard anheften**, und klicken Sie auf **Erstellen**.
 
4. Geben Sie dann einen **Namen** ein, z.B. `TweetSentiment`, verwenden Sie die Einstellungen gemäß den Angaben in der Tabelle, akzeptieren Sie die Bedingungen, und aktivieren Sie die Option **An Dashboard anheften**.

    ![Erstellen einer Logik-App im Azure-Portal](./media/functions-twitter-email/new_logicApp.png)

    | Einstellung      |  Empfohlener Wert   | Beschreibung                                        |
    | ----------------- | ------------ | ------------- |
    | **Name** | TweetSentiment | Wählen Sie einen geeigneten Namen für Ihre App. |
    | **Ressourcengruppe** | myResourceGroup | API zum Analysieren von Text  |
    | **Location** | USA (Ost) | Wählen Sie einen Standort in Ihrer Nähe aus. |
    | **Ressourcengruppe** | myResourceGroup | Wählen Sie dieselbe vorhandene Ressourcengruppe wie zuvor.|

4. Klicken Sie auf **Erstellen**, um die Logik-App zu erstellen. Klicken Sie nach der Erstellung der App auf Ihre neue Logik-App, die an das Dashboard angeheftet ist. Führen Sie dann im Logik-App-Designer einen Bildlauf nach unten durch, und klicken Sie auf die Vorlage **Leere Logik-App**. 

    ![Vorlage „Leere Logik-App“](media/functions-twitter-email/blank.png)

Sie können jetzt den Logik-App-Designer verwenden, um Ihrer App Dienste und Trigger hinzuzufügen.

## <a name="connect-to-twitter"></a>Verbinden mit Twitter

Erstellen Sie zuerst eine Verbindung mit Ihrem Twitter-Konto. Die Logik-App führt eine Abfrage nach Tweets durch, die die Ausführung der App auslösen.

1. Klicken Sie im Designer auf den Dienst **Twitter**, und klicken Sie dann auf den Trigger **Wenn ein neuer Tweet gepostet wird**. Melden Sie sich an Ihrem Twitter-Konto an, und lassen Sie für Logic Apps die Verwendung Ihres Kontos zu.

2. Verwenden Sie die Twitter-Triggereinstellungen, wie in der Tabelle angegeben. 

    ![Twitter-Connectoreinstellungen](media/functions-twitter-email/azure_tweet.png)

    | Einstellung      |  Empfohlener Wert   | Beschreibung                                        |
    | ----------------- | ------------ | ------------- |
    | **Suchtext** | #Azure | Verwenden Sie ein Hashtag, das beliebt genug ist, damit im gewählten Zeitraum neue Tweets generiert werden. Wenn Sie den Free-Tarif verwenden und Ihr Hashtag eine zu hohe Beliebtheit aufweist, kann es passieren, dass die Transaktionen Ihres Cognitive Services-Kontos schnell aufgebraucht sind. |
    | **Frequency** | Minute | Die Häufigkeitseinheit, die zum Abfragen von Twitter verwendet wird.  |
    | **Intervall** | 15 | Die zwischen Twitter-Anforderungen verstrichene Zeit in Häufigkeitseinheiten. |

3.  Klicken Sie auf **Speichern**, um eine Verbindung mit Ihrem Twitter-Konto herzustellen. 

Ihre App ist jetzt mit Twitter verbunden. Stellen Sie als Nächstes eine Verbindung mit der Textanalyse her, um die Stimmung für die gesammelten Tweets zu erkennen.

## <a name="add-sentiment-detection"></a>Hinzufügen der Stimmungserkennung

1. Klicken Sie auf **Neuer Schritt** und dann auf **Aktion hinzufügen**.

    ![„Neuer Schritt“ und anschließend „Aktion hinzufügen“](media/functions-twitter-email/new_step.png)

2. Klicken Sie unter **Aktion auswählen** auf **Textanalyse** und anschließend auf die Aktion **Detect sentiment** (Stimmung erkennen).

    ![Stimmung erkennen](media/functions-twitter-email/detect_sent.png)

3. Geben Sie einen Verbindungsnamen ein, z.B. `MyCognitiveServicesConnection`, fügen Sie den Schlüssel für Ihr gespeichertes Cognitive Services-Konto ein, und klicken Sie auf **Erstellen**.  

4. Klicken Sie auf **Text to analyze (Zu analysierender Text)** > **Tweettext** und dann auf **Speichern**.  

    ![Stimmung erkennen](media/functions-twitter-email/ds_tta.png)

Nachdem die Stimmungserkennung nun konfiguriert wurde, können Sie Ihrer Funktion eine Verbindung hinzufügen, die die Ausgabe mit dem Stimmungswert nutzt.

## <a name="connect-sentiment-output-to-your-function"></a>Ausgabe mit dem Stimmungswert für Ihre Funktion

1. Klicken Sie im Logik-App-Designer auf **Neuer Schritt** > **Aktion hinzufügen** und dann auf **Azure Functions**. 

2. Klicken Sie auf **Azure-Funktion wählen**, und wählen Sie die zuvor erstellte Funktion **CategorizeSentiment**.  

    ![Azure Functions-Feld mit der Funktion „Azure-Funktion wählen“](media/functions-twitter-email/choose_fun.png)

3. Klicken Sie unter **Anforderungstext** auf **Score** (Ergebnis) und dann auf **Speichern**.

    ![Punkte](media/functions-twitter-email/trigger_score.png)

Ihre Funktion wird jetzt ausgelöst, wenn von der Logik-App ein Stimmungswert gesendet wird. Von der Funktion wird eine Kategorie mit Farbcodierung an die Logik-App zurückgegeben. Als Nächstes fügen Sie eine E-Mail-Benachrichtigung hinzu, die gesendet wird, wenn der Stimmungswert **RED** (ROT) von der Funktion zurückgegeben wird. 

## <a name="add-email-notifications"></a>Hinzufügen von E-Mail-Benachrichtigungen

Der letzte Teil des Workflows ist das Auslösen einer E-Mail, wenn der Wert für die Stimmung _RED_ (ROT) lautet. In diesem Thema wird ein Outlook.com-Connector verwendet. Sie können ähnliche Schritte ausführen, um einen Gmail- oder Office 365 Outlook-Connector zu verwenden.   

1. Klicken Sie im Logik-App-Designer auf **Neuer Schritt** > **Bedingung hinzufügen**. 

2. Klicken Sie auf **Wert auswählen** und dann auf **Text**. Wählen Sie die Option **ist gleich**, klicken Sie auf **Wert auswählen**, und geben Sie `RED` ein. Klicken Sie anschließen auf **Speichern**. 

    ![Fügen Sie der Logik-App eine Bedingung hinzu.](media/functions-twitter-email/condition.png)

3. Klicken Sie unter **IF YES, DO NOTHING** (WENN JA, KEINE AKTION AUSFÜHREN) auf **Aktion hinzufügen**, suchen Sie nach `outlook.com`, klicken Sie auf **E-Mail senden**, und melden Sie sich an Ihrem Outlook.com-Konto an.
    
    ![Wählen Sie eine Aktion für die Bedingung aus.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > Wenn Sie kein Outlook.com-Konto besitzen, können Sie einen anderen Connector wählen, z.B. Gmail oder Office 365 Outlook.

4. Verwenden Sie in der Aktion **E-Mail senden** die E-Mail-Einstellungen gemäß den Angaben in der Tabelle. 

    ![Konfigurieren Sie die E-Mail für die Aktion „E-Mail senden“.](media/functions-twitter-email/sendemail.png)

    | Einstellung      |  Empfohlener Wert   | Beschreibung  |
    | ----------------- | ------------ | ------------- |
    | **An** | Geben Sie Ihre E-Mail-Adresse ein | Die E-Mail-Adresse, an die die Benachrichtigung gesendet wird. |
    | **Betreff** | Negative Tweet-Stimmung erkannt  | Die Betreffzeile der E-Mail-Benachrichtigung.  |
    | **Text** | Tweettext, Standort | Klicken Sie auf die Parameter **Tweettext** und **Standort**. |

5.  Klicken Sie auf **Speichern**.

Nachdem der Workflow jetzt fertig ist, können Sie die Logik-App aktivieren und die Funktion in Aktion sehen.

## <a name="test-the-workflow"></a>Testen des Workflows

1. Klicken Sie im Logik-App-Designer auf **Ausführen**, um die App zu starten.

2. Klicken Sie in der linken Spalte auf **Übersicht**, um den Status der Logik-App anzuzeigen. 
 
    ![Ausführungsstatus der Logik-App](media/functions-twitter-email/over1.png)

3. (Optional) Klicken Sie auf eine der Ausführungen, um die Details dazu anzuzeigen.

4. Navigieren Sie zu Ihrer Funktion, zeigen Sie die Protokolle an, und stellen Sie sicher, dass Stimmungswerte empfangen und verarbeitet wurden.
 
    ![Anzeigen von Funktionsprotokollen](media/functions-twitter-email/sent.png)

5. Wenn eine potenziell negative Stimmung erkannt wird, erhalten Sie eine E-Mail. Falls Sie keine E-Mail erhalten haben, können Sie den Funktionscode so ändern, dass jedes Mal RED (ROT) zurückgegeben wird:

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    Wechseln Sie nach der Überprüfung der E-Mail-Benachrichtigungen zurück zum ursprünglichen Code:

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > Nach Abschluss dieses Tutorials sollten Sie die Logik-App deaktivieren. Durch das Deaktivieren der App verhindern Sie, dass Ihnen für Ausführungen Gebühren berechnet und die Transaktionen für Ihr Cognitive Services-Konto aufgebraucht werden.

Sie haben gesehen, wie einfach es ist, Functions in einen Logik-Apps-Workflow zu integrieren.

## <a name="disable-the-logic-app"></a>Deaktivieren der Logik-App

Klicken Sie zum Deaktivieren der Logik-App auf **Übersicht** und dann oben auf dem Bildschirm auf **Deaktivieren**. So wird verhindert, dass die Logik-App ausgeführt wird und Gebühren anfallen, ohne dass die App gelöscht wird. 

![Funktionsprotokolle](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen Sie ein Cognitive Services-Konto.
> * Erstellen einer Funktion, mit der die Stimmung von Tweets kategorisiert wird
> * Erstellen einer Logik-App, mit der eine Verbindung mit Twitter hergestellt wird
> * Hinzufügen der Stimmungserkennung zur Logik-App 
> * Verbinden der Logik-App mit der Funktion
> * Senden einer E-Mail basierend auf der Antwort von der Funktion

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie für Ihre Funktion eine serverlose API erstellen.

> [!div class="nextstepaction"] 
> [Erstellen einer serverlosen API mit Azure Functions](functions-create-serverless-api.md)

Weitere Informationen zu Logik-Apps finden Sie unter [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).


