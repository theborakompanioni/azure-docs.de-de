<properties
   pageTitle="Überblick: Sprechererkennungs-API von Azure Cognitive Services | Microsoft Azure"
   description="Erfahren Sie, wie sich Anwendungen mithilfe der Sprechererkennung verbessern lassen. Sprechererkennungs-APIs verfügen über modernste Algorithmen zur Sprecherüberprüfung und Sprecheridentifikation."
   services="cognitive-services"
   documentationCenter="na"
   authors="cjgronlund"
   manager="paulettem"
   editor="cjgronlund"/>

<tags
   ms.service="cognitive-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/02/2016"
   ms.author="cgronlun"/>

# Überblick: Sprechererkennungs-API von Azure Cognitive Services

Erfahren Sie, wie Sie mit Funktionen zur Sprechererkennung Ihre Anwendungen verbessern können. Bei Sprechererkennungs-APIs handelt es sich um cloudbasierte APIs, die modernste Algorithmen zur Sprecherüberprüfung und Sprecheridentifikation bereitstellen. Die Sprechererkennung lässt sich in zwei Kategorien aufteilen:

  * Sprecherüberprüfung
  * Sprecheridentifikation

## Sprecherüberprüfung

Jede menschliche Stimme verfügt über eindeutige Merkmale, die zum Identifizieren einer Person verwendet werden können – genauso wie ein Fingerabdruck. Das Verwenden von Stimme und Sprache als Signal für die Zugriffssteuerung hat sich als innovatives Werkzeug erwiesen, mit dem sich ein höheres Niveau an Sicherheit und mehr Komfort bei der Authentifizierung für die Kunden erreichen lässt.

Die Sprecherüberprüfungs-APIs können Benutzer anhand ihrer Stimme und Sprache automatisch authentifizieren.

### Registrierung für die Sprecherüberprüfung

Die Registrierung für die Sprecherüberprüfung erfolgt textgebunden. Das heißt, dass die Sprecher eine bestimmte Passphrase auswählen müssen, die sie sowohl bei der Registrierung als auch bei der Überprüfung verwenden.

Bei der Registrierung spricht der Sprecher eine bestimmte Wortgruppe als Passphrase, während seine Stimme aufgezeichnet wird. Anschließend wird eine Reihe von Merkmalen extrahiert, und die gewählte Phrase wird erkannt. Die extrahierten Merkmale und die gewählte Phrase bilden gemeinsam eine eindeutige Stimmsignatur.

### Überprüfen der Spracheingabe und der Passphrase

Bei der Überprüfung werden die Stimme der Spracheingabe und die gesprochene Passphrase mit der Stimmsignatur und der Phrase aus der Registrierung verglichen. Es wird überprüft, ob es sich um dieselbe Person handelt und ob sie die richtige Passphrase sagt.

Weitere Informationen zur Sprecherüberprüfung finden Sie in der [Dokumentation zur Sprechererkennungs-API im Abschnitt zur Überprüfung](https://dev.projectoxford.ai/docs/services/563309b6778daf02acc0a508/operations/563309b7778daf06340c9652).

## Sprecheridentifikation

Sprecheridentifikations-APIs können die sprechende Person anhand einer Audiodatei automatisch erkennen, sofern sie zur einer Gruppe erwarteter Sprecher gehört. Das eingehende Audiosignal wird mit den Daten der bereitgestellten Gruppe von Sprechern abgeglichen, und im Fall einer Übereinstimmung wird die Identität des Sprechers zurückgegeben.

Alle Sprecher müssen zuerst einen Registrierungsprozess absolvieren, bei dem ihre Stimme im System erfasst und ihr stimmlicher Fingerabdruck erstellt wird.

### Registrierung für die Sprecheridentifikation

Die Registrierung für die Sprecheridentifikation erfolgt nicht textgebunden. Das heißt, es gibt keine Beschränkungen dafür, was der Sprecher für die Audiodatei sagt. Die Stimme des Sprechers wird aufgezeichnet, und eine Reihe von Merkmalen wird extrahiert, um eine eindeutige Stimmsignatur zu erstellen.

### Sprechererkennung: Abgleich der Spracheingabe

Für die Erkennung werden das Audiosignal des unbekannten Sprechers und die Daten der Gruppe der erwarteten Sprecher herangezogen. Die Spracheingabe wird mit den Daten aller Sprecher aus der Gruppe verglichen, um festzustellen, um wessen Stimme es sich handelt. Wenn eine Übereinstimmung gefunden wird, wird die Identität des Sprechers zurückgegeben.

Weitere Informationen zur Sprecheridentifikation finden Sie in der [Dokumentation zur Sprechererkennungs-API im Abschnitt zur Identifikation](https://dev.projectoxford.ai/docs/services/563309b6778daf02acc0a508/operations/5645c068e597ed22ec38f42e).

<!---HONumber=AcomDC_0504_2016-->