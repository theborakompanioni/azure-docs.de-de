---

title: Was ist die gruppenbasierte Lizenzierung in Azure Active Directory? | Microsoft Docs
description: "Beschreibung der gruppenbasierten Lizenzierung in Azure Active Directory sowie ihrer Funktionsweise und bewährter Methoden"
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
ms.date: 06/29/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017;it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: 52dd48ce4e4acaf48f31edc51bbb657f8cd249cd
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017


---

# <a name="group-based-licensing-basics-in-azure-active-directory"></a>Grundlagen der gruppenbasierten Lizenzierung in Azure Active Directory

Die Verwendung von zahlungspflichtigen Microsoft-Clouddiensten wie Office 365, Enterprise Mobility + Security, Dynamics CRM und ähnlichen Produkte erfordern Lizenzen. Diese Lizenzen werden jedem Benutzer zugewiesen, der Zugriff auf diese Dienste benötigt. Administratoren verwalten Lizenzen über eines der Verwaltungsportale (Office, Azure) und PowerShell-Cmdlets. Azure Active Directory (Azure AD) ist die zugrunde liegende Infrastruktur, die die Identitätsverwaltung aller Microsoft-Clouddienste unterstützt. Azure AD speichert Informationen zum Lizenzzuweisungsstatus für Benutzer.

Bislang konnten Lizenzen nur auf Ebene einzelner Benutzer zugewiesen werden, was die umfassende Verwaltung erschweren kann. Um Benutzerlizenzen basierend auf Organisationsänderungen zu vergeben oder zu entziehen, wenn beispielsweise Benutzer der Organisation oder Abteilung beitreten oder diese verlassen, muss ein Administrator häufig ein komplexes PowerShell-Skript schreiben. Dieses Skript richtet einzelne Aufrufe an den Clouddienst.

Um diese Probleme zu beheben, enthält Azure AD jetzt die gruppenbasierte Lizenzierung. Sie können einer Gruppe eine oder mehrere Produktlizenzen zuweisen. Azure AD stellt sicher, dass die Lizenzen allen Mitgliedern der Gruppe zugewiesen werden. Allen neuen Mitgliedern, die der Gruppe beitreten, werden die entsprechenden Lizenzen zugewiesen. Wenn sie die Gruppe verlassen, werden diese Lizenzen entfernt. Dadurch ist keine automatisierte Lizenzverwaltung über PowerShell mehr erforderlich, um Änderungen in der Organisations- und Abteilungsstruktur benutzerbezogen widerzuspiegeln.

## <a name="features"></a>Features

Hier die Hauptmerkmale der gruppenbasierten Lizenzierung:

- Lizenzen können beliebigen Sicherheitsgruppen in Azure AD zugewiesen werden. Sicherheitsgruppen können mithilfe von Azure AD Connect lokal synchronisiert werden. Sie können Sicherheitsgruppen auch direkt in Azure AD Connect (auch als reine Cloudgruppen bezeichnet) oder automatisch über das Azure AD-Feature „Dynamische Gruppe“ erstellen.

- Wenn eine Produktlizenz einer Gruppe zugewiesen wird, kann der Administrator einen oder mehrere Servicepläne im Produkt deaktivieren. In der Regel erfolgt dies, wenn die Organisation noch nicht für die Verwendung eines Dienstes in einem Produkt bereit ist. Beispielsweise könnte der Administrator Office 365 einer Abteilung zuweisen, aber den Yammer-Dienst vorübergehend deaktivieren.

- Alle Microsoft-Clouddienste, die eine Lizenzierung auf Benutzerebene erfordern, werden unterstützt. Dazu zählen alle Office 365-Produkte, Enterprise Mobility + Security und Dynamics CRM.

- Die gruppenbasierte Lizenzierung ist derzeit nur über [das Azure-Portal](https://portal.azure.com) verfügbar. Wenn Sie in erster Linie andere Verwaltungsportale für die Benutzer- und Gruppenverwaltung nutzen, z.B. das Office 365-Portal, können Sie dies weiterhin tun. Jedoch sollten Sie das Azure-Portal zum Verwalten von Lizenzen auf Gruppenebene verwenden.

- Azure AD verwaltet Lizenzänderungen, die sich aus Änderungen an der Gruppenmitgliedschaft ergeben, automatisch. In der Regel erfolgen Änderungen des Lizenzstatus binnen Minuten nach einer Mitgliedschaftsänderung.

- Ein Benutzer kann Mitglied mehrerer Gruppen mit angegebenen Lizenzrichtlinien sein. Ein Benutzer kann auch über Lizenzen verfügen, die außerhalb von Gruppen zugewiesen wurden. Der resultierende Benutzerstatus ist eine Kombination aller zugewiesenen Produkt- und Dienstlizenzen.

- In manchen Fällen können Benutzern keine Lizenzen zugewiesen werden. Mögliche Gründe sind das Fehlen verfügbarer Lizenzen im Mandanten oder in Konflikt stehende Dienste, die gleichzeitig zugewiesen wurden. Administratoren haben Zugriff auf Informationen zu Benutzern, für die Azure AD Gruppenlizenzen nicht vollständig verarbeiten konnte. Sie können anhand dieser Informationen Korrekturmaßnahmen vornehmen.

- Während der öffentlichen Vorschauphase ist im Mandanten ein kostenpflichtiges oder Testabonnement für Azure AD Basic oder Premium Edition erforderlich, um die gruppenbasierte Lizenzverwaltung nutzen zu können.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Szenarien für die gruppenbasierte Lizenzverwaltung finden Sie unter:

* [Erste Schritte mit Lizenzen in Azure Active Directory](active-directory-licensing-get-started-azure-portal.md)
* [Zuweisen von Lizenzen zu einer Gruppe in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Bestimmen und Beheben von Lizenzproblemen für eine Gruppe in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Migrieren einzelner lizenzierter Benutzer zur gruppenbasierten Lizenzierung in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Gruppenbasierte Azure Active Directory-Lizenzierung – zusätzliche Szenarien](active-directory-licensing-group-advanced.md)

