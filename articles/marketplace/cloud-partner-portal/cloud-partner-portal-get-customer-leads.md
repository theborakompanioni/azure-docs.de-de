---
title: "Generieren von Kundenleads für Azure | Microsoft-Dokumentation"
description: "In diesem Artikel wird detailliert erläutert, wie Sie Microsoft-Leads generieren und Ihr CRM-System zur Integration in Ihre Vertriebspipelines mit dem Cloudpartnerportal verbinden."
services: cloud-partner-portal
documentationcenter: 
author: Bigbrd
manager: hamidm
ms.service: cloud-partner-portal
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: brdi
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 6207695ede6c01f9423ddde7467d2a9b1ed35bd3
ms.lasthandoff: 04/18/2017


---

# <a name="get-customer-leads"></a>Generieren von Kundenleads 
Kunden spielen für erfolgreiche Unternehmen eine entscheidende Rolle. Angesichts des Wandels beim Kauf von Produkten sind Marketingfachleute darauf angewiesen, einen direkten Kontakt mit Kunden herzustellen und eine Beziehung aufzubauen. Aus diesem Grund stellt die Generierung hochwertiger Leads ein unverzichtbares Werkzeug für Ihren Vertriebszyklus dar.
Mit dem Cloudpartnerportal haben wir Ihnen Möglichkeiten verschafft, Kontaktinformationen von Kunden **unmittelbar** programmgesteuert zu erhalten, nachdem ein Kunde Interesse bekundet oder Ihr Produkt bereitgestellt hat. Dieses Dokument enthält alle Informationen, die Sie zu diesen generierten Leads, zu ihrer Einbindung in Ihr CRM-System sowie zu ihrer Integration in Vertriebspipelines kennen müssen.

