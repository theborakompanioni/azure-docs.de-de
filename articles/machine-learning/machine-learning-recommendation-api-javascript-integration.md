---
title: "Machine Learning-Empfehlungen – JavaScript-Integration | Microsoft Docs"
description: "Azure Machine Learning-Empfehlungen – JavaScript-Integration – Dokumentation"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: bbbb5bb6-489d-4a62-a2ae-f36237e9e2e1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 12/08/2016
ms.author: luisca
translationtype: Human Translation
ms.sourcegitcommit: dc07007eec860e0ad7342a4926c6797341719c5b
ms.openlocfilehash: 20791d5729a65497b5f76f929bf331906ac65818


---
# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Azure Machine Learning-Empfehlungen – JavaScript-Integration
> [!NOTE]
> Beginnen Sie mit der Nutzung der Empfehlungs-API des Cognitive Service anstatt mit dieser Version. Der Recommendations Cognitive Service wird diesen Dienst ersetzen, weshalb alle neuen Features dafür entwickelt werden. Der Dienst bietet neue Funktionen wie Unterstützung der Batchverarbeitung, einen besseren API-Explorer, eine übersichtlichere API-Oberfläche, eine einheitlicherere Registrierungs-/Abrechnungsumgebung usw.
> Erfahren Sie mehr zur [Migration zum neuen Cognitive Service](http://aka.ms/recomigrate)
> 
> 

Dieses Dokument beschreibt die Integration Ihrer Website mithilfe von JavaScript. JavaScript ermöglicht es Ihnen, Datensammlungsereignisse zu senden und Empfehlungen zu nutzen, nachdem Sie ein Empfehlungsmodell erstellt haben. Alle Vorgänge, die über JS erfolgen, können auch von der Serverseite her erfolgen.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a>1. Allgemeine Übersicht
Die Integration Ihrer Website in Azure ML-Empfehlungen umfasst zwei Phasen:

1. Senden von Ereignissen an Azure ML-Empfehlungen. Dadurch wird die Erstellung eines Empfehlungsmodells ermöglicht.
2. Nutzen der Empfehlungen. Nach der Erstellung des Modells können Sie die Empfehlungen nutzen. (In diesem Dokument wird nicht erklärt, wie ein Modells erstellt wird, weitere Informationen dazu finden Sie in der Kurzanleitung.)

<ins>Phase I</ins>

In der ersten Phase fügen Sie eine kleine JavaScript-Bibliothek in die HTML-Seiten ein, die es den Seiten ermöglicht, Ereignisse, die auf einer HTML-Seite auftreten, (über Data Market) an die Azure ML Empfehlungen-Server zu senden:

![Drawing1][1]

<ins>Phase II</ins>

In der zweiten Phase wählen Sie eine der folgenden Optionen, wenn die Empfehlungen auf der Seite angezeigt werden sollen:

1. Ihr Server ruft (beim Rendern der Seite) Azure ML Empfehlungen-Server (über Data Market) auf, um Empfehlungen abzurufen. Die Ergebnisse enthalten eine Liste mit Element-IDs. Der Server muss die Ergebnisse mit den Elementmetadaten (z. B. Bilder, Beschreibung) ausstatten und die erstellte Seite an den Browser senden.

![Drawing2][2]

2. Die andere Option besteht darin, die kleine JavaScript-Datei aus Phase 1 zu verwenden, um eine einfache Liste der empfohlenen Elemente abzurufen. Die daraus resultierenden Daten sind schlanker als bei der ersten Option.

![Drawing3][3]

## <a name="2-prerequisites"></a>2. Voraussetzungen
1. Erstellen Sie ein neues Modell mit den APIs. Informationen zur Vorgehensweise finden Sie in der Kurzanleitung.
2. Codieren Sie &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; mit base64. (Dies wird zur Standardauthentifizierung verwendet, damit der JS-Code die APIs aufrufen kann).

## <a name="3-send-data-acquisition-events-using-javascript"></a>3. Senden von Datensammlungsereignissen mit JavaScript
Die folgenden Schritte erleichtern das Senden von Ereignissen:

1. Binden Sie die JQuery-Bibliothek in Ihren Code ein. Sie können ihn über die folgende URL von Nuget herunterladen.
   
     http://www.nuget.org/packages/jQuery/1.8.2
2. Binden Sie die JavaScript-Bibliothek für Empfehlungen von folgender URL ein: http://aka.ms/RecoJSLib1.
3. Initialisieren Sie die Azure ML-Recommendations-Bibliothek mit den entsprechenden Parametern.
   
     <script>
         AzureMLRecommendationsStart("<base64encoding of username:key>",       "<model_id>");   </script>
4. Senden Sie das passende Ereignis. Im folgenden Abschnitt finden Sie detaillierte Beschreibungen zu allen Arten von Ereignissen (Beispiel für das click-Ereignis).
   
     <script>
         if (typeof AzureMLRecommendationsEvent=="undefined") {         
                     AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" });   </script>

### <a name="31----limitations-and-browser-support"></a>3.1.    Einschränkungen und Browserunterstützung
Dies ist eine Referenzimplementierung, die ohne Mängelgewähr bereitgestellt wird. Sie sollte allen wichtigen Browser unterstützen.

### <a name="32----type-of-events"></a>3.2.    Ereignistypen
Es gibt fünf Arten von Ereignissen, die von der Bibliothek unterstützt werden: Klicken (click), auf Empfehlung klicken (recommendationclick), dem Einkaufswagen etwas hinzufügen (addshopcart), etwas aus dem Einkaufswagen entfernen (removeshopcart) und einkaufen (purchase). Es ist ein weiteres Ereignis namens Login verfügbar, das zum Festlegen des Benutzerkontexts verwendet wird.

#### <a name="321-click-event"></a>3.2.1. Click-Ereignis
Dieses Ereignis sollte jedes Mal, wenn ein Benutzer auf ein Element klickt, verwendet werden. Wenn der Benutzer auf ein Element klickt, wird in der Regel eine neue Seite mit den Elementdetails geöffnet. Auf dieser Seite sollte dieses Ereignis ausgelöst werden.

Parameter:

* event (Zeichenfolge, erforderlich) – „click“
* item (Zeichenfolge, erforderlich) – eindeutiger Bezeichner des Elements
* itemName (Zeichenfolge, optional) – der Name des Elements
* itemDescription (Zeichenfolge, optional) – die Beschreibung des Elements
* itemCategory (Zeichenfolge, optional) – die Kategorie des Elements
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Oder mit optionalen Daten:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


#### <a name="322-recommendation-click-event"></a>3.2.2. Recommendationclick-Ereignis
Dieses Ereignis sollte jedes Mal verwendet werden, wenn ein Benutzer auf ein Element klickt, das von Azure ML-Empfehlungen als empfohlenes Element empfangen wurde. Wenn der Benutzer auf ein Element klickt, wird in der Regel eine neue Seite mit den Elementdetails geöffnet. Auf dieser Seite sollte dieses Ereignis ausgelöst werden.

Parameter:

* event (Zeichenfolge, erforderlich) – „recommendationclick“
* item (Zeichenfolge, erforderlich) – eindeutiger Bezeichner des Elements
* itemName (Zeichenfolge, optional) – der Name des Elements
* itemDescription (Zeichenfolge, optional) – die Beschreibung des Elements
* itemCategory (Zeichenfolge, optional) – die Kategorie des Elements
* seeds (Zeichenfolgenarray, optional) – die Seeds, die die Empfehlungsabfrage generiert haben
* recoList (Zeichenfolgenarray, optional) – das Ergebnis der Empfehlungsanforderung, die das Element generiert hat, auf das geklickt wurde
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Oder mit optionalen Daten:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]                 });
        </script>


