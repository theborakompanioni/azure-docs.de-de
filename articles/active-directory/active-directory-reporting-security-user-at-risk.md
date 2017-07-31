---
title: "Sicherheitsbericht „Gefährdete Benutzer“ im Azure Active Directory-Portal | Microsoft-Dokumentation"
description: "Enthält Informationen zum Sicherheitsbericht „Gefährdete Benutzer“ im Azure Active Directory-Portal."
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 01ecb98c02b2a01007c7f76805d4db4b7aeee1f0
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---
# <a name="users-at-risk-security-report-in-the-azure-active-directory-portal"></a>Sicherheitsbericht „Gefährdete Benutzer“ im Azure Active Directory-Portal

Mit den Sicherheitsberichten in Azure Active Directory (Azure AD) erhalten Sie Einblicke in die Wahrscheinlichkeit für kompromittierte Benutzerkonten in Ihrer Umgebung. 

Azure Active Directory erkennt verdächtige Aktionen im Zusammenhang mit Ihren Benutzerkonten. Für jede erkannte Aktion wird ein Datensatz mit der Bezeichnung *Risikoereignis* erstellt. Weitere Details finden Sie unter [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md) (Azure Active Directory-Risikoereignisse). 

Die erkannten Risikoereignisse werden zum Berechnen folgender Werte verwendet:

- **Riskante Anmeldungen:** Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist. Weitere Informationen finden Sie unter [Riskante Anmeldungen](active-directory-identityprotection.md#risky-sign-ins). 

- **Benutzer mit Risikomarkierung:** Ein Benutzer mit Risikomarkierung ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto. Weitere Informationen finden Sie unter [Benutzer mit Risikomarkierung](active-directory-identityprotection.md#users-flagged-for-risk).  

Im Azure-Portal befinden sich die Sicherheitsberichte auf dem Blatt **Azure Active Directory** im Abschnitt **Sicherheit**.  

![Riskante Anmeldungen](./media/active-directory-reporting-security-user-at-risk/10.png)

## <a name="azure-active-directory-free-and-basic-edition"></a>Azure Active Directory – Free und Basic Edition

Der Bericht „Gefährdete Benutzer“ in den Editionen Free und Basic von Azure Active Directory enthält eine Liste mit Benutzerkonten, die unter Umständen kompromittiert wurden. 


![Riskante Anmeldungen](./media/active-directory-reporting-security-user-at-risk/03.png)

Wenn Sie einen Benutzer auswählen, wird das entsprechende Blatt mit den Benutzerdaten geöffnet.
Sie können für gefährdete Benutzer den Anmeldeverlauf des Benutzers prüfen und bei Bedarf das Kennwort zurücksetzen.

![Riskante Anmeldungen](./media/active-directory-reporting-security-user-at-risk/46.png)

## <a name="azure-active-directory-premium-editions"></a>Azure Active Directory – Premium Editionen

Der Bericht „Gefährdete Benutzer“ in den Premium Editionen von Azure Active Directory enthält Folgendes:

- Eine [Liste mit Benutzerkonten](active-directory-identityprotection.md#users-flagged-for-risk), die unter Umständen kompromittiert wurden 

- Aggregierte Informationen zu den erkannten [Risikoereignistypen](active-directory-identity-protection-risk-events.md)

- Option zum Herunterladen des Berichts

- Option zum Konfigurieren einer [Richtlinie zum Beheben des Benutzerrisikos](active-directory-identityprotection.md#user-risk-security-policy)  


![Riskante Anmeldungen](./media/active-directory-reporting-security-user-at-risk/71.png)

Wenn Sie einen Benutzer auswählen, erhalten Sie eine ausführliche Berichtsansicht für diesen Benutzer mit folgenden Optionen:

- Öffnen der Ansicht „Alle Anmeldungen“

- Das Kennwort des Benutzers zurücksetzen

- Verwerfen aller Ereignisse

- Untersuchen der gemeldeten Risikoereignisse für den Benutzer 


![Riskante Anmeldungen](./media/active-directory-reporting-security-user-at-risk/324.png)


Wählen Sie in der Liste ein Risikoereignis aus, um es zu untersuchen.  
Das Blatt **Details** wird für das Risikoereignis geöffnet. Auf dem Blatt **Details** können Sie ein [Risikoereignis manuell schließen](active-directory-identityprotection.md#closing-risk-events-manually) oder ein manuell geschlossenes Risikoereignis wieder aktivieren. 


![Riskante Anmeldungen](./media/active-directory-reporting-security-user-at-risk/325.png)



## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure Active Directory Identity Protection finden Sie unter [Azure Active Directory Identity Protection](active-directory-identityprotection.md).


