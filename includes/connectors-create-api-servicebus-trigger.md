Hier erfahren Sie, wie Sie mithilfe des Triggers **Service Bus - When a message is received in a queue** (Service Bus – Wenn bei einer Warteschlange eine Nachricht eingeht) einen Logik-App-Workflow initiieren, wenn ein neues Element an eine Service Bus-Warteschlange gesendet wird.  

> [!NOTE]
> Sie werden aufgefordert, sich mit der Service Bus-Verbindungszeichenfolge anzumelden, wenn Sie noch keine Verbindung mit Service Bus erstellt haben.  
> 
> 

1. Geben Sie in das Suchfeld des Designers für Logik-Apps die Zeichenfolge **service bus**ein. Wählen Sie dann den Trigger **Service Bus - When a message is received in a queue** (Service Bus – Wenn bei einer Warteschlange eine Nachricht eingeht) aus.  
   ![Service Bus-Trigger – Abbildung 1](./media/connectors-create-api-servicebus/trigger-1.png)   
2. Das Dialogfeld **When a message is received in a queue** (Wenn bei einer Warteschlange eine Nachricht eingeht) wird angezeigt.  
   ![Service Bus-Trigger – Abbildung 2](./media/connectors-create-api-servicebus/trigger-2.png)   
3. Geben Sie den Namen der Service Bus-Warteschlange ein, die der Trigger überwachen soll.   
   ![Service Bus-Trigger – Abbildung 3](./media/connectors-create-api-servicebus/trigger-3.png)   

Ihre Logik-App ist nun mit einem Trigger konfiguriert. Wenn bei der ausgewählten Warteschlange ein neues Element eingeht, startet der Trigger die anderen Trigger und Aktionen des Workflows.    

