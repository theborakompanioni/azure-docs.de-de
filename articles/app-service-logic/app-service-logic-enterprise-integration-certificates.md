<properties 
	pageTitle="Verwenden von Zertifikaten mit dem Enterprise Integration Pack | Microsoft Azure App Service" 
	description="Erfahren Sie, wie Sie Zertifikate mit dem Enterprise Integration Pack und Logik-Apps verwenden" 
	services="logic-apps" 
	documentationCenter=".net,nodejs,java"
	authors="msftman" 
	manager="erikre" 
	editor="cgronlun"/>

<tags 
	ms.service="logic-apps" 
	ms.workload="integration" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/06/2016" 
	ms.author="deonhe"/>

# Informationen zu Zertifikaten und Enterprise Integration Pack

## Übersicht
Für die Unternehmensintegration werden Zertifikate zum Schutz der B2B-Kommunikation verwendet. Sie können zwei Typen von Zertifikaten in Ihren Unternehmensintegrations-Apps nutzen:
- Öffentliche Zertifikate, die bei einer Zertifizierungsstelle erworben werden müssen.
- Private, selbst ausgestellte Zertifikate. Diese Zertifikate werden gelegentlich auch als selbstsignierte Zertifikate bezeichnet.

## Was sind Zertifikate?
Zertifikate sind digitale Dokumente, die zum Überprüfen der Identität der Teilnehmer an einer elektronischen Kommunikation und zum Schutz der elektronischen Kommunikation verwendet werden.

## Gründe für Zertifikate
Die Kommunikation zwischen zwei Unternehmen muss mitunter vertraulich gehalten werden. Für die Unternehmensintegration werden Zertifikate zum Schutz dieser Kommunikation auf zwei Arten verwendet:
- Verschlüsseln des Inhalts von Nachrichten
- Digitales Signieren von Nachrichten

## Hochladen von Zertifikaten

### Öffentliches Zertifikat
Um ein **öffentliches Zertifikat** in Ihren Logik-Apps mit B2B-Funktionen zu verwenden, müssen Sie es zuerst in Ihr Integrationskonto hochladen. Wenn Sie hingegen ein **selbstsigniertes Zertifikat** verwenden, müssen Sie es zuerst in den [Schlüsseltresor](../key-vault/key-vault-get-started.md "Informationen zu Key Vault") hochladen.

Hochgeladene Zertifikate stehen zum Schutz Ihrer B2B-Nachrichten zur Verfügung, wenn Sie deren Eigenschaften in von Ihnen erstellten [Vereinbarungen](./app-service-logic-enterprise-integration-agreements.md) definieren.

Im Anschluss folgen die einzelnen Schritte, mit denen Sie Ihre öffentlichen Zertifikate in Ihr Integrationskonto hochladen können, nachdem Sie sich beim Azure-Portal angemeldet haben:
1. Wählen Sie **Durchsuchen** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)
3. Wählen Sie das **Integrationskonto** aus, dem Sie das Zertifikat hinzufügen möchten. ![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)
4.  Wählen Sie die Kachel **Zertifikate** aus. ![](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)
5. Wählen Sie auf dem Blatt „Zertifikate“ die Schaltfläche **Hinzufügen** aus. ![](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)
6. Geben Sie einen **Namen** für Ihr Zertifikat ein. Wählen Sie den Zertifikatstyp (in diesem Beispiel habe ich ein öffentliches Zertifikat verwendet). Wählen Sie dann das Symbol „Ordner“ rechts neben dem Textfeld **Zertifikat** aus. Die Dateiauswahl wird geöffnet. Hier können Sie zu der Zertifikatsdatei navigieren, die Sie in Ihr Integrationskonto hochladen möchten, und die Datei auswählen. Nachdem Sie das Zertifikat ausgewählt haben, wählen Sie in der Dateiauswahl **OK** aus. Dadurch wird das Zertifikat überprüft und in Ihr Integrationskonto hochgeladen. Wählen Sie nach Rückkehr zum Blatt **Zertifikat hinzufügen** die Schaltfläche **OK** aus. ![](./media/app-service-logic-enterprise-integration-certificates/certificate-3.png)
7. Innerhalb einer Minute erhalten Sie die Benachrichtigung, dass das Hochladen des Zertifikats abgeschlossen ist.
8. Wählen Sie die Kachel **Zertifikate** aus. Nach dem Aktualisieren der Seite wird das neu hinzugefügte Zertifikat angezeigt: ![](./media/app-service-logic-enterprise-integration-certificates/certificate-4.png)

