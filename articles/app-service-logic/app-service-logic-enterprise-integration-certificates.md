
---
title: Verwenden von Zertifikaten mit dem Enterprise Integration Pack | Microsoft Docs
description: Erfahren Sie, wie Sie Zertifikate mit dem Enterprise Integration Pack und Logic Apps verwenden
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: cgronlun

ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: deonhe

---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Informationen zu Zertifikaten und Enterprise Integration Pack
## <a name="overview"></a>Übersicht
Für die Unternehmensintegration werden Zertifikate zum Schutz der B2B-Kommunikation verwendet. Sie können zwei Typen von Zertifikaten in Ihren Unternehmensintegrations-Apps nutzen:

* Öffentliche Zertifikate, die bei einer Zertifizierungsstelle erworben werden müssen.
* Private, selbst ausgestellte Zertifikate. Diese Zertifikate werden gelegentlich auch als selbstsignierte Zertifikate bezeichnet.

## <a name="what-are-certificates?"></a>Was sind Zertifikate?
Zertifikate sind digitale Dokumente, mit denen die Identität der Teilnehmer an einer elektronischen Kommunikation überprüft wird und die auch zur Sicherung der elektronischen Kommunikation dienen.

## <a name="why-use-certificates?"></a>Gründe für Zertifikate
Die Kommunikation zwischen zwei Unternehmen (Business-to-Business, B2B)muss mitunter vertraulich gehalten werden. Für die Unternehmensintegration werden Zertifikate zum Schutz dieser Kommunikation auf zwei Arten verwendet:

* Verschlüsseln des Inhalts von Nachrichten
* Digitales Signieren von Nachrichten  

## <a name="how-do-you-upload-certificates?"></a>Hochladen von Zertifikaten
### <a name="public-certificates"></a>Öffentliche Zertifikate
Um ein *öffentliches Zertifikat* in Ihren Logik-Apps mit B2B-Funktionen zu verwenden, müssen Sie das Zertifikat zuerst in Ihr Integrationskonto hochladen. Um ein *selbstsigniertes Zertifikat*verwenden, müssen Sie es zuerst in [Azure Key Vault](../key-vault/key-vault-get-started.md "Informationen zu Key Vault")hochladen.

Hochgeladene Zertifikate stehen zum Sichern Ihrer B2B-Nachrichten zur Verfügung, wenn Sie deren Eigenschaften in von Ihnen erstellten [Vereinbarungen](app-service-logic-enterprise-integration-agreements.md) definieren.  

Hier finden Sie die einzelnen Schritte, mit denen Sie Ihre öffentlichen Zertifikate in Ihr Integrationskonto hochladen können, nachdem Sie sich beim Azure-Portal angemeldet haben:

1. Wählen Sie **Durchsuchen**aus.  
    ![„Durchsuchen“ wählen](./media/app-service-logic-enterprise-integration-overview/overview-1.png)  
2. Geben Sie den Begriff **Integration** in das Filtersuchfeld ein, und wählen Sie in der Ergebnisliste die Option **Integrationskonten**.     
    ![„Integrationskonten“ wählen](./media/app-service-logic-enterprise-integration-overview/overview-2.png)
