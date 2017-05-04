In dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie den Trigger **Salesforce - Wenn ein Objekt erstellt wird** verwenden, um den Workflow einer Logik-App zu starten, wenn ein neuer Lead in Salesforce erstellt wird.

> [!NOTE]
> Sie werden aufgefordert, sich bei Ihrem Salesforce-Konto anzumelden, wenn Sie noch keine *Verbindung* mit Salesforce erstellt haben.  
> 
> 

1. Geben Sie im Suchfeld des Designers für Logik-Apps die Zeichenfolge *Salesforce* ein, und wählen Sie anschließend den Trigger **Salesforce – Wenn ein Objekt erstellt wird** aus.  
   ![Salesforce-Trigger – Abbildung 1](./media/connectors-create-api-salesforce/trigger-1.png)   
2. Das Steuerelement **Wenn ein Objekt erstellt wird** wird angezeigt.  
   ![Salesforce-Trigger – Abbildung 2](./media/connectors-create-api-salesforce/trigger-2.png)   
3. Wählen Sie den **Objekttyp** und anschließend in der Objektliste die Option *Lead* aus. In diesem Schritt geben Sie an, dass Sie einen Trigger erstellen, der Ihre Logik-App benachrichtigt, wenn in Salesforce ein neuer Lead erstellt wird.   
   ![Salesforce-Trigger – Abbildung 3](./media/connectors-create-api-salesforce/trigger-3.png)   
4. Das ist alles. Sie haben den Trigger erstellt. Allerdings müssen Sie mindestens eine Aktion erstellen, um daraus eine gültige Logik-App zu machen.    
   ![Salesforce-Trigger – Abbildung 4](./media/connectors-create-api-salesforce/trigger-4.png)   

Jetzt haben Sie Ihre Logik-App mit einem Trigger konfiguriert, der eine Ausführung der anderen Trigger und Aktionen im Workflow startet, wenn ein neues Element in Salesforce erstellt wird.  

