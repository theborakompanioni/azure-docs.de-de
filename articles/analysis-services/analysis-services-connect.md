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
ms.date: 02/13/2017
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: eccc181e2203a97de58005f9597613775338ebcc
ms.lasthandoff: 03/25/2017


---
# <a name="get-data-from-azure-analysis-services"></a>Abrufen von Daten aus Azure Analysis Services

Sobald Sie in Azure einen Server erstellt, und ein tabellarisches Modell bereitgestellt haben, können Benutzer in Ihrer Organisation beginnen, eine Verbindung herzustellen und Daten zu durchsuchen.

## <a name="data-providers-aka-client-libraries"></a>Datenanbieter (oder auch Clientbibliotheken)

Clientanwendungen wie Power BI Desktop und Microsoft Excel nutzen aktualisierte AMO-, ADOMD.NET- und OLEDB-Anbieter, um eine Verbindung und Schnittstelle mit Analysis Services herzustellen. Bei einigen älteren Versionen von Excel oder benutzerdefinierten Anwendungen müssen Sie unter Umständen die aktuellen Datenanbieter installieren, um eine Verbindung mit Azure Analysis Services herstellen zu können. Weitere Informationen finden Sie unter [Datenanbieter](analysis-services-data-providers.md).

## <a name="server-name"></a>Servername

Wenn Sie einen Analysis Services-Server in Azure erstellen, geben Sie einen eindeutigen Namen, und die Region an, in der der Server erstellt werden soll. Wenn Sie einen Servernamen in der Verbindung angeben, lautet das Benennungsschema des Servers wie folgt:

```
<protocol>://<region>/<servername>
```
 Wenn „protocol“ die Zeichenfolge **asazure** enthält, ist die Region der URI der Region, in welcher der Server erstellt wurde (z.B. westus.asazure.windows.net bei USA, Westen), und der Servername der Name Ihres eindeutigen Servers innerhalb der Region.

## <a name="get-the-server-name"></a>Abrufen des Servernamens

Bevor Sie eine Verbindung herstellen, müssen Sie den Namen des Servers abrufen. Kopieren Sie den gesamten Servernamen über **Azure-Portal** > Server > **Übersicht** > **Servername**. Wenn auch andere Benutzer in Ihrer Organisation Verbindungen mit diesem Server herstellen, können Sie diesen Servernamen mit ihnen teilen. Wenn Sie einen Servernamen angeben, muss der gesamte Pfad verwendet werden.

![Abrufen von Servernamen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

## <a name="connect-in-power-bi-desktop"></a>Herstellen einer Verbindung in Power BI Desktop

> [!NOTE]
> Dieses Feature befindet sich in der Vorschau.
> 
> 

1. Klicken Sie in [Power BI Desktop](https://powerbi.microsoft.com/desktop/) auf **Daten abrufen** > **Azure** > **Microsoft Azure Analysis Services-Datenbank**.
2. Fügen Sie unter **Server** den Servernamen aus der Zwischenablage ein.
3. Wenn Sie den Namen der Datenbank für tabellarische Modelle oder die Perspektive kennen, mit der Sie sich verbinden wollen, fügen Sie ihn unter **Datenbanken** ein. Andernfalls können Sie dieses Feld leer lassen. Sie können eine Datenbank oder eine Perspektive auf dem nächsten Bildschirm auswählen.
4. Lassen Sie den Standardwert **Live verbinden** ausgewählt, und klicken Sie dann auf **Verbinden**. Geben Sie Ihr Organisationskonto ein, wenn Sie aufgefordert werden ein Konto einzugeben.
5. Erweitern Sie unter **Navigator** den Server, wählen Sie das Modell oder die Perspektive aus, mit denen Sie eine Verbindung herstellen möchten, und klicken Sie dann auf **Verbinden**. Durch Klicken auf ein Modell oder eine Perspektive werden alle Objekte für diese Ansicht gezeigt.

## <a name="connect-in-power-bi"></a>Herstellen einer Verbindung in Power BI

1. Erstellen Sie eine Power BI Desktop-Datei, die über eine Liveverbindung zu Ihrem Modell auf dem Server verfügt.
2. Klicken Sie unter [Power BI](https://powerbi.microsoft.com) auf **Daten abrufen** > **Dateien**. Suchen und markieren Sie Ihre Datei.

## <a name="connect-in-excel"></a>Eine Verbindung in Excel herstellen

Das Herstellen einer Verbindung zu einem Azure Analysis Services-Server in Excel wird unterstützt. Dies geschieht mithilfe der Funktionen „Daten abrufen“ in Excel 2016 oder „Power Query“ in früheren Versionen. Ein [MSOLAP.7 ](analysis-services-data-providers.md)-Anbieter ist erforderlich. Das Herstellen einer Verbindung mithilfe des Import Table Wizard (Assistent „Tabelle importieren“) in Power Pivot wird nicht unterstützt.

> [!NOTE]
> Einige Organisationen stellen Office 365-Updates auf dem verzögerten Kanal bereit. Das bedeutet, dass Versionsupdates bis zu vier Monate von der aktuellen Version verzögert werden. Für Excel 2016-Versionen bis 1609.7369.2115 oder Excel 2013 können Sie eine ODC-Datei erstellen und den MSOLAP.7-Anbieter für die Verbindung mit einem Server manuell aktualisieren. Weitere Informationen finden Sie unter [Erstellen einer ODC-Datei](analysis-services-odc.md).
> 
> 

**So stellen Sie Verbindungen von Excel 2016 her**

1. Klicken Sie in Excel 2016 im Menüband **Daten** auf **Get External Data (Externe Daten abrufen)** > **Aus anderen Quellen** > **Aus Analysis Services**.
2. Fügen Sie im Datenverbindungs-Assistenten unter **Servername** den Servernamen aus der Zwischenablage ein. Wählen Sie dann unter **Anmeldeinformationen** die Option **Benutzername und Kennwort verwenden** aus, und geben Sie den Benutzernamen der Organisation, z.B. nancy@adventureworks.com, und das Kennwort ein.

    ![Verbindung in Excel herstellen – logon](./media/analysis-services-connect/aas-connect-excel-logon.png)
3. Wählen Sie unter **Datenbank und Tabelle wählen** die Datenbank und das Modell oder die Perspektive aus, und klicken Sie dann auf **Fertig stellen**.
   
    ![Verbindung in Excel herstellen – Modell auswählen](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Verbindungszeichenfolge

Wenn Sie eine Verbindung zu Azure Analysis Services mithilfe des Tabellenobjektmodells herstellen, verwenden Sie folgende Formate für Verbindungszeichenfolgen:

###### <a name="integrated-azure-active-directory-authentication"></a>Integrierte Authentifizierung über Azure Active Directory

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
Die integrierte Authentifizierung ruft den Cache für Anmeldeinformationen von Azure Active Directory ab, falls vorhanden. Wenn dies nicht der Fall ist, öffnet sich das Azure-Anmeldefenster.

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Authentifizierung über Azure Active Directory mit Benutzername und Kennwort

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="next-steps"></a>Nächste Schritte

[Manage your server (Verwalten des Servers)](analysis-services-manage.md)


