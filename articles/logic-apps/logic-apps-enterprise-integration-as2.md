---
title: "Informationen zum Erstellen einer AS2-Vereinbarung für das Enterprise Integration Pack | Microsoft-Dokumentation"
description: "Informationen zum Erstellen einer AS2-Vereinbarung für das Enterprise Integration Pack | Azure-Logik-Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: 
ms.assetid: c9b7e1a9-4791-474c-855f-988bd7bf4b7f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 47a1c5653d84a55224c18222bc547e1c863ea442
ms.openlocfilehash: ea256557a38b3ce6cb457d195fa4b48cfd8879b8


---
# <a name="enterprise-integration-with-as2"></a>Unternehmensintegration mit AS2
## <a name="create-an-as2-agreement"></a>Erstellen einer AS2-Vereinbarung
Zum Verwenden der Features für die Unternehmensintegration in Logik-Apps müssen Sie zuerst Vereinbarungen erstellen. 

### <a name="heres-what-you-need-before-you-get-started"></a>Bevor Sie beginnen, benötigen Sie Folgendes:
* Ein in Ihrem Azure-Abonnement definiertes [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-accounts.md)  
* Mindestens zwei [Partner](logic-apps-enterprise-integration-partners.md), die bereits in Ihrem Integrationskonto definiert sind  

> [!NOTE]
> Beim Erstellen einer Vereinbarung muss der Inhalt der Vereinbarungsdatei dem Vereinbarungstyp entsprechen.    
> 
> 

Nachdem Sie [ein Integrationskonto erstellt](../logic-apps/logic-apps-enterprise-integration-accounts.md) und [Partner hinzugefügt](logic-apps-enterprise-integration-partners.md) haben, können Sie eine Vereinbarung erstellen, indem Sie folgende Schritte ausführen:  