### Privates Zertifikat
Sie können auch private Zertifikate in Ihr Integrationskonto hochladen. Führen Sie hierzu die folgenden Schritte aus:

1. [Laden Sie Ihren privaten Schlüssel in den Schlüsseltresor hoch.](../key-vault/key-vault-get-started.md "Informationen zu Key Vault")

	> [AZURE.TIP] Der Logic Apps-Dienst muss zum Ausführen von Vorgängen für Key Vault autorisiert werden. Verwenden Sie den folgenden PowerShell-Befehl, um dem Logic Apps-Dienstprinzipal Zugriff zu gewähren: `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`

2. Erstellen Sie ein privates Zertifikat.
3. Laden Sie das private Zertifikat in Ihr Integrationskonto hoch.

Nachdem Sie diese Schritte ausgeführt haben, können Sie das private Zertifikat nutzen, um Vereinbarungen zu erstellen.

Im Anschluss folgen die einzelnen Schritte, mit denen Sie Ihre privaten Zertifikate in Ihr Integrationskonto hochladen können, nachdem Sie sich beim Azure-Portal angemeldet haben:
1. Wählen Sie **Durchsuchen** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)
3. Wählen Sie das **Integrationskonto** aus, dem Sie das Zertifikat hinzufügen möchten. ![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)
4.  Wählen Sie die Kachel **Zertifikate** aus. ![](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)
5. Wählen Sie auf dem Blatt „Zertifikate“ die Schaltfläche **Hinzufügen** aus. ![](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)
6. Geben Sie einen **Namen** für Ihr Zertifikat ein. Wählen Sie den Zertifikatstyp (in diesem Beispiel habe ich ein öffentliches Zertifikat verwendet). Wählen Sie dann das Symbol „Ordner“ rechts neben dem Textfeld **Zertifikat** aus. Die Dateiauswahl wird geöffnet. Hier können Sie zu der Zertifikatsdatei navigieren, die Sie in Ihr Integrationskonto hochladen möchten, und die Datei auswählen. Nachdem Sie das Zertifikat ausgewählt haben, wählen Sie in der Dateiauswahl **OK** aus. Dadurch wird das Zertifikat überprüft und in Ihr Integrationskonto hochgeladen. Wählen Sie nach Rückkehr zum Blatt **Zertifikat hinzufügen** die Schaltfläche **OK** aus. ![](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-1.png)
7. Innerhalb einer Minute erhalten Sie die Benachrichtigung, dass das Hochladen des Zertifikats abgeschlossen ist.
8. Wählen Sie die Kachel **Zertifikate** aus. Das neu hinzugefügte Zertifikat wird angezeigt: ![](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-2.png)

Hochgeladene Zertifikate stehen zum Schutz Ihrer B2B-Nachrichten zur Verfügung, wenn Sie deren Eigenschaften in [Vereinbarungen](./app-service-logic-enterprise-integration-agreements.md) definieren.


## Nächste Schritte
- [Erstellen einer Logik-App mit B2B-Features](./app-service-logic-enterprise-integration-b2b.md)
- [Erstellen einer B2B-Vereinbarung](./app-service-logic-enterprise-integration-agreements.md)
- [Weitere Informationen zum Schlüsseltresor](../key-vault/key-vault-get-started.md "Informationen zu Key Vault")

<!---HONumber=AcomDC_0907_2016-->