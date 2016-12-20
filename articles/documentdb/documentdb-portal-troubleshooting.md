---
title: Problembehandlung im DocumentDB-Portal | Microsoft Docs
description: "Finden Sie heraus, wie Sie Probleme im DocumentDB-Azure-Portal lösen."
services: documentdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: monicar
ms.assetid: 36ad9bf4-2617-4347-ba29-edebf62fc3ec
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2016
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d41cbded31d64525a3e04ef742091c8d87760416


---
# <a name="azure-documentdb-portal-troubleshooting-tips"></a>Tipps zur Problembehandlung im Azure DocumentDB-Portal
Dieser Artikel beschreibt, wie Sie DocumentDB-Probleme im Azure-Portal lösen. 

## <a name="resources-are-missing"></a>Ressourcen fehlen
**Symptom**: Datenbanken oder Sammlungen fehlen in Ihren Portalblättern.

**Lösung**: Niedrigere Anwendungsnutzung für Betrieb unter dem maximalen Durchsatzkontingent für die Sammlung. 

**Erklärung**: Das Portal ist eine Anwendung wie jede andere, die Aufrufe an Ihre DocumentDB-Datenbank und Sammlung richtet. Wenn Ihre Anfragen derzeit aufgrund von Aufrufen von einer separaten Anwendung gedrosselt sind, kann das Portal auch gedrosselt werden, sodass Ressourcen nicht im Portal angezeigt werden. Um das Problem zu lösen, beheben Sie die Ursache für den hohen Durchsatz, und aktualisieren Sie dann das Portalblatt. Informationen zum Messen und Senken der Durchsatznutzung finden Sie im Abschnitt [Durchsatz](documentdb-performance-tips.md#throughput) des Artikels [Leistungstipps](documentdb-performance-tips.md).

## <a name="pages-or-blades-wont-load"></a>Seiten oder Blätter werden nicht geladen
**Symptom**: Seiten und Blätter im Portal werden nicht angezeigt.

**Lösung**: Niedrigere Anwendungsnutzung für Betrieb unter dem maximalen Durchsatzkontingent für die Sammlung. 

**Erklärung**: Das Portal ist eine Anwendung wie jede andere, die Aufrufe an Ihre DocumentDB-Datenbank und Sammlung richtet. Wenn Ihre Anfragen derzeit aufgrund von Aufrufen von einer separaten Anwendung gedrosselt sind, kann das Portal auch gedrosselt werden, sodass Ressourcen nicht im Portal angezeigt werden. Um das Problem zu lösen, beheben Sie die Ursache für den hohen Durchsatz, und aktualisieren Sie dann das Portalblatt. Informationen zum Messen und Senken der Durchsatznutzung finden Sie im Abschnitt [Durchsatz](documentdb-performance-tips.md#throughput) des Artikels [Leistungstipps](documentdb-performance-tips.md).

## <a name="add-collection-button-is-disabled"></a>Schaltfläche „Sammlung hinzufügen“ ist deaktiviert
**Symptom**: Auf dem Blatt „Datenbank“ ist die Schaltfläche **Sammlung hinzufügen** deaktiviert.

**Erklärung**: Wenn Ihrem Azure-Abonnement Leistungsguthaben zugeordnet ist, z.B. kostenlose Gutschriften im Rahmen eines MSDN-Abonnements, und Sie Ihr gesamtes Guthaben für den Monat verwendet haben, können Sie keine zusätzlichen Sammlungen in DocumentDB erstellen.

**Lösung**: Entfernen Sie das Ausgabenlimit für Ihr Konto.

1. Klicken Sie im Azure-Portal in der Navigationsleiste auf **Abonnements**, klicken Sie auf das Abonnement, das der DocumentDB-Datenbank zugeordnet ist, und klicken Sie auf dem Blatt **Abonnement** auf **Verwalten**. 
    ![DocumentDB bietet mehrere klar definierte (gelockerte) Konsistenzmodelle.](./media/documentdb-portal-troubleshooting/documentdb-change-billing.png)
2. Im neuen Browserfenster sehen Sie, dass Sie nicht mehr über Guthaben verfügen. Klicken Sie auf die Schaltfläche **Ausgabenlimit entfernen** , um die Ausgaben nur für den aktuellen Abrechnungszeitraum oder dauerhaft zu entfernen. Schließen Sie dann den Assistenten, um Ihre Kreditkarteninformationen hinzuzufügen oder zu bestätigen. 
    ![DocumentDB bietet mehrere klar definierte (gelockerte) Konsistenzmodelle.](./media/documentdb-portal-troubleshooting/documentdb-remove-spending-limit.png)

## <a name="query-explorer-completes-with-errors"></a>Abfrage-Explorer wird mit Fehlern abgeschlossen
Siehe [Schreiben, Bearbeiten und Ausführen von SQL-Abfragen für DocumentDB mit dem Abfrage-Explorer](documentdb-query-collections-query-explorer.md#troubleshoot).

## <a name="no-data-available-in-monitoring-tiles"></a>Keine Daten in Überwachungskacheln verfügbar
Siehe [Überwachen von Anforderungen, Nutzung und Speicher in DocumentDB](documentdb-monitor-accounts.md#troubleshooting).

## <a name="no-documents-returned-in-document-explorer"></a>Im Dokument-Explorer werden keine Dokumente zurückgegeben
Siehe [Anzeigen, Bearbeiten, Erstellen und Hochladen von JSON-Dokumenten mithilfe von DocumentDB-Dokument-Explorer](documentdb-view-json-document-explorer.md#troubleshoot).

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie im Portal weiterhin auf Probleme stoßen, bitten Sie in einer E-Mail an [askdocdb@microsoft.com](mailto:askdocdb@microsoft.com) um Unterstützung, oder stellen Sie eine Supportanfrage im Portal, indem Sie auf **Durchsuchen**, **Hilfe & Support** und dann auf **Supportanfrage erstellen** klicken.




<!--HONumber=Nov16_HO3-->


