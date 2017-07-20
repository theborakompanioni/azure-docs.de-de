---
title: "Veröffentlichungsleitfaden für Cortana Intelligence in AppSource | Microsoft-Dokumentation"
description: "Im Folgenden werden alle Schritte beschrieben, die Sie als Microsoft-Partner beim Veröffentlichen Ihrer Cortana Intelligence-Lösung in AppSource durchlaufen müssen."
services: machine-learning
documentationcenter: 
author: AnupamMicrosoft
manager: jhubbard
editor: cgronlun
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: anupams;v-bruham;garye
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 67e2e7906df78ff3e57765e0a6706f98123c3428
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

--- 
# <a name="cortana-intelligence-appsource-publishing-guide"></a>Veröffentlichungsleitfaden für Cortana Intelligence in AppSource

## <a name="overview"></a>Übersicht
AppSource ist der einzige Ort für Entscheidungsträger (BDMs), um von Partnern erstellte und von Microsoft ausgewertete Unternehmenslösungen/Apps zu entdecken und direkt auszuprobieren. Sehen Sie sich [dieses Video](https://youtu.be/hpq_Y9LuIB8) an, um zu erfahren, wie AppSource funktioniert. 

Als Microsoft-Partner können Sie wirklich vom Veröffentlichen auf AppSource profitieren, wenn Sie:
- Eine intelligente Lösung/App mithilfe von [Cortana Intelligence Suite](https://azure.microsoft.com/en-us/suites/cortana-intelligence-suite/?cdn=disable) erstellt haben.
- Ihre Lösung oder App ein bestimmtes Geschäftsproblem behandelt.
- Sie Module oder geistiges Eigentum erstellt haben, das Ihre Kunden in einer vorhersagbaren Weise relativ schnell wiederverwenden können.

In diesem Artikel werden die Schritte zum Veröffentlichen der Cortana Intelligence-Lösung eines Partners auf AppSource durchlaufen

## <a name="getting-started"></a>Erste Schritte
1. Im [Leitfaden zu den Vorteilen der Partner-Community (Partner Community Benefits Guide)](https://www.microsoftpartnerserverandcloud.com/_layouts/download.aspx?SourceUrl=Hosted%20Documents/Partner%20Community%20Benefits%20Guide%20-%20Cloud%20and%20Enterprise.pdf) (PDF), erfahren Sie auf Seite 9, wie sie als Advanced Analytics-Partner gelistet werden.
1. Füllen Sie das Formular [App übermitteln](https://appsource.microsoft.com/en-us/) aus.

    Aktivieren Sie bei der Frage *„Produkte wählen, für die Ihre App erstellt wurde“* das Kontrollkästchen **Andere**, und geben Sie im Bearbeitungssteuerelement „Cortana Intelligence“ an.
1. Bevor eine Cortana Intelligence-App in AppSource integriert werden kann, muss sie durch das Partnerlösungs-Technologieteam von Cortana-Intelligence zertifiziert werden. Um diesen Vorgang zu beginnen, teilen Sie uns Einzelheiten zu Ihrer App mit, indem Sie das Umfrageformular unter [Cortana Intelligence-Lösungsauswertung für die Veröffentlichung mit AppSource](https://aka.ms/cisappsrceval) ausfüllen. Diese Seite ist durch ein Kennwort geschützt und enthält Anweisungen zum Abrufen des Kennworts.

## <a name="solution-evaluation-criteria"></a>Kriterien für die Lösungsauswertung
Hier finden Sie eine Auflistung der Kriterien, die die App erfüllen muss.
1. Die App muss ein bestimmtes geschäftliches Anwendungsfallproblem innerhalb eines bestimmten Funktionsbereichs wiederholbar und mit Minimalkonfigurationen für vordefinierte, innerhalb eines kurzen Zeitraums implementierbaren Wertbeiträgen ansprechen.
1. Die Lösung sollte mindestens eine der folgenden Komponenten verwenden:

    - HDInsight
    - Machine Learning
    - Data Lake Analytics
    - Stream Analytics
    - Cognitive Services
    - Bot Framework
    - Analysis Services
    - Microsoft R Server eigenständig
    - R-Services auf SQL 2016 oder HDInsight Premium
1. Pro Monat und Kunde sollte die Lösung mindestens 1000 $ mit DPOR/CSP generieren.
1. Die Lösung sollte die einmalige Azure Active Directory-Verbundanmeldung (AAD-Verbund-SSO) mit aktivierter Zustimmung für die Benutzerauthentifizierung und Ressourcenzugriffssteuerungen verwenden. Sie müssen dem Auswertungsteam zeigen, dass bei Ihrer Lösung AAD Verbund-SSO aktiviert ist, bevor Ihre App auf AppSource integriert werden kann.

     Um zu sehen, was es bedeutet, AAD Verbund-SSO aktiviert zu haben, gehen Sie in dem Video [AppSource-Testbenutzeroberflächen-Walkthrough](https://aka.ms/trialexperienceforwebapps) zu der Stelle 02:35. Wenn bei Ihrer App AAD Verbund-SSO noch nicht aktiviert ist, finden Sie hier noch einige Dokumentation dazu
   1. [Anmeldung mit nur einem Klick](https://identity.microsoft.com/Landing?ru=https://identity.microsoft.com/).
   1. [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application).
     
1. Verwenden Sie Power BI in Ihrer Lösung: optional, aber dringend empfohlen, da hierbei nachweislich mehr Leads generiert werden.

## <a name="devcenter-account-setup"></a>Einrichtung des DevCenter-Kontos
Hier handelt es sich um den Vorgang zur Registrierung Ihres Unternehmens als Verleger bei Microsoft. Wenn Sie bereits ein registrierter Verleger mit einem vorhandenen DevCenter-Konto sind, geben Sie die E-Mail-ID an, die Ihrem Konto DevCenter zugeordnet ist. 

Wenn Sie keine haben sollten, finden Sie unten die wichtigsten Schritte zur Einrichtung eines DevCenter-Kontos.
1. Erstellen Sie [hier](https://signup.live.com/signup.aspx) ein Microsoft-Konto.
1. Gehen Sie zur Microsoft-DevCenter [Registrierungsseite](http://go.microsoft.com/fwlink/?LinkId=615100) und klicken Sie auf „Registrieren“.
1. Wenn Sie zur Zahlung aufgefordert werden, verwenden Sie den Code, den wir für Sie bereitgestellt haben. Wenn Sie über keinen Code verfügen, wenden Sie sich an [appsourcecissupport@microsoft.com](mailto:appsourcecissupport@microsoft.com?subject=Request%20for%20promotion-code%20for%20DevCenter%20account%20setup).
1. Wählen Sie [Land/Region](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees) in dem/der Sie leben, oder in dem/der sich Ihr Unternehmen befindet aus. **Sie können dies später nicht mehr ändern.**
1. Wählen Sie Ihren [Entwicklerkontotyp](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees) aus. Für AppSource, wählen Sie **Unternehmen** aus. Lesen Sie sich bei einem Unternehmenskonto unbedingt diese [Richtlinien](https://docs.microsoft.com/windows/uwp/publish/opening-a-developer-account) durch.
1. Geben Sie die Kontaktinformationen ein, die Sie für Ihr Entwicklerkonto verwenden möchten.
Eine ausführliche Anleitung zum Einrichten des DevCenter-Kontos finden Sie [hier](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-accounts-creation-registration).

## <a name="provide-info-for-microsoft-sellers"></a>Geben Sie Informationen für Microsoft-Verkäufer an
Einer der zentralen Wertbeiträge von AppSource für Partner liegt darin, mit Microsoft-Verkäufern bei der Positionierung von Partner-Apps vor potenziellen Kunden zusammenzuarbeiten.

Füllen Sie [Partnerlösungsinformationen für Microsoft-Verkäufer](https://aka.ms/aapartnerappinfo) aus, und senden Sie diese an [appsourcecissupport@microsoft.com](mailto:appsourcecissupport@microsoft.com?subject=Request%20publisher%20account%20creation%20for%20%3cPartner%20Name%3e%20and%20whitelist%20owner/contributer%20AAD/MSA%20email%20IDs). Dies ist ein notwendiger Schritt, um eine Genehmigung für die Veröffentlichung einer Cortana Intelligence-App zu erhalten.

## <a name="build-a-compelling-customer-walkthrough-on-appsource"></a>Erstellen Sie auf AppSource eine überzeugende Exemplarische Vorgehensweise für Ihre Kunden
Sehen Sie sich zunächst [Bestandsoptimierung bei Neal Analytics](https://appsource.microsoft.com/en-us/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview&tag=CISHome) auf AppSource an. Jeder App-Eintrag in AppSource verfügt, abgesehen von einem Einstiegspunkt für eine Testbenutzeroberfläche, über Titel, Zusammenfassung (max. 100 Zeichen), Beschreibung (max. 1300 Zeichen), Bilder, Videos (optional) und PDF-Dokumente. Partner sollten all diese Elemente zum Erstellen einer attraktiven Kundenerfahrung wirksam einsetzen.

Partner sollten sich den Inhalt, den Sie auf AppSource stellen, als eine End-to-End-Verkaufsorchestrierung vorstellen. Kunden lesen den Titel und die Beschreibung, um den Wertbeitrag zu erkennen und sehen sich dann Bilder und Videos an, um zu verstehen, worum es in der Lösung geht. Kundenreferenzen helfen potenziellen Kunden dabei zu erfahren, wie andere Kunden davon profitieren. 

All dies sollte das Interesse des Kunden wecken und dazu anregen, mehr erfahren zu wollen. Stellen Sie dies als eine Werbepräsentation vor, durch die ein guter technischer Vertriebsmitarbeiter einen neuen Kunden führen würde. Es wird empfohlen, den Text der Beschreibung in auf Wertbeiträge basierende Unterabschnitte aufzuteilen, die jeweils durch einen Untertitel hervorgehoben werden. 

Besucher werfen in der Regel einen kurzen Blick auf das Feld „Angebotszusammenfassung“ und die Untertitel, um sich einen Überblick darüber zu verschaffen, welches Problem die App anspricht und warum sie die App in Betracht ziehen sollten. Deshalb ist es wichtig, die Aufmerksamkeit der Nutzer zu erhalten und ihnen einen Grund zu bieten, sich die Einzelheiten durchzulesen.

Sehen sie sich an, was andere Partnern gemacht haben.
- [Bestandsoptimierung bei Neal Analytics](https://appsource.microsoft.com/en-us/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview)
- [Plexure Retail Personalization (Einzelhandelpersonalisierung)](https://appsource.microsoft.com/en-us/product/web-apps/plexure.c82dc2fc-817b-487e-ae83-1658c1bc8ff2?tab=Overview)
- [AvePoint Citizen Services (Bürgerdienste)](https://appsource.microsoft.com/en-us/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview)

Der letzte Vorgang des Verkaufs besteht darin, eine Demo der App/Lösung zu zeigen, in welcher deutlich wird, wie der Wertbeitrag übermittelt wird. Genau dafür ist eine Kunden-Testbenutzeroberfläche auf AppSource gedacht. Eine gute Demo berücksichtigt Folgendes:
- Den Wertbeitrag kurz zusammenfassen und die Personen innerhalb der Kundenfirma, die mit der Lösung zu tun hätten, auflisten
- Mithilfe einer Geschichte über einen Beispielkunden, der mit spezifischen Problemen zu tun hat, einen Kontext liefern
- Erläutern, wie die Situation allgemein übertragen werden und Auswirkungen auf Kunden haben kann (vorher) VS was passieren würde, wenn die Lösung verwendet wird. Dies kann mithilfe von Power BI-Dashboards usw. dargestellt werden.
- Fassen Sie mithilfe eines beliebigen spezifischen Machine Learning-Algorithmus usw. zusammen, wie die Lösung dies erreicht

Der Inhalt, der zu AppSource hinzugefügt wird, sollte von hoher Qualität und ausreichend vereinheitlicht sein, um Folgendes zu ermöglichen:
- Die Mitarbeiter des technische Vertriebs des Partners sollten ihn für ihre Vertriebsorchestrierung verwenden. Wenn Ihre Vertriebsteams sie verwendet, ist dies ein gutes Zeichen dafür, dass die MS-Vertriebsmitarbeiter dies auch tun können. Dadurch wird der Ansprechpartner des Kunden in der Lage sein, den Teamkollegen und Vorgesetzten konsistent die gleiche Geschichte weiterzugeben, und so bereits vor einer Kaufverhandlung Budget und Genehmigungen zu erhalten.
- Ein Kunde, der die Website besucht, kann den Inhalt alleine durchgehen und fühlt sich dazu angeregt, sich direkt bei der Partnerkommunikation zu melden, um mit den nächsten Schritten fortfahren zu können.

Daher sollten sich Partner den Inhalt, den sie auf AppSource stellen, als eine End-to-End-Verkaufsorchestrierung vorstellen. Basierend auf der Storyline und den in der Testbenutzeroberfläche dargestellten Funktionen kann über die Art des Angebots entschieden werden.

## <a name="publish-your-app-on-the-publishing-portal"></a>Veröffentlichen der App im Veröffentlichungsportal
Nachdem wir die oben genannten Schritte für Ihre Anwendung ausgewertet haben, erhalten Sie Zugriff auf das Veröffentlichungsportal und erhalten ausführliche Anweisungen zu den nächsten Schritten unter [How to publish a Cortana Intelligence offer via Cloud Partner Portal (So veröffentlichen Sie ein Cortana Intelligence-Angebot über das Cloudpartnerportal)](https://cloudpartner.azure.com/#documentation/cloud-partner-portal-publish-cortana-intelligence-app).

Wenn Sie Informationen zu den Feldern benötigen, senden Sie eine E-Mail an <appsourcecissupport@microsoft.com>.
## <a name="my-app-is-published-on-appsource---now-what"></a>Meine App wird auf AppSource veröffentlicht – was nun?
Zunächst einmal herzlichen Glückwunsch zur Veröffentlichung Ihrer App.
Das Ertragsniveau, das Sie mit der Veröffentlichung Ihrer App auf AppSource erzielen, hängt stark davon ab, wie Sie Ihr Zielpublikum beeinflussen. Weitere Informationen zur Maximierung der Rentabilität finden Sie unter [Growth-Hacking your Cortana Intelligence app on AppSource (Growth Hacking Ihrer Cortana Intelligence-App auf AppSource)](http://aka.ms/aagrowthhackguide).

Wenn Sie Fragen oder Vorschläge haben, kontaktieren Sie uns bitte unter <appsourcecissupport@microsoft.com>.


