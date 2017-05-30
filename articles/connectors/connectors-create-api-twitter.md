---
title: Informationen zum Verwenden des Twitter-Connectors in Logik-Apps | Microsoft Docs
description: "Übersicht über den Twitter-Connector mit REST-API-Parametern"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8bce2183-544d-4668-a2dc-9a62c152d9fa
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: mandia; ladocs
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 7b4e14ff2e7e6575574e9da412d29cc2c228990f
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="get-started-with-the-twitter-connector"></a>Erste Schritte mit dem Twitter-Connector
Der Twitter-Connector ermöglicht Folgendes:

* Tweets posten und abrufen
* Auf Timelines, Freunde und Follower zugreifen
* Andere Aktionen und Trigger ausführen, die weiter unten beschrieben sind  

Wenn Sie [einen Connector](apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.  

## <a name="connect-to-twitter"></a>Verbinden mit Twitter
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine *Verbindung* mit dem Dienst herstellen. Eine [Verbindung](connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.  

### <a name="create-a-connection-to-twitter"></a>Erstellen einer Verbindung mit Twitter
> [!INCLUDE [Steps to create a connection to Twitter](../../includes/connectors-create-api-twitter.md)]
> 
> 

## <a name="use-a-twitter-trigger"></a>Verwenden eines Twitter-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

In diesem Beispiel zeige ich Ihnen, wie der Trigger **Wenn ein neuer Tweet gepostet wird** verwendet wird, um nach #Seattle zu suchen, und wie eine Datei in Dropbox mit dem Text aus dem Tweet aktualisiert wird, wenn #Seattle gefunden wird. In einem Unternehmensbeispiel könnten Sie nach dem Namen Ihres Unternehmens suchen und eine SQL-Datenbank mit dem Text aus dem Tweet aktualisieren.

1. Geben Sie *Twitter* im Suchfeld im Designer für Logik-Apps ein, und wählen Sie dann den Trigger **Twitter – Wenn ein neuer Tweet gepostet wird** aus.   
   ![Twitter-Trigger – Abbildung 1](./media/connectors-create-api-twitter/trigger-1.png)  
2. Geben Sie *#Seattle* in das Steuerelement **Suchtext** ein.  
   ![Twitter-Trigger – Abbildung 2](./media/connectors-create-api-twitter/trigger-2.png) 

Jetzt haben Sie Ihre Logik-App mit einem Trigger konfiguriert, der eine Ausführung der anderen Trigger und Aktionen im Workflow startet. 

> [!NOTE]
> Damit eine Logik-App funktionsfähig ist, muss sie mindestens einen Trigger und eine Aktion enthalten. Führen Sie die Schritte im nächsten Abschnitt aus, um eine Aktion hinzuzufügen.  
> 
> 

## <a name="add-a-condition"></a>Bedingung hinzufügen
Da uns nur Tweets von Benutzern mit mehr als 50 Followern interessieren, muss zunächst eine Bedingung, die die Anzahl der Follower überprüft, zur Logik-App hinzugefügt werden.  

1. Wählen Sie **+ Neuer Schritt** aus, um die Aktion hinzuzufügen, die ausgeführt werden soll, wenn #Seattle in einem neuen Tweet gefunden wird.  
   ![Twitter-Aktion – Abbildung 1](../../includes/media/connectors-create-api-twitter/action-1.png)  
2. Wählen Sie den Link **Bedingung hinzufügen** aus.  
   ![Twitter-Bedingung – Abbildung 1](../../includes/media/connectors-create-api-twitter/condition-1.png)   
   Daraufhin wird das Steuerelement **Bedingung** geöffnet, in dem Sie Bedingungen wie z.B. *ist gleich*, *ist kleiner als*, *ist größer als* und *enthält* auswählen können.  
   ![Twitter-Bedingung – Abbildung 2](../../includes/media/connectors-create-api-twitter/condition-2.png)   
3. Wählen Sie das Steuerelement **Wert auswählen** aus.  
   In diesem Steuerelement können Sie eine oder mehrere Eigenschaften von vorherigen Aktionen oder Triggern als den Wert auswählen, dessen Bedingung als wahr oder falsch ausgewertet wird.
   ![Twitter-Bedingung – Abbildung 3](../../includes/media/connectors-create-api-twitter/condition-3.png)   
4. Wählen Sie **...** aus, um die Liste der Eigenschaften zu erweitern, damit Sie alle verfügbaren Eigenschaften sehen können.        
   ![Twitter-Bedingung – Abbildung 4](../../includes/media/connectors-create-api-twitter/condition-4.png)   
5. Wählen Sie die Eigenschaft **Anzahl von Followern** aus.    
   ![Twitter-Bedingung – Abbildung 5](../../includes/media/connectors-create-api-twitter/condition-5.png)   
6. Beachten Sie, dass die Eigenschaft für die Anzahl von Followern jetzt in das Steuerelement für den Wert aufgenommen wurde.    
   ![Twitter-Bedingung – Abbildung 6](../../includes/media/connectors-create-api-twitter/condition-6.png)   
7. Wählen Sie **ist größer als** aus der Liste der Operatoren aus.    
   ![Twitter-Bedingung – Abbildung 7](../../includes/media/connectors-create-api-twitter/condition-7.png)   
8. Geben Sie „50“ als Operanden für den Operator *ist größer als* ein.  
   Die Bedingung wird hinzugefügt. Speichern Sie Ihre Arbeit mit dem Link **Speichern** oben im Menü.    
   ![Twitter-Bedingung – Abbildung 8](../../includes/media/connectors-create-api-twitter/condition-8.png)   

## <a name="use-a-twitter-action"></a>Verwenden einer Twitter-Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

Nachdem Sie nun einen Trigger hinzugefügt haben, gehen Sie wie folgt vor, um eine Aktion hinzuzufügen, mit der ein neuer Tweet gepostet wird, der den Inhalt der vom Trigger gefundenen Tweets enthält. Im Rahmen dieser exemplarischen Vorgehensweise werden nur die Tweets von Benutzern mit mehr als 50 Followern gepostet.  

Im nächsten Schritt fügen Sie eine Twitter-Aktion hinzu. Diese Aktion postet Tweets auf der Grundlage von Eigenschaften aller Tweets, die von Benutzern mit jeweils mehr als 50 Followern gepostet wurden.  

1. Wählen Sie **Aktion hinzufügen**aus. Daraufhin wird das Steuerelement für die Suche geöffnet, in dem Sie nach anderen Aktionen und Triggern suchen können.  
   ![Twitter-Bedingung – Abbildung 9](../../includes/media/connectors-create-api-twitter/condition-9.png)   
2. Geben Sie *Twitter* in das Suchfeld ein, und wählen Sie die Aktion **Twitter - Tweet posten** aus. Daraufhin wird das Steuerelement **Tweet posten** geöffnet, in das Sie alle Details für den zu postenden Tweet eingeben.      
   ![Twitter-Aktion – Abbildung 1-5](../../includes/media/connectors-create-api-twitter/action-1-5.png)   
3. Wählen Sie das Steuerelement **Tweettext** aus. Alle Ausgaben früherer Aktionen und Trigger in der Logik-App werden jetzt angezeigt. Sie können beliebige davon auswählen und als Teil des Tweettexts des neuen Tweets verwenden.     
   ![Twitter-Aktion – Abbildung 2](../../includes/media/connectors-create-api-twitter/action-2.png)   
4. Wählen Sie **Benutzername** aus.   
5. Geben Sie in das Steuerelement für den Tweettext *sagt:* direkt nach dem Benutzernamen ein.  
6. Wählen Sie *Tweettext* aus.       
   ![Twitter-Aktion – Abbildung 3](../../includes/media/connectors-create-api-twitter/action-3.png)   
7. Speichern Sie Ihre Arbeit, und senden Sie einen Tweet mit dem Hashtag #Seattle, um den Workflow zu aktivieren.  


## <a name="view-the-swagger"></a>Anzeigen von Swagger
Weitere Informationen finden Sie unter [Details zu Swagger](/connectors/twitterconnector/). 

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)


