---
title: Erstellen einer AS2-Vereinbarung in Azure Logic Apps | Microsoft-Dokumentation
description: "Erstellen einer AS2-Vereinbarung für das Enterprise Integration Pack | Azure Logic Apps"
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
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 6a947ea997bbcfe1b6b28c7cbb49911836750e6a
ms.openlocfilehash: a490b89c5420aecdfb3f79289979faab9a4630e9


---
# <a name="enterprise-integration-with-as2"></a>Unternehmensintegration mit AS2
Zum Verwenden der Features für die Unternehmensintegration in Logik-Apps müssen Sie zuerst Vereinbarungen erstellen.

## <a name="prerequisites"></a>Voraussetzungen
* In Ihrem Azure-Abonnement muss ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-accounts.md) definiert sein.  
* Mindestens zwei [Partner](logic-apps-enterprise-integration-partners.md) müssen bereits in Ihrem Integrationskonto definiert sein.  

> [!NOTE]
> Beim Erstellen einer Vereinbarung muss der Inhalt der Vereinbarungsdatei dem Vereinbarungstyp entsprechen.    

Nachdem Sie ein Integrationskonto erstellt und Partner hinzugefügt haben, können Sie mit den Verfahren in den folgenden Abschnitten eine Vereinbarung erstellen:  

## <a name="create-an-agreement"></a>Erstellen einer Vereinbarung

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com "Azure-Portal") an.  
2. Wählen Sie **Weitere Dienste**, geben Sie den Begriff **Integration** in das Filtersuchfeld ein, und wählen Sie in der Ergebnisliste die Option **Integrationskonten**.

 ![Auswählen von „Integrationskonten“ in der Suchergebnisliste](./media/logic-apps-enterprise-integration-agreements/overview-1.png)    
3. Wählen Sie das Integrationskonto aus, dem Sie das Zertifikat hinzufügen möchten.

 ![Auswählen des Integrationskontos](./media/logic-apps-enterprise-integration-overview/overview-3.png)  
4. Wählen Sie die Kachel **Vereinbarungen** aus. Wenn die Kachel nicht angezeigt wird, fügen Sie sie hinzu.

 ![Wählen Sie die Kachel „Vereinbarungen“ aus.](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)   
5. Wählen Sie auf dem Blatt **Vereinbarungen** die Option **Hinzufügen** aus.

 ![Wählen Sie "Hinzufügen" aus.](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)  
6. Geben Sie einen Namen für die Vereinbarung ein. Wählen Sie dann in der Liste **Vereinbarungstyp** den Typ **AS2** aus, und geben Sie die jeweiligen Informationen in die Listen **Hostpartner**, **Hostidentität**, **Gastpartner** und **Gastidentität** ein.

 ![Eingeben eines Namens für die Vereinbarung](./media/logic-apps-enterprise-integration-agreements/agreement-3.png)  

 In der folgenden Tabelle werden die Eigenschaften im Dialogfeld **Hinzufügen** beschrieben:

    | Eigenschaft | Beschreibung |
    | --- | --- |
    | Hostpartner | Eine Vereinbarung erfordert einen Host- und einen Gastpartner. Der Hostpartner stellt die Organisation dar, die die Vereinbarung konfiguriert. |
    | Hostidentität | Ein Bezeichner für den Hostpartner. |
    | Gastpartner | Eine Vereinbarung erfordert einen Host- und einen Gastpartner. Der Gastpartner stellt die Organisation dar, die Geschäfte mit dem Hostpartner tätigt. |
    | Gastidentität | Ein Bezeichner für den Gastpartner. |
    | Empfangseinstellungen | Die Eigenschaften, die für alle Nachrichten gelten, die von der Vereinbarung empfangen werden. |
    | Sendeeinstellungen | Die Eigenschaften, die für alle Nachrichten gelten, die von der Vereinbarung gesendet werden. |

