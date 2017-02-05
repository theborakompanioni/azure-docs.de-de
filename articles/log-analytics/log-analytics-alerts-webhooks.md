---
title: "Beispiel für einen Webhook für eine Log Analytics-Warnung"
description: "Eine der Aktionen, die Sie als Antwort auf eine Log Analytics-Warnung ausführen können, ist ein *Webhook*. Damit können Sie einen externen Prozess über eine einzelne HTTP-Anforderung aufrufen. In diesem Artikel finden Sie ein Beispiel für das Erstellen einer Webhookaktion in einer Log Analytics-Warnung mit Slack."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 13c39f0f-fd3c-472d-8324-ddf7538be45e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/27/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 001f8abfc5862aeb1914f6e93b68cadeed42eba0


---
# <a name="webhooks-in-log-analytics-alerts"></a>Webhooks in Log Analytics-Warnungen
Eine der Aktionen, die Sie als Antwort auf eine [Log Analytics-Warnung](log-analytics-alerts.md) ausführen können, ist ein *Webhook*. Damit können Sie einen externen Prozess über eine einzelne HTTP-Anforderung aufrufen.  Einzelheiten zu Warnungen und Webhooks finden Sie in [Warnungen in Log Analytics](log-analytics-alerts.md).

In diesem Artikel bearbeiten wir ein Beispiel für das Erstellen einer Webhookaktion in einer Log Analytics-Warnung mit dem Messaging-Dienst Slack.

> [!NOTE]
> Sie benötigen für dieses Beispiel ein Slack-Konto.  Sie können sich unter [slack.com](http://slack.com)für ein kostenloses Konto anmelden.
> 
> 

## <a name="step-1---enable-webhooks-in-slack"></a>Schritt 1: Aktivieren der Webhooks in Slack
1. Melden Sie sich bei Slack unter [slack.com](http://slack.com)an.
2. Wählen Sie im Abschnitt **Channels** im linken Bereich einen Kanal aus.  Dies ist der Kanal, an den die Nachricht gesendet wird.  Sie können einen der Standardkanäle wie **general** oder **random** auswählen.  In einem Produktionsszenario würden Sie wahrscheinlich einen speziellen Kanal wie **wichtigedienstwarnungen**erstellen. <br>
   
   ![Slack-Kanäle](media/log-analytics-alerts-webhooks/oms-webhooks01.png)
3. Klicken Sie auf **Add an app or custom integration** (App oder benutzerdefinierte Integration hinzufügen), um das App-Verzeichnis zu öffnen.
4. Geben Sie *Webhooks* in das Suchfeld ein, und wählen Sie dann **Incoming WebHooks**(Eingehende Webhooks) aus. <br>
   
   ![Slack-Kanäle](media/log-analytics-alerts-webhooks/oms-webhooks02.png)
5. Klicken Sie neben dem Namen Ihres Teams auf **Installieren** .
6. Klicken Sie auf **Konfiguration hinzufügen**.
7. Wählen Sie den Kanal aus, den Sie für dieses Beispiel verwenden möchten, und klicken Sie dann auf **Add Incoming WebHooks integration**(Integration eingehender WebHooks hinzufügen).  
8. Kopieren Sie die **Webhook-URL**.  Sie müssen diese in die Warnungskonfiguration einfügen. <br>
   
    ![Slack-Kanäle](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Schritt 2: Erstellen einer Warnungsregel in Log Analytics
1. [Erstellen Sie eine Warnungsregel](log-analytics-alerts.md) mit den folgenden Einstellungen.
   * Abfrage: ```    Type=Event EventLevelName=error ```
   * Diese Warnung überprüfen alle: 5 Minuten
   * Die Anzahl der Ergebnisse ist: größer als 10
   * Über dieses Zeitfenster: 60 Minuten
   * Wählen Sie **Ja** für **Webhook** und **Nein** für andere Aktionen aus.
2. Fügen Sie die Slack-URL in das Feld **Webhook-URL** ein.
3. Wählen Sie die Option zum **Einfügen einer benutzerdefinierten JSON-Nutzlast**aus.
4. Slack erwartet eine Nutzlast, die in JSON formatiert ist und einen Parameter mit dem Namen *text*enthält.  Dies ist der Text, der in der erstellten Nachricht angezeigt wird.  Sie können einen oder mehrere der Warnungsparameter mithilfe des Symbols *#* verwenden, wie im folgenden Beispiel veranschaulicht.
   
    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```
   
    ![Beispiel-JSON-Nutzlast](media/log-analytics-alerts-webhooks/oms-webhooks07.png)
5. Klicken Sie auf **Speichern** , um die Warnungsregel zu speichern.
6. Warten Sie ausreichend lange, bis eine Warnung erstellt wurde, und überprüfen Sie dann Slack auf eine Nachricht, die etwa wie folgt aussieht.
   
   ![Beispielwebhook in Slack](media/log-analytics-alerts-webhooks/oms-webhooks08.png)

### <a name="advanced-webhook-payload-for-slack"></a>Erweiterte Webhook-Nutzlast für Slack
Sie können die eingehenden Nachrichten in Slack umfassend anpassen. Weitere Informationen finden Sie unter [Incoming Webhooks](https://api.slack.com/incoming-webhooks) (Eingehende Webhooks) auf der Slack-Website. Im Folgenden finden Sie eine komplexere Nutzlast, mit der eine umfangreiche Nachricht mit Formatierung erstellt wird:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


Damit würde in Slack eine Nachricht erstellt werden, die der folgenden ähnelt.

![Beispielnachricht in Slack](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Zusammenfassung
Mit dieser Warnungsregel wird jedes Mal eine Nachricht an Slack gesendet, wenn die Kriterien erfüllt sind.  

Dies ist nur ein Beispiel für eine Aktion, die Sie als Reaktion auf eine Warnung erstellen können.  Sie könnten beispielsweise eine Webhookaktion zum Aufrufen eines anderen externen Diensts, eine Runbook-Aktion zum Starten eines Runbooks in Azure Automation oder eine E-Mail-Aktion zum Senden einer E-Mail an Sie oder andere Empfänger erstellen.   

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Warnungen in Log Analytics](log-analytics-alerts.md) einschließlich weiterer Aktionen.
* [Erstellen von Runbooks in Azure Automation](../automation/automation-webhooks.md) die aus einem Webhook aufgerufen werden können.




<!--HONumber=Nov16_HO3-->


