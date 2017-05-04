Nachdem Sie nun eine Bedingung hinzugefügt haben, ist es an der Zeit, die vom Trigger generierten Daten sinnvoll zu nutzen. Führen Sie die folgenden Schritte zum Hinzufügen der Aktion **Salesforce - Objekt abrufen** aus. Diese Aktion ruft die Daten jedes Mal ab, wenn ein neuer Lead erstellt wird. Sie können außerdem eine zweite Aktion hinzufügen, die die Daten der Aktion „Salesforce - Objekt abrufen“ verwendet, um eine E-Mail mithilfe des Office 365-Connectors zu senden.  

Zum Konfigurieren dieser Aktion müssen Sie die folgenden Informationen angeben. Sie werden feststellen, dass durch den Trigger generierte Daten problemlos als Eingabe für einige der Eigenschaften für die neue Datei verwendet werden können:

| Erstellen der Dateieigenschaft | Beschreibung |
| --- | --- |
| Objekttyp |Dies ist der Typ des Salesforce-Objekts, an dem Sie interessiert sind. Beispiele sind Lead, Konto usw. |
| Object ID (Objekt-ID) |Dies stellt einen Bezeichner für das Objekt dar. |

1. Wählen Sie den Link **Aktion hinzufügen** aus. Daraufhin öffnet sich das Suchfeld, mit dem Sie nach der gewünschten Aktion suchen können. In diesem Beispiel sind Salesforce-Aktionen von Interesse.      
   ![Salesforce-Aktion – Abbildung 1](./media/connectors-create-api-salesforce/action-1.png)  
2. Geben Sie *Salesforce* ein, um nach Aktionen im Zusammenhang mit Salesforce zu suchen.
3. Wählen Sie **Salesforce - Objekt abrufen** als die auszuführende Aktion aus.   **Hinweis**: Sie werden aufgefordert, den Zugriff Ihrer Logik-App auf Ihr Salesforce-Konto zu autorisieren, sofern Sie diesen Schritt noch nicht ausgeführt haben.    
   ![Salesforce-Aktion – Abbildung 2](./media/connectors-create-api-salesforce/action-2.png)    
4. Das Steuerelement **Objekt abrufen** wird geöffnet.  
5. Wählen Sie *Lead* als Objekttyp aus.
6. Wählen Sie das Steuerelement **Objekt-ID** aus.
7. Wählen Sie **...** aus, um die Liste der Token zu erweitern, die als Eingabe für Aktionen verwendet werden können.       
   ![Salesforce-Aktion – Abbildung 3](./media/connectors-create-api-salesforce/action-3.png)    
8. Wählen Sie das Steuerelement **Lead-ID** aus.   
   ![Salesforce-Aktion – Abbildung 4](./media/connectors-create-api-salesforce/action-4.png)     
9. Beachten Sie, dass das Lead-ID-Token jetzt im Objekt-ID-Steuerelement enthalten ist. Damit wird angegeben, dass die Aktion „Objekt abrufen“ nach einem Lead mit einer ID sucht, die der Lead-ID des Leads entspricht, der diese Logik-App ausgelöst hat.  
   ![Salesforce-Aktion – Abbildung 5](./media/connectors-create-api-salesforce/action-5.png)  
10. Speichern Sie Ihre Arbeit. Das war alles, Sie haben die Aktion „Objekt abrufen“ Ihrer Logik-App hinzugefügt. Das Steuerelement „Objekt abrufen“ sollte wie folgt aussehen:     
    ![Salesforce-Aktion – Abbildung 6](./media/connectors-create-api-salesforce/action-6.png)  

Nachdem Sie eine Aktion zum Abrufen eines Leads hinzugefügt haben, sollten Sie den neu erstellten Lead sinnvoll nutzen. In einem Unternehmen könnten Sie eine E-Mail senden, um einer Verteilerliste mitzuteilen, dass ein neuer Lead erstellt wurde. Verwenden wir jetzt den Office 365-Connector, um eine E-Mail mit einigen relevanten Informationen aus dem neuen Leadobjekt in Salesforce zu senden.  

1. Wählen Sie **Aktion hinzufügen** aus, und geben Sie *E-Mail* in das Steuerelement für die Suche ein. Dadurch werden die Aktionen herausgefiltert, die sich auf das Senden und Empfangen von E-Mails beziehen.  
2. Wählen Sie das Listenelement **Office 365 Outlook - E-Mail senden** aus. Wenn Sie noch keine *Verbindung* mit Ihrem Office 365-Konto erstellt haben, werden Sie aufgefordert, Ihre Office 365-Anmeldeinformationen einzugeben, um sie jetzt zu erstellen. Wenn Sie fertig sind, wird das Steuerelement **E-Mail senden** geöffnet.        
   ![Salesforce-Aktion – Abbildung 7](./media/connectors-create-api-salesforce/action-7.png)  
3. Geben Sie die E-Mail-Adresse, an die Sie die E-Mail senden möchten, in das Steuerelement **An** ein.
4. Geben Sie in das Steuerelement **Betreff** den Text *Neuer Lead erstellt* ein, und wählen Sie dann das Token *Unternehmen* aus. Damit wird das Feld *Unternehmen* des neuen Leads angezeigt, der in Salesforce erstellt wurde.  
5. Im Steuerelement **Text** können Sie beliebige Token des neuen Leadobjekts auswählen, und Sie können auch den Text eingeben, der als Text der E-Mail angezeigt werden soll. Hier ist ein Beispiel angegeben:   
   ![Salesforce-Aktion – Abbildung 8](./media/connectors-create-api-salesforce/action-8.png)   
6. Speichern Sie den Workflow.  

Das ist alles. Ihre Logik-App ist jetzt fertig.  

Nun können Sie Ihre Logik-App testen: Erstellen Sie in Salesforce einen neuen Lead, der die erstellte Bedingung erfüllt.  Wenn Sie diese exemplarische Vorgehensweise vollständig befolgt haben, erstellen Sie einfach einen Lead mit einer E-Mail-Adresse, die *amazon.com* enthält. Nach wenigen Sekunden sollte Ihre Logik-App ausgelöst werden, und das Ergebnis könnte etwa wie folgt aussehen:   
![Salesforce-Aktion – Abbildung 9](./media/connectors-create-api-salesforce/action-9.png)  

