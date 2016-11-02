<properties
	pageTitle="Aktualisieren auf Version 2 der Text Analytics-API | Microsoft Azure"
	description="Azure Machine Learning Text Analytics – Upgrade auf Version 2"
	services="cognitive-services"
	documentationCenter=""
	authors="onewth"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
	ms.service="cognitive-services"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/05/2016"
	ms.author="onewth"/>

# Aktualisieren auf Version 2 der Text Analytics-API #

In diesem Leitfaden werden Sie durch den Prozess der Aktualisierung des Codes von der [ersten Version der API](../machine-learning/machine-learning-apps-text-analytics.md) auf die zweite Version geführt.

Falls Sie die API nicht verwendet haben und weitere Informationen benötigen, können Sie **[hier mehr zur API erfahren](//go.microsoft.com/fwlink/?LinkID=759711)** oder die **[Schnellstartanleitung verwenden](//go.microsoft.com/fwlink/?LinkID=760860)**. Die technische Referenz finden Sie in der **[API-Definition](//go.microsoft.com/fwlink/?LinkID=759346)**.

### Teil 1: Abrufen eines neuen Schlüssels ###

Zuerst müssen Sie aus dem **Azure-Portal** einen neuen API-Schlüssel abrufen:

1. Navigieren Sie über den [Cortana Analytics-Katalog](//gallery.cortanaintelligence.com/MachineLearningAPI/Text-Analytics-2) zum Text Analytics-Dienst. Hier finden Sie auch Links zur Dokumentation und zu Codebeispielen.

1. Klicken Sie auf **Registrieren**. Mit diesem Link gelangen Sie zum Azure-Verwaltungsportal, in dem Sie sich für den Dienst registrieren können.

1. Wählen Sie einen Plan. Sie können z. B. den **Free-Tarif für 5.000 Transaktionen pro Monat** auswählen. Da es sich um einen kostenlosen Plan handelt, wird die Nutzung dieses Diensts nicht berechnet. Sie müssen sich bei Ihrem Azure-Abonnement anmelden.

1. Nach der Registrierung für Text Analytics erhalten Sie einen **API-Schlüssel**. Kopieren Sie diesen Schlüssel, da Sie ihn beim Verwenden der API-Dienste benötigen.

### Teil 2: Aktualisieren der Header ###

Aktualisieren Sie die übermittelten Headerwerte wie unten dargestellt. Beachten Sie, dass der Kontoschlüssel nicht mehr codiert ist.

**Version 1**

    Authorization: Basic base64encode(<your Data Market account key>)
    Accept: application/json

**Version 2**

    Content-Type: application/json
    Accept: application/json
    Ocp-Apim-Subscription-Key: <your Azure Portal account key>


### Teil 3: Aktualisieren der Basis-URL ###

**Version 1**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/

**Version 2**

    https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/

### Teil 4a: Aktualisieren der Formate für Stimmung, Schlüsselwörter und Sprachen ###

#### Endpunkte ####

GET-Endpunkte sind inzwischen veraltet, sodass alle Eingaben als POST-Anforderungen übermittelt werden sollten. Aktualisieren Sie die Endpunkte auf die unten angegebenen Endpunkte.

| |Version 1 – Einzelner Endpunkt|Version 1 – Batchendpunkt|Version 2 – Endpunkt|
|---|---|---|---|
|Aufruftyp|GET|POST|POST|
|Stimmung|```GetSentiment```|```GetSentimentBatch```|```sentiment```|
|Schlüsselwörter|```GetKeyPhrases```|```GetKeyPhrasesBatch```|```keyPhrases```|
|Sprachen|```GetLanguage```|```GetLanguageBatch```|```languages```|

#### Eingabeformate ####

Beachten Sie, dass nur noch das POST-Format akzeptiert wird. Sie sollten also alle Eingaben, für die bisher die Endpunkte mit einzelnen Dokumenten verwendet wurden, entsprechend neu formatieren. Bei den Eingaben wird die Groß-/Kleinschreibung nicht beachtet.

**Version 1 (Batch)**

    {
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Version 2**

    {
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### Ausgabe für Stimmung ####

**Version 1**

    {
      "SentimentBatch":[{
        "Id":"string",
        "Score":"double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2**

    {
      "documents":[{
        "id":"string",
        "score":"double"
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### Ausgabe für Schlüsselwörter ####

**Version 1**

    {
      "KeyPhrasesBatch":[{
        "Id":"string",
        "KeyPhrases":["string"]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2**

    {
      "documents":[{
        "id":"string",
        "keyPhrases":["string"]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### Ausgabe für Sprachen ####


**Version 1**

    {
      "LanguageBatch":[{
        "id":"string",
        "detectedLanguages": [{
          "Score":"double"
          "Name":"string",
          "Iso6391Name":"string"
        }]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2**

    {
      "documents":[{
        "id":"string",
        "detectedLanguages": [{
          "score":"double"
          "name":"string",
          "iso6391Name":"string"
        }]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }


### Teil 4b: Aktualisieren der Formate für Themen ###

#### Endpunkte ####

| |Version 1 – Endpunkt | Version 2 – Endpunkt|
|---|---|---|
|Für Themenerkennung übermitteln (POST)|```StartTopicDetection```|```topics```|
|Themenergebnisse abrufen (GET)|```GetTopicDetectionResult?JobId=<jobId>```|```operations/<operationId>```|

#### Eingabeformate ####

**Version 1**

    {
      "StopWords": [
        "string"
      ],
      "StopPhrases": [
        "string"
      ], 
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Version 2**

    {
      "stopWords": [
        "string"
      ],
      "stopPhrases": [
        "string"
      ],
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### Übermittlungsergebnisse ####

**Version 1 (POST)**

Bisher haben Sie nach Abschluss des Auftrags die folgende JSON-Ausgabe erhalten, bei der die jobId zum Abrufen der Ausgabe an eine URL angefügt wurde.

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

**Version 2 (POST)**

Die Antwort enthält jetzt wie folgt einen Headerwert, bei dem `operation-location` als Endpunkt zum Abfragen der Ergebnisse verwendet wird:

    'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

#### Operationsergebnis ####

**Version 1 (GET)**

    {
      "TopicInfo" : [{
        "TopicId" : "string"
        "Score" : "double"
        "KeyPhrase" : "string"
      }],
      "TopicAssignment" : [{
        "Id" : "string",
        "TopicId" : "string",
        "Distance" : "double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2 (GET)**

**Rufen Sie wieder regelmäßig die Ausgabe ab** (Empfehlung: jede Minute), bis die Ausgabe zurückgegeben wird.

Nachdem die Themen-API den Vorgang abgeschlossen hat, wird der Status `succeeded` zurückgegeben. Dies umfasst auch die Ausgabeergebnisse im unten angegebenen Format:

    {
        "status": "succeeded",
        "createdDateTime": "string",
        "operationType": "topics",
        "processingResult": {
            "topics" : [{
            "id" : "string"
            "score" : "double"
            "keyPhrase" : "string"
          }],
          "topicAssignments" : [{
            "topicId" : "string",
            "documentId" : "string",
            "distance" : "double"
          }],
          "errors" : [{
              "id":"string",
              "message":"string"
          }]
        }
    }

### Teil 5: Testen Sie es! ###

Jetzt sollte alles startklar sein! Testen Sie Ihren Code mit einer kleinen Stichprobe, um sicherzustellen, dass Sie die Daten erfolgreich verarbeiten können.

<!---HONumber=AcomDC_0914_2016-->