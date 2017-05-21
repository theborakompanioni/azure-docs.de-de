---
title: "Übersicht über verwaltete Azure-Anwendungen | Microsoft-Dokumentation"
description: "Hier werden die Konzepte für eine verwaltete Azure-Anwendung beschrieben."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/11/2017
ms.author: gauravbh; tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: a4348a4d2348d744c03ad3a89d0548526fa2f9f8
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="azure-managed-applications-overview"></a>Übersicht über verwaltete Azure-Anwendungen

Über Azure ist ein zuverlässiger Marketplace verfügbar, in dem ISVs und Start-up-Unternehmen ihre Lösungen für Kunden auf der ganzen Welt anbieten können. Bei Azure Marketplace handelt es sich um einen Katalog, der aus Hunderten komplexer, mehrere Ressourcen umfassender Vorlagen von Erst- und Drittanbietern besteht. Kunden können innerhalb weniger Minuten PaaS- und SaaS-Anwendungen bereitstellen und starten. Ein Angebot kann zwar schnell bereitgestellt werden, für die Wartung und Aktualisierung der Lösung ist jedoch weiterhin der Kunde zuständig. Hersteller haben keine Möglichkeit, Kunden für die Nutzung einer Anwendung über die Kosten des VM-Images hinaus Gebühren in Rechnung zu stellen. Darüber hinaus können Hersteller weder verhindern, dass Kunden wichtige Anwendungsressourcen ändern, noch den Zugriff auf das geistige Eigentum blockieren, das eine Anwendung ausmacht. Verwaltete Azure-Anwendungen bieten eine Lösung für diese Probleme. 

## <a name="advantages-of-managed-applications"></a>Vorteile von verwalteten Anwendungen

Verwaltete Azure-Anwendungen ermöglichen ein Ökosystem, in dem Hersteller einen PaaS- oder SaaS-Dienst als eigenständige Anwendungen verfügbar machen können. Kunden stellen verwaltete Anwendungen in ihren Abonnements bereit, Hersteller können sie jedoch verwalten. Hersteller können Kunden mithilfe des Azure-Abrechnungssystems die Kosten in Rechnung stellen und mithilfe von Vorlagen den Lebenszyklus bereitgestellter Anwendungen verwalten. Auf der anderen Seite können Kunden automatisch Updates abrufen und für Support und Wartung bezahlen. Sie müssen die Anwendung nicht selbst warten oder aktualisieren oder bei einem Fehler der Anwendung eine Diagnose oder Problembehandlung ausführen.

Von einem derartigen Ökosystem in Azure profitieren nicht nur PaaS- und SaaS-Hersteller, sondern auch Teams in Unternehmen mit zentraler Plattform und Systemintegratoren, die ihre Lösungen bündeln und weiterverkaufen möchten.

## <a name="how-managed-applications-work"></a>Funktionsweise von verwalteten Anwendungen
Es gibt zwei Szenarien beim Arbeiten mit verwalteten Anwendungen:

1. Bei einem steht der Hersteller oder unabhängige Softwarehersteller (Independent Software Vendor, ISV), der eine verwaltete Anwendung erstellt und zur allgemeinen Nutzung zur Verfügung stellt, im Mittelpunkt. 
2. Beim anderen geht es um den Kunden oder Verbraucher, der die veröffentlichte Anwendung erstellen und verwenden möchte. 

In diesem Artikel werden beide Szenarien behandelt. Sehen wir uns zunächst die Funktionsweise von verwalteten Anwendungen genauer an. 

Eine verwaltete Anwendung ähnelt bis auf einen wichtigen Unterschied einer Marketplace-Lösungsvorlage. In einer verwalteten Anwendung werden die Ressourcen in einer Ressourcengruppe bereitgestellt, die vom ISV/Hersteller verwaltet wird. Die Ressourcengruppe ist im Abonnement des Kunden vorhanden, aber ein Benutzer, eine Benutzergruppe oder eine Anwendung im Mandanten des ISV hat Zugriff auf die Ressourcengruppe. Zum Verwalten und Warten der Anwendung wird die Identität des Herstellers einem Active Directory-Besitzer, -Mitwirkenden oder -Leser bzw. einer anderen integrierten Rolle hinzugefügt. 

## <a name="key-concepts"></a>Wichtige Begriffe

### <a name="managed-resource-group"></a>Verwaltete Ressourcengruppe
Die Ressourcengruppe, in der alle in der Vorlage bereitgestellten Azure-Ressourcen erstellt werden. Beispiel: Wenn die Anwendung ein Speicherkonto erstellt, enthält diese Ressourcengruppe die Speicherkontoressource. Sie enthält nicht die Geräteressource.

### <a name="appliance-package"></a>Gerätepaket
Der Herausgeber erstellt ein Paket, das die Vorlagendateien und die createUIDefinition-Datei enthält. Es enthält im Einzelnen die folgenden Dateien:

- **applianceMainTemplate.json**: Die Vorlagendatei, die alle vom Gerät bereitgestellten Ressourcen definiert. Diese Datei ist eine reguläre Vorlagendatei, die zum Erstellen von Ressourcen verwendet wird.

- **MainTemplate.json**: Eine Vorlagendatei, die die Geräteressource definiert (Microsoft.Solutions/appliances). Eine in dieser Ressource definierte Schlüsseleigenschaft ist „ManagedResourceGroupId“. Diese Eigenschaft gibt an, welche Ressourcengruppe zum Hosten der in „applianceMainTemplate.json“ definierten tatsächlichen Ressourcen verwendet wird.

- **createUIDefinition.json**: Diese Datei beschreibt, wie die Benutzeroberfläche gerendert wird, die für die in der Vorlage definierten Parameter erforderlich ist.

### <a name="authorization"></a>Autorisierung
Der Herausgeber muss die Berechtigungen angeben, die der Hersteller zum Verwalten der Ressourcen im Auftrag des Kunden benötigt. Diese Berechtigung gilt für die verwaltete Ressourcengruppe. Sie legen die folgenden Werte fest:

- **PrincipalID**: Der Azure AD-Bezeichner des Benutzers, der Gruppe oder der Anwendung, der zum Gewähren des Zugriffs auf die verwaltete Ressourcengruppe verwendet wird. Dieser Bezeichner gehört zum Mandanten des Herausgebers.

- **RoleDefinitionID**: Der Azure AD-Bezeichner der Rolle, die der obigen Prinzipal-ID zugewiesen ist. Dabei kann es sich um eine beliebige integrierte RBAC-Rolle im Mandanten des Herausgebers handeln.

## <a name="next-steps"></a>Nächste Schritte

* Grundlegendes zur Anbietererfahrung erfahren Sie unter [Erstellen und Veröffentlichen von verwalteten Azure-Anwendungen](managed-application-publishing.md).
* Grundlegendes zur Kundenerfahrung erfahren Sie unter [Nutzen einer verwalteten Azure-Anwendung](managed-application-consumption.md).
* Informationen zum Erstellen einer Benutzeroberflächen-Definitionsdatei finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).
