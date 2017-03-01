---
title: Erste Schritte mit Azure Active Directory-Berichten | Microsoft Docs
description: "Listet die verschiedenen verfügbaren Berichte in Azure Active Directory Reporting auf"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 7ac99919-8df5-4424-9298-fc7c025ba949
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/07/2016
ms.author: dhanyahk;markvi
translationtype: Human Translation
ms.sourcegitcommit: eec9b73cbaccfa50eec6f237e4d1d810c6efa1d9
ms.openlocfilehash: e5b8ac91914203156bd395d7f462385e9f6dbcb4
ms.lasthandoff: 02/24/2017


---
# <a name="getting-started-with-azure-active-directory-reporting"></a>Erste Schritte mit Azure Active Directory-Berichten
## <a name="what-it-is"></a>Funktionsbeschreibung
Azure Active Directory (Azure AD) umfasst Sicherheits-, Aktivitäts- und Prüfberichte für Ihr Verzeichnis. Hier sehen Sie eine Liste der enthaltenen Berichte:

### <a name="security-reports"></a>Sicherheitsberichte
* Anmeldungen von unbekannten Quellen
* Anmeldungen nach mehreren Fehlern
* Anmeldungen aus mehreren geografischen Regionen
* Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten
* Irreguläre Anmeldeaktivitäten
* Anmeldungen von möglicherweise infizierten Geräten
* Benutzer mit anomalen Anmeldeaktivitäten

### <a name="activity-reports"></a>Aktivitätsberichte
* Anwendungsnutzung: Zusammenfassung
* Anwendungsnutzung: detailliert
* Anwendungsdashboard
* Kontobereitstellungsfehler
* Geräte einzelner Benutzer
* Aktivität einzelner Benutzer
* Gruppenaktivitätsbericht
* Bericht zur Registrierung für die Kennwortzurücksetzung
* Kennwortzurücksetzungsaktivität

### <a name="audit-reports"></a>Prüfberichte
* Verzeichnisprüfbericht

