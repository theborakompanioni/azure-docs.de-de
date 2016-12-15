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
ms.date: 12/06/2016
ms.author: rachelap@microsoft.com
translationtype: Human Translation
ms.sourcegitcommit: f46a67f2591ef98eeda03f5c3bc556d5b8bcc096
ms.openlocfilehash: 4e0dd8b922107b232a120c25d1f656c5d667748b


---
# <a name="create-an-azure-function-connected-to-an-azure-service"></a>Erstellen einer Azure Functions-Funktion, die mit einem Azure-Dienst verbunden ist

In diesem Thema wird veranschaulicht, wie Sie eine Azure Functions-Funktion erstellen, die über eine Azure Storage-Warteschlange auf Nachrichten lauscht und die Nachrichten in die Zeilen einer Azure Storage-Tabelle kopiert. Eine per Timer ausgelöste Funktion wird verwendet, um Nachrichten in die Warteschlange zu laden. Mit einer zweiten Funktion werden Daten aus der Warteschlange ausgelesen und Nachrichten in die Tabelle geschrieben. Sowohl die Warteschlange als auch die Tabelle werden für Sie basierend auf den Bindungsdefinitionen von Azure Functions erstellt. 

Interessant ist hierbei, dass eine Funktion in JavaScript und die andere in C#-Skript geschrieben wurde. Dies zeigt, dass eine Funktionen-App über Funktionen in unterschiedlichen Sprachen verfügen kann.

## <a name="watch-the-video"></a>Video ansehen
>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player]
>
>

## <a name="create-a-function-that-writes-to-the-queue"></a>Erstellen einer Funktion, die in die Warteschlange schreibt

