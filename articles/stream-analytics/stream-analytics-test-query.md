---
title: Testen von Azure Stream Analytics-Abfragen | Microsoft-Dokumentation
description: "Informationen zum Testen von Abfragen in Stream Analytics-Aufträgen."
keywords: Testen von Abfragen, Problembehandlung bei Abfragen
documentation center: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 9ca96196d2d4b898c79556867b5ff8d8be0f66dc
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017


---
# <a name="test-azure-stream-analytics-queries-in-the-azure-portal"></a>Testen von Azure Stream Analytics-Abfragen im Azure-Portal

Mit Azure Stream Analytics können Sie Abfragen im Azure-Portal testen, ohne einen Auftrag starten oder beenden zu müssen.

## <a name="test-the-input"></a>Testen der Eingabe

1. Um die Beispieleingabedaten zu testen, klicken Sie mit der rechten Maustaste auf eine Ihrer Eingaben und wählen dann **Beispieldaten aus Datei hochladen** aus.

    ![Testen von Abfragen im Stream Analytics-Abfrage-Editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. Nachdem das Hochladen abgeschlossen ist, klicken Sie auf **Testen**, um diese Abfrage mit den Beispieldaten zu testen, die Sie bereitgestellt haben.

    ![Testen von Beispieldaten im Stream Analytics-Abfrage-Editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

Wenn Sie die Testausgabe zur späteren Verwendung speichern möchten, wird die Ausgabe der Abfrage im Browser mit einem Link zu den Downloadergebnissen angezeigt. Sie können nun Ihre Abfrage einfach und wiederholt ändern und testen, um festzustellen, wie sich die Ausgabe ändert.

![Beispielausgabe im Stream Analytics-Abfrage-Editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

Wenn in einer Abfrage mehrere Ausgaben verwendet werden, können Sie die Ergebnisse jeder Ausgabe separat anzeigen und mühelos dazwischen umschalten.

Sobald Sie mit den im Browser angezeigten Ergebnissen zufrieden sind, können Sie Ihre Abfrage speichern, Ihren Auftrag starten und ihn Ereignisse fehlerfrei verarbeiten lassen.

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

