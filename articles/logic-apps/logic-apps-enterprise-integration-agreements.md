---
title: "Vereinbarungen für B2B-Kommunikation: Azure Logic Apps | Microsoft-Dokumentation"
description: "Erstellen von Vereinbarungen, sodass Partner in B2B-Szenarien für Azure Logic Apps und das Enterprise Integration Pack kommunizieren können"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 447ffb8e-3e91-4403-872b-2f496495899d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: LADocs
ms.translationtype: Human Translation
ms.sourcegitcommit: 8dd90542440322e6d57406cd950fa0a13bd4fe1d
ms.openlocfilehash: 1068b5bd5f2c86de0c82f5a96cb2718645c0a3d3
ms.contentlocale: de-de
ms.lasthandoff: 02/07/2017


---
# <a name="partner-agreements-for-b2b-communication-with-azure-logic-apps-and-enterprise-integration-pack"></a>Partnervereinbarungen für die B2B-Kommunikation mit Azure Logic Apps und Enterprise Integration Pack

Vereinbarungen sind die Eckpfeiler der B2B-Kommunikation (Business-to-Business, Kommunikation zwischen Unternehmen) und ermöglichen Geschäftsentitäten eine reibungslose Kommunikation über Protokolle gemäß Industriestandard. Bei Aktivierung von B2B-Szenarien für Logik-Apps mit dem Enterprise Integration Pack ist eine Vereinbarung eine Kommunikationsabsprache zwischen B2B-Handelspartnern. Diese Vereinbarung basiert auf der Kommunikation, die beide Partner einrichten möchten, und ist protokoll- oder transportspezifisch.

Für die Unternehmensintegration werden diese Protokoll- bzw. Transportstandards unterstützt:

* [AS2](logic-apps-enterprise-integration-as2.md)
* [X12](logic-apps-enterprise-integration-x12.md)
* [EDIFACT](logic-apps-enterprise-integration-edifact.md)

## <a name="why-use-agreements"></a>Gründe für Vereinbarungen

Es folgen einige der allgemeinen Vorteile der Verwendung von Vereinbarungen:

* Ermöglichen, dass Organisationen und Unternehmen Informationen in einem bekannten Format austauschen
* Verbessern der Effizienz von B2B-Transaktionen
* Einfache Erstellung, Verwaltung und Nutzung beim Erstellen von Apps für die Unternehmensintegration

## <a name="how-to-create-agreements"></a>Erstellen von Vereinbarungen

* [Erstellen einer AS2-Vereinbarung](logic-apps-enterprise-integration-as2.md)
* [Erstellen einer X12-Vereinbarung](logic-apps-enterprise-integration-x12.md)
* [Erstellen einer EDIFACT-Vereinbarung](logic-apps-enterprise-integration-edifact.md)

## <a name="how-to-use-an-agreement"></a>Verwenden einer Vereinbarung

Sie können [Logik-Apps](logic-apps-what-are-logic-apps.md "Informationen zu Logik-Apps") mit B2B-Funktionen mithilfe einer von Ihnen definierten Vereinbarung erstellen.

## <a name="how-to-edit-an-agreement"></a>Bearbeiten einer Vereinbarung

Sie können eine Vereinbarung bearbeiten, indem Sie folgende Schritte ausführen:

1. Wählen Sie das Integrationskonto aus, das die Vereinbarung enthält, die Sie ändern möchten.

2. Wählen Sie die Kachel **Vereinbarungen** aus.

3. Wählen Sie auf dem Blatt **Vereinbarungen** die Vereinbarung aus.

4. Wählen Sie **Bearbeiten** aus. Nehmen Sie Ihre Änderungen vor.

5. Klicken Sie zum Speichern der Änderungen auf **OK**.

## <a name="how-to-delete-an-agreement"></a>Löschen einer Vereinbarung

Sie können Vereinbarungen löschen, indem Sie folgende Schritte ausführen:

1. Wählen Sie das Integrationskonto aus, das die Vereinbarung enthält, die Sie löschen möchten.
2. Wählen Sie die Kachel **Vereinbarungen** aus.
3. Wählen Sie auf dem Blatt **Vereinbarungen** die Vereinbarung aus.
4. Wählen Sie **Löschen** aus.
5. Bestätigen Sie, dass Sie die ausgewählte Vereinbarung löschen möchten.

    Die gelöschte Vereinbarung wird nicht mehr auf dem Blatt „Vereinbarungen“ angezeigt.

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen einer AS2-Vereinbarung](logic-apps-enterprise-integration-as2.md)

