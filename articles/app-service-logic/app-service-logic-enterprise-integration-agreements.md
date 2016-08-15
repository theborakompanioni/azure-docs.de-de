<properties 
	pageTitle="Übersicht über Partner und Enterprise Integration Pack | Microsoft Azure App Service" 
	description="Erfahren Sie, wie Sie Partner mit dem Enterprise Integration Pack und Logik-Apps verwenden" 
	services="logic-apps" 
	documentationCenter=".net,nodejs,java"
	authors="msftman" 
	manager="erikre" 
	editor="cgronlun"/>

<tags 
	ms.service="logic-apps" 
	ms.workload="integration" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/29/2016" 
	ms.author="deonhe"/>


# Informationen zu Vereinbarungen und Enterprise Integration Pack

## Übersicht
Vereinbarungen sind die Eckpfeiler der B2B-Kommunikation (Business-to-Business, Kommunikation zwischen Unternehmen) und ermöglichen Geschäftsentitäten eine reibungslose Kommunikation über Protokolle gemäß Industriestandard.

## Was ist eine Vereinbarung?

Eine Vereinbarung mit Blick auf das Enterprise Integration Pack ist eine Kommunikationsabsprache zwischen B2B-Handelspartnern. Eine Vereinbarung basiert auf der Kommunikation, die beide Partner wünschen, und ist protokoll- oder transportspezifisch.

Für die Unternehmensintegration werden drei Protokoll- bzw. Transportstandards unterstützt:

- [AS2](./app-service-logic-enterprise-integration-as2.md)
- [X12](./app-service-logic-enterprise-integration-x12.md)
- [EDIFACT](./app-service-logic-enterprise-integration-edifact.md) (in Kürze verfügbar)

## Gründe für Vereinbarungen
Es folgen einige Vorteile der Verwendung von Vereinbarungen:
- Ermöglichen, dass Organisationen und Unternehmen Informationen in einem bekannten Format austauschen können.
- Verbessern der Effizienz von B2B-Transaktionen
- Einfache Erstellung, Verwaltung und Nutzung beim Erstellen von Apps für die Unternehmensintegration

## Erstellen von Vereinbarungen
- [Erstellen einer AS2-Vereinbarung](./app-service-logic-enterprise-integration-as2.md)
- [Erstellen einer X12-Vereinbarung](./app-service-logic-enterprise-integration-x12.md)

## Verwenden einer Vereinbarung
Nach dem Erstellen einer Vereinbarung können Sie sie im Azure-Portal zum Erstellen von [Logik-Apps](./app-service-logic-what-are-logic-apps.md "Informationen zu Logik-Apps") mit B2B-Features nutzen.

## Bearbeiten einer Vereinbarung
Sie können eine Vereinbarung bearbeiten, indem Sie folgende Schritte ausführen:
1. Wählen Sie das Integrationskonto aus, das die Vereinbarung enthält, die Sie ändern möchten.
2. Wählen Sie die Kachel **Vereinbarungen** aus.
3. Wählen Sie auf dem Blatt **Vereinbarungen** die Vereinbarung aus, die Sie ändern möchten.
4. Wählen Sie im oberen Menü **Bearbeiten** aus.
5. Nehmen Sie im eingeblendeten Menü „Bearbeiten“ die gewünschten Änderungen vor, und wählen Sie dann **OK** aus, um die Änderungen zu speichern.

## Löschen einer Vereinbarung
Sie können eine Vereinbarung löschen, indem Sie diese Schritte im Integrationskonto mit der Vereinbarung ausführen, die Sie löschen möchten:
1. Wählen Sie die Kachel **Vereinbarungen** aus.
2. Wählen Sie auf dem Blatt **Vereinbarungen** die Vereinbarung aus, die Sie löschen möchten.
3. Wählen Sie im oberen Menü **Löschen** aus.
4. Bestätigen Sie, dass Sie die Vereinbarung tatsächlich löschen möchten.
5. Die Vereinbarung ist nun nicht mehr auf dem Blatt „Vereinbarungen“ aufgeführt.
 

## Nächste Schritte
- [Erstellen einer AS2-Vereinbarung](./app-service-logic-enterprise-integration-as2.md)

<!---HONumber=AcomDC_0803_2016-->