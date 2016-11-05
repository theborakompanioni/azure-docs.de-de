---
title: Rollenbasierte Zugriffssteuerung | Microsoft Docs
description: Führen Sie die ersten Schritte der Zugriffsverwaltung mit der rollenbasierten Zugriffssteuerung von Azure im Azure-Portal aus. Verwenden Sie Rollenzuweisungen, um in Ihrem Verzeichnis Berechtigungen zuzuweisen.
services: active-directory
documentationcenter: ''
author: kgremban
manager: femila
editor: ''

ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/03/2016
ms.author: kgremban

---
# Erste Schritte mit der Zugriffsverwaltung im Azure-Portal
Sicherheitsorientierte Unternehmen müssen sich darauf konzentrieren, Mitarbeitern genau die Berechtigungen zuzuweisen, die sie benötigen. Zu viele Berechtigungen machen ein Konto zum leichten Angriffsziel. Wenn die Berechtigungen nicht ausreichen, können Mitarbeiter nicht effizient arbeiten. Die rollenbasierte Zugriffssteuerung in Azure (Role-Based Access Control, RBAC) begegnet diesem Problem, indem sie eine präzise Zugriffsverwaltung für Azure ermöglicht.

Mithilfe von RBAC können Sie Aufgaben in Ihrem Team verteilen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen. Anstatt allen uneingeschränkte Berechtigungen in Ihrem Azure-Abonnement oder Ihren Ressourcen zu gewähren, können Sie die Berechtigungen auf bestimmte Aktionen beschränken. Gestatten Sie z.B. mit RBAC einem Mitarbeiter die Verwaltung virtueller Computer in einem Abonnement, während ein anderer im gleichen Abonnement SQL-Datenbanken verwalten kann.

## Grundlagen zur Zugriffsverwaltung in Azure
Jedes Azure-Abonnement ist mit einem Azure Active Directory-Verzeichnis (AD) verknüpft. Benutzer, Gruppen und Anwendungen aus diesem Verzeichnis können Ressourcen im Azure-Abonnement verwalten. Weisen Sie diese Zugriffsrechte mit dem Azure-Portal, Azure-Befehlszeilentools und Azure-Verwaltungs-APIs zu.

Gewähren Sie Zugriff, indem Sie Benutzern, Gruppen und Anwendungen die jeweils geeignete RBAC-Rolle für einen bestimmten Bereich zuweisen. Der Bereich einer Rollenzuweisung kann ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein. Mit einer Rolle, die einem übergeordneten Bereich zugewiesen ist, wird außerdem der Zugriff auf die darin enthaltenen untergeordneten Elemente gewährt. Ein Benutzer mit Zugriff auf eine Ressourcengruppe kann beispielsweise alle Ressourcen verwalten, die darin enthalten sind, z.B. Websites, virtuelle Computer und Subnetze.

![Beziehung zwischen Azure Active Directory-Elementen – Diagramm](./media/role-based-access-control-what-is/rbac_aad.png)

Die von Ihnen zugewiesene RBAC-Rolle legt fest, welche Ressourcen der Benutzer, die Gruppe oder die Anwendung auf dieser Ebene verwalten kann.

## Integrierte Rollen
Azure RBAC verfügt über drei grundlegende Rollen, die für alle Ressourcentypen gelten:

* **Besitzer** verfügen über vollständigen Zugriff auf alle Ressourcen, einschließlich des Rechts, den Zugriff an andere Personen zu delegieren.
* **Mitwirkende** können alle Arten von Azure-Ressourcen erstellen und verwalten, aber keinen anderen Personen Zugriff gewähren.
* **Leser** können vorhandene Azure-Ressourcen anzeigen.

Die verbleibenden RBAC-Rollen in Azure ermöglichen die Verwaltung von bestimmten Azure-Ressourcen. Mit der Rolle „Mitwirkender von virtuellen Computern“ können Benutzer beispielsweise virtuelle Computer erstellen und verwalten. Sie haben damit keinen Zugriff auf das virtuelle Netzwerk oder das Subnetz, mit dem der virtuelle Computer verbunden ist.

Unter [Integrierte RBAC-Rollen](role-based-access-built-in-roles.md) sind die Rollen aufgeführt, die in Azure verfügbar sind. Hierbei werden die Vorgänge und der Bereich angegeben, die mit jeder integrierten Rolle für Benutzer gewährt werden. Wenn Sie Ihre eigenen Rollen definieren möchten, um eine noch bessere Kontrolle zu haben, helfen Ihnen die Informationen zum Erstellen von [benutzerdefinierten Rollen in Azure RBAC](role-based-access-control-custom-roles.md) weiter.

## Ressourcenhierarchie und Zugriffsvererbung
* Jedes **Abonnement** in Azure gehört jeweils nur zu einem Verzeichnis.
* Jede **Ressourcengruppe** gehört jeweils nur zu einem Abonnement.
* Jede **Ressource** gehört jeweils nur zu einer Ressourcengruppe.

Der Zugriff, den Sie auf übergeordneter Ebene gewähren, wird auf untergeordneter Ebene geerbt. Zum Beispiel:

* Sie weisen die Rolle „Leser“ einer Azure AD-Gruppe im Abonnementbereich zu. Die Mitglieder dieser Gruppe können alle Ressourcengruppen und Ressourcen im Abonnement anzeigen.
* Sie weisen die Rolle „Mitwirkender“ einer Anwendung im Ressourcengruppenbereich zu. Damit können Benutzer Ressourcen aller Typen in dieser Ressourcengruppe verwalten, aber keine anderen Ressourcengruppen des Abonnements.

## Azure RBAC im Vergleich zu klassischen Administratoren für Abonnements
Klassische Administratoren und Co-Admins für Abonnements verfügen über vollständigen Zugriff auf das Azure-Abonnement. Sie können Ressourcen über das [Azure-Portal](https://portal.azure.com) mit Azure Resource Manager-APIs oder über das [klassische Azure-Portal](https://manage.windowsazure.com) und das klassische Azure-Bereitstellungsmodell verwalten. Im RBAC-Modell wird klassischen Administratoren die Besitzerrolle auf Abonnementebene zugewiesen.

Nur das Azure-Portal und die neuen Azure Resource Manager-APIs unterstützen Azure RBAC. Benutzer und Anwendungen, denen eine RBAC-Rolle zugewiesen ist, können das klassische Verwaltungsportal und das klassische Azure-Bereitstellungsmodell nicht verwenden.

## Autorisierung für Verwaltungsvorgänge im Vergleich zu Datenvorgängen
Azure RBAC unterstützt nur Verwaltungsvorgänge von Azure-Ressourcen im Azure-Portal und in den Azure Resource Manager-APIs. Azure RBAC kann nicht alle Vorgänge auf Datenebene für Azure-Ressourcen autorisieren. Sie können z.B. eine Person zum Verwalten von Speicherkonten autorisieren, jedoch nicht zum Verwalten von Blobs oder Tabellen innerhalb eines Speicherkontos. Ebenso kann eine SQL-Datenbank verwaltet werden, aber nicht die darin enthaltenen Tabellen.

## Nächste Schritte
* Erste Schritte mit der [rollenbasierten Zugriffssteuerung im Azure-Portal](role-based-access-control-configure.md).
* Weitere Informationen finden Sie unter [Integrierte RBAC-Rollen in Azure](role-based-access-built-in-roles.md).
* Definieren Sie Ihre eigenen [benutzerdefinierten Rollen in Azure RBAC](role-based-access-control-custom-roles.md).

<!---HONumber=AcomDC_0810_2016-->