7. Führen Sie die folgenden Schritte aus, um zu konfigurieren, wie Nachrichten behandelt werden, die über diese Vereinbarung empfangen werden:

 a. Wählen Sie **Empfangseinstellungen**.

 b. Optional können Sie die Eigenschaften eingehender Nachrichten überschreiben, indem Sie das Kontrollkästchen **Nachrichteneigenschaften überschreiben** aktivieren.

 c. Wenn Sie festlegen möchten, dass alle eingehenden Nachrichten signiert sein müssen, aktivieren Sie das Kontrollkästchen **Nachricht muss signiert sein**. Wenn Sie diese Option auswählen, überprüfen Sie die Signatur in den Nachrichten, indem Sie in der Liste **Zertifikat** das **öffentliche Zertifikat des Gastpartners** auswählen.

 d. Wenn Sie festlegen möchten, dass alle eingehenden Nachrichten verschlüsselt sein müssen, aktivieren Sie das Kontrollkästchen **Nachricht muss verschlüsselt sein**. Wenn Sie diese Option auswählen, entschlüsseln Sie die eingehenden Nachrichten, indem Sie in der Liste **Zertifikat** das **private Zertifikat des Hostpartners** auswählen.

 e. Wenn Sie festlegen möchten, dass Nachrichten komprimiert sein müssen, aktivieren Sie das Kontrollkästchen **Nachricht muss komprimiert sein**.    

 f. Aktivieren Sie das Kontrollkästchen **MDN senden**, um eine synchrone Benachrichtigung über den Nachrichtenstatus (Message Disposition Notification, MDN) für empfangene Nachrichten zu senden.

 g. Aktivieren Sie das Kontrollkästchen **Signierte MDN senden**, um signierte MDNs für empfangene Nachrichten zu senden.

 h. Aktivieren Sie das Kontrollkästchen **Asynchrone MDN senden**, um asynchrone MDNs für empfangene Nachrichten zu senden.

 ![Festlegen der Eigenschaften für „Empfangseinstellungen“](./media/logic-apps-enterprise-integration-agreements/agreement-4.png)  

 In der folgenden Tabelle sind die Eigenschaften für **Empfangseinstellungen** beschrieben:  

 | Eigenschaft | Beschreibung |
 | --- | --- |
 | Nachrichteneigenschaften überschreiben | Gibt an, dass Eigenschaften in empfangenen Nachrichten überschrieben werden können. |
 | Nachricht muss signiert sein | Erfordert, dass Nachrichten digital signiert werden. Konfigurieren Sie das öffentliche Zertifikat des Gastpartners für die Signaturüberprüfung.  |
 | Nachricht muss verschlüsselt sein | Erfordert, dass Nachrichten verschlüsselt werden. Nicht verschlüsselte Nachrichten werden zurückgewiesen. Konfigurieren Sie das private Zertifikat des Hostpartners für das Entschlüsseln von Nachrichten.  |
 | Nachricht muss komprimiert sein | Erfordert, dass Nachrichten komprimiert werden. Nicht komprimierte Nachrichten werden zurückgewiesen. |
 | MDN-Text | Die standardmäßige Benachrichtigung über den Nachrichtenstatus (MDN), die an den Absender der Nachricht gesendet werden soll. |
 | MDN senden | Erfordert, dass MDNs gesendet werden. |
 | Signierte MDN senden | Erfordert, dass MDNs signiert werden. |
 | MIC-Algorithmus | |
 | Asynchrone MDN senden | Erfordert, dass Nachrichten asynchron gesendet werden. |
 | URL | Die URL, an die die MDNs gesendet werden. |

