<properties 
	pageTitle="Informationen zum Erstellen einer AS2-Vereinbarung für das Enterprise Integration Pack" 
	description="Informationen zum Erstellen einer AS2-Vereinbarung für das Enterprise Integration Pack | Microsoft Azure App Service" 
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
	ms.date="06/29/2016" 
	ms.author="deonhe"/>

# Unternehmensintegration mit AS2

## Erstellen einer AS2-Vereinbarung
Um die Features für die Unternehmensintegration in Logik-Apps zu verwenden, müssen Sie zuerst Vereinbarungen erstellen.

### Bevor Sie beginnen, benötigen Sie Folgendes:
- Ein in Ihrem Azure-Abonnement definiertes [Integrationskonto](./app-service-logic-enterprise-integration-accounts.md)
- Mindestens zwei [Partner](./app-service-logic-enterprise-integration-partners.md), die bereits in Ihrem Integrationskonto definiert sind

>[AZURE.NOTE]Beim Erstellen einer Vereinbarung muss der Inhalt der Vereinbarungsdatei dem Vereinbarungstyp entsprechen.


Nachdem Sie [ein Integrationskonto erstellt](./app-service-logic-enterprise-integration-accounts.md) und [Partner hinzugefügt](./app-service-logic-enterprise-integration-partners.md) haben, können Sie eine Vereinbarung erstellen, indem Sie folgende Schritte ausführen:

### Auf der Startseite des Azure-Portals

Nach Anmeldung beim [Azure-Portal](http://portal.azure.com "Azure-Portal"):
1. Wählen Sie im Menü links **Durchsuchen** aus.

>[AZURE.TIP]Wenn der Link **Durchsuchen** nicht angezeigt wird, müssen Sie möglicherweise zuerst das Menü erweitern. Wählen Sie dazu den Link **Menü anzeigen** aus, der sich links oben neben dem zugeklappten Menü befindet.

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)
2. Geben Sie in das Filtersuchfeld *Integration* ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus. ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)
3. Wählen Sie auf dem geöffneten Blatt **Integrationskonten** das Integrationskonto aus, in dem Sie die Vereinbarung erstellen möchten. Wenn keine Listen mit Integrationskonten angezeigt werden, [erstellen Sie zunächst ein Konto](./app-service-logic-enterprise-integration-accounts.md "Alles über Integrationskonten"). ![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)
4.  Wählen Sie die Kachel **Vereinbarungen** aus. Wenn die Kachel „Vereinbarungen“ nicht angezeigt wird, fügen Sie sie zunächst hinzu. ![](./media/app-service-logic-enterprise-integration-agreements/agreement-1.png)
5. Wählen Sie auf dem geöffneten Blatt „Vereinbarungen“ die Schaltfläche **Hinzufügen** aus. ![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)
6. Geben Sie einen **Namen** für die Vereinbarung ein. Wählen Sie dann auf dem geöffneten Blatt „Vereinbarungen“ den **Hostpartner**, die **Hostidentität**, den **Gastpartner** und die **Gastidentität** aus. ![](./media/app-service-logic-enterprise-integration-agreements/agreement-3.png)

Es folgen Details, die nützlich sein können, wenn Sie die Einstellungen für die Vereinbarung konfigurieren:
  
|Eigenschaft|Beschreibung|
|----|----|
|Hostpartner|Eine Vereinbarung benötigt einen Host- und einen Gastpartner. Der Hostpartner stellt die Organisation dar, die die Vereinbarung konfiguriert.|
|Hostidentität|Ein Bezeichner für den Hostpartner. |
|Gastpartner|Eine Vereinbarung benötigt einen Host- und einen Gastpartner. Der Gastpartner stellt die Organisation dar, die Geschäfte mit dem Hostpartner tätigt.|
|Gastidentität|Ein Bezeichner für den Gastpartner.|
|Empfangseinstellungen|Diese Eigenschaften gelten für alle Nachrichten, die von einer Vereinbarung empfangen werden.|
|Sendeeinstellungen|Diese Eigenschaften gelten für alle Nachrichten, die von einer Vereinbarung gesendet werden.|  
Lassen Sie uns fortfahren:
7. Wählen Sie **Empfangseinstellungen** aus, um zu konfigurieren, wie Nachrichten, die über diese Vereinbarung empfangen werden, behandelt werden.
 
 - Optional können Sie die Eigenschaften in der eingehenden Nachricht überschreiben. Aktivieren Sie hierzu das Kontrollkästchen **Nachrichteneigenschaften überschreiben**.
  - Aktivieren Sie das Kontrollkästchen **Nachricht muss signiert sein**, wenn Sie möchten, dass alle eingehenden Nachrichten signiert sein sollen. Wenn Sie diese Option auswählen, müssen Sie auch das **Zertifikat** auswählen, das zum Überprüfen der Signatur der Nachrichten verwendet wird.
  - Optional können Sie auch die Verschlüsselung von Nachrichten anfordern. Aktivieren Sie hierzu das Kontrollkästchen **Nachricht muss verschlüsselt sein**. Sie müssen dann das **Zertifikat** auswählen, das zum Entschlüsseln der eingehenden Nachrichten verwendet wird.
  - Sie können auch die Komprimierung von Nachrichten anfordern. Aktivieren Sie hierzu das Kontrollkästchen **Nachricht muss komprimiert sein**. ![](./media/app-service-logic-enterprise-integration-agreements/agreement-4.png)

