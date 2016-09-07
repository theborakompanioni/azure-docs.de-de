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
	ms.date="08/24/2016" 
	ms.author="deonhe"/>

# Informationen zu Zertifikaten und Enterprise Integration Pack

## Übersicht
Für die Unternehmensintegration werden Zertifikate zum Schutz der B2B-Kommunikation verwendet. Sie können zwei Typen von Zertifikaten in Ihren Unternehmensintegrations-Apps nutzen:
- Öffentliche Zertifikate, die bei einer Zertifizierungsstelle erworben werden müssen.
- Private Zertifikate, die Sie selbst ausstellen können und mitunter als selbstsignierte Zertifikate bezeichnet werden.

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

Es folgen die einzelnen Schritte zum Hochladen Ihrer öffentlichen Zertifikate in Ihr Integrationskonto, nachdem Sie sich beim Azure-Portal angemeldet haben:
1. Wählen Sie **Durchsuchen** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)
3. Wählen Sie das **Integrationskonto** aus, dem Sie das Zertifikat hinzufügen möchten. ![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)
4.  Wählen Sie die Kachel **Zertifikate** aus. ![](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)
5. Wählen Sie auf dem Blatt „Zertifikate“ die Schaltfläche **Hinzufügen** aus. ![](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)
6. Geben Sie einen **Namen** für Ihr Zertifikat ein. Wählen Sie den Zertifikatstyp (in diesem Beispiel habe ich ein öffentliches Zertifikat verwendet). Wählen Sie dann das Symbol „Ordner“ rechts neben dem Textfeld **Zertifikat** aus. Dies öffnet das Dialogfeld zur Dateiauswahl, indem Sie zur gewünschten Zertifikatsdatei navigieren, die Sie in Ihr Integrationskonto hochladen möchten. Nachdem Sie das Zertifikat ausgewählt haben, wählen Sie in der Dateiauswahl **OK** aus. Dadurch wird das Zertifikat überprüft und in Ihr Integrationskonto hochgeladen. Wählen Sie nach Rückkehr zum Blatt **Zertifikat hinzufügen** die Schaltfläche **OK** aus. ![](./media/app-service-logic-enterprise-integration-certificates/certificate-3.png)
7. Innerhalb einer Minute erhalten Sie die Benachrichtigung, dass das Hochladen des Zertifikats abgeschlossen ist.
8. Wählen Sie die Kachel **Zertifikate** aus. Dies aktualisiert die Seite, und das neu hinzugefügte Zertifikat sollte angezeigt werden: ![](./media/app-service-logic-enterprise-integration-certificates/certificate-4.png)

### Privates Zertifikat
Sie können in Ihr Integrationskonto auch private Zertifikate hochladen. Dafür müssen Sie die folgenden Schritte ausführen:
1. [Laden Sie Ihren privaten Schlüssel in den Schlüsseltresor hoch.](../key-vault/key-vault-get-started.md "Informationen zu Key Vault")
2. Erstellen Sie ein privates Zertifikat.
3. Laden Sie das private Zertifikat in Ihr Integrationskonto hoch.

Nachdem Sie diese Schritte ausgeführt haben, können Sie das private Zertifikat nutzen, um Vereinbarungen zu erstellen.

Es folgen die einzelnen Schritte zum Hochladen Ihrer privaten Zertifikate in Ihr Integrationskonto, nachdem Sie sich beim Azure-Portal angemeldet haben:
1. Wählen Sie **Durchsuchen** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)
2. Geben Sie in das Filtersuchfeld **Integration** ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)
3. Wählen Sie das **Integrationskonto** aus, dem Sie das Zertifikat hinzufügen möchten. ![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)
4.  Wählen Sie die Kachel **Zertifikate** aus. ![](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)
5. Wählen Sie auf dem Blatt „Zertifikate“ die Schaltfläche **Hinzufügen** aus. ![](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)
6. Geben Sie einen Namen für Ihr Zertifikat ein. Wählen Sie den Zertifikatstyp aus. (In diesem Beispiel habe ich ein privates Zertifikat verwendet.) Wählen Sie dann rechts neben dem Textfeld **Zertifikat** das Ordnersymbol aus. Daraufhin öffnet sich die Dateiauswahl, in der Sie zu der Zertifikatsdatei navigieren können, die Sie in Ihr Integrationskonto hochladen möchten. Wählen Sie in der Dateiauswahl das gewünschte Zertifikat und anschließend die Option **OK** aus. Dadurch wird das Zertifikat überprüft und in Ihr Integrationskonto hochgeladen. Wählen Sie abschließend auf dem Blatt **Zertifikat hinzufügen** die Schaltfläche **OK** aus. ![](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-1.png)
7. Innerhalb einer Minute erhalten Sie die Benachrichtigung, dass das Hochladen des Zertifikats abgeschlossen ist.
8. Wählen Sie die Kachel **Zertifikate** aus. Daraufhin wird die Seite aktualisiert, und das neu hinzugefügte Zertifikat wird angezeigt: ![](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-2.png)

Hochgeladene Zertifikate stehen zum Schutz Ihrer B2B-Nachrichten zur Verfügung, wenn Sie deren Eigenschaften in [Vereinbarungen](./app-service-logic-enterprise-integration-agreements.md) definieren.


## Nächste Schritte
- - [Erstellen einer Logik-App mit B2B-Features](./app-service-logic-enterprise-integration-b2b.md)
- [Erstellen einer B2B-Vereinbarung](./app-service-logic-enterprise-integration-agreements.md)
- [Weitere Informationen zum Schlüsseltresor](../key-vault/key-vault-get-started.md "Informationen zu Key Vault")

<!---HONumber=AcomDC_0824_2016-->