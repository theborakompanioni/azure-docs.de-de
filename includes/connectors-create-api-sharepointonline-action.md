Nachdem Sie nun einen Trigger hinzugefügt haben, ist es an der Zeit, die vom Trigger generierten Daten sinnvoll zu nutzen. Führen Sie die folgenden Schritte zum Hinzufügen einer Aktion **SharePoint Online - Datei erstellen** aus. Mit dieser Aktion wird jedes Mal, wenn der Trigger für neue Elemente ausgelöst wird, eine Datei in SharePoint Online erstellt. 

Zum Konfigurieren dieser Aktion müssen Sie die folgenden Informationen angeben. Wenn Sie diese Informationen angeben, werden Sie feststellen, dass durch den Trigger generierte Daten problemlos als Eingabe für einige der Eigenschaften für die neue Datei verwendet werden können:

| Erstellen der Dateieigenschaft | Beschreibung |
| --- | --- |
| Website-URL |Dies ist die URL der SharePoint Online-Website, auf der Sie die neue Datei erstellen möchten. Wählen Sie die Website aus der angezeigten Liste aus. |
| Ordnerpfad |Dies ist der Ordner (der im vorherigen Schritt ausgewählten Website-URL), in dem die neue Datei platziert wird. Suchen Sie nach dem Ordner, und wählen Sie ihn aus. |
| Dateiname |Dies ist der Name der Datei, die erstellt wird. |
| Dateiinhalte |Der Inhalt, der in die Datei geschrieben wird. |

1. Wählen Sie **+ Neuer Schritt** aus, um die Aktion hinzuzufügen.  
   ![SharePoint Online-Aktion – Abbildung 1](./media/connectors-create-api-sharepointonline/action-1.png)  
2. Wählen Sie den Link **Aktion hinzufügen** aus. Daraufhin öffnet sich das Suchfeld, mit dem Sie nach der gewünschten Aktion suchen können. In diesem Beispiel sind SharePoint-Aktionen von Interesse.    
   ![SharePoint Online-Aktion – Abbildung 2](./media/connectors-create-api-sharepointonline/action-2.png)    
3. Geben Sie *SharePoint* ein, um nach Aktionen im Zusammenhang mit SharePoint zu suchen.
4. Wählen Sie **SharePoint Online - Datei erstellen** als die auszuführende Aktion aus.   **Hinweis**: Sie werden aufgefordert, den Zugriff Ihrer Logik-App auf Ihr SharePoint-Konto zu autorisieren, sofern Sie noch keine Verbindung mit SharePoint Online erstellt haben.    
   ![SharePoint Online-Aktion – Abbildung 3](./media/connectors-create-api-sharepointonline/action-3.png)    
5. Das Steuerelement **Datei erstellen** wird geöffnet.   
   ![SharePoint Online-Aktion – Abbildung 4](./media/connectors-create-api-sharepointonline/action-4.png)     
6. Wählen Sie **Website-URL** aus, und suchen Sie nach der Website, auf der Sie die Datei erstellen möchten.     
   ![SharePoint Online-Aktion – Abbildung 5](./media/connectors-create-api-sharepointonline/action-5.png)  
7. Wählen Sie **Ordnerpfad** aus, und suchen Sie nach dem Ordner, in dem die neue Datei platziert werden soll.  
   ![SharePoint Online-Aktion – Abbildung 6](./media/connectors-create-api-sharepointonline/action-6.png)  
8. Wählen Sie das Steuerelement **Dateiname** aus, und geben Sie den Namen der Datei ein, die Sie erstellen möchten. Hier können Sie den Dateinamen direkt eingeben oder Eigenschaften des Triggers verwenden, den Sie zuvor erstellt haben. Hierzu wählen Sie Eigenschaften aus der Liste der **Ausgaben von „Wenn ein neues Element erstellt wird“**aus. Nur diese Liste wird nach der Auswahl des Steuerelements **Dateiname** angezeigt. In dieser exemplarischen Vorgehensweise habe ich „ID“ (die ID des neuen Listenelements) als Namen für die Datei ausgewählt, die mit der Aktion **SharePoint Online - Datei erstellen** erstellt wird.    
   ![SharePoint Online-Aktion – Abbildung 7](./media/connectors-create-api-sharepointonline/action-7.png)  
9. Wählen Sie das Steuerelement **Dateiinhalt** aus, und geben Sie den Inhalt ein, der in die Datei geschrieben werden soll, die erstellt wird. Beachten Sie für den Dateiinhalt, dass Sie alle Eigenschaften des Triggers verwenden können, den Sie zuvor erstellt haben. Wählen Sie einfach die Eigenschaften aus der angezeigten Liste aus. Alternativ können Sie den Text für den **Dateiinhalt** direkt in das Steuerelement eingeben. In diesem Beispiel habe ich einige Eigenschaften ausgewählt und Leerzeichen und einen Bindestrich zwischen den Eigenschaften hinzugefügt.        
   ![SharePoint Online-Aktion – Abbildung 8](./media/connectors-create-api-sharepointonline/action-8.png)  
10. Speichern Sie die Änderungen am Workflow.  
11. Herzlichen Glückwunsch, Sie verfügen nun über eine voll funktionsfähige Logik-App, die ausgelöst wird, wenn einer SharePoint Online-Liste ein neues Element hinzugefügt wird. Die App erstellt dann eine Datei. Dabei werden einige der Eigenschaften des neuen Listenelements verwendet.  Durch Erstellen eines neuen Elements in der SharePoint-Liste können Sie sie jetzt testen. 

