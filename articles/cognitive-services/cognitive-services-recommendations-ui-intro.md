---
title: "Erstellen eines Modells mit der Empfehlungsbenutzeroberfläche | Microsoft Docs"
description: "Azure Machine Learning-Empfehlungen – Erstellen eines Modells mit der Empfehlungs-Benutzeroberfläche"
services: cognitive-services
documentationcenter: 
author: luiscabrer
manager: jhubbard
editor: cgronlun
ms.assetid: b264fe44-f94e-40ae-9754-60ad7d6cfeb9
ms.service: cognitive-services
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2016
ms.author: luisca
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 5f98395e2beba671192e50f8e53c6198e1efed29


---
# <a name="building-a-model-with-the-recommendations-ui"></a>Erstellen eines Modells mit der Empfehlungs-Benutzeroberfläche
Dieses Dokument ist eine schrittweise Anleitung. Unser Ziel ist es, Sie durch die erforderlichen Schritte zum Trainieren eines Modells mithilfe der [Empfehlungs-Benutzeroberfläche](https://recommendations-portal.azurewebsites.net/) zu führen.
Beim Durcharbeiten dieser Übung lernen Sie den Prozess zum Erstellen eines Prozess kennen, bevor Sie dies programmgesteuert durchführen. Sie erhalten auch Informationen zur Benutzeroberfläche, was sehr praktisch ist, da Sie beginnen, mit dem Dienst zu arbeiten.

Diese Übung dauert etwa 30 Minuten.

<a name="Step1"></a>

## <a name="step-1---sign-in-to-the-recommendations-ui"></a>Schritt 1 – Anmelden bei der Empfehlungs-Benutzeroberfläche
1. Wenn Sie es nicht schon getan haben, müssen Sie sich für ein neues [Empfehlungs-API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api)-Abonnement [registrieren](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1). Sie können sich für den Dienst unter **Azure** unter [http://portal.azure.com/](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) registrieren und sich mit Ihrem Azure-Konto anmelden. Ausführliche Anweisungen zum Registrierungsprozess stehen unter *Aufgabe 1* in der [Kurzanleitung für die Empfehlungs-API der kognitiven Dienste](cognitive-services-recommendations-quick-start.md) bereit. 
2. Sobald Sie einen **Schlüssel** für Ihr Empfehlungs-API-Abonnement erhalten haben, gehen Sie zur [Empfehlungs-Benutzeroberfläche](https://recommendations-portal.azurewebsites.net/). 
3. Melden Sie sich im Portal an, indem Sie Ihren Schlüssel in das Feld **Kontoschlüssel** eingeben und dann auf die Schaltfläche **Anmeldung** klicken.
   
    ![Empfehlungs-Benutzeroberfläche: Anmeldedialogfeld][reco_signin]

<a name="Step2"></a>

## <a name="step-2---lets-gather-some-training-data"></a>Schritt 2: Sammeln von Trainingsdaten
Bevor Sie einen Build erstellen können, benötigt das Modul zwei Angaben: eine Katalogdatei und einen Satz von Nutzungsdateien. 

Die Katalogdatei enthält Informationen zu den Artikeln, die Sie Ihren Kunden anbieten. Eine Nutzungsdatei enthält Informationen zur Verwendungsweise dieser Artikel oder zu den Transaktionen Ihres Geschäfts.

In der Regel würden Sie Ihre lokale Datenbank nach diesen Informationen abfragen. Heute haben wir einige Beispieldaten für Sie bereitgestellt, damit Sie lernen, wie die Empfehlungs-API verwendet wird.

Sie können die Daten hier herunterladen: [http://aka.ms/RecoSampleData](http://aka.ms/RecoSampleData). Kopieren Sie die Datei **Books.Zip** in einen Ordner Ihres lokalen Computers, und entpacken Sie sie dort. Zum Beispiel **c:\data**.

Ausführliche Informationen zum Schema der Katalog- und Nutzungsdateien finden Sie im Artikel [Erfassen von Daten zum Trainieren Ihres Modells](cognitive-services-recommendations-collecting-data.md).

In dieser Übung werden wir mit einer kleinen Datei arbeiten, sodass Sie nicht sehr lange auf das Training warten müssen. Wenn Sie eine realistischere Datei ausprobieren möchten, dann verwenden Sie die von uns bereitgestellte Datei **MsStoreData.zip**, die Beispieltransaktionen aus dem Microsoft Store [im selben Speicherort](http://aka.ms/RecoSampleData) enthält.

<a name="Step3"></a>

## <a name="step-3---create-a-project-and-upload-catalog-and-usage-data"></a>Schritt 3 – Erstellen eines Projekts und Hochladen von Katalog- und Nutzungsdaten
Nach der Anmeldung bei der [Empfehlungs-Benutzeroberfläche](https://recommendations-portal.azurewebsites.net/) wird die Seite „Projekte“ angezeigt. Wenn Sie zuvor Projekte erstellt haben, sollten diese hier angezeigt werden.
Ein Projekt (auch bekannt als *ein Modell* in der API-Referenz) ist ein Container für die Katalog- und Benutzerdaten. Sie können innerhalb des Projekts verschiedene *Builds* erstellen. Sie werden in den folgenden Schritten durch diesen Prozess geführt:

1. Um ein neues Projekt zu erstellen, geben Sie den Namen in das Textfeld ein (Etwas wie „MyFirstModel“ würde funktionieren), und klicken Sie auf **Projekt hinzufügen**.
   
    ![Empfehlungs-Benutzeroberfläche: Seite „Projekte“][reco_projects]
2. Wenn das Projekt erstellt wird, klicken Sie auf die Schaltfläche **Datei suchen** im Abschnitt **Hinzufügen eine Katalogdatei**. 
   Laden Sie den Katalog hoch, den Sie in Schritt 2 erhalten haben. Wenn Sie ihn unter *c:\data* gespeichert haben, müssen Sie zu diesem Ordner navigieren.
   
     ![Empfehlungs-Benutzeroberfläche: Hinzufügen von Daten zu einem Projekt][reco_firstmodel]
3. Nachdem der Katalog hochgeladen wurde, klicken Sie auf die Schaltfläche **Datei suchen** im Abschnitt **Hinzufügen von Nutzungsdateien**. Fügen Sie die Datei „usage_large.txt“ hinzu.

> **Was geschieht, wenn ich eine große Datei mit Verwendungsdaten besitze?**
> 
> Nur Nutzungsdateien, die kleiner als 200 MB sind, können hochgeladen werden. Andererseits kann das System bis zu 2 GB Transaktionsdaten aufnehmen, damit Sie bei Bedarf mehr als eine Datei hochladen können.
> Sie benötigen womöglich nicht so viele Daten, um ein gutes Modell hochzuladen. Es ist nicht nur die Größe der Daten wichtig, aber auch die Qualität der Daten. Häufig werden Nutzungsdaten angezeigt, bei denen die meisten Transaktionen nur für einige beliebte Artike erfolgen und nur ein „kleines Signal“ für andere Artikel vorhanden ist.
> 
> 

<a name="Step4"></a>

## <a name="step-4---lets-do-some-training"></a>Schritt 4 – Lassen Sie uns mit dem Training beginnen!
Da Sie nun den Katalog- und die Nutzungsdaten hochgeladen haben, können wir mit dem Trainieren des Moduls beginnen, damit es Muster aus unseren Daten lernen kann.

1. Klicken Sie auf die Schaltfläche **New Build** (Neuer Build).
2. Wählen Sie **Empfehlungen** als Buildtyp aus. Beachten Sie, dass wir ein Rangfolgebuild und FTB-Buildtypen (Frequently Bought Together, Häufig zusammen gekauft) ebenfalls unterstützen.
   
   ![Empfehlungs-Benutzeroberfläche: Build-Dialogfeld][reco_build_dialog.png]

    Mit einem FTB-Build können Sie Muster für Produkte erkennen, die in der Regel in derselben Transaktion gekauft/verbraucht werden.
    Ein Rangfolgebuild wird verwendet, um interessante Funktionen zu identifizieren. 
    Wir vertiefen das Thema FBT oder Rangfolgebuilds in diesem Workshop nicht zu sehr, doch wenn Sie daran interessiert sind, sollten Sie sich die Seite [Buildtypen und Modellqualität](cognitive-services-recommendations-buildtypes.md) ansehen.

1. Klicken Sie auf die Schaltfläche **Build**. Der Buildprozess nimmt etwa fünf Minuten in Anspruch, wenn Sie Daten von Büchern verwenden. Bei größeren Datasets dauert es länger.

<a name="Step5"></a>

## <a name="step-5---lets-find-out-what-the-machine-learned"></a>Schritt 5 – Herausfinden, was der Computer gelernt hat
Wenn der Build abgeschlossen ist, sehen Sie einen neuen Build in der Liste der Builds. Dieser Build kann für Artikel und Benutzerempfehlungen abgefragt werden.

1. Nachdem Ihr Build abgeschlossen ist, klicken Sie auf **Score** (Bewertung). Dadurch können Sie mit dem Modell spielen und herausfinden, welche Artikel empfohlen werden.
   
    ![Empfehlungs-Benutzeroberfläche: Schaltfläche „Score“ (Bewertung)][reco_score_button]
2. Wählen Sie ein Element aus, um festzustellen, welche Elemente als Empfehlungen für dieses Element zurückgegeben werden. Beachten Sie, dass wenn zu wenige Transaktionen vorhanden sind, um eine Empfehlung für einen bestimmten Artikel vorherzusagen, gibt das System keine Empfehlungen für diesen Artikel zurück.  Wenn Sie aus irgendeinem Grund über einen Artikel verfügen, der keine Empfehlungen zurückgibt, versuchen Sie, andere Artikel zu bewerten.

<a name="Step6"></a>

## <a name="step-6---next-steps"></a>Schritt 6 – Nächste Schritte
Glückwunsch! Sie haben ein Modell trainiert und dann haben Sie Empfehlungen von diesem Modell erhalten.  Die Empfehlungs-Benutzeroberfläche ist ein nützliches Tool, mit dem Sie den Status Ihres Projekts und Builds anzeigen können. 

Da Sie nun über ein Modell verfügen, möchten Sie möglicherweise erfahren, wie Sie alle oben genannten Schritte programmgesteuert durchführen. Um zu erfahren, wie Sie die API programmgesteuert aufrufen, laden wir Sie ein, die [Empfehlungs-API-Referenz (in englischer Sprache)](http://go.microsoft.com/fwlink/?LinkId=759348) zu lesen, und sich die [Empfehlungs-Beispielanwendung](http://go.microsoft.com/fwlink/?LinkID=759344) herunterzuladen.

[reco_signin]:../media/cognitive-services/reco_signin.PNG
[reco_projects]:../media/cognitive-services/reco_projects.PNG
[reco_firstmodel]:../media/cognitive-services/reco_firstmodel.png
[reco_build_dialog.png]:../media/cognitive-services/reco_build_dialog.png
[reco_score_button]:../media/cognitive-services/reco_score_button.png



<!--HONumber=Feb17_HO3-->


