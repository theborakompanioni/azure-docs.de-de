---
title: "Übersicht über verwaltete Azure-Anwendungen | Microsoft-Dokumentation"
description: "Hier werden die Begriffe für verwaltete Azure-Anwendungen beschrieben."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: 7ace8e1ea8038e0748bfed00c0cc0a4fa340588b
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---

# <a name="azure-managed-applications-overview"></a>Übersicht über verwaltete Azure-Anwendungen

Anbieter, die Azure verwenden, können Kunden auf der ganzen Welt Lösungen anbieten. Beim Azure Marketplace handelt es sich um einen Katalog, der aus Hunderten komplexer, mehrere Ressourcen umfassender Vorlagen von Erst- und Drittanbietern besteht. Kunden können innerhalb weniger Minuten Bereitstellungen durchführen und mit der Verwendung von Platform as a Service(PaaS)- und Software-as-a-Service(SaaS)-Anwendungen beginnen. 

Ein Angebot kann für Kunden zwar schnell über den Marketplace bereitgestellt werden, für die Wartung und Aktualisierung der Lösung ist jedoch der Kunde zuständig. Anbieter haben keine Möglichkeit, Kunden für die Nutzung einer Anwendung über die Kosten des VM-Images hinaus Gebühren in Rechnung zu stellen. Darüber hinaus können Anbieter Kunden nicht daran hindern, kritische Anwendungsressourcen zu ändern. Auch den Zugriff auf geistiges Eigentum, die eine Anwendung erzeugt, können Anbieter nicht blockieren. Verwaltete Azure-Anwendungen bieten Lösungen für diese Probleme. 

Eine verwaltete Anwendung ähnelt bis auf einen wichtigen Unterschied einer Marketplace-Lösungsvorlage. In einer verwalteten Anwendung werden die Ressourcen in einer Ressourcengruppe bereitgestellt, die vom Anbieter verwaltet wird. Die Ressourcengruppe ist im Abonnement des Kunden vorhanden, aber eine Identität im Mandanten hat Zugriff auf die Ressourcengruppe.

## <a name="advantages-of-managed-applications"></a>Vorteile von verwalteten Anwendungen

Managed Service Provider (MSPs), ISVs und unternehmensinterne IT-Teams können mithilfe verwalteter Anwendungen Lösungen über den Marketplace oder den Dienstkatalog bereitstellen. Obwohl Kunden diese verwalteten Anwendungen in ihren Abonnements bereitstellen, müssen sie diese nicht verwalten, aktualisieren oder betreuen. Da die Verwaltung der Anwendungen und die Bereitstellung des Supports durch die Anbieter erfolgen, müssen sich Kunden kein anwendungsspezifisches Domänenwissen aneignen, um diese Anwendungen zu verwalten. Kunden können automatisch Anwendungsupdates abrufen, ohne sich mit der Problembehandlung und Diagnose von Problemen mit den Anwendungen befassen zu müssen.

Für Anbieter erstellen verwaltete Anwendungen einen Kanal zum Verkauf von Infrastruktur und Software über den Marketplace. Verwaltete Anwendungen bieten Azure-Kunden auch die Möglichkeit, Dienste und Support für den Betrieb zur Verfügung zu stellen. Anbieter können Kunden mithilfe des Azure-Abrechnungssystems Rechnungen stellen. Sie können den Lebenszyklus der bereitgestellten Anwendungen mithilfe von Vorlagen steuern. Diese Lösungen sind eigenständig und für den Kunden versiegelt, sodass Lieferanten qualitativ hochwertige Dienste bereitstellen können. Dieser Ansatz kommt PaaS- und SaaS-Anbietern zugute. Auch unternehmensinterne Plattformteams und Systemintegratoren (SIs), die ihre Lösungen verpacken und verkaufen möchten, können davon profitieren.

## <a name="managed-application-types"></a>Typen von verwalteten Anwendungen
Verwaltete Azure-Anwendungen gibt es in zwei Varianten – Dienstkatalog und Marketplace.
 
### <a name="service-catalog"></a>Dienstkatalog  

Mit dem Dienstkatalog können Kunden einen Katalog genehmigter Lösungen für Azure erstellen, der von Personen in dieser Organisation verwendet werden kann. Das Führen eines solchen Lösungskatalogs ist für zentrale IT-Teams in Unternehmen hilfreich. Mithilfe des Katalogs können sie die Einhaltung bestimmter Organisationsstandards gewährleisten und gleichzeitig Lösungen für ihre Organisationen bereitstellen. Sie können diese Anwendungen steuern, aktualisieren und verwalten. Mitarbeiter können mithilfe des Katalogs mühelos den umfangreichen Satz von Anwendungen ermitteln, die von den IT-Abteilungen empfohlen werden und von dieser genehmigt sind. Kunden sehen die verwalteten Dienstkataloganwendungen, die sie erstellt haben. Des Weiteren können sie auch die verwalteten Anwendungen sehen, die andere Personen in ihrer Organisation gemeinsam mit ihnen nutzen.
 
Informationen zum Veröffentlichen einer verwalteten Dienstkataloganwendung finden Sie unter [Erstellen und Veröffentlichen einer verwalteten Dienstkataloganwendung](managed-application-publishing.md).
 
