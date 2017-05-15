---
title: "Schützen von Kennwörtern in Azure AD und Zurücksetzen von Kennwörtern, die per Smart Password Lockout blockiert werden | Microsoft-Dokumentation"
description: "Hier wird erklärt, was ein Azure AD-Mandant ist und wie Azure über Azure Active Directory verwaltet wird."
services: active-directory
documentationcenter: 
author: markvi
writer: v-lorisc
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/02/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 910ca62c331df0825e1403c19fe2f53672a7359d
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="secure-passwords--in-azure-ad-and-reset-passwords-that-get-blocked-by-smart-password-lockout"></a>Schützen von Kennwörtern in Azure AD und Zurücksetzen von Kennwörtern, die per Smart Password Lockout blockiert werden
In diesem Artikel werden die bewährten Methoden beschrieben, die Sie als Benutzer oder Administrator verwenden können, um Ihre Azure Active Directory- (Azure AD) und Microsoft-Kontodienst-Konten zu schützen. 

 >[!NOTE]
 >Azure AD-Administratoren können Benutzerkennwörter zurücksetzen, indem sie auf den Verzeichnisnamen klicken. Wählen Sie im [Azure-Verwaltungsportal](https://manage.windowsazure.com) die Seite „Benutzer“, und klicken Sie auf den Namen des Benutzers und dann auf „Kennwort zurücksetzen“. 
 >

Azure AD verfügt über die folgenden gängigen Ansätze zum Schützen von Kennwörtern:
 *    Anforderungen an die Kennwortlänge
 *    Anforderungen an die „Komplexität“ des Kennworts
 *    Regelmäßiger bzw. periodischer Kennwortablauf 

Informationen zu den Funktionen zur Kennwortverwaltung finden Sie unter [Verwalten von Kennwörtern in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-manage-passwords). 

## <a name="azure-ad-password-protection"></a>Kennwortschutz für Azure AD
Für Azure AD und das Microsoft-Kontosystem werden in der Branche bewährte Ansätze verwendet, um den Schutz von Benutzer- und Administratorkennwörtern sicherzustellen. 

In diesem Abschnitt wird beschrieben, wie Kennwörter in Azure AD mit den folgenden Methoden geschützt werden:
 *    Dynamisch gesperrte Kennwörter
 *    Intelligente Kennwortsperrung

Informationen zur Kennwortverwaltung nach dem neuesten Forschungsstand finden Sie im Whitepaper [Password Guidance](http://aka.ms/passwordguidance) (Anleitung für Kennwörter). 

### <a name="dynamically-banned-passwords"></a>Dynamisch gesperrte Kennwörter
In Azure AD und im Microsoft-Kontosystem werden Kennwörter geschützt, indem alle häufig verwendeten Kennwörter dynamisch gesperrt werden. Das Azure AD Identity Protection-Team analysiert die Liste mit den gesperrten Kennwörtern routinemäßig, um zu verhindern, dass Benutzer häufig verwendete Kennwörter wählen. Dieser Dienst ist für Kunden von Azure AD und des Microsoft-Kontodiensts verfügbar. 

Beim Erstellen von Kennwörtern ist es wichtig, dass Administratoren die Benutzer zur Wahl von ungewöhnlichen Kennwörtern ermutigen, die eine eindeutige Kombination aus Buchstaben, Zahlen und Zeichen enthalten. So kann erreicht werden, dass eine Kompromittierung von Benutzerkennwörtern nahezu unmöglich ist. 

**Listen zu Verletzungen**

Bei Azure AD arbeiten wir ununterbrochen daran, den Cyberkriminellen immer einen Schritt voraus zu sein. Eine Möglichkeit besteht hierbei darin, dass Benutzer an der Erstellung von Kennwörtern gehindert werden, die in der Liste mit den aktuellen Angriffen aufgeführt sind.

Das Azure AD Identity Protection-Team analysiert ständig, welche Kennwörter häufig verwendet werden. Auch Cyberkriminelle nutzen ähnliche Strategien, um die Chancen bei ihren Angriffen zu erhöhen, z.B. durch die Erstellung einer [Rainbow Table](https://en.wikipedia.org/wiki/Rainbow_table) (Regenbogentabelle) zum Cracken von Kennworthashes. 

Microsoft führt ständig Analysen in Bezug auf [Datenverletzungen](https://www.privacyrights.org/data-breaches) durch, um eine Liste mit gesperrten Kennwörtern zu führen, die dynamisch aktualisiert wird. So wird sichergestellt, dass anfällige Kennwörter gesperrt werden, bevor sie für Azure AD-Kunden zu einer echten Bedrohung werden. Weitere Informationen zu unserer derzeitigen Arbeit in Bezug auf Sicherheitsmaßnahmen finden Sie im [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx). 

### <a name="smart-password-lockout"></a>Intelligente Kennwortsperrung

Wenn Azure AD einen potenziellen Cyberkriminellen erkennt, der versucht, ein Benutzerkennwort zu hacken, wird das Benutzerkonto per Smart Password Lockout gesperrt. Azure AD ist dafür ausgelegt, das Risiko zu ermitteln, das mit bestimmten Anmeldesitzungen verbunden ist. 

Indem wir die aktuellsten Sicherheitsdaten verwenden, wenden wir Sperrsemantiken auf Cyberbedrohungen an. Auf diese Weise werden Benutzer nicht ausgeschlossen, wenn ein Cyberkrimineller Benutzerkennwörter im Netzwerk gehackt hat.

Wenn ein Benutzer aus Azure AD ausgeschlossen wird, wird auf dem Bildschirm etwa Folgendes angezeigt:

  ![Ausschluss aus Azure AD](./media/active-directory-secure-passwords/locked-out-azuread.png)
  
Für andere Microsoft-Konten sieht der Bildschirm in etwa wie folgt aus:

  ![Ausschluss aus einem Microsoft-Konto](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

Informationen zur Kennwortverwaltung in Azure Active Directory finden Sie unter [Funktionsweise der Kennwortverwaltung](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-how-it-works).

  >[!NOTE]
  >Wenn Sie Azure AD-Administrator sind, ist die Verwendung von [Windows Hello](https://www.microsoft.com/en-us/windows/windows-hello) ratsam, um ganz zu verhindern, dass Ihre Benutzer herkömmliche Kennwörter erstellen müssen.
  >

## <a name="next-steps"></a>Nächste Schritte
[Aktualisieren Ihres eigenen Kennworts](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-update-your-own-password)<br>
[Grundlagen der Identitätsverwaltung in Azure](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity)<br>
[Operative Einblicke durch Berichte zur Kennwortverwaltung](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-get-insights#view-password-reset-activity)



