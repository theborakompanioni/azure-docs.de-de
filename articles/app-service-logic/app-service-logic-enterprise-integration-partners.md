---
title: Informationen zu Partnern und Enterprise Integration Pack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Partner mit dem Enterprise Integration Pack und Logik-Apps verwenden
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: cgronlun
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b5272f3a09295aac62c8e9ffa17270803f556ba5


---
# <a name="learn-about-partners-and-enterprise-integration-pack"></a>Informationen zu Partnern und zum Enterprise Integration Pack
## <a name="overview"></a>Übersicht
Bevor Sie eine Partnerschaft einrichten können, müssen Sie und die Organisation, mit der Sie beabsichtigen, Geschäfte zu tätigen, Informationen austauschen, mit denen Hilfe Sie Nachrichten sowohl bestimmen als auch überprüfen können, die gegenseitig gesendet werden. Nachdem dies erfolgt ist und Sie bereit sind, die Geschäftsbeziehung zu starten, können Sie in Ihrem Integrationskonto einen *Partner* erstellen.

## <a name="what-is-a-partner"></a>Was ist ein Partner?
Partner sind die Entitäten, die am B2B-Nachrichtenaustausch (Business-To-Business) und B2B-Transaktionen teilnehmen. 

## <a name="how-are-partners-used"></a>Zwecke von Partnern
Partner werden verwendet, um Vereinbarungen zu erstellen. Eine Vereinbarung definiert die Details zu den Nachrichten, die zwischen Partnern ausgetauscht werden. 

Bevor Sie eine Vereinbarung erstellen können, müssen Sie Ihrem Integrationskonto mindestens zwei Partner hinzugefügt haben. Einer der Partner einer Vereinbarung muss Ihre Organisation sein. Der Partner, der Ihre Organisation darstellt, wird als **Hostpartner**bezeichnet. Der zweite Partner ist die andere Organisation, mit der Ihre Organisation Nachrichten austauscht. Der zweite Partner wird als **Gastpartner**bezeichnet. Der Gastpartner kann ein anderes Unternehmen oder sogar eine Abteilung innerhalb Ihrer Organisation sein.  

Nachdem Sie die Partner hinzugefügt haben, erstellen Sie mithilfe dieser Partner eine Vereinbarung. 

Die Empfangs- und Sendeeinstellungen orientieren sich am Hostpartner. Die Empfangseinstellungen in einer Vereinbarung bestimmen beispielsweise, wie der Hostpartner Nachrichten von einem Gastpartner empfängt. Die Sendeeinstellungen in der Vereinbarung geben an, wie der Hostpartner Nachrichten an den Gastpartner sendet.

## <a name="how-to-create-a-partner"></a>Erstellen eines Partners
Gehen Sie im Azure-Portal so vor:  

1. Wählen Sie **Durchsuchen**.  
   ![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus.     
   ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Wählen Sie das **Integrationskonto** aus, dem Sie die Partner hinzufügen möchten.  
   ![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Wählen Sie die Kachel **Partner** aus.  
   ![](./media/app-service-logic-enterprise-integration-partners/partner-1.png)  
5. Wählen Sie auf dem geöffneten Blatt „Partner“ die Schaltfläche **Hinzufügen**.  
   ![](./media/app-service-logic-enterprise-integration-partners/partner-2.png)  
6. Geben Sie einen **Namen** für den Partner ein, wählen Sie dann den **Qualifizierer**, und geben Sie abschließend einen **Wert** ein. Der Wert wird verwendet, um Dokumente zu identifizieren, die in Ihre Apps gelangen.  
   ![](./media/app-service-logic-enterprise-integration-partners/partner-3.png)  
7. Wählen Sie das Benachrichtigungssymbol *Glocke* aus, um den Status des Partnererstellungsprozesses anzuzeigen.  
   ![](./media/app-service-logic-enterprise-integration-partners/partner-4.png)  
8. Wählen Sie die Kachel **Partner**. Dadurch wird die Kachel aktualisiert. An der Erhöhung der Anzahl der Partner erkennen Sie, dass der neue Partner erfolgreich hinzugefügt wurde.    
   ![](./media/app-service-logic-enterprise-integration-partners/partner-5.png)  
9. Nachdem Sie die Kachel „Partner“ ausgewählt haben, wird der neu hinzugefügte Partner auf dem Blatt „Partner“ angezeigt.    
   ![](./media/app-service-logic-enterprise-integration-partners/partner-6.png)  

## <a name="how-to-edit-a-partner"></a>Bearbeiten eines Partners
Gehen Sie wie folgt vor, um einen Partner zu bearbeiten, der bereits in Ihrem Integrationskonto vorhanden ist:  

1. Wählen Sie die Kachel **Partner** aus.  
2. Wählen Sie auf dem geöffneten Blatt „Partner“ den Partner aus, den Sie bearbeiten möchten.  
3. Nehmen Sie auf der Kachel **Partner aktualisieren** die gewünschten Änderungen vor.  
4. Wenn Sie mit den Änderungen zufrieden sind, wählen Sie den Link **Speichern**. Wählen Sie andernfalls den Link **Verwerfen**.  
   ![](./media/app-service-logic-enterprise-integration-partners/edit-1.png)  

## <a name="how-to-delete-a-partner"></a>Löschen eines Partners
1. Wählen Sie die Kachel **Partner** aus.  
2. Wählen Sie auf dem geöffneten Blatt „Partner“ den Partner aus, den Sie bearbeiten möchten.  
3. Wählen Sie den Link **Löschen**.    
   ![](./media/app-service-logic-enterprise-integration-partners/delete-1.png)   

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zu Vereinbarungen](app-service-logic-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  




<!--HONumber=Nov16_HO3-->


