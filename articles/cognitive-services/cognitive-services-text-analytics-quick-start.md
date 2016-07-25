<properties
	pageTitle="Kurzanleitung: Machine Learning-Textanalyse-APIs | Microsoft Azure"
	description="Azure Machine Learning-Textanalyse – Kurzanleitung"
	services="cognitive-services"
	documentationCenter=""
	authors="onewth"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="cognitive-services"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/05/2016"
	ms.author="onewth"/>

# Erste Schritte mit den Textanalyse-APIs zur Erkennung von Stimmung, Schlüsselwörtern, Themen und Sprache

<a name="HOLTop"></a>

Dieses Dokument beschreibt, wie Sie Ihren Dienst oder Ihre Anwendung integrieren können, um die [Textanalyse-APIs](//go.microsoft.com/fwlink/?LinkID=759711) zu verwenden. Verwenden Sie diese APIs, um Stimmung, Schlüsselwörter, Themen und Sprache des Textes zu erkennen. [Klicken Sie hier, um sich eine interaktive Demo der Nutzung anzuschauen.](//go.microsoft.com/fwlink/?LinkID=759712)

Technische Dokumentation für die APIs finden Sie in den [API-Definitionen](//go.microsoft.com/fwlink/?LinkID=759346).

Dieses Handbuch bezieht sich auf Version 2 der APIs. Ausführliche Informationen zu Version 1 der APIs [finden Sie in diesem Dokument](../machine-learning/machine-learning-apps-text-analytics.md).

Am Ende dieses Tutorials können Sie programmgesteuert Folgendes erkennen:

- **Stimmung**: Ist der Text positiv oder negativ?

- **Schlüsselwörter**: Was wird in einem einzigen Artikel besprochen?

- **Themen**: Worüber diskutieren die Menschen in verschiedenen Artikeln?

- **Sprachen**: In welcher Sprache wurde der Text geschrieben?

Beachten Sie, dass diese API pro übermitteltem Dokument 1 Transaktion berechnet. Wenn Sie also beispielsweise bei einem Aufruf die Stimmung für 1.000 Dokumente anfordern, fallen 1.000 Transaktionen an.



<a name="Overview"></a>
## Allgemeine Übersicht ##

Dieses Dokument ist eine schrittweise Anleitung. Ziel ist es, Ihnen Schritt für Schritt die erforderlichen Schritte zum Trainieren eines Modells vorzustellen und Ihnen Ressourcen aufzuzeigen, mit denen Sie das Training umsetzen können. Diese Übung dauert ungefähr 30 Minuten.

Für diese Aufgaben benötigen Sie einen Editor. Zudem müssen Sie die RESTful-Endpunkte in der Sprache Ihrer Wahl aufrufen.

Lassen Sie uns anfangen.

## Aufgabe 1: Registrieren für die Textanalyse-APIs ####

In dieser Aufgabe können Sie sich für den Textanalysedienst registrieren.

1. Navigieren Sie im [Azure-Portal](//go.microsoft.com/fwlink/?LinkId=761108) zu **Kognitive Dienste**, und stellen Sie sicher, dass als „API-Typ“ **Textanalyse** ausgewählt ist.

1. Wählen Sie einen Plan. Sie können z. B. den **Free-Tarif für 5.000 Transaktionen pro Monat** auswählen. Da es sich um einen kostenlosen Plan handelt, wird die Nutzung dieses Diensts nicht berechnet. Sie müssen sich bei Ihrem Azure-Abonnement anmelden.

1. Füllen Sie die anderen Felder aus, und erstellen Sie Ihr Konto.

1. Nach der Anmeldung bei der Textanalyse erhalten Sie Ihren **API-Schlüssel**. Kopieren Sie den Primärschlüssel, da Sie ihn für die Nutzung der API-Dienste benötigen.


## Aufgabe 2: Erkennung von Stimmung, Schlüsselwörtern und Sprachen ####

Die Erkennung von Stimmung, Schlüsselwörtern und Sprachen im Text ist einfach. Es werden programmgesteuert die gleichen Ergebnisse wie in der [Nutzungsdemo](//go.microsoft.com/fwlink/?LinkID=759712) zurückgegeben.

>[AZURE.TIP] Für die Stimmungsanalyse empfehlen wir, dass Sie den Text in Sätze aufteilen. Dies trägt im Allgemeinen zu einer höhere Genauigkeit der Stimmungsvorhersagen bei.

Die folgenden Sprachen werden unterstützt:

| Funktion | Unterstützte Sprachcodes |
|:-----|:----|
| Stimmung | `en` (Englisch), `es` (Spanisch), `fr` (Französisch), `pt` (Portugiesisch) |
| Schlüsselwörter | `en` (Englisch), `es` (Spanisch), `de` (Deutsch), `ja` (Japanisch) |


1. Dafür müssen die Header wie folgt festgelegt werden. Beachten Sie, dass JSON derzeit das einzige akzeptierte Eingabeformat für die APIs ist. XML wird nicht unterstützt.

		Ocp-Apim-Subscription-Key: <your API key>
		Content-Type: application/json
		Accept: application/json

1. Formatieren Sie als Nächstes die Eingabezeilen in JSON. Das Format für Stimmung, Schlüsselwörter und Sprache ist identisch. Beachten Sie, dass jede ID eindeutig sein muss und die vom System zurückgegebene ID ist. Die maximale Größe eines einzelnen Dokuments, das übermittelt werden kann, beträgt 10 KB, und die maximale Gesamtgröße der übermittelten Eingabe beträgt 1 MB. In einem Aufruf können nicht mehr als 1.000 Dokumente übermittelt werden. Die Sprache ist ein optionaler Parameter, der angegeben werden muss, wenn nicht englischer Text analysiert wird. Ein Beispiel für die Eingabe ist unten dargestellt, wobei der optionale Parameter `language` für die Stimmungsanalyse oder Schlüsselwortextraktion hinzugefügt wurde:

		{
			"documents": [
				{
					"language": "en",
					"id": "1",
					"text": "First document"
				},
                ...
                {
					"language": "en",
					"id": "100",
					"text": "Final document"
				}
			]
		}

1. Führen Sie einen **POST**-Aufruf an das System mit der Eingabe für Stimmung, Schlüsselwörter und Sprache durch. Die URLs sehen wie folgt aus:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages

1. Der Aufruf gibt eine JSON-formatierte Antwort mit den IDs und erkannten Eigenschaften zurück. Ein Beispiel für die Stimmungsausgabe wird unten dargestellt (ausgeschlossen Fehlerdetails). Bei der Stimmungserkennung wird ein Wert zwischen 0 und 1 für jedes Dokument zurückgegeben:

        // Sentiment response
		{
		  	"documents": [
				{
					"id": "1",
					"score": "0.934"
		        },
                ...
                {
					"id": "100",
					"score": "0.002"
		        },
			]
		}

        // Key phrases response
        {
		  	"documents": [
				{
					"id": "1",
					"keyPhrases": ["key phrase 1", ..., "key phrase n"]
		        },
                ...
                {
					"id": "100",
					"keyPhrases": ["key phrase 1", ..., "key phrase n"]
		        },
			]
		}

        // Languages response
        {
		  	"documents": [
				{
					"id": "1",
					"detectedLanguages": [
                        {
                            "name": "English",
                            "iso6391Name": "en",
                            "score": "1"
                        }
                    ]
		        },
                ...
                {
                    "id": "100",
                    "detectedLanguages": [
                        {
                            "name": "French",
                            "iso6391Name": "fr",
                            "score": "0.985"
                        }
                    ]
		        }
			]
		}


## Aufgabe 3: Erkennen von Themen in einem Textcorpus ####

Diese neu veröffentlichte API gibt die am meisten erkannten Themen für eine Liste übermittelter Textdatensätze zurück. Ein Thema wird anhand eines Schlüsselausdrucks identifiziert, der aus einem oder mehreren darauf bezogenen Wörtern bestehen kann. Die API eignet sich ideal für kurzen, von Menschen geschriebenen Text, z.B. Kritiken und Feedback von Benutzern.

Für diese API müssen **mindestens 100 Textdatensätze** übermittelt werden, aber sie kann Themen in Hunderten bis Tausenden von Datensätzen erkennen. Alle nicht englischsprachigen Datensätze oder Datensätze mit weniger als 3 Wörtern werden verworfen. Ihnen wird daher kein Thema zugewiesen. Die maximale Größe eines einzelnen Dokuments, das für die Themenerkennung übermittelt werden kann, beträgt 30 KB, und die maximale Gesamtgröße der übermittelten Eingabe beträgt 30 MB.

Es gibt zwei zusätzliche **optionale** Eingabeparameter, die zur Verbesserung der Ergebnisqualität beitragen können:

- **Stoppwörter.** Diese Wörter und deren verwandte Formen (z. B. Pluralformen) werden aus der gesamten Themenerkennungspipeline ausgeschlossen. Verwenden Sie dies für häufige Wörter. („Problem“, „Fehler“ und „Benutzer“ sind womöglich beispielsweise angemessene Optionen für Kundenbeschwerden über Software.) Jede Zeichenfolge sollte ein einzelnes Wort sein.
- **Stoppausdrücke**: Diese Ausdrücke werden von der Liste der zurückgegebenen Themen ausgeschlossen. Verwenden Sie diese Option, um allgemeine Themen auszuschließen, die nicht in den Ergebnissen angezeigt werden sollen. „Microsoft“ und „Azure“ sind beispielsweise geeignete Optionen für Themen, die Sie ausschließen möchten. Zeichenfolgen können mehrere Wörter enthalten.

Gehen Sie wie folgt vor, um Themen im Text zu erkennen.

1. Formatieren Sie die Eingabe in JSON. Diesmal können Sie Stoppwörter und Stoppausdrücke definieren.

		{
			"documents": [
				{
					"id": "1",
					"text": "First document"
				},
                ...
                {
					"id": "100",
					"text": "Final document"
				}
			],
			"stopWords": [
				"issue", "error", "user"
			],
			"stopPhrases": [
				"Microsoft", "Azure"
			]
		}

1. Richten Sie unter Verwendung der gleichen Header wie in Aufgabe 2 definiert einen **POST**-Aufruf an den Themenendpunkt:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/topics

1. Dies gibt eine `operation-location` als Header in der Antwort zurück, wobei der Wert die abzufragende URL für die resultierenden Themen ist:

        'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

1. Fragen Sie regelmäßig die zurückgegebene `operation-location` mit einer **GET**-Anforderung ab. Empfohlen ist einmal pro Minute.

        GET https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>

1. Der Endpunkt gibt eine Antwort einschließlich `{"status": "notstarted"}` vor der Verarbeitung, `{"status": "running"}` während der Verarbeitung und `{"status": "succeeded"}` mit der Ausgabe nach Abschluss dieses Vorgangs zurück. Sie können dann die Ausgabe nutzen, die das folgende Format aufweist (beachten Sie, dass Details wie Fehlerformat und Daten aus diesem Beispiel ausgeschlossen wurden):

		{
			"status": "succeeded",
			"operationProcessingResult": {
			  	"topics": [
                    {
					    "id": "8b89dd7e-de2b-4a48-94c0-8e7844265196"
					    "score": "5"
					    "keyPhrase": "first topic name"
                    },
                    ...
                    {
					    "id": "359ed9cb-f793-4168-9cde-cd63d24e0d6d"
					    "score": "3"
					    "keyPhrase": "final topic name"
                    }
                ],
			  	"topicAssignments": [
                    {
					    "topicId": "8b89dd7e-de2b-4a48-94c0-8e7844265196",
					    "documentId": "1",
					    "distance": "0.354"
                    },
                    ...
                    {
					    "topicId": "359ed9cb-f793-4168-9cde-cd63d24e0d6d",
					    "documentId": "55",
					    "distance": "0.758"
                    },            
                ]
			}
		}

Beachten Sie, dass die erfolgreiche Antwort für Themen vom `operations`-Endpunkt das folgende Schema hat:

	{
    		"topics" : [{
        		"id" : "string",
        		"score" : "number",
        		"keyPhrase" : "string"
    		}],
    		"topicAssignments" : [{
        		"documentId" : "string",
        		"topicId" : "string",
        		"distance" : "number"
    		}],
    		"errors" : [{
        		"id" : "string",
        		"message" : "string"
    		}]
    	}

Die Erläuterungen für jeden Teil dieser Antwort lauten:

**topics**

| Schlüssel | Beschreibung |
|:-----|:----|
| id | Ein eindeutiger Bezeichner für jedes Thema. |
| Ergebnis Ihrer App | Anzahl der Dokumente, die dem Thema zugewiesen sind. |
| keyPhrase | Eine Zusammenfassung des Themas in einem Wort oder Ausdruck. |

**topicAssignments**

| Schlüssel | Beschreibung |
|:-----|:----|
| documentId | Bezeichner des Dokuments. Entspricht der in der Eingabe enthaltenen ID. |
| topicId | Die Themen-ID, der das Dokument zugewiesen wurde. |
| distance | Dokument-Thema-Zugehörigkeitsfaktor zwischen 0 und 1. Je niedriger der Abstandswert, desto stärker die Zugehörigkeit des Themas. |

**errors**

| Schlüssel | Beschreibung |
|:-----|:----|
| id | Eindeutiger Bezeichner des Eingabedokuments, auf den sich der Fehler bezieht. |
| message | Fehlermeldung. |

## Nächste Schritte ##

Glückwunsch! Sie haben nun die Nutzung der Textanalyse für Ihre Daten abgeschlossen. Sie können nun Informationen über die Nutzung eines Tools wie z.B. [Power BI](//powerbi.microsoft.com) einholen, um Ihre Daten zu visualisieren, und Ihre Erkenntnisse automatisieren, damit Sie Ihre Textdaten in Echtzeit anzeigen können.

Wie Textanalysefunktionen, z.B. Stimmung, als Teil eines Bots verwendet werden können, zeigt Ihnen das Beispiel [Emotional Bot](http://docs.botframework.com/de-DE/bot-intelligence/language/#example-emotional-bot) (Emotionaler Bot) auf der Bot-Framework-Website.

<!---HONumber=AcomDC_0713_2016-->