3. Wählen Sie das Integrationskonto aus, dem Sie das Zertifikat hinzufügen möchten.  
    ![Integrationskonto auswählen, dem Sie das Zertifikat hinzufügen möchten](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Wählen Sie die Kachel **Zertifikate** aus.  
   ![Kachel „Zertifikate“ auswählen](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)
5. Wählen Sie auf dem Blatt **Zertifikate** die Schaltfläche **Hinzufügen**.
    ![Schaltfläche „Hinzufügen“ wählen](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)
6. Geben Sie einen **Namen** für Ihr Zertifikat ein, und wählen Sie dann den Zertifikattyp aus. (In diesem Beispiel wurde der Zertifikattyp „öffentlich“ verwendet.) Wählen Sie das Ordnersymbol rechts neben dem Textfeld **Zertifikat**.
7. Wenn die Dateiauswahl geöffnet wird, können Sie die Zertifikatsdatei suchen und auswählen, die Sie in Ihr Integrationskonto hochladen möchten.
8. Wählen Sie in der Dateiauswahl das Zertifikat und dann die Option **OK** aus. Dadurch wird das Zertifikat überprüft und in Ihr Integrationskonto hochgeladen.
9. Wählen Sie nach Rückkehr zum Blatt **Zertifikat hinzufügen** die Schaltfläche **OK**.  
    ![Schaltfläche „OK“ wählen](./media/app-service-logic-enterprise-integration-certificates/certificate-3.png)  
10. Sie erhalten nach etwa einer Minute die Benachrichtigung, dass das Hochladen des Zertifikats abgeschlossen ist.
11. Wählen Sie die Kachel **Zertifikate** aus. Das neu hinzugefügte Zertifikat wird angezeigt.  
    ![Neues Zertifikat anzeigen](./media/app-service-logic-enterprise-integration-certificates/certificate-4.png)  

### <a name="private-certificates"></a>Private Zertifikate
Sie können private Zertifikate in Ihr Integrationskonto hochladen, indem Sie die folgenden Schritte ausführen:  

1. [Laden Sie Ihren privaten Schlüssel in den Schlüsseltresor hoch](../key-vault/key-vault-get-started.md "Informationen zu Key Vault")hochladen.  
   
   > [!TIP]
   > Sie müssen die Logic Apps-Funktion von Azure App Service zum Ausführen von Vorgängen in Key Vault autorisieren. Verwenden Sie den folgenden PowerShell-Befehl, um dem Logic Apps-Dienstprinzipal Zugriff zu gewähren: `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  
   > 
   > 
2. Erstellen Sie ein privates Zertifikat.  
3. Laden Sie das private Zertifikat in Ihr Integrationskonto hoch.

Nachdem Sie diese Schritte ausgeführt haben, können Sie das private Zertifikat nutzen, um Vereinbarungen zu erstellen.

Hier finden Sie die einzelnen Schritte, mit denen Sie Ihre privaten Zertifikate in Ihr Integrationskonto hochladen können, nachdem Sie sich beim Azure-Portal angemeldet haben:  

1. Wählen Sie **Durchsuchen**aus.  
    ![Das private Zertifikat in Ihr Integrationskonto hochladen](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Geben Sie den Begriff **Integration** in das Filtersuchfeld ein, und wählen Sie in der Ergebnisliste die Option **Integrationskonten**.     
    ![„Integrationskonten“ wählen](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Wählen Sie das Integrationskonto aus, dem Sie das Zertifikat hinzufügen möchten.  
    ![Integrationskonto auswählen, dem Sie das Zertifikat hinzufügen möchten](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Wählen Sie die Kachel **Zertifikate** aus.  
    ![Kachel „Zertifikate“ wählen](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)  
5. Wählen Sie auf dem Blatt **Zertifikate** die Schaltfläche **Hinzufügen**.
    ![Schaltfläche „Hinzufügen“ wählen](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)
6. Geben Sie einen **Namen** für Ihr Zertifikat ein, und wählen Sie dann den Zertifikattyp aus. (In diesem Beispiel wurde der Zertifikattyp „öffentlich“ verwendet.) Wählen Sie das Ordnersymbol rechts neben dem Textfeld **Zertifikat**.
7. Wenn die Dateiauswahl geöffnet wird, können Sie die Zertifikatsdatei suchen und auswählen, die Sie in Ihr Integrationskonto hochladen möchten.
8. Nachdem Sie das Zertifikat ausgewählt haben, wählen Sie in der Dateiauswahl **OK** aus. Dadurch wird das Zertifikat überprüft und in Ihr Integrationskonto hochgeladen.
9. Wählen Sie nach Rückkehr zum Blatt **Zertifikat hinzufügen** die Schaltfläche **OK**.  
    ![Zertifikat hinzufügen](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-1.png)  
10. Sie erhalten nach etwa einer Minute die Benachrichtigung, dass das Hochladen des Zertifikats abgeschlossen ist.
11. Wählen Sie die Kachel **Zertifikate** aus. Das neu hinzugefügte Zertifikat wird angezeigt.
    ![Neues Zertifikat anzeigen](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-2.png)  

Hochgeladene Zertifikate stehen zum Sichern Ihrer B2B-Nachrichten zur Verfügung, wenn Sie deren Eigenschaften in [Vereinbarungen](app-service-logic-enterprise-integration-agreements.md)definieren.  

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen einer Logik-App mit B2B-Features](app-service-logic-enterprise-integration-b2b.md)  
* [Erstellen einer B2B-Vereinbarung](app-service-logic-enterprise-integration-agreements.md)  
* [Weitere Informationen zum Schlüsseltresor](../key-vault/key-vault-get-started.md "Informationen zu Key Vault")  

<!--HONumber=Oct16_HO2-->