### <a name="from-the-azure-portal-home-page"></a>Auf der Startseite des Azure-Portals
Nach der Anmeldung beim [Azure-Portal](http://portal.azure.com "Azure-Portal"):  

1. Wählen Sie **Weitere Dienste**, und geben Sie im Filtersuchfeld den Begriff **Integration** ein. Wählen Sie in der Ergebnisliste **Integrationskonten** aus.    
![](./media/logic-apps-enterprise-integration-agreements/overview-1.png)    
2. Wählen Sie das Integrationskonto aus, dem Sie das Zertifikat hinzufügen möchten. 
![](./media/logic-apps-enterprise-integration-overview/overview-3.png)  
3. Wählen Sie die Kachel **Vereinbarungen** aus. Wenn die Kachel „Vereinbarungen“ nicht angezeigt wird, fügen Sie sie zunächst hinzu.   
![](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)   
4. Wählen Sie auf dem geöffneten Blatt „Vereinbarungen“ die Schaltfläche **Hinzufügen** aus.  
![](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)  
5. Geben Sie einen **Namen** für die Vereinbarung ein. Wählen Sie dann auf dem Blatt „Vereinbarungen“ den **Vereinbarungstyp** **AS2**, den **Hostpartner**, die **Hostidentität**, den **Gastpartner** und die **Gastidentität** aus.  
![](./media/logic-apps-enterprise-integration-agreements/agreement-3.png)  

Es folgen Details, die nützlich sein können, wenn Sie die Einstellungen für die Vereinbarung konfigurieren: 

| Eigenschaft | Beschreibung |
| --- | --- |
| Hostpartner |Eine Vereinbarung benötigt einen Host- und einen Gastpartner. Der Hostpartner stellt die Organisation dar, die die Vereinbarung konfiguriert. |
| Hostidentität |Ein Bezeichner für den Hostpartner. |
| Gastpartner |Eine Vereinbarung benötigt einen Host- und einen Gastpartner. Der Gastpartner stellt die Organisation dar, die Geschäfte mit dem Hostpartner tätigt. |
| Gastidentität |Ein Bezeichner für den Gastpartner. |
| Empfangseinstellungen |Diese Eigenschaften gelten für alle Nachrichten, die von einer Vereinbarung empfangen werden. |
| Sendeeinstellungen |Diese Eigenschaften gelten für alle Nachrichten, die von einer Vereinbarung gesendet werden. |

Lassen Sie uns fortfahren:  

1. Wählen Sie **Empfangseinstellungen** aus, um zu konfigurieren, wie Nachrichten, die über diese Vereinbarung empfangen werden, behandelt werden.  
   
   * Optional können Sie die Eigenschaften in der eingehenden Nachricht überschreiben. Aktivieren Sie dazu die Option **Nachrichteneigenschaften überschreiben**.
   * Aktivieren Sie **Nachricht muss signiert sein**, wenn Sie festlegen möchten, dass alle eingehenden Nachrichten signiert sein müssen. Wenn Sie diese Option auswählen, müssen Sie das *öffentliche Zertifikat des Gastpartners* auswählen, um die Signatur der Nachrichten zu überprüfen.
   * Aktivieren Sie **Nachricht muss verschlüsselt sein**, wenn Sie festlegen möchten, dass alle eingehenden Nachrichten verschlüsselt sein müssen.  Wenn Sie diese Option auswählen, müssen Sie das *private Zertifikat des Hostpartners* auswählen, um die eingehenden Nachrichten zu entschlüsseln.
   * Sie können auch die Komprimierung von Nachrichten anfordern. Aktivieren Sie dazu **Nachricht muss komprimiert sein**.    
   * Aktivieren Sie **MDN senden**, um synchronisierte MDN für die empfangenen Nachrichten zu senden.
   * Aktivieren Sie **Signierte MDN senden**, um signierte MDN für die empfangenen Nachrichten zu senden.
   * Aktivieren Sie **Asynchrone MDN senden**, um asynchrone MDN für die empfangenen Nachrichten zu senden.     
    ![](./media/logic-apps-enterprise-integration-agreements/agreement-4.png)  

In der nachstehenden Tabelle erfahren Sie, was die Empfangseinstellungen ermöglichen.  

| Eigenschaft | Beschreibung |
| --- | --- |
| Nachrichteneigenschaften überschreiben |Wählen Sie diese Option aus, um anzugeben, dass Eigenschaften in empfangenen Nachrichten überschrieben werden können. |
| Nachricht muss signiert sein |Aktivieren Sie diese Option, um anzufordern, dass Nachrichten digital signiert werden.  Konfigurieren des öffentlichen Zertifikats des Gastpartners für die Signaturüberprüfung  |
| Nachricht muss verschlüsselt sein |Aktivieren Sie diese Option, um anzufordern, dass Nachrichten verschlüsselt werden müssen. Nicht verschlüsselte Nachrichten werden zurückgewiesen. Konfigurieren des privaten Zertifikats des Hostpartners für die Nachrichtenentschlüsselung  |
| Nachricht muss komprimiert sein |Aktivieren Sie diese Option, um anzufordern, dass Nachrichten komprimiert werden müssen. Nicht komprimierte Nachrichten werden zurückgewiesen. |
| MDN-Text |Dies ist eine Standard-MDN, die an den Absender der Nachricht gesendet wird. |
| MDN senden |Aktivieren Sie diese Option, damit MDNs gesendet werden. |
| Signierte MDN senden |Aktivieren Sie diese Option, um anzufordern, dass MDNs signiert werden müssen. |
| MIC-Algorithmus | |
| Asynchrone MDN senden |Aktivieren Sie diese Option, um anzufordern, dass Nachrichten asynchron gesendet werden. |
| URL |An diese URL werden die MDNs gesendet. |

Lassen Sie uns nun fortfahren:  

1. Wählen Sie **Sendeeinstellungen** aus, um zu konfigurieren, wie Nachrichten, die über diese Vereinbarung gesendet werden, behandelt werden.  

   * Aktivieren Sie **Nachrichtensignatur aktivieren**, um signierte Nachrichten an den Partner zu senden. Wenn Sie diese Option auswählen, müssen Sie den *MIC-Algorithmus für das private Zertifikat des Hostpartners* und das *private Zertifikat des Hostpartners* auswählen, um die Nachrichten zu signieren.
   * Aktivieren Sie **Nachrichtenverschlüsselung aktivieren**, um verschlüsselte Nachrichten an den Partner zu senden. Wenn Sie diese Option auswählen, müssen Sie den *Algorithmus für das öffentliche Zertifikat des Gastpartners* und das *öffentliche Zertifikat des Gastpartners* auswählen, um die Nachrichten zu verschlüsseln.
   * Aktivieren Sie **Nachricht muss komprimiert sein**, um die Nachricht zu komprimieren. 
   * Aktivieren Sie **HTTP-Header erweitern**, um HTTP-Header des Typs „Content-Type“ in eine einzige Zeile zu erweitern. 
   * Aktivieren Sie **MDN anfordern**, um synchronisierte MDN für die gesendeten Nachrichten zu empfangen.
   * Aktivieren Sie **Signierte MDN anfordern**, um signierte MDN für die gesendeten Nachrichten zu empfangen.
   * Aktivieren Sie **Asynchrone MDN anfordern**, um asynchrone MDN für die gesendeten Nachrichten zu empfangen. Wenn Sie diese Option auswählen, müssen Sie eine URL angeben, an die MDNs gesendet werden.  
   * Aktivieren Sie **NRR aktivieren**, um die Nichtabstreitbarkeit des Empfangs zu aktivieren.    
   ![](./media/logic-apps-enterprise-integration-agreements/agreement-5.png)  

In der nachstehenden Tabelle erfahren Sie, was die Sendeeinstellungen ermöglichen.  

| Eigenschaft | Beschreibung |
| --- | --- |
| Nachrichtensignatur aktivieren |Aktivieren Sie diese Option, um dafür zu sorgen, dass alle von der Vereinbarung gesendeten Nachrichten signiert sind. |
| MIC-Algorithmus |Wählen Sie den Algorithmus für das Signieren von Nachrichten. Konfigurieren des MIC-Algorithmus für das private Zertifikat des Hostpartners für das Signieren von Nachrichten |
| Zertifikat |Wählen Sie das Zertifikat für das Signieren von Nachrichten. Konfigurieren des privaten Zertifikats des Hostpartners für das Signieren von Nachrichten |
| Nachrichtenverschlüsselung aktivieren |Aktivieren Sie diese Option, um alle von der Vereinbarung gesendeten Nachrichten zu verschlüsseln. Konfigurieren des Algorithmus für das öffentliche Zertifikat des Gastpartners für das Verschlüsseln der Nachrichten |
| Verschlüsselungsalgorithmus |Wählen Sie den Verschlüsselungsalgorithmus für die Verschlüsselung von Nachrichten aus. Konfigurieren des öffentlichen Zertifikats des Gastpartners für das Verschlüsseln der Nachrichten |
| HTTP-Header erweitern |Aktivieren Sie diese Option, um HTTP-Header des Typs „Content-Type“ in eine einzige Zeile zu erweitern. |
| MDN anfordern |Aktivieren Sie diese Option, um eine MDN für alle von dieser Vereinbarung gesendeten Daten anzufordern. |
| Signierte MDN anfordern |Aktivieren Sie diese Option, um anzufordern, dass alle MDNs signiert werden, die an diese Vereinbarung gesendet werden. |
| Asynchrone MDN anfordern |Aktivieren Sie diese Option, um anzufordern, dass eine asynchrone MDN an diese Vereinbarung gesendet wird. |
| URL |Die URL, an die MDNs gesendet werden. |
| NRR aktivieren |Aktivieren Sie diese Option, um die Nichtabstreitbarkeit des Empfangs zu aktivieren. |

Wählen Sie auf dem Blatt „Integrationskonto“ die Kachel **Vereinbarungen** aus, auf dem die neu hinzugefügte Vereinbarung aufgeführt ist.  
![](./media/logic-apps-enterprise-integration-agreements/agreement-6.png)

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")  


<!--HONumber=Jan17_HO4-->


