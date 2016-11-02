<properties 
    pageTitle="Erstellen von Webdienst-Endpunkten in Machine Learning | Microsoft Azure" 
    description="Erstellen von Webdienst-Endpunkten in Azure Machine Learning" 
    services="machine-learning" 
    documentationCenter="" 
    authors="hiteshmadan" 
    manager="padou" 
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd" 
    ms.date="10/04/2016"
    ms.author="himad"/>



# <a name="creating-endpoints"></a>Erstellen von Endpunkten

>[AZURE.NOTE] In diesem Thema werden für einen klassischen Webdienst geltende Verfahren beschrieben.

Wenn Sie Webdienste erstellen, die Sie an Ihre Kunden weiter verkaufen, müssen Sie jedem Kunden trainierte Modelle zur Verfügung stellen, die weiter mit dem Experiment verknüpft sind, anhand dessen der Webdienst erstellt wurde. Darüber hinaus müssen Aktualisierungen am Experiment selektiv auf einen Endpunkt angewendet werden, ohne die Anpassungen zu überschreiben.

Hierzu ermöglicht Ihnen Azure Machine Learning, mehrere Endpunkte für einen bereitgestellten Webdienst zu erstellen. Jeder Endpunkt im Webdienst wird unabhängig adressiert, gedrosselt und verwaltet. Zu jedem Endpunkt gehört eine eindeutige URL und ein Autorisierungsschlüssel zur Verteilung an Ihre Kunden.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Hinzufügen von Endpunkten zu einem Webdienst

Es gibt drei Möglichkeiten zum Hinzufügen eines Endpunkts zu einem Webdienst.

* Programmgesteuert
* Über das Azure Machine Learning Web Services-Portal
* Über das klassische Azure-Portal

Nach dem Erstellen kann der Endpunkt über synchrone APIs, Batch-APIs und Excel-Arbeitsblätter genutzt werden. Zusätzlich zum Hinzufügen von Endpunkten über diese Benutzeroberfläche können Sie auch die Endpunktverwaltungs-APIs verwenden, um Endpunkte programmgesteuert hinzuzufügen. 
 
 >[AZURE.NOTE] Wenn Sie dem Webdienst zusätzliche Endpunkte hinzugefügt haben, können Sie den Standardendpunkt nicht löschen.

## <a name="adding-an-endpoint-programmatically"></a>Programmgesteuertes Hinzufügen eines Endpunkts

Sie können Ihrem Webdienst einen Endpunkt mithilfe des Beispielcodes [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) programmgesteuert hinzufügen.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Hinzufügen eines Endpunkts über das Azure Machine Learning Web Services-Portal

1. Klicken Sie in Machine Learning Studio links auf „Webdienste“.
2. Klicken Sie unten im Dashboard des Webdiensts auf **Manage endpoints**. Das Azure Machine Learning Web Services-Portal wird mit der Seite mit den Endpunkten für den Webdienst geöffnet.
3. Klicken Sie auf **New**.
4. Geben Sie einen Namen und eine Beschreibung für den neuen Endpunkt ein. Endpunktnamen dürfen maximal 24 Zeichen lang sein und müssen aus Kleinbuchstaben oder Zahlen bestehen. Wählen Sie die Protokollierungsstufe aus, und legen Sie fest, ob Beispieldaten aktiviert sind. Weitere Informationen zur Protokollierung finden Sie unter [Aktivieren der Protokollierung für Machine Learning-Webdienste](machine-learning-Web-services-logging.md).

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a>Hinzufügen eines Endpunkts über das klassische Azure-Portal


1. Melden Sie sich beim [klassischen Azure-Portal](http://manage.windowsazure.com) an, und klicken Sie in der linken Spalte auf **Machine Learning**. Klicken Sie auf den Arbeitsbereich, der den Webdienst enthält, an dem Sie interessiert sind.

    ![Navigieren Sie zum Arbeitsbereich](./media/machine-learning-create-endpoint/figure-1.png)

2. Klicken Sie auf **Webdienste**.

    ![Navigieren zu Webdiensten](./media/machine-learning-create-endpoint/figure-2.png)

3. Klicken Sie auf den Webdienst, für den Sie sich interessieren, um die Liste der verfügbaren Endpunkte anzuzeigen.

    ![Navigieren Sie zum Endpunkt](./media/machine-learning-create-endpoint/figure-3.png)

4. Klicken Sie unten auf der Seite auf **Endpunkt hinzufügen**. Geben Sie einen Namen und eine Beschreibung ein, und stellen Sie sicher, dass keine anderen Endpunkte mit demselben Namen in diesem Webdienst vorhanden sind. Behalten Sie die Standardeinstellung des Drosselungsgrads bei, sofern keine besonderen Anforderungen bestehen. Wenn Sie mehr über Drosselung erfahren möchten, besuchen Sie die Seite [Skalieren von API-Endpunkten](machine-learning-scaling-Webservice.md).

    ![Endpunkt erstellen](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a>Nächste Schritte

[Nutzen eines veröffentlichten Azure Machine Learning-Webdiensts](machine-learning-consume-Web-services.md) 


<!--HONumber=Oct16_HO2-->


