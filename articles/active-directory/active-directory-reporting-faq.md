---
title: "Häufig gestellte Fragen zu Azure Active Directory-Berichten | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu Azure Active Directory-Berichten."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: accf292f70bf0eafdefc00c3feeaf8e346605401
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="azure-active-directory-reporting-faq"></a>Häufig gestellte Fragen zu Azure Active Directory-Berichten

Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Azure Active Directory-Berichten.  
Weitere Informationen finden Sie unter [Azure Active Directory-Berichte](active-directory-reporting-azure-portal.md). 

**F: Wie lange werden Aktivitätsprotokolldaten (Überwachung und Anmeldevorgänge) im Azure-Portal aufbewahrt?** 

**A:** 7 Tage für unsere Kunden im Tarif „Free“. Nach einem Wechsel zu einer Azure AD Premium 1- oder Premium 2-Lizenz können Sie bis zu 30 Tage auf Daten zugreifen. Weitere Informationen zur Aufbewahrung finden Sie unter [Aufbewahrungsrichtlinien für Azure Active Directory-Berichte](active-directory-reporting-retention.md).

--- 

**F: Wie lange dauert es, bis nach Abschluss einer Aufgabe Aktivitätsdaten angezeigt werden?**

**A:** Für Aktivitätsüberwachungsprotokolle fällt eine Latenz von 15 Minuten bis zu einer Stunde an. Für Aktivitätsprotokolle für Anmeldevorgänge fällt bei den meisten Datensätzen eine Latenz von 15 Minuten und bei einigen wenigen Datensätzen von bis zu 2 Stunden an.

---

**F: Muss ich ein globaler Administrator sein, um die Aktivitätsprotokolle im Azure-Portal anzeigen oder die Daten über die API abzurufen?**

**A:** Nein. Sie müssen entweder ein **Benutzer mit Leseberechtigung für Sicherheitsfunktionen**, **Sicherheitsadministrator** oder **globaler Administrator** sein, um Berichtsdaten im Azure-Portal anzeigen oder über die API darauf zugreifen zu können.

---

**F: Kann ich Office 365-Aktivitätsprotokollinformationen über das Azure Portal abrufen?**

**A:** Obwohl Office 365- und Azure AD-Aktivitätsprotokolle einen Großteil der Verzeichnisressourcen gemeinsam nutzen, müssen Sie, wenn Sie eine vollständige Ansicht der Office 365-Aktivitätsprotokolle wünschen, das Office 365 Admin Center besuchen, um Office 365-Aktivitätsprotokollinformationen abzurufen.

---


**F: Mit welchen APIs kann ich Informationen zu Office 365-Aktivitätsprotokollen abrufen?**

**A:** Über die Office 365-Verwaltungs-APIs können Sie auf die [Office 365-Aktivitätsprotokolle](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview) zugreifen.

---

**F: Wie viele Datensätze kann ich aus dem Azure-Portal herunterladen?**

**A:** Sie können bis zu 120.000 Datensätze aus dem Azure-Portal herunterladen. Die Datensätze werden nach *Aktualität* sortiert, und standardmäßig erhalten Sie die neuesten 120.000 Datensätze. 

---

**F: Wie viele Datensätze kann ich über die Aktivitäten-API abfragen?**

**A:** Sie können bis zu 1 Million Datensätze abfragen (wenn Sie nicht den Operator verwenden, der die Datensätze nach Aktualität sortiert). Wenn Sie diesen Operator verwenden, können Sie bis zu 500.000 Datensätze abfragen. [Hier](active-directory-reporting-api-getting-started.md) finden Sie Beispielabfragen, die die Verwendung der API veranschaulichen.

---

**F: Wie erhalte ich eine Premium-Lizenz?**

**A:** Die Antwort auf diese Frage finden Sie unter [Erste Schritte mit Azure Active Directory Premium](active-directory-get-started-premium.md).

---

**F: Wie schnell werden nach Erwerb einer Premium-Lizenz Daten zu Aktivitäten angezeigt?**

**A:** Wenn Ihnen bereits mit einer Lizenz im Tarif „Free“ Daten zu Aktivitäten angezeigt werden, sehen Sie dieselben Daten. Wenn Sie noch keine Daten haben, dauert es ein bis zwei Tage.

---

**F:Werden nach Erwerb einer Azure AD-Premium-Lizenz die Daten des letzten Monats angezeigt?**

**A:** Wenn Sie vor Kurzem zu einer Premium-Version (einschließlich einer Testversion) gewechselt sind, werden Ihnen anfänglich Daten von bis zu 7 Tagen angezeigt. Sobald sich Daten angesammelt haben, werden Daten von bis zu 30 Tagen angezeigt.

---

**F: Es gibt ein Risikoereignis in Identity Protection, aber ich sehe keine entsprechende Anmeldung unter „Alle Anmeldungen“. Entspricht dies dem erwarteten Verhalten?**

**A:** Ja, Identity Protection beurteilt das Risiko für alle Authentifizierungsflows, egal ob diese interaktiv oder nicht interaktiv sind. „Alle Anmeldungen“ führt jedoch nur die interaktiven Anmeldungen auf.

---

**F: Wie kann ich den Bericht „Benutzer mit Risikokennzeichnung“ im Azure-Portal herunterladen?**

**A:** Die Möglichkeit zum Herunterladen des Berichts *Benutzer mit Risikokennzeichnung* wird bald hinzugefügt.

---

**F: Wie erfahre ich, warum eine Anmeldung oder ein Benutzer im Azure-Portal als riskant gekennzeichnet wurde?**

**A:** Kunden der Premium Edition können mehr über die zugrunde liegenden Risikoereignisse erfahren, indem Sie auf den Benutzer unter „Benutzer mit Risikokennzeichnung“ oder auf „riskante Anmeldevorgänge“ klicken. Kunden der kostenlosen und der Basic Edition können die riskanten Benutzer und Anmeldungen ohne die zugrunde liegenden Risikoereignisse sehen.

---

**F: Wie werden IP-Adressen im Bericht zu Anmeldungen und riskanten Anmeldungen berechnet?**

**A:** IP-Adressen werden so ausgestellt, dass es keine definitive Verbindung zwischen einer IP-Adresse und dem physischen Standort des Computers mit dieser Adresse gibt. Dies wird durch Faktoren wie Mobilfunkanbieter und VPNs verkompliziert, die IP-Adressen aus zentralen Pools zuweisen, die häufig sehr weit von den Orten entfernt sind, an denen das Clientgerät tatsächlich verwendet wird. Bei Berücksichtigung dieser Vorgaben erfolgt das Konvertieren einer IP-Adresse in einen physischen Standort basierend auf Ablaufverfolgungen, Registrierungsdaten, Reverse-Lookups und anderen Informationen. 

---

