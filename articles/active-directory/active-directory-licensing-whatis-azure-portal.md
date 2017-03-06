---

title: Was ist die gruppenbasierte Lizenzierung in Azure Active Directory? | Microsoft-Dokumentation
description: "Beschreibung des gruppenbasierten Lizenzierung in Azure Active Directory, ihrer Funktionsweise, der ersten Schritte und bewährten Methoden"
services: active-directory
keywords: "Azure AD-Lizenzierung"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/21/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 6a9cebafd1ad8f513bfab897970241f7b82b2a53
ms.openlocfilehash: 1f4e0904c9025ce0b21d904acc7b959e823039ef
ms.lasthandoff: 02/22/2017


---

# <a name="what-is-group-based-licensing-in-azure-active-directory"></a>Was ist die gruppenbasierte Lizenzierung in Azure Active Directory?

Microsoft-Clouddienste wie Office 365, Enterprise Mobility + Security, Dynamics CRM und andere ähnliche Produkte erfordern Lizenzen, die jedem Benutzer zugewiesen werden müssen, der Zugriff auf diese Dienste benötigt. Die Lizenzverwaltung führen Administratoren über eines der Verwaltungsportale (Office, Azure) und PowerShell-Cmdlets durch. Der Lizenzzuweisungsstatus wird in Azure Active Directory gespeichert, der zugrunde liegenden Infrastruktur für die Identitätsverwaltung aller Microsoft-Clouddienste.

Bislang konnten Lizenzen nur auf Ebene einzelner Benutzer zugewiesen werden, was die umfassende Verwaltung für unsere Kunden erschweren kann. Um Benutzerlizenzen basierend auf Organisationsänderungen zu vergeben oder zu entziehen, wenn beispielsweise Benutzer der Organisation oder Abteilung beitreten oder diese verlassen, muss ein Administrator häufig ein komplexes PowerShell-Skript schreiben, um einzelne Aufrufe an den Clouddienst zu richten.

Um diese Aufgaben zu vereinfachen, haben wir in das Azure AD-Lizenzverwaltungssystem ein neues Feature eingeführt, die gruppenbasierte Lizenzierung. Es ist nun möglich, einer Gruppe eine oder mehrere Produktlizenzen zuzuweisen. Azure AD stellt sicher, dass die Lizenzen allen Mitgliedern der Gruppe zugewiesen werden. Alle neuen Mitglieder, die der Gruppe beitreten, werden die entsprechenden Lizenzen zugewiesen. Wenn sie die Gruppe später verlassen sollten, werden diese Lizenzen entzogen. Dadurch ist keine automatisierte Lizenzverwaltung über PowerShell mehr erforderlich, um Änderungen in der Organisations- und Abteilungsstruktur benutzerbezogen widerzuspiegeln.

## <a name="features"></a>Features

Es folgen die Hauptmerkmale der gruppenbasierten Lizenzierung:

- Lizenzen können beliebigen Sicherheitsgruppen in Azure AD zugewiesen werden. Sicherheitsgruppen können lokal mithilfe von Azure AD Connect synchronisiert, in Azure AD (auch als reine Cloudgruppen) direkt erstellt oder automatisch über das Azure AD-Feature „Dynamische Gruppe“ erstellt werden.

- Wenn eine Produktlizenz einer Gruppe zugewiesen wird, kann der Administrator einen oder mehrere Servicepläne im Produkt deaktivieren. Dies erfolgt in der Regel, wenn die Organisation noch nicht bereit ist, mit der Nutzung eines im Produkt enthaltenen Diensts zu beginnen. Der Administrator möchte beispielsweise das Produkt Office 365 E3 einer Abteilung zuweisen, aber den Yammer Enterprise-Dienst vorübergehend deaktivieren.

- Alle Microsoft-Clouddienste, die eine Lizenzierung auf Benutzerebene erfordern, werden unterstützt. Dazu zählen alle Office 365-Produkte, Enterprise Mobility + Security, Dynamics CRM usw.

- Die gruppenbasierte Lizenzierung ist derzeit nur über [das Azure-Portal](https://portal.azure.com) verfügbar. Kunden, die in erster Linie andere Verwaltungsportale für die Benutzer- und Gruppenverwaltung nutzen, z.B. das Office 365-Portal, können dies weiterhin tun. Sie müssen jedoch das Azure-Portal zum Verwalten von Lizenzen auf Gruppenebene nutzen.

- Azure AD verwaltet Lizenzänderungen, die sich aus Änderungen an der Gruppenmitgliedschaft ergeben, automatisch. In der Regel erfolgt bei einem Benutzer, der einer Gruppe beitritt oder diese verlässt, eine Änderung des Lizenzstatus binnen Minuten nach der Mitgliedschaftsänderung.

- Ein Benutzer kann Mitglied mehrerer Gruppen mit angegebenen Lizenzrichtlinien sein und auch über Lizenzen verfügen, die im außerhalb von Gruppen zugewiesen wurden. Der resultierende Benutzerstatus ist eine Kombination aller zugewiesenen Produkt- und Dienstlizenzen.

- In einigen Fällen können einem Benutzer keine Lizenzen zugewiesen werden. Gründe sind das Fehlen verfügbarer Lizenzen im Mandanten oder in Konflikt stehende Dienste, die gleichzeitig zugewiesen wurden. Administratoren haben Zugriff auf Informationen zu Benutzern, für die Azure AD Gruppenlizenzen nicht vollständig verarbeiten konnte. Sie können anhand dieser Informationen Korrekturmaßnahmen vornehmen.

- Während der öffentlichen Vorschauphase ist im Mandanten ein kostenpflichtiges oder Testabonnement für Azure AD Basic oder höher erforderlich, um die gruppenbasierte Lizenzverwaltung nutzen zu können. Außerdem muss jeder Benutzer, dem Lizenzen über Gruppen zugewiesen werden, über die kostenpflichtige Azure AD-Editionslizenz verfügen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Szenarien für die gruppenbasierte Lizenzverwaltung finden Sie in folgenden Artikeln.

* [Zuweisen von Lizenzen zu einer Gruppe in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Bestimmen und Beheben von Lizenzproblemen für eine Gruppe in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Migrieren einzelner lizenzierter Benutzer zur gruppenbasierten Lizenzierung in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Gruppenbasierte Azure Active Directory-Lizenzierung – zusätzliche Szenarien](active-directory-licensing-group-advanced.md)

