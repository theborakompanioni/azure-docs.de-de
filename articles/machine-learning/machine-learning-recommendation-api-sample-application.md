---
title: "Allgemeine Vorgänge in der Machine Learning-Empfehlungen-API | Microsoft Docs"
description: Azure ML Recommendation-Beispielanwendung
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 0220bc17-3315-47d7-84a3-ef490263a343
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.translationtype: Human Translation
ms.sourcegitcommit: 29c718d0c34d1e2f9d17b285a7270541a9ff15cf
ms.openlocfilehash: 9afbae9b7c24c72cbb2ef64e693f7a317dc81a46
ms.contentlocale: de-de
ms.lasthandoff: 02/24/2017


---
# <a name="recommendations-api-sample-application-walkthrough"></a>Die Azure Machine Learning-Empfehlungen-API anhand einer Beispielanwendung
> [!NOTE]
> Beginnen Sie mit der Nutzung der Empfehlungs-API des Cognitive Service anstatt mit dieser Version. Der Recommendations Cognitive Service wird diesen Dienst ersetzen, weshalb alle neuen Features dafür entwickelt werden. Der Dienst bietet neue Funktionen wie Unterstützung der Batchverarbeitung, einen besseren API-Explorer, eine übersichtlichere API-Oberfläche, eine einheitlicherere Registrierungs-/Abrechnungsumgebung usw.
> Erfahren Sie mehr zur [Migration zum neuen Cognitive Service](http://aka.ms/recomigrate)
> 
> 

## <a name="purpose"></a>Zweck
Dieses Dokument zeigt die Verwendung der Azure Machine Learning RECOMMENDATIONS-API anhand einer [Beispielanwendung](https://code.msdn.microsoft.com/Recommendations-144df403).

Diese Anwendung ist nicht dazu vorgesehen, vollständige Funktionen zu enthalten oder alle APIs zu verwenden. Sie soll nur einige allgemeine Anwendungen zeigen, wenn Sie den Azure Machine Learning-Empfehlungsdienst zum ersten Mal verwenden möchten. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="introduction-to-machine-learning-recommendation-service"></a>Einführung zum Machine Learning-Empfehlungsdienst
Die Empfehlungen über den Azure Machine Learning-Empfehlungsdienst sind aktiviert, wenn Sie ein Empfehlungsmodell anhand der folgenden Daten erstellen:

* Ein Repository für die zu empfehlenden Elemente, auch als Katalog bezeichnet
* Daten, die der Nutzung der Elemente pro Benutzer oder Sitzungen entsprechen (diese können im Lauf der Zeit und nicht als Teil der Beispiel-App gesammelt werden)

Mit dem erstellten Empfehlungsmodell können Sie Elemente vorhersagen, für die sich ein Benutzer basierend auf einer Reihe der von ihm ausgewählten Elemente (oder einem einzelnen Element) möglicherweise interessiert.

Um dieses Szenario zu aktivieren, gehen Sie im Machine Learning-Empfehlungsdienst folgendermaßen vor:

* Erstellen eines Modells: Dies ist ein logischer Container mit den Daten (Katalog und Nutzung) und den Vorhersagemodellen. Jeder Modellcontainer wird über eine eindeutige ID identifiziert, die beim Erstellen zugewiesen wird. Diese ID wird als Modell-ID bezeichnet und für die meisten APIs verwendet. 
* Hochladen in den Katalog: Nachdem ein Modellcontainer erstellt wurde, kann diesem ein Katalog zugeordnet werden.

**Hinweis**: Das Erstellen eines Modells und das Hochladen des Katalogs erfolgt in der Regel einmal pro Lebenszyklus eines Modells.

* Hochladen der Nutzung: Hiermit werden dem Modellcontainer Nutzungsdaten hinzugefügt.
* Erstellen eines Empfehlungsmodells: Wenn Sie über ausreichend Daten verfügen, können Sie das Empfehlungsmodell erstellen. Für diesen Vorgang werden modernste Algorithmen für maschinelles Lernen verwendet, um das Empfehlungsmodell zu erstellen. Jedem Build ist eine eindeutige ID zugeordnet Sie müssen diese ID aufzeichnen, da sie für die Funktionen einiger APIs erforderlich ist.
* Überwachen des Erstellungsvorgangs: Das Erstellen eines Empfehlungsmodells ist ein asynchroner Vorgang, der einige Minuten oder bis zu mehrere Stunden dauern kann. Dies ist abhängig von der Datenmenge (Katalog und Nutzung) und den Buildparametern. Aus diesem Grund müssen Sie den Build überwachen. Ein Empfehlungsmodell wird nur dann erstellt, wenn der zugeordnete Build erfolgreich abgeschlossen wurde.
* (Optional:) Wählen Sie einen aktiven Empfehlungsmodell-Build aus. Dieser Schritt ist nur erforderlich, wenn Ihr Modellcontainer mehr als ein Empfehlungsmodell umfasst. Alle Empfehlungsanforderungen ohne Angabe des aktiven Empfehlungsmodells werden automatisch zum aktiven Standardbuild umgeleitet. 

**Hinweis:**Ein aktives Empfehlungsmodell ist einsatzbereit und für die Produktionslast ausgelegt. Dies ist ein Unterschied zu einem inaktiven Empfehlungsmodell, das in einer Art Testumgebung verbleibt (mitunter als "Staging" bezeichnet).

* Abrufen von Empfehlungen: Wenn Sie über ein Empfehlungsmodell verfügen, können Sie Empfehlungen für einzelne ausgewählte Elemente oder eine Elementliste erstellen. 

In der Regel rufen Sie „Get recommendation“ über einen bestimmten Zeitraum auf. Während dieses Zeitraums können Sie die Nutzungsdaten an das Machine Learning-Empfehlungssystem umleiten, damit diese Daten dem angegebenen Modellcontainer hinzugefügt werden. Wenn Sie über ausreichend Nutzungsdaten verfügen, können Sie ein neues Empfehlungsmodell erstellen, das die zusätzlichen Nutzungsdaten integriert. 

## <a name="prerequisites"></a>Voraussetzungen
* Visual Studio 2013 oder höher
* Zugriff auf das Internet 
* Abonnement für die Empfehlungen-API (https://datamarket.azure.com/dataset/amla/recommendations).

## <a name="azure-machine-learning-sample-app-solution"></a>Beispielanwendungslösung für Azure Machine Learning
Die Lösung enthält den Quellcode, eine Beispielnutzung, eine Katalogdatei und Anweisungen zum Herunterladen der für die Kompilierung erforderlichen Nuget-Pakete.

## <a name="the-apis-used"></a>Die verwendeten APIs
Die Anwendung verwendet die Machine Learning-Empfehlungsfunktionen über eine Teilmenge der verfügbaren APIs. Die folgenden APIs werden in der Anwendung veranschaulicht:

* Create model: Erstellen Sie einen logischen Container für Daten und Empfehlungsmodelle. Ein Modell wird anhand eines Namens identifiziert. Sie können jeweils nur ein Modell mit dem gleichen Namen erstellen.
* Upload catalog file: wird zum Hochladen von Katalogdaten verwendet.
* Upload usage file: wird zum Hochladen der Nutzungsdaten verwendet.
* Auslösen des Builds: wird zum Erstellen eines Empfehlungsmodells verwendet.
* Trigger build: wird zum Überwachen des Status eines Empfehlungsmodell-Builds verwendet.
* Choose a built model for recommendation: wird verwendet, um anzugeben, welches Empfehlungsmodell standardmäßig für einen bestimmten Modellcontainer verwendet werden soll. Dieser Schritt ist nur dann erforderlich, wenn Sie über mehrere Empfehlungsmodelle verfügen und einen inaktiven Build als aktives Empfehlungsmodell aktivieren möchten.
* Get recommendation: wird zum Abrufen empfohlener Elemente anhand eines bestimmten Einzelelements oder eines Elementsatzes verwendet. 

Eine vollständige Beschreibung der APIs finden Sie in der Dokumentation zum Microsoft Azure Marketplace. 

**Hinweis:**Ein Modell kann im Lauf der Zeit über mehrere Builds verfügen (nicht jedoch gleichzeitig). Jeder Build wird mit demselben oder einem aktualisierten Katalog und zusätzlichen Nutzungsdaten erstellt.

## <a name="common-pitfalls"></a>Häufige Probleme
* Sie müssen Ihren Benutzernamen und den Microsoft Azure Marketplace-Hauptkontoschlüssel angeben, um die Beispiel-App auszuführen.
* Die Beispiel-App kann nicht mehrfach hintereinander ausgeführt werden. Der Anwendungsfluss umfasst das Erstellen, das Hochladen, das Aufbauen des Bildschirms sowie das Abrufen von Empfehlungen von einem vordefinierten Modell. Daher schlägt eine fortlaufende Ausführung fehl, wenn Sie zwischen den Aufrufen nicht den Modellnamen ändern.
* Empfehlungen werden möglicherweise ohne Daten zurückgegeben. Die Beispiel-App verwendet einen sehr kleinen Katalog. Dies gilt auch für die Nutzungsdatei. Daher verfügen einige Elemente des Katalogs nicht über empfohlene Elemente.

## <a name="disclaimer"></a>Haftungsausschluss
Die Beispiel-App ist nicht für die Ausführung in einer Produktionsumgebung gedacht. Der Katalog umfasst nur sehr wenige Daten, die kein sinnvolles Empfehlungsmodell ergeben. Die Daten werden als Beispiel bereitgestellt. 