In der nachstehenden Tabelle erfahren Sie, was die Empfangseinstellungen ermöglichen.

|Eigenschaft|Beschreibung|
|----|----|
|Nachrichteneigenschaften überschreiben|Wählen Sie diese Option aus, um anzugeben, dass Eigenschaften in empfangenen Nachrichten überschrieben werden können. |
|Nachricht muss signiert sein|Aktivieren Sie diese Option, um anzufordern, dass Nachrichten digital signiert werden müssen.|
|Nachricht muss verschlüsselt sein|Aktivieren Sie diese Option, um anzufordern, dass Nachrichten verschlüsselt werden müssen. Nicht verschlüsselte Nachrichten werden zurückgewiesen.|
|Nachricht muss komprimiert sein|Aktivieren Sie diese Option, um anzufordern, dass Nachrichten komprimiert werden müssen. Nicht komprimierte Nachrichten werden zurückgewiesen.|
|MDN-Text|Dies ist eine Standard-MDN, die an den Absender der Nachricht gesendet wird.|
|MDN senden|Aktivieren Sie diese Option, damit MDNs gesendet werden.|
|Signierte MDN senden|Aktivieren Sie diese Option, um anzufordern, dass MDNs signiert werden müssen.|
|MIC-Algorithmus||
|Asynchrone MDN senden|Aktivieren Sie diese Option, um anzufordern, dass Nachrichten asynchron gesendet werden.|
|URL|Dies ist die URL, an die Nachrichten gesendet werden.|
Lassen Sie uns nun fortfahren:
8. Wählen Sie **Sendeeinstellungen** aus, um zu konfigurieren, wie Nachrichten, die über diese Vereinbarung gesendet werden, behandelt werden. ![](./media/app-service-logic-enterprise-integration-agreements/agreement-5.png)

In der nachstehenden Tabelle erfahren Sie, was die Sendeeinstellungen ermöglichen.

|Eigenschaft|Beschreibung|
|----|----|
|Nachrichtensignatur aktivieren|Aktivieren Sie dieses Kontrollkästchen, um dafür zu sorgen, dass alle von der Vereinbarung gesendeten Nachrichten signiert sind.|
|MIC-Algorithmus|Wählen Sie den Algorithmus zum Signieren von Nachrichten.|
|Zertifikat|Wählen Sie das Zertifikat zum Signieren von Nachrichten.|
|Nachrichtenverschlüsselung aktivieren|Aktivieren Sie dieses Kontrollkästchen, um alle von der Vereinbarung gesendeten Nachrichten zu verschlüsseln.|
|Verschlüsselungsalgorithmus|Wählen Sie den Verschlüsselungsalgorithmus für die Verschlüsselung von Nachrichten aus.|
|HTTP-Header erweitern|Aktivieren Sie dieses Kontrollkästchen, um HTTP-Header des Typs „Content-Type“ in eine einzige Zeile zu erweitern.|
|MDN anfordern|Aktivieren Sie dieses Kontrollkästchen, um eine MDN für alle von dieser Vereinbarung gesendeten Daten anzufordern.|
|Signierte MDN anfordern|Aktivieren Sie diese Option, um anzufordern, dass alle MDNs signiert werden, die an diese Vereinbarung gesendet werden.|
|Asynchrone MDN anfordern|Aktivieren Sie diese Option, um anzufordern, dass eine asynchrone MDN an diese Vereinbarung gesendet wird.|
|URL|Die URL, an die MDNs gesendet werden.|
|NRR aktivieren|Aktivieren Sie dieses Kontrollkästchen, um die Nichtabstreitbarkeit des Empfangs zu aktivieren.|
Wir sind fast fertig!
9. Wählen Sie auf dem Blatt „Integrationskonto“ die Kachel **Vereinbarungen** aus, auf dem die neu hinzugefügte Vereinbarung aufgeführt ist. ![](./media/app-service-logic-enterprise-integration-agreements/agreement-6.png)

<!---HONumber=AcomDC_0803_2016-->