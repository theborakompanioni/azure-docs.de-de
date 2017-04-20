---
title: Verwalten von Benutzern in Azure Analysis Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Benutzer auf einem Analysis Services-Server in Azure verwalten.
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
ms.date: 04/18/2016
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 194910a3e4cb655b39a64d2540994d90d34a68e4
ms.openlocfilehash: 039ed6f4be9f3e0f6b92e5a9f11e12392912df9d
ms.lasthandoff: 02/16/2017


---
# <a name="manage-users-in-azure-analysis-services"></a>Verwalten von Benutzern in Azure Analysis Services
In Azure Analysis Services gibt es zwei Benutzertypen: Serveradministratoren und Datenbankbenutzer. 

## <a name="server-administrators"></a>Serveradministratoren
Zum Verwalten von Serveradministratoren können Sie die Option **Analysis Services-Administratoren** auf dem Blatt für die Steuerung des Servers im Azure-Portal oder die Servereigenschaften in SSMS verwenden. Analysis Services-Administratoren sind Datenbankserveradministratoren mit Rechten für allgemeine Datenbankverwaltungsaufgaben, z.B. das Hinzufügen und Entfernen von Datenbanken sowie das Verwalten von Benutzern. Standardmäßig wird der Benutzer, der den Server im Azure-Portal erstellt, automatisch als Analysis Services-Administrator hinzugefügt.

![Serveradministratoren im Azure-Portal](./media/analysis-services-manage-users/aas-manage-users-admins.png)

Außerdem sollten Sie Folgendes wissen:

* Windows Live ID ist kein unterstützter Identitätstyp für Azure Analysis Services.  
* Analysis Services-Administratoren müssen gültigen Azure Active Directory-Benutzer sein.
* Beim Erstellen eines Azure Analysis Services-Servers über Azure Resource Manager-Vorlagen kann in „Analysis Services-Administratoren“ ein JSON-Array mit den Benutzern übergeben werden, die als Administratoren hinzugefügt werden sollten.

Analysis Services-Administratoren können sich von Azure-Ressourcenadministratoren unterscheiden, die Ressourcen für Azure-Abonnements verwalten können. Hierdurch bleibt die Kompatibilität mit vorhandenem XMLA- und TSML-Verwaltungsverhalten in Analysis Services erhalten, und Sie können Aufgaben zwischen der Azure-Ressourcenverwaltung und der Analysis Services-Datenbankverwaltung trennen. Um alle Rollen und Zugriffstypen für die Azure Analysis Services-Ressource anzuzeigen, verwenden Sie die Zugriffssteuerung (IAM) im Blatt für die Steuerung.

### <a name="to-add-administrators-using-azure-portal"></a>Hinzufügen von Administratoren über das Azure-Portal
1. Klicken Sie auf dem Steuerungsblatt für den Server auf **Analysis Services-Administratoren**.
2. Klicken Sie auf dem Blatt **\<Servername >-Analysis Services-Administratoren** auf **Hinzufügen**.
3. Wählen Sie auf dem Blatt **Serveradministratoren hinzufügen** die hinzuzufügenden Benutzerkonten aus.

## <a name="database-users"></a>Datenbankbenutzer
Datenbankbenutzer müssen Datenbankrollen hinzugefügt werden. Rollen definieren Benutzer und Gruppen, die über dieselben Berechtigungen für eine Datenbank verfügen. Datenbanken mit tabellarischem Modell haben standardmäßig eine Standardrolle für Benutzer mit Leseberechtigungen. Weitere Informationen finden Sie unter [Rollen in tabellarischen Modellen](https://msdn.microsoft.com/library/hh213165.aspx).

Benutzer der Azure Analysis Services-Modelldatenbank *müssen in Ihrem Azure Active Directory vorhanden sein*. Angegebene Benutzernamen müssen die E-Mail-Adresse für die Organisation oder der UPN sein. Dies unterscheidet sich von lokalen tabellarischen Modelldatenbanken, die Benutzer mit Windows-Domänenbenutzernamen unterstützen. 

Das Erstellen von Datenbankrollen, das Hinzufügen von Benutzern und Gruppen zu Rollen sowie das Konfigurieren von Sicherheit auf Zeilenebene ist in SQL Server Data Tools (SSDT) oder in SQL Server Management Studio (SSMS) möglich. Sie können auch [Analysis Services-PowerShell-Cmdlets](https://msdn.microsoft.com/library/hh758425.aspx) oder [Tabular Model Scripting Language](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) verwenden, um Benutzern zu Rollen hinzuzufügen oder daraus zu entfernen.

**TMSL-Beispielskript**

In diesem Beispiel werden der Rolle „Benutzer“ die SalesBI-Datenbank ein Benutzer und eine Gruppe hinzugefügt.

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

## <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)

Abonnementadministratoren können **Access Control** auf dem Blatt „Steuerung“ verwenden, um Rollen zu konfigurieren. Dies ist nicht identisch mit Serveradministratoren oder Datenbankbenutzern, die wie oben beschrieben auf Server- oder Datenbankebene konfiguriert werden. 

![Access Control im Azure-Portal](./media/analysis-services-manage-users/aas-manage-users-rbac.png)

Rollen gelten für Benutzer oder Konten, die sie zum Ausführen von Aufgaben im Portal oder mithilfe von Azure Resource Manager-Vorlagen benötigen. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-what-is.md).

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie auf dem Server noch kein tabellarisches Modell bereitgestellt haben, ist jetzt ein guter Zeitpunkt dafür. Weitere Informationen finden Sie unter [Deploy to Azure Analysis Services](analysis-services-deploy.md) (Bereitstellen in Azure Analysis Services, in englischer Sprache).

Wenn Sie ein Modell auf dem Server bereitgestellt haben, können Sie mithilfe eines Clients oder Browsers eine Verbindung mit diesem herstellen. Weitere Informationen finden Sie unter [Get data from Azure Analysis Services](analysis-services-connect.md) (Abrufen von Daten aus Azure Analysis Services, in englischer Sprache).


