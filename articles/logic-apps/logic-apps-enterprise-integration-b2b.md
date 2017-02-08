---
title: "Erstellen von B2B-Lösungen mit dem Enterprise Integration Pack | Microsoft-Dokumentation"
description: Informationen zum Empfangen von Daten mithilfe der B2B-Features des Enterprise Integration Packs
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 20fc3722-6f8b-402f-b391-b84e9df6fcff
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: cb8cac0c63930139760617d721faffeef70cfbec


---
# <a name="learn-about-receiving-data-using-the-b2b-features-of-the-enterprise-integration-pack"></a>Informationen zum Empfangen von Daten mithilfe der B2B-Features des Enterprise Integration Packs
## <a name="overview"></a>Übersicht
Dieses Dokument gehört zum Enterprise Integration Pack für Logik-Apps. Sehen Sie sich die Übersicht an, um weitere Informationen zu den [Funktionen des Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)zu erhalten.

## <a name="prerequisites"></a>Voraussetzungen
Um die AS2- und X12-Aktionen verwenden zu können, benötigen Sie ein Unternehmensintegrationskonto.

[Erstellen eines Unternehmensintegrationskontos](../logic-apps/logic-apps-enterprise-integration-accounts.md)

## <a name="how-to-use-the-logic-apps-b2b-connectors"></a>Verwenden der B2B-Connectors für Logik-Apps
Sobald Sie ein Integrationskonto erstellt und diesem Partner und Vereinbarungen hinzugefügt haben, können Sie eine Logik-App erstellen, die einen B2B-Workflow (Business-to-Business) implementiert.

In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie die AS2- und X12-Aktionen zum Erstellen einer B2B-Logik-App verwenden, die Daten von einem Handelspartner empfängt.

1. Erstellen Sie eine neue Logik-App, und [verknüpfen Sie sie mit Ihrem Integrationskonto](../logic-apps/logic-apps-enterprise-integration-accounts.md).  
2. Fügen Sie Ihrer Logik-App den Trigger **Anforderung – Wenn eine HTTP-Anforderung empfangen wird** hinzu.  
   ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)  
3. Fügen Sie die Aktion **AS2 decodieren** hinzu, indem Sie zuerst **Aktion hinzufügen** auswählen.  
   ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)  
4. Geben Sie **as2** in das Suchfeld ein, um die Aktion herauszufiltern, die Sie verwenden möchten.  
   ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)  
5. Wählen Sie die Aktion **AS2 – AS2-Nachricht decodieren** aus.  
   ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)  
6. Fügen Sie den **Text** hinzu, den Sie als Eingabe verwenden. Wählen Sie in diesem Beispiel den Text der HTTP-Anforderung aus, die die Logik-App ausgelöst hat. Alternativ können Sie einen Ausdruck eingeben, um die Header in das Feld**HEADERS** einzugeben:
   
    @triggerOutputs()['headers']
7. Fügen Sie die **Header** hinzu, die für AS2 erforderlich sind. Diese sind in den HTTP-Anforderungsheadern enthalten. Wählen Sie in diesem Beispiel die Header der HTTP-Anforderung aus, die die Logik-App ausgelöst hat.
8. Fügen Sie jetzt die Aktion „X12-Nachricht decodieren“ hinzu, indem Sie nochmals **Aktion hinzufügen** auswählen.  
   ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)   
9. Geben Sie **x12** in das Suchfeld ein, um die Aktion herauszufiltern, die Sie verwenden möchten.  
   ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)  
10. Wählen Sie die Aktion **X12 – X12-Nachricht decodieren** aus, um sie der Logik-App hinzuzufügen.  
    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)  
11. Nun müssen Sie die Eingabe in diese Aktion angeben, die die Ausgabe der obigen AS2-Aktion sein wird. Der tatsächliche Nachrichteninhalt befindet sich in einem JSON-Objekt und ist mit base64 codiert. Deshalb müssen Sie einen Ausdruck als Eingabe angeben. Geben Sie daher den folgenden Ausdruck in das Eingabefeld **X12 FLAT FILE MESSAGE TO DECODE** (ZU DECODIERENDE X12-FLATFILENACHRICHT) ein.  
    
    @base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])  
12. In diesem Schritt werden die vom Handelspartner empfangenen X12-Daten decodiert, und in einem JSON-Objekt werden verschiedene Elemente ausgegeben. Um den Partner über den Empfang der Daten zu informieren, können Sie eine Nachricht zurücksenden, die eine AS2 MDN (Message Disposition Notification, Benachrichtigung über den Nachrichtenstatus) in einer HTTP-Antwortaktion enthält.  
13. Fügen Sie die Aktion **Antwort** hinzu, indem Sie **Aktion hinzufügen**  auswählen.  
    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)  
14. Geben Sie das Wort **Antwort** in das Suchfeld ein, um die Aktion herauszufiltern, die Sie verwenden möchten.  
    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)  
15. Wählen Sie die Aktion **Antwort** aus, um sie hinzuzufügen.  
    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)  
16. Legen Sie das Antwortfeld **TEXT** mit dem folgenden Ausdruck fest, um auf die MDN in der Ausgabe der Aktion **X12-Nachricht decodieren** zuzugreifen.  
    
    @base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])  

![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

1. Speichern Sie Ihre Arbeit.  
   ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

An diesem Punkt ist das Einrichten der B2B-Logik-App abgeschlossen. In einer realen Anwendung werden die decodierten X12-Daten in einer branchenspezifischen Anwendung oder einem Datenspeicher gespeichert. Sie können hierfür problemlos weitere Aktionen hinzufügen oder eine benutzerdefinierte API schreiben, um eine Verbindung mit Ihren branchenspezifischen Anwendungen herzustellen und diese APIs in Ihrer Logik-App zu verwenden.

## <a name="features-and-use-cases"></a>Features und Anwendungsfälle
* Die AS2- und X12-Codierungs- und Decodierungsaktionen ermöglichen Ihnen das Empfangen von Daten von und Senden von Daten an Handelspartner mithilfe von Protokollen gemäß Industriestandard und Logik-Apps.  
* Sie können AS2 und X12 zusammen oder einzeln verwenden, um Daten nach Bedarf mit Handelspartnern auszutauschen.
* Die B2B-Aktionen erleichtern das Erstellen von Partnern und Vereinbarungen im Integrationskonto und deren Nutzung in einer Logik-App.  
* Durch das Erweitern Ihrer Logik-App mit anderen Aktionen können Sie Daten mit anderen Anwendungen und Diensten wie Salesforce austauschen.  

## <a name="learn-more"></a>Weitere Informationen
[Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)  




<!--HONumber=Jan17_HO3-->


