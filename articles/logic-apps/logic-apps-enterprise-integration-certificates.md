
---
title: Verwenden von Zertifikaten mit dem Enterprise Integration Pack | Microsoft-Dokumentation
description: Informationen zum Verwenden von Zertifikaten mit Enterprise Integration Pack | Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: cgronlun
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.translationtype: Human Translation
ms.sourcegitcommit: c8cfe5fb933cf9056b971d3e524f3c9ebc910215
ms.openlocfilehash: b494d5abfdd4045776b408cbb8ec99ee3905cf72
ms.contentlocale: de-de
ms.lasthandoff: 02/04/2017


---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Informationen zu Zertifikaten und Enterprise Integration Pack
## <a name="overview"></a>Übersicht
Für die Unternehmensintegration werden Zertifikate zum Schutz der B2B-Kommunikation verwendet. Sie können zwei Typen von Zertifikaten in Ihren Unternehmensintegrations-Apps nutzen:

* Öffentliche Zertifikate, die bei einer Zertifizierungsstelle erworben werden müssen.
* Private, selbst ausgestellte Zertifikate. Diese Zertifikate werden gelegentlich auch als selbstsignierte Zertifikate bezeichnet.

## <a name="what-are-certificates"></a>Was sind Zertifikate?
Zertifikate sind digitale Dokumente, mit denen die Identität der Teilnehmer an einer elektronischen Kommunikation überprüft wird und die auch zur Sicherung der elektronischen Kommunikation dienen.

## <a name="why-use-certificates"></a>Gründe für Zertifikate
Die Kommunikation zwischen zwei Unternehmen (Business-to-Business, B2B)muss mitunter vertraulich gehalten werden. Für die Unternehmensintegration werden Zertifikate zum Schutz dieser Kommunikation auf zwei Arten verwendet:

* Verschlüsseln des Inhalts von Nachrichten
* Digitales Signieren von Nachrichten  

## <a name="upload-a-public-certificate"></a>Hochladen eines öffentlichen Zertifikats

Um ein *öffentliches Zertifikat* in Ihren Logik-Apps mit B2B-Funktionen zu verwenden, müssen Sie das Zertifikat zuerst in Ihr Integrationskonto hochladen.  

Hochgeladene Zertifikate stehen zum Sichern Ihrer B2B-Nachrichten zur Verfügung, wenn Sie deren Eigenschaften in von Ihnen erstellten [Vereinbarungen](logic-apps-enterprise-integration-agreements.md) definieren.  

Hier finden Sie die einzelnen Schritte, mit denen Sie Ihre öffentlichen Zertifikate in Ihr Integrationskonto hochladen können, nachdem Sie sich beim Azure-Portal angemeldet haben:

