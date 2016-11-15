---
title: Erstellen einer Azure Functions-Funktion zum Erstellen einer Bindung an einen Azure-Dienst | Microsoft-Dokumentation
description: Erstellen Sie eine Azure Functions-Funktion, eine serverlose Anwendung, die mit anderen Azure-Diensten interagiert.
services: functions
documentationcenter: dev-center-name
author: yochay
manager: manager-alias
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: ab86065d-6050-46c9-a336-1bfc1fa4b5a1
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/25/2016
ms.author: rachelap@microsoft.com
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9ffd199c9e3c621a808ade109ed044b6c9b689b7


---
# <a name="create-an-azure-function-which-binds-to-an-azure-service"></a>Erstellen einer Azure Functions-Funktion zum Erstellen einer Bindung an einen Azure-Dienst
[!INCLUDE [Getting Started Note](../../includes/functions-getting-started.md)]

In diesem kurzen Video erfahren Sie, wie Sie eine Azure Functions-Funktion erstellen, die eine Azure-Warteschlange auf Nachrichten überwacht und die Nachrichten in ein Azure-Blob kopiert.

## <a name="watch-the-video"></a>Video ansehen
>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player]
>
>

## <a name="create-an-input-queue-trigger-function"></a>Erstellen einer Trigger-Funktion für die Eingabewarteschlange
Mit dieser Funktion soll alle zehn Sekunden eine Nachricht in eine Warteschlange geschrieben werden. Hierzu müssen Sie die Funktion und die Nachrichtenwarteschlangen erstellen und den neu erstellten Warteschlangen den Code zum Schreiben von Nachrichten hinzufügen.

1. Navigieren Sie im Azure-Portal zu Ihrer Azure-Funktionen-App.
2. Klicken Sie auf **Neue Funktion** > **TimerTrigger – Node**. Nennen Sie die Funktion **FunctionsBindingsDemo1**.
3. Geben Sie für den Zeitplan den Wert „0/10 * * * * *“ ein. Dieser Wert wird als CRON-Ausdruck angegeben. Dadurch wird der Timer alle 10 Sekunden ausgeführt.
4. Klicken Sie auf die Schaltfläche **Erstellen**, um die Funktion zu erstellen.
   
    ![Hinzufügen einer Funktion zum Auslösen des Timers](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)