## <a name="what-are-these-leads"></a>Worum handelt es sich bei diesen Leads?
Diese Leads sind Kunden, die Ihre Produkte über Marketplace bereitstellen. Ganz gleich, ob Ihr Produkt auf [Azure Marketplace](https://azuremarketplace.microsoft.com/) oder in [AppSource](https://appsource.com/) aufgeführt wird, erhalten Sie Leads von Kunden, die an Ihrem Produkt interessiert sind.
### <a name="azure-marketplace"></a>Azure Marketplace
1.    Der Kunde erhält eine Testversion Ihres Angebots. Diese auch „Test Drives“ genannten Versionen optimieren Ihre Chancen, Ihrem Unternehmen umgehend und ohne Zugangsbeschränkungen potenzielle Kunden zu vermitteln. Alle Test Drives generieren einen Lead für Kunden, die Interesse hatten, Ihr Produkt auszuprobieren, um mehr darüber zu erfahren.  Weitere Informationen zu Test Drives finden Sie unter [Azure Marketplace Test Drive](https://azuremarketplace.azureedge.net/documents/azure-marketplace-test-drive-program.pdf).

    ![Test Drive-Angebot](./media/cloud-partner-portal-get-customer-leads/test-drive-offer.png)
 

2.    Der Kunde erklärt sich durch Klicken auf die Schaltfläche „Jetzt anfordern“ mit der Freigabe seiner Informationen einverstanden. Bei diesem Lead handelt es sich um einen Lead mit erstem Interesse, mit dem wir Kunden vermitteln, die Interesse am Erhalt Ihres Produkts bekundet haben und sich an oberster Stelle des Kauftrichters befinden.
 
    ![Schaltfläche „Jetzt anfordern“](./media/cloud-partner-portal-get-customer-leads/get-it-now-button.png)

3.    Der Kunde klickt beim offiziellen Erwerb Ihres Produkts im [Azure-Portal](https://portal.azure.com/) auf „Kaufen“. Bei diesem Lead handelt es sich um einen aktiven Lead, mit dem wir Kunden vermitteln, die über ein Azure-Abonnement verfügen und mit der Bereitstellung Ihres eigentlichen Produkts begonnen haben.

    ![Textschaltfläche „Kaufen“](./media/cloud-partner-portal-get-customer-leads/purchase-button.png) 

### <a name="appsource"></a>AppSource
1.    Der Kunde erhält eine Testversion Ihres Angebots. Diese auch „Test Drives“ genannten Versionen optimieren Ihre Chancen, Ihrem Unternehmen umgehend und ohne Zugangsbeschränkungen potenzielle Kunden zu vermitteln. Alle Test Drives generieren einen Lead für Kunden, die Interesse hatten, Ihr Produkt auszuprobieren, um mehr darüber zu erfahren.  Weitere Informationen zu Test Drives finden Sie unter [AppSource Test Drive](https://appsource.microsoft.com/blogs/want-to-try-an-app-take-a-test-drive).

    ![Test Drive-Angebot](./media/cloud-partner-portal-get-customer-leads/test-drive-offer-2.png)

2.    Der Kunde erklärt sich durch Klicken auf die Schaltfläche „Jetzt anfordern“ mit der Freigabe seiner Informationen einverstanden. Bei diesem Lead handelt es sich um einen Lead mit erstem Interesse, mit dem wir Kunden vermitteln, die Interesse am Erhalt Ihres Produkts bekundet haben und sich an oberster Stelle des Kauftrichters befinden.

    ![Schaltfläche „Jetzt anfordern“](./media/cloud-partner-portal-get-customer-leads/get-it-now-button-2.png)

3.    Der Kunde klickt in Ihrem Angebot auf die Schaltfläche „Kontakt mit mir aufnehmen“. Bei diesem Lead handelt es sich um einen aktiven Lead, mit dem wir Kunden vermitteln, die aktiv Follow-up-Materialien zu Ihrem Produkt gefordert haben.

    ![Kontakt mit mir aufnehmen](./media/cloud-partner-portal-get-customer-leads/contact-me-image.png)

## <a name="lead-data"></a>Leaddaten
Zu jedem Lead, den Sie erhalten, werden Daten in diese spezifischen Felder eingegeben. Da Sie in verschiedenen Phasen des Kaufzyklus der einzelnen Kunden Leads erhalten, stellen Deduplizierung und Personalisierung der Follow-up-Materialien die beste Möglichkeit im Umgang mit den Leads dar. Auf diese Weise erhält jeder Kunde eine auf ihn zugeschnittene Botschaft, die es Ihnen ermöglicht, stets eine individuelle Beziehung aufzubauen.

|Feldname|Beispiele|
|----------|--------|
|LeadSource| Das Format lautet wie folgt: **Quelle**-**Aktion**&#124;**Angebot** <br/><br/>**Quellen**:<br/>„AzureMarketplace“,<br/>„AzurePortal“,<br/>„TestDrive“, <br/>„SPZA“ (Akronym für AppSource)<br/><br/>**Aktionen**:<br/>„INS“ – Steht für „Installation“. Diese Aktion wird immer dann auf Azure Marketplace oder in AppSource angezeigt, wenn ein Kunde auf die Schaltfläche zum Erwerb Ihres Produkts klickt.<br/><br/>„PLT“ – Steht für „Partner Led Trial“ (Vom Partner geleiteter Test). Diese Aktion wird immer dann in AppSource angezeigt, wenn ein Kunde auf die Schaltfläche „Kontakt mit mir aufnehmen“ klickt.<br/><br/>„DNC“ – Steht für „Do Not Contact“ (Bitte nicht kontaktieren). Diese Aktion wird immer dann in AppSource angezeigt, wenn ein Partner, der auf Ihrer App-Seite querverwiesen wurde, um eine Kontaktaufnahme gebeten wird. Wir übermitteln die Vorwarnung, dass dieser Kunde zu Ihrer App querverwiesen wurde, jedoch nicht kontaktiert werden muss.<br/><br/>„Erstellen“ – Diese Aktion ist nur im Azure-Portal vorhanden und wird immer dann angezeigt, wenn ein Kunde mit seinem Konto Ihr Angebot erwirbt.<br/><br/>„StartTestDrive“ – Diese Aktion gilt nur für Test Drives und wird immer dann angezeigt, wenn ein Kunde seinen Test Drive startet.<br/><br/>**Angebote**:<br/>    „checkpoint.check-point-r77-10sg-byol“,<br/>„bitnami.openedxcypress“,<br/>„docusign.3701c77e-1cfa-4c56-91e6-3ed0b622145a“<br/> <br/> Dies sind die eindeutigen Bezeichner für Sie und Ihr spezifisches Angebot. |
|CustomerInfo|    {<br/>„FirstName“:„John“,<br/>„LastName“:„Smith“,<br/>„Email“:„jsmith@microsoft.com“,<br/>„Phone“:„1234567890“,<br/>„Country“:„US“,<br/>„Company“:„Microsoft“,<br/>„Title“:„CTO“<br/>}<br/><br/>Hinweis: Es sind nicht alle Daten für jeden Lead verfügbar.|


Wir arbeiten aktiv an der Verbesserung von Leads. Wenn daher ein von Ihnen gewünschtes Datenfeld hier nicht angezeigt wird, [senden Sie uns Ihr Feedback](mailto:AzureMarketOnboard@microsoft.com).

## <a name="how-to-connect-your-crm-system-with-the-cloud-partner-portal"></a>Vorgehensweise zum Verbinden Ihres CRM-Systems mit dem Cloudpartnerportal 
Um mit dem Erhalten von Leads zu beginnen, haben wir unseren Connector zur Leadverwaltung in das Cloudpartnerportal integriert, sodass Sie Ihre CRM-Daten mühelos einbinden können. Die Verbindung stellen wir für Sie her. Nun können Sie ohne großen Entwicklungsaufwand für die Integration in ein externes System mühelos die vom Marketplace generierten Leads nutzen.
 
![Connector zur Leadverwaltung](./media/cloud-partner-portal-get-customer-leads/lead-management-connector.png) Leads können in eine Reihe von CRM-Systemen oder direkt in eine Azure Storage-Tabelle geschrieben werden, in der Sie die Leads ganz nach Belieben verwalten können. Im Folgenden werden Schritt-für-Schritt-Anleitungen zum Herstellen einer Verbindung zu den einzelnen möglichen Leadzielen vorgestellt: <Verwenden Sie hierzu aktualisierte Markdowndateien als Verknüpfungen von AppSource/Test Drive. Gehen Sie die Leadverbindungen erneut durch. Nun sind nicht nur JSON-Felder auf separaten Seiten erforderlich.>

*    Dynamics CRM Online – Klicken Sie [hier](./cloud-partner-portal-lead-management-instructions-dynamics.md), um die Anweisungen zur Konfiguration von Dynamics CRM Online für den Erhalt von Leads abzurufen. 
*    Marketo – Klicken Sie [hier](./cloud-partner-portal-lead-management-instructions-marketo.md), um Anweisungen zum Einrichten der Marketo-Leadkonfiguration zum Abrufen von Leads zu erhalten.
*    Salesforce – Klicken Sie [hier](./cloud-partner-portal-lead-management-instructions-salesforce.md), um Anweisungen zum Einrichten Ihrer Salesforce-Instanz zum Abrufen von Leads zu erhalten.
*    Azure-Tabelle – Klicken Sie [hier](./cloud-partner-portal-lead-management-instructions-azure-table.md), um Anweisungen zum Einrichten Ihres Azure-Speicherkontos zum Abrufen von Leads in einer Azure-Tabelle zu erhalten. 

Nachdem Sie Ihr Leadziel ordnungsgemäß konfiguriert und in Ihrem Angebot auf „Veröffentlichen“ geklickt haben, überprüfen wir die Verbindung und senden Ihnen einen Testlead zu. Wenn Sie das Angebot vor der Liveschaltung anzeigen, können Sie Ihre Leadverbindung ebenfalls testen, indem Sie selbst versuchen, das Angebot in der Vorschauumgebung zu erwerben.  Es muss unbedingt sichergestellt werden, dass Ihre Leadeinstellungen auf dem neuesten Stand sind, damit Sie keine Leads verlieren. Aktualisieren Sie daher immer diese Verbindungen, wenn sich bei Ihnen etwas geändert hat.

## <a name="what-next"></a>Nächste Schritte 
Nach der technischen Einrichtung sollten Sie diese Leads in Ihre aktuelle Vertriebs- und Marketingstrategie sowie in Ihre Betriebsprozesse integrieren. Wir sind sehr daran interessiert, einen besseren Überblick über Ihren allgemeinen Vertriebsprozess zu erhalten, und möchten bei der Bereitstellung hochwertiger Leads und ausreichender Daten eng mit Ihnen zusammenarbeiten, um zu Ihrem Erfolg beizutragen. Wir freuen uns, wenn Sie uns Ihr Feedback mitteilen würden, wie wir die Leads, die wir Ihnen zusenden, durch zusätzliche Daten optimieren und verbessern können, um diesen Kunden zum Erfolg zu verhelfen.
Teilen Sie uns Ihr [Feedback](mailto:AzureMarketOnboard@microsoft.com) und Ihre Vorschläge mit, damit wir Ihrem Vertriebsteam durch Marketplace-Leads zu größerem Erfolg verhelfen können.




