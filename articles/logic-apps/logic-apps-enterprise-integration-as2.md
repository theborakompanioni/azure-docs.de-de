---
title: "AS2-Nachrichten für die B2B-Unternehmensintegration – Azure Logic Apps | Microsoft-Dokumentation"
description: "Austauschen von AS2-Nachrichten für die B2B-Unternehmensintegration mit Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: c9b7e1a9-4791-474c-855f-988bd7bf4b7f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; mandia
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 09a2373585f49a39192a841072d86e395ff311db
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="exchange-as2-messages-for-enterprise-integration-with-logic-apps"></a>Austauschen von AS2-Nachrichten für die Unternehmensintegration mit Logik-Apps

Bevor Sie AS2-Nachrichten für Azure Logic Apps austauschen können, müssen Sie eine AS2-Vereinbarung erstellen und in Ihrem Integrationskonto speichern. Hier erfahren Sie, wie Sie eine AS2-Vereinbarung erstellen.

## <a name="before-you-start"></a>Vorbereitung

Sie benötigen Folgendes:

* Ein bereits definiertes und mit Ihrem Azure-Abonnement verknüpftes [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-accounts.md)
* Mindestens zwei [Partner](logic-apps-enterprise-integration-partners.md), die bereits in Ihrem Integrationskonto definiert und mit dem AS2-Qualifizierer unter **Geschäftsidentitäten** konfiguriert sind

> [!NOTE]
> Beim Erstellen einer Vereinbarung muss der Inhalt der Vereinbarungsdatei dem Vereinbarungstyp entsprechen.    

Nachdem Sie [ein Integrationskonto erstellt](../logic-apps/logic-apps-enterprise-integration-accounts.md) und [Partner hinzugefügt](logic-apps-enterprise-integration-partners.md) haben, können Sie eine AS2-Vereinbarung erstellen. Gehen Sie dazu wie folgt vor:

## <a name="create-an-as2-agreement"></a>Erstellen einer AS2-Vereinbarung

