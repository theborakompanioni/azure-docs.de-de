Mit dieser Bedingung wird das Feld mit der E-Mail-Adresse für jeden neuen Salesforce-Lead ausgewertet. Wenn die E-Mail-Adresse *amazon.com* enthält, ist das Ergebnis der Bedingung *True*.

1. Wählen Sie **+ Neuer Schritt**aus.  
   ![Salesforce-Bedingung – Abbildung 1](./media/connectors-create-api-salesforce/condition-1.png)   
2. Wählen Sie **Bedingung hinzufügen**aus.    
   ![Salesforce-Bedingung – Abbildung 2](./media/connectors-create-api-salesforce/condition-2.png)  
3. Wählen Sie **Wert auswählen**aus.    
   ![Salesforce-Bedingung – Abbildung 3](./media/connectors-create-api-salesforce/condition-3.png)  
4. Wählen Sie das Token *E-Mail* des Leads des Triggers aus.    
   ![Salesforce-Bedingung – Abbildung 4](./media/connectors-create-api-salesforce/condition-4.png)  
5. Wählen Sie *Enthält*aus.      
   ![Salesforce-Bedingung – Abbildung 5](./media/connectors-create-api-salesforce/condition-5.png)  
6. Wählen Sie **Wert auswählen** am unteren Rand des Steuerelements aus.     
   ![Salesforce-Bedingung – Abbildung 6](./media/connectors-create-api-salesforce/condition-6.png)  
7. Geben Sie *amazon.com* als den Wert ein, nach dem Sie die E-Mail-Adresse des neuen Leads auswerten möchten. Enthält die E-Mail-Adresse *amazon.com*, wird die Bedingung als *True* ausgewertet, und die anderen Schritte in Ihrer Logik-App können ausgeführt werden.    
   ![Salesforce-Bedingung – Abbildung 7](./media/connectors-create-api-salesforce/condition-7.png)  
8. Speichern Sie Ihre Logik-Apps.  

