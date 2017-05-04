In diesem Beispiel zeige ich Ihnen, wie mit dem Trigger **SharePoint Online - Wenn ein neues Element erstellt wird** der Workflow einer Logik-App ausgelöst wird, wenn ein neues Element in einer SharePoint Online-Liste erstellt wird.

> [!NOTE]
> Sie werden aufgefordert, sich bei Ihrem SharePoint-Konto anzumelden, wenn Sie noch keine *Verbindung* mit SharePoint Online erstellt haben.  
> 
> 

1. Geben Sie im Suchfeld des Designers für Logik-Apps die Zeichenfolge *SharePoint* ein, und wählen Sie anschließend den Trigger **SharePoint – Wenn ein neues Element erstellt wird** aus.  
   ![SharePoint Online-Trigger – Abbildung](./media/connectors-create-api-sharepointonline/trigger-1.png)  
2. Das Steuerelement **Wenn ein neues Element erstellt wird** wird angezeigt.  
   ![SharePoint Online-Trigger – Abbildung 2](./media/connectors-create-api-sharepointonline/trigger-2.png)   
3. Wählen Sie einen **Website-URL**aus. Dies ist die SharePoint Online-Website, die Sie auf neue Elemente überwachen möchten, um den Workflow auszulösen.  
   ![SharePoint Online-Trigger – Abbildung 3](./media/connectors-create-api-sharepointonline/trigger-3.png)   
4. Wählen Sie einen **Listenname**aus. Dies ist die Liste auf der SharePoint Online-Website, die Sie auf neue Elemente überwachen möchten, die den Workflow auslösen.  
   ![SharePoint Online-Trigger – Abbildung 4](./media/connectors-create-api-sharepointonline/trigger-4.png)   

Jetzt haben Sie Ihre Logik-App mit einem Trigger konfiguriert, der eine Ausführung der anderen Trigger und Aktionen im Workflow startet. Dies findet jedes Mal statt, wenn ein neues Element in der ausgewählten SharePoint Online-Liste erstellt wird.  

