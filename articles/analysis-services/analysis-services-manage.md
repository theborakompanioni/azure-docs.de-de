---
title: Verwalten von Azure Analysis Services | Microsoft Docs
description: Erfahren Sie, wie Sie in Azure einen Analysis Services-Server verwalten.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 79491d0b-b00d-4e02-9ca7-adc99bc02fdb
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 10/31/2016
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 193c939065979dc48243d31e7f97cd87d96bf9a8
ms.openlocfilehash: 55a016a0943885a3aaa636316808939777afb0f8


---
# <a name="manage-analysis-services"></a>Verwalten von Analysis Services
Nachdem Sie in Azure einen Analysis Services-Server erstellt haben, müssen Sie möglicherweise sofort oder zu einem späteren Zeitpunkt einige Verwaltungsaufgaben ausführen. Sie können z. B. Daten aktualisieren, steuern, wem Zugriff auf die Modelle auf dem Server gewährt wird, oder die Integrität des Servers überwachen. Einige Verwaltungsaufgaben können nur im Azure-Portal, andere in SQL Server Management Studio (SSMS) und bestimmte sowohl im Portal als auch in SSMS ausgeführt werden.

## <a name="azure-portal"></a>Azure-Portal
Im [Azure-Portal](http://portal.azure.com/) können Sie Server erstellen und löschen, Serverressourcen überwachen, die Größe ändern und steuern, wer Zugriff auf die Server hat.  Wenn Probleme auftreten, können Sie eine Supportanfrage senden.

![Abrufen von Servernamen in Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Das Herstellen einer Verbindung mit dem Server in Azure entspricht dem Herstellen einer Verbindung mit einer Serverinstanz in der eigenen Organisation. In SSMS können Sie viele der gleichen Aufgaben ausführen, z. B. Verarbeiten von Daten oder Erstellen eines Verarbeitungsskripts, Verwalten von Rollen und Verwenden von PowerShell.

![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

 Einer der größeren Unterschiede ist die Authentifizierung zum Herstellen einer Verbindung mit dem Server. Zum Herstellen einer Verbindung mit dem Azure Analysis Services-Server müssen Sie **Active Directory-Kennwortauthentifizierung** auswählen.

 Wenn Sie SSMS verwenden, stellen Sie sicher, dass Ihr Benutzername in der Analysis Services-Administratoren-Gruppe enthalten ist, bevor Sie das erste Mal eine Verbindung mit dem Server herstellen. Weitere Informationen finden Sie in diesem Artikel weiter unten unter [Serveradministratoren](#server-administrators).

### <a name="to-connect-with-ssms"></a>So stellen Sie eine Verbindung mit SSMS her
1. Bevor Sie eine Verbindung herstellen, müssen Sie den Namen des Servers abrufen. Kopieren Sie den Servernamen über **Azure-Portal** > Server > **Übersicht** > **Servername**.
   
    ![Abrufen von Servernamen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. Klicken Sie in SSMS im **Objekt-Explorer** auf **Verbinden** > **Analysis Services**.
3. Fügen Sie im Dialogfeld **Mit Server verbinden** den Servernamen ein, und wählen Sie dann in **Authentifizierung** eine der folgenden Optionen aus:
   
    **Integrierte Active Directory-Authentifizierung**, um einmaliges Anmelden mit Active Directory und Azure Active Directory-Verbund zu verwenden.
   
    **Active Directory-Kennwortauthentifizierung**, um ein Organisationskonto zu verwenden. Dies ist beispielsweise beim Herstellen einer Verbindung von einem nicht mit der Domäne verbundenen Computer sinnvoll.
   
    Hinweis: Wenn Active Directory-Authentifizierung nicht angezeigt wird, müssen Sie möglicherweise in SSMS [Azure Active Directory-Authentifizierung aktivieren](#enable-azure-active-directory-authentication).
   
    ![Herstellen der Verbindung in SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

Da das Verwalten des Servers in Azure mithilfe von SSMS auf die gleiche Weise wie das Verwalten eines lokalen Servers erfolgt, wird dies hier nicht ausführlich beschrieben. Sämtliche Informationen, die Sie benötigen, finden Sie auf MSDN unter [Analysis Services-Instanzverwaltung](https://msdn.microsoft.com/library/hh230806.aspx).

## <a name="server-administrators"></a>Serveradministratoren
Sie können zum Verwalten von Serveradministratoren die Option **Analysis Services-Administratoren** im Blatt für die Steuerung des Servers im Azure-Portal oder SSMS verwenden. Analysis Services-Administratoren sind Datenbankserveradministratoren mit Rechten für allgemeine Datenbankverwaltungsaufgaben, z. B. das Hinzufügen und Entfernen von Datenbanken sowie das Verwalten von Benutzern. Standardmäßig wird der Benutzer, der den Server im Azure-Portal erstellt, automatisch als Analysis Services-Administrator hinzugefügt.

Außerdem sollten Sie Folgendes wissen:

* Windows Live ID ist kein unterstützter Identitätstyp für Azure Analysis Services.  
* Analysis Services-Administratoren müssen gültigen Azure Active Directory-Benutzer sein.
* Beim Erstellen eines Azure Analysis Services-Servers über Azure Resource Manager-Vorlagen kann in „Analysis Services-Administratoren“ ein JSON-Array mit den Benutzern übergeben werden, die als Administratoren hinzugefügt werden sollten.

Analysis Services-Administratoren können sich von Azure-Ressourcenadministratoren unterscheiden, die Ressourcen für Azure-Abonnements verwalten können. Hierdurch bleibt die Kompatibilität mit vorhandenem XMLA- und TSML-Verwaltungsverhalten in Analysis Services erhalten, und Sie können Aufgaben zwischen der Azure-Ressourcenverwaltung und der Analysis Services-Datenbankverwaltung trennen.

Um alle Rollen und Zugriffstypen für die Azure Analysis Services-Ressource anzuzeigen, verwenden Sie die Zugriffssteuerung (IAM) im Blatt für die Steuerung.

## <a name="database-users"></a>Datenbankbenutzer
Benutzer der Azure Analysis Services-Modelldatenbank müssen in Azure Active Directory vorhanden sein. Für die Modelldatenbank angegebene Benutzernamen müssen die E-Mail-Adresse für die Organisation oder der UPN sein. Dies unterscheidet sich von lokalen Modelldatenbanken, die Benutzer mit Windows-Domänenbenutzernamen unterstützen.

Sie können Benutzer mithilfe von [Rollenzuweisungen in Azure Active Directory](../active-directory/role-based-access-control-configure.md) oder mithilfe von [Tabular Model Scripting Language](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) in SQL Server Management Studio hinzufügen.

**TMSL-Beispielskript**

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Users"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@contoso.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="enable-azure-active-directory-authentication"></a>Aktivieren der Azure Active Directory-Authentifizierung
Um in der Registrierung die Azure Active Directory-Authentifizierung für SSMS zu aktivieren, erstellen Sie eine Textdatei mit dem Namen „EnableAAD.reg“, und kopieren Sie dann Folgendes in die Datei:

```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\Microsoft Analysis Services\Settings]
"AS AAD Enabled"="True"
```

Speichern Sie die Datei, und führen Sie sie dann aus.

## <a name="troubleshooting-connection-problems"></a>Beheben von Verbindungsproblemen
Wenn Sie mit SSMS eine Verbindung zu Ihrem Server herstellen möchten, nachdem Sie versucht haben (wie in Schritt 3), sich mit einem nicht im Verbund befindlichen Konto anzumelden und die Verbindung fehlgeschlagen ist, könnte es sein, dass Sie Ihren Anmelde-Cache leeren müssen. Schließen Sie SSMS, bevor Sie diese Schritte befolgen.

1. Navigieren Sie im Datei-Explorer zu `C:\Users\<user_name>\AppData\Local\`.
2. Löschen Sie den **AADCacheOM**-Ordner.
3. Suchen Sie im **lokalen** Ordner nach DAT-Dateien, die mit dem Namen **omlibs-token-cache** beginnen. Wenn Sie welche finden, löschen Sie diese.
4. Öffnen Sie SSMS, und wiederholen Sie die Schritte unter [Verbinden mit SSMS](#to-connect-with-ssms).

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie auf dem neuen Server noch kein tabellarisches Modell bereitgestellt haben, ist jetzt ein guter Zeitpunkt dafür. Weitere Informationen finden Sie unter [Deploy to Azure Analysis Services](analysis-services-deploy.md) (Bereitstellen in Azure Analysis Services, in englischer Sprache).

Wenn Sie ein Modell auf dem Server bereitgestellt haben, können Sie mithilfe eines Clients oder Browsers eine Verbindung mit diesem herstellen. Weitere Informationen finden Sie unter [Get data from Azure Analysis Services](analysis-services-connect.md) (Abrufen von Daten aus Azure Analysis Services, in englischer Sprache).




<!--HONumber=Nov16_HO3-->


