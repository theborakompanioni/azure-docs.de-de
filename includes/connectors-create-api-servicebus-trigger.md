In diesem Beispiel zeige ich Ihnen, wie der Trigger **Service Bus - Wenn bei einer Warteschlange eine Nachricht eingeht** verwendet wird, um einen Logik-App-Workflow zu initiieren, wenn ein neues Element bei einer Service Bus-Warteschlange eingeht.

>[AZURE.NOTE]Sie werden aufgefordert, sich mit der Service Bus-Verbindungszeichenfolge anzumelden, wenn Sie noch keine *Verbindung* mit Service Bus erstellt haben.

1. Geben Sie *Service Bus* in das Suchfeld des Designers für Logik-Apps ein, und wählen Sie dann den Trigger **Service Bus - Wenn bei einer Warteschlange eine Nachricht eingeht** aus.  
![Service Bus-Trigger – Abbildung 1](./media/connectors-create-api-servicebus/trigger-1.png)   
- Das Steuerelement **Wenn bei einer Warteschlange eine Nachricht eingeht** wird angezeigt.  
![Service Bus-Trigger – Abbildung 2](./media/connectors-create-api-servicebus/trigger-2.png)   
- Geben Sie den Namen der Service Bus-Warteschlange ein, die der Trigger überwachen soll.  
![Service Bus-Trigger – Abbildung 3](./media/connectors-create-api-servicebus/trigger-3.png)   

Jetzt haben Sie Ihre Logik-App mit einem Trigger konfiguriert, der eine Ausführung der anderen Trigger und Aktionen im Workflow startet, wenn ein neues Element bei der ausgewählten Warteschlange eingeht.

<!---HONumber=AcomDC_0727_2016-->
