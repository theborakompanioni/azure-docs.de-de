---
title: Anfordern von technischem Support vom Application Insights-Entwicklungsteam | Microsoft Docs
description: Wenn Sie einen Fall haben, der die besondere Unterstützung durch das Application Insights-Entwicklungsteam erfordert, erfahren Sie hier, wie Sie die Details übermitteln können, um Support zu erhalten.
services: application-insights
documentationcenter: ''
author: alexbulankou
manager: douge

ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/01/2016
ms.author: albulank

---
# Anfordern von technischem Support vom Application Insights-Entwicklungsteam
Wenn Sie bei der Nutzung von [Visual Studio Application Insights](app-insights-overview.md) technische Probleme haben sollten, finden Sie hier Optionen, wie Sie Hilfe erhalten können:

## 1\. Die Dokumente überprüfen
* Fehlen Daten? Überprüfen Sie Folgendes: [Stichprobennahme](app-insights-sampling.md), [Kontingente und Drosselung](app-insights-pricing.md).
* Problembehandlung: [ASP.NET](app-insights-troubleshoot-faq.md) | [Java](app-insights-java-troubleshoot.md)

## 2\. Die Foren durchsuchen
* [MSDN-Forum](https://social.msdn.microsoft.com/Forums/vstudio/home?forum=ApplicationInsights)
* [StackOverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)

## 3\. Azure-Supportplan?
Es gibt Situationen, in denen Sie möchten, dass sich Entwickler Ihren speziellen Fall ansehen.

Wenn Sie einen [Supportplan für Microsoft Azure](https://azure.microsoft.com/support/plans/) haben, können Sie [ein Supportticket öffnen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## 4\. Das Application Insights-Team kontaktieren
Wenn Sie keinen Supportplan haben, wird sich unser Supportteam nach besten Kräften bemühen, für Application Insights-Kunden Support zu leisten, während wir uns auf den Meilenstein „Allgemeine Verfügbarkeit“ (General Availability, GA) vorbereiten. Wir bieten eine **neue Supportoption**: Sie können uns Ihren Fall beschreiben, indem Sie uns im Azure-Portal ein Feedbackformular senden, woraufhin sich ein Entwickler aus dem Application Insights-Team bei Ihnen meldet, um Ihnen bei der Lösung Ihres Problems zu helfen.

1. Klicken Sie im [Application Insights-Portal](https://portal.azure.com) rechts oben auf das Smileysymbol:  
   
    ![Schaltfläche „Feedback“](./media/app-insights-get-dev-support/01.png)
2. Geben Sie im Kommentarfeld **AppInsights** in die erste Zeile ein, und fügen Sie dann die folgenden Informationen hinzu:
   
    ```
   
    AppInsights   
    ikey: <instrumentation key>   
    sdk: <SDK that you are using, including name and version>  
    issue: <please describe the problem you are having>
   
    ```   
   
    ![Dialogfeld „Feedback“](./media/app-insights-get-dev-support/02.png)
3. Aktivieren Sie „Ja, Sie dürfen mich bezüglich meines Feedbacks per E-Mail kontaktieren“.
   
    ![Abschnitt „Senden“](./media/app-insights-get-dev-support/03.png)

Ein Entwickler aus dem Application Insights-Team wird sich bei Ihnen melden. Da wir diesen Service nach besten Kräften bieten, gilt derzeit keine formelle SLA.

<!---HONumber=AcomDC_0615_2016-->