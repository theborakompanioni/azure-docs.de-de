---
title: REST-API für Log Analytics-Warnungen
description: Mit der REST-API für Log Analytics-Warnungen können Sie Warnungen in Operations Management Suite (OMS) erstellen und verwalten.  Dieser Artikel enthält die Details der API und mehrere Beispiele für verschiedene Vorgänge.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn

ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/18/2016
ms.author: bwren

---
# <a name="log-analytics-alert-rest-api"></a>REST-API für Log Analytics-Warnungen
Mit der REST-API für Log Analytics-Warnungen können Sie Warnungen in Operations Management Suite (OMS) erstellen und verwalten.  Dieser Artikel enthält die Details der API und mehrere Beispiele für verschiedene Vorgänge.

Die REST-API für die Log Analytics-Suche ist RESTful. Der Zugriff darauf erfolgt über die Azure Resource Manager-REST-API. In diesem Dokument finden Sie Beispiele, in denen über eine PowerShell-Befehlszeile per [ARMClient](https://github.com/projectkudu/ARMClient) auf die API zugegriffen wird. Dies ist ein Open-Source-Befehlszeilentool, mit dem das Aufrufen der Azure Resource Manager-API vereinfacht wird. Die Verwendung von ARMClient und PowerShell ist eine von vielen Möglichkeiten, auf die Protokollsuch-API von Log Analytics zuzugreifen. Mit diesen Tools können Sie die RESTful-API von Azure Resource Manager für Aufrufe an OMS-Arbeitsbereiche nutzen und darin Suchbefehle ausführen. Die API wird Ihnen Suchergebnisse im JSON-Format ausgeben, und Sie können die Suchergebnisse programmgesteuert auf viele verschiedene Arten verwenden.

## <a name="prerequisites"></a>Voraussetzungen
Derzeit können Warnungen nur mit einer gespeicherten Suche in Log Analytics erstellt werden.  Weitere Informationen finden Sie unter [REST-API für die Log Analytics-Suche](log-analytics-log-search-api.md) .

## <a name="schedules"></a>Zeitpläne
Eine gespeicherte Suche kann über einen oder mehrere Zeitpläne verfügen. Der Zeitplan definiert, wie oft die Suche durchgeführt wird und welches Zeitintervall für die Identifizierung der Kriterien verwendet wird.
Die Zeitplaneigenschaften sind in der folgenden Tabelle aufgeführt.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Intervall |Wie oft die Suche durchgeführt wird. Dieser Wert wird in Minuten gemessen. |
| QueryTimeSpan |Das Zeitintervall, in dem die Kriterien ausgewertet werden. Der Wert muss größer oder gleich dem Intervall sein. Dieser Wert wird in Minuten gemessen. |
| Version |Die verwendete API-Version.  Dieser Wert sollte derzeit immer 1 lauten. |

Stellen Sie sich beispielsweise eine Ereignisabfrage mit einem Intervall von 15 Minuten und einer Zeitspanne von 30 Minuten vor. In diesem Fall würde die Abfrage alle 15 Minuten ausgeführt werden, und es würde eine Warnung ausgelöst werden, wenn sich für die Kriterien weiterhin 30 Minuten lang die Auflösung „true“ ergibt.

### <a name="retrieving-schedules"></a>Abrufen von Zeitplänen
Verwenden Sie die Get-Methode, um alle Zeitpläne für eine gespeicherte Suche abzurufen.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Verwenden Sie die Get-Methode mit einer Zeitplan-ID, um einen bestimmten Zeitplan für eine gespeicherte Suche abzurufen.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Im Folgenden finden Sie eine Beispielantwort für einen Zeitplan.

    {
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
        "Interval": 15,
        "QueryTimeSpan": 15
    }

### <a name="creating-a-schedule"></a>Erstellen eines Zeitplans
Verwenden Sie die Put-Methode mit der eindeutigen Zeitplan-ID, um einen neuen Zeitplan zu erstellen.  Beachten Sie, dass zwei Zeitpläne dieselbe ID haben können, auch wenn sie unterschiedlichen gespeicherten Suchvorgängen zugeordnet sind.  Wenn Sie einen Zeitplan in der OMS-Konsole erstellen, wird für die Zeitplan-ID eine GUID erstellt.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Bearbeiten eines Zeitplans
Verwenden Sie die Put-Methode mit einer vorhandenen Zeitplan-ID für die gleiche gespeicherte Suche, um diesen Zeitplan zu bearbeiten.  Der Hauptteil der Anforderung muss das ETag des Zeitplans enthalten.

    $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Löschen von Zeitplänen
Verwenden Sie die Delete-Methode mit einer Zeitplan-ID, um einen Zeitplan zu löschen.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Actions
Ein Zeitplan kann mehrere Aktionen umfassen. Mit einer Aktion können ein oder mehrere durchzuführende Prozesse definiert werden, z.B. das Senden einer E-Mail oder das Starten eines Runbooks. Es kann auch ein Schwellenwert definiert werden, der bestimmt, wann die Ergebnisse einer Suche eine Übereinstimmung mit Kriterien ergeben.  Mit einigen Aktionen wird beides definiert, sodass die Prozesse durchgeführt werden, wenn der Schwellenwert erreicht ist.

Die Eigenschaften aller Aktionen sind in der folgenden Tabelle aufgeführt.  Unterschiedliche Arten von Warnungen verfügen über unterschiedliche Eigenschaften, die unten beschrieben sind.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Typ |Der Typ der Aktion.  Derzeit sind die möglichen Werte „Warnung“ und „Webhook“. |
| Name |Der Anzeigename für die Warnung. |
| Version |Die verwendete API-Version.  Dieser Wert sollte derzeit immer 1 lauten. |

### <a name="retrieving-actions"></a>Abrufen von Aktionen
Verwenden Sie die Get-Methode, um alle Aktionen für einen Zeitplan abzurufen.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Verwenden Sie die Get-Methode mit der Aktions-ID, um eine bestimmte Aktion für einen Zeitplan abzurufen.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Erstellen oder Bearbeiten von Aktionen
Verwenden Sie die Put-Methode mit einer Aktions-ID, die für den Zeitplan eindeutig ist, um eine neue Aktion zu erstellen.  Wenn Sie eine Aktivität in der OMS-Konsole erstellen, wird eine GUID für die Aktions-ID erstellt.

Verwenden Sie die Put-Methode mit einer vorhandenen Aktions-ID für die gleiche gespeicherte Suche, um diesen Zeitplan zu bearbeiten.  Der Hauptteil der Anforderung muss das ETag des Zeitplans enthalten.

Das Anforderungsformat zum Erstellen einer neuen Aktion variiert je nach Aktivitätstyp. Beispiele hierzu finden Sie in den Abschnitten unten.

### <a name="deleting-actions"></a>Löschen von Aktionen
Verwenden Sie die Delete-Methode mit der Aktions-ID, um eine Aktion zu löschen.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Warnungsaktionen
Ein Zeitplan sollte nur über genau eine Warnungsaktion verfügen.  Warnungsaktionen weisen einen oder mehrere Abschnitte auf, die in der folgenden Tabelle aufgeführt sind.  Jeder Abschnitt wird unten ausführlicher beschrieben.

| Abschnitt | Beschreibung |
|:--- |:--- |
| Schwellenwert |Kriterien für den Zeitpunkt der Ausführung einer Aktion. |
| EmailNotification |Senden von E-Mails an mehrere Empfänger. |
| Korrektur |Starten Sie ein Runbook in Azure Automation, um zu versuchen, das identifizierte Problem zu beheben. |

#### <a name="thresholds"></a>Schwellenwerte
Eine Warnungsaktion sollte nur über genau einen Schwellenwert verfügen.  Wenn die Ergebnisse einer gespeicherten Suche mit dem Schwellenwert in einer Aktion übereinstimmen, die der Suche zugeordnet ist, werden alle anderen Prozesse in dieser Aktion ausgeführt.  Eine Aktion kann einen Schwellenwert auch nur zu dem Zweck enthalten, dass sie mit Aktionen eines anderen Typs verwendet werden kann, die keine Schwellenwerte aufweisen.

Die Eigenschaften von Schwellenwerten sind in der folgenden Tabelle aufgeführt.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Operator |Operator für den Schwellenwertvergleich. <br> gt = Greater Than (Größer als) <br>  lt = Less Than (Kleiner als) |
| Wert |Der Wert für den Schwellenwert. |

Stellen Sie sich beispielsweise eine Ereignisabfrage mit einem Intervall von 15 Minuten, einer Zeitspanne von 30 Minuten und einem Schwellenwert größer als 10 vor. In diesem Fall wird die Abfrage alle 15 Minuten ausgeführt. Eine Warnung wird ausgelöst, wenn 10 Ereignisse zurückgegeben werden, die über einen Zeitraum von 30 Minuten erstellt wurden.

Unten ist eine Beispielantwort für eine Aktion angegeben, die nur über einen Schwellenwert verfügt.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Verwenden Sie die Put-Methode mit einer eindeutigen Aktions-ID, um eine neue Schwellenwertaktion für einen Zeitplan zu erstellen.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Verwenden Sie die Put-Methode mit einer vorhandenen Aktions-ID, um eine Schwellenwertaktion für einen Zeitplan zu ändern.  Der Hauptteil der Anforderung muss das ETag der Aktion enthalten.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>E-Mail-Benachrichtigung
Per E-Mail-Benachrichtigung werden E-Mails an einen oder mehrere Empfänger gesendet.  Sie enthalten die Eigenschaften, die in der folgenden Tabelle angegeben sind.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Recipients |Liste mit den E-Mail-Adressen |
| Betreff |E-Mail-Betreff |
| Anhang |Anlagen werden derzeit nicht unterstützt. Der Wert lautet also immer „Keine“. |

Unten ist eine Beispielantwort für eine E-Mail-Benachrichtigungsaktion mit einem Schwellenwert angegeben.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Verwenden Sie die Put-Methode mit einer eindeutigen Aktions-ID, um eine neue E-Mail-Aktion für einen Zeitplan zu erstellen.  Im folgenden Beispiel wird eine E-Mail-Benachrichtigung mit einem Schwellenwert erstellt, damit die E-Mail gesendet wird, wenn die Ergebnisse der gespeicherten Suche den Schwellenwert überschreiten.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

Verwenden Sie die Put-Methode mit einer vorhandenen Aktions-ID, um eine E-Mail-Aktion für einen Zeitplan zu ändern.  Der Hauptteil der Anforderung muss das ETag der Aktion enthalten.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

#### <a name="remediation-actions"></a>Aktionen zur Problembehebung
Bei Behebungen wird ein Runbook in Azure Automation gestartet, mit dem versucht wird, das von der Warnung identifizierte Problem zu korrigieren.  Sie müssen einen Webhook für das Runbook erstellen, das in der Aktion für die Problembehebung verwendet wird, und anschließend den URI in der WebhookUri-Eigenschaft angeben.  Wenn Sie diese Aktion mit der OMS-Konsole erstellen, wird automatisch ein neuer Webhook für das Runbook erstellt.

Behebungen enthalten die Eigenschaften, die in der folgenden Tabelle angegeben sind.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| RunbookName |Der Name des Runbooks. Er muss mit einem veröffentlichten Runbook im Automation-Konto übereinstimmen, das in der Automation-Lösung in Ihrem OMS-Arbeitsbereich konfiguriert ist. |
| WebhookUri |Der URI des Webhooks. |
| Expiry |Das Ablaufdatum und die Uhrzeit des Webhooks.  Wenn der Webhook kein Ablaufdatum enthält, kann dies jedes gültige Datum in der Zukunft sein. |

Unten ist eine Beispielantwort für eine Aktion zur Problembehebung mit einem Schwellenwert angegeben.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Verwenden Sie die Put-Methode mit einer eindeutigen Aktions-ID, um eine neue Aktion zur Problembehandlung für einen Zeitplan zu erstellen.  Im folgenden Beispiel wird eine Problembehebung mit einem Schwellenwert erstellt, damit das Runbook gestartet wird, wenn die Ergebnisse der gespeicherten Suche den Schwellenwert überschreiten.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Verwenden Sie die Put-Methode mit einer vorhandenen Aktions-ID, um eine Aktion zur Problembehandlung für einen Zeitplan zu ändern.  Der Hauptteil der Anforderung muss das ETag der Aktion enthalten.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Beispiel
Unten ist ein vollständiges Beispiel zur Erstellung einer neuen E-Mail-Benachrichtigung angegeben.  Es werden ein neuer Zeitplan und eine Aktion mit einem Schwellenwert und einer E-Mail erstellt.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $workspaceId    = "MyWorkspace"
    $searchId       = "51cf0bd9-5c74-6bcb-927e-d1e9080b934e"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule?api-version=2015-03-20 $scheduleJson

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/mythreshold?api-version=2015-03-20 $thresholdJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/myemailaction?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Webhookaktionen
Bei Webhookaktionen wird ein Prozess gestartet, indem eine URL aufgerufen und optional eine zu sendende Nutzlast bereitgestellt wird.  Diese Aktionen sind mit Aktionen zur Problembehebung vergleichbar. Sie sind aber für Webhooks bestimmt, mit denen andere Prozesse als Azure Automation-Runbooks aufgerufen werden können.  Außerdem verfügen sie über die zusätzliche Option zum Angeben einer Nutzlast, die für den Remoteprozess bereitgestellt wird.

Webhookaktionen verfügen nicht über einen Schwellenwert, sondern sollten stattdessen einem Zeitplan hinzugefügt werden, der über eine Warnungsaktion mit Schwellenwert verfügt.  Sie können mehrere Webhookaktionen hinzufügen, die alle ausgeführt werden, wenn der Schwellenwert erreicht wird.

Webhookaktionen enthalten die Eigenschaften, die in der folgenden Tabelle angegeben sind.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| WebhookUri |E-Mail-Betreff |
| CustomPayload |Benutzerdefinierte Nutzlast, die an den Webhook gesendet wird.  Das Format hängt davon ab, was vom Webhook erwartet wird. |

Unten sind eine Beispielantwort für eine Webhookaktion und eine zugeordnete Warnungsaktion mit einem Schwellenwert angegeben.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Erstellen oder Bearbeiten einer Webhookaktion
Verwenden Sie die Put-Methode mit einer eindeutigen Aktions-ID, um eine neue Webhookaktion für einen Zeitplan zu erstellen.  Im folgenden Beispiel werden eine Webhookaktion und eine Warnungsaktion mit einem Schwellenwert erstellt, damit der Webhook ausgelöst wird, wenn die Ergebnisse der gespeicherten Suche den Schwellenwert überschreiten.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Verwenden Sie die Put-Methode mit einer vorhandenen Aktions-ID, um eine Webhookaktion für einen Zeitplan zu ändern.  Der Hauptteil der Anforderung muss das ETag der Aktion enthalten.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie die [REST-API zum Durchführen von Protokollsuchen](log-analytics-log-search-api.md) in Log Analytics.

<!--HONumber=Oct16_HO2-->


