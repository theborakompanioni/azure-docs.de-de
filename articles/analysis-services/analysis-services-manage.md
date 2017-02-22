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
ms.date: 01/20/2017
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 13eb8ab1bf3c218f14b4c23ca1a46e9552d55b25
ms.openlocfilehash: a5db6cccf6c3dc55ee2cda59cb9e2ecd2292fcb5


---
# <a name="manage-analysis-services"></a>Verwalten von Analysis Services
Nachdem Sie in Azure einen Analysis Services-Server erstellt haben, müssen Sie möglicherweise sofort oder zu einem späteren Zeitpunkt einige Verwaltungsaufgaben ausführen. Sie können z. B. Daten aktualisieren, steuern, wem Zugriff auf die Modelle auf dem Server gewährt wird, oder die Integrität des Servers überwachen. Einige Verwaltungsaufgaben können nur im Azure-Portal, andere in SQL Server Management Studio (SSMS) und bestimmte sowohl im Portal als auch in SSMS ausgeführt werden.

## <a name="azure-portal"></a>Azure-Portal
Im [Azure-Portal](http://portal.azure.com/) können Sie Server erstellen und löschen, Serverressourcen überwachen, die Größe ändern und steuern, wer Zugriff auf die Server hat.  Wenn Probleme auftreten, können Sie eine Supportanfrage senden.

![Abrufen von Servernamen in Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Das Herstellen einer Verbindung mit dem Server in Azure entspricht dem Herstellen einer Verbindung mit einer Serverinstanz in der eigenen Organisation. In SSMS können Sie viele der gleichen Aufgaben ausführen, z. B. Verarbeiten von Daten oder Erstellen eines Verarbeitungsskripts, Verwalten von Rollen und Verwenden von PowerShell. [Laden Sie die aktuelle Version von SSMS herunter, und installieren Sie sie](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

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
   
    Hinweis: Wenn keine Active Directory-Authentifizierung angeboten wird, müssen Sie vermutlich [SSMS auf die neueste Version aktualisieren](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).
   
    ![Herstellen der Verbindung in SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

Da das Verwalten des Servers in Azure mithilfe von SSMS auf die gleiche Weise wie das Verwalten eines lokalen Servers erfolgt, wird dies hier nicht ausführlich beschrieben. Sämtliche Informationen, die Sie benötigen, finden Sie auf MSDN unter [Analysis Services-Instanzverwaltung](https://msdn.microsoft.com/library/hh230806.aspx).

## <a name="server-administrators-and-database-users"></a>Serveradministratoren und Datenbankbenutzer
In Azure Analysis Services gibt es zwei Benutzertypen: Serveradministratoren und Datenbankbenutzer. Beide Benutzertypen müssen in Ihrer Azure Active Directory-Instanz vorhanden sein und durch eine Organisations-E-Mail-Adresse oder einen UPN angegeben werden. Dies unterscheidet sich von lokalen tabellarischen Modelldatenbanken, die Serveradministratoren und Datenbankbenutzer mit Windows-Domänenbenutzernamen unterstützen. Weitere Informationen finden Sie unter [Verwalten von Benutzern in Azure Analysis Services](analysis-services-manage-users.md).


## <a name="troubleshooting-connection-problems"></a>Beheben von Verbindungsproblemen
Wenn Sie mit SSMS eine Verbindung zu Ihrem Server herstellen möchten, nachdem Sie versucht haben (wie in Schritt 3), sich mit einem nicht im Verbund befindlichen Konto anzumelden und die Verbindung fehlgeschlagen ist, könnte es sein, dass Sie Ihren Anmelde-Cache leeren müssen. Schließen Sie SSMS, bevor Sie diese Schritte befolgen.

1. Navigieren Sie im Datei-Explorer zu `C:\Users\<user_name>\AppData\Local\`.
2. Löschen Sie den **AADCacheOM**-Ordner.
3. Suchen Sie im **lokalen** Ordner nach DAT-Dateien, die mit dem Namen **omlibs-token-cache** beginnen. Wenn Sie welche finden, löschen Sie diese.
4. Öffnen Sie SSMS, und wiederholen Sie die Schritte unter [Verbinden mit SSMS](#to-connect-with-ssms).

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie auf dem neuen Server noch kein tabellarisches Modell bereitgestellt haben, ist jetzt ein guter Zeitpunkt dafür. Weitere Informationen finden Sie unter [Deploy to Azure Analysis Services](analysis-services-deploy.md) (Bereitstellen in Azure Analysis Services, in englischer Sprache).

Wenn Sie ein Modell auf dem Server bereitgestellt haben, können Sie mithilfe eines Clients oder Browsers eine Verbindung mit diesem herstellen. Weitere Informationen finden Sie unter [Get data from Azure Analysis Services](analysis-services-connect.md) (Abrufen von Daten aus Azure Analysis Services, in englischer Sprache).




<!--HONumber=Feb17_HO2-->


