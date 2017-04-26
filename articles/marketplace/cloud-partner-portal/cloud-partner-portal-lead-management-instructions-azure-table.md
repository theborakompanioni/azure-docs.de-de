---
title: "Anweisungen für die Leadverwaltung für Azure-Tabellen in Azure Marketplace | Microsoft-Dokumentation"
description: "Dieser Artikel erläutert schrittweise, wie Herausgeber die Leadverwaltung mit Azure Table Storage einrichten können."
services: cloud-partner-portal
documentationcenter: 
author: Bigbrd
manager: hamidm
ms.service: cloud-partner-portal
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: brdi
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 45aa2e0b178969215df9ae305d0a13a350ae0fda
ms.lasthandoff: 04/18/2017


---

# <a name="lead-management-instructions-for-azure-table"></a>Anweisungen für die Leadverwaltung für Azure-Tabellen

Dieses Dokument enthält Anweisungen für die Einrichtung Ihrer Azure-Tabelle, damit Microsoft Ihnen Vertriebsleads bereitstellen kann. Eine Azure-Tabelle ist die beste Wahl für, wenn Sie Ihre Leads auf benutzerdefinierte Weise speichern möchten.

1. Wenn Sie noch kein Azure-Konto haben, [erstellen Sie zuerst eines](https://azure.microsoft.com/pricing/free-trial/).

2. Sobald Ihr Azure-Konto eingerichtet ist, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, um ein Speicherkonto zu erstellen. Der nachstehende Screenshot bietet Anweisungen, und [hier erfahren Sie mehr zu Speicherpreisen](https://azure.microsoft.com/pricing/details/storage/). <br/>
  ![Abbildung der Erstellung eines Azure-Speicherkontos](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

3. Kopieren Sie als Nächstes die Verbindungszeichenfolge für den Schlüssel, und fügen Sie sie im Cloudpartnerportal in das Feld <b>Verbindungszeichenfolge für Speicherkonto</b> ein. <br/> 
  ![Abbildung des Azure-Speicherschlüssels](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

  Beispiel einer endgültigen Verbindungszeichenfolge: <br/>
`{"connectionString":"DefaultEndpointsProtocol=https;AccountName=leadaccount;AccountKey=ObS0EW7tDmXrC8oNeG6IRHpx2IUioBQTQynQcR/MUMqrNqQ/RC6zctP8HfucNJO+ond7dJHTROO9ziiPNspjEg=="}`

Im [Azure-Speicher-Explorer](http://azurestorageexplorer.codeplex.com/) (Drittanbieter-App) oder einem anderen Tool können Sie die Daten in Ihrer Speichertabelle anzeigen oder die Daten exportieren.

### <a name="optional-azure-functions--azure-table"></a>**(Optional)**  Azure-Funktionen + Azure-Tabelle
Wenn Sie anpassen möchten, wie Sie diese Leads erhalten, können Sie Ihren Prozess zur Generierung von Leads mithilfe des Diensts [Azure-Funktionen](https://azure.microsoft.com/services/functions/) überaus flexibel automatisieren. Es folgt ein Beispiel der Erstellung einer Azure-Funktion mit einem Timer, der alle fünf Minuten ausgeführt, in einer Azure-Tabelle nach neuen Datensätzen sucht und den Tarif „Free“ des SendGrid-Diensts verwendet, um eine einfache E-Mail-Benachrichtigung zu senden.

1. [Erstellen](https://portal.azure.com/#create/SendGrid.SendGrid) Sie in Ihrem Azure-Abonnement ein kostenloses SendGrid-Dienstkonto.
  
    ![Erstellen eines SendGrid-Kontos](./media/cloud-partner-portal-lead-management-instructions-azure-table/createsendgrid.png)
  
2. Erstellen Sie einen SendGrid-API-Schlüssel (klicken Sie auf die Schaltfläche „Verwalten“, um zur SendGrid-Benutzeroberfläche zu gelangen. Klicken Sie auf „Einstellungen ->PI-Schlüssel“, und erstellen einen Schlüssel mit „E-Mail senden -> Vollzugriff“, und speichern Sie den API-Schlüssel).
 
    ![SendGrid-API-Schlüssel](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridkey.png)

3. [Erstellen Sie](https://portal.azure.com/#create/Microsoft.FunctionApp) eine Azure-Funktionen-App mithilfe der Hostingplanoption „Verbrauchstarif“. 
  
    ![Erstellen einer Azure-Funktion](./media/cloud-partner-portal-lead-management-instructions-azure-table/createfunction.png)

4. Erstellen Sie eine neue Funktionsdefinition. 
  
    ![Erstellen einer Azure-Funktionsdefinition](./media/cloud-partner-portal-lead-management-instructions-azure-table/createdefinition.png)

5. Wenn Sie möchten, dass die Funktion zu einem bestimmten Zeitpunkt eine Aktualisierung sendet, wählen Sie „TimerTrigger-CSharp“ als Startoption.
  
    ![Azure-Funktion mit uhrzeitabhängiger Triggeroption](./media/cloud-partner-portal-lead-management-instructions-azure-table/timetrigger.png)
  
6. Ersetzen Sie den Code „Entwickeln“ durch den in der folgenden Tabelle. Passen Sie die E-Mail-Adressen an den Absender und Empfänger an.
Dies ist nur Beispielcode, den Sie bei Bedarf verbessern können.
  
        #r "Microsoft.WindowsAzure.Storage"
        #r "SendGrid"
        using Microsoft.WindowsAzure.Storage.Table;
        using System;
        using SendGrid;
        using SendGrid.Helpers.Mail;

        public class MyRow : TableEntity
        {
            public string Name { get; set; }
        }

        public static void Run(TimerInfo myTimer, IQueryable<MyRow> inputTable, out Mail message, TraceWriter log)
        {
            // UTC datetime that is 5.5 minutes ago while the cron timer schedule is every 5 minutes
            DateTime dateFrom = DateTime.UtcNow.AddSeconds(-(5 * 60 + 30));
            var emailFrom = "YOUR EMAIL";
            var emailTo = "YOUR EMAIL";
            var emailSubject = "Azure Table Notification";

            // Look in the table for rows that were added recently
            var rowsList = inputTable.Where(r => r.Timestamp > dateFrom).ToList();

            // Check how many rows were added
            int rowsCount = rowsList.Count;
            
            if (rowsCount > 0)
            {
                log.Info($"Found {rowsCount} rows added since {dateFrom} UTC");

                // Configure the email message describing how many rows were added
                message = new Mail
                {
                    From = new Email(emailFrom),
                    Subject = emailSubject + " (" + rowsCount + " new rows)"
                };

                var personalization = new Personalization();
                personalization.AddTo(new Email(emailTo));
                message.AddPersonalization(personalization);

                var content = new Content
                {
                    Type = "text/plain",
                    Value = "Found " + rowsCount + " new rows added since " + dateFrom.ToString("yyyy-MM-dd HH:mm:ss") + " UTC"
                };
                message.AddContent(content);
            }
            else
            {
                // Do not send the email if no new rows were found
                message = null;
            }
        }
   ![Codeausschnitt für Azure-Funktion](./media/cloud-partner-portal-lead-management-instructions-azure-table/code.png)

7. Klicken Sie auf „Integrieren“ und „Eingaben“, um die Azure-Tabellenverbindung zu definieren:
  
    ![Option „Integrieren“ für Azure-Funktion](./media/cloud-partner-portal-lead-management-instructions-azure-table/integrate.png)

8. Geben Sie den Tabellennamen ein, und definieren Sie die Verbindungszeichenfolge, indem Sie auf „Neu“ klicken.
  
    ![Verbindung mit Azure-Funktionen-Tabelle](./media/cloud-partner-portal-lead-management-instructions-azure-table/configtable.png)

9. Definieren Sie nun die Ausgabe als SendGrid, und übernehmen Sie alle Standardeinstellungen.
  
    ![SendGrid-Ausgabe](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridoutput.png)
    ![Standardwerte für SendGrid-Ausgabe](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridoutputdefaults.png)

10. Fügen Sie den SendGrid-API-Schlüssel den Einstellungen der Funktionen-App hinzu, indem Sie den Namen „SendGridApiKey“ und den Wert verwenden, den Sie aus „API-Schlüssel“ auf der SendGrid-Benutzeroberfläche abgerufen haben.
  
    ![Verwalten von SendGrid](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridmanage.png)
    ![SendGrid mit managekey](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridmanagekey.png)

Sobald dies alles konfiguriert ist, sollte der Code im Abschnitt „Integrieren“ wie folgt aussehen:

    {
      "bindings": [
        {
          "name": "myTimer",
          "type": "timerTrigger",
          "direction": "in",
          "schedule": "0 */5 * * * *"
        },
        {
          "type": "table",
          "name": "inputTable",
          "tableName": "MarketplaceLeads",
          "take": 50,
          "connection": "yourstorageaccount_STORAGE",
          "direction": "in"
        },
        {
          "type": "sendGrid",
          "name": "message",
          "apiKey": "SendGridApiKey",
          "direction": "out"
        }
      ],
      "disabled": false
    }
  
Abschließend müssen Sie zur Entwicklungsbenutzeroberfläche der Funktion zurückkehren und auf **Ausführen** klicken, um den Timer zu starten. Nun haben Sie alles eingerichtet, um Benachrichtigungen zu empfangen, wenn ein neuer Lead eingeht.

