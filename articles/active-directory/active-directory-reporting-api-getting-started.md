---
title: Erste Schritte mit der Azure AD-Berichterstellungs-API | Microsoft Docs
description: Vorgehensweise zum Einstieg in die Azure Active Directory-Berichterstellungs-API
services: active-directory
documentationcenter: ''
author: dhanyahk
manager: femila
editor: ''

ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2016
ms.author: dhanyahk;markvi

---
# Erste Schritte mit der Berichterstellungs-API von Azure Active Directory
*Diese Dokumentation ist Teil des [Leitfadens zur Azure Active Directory-Berichterstellung](active-directory-reporting-guide.md).*

Azure Active Directory stellt eine Vielzahl von Berichten zur Verfügung. Die Daten dieser Berichte können für Ihre Anwendungen – beispielsweise SIEM-Systeme, Überwachungs- und Business Intelligence-Tools – sehr nützlich sein. Die Azure AD-Berichterstellungs-APIs bieten über einen Satz von REST-basierten APIs programmgesteuerten Zugriff auf die Daten. Sie können diese APIs über verschiedene Programmiersprachen und Tools aufrufen.

In diesem Artikel erhalten Sie die Informationen, die Sie zum Einstieg in die Azure AD-Berichterstellungs-APIs benötigen. Im nächsten Abschnitt finden Sie ausführlichere Informationen zur Verwendung der APIs für Überwachung und Anmeldung. Alle weiteren APIs werden im Artikel [Azure AD-Berichte und -Ereignisse (Vorschau)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) behandelt.

Bei Fragen, Problemen oder zum Senden von Feedback wenden Sie sich an das [Hilfeteam für die AAD-Berichterstellung](mailto:aadreportinghelp@microsoft.com).

## Lernschritte
1. **Vorbereiten**: Damit Sie die Beispielen in diesem Thema verwenden können, müssen die [Voraussetzungen zum Zugriff auf die Azure AD-Berichterstellungs-API](active-directory-reporting-api-prerequisites.md) erfüllt sein.
2. **Untersuchen**: Verschaffen Sie sich einen ersten Eindruck der Berichterstellungs-APIs:
   
   * [Verwenden der Beispiele für die Überwachungs-API](active-directory-reporting-api-audit-samples.md)
   * [Verwenden der Beispiele für die Anmeldeaktivitätsbericht-API](active-directory-reporting-api-sign-in-activity-samples.md)
3. **Anpassen**: Erstellen Sie Ihre eigene Lösung:
   
   * [Verwenden der Referenz zur Überwachungs-API](active-directory-reporting-api-audit-reference.md)
   * [Verwenden der Referenz zur Anmeldeaktivitätsbericht-API](active-directory-reporting-api-sign-in-activity-reference.md)

## Nächste Schritte
Wenn Sie alle verfügbaren Azure AD Graph-API-Endpunkte anzeigen möchten, navigieren Sie zu dieser Adresse: [https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).

<!---HONumber=AcomDC_0928_2016-->