Informationen zum Nutzen einer verwalteten Dienstkataloganwendung, finden Sie unter [Nutzen einer verwalteten Dienstkataloganwendung](managed-application-consumption.md).
 
### <a name="marketplace"></a>Marketplace

Verwaltete Anwendungen stehen über Marketplace im Azure-Portal zur Verfügung. Nachdem der Anbieter diese Anwendungen veröffentlicht hat, können sie von allen Benutzern innerhalb oder außerhalb einer Organisation genutzt werden. Durch diesen Ansatz können MSPs, ISVs und SIs ihre Lösungen für alle Azure-Kunden anbieten. Kunden erhalten den Vorteil, dass sie solche komplexen Lösungen nutzen können, ohne sich um die Aneignung von Wissen und die Verwaltung der Lösungen kümmern zu müssen. 

Derzeit können Herausgeber ihre Angebote als verwaltete Anwendung oder als nicht verwaltete Lösungsvorlage zur Verfügung stellen. Zu den Hauptkomponenten bei der Veröffentlichung einer verwalteten Anwendung zählen die Vorlagendatei und die UI-Definitionsdatei. Die Vorlagendatei beschreibt die Ressourcen, die bereitgestellt werden. Die UI-Definitionsdatei beschreibt, wie die erforderlichen Eingaben zur Bereitstellung dieser Ressourcen im Portal angezeigt werden. Die erforderlichen Dateien sind in einer ZIP-Datei verpackt und werden über das Veröffentlichungsportal hochgeladen.
 
Informationen zum Veröffentlichen einer verwalteten Anwendung im Marketplace finden Sie unter [Verwaltete Azure-Anwendungen im Marketplace](managed-application-author-marketplace.md).

Informationen zur Nutzung einer verwalteten Anwendung aus dem Marketplace finden Sie unter [Nutzen verwalteter Azure-Anwendungen im Marketplace](managed-application-consume-marketplace.md).

## <a name="key-concepts"></a>Wichtige Begriffe

### <a name="managed-resource-group"></a>Verwaltete Ressourcengruppe
In der verwalteten Ressourcengruppe werden alle Azure-Ressourcen, die in der Vorlage bereitgestellt werden, erstellt. Beispiel: Wenn das Gerät zum Erstellen eines Speicherkontos verwendet wird, enthält diese Ressourcengruppe die Speicherkontoressource. Sie enthält nicht die Geräteressource.

### <a name="appliance-package"></a>Gerätepaket
Der Herausgeber erstellt ein Paket, das die Vorlagendateien und die createUIDefinition-Datei enthält. Es enthält im Einzelnen die folgenden Dateien:

- **applianceMainTemplate.json**: Die Vorlagendatei definiert alle vom Gerät bereitgestellten Ressourcen. Diese Datei ist eine reguläre Vorlagendatei, die zum Erstellen von Ressourcen verwendet wird.

- **MainTemplate.json**: Diese Vorlagendatei definiert die Geräteressource (Microsoft.Solutions/appliances). Eine in dieser Ressource definierte Schlüsseleigenschaft ist „ManagedResourceGroupId“. Diese Eigenschaft gibt an, welche Ressourcengruppe zum Hosten der tatsächlichen Ressourcen, die in „applianceMainTemplate.json“ definiert sind, verwendet wird.

- **applianceCreateUIDefinition.json**: Diese Datei beschreibt, wie die Benutzeroberfläche, die für die in der Vorlage definierten Parameter erforderlich ist, gerendert wird.

### <a name="authorization"></a>Autorisierung
Der Herausgeber muss die Berechtigungen angeben, die der Hersteller zum Verwalten der Ressourcen im Auftrag des Kunden benötigt. Diese Berechtigung gilt für die verwaltete Ressourcengruppe. Legen Sie die folgenden Werte fest:

- **PrincipalID**: Der Azure Active Directory(Azure AD)-Bezeichner des Benutzers, der Gruppe oder der Anwendung, der zum Gewähren des Zugriffs auf die verwaltete Ressourcengruppe verwendet wird. Dieser Bezeichner gehört zum Mandanten des Herausgebers.

- **RoleDefinitionID**: Der Azure AD-Bezeichner der Rolle, die der obigen Prinzipal-ID zugewiesen ist. Hierbei kann es sich um eine beliebige integrierte rollenbasierte Zugriffssteuerungsrolle im Mandanten des Herausgebers handeln. Weitere Informationen finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Veröffentlichen von verwalteten Anwendungen im Marketplace finden Sie unter [Verwaltete Azure-Anwendungen im Marketplace](managed-application-author-marketplace.md).
* Informationen zur Nutzung einer verwalteten Anwendung aus dem Marketplace finden Sie unter [Nutzen verwalteter Azure-Anwendungen im Marketplace](managed-application-consume-marketplace.md).
* Informationen zum Veröffentlichen einer verwalteten Dienstkataloganwendung finden Sie unter [Erstellen und Veröffentlichen einer verwalteten Dienstkataloganwendung](managed-application-publishing.md).
* Informationen zum Nutzen einer verwalteten Dienstkataloganwendung, finden Sie unter [Nutzen einer verwalteten Dienstkataloganwendung](managed-application-consumption.md).
* Informationen zum Erstellen einer UI-Definitionsdatei finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).

