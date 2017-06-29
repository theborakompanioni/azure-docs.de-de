---
title: "Azure Analysis Services-Tutorial – Lektion 11: Erstellen von Rollen | Microsoft-Dokumentation"
description: Dieser Artikel beschreibt, wie Rollen im Azure Analysis Services-Tutorialprojekt erstellt werden.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 085a36edd2a0e80123ac8754b438bceadfa6c0e9
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017

---
# <a name="lesson-11-create-roles"></a>Lektion 11: Erstellen von Rollen

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In dieser Lektion lernen Sie, wie Sie Rollen erstellen können. Rollen bieten Modelldatenbankobjekt- und Datensicherheit, indem sie den Zugriff auf diejenigen Benutzer einschränken, die Mitglieder der Rolle sind. Jede Rolle ist mit einer einzigen Berechtigung definiert: Keine, Lesen, Lesen und Verarbeiten, Verarbeiten und Administrator. Rollen können während des Schreibens des Modells mit dem Rollen-Manager definiert werden. Nachdem ein Modell bereitgestellt wurde, können Sie Rollen verwalten, indem Sie SSMS verwenden. Weitere Informationen finden Sie unter [Rollen](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).
  
> [!NOTE]  
> Das Erstellen von Rollen ist für den Abschluss dieses Tutorials nicht erforderlich. Standardmäßig hat das Konto, mit dem Sie gerade angemeldet sind, Administratorberechtigungen für das Modell. Sie müssen allerdings mindestens eine Rolle mit Leseberechtigungen erstellen und dieser Rolle Benutzer hinzufügen, damit andere Benutzer in Ihrer Organisation das Modell mit einem Berichterstellungsclient durchsuchen können.  
  
Sie erstellen drei Rollen:  
  
-   **Verkaufs-Manager**: Diese Rolle kann Benutzer Ihrer Organisation beinhalten, denen Sie Leseberechtigungen für alle Modellobjekte und -daten erteilen möchten.  
  
-   **Verkaufsanalytiker**: Diese Rolle kann Benutzer Ihrer Organisation beinhalten, die lediglich die Möglichkeit haben sollen, Daten zu durchsuchen, die mit dem Vertrieb in den USA in Zusammenhang stehen. Für diese Rollen verwenden Sie eine DAX-Formel, um einen *Zeilenfilter* zu definieren, der es Benutzer nur ermöglicht, Daten der USA zu durchsuchen.  
  
-   **Administrator**: Diese Rolle kann Benutzer beinhalten, denen Sie Administratorberechtigungen erteilen möchten. Damit haben sie unbegrenzten Zugriff und Berechtigungen, administrative Aufhaben in der Modelldatenbank auszuführen.  
  
Da Benutzer- und Gruppenkonten Ihrer Organisation unter Windows eindeutig sind, können Sie Membern Konten Ihrer bestimmten Organisation hinzufügen. Für dieses Tutorial können Sie die Member allerdings auch leer lassen. In „Lektion 12: Analysieren in Excel“ können Sie die Auswirkungen jeder Rolle testen.  
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **15 Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist Teil eines Tutorials zur Tabellenmodellierung, das in der richtigen Reihenfolge absolviert werden sollte. Bevor Sie diese Lektion beginnen, sollten Sie die vorherige [Lektion 10: Erstellen von Hierarchien](../tutorials/aas-lesson-10-create-partitions.md) abgeschlossen haben.  
  
## <a name="create-roles"></a>Erstellen von Rollen  
  
#### <a name="to-create-a-sales-manager-user-role"></a>So erstellen Sie die Benutzerrolle „Verkaufs-Manager“  
  
1.  Klicken Sie im Explorer für tabellarische Modelle mit der Rechten Maustaste auf **Rollen** > **Rollen**.  
  
2.  Klicken Sie im Rollen-Manager auf **Neu**.  
  
3.  Klicken Sie auf die neue Rolle und anschließend auf die Spalte **Name**, und benennen Sie die Rolle in **Verkaufs-Manager** um.  
  
4.  Klicken sie in der Spalte **Berechtigungen** auf die Dropdownliste, und wählen Sie anschließend die Berechtigung **Lesen** aus. 

    ![aas-lektion11-neue-rolle](../tutorials/media/aas-lesson11-new-role.png) 
  
5.  Optional: Klicken Sie auf die Registerkarte **Member** und anschließend auf **Hinzufügen**. Geben Sie im Dialogfeld **Select Users or Groups** (Benutzer oder Gruppen auswählen) die Windows-Benutzer oder -Gruppen Ihrer Organisation ein, denen Sie die Rolle erteilen möchten.  
  
#### <a name="to-create-a-sales-analyst-us-user-role"></a>So erstellen Sie die Benutzerrolle „Verkaufsanalytiker USA“  
  
1.  Klicken Sie im Rollen-Manager auf **Neu**.    
  
2.  Benennen Sie die Rolle in **Verkaufsanalytiker USA** um.  
  
3.  Erteilen Sie der Rolle die Berechtigung **Lesen**.  
  
4.  Klicken Sie auf die Registerkarte „Zeilenfilter“, und geben Sie nur für die Tabelle **DimGeography** in der Spalte „DAX-Filter“ folgende Formel ein:  
  
    ```Administrator
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    Eine Zeilenfilterformel muss einen Booleschen Wert ergeben (TRUE/FALSE). Mit dieser Formel geben Sie an, dass Benutzern nur Zeilen angezeigt werden, in denen der Wert des Landes/der Region dem Code „US“ entspricht.  
    ![aas-lektion11-rolle-filter](../tutorials/media/aas-lesson11-role-filter.png) 
  
6.  Optional: Klicken Sie auf die Registerkarte **Member** und anschließend auf **Hinzufügen**. Geben Sie im Dialogfeld **Select Users or Groups** (Benutzer oder Gruppen auswählen) die Windows-Benutzer oder -Gruppen Ihrer Organisation ein, denen Sie die Rolle erteilen möchten.  
  
#### <a name="to-create-an-administrator-user-role"></a>So erstellen Sie die Benutzerrolle „Administrator“  
  
1.  Klicken Sie auf **Neu**.  
  
2.  Benennen Sie die Rolle in **Administrator** um.  
  
3.  Erteilen Sie dieser Rolle die Berechtigung **Administrator**.  
  
4.  Optional: Klicken Sie auf die Registerkarte **Member** und anschließend auf **Hinzufügen**. Geben Sie im Dialogfeld **Select Users or Groups** (Benutzer oder Gruppen auswählen) die Windows-Benutzer oder -Gruppen Ihrer Organisation ein, denen Sie die Rolle erteilen möchten. 
  
  
## <a name="whats-next"></a>Wie geht es weiter?
[Lektion 12: Analysieren in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md)

  
  

