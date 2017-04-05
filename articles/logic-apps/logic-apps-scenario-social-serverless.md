---
title: "Szenario – Erstellen eines Customer Insights-Dashboards mit Azure Serverless | Microsoft-Dokumentation"
description: "Enthält ein Beispiel dafür, wie Sie ein Dashboard zum Verwalten von Kundenfeedback, Daten aus sozialen Netzwerken und mehr mit Azure Logic Apps und Azure Functions erstellen können."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2017
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 4676e0da4309b1460e471f94946161fa22d46226
ms.lasthandoff: 03/30/2017

---
# <a name="create-a-real-time-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Erstellen eines Customer Insights-Echtzeit-Dashboards mit Azure Logic Apps und Azure Functions

Azure Serverless-Tools verfügen über leistungsstarke Funktionen zum schnellen Erstellen und Hosten von Anwendungen in der Cloud, ohne dass Sie sich um die Infrastruktur kümmern müssen.  In diesem Szenario erstellen wir ein Dashboard zum Auslösen von Aktionen bei Kundenfeedback, Analysieren von Feedback per Machine Learning und Veröffentlichen von Einblicken in eine Quelle wie Power BI oder Azure Data Lake.

## <a name="overview-of-the-scenario-and-tools-used"></a>Übersicht über das Szenario und die verwendeten Tools