Bevor Sie eine Verbindung mit einer Speicherwarteschlange herstellen können, müssen Sie eine Funktion erstellen, die eine Nachrichtenwarteschlange lädt. Für die JavaScript-Funktion wird ein Trigger mit Timer verwendet, bei dem alle zehn Sekunden eine Nachricht in die Warteschlange geschrieben wird. Falls Sie noch kein Azure-Konto besitzen, können Sie sich die Seite zum Ausprobieren von Funktionen ([Try Azure Functions](https://functions.azure.com/try)) ansehen oder [ein kostenloses Azure-Konto erstellen](https://azure.microsoft.com/free/).

1. Navigieren Sie im Azure-Portal zu Ihrer Funktionen-App.

2. Klicken Sie auf **Neue Funktion** > **TimerTrigger-JavaScript**. 

3. Geben Sie der Funktion den Namen **FunctionsBindingsDemo1**, und geben Sie den CRON-Ausdruckswert `0/10 * * * * *` unter **Zeitplan** ein. Klicken Sie anschließend auf **Erstellen**.
   
    ![Hinzufügen einer Funktion mit Auslösung per Timer](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    Sie haben jetzt eine Funktion erstellt, die per Timer ausgelöst und alle zehn Sekunden ausgeführt wird.

5. Klicken Sie auf der Registerkarte **Entwickeln** auf **Protokolle**, und zeigen Sie die Aktivität im Protokoll an. Sie sehen, dass alle zehn Sekunden ein Protokolleintrag geschrieben wird.
   
    ![Anzeigen des Protokolls zur Überprüfung der Funktion](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a>Hinzufügen einer Ausgabebindung für die Nachrichtenwarteschlange

1. Wählen Sie auf der Registerkarte **Integrieren** die Option **Neue Ausgabe** > **Azure Queue Storage** > **Auswählen**.

    ![Hinzufügen einer Funktion mit Auslösung per Timer](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. Geben Sie `myQueueItem` unter **Nachrichtenparametername** und `functions-bindings` unter **Warteschlangenname** ein, und wählen Sie eine vorhandene **Speicherkontoverbindung** aus, oder klicken Sie auf **Neu**, um eine Speicherkontoverbindung zu erstellen. Klicken Sie anschließend auf **Speichern**.  

    ![Erstellen der Ausgabebindung mit der Speicherwarteschlange](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. Fügen Sie über die Registerkarte **Entwickeln** den folgenden Code an die Funktion an:
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. Suchen Sie nach der *if*-Anweisung, die sich ungefähr in Zeile 9 der Funktion befindet, und fügen Sie nach der Anweisung den folgenden Code ein.
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    Dieser Code erstellt ein **myQueueItem**-Element und legt dessen **time**-Eigenschaft auf den aktuellen Zeitstempel fest. Anschließend wird das neue Warteschlangenelement der **myQueueItem**-Bindung des Kontexts hinzugefügt.

3. Klicken Sie auf **Speichern und ausführen**.

## <a name="view-storage-updates-by-using-storage-explorer"></a>Anzeigen von Speicherupdates mit dem Speicher-Explorer
Sie können die Funktionsweise der Funktion überprüfen, indem Sie in der von Ihnen erstellten Warteschlange Nachrichten anzeigen.  Sie können eine Verbindung mit der Speicherwarteschlange herstellen, indem Sie den Cloud-Explorer in Visual Studio verwenden. Im Portal ist es aber einfach, mit dem Microsoft Azure-Speicher-Explorer eine Verbindung mit Ihrem Speicherkonto herzustellen.

1. Klicken Sie auf der Registerkarte **Integrieren** auf Ihre Warteschlangen-Ausgabebindung und dann auf **Dokumentation**. Blenden Sie anschließend die Verbindungszeichenfolge für Ihr Speicherkonto ein, und kopieren Sie den Wert. Sie verwenden diesen Wert, um eine Verbindung mit Ihrem Speicherkonto herzustellen.

    ![Herunterladen des Azure-Speicher-Explorers](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. Laden Sie den [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com) herunter, und installieren Sie ihn, falls Sie dies noch nicht getan haben. 
 
3. Klicken Sie im Speicher-Explorer auf das Symbol „Verbindung mit Azure-Speicher herstellen“, fügen Sie die Verbindungszeichenfolge in das Feld ein, und schließen Sie den Assistenten ab.

    ![Speicher-Explorer – Hinzufügen einer Verbindung](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. Erweitern Sie unter **Local and attached** (Lokal und angefügt) die Option **Speicherkonten** > Ihr Speicherkonto > **Warteschlangen** > **functions-bindings**, und vergewissern Sie sich, dass Nachrichten in die Warteschlange geschrieben werden.

    ![Anzeigen von Nachrichten in der Warteschlange](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    Wenn die Warteschlange nicht vorhanden oder leer ist, liegt wahrscheinlich ein Problem mit der Funktionsbindung oder dem Code vor.

## <a name="create-a-function-that-reads-from-the-queue"></a>Erstellen einer Funktion zum Auslesen aus der Warteschlange

Nachdem der Warteschlange jetzt Nachrichten hinzugefügt werden, können Sie eine weitere Funktion erstellen, mit der Daten aus der Warteschlange ausgelesen und die Nachrichten dauerhaft in eine Azure Storage-Tabelle geschrieben werden.

1. Klicken Sie auf **Neue Funktion** > **QueueTrigger-CSharp**. 
 
2. Geben Sie der Funktion den Namen `FunctionsBindingsDemo2`, geben Sie **functions-bindings** im Feld **Warteschlangenname** ein, und wählen Sie ein vorhandenes Speicherkonto aus, oder erstellen Sie ein Speicherkonto. Klicken Sie anschließend auf **Erstellen**.

    ![Hinzufügen einer Timer-Funktion für die Ausgabewarteschlange](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. (Optional) Sie können die Funktionsweise der neuen Funktion überprüfen, indem Sie die neue Warteschlange wie zuvor im Speicher-Explorer anzeigen. Sie können auch den Cloud-Explorer in Visual Studio verwenden.  

4. (Optional) Aktualisieren Sie die Warteschlange **functions-bindings**. Sie sehen, dass Elemente aus der Warteschlange entfernt wurden. Das Entfernen wird durchgeführt, weil die Funktion an die Warteschlange **functions-bindings** als Eingabetrigger gebunden ist und die Funktion Daten aus der Warteschlange ausliest. 
 
## <a name="add-a-table-output-binding"></a>Hinzufügen einer Tabellenausgabebindung

1. Klicken Sie in FunctionsBindingsDemo2 auf **Integrieren** > **Neue Ausgabe** > **Azure Table Storage** > **Auswählen**.

    ![Hinzufügen einer Bindung an eine Azure Storage-Tabelle](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. Geben Sie `TableItem` als **Tabellenname** und `functionbindings` als **Tabellenparametername** ein, und wählen Sie eine **Speicherkontoverbindung** aus, oder erstellen Sie eine neue Verbindung. Klicken Sie anschließend auf **Speichern**.

    ![Konfigurieren der Speichertabellenbindung](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. Ersetzen Sie auf der Registerkarte **Entwickeln** den vorhandenen Funktionscode durch Folgendes:
   
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
        
        // Add the item to the table binding collection.
        myTable.Add(myItem);
    
        log.Verbose($"C# Queue trigger function processed: {myItem.RowKey} | {myItem.Msg} | {myItem.Time}");
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
    Die **TableItem**-Klasse stellt eine Zeile in der Speichertabelle dar, und Sie können das Element der `myTable`-Sammlung mit **TableItem**-Objekten hinzufügen. Sie müssen die Eigenschaften **PartitionKey** und **RowKey** festlegen, um Elemente in die Tabelle einfügen zu können.

4. Klicken Sie auf **Speichern**.  Abschließend können Sie die Funktionsweise der Funktion überprüfen, indem Sie die Tabelle im Speicher-Explorer oder im Cloud-Explorer von Visual Studio anzeigen.

5. (Optional) Erweitern Sie unter Ihrem Speicherkonto im Speicher-Explorer die Option **Tabellen** > **functionsbindings**, und vergewissern Sie sich, dass der Tabelle Zeilen hinzugefügt werden. Dies ist auch im Cloud-Explorer von Visual Studio möglich.

    ![Anzeigen von Zeilen in der Tabelle](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    Wenn die Tabelle nicht vorhanden oder leer ist, liegt wahrscheinlich ein Problem mit der Funktionsbindung oder dem Code vor. 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Functions finden Sie in diesen Themen.

* [Entwicklerreferenz zu Azure Functions](functions-reference.md)  
   Referenz zum Programmieren von Funktionen sowie zum Festlegen von Triggern und Bindungen.
* [Testing Azure Functions (Testen von Azure Functions) (Testen von Azure Functions)](functions-test-a-function.md)  
   Beschreibt verschiedene Tools und Techniken zum Testen Ihrer Funktionen
* [How to scale Azure Functions (Skalieren von Azure Functions) (Skalieren von Azure Functions)](functions-scale.md)  
  Beschreibt die für Azure Functions verfügbaren Servicepläne (einschließlich des Hostingplans „Verbrauchstarif“) und enthält Informationen zur Wahl des geeigneten Plans. 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]




<!--HONumber=Dec16_HO2-->


