---
title: "Hinzufügen von Mobile Services mithilfe von verbundenen Diensten in Visual Studio | Microsoft Docs"
description: "Fügen Sie Mobile Services mithilfe des Dialogfelds &quot;Verbundene Dienste hinzufügen&quot; in Visual Studio hinzu"
services: visual-studio-online
documentationcenter: na
author: mlhoop
manager: douge
editor: 
ms.assetid: 75c3cb93-88e1-476d-a416-f34caa3608e3
ms.service: visual-studio-online
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: mobile
ms.date: 12/16/2015
ms.author: mlearned
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: dec722c49be99a0d186611e2ec8c324917609fdc


---
# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Hinzufügen von Mobile Services mithilfe von verbundenen Visual Studio-Diensten
Mit Visual Studio 2015 können Sie mithilfe des Dialogfelds **Verbundenen Dienst hinzufügen** eine Verbindung mit Azure Mobile Services herstellen. Sie können von jeder C#-Client-App, JavaScript-App oder plattformübergreifender Cordova-App eine Verbindung herstellen. Sobald Sie eine Verbindung herstellen, können Sie Daten erstellen und darauf zugreifen, benutzerdefinierten APIs und geplante Aufträge erstellen oder Unterstützung für Pushbenachrichtigungen hinzufügen.  Mit dem Vorgang für verbundene Dienste werden alle entsprechenden Verweise und der Verbindungscode hinzugefügt. Sie können auch die integrierte Unterstützung für die Authentifizierung bei einer Vielzahl von beliebten Identitätsschemas wie Azure AD, Facebook, Twitter und Microsoft-Konten nutzen.

## <a name="supported-project-types"></a>Unterstützte Projekttypen
> [!NOTE]
> In Visual Studio 2015 wird das Hinzufügen von Azure Mobile Services zu universellen Windows-Projekten (Windows 10) mit dem Dialogfeld "Verbundene Dienste hinzufügen" nicht unterstützt. Sie können Azure Mobile Services hinzufügen, indem Sie die entsprechenden Pakete mit dem NuGet-Paket-Manager für Ihr Projekt installieren.
> 
> 

Mit dem Dialogfeld "Verbundene Dienste" können Sie in den folgenden Projekttypen eine Verbindung mit Azure Mobile Services herstellen.

* .NET Windows 8.1 Store, Phone und universelle App-Projekte
* JavaScript Windows 8.1 Store, Phone und universelle App-Projekte
* Mit Visual Studio-Tools für Apache Cordova erstellte Projekte

## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Verwenden des Dialogfelds "Verbundenen Dienst hinzufügen" zum Verbinden mit Azure Mobile Services
1. Stellen Sie sicher, dass Sie über ein Azure-Konto verfügen. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose Testversion](http://go.microsoft.com/fwlink/?LinkId=518146)registrieren.
2. Öffnen Sie das Dialogfeld **Verbundene Dienste hinzufügen** .
   
   * Öffnen Sie für .NET-Apps das Projekt in Visual Studio, öffnen Sie das Kontextmenü für den Knoten **Verweise** im Projektmappen-Explorer, und wählen Sie dann **Verbundenen Dienst hinzufügen** aus.
     
        ![Herstellen einer Verbindung mit Azure Mobile Services](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)
   * Öffnen Sie für Apache Cordova-App-Projekte das Projekt in Visual Studio, öffnen Sie das Kontextmenü für den Projektknoten im Projektmappen-Explorer, und wählen Sie dann **Verbundenen Dienst hinzufügen**aus.
3. Wählen Sie im Dialogfeld **Verbundenen Dienst hinzufügen** die Option **Azure Mobile Services** aus, und klicken Sie dann auf die Schaltfläche **Konfigurieren**. Sie werden möglicherweise aufgefordert, sich bei Azure anzumelden, wenn Sie noch nicht angemeldet sind.
   
    ![Hinzufügen eines mobilen Diensts in Azure](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)
4. Wählen Sie im Dialogfeld **Azure Mobile Services** einen vorhandenen mobilen Dienst, sofern vorhanden. Wenn Sie einen neuen Azure Mobile Service erstellen müssen, befolgen Sie das im Folgenden geschilderte Verfahren. Gehen Sie andernfalls zum nächsten Schritt.
   
    So erstellen Sie ein neues Konto für einen mobilen Dienst
   
   1. Wählen Sie am unteren Rand des Fensters den Link **Dienst erstellen** aus.
       ![Hinzufügen eines neuen mobilen verbundenen Diensts](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)
   2. Im Dialogfeld **Mobilen Service erstellen** können Sie in der Dropdownliste **Laufzeit** einen mobilen JavaScript-Back-End-Dienst oder einen mobilen .NET-Back-End-Dienst auswählen. 
      
       ![Erstellen eines mobilen Diensts](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)
      
       Ein JavaScript-Back-End-Dienst ist einfach und leistungsstark. Wenn Sie einen mobilen JavaScript-Back-End-Dienst erstellen, wird der serverseitige JavaScript-Code in der Cloud gespeichert, aber Sie können mit dem Server-Explorer oder im Azure-Verwaltungsportal Serverskripts bearbeiten. 
      
       Ein mobiler .NET-Back-End-Dienst bietet Ihnen die volle Leistungsfähigkeit und Flexibilität von Web-APIs und Entity Framework. Wenn Sie einen mobilen .NET-Back-End-Dienst erstellen, wird ein Projekt für Sie erstellt und der Projektmappe hinzugefügt. 
   3. Wählen Sie die **Region** für den mobilen Dienst aus, und geben Sie dann einen Benutzernamen und ein Kennwort für den Server ein.
   4. Nachdem Sie alle erforderlichen Informationen eingegeben haben, klicken Sie auf die Schaltfläche **Erstellen** , um den mobilen Dienst zu erstellen.
   5. Der neue mobile Dienst sollte in der Dienstliste des Dialogfelds **Azure Mobile Services** angezeigt werden. Wählen Sie den neuen mobilen Dienst in der Liste und dann die Schaltfläche **Hinzufügen** aus, um den Dienst Ihrem Projekt hinzuzufügen.
5. Überprüfen Sie die Seite mit den ersten Schritten, die angezeigt wird, und finden Sie heraus, wie das Projekt geändert wurde. Eine Seite mit ersten Schritten wird im Browser angezeigt, sobald Sie einen verbundenen Dienst hinzufügen. Sie können die vorgeschlagenen nächsten Schritte und Codebeispiele prüfen oder zur Seite "Was ist passiert" wechseln, um herauszufinden, welche Verweise in Ihrem Projekt hinzugefügt wurden und wie Ihr Code und die Konfigurationsdateien geändert wurden.
6. Nutzen Sie die Codebeispiele als Leitfaden, um mit dem Schreiben von Code für den Zugriff auf Ihren mobilen Dienst zu beginnen.

## <a name="how-your-project-is-modified"></a>Änderungen am Projekt
Wie Visual Studio das Projekt ändert, hängt vom Projekttyp ab. Informationen zu C#-Client-Apps finden Sie unter [Was ist passiert – C#-Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513119). Informationen zu JavaScript-Client-Apps finden Sie unter [Was ist passiert – JavaScript-Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513120). Informationen zu Cordova-Apps finden Sie unter [Was ist passiert – Cordova Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513116).

## <a name="next-steps"></a>Nächste Schritte
Stellen Sie Fragen, und holen Sie sich Hilfe: 

* [MSDN-Forum: Azure Mobile Services](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)
* [Azure Mobile Services im Blog des Microsoft Azure-Teams](https://azure.microsoft.com/blog/topics/mobile/)
* [Azure Mobile Services auf "azure.microsoft.com"](https://azure.microsoft.com/services/mobile-services/)
* [Dokumentation für Azure Mobile Services auf "azure.microsoft.com"](https://azure.microsoft.com/documentation/services/mobile-services/)




<!--HONumber=Nov16_HO3-->