5. Vergewissern Sie sich anhand der Aktivität im Protokoll, dass die Funktion funktioniert. Zum Anzeigen des Protokollbereichs müssen Sie ggf. in der rechten oberen Ecke auf **Protokolle** klicken.
   
   ![Überprüfen der Funktionsfähigkeit der Funktion anhand des Protokolls](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

### <a name="add-a-message-queue"></a>Hinzufügen einer Nachrichtenwarteschlange
1. Navigieren Sie zur Registerkarte **Integrieren**.
2. Wählen Sie **Neue Ausgabe** > **Azure Storage-Warteschlange** > **Auswählen** aus.
3. Geben Sie im Textfeld **Nachrichtenparametername** die Zeichenfolge **myQueueItem** ein.
4. Wählen Sie ein Speicherkonto aus, oder klicken Sie auf **Neu**, um ein Speicherkonto zu erstellen, falls noch keins vorhanden ist.
5. Geben Sie im Textfeld **Warteschlangenname** die Zeichenfolge **functions-bindings** ein.
6. Klicken Sie auf **Speichern**.  
   
   ![Hinzufügen einer Funktion zum Auslösen des Timers](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

### <a name="write-to-the-message-queue"></a>Schreiben in die Nachrichtenwarteschlange
1. Kehren Sie zur Registerkarte **Entwickeln** zurück, und fügen Sie der Funktion nach dem bereits vorhandenen Code den folgenden Code hinzu:
   
    ```javascript
   
    function myQueueItem() 
      {
        return {
        msg: "some message goes here",
        time: "time goes here"
      }
    }
   
    ```
2. Ändern Sie den vorhandenen Funktionscode so, dass der in Schritt 1 hinzugefügte Code aufgerufen wird. Fügen Sie bei Zeile 9 der Funktion (nach der Anweisung *if*) den folgenden Code ein.
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueue = toBeQed;
   
    ```
   
    Dieser Code erstellt ein **myQueueItem**-Element und legt dessen **time**-Eigenschaft auf den aktuellen Zeitstempel fest. Anschließend wird das neue Warteschlangenelement der myQueue-Bindung des Kontexts hinzugefügt.
3. Klicken Sie auf **Speichern und ausführen**.
4. Sehen Sie sich die Warteschlange in Visual Studio an, um zu prüfen, ob der Code funktioniert.
   
   * Navigieren Sie in Visual Studio zu **Ansicht** > **Cloud-****Explorer**.
   * Suchen Sie das Speicherkonto und die Warteschlange **functions-bindings**, die Sie beim Erstellen der myQueue-Warteschlange verwendet haben. Es sollten mehrere Zeilen mit Protokolldaten angezeigt werden. Unter Umständen müssen Sie sich über Visual Studio bei Azure anmelden.  

## <a name="create-an-output-queue-trigger-function"></a>Erstellen einer Trigger-Funktion für die Ausgabewarteschlange
1. Klicken Sie auf **Neue Funktion** > **QueueTrigger – C#**. Nennen Sie die Funktion **FunctionsBindingsDemo2**. Beachten Sie, dass Sie innerhalb der gleichen Funktionen-App mehrere Sprachen (in diesem Fall: Node und C#) kombinieren können.
2. Geben Sie im Feld **Warteschlangenname** die Zeichenfolge **functions-bindings** ein.
3. Wählen Sie das gewünschte Speicherkonto aus, oder erstellen Sie ein neues.
4. Klicken Sie auf **Erstellen**.
5. Überprüfen Sie sowohl das Protokoll der Funktion als auch Visual Studio auf Updates, und vergewissern Sie sich, dass die neue Funktion funktioniert. Im Protokoll der Funktion sehen Sie, dass die Funktion ausgeführt wird und dass Elemente aus der Warteschlange entfernt werden. Da die Funktion als Eingabe-Trigger an die Ausgabewarteschlange **functions-bindings** gebunden ist, sollten die Elemente nach der Aktualisierung der Warteschlange **functions-bindings** in Visual Studio nicht mehr angezeigt werden. Sie wurden aus der Warteschlange entfernt.   
   
   ![Hinzufügen einer Timer-Funktion für die Ausgabewarteschlange](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png)   

### <a name="modify-the-queue-item-type-from-json-to-object"></a>Ändern des Warteschlangenelementtyps von „JSON“ in „Objekt“
1. Ersetzen Sie den Code in **FunctionsBindingsDemo2** durch folgenden Code:    
   
    ```cs
   
    using System;
   
    public static void Run(QItem myQueueItem, ICollector<TableItem> myTable, TraceWriter log)
    {
      TableItem myItem = new TableItem
      {
        PartitionKey = "key",
        RowKey = Guid.NewGuid().ToString(),
        Time = DateTime.Now.ToString("hh.mm.ss.ffffff"),
        Msg = myQueueItem.Msg,
        OriginalTime = myQueueItem.Time    
      };
      log.Verbose($"C# Queue trigger function processed: {myQueueItem.Msg} | {myQueueItem.Time}");
    }
   
    public class TableItem
    {
      public string PartitionKey {get; set;}
      public string RowKey {get; set;}
      public string Time {get; set;}
      public string Msg {get; set;}
      public string OriginalTime {get; set;}
    }
   
    public class QItem
    {
      public string Msg { get; set;}
      public string Time { get; set;}
    }
   
    ```
   
    Dieser Code fügt zwei Klassen (**TableItem** und **QItem**) hinzu, die zum Lesen und Schreiben in Warteschlangen verwendet werden. Darüber hinaus wurde die **Run**-Funktion geändert: Sie akzeptiert nun anstelle von **string** und **TraceWriter** die Parameter **QItem** und **TraceWriter**. 
2. Klicken Sie auf die Schaltfläche **Save** .
3. Vergewissern Sie sich anhand des Protokolls, dass der Code funktioniert. Das Objekt wird von Azure Functions automatisch serialisiert und deserialisiert. Dies vereinfacht den objektorientierten Zugriff auf die Warteschlange, um Daten zu bewegen. 

## <a name="store-messages-in-an-azure-table"></a>Speichern von Nachrichten in einer Azure-Tabelle
Nachdem die Warteschlangen nun zusammenarbeiten, muss noch eine Azure-Tabelle zur dauerhaften Speicherung der Warteschlangendaten hinzugefügt werden.

1. Navigieren Sie zur Registerkarte **Integrieren**.
2. Erstellen Sie eine Azure Storage-Tabelle für die Ausgabe, und nennen Sie sie **myTable**.
3. Beantworten Sie die Frage nach der Tabelle, in die die Daten geschrieben werden sollen, mit **functionsbindings**.
4. Ändern Sie die Einstellung **PartitionKey** von **{project-id}** in **{partition}**.
5. Wählen Sie ein Speicherkonto aus, oder erstellen Sie ein neues.
6. Klicken Sie auf **Speichern**.
7. Navigieren Sie zur Registerkarte **Entwickeln**.
8. Erstellen Sie eine **TableItem**-Klasse zur Darstellung einer Azure-Tabelle, und ändern Sie die Run-Funktion so, dass sie das neu erstellte TableItem-Objekt akzeptiert. Hierzu müssen die Eigenschaften **PartitionKey** und **RowKey** verwendet werden.
   
    ```cs
   
    public static void Run(QItem myQueueItem, ICollector<TableItem> myTable, TraceWriter log)
    {    
      TableItem myItem = new TableItem
      {
        PartitionKey = "key",
        RowKey = Guid.NewGuid().ToString(),
        Time = DateTime.Now.ToString("hh.mm.ss.ffffff"),
        Msg = myQueueItem.Msg,
        OriginalTime = myQueueItem.Time    
      };
   
      log.Verbose($"C# Queue trigger function processed: {myQueueItem.RowKey} | {myQueueItem.Msg} | {myQueueItem.Time}");
    }
   
    public class TableItem
    {
      public string PartitionKey {get; set;}
      public string RowKey {get; set;}
      public string Time {get; set;}
      public string Msg {get; set;}
      public string OriginalTime {get; set;}
    }
    ```
9. Klicken Sie auf **Speichern**.
10. Vergewissern Sie sich anhand der Protokolle der Funktion sowie in Visual Studio, dass der Code funktioniert. Navigieren Sie für die Überprüfung in Visual Studio mithilfe von **Cloud-Explorer** zur Azure-Tabelle **functionbindings**, und vergewissern Sie sich, dass sie Zeilen enthält.

[!INCLUDE [Getting Started Note](../../includes/functions-bindings-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO2-->


