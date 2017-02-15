---
title: Konfigurieren von Sicherheitswarnungen | Microsoft Docs
description: "Erfahren Sie, wie Sie Sicherheitswarnungen für die Erweiterung Azure Privileged Identity Management konfigurieren."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 4e0c911a-36c6-42a0-8f79-a01c03d2d04f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: dd8987a05c71643b49e0ac9e3c8ae43d47743225


---
# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Konfigurieren von Sicherheitswarnungen in Azure AD Privileged Identity Management
## <a name="security-alerts"></a>Sicherheitswarnungen
Azure Privileged Identity Management (PIM) generiert bei verdächtigen oder nicht sicheren Aktivitäten in Ihrer Umgebung Warnungen. Wenn eine Warnung ausgelöst wird, wird sie im PIM-Dashboard angezeigt. Wählen Sie die Warnung aus, um einen Bericht mit allen Benutzern oder Rollen anzuzeigen, die die Warnung ausgelöst haben.

![Sicherheitswarnungen im PIM-Dashboard – Screenshot][1]

| Warnung | Trigger | Empfehlung |
| --- | --- | --- |
| **Rollen werden außerhalb von PIM zugewiesen** |Ein Administrator wurde außerhalb der PIM-Benutzeroberfläche permanent zu einer Rolle zugewiesen. |Überprüfen Sie die neue Rollenzuweisung. Da andere Dienste nur permanente Administratoren zuweisen können, ändern Sie die Zuweisung bei Bedarf in eine berechtigte Zuweisung. |
| **Rollen werden zu häufig aktiviert** |Es sind zu viele erneute Aktivierungen derselben Rolle innerhalb des in den Einstellungen festgelegten Zeitraums erfolgt. |Wenden Sie sich an den Benutzer, um festzustellen, warum die Rolle so häufig aktiviert wurde. Möglicherweise ist der Zeitraum zu kurz, um die Aufgaben abzuschließen, oder Benutzer verwenden Skripts, um eine Rolle automatisch zu aktivieren. |
| **Rollen erfordern für die Aktivierung keine Multi-Factor Authentication** |Bei manchen Rollen ist MFA in den Einstellungen nicht aktiviert. |Wir fordern MFA für die meisten sehr privilegierten Rollen an, empfehlen jedoch dringend, dass Sie MFA für die Aktivierung aller Rollen aktivieren. |
| **Administratoren verwenden ihre privilegierten Rollen nicht** |Es gibt berechtigte Administratoren, die ihre Rollen längere Zeit nicht aktiviert haben. |Starten Sie eine Zugriffsüberprüfung, um die Benutzer zu ermitteln, die keinen Zugriff mehr benötigen. |
| **Es gibt zu viele globale Administratoren** |In den Einstellungen sind mehr globale Administratoren festgelegt als empfohlen. |Wenn Sie eine große Anzahl von globalen Administratoren haben, ist es wahrscheinlich, dass Benutzer mehr Berechtigungen als notwendig erhalten. Verlagern Sie Benutzer in weniger privilegierte Rollen, oder legen Sie einige von ihnen als für die Rolle berechtigt fest, statt sie permanent zuzuweisen. |

## <a name="configure-security-alert-settings"></a>Konfigurieren der Einstellungen für Sicherheitswarnungen
Sie können einige der Sicherheitswarnungen in PIM so anpassen, dass sie in der Umgebung und mit den Sicherheitszielen genutzt werden können. Gehen Sie wie folgt vor, um das Blatt mit den Einstellungen aufzurufen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und wählen Sie auf dem Dashboard die Kachel **Azure AD Privileged Identity Management** aus.
2. Wählen Sie **Privilegierte Rollen verwalten** > **Einstellungen** > **Warnungseinstellungen** aus.
   
    ![Navigieren zu den Einstellungen für Sicherheitswarnungen][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>Warnung „Rollen werden zu häufig aktiviert“
Diese Warnung wird ausgelöst, wenn ein Benutzer mehrmals innerhalb eines bestimmten Zeitraums dieselbe privilegierte Rolle aktiviert. Sie können den Zeitraum und die Anzahl der Aktivierungen konfigurieren.

* **Zeitrahmen für Aktivierungsverlängerung**: Geben Sie in Tagen, Stunden, Minuten und Sekunden den Zeitraum an, den Sie verwenden möchten, um verdächtige Erneuerungen nachzuverfolgen.
* **Anzahl von Aktivierungsverlängerungen**: Geben Sie für den gewählten Zeitraum eine Anzahl von Aktivierungen zwischen 2 und 100 an, für die Sie eine Warnung ausgeben möchten. Sie können diese Einstellung mithilfe des Schiebereglers oder durch Eingabe einer Zahl in das Textfeld ändern.

### <a name="there-are-too-many-global-administrators-alert"></a>Warnung „Es gibt zu viele globale Administratoren“
In PIM wird diese Warnung ausgelöst, wenn zwei verschiedene Kriterien erfüllt sind, und Sie können beide konfigurieren. Erstens müssen Sie einen bestimmten Schwellenwert an globalen Administratoren erreichen. Zweitens muss ein bestimmter Anteil aller Rollenzuweisungen globale Administratoren betreffen. Wenn nur eines dieser Kriterien erfüllt ist, wird die Warnung nicht angezeigt.  

* **Mindestanzahl von globalen Administratoren**: Geben Sie eine Anzahl von globalen Administratoren zwischen 2 und 100 an, die Sie als eine unsichere Anzahl betrachten.
* **Prozentsatz von globalen Administratoren**: Geben Sie den Prozentsatz von Administratoren, bei denen es sich um globale Administratoren handelt, zwischen 0 % und 100 % an, der in Ihrer Umgebung als unsicher gilt.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>Warnung „Administratoren verwenden ihre privilegierten Rollen nicht“
Diese Warnung wird auslöst, wenn ein Benutzer für eine bestimmte Zeit eine Rolle nicht aktiviert hat.

* **Anzahl Tage**: Geben Sie eine Anzahl von Tagen zwischen 0 und 100 an, die ein Benutzer eine Rolle nicht aktivieren muss.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png



<!--HONumber=Dec16_HO4-->


