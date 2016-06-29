<properties
	pageTitle="Azure Mobile Engagement Web SDK-APIs| Microsoft Azure"
	description="Neueste Updates und Verfahren für das Web SDK für Azure Mobile Engagement"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="erikre"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="web"
	ms.devlang="js"
	ms.topic="article"
	ms.date="06/07/2016"
	ms.author="piyushjo" />

# Verwenden der Mobile Engagement-API in einer Webanwendung

Dieses Dokument ist eine Ergänzung zum Dokument [Integrieren von Mobile Engagement in eine Webanwendung](mobile-engagement-web-integrate-engagement.md). Es bietet tiefergehende Details zur Verwendung der Engagement-API, um Ihre Anwendungsstatistik zu melden.

Die Mobile Engagement-API wird vom `engagement.agent`-Objekt bereitgestellt. `engagement` ist der Standardalias des Engagement SDK, der in der SDK-Konfiguration neu definiert werden kann.

## Engagement-Konzepte

In den folgenden Abschnitten werden die [Mobile Engagement-Konzepte](mobile-engagement-concepts.md) für die Webplattform genauer dargestellt.

### `Session` und `Activity`

Wenn sich der Benutzer zwischen zwei *Aktivitäten* mehr als zwei Sekunden im Leerlauf befindet, dann wird diese Folge von *Aktivitäten* in zwei einzelne *Sitzungen* unterteilt. Diese paar Sekunden werden als *Sitzungstimeout* bezeichnet.

Wenn Ihre Webanwendung das Ende der Benutzeraktivitäten nicht selbst deklariert (durch Aufrufen der `engagement.agent.endActivity`-Funktion), lässt der Engagement-Server die Benutzersitzung automatisch binnen drei Minuten auslaufen, nachdem die Anwendungsseite geschlossen wurde. Dieses Verhalten wird als *Sitzungstimeout* des Servers bezeichnet.

### `Crash`

Es gibt keinen automatisierten Bericht zu nicht abgefangenen JavaScript-Ausnahmen. Sie können jedoch Abstürze mithilfe der `sendCrash`-Funktion (siehe unten) manuell melden.

## Berichterstellung für Aktivitäten

### Benutzer startet eine neue Aktivität

	engagement.agent.startActivity("MyUserActivity");

Sie müssen jedes Mal `startActivity()` aufrufen, wenn sich die Benutzeraktivität ändert. Der erste Aufruf dieser Funktion startet eine neue Benutzersitzung.

### Der Benutzer beendet seine aktuelle Aktivität

	engagement.agent.endActivity();

Sie müssen `endActivity()` mindestens einmal aufrufen, wenn der Benutzer seine letzte Aktivität beendet. Dadurch wird das Engagement-SDK darüber informiert, dass sich der Benutzer derzeit im Leerlauf befindet und die Benutzersitzung geschlossen werden muss, sobald das Sitzungszeitlimit abläuft (wenn Ihr Aufruf `startActivity()` von vor dem Sitzungszeitlimit abläuft, wird die Sitzung einfach fortgesetzt).

Es ist oft schwierig oder unmöglich, das Ende von Benutzeraktivitäten innerhalb von Webumgebungen abzufangen (kein zuverlässiger Aufruf, wenn das Navigatorfenster geschlossen wird). Deshalb wird die Benutzersitzung vom Engagement-Server automatisch binnen drei Minuten nach Schließen der Anwendungsseite beendet.

## Berichterstellung für Ereignisse

### Sitzungsereignisse

Sitzungsereignisse werden normalerweise verwendet, um die Aktionen eines Benutzers während seiner Sitzung zu melden.

**Beispiel ohne zusätzliche Daten:**

	loginButton.onclick = function() {
	  engagement.agent.sendSessionEvent('login');
	  // [...]
	}

**Beispiel mit zusätzlichen Daten:**

	loginButton.onclick = function() {
	  engagement.agent.sendSessionEvent('login', {user: 'alice'});
	  // [...]
	}

### Eigenständige Ereignisse

Im Gegensatz zu Sitzungsereignissen können eigenständige Ereignisse außerhalb eines Sitzungskontextes auftreten.

Verwenden Sie dazu ``engagement.agent.sendEvent`` anstelle von ``engagement.agent.sendSessionEvent``.

## Melden von Fehlern

### Sitzungsfehler

Sitzungsfehler werden normalerweise zum Melden der Fehler verwendet, die Auswirkungen auf den Benutzer während seiner Sitzung haben.

**Beispiel ohne zusätzliche Daten:**

	var validateForm = function() {
	  // [...]
	  if (password.length < 6) {
	    engagement.agent.sendSessionError('password_too_short');
	  }
	  // [...]
	}

