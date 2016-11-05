---
title: Erstellen einer API für Logik-Apps
description: Erstellen einer benutzerdefinierten API zur Verwendung mit Logik-Apps
author: jeffhollan
manager: dwrede
editor: ''
services: logic-apps
documentationcenter: ''

ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2016
ms.author: jehollan

---
# Erstellen einer benutzerdefinierten API zur Verwendung mit Logik-Apps
Wenn Sie die Logik-Apps-Plattform erweitern möchten, gibt es viele Möglichkeiten, Aufrufe in APIs oder Systemen durchzuführen, die nicht im Rahmen unserer zahlreichen sofort einsetzbaren Connectors zur Verfügung stehen. Eine dieser Möglichkeiten ist die Erstellung einer API-App, die von einem Logik-App-Workflow aus aufgerufen werden kann.

## Nützliche Tools
Damit APIs optimal mit Logik-Apps funktionieren, empfehlen wir, ein [Swagger](http://swagger.io)-Dokument zu generieren, in dem die unterstützten Vorgänge und Parameter für Ihre API im Detail aufgeführt sind. Es gibt viele Bibliotheken (z. B. [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)), die das Swagger-Dokument automatisch für Sie generieren. Mit [TRex](https://github.com/nihaue/TRex) können Sie das Swagger-Dokument auch mit Anmerkungen versehen, damit es mit Logik-Apps (Anzeigenamen, Eigenschaftstypen usw.) gut eingesetzt werden kann. Für einige Beispiele von für Logik-Apps erstellte API-Apps sehen Sie sich unbedingt unser [GitHub-Repository](http://github.com/logicappsio) oder den [Blog](http://aka.ms/logicappsblog) an.

## Aktionen
Die grundlegende Aktion für eine Logik-App ist ein Controller, der eine HTTP-Anforderung akzeptiert und eine Antwort (in der Regel 200) zurückgibt. Es gibt jedoch verschiedene Muster, die Sie befolgen können, um Aktionen Ihren Anforderungen entsprechend zu erweitern.

Standardmäßig tritt für eine Anforderung nach einer Minute ein Timeout durch die Logik-App ein. Allerdings können Sie Ihre API auch längerfristige Aktionen ausführen und das Modul auf deren Abschluss warten lassen, indem Sie entweder einem Async- oder einem Webhookmuster folgen, wie unten beschrieben.

Schreiben Sie für Standardaktionen einfach eine HTTP-Anforderungsmethode in Ihrer API, die über Swagger verfügbar gemacht wird. Beispiele für API-Apps, die mit Logic-Apps arbeiten, finden Sie in unserem [GitHub-Repository](https://github.com/logicappsio). Nachfolgend finden Sie Möglichkeiten, um mit einem benutzerdefinierten Connector allgemeine Muster zu erreichen.

### Aktionen mit langer Ausführungszeit – Async-Muster
Beim Ausführen eines Schritts oder einer Aufgabe mit langer Ausführungszeit müssen Sie als Erstes dem Modul angeben, dass kein Timeout aufgetreten ist. Sie müssen dem Modul außerdem mitteilen, wie es feststellen kann, wann die Aufgabe beendet ist. Zuletzt müssen Sie relevante Daten an das Modul zurückgeben, damit der Workflow fortgesetzt werden kann. Dies kann über eine API umgesetzt werden, indem Sie den unten aufgeführten Fluss befolgen. Diese Schritte werden aus der Sicht der benutzerdefinierten API beschrieben:

1. Wenn eine Anforderung empfangen wird, sofort eine Antwort zurückgeben (bevor die Arbeit abgeschlossen ist). Diese Antwort ist eine `202 ACCEPTED`-Antwort, um dem Modul mitzuteilen, dass die Daten erhalten wurden, die Nutzlast angenommen wurde und jetzt die Verarbeitung stattfindet. Die 202-Antwort sollte die folgenden Header enthalten:
   
   * `location`-Header (erforderlich): Dies ist ein absoluter Pfad zu der URL, die von Logik-Apps zum Überprüfen des Status des Auftrags verwendet werden kann.
   * `retry-after` (optional, Standardwert für Aktionen lautet 20). Dies ist die Anzahl der Sekunden, die das Modul warten soll, bevor die Adressheader-URL abgerufen wird, um den Status zu überprüfen.
2. Bei der Überprüfung des Auftragsstatus folgende Prüfungen durchführen:
   
   * Wenn der Auftrag abgeschlossen ist: eine `200 OK`-Antwort mit der Antwortnutzlast zurückgeben.
   * Wenn der Auftrag noch immer verarbeitet wird: Eine weitere `202 ACCEPTED`-Antwort mit den gleichen Headern wie in der ersten Antwort zurückgeben.

Anhand dieses Musters können Sie Aufgaben mit extrem langer Ausführung innerhalb eines Threads Ihrer benutzerdefinierten API ausführen, dabei aber eine aktive Verbindung mit dem Logik-Apps-Modul aufrechterhalten, sodass kein Timeout erfolgt oder der Vorgang fortgesetzt wird, bevor die Arbeit abgeschlossen ist. Wenn Sie Ihrer Logik-App diese Funktionalität hinzufügen, sollten Sie unbedingt beachten, dass Sie an Ihrer Definition keine Änderungen vornehmen müssen, damit die Logik-App weiterhin den Status abruft und überprüft. Sobald das Modul eine 202 ACCEPTED-Antwort mit einem gültigen Adressheader erhält, berücksichtigt es das Async-Muster und ruft weiterhin den Adressheader ab, bis eine andere Antwort als 202 zurückgegeben wird.

Ein Beispiel für dieses Muster finden Sie [hier](https://github.com/jeffhollan/LogicAppsAsyncResponseSample) in GitHub.

### Webhookaktionen
Während des Workflows können Sie die Logik-App anhalten und auf einen Rückruf warten lassen, bevor der Vorgang fortgesetzt wird. Dieser Rückruf wird als HTTP POST gesendet. Um dieses Muster zu implementieren, müssen Sie zwei Endpunkte auf dem Controller angeben: „subscribe“ und „unsubscribe“.

Bei „subscribe“ erstellt und registriert die Logik-App eine Rückruf-URL, die von der API gespeichert und mit dem Status „Ready“ als HTTP POST zurückgerufen werden kann. Inhalte/Header werden an die Logik-App übergeben und können im übrigen Workflow verwendet werden. Das Logik-App-Modul ruft bei Ausführung den Endpunkt „subscribe“ auf, sobald dieser Schritt erreicht wird.

Wenn die Ausführung abgebrochen wurde, ruft das Logik-App-Modul den Endpunkt „unsubscribe“ auf. Ihre API kann dann die Registrierung der Rückruf-URL nach Bedarf aufheben.

Derzeit wird das Ermitteln eines Webhookendpunkts über Swagger nicht vom Logik-App-Designer unterstützt. Um diese Art von Aktion zu verwenden, müssen Sie daher die Aktion „Webhook“ hinzufügen und die URL, die Header und den Text Ihrer Anforderung angeben. Sie können in jedem dieser Felder nach Bedarf die `@listCallbackUrl()`-Workflowfunktion verwenden, um die Rückruf-URL zu übergeben.

Ein Beispiel für dieses Webhookmuster finden Sie [hier](https://github.com/jeffhollan/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs) in GitHub.

## Trigger
Zusätzlich zu Aktionen können Sie Ihre benutzerdefinierte API als Trigger für eine Logik-App einsetzen. Es gibt die zwei folgenden Muster, die Sie befolgen können, um eine Logik-App auszulösen:

### Abfragetrigger
Abfragetrigger verhalten sich ähnlich wie die oben beschriebenen Async-Aktionen mit langer Ausführung. Das Logik-App-Modul ruft den Triggerendpunkt nach Ablauf einer bestimmten Zeitspanne ab (SKU-abhängig: 15 Sekunden für Premium, 1 Minute für Standard und 1 Stunde für Free).

Wenn keine Daten vorhanden sind, gibt der Trigger eine `202 ACCEPTED`-Antwort mit einem `location`- und einem `retry-after`-Header zurück. Allerdings empfiehlt es sich für Trigger, dass der `location`-Header einen Abfrageparameter `triggerState` enthält. Anhand dieses Bezeichners erkennt Ihre API, wann die Logik-App zuletzt ausgelöst wurde. Wenn Daten vorhanden sind, gibt der Trigger eine `200 OK`-Antwort mit der Inhaltsnutzlast zurück. Dadurch wird die Logik-App ausgelöst.

Wenn Sie beispielsweise einen Abruf durchführen, um die Verfügbarkeit einer Datei festzustellen, könnten Sie einen Abfragetrigger erstellen, der folgende Schritte durchführt:

* Wenn eine Anforderung ohne triggerState empfangen wird, gibt die API `202 ACCEPTED` mit einem `location`-Header zurück, der einen triggerState mit der aktuellen Uhrzeit und einen `retry-after`-Wert von 15 enthält.
* Wenn eine Anforderung mit einem triggerState empfangen wird:
  * Es wird überprüft, ob nach dem DateTime-Wert von triggerState Dateien hinzugefügt wurden.
  * Wenn 1 Datei hinzugefügt wurde, wird eine `200 OK`-Antwort mit der Inhaltsnutzlast zurückgegeben, triggerState wird auf den DateTime-Wert der zurückgegebenen Datei erhöht, und `retry-after` wird auf 15 festgelegt.
  * Wenn mehrere Dateien hinzugefügt wurden, kann eine nach der anderen mit `200 OK` zurückgegeben werden. triggerState wird im `location`-Header erhöht, und `retry-after` wird auf 0 festgelegt. Auf diese Weise wird dem Modul mitgeteilt, dass weitere Daten vorhanden sind, die sofort am angegebenen `location`-Header angefordert werden.
  * Wenn keine Dateien verfügbar sind, wird eine `202 ACCEPTED`-Antwort zurückgegeben, und der `location`-triggerState bleibt unverändert. Legen Sie `retry-after` auf 15 fest.

Ein Beispiel für einen Abfragetrigger finden Sie [hier](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers) in GitHub.

### Webhooktrigger
Webhooktrigger verhalten sich ähnlich wie die oben beschriebenen Webhookaktionen. Das Logik-App-Modul ruft den Endpunkt „subscribe“ jedes Mal auf, wenn ein Webhooktrigger hinzugefügt und gespeichert wird. Ihre API kann die Webhook-URL registrieren und über HTTP POST aufrufen, sobald Daten verfügbar sind. Die Inhaltsnutzlast und Header werden der Logik-App-Ausführung übergeben.

Falls ein Webhooktrigger gelöscht wird (entweder die gesamte Logik-App oder nur der Webhooktrigger), ruft das Modul die URL „unsubscribe“ auf, über die Ihre API die Registrierung der Rückruf-URL aufheben und nach Bedarf alle Prozesse beenden kann.

Derzeit wird das Ermitteln eines Webhooktriggers über Swagger nicht vom Logik-App-Designer unterstützt. Um diese Art von Aktion zu verwenden, müssen Sie daher den Trigger „Webhook“ hinzufügen und die URL, die Header und den Text Ihrer Anforderung angeben. Sie können in jedem dieser Felder nach Bedarf die `@listCallbackUrl()`-Workflowfunktion verwenden, um die Rückruf-URL zu übergeben.

Ein Beispiel für einen Webhooktrigger finden Sie [hier](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers) in GitHub.

<!---HONumber=AcomDC_0803_2016-->