1. Wählen Sie **Weitere Dienste** aus, und geben Sie im Filtersuchfeld den Begriff **Integration** ein. Wählen Sie in der Ergebnisliste **Integrationskonten** aus.     
![„Durchsuchen“ wählen](media/logic-apps-enterprise-integration-certificates/overview-1.png)  
2. Wählen Sie das Integrationskonto aus, dem Sie das Zertifikat hinzufügen möchten.  
![Integrationskonto auswählen, dem Sie das Zertifikat hinzufügen möchten](media/logic-apps-enterprise-integration-certificates/overview-3.png)  
3. Wählen Sie die Kachel **Zertifikate** aus.  
![Kachel „Zertifikate“ auswählen](media/logic-apps-enterprise-integration-certificates/certificate-1.png)
4. Wählen Sie auf dem Blatt **Zertifikate** die Schaltfläche **Hinzufügen**.   
![Schaltfläche „Hinzufügen“ wählen](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
5. Geben Sie unter **Name** einen Namen für Ihr Zertifikat ein, und wählen Sie in der Dropdownliste für den Zertifikattyp die Option **öffentlich** aus.  
6. Wählen Sie rechts neben dem Textfeld **Zertifikat** das Ordnersymbol aus. Wenn die Dateiauswahl geöffnet wird, können Sie die Zertifikatsdatei suchen und auswählen, die Sie in Ihr Integrationskonto hochladen möchten.
7. Wählen Sie in der Dateiauswahl das Zertifikat und dann die Option **OK** aus. Dadurch wird das Zertifikat überprüft und in Ihr Integrationskonto hochgeladen.
8. Wählen Sie nach Rückkehr zum Blatt **Zertifikat hinzufügen** die Schaltfläche **OK**.  
![Schaltfläche „OK“ wählen](media/logic-apps-enterprise-integration-certificates/certificate-3.png)  
9. Wählen Sie die Kachel **Zertifikate** aus. Das neu hinzugefügte Zertifikat wird angezeigt.  
![Neues Zertifikat anzeigen](media/logic-apps-enterprise-integration-certificates/certificate-4.png)  

## <a name="upload-a-private-certificate"></a>Hochladen eines privaten Zertifikats

Um ein *privates Zertifikat* in Ihren Logik-Apps mit B2B-Funktionen zu verwenden, können Sie ein privates Zertifikat in Ihr Integrationskonto hochladen, indem Sie die folgenden Schritte ausführen.

1. [Laden Sie Ihren privaten Schlüssel in Key Vault hoch](../key-vault/key-vault-get-started.md "Informationen zu Key Vault"), und geben Sie einen **Schlüsselnamen** an. 
   
   > [!TIP]
   > Logic Apps muss zum Ausführen von Vorgängen für Key Vault autorisiert werden. Verwenden Sie den folgenden PowerShell-Befehl, um dem Logic Apps-Dienstprinzipal Zugriff zu gewähren: `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  
   > 
   > 

Fügen Sie dem Integrationskonto nach dem vorherigen Schritt ein privates Zertifikat hinzu.

Hier finden Sie die einzelnen Schritte, mit denen Sie Ihre privaten Zertifikate in Ihr Integrationskonto hochladen können, nachdem Sie sich beim Azure-Portal angemeldet haben:  
 
1. Wählen Sie das Integrationskonto aus, dem Sie das Zertifikat hinzufügen möchten, und wählen Sie anschließend die Kachel **Zertifikate** aus.  
![Kachel „Zertifikate“ auswählen](media/logic-apps-enterprise-integration-certificates/certificate-1.png)  
2. Wählen Sie auf dem Blatt **Zertifikate** die Schaltfläche **Hinzufügen**.   
![Schaltfläche „Hinzufügen“ wählen](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
3. Geben Sie unter **Name** einen Namen für Ihr Zertifikat ein, und wählen Sie in der Dropdownliste für den Zertifikattyp die Option **privat** aus.   
4. Wählen Sie rechts neben dem Textfeld **Zertifikat** das Ordnersymbol aus. Suchen Sie in der Dateiauswahl nach dem entsprechenden öffentlichen Zertifikat, das Sie in Ihr Integrationskonto hochladen möchten.   
   
   > [!Note]
   > Beim Hinzufügen eines privaten Zertifikats muss das entsprechende öffentliche Zertifikat hinzugefügt werden, sodass es in den Empfangs- und Sendeeinstellungen der [AS2-Vereinbarung](logic-apps-enterprise-integration-as2.md) zum Signieren und Verschlüsseln der Nachrichten angezeigt wird.
   > 
   >   

5. Wählen Sie Werte für **Ressourcengruppe**, **Schlüsseltresor** und **Schlüsselname** und anschließend die Schaltfläche **OK** aus.  
![Zertifikat hinzufügen](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)  
6. Wählen Sie die Kachel **Zertifikate** aus. Das neu hinzugefügte Zertifikat wird angezeigt.
![Neues Zertifikat anzeigen](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)  



* [Erstellen einer B2B-Vereinbarung](logic-apps-enterprise-integration-agreements.md)  
* [Weitere Informationen zum Schlüsseltresor](../key-vault/key-vault-get-started.md "Informationen zu Key Vault")  


