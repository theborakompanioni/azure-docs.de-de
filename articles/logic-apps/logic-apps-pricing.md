---
title: "Preise und Abrechnung – Azure Logic Apps | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Preisdetails und Abrechnung für Azure Logic Apps funktionieren."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: klam
translationtype: Human Translation
ms.sourcegitcommit: 17b33d3296ffbc65a22bf9a5077282fffdb7b3de
ms.openlocfilehash: 4a0af9f3f9fab9ff459450eb8658273d4a1bc5dc

---
# <a name="logic-apps-pricing-model"></a>Preismodell für Logik-Apps
Mit Azure Logic Apps können Sie Integrationsworkflows in der Cloud skalieren und ausführen.  Weiter unten finden Sie Details zum Abrechnungsplan und zu den Tarifen für Logic Apps.
## <a name="consumption-pricing"></a>Nutzungspreise
Für neu erstellte Logik-Apps wird ein Nutzungsplan verwendet. Beim Preismodell, das auf der Nutzung von Logik-Apps basiert, ist die Bezahlung nutzungsabhängig.  Logik-Apps werden bei Verwendung eines Nutzungsplans nicht gedrosselt.
Alle in einer Ausführung einer Logik-App-Instanz ausgeführten Aktionen werden gemessen.
### <a name="what-are-action-executions"></a>Was sind Aktionsausführungen?
Jeder Schritt einer Logik-App-Definition ist eine Aktion. Beispiele hierfür sind Trigger, Ablaufsteuerungsschritte wie Bedingungen, Bereiche, For-Each-Schleifen und Do-Until-Schleifen, Aufrufe von Connectors und Aufrufe von nativen Aktionen.
Trigger sind spezielle Aktionen, die für das Instanziieren einer neuen Instanz einer Logik-App beim Auftreten eines bestimmten Ereignisses ausgelegt sind.  Es gibt eine Reihe von verschiedenen Verhalten für Trigger, die sich darauf auswirken können, wie die Logik-App gemessen wird.
* **Abfragetrigger**: Mit diesem Trigger wird ein Endpunkt fortlaufend abgefragt, bis er eine Nachricht empfängt, die die Kriterien für die Erstellung der Instanz einer Logik-App erfüllt.  Das Abrufintervall kann mit dem Designer für Logik-Apps im Trigger konfiguriert werden.  Jede Abfrageanforderung wird als Aktionsausführung gezählt. Dies gilt auch, wenn keine Instanz einer Logik-App erstellt wird.
* **Webhooktrigger** : Dieser Trigger wartet darauf, dass von einem Client eine Anforderung an einen bestimmten Endpunkt gesendet wird.  Jede Anforderung, die an den Webhookendpunkt gesendet wird, wird als eine Aktionsausführung gezählt. Sowohl beim Anforderungstrigger als auch beim HTTP-Webhooktrigger handelt es sich um Webhooktrigger.
* **Wiederholungstrigger**: Dieser Trigger erstellt eine Instanz der Logik-App basierend auf dem Wiederholungsintervall, das im Trigger konfiguriert ist.  Beispielsweise kann ein Wiederholungstrigger so konfiguriert werden, dass er alle drei Tage oder auch jede Minute ausgeführt wird.

Triggerausführungen werden auf dem Ressourcenblatt „Logik-Apps“ unter dem „Triggerverlauf“ angezeigt.

Alle Aktionen, die ausgeführt wurden – ob erfolgreich oder nicht – werden als Aktionsausführung gemessen.  Aktionen, die aufgrund einer nicht erfüllten Bedingung übersprungen wurden, oder Aktionen, die aufgrund einer Beendigung der Logik-App vor dem Abschluss nicht ausgeführt wurden, werden nicht als Aktionsausführungen gezählt.

Innerhalb von Schleifen ausgeführte Aktionen werden pro Iteration der Schleife gezählt.  Eine einzelne Aktion in einer For-Each-Schleife, die eine Liste mit zehn Einträgen durchläuft, wird wie folgt gezählt: Anzahl von Einträgen in der Liste (10) multipliziert mit der Anzahl von Aktionen in der Schleife (1) plus eine Aktion für die Initiierung der Schleife. Für dieses Beispiel ergeben sich also (10 · 1) + 1 = 11 Aktionsausführungen.
Für deaktivierte Logik-Apps können keine neuen Instanzen instanziiert werden. Daher werden sie für die Dauer der Deaktivierung auch nicht berechnet.  Beachten Sie, dass es nach dem Deaktivieren einer Logik-App etwas dauern kann, bis die Instanzen stillgelegt werden und vollständig deaktiviert sind.
### <a name="integration-account-usage"></a>Nutzung des Integrationskontos
Teil der verbrauchsbasierten Nutzung ist ein [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md) für Untersuchungs-, Entwicklungs- und Testzwecke, damit Sie die Features [B2B/EDI](logic-apps-enterprise-integration-b2b.md) und [XML-Verarbeitung](logic-apps-enterprise-integration-xml.md) von Logik-Apps ohne zusätzliche Kosten verwenden können. Sie können maximal ein Konto pro Region erstellen und bis zu zehn Vereinbarungen und 25 Zuordnungen speichern. Für Schemas, Zertifikate und Partner gelten keine Grenzwerte, und Sie können so viele hochladen, wie Sie benötigen.

Zusätzlich zur Einbindung von Integrationskonten in den Verbrauch können Sie auch Standardintegrationskonten ohne diese Grenzwerte und mit unserem Standard-SLA für Logik-Apps erstellen. Weitere Informationen finden Sie in der [Azure-Preisübersicht](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="app-service-plans"></a>App Service-Pläne
Zuvor erstellte Logik-Apps, für die auf einen App Service-Plan verwiesen wird, verhalten sich wie bisher. Je nach gewähltem Plan erfolgt die Drosselung nach dem Erreichen der vorgeschriebenen täglichen Ausführungen, aber die Abrechnung wird über die Messung der Aktionsausführungen durchgeführt.
EA-Kunden, die in ihrem Abonnement über einen App Service-Plan verfügen (der der Logik-App nicht explizit zugeordnet sein muss), erhalten den entsprechenden Mengenrabatt.  Wenn Sie beispielsweise in Ihrem EA-Abonnement über einen App Service-Standardplan und in demselben Abonnement über eine Logik-App verfügen, werden Ihnen nicht 10.000 Aktionsausführungen pro Tag berechnet (siehe folgende Tabelle). 

App Service-Pläne und ihre täglich zulässigen Aktionsausführungen:
|  | Free/Shared/Basic | Standard | Premium |
| --- | --- | --- | --- |
| Aktionsausführungen pro Tag |200 |10.000 |50.000 |
### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Umstellung von einem App Service-Plan auf den nutzungsbasierten Tarif
Um eine Logik-App mit zugeordnetem App Service-Plan in ein Verbrauchsmodell zu ändern, entfernen Sie den Verweis auf den App Service-Plan in der Logik-App-Definition.  Diese Änderung ist durch einen Aufruf eines PowerShell-Cmdlets möglich: `Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`
## <a name="pricing"></a>Preise
Eine Preisübersicht finden Sie unter [Logik-Apps Preise](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über Logik-Apps][whatis]
* [Erstellen Ihrer ersten Logik-App][create] [pricing]: https://azure.microsoft.com/pricing/details/logic-apps/ [whatis]: logic-apps-what-are-logic-apps.md [create]: logic-apps-create-a-logic-app.md




<!--HONumber=Feb17_HO2-->