#### <a name="323-add-shopping-cart-event"></a>3.2.3. Addshopcart-Ereignis
Dieses Ereignis sollte verwendet werden, wenn der Benutzer ein Element zum Einkaufswagen hinzufügt.
Parameter:

* event (Zeichenfolge, erforderlich) – „addshopcart“
* item (Zeichenfolge, erforderlich) – eindeutiger Bezeichner des Elements
* itemName (Zeichenfolge, optional) – der Name des Elements
* itemDescription (Zeichenfolge, optional) – die Beschreibung des Elements
* itemCategory (Zeichenfolge, optional) – die Kategorie des Elements
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

#### <a name="324-remove-shopping-cart-event"></a>3.2.4. Removeshopcart-Ereignis
Dieses Ereignis sollte verwendet werden, wenn der Benutzer ein Element aus dem Einkaufswagen entfernt.

Parameter:

* event (Zeichenfolge, erforderlich) – „removeshopcart“
* item (Zeichenfolge, erforderlich) – eindeutiger Bezeichner des Elements
* itemName (Zeichenfolge, optional) – der Name des Elements
* itemDescription (Zeichenfolge, optional) – die Beschreibung des Elements
* itemCategory (Zeichenfolge, optional) – die Kategorie des Elements
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

#### <a name="325-purchase-event"></a>3.2.5. Purchase-Ereignis
Dieses Ereignis sollte verwendet werden, wenn der Benutzer seinen Einkaufswagen gekauft hat.

