---
title: Berechtigungen in Azure Security Center | Microsoft-Dokumentation
description: "In diesem Artikel wird beschrieben, wie Azure Security Center die rollenbasierte Zugriffssteuerung zum Zuweisen von Berechtigungen für Benutzer verwendet und die zulässigen Aktionen für jede Rolle identifiziert."
services: security-center
cloud: na
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
ms.assetid: 
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 5c4a08d96175d431e0a29dfc5927b64567c40117
ms.openlocfilehash: d1c8a39bbcda7d22fdce08d122098e994ca87451


---

# <a name="permissions-in-azure-security-center"></a>Berechtigungen in Azure Security Center

Azure Security Center verwendet die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](../active-directory/role-based-access-control-configure.md). Diese stellt [integrierte Rollen](../active-directory/role-based-access-built-in-roles.md) bereit, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können.

Security Center bewertet die Konfiguration Ihrer Ressourcen, um die Sicherheitsprobleme und Sicherheitsrisiken zu identifizieren. In Security Center werden Ihnen nur dann Informationen zu einer Ressource angezeigt, wenn Ihnen für das Abonnement oder die Ressourcengruppe, der eine Ressource angehört, die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ zugewiesen ist.

## <a name="roles-and-allowed-actions"></a>Rollen und zulässige Aktionen

In der folgenden Tabelle sind die Rollen und zulässigen Aktionen von Security Center aufgeführt. Mit „X“ wird angegeben, dass die Aktion für die jeweilige Rolle zulässig ist.

| Rolle | Sicherheitsrichtlinie bearbeiten | Sicherheitsempfehlungen für eine Ressource anwenden | Warnungen warten oder verwerfen | Warnungen abonnementübergreifend anzeigen | Warnungen für eine bestimmte Ressource anzeigen |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Besitzer des Abonnements | X | X | X | X | X |
| Mitwirkender des Abonnements | X | X | X | X | X |
| Ressourcengruppenbesitzer | -- | X | -- | -- | X |
| Ressourcengruppenmitwirkender | -- | X | -- | -- | X |
| Leser | -- | -- | -- | X | X |

> [!NOTE]
> Es empfiehlt sich, den Benutzern eine Rolle zuzuweisen, die jeweils nur so viele Berechtigungen umfasst wie für die Erfüllung ihrer Aufgaben erforderlich sind. Weisen Sie die Rolle „Leser“ etwa Benutzern zu, die nur Informationen zur Sicherheitsintegrität einer Ressource anzeigen, aber keine Aktionen durchführen müssen (also beispielsweise keine Empfehlungen umsetzen oder Richtlinien bearbeiten).
>
>

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde beschrieben, wie Security Center die rollenbasierte Zugriffssteuerung (RBAC) zum Zuweisen von Berechtigungen für Benutzer nutzt und welche Aktionen für die einzelnen Rollen zulässig sind. Informieren Sie sich als Nächstes über Folgendes, nachdem Sie sich mit den Rollenzuweisungen zum Überwachen des Sicherheitszustands Ihres Abonnements, Bearbeiten von Sicherheitsrichtlinien und Anwenden von Empfehlungen vertraut gemacht haben:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md)
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md)
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md)
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md)
- [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md)



<!--HONumber=Dec16_HO2-->


