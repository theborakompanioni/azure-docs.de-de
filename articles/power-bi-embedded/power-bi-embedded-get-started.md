---
title: Erste Schritte mit Microsoft Power BI Embedded
description: "Power BI Embedded, Hinzufügen interaktiver Power BI-Berichte zu Ihrer Business Intelligence-Anwendung"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 4787cf44-5d1c-4bc3-b3fd-bf396e5c1176
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 02/06/2017
ms.author: asaxton
translationtype: Human Translation
ms.sourcegitcommit: bd7925f3fa9a717cbe0649bf899cdd00511d5ca6
ms.openlocfilehash: b9dff45d1bb60d50c882c6daf363fca86a7f8f4c
ms.lasthandoff: 02/22/2017


---
# <a name="get-started-with-microsoft-power-bi-embedded"></a>Erste Schritte mit Microsoft Power BI Embedded
**Power BI Embedded** ist ein Azure-Dienst, mit dem Anwendungsentwickler eigenen Anwendungen interaktive Power BI-Berichte hinzufügen können. **Power BI Embedded** kann mit vorhandenen Anwendungen verwendet werden, ohne dass der Entwurf oder der Anmeldevorgang der Benutzer geändert werden müssen.

Ressourcen für **Microsoft Power BI Embedded** werden über die [Azure ARM-APIs](https://msdn.microsoft.com/library/mt712306.aspx)bereitgestellt. In diesem Fall ist die Ressource, die Sie bereitstellen, eine **Power BI-Arbeitsbereichssammlung**.

![](media/power-bi-embedded-get-started/introduction.png)

## <a name="create-a-workspace-collection"></a>Erstellen einer Arbeitsbereichssammlung
Eine **Arbeitsbereichssammlung** ist die Azure-Ressource der obersten Ebene und ein Container für den Inhalt, der in Ihre Anwendung eingebettet wird. Eine **Arbeitsbereichssammlung** kann auf zwei Arten erstellt werden:

* Manuell mit dem Azure-Portal
* Programmgesteuert mit den Azure Resource Manager-APIs (ARM-APIs)

Wir führen nun die Schritte zum Erstellen einer **Arbeitsbereichssammlung** mit dem Azure-Portal aus.

1. Öffnen Sie das **Azure-Portal**, und melden Sie sich an: [http://portal.azure.com](http://portal.azure.com).
2. Klicken Sie im oberen Bereich auf **+ Neu** .
   
   ![](media/power-bi-embedded-get-started/create-workspace-1.png)
3. Klicken Sie unter **Daten und Analysen** auf **Power BI Embedded**.
4. Geben Sie auf dem Blatt für die **Arbeitsbereichssammlung** die erforderlichen Informationen ein. Informationen zu **Preisen**finden Sie unter [Power BI Embedded – Preise](http://go.microsoft.com/fwlink/?LinkID=760527).
   
   ![](media/power-bi-embedded-get-started/create-workspace-2.png)
5. Klicken Sie auf **Erstellen**.

Es dauert einen Moment, bis die **Arbeitsbereichssammlung** bereitgestellt wird. Nach Abschluss des Vorgangs wird das Blatt **Arbeitsbereichssammlung**angezeigt.

   ![](media/power-bi-embedded-get-started/create-workspace-3.png)

Das Blatt für die **Erstellung** enthält die erforderlichen Informationen zum Aufrufen der APIs, mit denen Arbeitsbereiche erstellt und Inhalte dafür bereitgestellt werden.

<a name="view-access-keys"/>

## <a name="view-power-bi-api-access-keys"></a>Anzeigen von Power BI-API-Zugriffsschlüsseln
Zu den wichtigsten Informationen zum Aufrufen der Power BI-REST-APIs gehören die **Zugriffsschlüssel**. Sie dienen zum Generieren der **App-Token** , die zum Authentifizieren Ihrer API-Anforderungen verwendet werden. Klicken Sie zum Anzeigen der **Zugriffsschlüssel** auf dem Blatt **Einstellungen** auf **Zugriffsschlüssel**. Weitere Informationen zu **App-Token**finden Sie unter [Authentifizieren und Autorisieren mit Power BI Embedded](power-bi-embedded-app-token-flow.md).

   ![](media/power-bi-embedded-get-started/access-keys.png)

Sie werden feststellen, dass Sie über zwei Schlüssel verfügen.

   ![](media/power-bi-embedded-get-started/access-keys-2.png)

Kopieren Sie diese Schlüssel, und speichern Sie sie sicher in Ihrer Anwendung. Es ist sehr wichtig, dass Sie diese Schlüssel wie ein Kennwort behandeln, da hiermit Zugriff auf den gesamten Inhalt Ihrer **Arbeitsbereichssammlung**besteht.

Es sind zwar zwei Schlüssel aufgeführt, aber es wird nur jeweils ein Schlüssel benötigt. Der zweite Schlüssel wird bereitgestellt, damit Sie Schlüssel regelmäßig neu generieren können, ohne den Zugriff auf den Dienst zu unterbrechen.

Nachdem Sie nun über eine Instanz von Power BI für Ihre Anwendung und **Zugriffsschlüssel**verfügen, können Sie einen Bericht in Ihre eigene App importieren. Bevor das Importieren eines Berichts erklärt wird, wird im nächsten Abschnitt beschrieben, wie Sie Power BI-Datasets und -Berichte zum Einbetten in eine App erstellen.

## <a name="working-with-workspaces"></a>Verwenden von Arbeitsbereichen

Nach der Erstellung Ihrer Arbeitsbereichssammlung müssen Sie einen Arbeitsbereich für Ihre Berichte und Datasets erstellen. Zum Erstellen eines Arbeitsbereichs müssen Sie die [REST-API „Post Workspace“](https://msdn.microsoft.com/library/azure/mt711503.aspx) verwenden.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app"></a>Erstellen von Power BI-Datasets und -Berichten zum Einbetten in eine App
Nachdem Sie nun eine Instanz von Power BI für Ihre Anwendung erstellt haben und über **Zugriffsschlüssel**verfügen, müssen Sie die Power BI-Datasets und -Berichte erstellen, die eingebettet werden sollen. Sie können Datasets und Berichte mit **Power BI Desktop** erstellen. Sie können [Power BI Desktop kostenlos herunterladen](https://go.microsoft.com/fwlink/?LinkId=521662). Alternativ können Sie als schnellen Einstieg das [PBIX-Beispiel „Retail Analysis“](http://go.microsoft.com/fwlink/?LinkID=780547)herunterladen.

> [!NOTE]
> Weitere Informationen zur Verwendung von **Power BI Desktop** finden Sie unter [Getting Started with Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop) (Erste Schritte mit Power BI Desktop).

Mit **Power BI Desktop** stellen Sie eine Verbindung mit der Datenquelle her, indem Sie eine Kopie der Daten in **Power BI Desktop** importieren oder per **DirectQuery** eine direkte Verbindung mit der Datenquelle herstellen.

Im Folgenden werden die Unterschiede zwischen der Verwendung des **Importvorgangs** und der Verwendung von **DirectQuery** aufgeführt.

| Importieren | DirectQuery |
| --- | --- |
| Tabellen, Spalten *und Daten* werden in **Power BI Desktop** importiert oder kopiert. Bei der Arbeit mit Visualisierungen fragt **Power BI Desktop** eine Kopie der Daten ab. Um Änderungen anzuzeigen, die ggf. an den zugrunde liegenden Daten vorgenommen wurden, müssen Sie ein Dataset aktualisieren bzw. ein vollständiges aktuelles Dataset importieren. |Nur *Tabellen und Spalten* werden in **Power BI Desktop** importiert oder kopiert. Bei der Arbeit mit Visualisierungen fragt **Power BI Desktop** die zugrunde liegende Datenquelle ab. Dies bedeutet, dass die angezeigten Daten immer aktuell sind. |

Weitere Informationen zum Herstellen einer Verbindung mit einer Datenquelle finden Sie unter [Herstellen einer Verbindung mit einer Datenquelle](power-bi-embedded-connect-datasource.md).

Wenn Sie Ihre Arbeit in **Power BI Desktop** speichern, wird eine PBIX-Datei erstellt. Diese Datei enthält den Bericht. Wenn Sie Daten importieren, enthält die PBIX-Datei das vollständige Dataset. Bei Verwendung von **DirectQuery** enthält die PBIX-Datei nur ein Datasetschema. Sie stellen die PBIX-Datei programmgesteuert in Ihrem Arbeitsbereich bereit, indem Sie die [Power BI-API für den Import](https://msdn.microsoft.com/library/mt711504.aspx) verwenden.

> [!NOTE]
> **Power BI Embedded** verfügt über weitere APIs zum Ändern des Servers und der Datenbank, auf den bzw. auf die Ihr Dataset verweist, sowie zum Festlegen von Dienstkonto-Anmeldeinformationen, die vom Dataset zum Herstellen der Verbindung mit Ihrer Datenbank verwendet werden. Weitere Informationen finden Sie unter [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) und [Patch GatewayDatasource](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="next-steps"></a>Nächste Schritte
In den obigen Schritten haben Sie eine Arbeitsbereichssammlung und Ihren ersten Bericht und ein Dataset erstellt. Als Nächstes erfahren Sie, wie Sie Code für **Power BI Embedded**schreiben. Als Starthilfe haben wir eine Beispiel-Web-App erstellt: [Erste Schritte mit dem Beispiel](power-bi-embedded-get-started-sample.md). Mit dem Beispiel wird Folgendes veranschaulicht:

* Bereitstellen von Inhalt
  * Erstellen eines Arbeitsbereichs
  * Importieren einer PBIX-Datei
  * Aktualisieren von Verbindungszeichenfolgen und Festlegen von Anmeldeinformationen für Ihre Datasets
* Sicheres Einbetten eines Berichts

## <a name="see-also"></a>Weitere Informationen
* [Erste Schritte mit dem Beispiel](power-bi-embedded-get-started-sample.md)
* [Authentifizieren und Autorisieren mit Power BI Embedded](power-bi-embedded-app-token-flow.md)
* [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)

Weitere Fragen? [Power BI-Community ausprobieren](http://community.powerbi.com/)


