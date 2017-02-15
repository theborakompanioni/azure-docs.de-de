---
title: 'Abrufen von Empfehlungen in Batches: Machine Learning-Empfehlungs-API | Microsoft Docs'
description: "Azure Machine Learning-Empfehlungen – Abrufen von Empfehlungen in Batches"
services: cognitive-services
documentationcenter: 
author: luiscabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 325d4922-8a07-4e67-99e0-f513201f14f7
ms.service: cognitive-services
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: luisca
translationtype: Human Translation
ms.sourcegitcommit: 0af5a4e2139a202c7f62f48c7a7e8552457ae76d
ms.openlocfilehash: e63218d9c882d84342a3992f05e0a8c9f306d9c6


---
# <a name="get-recommendations-in-batches"></a>Abrufen von Empfehlungen in Batches
> [!NOTE]
> Das Abrufen von Empfehlungen in Batches ist komplizierter als das Abrufen einzelner Empfehlungen. Informationen zum Abrufen von Empfehlungen für eine einzelne Anforderung finden Sie unter den APIs:
> 
> [Element-zu-Element-Empfehlungen](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d4)<br>
> [Benutzer-zu-Element-Empfehlungen](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)
> 
> Batchbewertungen können nur für Builds verwendet werden, die nach dem 21. Juli 2016 erstellt wurden.
> 
> 

In bestimmten Situationen müssen Empfehlungen für mehrere Elemente gleichzeitig abgerufen werden. So können Sie beispielsweise einen Zwischenspeicher für Empfehlungen erstellen und sogar die Arten der erhaltenen Empfehlungen analysieren.

Diese so genannten Batchbewertungsvorgänge sind asynchrone Vorgänge. Sie müssen die Anforderung übermitteln, auf den Abschluss des Vorgangs warten und anschließend die Ergebnisse abrufen.  

Um genau zu sein, müssen die folgenden Schritte ausgeführt werden:

1. Erstellen Sie einen Azure Storage-Container, sofern noch keiner vorhanden ist.
2. Laden Sie eine Eingabedatei hoch, die die einzelnen Empfehlungsanforderungen für Azure Blob Storage beschreibt.
3. Starten Sie den Batchbewertungsauftrag.
4. Warten Sie, bis der asynchrone Vorgang abgeschlossen ist.
5. Rufen Sie anschließend die Ergebnisse aus Blob Storage ab.

Im Anschluss sehen wir uns die einzelnen Schritte genauer an.

## <a name="create-a-storage-container-if-you-dont-have-one-already"></a>Erstellen eines Storage-Containers, sofern noch keiner vorhanden ist
Erstellen Sie im [Azure-Portal](https://portal.azure.com) ein neues Speicherkonto, falls noch keines vorhanden ist. Navigieren Sie hierzu zu **Neu** > **Daten** + **Speicher** > **Speicherkonto**.

Wenn Sie über ein Speicherkonto verfügen, müssen Sie die Blobcontainer erstellen, in denen die Ein- und Ausgabe der Batchausführung gespeichert wird.

Laden Sie eine Eingabedatei hoch, die jede Ihrer Empfehlungsanforderungen für Blob Storage beschreibt. In diesem Beispiel nennen wir die Datei „input.json“.
Wenn Sie über einen Container verfügen, müssen Sie eine Datei hochladen, in der die einzelnen Anforderungen beschrieben werden, die über den Empfehlungsdienst ausgeführt werden sollen.

Ein Batch kann immer nur eine Art von Anforderung eines spezifischen Builds ausführen. Die Vorgehensweise zum Definieren dieser Informationen wird im nächsten Abschnitt erläutert. Wir gehen vorerst einfach davon aus, dass wir Elementempfehlungen über einen bestimmten Build ausführen. Die Eingabedatei enthält dann die Eingabeinformationen (in diesem Fall die Seed-Elemente) für die einzelnen Anforderungen.

Hier sehen Sie ein Beispiel für die Datei „input.json“:

    {
      "requests": [
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F34-03453" ] },
      { "SeedItems": [ "D16-3244" ] },
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F43-01467" ] },
      { "SeedItems": [ "BD5-06013" ] },
      { "SeedItems": [ "P45-00163", "FKF-00689" ] },
      { "SeedItems": [ "C9A-69320" ] }
      ]
    }

Wie Sie sehen, handelt es sich um eine JSON-Datei, und jede der Anforderungen verfügt über die Informationen, die zum Senden einer Empfehlungsanforderung benötigt werden. Erstellen Sie eine ähnliche JSON-Datei für die Anforderungen in Ihrem Szenario, und kopieren Sie sie in den soeben erstellten Container in Blob Storage.

## <a name="kick-start-the-batch-job"></a>Starten des Batchauftrags
Als Nächstes muss ein neuer Batchauftrag übermittelt werden. Weitere Informationen finden Sie in der [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/).

Der Anforderungstext der API muss die Speicherorte für die Eingabe-, Ausgabe- und Fehlerdateien definieren. Außerdem müssen die erforderlichen Anmeldeinformationen für den Zugriff auf diese Speicherorte definiert werden. Darüber hinaus müssen einige Parameter angegeben werden, die für den gesamten Batch gelten. Hierzu zählen unter anderem die Art der anzufordernden Empfehlungen, das zu verwendende Modell/der zu verwendende Build und die Anzahl von Ergebnissen pro Aufruf.

Hier sehen Sie ein Beispiel für den Anforderungstext:

    {
      "input": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchInput.json",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "output": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchOutput.json ",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "error": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/errors.txt",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "job": {
        "apiName": "ItemRecommend",
        "modelId": "9ac12a0a-1add-4bdc-bf42-c6517942b3a6",
        "buildId": 1015703,
        "numberOfResults": 10,
        "includeMetadata": true,
        "minimalScore": 0.0
      }
    }