**Beispiel mit zusätzlichen Daten:**

	var validateForm = function() {
	  // [...]
	  if (password.length < 6) {
	    engagement.agent.sendSessionError('password_too_short', {length: 4});
	  }
	  // [...]
	}

### Eigenständige Fehler

Im Gegensatz zu Sitzungsfehlern können eigenständige Fehler außerhalb des Kontexts einer Sitzung auftreten.

Verwenden Sie dazu `engagement.agent.sendError` anstelle von `engagement.agent.sendSessionError`.

## Berichterstellung für Aufträge

### Beispiel

Angenommen, Sie möchten eine Ajax-Anforderung überwachen:
			
	// [...]
	xhr.onreadystatechange = function() {
	  if (xhr.readyState == 4) {
	  // [...]
	    engagement.agent.endJob('publish');
	  }
	}
	engagement.agent.startJob('publish');
	xhr.send();
	// [...]

### Melden von Fehlern im Verlauf eines Auftrags

Fehler können mit einem ausgeführten Auftrag in Zusammenhang stehen anstatt mit der aktuellen Benutzersitzung.

**Beispiel:**

Angenommen, Sie möchten einen Fehler melden, wenn bei einer Ajax-Anforderung ein Fehler auftritt:

	// [...]
	xhr.onreadystatechange = function() {
	  if (xhr.readyState == 4) {
	    // [...]
	    if (xhr.status == 0 || xhr.status >= 400) {
	      engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
	    }
	    engagement.agent.endJob('publish');
	  }
	}
	engagement.agent.startJob('publish');
	xhr.send();
	// [...]

### Melden von Ereignissen während eines Auftrags

Ereignisse können dank der `engagement.agent.sendJobEvent`-Funktion statt mit der aktuellen Benutzersitzung in Zusammenhang mit einem ausgeführten Auftrag stehen.

Diese Funktion funktioniert genauso wie `engagement.agent.sendJobError`.

### Berichterstellung von Abstürzen

Die `sendCrash`-Funktion dient zum manuellen Melden von Abstürzen.

Das `crashid`-Argument ist eine Zeichenfolge zum Bestimmen des Typs des Absturzes. Das `crash`-Argument ist in der Regel die Stapelüberwachung des Absturzes als Zeichenfolge.

	engagement.agent.sendCrash(crashid, crash);

## Zusätzliche Parameter

Einem Ereignis, einem Fehler, einer Aktivität oder einem Auftrag können beliebige Daten zugeordnet werden.

Diese Daten können ein beliebiges JSON-Objekt (kein Array oder primitiver Typ) sein.

**Beispiel**

	var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
	engagement.agent.sendEvent("video_clicked", extras);

### Grenzen

#### Schlüssel

Jeder Schlüssel in dem Objekt muss mit dem folgenden regulären Ausdruck übereinstimmen:

	^[a-zA-Z][a-zA-Z_0-9]*

Das bedeutet, dass Schlüssel mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstrichen (\_) beginnen müssen.

#### Werte

Werte sind auf Zeichenfolgen-, numerische und boolesche Typen beschränkt.

#### Größe

Zusätzliche Daten sind auf **1024** Zeichen pro Aufruf beschränkt (nach Codierung in JSON durch das SDK).

## Informationen zur Berichterstellung

Sie können Berichte zur Nachverfolgung (oder zu anderen anwendungsspezifischen Informationen) mithilfe der `sendAppInfo()`-Funktion manuell erstellen.

Beachten Sie, dass diese Informationen inkrementell gesendet werden können: Nur der letzte Wert für einen bestimmten Schlüssel wird für ein bestimmtes Gerät gespeichert.

Wie bei Ereigniszusätzen kann ein beliebiges JSON-Objekt zum Abstrahieren von Anwendungsinformationen verwendet werden. Beachten Sie, dass Arrays oder Unterobjekte als einfache Zeichenfolgen (mithilfe der JSON-Serialisierung) behandelt werden.

### Beispiel

Hier folgt ein Codebeispiel zum Senden von Geschlecht und Geburtstag des Benutzers:

	var appInfos = {"birthdate":"1983-12-07","gender":"female"};
	engagement.agent.sendAppInfo(appInfos);

### Grenzen

#### Schlüssel

Jeder Schlüssel in dem Objekt muss mit dem folgenden regulären Ausdruck übereinstimmen:

	^[a-zA-Z][a-zA-Z_0-9]*

Das bedeutet, dass Schlüssel mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstrichen (\_) beginnen müssen.

#### Größe

Anwendungsinformationen sind beschränkt auf **1024** Zeichen pro Aufruf (einmal im JSON-Format vom SDK codiert).

Im vorherigen Beispiel enthält die an den Server gesendete JSON 44 Zeichen:

	{"birthdate":"1983-12-07","gender":"female"}
 

<!---HONumber=AcomDC_0615_2016-->