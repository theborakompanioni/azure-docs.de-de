<properties 
	pageTitle="Windows Universal SDK – Übersicht" 
	description="Übersicht über das Windows Universal SDK für Azure Mobile Engagement" 									
	services="mobile-engagement" 
	documentationCenter="mobile" 
	authors="piyushjo" 
	manager="dwrede" 
	editor="" />

<tags 
	ms.service="mobile-engagement" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="mobile-windows-store" 
	ms.devlang="dotnet" 
	ms.topic="article" 
	ms.date="02/29/2016" 
	ms.author="piyushjo" />

#Übersicht über das Windows Universal SDK für Azure Mobile Engagement

Starten Sie hier, um sich näher über die Integration von Azure Mobile Engagement in eine universelle Windows-App zu informieren. Wenn Sie es gleich ausprobieren möchten, sollten Sie unbedingt das [15-Minuten-Lernprogramm](mobile-engagement-windows-store-dotnet-get-started.md) bearbeiten.

Klicken Sie, um den [SDK-Inhalt](mobile-engagement-windows-store-sdk-content.md) anzuzeigen

##Integrationsverfahren

1. Hier starten: [Integrieren von Mobile Engagement in eine universelle Windows-App](mobile-engagement-windows-store-integrate-engagement.md)

2. Für Benachrichtigungen: [Integration von Reach (Benachrichtigungen) in einer universellen Windows-App](mobile-engagement-windows-store-integrate-engagement-reach.md)

3. Tag-Plan-Implementierung: [Verwenden der erweiterten Mobile Engagement-Tagging-API in einer universellen Windows-App](mobile-engagement-windows-store-use-engagement-api.md)

##Versionshinweise

###3\.4.0 (19.04.2016)

-   Reach-Overlayverbesserungen.
-   „TestLogLevel“-API zum Aktivieren/Deaktivieren/Filtern von Konsolenprotokollen, die vom SDK ausgegeben wurden, wurde hinzugefügt.
-   Behebung des Problems mit Inaktivitätsbenachrichtigungen, die sich darauf bezogen, dass die erste Aktivität beim App-Start nicht angezeigt wurde.

Eine frühere Version finden Sie unter [Vollständige Versionshinweise](mobile-engagement-windows-store-release-notes.md)

##Upgrade-Verfahren

Wenn Sie bereits eine ältere Version von Engagement in Ihre Anwendung integriert haben, müssen Sie die folgenden Punkte beim Aktualisieren des SDK beachten.

Möglicherweise müssen Sie mehrere Verfahren ausführen, wenn Sie mehrere SDK-Versionen übersprungen haben. Umfassende Informationen finden Sie unter [Upgrade-Verfahren](mobile-engagement-windows-store-upgrade-procedure.md) . Wenn Sie beispielsweise von 0.10.1 zu 0.11.0 migrieren, müssen Sie zunächst den Vorgang "von 0.9.0 zu 0.10.1" ausführen und anschließend den Vorgang "von 0.10.1 zu 0.11.0".

###Von 3.3.0 bis 3.4.0

####Testprotokolle

Vom SDK produzierte Konsolenprotokolle können jetzt aktiviert/deaktiviert/gefiltert werden. Um diese anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen der aus der `EngagementTestLogLevel`-Enumeration verfügbaren Werte, beispielsweise:

			EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
			EngagementAgent.Instance.Init();

####Ressourcen

Das Reach-Overlay wurde verbessert. Es ist Teil der SDK-NuGet-Paket-Ressourcen.

Während der Aktualisierung auf die neue Version des SDKs können Sie auswählen, ob Sie vorhandene Dateien aus dem Overlayordner von Ihren Ressourcen beibehalten möchten oder nicht:

* Wenn das vorherige Overlay bei Ihnen funktioniert, oder Sie die `WebView`-Elemente manuell integrieren, dann können Sie entscheiden, Ihre vorhandenen Dateien beizubehalten – es wird weiterhin funktionieren. 
* Wenn Sie auf das neue Overlay aktualisieren möchten, ersetzen Sie einfach den gesamten `overlay`-Ordner aus Ihren Ressourcen durch den neuen aus dem SDK-Paket (UWP-Apps: Nach dem Upgrade erhalten Sie den neuen Overlayordner aus % USERPROFILE%\\.nuget\\packages\\MicrosoftAzure.MobileEngagement\\3.4.0\\content\\win81\\Resources).

> [AZURE.WARNING] Mithilfe des neuen Overlays werden alle an der vorherigen Version vorgenommenen Anpassungen überschrieben.

### Upgrade von älteren Versionen

Siehe [Upgrade-Verfahren](mobile-engagement-windows-store-upgrade-procedure/)

<!---HONumber=AcomDC_0420_2016-->