Wichtig:

* **authenticationType** muss nach aktuellem Stand immer auf **PublicOrSas** festgelegt werden.
* Sie benötigen ein SAS-Token (Shared Access Signature), um der Empfehlungs-API Lese- und Schreibzugriff für Ihr Blob-Speicherkonto zu erteilen. Weitere Informationen zum Generieren von SAS-Token finden Sie auf der [Seite für die Empfehlungs-API](../storage/storage-dotnet-shared-access-signature-part-1.md).
* Derzeit wird für **apiName** ausschließlich **ItemRecommend** (für Element-zu-Element-Empfehlungen) unterstützt. Benutzer-zu-Element-Empfehlungen werden derzeit von der Batchverarbeitung nicht unterstützt.

## <a name="wait-for-the-asynchronous-operation-to-finish"></a>Warten auf den Abschluss des asynchronen Vorgangs
Wenn Sie den Batchvorgang starten, gibt die Antwort den operation-location-Header mit den Informationen zurück, die Sie zum Nachverfolgen des Vorgangs benötigen.
Der Vorgangsstatus wird genau wie beim Nachverfolgen eines Buildvorgangs mithilfe der [API zum Abrufen des Vorgangsstatus](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3da)nachverfolgt.

## <a name="get-the-results"></a>Abrufen der Ergebnisse
Wenn der Vorgang fehlerfrei abgeschlossen wurde, können Sie die Ergebnisse aus dem Ausgabe-Blobspeicher abrufen.

Im Anschluss sehen Sie eine Beispielausgabe. Zur besseren Übersichtlichkeit zeigt das Beispiel Ergebnisse für einen Batch mit nur zwei Anforderungen.

    {
      "results":
      [   
        {
          "request": { "seedItems": [ "DAF-00500", "P3T-00003" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "F5U-00011",
                  "name": "L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.722,
              "reasoning": [ "People who like the selected items also like 'L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr'" ]
            },
            {
              "items": [
                {
                  "itemId": "G5Y-00001",
                  "name": "Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.718,
              "reasoning": [ "People who like the selected items also like 'Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr'" ]
            }
          ]
        },
        {
          "request": { "seedItems": [ "C9F-00163" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "C9F-00172",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Green",
                  "metadata": ""
                }
              ],
              "rating": 0.649,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Green'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00171",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Orange",
                  "metadata": ""
                }
              ],
              "rating": 0.647,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Orange'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00170",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Yellow",
                  "metadata": ""
                }
              ],
              "rating": 0.646,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Yellow'" ]
            }       
          ]
        }
    ]}


## <a name="learn-about-the-limitations"></a>Einschränkungen
* Pro Abonnement kann immer nur ein Batchauftrag aufgerufen werden.
* Eine Eingabedatei für einen Batchauftrag darf maximal 2 MB groß sein.




<!--HONumber=Dec16_HO2-->


