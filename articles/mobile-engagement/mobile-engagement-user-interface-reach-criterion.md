---
title: "Azure Mobile Engagement – Benutzeroberfläche – Reichweitenkriterium"
description: "Erfahren Sie, wie Sie die Zielgruppenadressierung anhand bestimmter Kriterien von Azure Mobile Engagement verwenden können, um Pushkampagnen an eine Teilmenge der Benutzer versenden"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: dwrede
editor: 
ms.assetid: a4ed03a0-55b1-4dd8-b0bd-c475005afb66
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: afa6625e4c3ebe2a408b4bd956c8d2eb9ac6a34b


---
# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>Zielgruppenadressierung anhand bestimmter Kriterien zur Versendung von Pushkampagnen an eine Teilmenge der Benutzer
Die Zielgruppenadressierung anhand spezifischer Kriterien mithilfe der neuen Schaltfläche "Neue Kriterien" ist eines der leistungsstärksten Konzepte in Azure Mobile Engagement. Denn Sie können relevante Pushbenachrichtigungen senden, auf die die Kunden reagieren, anstatt Empfänger mit Spam zu belästigen. Sie können Ihre Zielgruppe basierend auf Standardkriterien einschränken und Pushvorgänge simulieren, um zu bestimmen, wie viele Personen die Benachrichtigung erhalten sollen.

**Weitere Informationen:**

* [Dokumentation zur Benutzeroberfläche – Reichweite – Neue Pushkampagne][Link 27]

## <a name="audience-criteria-can-include"></a>Kriterien für die Zielgruppe können Folgendes umfassen:
* **Technische Informationen: ** Sie können die Zielgruppe auf der Grundlage der gleichen technischen Informationen bestimmen, die in den Bereichen „Analyse“ und „Überwachen“ angezeigt werden. **Weitere Informationen:** [Dokumentation zur Benutzeroberfläche – Analyse][Link 15], [Dokumentation zur Benutzeroberfläche – Monitor][Link 16]
* **Standort:** Anwendungen, die "Echtzeit-Berichterstellung für Standorte" mit Geofencing verwenden, ist es möglich, Geolocation als Kriterium auszuwählen, um eine Zielgruppe über den GPS-Standort zu bestimmen. "Verzögerte Berichterstellung für Standorte" kann ebenfalls verwendet werden, um eine Zielgruppe über den Standort des Mobiltelefons zu bestimmen ("Echtzeit-Berichterstellung für Standorte" und "Verzögerte Berichterstellung für Standorte" müssen im SDK aktiviert sein). **Weitere Informationen:** [SDK-Dokumentation – iOS – Integration][Link 5], [SDK-Dokumentation – Android – Integration][Link 5]
* **Feedback zur Reichweite:** Durch Reichweitenfeedback aus Ankündigungen, Umfragen und Datenpushes können Sie Ihre Zielgruppe basierend auf deren Feedback aus vorherigen Reichweitenbenachrichtigungen bestimmen. So können Sie Ihre Zielgruppe nach zwei oder drei Kampagnen besser bestimmen als bei der ersten. Die Funktion kann auch Benutzer herausfiltern, die bereits eine Benachrichtigung mit ähnlichem Inhalt erhalten haben. Hierfür erstellt sie eine Kampagne, die NICHT an Benutzer verschickt wird, die eine solche vorherige Kampagne schon erhalten haben. Sie können sogar Benutzer, die Teil einer aktiven Kampagne sind, vom Erhalt neuer Pushes ausschließen. **Weitere Informationen:** [Dokumentation zur Benutzeroberfläche – Reichweite – Pushübertragung von Inhalten][Link 29]
* **Nachverfolgen der Installation:** Sie können nachverfolgen, wo die Benutzer Ihre App installiert haben. **Weitere Informationen:** [Dokumentation zur Benutzeroberfläche – Einstellungen][Link 20]
* **Benutzerprofil:** Sie können Ihre Zielgruppe basierend auf den Standardbenutzerdaten oder den von Ihnen erstellten Anwendungsinformationen erreichen. Dazu gehören derzeit angemeldete Benutzer und Benutzer, die bestimmte von Ihnen in der App selbst gestellte Fragen beantwortet haben, anstatt nur hinsichtlich ihrer Reaktion auf frühere Kampagnen. Alle App-Infos, die Sie für die App definiert haben, werden in dieser Liste angezeigt.
* Segmente: Sie können die Zielgruppe auch basierend auf Segmenten besimmen, die Sie auf Grundlage bestimmter Benutzerverhalten, die mehrere Kriterien umfassen, erstellt haben. Alle Segmente, die Sie für die App definiert haben, werden in dieser Liste angezeigt. **Weitere Informationen:** [Dokumentation zur Benutzeroberfläche – Segmente][Link 18]
* **App-Info:** Zum Nachverfolgen des Benutzerverhaltens können benutzerdefinierte Info-Tags der App aus den "Einstellungen" erstellt werden. **Weitere Informationen:** [Dokumentation zur Benutzeroberfläche – Einstellungen][Link 20]