Parameter:

* event (Zeichenfolge) – „purchase“
* items ( Purchased[] ) – Array mit einem Eintrag für jeden gekauften Artikel<br><br>
   Format für „Purchased“:
  * item (Zeichenfolge) – eindeutiger Bezeichner des Elements
  * count (Ganzzahl oder Zeichenfolge) – Anzahl der gekauften Elemente
  * price (Gleitkommazahl oder Zeichenfolge) – optionales Feld – der Preis des Elements

Das folgende Beispiel zeigt den Kauf von 3 Elementen (33, 34, 35), zwei Elemente, bei denen alle Feldern aufgefüllt sind (item, count, price), und ein Element ohne Preis (item 34).

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

#### <a name="326-user-login-event"></a>3.2.6. Userlogin-Ereignis
Die Ereignisbibliothek für Azure ML-Empfehlungen Bibliothek erstellt und verwendet ein Cookie , um Ereignisse zu identifizieren, die vom selben Browser stammen. Um die Modellergebnisse zu verbessern, kann in Azure ML-Empfehlungen eine eindeutige Kennung für Benutzer festgelegt werden, die die Verwendung von Cookies außer Kraft setzt.

Dieses Ereignis sollte nach der Benutzeranmeldung auf Ihrer Website verwendet werden.

Parameter:

* event (Zeichenfolge) – „userlogin“
* user (Zeichenfolge) – eindeutige Identifikation des Benutzers
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "userlogin", user: “ABCD10AA” });
        </script>

## <a name="4-consume-recommendations-via-javascript"></a>4. Nutzen von Empfehlungen über JavaScript
Der Code, der die Empfehlung nutzt, wird durch ein JavaScript-Ereignis von der Client-Webseite ausgelöst. Die Antwort mit der Empfehlung enthält die Ids der empfohlenen Elemente, deren Namen und ihre Bewertungen. Es empfiehlt sich, diese Option nur für eine Listenanzeige der empfohlenen Elemente zu verwenden. Eine komplexere Aufbereitung (wie z.B. das Hinzufügen von Metadaten des Elements) sollte in der serverseitigen Integration erledigt werden.

### <a name="41-consume-recommendations"></a>4.1 Nutzen der Empfehlungen
Um Empfehlungen nutzen zu können, müssen Sie die erforderlichen JavaScript-Bibliotheken in die Seite einbinden und AzureMLRecommendationsStart aufrufen. Siehe Abschnitt 2.

Um Empfehlungen für ein oder mehrere Elemente zu nutzen, müssen Sie eine Methode namens AzureMLRecommendationsGetI2IRecommendation aufrufen.

Parameter:

* items (Zeichenfolgenarray) – ein oder mehrere Elemente, für die Empfehlungen abgerufen werden sollen. Wenn Sie einen Fbt-Build nutzen, können Sie hier nur ein Element festlegen.
* numberOfResults (Ganzzahl) – Anzahl der erforderlichen Ergebnisse
* includeMetadata (Boolesch, optional) – wenn der Wert auf „true“ festgelegt ist, weist dies darauf hin, dass das Metadatenfeld im Ergebnis aufgefüllt werden muss
* Verarbeitungsfunktion – eine Funktion, die die zurückgegebenen Empfehlungen verarbeitet. Die Daten werden als Array folgender Werte zurückgegeben:
  * Item – eindeutige Element-ID
  * name – Elementname (sofern im Katalog vorhanden)
  * rating – Empfehlungsbewertung
  * metadata – eine Zeichenfolge, die die Metadaten des Elements repräsentiert

Beispiel: Mit dem folgenden Code werden acht Empfehlungen für das Element „64f6eb0d-947a-4c18-a16c-888da9e228ba“ angefordert (und da includeMetadata nicht angegeben wird, besagt dies implizit, dass keine Metadaten erforderlich sind) und die Ergebnisse in einen Puffer verkettet.

        <script>
             var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                 var buff = "";
                 for (var ii = 0; ii < reco.length; ii++) {
                       buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                 }
                 alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png



<!--HONumber=Dec16_HO2-->