Zum Implementieren dieser Lösung nutzen wir die beiden Hauptkomponenten von serverlosen Apps in Azure: [Azure Functions](https://azure.microsoft.com/services/functions/) und [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).

Bei Logic Apps handelt es sich um ein serverloses Workflowmodul in der Cloud.  Es ermöglicht die Orchestrierung über serverlose Komponenten hinweg und eine Verbindung mit mehr als 100 Diensten und APIs.  Für dieses Szenario erstellen wir eine Logik-App, um bei Feedback von Kunden Aktionen auszulösen.  Einige Connectors, die beim Reagieren auf Kundenfeedback hilfreich sein können, sind Outlook.com, Office 365, Survey Monkey, Twitter und eine HTTP-Anforderung [aus einem Webformular](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).  Für den unten angegebenen Workflow überwachen wir ein Hashtag auf Twitter.

Azure Functions ermöglicht serverlose Computevorgänge in der Cloud.  In diesem Szenario verwenden wir Azure Functions, um Tweets von Kunden anhand von verschiedenen vordefinierten Schlüsselwörtern zu kennzeichnen.

Die gesamte Lösung kann [in Visual Studio erstellt](logic-apps-deploy-from-vs.md) und [als Teil einer Ressourcenvorlage bereitgestellt werden](logic-apps-create-deploy-template.md).  Bei [Channel 9](http://aka.ms/logicappsdemo) finden Sie außerdem ein Video mit der exemplarischen Vorgehensweise zu diesem Szenario.

## <a name="building-the-logic-app-to-trigger-on-customer-data"></a>Erstellen der Logik-App für die Auslösung basierend auf Kundendaten

Gehen Sie wie folgt vor, nachdem Sie in Visual Studio oder im Azure-Portal eine [Logik-App erstellt](logic-apps-create-a-logic-app.md) haben:

1. Fügen Sie einen Trigger für **On New Tweets** (Bei neuen Tweets) von Twitter hinzu.
1. Konfigurieren Sie den Trigger so, dass er basierend auf einem Schlüsselwort oder Hashtag auf Tweets lauscht.

> [!NOTE]
> Mit der Wiederholungseigenschaft des Triggers wird bestimmt, wie häufig die Logik-App bei abrufbasierten Triggern eine Überprüfung auf neue Elemente durchführt.

![Beispiel für Twitter-Trigger][1]

Diese App wird jetzt für alle neuen Tweets ausgelöst.  Wir können diese Tweetdaten dann auf die darin ausgedrückte Stimmung untersuchen.  Um die Stimmung des Texts zu erkennen, nutzen wir den [Cognitive Service von Azure](https://azure.microsoft.com/services/cognitive-services/).

1. Klicken Sie auf **Neuer Schritt**.
1. Wählen Sie den Connector **Textanalyse** (bzw. suchen Sie danach).
1. Wählen Sie den Vorgang **Detect Sentiment** (Stimmung erkennen) aus.
1. Geben Sie nach Aufforderung einen gültigen Cognitive Services-Schlüssel für den Textanalysedienst an.
1. Fügen Sie den **Tweettext** als zu analysierenden Text hinzu.

Nachdem wir nun über die Tweetdaten und Erkenntnisse zum Tweet verfügen, sind ggf. einige andere Connectors relevant:
* Power BI – Zeilen zum Streamingdataset hinzufügen: Dient zum Anzeigen von Tweets in Echtzeit in einem Power BI-Dashboard.
* Azure Data Lake – Datei anfügen: Dient zum Hinzufügen von Kundendaten zu einem Azure Data Lake-Dataset, um Analyseaufträge einzubinden.
* SQL – Zeilen hinzufügen: Dient zum Speichern von Daten in einer Datenbank zur späteren Wiederverwendung.
* Slack – Nachricht senden: Dient zum Erstellen einer Warnung für einen Slack-Kanal bei negativem Feedback, für das Aktionen erforderlich sind.

Außerdem kann eine Azure-Funktion verwendet werden, um für die Daten weitere benutzerdefinierte Computeaktionen durchzuführen.

## <a name="enriching-the-data-with-an-azure-function"></a>Erweitern der Daten mit einer Azure-Funktion

Bevor wir eine Funktion erstellen können, müssen wir unter unserem Azure-Abonnement über eine Funktionen-App verfügen.  Ausführliche Informationen zum Erstellen einer Azure-Funktion im Portal finden Sie [hier](../azure-functions/functions-create-first-azure-function-azure-portal.md).

Damit eine Funktion direkt aus einer Logik-App aufgerufen werden kann, muss sie über eine HTTP-Triggerbindung verfügen.  Es wird empfohlen, die Vorlage **HttpTrigger** zu verwenden.

In diesem Szenario ist der Anforderungstext der Azure-Funktion der Tweettext.  Legen Sie im Funktionscode einfach fest, dass die Logik aktiviert werden soll, wenn der Tweettext ein Schlüsselwort oder einen Ausdruck enthält.  Die Funktion selbst kann je nach Szenario einfach oder komplex sein.

Geben Sie am Ende der Funktion einfach eine Antwort mit einigen Daten für die Logik-App zurück.  Dies kann ein einfacher boolescher Wert (z.B. `containsKeyword`) oder ein komplexes Objekt sein.

![Schritt zum Konfigurieren der Azure-Funktion][2]

> [!TIP]
> Verwenden Sie beim Zugreifen auf eine komplexe Antwort einer Funktion in einer Logik-App die Aktion „JSON analysieren“.

Nachdem die Funktion gespeichert wurde, kann sie der oben erstellten Logik-App hinzugefügt werden.  In der Logik-App:

1. Klicken Sie, um einen **Neuen Schritt** hinzuzufügen.
1. Wählen Sie den Connector **Azure Functions** aus.
1. Wählen Sie die Option zum Auswählen einer vorhandenen Funktion, und greifen Sie auf die erstellte Funktion zu.
1. Senden Sie den **Tweettext** für den **Anforderungstext**.

## <a name="running-and-monitoring-the-solution"></a>Ausführen und Überwachen der Lösung

Einer der Vorteile bei der Erstellung von serverlosen Orchestrierungen in Logik-Apps sind die umfassenden Debug- und Überwachungsfunktionen.  Alle Ausführungen (aktuell oder älter) können in Visual Studio, im Azure-Portal oder über die REST-API und SDKs angezeigt werden.

Eine der einfachsten Möglichkeiten zum Testen einer Logik-App ist die Verwendung der Schaltfläche **Ausführen** im Designer.  Wenn Sie auf **Ausführen** klicken, wird die Triggerabfrage weiterhin alle fünf Sekunden durchgeführt, bis ein Ereignis erkannt wird. Außerdem erhalten Sie während der Ausführung eine Liveanzeige.

Vorherige Ausführungsverläufe können auf dem Blatt „Übersicht“ im Azure-Portal angezeigt werden, oder Sie können den Visual Studio Cloud-Explorer verwenden.

## <a name="creating-a-deployment-template-for-automated-deployments"></a>Erstellen einer Bereitstellungsvorlage für automatisierte Bereitstellungen

Nachdem eine Lösung entwickelt wurde, kann sie über eine Azure-Bereitstellungsvorlage für jede Azure-Region weltweit erfasst und bereitgestellt werden.  Dies ist nicht nur zum Ändern von Parametern für unterschiedliche Versionen dieses Workflows hilfreich, sondern auch zum Integrieren dieser Lösung in eine Build- und Releasepipeline.  Informationen zum Erstellen einer Bereitstellungsvorlage finden Sie in [diesem Artikel](logic-apps-create-deploy-template.md).

Azure Functions kann auch in die Bereitstellungsvorlage eingebunden werden, damit die gesamte Lösung mit allen Abhängigkeiten als einzelne Vorlage verwaltet werden kann.  Ein Beispiel für eine Bereitstellungsvorlage für Funktionen finden Sie im [Repository mit den Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).

## <a name="whats-next"></a>Nächste Schritte

* [Weitere Beispiele und Szenarien für Azure Logic Apps](logic-apps-examples-and-scenarios.md)
* [Video: Umfassende exemplarische Vorgehensweise zum Erstellen dieser Lösung](http://aka.ms/logicappsdemo)
* [Informationen zum Behandeln und Abfangen von Ausnahmen in einer Logik-App](logic-apps-exception-handling.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png
