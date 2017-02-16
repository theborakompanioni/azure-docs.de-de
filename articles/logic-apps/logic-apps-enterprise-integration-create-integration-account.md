---
title: "Erstellen, Verknüpfen, Löschen oder Verschieben eines Integrationskontos in Azure Logic Apps | Microsoft-Dokumentation"
description: "Erstellen eines Integrationskontos und Verknüpfen eines Integrationskontos mit Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: dca762854f22721de76f69cbc28c0377d563fe28
ms.openlocfilehash: e26271a1291fc76e3ea8f93a7aa9ab8c811ff604


---

# <a name="what-is-an-integration-account"></a>Was ist eine Integrationskonto?
Mit einem Integrationskonto können Apps für die Unternehmensintegration Artefakte wie Schemas, Zuordnungen, Zertifikate, Partner und Vereinbarungen verwalten. Jede Integrations-App, die Sie erstellen, verwendet ein Integrationskonto, um auf diese Schemas, Zuordnungen, Zertifikate usw. zuzugreifen.

## <a name="create-an-integration-account"></a>Erstellen eines Integrationskontos
1. Wählen Sie **Durchsuchen**:   
   ![](./media/logic-apps-enterprise-integration-accounts/account-1.png)  
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus:     
   ![](./media/logic-apps-enterprise-integration-accounts/account-2.png)  
3. Wählen im Menü oben auf der Seite die Schaltfläche *Hinzufügen* aus:      
   ![](./media/logic-apps-enterprise-integration-accounts/account-3.png)  
4. Geben Sie den Namen ins Feld **Name** ein, wählen Sie das zu verwendende **Abonnement** aus, und erstellen Sie eine neue **Ressourcengruppe**, oder wählen Sie eine vorhandene Ressourcengruppe aus. Wählen Sie zudem einen **Standort**, an dem das Integrationskonto gehostet wird, und einen **Tarif** aus, und wählen Sie dann die Schaltfläche **Erstellen**.   
   
   Das Integrationskonto wird dann am ausgewählten Speicherort bereitgestellt. Dieser Vorgang sollte nicht länger als 1 Minute dauern.    
   ![](./media/logic-apps-enterprise-integration-accounts/account-4.png)  
5. Aktualisieren Sie die Seite. Das neue Integrationskonto ist nun aufgeführt:  
   ![](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

Anschließend verknüpfen Sie das Integrationskonto, das Sie gerade erstellt haben, mit Ihrer Logic App. 

## <a name="link-an-integration-account-to-a-logic-app"></a>Verknüpfen eines Integrationskontos mit einer Logic App
Damit Ihre Logic Apps auf Zuordnungen, Schemas, Vereinbarungen und andere Artefakte in Ihrem Integrationskonto zugreifen können, müssen Sie das Integrationskonto mit Ihrer Logic App verknüpfen.

#### <a name="prereqs"></a>Voraussetzungen
* Ein Integrationskonto
* Eine Logic App

> [!NOTE] 
> Bevor Sie beginnen, vergewissern Sie sich, dass sich Ihr Integrationskonto und Ihre Logic App am **gleichen Azure-Speicherort** befinden.

1. Wählen Sie im Menü Ihrer Logic App den Link **Einstellungen**:  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)   
2. Wählen Sie auf dem Blatt „Einstellungen“ das Element **Integrationskonto** aus:  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)   
3. Wählen Sie in der Dropdownliste **Wählen Sie ein Integrationskonto aus.** das Integrationskonto aus, das Sie mit Ihrer Logic App verknüpfen möchten:  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)   
4. Speichern Sie Ihre Arbeit:  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-4.png)   
5. Es wird die Benachrichtigung angezeigt, dass Ihr Integrationskonto mit Ihrer Logic App verknüpft wurde, und dass alle Artefakte in Ihrem Integrationskonto nun Ihrer Logic App zur Verfügung stehen:  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)   

Da Ihr Integrationskonto jetzt mit Ihrer Logic App verknüpft ist, können Sie die B2B-Connectors in den Logic Apps verwenden. Einige gängige B2B-Connectors enthalten XML-Überprüfung und Flatfilecodierung/-decodierung.  

## <a name="how-to-delete-an-integration-account"></a>Löschen eines Integrationskontos
1. Wählen Sie **Durchsuchen**:  
   ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)    
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus:     
   ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)  
3. Wählen Sie das **Integrationskonto** aus, das Sie löschen möchten:  
   ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)  
4. Wählen Sie im Menü den Link **Löschen**:   
   ![](./media/logic-apps-enterprise-integration-accounts/delete.png)  
5. Bestätigen Sie Ihre Auswahl.    

## <a name="how-to-move-an-integration-account"></a>Verschieben eines Integrationskontos
Sie können ein Integrationskonto einfach in ein neues Abonnement und eine neue Ressourcengruppe verschieben. Gehen Sie folgendermaßen vor, wenn Sie Ihr Integrationskonto verschieben müssen:

> [!IMPORTANT]
> Nach dem Verschieben eines Integrationskontos müssen Sie alle Skripts für die Verwendung der neuen Ressourcen-IDs aktualisieren.

1. Wählen Sie **Durchsuchen**:  
   ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)    
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus:     
   ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)  
3. Wählen Sie das **Integrationskonto** aus, das Sie löschen möchten:  
   ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)  
4. Wählen Sie im Menü den Link **Verschieben**:   
   ![](./media/logic-apps-enterprise-integration-accounts/move.png)  
5. Bestätigen Sie Ihre Auswahl.    

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zu Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  




<!--HONumber=Jan17_HO5-->


