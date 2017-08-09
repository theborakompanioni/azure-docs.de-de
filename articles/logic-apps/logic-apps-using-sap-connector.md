---
title: Herstellen einer Verbindung mit einem lokalen SAP-System in Azure Logic Apps | Microsoft-Dokumentation
description: "Herstellen einer Verbindung mit einem lokalen SAP-System in Ihrem Logik-App-Workflow über das lokale Datengateway"
services: logic-apps
author: padmavc
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: LADocs; padmavc
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 3fea93f558d5a4ef62550fd1f6486903cb812930
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---

# <a name="connect-to-an-on-premises-sap-system-from-logic-apps-with-the-sap-connector"></a>Herstellen einer Verbindung mit einem lokalen SAP-System in Logik-Apps mit dem SAP-Connector 

Das lokale Datengateway ermöglicht das Verwalten von Daten und den sicheren Zugriff auf lokale Ressourcen. In diesem Thema wird gezeigt, wie Sie eine Verbindung zwischen Logik-Apps und einem lokalen SAP-System herstellen können. In diesem Beispiel wird mit der Logik-App ein IDoc über HTTP angefordert und die Antwort zurückgesendet.    

> [!NOTE]
> Aktuelle Einschränkungen: 
> - Bei der Logik-App tritt ein Timeout auf, wenn nicht alle für die Antwort erforderlichen Schritte innerhalb des [Anforderungstimeouts](./logic-apps-limits-and-config.md) abgeschlossen wurden. In diesem Szenario können Anforderungen blockiert werden. 
> - In der Dateiauswahl werden nicht alle verfügbaren Felder angezeigt. In diesem Szenario können Sie Pfade manuell hinzufügen.

## <a name="prerequisites"></a>Voraussetzungen

- Installieren und konfigurieren Sie das neueste [lokale Datengateway](https://www.microsoft.com/download/details.aspx?id=53127), Version 1.15.6150.1 oder höher. In [Herstellen einer Verbindung mit dem lokalen Datengateway in einer Logik-App](http://aka.ms/logicapps-gateway) sind die Schritte aufgeführt. Das Gateway muss auf einem lokalen Computer installiert werden, bevor Sie fortfahren können.

- Sie müssen die aktuelle SAP-Clientbibliothek auf den Computer herunterladen, auf dem Sie das Datengateway installiert haben, und installieren. Verwenden Sie eine der folgenden SAP-Versionen: 
    - SAP Server
        - SAP Server, der .NET Connector (NCo) 3.0 unterstützt
 
    - SAP Client
        - SAP .NET Connector (NCo) 3.0

## <a name="add-triggers-and-actions-for-connecting-to-your-sap-system"></a>Hinzufügen von Triggern und Aktionen für die Verbindung mit dem SAP-System

Der SAP-Connector umfasst Aktionen, jedoch keine Trigger. Am Anfang des Workflows muss daher ein anderer Trigger verwendet werden. 

1. Fügen Sie den Anforderung-/Antwort-Trigger hinzu, und wählen Sie dann **Neuer Schritt**.

2. Wählen Sie **Aktion hinzufügen** und dann den SAP-Connector, indem Sie `SAP` in das Suchfeld eingeben:    

     ![Auswählen von „SAP Application Server“ (SAP-Anwendungsserver) oder „SAP Message Server“ (SAP-Nachrichtenserver)](media/logic-apps-using-sap-connector/sap-action.png)

3. Wählen Sie je nach SAP-Setup [**SAP Application Server**](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) (SAP-Anwendungsserver) oder [**SAP Message Server**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm) (SAP-Nachrichtenserver) aus. Wenn noch keine Verbindung vorhanden ist, werden Sie aufgefordert, eine herzustellen.

   1. Wählen Sie **Connect via on-premises data gateway** (Über lokales Datengateway verbinden) aus, und geben Sie die Details Ihres SAP-Systems ein:   

       ![Hinzufügen der Verbindungszeichenfolge für SAP](media/logic-apps-using-sap-connector/picture2.png)  

   2. Wählen Sie unter **Gateway** ein vorhandenes Gateway aus, oder wählen Sie **Gateway installieren** aus, um ein neues Gateway zu installieren.

        ![Installieren eines neuen Gateways](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. Nachdem Sie alle Details eingegeben haben, wählen Sie **Erstellen** aus. 
   Die Verbindung wird in Logic Apps konfiguriert und getestet, um sicherzustellen, dass sie ordnungsgemäß funktioniert.

4. Geben Sie einen Namen für Ihre SAP-Verbindung ein.

5. Die verschiedenen SAP-Optionen sind jetzt verfügbar. Suchen Sie die IDoc-Kategorie in der Liste, und wählen Sie sie aus. Oder geben Sie den Pfad manuell ein und wählen im Feld **Text** die HTTP-Antwort aus:

     ![SAP-Aktion](media/logic-apps-using-sap-connector/picture3.png)

6. Fügen Sie die Aktion zum Erstellen einer **HTTP-Antwort** hinzu. Die Antwortnachricht sollte von der SAP-Ausgabe stammen.

7. Speichern Sie Ihre Logik-App. Testen Sie sie durch Senden eines IDOC über die URL des HTTP-Triggers. Nachdem das IDoc gesendet wurde, warten Sie auf die Antwort der Logik-App:   

     > [!TIP]
     > Lesen Sie, wie Sie [Logik-Apps überwachen](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Nachdem der SAP-Connector Ihrer Logik-App hinzugefügt wurde, beginnen Sie mit dem Erkunden der anderen Funktionen:

- BAPI
- RFC

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Im [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) können Sie Fragen stellen, Fragen beantworten und sich über die Aktivitäten anderer Azure Logic Apps-Benutzer informieren.

Zur Optimierung von Azure Logic Apps und Connectors können Sie auf der [Benutzerfeedbackwebsite für Azure Logic Apps](http://aka.ms/logicapps-wish) über Ideen abstimmen oder selbst Ideen einreichen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie die Überprüfung, Transformation und andere BizTalk-ähnliche Aufgaben im [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) ausführen. 
- [Herstellen einer Verbindung mit lokalen Daten für Logik-Apps](../logic-apps/logic-apps-gateway-connection.md)

