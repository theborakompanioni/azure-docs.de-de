<properties
   pageTitle="Skalieren von Webdiensten | Microsoft Azure"
   description="Erfahren Sie, wie Sie einen Webdienst skalieren, indem Sie die Parallelität erhöhen und neue Endpunkte hinzufügen."
   services="machine-learning"
   documentationCenter=""
   authors="neerajkh"
   manager="srikants"
   editor="cgronlun"
   keywords="Azure Machine Learning, Webdienste, Operationalisierung, Skalierung, Endpunkt, Parallelität"
   />
<tags
   ms.service="machine-learning"
   ms.devlang="NA"
   ms.workload="data-services"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.date="07/06/2016"
   ms.author="neerajkh"/>

# Skalieren eines Webdiensts

>[AZURE.NOTE] In diesem Thema werden für einen klassischen Webdienst geltende Verfahren beschrieben.

Standardmäßig ist jeder veröffentlichte Webdienst darauf konfiguriert, 20 Anforderungen gleichzeitig zu unterstützen, wobei maximal 200 Anforderungen gleichzeitig unterstützt werden können. Während das klassische Azure-Portal eine Möglichkeit zum Festlegen dieses Werts bietet, optimiert Azure Machine Learning diese Einstellung automatisch, um die beste Leistung für Ihren Webdienst zu bieten. Der Portalwert wird ignoriert.

Wenn Sie die API stärker als mit dem unterstützen Maximalwert von 200 gleichzeitigen Aufrufen auslasten möchten, müssen Sie mehrere Endpunkte im gleichen Webdienst erstellen und die Last wahllos auf alle Endpunkte verteilen.

## Hinzufügen von neuen Endpunkten für denselben Webdienst

Das Skalieren eines Webdiensts ist eine gängige Aufgabe zum Unterstützen von mehr als 200 gleichzeitigen Anforderungen, zum Erhöhen der Verfügbarkeit durch mehrere Endpunkte oder zum Bereitstellen eines separaten Endpunkts für andere Consumer des Webdiensts. Sie können die Skalierung erhöhen, indem Sie über das [klassische Azure-Portal](https://manage.windowsazure.com/) zusätzliche Endpunkte für den gleichen Webdienst hinzufügen (siehe die folgende Abbildung):

Bedenken Sie, dass sich die Verwendung einer hohen Anzahl gleichzeitiger Aufrufe nachteilig auswirken kann, wenn die API nicht entsprechend ausgelastet wird. Es können sporadische Timeouts und/oder Spitzen in die Latenz auftreten, wenn eine für eine hohe Auslastung konfigurierte API relativ gering ausgelastet wird.

Die synchronen APIs werden in der Regel in Situationen verwendet, in denen eine niedrige Latenz erwünscht ist. Mit Latenz ist hier der Zeitraum gemeint, der von der API benötigt wird, um eine Anforderung abzuschließen, wobei keinerlei Netzwerkverzögerungen berücksichtigt werden. Angenommen, Sie haben eine API mit einer Latenz von 50 ms (Millisekunden). Um die verfügbare Kapazität mit der Drosselungsstufe High (Hoch) und einer Höchstzahl gleichzeitiger Anrufe = 20 voll nutzen zu können, müssen Sie diese API 20 * 1000 / 50 = 400 Mal pro Sekunde aufrufen. Wenn man dieses Beispiel fortspinnt, ermöglicht eine Höchstzahl von 200 gleichzeitigen Aufrufen 4000 Aufrufe der API pro Sekunde, sofern die Latenz 50 ms beträgt.

Wechseln Sie zum [klassischen Azure-Portal](https://manage.windowsazure.com/), klicken Sie auf das Machine Learning-Symbol auf der linken Seite, wählen Sie den Arbeitsbereich für die Veröffentlichung des Webdiensts aus, klicken Sie auf den gewünschten Webdienst, wählen Sie im unteren Bereich **ENDPUNKT HINZUFÜGEN** aus, und geben Sie dann einen Namen, eine Beschreibung und die gewünschte Währung für den neuen Endpunkt an.

Informationen zum Hinzufügen neuer Endpunkte finden Sie unter [Erstellen von Endpunkten](machine-learning-create-endpoint.md).

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png

<!---HONumber=AcomDC_0720_2016-->