8. Führen Sie die folgenden Schritte aus, um zu konfigurieren, wie Nachrichten behandelt werden, die über diese Vereinbarung gesendet werden:

 a. Wählen Sie **Sendeeinstellungen**.  

 b. Aktivieren Sie das Kontrollkästchen **Nachrichtensignatur aktivieren**, um signierte Nachrichten an den Partner zu senden. Wenn Sie diese Option auswählen, signieren Sie die Nachrichten, indem Sie in der Liste **MIC-Algorithmus** den **MIC-Algorithmus für das private Zertifikat des Hostpartners** und in der Liste **Zertifikat** das **private Zertifikat des Hostpartners** auswählen.

 c. Aktivieren Sie das Kontrollkästchen **Nachrichtenverschlüsselung aktivieren**, um verschlüsselte Nachrichten an den Partner zu senden. Wenn Sie diese Option auswählen, verschlüsseln Sie die Nachrichten, indem Sie in der Liste **Verschlüsselungsalgorithmus** den **Algorithmus für das öffentliche Zertifikat des Gastpartners** und in der Liste **Zertifikat** das **öffentliche Zertifikat des Gastpartners** auswählen.

 d. Aktivieren Sie das Kontrollkästchen **Nachrichtenkomprimierung aktivieren**, um die Nachricht zu komprimieren.

 e. Aktivieren Sie das Kontrollkästchen **HTTP-Header erweitern**, um HTTP-Header des Typs „Content-Type“ in eine einzige Zeile zu erweitern.

 f. Aktivieren Sie das Kontrollkästchen **MDN anfordern**, um synchrone MDNs für die gesendeten Nachrichten zu erhalten.

 g. Aktivieren Sie das Kontrollkästchen **Signierte MDN anfordern**, um signierte MDNs für die gesendeten Nachrichten zu erhalten.

 h. Aktivieren Sie das Kontrollkästchen **Asynchrone MDN anfordern**, um asynchrone MDNs für die gesendeten Nachrichten zu erhalten. Wenn Sie diese Option auswählen, geben Sie die URL ein, an die die MDNs gesendet werden sollen.  

 i. Aktivieren Sie das Kontrollkästchen **NRR aktivieren**, um die Nichtabstreitbarkeit des Empfangs zu erfordern.

 j. Klicken Sie auf **OK**.

 ![Festlegen der Eigenschaften für „Sendeeinstellungen“](./media/logic-apps-enterprise-integration-agreements/agreement-5.png)  

 In der folgenden Tabelle sind die Eigenschaften für **Sendeeinstellungen** beschrieben:  

 | Eigenschaft | Beschreibung |
 | --- | --- |
 | Nachrichtensignatur aktivieren | Erfordert, dass alle von der Vereinbarung gesendeten Nachrichten signiert sind. |
 | MIC-Algorithmus | Der zum Signieren von Nachrichten zu verwendende Algorithmus. Konfiguriert den MIC-Algorithmus für das private Zertifikat des Hostpartners für das Signieren der Nachrichten. |
 | Zertifikat | Das zum Signieren von Nachrichten zu verwendende Zertifikat. Konfiguriert Sie das private Zertifikat des Hostpartners für das Signieren der Nachrichten. |
 | Nachrichtenverschlüsselung aktivieren | Erfordert, dass alle von dieser Vereinbarung gesendeten Nachrichten verschlüsselt sind. Konfiguriert den Algorithmus für das öffentliche Zertifikat des Gastpartners für das Verschlüsseln der Nachrichten. |
 | Verschlüsselungsalgorithmus | Der für die Verschlüsselung von Nachrichten zu verwendende Verschlüsselungsalgorithmus. Konfiguriert das öffentliche Zertifikat des Gastpartners für das Verschlüsseln der Nachrichten. |
 | Zertifikat | Das zum Verschlüsseln von Nachrichten zu verwendende Zertifikat. Konfiguriert das private Zertifikat des Gastpartners für das Verschlüsseln der Nachrichten. |
 | Nachrichtenkomprimierung aktivieren | Erfordert, dass alle von dieser Vereinbarung gesendeten Nachrichten komprimiert sind. |
 | HTTP-Header erweitern | Erweitert HTTP-Header des Typs „Content-Type“ in eine einzige Zeile. |
 | MDN anfordern | Erfordert eine MDN für alle von dieser Vereinbarung gesendeten Nachrichten. |
 | Signierte MDN anfordern | Erfordert, dass alle an diese Vereinbarung gesendeten MDNs signiert sind. |
 | Asynchrone MDN anfordern | Erfordert, dass asynchrone MDNs an diese Vereinbarung gesendet werden. |
 | URL | Die URL, an die die MDNs gesendet werden. |
 | NRR aktivieren | Erfordert die Nichtabstreitbarkeit des Empfangs (NRR), ein Kommunikationsattribut, das nachweist, dass die Daten ordnungsgemäß empfangen wurden. |

## <a name="view-the-agreements-list"></a>Anzeigen der Liste mit Vereinbarungen
Wählen Sie zum Anzeigen der neu hinzugefügten Vereinbarung auf dem Blatt **Integrationskonto** die Kachel **Vereinbarungen** aus.

![Anzeigen der Liste „Vereinbarungen“](./media/logic-apps-enterprise-integration-agreements/agreement-6.png)

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")  



<!--HONumber=Feb17_HO1-->


