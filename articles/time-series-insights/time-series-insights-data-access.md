---
title: Datenzugriffsrichtlinien in Azure Time Series Insights | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Datenzugriffsrichtlinien in Time Series Insights verwalten.
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: omravi
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 143d48b9fc551ade0d0fd043cdb0cf1f3340a9af
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017

---

# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Gewähren von Datenzugriff für eine Time Series Insights-Umgebung über das Azure-Portal

Time Series Insights-Umgebungen verfügen über zwei unabhängige Typen von Zugriffsrichtlinien:

* Verwaltungszugriffsrichtlinien
* Datenzugriffsrichtlinien

Beide Richtlinien gewähren Azure Active Directory-Prinzipalen (Benutzern und Apps) diverse Berechtigungen für eine bestimmte Umgebung. Die Prinzipale (Benutzer und Apps) müssen dem Active Directory (oder „Azure-Mandanten“) angehören, der dem Abonnement mit der Umgebung zugeordnet ist.

Verwaltungszugriffsrichtlinien gewähren Berechtigungen im Zusammenhang mit der Konfiguration der Umgebung. Hierzu zählt beispielsweise Folgendes:
*    Erstellung und Löschung der Umgebung, Ereignisquellen und Verweisdatasets sowie
*    Verwaltung der Datenzugriffsrichtlinien.

Datenzugriffsrichtlinien gewähren Berechtigungen für die Erstellung von Datenabfragen, für die Bearbeitung von Verweisdaten in der Umgebung und für gespeicherte freigegebene Abfragen und Perspektiven, die der Umgebung zugeordnet sind.

Die beiden Arten von Richtlinien ermöglichen eine klare Trennung zwischen dem Zugriff auf die Umgebungsverwaltung und dem Zugriff auf die Daten in der Umgebung. Dadurch können Sie eine Umgebung beispielsweise so einrichten, dass der Besitzer/Ersteller der Umgebung keinen Datenzugriff hat. Ebenso können Sie Benutzern und Diensten, die über die Berechtigung zum Lesen von Daten aus der Umgebung verfügen, den Zugriff auf die Konfiguration der Umgebung verweigern.

## <a name="grant-data-access"></a>Gewähren des Datenzugriffs
Die folgenden Schritte zeigen, wie Sie einem Benutzerprinzipal Datenzugriff gewähren:

1.    Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2.    Klicken Sie im Azure-Portal im Menü auf der linken Seite auf „Alle Ressourcen“.
3.    Wählen Sie Ihre Time Series Insights-Umgebung aus.

  ![Verwalten der Time Series Insights-Quelle – Umgebung](media/data-access/getstarted-grant-data-access1.png)

4.    Wählen Sie „Datenebenenzugriff“ aus, und klicken Sie anschließend auf „Hinzufügen“.

  ![Verwalten der Time Series Insights-Quelle – Hinzufügen](media/data-access/getstarted-grant-data-access2.png)

5.    Klicken Sie auf „Benutzer auswählen“.
6.    Suchen Sie den Benutzer anhand der E-Mail-Adresse, und wählen Sie ihn aus.
7.    Klicken Sie auf dem Blatt „Benutzer auswählen“ auf „Auswählen“.

  ![Verwalten der Time Series Insights-Quelle – Benutzer auswählen](media/data-access/getstarted-grant-data-access3.png)

8.    Klicken Sie auf „Rolle auswählen“.
9.    Wählen Sie „Mitwirkender“ aus, wenn Sie dem Benutzer das Ändern von Verweisdaten und das Freigeben gespeicherter Abfragen und Perspektiven für andere Benutzer der Umgebung ermöglichen möchten. Wählen Sie andernfalls „Leser“ aus, um dem Benutzer das Abfragen von Daten und das Speichern persönlicher (nicht geteilter) Abfragen in der Umgebung zu ermöglichen.
10.    Klicken Sie auf dem Blatt „Rolle auswählen“ auf „OK“.

  ![Verwalten der Time Series Insights-Quelle – Rolle auswählen](media/data-access/getstarted-grant-data-access4.png)

11.    Klicken Sie auf dem Blatt „Benutzerrolle auswählen“ auf „OK“.
12.    Folgendes sollte angezeigt werden:

  ![Verwalten der Time Series Insights-Quelle – Ergebnisse](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer Ereignisquelle](time-series-insights-add-event-source.md)
* [Senden von Ereignissen](time-series-insights-send-events.md) an die Ereignisquelle
* Anzeigen Ihrer Umgebung im [Time Series Insights-Portal](https://insights.timeseries.azure.com)

