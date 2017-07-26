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
ms.date: 06/14/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 8be2bcb9179e9af0957fcee69680ac803fd3d918
ms.openlocfilehash: d41dc3c05d0d0cd0d9466c43bbebadcffd21e0dd
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017


---
# <a name="manage-analysis-services"></a>Verwalten von Analysis Services
Nachdem Sie in Azure einen Analysis Services-Server erstellt haben, müssen Sie möglicherweise sofort oder zu einem späteren Zeitpunkt einige Verwaltungsaufgaben ausführen. Sie können z. B. Daten aktualisieren, steuern, wem Zugriff auf die Modelle auf dem Server gewährt wird, oder die Integrität des Servers überwachen. Einige Verwaltungsaufgaben können nur im Azure-Portal, andere in SQL Server Management Studio (SSMS) und bestimmte sowohl im Portal als auch in SSMS ausgeführt werden.

## <a name="azure-portal"></a>Azure-Portal
Im [Azure-Portal](http://portal.azure.com/) können Sie Server erstellen und löschen, Serverressourcen überwachen, die Größe ändern und steuern, wer Zugriff auf die Server hat.  Wenn Probleme auftreten, können Sie eine Supportanfrage senden.

![Abrufen von Servernamen in Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Das Herstellen einer Verbindung mit dem Server in Azure entspricht dem Herstellen einer Verbindung mit einer Serverinstanz in der eigenen Organisation. In SSMS können Sie viele der gleichen Aufgaben ausführen, z. B. Verarbeiten von Daten oder Erstellen eines Verarbeitungsskripts, Verwalten von Rollen und Verwenden von PowerShell.
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a>Herunterladen und Installieren von SSMS
Verwenden Sie die neueste Version von SSMS, um alle aktuellen Features zu erhalten und die Verbindung mit dem Azure Analysis Services-Server reibungslos herzustellen. 

[Laden Sie SQL Server Management Studio herunter](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


### <a name="to-connect-with-ssms"></a>So stellen Sie eine Verbindung mit SSMS her
 Wenn Sie SSMS verwenden, stellen Sie sicher, dass Ihr Benutzername in der Analysis Services-Administratoren-Gruppe enthalten ist, bevor Sie das erste Mal eine Verbindung mit dem Server herstellen. Weitere Informationen finden Sie in diesem Artikel weiter unten unter [Serveradministratoren](#server-administrators).

1. Bevor Sie eine Verbindung herstellen, müssen Sie den Namen des Servers abrufen. Kopieren Sie den Servernamen über **Azure-Portal** > Server > **Übersicht** > **Servername**.
   
    ![Abrufen von Servernamen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. Klicken Sie in SSMS im **Objekt-Explorer** auf **Verbinden** > **Analysis Services**.
3. Fügen Sie im Dialogfeld **Mit Server verbinden** den Servernamen ein, und wählen Sie dann in **Authentifizierung** einen der folgenden Authentifizierungstypen aus:
   
    **Windows-Authentifizierung**, um Ihre Windows-Domäne\Ihren Benutzernamen und Ihr Kennwort als Anmeldeinformationen zu verwenden.

    **Active Directory-Kennwortauthentifizierung**, um ein Organisationskonto zu verwenden. Dies ist beispielsweise beim Herstellen einer Verbindung von einem nicht mit der Domäne verbundenen Computer sinnvoll.

    **Universelle Active Directory-Authentifizierung**, um [nicht interaktive oder Multi-Factor Authentication](../sql-database/sql-database-ssms-mfa-authentication.md) zu verwenden. 
   
    ![Herstellen der Verbindung in SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a>Serveradministratoren und Datenbankbenutzer
In Azure Analysis Services gibt es zwei Benutzertypen: Serveradministratoren und Datenbankbenutzer. Beide Benutzertypen müssen in Ihrer Azure Active Directory-Instanz vorhanden sein und durch eine Organisations-E-Mail-Adresse oder einen UPN angegeben werden. Weitere Informationen finden Sie unter [Authentifizierung und Benutzerberechtigungen](analysis-services-manage-users.md).


## <a name="troubleshooting-connection-problems"></a>Beheben von Verbindungsproblemen
Wenn beim Herstellen einer Verbindung mithilfe von SSMS Probleme auftreten, müssen Sie den Anmeldecache löschen. Auf dem Datenträger wird nichts zwischengespeichert. Beenden Sie den Verbindungsvorgang, und starten Sie ihn neu, um den Cache zu löschen. 

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie auf dem neuen Server noch kein tabellarisches Modell bereitgestellt haben, ist jetzt ein guter Zeitpunkt dafür. Weitere Informationen finden Sie unter [Deploy to Azure Analysis Services](analysis-services-deploy.md) (Bereitstellen in Azure Analysis Services, in englischer Sprache).

Wenn Sie ein Modell auf dem Server bereitgestellt haben, können Sie mithilfe eines Clients oder Browsers eine Verbindung mit diesem herstellen. Weitere Informationen finden Sie unter [Get data from Azure Analysis Services](analysis-services-connect.md) (Abrufen von Daten aus Azure Analysis Services, in englischer Sprache).