1.    Melden Sie sich beim [Azure-Portal](http://portal.azure.com "Azure-Portal") an.  

2.    Wählen Sie im linken Menü die Option **Weitere Dienste** aus. Geben Sie im Suchfeld den Begriff **integration** als Filter ein. Wählen Sie in der Ergebnisliste die Option **Integrationskonten** aus.

    > [!TIP]
    > Sollte **Weitere Dienste** nicht angezeigt werden, müssen Sie das Menü möglicherweise erst erweitern. Wählen Sie im oberen Bereich des reduzierten Menüs die Option **Menü anzeigen** aus.

    ![Weitere Dienste, nach „Integration“ filtern, „Integrationskonten“ auswählen](./media/logic-apps-enterprise-integration-agreements/overview-1.png)

3. Wählen Sie auf dem geöffneten Blatt **Integrationskonten** das Integrationskonto aus, in dem Sie die Vereinbarung erstellen möchten.
Sollten keine Integrationskonten angezeigt werden, [erstellen Sie zunächst ein Konto](../logic-apps/logic-apps-enterprise-integration-accounts.md "Alles über Integrationskonten").  

    ![Integrationskonto für die Erstellung der Vereinbarung auswählen](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Wählen Sie die Kachel **Vereinbarungen** aus. Sollte die Kachel „Vereinbarungen“ nicht angezeigt werden, fügen Sie sie hinzu.

    ![Kachel „Vereinbarungen“ auswählen](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

5. Wählen Sie auf dem geöffneten Blatt „Vereinbarungen“ die Option **Hinzufügen** aus.

    ![„Hinzufügen“ auswählen](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)

6. Geben Sie unter **Hinzufügen** im Feld **Name** einen Namen für Ihre Vereinbarung ein. Wählen Sie unter **Vertragstyp** die Option **AS2** aus. Wählen Sie den **Hostpartner**, die **Hostidentität**, den **Gastpartner** und die **Gastidentität** für Ihre Vereinbarung aus.

    ![Details zur Vereinbarung angeben](./media/logic-apps-enterprise-integration-agreements/agreement-3.png)  

    | Eigenschaft | Beschreibung |
    | --- | --- |
    | Name |Name der Vereinbarung |
    | Vereinbarungstyp | Muss „AS2“ sein |
    | Hostpartner |Eine Vereinbarung benötigt einen Host- und einen Gastpartner. Der Hostpartner stellt die Organisation dar, die die Vereinbarung konfiguriert. |
    | Hostidentität |Ein Bezeichner für den Hostpartner. |
    | Gastpartner |Eine Vereinbarung benötigt einen Host- und einen Gastpartner. Der Gastpartner stellt die Organisation dar, die Geschäfte mit dem Hostpartner tätigt. |
    | Gastidentität |Ein Bezeichner für den Gastpartner. |
    | Empfangseinstellungen |Diese Eigenschaften gelten für alle Nachrichten, die von einer Vereinbarung empfangen werden. |
    | Sendeeinstellungen |Diese Eigenschaften gelten für alle Nachrichten, die von einer Vereinbarung gesendet werden. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Konfigurieren der Behandlung empfangener Nachrichten durch die Vereinbarung

Nachdem Sie die Vereinbarungseigenschaften festgelegt haben, können Sie konfigurieren, wie die Vereinbarung eingehende Nachrichten identifizieren und behandeln soll, die im Rahmen dieser Vereinbarung von Ihrem Partner gesendet werden.

1.    Wählen Sie unter **Hinzufügen** die Option **Empfangseinstellungen** aus.
Konfigurieren Sie die Eigenschaften auf der Grundlage Ihrer Vereinbarung mit dem Partner, der Nachrichten mit Ihnen austauscht. Die Eigenschaften werden in der Tabelle in diesem Abschnitt beschrieben.

    ![Empfangseinstellungen konfigurieren](./media/logic-apps-enterprise-integration-agreements/agreement-4.png)

2. Optional können Sie die Eigenschaften eingehender Nachrichten überschreiben, indem Sie das Kontrollkästchen **Nachrichteneigenschaften überschreiben** aktivieren.

3. Wenn Sie festlegen möchten, dass alle eingehenden Nachrichten signiert sein müssen, aktivieren Sie das Kontrollkästchen **Nachricht muss signiert sein**. Wählen Sie zur Validierung der Nachrichtensignatur in der Liste **Zertifikat** ein vorhandenes [öffentliches Gastpartnerzertifikat](../logic-apps/logic-apps-enterprise-integration-certificates.md) aus. Oder: Erstellen Sie das Zertifikat, falls Sie noch keins besitzen.

4.    Wenn Sie festlegen möchten, dass alle eingehenden Nachrichten verschlüsselt sein müssen, aktivieren Sie das Kontrollkästchen **Nachricht muss verschlüsselt sein**. Wählen Sie zur Entschlüsselung eingehender Nachrichten in der Liste **Zertifikat** ein vorhandenes [privates Hostpartnerzertifikat](../logic-apps/logic-apps-enterprise-integration-certificates.md) aus. Oder: Erstellen Sie das Zertifikat, falls Sie noch keins besitzen.

5. Wenn Sie festlegen möchten, dass Nachrichten komprimiert sein müssen, aktivieren Sie das Kontrollkästchen **Nachricht muss komprimiert sein**.

6. Aktivieren Sie das Kontrollkästchen **MDN senden**, wenn für empfangene Nachrichten eine synchrone Benachrichtigung über den Nachrichtenstatus (Message Disposition Notification, MDN) gesendet werden soll.

7. Aktivieren Sie das Kontrollkästchen **Signierte MDN senden**, wenn für empfangene Nachrichten signierte MDNs gesendet werden sollen.

8. Aktivieren Sie das Kontrollkästchen **Asynchrone MDN senden**, wenn für empfangene Nachrichten asynchrone MDNs gesendet werden sollen.

9. Klicken Sie abschließend auf **OK**, um die Einstellungen zu speichern.

Ihre Vereinbarung kann nun eingehende Nachrichten verarbeiten, die den ausgewählten Einstellungen entsprechen.

| Eigenschaft | Beschreibung |
| --- | --- |
| Nachrichteneigenschaften überschreiben |Gibt an, dass Eigenschaften in empfangenen Nachrichten überschrieben werden können. |
| Nachricht muss signiert sein |Erfordert, dass Nachrichten digital signiert werden. Konfigurieren Sie das öffentliche Zertifikat des Gastpartners für die Signaturüberprüfung.  |
| Nachricht muss verschlüsselt sein |Erfordert, dass Nachrichten verschlüsselt werden. Nicht verschlüsselte Nachrichten werden abgelehnt. Konfigurieren Sie das private Zertifikat des Hostpartners für das Entschlüsseln von Nachrichten.  |
| Nachricht muss komprimiert sein |Erfordert, dass Nachrichten komprimiert werden. Nicht komprimierte Nachrichten werden abgelehnt. |
| MDN-Text |Die standardmäßige Benachrichtigung über den Nachrichtenstatus (MDN), die an den Absender der Nachricht gesendet werden soll. |
| MDN senden |Erfordert, dass MDNs gesendet werden. |
| Signierte MDN senden |Erfordert, dass MDNs signiert werden. |
| MIC-Algorithmus |Wählen Sie den Algorithmus zum Signieren der Nachrichten aus. |
| Asynchrone MDN senden | Erfordert, dass Nachrichten asynchron gesendet werden. |
| URL | Geben Sie die URL an, an die die MDNs gesendet werden sollen. |

## <a name="configure-how-your-agreement-sends-messages"></a>Konfigurieren des Nachrichtenversands Ihrer Vereinbarung

Sie können konfigurieren, wie Ihre Vereinbarung ausgehende Nachrichten identifizieren und behandeln soll, die Sie im Rahmen dieser Vereinbarung an Ihre Partner senden.

1.    Wählen Sie unter **Hinzufügen** die Option **Sendeeinstellungen** aus.
Konfigurieren Sie die Eigenschaften auf der Grundlage Ihrer Vereinbarung mit dem Partner, der Nachrichten mit Ihnen austauscht. Die Eigenschaften werden in der Tabelle in diesem Abschnitt beschrieben.

    ![Festlegen der Eigenschaften für „Sendeeinstellungen“](./media/logic-apps-enterprise-integration-agreements/agreement-5.png)

2. Wenn Sie signierte Nachrichten an den Partner senden möchten, aktivieren Sie das Kontrollkästchen **Nachrichtensignatur aktivieren**. Wählen Sie zum Signieren der Nachrichten in der Liste **MIC-Algorithmus** den *MIC-Algorithmus für das private Hostpartnerzertifikat* aus. Wählen Sie außerdem in der Liste **Zertifikat** ein vorhandenes [privates Hostpartnerzertifikat](../logic-apps/logic-apps-enterprise-integration-certificates.md) aus.

3. Wenn Sie verschlüsselte Nachrichten an den Partner senden möchten, aktivieren Sie das Kontrollkästchen **Nachrichtenverschlüsselung aktivieren**. Wählen Sie zum Verschlüsseln der Nachrichten in der Liste **Verschlüsselungsalgorithmus** den *Algorithmus für das öffentliche Gastpartnerzertifikat* aus.
Wählen Sie außerdem in der Liste **Zertifikat** ein vorhandenes [öffentliches Gastpartnerzertifikat](../logic-apps/logic-apps-enterprise-integration-certificates.md) aus.

4. Wenn die Nachricht komprimiert werden soll, aktivieren Sie das Kontrollkästchen **Nachrichtenkomprimierung aktivieren**.

5. Wenn der HTTP-Header „Content-Type“ in eine einzelne Zeile umgewandelt werden soll, aktivieren Sie das Kontrollkästchen **HTTP-Header erweitern**.

6. Wenn Sie für die gesendeten Nachrichten synchrone MDNs erhalten möchten, aktivieren Sie das Kontrollkästchen **MDN anfordern**.

7. Wenn Sie für die gesendeten Nachrichten signierte MDNs erhalten möchten, aktivieren Sie das Kontrollkästchen **Signierte MDN anfordern**.

8. Wenn Sie für die gesendeten Nachrichten asynchrone MDNs erhalten möchten, aktivieren Sie das Kontrollkästchen **Asynchrone MDN anfordern**. Geben Sie bei Verwendung dieser Option die URL ein, an die die MDNs gesendet werden sollen.

9. Falls Sie die Nichtabstreitbarkeit des Empfangs aktivieren möchten, aktivieren Sie das Kontrollkästchen **NRR aktivieren**.

10. Klicken Sie abschließend auf **OK**, um die Einstellungen zu speichern.

Ihre Vereinbarung kann nun ausgehende Nachrichten verarbeiten, die den ausgewählten Einstellungen entsprechen.

| Eigenschaft | Beschreibung |
| --- | --- |
| Nachrichtensignatur aktivieren |Erfordert, dass alle von der Vereinbarung gesendeten Nachrichten signiert sind. |
| MIC-Algorithmus |Der Algorithmus zum Signieren der Nachrichten. Konfiguriert den MIC-Algorithmus für das private Zertifikat des Hostpartners für das Signieren der Nachrichten. |
| Zertifikat |Wählen Sie das Zertifikat zum Signieren der Nachrichten aus. Konfiguriert Sie das private Zertifikat des Hostpartners für das Signieren der Nachrichten. |
| Nachrichtenverschlüsselung aktivieren |Erfordert, dass alle von dieser Vereinbarung gesendeten Nachrichten verschlüsselt sind. Konfiguriert den Algorithmus für das öffentliche Zertifikat des Gastpartners für das Verschlüsseln der Nachrichten. |
| Verschlüsselungsalgorithmus |Der für die Verschlüsselung von Nachrichten zu verwendende Verschlüsselungsalgorithmus. Konfiguriert das öffentliche Zertifikat des Gastpartners für das Verschlüsseln der Nachrichten. |
| Zertifikat |Das zum Verschlüsseln von Nachrichten zu verwendende Zertifikat. Konfiguriert das private Zertifikat des Gastpartners für das Verschlüsseln der Nachrichten. |
| Nachrichtenkomprimierung aktivieren |Erfordert, dass alle von dieser Vereinbarung gesendeten Nachrichten komprimiert sind. |
| HTTP-Header erweitern |Erweitert HTTP-Header des Typs „Content-Type“ in eine einzige Zeile. |
| MDN anfordern |Erfordert eine MDN für alle von dieser Vereinbarung gesendeten Nachrichten. |
| Signierte MDN anfordern |Erfordert, dass alle an diese Vereinbarung gesendeten MDNs signiert sind. |
| Asynchrone MDN anfordern |Erfordert, dass asynchrone MDNs an diese Vereinbarung gesendet werden. |
| URL |Geben Sie die URL an, an die die MDNs gesendet werden sollen. |
| NRR aktivieren |Erfordert die Nichtabstreitbarkeit des Empfangs (NRR), ein Kommunikationsattribut, das nachweist, dass die Daten ordnungsgemäß empfangen wurden. |

## <a name="find-your-created-agreement"></a>Suchen der erstellten Vereinbarung

1.    Wählen Sie nach dem Festlegen der Vereinbarungseigenschaften auf dem Blatt **Hinzufügen** die Option **OK** aus, um die Erstellung Ihrer Vereinbarung abzuschließen und zum Blatt Ihres Integrationskontos zurückzukehren.

    Die neu hinzugefügte Vereinbarung ist nun in der Liste **Vereinbarungen** enthalten.

2.    Sie können Ihre Vereinbarungen auch in der Integrationskontoübersicht anzeigen. Wählen Sie auf dem Blatt Ihres Integrationskontos die Option **Übersicht** und anschließend die Kachel **Vereinbarungen** aus. 

    ![Kachel „Vereinbarungen“ auswählen, um alle Vereinbarungen anzuzeigen](./media/logic-apps-enterprise-integration-agreements/agreement-6.png)

## <a name="view-the-swagger"></a>Anzeigen von Swagger
Weitere Informationen finden Sie unter [Details zu Swagger](/connectors/as2/). 

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")  

