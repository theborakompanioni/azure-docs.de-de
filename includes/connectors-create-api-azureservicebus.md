Nachdem Sie nun einen Trigger hinzugefügt haben, ist es an der Zeit, die vom Trigger generierten Daten sinnvoll zu nutzen. Führen Sie die folgenden Schritte zum Hinzufügen einer Aktion **SharePoint Online - Datei erstellen** aus. Mit dieser Aktion wird jedes Mal, wenn der Trigger für neue Elemente ausgelöst wird, eine Datei in SharePoint Online erstellt. 

Zum Konfigurieren dieser Aktion müssen Sie die folgenden Informationen angeben. Sie werden feststellen, dass durch den Trigger generierte Daten problemlos als Eingabe für einige der Eigenschaften für die neue Datei verwendet werden können:

| Erstellen der Dateieigenschaft | Beschreibung |
| --- | --- |
| Website-URL |Dies ist die URL der SharePoint Online-Website, auf der Sie die neue Datei erstellen möchten. Wählen Sie die Website aus der angezeigten Liste aus. |
| Ordnerpfad |Dies ist der Ordner (der Website-URL), in dem die neue Datei platziert wird. Suchen Sie nach dem Ordner, und wählen Sie ihn aus. |
| Dateiname |Dies ist der Name der Datei, die erstellt wird. |
| Dateiinhalte |Der Inhalt, der in die Datei geschrieben wird. |

1. Wählen Sie **+ Neuer Schritt** aus, um die Aktion hinzuzufügen.  
   ![](./media/connectors-create-api-sharepointonline/action-1.png)  
2. Wählen Sie den Link **Aktion hinzufügen** aus. Daraufhin öffnet sich das Suchfeld, mit dem Sie nach der gewünschten Aktion suchen können. In diesem Beispiel sind SharePoint-Aktionen von Interesse.    
   ![](./media/connectors-create-api-sharepointonline/action-2.png)    
3. Geben Sie *SharePoint* ein, um nach Aktionen im Zusammenhang mit SharePoint zu suchen.
4. Wählen Sie **SharePoint Online - Datei erstellen** als die auszuführende Aktion aus.   **Hinweis**: Sie werden aufgefordert, den Zugriff Ihrer Logik-App auf Ihr SharePoint-Konto zu autorisieren, sofern Sie diesen Schritt noch nicht ausgeführt haben.    
   ![](./media/connectors-create-api-sharepointonline/action-3.png)    
5. Das Steuerelement **Datei erstellen** wird geöffnet.   
   ![](./media/connectors-create-api-sharepointonline/action-4.png)     
6. Wählen Sie **Website-URL** aus, und suchen Sie nach der Website, auf der Sie die Datei erstellen möchten.     
   ![](./media/connectors-create-api-sharepointonline/action-5.png)  
7. Wählen Sie **Ordnerpfad** aus, und suchen Sie nach dem Ordner, in dem die neue Datei platziert werden soll.  
   ![](./media/connectors-create-api-sharepointonline/action-6.png)  
8. Wählen Sie das Steuerelement **Dateiname** aus, und geben Sie den Namen der Datei ein, die Sie erstellen möchten. Beachten Sie für den Dateinamen, dass Sie alle Eigenschaften des Triggers verwenden können, den Sie zuvor erstellt haben, indem Sie ihn einfach aus der angezeigten Liste auswählen.     
   ![](./media/connectors-create-api-sharepointonline/action-7.png)  
9. Wählen Sie das Steuerelement **Dateiinhalt** aus, und geben Sie den Inhalt ein, der in die Datei geschrieben werden soll, die erstellt wird. Beachten Sie für den Dateiinhalt, dass Sie alle Eigenschaften des Triggers verwenden können, den Sie zuvor erstellt haben. Wählen Sie einfach die Eigenschaften aus der angezeigten Liste aus. Alternativ können Sie den Text für den **Dateiinhalt** direkt in das Steuerelement eingeben. In diesem Beispiel habe ich einige Eigenschaften ausgewählt und Leerzeichen und einen Bindestrich zwischen den Eigenschaften hinzugefügt.        
   ![](./media/connectors-create-api-sharepointonline/action-8.png)  
10. Speichern Sie die Änderungen am Workflow.  



<!--HONumber=Jan17_HO3-->