## <a name="example"></a>Beispiel:
Angenommen, Sie möchten eine Ankündigung per Push nur an die Benutzer verteilen, die einen In-App-Einkauf getätigt haben.

1. Wechseln Sie zur Seite mit den Anwendungseinstellungen, und wählen Sie im Menü „App-Informationen“ die Option „New app info“ (Neue App-Information) aus.
2. Registrieren Sie eine neue boolesche App-Info namens "InAppPurchase".
3. Sorgen Sie dafür, dass diese App-Information in Ihrer Anwendung auf „true“ festgelegt wird, wenn der Benutzer erfolgreich einen In-App-Einkauf getätigt hat. Verwenden Sie hierzu die Funktion „sendAppInfo("inAppPurchase", ...)“.
4. Wenn Sie das nicht in Ihrer Anwendung vornehmen möchten, können Sie dies in Ihrem Back-End mithilfe der Device-API tun.
5. Sie müssen dann lediglich Ihre Ankündigung mit einem Kriterium erstellen, das Ihre Zielgruppe auf Benutzer begrenzt, bei denen "inAppPurchase" auf "true" festgelegt ist.

> [!NOTE]
> Für eine Zielgruppenadressierung, die auf anderen Kriterien als App-Infotags basiert, muss Azure Mobile Engagement Informationen von den Geräten Ihrer Benutzer erfassen, ehe der Push gesendet wird, was eine Verzögerung verursachen kann. Komplexe Pushkonfigurationsoptionen (wie die Aktualisierung von Badges) können ebenfalls Pushvorgänge verzögern. Eine "einmalige" Kampagne mithilfe der Push-API ist die absolut schnellste Pushmethode in Azure Mobile Engagement. Das ausschließliche Verwenden von App-Infotags als Pushkriterien für eine Reach-Kampagne (entweder über die Reach-API oder die Benutzeroberfläche) ist die zweitschnellste Methode, da App-Infotags auf Serverseite gespeichert werden. Das Verwenden anderer Kriterien für die Zielgruppenadressierung für eine Pushkampagne ist die flexibelste, aber langsamste Pushmethode, da Azure Mobile Engagement die Geräte abfragen muss, um die Kampagne senden zu können.

![Reichweitenkriterium1][29] 

## <a name="criterion-options-apply-to"></a>Kriteriumoptionen gelten für:
* **Technische Informationen**     
* Firmwarename: Name der Firmware
* Firmwareversion: Firmwareversion
* Gerätemodell: Gerätemodell
* Gerätehersteller: Gerätehersteller
* Anwendungsversion: Anwendungsversion
* Name des Netzbetreibers: Name des Netzbetreibers, nicht definiert
* Carrier country (Land des Netzbetreibers): Land des Netzbetreibers, nicht definiert
* Netzwerktyp: Netzwerktyp
* Gebietsschema: Gebietsschema
* Bildschirmgröße: Bildschirmgröße
* **Standort**      
* Letzter bekannter Bereich: Land, Region, Ort
* Echtzeit-Geofencing: Liste der POIs (Name, Aktionen), kreisförmiger POIs (Name, Breite, Länge, Radius in Metern)
* **Feedback zur Reichweite**     
* Ankündigungsfeedback: Ankündigung, Feedback
* Umfragefeedback: Umfrage, Feedback
* Umfrageantwort-Feedback: Antwortfeedback der Umfrage, Frage, Auswahl
* Datenpush-Feedback: Datenpush, Feedback
* **Nachverfolgung der Installation**     
* Store: Store, nicht definiert
* Quelle: Quelle, nicht definiert
* **Benutzerprofil**     
* Geschlecht: männlich oder weiblich, nicht definiert
* Geburtsdatum: Operator, Datum, nicht definiert
* Pushbenachrichtigungen zulässig: TRUE oder FALSE, nicht definiert
* **App-Info**      
* Zeichenfolge: Zeichenfolge, nicht definiert
* Datum: Operator, Datum, nicht definiert
* Integer: Operator, Nummer, nicht definiert
* Boolesch: TRUE oder FALSE, nicht definiert
* **Segment**    
* Name von Segmenten (in Dropdownliste), Ausschluss (Zielbenutzer, die nicht Teil dieses Segments sind).

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md




<!--HONumber=Feb17_HO3-->


