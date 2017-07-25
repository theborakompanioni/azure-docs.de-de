---
title: Azure Active Directory-Berichterstellung | Microsoft-Dokumentation
description: "Enthält eine allgemeine Übersicht über die Azure Active Directory-Berichterstellung."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: 738c8f4a56586b87f03973ec258b0a3023affa60
ms.contentlocale: de-de
ms.lasthandoff: 07/17/2017

---
# <a name="azure-active-directory-reporting"></a>Azure Active Directory-Berichterstellung

Mit der Azure Active Directory-Berichterstellung erhalten Sie Einblicke in den Leistungszustand Ihrer Umgebung.  
Die bereitgestellten Daten ermöglichen Ihnen Folgendes:

- Ermitteln, wie Ihre Apps und Dienste von Ihren Benutzern genutzt werden
- Erkennen potenzieller Risiken für die Integrität Ihrer Umgebung
- Behandeln von Problemen, die Ihre Benutzer an der Erledigung ihrer Arbeit hindern  

Die Architektur der Berichterstellung basiert auf zwei Hauptkomponenten:

- Sicherheitsberichte
- Aktivitätsberichte

![Berichterstellung](./media/active-directory-reporting-azure-portal/01.png)



## <a name="security-reports"></a>Sicherheitsberichte

Mit den Sicherheitsberichten in Azure Active Directory können Sie die Identitäten Ihrer Organisation schützen.  
In Azure Active Directory gibt es zwei Arten von Sicherheitsberichten:

- **Benutzer mit Risikomarkierung:** Im [Sicherheitsbericht „Benutzer mit Risikomarkierung“](active-directory-reporting-security-user-at-risk.md) erhalten Sie eine Übersicht über Benutzerkonten, die unter Umständen kompromittiert wurden.

- **Riskante Anmeldungen:** Mit dem [Sicherheitsbericht „Riskante Anmeldungen“](active-directory-reporting-security-risky-sign-ins.md) erhalten Sie einen Indikator für Anmeldeversuche von Benutzern, die nicht der rechtmäßige Besitzer eines Benutzerkontos sind. 

**Welche Azure AD-Lizenz benötigen Sie für den Zugriff auf einen Sicherheitsbericht?**  
Alle Editionen von Azure Active Directory verfügen über die Sicherheitsberichte „Benutzer mit Risikomarkierung“ und „Riskante Anmeldungen“.  
Die Granularitätsebene von Berichten kann für die einzelnen Editionen aber variieren: 

- In den **Free- und Basic-Editionen von Azure Active Directory** erhalten Sie bereits die Listen „Benutzer mit Risikomarkierung“ und „Riskante Anmeldungen“. 

- Mit der Edition **Azure Active Directory Premium 1** wird dieses Modell erweitert, indem Sie zusätzlich jeweils einige zugrunde liegende Risikoereignisse untersuchen können, die für einen Bericht erkannt wurden. 

- In der Edition **Azure Active Directory Premium 2** erhalten Sie die ausführlichsten Informationen zu den zugrunde liegenden Risikoereignissen, und Sie können Sicherheitsrichtlinien konfigurieren, mit denen automatisch auf konfigurierte Risikostufen reagiert wird.


## <a name="activity-reports"></a>Aktivitätsberichte

In Azure Active Directory gibt es zwei Arten von Aktivitätsberichten:

- **Überwachungsprotokolle:** Mit dem [Aktivitätsbericht „Überwachungsprotokolle“](active-directory-reporting-activity-audit-logs.md) erhalten Sie Zugriff auf den Verlauf aller Aufgaben, die in Ihrem Mandanten durchgeführt werden.

- **Anmeldungen:** Mit dem [Aktivitätsbericht „Anmeldungen“](active-directory-reporting-activity-sign-ins.md) können Sie ermitteln, von wem die Aufgaben durchgeführt wurden, die im Bericht „Überwachungsprotokolle“ aufgeführt sind.



Der **Bericht „Überwachungsprotokolle“** enthält Datensätze zu Systemaktivitäten, die für die Konformität relevant sind.
Die bereitgestellten Daten sind beispielsweise in den folgenden häufigen Szenarien hilfreich:

- Ein Benutzer meines Mandanten hat Zugriff auf eine Administratorgruppe erhalten. Wer hat dem Benutzer den Zugriff gewährt? 

- Ich möchte eine Liste mit Benutzern erhalten, die sich an einer bestimmten App angemeldet haben, seitdem ich das App-Onboarding durchgeführt habe, und ich möchte Informationen zur Leistung der App erhalten.

- Ich möchte wissen, wie viele Kennwortzurücksetzungen unter meinem Mandanten durchgeführt werden.


**Welche Azure AD-Lizenz benötigen Sie, um auf den Bericht „Überwachungsprotokolle“ zuzugreifen?**  
Der Bericht „Überwachungsprotokolle“ ist für Features verfügbar, für die Sie über Lizenzen verfügen. Wenn Sie im Besitz einer Lizenz für ein bestimmtes Feature sind, haben Sie auch Zugriff auf die dazugehörigen Informationen zum Überwachungsprotokoll.

Weitere Informationen finden Sie auf der Seite [Azure Active Directory – Features und Funktionen](https://www.microsoft.com/cloud-platform/azure-active-directory-features) unter **Vergleich: Allgemein verfügbare Features der Editionen Free, Basic und Premium**.   



Mit dem **Aktivitätsbericht „Anmeldungen“** können Sie beispielsweise folgende Fragen beantworten:

- Wie sieht das Anmeldemuster eines Benutzers aus?
- Wie viele Benutzer sind für Benutzer im Laufe einer Woche angemeldet?
- Wie lautet der Status dieser Anmeldungen?


**Welche Azure AD-Lizenz ist erforderlich, um auf den Aktivitätsbericht „Anmeldungen“ zuzugreifen?**  
Ihrem Mandanten muss eine Azure AD Premium-Lizenz zugewiesen sein, um auf den Aktivitätsbericht „Anmeldungen“ zugreifen zu können.


## <a name="programmatic-access"></a>Programmgesteuerter Zugriff

Zusätzlich zur Benutzeroberfläche ermöglicht die Azure Active Directory-Berichterstellung Ihnen auch den [programmgesteuerten Zugriff](active-directory-reporting-api-getting-started-azure-portal.md) auf die Berichtsdaten. Die Daten dieser Berichte können für Ihre Anwendungen – beispielsweise SIEM-Systeme, Überwachungs- und Business Intelligence-Tools – sehr nützlich sein. Die Azure AD-Berichterstellungs-APIs bieten über einen Satz von REST-basierten APIs programmgesteuerten Zugriff auf die Daten. Sie können diese APIs über verschiedene Programmiersprachen und Tools aufrufen. 


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den unterschiedlichen Arten von Azure Active Directory-Berichten finden Sie unter:

- [Bericht „Benutzer mit Risikomarkierung“](active-directory-reporting-security-user-at-risk.md)
- [Bericht „Riskante Anmeldungen“](active-directory-reporting-security-risky-sign-ins.md)
- [Bericht „Überwachungsprotokolle“](active-directory-reporting-activity-audit-logs.md)
- [Bericht „Anmeldungen“](active-directory-reporting-activity-sign-ins.md)

Weitere Informationen zum Zugriff auf die Berichtsdaten mit der Berichterstellungs-API finden Sie unter: 

- [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](active-directory-reporting-api-getting-started-azure-portal.md)


<!--Image references-->
[1]: ./media/active-directory-reporting-azure-portal/ic195031.png
