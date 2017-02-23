---
title: Herstellen einer Verbindung mit einem lokalen SAP-System in Azure Logic Apps | Microsoft-Dokumentation
description: Verwenden des lokalen Datengateways zum Herstellen einer Verbindung mit einem lokalen SAP-System in Ihrem Logik-App-Workflow
services: logic-apps
documentationcenter: 
author: padmavc
manager: anneta
ms.service: logic-apps
ms.devlang: wdl
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 72ac4936c656847fa07f1c1a37d6ddbf4ec1acf4
ms.openlocfilehash: 62b30acf324a5ed6a1b817157c86575d83b4104e


---
# <a name="use-the-sap-connector-in-your-logic-app"></a>Verwenden des SAP-Connectors in Logik-Apps 

Das lokale Datengateway ermöglicht das Verwalten von Daten und den sicheren Zugriff auf lokale Ressourcen. In diesem Artikel wird das Herstellen einer Verbindung mit einem lokalen SAP-System hergestellt, um ein IDOC über HTTP anzufordern und die Antwort zurückzusenden.    

 > [!NOTE]
> Aktuelle Einschränkungen:
 > - Bei der Logik-App tritt ein Timeout auf, wenn die Anforderung 90 Sekunden überschreitet. In diesem Szenario können Anforderungen blockiert werden. 
 > - In der Dateiauswahl werden nicht alle verfügbaren Felder angezeigt. In diesem Szenario können Sie Pfade manuell hinzufügen.

## <a name="prerequisites"></a>Voraussetzungen
- Installieren und konfigurieren Sie das neueste [lokale Datengateway](https://www.microsoft.com/download/details.aspx?id=53127), Version 1.15.6150.1 oder höher. In [Herstellen einer Verbindung mit dem lokalen Datengateway in einer Logik-App](http://aka.ms/logicapps-gateway) sind die Schritte aufgeführt. Das Gateway muss auf einem lokalen Computer installiert werden, bevor Sie fortfahren können.

- Sie müssen die aktuelle SAP-Clientbibliothek auf den Computer herunterladen, auf dem Sie das Datengateway installiert haben, und installieren. Verwenden Sie eine der folgenden SAP-Versionen: 
    - SAP Server
        - SAP ECC 6.0 Unicode
        - SAP ECC 6.0 Unicode mit EHP 4.0
        - SAP ECC 6.0 mit EHP 7.0 und allen EHP-Vorversionen
 
    - SAP Client
        - SAP RFC SDK 7.20 UNICODE
        - SAP .NET Connector (NCo) 3.0

## <a name="add-the-sap-connector"></a>Hinzufügen des SAP-Connectors

Der SAP-Connector bietet Aktionen, jedoch keine Trigger. Verwenden Sie daher am Anfang Ihres Workflows einen anderen Trigger. 

1. Fügen Sie den Anforderung-/Antwort-Trigger hinzu, und wählen Sie dann **Neuer Schritt**.
2. Wählen Sie **Aktion hinzufügen** und dann den SAP-Connector, indem Sie `SAP` in das Suchfeld eingeben:    
 ![Wählen von SAP Application Server oder SAP Message Server](media/logic-apps-using-sap-connector/picture1.png)

3. Wählen Sie je nach SAP-Setup **SAP** [Application Server](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) oder [Message Server](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm). Wenn noch keine Verbindung vorhanden ist, werden Sie aufgefordert, eine herzustellen: 
    1. Wählen Sie **Connect via on-premises data gateway** (Über lokales Datengateway verbinden) aus, und geben Sie die Details Ihres SAP-Systems ein:   
 ![Hinzufügen der Verbindungszeichenfolge für SAP](media/logic-apps-using-sap-connector/picture2.png)  
    2. Wählen Sie ein vorhandenes **Gateway** aus. Oder wählen Sie **Gateway installieren**, um ein neues Gateway zu installieren:    
 ![Installieren eines neuen Gateways](media/logic-apps-using-sap-connector/install-gateway.png)
  
    3. Nachdem Sie alle Details eingegeben haben, wählen Sie **Erstellen** aus. Azure Logic Apps konfiguriert und testet die Verbindung, um sicherzustellen, dass sie ordnungsgemäß funktioniert.

4. Geben Sie einen Namen für Ihre SAP-Verbindung ein.

5. Die verschiedenen SAP-Optionen sind jetzt verfügbar. Verwenden Sie die Dateiauswahl, um Ihre IDOC-Kategorie zu suchen. Oder geben Sie den Pfad manuell ein und wählen im Feld **Text** die HTTP-Antwort aus:     
 ![SAP-AKTION](media/logic-apps-using-sap-connector/picture3.png)

6. Erstellen Sie eine HTTP-Antwort, indem Sie eine neue Aktion hinzufügen. Die Antwortnachricht sollte von der SAP-Ausgabe stammen.

7. Speichern Sie Ihre Logik-App. Testen Sie sie durch Senden eines IDOC über die URL des HTTP-Triggers. Nachdem das IDOC gesendet wurde, warten Sie auf die Antwort der Logik-App:   

  > [!TIP]
  > Lesen Sie, wie Sie [Logik-Apps überwachen](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Nachdem der SAP-Connector Ihrer Logik-App hinzugefügt wurde, beginnen Sie mit dem Erkunden der anderen Funktionen:

  - BAPI
  - RFC

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie die Überprüfung, Transformation und andere BizTalk-ähnliche Aufgaben im [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) ausführen. 
- Erstellen einer [lokalen Verbindung](../logic-apps/logic-apps-gateway-connection.md) mit Logic Apps.



<!--HONumber=Feb17_HO1-->


