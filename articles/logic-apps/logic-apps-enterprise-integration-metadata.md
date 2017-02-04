---
title: "Metadaten für das Azure Logic Apps-Integrationskonto | Microsoft Docs"
description: "Übersicht über Metadaten für Integrationskonten"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: ba109fdc2a97e99f6216aa038261e6ccc5e356d9
ms.openlocfilehash: 68a1b8515bbcebbe7ce4d4224a4015f18af5040b


---
# <a name="azure-logic-apps-integration-account-metadata"></a>Metadaten für das Azure Logic Apps-Integrationskonto 

## <a name="overview"></a>Übersicht

Für Partner, Vereinbarungen, Schemas und Zuordnungen, die einem Integrationskonto hinzugefügt werden, werden Metadaten als Schlüssel-Wert-Paare gespeichert. Sie können benutzerdefinierten Metadaten definieren, die zur Laufzeit abgerufen werden können.  Derzeit bieten die Artefakte keine Möglichkeit zum Erstellen von Metadaten auf der Benutzeroberfläche. Verwenden Sie die REST-API, um sie zu erstellen.  Partner, Vereinbarungen und Schema bieten die Option **EDIT as JSON** (Als JSON bearbeiten) und lassen die Eingabe von Metadateninformationen zu.  In einer Logik-App hilft die Option **Integration Account Artifact LookUp** (Nachschlagen von Integrationskontoartefakten) beim Abrufen der Metadateninformationen.

## <a name="how-to-store-metadata"></a>Speichern von Metadaten 

1. Erstellen Sie ein [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md).   

2. Fügen Sie einem Integrationskonto einen [Partner](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), eine [Vereinbarung](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements) oder ein [Schema](logic-apps-enterprise-integration-schemas.md) hinzu.

3. Wählen Sie einen Partner oder eine Vereinbarung oder ein Schema aus. Wählen Sie **Edit as JSON** (Als JSON bearbeiten) aus, und geben Sie Metadatendetails ein.    
![Geben Sie Metadaten ein](media/logic-apps-enterprise-integration-metadata/image1.png)  

## <a name="call-integration-account-artifact-lookup-from-a-logic-app"></a>Rufen Sie die Option zum **Integration Account Artifact LookUp** (Integrationskontoartefakt nachschlagen) in einer Logik-App auf.

1. Erstellen Sie eine [Logik-App](logic-apps-create-a-logic-app.md).

2. [Verknüpfen](logic-apps-enterprise-integration-create-integration-account.md#how-to-link-an-integration-account-to-a-logic-app) Sie die Logik-App mit einem Integrationskonto.    

3. Erstellen Sie einen Trigger, z.B. unter Verwendung von *Request* oder *HTTP*, vor dem Suchen nach **Integration Account Artifact LookUp** (Integrationskontoartefakt nachschlagen).  Suchen Sie **Integration**, um nach **Integration Account Artifact LookUp (Integrationskontoartefakt nachschlagen)** 
![Suchen](media/logic-apps-enterprise-integration-metadata/image2.png) zu suchen. 

3. Wählen Sie **Integration Account Artifact LookUp** (Integrationskontoartefakt nachschlagen) aus.  

4. Wählen Sie **Artefakttyp** aus, und geben Sie den **Namen des Artefakts** ein.  
![Suchen Sie „lookup“ (Suchen).](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="an-example-to-retrieve-partner-metadata"></a>Beispiel zum Abrufen von Metadaten für Partner 

1. Partnermetadaten mit Details zur Routing-URL    
![Suchen Sie „lookup“ (Suchen).](media/logic-apps-enterprise-integration-metadata/image6.png)

2. Konfigurieren Sie in einer Logik-App **Integration Account Artifact LookUp** (Integrationskontoartefakt nachschlagen) und **HTTP** .  
![Suchen Sie „lookup“ (Suchen).](media/logic-apps-enterprise-integration-metadata/image4.png)

3. Zum Abrufen des URI sollte die Codeansicht aussehen wie    
![Suchen Sie „lookup“ (Suchen).](media/logic-apps-enterprise-integration-metadata/image5.png)


## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zu Vereinbarungen](logic-apps-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  


<!--HONumber=Jan17_HO3-->


