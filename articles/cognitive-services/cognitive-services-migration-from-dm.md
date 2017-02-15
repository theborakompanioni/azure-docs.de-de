---
title: Migrieren von der DataMarket-Empfehlungs-API zur Azure Cognitive Services-Empfehlungs-API | Microsoft Docs
description: "Azure Machine Learning-Empfehlungen – Migration zu Cognitive Services-Empfehlungen"
services: cognitive-services
documentationcenter: 
author: luiscabrer
manager: jhubbard
editor: cgronlun
ms.assetid: ec9cc302-fef5-4b68-8f9b-fa73538d0424
ms.service: cognitive-services
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/01/2016
ms.author: luisca
translationtype: Human Translation
ms.sourcegitcommit: 8329cc800f685455be113521446e691b5798c1fc
ms.openlocfilehash: 072f92b186f32813eef9b17e742081b680ef86a3


---
# <a name="migrate-to-azure-cognitive-services-recommendations-api-from-the-datamarket-recommendations-api"></a>Migrieren zur Azure Cognitive Services-Empfehlungs-API von der DataMarket-Empfehlungs-API
In diesem Artikel wird beschrieben, wie Sie die Migration von der [Microsoft DataMarket-Empfehlungs-API](https://datamarket.azure.com/dataset/amla/recommendations) zur [Microsoft Azure Cognitive Services-Empfehlungs-API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) durchführen.

Für die DataMarket-Empfehlungs-API wird die Annahme neuer Kunden am 31. Dezember 2016 beendet, und die API wird am 28. Februar 2017 eingestellt.

## <a name="how-do-i-start-using-the-azure-cognitive-services-recommendations-api"></a>Wie beginne ich mit der Verwendung der Azure Cognitive Services-Empfehlungs-API?
Gehen Sie wie folgt vor, um die Cognitive Services-Empfehlungs-API zu migrieren:

1. [Registrieren](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) Sie sich für ein Azure-Abonnement, wenn Sie noch keins besitzen. 
2. Verwenden Sie die Schritt-für-Schritt-Vorgehensweise aus der [Kurzanleitung](cognitive-services-recommendations-quick-start.md) , um sich für die Cognitive Services-Empfehlungs-API zu registrieren und diese programmgesteuert zu nutzen. 
3. Wechseln Sie auf die [Seite der Cognitive Services-Empfehlungs-API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api), um Informationen zum Dienst zu erhalten und nach Dokumentationen zu suchen.

## <a name="i-used-the-recommendations-ui-to-build-my-models-is-there-a-similar-tool-for-the-cognitive-services-recommendations-api"></a>Ich habe die Empfehlungs-API verwendet, um meine Modelle zu erstellen. Gibt es ein ähnliches Tool für die Cognitive Services-Empfehlungs-API?
Auf jeden Fall. Die URL für die neue [Empfehlungs-API](http://recommendations-portal.azurewebsites.net/) lautet: http://recommendations-portal.azurewebsites.net . 

> [!NOTE]
> Ihre DataMarket-Anmeldeinformationen funktionieren hierfür nicht. Registrieren Sie sich für ein Abonnement im Azure-Portal, und rufen Sie den Kontoschlüssel ab, der für die Verwendung der neuen [Benutzeroberfläche mit Empfehlungen](http://recommendations-portal.azurewebsites.net/)benötigt wird.
> Wenn Sie keinen Kontoschlüssel haben, hilft Ihnen Aufgabe 1 in der [Kurzanleitung](cognitive-services-recommendations-quick-start.md)weiter.
> 
> 

## <a name="is-the-new-api-format-the-same-as-the-datamarket-recommendations-api"></a>Ist das neue API-Format mit der DataMarket-Empfehlungs-API identisch?
Die API unterstützt die gleichen Szenarien und Prozessflüsse wie die in der DataMarket-Version unterstützten Szenarien, aber die eigentliche API-Schnittstelle wurde aktualisiert, damit sie die [Microsoft REST-API-Richtlinien](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md)erfüllt. Die APIs sind einheitlicher und können besser mit Tools genutzt werden, die Swagger unterstützen.

Sehen Sie sich den [API-Explorer](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3db) an, um Informationen zu beiden APIs zu erhalten.
Verwenden Sie die Schaltfläche *Ausprobieren*, um einen API-Aufruf zu testen. Das Format der Dateien, die von der Empfehlungs-API verwendet werden (Katalog- und Nutzungsdateien), hat sich nicht geändert. Sie können also die gleichen Dateien bzw. die gleiche Infrastruktur beibehalten, die Sie zum Generieren dieser Dateien erstellt haben.

## <a name="what-are-some-new-features-in-the-cognitive-services-recommendations-api"></a>Welche neuen Features sind für die Cognitive Services-Empfehlungs-API verfügbar?
In den letzten zwei Monaten haben wir neue Funktionen für die Cognitive Services-Empfehlungs-API veröffentlicht:

* Benutzeroberfläche mit Empfehlungen für das Trainieren und Testen von Modellen, ohne dafür auch nur eine Codezeile schreiben zu müssen
* Batchbewertung zur gleichzeitigen Bereitstellung von Tausenden von Empfehlungen
* Erstellung von Metrikunterstützung zum Abfragen der Qualität von Empfehlungsmodellen
* Unterstützung für Geschäftsregeln
* Möglichkeit zum Auflisten und Herunterladen von Nutzungs- und Katalogdateien
* Rangfolgebuild-Unterstützung zum Abfragen der Qualität von Elementfeatures in einem Empfehlungsmodell
* Neue Möglichkeit zum Suchen nach einem Produkt im Katalog

## <a name="when-does-microsoft-stop-supporting-the-datamarket-recommendations-api"></a>Wann beendet Microsoft den Support für die DataMarket-Empfehlungs-API?
Für die [DataMarket-Empfehlungs-API](https://datamarket.azure.com/dataset/amla/recommendations) wird die Annahme neuer Kunden nach dem 31. Dezember 2016 beendet, und die API wird am 28. Februar 2017 vollständig eingestellt. 

## <a name="what-if-i-dont-have-the-data-that-i-need-to-recreate-my-models-in-the-cognitive-services-recommendations-api"></a>Was passiert, wenn ich nicht über die Daten verfüge, die ich für die Neuerstellung meiner Modelle in der Cognitive Services-Empfehlungs-API benötige?
Wir möchten diese Umstellung für Sie so einfach wie möglich gestalten. Wir können Sie dabei unterstützen, Ihre alten Modelle aus dem DataMarket-Konto in Ihr neues Abonnement der Azure Cognitive Services-Empfehlungs-API zu verschieben. Wenden Sie sich unter der Adresse [mlapi@microsoft.com](mailto://mlapi@microsoft.com)durchführen. Sie werden von uns dann zum Angeben Ihrer DataMarket-Abonnement-ID und Ihrer Cognitive Services-Abonnement-ID aufgefordert, bevor wir die Modelle dem neuen Konto zuordnen.




<!--HONumber=Dec16_HO2-->


