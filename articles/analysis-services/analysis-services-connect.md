---
title: Abrufen von Daten aus Azure Analysis Services | Microsoft Docs
description: Erfahren Sie, wie Sie in Azure eine Verbindung mit Analysis Services herstellen und Daten abrufen.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: b37f70a0-9166-4173-932d-935d769539d1
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/28/2016
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 193c939065979dc48243d31e7f97cd87d96bf9a8
ms.openlocfilehash: 6b65689c75247af81e916730f125ff39bb49489f


---
# <a name="get-data-from-azure-analysis-services"></a>Abrufen von Daten aus Azure Analysis Services
Sobald Sie in Azure einen Server erstellt, und ein tabellarisches Modell bereitgestellt haben, können Benutzer in Ihrer Organisation beginnen, eine Verbindung herzustellen und Daten zu durchsuchen.

Azure Analysis Services unterstützt Clientverbindungen, die [aktualisierte Objektmodelle](#client-libraries) verwenden; TOM, AMO, Adomd.Net oder MSOLAP, um via XMLA eine Verbindung zum Server herzustellen. Beispiel: Power BI, Power BI Desktop, Excel, oder eine beliebige Clientanwendung, die die Objektmodelle unterstützt.

## <a name="server-name"></a>Servername
Wenn Sie einen Analysis Services-Server in Azure erstellen, geben Sie einen eindeutigen Namen, und die Region an, in der der Server erstellt werden soll. Wenn Sie einen Servernamen in der Verbindung angeben, lautet das Benennungsschema des Servers wie folgt:

```
<protocol>://<region>/<servername>
```
 Wenn „protocol“ die Zeichenfolge **asazure** enthält, ist die Region der URI der Region, in welcher der Server erstellt wurde (z.B. westus.asazure.windows.net bei USA, Westen), und der Servername der Name Ihres eindeutigen Servers innerhalb der Region.

## <a name="get-the-server-name"></a>Abrufen des Servernamens
Bevor Sie eine Verbindung herstellen, müssen Sie den Namen des Servers abrufen. Kopieren Sie den gesamten Servernamen über **Azure-Portal** > Server > **Übersicht** > **Servername**. Wenn sich andere Benutzer in Ihrer Organisation ebenfalls mit diesem Server verbinden, möchten Sie diesen Servernamen vermutlich mit ihnen teilen. Wenn Sie einen Servernamen angeben, muss der gesamte Pfad verwendet werden.

![Abrufen von Servernamen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

## <a name="connect-in-power-bi-desktop"></a>Herstellen einer Verbindung in Power BI Desktop
> [!NOTE]
> Dieses Feature befindet sich in der Vorschau.
> 
> 

1. Klicken Sie in [Power BI Desktop](https://powerbi.microsoft.com/desktop/) auf **Daten abrufen** > **Datenbanken** > **Azure Analysis Services**.
2. Fügen Sie unter **Server** den Servernamen aus der Zwischenablage ein.
3. Wenn Sie den Namen der Datenbank für tabellarische Modelle oder die Perspektive kennen, mit der Sie sich verbinden wollen, fügen Sie ihn unter **Datenbanken** ein. Andernfalls können Sie dieses Feld leer lassen. Sie können eine Datenbank oder eine Perspektive auf dem nächsten Bildschirm auswählen.
4. Lassen Sie den Standardwert **Live verbinden** ausgewählt, und klicken Sie dann auf **Verbinden**. Geben Sie Ihr Organisationskonto ein, wenn Sie aufgefordert werden ein Konto einzugeben.
5. Erweitern Sie unter **Navigator** den Server, wählen Sie das Modell oder die Perspektive aus, mit denen Sie eine Verbindung herstellen möchten, und klicken Sie dann auf **Verbinden**. Mit nur einem Klick auf ein Modell oder eine Perspektive werden alle Objekte für diese Ansicht gezeigt.

## <a name="connect-in-power-bi"></a>Herstellen einer Verbindung in Power BI
1. Erstellen Sie eine Power BI Desktop-Datei, die über eine Liveverbindung zu Ihrem Modell auf dem Server verfügt.
2. Klicken Sie unter [Power BI](https://powerbi.microsoft.com) auf **Daten abrufen** > **Dateien**. Suchen und markieren Sie Ihre Datei.

## <a name="connect-in-excel"></a>Eine Verbindung in Excel herstellen
Das Herstellen einer Verbindung zu einem Azure Analysis Services-Server in Excel wird unterstützt. Dies geschieht mithilfe der Funktionen „Daten abrufen“ in Excel 2016 oder „Power Query“ in früheren Versionen. Ein [MSOLAP.7 ](https://aka.ms/msolap)-Anbieter ist erforderlich. Das Herstellen einer Verbindung mithilfe des Import Table Wizard (Assistent „Tabelle importieren“) in Power Pivot wird nicht unterstützt.

1. Klicken Sie in Excel 2016 im Menüband **Daten** auf **Get External Data (Externe Daten abrufen)** > **Aus anderen Quellen** > **Aus Analysis Services**.
2. Fügen Sie im Datenverbindungs-Assistenten unter **Servername** den Servernamen aus der Zwischenablage ein. Wählen Sie dann unter **Anmeldeinformationen**, **Benutzername und Kennwort verwenden**, und geben Sie dann den Benutzernamen der Organisation, z.B. nancy@adventureworks.com,, und das Passwort ein.
   
    ![Verbindung in Excel herstellen – logon](./media/analysis-services-connect/aas-connect-excel-logon.png)
3. Wählen Sie unter **Datenbank und Tabelle wählen** die Datenbank und das Modell oder die Perspektive aus, und klicken Sie dann auf **Fertig stellen**.
   
    ![Verbindung in Excel herstellen – Modell auswählen](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Verbindungszeichenfolge
Wenn Sie eine Verbindung zu Azure Analysis Services mithilfe des Tabellenobjektmodells herstellen, verwenden Sie folgende Formate für Verbindungszeichenfolgen:

###### <a name="integrated-azure-active-directory-authentication"></a>Integrierte Authentifizierung über Azure Active Directory
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
Die integrierte Authentifizierung ruft das Cache für Anmeldeinformationen von Azure Active Directory ab, falls vorhanden. Wenn dies nicht der Fall ist, öffnet sich das Azure-Anmeldefenster.

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Authentifizierung über Azure Active Directory mit Benutzername und Kennwort
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>Clientbibliotheken
Wenn Sie von Excel oder anderen Schnittstellen, wie TOM, AsCmD oder ADOMD.NET, eine Verbindung zu Azure Analysis Services herstellen, müssen Sie möglicherweise die aktuellsten Clientbibliotheken des Anbieters herunterladen. Hier finden Sie die aktuellsten:  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>Nächste Schritte
[Manage your server (Verwalten des Servers)](analysis-services-manage.md)




<!--HONumber=Nov16_HO3-->


