---
title: "Übersicht über Integrationskonten und das Enterprise Integration Pack | Microsoft-Dokumentation"
description: "Erfahren Sie alles über Integrationskonten, das Enterprise Integration Pack und Logik-Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: ce2f01045195b9881bc1b3c337c453d10cd39540


---
# <a name="overview-of-integration-accounts"></a>Übersicht über Integrationskonten
## <a name="what-is-an-integration-account"></a>Was ist eine Integrationskonto?
Ein Integrationskonto ist ein Azure-Konto, mit dem Apps für die Unternehmensintegration Artefakte wie Schemas, Zuordnungen, Zertifikate, Partner und Vereinbarungen verwalten können. Von Ihnen erstellte Integrations-Apps benötigen ein Integrationskonto, um z.B. auf ein Schema, eine Zuordnung oder ein Zertifikat zuzugreifen.

## <a name="create-an-integration-account"></a>Erstellen eines Integrationskontos
1. Wählen Sie **Durchsuchen** .  
   ![](./media/logic-apps-enterprise-integration-accounts/account-1.png)  
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus.     
   ![](./media/logic-apps-enterprise-integration-accounts/account-2.png)  
3. Wählen im Menü oben auf der Seite die Schaltfläche *Hinzufügen* aus.      
   ![](./media/logic-apps-enterprise-integration-accounts/account-3.png)  
4. Geben Sie den Namen ins Feld **Name** ein, wählen Sie das zu verwendende **Abonnement** aus, und erstellen Sie eine neue **Ressourcengruppe**, oder wählen Sie eine vorhandene Ressourcengruppe aus. Wählen Sie zudem einen **Standort**, an dem das Integrationskonto gehostet wird, und einen **Tarif** aus, und wählen Sie dann die Schaltfläche **Erstellen**.   
   
   Das Integrationskonto wird dann am ausgewählten Speicherort bereitgestellt. Dieser Vorgang sollte nicht länger als 1 Minute dauern.    
   ![](./media/logic-apps-enterprise-integration-accounts/account-4.png)  
5. Aktualisieren Sie die Seite. Das neue Integrationskonto ist nun aufgeführt. Glückwunsch!  
   ![](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

## <a name="how-to-link-an-integration-account-to-a-logic-app"></a>Verknüpfen eines Integrationskontos mit einer Logik-App
Damit Ihre Logik-Apps auf Zuordnungen, Schemas, Vereinbarungen und andere Artefakte zugreifen können, die in Ihrem Integrationskonto enthalten sind, müssen Sie zuerst das Integrationskonto mit Ihrer Logik-App verknüpfen.

### <a name="here-are-the-steps-to-link-an-integration-account-to-a-logic-app"></a>Es folgen die Schritte zum Verknüpfen eines Integrationskontos mit einer Logik-App.
#### <a name="prerequisites"></a>Voraussetzungen
* Ein Integrationskonto
* Eine Logik-App

> [!NOTE]
> Vergewissern Sie sich, dass sich Ihr Integrationskonto und Ihre Logik-App am **gleichen Azure-Speicherort** befinden, bevor Sie beginnen.
> 
> 

1. Wählen Sie im Menü Ihrer Logik-App den Link **Einstellungen**.  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)   
2. Wählen Sie auf dem Blatt „Einstellungen“ das Element **Integrationskonto** aus.  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)   
3. Wählen Sie in der Dropdownliste **Wählen Sie ein Integrationskonto aus.** das Integrationskonto aus, das Sie mit Ihrer Logik-App verknüpfen möchten.  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)   
4. Speichern Sie Ihre Arbeit.  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-4.png)   
5. Es wird die Benachrichtigung angezeigt, dass Ihr Integrationskonto mit Ihrer Logik-App verknüpft wurde und dass alle Artefakte in Ihrem Integrationskonto nun Ihrer Logik-App zur Verfügung stehen.  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)   

Nun da Ihr Integrationskonto mit Ihrer Logik-App verknüpft ist, können Sie zu Ihrer Logik-App wechseln und B2B-Connectors verwenden, wie z.B. die XML-Validierung, Codierung/Decodierung von Flatfiles oder Transformation, um Apps mit B2B-Features zu erstellen.  

## <a name="how-to-delete-an-integration-account"></a>Löschen eines Integrationskontos
1. Wählen Sie **Durchsuchen**.  
   ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)    
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus.     
   ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)  
3. Wählen Sie das **Integrationskonto** aus, das Sie löschen möchten.  
   ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)  
4. Wählen Sie im Menü den Link **Löschen**.   
   ![](./media/logic-apps-enterprise-integration-accounts/delete.png)  
5. Bestätigen Sie Ihre Auswahl.    

## <a name="how-to-move-an-integration-account"></a>Verschieben eines Integrationskontos
Sie können ein Integrationskonto einfach in ein neues Abonnement und eine neue Ressourcengruppe verschieben. Gehen Sie folgendermaßen vor, wenn Sie Ihr Integrationskonto verschieben müssen:

> [!IMPORTANT]
> Nach dem Verschieben eines Integrationskontos müssen Sie alle Skripts für die Verwendung der neuen Ressourcen-IDs ändern.
> 
> 

1. Wählen Sie **Durchsuchen**.  
   ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)    
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus.     
   ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)  
3. Wählen Sie das **Integrationskonto** aus, das Sie löschen möchten.  
   ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)  
4. Wählen Sie im Menü den Link **Verschieben**.   
   ![](./media/logic-apps-enterprise-integration-accounts/move.png)  
5. Bestätigen Sie Ihre Auswahl.    

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zu Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  




<!--HONumber=Jan17_HO3-->


