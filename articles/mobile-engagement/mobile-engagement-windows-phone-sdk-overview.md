<properties 
	pageTitle="Windows Phone Silverlight SDK – Übersicht" 
	description="Übersicht über das Windows Phone Silverlight SDK für Azure Mobile Engagement" 					
	services="mobile-engagement" 
	documentationCenter="mobile" 
	authors="piyushjo" 
	manager="dwrede"
	editor="" />

<tags 
	ms.service="mobile-engagement" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="mobile-windows-phone" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/29/2016" 
	ms.author="piyushjo" />

#Übersicht über das Windows Phone Silverlight SDK für Azure Mobile Engagement

Beginnen Sie hier, um Details zur Integration von Azure Mobile Engagement in einer Windows Phone Silverlight-App abzurufen. Wenn Sie es gleich ausprobieren möchten, sollten Sie unbedingt das [15-Minuten-Lernprogramm](mobile-engagement-windows-phone-get-started.md) bearbeiten.

Klicken Sie, um den [SDK-Inhalt](mobile-engagement-windows-phone-sdk-content.md) anzuzeigen

##Integrationsverfahren

1. Hier starten: [Integrieren von Mobile Engagement in eine Windows Phone Silverlight-App](mobile-engagement-windows-phone-integrate-engagement.md)

2. Für Benachrichtigungen: [Integration von Reach (Benachrichtigungen) in einer Windows Phone Silverlight-App](mobile-engagement-windows-phone-integrate-engagement-reach.md)

3. Tag-Plan-Implementierung: [Verwenden der erweiterten Mobile Engagement-Tagging-API in einer Windows Phone Silverlight-App](mobile-engagement-windows-phone-use-engagement-api.md)

##Versionshinweise

###3\.3.0 (19.04.2016)
Teil des NuGet-Pakets *MicrosoftAzure.MobileEngagement* **v3.4.0**

-   „TestLogLevel“-API zum Aktivieren/Deaktivieren/Filtern von Konsolenprotokollen, die vom SDK ausgegeben wurden, wurde hinzugefügt.

Eine frühere Version finden Sie unter [Vollständige Versionshinweise](mobile-engagement-windows-phone-release-notes.md)

##Upgrade-Verfahren

Wenn Sie bereits eine ältere Version unseres SDK in Ihrer Anwendung integriert haben, müssen Sie die folgenden Punkte beim Aktualisieren des SDK beachten.

Möglicherweise müssen Sie mehrere Verfahren befolgen, wenn Sie mehrere Versionen des SDK verpasst haben. Die vollständigen [Upgrade-Verfahren](mobile-engagement-windows-phone-upgrade-procedure/) lesen. Wenn Sie beispielsweise von 0.10.1 zu 0.11.0 migrieren, müssen Sie zunächst den Vorgang "von 0.9.0 zu 0.10.1" ausführen und anschließend den Vorgang "von 0.10.1 zu 0.11.0".

###Von 2.0.0 zu 3.3.0

####Testprotokolle

Vom SDK produzierte Konsolenprotokolle können jetzt aktiviert/deaktiviert/gefiltert werden. Um diese anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen der aus der `EngagementTestLogLevel`-Enumeration verfügbaren Werte, beispielsweise:

			EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
			EngagementAgent.Instance.Init();

### Upgrade von älteren Versionen

Siehe [Upgrade-Verfahren](mobile-engagement-windows-phone-upgrade-procedure/)
 

<!---HONumber=AcomDC_0420_2016-->