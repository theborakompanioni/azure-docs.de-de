---
title: Erstellen eines Tabellenmodells mit dem Azure Analysis Services-Web-Designer | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Azure Analysis Services-Tabellenmodell mit dem Web-Designer im Azure-Portal erstellen.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/21/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: bd58f1845dabf6afb47ce27236d14479677a8808
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="create-a-model-in-azure-portal"></a>Erstellen eines Modells im Azure-Portal

Das Feature Azure Analysis Services-Web-Designer (Vorschau) im Azure-Portal bietet eine schnelle und einfache Möglichkeit zum Erstellen und Bearbeiten von Tabellenmodellen und Abfragen von Modelldaten direkt im Browser. 

Beachten Sie, dass der Web-Designer eine **Vorschauversion** ist. Während immer neue Funktionen hinzugefügt werden, ist in der Vorschau die Funktionalität begrenzt. Bei komplexeren Modellentwicklungen und -tests ist es sinnvoll, Visual Studio (SSDT) und SQL Server Management Studio (SSMS) zu verwenden.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Analysis Services-Server auf der Ebene „Standard“ oder „Developer“. Neue, mit dem Web-Designer erstellte Modelle, sind DirectQuery, die nur von diesen Ebenen unterstützt werden.
- Eine Azure SQL-Datenbank, Azure SQL Data Warehouse oder eine Power BI Desktop-Datei (.pbix) als Datenquelle. Neue Modelle, die von Power BI Desktop-Dateien erstellt wurden, unterstützen Azure SQL-Datenbank, Azure SQL Data Warehouse, Oracle und Teradata-Datenquellen.
- Ein SQL Server-Konto und -Kennwort für das Herstellen einer Verbindung mit Azure SQL-Datenbank oder Azure SQL Data Warehouse-Datenquellen.

## <a name="to-create-a-new-tabular-model"></a>Erstellen eines neuen tabellarischen Modellprojekts

1. Klicken Sie auf dem Blatt **Übersicht** Ihres Servers auf **Web-Designer** und dann auf **Öffnen**.

    ![Erstellen eines Modells im Azure-Portal](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. Klicken Sie in **Web-Designer** > **Modelle** auf **+ Hinzufügen**.

    ![Erstellen eines Modells im Azure-Portal](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. Geben Sie einen Modellnamen in **Neues Modell** ein, und wählen Sie dann eine Datenquelle aus.

    ![Dialogfeld „Neues Modell“ im Azure-Portal](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. Geben Sie die Verbindungseigenschaften in **Verbinden** ein. Benutzername und Kennwort müssen ein SQL Server-Dienstkonto sein.

     ![Verbinden des Dialogfelds im Azure-Portal](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. Wählen Sie in **Tabellen und Ansichten** die Tabellen aus, die Sie in Ihr Modell einschließen möchten, und klicken Sie dann auf **Erstellen**. Es werden automatisch Beziehungen zwischen Tabellen mit einem Schlüsselpaar erstellt.

     ![Auswählen von Tabellen und Ansichten](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

Ihr neues Modell wird in Ihrem Browser angezeigt. Mögliche nächste Schritte:   

- Fragen Sie Modelldaten ab, indem Sie Felder in den Abfrage-Designer ziehen und Filter hinzufügen.
- Erstellen Sie neue Kennzahlen in Tabellen.
- Bearbeiten Sie die enthaltenen Modellmetadaten mithilfe des JSON-Editors.
- Öffnen Sie das Modell in Visual Studio (SSDT), Power BI Desktop oder Excel.

![Auswählen von Tabellen und Ansichten](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> Wenn Sie die darin enthaltenen Modellmetadaten bearbeiten oder neue Kennzahlen in Ihrem Browser erstellen, speichern Sie diese Änderungen Ihres Modells in Azure. Wenn Sie Ihr Modell auch in SSDT, Power BI Desktop oder Excel bearbeiten, ist Ihr Modell möglicherweise nicht mehr synchron.


## <a name="next-steps"></a>Nächste Schritte 
[Verwalten von Datenbankrollen und Benutzern](analysis-services-database-users.md)  
[Herstellen einer Verbindung mit Excel](analysis-services-connect-excel.md)  