> [!TIP]
> Weitere Dokumentationen zu Azure AD Reporting finden Sie unter [Anzeigen von Zugriffs- und Nutzungsberichten](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="how-it-works"></a>So funktioniert's
### <a name="reporting-pipeline"></a>Berichterstellungspipeline
Die Berichterstellungspipeline umfasst drei wesentliche Schritte. Jedes Mal, wenn sich ein Benutzer anmeldet oder eine Authentifizierung erfolgt, geschieht Folgendes:

* Erstens wird der Benutzer authentifiziert (erfolgreich oder nicht erfolgreich), und das Ergebnis wird in den Azure Active Directory-Dienstdatenbanken gespeichert.
* In regelmäßigen Abständen werden alle aktuellen Anmeldungen verarbeitet. Dabei durchsuchen Algorithmen für Sicherheit und anomale Aktivität alle aktuellen Anmeldungen nach verdächtigen Aktivitäten.
* Nach der Verarbeitung werden die Berichte geschrieben, zwischengespeichert und im klassischen Azure-Portal bereitgestellt.

### <a name="report-generation-times"></a>Berichterstellungszeiten
Aufgrund der großen Anzahl von Authentifizierungen und Anmeldungen, die von der Azure AD-Plattform verarbeitet werden, sind die aktuellsten verarbeiteten Anmeldungen im Durchschnitt eine Stunde alt. In seltenen Fällen kann es bis zu 8 Stunden dauern, die letzten Anmeldungen zu verarbeiten.

Sie finden die aktuellste verarbeitete Anmeldung im Hilfetext am oberen Rand jedes Berichts.

![Hilfetext am oberen Rand jedes Berichts](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [!TIP]
> Weitere Dokumentationen zu Azure AD Reporting finden Sie unter [Anzeigen von Zugriffs- und Nutzungsberichten](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="getting-started"></a>Erste Schritte
### <a name="sign-into-the-azure-classic-portal"></a>Melden Sie sich beim klassischen Azure-Portal an.
Zunächst müssen Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) als globaler oder Kompatibilitätsadministrator anmelden. Sie müssen außerdem ein Azure-Abonnementdienstadministrator oder -Co-Administrator sein, bzw. das Azure-Abonnement "Zugriff auf Azure AD" benutzen.

### <a name="navigate-to-reports"></a>Navigieren zu Berichten
Um Berichte anzuzeigen, navigieren Sie zur Registerkarte "Berichte" am Anfang des Verzeichnisses.

Wenn Sie zum ersten Mal Berichte anzeigen, müssen Sie ein Dialogfeld bestätigen, bevor Sie die Berichte anzeigen können. So wird sichergestellt, dass die Administratoren in Ihrer Organisation das Anzeigen dieser Daten akzeptieren, die in einigen Ländern als private Informationen gelten könnten.

![Dialogfeld](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Untersuchen der einzelnen Berichte
Navigieren Sie zu jedem Bericht, um die erfassten Daten und die verarbeiteten Anmeldungen zu sehen. Sie finden [hier eine Liste aller Berichte](active-directory-reporting-guide.md).

![Alle Berichte](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>Herunterladen der Berichte als CSV-Datei
Jeder Bericht kann als CSV-Datei (durch Trennzeichen getrennte Werte) heruntergeladen werden. Sie können diese Dateien in Excel, PowerBI oder Analyseprogrammen von Drittanbietern öffnen, um Ihre Daten näher zu analysieren.

Um einen Bericht im CSV-Format herunterzuladen, navigieren Sie zum Bericht, und klicken Sie unten auf "Herunterladen".

![Schaltfläche "Herunterladen"](./media/active-directory-reporting-getting-started/downloadButton.png)

> [!TIP]
> Weitere Dokumentationen zu Azure AD Reporting finden Sie unter [Anzeigen von Zugriffs- und Nutzungsberichten](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="next-steps"></a>Nächste Schritte
### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Anpassen von Warnungen bei anomaler Anmeldungsaktivität
Wechseln Sie zur Registerkarte "Konfigurieren" Ihres Verzeichnisses.

Führen Sie einen Bildlauf zum Abschnitt "Benachrichtigungen" aus.

Aktivieren oder deaktivieren Sie den Bereich „E-Mail-Benachrichtigungen über anomale Anmeldungen“.

![Abschnitt "Benachrichtigungen"](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>Integrieren in die Azure AD Reporting-API
Informationen hierzu finden Sie unter [Erste Schritte mit der Azure AD Reporting-API](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Einbinden der mehrstufigen Authentifizierung von Benutzern
Wählen Sie einen Benutzer in einem Bericht aus.

Klicken Sie am unteren Bildschirmrand auf die Schaltfläche "MFA aktivieren".

![Schaltfläche "Multi-Factor Authentication" am unteren Bildschirmrand](./media/active-directory-reporting-getting-started/mfaButton.png)

> [!TIP]
> Weitere Dokumentationen zu Azure AD Reporting finden Sie unter [Anzeigen von Zugriffs- und Nutzungsberichten](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="learn-more"></a>Weitere Informationen
### <a name="audit-events"></a>Überwachen von Ereignissen
Informationen dazu, welche Ereignisse im Verzeichnis überwacht werden, finden Sie unter [Azure Active Directory-Überwachungsberichtsereignisse](active-directory-reporting-audit-events.md).

### <a name="api-integration"></a>API-Integration
Informationen hierzu finden Sie unter [Erste Schritte mit der Azure AD Reporting-API](active-directory-reporting-api-getting-started.md) und in der [API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Setzen Sie sich mit uns in Verbindung
Unter der E-Mail-Adresse [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) erhalten Sie Feedback, Hilfe und Antworten auf Ihre Fragen.

> [!TIP]
> Weitere Dokumentationen zu Azure AD Reporting finden Sie unter [Anzeigen von Zugriffs- und Nutzungsberichten](active-directory-view-access-usage-reports.md).
> 
> 


